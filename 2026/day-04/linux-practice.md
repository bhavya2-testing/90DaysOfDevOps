# Real Time Output of Commands I Practiced

## Hostname Commands

### 1. View Current Hostname

`hostnamectl` - Displays detailed hostname and system information.

<img width="515" height="316" alt="01-hostnamectl" src="https://github.com/user-attachments/assets/ea929c34-9a19-4bab-b9c5-18cffaa63a57" />

### 2. Change Hostname

`sudo hostnamectl set-hostname devops-server` - Changes the system hostname.

<img width="1218" height="621" alt="02-set-hostname" src="https://github.com/user-attachments/assets/2d1fa0ed-f506-4335-86a9-940995066eef" />
---

## Process Commands

### 3. Check System Uptime

`uptime` - Shows how long the system has been running and load averages.

<img width="730" height="71" alt="03-uptime" src="https://github.com/user-attachments/assets/fc56a175-3653-4620-9890-3f684c5e3cc3" />

### 4. Find High CPU Consuming Processes

`ps aux --sort=-%cpu | head` - Lists processes consuming the most CPU.

<img width="1007" height="292" alt="04-psaux" src="https://github.com/user-attachments/assets/2daf26f1-dbd2-436f-b75a-4b1750d6cc58" />

### 5. Display Process Tree

`pstree -p` - Shows parent-child relationship between running processes.

<img width="583" height="677" alt="05-parent-child-pstree" src="https://github.com/user-attachments/assets/3416c2b6-982f-426f-b744-108833414acd" />

---

## Service Commands

### 6. List Running Services

`systemctl list-units --type=service --state=running`

Shows all currently running services.

<img width="1440" height="900" alt="06-systemctl" src="https://github.com/user-attachments/assets/5615fb52-66d6-436e-b716-83aeae20eef5" />

### 7. Check SSH Service Status

`systemctl status ssh`

Displays detailed information about the SSH service.

<img width="993" height="403" alt="07-systemctl-status" src="https://github.com/user-attachments/assets/41654a77-2992-429b-a5e9-825b6895f7f6" />


### 8. View SSH Service Dependencies

`systemctl list-dependencies ssh`

Shows services and targets required by SSH.

<img width="426" height="725" alt="08-systemctl-dependencies" src="https://github.com/user-attachments/assets/fa161520-26d6-4a6e-8040-7f9aef1d5ea8" />


## Log Commands

### 9. View SSH Logs

`journalctl -u ssh -n 20`

Displays the latest SSH service logs.

<img width="816" height="398" alt="09-sshlogs" src="https://github.com/user-attachments/assets/987d775e-47b5-4585-83de-351cf78fb164" />

### 10. View Error Logs

`journalctl -p err -b`

Shows boot-time error messages.

<img width="775" height="77" alt="10-systemctl-error" src="https://github.com/user-attachments/assets/d1ba9741-ace6-4f83-914e-28a3f0c9504c" />

---

## Storage Commands

### 11. Check Disk Usage

`df -h`

Displays filesystem usage in human-readable format.

<img width="447" height="197" alt="11-diskfree" src="https://github.com/user-attachments/assets/83aa2572-f064-4ef5-aefb-8920352e04c0" />

---
## What I Learned Today

- Learned how Linux manages processes and services.
- Explored process monitoring using `ps`, `uptime`, and `pstree`.
- Inspected SSH service status and dependencies.
- Viewed system and service logs using `journalctl`.
- Checked disk utilization using `df -h`.
- Changed the EC2 hostname using `hostnamectl`.
