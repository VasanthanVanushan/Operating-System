# Operating-System
Creating a Simple Operating system
# Vanushan OS

A minimal 512-byte bootable operating system that prints:

> hello, welcome Vanushan OS

This project demonstrates how to write a simple bootloader using x86 assembly. Great for beginners exploring low-level programming or OS development.

---

## Requirements

- Linux or Windows (with [WSL](https://learn.microsoft.com/en-us/windows/wsl/))
- Terminal basics
- Installed:
  - [`NASM`](https://www.nasm.us/) – Netwide Assembler
  - [`QEMU`](https://www.qemu.org/) – Hardware emulator

Install on Debian/Ubuntu:

```bash
sudo apt update
sudo apt install nasm qemu
```

---

## Setup

Create and navigate to your project folder:

```bash
mkdir vanushan-os
cd vanushan-os
```

---

## Code (bootloader.asm)

Create a file named `bootloader.asm` and paste:

```asm
[org 0x7c00]

start:
    mov si, message

print_loop:
    lodsb
    cmp al, 0
    je hang
    mov ah, 0x0E
    int 0x10
    jmp print_loop

hang:
    jmp $

message db 'hello, welcome Vanushan OS', 0

times 510 - ($ - $$) db 0
dw 0xAA55
```

---

## Build & Run

Assemble the code:

```bash
nasm -f bin bootloader.asm -o bootloader.bin
```

Boot using QEMU:

```bash
qemu-system-x86_64 -drive format=raw,file=bootloader.bin
```

---

## How It Works

- BIOS loads the boot sector (first 512 bytes) to memory address `0x7C00`
- Each character is printed using BIOS interrupt `int 0x10`
- Ends in an infinite loop after printing

---

## Next Steps

Try extending the OS:

- Add color support
- Capture keyboard input (`int 0x16`)
- Load data from disk
- Chainload a second-stage kernel

---

## License

MIT License. Free to use and modify.

---

## Credits

Inspired by the x86 bootloader community and low-level enthusiasts. Built by [Vanushan].
