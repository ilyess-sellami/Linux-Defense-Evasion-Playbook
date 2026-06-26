# 01 - Log Tampering

## Overview

Log Tampering refers to techniques used to interfere with Linux logging mechanisms in order to reduce the visibility of malicious activity. Attackers may attempt to delete logs, modify log files, disable logging services, or manipulate log generation to hinder forensic investigations and incident response.

---

## Linux Logging Architecture

Common log sources in Linux:

* System Logs (`/var/log/syslog`, `/var/log/messages`)
* Authentication Logs (`/var/log/auth.log`, `/var/log/secure`)
* Kernel Logs
* Journal Logs (`systemd-journald`)
* Audit Logs (`auditd`)
* Application Logs (Apache, Nginx, SSH, databases, etc.)

Depending on the Linux distribution, logs are commonly managed by **rsyslog**, **syslog-ng**, or **systemd-journald** and stored under the `/var/log/` directory or within the systemd journal.

---

## Common Log Tampering Techniques

### 1. Clearing Log Files

Attackers may attempt to erase log contents to remove evidence of executed commands or unauthorized access.

**Example Commands**

```bash
truncate -s 0 /var/log/auth.log ## (sets the size of the file to 0 bytes)
truncate -s 0 /var/log/syslog
echo "" > /var/log/secure
```

---

### 2. Deleting Log Files

Instead of clearing logs, attackers may remove them entirely.

**Example Commands**

```bash
rm /var/log/auth.log
rm /var/log/syslog
rm /var/log/messages
rm /var/log/secure
```

Typical log locations:

```text
/var/log/
/var/log/journal/
/var/log/auth.log
/var/log/secure
/var/log/syslog
/var/log/messages
```

---

### 3. Disabling Logging Services

Stopping or disabling logging services reduces system visibility.

**Example Commands**

```bash
systemctl stop rsyslog
systemctl stop systemd-journald
systemctl stop auditd
```

Disabling services at boot:

```bash
systemctl disable rsyslog
systemctl disable systemd-journald
```

---

### 4. Journal Log Removal

On systems using **systemd**, attackers may attempt to remove journal logs.

**Example Commands**

```bash
journalctl --rotate
journalctl --vacuum-time=1s
rm -rf /var/log/journal/*
```

---

### 5. Selective Log Manipulation

Rather than deleting all logs, attackers may attempt to reduce suspicion by modifying specific entries or generating misleading activity.

Examples include:

* Removing specific log entries
* Modifying timestamps
* Generating excessive log noise
* Altering application log files
* Deleting shell history (`.bash_history`)
* Changing the system clock to affect log timestamps