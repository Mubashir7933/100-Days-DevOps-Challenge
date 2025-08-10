# Nginx SSL Setup on App Server 1

## 🛠 Steps to Complete

### 1. Install Nginx
```bash
# For CentOS/RHEL
sudo yum install nginx -y

# For Ubuntu/Debian
sudo apt-get update && sudo apt-get install nginx -y
```
## 2. Enable and Start Nginx
```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```
## 3. Create SSL Directory and Move Certificate/Key
```bash
sudo mkdir -p /etc/nginx/ssl
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
sudo chmod 600 /etc/nginx/ssl/nautilus.key
```
## 4. Configure Nginx for SSL
Create a new SSL config file:

```bash
sudo vi /etc/nginx/conf.d/ssl.conf
```
Add the following:
```bash
server {
    listen 443 ssl;
    server_name _;
    ssl_certificate     /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;
    root /usr/share/nginx/html;
    index index.html;
}
```
## 5.Create index.html

```bash
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```
## 6. Test and Restart Nginx
```bash
sudo nginx -t
sudo systemctl restart nginx
```
## 7. Verify from Jump Host
```bash
curl -Ik https://stapp01   # or use server IP
```

📌 Notes
- The -k flag in curl is needed because the certificate is self-signed.
- /etc/nginx/ssl is used to keep SSL certs organized alongside Nginx configs.
- chmod 600 ensures only root can read the private key.
