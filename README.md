# Assignment-1

Database Automation Assignment

This repository contains two Python scripts to automate common MySQL database operations:

backup_script.py: Automates creating timestamped backups of a specified MySQL database using mysqldump.

deploy_changes_script.py: Automates deploying schema changes (e.g., creating tables, adding columns) via Python’s MySQL connector.

🚀 Features

Automated Backups: Create unique, timestamped .sql dumps to avoid overwriting previous backups.

Schema Deployment: Apply a series of SQL DDL statements in a repeatable, idempotent manner.

Easy Configuration: Centralized settings for host, credentials, and database details.

Graceful Error Reporting: Prints clear success/failure messages for both scripts.

🔧 Prerequisites

Python 3.6+ installed

MySQL server accessible via network

mysqldump CLI tool (for backup_script.py)

Python packages:

pip install mysql-connector-python

⚙️ Installation

Install Python dependencies:

pip install -r requirements.txt

🔌 Configuration

Edit the top of each script to set your own values:

DB_HOST     = 'localhost'        # MySQL server address
DB_USER     = 'your_username'    # Username with necessary privileges
DB_PASSWORD = 'your_password'    # Password for the user
DB_NAME     = 'your_database'    # Database to operate on
BACKUP_DIR  = '/path/to/backup_dir'  # Directory to store backup files

In deploy_changes_script.py, configure your SQL changes in the SQL_CHANGES list.

▶️ Usage

Backup Script

python backup_script.py

This creates a file named <DB_NAME>_backup_YYYYMMDD_HHMMSS.sql in BACKUP_DIR.

Deployment Script

python deploy_changes_script.py

Iterates through each SQL statement in SQL_CHANGES, applies it, and commits the transaction.

📖 Script Logic Explained

Backup Script Logic

Ensure Directory: Creates the backup folder if missing (os.makedirs).

Generate Filename: Uses datetime.now().strftime('%Y%m%d_%H%M%S') to produce a unique timestamp.

Run Dump: Executes mysqldump via subprocess.run, redirecting output to the timestamped file.

Verify: Checks returncode to confirm success or prints error details.

Deployment Script Functionality

Connect: Opens a MySQL connection via mysql.connector.connect(**DB_CONFIG).

Execute: Loops through SQL_CHANGES, running each DDL statement in order.

Commit: After all statements, calls conn.commit() to persist changes.

Cleanup: Closes cursor and connection in a finally block to avoid resource leaks.

🛑 Error Handling

Backup Failures: Non-zero exit code prints stderr from mysqldump.

Deployment Errors: Catches mysql.connector.Error, logs warnings (e.g., TABLE EXISTS), and reports other exceptions.
