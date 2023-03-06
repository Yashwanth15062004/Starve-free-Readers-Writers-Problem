# Starve-free-Readers-Writers-Problem
The Reader-Writer Problem is a well-known issue in Computer Science, where multiple processes concurrently access a data structure such as a database or storage area. To ensure proper synchronization, a critical section is established that only allows one writer to access it at a time, while allowing multiple readers to access it simultaneously. Semaphores are typically employed to ensure that writers and readers can access the critical section without conflicts. However, this approach can result in readers or writers experiencing starvation, depending on their priorities. To address this issue, a solution has been developed that is both efficient and starvation-free.

### Initialising semaphores and variables
In = 1 (semaphore)
mutex = 1 (semaphore)
wrt = 1 (semaphore)
read_count = 0 (variable)

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
if (read_count==0)  Signal(wrt);
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
### Explaination
In the above pseudocode, I have taken three semaphores In,mutex,wrt which are initialised to 1. To remove the priority between the readers and writers, I have used an
extra semaphore compared to the pseudocode expalined in the class. The main idea of using "In" semaphore is to remove the priority between readers and writers and 
control the starvation. In the pseudocode explained in the class we can observe that writers can't enter into the critical section when readers keep on comming , which
leads to starvation. So to avoid starvation, I have used FIFO concept. Among the readers and writers those who comes first will enter into the ready queue and execute
their respective codes. If readers comes after writer then readers need to wait untill writer leaves the C.S. If writer comes after readers then writer has to wait
untill all the readers leave the C.S i.e, when read_count=0 then last reader will signal writer using wrt semaphore.

### Drawback
The reader has to down 2 semaphores everytime it enters though if there was no writer, which is leading to extra time consumption. So to optimise it we can down the one semaphore while entering the C.S and down another one while exiting the C.S. 

# Optimised pseudocode
Let us declare semaphore(in) before entering the critical section and semaphore(out) after exiting the C.S in reader pseudocode and semaphore(wrt) for mutual exclusion betweeen readers and writers. Also two variables counter_in and counter_out. 

### Initialising semaphores and variables
in  = 1        (semaphore)
out = 1        (semphore)
wrt = 0        (semaphore)
counter_in = 0 (variable)
counter_out= 0 (variable)
bool_wait  = 0 (boolian)

### Optimised Pseudocode for Readers
```c++
while(true){
Wait(in);
counter_in++;
Signal(in);

**********Critical section************

Wait(out);
counter_out++
if (wait==1 && counter_in == counter_out)
 Signal(wrt);
Signal(out);
}
```
### Optimised Pseudocode for Writers
```c++
while(true){
Wait(in);
Wait(out);
if(counter_in == counter_out)
 Signal(out);
 else
 { wait=1;
  Signal(out);
  Wait(wrt);
  wait=0;
 }

*********Critical section*********
Signal(in);
}
```

