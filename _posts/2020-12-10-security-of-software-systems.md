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