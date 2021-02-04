---
layout: post
title:  "Draft - Security of software systems"
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

The *DES* cipher can operate in four different modes, in order to be able to encrypt blocks of data of different lenghts. These modes of operation are the following:
1. ***ECB***: Stands for *Electronic CodeBook*: The simplest mode, as it ciphers 64 bit blocks.
1. ***CBC***: Stands for *Cipher Block Chaining*: Uses the last encrypted block to do a XOR operation against the current block to encrypt. For this reason, it needs an initialization block that has to be known between both the encryption and the decryption algorithm.
1. ***CFB***: Stands for *Cipher FeedBack*: This mode allows the encryption algorithm to encrypt data of any size. As the last mode, this one needs an initialization block too, as it needs the last encrypted block to fill the empty bits of the 64 block. If the block to cipher is smaller than 64 bits, the remaining bits get filled with the bits from the previous encrypted block, resulting in a 64 bit block that can now be encrypted.
1. ***OFB***: Stands for *Output FeedBack*: Works the same as the Cipher FeedBack, but instead of chaining the encryption after the XOR operation, it does it before it. The advantage of this mode is that a transmission error in one block does not affect the rest of the blocks.

*3DES*: Stands for Triple DES, as it uses three 56 bit keys (one for each stage). Two of those keys are the same (for the even stages, the first and the third) and one is unique (for the odd stage, the second).

Its time complexity is 2^(120-log2(n)) such as n=plaintext lenght.

### IDEA

It stands for *International Data Encryption Algorithm*, uses 64 bit blocks and 128 bit keys.

It has 8 iterations and a final stage that transforms everything into a 64 bit ciphertext block and the same modes of operation as the *DES* algorithm.

### AES

Winner of the 1997 public contest promoted by the NIST whose purpose was to replace the *3DES* algorithm.

This algorithm is a part of the *Rijndael* algorithm (*Joan Daemen & Rijmen*), as the *Rijndael* allows several different block and key sizes.

