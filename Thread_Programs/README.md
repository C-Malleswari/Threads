#  POSIX Threads in C – System-Level Multithreading Guide

This repository contains **C programs** that demonstrate **thread programming** using **POSIX (Portable Operating System Interface) threads**, commonly known as **pthreads**, on Linux.

Multithreading is a critical concept in **system programming**, **OS design**, **embedded systems**, and **parallel computing**.

---

##  What Are Threads?

A **thread** is the smallest unit of execution within a process. All threads in the same process:
- Share the **same address space**
- Share **global variables**, **file descriptors**, and **heap**
- Have their own **stack**, **program counter**, and **registers**

> Threads are lightweight and allow efficient use of CPU cores.

---

##  Basic Diagram

```text
Main Process
    |
    |-- Thread 1 --> Task A
    |-- Thread 2 --> Task B
    |-- Thread 3 --> Task C
(Threads run in parallel and share the same memory)

---


##  Memory Model in Multithreading

| Memory Region | Shared Between Threads | Notes                         |
|---------------|------------------------|-------------------------------|
| Code/Text     |  Yes                   | All threads execute same code |
| Data/Heap     |  Yes                   | Shared variables/resources    |
| Stack         |  No                    | Each thread has its own       |

---

##  Thread Lifecycle (States)

NEW → READY → RUNNING → WAITING → TERMINATED



- **Create**: `pthread_create()`
- **Join**: `pthread_join()` → Waits for a thread to finish
- **Detach**: `pthread_detach()` → OS cleans up after thread finishes
- **Exit**: `pthread_exit()` → Thread ends itself

---

##  Race Conditions and Synchronization

###  Race Condition

Occurs when two or more threads access **shared memory** and try to change it at the same time.

###  Solutions

| Tool           | Use Case                                      |
|----------------|-----------------------------------------------|
| `pthread_mutex_t` | Locks critical sections (atomic access)   |
| `pthread_cond_t`  | Allows threads to wait and signal each other |
| `sem_t`           | Controls access to shared resources (counting) |

---

##  Example Use Cases

| Use Case                     | Threading Benefit                      |
|-----------------------------|----------------------------------------|
| Web Server                  | Handle multiple client connections     |
| Real-time Sensor Monitoring | Read and process sensor data in parallel |
| File Compression Tool       | Compress chunks of files concurrently  |
| Chat Application            | One thread listens, one displays       |

---

##  Files and Programs in This Repository

| File                      | Concept Demonstrated                    |
|---------------------------|-----------------------------------------|
| `single_thread.c`         | Basics of thread creation               |
| `multiple_threads.c`      | Creating multiple threads               |
| `thread_with_args.c`      | Passing arguments to threads            |
| `thread_return_value.c`   | Returning values from threads           |
| `mutex_example.c`         | Synchronization using mutex             |
| `semaphore_example.c`     | Counting semaphores to manage resources |
| `thread_join_detach.c`    | Thread cleanup techniques               |
| `condition_variable.c`    | Waiting and signaling with conditions   |
 
and so on...

---

##  How to Compile and Run

Use the `-pthread` flag for linking pthread library:

```bash
gcc filename.c -o output -pthread
./output

