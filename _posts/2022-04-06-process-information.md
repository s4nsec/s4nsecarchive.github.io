# Process Basic Information
*PID*:
Process ID. Starting from ID of 4 (belonging to the system processes)
They are multiples of 4

## Process Status
### GUI Process
Running when: GUI thread is responsive
Suspended when: All threads in the process are suspended
Not responding when: GUI thread does not check message queue in 5 seconds

### CUI Process
Running when: At least one thread is not responsive
Suspended when: All the threads in the process are suspended
Not responding when: Never

## UWP Process
Running when: At least one thread is not responsive
Suspended when: All the threads in the process are suspended
Not responding when: GUI thread does not check message queue in 5 seconds

A Process with a GUI must have one thread that handles the UI. This thread has a message queue and it should handle and process incoming messages. Listening functions are GetMessage and PeekMessage. If none of these are called in 5 seconds, the process gets into Not Responding status. This can happen for several reasons:
1. The thread is suspended for a reason
2. The thread is waiting for an I/O operations
3. CPU intensive work is being done by the thread and it's taking more than 5 secs

UWP Processes may go into Suspended status unwillingly. If you background calculator process you can see that its status is changed from Running to Suspended in Task Manager.
Non-UWP Processes are always in Running status since Windows doesn't know what they are doing. Unless all the threads inside the process are in a suspended state.

Windows API does not have a function to suspend a process, but it has a function to suspend a thread. So basically, by iterating through all the threads and suspending them, we can suspend a process. NtSuspendProcess can be used for this purpose. NtResumeProcess can be used to reverse this.

## User name
It is the name of the user under whom the process is running. A token object is attached to the process that holds the user's security context. Security context holds privileges, and groups that the user belongs to.

## Session ID
Under which session process executes. Session 0 is used by System process and services, session id 1 and more is used by interactive logins.

## CPU
It displays CPU usage by the process. It only shows whole numbers. More detailed info can be found at Process Explorer.

## Memory
There are two types of memory: 
1. Private working set
2. Active Private working set

Private working set is RAM used by the process alone.
Active private working set is shared with other processes.

How about the actual paged memory size? There's a Commit Size column that displays it, but Task Manager does not have it. However, you can have a look at it via Process Explorer.

## Base Priority
Base scheduling priority for threads
Idle (called Low in Task Manager) = 4
Below Normal = 6
Normal = 8
Above Normal = 10
High = 13
Real-time = 24

## Handles
Number of handles to kernel objects that are used by the process

## Threads
Shows the number of threads in a process. A process should at least have one thread
Secure  system process has no threads, because normal kernel is used for scheduling. System interrupts pseudo process is not a process, so it can't have threads. System Idle Process doesn't own a thread either. The number unde threads tab is the number of logical processors in the computer.
