# Linux Learning — Advanced

> Advanced Linux notes for system administration, DevOps, troubleshooting, security, networking, storage, performance, and automation.

## 1. Advanced Linux Architecture

Understand the relationship:

```text
Applications
    ↓
Libraries / Runtime
    ↓
System Calls
    ↓
Linux Kernel
    ↓
CPU / Memory / Devices
```

Important kernel responsibilities:
- Process scheduling
- Memory management
- Filesystems
- Networking
- Device management
- Security
- Inter-process communication

Useful commands:

```bash
uname -a
uname -r
lsmod
modinfo module_name
sysctl -a
```

---

## 2. Process Management Deep Dive

### Process hierarchy

```bash
ps -ef --forest
pstree
```

Every process has a PID and a parent process (PPID).

```bash
ps -o pid,ppid,user,stat,cmd
```

### Process states

Common states include:

```text
R = Running
S = Sleeping
D = Uninterruptible sleep
T = Stopped
Z = Zombie
```

### Signals

```bash
kill -TERM PID
kill -HUP PID
kill -INT PID
kill -KILL PID
```

Common meanings:

- `SIGTERM` — request graceful termination
- `SIGHUP` — commonly reload/reopen configuration
- `SIGINT` — interrupt
- `SIGKILL` — force termination; cannot be caught by the process

### Priority

```bash
nice -n 10 command
renice 10 -p PID
```

---

## 3. systemd Deep Dive

View units:

```bash
systemctl list-units
systemctl list-unit-files
```

Service lifecycle:

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl enable nginx
sudo systemctl disable nginx
```

Inspect:

```bash
systemctl status nginx
systemctl cat nginx
systemctl show nginx
```

Logs:

```bash
journalctl -u nginx
journalctl -u nginx --since "1 hour ago"
journalctl -u nginx -f
```

### Service troubleshooting

```text
1. systemctl status service
2. journalctl -u service
3. inspect configuration
4. check dependencies
5. check ports
6. check permissions
7. restart only after understanding the failure
```

---

## 4. Boot Process

High-level flow:

```text
Firmware
   ↓
Bootloader
   ↓
Linux Kernel
   ↓
initramfs
   ↓
systemd (PID 1)
   ↓
Targets / Services
   ↓
Login
```

Useful commands:

```bash
systemd-analyze
systemd-analyze blame
systemd-analyze critical-chain
```

---

## 5. Storage Architecture

Understand:

```text
Disk
 ↓
Partition
 ↓
Filesystem
 ↓
Mount point
 ↓
Application
```

Inspect:

```bash
lsblk
blkid
fdisk -l
df -hT
```

Filesystem types may include:
- ext4
- XFS
- Btrfs

---

## 6. Mounting Filesystems

Mount:

```bash
sudo mount /dev/sdb1 /mnt/data
```

Unmount:

```bash
sudo umount /mnt/data
```

Show mounts:

```bash
findmnt
mount
```

Persistent mounts are configured in:

```bash
/etc/fstab
```

Example concept:

```text
UUID=<uuid>  /data  ext4  defaults  0  2
```

Always validate carefully before rebooting after modifying `/etc/fstab`.

---

## 7. LVM

Logical Volume Management:

```text
Physical Disk
     ↓
Physical Volume (PV)
     ↓
Volume Group (VG)
     ↓
Logical Volume (LV)
     ↓
Filesystem
```

Inspect:

```bash
pvs
vgs
lvs
```

Create:

```bash
sudo pvcreate /dev/sdb
sudo vgcreate vgdata /dev/sdb
sudo lvcreate -L 10G -n lvapp vgdata
```

Create filesystem:

```bash
sudo mkfs.ext4 /dev/vgdata/lvapp
```

Extend an LV:

```bash
sudo lvextend -L +5G /dev/vgdata/lvapp
```

Then grow the filesystem according to its type.

---

## 8. Inodes and Links

Check inode usage:

```bash
df -i
```

Hard link:

```bash
ln original.txt hardlink.txt
```

Symbolic link:

```bash
ln -s /path/to/original symlink
```

Inspect:

```bash
ls -li
readlink symlink
```

Key distinction:

```text
Hard link → another directory entry for the same inode
Symlink   → file containing a path/reference to another file
```

---

## 9. Disk and I/O Troubleshooting

Useful commands:

```bash
iostat
iotop
vmstat
pidstat
```

Check disk space:

```bash
df -h
du -xh /var | sort -h
```

Check inode exhaustion:

```bash
df -i
```

Typical investigation:

```text
Disk full?
   ↓
