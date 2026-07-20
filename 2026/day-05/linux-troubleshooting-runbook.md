# Linux Troubleshooting Runbook – sshd

## Target service/process
Service : sshd(OpenSSH server)
Reason: Core remote access service; easy to validate via network and logs.

### 1. Environment basics
#### Command #1 - Kernel and system info.
`uname -a`

  - Purpose : Show kernel version name, version and archiecture.
  - Output : `Linux ubuntu 6.8.0-124-generic #124-Ubuntu SMP PREEMPT_DYNAMIC Tue May 26 13:00:45 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux`
  - Observation : 🏃‍♀️Linux OS Kernel with host name(ubuntu) and the kernel build #(6.8.0-124-generic) and version(#124-Ubuntu) and the CPU of 64 bit.

| Field             | Example                              | Description                                          |
| ----------------- | ------------------------------------ | ---------------------------------------------------- |
| Operating system  | `Linux`                              | Kernel/OS name                                       |
| Hostname          | `ubuntu`                             | System's network hostname                            |
| Kernel release    | `6.8.0-45-generic`                   | Kernel version number                                |
| Kernel version    | `#45-Ubuntu SMP PREEMPT_DYNAMIC ...` | Build information and compilation details            |
| Machine hardware  | `x86_64`                             | CPU architecture                                     |
| Processor         | `x86_64`                             | Processor type (may be `unknown` on some systems)    |
| Hardware platform | `x86_64`                             | Hardware platform (may be `unknown` on some systems) |
| Operating system  | `GNU/Linux`                          | Operating system name                                |

uname -s – Kernel name
uname -n – Hostname
uname -r – Kernel release
uname -v – Kernel version
uname -m – Machine architecture
uname -p – Processor type
uname -i – Hardware platform
uname -o – Operating system

#### Command #2 - OS Version
`cat /etc/os-release`

  - Purpose : Show the OS version information.
  - Output : `PRETTY_NAME="Ubuntu 24.04.4 LTS" NAME="Ubuntu" VERSION_ID="24.04" VERSION="24.04.4 LTS (Noble Numbat)" VERSION_CODENAME=noble ID=ubuntu
    ID_LIKE=debian`
  - Observation : Confirms the OS information running on the machine.
    
### 2. Filesystem Sanity Check
`mkdir -p /tmp/runbook-demo && cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo`

  - Purpose : Directory creation if one does not exists, copying the hosts file and listing the /tmp/runbook-demo directory contents.
  - Output : `-rw-r--r-- 1 root root 255 Jul  9 14:41 hosts-copy`
  - Observation : Directory creation and file copy succeeded seamlessly. /tmp is writable, and basic I/O operations are operational.

### 3.CPU and Memory Snapshot
#### Command #1 - Live CPU and Memory overview.
`htop`

 - Purpose : To locate Live CPU usage or memory hogging processes.
 - htop is an interactive process monitoring tool for Linux. It provides a real-time view of system resources, running processes, CPU and memory usage, and allows you to manage processes from the terminal
 - CPU usage for each core
    - Memory (RAM) usage
    - Swap usage
    - Load average
    - Uptime
- A list of running processes with details such as:
    -  PID
    -  User
    -  CPU%
    -  Memory%
    -  Running time
    -  Command
#### Command #2 – Service-specific CPU & Memory overview.
`free -h`

- Purpose : The free -h command displays the system's memory (RAM) and swap usage in a human-readable format.

#### Command #3 - Service-specific CPU & memory.
`pgrep sshd`

  - Purpose : Searches for running processes by name or other attributes and prints their Process IDs (PIDs)

