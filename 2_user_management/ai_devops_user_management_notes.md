# 🧑‍💻 Linux User & Group Management (DevOps Basics)

---

## 🔐 User ID (UID) Classification

| User Type       | UID Range      |
|-----------------|----------------|
| **Root**        | `0` (always)   |
| **System Users**| `1 - 999`      |
| **Local Users** | `1000 - 60000` |

> Note: `60000` is configurable.

---

## ❓ Why Not Use Root for Everyday Tasks?

- Sharing the root account makes it difficult to **track user-specific actions**.
- It's safer to give users **sudo access**, which grants temporary root privileges.

---

## 👥 User & Group Files

### 📄 `/etc/passwd`
```bash
sudo cat /etc/passwd
```
- Lists all users in the system.
- Fields are colon `:` separated:
  
  ```
  username:password_placeholder:uid:gid:home_dir:default_shell
  ```
  Example:
  ```
  root:*:0:0:System Administrator:/var/root:/bin/sh
  ```

> 🔹 `group` is useful to assign permissions collectively.

---

### 📄 `/etc/shadow`

```bash
sudo cat /etc/shadow
```

- Contains **password info** (hashed) for users.
- Example:
  ```
  ubuntu:!:20195:0:99999:7::
  ```
- `!` in password field means:
  - User may not have a password or password login is disabled.
  - Key-based login might still be enabled.

---

### 👁️ Check User Info

```bash
id <username>
```

Example:
```bash
id ubuntu
```

- Returns: UID, GID, and group memberships.

---

## 👤 User Management

### ➕ Add Users

#### Low-level Command:
```bash
sudo adduser chaicode
```
- Not recommended:  
  - Doesn't create password, home directory, or skeleton files.

#### Recommended Command:
```bash
sudo adduser chetan
```
Automatically:
- Assigns UID & GID
- Creates group & home directory
- Copies skeleton files (`/etc/skel`)
- Prompts for password & user details

---

### ❌ Delete Users

```bash
sudo userdel -r chaicode
```
- `-r`: Recursively removes user's home directory and files.

---

### 🔄 Modify Users

#### Rename User:
```bash
sudo usermod -l chetanrakheja chetan
```
- Changes username from `chetan` to `chetanrakheja`.
- Does **not** change home directory.

#### Change Home Directory:
```bash
sudo usermod -d /newdir chetan
```

---

## 👥 Group Management

### View Groups:
```bash
sudo cat /etc/group
```

### Add Group:
```bash
sudo addgroup developers
```

### Delete Group:
```bash
sudo delgroup dev
```

### Add User to Group:
```bash
sudo usermod -aG developers chetan
```
- `-a`: Append (don’t overwrite existing groups)
- `-G`: Specify group(s)

---

## 🔐 SSH & User Authentication

### Enable Password Login (Not Recommended)

1. Edit SSH config:
   ```bash
   cd /etc/ssh/sshd_config.d/
   sudo vim 60-cloud-settings.conf
   ```
2. Set:
   ```
   PasswordAuthentication yes
   ```

> Restart SSH service:
```bash
sudo systemctl restart ssh
```

---

### Switch User
```bash
su - <username>
```

---

### SSH Key-based Login Setup (for user `pokemon`)

1. Create `.ssh` folder:
   ```bash
   sudo -u pokemon mkdir -p /home/pokemon/.ssh
   ```

2. Set correct permissions:
   ```bash
   sudo chmod 700 /home/pokemon
   ```

3. Generate SSH Key:
   ```bash
   ssh-keygen -t rsa -b 4096 -f ~/Desktop/id_rsa.chetanrakheja
   ```

4. Add Public Key to Server:
   ```bash
   echo '<.pub content>' | sudo -u pokemon tee /home/pokemon/.ssh/authorized_keys
   ```

5. Secure authorized_keys file:
   ```bash
   sudo chmod 600 /home/pokemon/.ssh/authorized_keys
   ```

---

## 🔐 Root Account Management

### Set Root Password:
```bash
sudo passwd root
```

### Enable Root SSH Login (Not Recommended):

1. Edit:
   ```bash
   sudo vim /etc/ssh/sshd_config.d/60-cloud-settings.conf
   ```

2. Add:
   ```
   PermitRootLogin yes
   ```

3. Restart SSH:
   ```bash
   sudo systemctl restart ssh
   ```

---

## 🛡️ Sudo Access & Permissions

### What is `sudo`?
- **"Super User Do"**: Run commands with root privileges.
- By default, **not all users** have sudo rights.

### Add User to Sudo Group:
```bash
sudo usermod -aG sudo pokemon
```

> In **RHEL/CentOS**, use `wheel` group instead of `sudo`.

---

### View/Edit Sudo Permissions

#### View:
```bash
sudo cat /etc/sudoers
```

> ❗ Don't edit this file directly.

#### Edit Safely:
```bash
sudo visudo
```