Core components of Linux
############ KERNEL ######################
- Heart of linux
- Acts as a bridge between the physical hardware and the software running on the compuer

############ BOOTLOADER ##################
- The software responsible for booting the computer
- It manages the initial setup process,handing control over to the kernel

############ THE INIT SYSTEM (systemd)##############
- The very first process started by the kernel
- It is responsible for bootstraping the userspace and manageing all the background services(daemons)

############ THE SHELL ####################
- The userspace interface that accepts commands from the user and excutes them by interating with the kernel

**How process are created and managed**

PROCESS CREATION
- Linux uses a distinct fork() and exec() mechanism to create new processes.
- Instead of creating a brand new program for scratch, an existing processes clones itslef
PROCESS MANAGEMENT
- Linux system admins track, proritize and terminate runnning tasks through a varierty of command line tools.
- With use of commands like
    - ps aux
    - top
    - htop


Process States: 5 primary states
#1 - Running (R - TASK_RUNNING)
   ----- The process is either actively running on CPU core  OR
   ----- The process is sitting in the queue ready to execute as soon as time slice becomes available
#2 - Interruptible Sleep(S - TASK_INTERRUPTIBLE)
   ------ The process is temporarly inactive while waiting for an event or resource
   ------ such as keyboard input,a nertwork packet or a system timer
#3 - Uninterruptible Sleep(D - TASK_UNINTERRUPTIBLE)
   ------ The process is blocked and cannot be woken up by any signals including SIGKILL(kill -9)
   ------ Usually occurs during critical hardware I/O operations like waiting for slow disk drive to read data
#4 - Stopped(T - TASK_STOPPED)
   ------- The process has been completed suspended or paused usually by a user actions like pressing Ctrl+Z in the terminal or
   ------ sending a SIGSTOP signal
#5 - Zombie(Z - TASK_ZOMBIE)
   ------- The process has finished execution but still has an entry in the system process table
   ------- The process consumes zero CPU and memoery resources
   ------- The process lingers only because it's parent process hasn't collected it's exit status yet via the wait() system

5 Commands used 
########### Changes permissions of a file#############
chmod 

############ Displays files in a directory###########
ls 

############ Creates a new directory###########
mkdir

############ View all running processes on the system###########
ps aux

########### Check live process activiy and sort by CPU usage###########
top
