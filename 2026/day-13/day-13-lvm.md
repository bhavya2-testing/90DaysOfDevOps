
# Day 13 – Linux Volume Management (LVM)

## Task (What is LVM?)

LVM (Logical Volume Manager) A storage management system that lets you create, resize, and manage disk space flexibly. Instead of partitioning disks directly, LVM lets you create resizable volumes that can span multiple disks.

```
Physical Disk → Physical Volume (PV) → Volume Group (VG) → Logical Volume (LV) → Filesystem
```

| Layer | Component | Description |
|---|---|---|
| **PV** | Physical Volume | A physical disk or partition initialized for use by LVM. |
| **VG** | Volume Group | A pool of storage created by combining one or more Physical Volumes |
| **LV** | Logical Volume | A virtual partition created from a Volume Group that is formatted and mounted like a normal disk partition. |

---

## SetUp
No spare disk use? Loopback disk image is used to simulate a physical disk.
A loop device is just another block device as far as LVM is concerned, so the same commands (pvcreate, vgcreate, lvcreate, lvextend, etc.) work exactly the same way.

**Commands:**
```bash
# Create a 1GB blank disk image
dd if=/dev/zero of=/tmp/disk1.img bs=1M count=1024

# Attach the image as a loop device
losetup -fP /tmp/disk1.img

# Confirm which loop device was assigned
losetup -a
```

**Output:**
```
/dev/loop0: []: (/tmp/disk1.img)
```

> Our simulated disk is `/dev/loop0` — used for all LVM tasks below.

<img width="662" height="294" alt="01-loopdevice-creation" src="https://github.com/user-attachments/assets/f01429a3-de10-4f3d-a2c6-b26ff6ff9f64" />

---

## Challenge Tasks

### Task 1: Check Current Storage
Run: `lsblk`, `pvs`, `vgs`, `lvs`, `df -h`

Inspect the current state of disks, volumes, and mounted filesystems before making any changes.

**Commands:**
```bash
lsblk        # Show all block devices
pvs          # List Physical Volumes
vgs          # List Volume Groups
lvs          # List Logical Volumes
df -h        # Show mounted filesystems and usage
```

**Observations:**
- `lsblk` — showed `/dev/loop0` as a 1G disk alongside system disks
- `pvs` / `vgs` / `lvs` — no LVM objects exist yet
- `df -h` — `/mnt/app-data` does not exist yet

<img width="994" height="809" alt="02-current-stroage-status" src="https://github.com/user-attachments/assets/b0637b80-42a5-45a4-9cb7-046e4a0f33cf" />


### Task 2: Create Physical Volume

Mark the loop device as an LVM Physical Volume.

**Commands:**
```bash
sudo pvcreate /dev/loop0
sudo pvs
```

**Output:**
```
  Physical volume "/dev/loop0" successfully created.

  PV          VG  Fmt  Attr PSize  PFree
  /dev/loop0      lvm2 ---  1.00g  1.00g
```
<img width="710" height="82" alt="03-physicalcreate" src="https://github.com/user-attachments/assets/6e10759c-f989-482a-8b50-334142f85fcc" />



### Task 3: Create Volume Group

Create a Volume Group named `devops-vg` using the Physical Volume.

**Commands:**
```bash
sudo vgcreate devops-vg /dev/loop0
sudo vgs
```

**Output:**
```
  Volume group "devops-vg" successfully created

  VG         #PV #LV #SN Attr   VSize    VFree
  devops-vg    1   0   0 wz--n- 1020.00m 1020.00m
```

<img width="403" height="126" alt="04-groupcreate" src="https://github.com/user-attachments/assets/ceb5ca84-c4c7-4e5e-8e7e-b54befb9f69c" />

> The VG pools all PV storage into a single manageable unit. All Logical Volumes will be carved from this pool.


### Task 4: Create Logical Volume

Carve a 500MB Logical Volume named `app-data` from `devops-vg`.

**Commands:**
```bash
sudo lvcreate -L 500M -n app-data devops-vg
sudo lvs
```

**Output:**
```
  Logical volume "app-data" created.

  LV       VG         Attr       LSize
  app-data devops-vg  -wi-a----- 500.00m
```
<img width="730" height="110" alt="05-logicalcreate" src="https://github.com/user-attachments/assets/76fb8404-0016-452c-8d35-f83b297a5a17" />

> The LV acts like a virtual partition. It can be formatted with any filesystem and mounted like a regular disk.


### Task 5: Format and Mount
Format the Logical Volume with `ext4` and mount it.

**Commands:**
```bash
# Format with ext4 filesystem
sudo mkfs.ext4 /dev/devops-vg/app-data

# Create the mount point
sudo mkdir -p /mnt/app-data

# Mount the LV
sudo mount /dev/devops-vg/app-data /mnt/app-data

# Verify
df -h /mnt/app-data

# Verify
lsblk
```

<img width="562" height="577" alt="06_mountlv" src="https://github.com/user-attachments/assets/85ec4342-ac4e-4e33-9e8f-a5ea4b6db519" />


**Output:**


**Test file creation:**
```bash
touch /mnt/app-data/testfile
ls /mnt/app-data
```


<img width="497" height="236" alt="07-Filecreation" src="https://github.com/user-attachments/assets/e0442a9e-3f18-4cb2-9eda-5db0a9d538ce" />

---
### Task 6: Extend the Volume
```bash
lvextend -L +200M /dev/devops-vg/app-data
resize2fs /dev/devops-vg/app-data
df -h /mnt/app-data
```

---
What I learned

---
