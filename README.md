# Network Reconnaissance Lab

## Project Summary

A hands-on network reconnaissance project conducted in an isolated VirtualBox lab environment using Kali Linux as the assessment machine and Ubuntu Linux as the target.

The project demonstrates a basic reconnaissance workflow including network configuration, connectivity testing, host discovery, TCP port scanning, full-range TCP enumeration, service/version detection, HTTP reconnaissance, security observations, and technical documentation.

> **Lab environment:** VirtualBox
> **Assessment machine:** Kali Linux (`192.168.56.105`)
> **Target machine:** Ubuntu Linux (`192.168.56.104`)
> **Network:** `192.168.56.0/24`

---

## Key Findings

| Port            | Service | Version       | Observation                             |
| --------------- | ------- | ------------- | --------------------------------------- |
| `22/tcp`        | SSH     | OpenSSH 8.9p1 | Remote administration service exposed   |
| `80/tcp`        | HTTP    | Apache 2.4.52 | Web server responding                   |
| Other TCP ports | —       | —             | No additional open TCP ports identified |

The full TCP scan identified **2 open TCP ports out of 65,535**:

```text
22/tcp  open  ssh
80/tcp  open  http
```

---

## Objectives

* Configure an isolated cybersecurity lab network.
* Verify connectivity between Kali Linux and Ubuntu.
* Discover active hosts on the lab network.
* Identify open TCP ports on the Ubuntu target.
* Scan the complete TCP port range.
* Identify services and versions running on open ports.
* Perform basic HTTP service reconnaissance.
* Document findings and supporting evidence.
* Develop security recommendations based on reconnaissance results.

---

## Tools & Technologies

* Kali Linux
* Ubuntu Linux
* VirtualBox
* Nmap
* Curl
* Linux command line
* TCP/IP networking

---

## Lab Network

The virtual machines were connected through the `192.168.56.0/24` lab network.

| Machine      | IP Address       | Role                              |
| ------------ | ---------------- | --------------------------------- |
| Kali Linux   | `192.168.56.105` | Security testing / reconnaissance |
| Ubuntu Linux | `192.168.56.104` | Target                            |

All reconnaissance activities were performed against the intentionally configured Ubuntu virtual machine in a controlled lab environment.

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

* Kali: `192.168.56.105`
* Ubuntu: `192.168.56.104`

---

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
nmap -sn 192.168.56.1/24
```

This scan was used to identify live systems without performing a traditional TCP port scan.

---

## 4. Initial Port and Service Scan

The Ubuntu target was scanned using:

```bash
nmap -sV 192.168.56.104
```

### Results

| Port     | State | Service | Version                          |
| -------- | ----- | ------- | -------------------------------- |
| `22/tcp` | Open  | SSH     | OpenSSH 8.9p1 Ubuntu 3ubuntu0.16 |
| `80/tcp` | Open  | HTTP    | Apache httpd 2.4.52              |

The scan identified SSH and HTTP services running on the target.

---

## 5. Full TCP Port Scan

To examine the complete TCP port range rather than Nmap's default 1,000 ports:

```bash
nmap -p- 192.168.56.104
```

### Results

```text
22/tcp  open  ssh
80/tcp  open  http
```

Only two TCP ports were identified as open across the full `1–65535` range.

The scan reported **65,533 closed TCP ports**.

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

Nmap identified SSH host keys, including ECDSA and ED25519 keys.

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

## Finding 1 — SSH Service Exposed

**Port:** `22/tcp`
**Service:** SSH
**Version:** OpenSSH 8.9p1

SSH provides remote administration capabilities. In a production environment, SSH exposure should be appropriately restricted and protected through strong authentication, access controls, and secure configuration.

### Recommendations

* Restrict SSH access to authorized systems or networks where appropriate.
* Use strong authentication.
* Disable unnecessary authentication methods.
* Keep OpenSSH and the underlying operating system updated.
* Monitor authentication attempts and suspicious login activity.

---

## Finding 2 — HTTP Service Exposed

**Port:** `80/tcp`
**Service:** Apache HTTP Server
**Version:** Apache 2.4.52

An Apache web server was reachable from the Kali assessment machine.

### Recommendations

* Keep Apache and the operating system patched.
* Remove unnecessary default content.
* Review Apache configuration and enabled modules.
* Use HTTPS where appropriate for production applications.
* Limit unnecessary exposure of web services.

---

## Finding 3 — Limited TCP Service Exposure

The full TCP scan identified only two open ports:

```text
22/tcp
80/tcp
```

This represents a relatively limited TCP service exposure within the lab environment.

Reducing unnecessary exposed services can help reduce the potential attack surface of a system.

---

# Evidence

Supporting screenshots and scan outputs are organized in the following directories:

```text
screenshots/
scans/
findings/
```

### Screenshots

```text
screenshots/
├── kali-ip.png
├── ubuntu-ip.png
├── connectivity-test.png
├── nmap-service-scan.png
└── full-port-scan.png

```

### Scan Results

```text
scans/
├── host-discovery.txt
├── full-port-scan.txt
└── service-enumeration.txt
```

### Findings Report

```text
findings/
└── reconnaissance-report.md
```

---

# Skills Demonstrated

* Network reconnaissance
* Host discovery
* TCP port scanning
* Full-range TCP enumeration
* Service and version detection
* SSH reconnaissance
* HTTP reconnaissance
* Linux command-line administration
* IP addressing and subnetting
* Basic network security analysis
* Technical documentation

---

# Lessons Learned

This lab provided hands-on practice with a basic network reconnaissance workflow:

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

The project reinforced the importance of identifying exposed services and understanding a system's network attack surface before performing deeper security analysis.

---

# Ethical & Legal Scope

All scanning activities in this project were performed against virtual machines that I configured and controlled for cybersecurity training.

Nmap and other reconnaissance tools should only be used against systems for which you have explicit authorization.

---

# Future Improvements

Potential extensions to this lab include:

* Capture reconnaissance traffic with Wireshark.
* Perform additional HTTP enumeration.
* Analyze TCP connection behavior.
* Review Apache and SSH configurations on the Ubuntu target.
* Document remediation recommendations.
* Add additional screenshots and raw scan evidence.
* Compare reconnaissance results before and after security hardening.
