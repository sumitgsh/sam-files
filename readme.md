## As-1
```
Assignment-1
1(a):
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<unistd.h>
int main(){
pid_t ret;
res = fork();
if(res<0){
printf("Fork failure\n");
}
else if(res==0){
printf("Child process Id: %d\n",getpid());
printf("Child process parent Id: %d\n",getppid());
int a,b;
printf("\nEnter the dividend: ");
scanf("%d",&a);
printf("\nEnter the divisor: ");
scanf("%d",&b);
if(b==0){
printf("Divisor cannot be zero\n");
exit(1);
}
else{
printf("\nThe result is: %d\n",a/b);
exit(0);
}
}
else{
printf("\nParent process\n");
printf("Process Id: %d\n",getpid());
printf("Process parent Id: %d\n",getppid());
}
return 0;
}
1(b)
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<unistd.h>
#include<sys/wait.h>
int main(){
pid_t ret;
ret = fork();
if(ret<0){
printf("Fork failure\n");
}
else if(ret==0){
printf("\nChild process\n");
printf("Child process ID: %d\n",getpid());
printf("Child process parent ID: %d\n",getppid());
int a,b;
printf("\nEnter the dividend: ");
scanf("%d",&a);
printf("\nEnter the divisor: ");
scanf("%d",&b);
if(b==0){
printf("Divisor cannot be zero\n");
exit(1);
}
else{
printf("\nThe result is: %d\n",a/b);
exit(0);
}
}
else{
wait(NULL);
printf("\nParent process\n");
printf("Process ID: %d\n",getpid());
printf("Process parent ID: %d\n",getppid());
}
return 0;
}
1(c)
Note: The below user defined program is saved as OddEven.c
#include<stdio.h>
#include<stdlib.h>
int main(){
int a=5;
 if(a&1)
 {
 printf("odd");
 }else
 {
 printf("even");
 } return 0;
}
- OddEven.c file is used in the below program
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<unistd.h>
#include<sys/wait.h>
int main(){
pid_t ret;
int stat;
char *p;
ret = fork();
if(ret<0){
printf("Fork failure\n");
}
else if(ret==0){
char *argv[] = {"./OddEven",NULL};
p = argv[0];
printf("Program name: %s\n",p);
execv(argv[0],argv);
}
else{
wait(&stat);
if(WIFEXITED(stat)){
printf("Exit status %d\n",stat);
}
}
return 0;
}
```

