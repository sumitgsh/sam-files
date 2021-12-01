## As-3
### Fork-Signal-Thread


3-c(thread)->
```
#include<stdio.h>
#include<pthread.h>
#include <stdlib.h>
#include <unistd.h>

//Function prints HELLOW WORLD
//called by pthread
void print_hello(void *arg){
	pthread_t id = *(int *) arg;
    printf("HELLO WORLD( In Thread ID %d )\n",id);
    /* exiting from thread. */
	pthread_exit(NULL);
}

// Driver Function
int main(){
    pthread_t thread_id;
    /* Beginning the new thread */
    printf("Executing the thread\n");
     // creating a thread
    pthread_create(&thread_id,NULL,print_hello,&thread_id);
    /* waiting for thread execution to get finished. */
	pthread_join(thread_id, NULL);
    printf("Thread execution completed\n");
    /* Thread execution finished and exiting */
    exit(0);
}





```




## As-4
### PIPE

```
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<wait.h>
#include<signal.h>
#define N 100

void handler(int signo){

    int i, first_term ,second_term , third_term, no_of_terms;
    int fd[2],arr[N];
    pipe(fd);
    pid_t pid, gpid;
    pid = fork();

    if(pid < 0)
        exit(1);

    if(pid == 0){
        gpid = getpid();
        write(fd[1],&gpid,sizeof(gpid));

        printf("\nChild Process\n");
        printf("Enter the number of terms for the fibonacci series: ");
        scanf("%d",&no_of_terms);
        write(fd[1],&no_of_terms,sizeof(no_of_terms));

        first_term = 0;
        second_term = 1;
        write(fd[1],&first_term,sizeof(first_term));
        write(fd[1],&second_term,sizeof(second_term));

        for(i = 2; i< no_of_terms ; ++i){
            third_term = first_term + second_term;
            first_term = second_term;
            second_term = third_term;
            write(fd[1],&third_term,sizeof(third_term));
        }
        write(fd[1],&signo,sizeof(signo));

        exit(0);
    }

    else if(pid > 1){
        wait(NULL);
        printf("\nParent Process\n");
        read(fd[0],&gpid,sizeof(gpid));
        read(fd[0],&no_of_terms,sizeof(no_of_terms));
        read(fd[0],&arr,N);

        for(i = 0 ; i < no_of_terms; i++){
            printf(" %d ",arr[i]);
        }

        printf("\nSignal number is : %d\n",signo);
        exit(0);
    }
}

int main(){
    signal(SIGINT,handler);
    while(1){
        printf("Please Press Ctrl+C\n");
        sleep(2);
    }
    return 0;
}

```




## As-5

### PIPE or FIFO

5-a-1>
```
#include <stdio.h>
#include <string.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <unistd.h>

int main()
{
    int fd;

    //FiFo file path name..
    char *myfifo = "/tmp/myfifo";

    //Creating a name pipe using mkfifo() along with the permissions
    //Since mknod() is not supported in C99 Compilers
    mkfifo(myfifo, 0666); /* 0666 - Read Write Permission */

    //Input Store..
	char str1[80], str2[80];

    while (1)
    {

        //Opening the created file in read mode
        printf("\nWaiting for Program 2 to write... \n");
        fd = open(myfifo,O_RDONLY);

        //read from the opened file
		read(fd, str1, 80);

        //Print the String passed by Program2
        printf("\nProgram 2: %s\n", str1);
        sleep(fd);

		//Input of the String in program-1
		printf("Input a string (Program 1):");
        fgets(str2, 80, stdin);

        //Opening FIFO in write mode ,such that we can write the contents in the created file..
        fd = open(myfifo,O_WRONLY);

        /* Writing the string taken from user to FIFO and sleep */
        write(fd, str2, strlen(str2)+1);
        sleep(fd);
    }

    return 0;
}

```

5-a-2>

