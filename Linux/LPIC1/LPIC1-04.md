# Managing Files and Filesystems

- [Managing Files and Filesystems](#managing-files-and-filesystems)
  - [Maintaining Filesystem Health](#maintaining-filesystem-health)
    - [Tuning Filesystems](#tuning-filesystems)
      - [Obtaining Ext2fs Information](#obtaining-ext2fs-information)
      - [Adjusting Tunable Filesystem Parameters](#adjusting-tunable-filesystem-parameters)
      - [Interactively Debugging a Filesystem](#interactively-debugging-a-filesystem)
    - [Maintaining a Journal](#maintaining-a-journal)
    - [Checking Filesystems](#checking-filesystems)
    - [Monitoring Disk Use](#monitoring-disk-use)
      - [Monitoring Disk Use by Partition](#monitoring-disk-use-by-partition)
      - [Monitoring Disk Use by Directory](#monitoring-disk-use-by-directory)
  - [Mounting and Unmounting Filesystems](#mounting-and-unmounting-filesystems)
    - [Temporarily Mounting or Unmounting Filesystems](#temporarily-mounting-or-unmounting-filesystems)
    - [Permanently Mounting Filesystems](#permanently-mounting-filesystems)
  - [Managing Files](#managing-files)
    - [File Naming and Wildcard Expansion Rules](#file-naming-and-wildcard-expansion-rules)

## Maintaining Filesystem Health

### Tuning Filesystems

#### Obtaining Ext2fs Information

```bash
# Dump ext2/ext3/ext4 filesystem information

$ dumpe2fs [ -bfghixV ] [ -o superblock=superblock ] [ -o blocksize=blocksize ] device

# use with -h switch for usual workflow. e.g.

$ dumpe2fd -h /dev/sdb2
```

#### Adjusting Tunable Filesystem Parameters

```bash
# Adjust tunable filesystem parameters on ext2/ext3/ext4 filesystems

$ tune2fs [options] device 
```

#### Interactively Debugging a Filesystem

```bash
# ext2/ext3/ext4 file system debugger (interactive version of dumpe2fs and tune2fs)

$ debugfs [options] device
```

### Maintaining a Journal

```bash
# use -j option to add journal to a filesystem

$ tune2fs -j device
```

### Checking Filesystems

```bash
# Check and repair a Linux filesystem

$ fsck [-sACVRTNP] [-t fstype] [--] [fsck-options] filesystems
```

### Monitoring Disk Use

#### Monitoring Disk Use by Partition

```bash
# report file system disk space usage

$ df [OPTION]... [FILE]...
```

#### Monitoring Disk Use by Directory

```bash
# estimate file space usage

$ du [options] [directories]
```

---

## Mounting and Unmounting Filesystems

### Temporarily Mounting or Unmounting Filesystems

```bash
# mount - mount a filesystem

$ mount [-alrsvw] [-t fstype] [-o options] [device] [mountpoint]

# unmount filesystems
$ umount [-afnrv] [-t fstype] [device | mountpoint]
```

### Permanently Mounting Filesystems

The `/etc/fstab` file controls how Linux provides access to disk partitions and removable media devices.

---

## Managing Files

### File Naming and Wildcard Expansion Rules