## As-2
```
Assignment-2
2(a)
#include <stdio.h>
void longest_job_first(int at[], int bt[], int process)
{
 printf("\n LONGEST JOB FIRST ALGORITHM \n");
 int p[20], wt[20], tat[20], i, j, n, total = 0, pos, temp, x = 0;
 float avg_wt, avg_tat;
 n = process;
 for (i = 0; i < n; i++)
 {
 p[i] = i;
 }
 for (i = 0; i < n; i++)
 {
 pos = i;
 for (j = i + 1; j < n; j++)
 {
 if (bt[j] > bt[pos])
 pos = j;
 }
 temp = bt[i];
 bt[i] = bt[pos];
 bt[pos] = temp;
 temp = p[i];
 p[i] = p[pos];
 p[pos] = temp;
 }
 wt[0] = 0;
 for (i = 1; i < n; i++)
 {
 wt[i] = 0;
 for (j = 0; j < i; j++)
 {
 wt[i] += bt[j];
 }
 total += wt[i];
 }
 printf("\nGantt Chart\n");
 for (i = 0; i < n; i++)
 {
 x += bt[i];
 printf("\nP%d %d", p[i], x);
 }
 avg_wt = (float)total / n;
 total = 0;
 printf("\nProcesst Burst Time \tWaiting Time \tTurnaround Time");
 for (i = 0; i < n; i++)
 {
 tat[i] = bt[i] + wt[i];
 total += tat[i];
 printf("\np%d\t\t %d\t\t %d\t\t\t%d", p[i], bt[i], wt[i], tat[i]);
 }
 avg_tat = (float)total / n;
 printf("\nAverage Waiting Time=%f", avg_wt);
 printf("\nAverage Turnaround Time=%f", avg_tat);
}
void srtf(int at[], int bt[], int process)
{
 printf("\n\n**** SRTF ALGORITHM ****\n");
 int p[20], wt[20], tat[20], i, j, n, total = 0, pos, temp, x = 0;
 float avg_wt, avg_tat;
 n = process;
 for (i = 0; i < n; i++)
 {
 p[i] = i;
 }
 for (i = 0; i < n; i++)
 {
 pos = i;
 for (j = i + 1; j < n; j++)
 {
 if (bt[j] < bt[pos])
 pos = j;
 }
 temp = bt[i];
 bt[i] = bt[pos];
 bt[pos] = temp;
 temp = p[i];
 p[i] = p[pos];
 p[pos] = temp;
 }
 printf("\nGantt Chart\n");
 for (i = 0; i < n; i++)
 {
 x += bt[i];
 printf("\nP%d %d", p[i], x);
 }
 wt[0] = 0;
 for (i = 1; i < n; i++)
 {
 wt[i] = 0;
 for (j = 0; j < i; j++)
 wt[i] += bt[j];
 total += wt[i];
 }
 avg_wt = (float)total / n;
 total = 0;
 printf("\nProcesst Burst Time \tWaiting Time \tTurnaround Time");
 for (i = 0; i < n; i++)
 {
 tat[i] = bt[i] + wt[i];
 total += tat[i];
 printf("\np%d\t\t %d\t\t %d\t\t\t%d", p[i], bt[i], wt[i], tat[i]);
 }
 avg_tat = (float)total / n;
 printf("\nAverage Waiting Time=%f", avg_wt);
 printf("\nAverage Turnaround Time=%f", avg_tat);
}
int main()
{
 int n, i;
 int arrival_time[10], burst_time[10];
 printf(" Total number of process in the system: ");
 scanf("%d", &n);
 for (i = 0; i < n; i++)
 {
 printf("Enter the Arrival Time and Burst Time of P-%d ", i + 1);
 scanf("%d", &arrival_time[i]);
 scanf("%d", &burst_time[i]);
 }
 longest_job_first(arrival_time, burst_time, n);
 srtf(arrival_time, burst_time, n);
}
2(b)-
#include <stdio.h>
void main()
{
 int k, j, q, i, n, ts, temp;
 int aw;
 float awt, tat;
 //Assuming that the maximun values will be less than 10
 int burst_time[10], waiting_time[10], te[10], rt[10], arrival_time[10];
 j = 0;
 printf("Enter number of process :");
 scanf("%d", &n);
 for (i = 0; i < n; i++)
 {
 printf("\n Enter arrival time and Burst time of P-%d :", i + 1);
 scanf("%d", &arrival_time[i]);
 scanf("%d", &burst_time[i]);
 te[i] = 0;
 waiting_time[i] = 0;
 }
 for (i = 0; i < n; i++)
 {
 for (j = i + 1; j < n; j++)
 {
 if (arrival_time[i] > arrival_time[j])
 {
 //sorting according to arrival time
 temp = arrival_time[i];
 arrival_time[i] = arrival_time[j];
 arrival_time[j] = temp;
 temp = burst_time[i];
 burst_time[i] = burst_time[j];
 burst_time[j] = temp;
 }
 }
 }
 printf("\n Enter Time Qunatam: \t");
 scanf("%d", &ts);
 q = 0;
 printf("\nprocess :");
 for (i = 0; i < n; i++)
 {
 printf(" %d", i + 1);
 }
 printf("\nBrust time :");
 for (i = 0; i < n; i++)
 {
 printf(" %d", burst_time[i]);
 rt[i] = burst_time[i];
 }
 printf("\nArrival time :");
 for (i = 0; i < n; i++)
 {
 printf(" %d", arrival_time[i]);
 }
 printf("\n Gantt chart \n");
 j = 0;
 while (j <= n)
 {
 j++;
 for (i = 0; i < n; i++)
 {
 if (rt[i] == 0)
 continue;
 if (rt[i] > ts)
 {
 printf("\n %d\t P%d", q, i + 1);
 q = q + ts;
 rt[i] = rt[i] - ts;
 te[i] = te[i] + 1;
 }
 else
 {
 printf("\n %d\t P%d", q, i + 1);
 waiting_time[i] = q - te[i] * ts;
 q = q + rt[i];
 rt[i] = rt[i] - rt[i];
 }
 }
 } //end of while
 awt = 0;
 //calculate waiting time
 for (i = 0; i < n; i++)
 {
 waiting_time[i] = waiting_time[i] - arrival_time[i];
 awt = awt + waiting_time[i];
 }
 aw = awt;
 //calculate Turn around time
 tat = 0;
 for (int i = 0; i < n; i++)
 {
 tat += waiting_time[i] + burst_time[i];
 }
 printf("\n Average waiting time %f ", awt / n);
 printf("\n Average turn around time %f ", tat / n);
}
2(c)-
#include <stdio.h>
void priority_non_preemptive(int at[], int bt[], int process)
{
 printf("\n**** PRIORITY ALGORITHM (Non Preemptive) ****\n");
 int priority[10];
 int i, count = 0, highest = 9, time;
 int waiting = 0, turnaround = 0;
 float avg_wt, avg_tat;
 for (i = 0; i < process; i++)
 {
 printf("Enter priority of Process %d: ", i + 1);
 scanf("%d", &priority[i]);
 }
 priority[highest] = -1;
 printf("\nGannt Chart:\n");
 for (time = 0; count != process;)
 {
 highest = 9;
 for (i = 0; i < process; i++)
 {
 if (at[i] <= time && priority[i] > priority[highest])
 {
 highest = i;
 }
 }
 time += bt[highest];
 printf("Process: %d - Current CPU Time: %d \n", highest + 1, time);
 priority[highest] = -1;
 count++;
 waiting += time - at[highest] - bt[highest];
 turnaround += time - at[highest];
 }
 avg_wt = waiting * 1.0 / process;
 avg_tat = turnaround * 1.0 / process;
 printf("\nAverage Turn Around Time: %f \n", avg_wt);
 printf("Average Waiting Time: %f \n", avg_tat);
}
void priority_preemptive(int at[], int bt[], int process)
{
 printf("\n**** PRIORITY ALGORITHM (Preemptive) ****\n");
 int priority[10], temp[10];
 int i, count = 0, highest = 9, time, end;
 int waiting = 0, turnaround = 0;
 float avg_wt, avg_tat;
 for (i = 0; i < process; i++)
 {
 printf("Enter priority of Process %d: ", i + 1);
 scanf("%d", &priority[i]);
 temp[i] = bt[i];
 }
 priority[highest] = -1;
 for (time = 0; count != process; time++)
 {
 highest = 9;
 for (i = 0; i < process; i++)
 {
 if (at[i] <= time && priority[i] > priority[highest] && temp[i] > 0)
 {
 highest = i;
 }
 }
 temp[highest]--;
 printf("Process: %d - Current CPU Time: %d \n", highest + 1, time + 1);
 if (temp[highest] == 0)
 {
 count++;
 end = time + 1;
 waiting += end - at[highest] - bt[highest];
 turnaround += end - at[highest];
 }
 }
 avg_wt = waiting * 1.0 / process;
 avg_tat = turnaround * 1.0 / process;
 printf("\nAverage Turn Around Time: %f \n", avg_wt);
 printf("Average Waiting Time: %f \n", avg_tat);
}
int main()
{
 int NOP, i;
 int at[10], bt[10];
 printf(" Total number of process in the system: ");
 scanf("%d", &NOP);
 printf("Enter the Arrival Time and Burst Time\n");
 for (i = 0; i < NOP; i++)
 {
 printf("Process %d \n", i + 1);
 printf("Arrival time is: ");
 scanf("%d", &at[i]);
 printf("Burst time is: ");
 scanf("%d", &bt[i]);
 }
 priority_non_preemptive(at, bt, NOP);
 priority_preemptive(at, bt, NOP);
 return 0;
}
```

