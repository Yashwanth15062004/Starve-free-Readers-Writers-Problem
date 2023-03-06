# Starve-free-Readers-Writers-Problem
The Reader-Writer Problem is a well-known issue in Computer Science, where multiple processes concurrently access a data structure such as a database or storage area. To ensure proper synchronization, a critical section is established that only allows one writer to access it at a time, while allowing multiple readers to access it simultaneously. Semaphores are typically employed to ensure that writers and readers can access the critical section without conflicts. However, this approach can result in readers or writers experiencing starvation, depending on their priorities. To address this issue, a solution has been developed that is both efficient and starvation-free.

### Initialising semaphores and variables
In = 1 (semaphore),
mutex = 1 (semaphore),
wrt = 1 (semaphore),
read_count = 0 (variable)

### Pseudocode for Readers
```c++
while(true){

Wait(In);
Wait(mutex);
read_count++; //read_count is used to keep track of no.of readers inside the C.S
if (read_count==1) 
    Wait(wrt);
Signal(mutex);
Signal(In);

**Critical Section** //reading is performed

Wait(mutex);
read_count--;
if (read_count==0)  Signal(wrt);
Signal(mutex);

}
```
### Pseudocode for Writers
```c++
while(true){

Wait(In);
Wait(wrt);

**Critical Section**   //writing is performed

Signal(wrt);
Signal(In);

}
```
### Explaination
In the above pseudocode, I have taken three semaphores In,mutex,wrt which are initialised to 1. Semaphores "mutex" and "wrt" have the same usage as in case of
pseudocode explained in class. But to remove the priority between the readers and writers, I have used an extra semaphore(In). The main idea of using "In" semaphore is
to remove the priority between readers and writers and control the starvation. In the pseudocode explained in the class we can observe that writers can't enter into
the critical section when readers keep on comming , which leads to starvation. So to avoid starvation, I have used FCFS concept. Among thereaders and writers those who
comes first will enter into the ready queue and execute their respective codes. If readers comes after writer then readers need to wait untill writer leaves the C.S.
If writer comes after readers then writer has to wait until all the readers leave the C.S i.e, when read_count=0 then last reader will signal writer using wrt
semaphore.

### Conclusion

In the critical section at any instant, only readers (or) only one writer is allowed for mutual exclusion. It can be proved from the above code, that only readers
can enter the critical section keeping the writers blocked in the queue (if any), also only one writer can enter the C.S keeping the readers blocked in a queue if any.
Both of these is achieved by using "In" semaphore. This semaphore treats both readers and writers with equal priority and blocks them according to the arrival
times(FCFS). Thus Mutual Exclusion is satisfied in the above code.

We can observe that int the above code has no deadlock and entry of any process into the C.S is not decided by the remainder section, So the above code satisfies
Progresss also.

In the pseudocode discussed in the class where the readers are with more priority, we can observe that writers can't enter into the C.S if the readers keep on comming
which leads to starvation. But in theabove pseudocode, writers has to wait until all the readers(which came before him) leave the C.S which is obviously a finite
amount of time. Also a reader has to wait until the writer(if any) leave the C.S which is also a finite time waiting. Hence the above pseudocode also satisfies Bounded
waiting.

Hence all the requirements for a solution to be starve-free is satisfied by the above pseudocode.


