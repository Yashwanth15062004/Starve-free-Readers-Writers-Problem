# Starve-free-Readers-Writers-Problem
The Reader-Writer Problem is a well-known issue in Computer Science, where multiple processes concurrently access a data structure such as a database or storage area. To ensure proper synchronization, a critical section is established that only allows one writer to access it at a time, while allowing multiple readers to access it simultaneously. Semaphores are typically employed to ensure that writers and readers can access the critical section without conflicts. However, this approach can result in readers or writers experiencing starvation, depending on their priorities. To address this issue, a solution has been developed that is both efficient and starvation-free.

### Initialising semaphores
In = Semaphore(1)
mutex = Semaphore(1)
wrt = Semaphore(1)
read_count = 0 

### Pseudocode for Readers
```c++
while(true){

Wait(In);
Wait(mutex);
read_count++;
if (read_count==1)  Wait(wrt);
Signal(mutex);
Signal(In);

*********Critical Section**********  //reading is performed

Wait(mutex);
read_count--;
if (read_count)==0  Signal(wrt);
Signal(mutex);

}
```
### Pseudocode for Writers
```c++
while(true){

Wait(In);
Wait(wrt);

********Critical Section**********   //writing is performed

Signal(wrt);
Signal(In);

}
```




