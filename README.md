# 🔐 Windows Hello Reset (NGC Folder Permission Fix)

## 📌 Overview

This project documents a real-world identity and endpoint issue where **Windows Hello biometric data could not be removed** due to residual permissions from a previously joined domain.

The solution demonstrates how to:

* Regain control of protected system directories
* Resolve identity-related lockouts
* Restore local authentication functionality

---

## 🧠 IAM / Security Context

Windows Hello stores authentication data in a protected system directory:

```id="c1k9l2"
C:\Windows\ServiceProfiles\LocalService\AppData\Local\Microsoft\NGC
```

In enterprise environments, this folder may become:

* Owned by a **domain SID no longer present**
* Locked by **Group Policy remnants**
* Inaccessible even to local administrators

This creates a **broken authentication state**, preventing:

* Fingerprint removal
* PIN reset
* Reconfiguration of Windows Hello

---

## ⚠️ Problem Statement

* Unable to remove fingerprint via Settings
* "Access Denied" when modifying NGC folder
* Device previously domain-joined
* Standard admin privileges insufficient

---

## 🛠️ Solution: Ownership + Permission Reset

### Step 1: Open Elevated Command Prompt

---

### Step 2: Take Ownership

```id="9xk2pa"
takeown /f "C:\Windows\ServiceProfiles\LocalService\AppData\Local\Microsoft\NGC" /r /d y
```

---

### Step 3: Grant Full Control

```id="e2m4vb"
icacls "C:\Windows\ServiceProfiles\LocalService\AppData\Local\Microsoft\NGC" /grant administrators:F /t
```

---

### Step 4: Remove Corrupted Data

```id="7k1qwd"
rd /s /q "C:\Windows\ServiceProfiles\LocalService\AppData\Local\Microsoft\NGC"
```

---

### Step 5: Reboot System

---

## 🔄 Alternative Escalation Paths

### Safe Mode Execution

Run commands in Safe Mode to bypass file locks.

### Built-in Administrator Enablement

```id="n8v3zf"
net user administrator /active:yes
```

---

## ✅ Outcome

* Windows Hello fully reset
* Fingerprint/PIN reconfiguration restored
* Broken identity bindings removed
* System returned to clean authentication state

---

## 🧪 Real-World Use Case

This scenario is common in:

* Enterprise offboarding workflows
* Device re-provisioning
* Law firm / corporate IT environments
* Identity lifecycle management failures

---

## 📸 Screenshots (Optional but Recommended)

*Add these to strengthen the project:*

* Access denied error
* NGC folder permissions before/after
* Windows Hello settings screen

---

## 💡 Key Takeaways

* Identity data can persist beyond domain removal
* NTFS permissions directly impact authentication systems
* Local admin ≠ full control in enterprise-configured systems
* IAM troubleshooting often requires system-level intervention

---

## 🧰 Skills Demonstrated

* Identity & Access Management (IAM) fundamentals
* Windows OS internals
* NTFS permissions and ownership handling
* Endpoint troubleshooting
* Security-focused problem solving

---

## 👤 Author

**Edward Casanova**
Service Desk Analyst → IAM Engineer (In Progress)

---

## ⭐ Why This Project Matters

This project highlights the intersection of:

* Identity systems
* Endpoint security
* Real-world troubleshooting

It demonstrates the ability to go beyond surface-level fixes and resolve **root-cause authentication issues**, a key skill in IAM and security engineering roles.
