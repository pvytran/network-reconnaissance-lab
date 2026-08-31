# Network Reconnaissance Lab

## Overview

This project documents a hands-on network reconnaissance lab performed in an isolated virtual environment using **Kali Linux** as the assessment machine and **Ubuntu Linux** as the target.

The goal was to practice host discovery, TCP port scanning, service/version enumeration, and basic HTTP reconnaissance while documenting the results in a professional cybersecurity portfolio.

> **Lab environment:** VirtualBox  
> **Assessment machine:** Kali Linux  
> **Target machine:** Ubuntu Linux  
> **Target IP:** `192.168.56.104`  
> **Kali IP:** `192.168.56.105`

---

## Objectives

- Configure an isolated cybersecurity lab network.
- Verify connectivity between Kali Linux and Ubuntu.
- Discover active hosts on the lab network.
- Identify open TCP ports on the Ubuntu target.
- Scan the complete TCP port range.
- Identify services and versions running on open ports.
- Perform basic HTTP service reconnaissance.
- Document findings and evidence for a cybersecurity portfolio.

---

## Tools & Technologies

- Kali Linux
- Ubuntu Linux
- VirtualBox
- Nmap
- Curl
- Linux command line
- TCP/IP networking

---

## Lab Network

The virtual machines were connected through the `192.168.56.0/24` lab network.

| Machine | IP Address | Role |
|---|---|---|
| Kali Linux | `192.168.56.105` | Security testing / reconnaissance |
| Ubuntu Linux | `192.168.56.104` | Target |

The reconnaissance activities were performed against the intentionally configured Ubuntu virtual machine in my controlled lab environment.

---

# Reconnaissance Process

## 1. Verify Network Configuration

Kali Linux:

```bash
ip addr
```

Ubuntu Linux:

```bash
ip addr
```

The lab interfaces were configured with:

- Kali: `192.168.56.105`
- Ubuntu: `192.168.56.104`

## 2. Test Connectivity

From Kali:

```bash
ping -c 4 192.168.56.104
```

This verified network communication between the assessment machine and the target.

---

## 3. Host Discovery

The lab network was checked for active hosts:

```bash
nmap -sn 192.168.56.0/24
```

This was used to identify live systems without performing a traditional port scan.

---

## 4. Initial Port and Service Scan

The Ubuntu target was scanned using:

```bash
nmap -sV 192.168.56.104
```

### Results

| Port | State | Service | Version |
|---|---|---|---|
| `22/tcp` | Open | SSH | OpenSSH 8.9p1 Ubuntu 3ubuntu0.16 |
| `80/tcp` | Open | HTTP | Apache httpd 2.4.52 |

The scan identified the target as a Linux system.

---

## 5. Full TCP Port Scan

To check all TCP ports rather than only Nmap's default 1,000 ports:

```bash
nmap -p- 192.168.56.104
```

### Results

Only the following TCP ports were open:

```text
22/tcp   open   ssh
80/tcp   open   http
```

The scan reported **65,533 closed TCP ports**, confirming that no additional open TCP ports were found across the full port range.

---

## 6. Service Enumeration

A more detailed scan was performed against the identified services:

```bash
nmap -sC -sV -p 22,80 192.168.56.104
```

### SSH

```text
22/tcp open ssh
OpenSSH 8.9p1 Ubuntu 3ubuntu0.16
```

Nmap also identified SSH host keys using ECDSA and ED25519.

### HTTP

```text
80/tcp open http
Apache httpd 2.4.52 (Ubuntu)
```

The HTTP title returned by Nmap was:

```text
Apache2 Ubuntu Default Page: It works
```

This confirms that an Apache web server was responding on port 80.

---

# Findings & Security Observations

### Finding 1 — SSH is exposed

**Port:** `22/tcp`  
**Service:** SSH

SSH provides remote administration capabilities. In a production environment, SSH exposure should be appropriately restricted and protected through strong authentication, access controls, and secure configuration.

### Finding 2 — HTTP is exposed

**Port:** `80/tcp`  
**Service:** Apache HTTP Server

An Apache web server is publicly reachable from the lab assessment machine. Web servers should be maintained with appropriate security updates and configuration controls.

### Finding 3 — Limited TCP attack surface

The full TCP scan identified only two open ports out of 65,535:

```text
22/tcp
80/tcp
```

This demonstrates a relatively limited TCP service exposure within the lab environment.

---

# Evidence

Screenshots and scan outputs can be stored in the following directories:

```text
screenshots/
scans/
findings/
```

Recommended evidence files:

```text
screenshots/
├── kali-ip.png
├── ubuntu-ip.png
├── connectivity-test.png
└── nmap-service-scan.png

scans/
├── host-discovery.txt
├── full-port-scan.txt
└── service-enumeration.txt

findings/
└── reconnaissance-report.md
```

---

# Skills Demonstrated

- Network reconnaissance
- Host discovery
- TCP port scanning
- Full-range TCP enumeration
- Service and version detection
- SSH reconnaissance
- HTTP reconnaissance
- Linux command-line administration
- IP addressing and subnetting
- Basic network security analysis
- Technical documentation

---

# Lessons Learned

Through this lab, I practiced the reconnaissance workflow used to understand the network exposure of a target system:

```text
Network configuration
        ↓
Connectivity verification
        ↓
Host discovery
        ↓
Port scanning
        ↓
Full TCP enumeration
        ↓
Service/version identification
        ↓
Security observations
        ↓
Documentation
```

The lab reinforced the importance of identifying exposed services before performing deeper security analysis.

---

# Ethical & Legal Scope

All scanning in this project was performed against virtual machines that I configured and controlled for cybersecurity training.

Nmap and other reconnaissance tools should only be used against systems for which you have explicit authorization.

---

# Future Improvements

Planned extensions to this lab include:

- Capture reconnaissance traffic with Wireshark.
- Perform additional HTTP enumeration.
- Analyze TCP connection behavior.
- Review Apache and SSH configurations on the Ubuntu target.
- Document remediation recommendations.
- Add screenshots and raw scan evidence.
