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

---

### Part 2: Scenario-Based Practice

---

**Scenario 1: Service Not Starting** 
```
A web application service called 'myapp' failed to start after a server reboot.
What commands would you run to diagnose the issue?
Write at least 4 commands in order.
```
**Step-by-step solution:**


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