```
#include <stdio.h>
#include <string.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <unistd.h>

int main()
{
    int fd;

    //FiFo file path name..
    char * myfifo = "/tmp/myfifo";

    //Creating a name pipe using mkfifo() along with the permissions
    //Since mknod() is not supported in C99 Compilers
    mkfifo(myfifo, 0666); /* 0666 - Read Write Permission */

	/* Character array for storing the inputs of two programs */
    char str1[80], str2[80];

    while (1)
    {
        //Input of the String in program-2
        printf("Input a string (Program 2):");
        fgets(str2, 80, stdin);

        //Opening FIFO in write mode ,such that we can write the contents in the created file..
        fd = open(myfifo, O_WRONLY);

        /* Writing the string taken from user to FIFO and close */
        write(fd, str2, strlen(str2)+1);
        close(fd);

		//Opening FIFO in read mode ,such that we can read the contents from the created file..
        printf("\nWaiting for Program 1 to write... \n");
        fd = open(myfifo, O_RDONLY);

        /* Reading from FIFO */
        read(fd, str1, sizeof(str1));

        /* Print the read string and close */
        printf("\nProgram 1: %s\n", str1);
        close(fd);
    }

    return 0;
}

```


## As-6

### Shared Memory

6-a-1>

```
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
int main()
{
    // ftok to generate unique key
    key_t key = ftok("shmfile",65);

    // shmget returns an identifier in shmid
    
    // shmget(key_t key,size_t size,shmflg)
    int shmid = shmget(key,1024,0666|IPC_CREAT);

    // shmat to attach to shared memory
    //attaching the memory to the process address space..
    //void* shmat(int id,const *void address,flag)
    char *str = (char*) shmat(shmid,(void*)0,0);

    printf("Enter the string: ");
    fgets(str,100,stdin);

    printf("Data in the shared memory: %s\n",str);

    //detach from shared memory
    shmdt(str);

    return 0;
}
```

6-a-2>
```
#include <sys/ipc.h>
#include <sys/shm.h>
#include <stdio.h>


int main()
{
	// ftok to generate unique key
	key_t key = ftok("shmfile",65);

	// shmget returns an identifier in shmid
	int shmid = shmget(key,1024,0666|IPC_CREAT);

	// shmat to attach to shared memory
	char *str = (char*) shmat(shmid,(void*)0,0);

	printf("Data read from memory: %s\n",str);

	//detach from shared memory
	shmdt(str);

	// destroy the shared memory
	shmctl(shmid,IPC_RMID,NULL);

	return 0;
}
```

```
6-b>
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <sys/time.h>
#include <sys/resource.h>
#include <sys/times.h>
#include <time.h>

int main(){

    // Structure variables for recording before and after time
    struct tms before,after;
    //start recording time
    times(&before);

    pid_t pid;
    //Creation of the child process
    if((pid=fork())<0){
        printf("fork error\n");
    }
    /* Creating child process and
    having a loop run 100000 times such that CPU gets busy and we can get the diffference
    in start and stop time*/
    else if(pid==0){
        long int i;
        for(i=0;i<100000;i++){
            printf("%ld \n",i);
        }
        exit(0);
    }
    else {
        wait(NULL);
    }
    //end recoding time
    times(&after);
    //system clock tick
    long double clktck=sysconf(_SC_CLK_TCK);

    double user=(after.tms_utime-before.tms_utime)/(double)clktck;
    double system=(after.tms_stime-before.tms_stime)/(double)clktck;
    double cuser=(after.tms_cutime-before.tms_cutime)/(double)clktck;
    double csystem=(after.tms_cstime-before.tms_cstime)/(double)clktck;

    /*
    User time is real or wall clock time ,
    and system is cpu time minus i/o and wait time
    divided by system clock time time to get actual time in seconds
    instead of clock pulses */

    FILE *fptr;
    //accessing file in write mode and opening
    fptr = fopen("time.txt","w");

    if(fptr == NULL){
    printf("File Error!");
    exit(1);
    }

    //writing to the  file.
    fprintf(fptr,"Parent USER time: %lfs \n",user);
    fprintf(fptr,"Parent SYSTEM time: %lfs \n",system);
    fprintf(fptr,"Child USER time : %lfs \n",cuser);
    fprintf(fptr,"Child SYSTEM time: %lfs \n",csystem);
    printf("Output written in the file SUCESSFULLY!!\n");

    return 0;
}

```
