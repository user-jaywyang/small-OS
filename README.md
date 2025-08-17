# Small OS

## What is this?
This is a small 64-bit hobby operating system for the x86 architecture.  
It boots via GRUB2, switches the CPU into long mode, and runs a custom C kernel with:

- Text-mode VGA output
- Interrupt handling and PIC remapping
- A programmable PIT timer
- Keyboard input
- A simple heap allocator
- A minimal in-RAM virtual file system
- A tiny interactive shell

It’s intended as an educational project to learn how an OS boots, talks to hardware, and manages memory from scratch.

---


At boot you’ll see a text banner and a series of status lines as the system brings up:

- **IDT (Interrupt Descriptor Table)** for 64-bit mode
- **Keyboard driver** (IRQ1)
- **PIT timer driver** (IRQ0) for periodic ticks
- **Heap allocator** with `kmalloc`/`kfree`/`kcalloc`/`krealloc`
- **In-memory virtual file system** (directories + files)
- **Interactive shell**, accessible via the keyboard, running on a text-mode console

Once initialization completes, interrupts are enabled, and the kernel enters a `hlt` loop. From that point, the system is fully driven by hardware interrupts and shell input.

---

## Project Goals

This project is meant to give hands-on experience with:

- Multiboot2 boot process and early CPU setup
- Entering x86-64 long mode from assembly
- Setting up an IDT and basic interrupt handlers
- Talking directly to hardware with port-mapped I/O
- Implementing a simple heap allocator on bare metal
- Building a small virtual file system abstraction
- Wiring everything together behind a toy shell

---

## Installation
### Required Build Dependencies
- A modern C compiler(GNU gcc recommended)
- A linker (GNU ld recommended)
- An assembler (NASM recommended)
- grub-mkrescue
- make

### Required Emulator
- You must install the QEMU emulator to for this project.
- It is expected by `make run` to emulate the ignis.iso image created by `make`

### Getting Started
**Steps**:
- Install build dependencies.
- Clone the repo with `git clone https://github.com/user-jaywyang/small-OS && cd small-OS`
- Compile and package the project with `make` or `make all`
  - The `Makefile` creates a ignis.iso file in iso/boot/ that is emulated by QEMU
- Run the QEMU emulator with `make run` or `make run_debug`

---

## Project Layout

Repository root (simplified):

```text
IGNIS-OS/
├── .gitignore
├── LICENSE
├── Makefile
├── README.md              # (this file)
├── boot.asm               # Multiboot2 header + early 64-bit setup
├── idt.asm                # IDT loader + interrupt stubs (PIT, keyboard)
├── kernel.c               # C kernel entry point (kernel_main)
├── link.ld                # Linker script for the kernel
├── shell.c / shell.h      # Interactive shell implementation
├── libc/                  # Tiny C library subset
│   ├── stddef.h
│   ├── stdint.h
│   ├── string.c
│   └── string.h
├── io/                    # Low-level device I/O
│   ├── idt.c / idt.h      # 64-bit IDT setup
│   ├── ports.h            # inb/outb helpers
│   ├── vga.c / vga.h      # VGA text-mode driver
├── drivers/               # Hardware drivers
│   ├── keyboard.c / keyboard.h
│   └── pit.c / pit.h
├── mm/                    # Memory management
│   ├── memory.c
│   └── memory.h
├── fs/                    # Simple virtual file system (in RAM)
│   ├── vfs.c
│   └── vfs.h
└── iso/boot/grub/
    └── grub.cfg           # GRUB2 menu entry for IGNIS





