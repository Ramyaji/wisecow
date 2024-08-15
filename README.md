# Cow wisdom web server

## Prerequisites

```
sudo apt install fortune-mod cowsay -y
```

## How to use?

1. Run `./wisecow.sh`
2. Point the browser to server port (default 4499)

## What to expect?
![wisecow](https://github.com/nyrahul/wisecow/assets/9133227/8d6bfde3-4a5a-480e-8d55-3fef60300d98)

# Problem Statement
Deploy the wisecow application as a k8s app

## Requirement
1. Create Dockerfile for the image and corresponding k8s manifest to deploy in k8s env. The wisecow service should be exposed as k8s service.
2. Github action for creating new image when changes are made to this repo
3. [Challenge goal]: Enable secure TLS communication for the wisecow app.

## Expected Artifacts
1. Github repo containing the app with corresponding dockerfile, k8s manifest, any other artifacts needed.
2. Github repo with corresponding github action.
3. Github repo should be kept private and the access should be enabled for following github IDs: nyrahul, SujithKasireddy

   Regards
yataramya@gmail.com 

1.Ramyaji/wisecow (github.com)
2a. System Health Monitoring Script
This script monitors CPU usage, memory usage, disk space, and running processes. If any metric exceeds predefined thresholds, it logs an alert.

#!/bin/bash

# Configuration
CPU_THRESHOLD=80
MEMORY_THRESHOLD=80
DISK_THRESHOLD=90
LOG_FILE="/var/log/system_health.log"

# Function to check CPU usage
check_cpu() {
    cpu_usage=$(mpstat 1 1 | awk '/Average:/ {print 100 - $12}')
    if (( $(echo "$cpu_usage > $CPU_THRESHOLD" | bc -l) )); then
        echo "$(date) - HIGH CPU USAGE: ${cpu_usage}%" >> $LOG_FILE
    fi
}

# Function to check memory usage
check_memory() {
    memory_usage=$(free | awk '/Mem:/ {print $3/$2 * 100.0}')
    if (( $(echo "$memory_usage > $MEMORY_THRESHOLD" | bc -l) )); then
        echo "$(date) - HIGH MEMORY USAGE: ${memory_usage}%" >> $LOG_FILE
    fi
}

# Function to check disk space usage
check_disk() {
    disk_usage=$(df / | awk '/\// {print $5}' | sed 's/%//')
    if (( disk_usage > DISK_THRESHOLD )); then
        echo "$(date) - HIGH DISK USAGE: ${disk_usage}%" >> $LOG_FILE
    fi
}

# Function to check running processes
check_processes() {
    # Customize this function to check for specific processes or thresholds
    process_count=$(ps aux | wc -l)
    if (( process_count > 200 )); then
        echo "$(date) - HIGH NUMBER OF PROCESSES: ${process_count}" >> $LOG_FILE
    fi
}

# Run checks
check_cpu
check_memory
check_disk
check_processes

2b. Automated Backup Solution
This script backs up a specified directory to a remote server and logs the result.

#!/bin/bash

# Configuration
SOURCE_DIR="/path/to/source"
REMOTE_USER="user"
REMOTE_SERVER="server_address"
REMOTE_DIR="/path/to/remote/backup"
LOG_FILE="/var/log/backup.log"

# Backup command
rsync -avz $SOURCE_DIR $REMOTE_USER@$REMOTE_SERVER:$REMOTE_DIR > $LOG_FILE 2>&1

# Check if rsync was successful
if [ $? -eq 0 ]; then
    echo "$(date) - Backup succeeded" >> $LOG_FILE
else
    echo "$(date) - Backup failed" >> $LOG_FILE
fi

2c. Log File Analyzer
This script analyzes web server logs for common patterns such as 404 errors, most requested pages, and IP addresses with the most requests.

#!/bin/bash

# Configuration
LOG_FILE="/path/to/webserver.log"
REPORT_FILE="/var/log/log_report.txt"

# Analyze 404 errors
echo "404 Errors:" > $REPORT_FILE
grep "404" $LOG_FILE | wc -l >> $REPORT_FILE

# Most requested pages
echo -e "\nMost Requested Pages:" >> $REPORT_FILE
awk '{print $7}' $LOG_FILE | sort | uniq -c | sort -nr | head -n 10 >> $REPORT_FILE

# IP addresses with the most requests
echo -e "\nTop IP Addresses:" >> $REPORT_FILE
awk '{print $1}' $LOG_FILE | sort | uniq -c | sort -nr | head -n 10 >> $REPORT_FILE

2d. Application Health Checker
This script checks the uptime of an application by assessing HTTP status codes.

#!/bin/bash

# Configuration
URL="http://your_application_url"
STATUS_CODE_OK=200
LOG_FILE="/var/log/application_health.log"

# Check the status code
status_code=$(curl -o /dev/null -s -w "%{http_code}" $URL)

if [ "$status_code" -eq "$STATUS_CODE_OK" ]; then
    echo "$(date) - APPLICATION UP (Status Code: $status_code)" >> $LOG_FILE
else
    echo "$(date) - APPLICATION DOWN (Status Code: $status_code)" >> $LOG_FILE
fi



