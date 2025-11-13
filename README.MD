# Project Documentation

## Step 1:

### Setup
Installed required packages on Ubuntu Linux.

- **NASM:** Netwide Assembler — converts assembly code into executable machine code.  
- **QEMU:** System emulator used to run and test the operating system.

```bash
sudo apt install nasm qemu-system-x86
```

### Core Concepts
- **Assembler:** Translates assembly code into binary instructions that the CPU can execute.  
- **BIOS (Basic Input/Output System):** Firmware stored on a motherboard chip that initializes hardware before any OS loads. Resides in ROM, unlike RAM which is user-installed.


## Step 2:

### Goal: Create a file which the BIOS interprets as bootable disk.

- **Boot Sector:** The first 512 bytes of a storage device (hard drive, floppy disk, USB stick etc).

- **BIOS Load:** BIOS looks at DISKS after starting up, BIOS reads first 512 bytes from this disk into RAM and stores it at 0x7C00 (arbitrary location). Has history, but we don't care.

- **Boot Check:** The BIOS looks at bytes 511 and 512 of the boot sector to check if they are 0xAA55 which indicate *BOOTABLE OR NOT*.

- The BIOS reads exactly 512 bytes for the boot sector.

- We need to pad the "0xAA55" bytes into the correct position, so we will add 0s at the front.

```assembly
loop:
    jmp loop

times 510-($-$$) db 0

dw 0xaa55

```







