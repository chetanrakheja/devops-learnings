# 🛠️ Additional Linux User Management Commands

These commands are useful for managing users and groups beyond the basics.

---

## 👥 User Information

### 🔎 List Currently Logged In Users
```bash
who
```
or
```bash
w
```

### 🔎 Show Current User
```bash
whoami
```

### 🔎 List All Users (from /etc/passwd)
```bash
cut -d: -f1 /etc/passwd
```

### 🔎 Show Last Login for All Users
```bash
lastlog
```

---

## 📊 User Activity

### 👣 Show Login History
```bash
last
```

### 👁️ Show Current User Sessions
```bash
users
```

---

## 🔐 Password & Expiry Management

### 🔄 Force Password Change on Next Login
```bash
sudo chage -d 0 <username>
```

### ⏳ Set Password Expiry Info
```bash
sudo chage -M 90 -m 7 -W 14 <username>
```
- `-M`: Max days before password change required.
- `-m`: Min days between changes.
- `-W`: Days before expiry to warn user.

### 🗓️ View Password Expiry for a User
```bash
sudo chage -l <username>
```

---

## 👤 Create User with Expiry Date
```bash
sudo useradd -e 2025-12-31 <username>
```

---

## 🗂️ Manage User Home Directory

### 🔍 Check Home Directory Size
```bash
du -sh /home/<username>
```

### 🔄 Move User to a New Group (Primary Group)
```bash
sudo usermod -g <newgroup> <username>
```

---

## 🧼 Clean Up

### ❌ Lock/Disable a User Account
```bash
sudo usermod -L <username>
```

### ✅ Unlock a User Account
```bash
sudo usermod -U <username>
```

### ❌ Expire a User Account Immediately
```bash
sudo usermod -e 1970-01-01 <username>
```

---

## 🧪 Test User Shell or Environment

### 🔄 Switch to Another User’s Environment
```bash
sudo -i -u <username>
```

### 🐚 Start a Login Shell for Another User
```bash
sudo su - <username>
```

---

## 📁 Create Custom Skeleton Directory for New Users

1. Create a new skeleton dir (optional):
```bash
sudo mkdir /etc/custom_skel
```

2. Use with useradd:
```bash
sudo useradd -m -k /etc/custom_skel <username>
```

---

## 🔁 Copy Default .bashrc, .profile to Existing User

```bash
sudo cp /etc/skel/.bash* /home/<username>/
sudo chown <username>:<username> /home/<username>/.bash*
```

---

Let me know if you want cheatsheets or quick references for these.