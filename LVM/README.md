**LVM (Logical Volume Manager)**
---

## 🔹 1. Basics of LVM

* **Definition**: LVM is a device mapper framework that provides logical volume management for Linux.
* **Why LVM?**

  * Resize partitions (grow/shrink).
  * Combine multiple physical disks into one logical pool.
  * Snapshots for backup.
  * Easy storage management.

---

## 🔹 2. LVM Components (Interview Keywords)

1. **Physical Volume (PV)**

   * Actual disk or partition initialized for LVM.
   * Command: `pvcreate /dev/sdb`

2. **Volume Group (VG)**

   * A pool of storage made from PVs.
   * Command: `vgcreate my_vg /dev/sdb /dev/sdc`

3. **Logical Volume (LV)**

   * The “virtual partition” created from a VG.
   * Command: `lvcreate -n my_lv -L 10G my_vg`

4. **File System**

   * After creating LV, format and mount it.
   * Example:

     ```bash
     mkfs.ext4 /dev/my_vg/my_lv
     mount /dev/my_vg/my_lv /mnt
     ```

---

## 🔹 3. Common LVM Commands

* **Check PV, VG, LV**

  ```bash
  pvs
  vgs
  lvs
  ```
* **Detailed info**

  ```bash
  pvdisplay
  vgdisplay
  lvdisplay
  ```
* **Extend Logical Volume**

  ```bash
  lvextend -L +5G /dev/my_vg/my_lv
  resize2fs /dev/my_vg/my_lv   # for ext4
  ```
* **Reduce Logical Volume** (⚠ risky)

  ```bash
  umount /mnt
  e2fsck -f /dev/my_vg/my_lv
  resize2fs /dev/my_vg/my_lv 5G
  lvreduce -L 5G /dev/my_vg/my_lv
  mount /dev/my_vg/my_lv /mnt
  ```
* **Extend VG with new disk**

  ```bash
  pvcreate /dev/sdd
  vgextend my_vg /dev/sdd
  ```
* **Snapshot**

  ```bash
  lvcreate -L 2G -s -n my_snap /dev/my_vg/my_lv
  ```

---

## 🔹 4. Practice Scenarios (Common in Interviews)

1. **Create an LVM from scratch**

   * Add `/dev/sdb` and `/dev/sdc` → PV → VG → LV → Format → Mount.

2. **Extend a filesystem when disk space runs out**

   * Add a new disk `/dev/sdd`, extend VG, extend LV, resize FS.

3. **Shrink a filesystem safely**

   * Unmount, check FS, shrink FS, reduce LV, remount.

4. **Create and restore from snapshot**

   * Take snapshot before making changes, rollback if needed.

5. **Migrate data from one disk to another (pvmove)**

   ```bash
   pvmove /dev/sdb /dev/sdc
   ```

---

## 🔹 5. Useful Interview Questions

* What is the difference between LVM and standard partitioning?
* What are the main components of LVM?
* How do you extend a filesystem on LVM?
* How do you shrink a logical volume?
* What is a snapshot? How does it work in LVM?
* How would you migrate data from one physical volume to another without downtime?
* Difference between **thick** and **thin** provisioning in LVM?

---

## 🔹 6. Hands-On Lab Setup

You can practice on a VM:

1. Attach extra disks (like `/dev/sdb`, `/dev/sdc`).
2. Follow full workflow:

   * Create PV → VG → LV → FS.
   * Extend/reduce volumes.
   * Create snapshots.

---

# 🔹 Step 1: Check available disks

```bash
lsblk
fdisk -l
```

👉 Output will show unused disks (`/dev/sdb`, `/dev/sdc`).

---

# 🔹 Step 2: Create Physical Volumes (PV)

```bash
pvcreate /dev/sdb /dev/sdc
```

👉 Marks the disks as LVM usable.

Check:

```bash
pvs
pvdisplay
```

---

# 🔹 Step 3: Create Volume Group (VG)

```bash
vgcreate my_vg /dev/sdb /dev/sdc
```

👉 Combines both disks into one storage pool.

Check:

```bash
vgs
vgdisplay my_vg
```

---

# 🔹 Step 4: Create Logical Volume (LV)

