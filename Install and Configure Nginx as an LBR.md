# Nginx Load Balancer Setup for Nautilus StaticApp

This guide covers the complete steps to configure an **Nginx Load Balancer (LBR)** to distribute traffic across three Apache-based App Servers running on port **8085**.  
The requirement is to **only modify `/etc/nginx/nginx.conf`** on the LBR and **not change Apache ports** on the App Servers.

```bash
# ===== 1. Verify Apache on App Servers =====
# Run these on EACH App Server
sudo systemctl status httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo ss -lntp | egrep '(httpd|apache2)'
curl -I http://localhost:8085

# ===== 2. Install Nginx on LBR Server =====
# Run these on the LBR Server
sudo yum install -y epel-release nginx
sudo systemctl enable --now nginx
curl -I http://localhost

# ===== 3. Backup Nginx Main Config =====
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak.$(date +%F-%H%M%S)

# ===== 4. Disable extra includes to avoid conflicts =====
sudo sed -i 's|^\s*include\s\+/etc/nginx/conf.d/\*\.conf;|# &|' /etc/nginx/nginx.conf 2>/dev/null || true
sudo sed -i 's|^\s*include\s\+/etc/nginx/sites-enabled/\*;|# &|' /etc/nginx/nginx.conf 2>/dev/null || true

# ===== 5. Edit nginx.conf to add load balancer config =====
# Open file
sudo vi /etc/nginx/nginx.conf

# Inside the http { ... } block, add:
upstream app_servers {
    server 172.16.238.10:8085 max_fails=3 fail_timeout=10s;
    server 172.16.238.11:8085 max_fails=3 fail_timeout=10s;
    server 172.16.238.12:8085 max_fails=3 fail_timeout=10s;
    keepalive 32;
}

server {
    listen 80 default_server;
    server_name _;

    location / {
        proxy_pass http://app_servers;
        proxy_set_header Host              $host;
        proxy_set_header X-Real-IP         $remote_addr;
        proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_read_timeout 30s;
    }
}

# Save and exit (:wq in vi)

# ===== 6. Validate and reload Nginx =====
sudo nginx -t
sudo systemctl reload nginx

# ===== 7. Allow HTTP in firewall (if enabled) =====
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload

# ===== 8. Test each App Server directly from LBR =====
curl -I http://172.16.238.10:8085
curl -I http://172.16.238.11:8085
curl -I http://172.16.238.12:8085

# ===== 9. Test through Load Balancer =====
curl -I http://localhost

# ===== 10. (Optional) Check load distribution =====
# On each App Server in separate terminals:
# sudo tail -f /var/log/httpd/access_log
# Then from LBR:
for i in {1..12}; do curl -s http://localhost >/dev/null; done
