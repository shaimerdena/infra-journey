# Managing Disks and Filesystems

> Notes based on Linux Bible and personal learning experiments.

Table of Contents:
- [Managing Disks and Filesystems](#managing-disks-and-filesystems)
  - [Partitioning Hard Disks](#partitioning-hard-disks)
    - [Partition tables](#partition-tables)
    - [Viewing Disk Partitions](#viewing-disk-partitions)
    - [Device Naming](#device-naming)

## Partitioning Hard Disks

### Partition tables 

A **partition table** is a data structure stored on a disk that describes how the disk is divided into partitions. A partition table is stored at the **beginning of a disk**. It does **not** store user data—it only describes the disk layout. Before creating partitions, a disk must have a partition table.

It contains information such as:
- the number of partitions;
- the starting and ending location of each partition;
- the partition type;
- whether a partition is bootable.

Without a partition table, the operating system cannot determine where partitions begin or end.

Common Partition Table Types

| Type | Description |
|------|-------------|
| **MBR (Master Boot Record)** | Legacy partition table. Supports up to **4 primary partitions** and disks up to **2 TB**. |
| **GPT (GUID Partition Table)** | Modern partition table. Supports **up to 128 partitions**, disks larger than **2 TB**, and provides better reliability through redundancy and error checking. |

### Viewing Disk Partitions

Linux provides several commands to view disks and partitions.

**`parted -l`**

Displays general information about disks:

- disk model;
- disk size;
- partition table (GPT/MBR);
- partitions;
- filesystem type.

---

**`fdisk -l`**

Displays detailed partition information:

- partition name;
- partition size;
- partition type;
- sector information.

---

**`df -h`**

Displays mounted filesystems and disk usage.

Shows:

- filesystem;
- total size;
- used space;
- available space;
- mount point.

---

### Device Naming

**SATA / USB**

```text
/dev/sda
/dev/sdb
/dev/sdc
```

Partitions:

```text
/dev/sda1
/dev/sda2
```

**NVMe SSD**

```text
/dev/nvme0n1
```

Partitions:

```text
/dev/nvme0n1p1
/dev/nvme0n1p2
```

---
<i>Note: USB drives are usually assigned the first available `/dev/sdX` device (e.g., `/dev/sda`).</i>