Example: Create 10 GB LV

```bash
lvcreate -n my_lv -L 10G my_vg
```

👉 Creates LV named `my_lv`.

Check:

```bash
lvs
lvdisplay /dev/my_vg/my_lv
```

---

# 🔹 Step 5: Format LV with a Filesystem

```bash
mkfs.ext4 /dev/my_vg/my_lv
```

---

# 🔹 Step 6: Mount LV

```bash
mkdir /mnt/mydata
mount /dev/my_vg/my_lv /mnt/mydata
df -h | grep mydata
```

---

# 🔹 Step 7: Extend LV + Filesystem

Say you need +5 GB more:

```bash
lvextend -L +5G /dev/my_vg/my_lv
resize2fs /dev/my_vg/my_lv
```

👉 Filesystem is now bigger.

---

# 🔹 Step 8: Reduce LV + Filesystem (⚠ risky)

1. Unmount:

   ```bash
   umount /mnt/mydata
   ```
2. Check & shrink FS first:

   ```bash
   e2fsck -f /dev/my_vg/my_lv
   resize2fs /dev/my_vg/my_lv 8G
   ```
3. Reduce LV:

   ```bash
   lvreduce -L 8G /dev/my_vg/my_lv
   ```
4. Mount again:

   ```bash
   mount /dev/my_vg/my_lv /mnt/mydata
   ```

---

# 🔹 Step 9: Extend VG with new disk

If a new disk `/dev/sdd` is added:

```bash
pvcreate /dev/sdd
vgextend my_vg /dev/sdd
```

---

# 🔹 Step 10: Create Snapshot

```bash
lvcreate -L 2G -s -n my_snap /dev/my_vg/my_lv
```

👉 Snapshot LV `my_snap` created.

Check:

```bash
lvs
```

Restore (if needed):

```bash
lvconvert --merge /dev/my_vg/my_snap
```

---

# 🔹 Step 11: Move Data to New Disk (No Downtime)

If you want to move everything from `/dev/sdb` → `/dev/sdc`:

```bash
pvmove /dev/sdb /dev/sdc
```

---
LVM provides flexible, dynamic storage by abstracting physical disks into a pool of space for logical volumes, which can be resized or moved without downtime, 
unlike standard partitioning's fixed partitions. Key components are Physical Volumes (PVs), which are raw disk partitions; 
Volume Groups (VGs), a pool of PVs; and Logical Volumes (LVs), which are the usable storage units created from the VG. 
To extend a filesystem, you first extend the LV and then resize the filesystem on it using appropriate tools. Shrinking an 
LV requires decreasing the filesystem size first, then the LV, and involves a risk of data loss. LVM snapshots create a 
point-in-time, read-only copy of an LV for backups or testing, working by creating a new LV that stores the changes made 
to the original. Data migration without downtime involves creating a new LV on the destination disk, copying data, and 
then switching the mount point. Thick provisioning allocates all space immediately, while thin provisioning allocates 
space only when data is written, enabling over-allocation but requiring careful monitoring to prevent out-of-space errors. 
 
LVM vs. Standard Partitioning 

• Standard Partitioning: Creates fixed-size partitions directly on a disk. Resizing requires unmounting and repartitioning, often causing downtime.  
• LVM: Pools physical storage from one or more disks into a Volume Group (VG). Administrators then create flexible Logical Volumes (LVs) 
from this pool, which can be resized on the fly and even span across multiple disks.  

Main Components of LVM 

• Physical Volume (PV): Raw disk partitions or disks that have been initialized to be used by LVM. 
• Volume Group (VG): A pool of storage created by combining one or more PVs.   
• Logical Volume (LV): The "virtual partition" that is created from the VG. It's the logical storage unit that the operating system uses to create a filesystem. 

Extending a Filesystem on LVM

1. Extend the Logical Volume: Use commands to increase the size of the LV within the VG.   
2. Resize the Filesystem: Use filesystem-specific tools (e.g., resize2fs for ext4) to expand the filesystem to use the newly available space in the LV.   

Shrinking a Logical Volume  

