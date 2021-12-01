
## As-5




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
