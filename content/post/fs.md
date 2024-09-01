+++
author = "Hugo Authors"
title = "File Systems 💾"
date = "2024-08-25"
description = "A huge note I made about Unix File System"
tags = [
  "Tech", "Linux", "Unix", "Administration", "Engineering", "FileSystems", "Notes"
]
categories = [
    "Learning",
]
series = ["About Me"]

+++

## 1. ext4 💾 (Linux, BSD)
### Intro
**ext4** (Extended File System version 4) is the default file system for many Linux distributions.
### Links
https://en.wikipedia.org/wiki/Ext4
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/6/html/storage_administration_guide/ch-ext4#ch-ext4
### Technical Features: 🔍
- Journaling
- Extent-Based Allocation
- Delayed Allocation
- Persistent Pre-allocation
- Multi-block Allocation
- Online Resizing
- 64-bit File System Support
- Directory Indexing with HTree
- Defragmentation
- Backward Compatibility with ext2/ext3
- Barriers for Data Integrity
- Large File Support (up to 16 TiB)
- Metadata Checksumming (optional)
- Quotas
### Advantages 👍
- **Mature and Stable**: ext4 is a well-tested and widely-used file system with a long history of stability.
- **Performance**: It offers good performance for most workloads, especially for general-purpose usage.
- **Backward Compatibility**: Supports ext3 and ext2 file systems, making it easy to upgrade.
- **Journaling**: Provides a journaling feature that helps to prevent data corruption in case of a crash.
- **Wide Support**: Supported by almost all Linux distributions and has a large community.
### Downsides 👎
- **Limited Scalability**: While adequate for most users, ext4 doesn't scale as well as newer file systems for very large volumes and large numbers of files.
- **Lack of Advanced Features**: ext4 lacks features like snapshotting and built-in data integrity checks (e.g., checksums).
### Scale
- **Maximum File Size**: 16 TiB
- **Maximum Volume Size**: 1 EiB
### Distro Usage
- ext4 is the most widely used format spanning Linux and BSD.
## 2. XFS 💾 (Linux, BSD)
### Intro
**XFS** is a high-performance file system designed for parallel I/O operations, often used in enterprise environments.
### Links
https://en.wikipedia.org/wiki/XFS
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-xfs
### Technical Features: 🔍
- Extent-Based Allocation
- Journaling (Metadata Journaling)
- Delayed Allocation
- Persistent Pre-allocation
- Online Resizing (grow only)
- Dynamic Inode Allocation
- B+ Tree Directory Structure - (Quick Access B Tree)
- Direct I/O Support
- Data Striping for Performance
- Advanced Metadata Management
- Large File and Volume Support (up to 8 EiB)
- Online Defragmentation
- Quotas and Project Quotas
- Realtime Subvolume for Real-Time I/O
### Advantages 👍
- **High Performance**: Optimized for large files and supports high-performance parallel I/O, making it ideal for environments with large data sets.
- **Scalability**: Scales well with large volumes and large numbers of files, supporting file systems up to 500 TB.
- **Journaling**: Uses journaling to help prevent data corruption.
- **Online Resizing**: Supports online resizing of file systems (only grow).
### Downsides 👎
- **Complexity**: XFS is more complex to manage compared to ext4.
- **Limited Snapshot Support**: Has limited support for snapshots compared to Btrfs and OpenZFS.
- **Potential Data Loss on Power Failure**: In certain configurations, XFS may be more susceptible to data loss in the event of a sudden power loss.
### Technical Details 🔍
- **Maximum File Size**: 8 EiB
- **Maximum Volume Size**: 8 EiB
### Distro Usage
XFS has been in the Linux Kernel since 2001
It is the default file system for RHEL
## 3. Btrfs 💾 (Linux)
### Intro
Btrfs (B-tree File System) is a modern, copy-on-write file system designed for Linux that offers advanced features like snapshots, RAID support, self-healing, and efficient storage management, making it suitable for scalable and reliable data storage.
### Links
https://en.wikipedia.org/wiki/Btrfs
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/6/html/storage_administration_guide/ch-btrfs
https://docs.kernel.org/filesystems/btrfs.html
### Technical Features: 🔍
- Journaling
- Extent Base Allocation
- Persistent Pre-allocation
- Delayed Allocation
- Multi-block Allocation
- Stripe-aware Allocation
- Resizeable with resize2fs
- *B-tree Balancing Algorithm - Different from XFS (COW B Tee)
- Copy-on-Write (COW)
- Snapshots and Clones
- Built-in RAID Support
- Data and Metadata Checksumming
- Self-Healing
- Dynamic Subvolumes
- Online Resizing
- Compression (LZO. ZLIB. ZSTD)
- Deduplication
### Advantages 👍
- **Snapshot Support**: Provides built-in support for snapshots, allowing for quick backups and rollbacks.
- **Data Integrity**: Includes checksumming of data and metadata, which helps to ensure data integrity.
- **Self-Healing**: With RAID support, Btrfs can automatically repair corrupted data.
- **Dynamic Storage**: Allows for the dynamic addition and removal of storage devices.
### Downsides 👎
- **Stability**: Btrfs is considered less mature than ext4 or XFS, particularly for certain features like RAID 5/6.
- **Performance**: May not perform as well as XFS or ext4 in certain high-performance scenarios, particularly with heavy random writes.
- **Complexity**: The advanced features of Btrfs come with increased complexity.
### Technical Details 🔍
- **Maximum File Size**: 16 EiB
- **Maximum Volume Size**: 16 EiB
- **Better on SSDs**: Btrfs is well-suited for flash/solid-state storage because of TRIM support and CoW, which reduces write amplification.
### Distro Usage
Btrfs has been in the mainline linux Kernel since 2008
it is the default file system for SUSE and Fedora
## 4. OpenZFS 💾 (Unix)
### Intro
**OpenZFS** is an advanced file system and volume manager that originated from Sun Microsystems' ZFS and is now maintained by the OpenZFS project.
### Links
https://en.wikipedia.org/wiki/OpenZFS
https://openzfs.org/wiki/Main_Page
### Technical Features: 🔍
- Copy-on-Write (COW)
- Snapshots and Clones
- Pooled Storage (ZFS Storage Pools)
- Dynamic Striping
- Built-in RAID Support (RAID-Z1, RAID-Z2, RAID-Z3)
- Data and Metadata Checksumming
- Self-Healing
- Deduplication
- Compression (LZ4, GZIP, ZLE, etc.)
- Online Resizing
- Dynamic Block Size
- End-to-End Data Integrity
- ZFS Datasets (File Systems and Volumes)
- Adaptive Replacement Cache (ARC)
- Transparent Data Encryption
- ZFS Send/Receive for Backup and Replication
### Advantages 👍
- **Data Integrity**: Uses end-to-end checksums for all data, ensuring high data integrity.
- **Snapshots and Clones**: Supports efficient, low-overhead snapshots and clones, useful for backups and development.
- **RAID-Z Support**: Offers advanced RAID options (RAID-Z1, RAID-Z2, RAID-Z3), providing redundancy and fault tolerance.
- **Compression**: Built-in compression can save space and improve performance in certain workloads.
- **Scalability**: Designed to handle very large data sets and scales well with both size and number of files.
### Downsides 👎
- **Resource Intensive**: Can be resource-intensive, particularly in terms of memory usage.
- **Complexity**: The advanced features and flexibility of OpenZFS come with a steep learning curve.
- **Portability**: While available on many platforms, it is not as natively supported in Linux as ext4 or XFS.
- **Licensing**: OpenZFS is licensed under CDDL, which is incompatible with the GPL.
### Technical Details 🔍
- **Maximum File Size**: 16 EiB
- **Maximum Volume Size**: 256 ZiB (theoretical)
### Distro Usage
Open ZFS is **Not** available in the mainline Linux Kernel. Rather, it is available through a 3rd party module.
Works on Linux, BSD, and Mac
## HAMMER2 💾 (DragonflyBSD)
### Intro
**Hammer2** is a modern, advanced file system designed for high-performance and scalable storage solutions, particularly in clustered environments. It features robust capabilities such as copy-on-write, data deduplication, and built-in snapshots, providing high data integrity, efficient storage management, and instant crash recovery.
### Links
[Wikipedia: HAMMER2](https://en.wikipedia.org/wiki/HAMMER2)
[DragonFly BSD Hammer2](https://www.dragonflybsd.org/hammer/)
### Technical Features: 🔍
- Clustered File System Support
- Snapshot and Cloning Support
- Copy-on-Write (COW)
- Data Deduplication
- Data Compression (LZ4, ZLIB)
- Data and Metadata Checksumming
- Multi-Volume Support
- Instant Crash Recovery
- Fine-Grained Locking (for SMP scalability)
- RAID Support (1, 1+0)
- Thin Provisioning
- Asynchronous Bulk-Freeing
- Large Directory Support
- Built-in Data Integrity and Self-Healing
### Advantages 👍
- **High Performance**: Optimized for high-performance and scalable storage solutions.
- **Data Integrity**: Incorporates checksumming and self-healing features to maintain data integrity.
- **Efficient Storage Management**: Offers advanced features like data deduplication and compression to manage storage efficiently.
- **Scalability**: Designed to handle large volumes of data and support clustered environments.
### Downsides 👎
- **Complexity**: The advanced features and configuration options can introduce complexity.
- **Maturity**: As a newer file system, it may have fewer tools and less mature support compared to more established file systems.
- **Limited Adoption**: Less commonly used than other file systems, which may affect community support and documentation.
### Technical Details 🔍
- **Maximum File Size**: Not explicitly defined, but supports very large files.
- **Maximum Volume Size**: Not explicitly defined, but designed for large-scale storage.
### Distro Usage
- **DragonFly BSD**: The primary platform where Hammer2 is used and supported.
- **Limited Availability**: Not available in mainstream Linux distributions; primarily associated with DragonFly BSD.
## Key Concepts / Glossary 🔑
### Snapshots 📸
- **Snapshots** are read-only copies of a file system at a specific point in time, allowing users to save the state of the file system for backup and recovery purposes. They are efficient and consume minimal space, as only the differences between the current state and the snapshot are stored.
### Clones vs. Snapshots 📸🧬
- **Snapshots**: Read-only copies of the file system at a specific time.
- **Clones**: Writable copies of snapshots that can be modified independently.
### RAID-Z Levels ⛓️
- **RAID-Z1**: Single parity; can tolerate the loss of one disk.
- **RAID-Z2**: Double parity; can tolerate the loss of two disks.
- **RAID-Z3**: Triple parity; can tolerate the loss of three disks.
### RAID 5 and RAID 6 ⛓️
- **RAID 5**: Stripes data across disks with single parity; can tolerate the loss of one disk.
- **RAID 6**: Stripes data across disks with double parity; can tolerate the loss of two disks.
### Issues with RAID 5/6 in Btrfs
Btrfs's implementation of RAID 5/6 is considered unstable due to issues like the write hole problem, making it less reliable for production use. Data integrity may be compromised, leading to potential data loss.
### CDDL License 🪪
The **Common Development and Distribution License (CDDL)** is an open-source license created by Sun Microsystems. It is incompatible with the GPL, which can complicate integration with Linux.
### Btrfs Self-Healing ❤️‍🩹
**Self-Healing** in Btrfs works by verifying data against checksums and repairing any detected corruption using redundant data stored on other disks in a RAID configuration.
### Dynamic Storage 🧱
**Dynamic Storage** refers to the ability to manage multiple storage devices within a single file system, allowing for on-the-fly addition and removal of devices, with the file system automatically balancing data across them.
### Online Resizing 🗺️
**Online Resizing** allows the resizing of a file system while it is mounted and in use. XFS supports growing the file system online, while Btrfs supports both growing and shrinking.
### B-Trees ⚖️
A **B-tree** is a self-balancing tree data structure that maintains sorted data and allows efficient insertion, deletion, and search operations. B-trees are used in file systems like Btrfs to manage metadata and data blocks.
### Extent Base Allocation 👠
is a method used by modern file systems to manage data storage efficiently. Instead of tracking individual fixed-size blocks, the file system groups contiguous blocks into larger units called extents.
### Persistent Pre-allocation 🎟️
This technique reserves a specific amount of disk space for a file in advance, ensuring that the allocated space remains available, which helps in reducing fragmentation and guaranteeing storage for large files.
### Delayed Allocation ⏱️
Delayed allocation defers the assignment of specific disk blocks to file data until the data is flushed to disk, optimizing the allocation process and reducing fragmentation by allowing the file system to make better decisions about where to place data.
### Multi-block Allocation ⋔
Multi-block allocation allows a file system to allocate multiple contiguous blocks at once, rather than individually, improving performance and reducing fragmentation, especially for large files.
### Stripe-aware Allocation 🧠
Stripe-aware allocation is used in RAID configurations to ensure that data is distributed evenly across all disks in the array, optimizing performance by aligning data placement with the underlying stripe size of the RAID setup.
### Fine-Grained Locking (for SMP Scalability) 🚀
Fine-grained locking applies locks at a granular level, allowing multiple processors to concurrently access different parts of the file system, enhancing performance and scalability in multi-core environments.
### RAID 1+0 🖇️
RAID support includes configurations such as RAID 1 for data mirroring and RAID 1+0 for combining mirroring with striping to provide both redundancy and improved performance.
### Thin Provisioning 🔮
Thin provisioning allocates disk space on-demand rather than reserving it all upfront, optimizing storage utilization by only using the space actually required by data.
### Asynchronous Bulk-Freeing 🗑️
Asynchronous bulk-freeing performs large-scale space reclamation in the background, allowing the file system to manage deletions efficiently without impacting overall performance.
### Large Directory Support 🏢
Large directory support enables efficient management of directories with a vast number of entries, using optimized data structures to ensure fast performance for directory operations.
