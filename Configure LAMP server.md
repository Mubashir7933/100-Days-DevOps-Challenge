# WordPress Prep: Apache+PHP on App Hosts, MariaDB on DB Host, LBR Test

This guide prepares three App servers to serve PHP on **port 8082**, and a DB server (MariaDB) to accept connections from the Apps. A shared storage is already mounted at **/var/www/html** across all App hosts.

## Topology
- App hosts: `stapp01`, `stapp02`, `stapp03` (serve PHP via Apache on **8082**)
- DB host: `stdb01` (MariaDB 10.x, listens on **3306**, open to App hosts)
- Shared web root: `/var/www/html` (same content across all Apps)
- LBR: “App” button (routes to the App pool on port **8082**)

---

## Stage A — App Hosts (run on **each**: stapp01/02/03)

```bash
# A1) Install Apache + PHP + MySQL driver
sudo yum clean all
sudo yum install -y httpd php php-mysqlnd php-cli

# A2) Make Apache listen on port 8082 instead of 80
sudo sed -i 's/^[[:space:]]*Listen[[:space:]]\+80$/Listen 8082/' /etc/httpd/conf/httpd.conf

# (If SELinux is enforcing on your host, allow Apache to use 8082)
# Safe to attempt; will no-op if SELinux is off
sudo yum install -y policycoreutils-python-utils || true
sudo semanage port -a -t http_port_t -p tcp 8082 2>/dev/null || sudo semanage port -m -t http_port_t -p tcp 8082

# A3) Start & enable Apache
sudo systemctl enable --now httpd
sudo systemctl status httpd --no-pager

# A4) Add a DB connectivity test page to the shared web root
cat <<'PHP' | sudo tee /var/www/html/dbtest.php > /dev/null
<?php
$host = '172.16.239.10';   // DB server IP (stdb01)
$user = 'kodekloud_sam';
$pass = 'dCV3szSGNA';
$db   = 'kodekloud_db3';

$mysqli = @new mysqli($host, $user, $pass, $db);
if ($mysqli->connect_errno) {
  http_response_code(500);
  echo "DB connection FAILED: {$mysqli->connect_error}\n";
  exit;
}
echo "App is able to connect to the database using user {$user}";
$mysqli->close();
PHP

# A5) Verify Apache listens and serve the page locally
sudo ss -lntp | grep :8082
curl -iS http://localhost:8082/dbtest.php || true
