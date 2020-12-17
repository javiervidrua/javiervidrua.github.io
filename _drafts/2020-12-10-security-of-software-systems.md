---
layout: post
title:  "Security of software systems"
date:   2020-12-10 00:00:00 +0200
categories: jekyll update
---
In this article I will try to explain by my own words every single thing that I learned during my Computer Engineering degree when it comes to security of software systems.

## 01 - Cryptography

### Definitions

Here are some basic concepts:
* *Cryptography*: The study of techniques of altering the representation of a message.
* *Cleartext*: Original message. It is legible.
* *Ciphertext*: Illegible text. It is the result of encryption performed on plaintext.
* *Cipher*: Algorithm that transforms cleartext into ciphertext.
* *Key*: Sequence of symbols that the ciphers use.

### Symmetric cipher

The cleartext is encrypted and decrypted using one key.

![Symmetric cipher diagram](/images/sss-symmetric-cipher.png)

This technique relies on these factors:
* It is impossible to recover the original text knowing exclusively the ciphertext.
* The shared key is secret.
* It is possible to physicaly implement the algorithms.

These are some classic techniques that symmetric ciphers use:
1. **Substitution**: The symbols of the cleartext are substituted by others. There are several types:
   * *Simple Monoalphabetic Substitution*: Each symbol of the cleartext is replaced by another one.

     An example of this cipher is the caesar cipher:
     ```
     cleartext:         TREE
     ciphertext:        AYLL
     ```
   * *Polygraphic Monoalphabetic Substitution*: Several symbols are replaced by several others.
     
     An example of this type of cipher is the Play Fair cipher:
     ```
     cleartext:         TREE
     ciphertext:        ODKUKU
     ```
   * *Polyalphabetic substitution*: Use several monoalphabetic substitutions as you encrypt the cleartext.

     An example of this type of cipher is the Vigenère cipher:
     ```
     cleartext:         TREE
     key:               ABCD
     ciphertext:        TSGH
     ```
   * *One Time Pad*: Use a key as big as the cleartext. The key is independent from the text.

     To encrypt and to decrypt you use the *XOR* function:
     ```
     cleartext:         TREE
     key:               QWER
     ciphertext:        JNIV
     key:               QWER
     cleartext:         TREE
     ```
1. **Transposition**: The symbols of the cleartext get permutated. In other words, it reorders the symbols taking them by blocks.

   This is tribial to cryptoanalyze, so that's no good my friend.

### ENIGMA machine:
A set of 3 cylinders that rotate on their own axis. In each cylinder there are 26 contacts (as for 26 letters), and each contact is connected to another one of the next cylinder. Each cylinder is a cryptosystem that does Polyalphabetic Substitution of period 26. Each cylinder is connected to the next, as a cascade.

There are 26x26x26 = 17573 substitution alphabets before the system loops.

With 5 cylinders, that number scalates up to 11881376. This is the base of the *crypt* command of *UNIX*.

### Standard encryption

*DES* (which descents from the LUCIFER cipher, created by IBM) was adopted by the NIST in 1977 to be the Data Encryption Standard.

It encrypts 64 bit blocks using 56 bits keys.

Throughout 19 stages and 16 iterations it transforms 64 bits of the cleartext into 64 bits of the ciphertext.

Being a symmetric cipher, the algorithms used in encryption and decryption are the same.

The following scheme shows the bit-level algorithm:

![DES diagram](/images/sss-des-diagram.gif)

The DES cipher can operate in four different modes, in order to be able to encrypt blocks of data of different lenghts. These modes of operation are the following:
1. *ECB*: Stands for Electronic CodeBook: 30

### Asymmetric cipher


## 01 - Introduction

General culture concepts and knowledge:

* *Mirai botnet*: Largest DDoS attack in history
* *Wannacry*: A classic
* *Stuxnet worm*: Targeted attack on Iranian nuclear facilities
* *Security basis*: Confidentiality, integrity and availability
* *Other security basis*: Authenticity, accountability and non-repudiation
* *More security basis*: Privacy, anonymity, untraceability, unlinkability, unobservability
* *Protection basis*: Prevention, detection, reaction
* **Software vulnerabilities exist for a reason and cannot be completely eliminated, but they can be avoided**

## 02 - x86 ISA

It is necessary to know how processors work to be able to understand how vulnerabilities can be exploited.

Here will be explained the x86 and the x86_64 ISA of Intel processors, as Intel is the most common brand of processor (66% of the market).

### Program compilation process

The process of compiling a program written in C/C++ is as shown in the following image:

![Compilation process in C/C++](/images/sss-compilation-process.png "Compilation process in C/C++")

