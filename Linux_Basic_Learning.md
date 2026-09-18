# Linux Learning — Basic

> Beginner-to-intermediate Linux notes focused on strong fundamentals and practical command-line skills.

## 1. Linux Fundamentals

### What is Linux?
Linux is an open-source operating system kernel. Linux distributions combine the kernel with system utilities, libraries, package managers, and applications.

Common distributions:
- Ubuntu / Linux Mint — beginner friendly
- Fedora — modern Linux ecosystem
- Rocky Linux / RHEL — enterprise/server focused
- Arch Linux — advanced learning

### Kernel vs Shell vs Distribution

```text
User
  ↓
Applications
  ↓
Shell (bash/zsh)
  ↓
Linux Kernel
  ↓
Hardware
```

- **Kernel:** Manages CPU, memory, devices, processes, networking, and system calls.
- **Shell:** Command interpreter used to interact with the system.
- **Distribution:** Complete OS built around the Linux kernel.

---

## 2. Getting a Linux Practice Environment

Recommended options:
- WSL on Windows
- Virtual machine
- Cloud Linux VM
- Native Linux installation

For Windows:

```powershell
wsl --install
```

Verify Linux:

```bash
cat /etc/os-release
uname -a
```

---

## 3. Terminal and Basic Commands

### `pwd`

Shows the current directory.

```bash
pwd
```

### `ls`

Lists files and directories.

```bash
ls
ls -l
ls -la
ls -lh
ls -lt
```

Useful options:
- `-l` detailed listing
- `-a` include hidden files
- `-h` human-readable sizes
- `-t` sort by modification time

### `cd`

Change directory.

```bash
cd /etc
cd ..
cd ~
cd -
```

### `clear`

```bash
clear
```

### `history`

```bash
history
```

---

## 4. Linux Directory Structure

Important directories:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

| Directory | Purpose |
|---|---|
| `/` | Root of filesystem |
| `/home` | Normal users' home directories |
| `/root` | Root user's home |
| `/etc` | System/application configuration |
| `/var` | Variable data, logs, caches |
| `/tmp` | Temporary files |
| `/usr` | User-space programs and libraries |
| `/bin` | Essential commands |
| `/sbin` | System administration commands |
| `/dev` | Device files |
| `/proc` | Process/kernel information |
| `/sys` | Kernel/device information |
| `/boot` | Boot-related files |
| `/opt` | Optional software |

---

## 5. Files and Directories

### Create

```bash
touch file.txt
mkdir mydir
mkdir -p project/src/app
```

### Copy

```bash
cp file.txt backup.txt
cp -r mydir mydir_backup
```

### Move / Rename

```bash
mv file.txt newfile.txt
mv newfile.txt mydir/
```

### Delete

```bash
rm file.txt
rm -r mydir
```

Be careful with:

```bash
rm -rf
```

It can recursively delete files without confirmation.

### File type

```bash
file filename
```

---

## 6. Viewing File Contents

```bash
cat file.txt
less file.txt
more file.txt
head file.txt
head -n 20 file.txt
tail file.txt
tail -n 20 file.txt
tail -f application.log
```

`tail -f` is especially useful for watching logs.

---

## 7. Creating and Editing Text

```bash
echo "Hello Linux" > file.txt
echo "Another line" >> file.txt
```

Difference:

```text
>   overwrite
>>  append
```

Common editors:

```bash
nano file.txt
vim file.txt
```

---

## 8. Absolute vs Relative Paths

Absolute path starts from `/`:

```bash
/etc/ssh/sshd_config
```

Relative path starts from the current directory:

```bash
./config/app.conf
```

Special paths:

```text
.    current directory
..   parent directory
~    home directory
/    filesystem root
```

---

## 9. Wildcards

```bash
ls *.log
ls file?.txt
ls [abc].txt
```

Common patterns:

- `*` — any number of characters
- `?` — exactly one character
- `[abc]` — one character from the set

---

## 10. Searching for Files

### `find`

```bash
find /tmp -name "*.log"
find . -type f
find . -type d
find . -type f -size +100M
```

### `locate`

