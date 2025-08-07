## 🧠 Objective

Set up SSH key-based (password-less) authentication from the `thor` user on the **jump host** to the following users on the respective App Servers in the Stratos Datacenter:

- `tony@stapp01`
- `steve@stapp02`
- `banner@stapp03`

This setup is required to enable automated scripts and secure remote task execution without requiring interactive passwords.

---

## 🧰 Tools & Concepts

- `ssh-keygen` – Generates SSH key pairs
- `ssh-copy-id` – Copies the public key to the remote server
- `~/.ssh/id_rsa` – Private key (keep secret)
- `~/.ssh/id_rsa.pub` – Public key (shared with servers)
- `/home/username/.ssh/authorized_keys` – Stores accepted public keys on each app server

---

## 🛠️ Step-by-Step Instructions

### 1. Switch to `thor` user on the jump host

```bash
ssh thor@jump_host
```

---

### 2. Generate SSH key pair (if not already created)

```bash
ssh-keygen -t rsa
```

- Press `Enter` to accept the default location and no passphrase.
- This creates the following key pair:
  - `~/.ssh/id_rsa` (private)
  - `~/.ssh/id_rsa.pub` (public)

---

### 3. Copy public key to each app server

```bash
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03
```

- You will be asked for each user’s password **once**
- The public key will be added to each user’s `~/.ssh/authorized_keys`

---

### 4. Test password-less access

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

- You should now be able to log in **without entering a password**

---

## ✅ Outcome

- `thor` can now SSH into all app servers as their respective sudo users without a password prompt.
- This setup is required for automation tools like:
  - Bash scripts
  - Cron jobs
  - Ansible
  - CI/CD pipelines

---

## 📌 Notes

- Never share your **private key (`id_rsa`)**
- The **public key** can be reused across multiple servers
- Always set correct file permissions:
  ```bash
  chmod 700 ~/.ssh
  chmod 600 ~/.ssh/authorized_keys
  ```
