## 🔍 Task

MariaDB service was down on the Stratos Datacenter database server, causing the Nautilus application to lose database connectivity. The goal was to investigate and bring the service back online.

---

## ⚠️ Problem Observed

When checking the service status:

```bash
sudo systemctl status mariadb
```

It showed:
```text
MariaDB server is down
Can't create/write to file '/run/mariadb/mariadb.pid' (Errcode: 13 "Permission denied")
```

---

## 🧠 Root Cause (What I Learned)

- The MariaDB process was **unable to create its PID file** because it couldn't write to the directory `/run/mariadb/`
- This directory lives in **volatile memory** and is deleted after a system reboot
- If not recreated on boot, MariaDB will **fail to start**

---

## 🛠️ Solution Steps

### Step 1: Recreate the `/run/mariadb/` directory

```bash
sudo mkdir -p /run/mariadb
```

### Step 2: Set the correct ownership so MariaDB can access it

```bash
sudo chown mysql:mysql /run/mariadb
```

### Step 3: Start the service again

```bash
sudo systemctl start mariadb
```

### Step 4: Enable the service at boot (optional)

```bash
sudo systemctl enable mariadb
```

---

## ✅ Final Verification

Check service status again:

```bash
sudo systemctl status mariadb
```

Expected output:

```text
Active: active (running)
```

---

## 🧠 What I Learned

| Concept                          | Explanation                                                                 |
|----------------------------------|-----------------------------------------------------------------------------|
| PID file                         | A file that stores the process ID (PID) of a running service               |
| `/run` directory                 | Temporary filesystem (tmpfs) that resets on each reboot                   |
| `chown`                          | Linux command to change file/folder ownership                             |
| `systemctl`                      | Tool to manage system services (start, stop, status, etc.)                |
| Importance of SELinux/permissions| MariaDB must have permission to write where it expects (e.g., `/run`)     |

---

## 💡 Pro Tip

To automate this fix after every reboot, you can create a systemd override or custom script in `/etc/tmpfiles.d/mariadb.conf`:

```bash
# /etc/tmpfiles.d/mariadb.conf
d /run/mariadb 0755 mysql mysql
```

This ensures `/run/mariadb` is created **automatically on every boot**.

---
