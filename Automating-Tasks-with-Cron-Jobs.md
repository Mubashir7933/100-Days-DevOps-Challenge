Task: Install the cronie package and configure a test cron job for the root user that runs every 5 minutes and appends output to /tmp/cron_text.

🧠 What I Learned
How to install and enable the cron daemon on RHEL-based systems using cronie

The structure of a cron expression (*/5 * * * *)

How to write a one-liner cron job to perform a simple recurring task

How cron stores user-specific jobs under /var/spool/cron/

How to use tee with sudo to safely write cron jobs as root

⚙️ Tools & Concepts Used
Tool/Concept	Description
cronie	Package that provides the crond service for scheduling
crond	The background daemon that executes scheduled cron jobs
cron	Linux's built-in task scheduler
tee	Used with sudo to append output to protected files
/var/spool/cron/root	Location where cron jobs for root are stored
/tmp/cron_text	Temporary file used to verify output of cron execution

🛠️ Step-by-Step Automation with Bash Script
To streamline this task, I wrote a single bash script that:

Installs cronie

Starts and enables the crond service

Adds a root cron job to echo "hello" every 5 minutes into /tmp/cron_text

```bash
#!/bin/bash

# Setup: Install cronie and configure test cron job for root

echo "🔧 Installing cronie package..."
sudo yum install -y cronie

echo "🚀 Starting and enabling crond service..."
sudo systemctl start crond
sudo systemctl enable crond

echo "🕒 Adding cron job for root to run every 5 minutes..."
echo "*/5 * * * * echo hello > /tmp/cron_text" | sudo tee -a /var/spool/cron/root > /dev/null

echo "✅ Cron job added successfully!"
echo "⏳ Wait 5 minutes and then run: cat /tmp/cron_text"
```