## As-3
### Fork-Signal-Thread
3-a(signal handler)>


```
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>
#include<signal.h>

/* Handler function for the Signal */
void handler(int num){

    pid_t ret_id;

    //Create a child Process

    ret_id = fork();
    /* Failed fork if return status <0 */
    if(ret_id<0){
        printf("Failed to create process\n");
    }
    // Child Process
    else if(ret_id==0){
        printf("\n* Inside the child Process*\n");
        printf("The process ID is: %d\n",getpid());
        printf("The parent process ID is: %d\n",getppid());
    }
    // Parent Process
    else{
        wait(NULL);
        printf("\n* Inside the parent process *\n");
        printf("The process ID is: %d\n",getpid());
        printf("The parent process ID is: %d\n",getppid());
    }
    exit(0);
}

int main()
{

	/* signal() system call */
    signal(SIGINT,handler);

	while(1){
    printf("Waiting for Interrupt...(Press Ctrl+C to Interrupt)\n");
        sleep(1);
    }

    return 0;
}
```

3-a-modifed(signal handler)>

```
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
#include<stdlib.h>
#include<sys/types.h>


// SigInt
void sigint(){
    printf("Signal id: 2\n");
    exit(0);
}

// SigInt function call
void sighquit(){
    printf("Signal quit id: 3\n");
    exit(0);
}


// sigkill funciton call
void sigkill(){
    printf("Signal kill: 9\n");
    exit(0);
}

//sigabort function call
void sigabrt(){
    printf("Signal abort: 6\n");
    exit(0);
}

int main()
{
    int i;

    //Process Id genaration
	printf("Current process ID is: %d\n",getpid());
 	signal(SIGINT, sigint);
 	signal(SIGHUP, sighquit);
 	signal(SIGKILL, sigkill);
 	signal(SIGABRT, sigabrt);

    //Wait for process to send the signal
 	for(i=0; ;i++){
        printf("Waiting for another process to send the signal:\n");
        sleep(3);
    }
    return 0;
}
```


