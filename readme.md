
## As-6

### Shared Memory

6-a>

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

6-b>
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
