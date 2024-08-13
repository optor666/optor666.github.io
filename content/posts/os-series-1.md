+++
title = '自制操作系统：Hello, World'
date = 2024-05-29T19:50:44+08:00
draft = true
+++
在操作系统上，使用任何编程语言输出一个 "Hello, World" 可以说都挺简单的，但没有操作系统的话应该怎么办呢？
<!--more-->
一台计算机在安装操作系统前，上面还是有一个基本的软件程序 BIOS 的，所以没有操作系统的话，我们就需要面向 BIOS 编程了。

计算机在通电开机后，首先会执行主板上的 BIOS 程序，BIOS 程序完成硬件自检逻辑后，会尝试从磁盘或软盘中读取引导扇区中的程序到内存 0X7C00 处，这个程序通常是操作系统的引导程序，但在这里，这个程序就是我们的 "Hello, World" 程序。

另外一个需要知道的点是，一个扇区大小是 512 字节，并且最后两个字节是魔数 0xAA 0x55。

# QEMU
## 安装
``` Shell
# 参考 wiki：https://www.qemu.org/download/#linux
brew install qemu
apt install qemu-system
yum install qemu-kvm
```
## 运行
``` Shell
qemu-system-i386 --nographic -fda boot.bin
```

# NASM
主页：[https://www.nasm.us/](https://www.nasm.us/)
## 安装
``` Shell
brew install nasm
apt install nasm
```
## 汇编
``` Shell
nasm boot.asm -f bin -o boot.bin
```

# GAS
## 安装
1. macOS 系统使用了 ARM 芯片，不方便使用
2. 直接使用 X86 64 位的 Ubuntu 系统即可
## 汇编
``` Shell
as -o boot.o boot.asm
ld -Ttext=0x7c00 --oformat binary -o boot.bin boot.o
```

# 程序
## NASM
``` NASM
org 0x7C00                      ; BIOS loads our programm at this address
bits 16                         ; We're working at 16-bit mode here

start:
	cli                     ; Disable the interrupts
	mov si, msg             ; SI now points to our message
	mov ah, 0x0E            ; Indicate BIOS we're going to print chars
.loop	lodsb                   ; Loads SI into AL and increments SI [next char]
	or al, al               ; Checks if the end of the string
	jz halt                 ; Jump to halt if the end
	int 0x10                ; Otherwise, call interrupt for printing the char
	jmp .loop               ; Next iteration of the loop

halt:	hlt                     ; CPU command to halt the execution
msg:	db "Hello, World! ", 0   ; Our actual message to print

;; Magic numbers
times 510 - ($ - $$) db 0
dw 0xAA55
```
## GAS
``` GAS
.section .text
.code16
.globl _start

_start:
    cli                         # Disable interrupts
    mov $msg, %si               # SI now points to our message
    mov $0x0E, %ah              # Indicate BIOS we're going to print chars

loop:
    lodsb                       # Load byte at SI into AL and increment SI
    or %al, %al                 # Check if end of the string
    jz halt                     # Jump to halt if at end
    int $0x10                   # BIOS interrupt to print character
    jmp loop                    # Next iteration of the loop

halt:
    hlt                         # Halt CPU execution

msg:
    .ascii "Hello, World! "
    .byte 0                    # Null terminator

.org 510
.word 0xAA55                   # Boot signature
```
