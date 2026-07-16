# Day 07 – Linux File System Hierarchy & Scenario-Based Practice

### Part 1: Linux File System Hierarchy

**Core Directories (Must Know):**
- `/` (root) - The starting point of the entire linux system
- `/home` - User home directories
- `/root` - Root user's home directory(administrator)
- `/etc` - All Configuration files
- `/var/log` - Log files (very important for DevOps!)
- `/tmp` - Temporary files(cleared on reboot)

**Additional Directories (Good to Know):**
- `/bin` - Essential command binaries
- `/usr/bin` - User command binaries
- `/sbin` - Admin command binaries
- `/opt` - Optional/third-party applications
- `/dev` - Hardware devices as files
- `/proc` - Live kernel and process information
- `/mnt` - Manually mounted filesystems
- `/media` - Auto-mounted removable devices

> In Linux, **everything is a file** — hardware, processes, configs, logs. Understanding where things live is essential for any DevOps engineer.


---

### Part 2: Scenario-Based Practice

---

**Scenario 1: Service Not Starting** 
```
A web application service called 'myapp' failed to start after a server reboot.
What commands would you run to diagnose the issue?
Write at least 4 commands in order.
```
## Step-by-step solution:

#### Step 1 — Check service status

`systemctl status docker`

<img width="1001" height="402" alt="01-systemctl-status" src="https://github.com/user-attachments/assets/9454dab3-8c24-447e-96a3-9e616b208d3c" />

> Always the first command. Shows state (active/failed/inactive) + last 10 log lines in one view.
> Look for `Active: failed (Result: exit-code)` or a red dot `●`.

#### Step 2 — Check if service is configured to start automatically at boot

`systemctl is-enabled docker`

<img width="362" height="73" alt="02-isenabled-status" src="https://github.com/user-attachments/assets/d73fdb02-0def-4a80-ac8c-69e6f89236e7" />

> If output is `disabled` — service was never configured to auto-start. Survives a manual start but **dies after every reboot**.

#### Step 3 — Read service logs

```bash
journalctl -u docker -n 50
journalctl -u docker --since "today" --no-pager
```

<img width="992" height="663" alt="03-journalctl" src="https://github.com/user-attachments/assets/3276162c-8a74-4664-b103-f814e378d739" />

> Common errors you'll find here: `bind: Address already in use` (port conflict) · `No such file or directory` (missing config) · `Permission denied` (wrong file ownership)

#### Step 4 — Check for port conflicts

```bash
ss -tulnp | grep 80
```
> If another process already holds port 80, nginx cannot bind to it and fails to start. This command shows every listening port and which process owns it.


### Step 5 — Start service and watch live

```bash
sudo systemctl start docker.service
journalctl -u docker.service -f
```

<img width="1002" height="331" alt="04-servicestart" src="https://github.com/user-attachments/assets/d1c737a0-84c4-4677-8d4a-0c0621a0b574" />

> Start the service, then immediately follow logs with `-f`. You'll see the exact error the moment it occurs.


---

**Scenario 2: High CPU Usage** 
```
Your manager reports that the application server is slow.
You SSH into the server. What commands would you run to identify
which process is using high CPU?
```
**Step-by-step solution:**

---

**Scenario 3: Finding Service Logs** 
```
A developer asks: "Where are the logs for the 'docker' service?"
The service is managed by systemd.
What commands would you use?
```
**Step-by-step solution:**

---

**Scenario 4: File Permissions Issue** 
```
A script at /home/user/backup.sh is not executing.
When you run it: ./backup.sh
You get: "Permission denied"

What commands would you use to fix this?
```

**Step-by-step solution:**

Step 1: Check current permissions
Command: ls -l /home/user/backup.sh
Look for: -rw-r--r-- (notice no 'x' = not executable)

Step 2: Add execute permission
Command: chmod +x /home/user/backup.sh

Step 3: Verify it worked
Command: ls -l /home/user/backup.sh
Look for: -rwxr-xr-x (notice 'x' = executable)

Step 4: Try running it
Command: ./backup.sh


---