1. Shrink the Filesystem: First, use filesystem-specific tools to decrease the size of the filesystem. 
2. Shrink the Logical Volume: Then, use LVM commands to reduce the size of the LV itself. This is a high-risk operation that can lead to data loss, 
so it's crucial to ensure the filesystem is smaller than the target LV size. 

Snapshots in LVM [5, 32, 33]  

• What it is: A snapshot creates a point-in-time, read-only copy of a logical volume. 
• How it works: When a snapshot is created, the original LV is not copied. Instead, the LVM creates a new LV that acts as a "copy-on-write" area for 
any blocks in the original LV that are modified after the snapshot is taken. This allows you to back up a system even while applications are running. 

Migrating Data from one Physical Volume to another (Downtime) 

1. Create a new LV: Create a new logical volume on the destination physical disk. 
2. Copy Data: Copy the data from the original logical volume to the new one. 
3. Switch Mount Point: Unmount the original LV and mount the new LV in its place.  
4. Remove Old PV: Once the data is safely on the new LV and the original is no longer needed, the original physical volume can be removed from the LVM. 

Thick vs. Thin Provisioning in LVM 

• Thick Provisioning (Classic LVM): The entire space assigned to a logical volume is allocated and reserved immediately, even if it's 
not yet used. This guarantees space but can be less efficient. 
• Thin Provisioning: Only allocates storage space as data is actually written to the logical volume. This allows for 
"over-allocation" where the total provisioned space exceeds the physical capacity, but it requires careful monitoring to avoid running out of physical space if usage grows too rapidly. 
---

# 🔹 LVM Automation Script

```bash
#!/bin/bash
# LVM Automation Script
# ⚠️ WARNING: This will destroy data on /dev/sdb, /dev/sdc, /dev/sdd

set -e

VG_NAME="my_vg"
LV_NAME="my_lv"
MOUNT_DIR="/mnt/mydata"

echo "=== Step 1: Create Physical Volumes ==="
pvcreate /dev/sdb /dev/sdc

echo "=== Step 2: Create Volume Group ==="
vgcreate $VG_NAME /dev/sdb /dev/sdc

echo "=== Step 3: Create Logical Volume (10G) ==="
lvcreate -n $LV_NAME -L 10G $VG_NAME

echo "=== Step 4: Format LV with ext4 ==="
mkfs.ext4 /dev/$VG_NAME/$LV_NAME

echo "=== Step 5: Mount LV ==="
mkdir -p $MOUNT_DIR
mount /dev/$VG_NAME/$LV_NAME $MOUNT_DIR
echo "/dev/$VG_NAME/$LV_NAME $MOUNT_DIR ext4 defaults 0 0" >> /etc/fstab
df -h | grep $MOUNT_DIR

echo "=== Step 6: Extend LV (+5G) ==="
lvextend -L +5G /dev/$VG_NAME/$LV_NAME
resize2fs /dev/$VG_NAME/$LV_NAME
df -h | grep $MOUNT_DIR

echo "=== Step 7: Add New Disk (/dev/sdd) and Extend VG ==="
pvcreate /dev/sdd
vgextend $VG_NAME /dev/sdd
vgs

echo "=== Step 8: Create Snapshot ==="
lvcreate -L 2G -s -n ${LV_NAME}_snap /dev/$VG_NAME/$LV_NAME
lvs

echo "=== Step 9: Move Data from /dev/sdb to /dev/sdc ==="
pvmove /dev/sdb /dev/sdc

echo "=== LVM Setup Completed Successfully ==="
```

---

# 🔹 How to Use

1. Save script:

   ```bash
   nano lvm_lab.sh
   ```

   Paste script, save and exit.

2. Make executable:

   ```bash
   chmod +x lvm_lab.sh
   ```

3. Run as root:

   ```bash
   ./lvm_lab.sh
   ```

---

# 🔹 What This Script Does

✅ Creates PVs on `/dev/sdb` & `/dev/sdc`
✅ Creates VG `my_vg`
✅ Creates LV `my_lv` (10G)
✅ Formats and mounts it at `/mnt/mydata`
✅ Extends LV by +5G
✅ Adds a new disk `/dev/sdd` to VG
✅ Creates a 2G snapshot
✅ Moves data from `/dev/sdb` → `/dev/sdc`

---