# 02 - Linux Security Solutions Evasion

## Overview

Linux Security Solutions Evasion refers to techniques used to avoid detection or reduce the effectiveness of endpoint security products, logging frameworks, and monitoring solutions. Attackers may attempt to disable security services, terminate monitoring agents, exclude files from scanning, or execute malicious activity in ways that blend with normal system administration.

---

## Common Linux Security Solutions

Organizations commonly deploy the following security solutions on Linux systems:

* Endpoint Detection and Response (EDR)
* Antivirus (AV)
* File Integrity Monitoring (FIM)
* Audit Framework (`auditd`)
* Security Information and Event Management (SIEM)
* Intrusion Detection Systems (IDS)
* Container Runtime Security (Falco, etc.)

These solutions continuously monitor processes, file activity, authentication events, network connections, and system changes.

---

## Common Security Evasion Techniques

### 1. Disabling Security Services

Attackers may attempt to stop monitoring services to reduce visibility.

**Example Commands**

```bash
systemctl stop auditd
systemctl stop falco
systemctl stop clamav-daemon
```

---

### 2. Disabling Services at Boot

Preventing security tools from automatically starting after a reboot.

**Example Commands**

```bash
systemctl disable auditd
systemctl disable falco
systemctl disable clamav-daemon
```

---

### 3. Killing Security Processes

Instead of stopping a service, attackers may terminate the running monitoring process.

**Example Commands**

```bash
pkill auditd
pkill falco
pkill clamd
kill <PID>
```

---

### 4. Executing Trusted System Binaries

Rather than introducing custom tools, attackers often leverage trusted Linux utilities already present on the system.

Common examples include:

* `bash`
* `sh`
* `find`
* `awk`
* `sed`
* `curl`
* `wget`
* `python`
* `perl`
* `openssl`

Using legitimate binaries can make malicious activity appear similar to normal administrative operations.

---

### 5. File and Directory Exclusions

Security products may support exclusion rules that prevent specific files or directories from being scanned.

Examples include:

* Excluding temporary directories
* Excluding application directories
* Excluding specific file extensions
* Excluding backup locations

---

### 6. Running from Temporary or Memory-Backed Locations

Attackers may execute files from locations that are commonly used for temporary data.

Common locations include:

```text
/tmp/
/var/tmp/
/dev/shm/
```

These directories are frequently writable and often contain legitimate temporary files.

---

### 7. Living Off The Land

Instead of deploying additional binaries, attackers may rely on tools already installed on Linux systems to reduce their footprint.

Examples include:

* SSH
* SCP
* Rsync
* Tar
* Cron
* Systemctl
* Bash scripting

---

## Detection Opportunities

Defenders should monitor for:

* Security services unexpectedly stopping
* Disabled security agents
* Unexpected process termination
* Changes to security configurations
* Unauthorized exclusion rules
* Execution from temporary directories
* Unusual use of administrative utilities
* Missing or inactive monitoring agents