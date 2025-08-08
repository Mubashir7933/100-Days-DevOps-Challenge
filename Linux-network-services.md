# 🛠️ Apache Troubleshooting and Port Configuration Task (Stratos DC)

This task walks through how we diagnosed and resolved a service unavailability issue where Apache (httpd) was not reachable on port `3001` from outside the server (`stapp01` in Stratos Datacenter).

---

## 🎯 Objective

Make Apache accessible from the Jump Host on port 3001:
```bash
curl http://stapp01:3001
```
## ✅ Steps Performed

```bash
# 1. SSH into App Server
ssh tony@stapp01

# 2. Install Apache
sudo yum install -y httpd
sudo systemctl start httpd

# 3. Configure Apache to Listen on Port 3001
sudo vi /etc/httpd/conf/httpd.conf
# Change:
# Listen 80
# To:
# Listen 3001

# Restart Apache
sudo systemctl restart httpd

# 4. Resolve Apache Startup Failure
# Error seen:
# Address already in use: make_sock: could not bind to 0.0.0.0:3001

# Identify the process using port 3001
sudo netstat -tulnp | grep 3001

# Kill the blocking process (e.g., sendmail)
sudo kill -9 <PID>

# Restart Apache again
sudo systemctl restart httpd

# 5. Open Port 3001 in Firewall via iptables
sudo iptables -I INPUT -p tcp --dport 3001 -j ACCEPT

# 6. Final Verification from Jump Host
curl http://stapp01:3001
# ✅ Apache is now reachable on port 3001
```

## 🧠 Key Concepts Practiced

- Systemd service management
- Apache configuration
- Port conflict resolution
- iptables firewall rules
- Log analysis
- Real-world DevOps troubleshooting

---

## 📚 Tools Used

- `systemctl`
- `netstat`
- `iptables`
- `curl`
- `vi`