df -h
   ↓
Filesystem full or inode full?
   ↓
du / df -i
   ↓
Find large files/directories
   ↓
Check logs, deleted-open files, temporary data
```

Deleted files still held open can be investigated with:

```bash
lsof +L1
```

---

## 10. Memory Troubleshooting

```bash
free -h
vmstat 1
top
ps aux --sort=-%mem | head
```

Important concepts:

- Total memory
- Used memory
- Available memory
- Buffers/cache
- Swap

Check swap:

```bash
swapon --show
free -h
```

Do not assume that Linux using cache means the system is out of memory.

---

## 11. CPU Troubleshooting

```bash
top
uptime
mpstat
vmstat
pidstat
```

Load average:

```bash
uptime
```

Load average is not simply "CPU percentage"; interpret it together with CPU count, runnable processes, I/O wait, and system behavior.

Per-process CPU:

```bash
ps aux --sort=-%cpu | head
```

---

## 12. Networking Architecture

Know:

```text
Application
    ↓
Socket
    ↓
TCP/UDP
    ↓
IP
    ↓
Network Interface
    ↓
Switch/Router
```

Inspect interfaces:

```bash
ip addr
ip link
```

Routes:

```bash
ip route
ip route get 8.8.8.8
```

Neighbor table:

```bash
ip neigh
```

---

## 13. TCP/UDP Troubleshooting

Listening sockets:

```bash
ss -lntup
```

Established connections:

```bash
ss -ant
```

Find a process using a port:

```bash
sudo ss -lntup
sudo lsof -i :8080
```

Useful concepts:

- Source IP/port
- Destination IP/port
- TCP handshake
- Listening socket
- Established connection
- TIME_WAIT
- Connection refusal
- Timeout

---

## 14. DNS Troubleshooting

```bash
dig example.com
dig +short example.com
nslookup example.com
```

Check resolver configuration:

```bash
cat /etc/resolv.conf
```

Useful distinction:

```text
DNS resolution failure ≠ network connectivity failure
```

Test separately:

```bash
ping 8.8.8.8
dig example.com
curl -v https://example.com
```

---

## 15. HTTP Troubleshooting

```bash
curl -I https://example.com
curl -v https://example.com
curl -sS https://example.com
```

Think through the chain:

```text
DNS
 ↓
TCP
 ↓
TLS
 ↓
HTTP
 ↓
Application
```

If a service fails, identify which layer is failing instead of restarting everything.

---

## 16. SSH Advanced

Generate a key:

```bash
ssh-keygen -t ed25519
```

Copy public key:

```bash
ssh-copy-id user@server
```

Test:

```bash
ssh -v user@server
```

SSH configuration:

```bash
~/.ssh/config
/etc/ssh/sshd_config
```

Use restrictive permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
chmod 644 ~/.ssh/*.pub
```

---

## 17. Firewall

Common Linux firewall technologies/tools include:

- nftables
- firewalld
- iptables compatibility tooling
- UFW on Ubuntu

Examples with UFW:

```bash
sudo ufw status
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
```

With firewalld:

```bash
sudo firewall-cmd --state
sudo firewall-cmd --list-all
```

Firewall troubleshooting should consider:

```text
Application listening?
        ↓
Correct IP?
        ↓
Correct port?
        ↓
Local firewall?
        ↓
Cloud/security-group firewall?
        ↓
Network ACL/routing?
```

---

## 18. SELinux

Check status:

```bash
getenforce
sestatus
```

Modes:

```text
Enforcing
Permissive
Disabled
```

Context:

```bash
ls -Z
```

Search recent denials:

```bash
ausearch -m AVC -ts recent
```

Important principle:

> Do not disable SELinux simply because an application has a permission problem. Investigate the security context and policy first.

---

## 19. File ACLs

View ACL:

```bash
getfacl file.txt
```

Set ACL:

```bash
setfacl -m u:john:rwx file.txt
```

Remove ACL:

```bash
setfacl -x u:john file.txt
```

ACLs provide permissions beyond the basic user/group/other model.

---

## 20. Logs and Journald

Common logs may exist under:

```text
/var/log/
```

Search:

```bash
grep -R "ERROR" /var/log
```

Journald:

```bash
journalctl
journalctl -b
journalctl -p err
journalctl --since today
```

Follow:

```bash
journalctl -f
```

---

## 21. Advanced Text Processing

### grep

```bash
grep -E "ERROR|WARN" app.log
grep -v "DEBUG" app.log
grep -nE "timeout|failed|error" app.log
```

### sed

