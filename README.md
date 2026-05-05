# 🔐 John the Ripper - Demonstrations on Kali Linux

This repository contains the commands and examples used in the presentation about **John the Ripper**, a password auditing tool. The demonstrations are designed to run on **Kali Linux** (virtual machine or native).

## Installation

John the Ripper usually comes preinstalled on Kali. To check the version:

```bash
john --version

sudo apt update
sudo apt install john -y

cd /usr/share/wordlists
sudo gunzip rockyou.txt.gz
ls -lah | grep rockyou

```

---

## Test environment setup

Create a working folder and a file with an example MD5 hash (the password is `password`):

```bash
mkdir -p ~/john_demo
cd ~/john_demo
printf "password" | md5sum | cut -d' ' -f1 > hashes.txt
```

---

## Demonstration 1: Basic attack (auto-detection)

```bash
john hashes.txt
```

---

## Demonstration 2: Dictionary attack with `rockyou.txt`

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 hashes.txt
```

---

## Demonstration 3: Brute force (incremental) – digits only

```bash
printf "12345" | md5sum | cut -d' ' -f1 > hash_numeric.txt
john --incremental=Digits --format=raw-md5 hash_numeric.txt
```

---

## Demonstration 4: Mask attack (predefined pattern)

```bash
printf "abc123" | md5sum | cut -d' ' -f1 > hash_mask1.txt
john --mask='?l?l?l?d?d?d' --format=raw-md5 hash_mask1.txt

printf "abcd1234" | md5sum | cut -d' ' -f1 > hash_mask2.txt
john --mask='?l?l?l?l?d?d?d?d' --format=raw-md5 hash_mask2.txt
```

---

## Demonstration 5: Crack user pasword from txt file
```bash
echo 'admin:482c811da5d5b4bc6d497ffa98491e38' > hash_md5.txt
cat hash_md5.txt
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_md5.txt
```
---

## Demonstration 6: Crack real user passwords from the system

```bash
sudo useradd -m testuser

echo "testuser:password123" | sudo chpasswd -c MD5

sudo cat /etc/shadow | grep testuser

sudo cat /etc/shadow | grep testuser > ~/john_demo/demo3_shadow_md5.txt
cat /etc/passwd | grep testuser > ~/john_demo/demo3_passwd_md5.txt

sudo unshadow demo3_passwd_md5.txt demo3_shadow_md5.txt > demo3_hashes_md5.txt

john --wordlist=/usr/share/wordlists/rockyou.txt demo3_hashes_md5.txt

john --show demo3_hashes_md5.txt
```

## Demonstration 6: Crack a zip file
```bash
nano zipfile.txt
INFORME CONFIDENCIAL
Contraseña del ZIP: secret123

zip -P secret123 zipfile.zip zipfile.txt
zip2john zipfile.zip > zipfile_hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt zipfile_hash.txt
john --show zipfile_hash.txt
unzip -P secret123 zipfile.zip
```
---

## Remove Cache

```bash
rm ~/.john/john.pot
```
---
