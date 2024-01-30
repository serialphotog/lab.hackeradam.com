---
title: "NMap Reference"
description: "My quick reference for working with NMap"
date: '2024-01-27'
---

This is my personal quick reference for working with NMap.

| Flag | Description |
| ---- | ---- |
| -sV | Attempts to determine the version of the services that are running |
| -p `<x>` or -p- | Port scan for port `<x>` or scan all ports |
| -Pn | Disable host discovery and just scan for open ports |
| -A | Enables OS and version detection, executes in-build scripts for further enumeration. Aggressive Mode. |
| -sC | Scan with the default nmap script |
| -v | Verbose mode |
| -sU | UDP port scan |
| -sS | TCP SYN port scan |
| -Pn | Disable the ping scan |
| -T5 | Sets to time template level 5. |
| --script | Run with a script. |

## Example

Perform a verbose scan, attempt to identify the version of services that are running, enable OS and version detection with in-build scripts for further enumeration, and scan with the default NMap script:

```bash
nmap -v -sV -A -sC 10.10.58.39
```

# Scripts

A list of the built-in scripts can be found [here](https://nmap.org/nsedoc/scripts/).

**NOTE:** NMap stores its scripts in `/usr/share/nmap/scripts`.

# Firewall Evasion Techniques

| flag | Description |
|-----|-------------|
| -f | Fragment packets to make them harder for a firewall/IDS to detect. |
| --scan-delay `<time>`ms | Adds a delay between each sent packet |
| --badsum | Generates an invalid checksum for packets. Real TCP/IP stacks should drop this packet, however firewalls may respond automatically. As such, this can detect the presence of a firewall/IDS |

**NOTE:** More firewall evasion techniques can be found [here](https://nmap.org/book/man-bypass-firewalls-ids.html).