The official release of the AES can be found [**here**.](https://csrc.nist.gov/csrc/media/publications/fips/197/final/documents/fips-197.pdf)

Some particular attributes of this algorithm are the following:
* Works with bytes.
* Does operations with the body F256.
* Uses its own arithmetic operations (sum, product).
* Key size can be 128, 192 or 256 bits.
* Block size of 128 bits.

### Asymmetric cipher

Using two keys instead of one, it supposes a revolution in cryptology, as it solves two problems that are complex to solve with a secret key:
* Key distribution.
* Digital signature.

It is based on a series of new elemental transformations that were discovered at the time. Instead of transformations and substitutions, mathematic transformations are used.

The requirements for these type of algorithms are the following:
* The keys **can be** different.
* One algorithm and key for encryption and another pair for decryption.
* Impossible to compute one of the keys knowing the other one and the cipher algorithm.
* Optionally, the keys are interchangeable.

The basic scheme is presented in the next diagram:

![Asymmetric cipher diagram](/images/sss-asymmetric-cipher.png)

*Diffie-Hellman* rules:
* Easy for a computer to generate a pair of keys (public and private).
* Easy encryption, in computing terms.
* Knowing the private key, the decryption must be easy.
* Impossible to obtain the private key from the public key.
* Impossible to obtain the cleartext if only the public key is known.
* Optionally, the encryption and decryption algorithms can be applied in any order.

If the last requirement is met, now we are able to use this algorithm for:
* Authentication: Identification and validation.
* Digital signature: The message is signed with the private key, and only if the signing person is the one that claims to be, the signature validates using the public key.
* Does not guarantee confidentiallity.
* Guarantees integrity.

### RSA

One of the most used ones, since it was created in 1977 by the *MIT* investigators Ron **R**ivest, Adi **S**hamir, Leonard **A**dleman.

It basis its robustness in the complexity of big numbers factorization, wich is really good, but it has its cons:
* It uses very long keys when compared with the ones that the symmetric encryption ciphers use.
* The algorithm is very demanding in computing terms.

#### The discrete logarithm problem

Given values for *a*, *b* and *n* such as *n* is a prime number, the function *x* = *a*^*b* mod *n* is very easy to compute.

But if you know the values of *x*, *a* and *n*, finding the value of *b* is very hard to compute if the values of *x*, *a* and *n* are very large.

This is the basis of the public key cryptography.

#### Prime tests

To check if a very large number is prime takes a lot of time (with large numbers N, the estimated distance between prime numbers is ln(N)), so there are several different tests we can do:
* Miller-Rabin probability test.
* AKS algorithm (since 2002).
* Extended Euclides algorithm.

## 02 - Security services

### Related to the message

#### Confidentiality

This is met with both types of encryption (symmetric and asymmetric).

#### Integrity

This guarantees that the received message has not been modified and is the exact same message that was sent.

To do this, a one-way function (also known as hash function) generates a code that represents the message that will be sent.

The hash is then stored and compared to the hash of the received message.

#### Message authentication

It's the ability to guarantee the identity of the sender of the message.

On paper it's done via autograph.

With symmetric encryption it can be done several ways:
* Checksum.
* MAC (Message Authentication Code).
* Hash + key:
  ![Hash + key diagram](/images/sss-hash+key.png)

#### Non-repudiation

This means that a person cannot deny:
* That this person sent the message (this is done with digital signature).
* That this person received the message (e.g. this is the double tick that WhatsApp has).

### Related to the entity

#### Entity authentication

The authentication is done by using a piece of information (generally a key or a password) that the agent that wants to authenticate has.

Challenge-response authentication: The verifier sends a challenge to which the response must be a function applied to that. It can be done using:
* Public key schemes.
* Digital signature schemes.

2FA for people: After the user has successfully introduced his access credentials, the system needs more information to let the user in. This can be one of three basic categories:
* Something that the user knows.
* Something that the user has.
* Something that the user is.

#### Hash functions

A hash is a function that assigns a fixed length value to data of any length.

A good hash function must meet the following requirements:
* H can be applied to messages of any length.
* H produces output of a fixed length.
* H(x) is easy to compute.
* For a given hash *h* is not feasible to find *m*, such that H(m)=h.
* For a given block *x* is not feasible to find *y*, such that H(x)=H(y).
* It is computationally infeasible to find *x* and *y*, such that H(x)=H(y).

#### Hash vs CRC

A *hash* is a one-way function, and it is designed to make difficult to find an entry that produces certain output value.

A *CRC* is designed to detect accidental changes in the data. **Its purpose is not to protect against changes, but to detect them**.

#### Some hash functions

MD5: Improvements over MD4 and MD2, slower but more secure. Produces 128 bit output.

SHA-1: Secure Hash Algorithm, published in 1994. Similar to MD5 but this one produces 160 bit output.

SHA-2: Set of functions (SHA-224, SHA-256, SHA-384, SHA-512), published in 2001.

SHA-3: Set of functions published in 2015. Just a standard, not in use yet.

### Digital signature

Properties:
* Able to verify author, date and time.
* Authentify content at the time of the sign.
* Verifiable by third parties.

Requirements:
* Bit pattern independent from the message to sign.
* Signature issuer information to prevent falsification and impersonation and negation of the signature.
* Easy to generate.
* Easy to recognize and verify.
* Impossible to fake (nor signature or message).
* Must be practical to store a copy.

A message can be signed by more than one person, an it also can be signed by a supervisor.

### Key management

#### Symmetric key distribution

There are several posibilities:
* Use of session keys.
* KDC (Key Distribution Centre).
* 3 way protocol

#### Public key distribution

To distribute public keys, the possibilities are the following:
* Public announcement: A secure channel is needed.
* Public available directory: A big, reliable organization takes care of the manteinance.
* Public key authority: Mantained by an authority. Need of having a trustable public key issued by the authority. The authority sends keys to the users that request them.
* Public key certificate: X.509 certificates are a file digitally signed by a Certification Authority (CA). It links some data to an identity. Both sender and receiver trust the CA. A PKI standard is X.509.

#### CA

A CA is a trustable organization responsible of issuing certificates for users or servers.

Local scope: Enterprise, campus or country: e.g. in Spain, FNMT and DNIe. Autosigned certificates.

Global scope: We trust a certificate if it is signed by a trustable authority that we all trust. Two types of certification authorities networks, tree (PKI) and distributed (keyrings).

### Secure communication protocols

#### SSL

Standing for Secure Socket Layer, it was designed by Netscape Corporation for their Internet Browser. 

Works on the transport layer (TCP).

The services it offers are the following:
* Data compression.
* Security: (Parameter negotiation, client-server authentication, data integrity and confidentiality).

Stages:
1. Handshake: The parameters of the algorithms and the key length are determined between the both parts of the communications. The public keys are exchanged. The authentication is made via certificates.
1. Transference: Symmetric key determination and encrypted data exchange.

#### TLS

Based on SSL 3.0, but not compatible with it.

In contrast with SSL, TLS can reuse an already existing TCP connection, so it does not need dedicated ports to work. It is inmune to Man In The Middle type of attacks.

#### IPsec

Collection of security protocols at network layer (IP).

Two modes of operation:
* Transport mode: Protects the information send by the transport layer, this is, **it only protects the TCP payload.**. This mode is useful on end-to-end communications.
* Tunnel mode: **Protects the original IP datagram, this is, everything**. This mode is useful if one of the ends does not support IPsec, e.g firewall, VPN.

![IPsec working modes](/images/sss-ipsec-modes.png)

It has three protocols:
* AH (Authentication Header): It provides origin authentication and integrity, but not confidentiality.
* ESP (Encapsulating Security Payload): It provides origin authentication, integrity, and confidentiality too.
* IKE: Security Asociations (SA). One-way relationship. For a two-way communication we use two SA, and one of them establishes the first time that a datagram is interchanged. This converts a connectionless protocol into a connection oriented one.

![IPsec SA two way](/images/sss-sa-two-way.png)

## 03 - Operating systems security

### Definitions

* Threat: Any situation that endangers the security.
* Vulnerability: Weakness that is susceptible of producing an error.
* Exploit: Technique that allows the atacker to take advantage of certain vulnerability to break the security of a system.
* Social engineering: The art of manipulating people so they give up confidential information.
* APT: Advanced Persisten Threat.
* Botnets: Net of compromised, infected computers that can be used to perform distributed attacks.
* Risk: Latent probability of a security incident taking place.

### Vulnerabilities

Every system has vulnerabilities.

There are some strategies against them:
* Security backups.
* Risk analysis.
* Suspicious events detection.
* Constant revision of the security of the organisation.

The vulnerabilities must be classified. For that we have:
* CVE (Common Vulnerabilites and Exposures): A unique ID is assigned to every vulnerability that is found, so they can be classified and origanised. For example CVE-2017-0144 (Eternalblue).
* CVSS (Common Vulnerability Scoring System): A system designed to classify vulnerabilities based in their attributes and their possible effects.

### Operating systems

Every OS provides tools and mechanisms to guarantee the security of the system.

#### User management

The users must have the lowest privilege they need to operate the system and they must belong to only the necessary groups.

#### Filesystem

Needs:
* Confidentiality.
* Disponibility.
* Integrity.

Protections:
* Encrypted filesystems. They require the password at boot time.
* Secure file deletion. Tools like Scrub and Shred.

Types of alterations:
* In the data.
* In the programs. These ones are very dangerous.

#### Random alterations

Hardware alterations can be, for example:
* Memory, disks, USBs... Those can be prevented by using RAID architecture and doing regular backups.
* Power supply. A UPS prevents this of happening.

Software alterations are caused by:
* Bad program design.
* Programs in an inconsistent state.
* Users with wrong privileges.

#### Alterations prevention

To prevent alterations from happening, several things can be done:
* Use digital signature to check the authenticity of a file.
* Use CRC and hashes to verify the integrity of a file.
* Journaling: Log almost everything that happens in the system.

### Log files

Their goal is to monitorize the system so in case something bad happens we can look the logs to figure out what the cause of the problem was.

The logs:
* Store important events.
* Can be local or remote.
* Detect errors.
* Are produced by programs like Snare, ObserveIt, LogAnalyzer...
* In Linux they are store in the `/var/log` directory:
  * syslog
  * messages
  * auth.log
  * utmp
  * wtmp
  * btmp
  * lastlog
  * debug
  * apache
  * daemon
  * kern.log
  * user.log

### Access control

The goal is to authenticate that someone is who they say they are.

To do that, we check for something that:
* They know.
* They have.
* They are.

The authentication system must satisfy several characteristics:
* Very reliable.
* Economic.
* Stand strong against certain attacks.
* Acceptable by the users.

Password authentication system:
* Simple and cheap.
* The responsibility lies with the user.
* A hash of the password is stored.
* If the passwords are not hashed with salt, the hashes can be susceptible to a Rainbow table attack.
* To create strong passwords there are several systems out there, e.g. Diceware.

Card authentication system:
* Can be chipcards or smartcards.

Biometric authentication system:
* Iris, palm of the hand, fingerprint,...

### Secure programming

Secure code development, without vulnerabilities.

### Vulnerabilities

In order to detect them, most of the time a pentest or a security audit is necessary.

Types of vulnerabilities:
* Buffer overflow: Until 2004 they were the cause of half of the total discovered vulnerabilities.
  
  To get rid of them, there are several things that can be done:
  * Never trust the user inputs.
  * Disable code execution on the stack.
  * Use stackguard.
* Race conditions: Not use of critical sections. To fix them, use the tools that the operating system gives you e.g. semaphores.
* Common programming errors: Improper file management, not checking the inputs correctly, XSS...
* SQLi: One of the most common vulnerability that webpages have. Always check the user input before doing consults to a database with it.
* Rootkits: Persistent threat that provides the attacker root privileges when wanted. Very hard to detect, as they work at kernel level, but there are several tools like chkrootkit and rkhunter.

## 04 - Network security

The Internet comes with challenges that were never thought about, as it allows everything to be available from anywhere in the world 24/7.

The access control becomes harder. New security needs appear, to make safe services that were not made for so. e.g. WEP, GSM.

### Types of attacks

Active, that can be easily detected but are very hard to prevent:
* Interruption.
* Fabrication.
* Modification.

Passive, that are very hard to detect, but can erradicated with encryption and protections:
* Interruption.

### Network security

Only enable the necessary services and take countermeassures against their vulnerabilities.

A firewall controls the packets that enter and exit the system.

The network services can be:
* Independent: Like any other program
* Managed by *inetd*: The daemon *inetd* wakes up the process when needed, when not, it shuts them down. The behaviour can be configured in the file /etc/inetd.conf

#### TCP wrappers

In order for this work, the programs must be compiled with *libwrap* support.

In the file /etc/hosts.deny you specifiy the denied access, and in /etc/hosts.allow you specify the allowed access.

#### Sysctl

This allows us to communicate with the kernel in execution time. By editing the file /etc/sysctl.conf we can do things like:
* Ignore all ping requests: `net.ipv4.icmp_echo_ignore_all=1`
* Ignore all ping broadcasts: `net.ipv4.icmp_echo_ignore_broadcasts=1`
* Refuse to send packets with invalid IP addresses: `net.ipv4.conf.all.rp_filter=1` (Can be 0,1 or 2).
* Log the packets with an invalid IP: `net.ipv4.conf.all.log_martians=1`

To apply the changes run `sysctl -p`

#### Service checking

To check the network services that are running, tools like *netstat* and *nmap* can be used.

### Attacks

#### DoS

Connectivity lost due to port, network or resources saturation. Up to 3 years in jail.

#### IP spoofing

Send packets with a fake origin IP.

If you send ICMP ECHO REQUEST packages with the victim IP as the origin one, this attack is called *smurfing*.

Nowadays the routers don't allow to send broadcast datagrams outside their subnets of reach.

#### ARP spoofing, poisoning

Change the IP linked to a MAC. This can be done in a switched LAN.

To discover the hosts that are in reach: `arpscan -a`

To poison two victims a tool named *arpspoof* can be used, in conjunction with `echo 1 > /proc/sys/net/ipv4/ip_forward`

Countemeasures:
* Router with static MAC.
* Use `arpwatch` to monitorize the network.

#### TCP SYN flood

Takes advantage of the 3-way-handshake, as the server allocates resources when a SYN packet is received, and they are not released after 75 seconds have passed. It results in a *OOM* most of the time.

This can be erradicated:
* SYN cookies: Only allocate resources when the final message is received.
* SYN cache: Independent structure that can't grow infinitely. By default in FreeBSD.

#### UDP flood

Send packets to random ports, and cause the victim to send ICMP destination unreachable messages back to the attacker (or fake the origin IP and put another target).

#### DNS spoofing

Return a fake IP after a DNS query is made and captured by the attacker.

#### Web spoofing

These days this is called phising.

#### Mail spoofing

Saying in an email that the sender is another person.

#### MITM

Signifficant attacks when Diffie-Hellman without authentication is being used.

### Iptables

Iptables is a tool that the Linux system provides to manage *netfilter*, which is a very powerful packet manipulation framework provided by the kernel.

It is a stateful firewall. This means that it will only examine the first packet of a connection, make a decission, and treat the rest of the packets the same way.

The framework consists of three main tables:
* Filter
* Mangle
* Nat

It also has another two tables:
* Raw: For managing the state of packets (as netfiler is a stateful firewall).
* Security: Only used to set internal SELinux security context marks on packets.

And each table has several chains linked to it (not every tables has all the chains):
* PREROUTING
* INPUT
* FORWARD
* OUTPUT
* POSTROUTING

The following diagram represents the flow of the packets through the chains:

![IPtables tables](/images/sss-iptables-tables.jfif)

There are several options that can apply to a packet:
* ACCEPT
* DROP
* REJECT
* LOG
* SNAT
* DNAT
* MASQUERADE

### Tools

*Ettercap*, now replaced by Bettercap.

*Packit*: A tool to craft network packets and to do tests with.

*Hping3*: Like ping, but better.

### Sniffer detection

This is a very hard thing to do, but there are some tools that can help you to do so, like *Sniffdet*.

To make if more difficult to sniff network traffic, here are some things that can be done:
* Network and hosts segmentation using switches (but you gotta be careful with ARP poisoning).
* Encrypted communications.

### SNORT

Snort is an IDS (Intrusion Detection System), specifically a NIDS.

It's got filters, rules, abnormal events detector and a module for making reports and managing alarms.

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