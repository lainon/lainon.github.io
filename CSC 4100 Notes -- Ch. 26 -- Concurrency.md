# CSC 4100 Notes -- Ch. 26 -- Concurrency

## Recall the following information about processes

- Processes have their own program counters (PC)

- Processes have their own page directory base registers (PDBR)

- Processes have their own address space

- Information about processes is stored in the process struct (aka the process control block)

## Some initial thoughts about threads

* In contrast to our single-threaded view of computation, our programs might have more than one point of execution. This is handled with threads. 

* We can thus think of threads like separate processes, except for the fact that they share the same address space and can access the same data.

* Threads, like processes, undergo context switches. State information about threads is, therefore, stored in a similar manner to processes in the Thread Control Block. 

* When we undergo a context switch between threads, we do not change the PDBR.

* Note that we do still have to change the PC.

## Primary emphases when thinking about threads

1. Theads have the same address space

2. Uncontrolled Scheduling

## Multi-Threaded Address Space

| Program Code |
|:------------:|
| Heap         |
| (free)       |
| Stack 1      |
| (free)       |
| Stack 2      |

Another major difference between threads and processes concerns the stack; in a multi-threaded process, each thread runs independently, and thus needs to have its own stack for local variables, etc.

Note that it is okay for threads to share program code, as we expect threads to be a part of the same process. To account for this, each thread will have its own stack. Recall that the stack is a special region of memory that stores local/temporary variables created by functions, and is a LIFO data structure.

As we can see, the only place that remains for problems to arise is in the heap. Recall that the heap contains global variables, dynamically allocated memory, and so on, and is a FIFO data structure. Once memory is allocated on the heap, the programmer is responsible for deallocating it to avoid memory leaks.



## Why bother with threads?

1. <u>Parallelization</u> -- multiple threads, divide and conquer. Solve problems in parallel and save time

2. <u>I/O</u> -- we can perform context switches on threads to deal with I/O, just like we did with processes.



## Thread Creation Code

```c
/* Note that this syntax is particularly useful.
 * Because procedures are often used by multiple threads,
 * a return type and parameter of void * allow us to pass
 * in and return essentially arbitrary data */

void *mythread(void *arg)
{
    printf("%s\n", (char *) arg);
    return NULL;
}

int main(int argc, char *argv[])
{
    pthread_t p1, p2;
    int rc;
    
    printf("main: begin\n");

    Pthread_create(&p1, NULL, mythread, "A");
    Pthread_create(&p2, NULL, mythread, "B");
    
    //join waits for the threads to finish
    Pthread_join(p1, NULL);
    Pthread_join(p2, NULL);

    printf("main: end\n");

    return 0;
}
```


