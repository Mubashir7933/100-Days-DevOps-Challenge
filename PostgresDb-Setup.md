# PostgreSQL User & Database Setup – Nautilus Infra

This document explains and demonstrates how to set up a PostgreSQL database and user for a new application deployment on the Nautilus infrastructure in **Stratos DC**.

---

## **1. Technology Overview**
**PostgreSQL** is an open-source relational database management system (RDBMS) that uses SQL to store, retrieve, and manage structured data.  
It is chosen for:
- **Data integrity** via ACID compliance.
- **Security** through role-based access control.
- **Scalability** for small to enterprise applications.

---

## **2. Why This Task?**
The Nautilus application team requires:
- A dedicated **database** for the new app.
- A dedicated **user** to connect securely to that database.
This ensures separation of permissions and easier management.

---

## **3. How It Works**
1. **SSH into the DB server** where PostgreSQL is installed.
2. **Connect to PostgreSQL** as the `postgres` superuser.
3. **Create a new user** for the application.
4. **Create a new database** owned by that user.
5. **Grant privileges** to ensure the app user has full control.

---

## **4. Commands – Step-by-Step**

```bash
# 1. SSH into the database server
ssh <your_username>@<db_server_hostname>

# 2. Access PostgreSQL shell as postgres superuser
sudo -u postgres psql

# 3. Inside PostgreSQL, create the user with password
CREATE USER kodekloud_roy WITH PASSWORD 'dCV3szSGNA';

# 4. Create the database and assign ownership to the new user
CREATE DATABASE kodekloud_db2 OWNER kodekloud_roy;

# 5. Grant all privileges on the database to the user
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db2 TO kodekloud_roy;

# 6. Exit PostgreSQL
\q
