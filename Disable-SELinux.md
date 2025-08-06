# 🔐 SELinux Setup & Temporary Disabling – App Server 3 (Stratos Datacenter)

## 🧠 Context

Following a security audit at xFusionCorp Industries, we are preparing to enforce SELinux for enhanced security across production servers.

As an initial step, we’re preparing **App Server 3** by:

- Installing necessary SELinux packages
- Temporarily disabling SELinux (to be re-enabled after future configuration)

---

## 🎯 Task Summary

| Item | Action |
|------|--------|
| Server | App Server 3 |
| User | tony |
| Package Manager | `yum` (CentOS/RHEL based) |
| SELinux Mode | Permanently set to `disabled` |
| Reboot | Skipped (handled later by maintenance team) |

---

## 🛠️ Steps Performed
 SELinux 

```bash
sudo yum install -y selinux-policy selinux-policy-targeted
/etc/selinux/config
grep SELINUX= /etc/selinux/config
# Output:
SELINUX=disabled
getenforce
# Output: Enforcing
```
