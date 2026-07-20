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
- # `sudo ss -tulpn | grep ssh`

Displays SSH-related listening sockets and the process using them.

## Syntax

```bash
sudo ss -tulpn | grep ssh
```

## Command Breakdown

| Component | Description |
|-----------|-------------|
| `sudo` | Runs the command with administrative privileges to view all processes. |
| `ss` | Displays socket statistics (replacement for `netstat`). |
| `-t` | Show TCP sockets. |
| `-u` | Show UDP sockets. |
| `-l` | Display only listening sockets. |
| `-p` | Show the process (PID/program name) using each socket. |
| `-n` | Display numeric addresses and port numbers instead of resolving names. |
| `grep ssh` | Filter the output to show only SSH-related entries. |

## Example Output

```bash
tcp   LISTEN 0      128          0.0.0.0:22         0.0.0.0:*      users:(("sshd",pid=1023,fd=3))
tcp   LISTEN 0      128             [::]:22            [::]:*      users:(("sshd",pid=1023,fd=4))
```

## Output Fields

| Field | Description |
|-------|-------------|
| `tcp` | Protocol being used (TCP). |
| `LISTEN` | Socket state indicating the service is waiting for incoming connections. |
| `0` | Number of queued received packets. |
| `128` | Maximum connection backlog (queue size). |
| `0.0.0.0:22` | Listening on all IPv4 interfaces on port 22. |
| `[::]:22` | Listening on all IPv6 interfaces on port 22. |
| `users:(("sshd",pid=1023,fd=3))` | Process name, Process ID (PID), and file descriptor using the socket. |

## Commonly Used `ss` Options

| Option | Description |
|--------|-------------|
| `-t` | Show TCP sockets only. |
| `-u` | Show UDP sockets only. |
| `-l` | Display only listening sockets. |
| `-a` | Display all sockets (listening and non-listening). |
| `-p` | Show the process using each socket. |
| `-n` | Show numeric IP addresses and port numbers. |
| `-s` | Display socket statistics summary. |
| `-4` | Show IPv4 sockets only. |
| `-6` | Show IPv6 sockets only. |



#### Command #2 – Connectivity check
# `curl -v telnet://localhost:22`

Tests TCP connectivity to port **22** (SSH) on the local machine using `curl`'s Telnet protocol support. The `-v` option enables verbose output, allowing you to verify whether the SSH port is reachable.

> **Note:** This command **does not establish an SSH session**. It only checks whether a TCP connection to the SSH port can be made.

## Syntax

```bash
curl -v telnet://localhost:22
```

## Command Breakdown

| Component | Description |
|-----------|-------------|
| `curl` | Command-line tool for transferring data using various protocols. |
| `-v` | Enables verbose output, showing connection details and protocol negotiation. |
| `telnet://` | Uses the Telnet protocol handler to open a raw TCP connection. |
| `localhost` | Connects to the local machine (`127.0.0.1`). |
| `:22` | Specifies port **22**, the default SSH port. |

## Example Output

```bash
* Trying 127.0.0.1:22...
* Connected to localhost (127.0.0.1) port 22 (#0)
SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.8
```

## Output Fields

| Output | Description |
|--------|-------------|
| `Trying 127.0.0.1:22...` | Attempts to connect to the SSH port. |
| `Connected to localhost` | TCP connection was established successfully. |
| `SSH-2.0-OpenSSH_...` | SSH server banner indicating the SSH service is running. |

## Common `curl` Options

| Option | Description |
|--------|-------------|
| `-v` | Enable verbose output. |
| `-I` | Fetch HTTP headers only. |
| `-O` | Save the file using its remote filename. |
| `-o <file>` | Save output to a specified file. |
| `-L` | Follow HTTP redirects. |
| `-k` | Ignore SSL/TLS certificate validation. |
| `-u user:password` | Authenticate using a username and password. |
| `--connect-timeout <seconds>` | Set the connection timeout. |

  
### 5. Logs reviewed

### Command #1 - Recent service logs via journalctl
# `journalctl -u ssh -n 50` / `journalctl -u sshd -n 50`

Displays the **last 50 log entries** for the SSH service using `systemd` journal logs.

The service name depends on the Linux distribution:

- Debian/Ubuntu commonly use: `ssh`
- RHEL/CentOS/Fedora commonly use: `sshd`

## Syntax

```bash
journalctl -u <service-name> -n 50
```

Examples:

```bash
journalctl -u ssh -n 50
```

or

```bash
journalctl -u sshd -n 50
```

---

## Command Breakdown

| Component | Description |
|-----------|-------------|
| `journalctl` | Command used to query and display logs collected by `systemd-journald`. |
| `-u` | Filters logs for a specific systemd unit/service. |
| `ssh` / `sshd` | The SSH service unit name. |
| `-n 50` | Displays the latest 50 log entries. |

---

## Example Output

```bash
Mar 20 10:15:01 server sshd[1023]: Server listening on 0.0.0.0 port 22.
Mar 20 10:16:45 server sshd[2045]: Accepted password for user1 from 192.168.1.10 port 54321 ssh2
Mar 20 10:17:02 server sshd[2045]: pam_unix(sshd:session): session opened for user user1
```

---

## Output Fields

| Field | Description |
|-------|-------------|
| Timestamp | Date and time when the event occurred. |
| Hostname | Name of the server generating the log. |
| Service | Service generating the message (`sshd`). |
| PID | Process ID of the SSH daemon instance. |
| Message | Log event details (login, error, connection, etc.). |

---

## Commonly Used `journalctl` Options

| Option | Description | Example |
|--------|-------------|---------|
| `-u <service>` | Show logs for a specific service | `journalctl -u sshd` |
| `-n <number>` | Show the latest N log lines | `journalctl -n 100` |
| `-f` | Follow logs in real time | `journalctl -f` |
| `-b` | Show logs from current boot | `journalctl -b` |
| `-p <level>` | Filter by priority | `journalctl -p err` |
| `--since` | Show logs after a specific time | `journalctl --since "1 hour ago"` |
| `--until` | Show logs before a specific time | `journalctl --until "today"` |
| `--no-pager` | Display output directly without a pager | `journalctl --no-pager` |
| `-xe` | Show recent errors with explanations | `journalctl -xe` |

---

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

