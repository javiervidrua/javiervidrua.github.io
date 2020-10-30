---
layout: post
title:  "Symmetric key encryption"
date:   2020-10-04 00:00:00 +0200
categories: jekyll update
---

I was bored, taking a look at the syllabus of a subject I'll be taking this year, and saw that I'll learn ciphers, symmetric and asymmetric.
So I though: "Do I know enough about this? Can I encrypt something, for example, with a symmetric key?"

The answer to that question was: "Not now, but we'll see"

PD: 10 minutes later it was done, and I wrote this post too, so that's a win for me :P

That said, the first thing I did was investigate to see what encryption protocol would be the best for the use. I wanted to create a script to encrypt a file, and another one to decrypt a file.
After a Duckduckgo search I found [this](https://criptografia.webnode.es/algoritmos-simetricos/), and after looking and comparing all the encryption protocols I decided to use ARIA, because of its robustness and speed.

I created two very simple bash scripts, that rely on the openssl tool. To be able to use them, first you need to install it with the command: `apt install openssl`

Needless to say that **in order to be able to encrypt a file, you'll need to use a passphrase**, in case of these scripts, you'll be asked to enter it when encrypting and decrypting files.

### encrypt.sh
``` bash
#!/bin/bash

function usage(){
	echo '[*] Usage: ./encrypt.sh <unencrypted_file> <encrypted_file>'
}

if [ $# -ne 2 ]; then
	usage
	exit
fi
UNENCRYPTED_FILE=$1
ENCRYPTED_FILE=$2
openssl enc -aria-256-ecb -in ${UNENCRYPTED_FILE} -out ${ENCRYPTED_FILE} -iter 69
```

### decrypt.sh
``` bash
#!/bin/bash

function usage(){
	echo '[*] Usage: ./decrypt.sh <encrypted_file> <decrypted_file>'
}

if [ $# -ne 2 ]; then
	usage
	exit
fi
ENCRYPTED_FILE=$1
DECRYPTED_FILE=$2
openssl enc -aria-256-ecb -in ${ENCRYPTED_FILE} -out ${DECRYPTED_FILE} -iter 69 -d
```

## Example
Let's say that I have a file named "*secret.txt*":
* To encrypt it: `./encrypt.sh secret.txt secret-encrypted.txt`
* To decrypt it: `./decrypt.sh secret-encrypted.txt secret-decrypted.txt`
