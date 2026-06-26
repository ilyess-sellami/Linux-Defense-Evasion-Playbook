# 03 - Shell and Script Obfuscation

## Overview

Shell and Script Obfuscation refers to techniques used to make shell commands and scripts more difficult to read, analyze, or detect. Instead of executing commands in a clear and straightforward manner, attackers may rewrite them using encoding, variable expansion, string manipulation, or shell features to reduce the likelihood of detection by security tools and complicate manual analysis.

---

## Common Shell Obfuscation Techniques

### 1. Variable Expansion

Commands can be constructed dynamically using shell variables instead of writing them directly.

**Example**

```bash
CMD="whoami"
$CMD
```

---

### 2. Command Substitution

Shells can execute the output of another command as part of a larger command.

**Example**

```bash
CURRENT_USER=$(whoami)
echo $CURRENT_USER
```

---

### 3. String Concatenation

Commands may be assembled from multiple strings before execution.

**Example**

```bash
PART1="who"
PART2="ami"

${PART1}${PART2}
```

---

### 4. Character Escaping

Special characters can be escaped to change the appearance of commands while preserving their behavior.

**Example**

```bash
w\h\o\a\m\i
```

---

### 5. Base64 Encoding

Scripts or commands may be stored in Base64 format and decoded immediately before execution.

**Example**

```bash
echo "d2hvYW1pCg==" | base64 -d
```

---

### 6. Hexadecimal Representation

Characters can be represented using hexadecimal values before execution.

**Example**

```bash
printf "\x77\x68\x6f\x61\x6d\x69\n"
```

---

### 7. Here Documents

Multi-line scripts can be embedded directly within a shell command.

**Example**

```bash
cat << EOF
System Information
Hostname: $(hostname)
Current User: $(whoami)
EOF
```

---

### 8. Using Multiple Shell Interpreters

Commands may be executed through different interpreters.

Examples include:

```bash
bash script.sh
sh script.sh
dash script.sh
zsh script.sh
```

---

## Why Obfuscate Scripts?

Common objectives include:

* Making scripts more difficult to read
* Reducing signature-based detection
* Hiding the true intent of commands
* Delaying manual analysis
* Blending with legitimate shell activity

---

## Detection Opportunities

Defenders should monitor for:

* Excessive use of Base64 decoding
* Suspicious use of `eval`
* Dynamic command construction
* Unusual variable expansion
* Encoded or hexadecimal strings
* Execution of obfuscated shell scripts
* Scripts launched from temporary directories
* Unexpected child processes spawned by shell interpreters
