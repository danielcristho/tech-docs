---
sidebar_positon: 2
date: 2023-02-11
comments: true
tags:
    - database
    - linux
---

# BASH Script to backup and restore MySQL database

## 1. Create a script file for backup

Make new .sh file on **root** directory called **mysql_backup.sh**:

```bash
#!/bin/bash
# This script will backup the database
# and store in a specified directory.

# Constants

# Database credentials
USER="dev"
PASS="mypass123!"
DB_NAME="mydb"
HOST="localhost"
BACKUP_DIRECTORY="/root/backup_db"

# Add time stamp ( formated YYYYMMDD + HHMMSS)
# uses for the file name
CURRENT_DATA=$(date "+%Y%m%d-%H%M%S")

# Run mysqldump command
# we will backup the database into a .gz file using gzip
mysqldump -h ${HOST} \
-u ${USER} \
-p${PASS} \
${DB_NAME} | gzip - > $BACKUP_DIRECTORY/$DB_NAME\_$CURRENT_DATE.sql.gz
```

## 2. Change file permission

```bash
chmod 700 /root/mysql_backup.sh
or
chmod +x /root/mysql_backup.sh
```

## 3. Add new cron using crontab

To open crontab, just type ``crontab -e`` on terminal

```bash
crontab -e
```

add this command at the end of the file so your database will be backup up every 30 minutes.

```bash
*/30 * * * * bash /root/mysql_backup.sh > /dev/null 2>&1
```

## 4. Restore database

You can restore the .gz using gunzip into the current database.

```bash
gunzip -c ...sql.gz | mysql -h localhost -U DB_NAME -u USER -p
```

**Thank you for reading this post. Happy learn.**