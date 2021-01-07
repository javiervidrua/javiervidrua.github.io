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