If you compile a C program with the option `-save-temps`, *gcc* won't delete *.i* and *.s* files:
```bash
┌─[javier@torre]─[~]
└──╼ $cat sample.c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char * argv){
        fprintf(stdout, "[*] This is a sample program\n");
        return 0;
}
┌─[javier@torre]─[~]
└──╼ $gcc -save-temps sample.c -o sample
┌─[javier@torre]─[~]
└──╼ $ls -l
total 76
-rwxr-xr-x 1 javier javier 16656 Dec 12 23:05 sample
-rw-r--r-- 1 javier javier   148 Dec 12 23:05 sample.c
-rw-r--r-- 1 javier javier 42336 Dec 12 23:05 sample.i
-rw-r--r-- 1 javier javier  1632 Dec 12 23:05 sample.o
-rw-r--r-- 1 javier javier   593 Dec 12 23:05 sample.s
┌─[javier@torre]─[~]
└──╼ $./sample
[*] This is a sample program
```

Assembly code is written using mnemonics. To demostrate it, the following is an example of assembly code for the Motorola 6809 processor (the first assembly language I learned):
```assembly
; hello.asm: Simple program that prints the string on the screen

        .area PROG (ABS)

        .org 0x100          ; Start at 0x100
string: .ascii "Hello sir"
        .byte   10          ; 10 = CTRL+J = \cr
        .byte   0           ; 0  = CTRL+@ = \lf

        .globl program      ; Here starts the code
program:
        ldx #string
loop:   lda ,x+
        beq end
        sta 0xFF00          ; Print on the screen
        bra loop
end:    clra
        sta 0xFF01

        .org 0xFFFE         ; RESET vector
        .word program

```

### Concepts

Machine code: Code that can be directly executed by the computer without further translation.

Bytecode: Code that can be executed by a virtual machine, for example, Java creates this, and then the Java virtual machine runs it.

Opcode: Number that represents an operation (OPeration CODE).

Mnemonics: Instructions in assembly language. It can be a string or an opcode with zero or more arguments.

### x86 registers

A register is a form of storage that the processor has. They're really fast to access and to operate with them, and they are different from the main memory, as they are inside the processor.

There are regiters for different kinds of things:

* General purpose: Used for storing immediate values and memory addresses.
* Segment: Used for identifying segments in memory.
* Program status and control (flag registers): They store the flags that indicate the result of an arithmetic operation (overflow, zero, ...).
* Instruction pointer: Also named *PC* as for *Program Counter*

### General purpose registers on x86

* EAX: Extended Accumulator register.
* EBX: Extended Base register, a base pointer for memory access.
* ECX: Extended Counter register, counter for loop/string operations.
* EDX: Extended Data register, a pointer for I/O.
* ESI: Extended Source Index pointer for string operations.
* EDI: Extended Destination Index pointer for string operations.
* EBP: Extended Base Pointer, a pointer to data on the stack.
* ESP: Extended Stack Pointer.

The following image illustrates the mapping of the registers:
![x86 General purpose registers](/images/sss-x86-general-purpose-registers.png "x86 General purpose registers")

### Instruction pointer register on x86

The EIP (Extended Instruction Pointer) points to the next instruction to be executed. It can only be accesed by using branch instructions (call, jmp, ret).

### Program status and control (flag registers)

The EFLAGS register has several flags:

* CF: Carry Flag.
* PF: Parity Flag.
* ZF: Zero Flag.
* TF: Trap Flag.
* OF: Overflow Flag.
* SF: Sign Flag.

### Data types

There are several data types, each one being the double of the previous:

* Byte: 8 bits.
* Word: 16 bits.
* Double word: 32 bits.
* Quad word: 64 bits. (Combining EDX and EAX into one)

### x86 endianness

The x86 uses the little endian format to store information in memory. This means that, for example, the word `x28A1427F` will be stored as `x7F42A128`.

### Data movement

To move data, the x86 has the MOV instruction:

* Immediate to register: `mov eax, 0x41`. This will put the value 0x41 in the EAX register.
* Register to register: `mov eax, ebx`. This will put whatever is on the EAX register into the EBX register.
* Immediate to memory: `mov [eax], 0x41`. This will put 0x41 in the memory address that the EAX register contains.
* Register to memory: `mov [eax], ebx`. This will put whatever is in the EBX register in the memory address that the EAX register contains.
* Memory to register: `mov eax, [ebx]`. This will put whatever is in the memory address that the EBX register contains into the EAX register.

### LEA

This instruction will Load the Effective Address into a register. Example: `lea eax, [ebx+0x04]`

### Arithmetic instructions

`add eax, ebx`

`sub eax, ebx`

`inc edi`

`dec esi`


### Bit-level instructions

`and eax, ebx`

`or eax, ebx`

`xor ebx, eax`

`not eax`

### Jump instructions

* Unconditional:

  `jmp eax`

  `jmp [ebx]`
* Conditional:

  Preceeded by `cmp eax, ebx` most of the time.

  `jle eax, ebx`

  `jz ebx, 0x01`

### More info about instructions

* https://en.wikibooks.org/wiki/X86_Assembly/Control_Flow

* https://en.wikipedia.org/wiki/X86_instruction_listings

### Memory segmentation and stack operations

In order to exploit security bugs, most of the time you'll have to overwrite or overflow a portion of the memory into another one.

The following image illustrates the structure of a C program:

![C program memory diagram](/images/sss-c-memory-diagram.png "C program memory diagram")

### Stack

The stack is a LIFO structure, and it grows downwards.

It has two operations, `push` and `pop`.

