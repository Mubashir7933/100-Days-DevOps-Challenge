# 🚀 Java Web Application Deployment using Apache Tomcat

This guide documents the process of installing and configuring Apache Tomcat on a CentOS-based system, and deploying a Java `.war` application (`ROOT.war`) accessible via a base URL on a custom port (`8084`).

---

## 📋 Task Overview

| Step | Description |
|------|-------------|
| ✅ | Install Apache Tomcat on App Server 1 |
| ✅ | Configure Tomcat to run on port `8084` |
| ✅ | Deploy `ROOT.war` file from Jump Host to Tomcat server |
| ✅ | Verify application is reachable via base URL |

---

## 🛠️ Environment Details

- **OS**: CentOS / RHEL-based
- **App Server**: `stapp01`
- **Username**: `tony`
- **WAR File**: `/tmp/ROOT.war` (on Jump Host)
- **Tomcat Port**: `8084`
- **Deployment URL**: `http://stapp01:8084`

---

## 🧩 Step-by-Step Instructions

```bash
# 🟢 1. SSH into App Server 1 (stapp01)
ssh tony@stapp01

# 🟢 2. Install Apache Tomcat
sudo yum install -y tomcat tomcat-webapps tomcat-admin-webapps

# 🟢 3. Change Default Tomcat Port to 8084
sudo vi /etc/tomcat/server.xml

# Inside vi, find this line:
# <Connector port="8080" protocol="HTTP/1.1"
# Change it to:
# <Connector port="8084" protocol="HTTP/1.1"
# Save and exit by typing: :wq

# 🟢 4. Start and Enable Tomcat
sudo systemctl start tomcat
sudo systemctl enable tomcat
