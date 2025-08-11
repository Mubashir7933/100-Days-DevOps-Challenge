# Host Two Static Sites on Apache (Port 8084)

## Objective
Prepare **App Server 2** to serve two static websites:
- `http://localhost:8084/ecommerce/`
- `http://localhost:8084/games/`

Source folders are on **jump_host** at:
- `/home/thor/ecommerce`
- `/home/thor/games`

---

## Overview
**Technology:** Apache HTTP Server (`httpd`) — an open-source web server that listens for HTTP requests and serves files or responses.  
**Why:** Apache is stable, widely used, and supports hosting multiple websites on one server using aliases or virtual hosts.  
**How:** Install Apache, change its listening port to **8084**, copy website files into `/var/www/html`, configure URL-to-folder mapping with `Alias`, start Apache, and verify via `curl`.

---

## Full Command Sequence
```bash
# Step 1: Connect to App Server 2 and install Apache
ssh steve@stapp02
sudo yum install -y httpd

# Step 2: Configure Apache to listen on port 8084
sudo sed -i 's/^Listen 80$/Listen 8084/' /etc/httpd/conf/httpd.conf

# Step 3: On jump_host, copy website directories to App Server 2 home directory
scp -r /home/thor/ecommerce steve@stapp02:~
scp -r /home/thor/games     steve@stapp02:~

# Step 4: On App Server 2, move directories into Apache web root and set permissions
sudo mkdir -p /var/www/html
sudo mv ~/ecommerce /var/www/html/
sudo mv ~/games     /var/www/html/
sudo chown -R apache:apache /var/www/html/ecommerce /var/www/html/games || true
sudo restorecon -R /var/www/html || true

# Step 5: Configure Apache aliases for both sites
sudo tee /etc/httpd/conf.d/multisite.conf >/dev/null <<'EOF'
Alias /ecommerce /var/www/html/ecommerce
<Directory /var/www/html/ecommerce>
    AllowOverride None
    Require all granted
</Directory>

Alias /games /var/www/html/games
<Directory /var/www/html/games>
    AllowOverride None
    Require all granted
</Directory>
EOF

# Step 6: Start and enable Apache
sudo systemctl start httpd
sudo systemctl enable httpd

# Step 7: Verify using curl
curl http://localhost:8084/ecommerce/
curl http://localhost:8084/games/