Replace:

```bash
sed 's/old/new/g' file.txt
```

Edit matching lines:

```bash
sed -i 's/old/new/g' file.txt
```

### awk

Print columns:

```bash
awk '{print $1, $3}' file.txt
```

With delimiter:

```bash
awk -F: '{print $1}' /etc/passwd
```

Count values:

```bash
awk '{count[$1]++} END {for (x in count) print x, count[x]}' file.txt
```

---

## 22. Regular Expressions

Useful symbols:

```text
^   beginning
$   end
.   any character
*   zero or more
+   one or more
?   zero or one
[]  character class
()  group
|   OR
```

Example:

```bash
grep -E '^ERROR|^WARN' app.log
```

---

## 23. Bash Scripting — Advanced

### Variables

```bash
#!/bin/bash

APP="myapp"
COUNT=10

echo "$APP"
echo "$COUNT"
```

### Arguments

```bash
echo "$1"
echo "$2"
echo "$#"
echo "$@"
```

Run:

```bash
./script.sh file.txt backup
```

### Exit status

```bash
command
echo $?
```

Success normally returns `0`.

### Safer scripting

```bash
set -euo pipefail
```

Understand each option before using it blindly, especially in production scripts.

### Functions

```bash
backup_file() {
    local file="$1"
    cp "$file" "${file}.bak"
}

backup_file app.conf
```

---

## 24. Shell Error Handling

Example:

```bash
if ! systemctl is-active --quiet nginx; then
    echo "nginx is not running"
    exit 1
fi
```

Use meaningful exit codes and clear error messages.

---

## 25. Cron and Scheduling

View:

```bash
crontab -l
```

System cron locations:

```text
/etc/crontab
/etc/cron.d/
/etc/cron.daily/
/etc/cron.hourly/
```

Example:

```cron
*/10 * * * * /opt/scripts/check.sh
```

For modern Linux systems, also understand systemd timers:

```bash
systemctl list-timers
```

---

## 26. Environment and Shell Configuration

Important files:

```text
~/.bashrc
~/.bash_profile
~/.profile
/etc/profile
/etc/bash.bashrc
```

Check shell:

```bash
echo "$SHELL"
```

PATH:

```bash
echo "$PATH"
```

Find command location:

```bash
which python
type python
command -v python
```

---

## 27. Package Management — Advanced

Debian/Ubuntu:

```bash
apt update
apt upgrade
apt install package
apt remove package
apt autoremove
dpkg -l
```

RHEL/Fedora:

```bash
dnf install package
dnf update
dnf remove package
rpm -qa
```

Always distinguish:
- Repository metadata
- Package
- Installed files
- Dependencies
- Configuration files

---

## 28. Containers

Linux concepts behind containers include:

- Namespaces
- cgroups
- Capabilities
- Union/overlay filesystems
- Container images
- Container networking

Docker basics:

```bash
docker ps
docker images
docker pull nginx
docker run -d -p 8080:80 nginx
docker logs <container>
docker exec -it <container> /bin/sh
```

Understand the difference between:

```text
Container image → immutable template
Container        → running instance
Volume           → persistent data
Network          → container connectivity
```

---

## 29. Container Troubleshooting

Check:

```bash
docker ps -a
docker logs <container>
docker inspect <container>
docker exec -it <container> sh
```

Troubleshooting flow:

```text
Container running?
   ↓
Application process running?
   ↓
Port exposed?
   ↓
Port published?
   ↓
Application listening on correct interface?
   ↓
Network reachable?
   ↓
Logs healthy?
```

---

## 30. Linux Security Fundamentals

Apply:

```text
Least privilege
Defense in depth
Patch management
Strong authentication
Minimal exposed services
Secure file permissions
Centralized logging
Monitoring
Backups
```

Check listening services:

```bash
ss -lntup
```

Check privileged users:

```bash
getent group sudo
getent group wheel
```

Find SUID files:

```bash
find / -perm -4000 -type f 2>/dev/null
```

Treat security changes carefully and test them in a lab first.

---

## 31. Capabilities

Linux capabilities split some root privileges into smaller units.

View capabilities:

```bash
getcap /path/to/program
```

Set a capability:

```bash
sudo setcap cap_net_bind_service=+ep /path/to/program
```

Understand why capabilities can sometimes allow a process to perform a privileged operation without full root privileges.

---

## 32. Kernel Parameters with sysctl

View:

```bash
sysctl -a
```

Read one:

```bash
sysctl net.ipv4.ip_forward
```

Temporary change:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

Persistent configuration is commonly placed under:

