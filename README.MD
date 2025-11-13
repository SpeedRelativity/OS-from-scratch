# Project Documentation

## Step 1

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

---

## Step 2

### Goal
Create a file that the BIOS interprets as a bootable disk.

### Concepts
- **Boot Sector:** The first 512 bytes of a storage device (hard drive, floppy disk, USB stick, etc.).  
- **BIOS Load:** At startup, the BIOS reads the first 512 bytes from a disk into RAM at address `0x7C00`.  
- **Boot Check:** The BIOS checks bytes 511 and 512 for the value `0xAA55` to verify if the disk is bootable.  
- **Padding:** The boot sector must be exactly 512 bytes. We pad unused bytes with zeros so the final two bytes can store `0xAA55`.

### Assembly Code
```assembly
loop:
    jmp loop

times 510-($-$$) db 0
dw 0xaa55
```

### Explanation
- `times`: Repeat the following instruction a specified number of times.  
- `510`: Target size before the two signature bytes.  
- `$`: Current address/position.  
- `$$`: Start of the section.  
- `$ - $$`: Represents the current offset (end - start).  
- `db 0`: Define one byte with value `0`.

**Calculation:**
- 3 bytes for the infinite loop  
- 507 bytes of zeros  
- 2 bytes for the boot signature  
- **512 total bytes**

### Building the Binary
Created a new file `boot_sect_simple.asm` and compiled it:

```bash
nasm -f bin boot_sect_simple.asm -o boot_sect_simple.bin
```

### Verification
Checked the beginning and end of the generated `.bin` file:

```bash
xxd boot_sect_simple.bin | head
xxd boot_sect_simple.bin | tail
```
