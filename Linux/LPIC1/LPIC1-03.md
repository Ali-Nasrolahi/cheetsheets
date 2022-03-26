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