```text
/etc/sysctl.conf
/etc/sysctl.d/
```

Do not change kernel parameters without understanding their effect.

---

## 33. Performance Investigation Framework

Use the four major resources:

```text
CPU
Memory
Disk I/O
Network
```

Initial commands:

```bash
uptime
top
free -h
vmstat 1
iostat
ss -s
df -h
```

Then narrow down:

```text
Symptom
  ↓
Resource
  ↓
Process
  ↓
System call / I/O / network / configuration
  ↓
Root cause
  ↓
Controlled fix
  ↓
Verification
```

---

## 34. Observability and Troubleshooting

Useful tools:

```bash
ps
top
vmstat
iostat
sar
pidstat
ss
lsof
strace
journalctl
```

### `strace`

Trace system calls:

```bash
strace -p PID
```

Run a command under tracing:

```bash
strace -f command
```

Useful for investigating:
- Missing files
- Permission errors
- Network calls
- Process behavior
- System-call failures

Use carefully in production because tracing can add overhead.

---

## 35. File Descriptors

View process file descriptors:

```bash
ls -l /proc/PID/fd
```

Using `lsof`:

```bash
lsof -p PID
```

System-wide open files:

```bash
lsof
```

Understand:

```text
stdin  = 0
stdout = 1
stderr = 2
```

---

## 36. `/proc` and `/sys`

`/proc` exposes process and kernel information.

Examples:

```bash
cat /proc/cpuinfo
cat /proc/meminfo
cat /proc/uptime
cat /proc/loadavg
```

For a process:

```bash
cat /proc/PID/status
ls -l /proc/PID/fd
```

`/sys` exposes kernel/device information:

```bash
ls /sys
```

---

## 37. Namespaces

Linux namespaces isolate resources between processes.

Important types include:

```text
PID
Network
Mount
UTS
IPC
User
Cgroup
```

Containers rely heavily on namespaces for isolation.

Inspect process namespaces:

```bash
ls -l /proc/PID/ns
```

---

## 38. cgroups

Control and account for resources such as:

- CPU
- Memory
- Processes
- I/O

Containers use cgroups to enforce resource limits.

Concept:

```text
Host
 ├── Group A → CPU/Memory limits
 └── Group B → CPU/Memory limits
```

---

## 39. Virtualization

Understand the difference:

```text
Virtual Machine
→ virtualizes hardware
→ contains a full guest OS

Container
→ shares host kernel
→ isolates processes/userspaces
```

Linux virtualization commonly uses KVM/QEMU.

Useful tools may include:

```bash
virsh list --all
```

---

## 40. NFS

Network File System allows remote filesystems to be mounted.

Client concepts:

```text
NFS Server
    ↓
Network
    ↓
NFS Client
    ↓
Mount point
```

Inspect mounts:

```bash
findmnt
mount
```

Troubleshooting should cover:
- DNS
- Network connectivity
- Server export
- Permissions
- Firewall
- NFS version
- Mount options

---

## 41. Linux Boot and Recovery Troubleshooting

If a system fails to boot:

```text
Firmware
 ↓
Bootloader
 ↓
Kernel
 ↓
initramfs
 ↓
systemd
 ↓
services
```

Investigate the failing layer rather than making random changes.

Useful commands from a working/recovery environment include:

```bash
journalctl -b
systemctl --failed
lsblk
blkid
```

---

## 42. Backup and Recovery

A production Linux system should have a recovery strategy.

Consider:

```text
Files
Databases
Configuration
Secrets
Application artifacts
System state
```

Basic archive:

```bash
tar -czvf backup.tar.gz /etc/myapp
```

Restore:

```bash
tar -xzvf backup.tar.gz
```

A backup is only useful if restoration has been tested.

---

## 43. Linux + DevOps

Linux is the foundation for many DevOps workflows.

Typical flow:

```text
Developer
   ↓
Git
   ↓
CI/CD
   ↓
Linux Build Agent
   ↓
Docker
   ↓
Kubernetes
   ↓
Cloud Infrastructure
```

Important Linux skills for DevOps:

- SSH
- Permissions
- Processes
- Services
- Networking
- Logs
- Bash
- Package management
- Containers
- Troubleshooting
- Automation

---

## 44. Linux + Kubernetes

Linux concepts map directly to Kubernetes:

```text
Linux Process       → Container process
Linux namespace     → Container isolation
cgroup              → Resource limits
Network interface   → Pod networking
Filesystem          → Container filesystem
Linux node          → Kubernetes worker node
systemd             → Host service management
```