The ESP points to the address of the oldest pushed data that is currently on the stack.

### Function calls

To call a function, you use the `call` instruction.

Once you call a function, the return address (next instruction after the function ends) is pushed to the stack.

The return of the function is performed via the `ret` instruction.

Before calling a function, there is what is called a function prologe. Its purpose is to backup the selected registers and save space for the local variables.

After calling a function, everything that was done in the prologue is done backwards before issuing the return.

### Function invocation

* Windows:

  First four arguments are loaded into RCX, RDX, R8, R9.

  The rest of them are passed trough the stack from right to left.
* Linux:

  First six arguments are loaded into RDI, RSI, RDX, RCX, R8, R9.

  The rest of them are passed trough the stack from right to left.

### Exercises

1. Prepare the system:

   You'll need a Linux system with *gcc*, *gdb* and *nasm* installed on it.
   
   You can do so by running the following command:
   ```bash
   apt update && apt install gcc gdb nasm -y
   ```

   Now configure *gdb* to show disassemblies in the intel format (we don't want the AT&T format) by running this command:
   ```bash
   echo 'set disassembly-flavor intel' >> ~/.gdbinit
   ```

   If you want to use *objdump* for disassemblies, use the option `-M intel` to get the output in the intel format.
   
   An example against the sample program that we compiled before:
   ```bash
   ┌─[javier@torre]─[~/sample]
   └──╼ $objdump -M intel sample -d | head -15
   
   sample:     file format elf64-x86-64
   
   
   Disassembly of section .init:
   
   0000000000001000 <_init>:
       1000:       48 83 ec 08             sub    rsp,0x8
       1004:       48 8b 05 dd 2f 00 00    mov    rax,QWORD PTR [rip+0x2fdd]        # 3fe8 <__gmon_start__>
       100b:       48 85 c0                test   rax,rax
       100e:       74 02                   je     1012 <_init+0x12>
       1010:       ff d0                   call   rax
       1012:       48 83 c4 08             add    rsp,0x8
       1016:       c3                      ret
   ```

## 03 - Runtime attacks

Text

## 05 - Defenses against runtime attacks

Text

## 04 - Code reuse: Attacks and defenses

Text

## 06 - Web security

Text

## 07 - Blockchain

Text

## 08 - Smart contracts

Text

## 09 - Side channel attacks

Text

## 10-Hardware security

Text

## Topics for thesis
Internet of Things (IoT)

Trust Management in IoT networks

Distributed and autonomous market place of IoT data

Secure Code update in IoT networks

Mobile Security

Improving privacy in mobile messenger apps

Detection of UI spoofing attacks on mobile platforms

Data collector app for Machine Learning

Software security

Microarchitectural attacks (Meltdown and Spectre) and countermeasures

Side-Channel Protection for Intel SGX

Control-Flow Integrity for SGX enclaves

Blockchains and Smart Contracts

Countermeasures against malicious data inclusion attacks in Bitcoin

Sharding Ethereum client to mitigate DoS attacks

Blockchain-based proxy certificates for TLS

Life cycle management and code updates in smart contracts

Security policy engineering for Smart Contracts

Smart Contract Security

Fuzzing smart contracts for vulnerability detection

Vulnerability patching in smart contracts

Theses/Practicum

Bachelor Thesis / Master Thesis / Master Practicum

Software Security

(Master Thesis) Vulnerability assessment in browser extensions using AI and ML

(Master Practicum) Vulnerability assessment of smart contracts using AI and ML

(Master Thesis) Vulnerability detection in smart contracts using fuzzing

(Master Thesis) Patching vulnerabilities in smart contracts
 

Blockchains and Smart Contracts

(Master Thesis / Master Practicum) Life cycle management and code updates in smart contracts

(Master Thesis) Blockchain reductions for reducing storage requirements of full nodes

(Bachelor Thesis / Master Thesis) Sharding Ethereum client to mitigate DoS attacks
 

Privacy

(Bachelor Thesis) Privacy of mobile messengers (Telegram, WhatsApp, Signal)

(Master Thesis) Analysis of privacy leaks in social platforms (Facebook, Instagram)
 

Network Security 

(Master Thesis) Vulnerability assessment in SDN setup using fuzzing

(Master Thesis) Detection of vulnerable firewall configurations using AI and ML
 
Mobile Security 

(Master Thesis) Thwarting privilege escalation attacks on Android using AI and ML
 

IoT Security 

(Bachelor Thesis / Master Practicum) Blockchain-based market place for IoT data
 
Security of Systems and Protocols

(Master Thesis) Attacks on speech recognition systems (Alexa and Co.) and countermeasures

(Bachelor Thesis / Master Thesis / Master Practicum) Securing 3D-Secure payments using AI-based transaction classifier

(Bachelor Thesis / Master Thesis / Master Practicum) Biometric user authentication on mobile platforms

(Bachelor Thesis / Master Practicum) Security analysis of UniNow app

(Bachelor Thesis / Master Practicum) Formal analysis of Corona tracing protocols
 

Ethical Hacking

(Bachelor Thesis / Master Practicum) Gamification of Hacking Lab