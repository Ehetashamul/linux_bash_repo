# Linux and Bash Learning

This repository contains Linux administration notes, Bash scripting guidance, practical labs, and reference material for DevOps learning. The examples progress from command-line fundamentals to production troubleshooting and automation.

## Learning Path

Work through the material in this order:

1. [`Linux_Basic_Learning.md`](Linux_Basic_Learning.md): command-line fundamentals, filesystems, permissions, users, processes, pipes, redirection, text processing, packages, archives, networking, SSH, services, cron, and troubleshooting.
2. [`Bash_Scripting_Handbook.md`](Bash_Scripting_Handbook.md): variables, arrays, strings, input, arithmetic, conditions, loops, functions, arguments, `getopts`, logging, background jobs, and cron.
3. [`Linux_Advanced_Learning.md`](Linux_Advanced_Learning.md): systemd, boot, storage, mounts, LVM, inodes, networking, DNS, HTTP, SSH, firewalls, SELinux, ACLs, logs, containers, namespaces, cgroups, performance, recovery, and Kubernetes fundamentals.
4. [`Advanced_Bash_Scripting.md`](Advanced_Bash_Scripting.md): traps, subshells, error handling, logging, file automation, monitoring, CI/CD helpers, scheduling, and argument parsing.
5. [`Shell_Varibale.md`](Shell_Varibale.md): a reference for positional, process, environment, random, and input/output shell variables.

## Prerequisites

Use a Linux machine, virtual machine, cloud VM, or WSL environment. On Windows, WSL can be installed with:

```powershell
wsl --install
```

Verify the environment from a Linux shell:

```bash
cat /etc/os-release
uname -a
```

Some labs require additional tools or services, including `sudo`, `systemd`, Nginx, Docker, `dig`, `lsof`, `at`, mail utilities, and performance tools.

## Running Bash Examples

Most examples are included in the Markdown files. To try one, create a script in a lab environment:

```bash
cat > hello.sh <<'EOF'
#!/usr/bin/env bash
echo "Hello from Bash"
EOF

chmod +x hello.sh
./hello.sh
```

You can also run a script without changing its permissions:

```bash
bash hello.sh
```

The labs cover file and permission management, process investigation, network troubleshooting, Bash health checks, Nginx service troubleshooting, and Docker troubleshooting.

## Reference Material

Additional PDF references are available in the [`Bash`](Bash) directory:

- `Shell Scripting Crash Course.pdf`
- `Master Shell Scripting with This Free PDF Guide.pdf`

## Safety Notes

- Run system administration examples in a disposable lab or test environment.
- Commands involving `sudo`, `rm -rf`, package removal, firewall changes, LVM, SELinux, `sysctl`, users, or services can change or damage a system.
- Replace placeholders such as `PID`, `SERVICE`, `PORT`, `DOMAIN`, and `URL` before running commands.
- Package commands differ between Debian/Ubuntu and RHEL/Fedora systems.
- Deployment examples using `ssh`, `git pull`, or `systemctl restart` are illustrative and should not be run unchanged.

There are currently no checked-in `.sh` scripts or automated tests; the repository is organized as study notes and command examples.