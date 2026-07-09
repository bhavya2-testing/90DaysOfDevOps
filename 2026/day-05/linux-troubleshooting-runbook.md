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

 - Purpose : To locate Live CPU usage or  memory hogging processes.

#### Command #2 – Service-specific CPU & Memory overview.
`free -h`

- Purpose : Generates report on the computer's total memory status.

#### Command #3 - Service-specific CPU & memory.
`pgrep sshd`

  - Purpose : To get PID of sshd

#### Command #4 - CPU and memory percentage for sshd.
`ps -o pid,pcpu,pmem,comm -p <PID>`

- Purpose : Gives an exact CPU/memory footprint for your service instead of global system averages.

### 4. Disk & I/O snapshot
#### Command #1 - Disk usage
`df -h`

- Purpose : To check disk filesystem space usage in human-readable sizes.

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

