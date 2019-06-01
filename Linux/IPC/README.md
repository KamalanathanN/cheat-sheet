# List
| Name | Feature | Extra Synchronization | Live with kernel/process |
| --- | --- | --- | --- |
| Shared Memory | fastest | Y | kernel |
| Message Queue | message with priority | N | kernel |
| PIPE | one-direction | N | process |
| FIFO | one-direction | N | process |
| Unix Socket | simple implementation | N | process |

# Shared Memory
## prototype
```c
#include <sys/shm.h> // include file
// Create/Get a shared memory segmnet
// Return: shared memory ID, or -1 while fail
int shmget(key_t key, size_t size, int shmflg);
// Map shared memory
// Return: return mapping address, or -1 while fail
void *shmat(int shmid, const void *shmaddr, int shmflg);
// Unmap shared memory
// Return: success with 0, fail with -1
int shmdt(const void *shmaddr);
// Shared memory control operations
// Return: success with 0, fail with -1
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
```
## example
Please refer to [shm_proc1.c](shm_proc1.c) and [shm_proc2.c](shm_proc2.c).

**You should use root privilege to run the program.**

# Mesage Queue
## prototype
```c
#include <sys/msg.h> // include file
// Create/open message queue
// Return: queue ID, or -1 while fail
int msgget(key_t key, int msgflg);
// Send message
// Return: success with 0, fail with -1
int msgsnd(int msqid, const void *msgp, size_t msgsz, int msgflg);
// Read message
// Return: message length, or -1 while fail
ssize_t msgrcv(int msqid, void *msgp, size_t msgsz, long msgtyp, int msgflg);
// Message queue control operations
// Return: success with 0, fail with -1
int msgctl(int msqid, int cmd, struct msqid_ds *buf);
```
## example
Please refer to [msgq_server.c](msgq_server.c) and [msgq_client.c](msgq_client.c).

**You should use root privilege to run the program.**

* The msgq in `msgsnd` and `msgrcv` should be like this
```c
struct msgbuf {
    long mtype;       /* message type, must be > 0 */
    char mtext[1];    /* message data, this variable can be changed */
};
```
* In `msgrcv`, different type has different meaning:
  - `type == 0`: The first message in queue.
  - `type > 0`: The first message which type==mtype.
  - `type < 0`: Find the message which mtype <= |type| and the mtype is the minimum.

# PIPE

# FIFO

# Unix Socket