3-b(along with kill)>
```
#define _POSIX_SOURCE
#include<stdio.h>
#include <stdlib.h>
#include<signal.h>
#include<unistd.h>
#include<unistd.h>
#include<sys/types.h>

int main()
{
     /* Stores the Process ID to send signal */
	pid_t PROCESS_ID;

	//Bucket for the Signal Number
    int SIGNAL_NUM;
    printf("Enter a Process ID: ");
    scanf("%d",&PROCESS_ID);

    printf("Select or Type signal value from the below list:\n");
    printf("Interrupt Signal(SIGINT) type value->2\n");
    printf("Quit the process(SIGQUIT) type value->3\n");
    printf("Kill Process(SIGKILL) type value->9\n");
    printf("Abort Process(SIGABRT) type value->6\n");

    printf("\n Enter the signal value :");
    scanf("%d",&SIGNAL_NUM);
    /* Sending signal to another process */
    kill(PROCESS_ID,SIGNAL_NUM);
    return 0;
}

```




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

## As-10
### client
```



#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>

// structure for message queue
struct mesg_buffer {
    //type of the message..
	long msg_type;
	//size of the message limited to 100 characters..
	char msg_text[100];
} message;

int main()
{
	key_t key;
	int msgid;

	// Generate unique key
	key = ftok("progfile", 65);

	// msgget creates a message queue and returns identifier
	msgid = msgget(key, 0666 | IPC_CREAT);

	// Printing the message queue ID
	printf("Message Queue ID is: %d \n", msgid);

	// msgrcv to receive message
	msgrcv(msgid, &message, sizeof(message), 1, 0);

	// Displaying the message
	printf("Data Received from server : %s \n", message.msg_text);

	// Removing the message queue
	printf("Removing the queue. \n");
	msgctl(msgid, IPC_RMID, NULL);

	return 0;
}

```

### server
```
// C Program for Message Queue (Writer Process)
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#define MAX 100

// structure for message queue
struct mesg_buffer {
	long msg_type;
	char msg_text[100];
} message;

int main()
{
	key_t key;
	int msgid;

	// ftok to generate unique key
	key = ftok("progfile", 65);

	// msgget creates a message queue and returns identifier
	msgid = msgget(key, 0666 | IPC_CREAT);
	message.msg_type = 1;

	// printing the message queue ID
	printf("Message Queue ID is: %d \n", msgid);

	// Taking message as input
	printf("Enter the message: ");
	fgets(message.msg_text, MAX, stdin);

	// msgsnd to send message
	msgsnd(msgid, &message, sizeof(message), 0);

	// Displaying the sent message
	printf("Sent data to client: %s \n", message.msg_text);

	return 0;
}

```
