# Apache Service Fix & Port Configuration (Stratos DC)

## 📝 Task Overview
The Apache service on one of the application servers was reported as **unavailable** by the monitoring system.  
Our goals were to:
1. Identify the faulty app host.
2. Ensure Apache service is **up and running** on all app servers.
3. Configure Apache to run on **port 8084**.

---

## 🛠️ Steps Performed

### 1. Connect to each App Server
```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```