| Option | Description                           | Example                    |
| ------ | ------------------------------------- | -------------------------- |
| `-l`   | Show PID and process name             | `pgrep -l sshd`            |
| `-a`   | Show PID and full command line        | `pgrep -a sshd`            |
| `-f`   | Match against the full command line   | `pgrep -f "python app.py"` |
| `-x`   | Match the exact process name          | `pgrep -x sshd`            |
| `-u`   | Match processes owned by a user       | `pgrep -u root sshd`       |
| `-U`   | Match by real user ID                 | `pgrep -U root`            |
| `-P`   | Match child processes of a parent PID | `pgrep -P 1234`            |
| `-o`   | Return the oldest matching process    | `pgrep -o nginx`           |
| `-n`   | Return the newest matching process    | `pgrep -n nginx`           |
| `-c`   | Count matching processes              | `pgrep -c sshd`            |
| `-v`   | Invert the match                      | `pgrep -v sshd`            |

#### Command #4 - CPU and memory percentage for sshd.
`ps -o pid,pcpu,pmem,comm -p <PID>`

- Purpose : Gives an exact CPU/memory footprint for your service instead of global system averages.


| Component            | Description                                                                                                    |
| -------------------- | -------------------------------------------------------------------------------------------------------------- |
| `ps`                 | Displays information about currently running processes.                                                        |
| `-o`                 | Specifies a custom output format by selecting the columns to display.                                          |
| `pid,pcpu,pmem,comm` | The list of columns to display: Process ID, CPU usage, Memory usage, and Command name.                         |
| `-p <PID>`           | Shows information only for the specified process ID. Replace `<PID>` with an actual PID (for example, `1234`). |


| Column             | Meaning                                                     |
| ------------------ | ----------------------------------------------------------- |
| `PID`              | Unique Process ID.                                          |
| `%CPU` (`pcpu`)    | Percentage of CPU currently used by the process.            |
| `%MEM` (`pmem`)    | Percentage of physical memory (RAM) used by the process.    |
| `COMMAND` (`comm`) | Executable name of the process (not the full command line). |


### 4. Disk & I/O snapshot
#### Command #1 - Disk usage
`df -h`

- Purpose : To check disk **filesystem** space usage in human-readable sizes.
  - df = Disk Filesystem
  - h = Human-readable output
```bash
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           191M  996K  190M   1% /run
/dev/vda1        19G  5.4G   14G  29% /
tmpfs           952M   84K  952M   1% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/vda16      881M  117M  703M  15% /boot
/dev/vda15      105M  6.2M   99M   6% /boot/efi
```

| Column         | Description                                              |
| -------------- | -------------------------------------------------------- |
| **Filesystem** | The device or filesystem being used (e.g., `/dev/sda2`). |
| **Size**       | Total size of the filesystem.                            |
| **Used**       | Space currently used.                                    |
| **Avail**      | Space available for users.                               |
| **Use%**       | Percentage of the filesystem in use.                     |
| **Mounted on** | Directory where the filesystem is mounted.               |

**df vs du**
| Command              | Purpose                                                    |
| -------------------- | ---------------------------------------------------------- |
| `df -h`              | Shows free and used space for entire mounted filesystems.  |
| `du -sh <directory>` | Shows the disk space used by a specific directory or file. |


#### Command #2 - Log directory size
`du -sh /var/log`

- Purpose : Tell much of disk space the /var/log directory is using.

### 5. Network snapshot
#### Command #1 – Listening ports and service status
`sudo ss -tulpn | grep ssh`

- Purpose : Confirms sshd is listening on expected port (usually 22) and no port conflict

#### Command #2 – Connectivity check
`curl -v telnet://localhost:22`

- Purpose : Checks if port 22 is open.
  
### 5. Logs reviewed

### Command #1 - Recent service logs via journalctl
`journalctl -u ssh -n 50` OR `journalctl -u sshd -n 50`

- Purpose: Shows recent restarts, failures, auth issues, or configuration errors.

### Command #2 – Log file tail
`tail -n 50 /var/log/auth.log`

- Purpose: Shows last 50 lines from the auth log, which includes SSH login attempts.

#### Quick review

- ssh service running normally with low CPU usage
- Disk and logs size is healthy
- Network port 22 is open and serving connections.
- No errors in logs.

#### If this worsens

- Check logs again
- Check CPU usage/Disk usage
- Restart service
- Check if port is used by other service