```bash
locate nginx.conf
```

`locate` is fast but depends on its database being updated.

---

## 11. Users and Groups

Check current user:

```bash
whoami
id
```

See logged-in users:

```bash
who
w
```

User information:

```bash
cat /etc/passwd
```

Group information:

```bash
cat /etc/group
```

Create a user:

```bash
sudo useradd -m john
sudo passwd john
```

Create a group:

```bash
sudo groupadd developers
```

Add user to group:

```bash
sudo usermod -aG developers john
```

---

## 12. File Ownership

Check ownership:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 john developers 1200 file.txt
```

Change owner:

```bash
sudo chown john file.txt
```

Change group:

```bash
sudo chgrp developers file.txt
```

Change both:

```bash
sudo chown john:developers file.txt
```

---

## 13. Linux Permissions

Three permission groups:

```text
user     group     others
rwx      rwx       rwx
```

Permissions:

```text
r = read
w = write
x = execute
```

Numeric values:

```text
r = 4
w = 2
x = 1
```

Examples:

```bash
chmod 755 script.sh
chmod 644 file.txt
chmod 700 private.sh
```

### Symbolic permissions

```bash
chmod u+x script.sh
chmod g+w file.txt
chmod o-r file.txt
```

---

## 14. `sudo` and Root

Run a command with elevated privileges:

```bash
sudo command
```

Switch to root:

```bash
sudo -i
```

Check current identity:

```bash
whoami
```

Avoid staying logged in as root unnecessarily.

---

## 15. Processes

List processes:

```bash
ps
ps aux
ps -ef
```

Interactive monitoring:

```bash
top
```

If installed:

```bash
htop
```

Find a process:

```bash
pgrep nginx
```

Terminate:

```bash
kill PID
kill -9 PID
```

Prefer normal `kill` first; `kill -9` should generally be a last resort.

---

## 16. Jobs and Background Processes

Run in background:

```bash
command &
```

List jobs:

```bash
jobs
```

Bring job to foreground:

```bash
fg
```

Suspend a foreground process:

```text
Ctrl+Z
```

Continue in background:

```bash
bg
```

---

## 17. Pipes

A pipe sends the output of one command to another.

```bash
ps aux | grep nginx
```

Examples:

```bash
ls -l | less
cat /etc/passwd | grep bash
```

Think:

```text
command1 → output → command2
```

---

## 18. Redirection

Standard output:

```bash
command > output.txt
```

Append:

```bash
command >> output.txt
```

Standard input:

```bash
command < input.txt
```

Standard error:

```bash
command 2> error.log
```

Both output and error:

```bash
command > output.log 2>&1
```

---

## 19. Text Processing Basics

### `grep`

```bash
grep "error" app.log
grep -i "error" app.log
grep -r "TODO" .
grep -n "error" app.log
```

### `sort`

```bash
sort names.txt
sort -n numbers.txt
```

### `uniq`

```bash
sort names.txt | uniq
sort names.txt | uniq -c
```

### `wc`

```bash
wc file.txt
wc -l file.txt
wc -w file.txt
```

### `cut`

```bash
cut -d: -f1 /etc/passwd
```

---

## 20. Environment Variables

View variables:

```bash
env
printenv
```

View one variable:

```bash
echo $PATH
```

Set temporarily:

```bash
export APP_ENV=dev
```

Remove:

```bash
unset APP_ENV
```

Important variables:

```text
PATH
HOME
USER
SHELL
PWD
LANG
```

---

## 21. Package Management

Ubuntu/Debian:

```bash
sudo apt update
sudo apt upgrade
sudo apt install nginx
sudo apt remove nginx
```

Fedora/RHEL-family:

```bash
sudo dnf install nginx
sudo dnf update
sudo dnf remove nginx
```

---

## 22. Archives

Create tar archive:

```bash
tar -cvf backup.tar mydir/
```

Extract:

```bash
tar -xvf backup.tar
```

Gzip:

```bash
gzip file.txt
gunzip file.txt.gz
```

Tar + gzip:

```bash
tar -czvf backup.tar.gz mydir/
tar -xzvf backup.tar.gz
```

---

## 23. Disk Usage

Filesystem usage:

```bash
df -h
```

Directory usage:

```bash
du -sh .
du -sh *
```

Block devices:

```bash
lsblk
```

---

## 24. Networking Basics

Show IP information:

```bash
ip addr
ip link
```

Routes:

```bash
ip route
```

Connectivity:

```bash
ping 8.8.8.8
```

DNS lookup:

```bash
nslookup example.com
dig example.com
```

Check listening ports:

```bash
ss -tuln
```

---

## 25. SSH

Connect to a remote machine:

```bash
ssh user@server-ip
```

Specify a key:

```bash
ssh -i ~/.ssh/id_rsa user@server-ip
```

Copy a file:

```bash
scp file.txt user@server:/tmp/
```

---

## 26. Services

On systemd systems:

```bash
systemctl status nginx
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl enable nginx
```

Logs:

```bash
journalctl -u nginx
journalctl -u nginx -f
```

---

## 27. Cron Basics

Edit current user's cron:

```bash
crontab -e
```

Example:

```cron
0 2 * * * /home/user/backup.sh
```

Meaning:

```text
minute hour day month weekday
```

---

## 28. Shell Scripting Basics

Create:

```bash
nano hello.sh
```

Script:

```bash
#!/bin/bash

