## 🧠 Objective

Install **Ansible version 4.8.0** on the **Jump Host** using `pip3` and ensure that the `ansible` command is accessible to **all users system-wide** (i.e., globally).

This setup is required to prepare the Jump Host as an **Ansible Control Node**, allowing automation tasks to be executed across all App Servers in the Stratos Datacenter.

---

## 🧰 Tools & Concepts Used

| Tool/Concept        | Description                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| `pip3`              | Python package manager used to install specific versions of Ansible         |
| `ansible==4.8.0`    | Required version of Ansible for consistency across the team                 |
| `--prefix=/usr/local` | Installs Ansible globally so all users can access it                      |
| `/etc/profile.d/`   | Directory to persist environment changes for all users                      |

---

## 🛠️ Step-by-Step Installation

### 1. Install `pip3` (if not already installed)

```bash
sudo yum install -y python3-pip
```

---

### 2. Install Ansible 4.8.0 Globally Using `pip3`

```bash
sudo pip3 install ansible==4.8.0 --prefix=/usr/local
```

> This installs Ansible under `/usr/local/`, making it available system-wide.

---

### 3. Ensure `/usr/local/bin` is in System PATH

If Ansible is not recognized after installation, add `/usr/local/bin` to the global `PATH`:

```bash
echo 'export PATH=$PATH:/usr/local/bin' | sudo tee /etc/profile.d/ansible.sh
sudo chmod +x /etc/profile.d/ansible.sh
source /etc/profile
```

---

### 4. Verify Ansible Installation

```bash
ansible --version
```

Expected output:

```
ansible [core 2.11.x]
  ...
  ansible [collection version 4.8.0]
```

---

## ✅ Outcome

- Ansible 4.8.0 is installed and accessible by all users on the Jump Host.
- The system is now ready to be used as an **Ansible Controller Node**.
- Future automation tasks (e.g., playbooks, inventory targeting App Servers) can be executed from here without any version issues.

---

## 📌 Notes

- Installing Ansible via `pip3` gives you more control over the version than OS package managers.
- The `--prefix=/usr/local` ensures global access instead of a local user install.
- Always verify the final binary location with `which ansible` and adjust the `PATH` if necessary.