A DevOps engineer should understand the Linux layer underneath Kubernetes.

---

## 45. Advanced Hands-On Labs

### Lab 1 — Service Troubleshooting

Install a web server in a lab VM.

Tasks:
1. Start it.
2. Verify its process.
3. Verify its listening port.
4. Test it using `curl`.
5. Stop it.
6. Inspect logs.
7. Start it again.
8. Configure it to start at boot.

Useful commands:

```bash
systemctl status nginx
ps aux | grep nginx
ss -lntp
curl -I http://localhost
journalctl -u nginx
```

### Lab 2 — Disk Investigation

Fill a test directory with dummy data.

Tasks:
1. Check filesystem usage.
2. Find the largest directories.
3. Check inode usage.
4. Find large files.
5. Delete test data.
6. Verify recovered space.

### Lab 3 — Network Troubleshooting

For a test application:

```text
Application
↓
Port
↓
Firewall
↓
IP
↓
Route
↓
DNS
```

Verify each layer separately.

### Lab 4 — Bash Automation

Create a script that:

1. Checks disk usage.
2. Checks memory.
3. Checks a service.
4. Writes results to a timestamped log.
5. Returns a non-zero exit code if a critical check fails.

### Lab 5 — Container Troubleshooting

Run nginx in Docker:

```bash
docker run -d --name web -p 8080:80 nginx
```

Verify:

```bash
docker ps
curl http://localhost:8080
docker logs web
docker exec -it web sh
```

Then intentionally stop the container and investigate why the service is unavailable.

---

## 46. Advanced Interview Topics

Be prepared to explain and troubleshoot:

### Processes
- PID / PPID
- Process states
- Signals
- Zombie processes
- Nice values
- Load average

### Memory
- Virtual memory
- Page cache
- Swap
- OOM behavior
- `/proc/meminfo`

### Storage
- Filesystems
- Inodes
- Mounts
- `/etc/fstab`
- LVM
- Disk-full vs inode-full
- Deleted-open files

### Networking
- TCP/IP
- DNS
- Routing
- Ports
- Sockets
- `ss`
- SSH
- Firewall
- HTTP troubleshooting

### Security
- Permissions
- ACL
- SUID/SGID
- Capabilities
- SELinux
- SSH hardening
- Least privilege

### Services
- systemd
- journald
- boot sequence
- dependencies
- service failures

### Automation
- Bash
- cron
- systemd timers
- text processing
- scripting error handling

### DevOps
- Docker
- Linux namespaces
- cgroups
- CI/CD agents
- Kubernetes Linux fundamentals
- Infrastructure automation

---

## 47. Production Troubleshooting Cheat Sheet

### CPU high

```bash
uptime
top
ps aux --sort=-%cpu | head
mpstat
pidstat
```

### Memory high

```bash
free -h
top
ps aux --sort=-%mem | head
vmstat 1
```

### Disk full

```bash
df -h
df -i
du -xh /var | sort -h
lsof +L1
```

### Service down

```bash
systemctl status SERVICE
journalctl -u SERVICE
ps aux | grep SERVICE
ss -lntup
```

### Port unavailable

```bash
ss -lntup
lsof -i :PORT
```

### DNS problem

```bash
cat /etc/resolv.conf
dig DOMAIN
getent hosts DOMAIN
```

### Network problem

```bash
ip addr
ip route
ip neigh
ping IP
ss -s
curl -v URL
```

---

## 48. Golden Rules

1. **Do not run commands blindly.**
2. Understand what a command changes before using it.
3. Avoid unnecessary root access.
4. Prefer graceful service/process termination.
5. Check logs before restarting services.
6. Make one change at a time during troubleshooting.
7. Back up important configuration before modifying it.
8. Test destructive commands in a lab first.
9. Separate symptoms from root causes.
10. Verify every fix.
11. Learn concepts, not only command memorization.
12. For DevOps, understand the Linux layer beneath your tools.

---

## Final Advanced Learning Path

```text
Linux Basics
     ↓
Shell + Permissions
     ↓
Processes + Services
     ↓
Networking
     ↓
Storage + LVM
     ↓
Logs + Troubleshooting
     ↓
Security + SELinux
     ↓
Bash Automation
     ↓
Containers
     ↓
Namespaces + cgroups
     ↓
Kubernetes Linux Fundamentals
     ↓
Production Troubleshooting
     ↓
DevOps
```

The goal is not to memorize hundreds of commands. The goal is to understand **how Linux works, how to investigate failures systematically, and how Linux supports modern DevOps infrastructure.**
