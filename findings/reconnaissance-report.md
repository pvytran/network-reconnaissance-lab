# Network Reconnaissance Findings Report

## 1. Assessment Overview

This report documents the results of a network reconnaissance assessment performed in an isolated VirtualBox lab environment.

The assessment was conducted from a Kali Linux virtual machine against an Ubuntu Linux virtual machine that I configured and controlled for cybersecurity training.

**Assessment Machine:** Kali Linux
**Assessment IP:** `192.168.56.105`
**Target Machine:** Ubuntu Linux
**Target IP:** `192.168.56.104`
**Lab Network:** `192.168.56.1/24`

---

## 2. Objective

The objective of this assessment was to identify the target's network exposure by:

* Verifying connectivity between the lab systems.
* Discovering active hosts.
* Identifying open TCP ports.
* Scanning the complete TCP port range.
* Identifying services and software versions.
* Performing basic HTTP reconnaissance.
* Documenting security observations and recommendations.

---

## 3. Connectivity Verification

Connectivity between Kali Linux and the Ubuntu target was verified using:

```bash
ping -c 4 192.168.56.104
```

The successful connectivity test confirmed that the assessment machine could communicate with the target over the isolated lab network.

---

## 4. Host Discovery

The lab network was scanned using:

```bash
nmap -sn 192.168.56.1/24
```

Host discovery was used to identify active systems on the `192.168.56.1/24` network before performing more targeted reconnaissance.

---

## 5. Service Enumeration

The Ubuntu target was scanned using:

```bash
nmap -sV 192.168.56.104
```

The following open TCP services were identified:

| Port     | Service | Version                          |
| -------- | ------- | -------------------------------- |
| `22/tcp` | SSH     | OpenSSH 8.9p1 Ubuntu 3ubuntu0.16 |
| `80/tcp` | HTTP    | Apache httpd 2.4.52              |

---

## 6. Full TCP Port Scan

A complete TCP port scan was performed using:

```bash
nmap -p- 192.168.56.104
```

The scan examined the full TCP port range from `1` through `65535`.

Only two open TCP ports were identified:

```text
22/tcp  open  ssh
80/tcp  open  http
```

The remaining **65,533 TCP ports were closed**, based on the scan results.

This indicates that the target had a relatively limited number of exposed TCP services within the lab environment.

---

## 7. Detailed Service Enumeration

A more detailed scan was performed using:

```bash
nmap -sC -sV -p 22,80 192.168.56.104
```

### SSH — Port 22

The scan identified:

```text
22/tcp open ssh
OpenSSH 8.9p1 Ubuntu 3ubuntu0.16
```

SSH provides remote administration functionality.

Nmap also identified SSH host keys, including ECDSA and ED25519 keys.

### HTTP — Port 80

The scan identified:

```text
80/tcp open http
Apache httpd 2.4.52 (Ubuntu)
```

The HTTP service returned the following page title:

```text
Apache2 Ubuntu Default Page: It works
```

This confirmed that an Apache web server was responding to HTTP requests from the assessment machine.

---

# 8. Security Findings

## Finding 1 — SSH Exposure

**Severity:** Informational / Configuration Review

**Port:** `22/tcp`
**Service:** SSH
**Software:** OpenSSH 8.9p1

SSH was accessible from the Kali assessment machine.

SSH is commonly required for remote administration, but unnecessary network exposure can increase the attack surface of a system.

### Recommendations

* Restrict SSH access to authorized users and networks.
* Use strong authentication.
* Review SSH configuration for unnecessary access methods.
* Keep OpenSSH and the operating system updated.
* Monitor authentication activity and failed login attempts.

---

## Finding 2 — HTTP Service Exposure

**Severity:** Informational / Configuration Review

**Port:** `80/tcp`
**Service:** Apache HTTP Server
**Software:** Apache 2.4.52

An Apache HTTP server was accessible from the Kali assessment machine.

The default Apache page was identified during reconnaissance.

### Recommendations

* Remove unnecessary default web content in production environments.
* Keep Apache and the operating system updated.
* Review Apache configuration and enabled modules.
* Use HTTPS where appropriate.
* Restrict web services that do not need to be publicly accessible.

---

## Finding 3 — Limited TCP Attack Surface

**Severity:** Informational

The full TCP scan identified only two open ports:

```text
22/tcp
80/tcp
```

No additional open TCP ports were identified across the complete `1–65535` port range.

Limiting unnecessary network services can reduce a system's potential attack surface.

---

# 9. Overall Assessment

The reconnaissance assessment identified two exposed TCP services on the Ubuntu target:

```text
22/tcp — SSH
80/tcp — HTTP
```

The assessment demonstrated a basic reconnaissance workflow from initial connectivity verification through host discovery, port scanning, service identification, and security observation.

The results provide a baseline for future security hardening and additional testing within the controlled lab environment.

---

# 10. Evidence

Supporting evidence for this assessment is maintained in the GitHub repository.

### Scan Results

```text
scans/
├── host-discovery.txt
├── full-port-scan.txt
└── service-enumeration.txt
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

---

# 11. Skills Demonstrated

* Network reconnaissance
* Host discovery
* TCP port scanning
* Full-range TCP enumeration
* Service/version detection
* SSH reconnaissance
* HTTP reconnaissance
* Linux command-line usage
* IP addressing and subnetting
* Basic security analysis
* Technical documentation

---

# 12. Ethical Scope

All reconnaissance activities documented in this report were performed against virtual machines that I configured and controlled for cybersecurity training.

Nmap and other security testing tools should only be used against systems for which you have explicit authorization.

