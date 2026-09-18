
```markdown
# 🚀 Advanced Shell Scripting for DevOps
```

## 1. Pipes & Redirection
- **Pipe (`|`)** → send output of one command into another
  ```bash
  ps aux | grep nginx
  ```
- **Redirect stdout**: `>` (overwrite), `>>` (append)
- **Redirect stderr**: `2>`  
- **Redirect both**: `&>`  
- **Discard output**: `/dev/null`

### ASCII Flow
```
[Command A] ---> stdout ---> [Command B]
```

---

## 2. Here Documents & Here Strings
- **Here Document**:
  ```bash
  cat <<EOF
  Line1
  Line2
  EOF
  ```
- **Here String**:
  ```bash
  grep "error" <<< "this is an error line"
  ```

---

## 3. Signal Handling with `trap`
```bash
trap "echo 'Interrupted!'; exit" SIGINT SIGTERM
```
- Useful for **graceful shutdowns** in automation scripts.

---

## 4. Subshells & Grouping
- Run commands in isolated subshell:
  ```bash
  (cd /tmp && ls)
  ```
- Group commands:
  ```bash
  { echo "Start"; date; }
  ```

---

## 5. Exporting & Environment Variables
```bash
export PATH=$PATH:/opt/tools
```
- Makes variables available to child processes (important in CI/CD pipelines).

---

## 6. Error Handling Patterns
- **Short-circuit execution**:
  ```bash
  mkdir /data && echo "Created" || echo "Failed"
  ```
- **Exit on error**:
  ```bash
  set -e
  ```

---

## 7. Logging & Debugging
- **Enable debugging**:
  ```bash
  set -x
  ```
- **Log messages**:
  ```bash
  logger "Deployment started"
  ```

---

## 8. File & Directory Automation
- **Batch rename**:
  ```bash
  for f in *.log; do mv "$f" "${f%.log}.bak"; done
  ```
- **Iterate over files**:
  ```bash
  for file in /var/log/*.log; do
    echo "Processing $file"
  done
  ```

---

## 9. Monitoring Scripts (DevOps Use Cases)
### Check Disk Usage
```bash
#!/bin/bash
THRESHOLD=80
USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
if [ $USAGE -gt $THRESHOLD ]; then
  echo "Disk usage critical: $USAGE%" | mail -s "Disk Alert" admin@example.com
fi
```

### Check Memory Usage
```bash
free -m | awk 'NR==2{printf "Memory Usage: %.2f%%\n", $3*100/$2 }'
```

---

## 10. CI/CD Helpers
### Git Automation
```bash
#!/bin/bash
git add .
git commit -m "Automated commit"
git push origin main
```

### Deployment Script
```bash
#!/bin/bash
ssh user@server "cd /app && git pull && systemctl restart app"
```

---

## 11. Scheduling Jobs
- **One-time job**:
  ```bash
  echo "backup.sh" | at 02:00
  ```
- **Recurring job**:
  ```bash
  crontab -e
  0 2 * * * /home/user/backup.sh
  
```
* * * * *  command
│ │ │ │ │
│ │ │ │ └── Day of week (0–6) (Sunday=0)
│ │ │ └──── Month (1–12)
│ │ └────── Day of month (1–31)
│ └──────── Hour (0–23)
└────────── Minute (0–59)

```

---

## 12. Advanced Argument Handling
```bash
while getopts "u:p:" opt; do
  case $opt in
    u) USER=$OPTARG ;;
    p) PASS=$OPTARG ;;
    *) echo "Invalid option"; exit 1 ;;
  esac
done
```

