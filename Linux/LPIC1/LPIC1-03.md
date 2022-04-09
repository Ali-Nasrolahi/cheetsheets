# Configuring Hardware

- [Configuring Hardware](#configuring-hardware)
  - [Configuring the BIOS and Core Hardware](#configuring-the-bios-and-core-hardware)
    - [IRQs](#irqs)
    - [I/O Addresses](#io-addresses)
    - [DMA Addresses](#dma-addresses)
  - [Configuring Expansion Cards](#configuring-expansion-cards)
    - [Configuring ISA Devices](#configuring-isa-devices)
      - [Using Non-Kernel Utilities](#using-non-kernel-utilities)
    - [Configuring PCI Devices](#configuring-pci-devices)
  - [Configuring Modems](#configuring-modems)
  - [Creating Partitions and Filesystems](#creating-partitions-and-filesystems)
    - [Partitioning a Disk](#partitioning-a-disk)
      - [Using `fdisk`](#using-fdisk)
      - [Using GNU Parted](#using-gnu-parted)
    - [Preparing a Partition for Use](#preparing-a-partition-for-use)
      - [Creating a Filesystem](#creating-a-filesystem)
      - [Creating Swap Space](#creating-swap-space)
  - [Installing Boot Loaders](#installing-boot-loaders)
  - [END](#end)

## Configuring the BIOS and Core Hardware

### IRQs

`/proc/interrupts` file lists IRQs that are *in use* by Linux.

### I/O Addresses

`/proc/ioports` file lists what I/O addresses are being *used*.

### DMA Addresses

`/proc/dma/` file contains what DMA channels are *in use* on your system

---

## Configuring Expansion Cards

### Configuring ISA Devices

#### Using Non-Kernel Utilities

`pnpdump`: retrieves the current configuration of ISA PnP devices.

`isapnp`: mplements the ISA PnP configuration stored in the configuration file you give it.

```bash
pnpdump > isapnp.conf

isapnp /etc/isapnp.conf
```

### Configuring PCI Devices

`setpci`: can be used to directly query and adjust PCI devices’ configurations.

`lspci`: display all information about the PCI busses on your system and all devices that are connected to these busses

---

## Configuring Modems

---

## Creating Partitions and Filesystems

### Partitioning a Disk

#### Using `fdisk`

`fdisk`: manipulate disk partition table.

```bash
$ fdisk [options] device # enter fdisk interactive mode on device

$ fdisk -l [device...] # lists partions in device

# In interactive-mode:

p # Display the current partition table

n # Create a partition

d # Delete a partition

t # Change a partition’s type

l # List partition types

a # Mark a partition bootable

m or ? # Get help

q # Exit without saving

w # Exit with saving
```

#### Using GNU Parted

`parted`: a partition manipulation program

```bash
$ parted [options] [device [command [options...]...]]

? # Shows all coammads

print # displays the current partition table

mkpart # creates (makes) a partition

rm # removes a partition

move # moves a partition

resize # changes a partition’s size
```

### Preparing a Partition for Use

#### Creating a Filesystem

`mkfs`: build a Linux filesystem.

```bash
mkfs [options] [-t type] [fs-options] device [size]

mkfs -t ext3 /dev/hda6
```

This mkfs frontend is **deprecated** in favour of filesystem specific `mkfs.type` utils.

```bash
# for example
$ mkfs.ext2 # ext2 partition
$ mkfs.ext3 # ext3 partition
$ mkfs.ext4 # ext4 partition
$ mkfs.fat  # fat partition
$ mkfs.ntfs # ntf partition
```

#### Creating Swap Space

`mkswap`: set up a Linux swap area.

```bash
$ mkswap [options] device [size]

# e.g.

$ mkswap /dev/hda7

# To use the swap space, you must activate it with the swapon command

$ swapon /dev/hda7
```

---

## Installing Boot Loaders

For `LILO`, the file is `/etc/lilo.conf`; for `GRUB`, it’s either `/boot/grub/grub.conf` or `/boot/grub/menu.lst`.

---

## END
