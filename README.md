# backup_database

#!/bin/bash

# Read user input for source database credentials
read -p "Enter source database host: " SRC_DB_HOST
read -p "Enter source database username: " SRC_DB_USER
read -s -p "Enter source database password: " SRC_DB_PASS
echo
read -p "Enter source database name: " SRC_DB_NAME

# Read user input for destination database credentials
read -p "Enter destination database host: " DEST_DB_HOST
read -p "Enter destination database username: " DEST_DB_USER
read -s -p "Enter destination database password: " DEST_DB_PASS
echo
read -p "Enter destination database name: " DEST_DB_NAME

# Backup destination directory
BACKUP_DIR="/var/www/html/backup"

# Create backup directory if it doesn't exist
mkdir -p $BACKUP_DIR

# Generate a timestamp for the backup file
TIMESTAMP=$(date +%Y%m%d%H%M%S)
BACKUP_FILE="${SRC_DB_NAME}_backup_${TIMESTAMP}.sql"

# Run mysqldump command to backup the source database
mysqldump --single-transaction -h $SRC_DB_HOST -u $SRC_DB_USER -p$SRC_DB_PASS $SRC_DB_NAME > $BACKUP_DIR/$BACKUP_FILE

# Check if the backup was successful
if [ $? -eq 0 ]; then
  echo "Backup created successfully: $BACKUP_DIR/$BACKUP_FILE"
else
  echo "Backup failed!"
  exit 1
fi
