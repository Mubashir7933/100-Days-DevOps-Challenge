🧠 What I Learned
How Linux handles file permissions (r, w, x)

The difference between symbolic (+x) and numeric (chmod 755) permission modes

Why a script needs execute permissions to be runnable

How to safely make a script executable for all users

🔍 Problem
The script xfusioncorp.sh was copied to /tmp but was missing executable permissions.
Initial permissions were:
```bash
---------- 1 root root 40 Aug 6 18:12 /tmp/xfusioncorp.sh
```
🛠️ Solution
```bash
sudo chmod 755 /tmp/xfusioncorp.sh
```