name="Linux"

echo "Hello $name"
```

Make executable:

```bash
chmod +x hello.sh
```

Run:

```bash
./hello.sh
```

### Condition

```bash
if [ "$name" = "Linux" ]; then
    echo "Correct"
else
    echo "Wrong"
fi
```

### Loop

```bash
for file in *.log; do
    echo "$file"
done
```

---

## 29. Basic Troubleshooting Workflow

When something fails:

```text
1. Understand the error
2. Check command syntax
3. Check permissions
4. Check process/service status
5. Check ports
6. Check disk/memory/CPU
7. Check logs
8. Test connectivity
9. Make one change at a time
10. Verify the result
```

Useful commands:

```bash
systemctl status service
journalctl -xe
df -h
free -h
top
ps aux
ss -tuln
ip addr
```

---

## 30. Hands-On Practice

### Lab 1 — Files

```bash
mkdir -p ~/linux-lab/{logs,backup,scripts}
cd ~/linux-lab
touch logs/app.log logs/error.log
echo "Application started" > logs/app.log
echo "ERROR: connection failed" >> logs/error.log
```

Tasks:
1. List all files.
2. Display the logs.
3. Find all `.log` files.
4. Search for `ERROR`.
5. Copy the logs into `backup`.

### Lab 2 — Permissions

```bash
touch secret.txt
chmod 600 secret.txt
ls -l secret.txt
```

Tasks:
1. Explain `600`.
2. Change it to `644`.
3. Change it to `755`.
4. Explain why `755` is normally unsuitable for private files.

### Lab 3 — Processes

```bash
sleep 300 &
ps aux | grep sleep
```

Tasks:
1. Find its PID.
2. Terminate it.
3. Verify that it stopped.

### Lab 4 — Networking

```bash
ip addr
ip route
ss -tuln
```

Tasks:
1. Identify the machine's IP.
2. Identify the default route.
3. List listening ports.

---

## Basic Linux Interview Checklist

You should be able to explain:

- Linux kernel vs distribution
- Absolute vs relative path
- `/etc`, `/var`, `/home`, `/tmp`, `/proc`
- `ls`, `cd`, `pwd`, `cp`, `mv`, `rm`
- `cat`, `less`, `head`, `tail`
- `grep`, `find`
- Pipes and redirection
- Users and groups
- `chmod`, `chown`
- `sudo`
- Processes and PIDs
- `kill`
- Environment variables
- Package managers
- `tar`
- `df` vs `du`
- Basic IP/network commands
- SSH/SCP
- systemd basics
- cron
- Bash scripting fundamentals

---

## Progression

Do not move to advanced Linux until you can perform the above tasks without constantly looking up basic command syntax.

**Next:** Advanced Linux → system administration, networking, storage, security, performance, troubleshooting, automation, containers, and DevOps integration.
