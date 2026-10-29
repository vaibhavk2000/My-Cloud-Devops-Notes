# Linux Notes for DevOps Interviews and Practical Work

These notes cover Linux fundamentals, essential commands, file permissions, users, processes, networking, logs, SSH, and troubleshooting. The examples are especially useful for your Jenkins, Docker, Terraform, and AWS EC2 practice.

## 1. What is Linux?

Linux is an open-source operating system kernel. Linux distributions such as Ubuntu, Debian, and Amazon Linux combine the kernel with system utilities and other software to create a complete operating system.

Why is Linux important in DevOps?

- Hosts Jenkins, Docker, Kubernetes tools, and web applications.
- Runs cloud servers such as AWS EC2 instances.
- Supports shell scripting and deployment automation.
- Provides tools for monitoring processes, logs, networking, and system resources.

Interview answer: “Linux is widely used in DevOps because it provides a stable environment for hosting applications, automating tasks through shell scripts, managing infrastructure, and troubleshooting servers.”

## 2. Linux file system structure

`/` — Root directory

`/home`

Regular users' home directories

`/root`

Root user's home directory

`/etc`

System configuration files

`/var/log`

Many system and application logs

`/var/lib`

Persistent application and service data

`/tmp`

Temporary files

`/usr/bin`

Many user commands and executables

`/opt`

Optional or third-party software

Other important directories:

- `/bin` and `/sbin`: Essential commands; on many modern distributions, these are links into `/usr`.
- `/dev`: Device files.
- `/proc`: Virtual filesystem exposing process and kernel information.
- `/sys`: Kernel and device information.
- `/boot`: Files required for booting.
- `/mnt` and `/media`: Common mount points for filesystems and removable media.
- `/var/lib/jenkins`: Common Jenkins home directory on Linux package installations, though it can be configured differently.

## 3. Basic Linux commands

### Navigation and files

| Command  | Purpose                                   | Example              |
| -------- | ----------------------------------------- | -------------------- |
| `pwd`    | Show current directory                    | `pwd`                |
| `ls`     | List files                                | `ls -l`              |
| `ls -la` | Include hidden files                      | `ls -la`             |
| `cd`     | Change directory                          | `cd /var/log`        |
| `cd ..`  | Go to parent directory                    | `cd ..`              |
| `mkdir`  | Create directory                          | `mkdir project`      |
| `touch`  | Create an empty file or update timestamps | `touch app.log`      |
| `cp`     | Copy files                                | `cp a.txt b.txt`     |
| `mv`     | Move or rename files                      | `mv old.txt new.txt` |
| `rm`     | Delete files                              | `rm file.txt`        |
| `rmdir`  | Remove an empty directory                 | `rmdir olddir`       |
| `file`   | Identify file type                        | `file app.jar`       |

Be careful with `rm -r` and especially `rm -rf`; they can permanently delete large directory trees.

### Reading and searching files

```
cat file.txt
less file.txt
head -n 20 file.txt
tail -n 50 app.log
tail -f app.log
grep "ERROR" app.log
grep -in "failed" app.log
find /var/log -name "*.log"
```

- `cat`: Display file content.
- `less`: Read a file page by page.
- `head` / `tail`: Display the beginning or end of a file.
- `tail -f`: Follow new log entries.
- `grep`: Search for matching text.
- `find`: Search for files and directories.

DevOps example: If your Spring Boot application fails to start, inspect its container logs or application log files and search for `ERROR`, `Exception`, or `Caused by`.

## 4. Linux file permissions

Linux permissions determine who can read, modify, or execute files.

There are three permission categories:

- Owner (`u`) — the file's owner.
- Group (`g`) — users in the assigned group.
- Others (`o`) — all other users.

| Permission    | Letter | Numeric value |
| ------------- | ------ | ------------- |
| Read          | `r`    | 4             |
| Write         | `w`    | 2             |
| Execute       | `x`    | 1             |
| No permission | `-`    | 0             |

For example, `-rwxr-xr--` means the owner can read, write, and execute; the group can read and execute; others can only read.

### Common permission commands

```
ls -l
chmod 755 script.sh
chmod 644 config.txt
chmod +x deploy.sh
chown ubuntu:ubuntu deploy.sh
```

- `755`: Owner has `rwx`; group and others have `r-x`.
- `644`: Owner has `rw-`; group and others have `r--`.
- `chmod +x`: Adds execute permission according to the command's default target.
- `chown`: Changes file ownership.

Important for Jenkins: If Jenkins cannot access a file or Docker socket, investigate the owner, group membership, directory permissions, and service configuration. Avoid using `chmod 777` as a shortcut.

## 5. Users and groups

Linux supports multiple users, each with its own permissions and access.

```
whoami
id
who
groups
sudo adduser devuser
sudo usermod -aG docker devuser
sudo passwd devuser
```

These commands show the current user, user and group IDs, logged-in users, group membership, create a user, add a user to a group, and set a password.

On some distributions, user-management commands differ. For example, `useradd` is available on many distributions, while `adduser` may be an interactive wrapper.

What is `sudo`?

`sudo` runs a command with elevated privileges when the user's policy permits it.

```
sudo systemctl status jenkins
sudo journalctl -u jenkins -n 100
```

Be cautious when granting administrator access or adding users to the `docker` group: Docker access can effectively grant root-level control of the host.

## 6. Processes and services

A process is a running program. A service is software managed to run in the background, often through `systemd`.

### Process commands

```
ps aux
top
pgrep -af java
kill PID
```

- `ps aux`: List running processes.
- `top`: Monitor processes and resource usage interactively.
- `pgrep -af java`: Search for processes whose command lines match `java`.
- `kill PID`: Send the default termination signal to a process.

Use `kill -9 PID` only as a last resort because it does not allow the process to shut down gracefully.

### Service management with systemd

```
sudo systemctl status jenkins
sudo systemctl start jenkins
sudo systemctl stop jenkins
sudo systemctl restart jenkins
sudo systemctl enable jenkins
sudo systemctl disable jenkins
```

- `start`: Start a service now.
- `stop`: Stop a service.
- `restart`: Restart a service.
- `enable`: Configure the service to start automatically at boot.
- `status`: Show service state and recent status messages.

### View service logs

```
sudo journalctl -u jenkins
sudo journalctl -u jenkins -n 100
sudo journalctl -u jenkins -f
```

These are useful when Jenkins is down, a service fails to start, or a deployment command behaves unexpectedly.

## 7. Disk, memory, and CPU monitoring

| Command        | Purpose                                         |
| -------------- | ----------------------------------------------- |
| `df -h`        | Check filesystem space                          |
| `du -sh /path` | Check directory size                            |
| `free -h`      | Check memory usage                              |
| `uptime`       | Show uptime and load averages                   |
| `top`          | Inspect processes and system load               |
| `lscpu`        | Show CPU details                                |
| `lsblk`        | Show block devices and partitions               |
| `vmstat 1`     | Display system activity at one-second intervals |

Example:

```
df -h
free -h
du -sh /var/lib/jenkins
```

If a Jenkins build fails with “No space left on device,” check `df -h` first. Use `du -sh` to identify large directories. Do not delete Jenkins workspaces, Docker data, or logs blindly because they may contain important data.

## 8. Networking commands

These commands are particularly important when an EC2-hosted application is unreachable.

```
ip addr
ip route
ss -tulpn
curl -I http://localhost:8080
curl -v http://SERVER_IP:8080
getent hosts example.com
ping -c 4 8.8.8.8
```

- `ip addr`: Displays network interfaces and addresses.
- `ip route`: Displays routing information.
- `ss -tulpn`: Shows listening TCP/UDP sockets and, where permitted, process information.
- `curl`: Tests HTTP connectivity.
- `getent hosts`: Checks name resolution through the configured system resolver.
- `ping`: Tests ICMP reachability when allowed.

For your AWS projects, an application working on `localhost:8080` does not automatically mean it is accessible externally. Check the application bind address, host firewall, EC2 security group, subnet routes, and public addressing.

## 9. Package management

Package commands depend on the Linux distribution.

| Task                     | Ubuntu / Debian          | Amazon Linux / RHEL family |
| ------------------------ | ------------------------ | -------------------------- |
| Refresh package metadata | `sudo apt update`        | `sudo dnf check-update`    |
| Install a package        | `sudo apt install nginx` | `sudo dnf install nginx`   |
| Upgrade packages         | `sudo apt upgrade`       | `sudo dnf upgrade`         |
| Search packages          | `apt search nginx`       | `dnf search nginx`         |

Some Amazon Linux releases use `yum`; check the distribution before following commands from a tutorial.

## 10. SSH and file transfer

SSH provides secure remote terminal access.

```
ssh -i my-key.pem ubuntu@SERVER_IP
```

To copy a file to a remote server:

```
scp -i my-key.pem app.jar ubuntu@SERVER_IP:/home/ubuntu/
```

To copy a directory:

```
scp -i my-key.pem -r ./config ubuntu@SERVER_IP:/home/ubuntu/
```

For production environments, prefer managed access or tightly restricted SSH access, protect private keys, and avoid sharing credentials.

## 11. Linux shell scripting

A shell script automates commands that would otherwise be executed manually.

Create a file named `deploy.sh`:

```
#!/bin/bash
set -e

echo "Starting deployment..."

if command -v java >/dev/null 2>&1; then
    java -version
else
    echo "Java is not installed"
    exit 1
fi

echo "Deployment checks completed."
```

Run it:

```
chmod +x deploy.sh
./deploy.sh
```

Important concepts:

- `#!/bin/bash`: Specifies the interpreter.
- `echo`: Prints messages.
- `if`: Performs conditional checks.
- `exit 1`: Exits with a nonzero status to indicate failure.
- `set -e`: Stops the script when certain commands fail.
- `$1`, `$2`: Access positional arguments.
- `$?`: Contains the exit status of the previous command.

Example:

```
#!/bin/bash
echo "Application: $1"
echo "Environment: $2"
```

Execute with:

```
./deploy.sh student-app production
```

## 12. Environment variables

Environment variables provide configuration to applications without hardcoding values in the application source.

```
export APP_ENV=production
echo "$APP_ENV"
printenv
```

For a Spring Boot application:

```
export SPRING_PROFILES_ACTIVE=prod
export DB_HOST=my-database.example
java -jar app.jar
```

These are example settings, not actual database credentials.

Security rule: Do not commit passwords, database credentials, or secret API keys to GitHub. Use an appropriate secret manager, protected CI/CD credentials, or securely configured environment variables. Remember that environment variables may be visible to privileged users and processes.

## 13. Scheduling jobs with cron

Cron runs scheduled commands on supported Linux systems.

Edit your user's crontab:

```
crontab -e
```

Example schedule:

```
0 2 * * * /home/ubuntu/backup.sh
```

This runs the script every day at 2:00 AM according to the system's configured time zone.

Cron syntax:

`minute hour day-of-month month day-of-week`

Examples:

| Schedule      | Meaning              |
| ------------- | -------------------- |
| `0 * * * *`   | Every hour           |
| `*/5 * * * *` | Every five minutes   |
| `0 2 * * *`   | Daily at 2 AM        |
| `0 9 * * 1`   | Every Monday at 9 AM |

Check existing schedules using `crontab -l`.

## 14. Important Linux troubleshooting scenarios

Scenario 1: Jenkins service is not running

```
sudo systemctl status jenkins
sudo journalctl -u jenkins -n 100
sudo systemctl restart jenkins
```

Check the logs for Java problems, configuration errors, or port conflicts before restarting repeatedly.

Scenario 2: Docker permission denied

```
whoami
id
docker ps
sudo docker ps
```

If `sudo docker ps` works but `docker ps` fails, investigate Docker socket permissions and group membership. Re-login after group changes, and remember that Docker group access is highly privileged.

Scenario 3: Application container exited

```
docker ps -a
docker logs --tail 100 backend-container
docker inspect backend-container
```

Check the application exception, environment variables, database connectivity, port configuration, and container exit code.

Scenario 4: Port 8080 is not accessible

```
sudo ss -ltnp
curl -v http://localhost:8080
```

If the local request fails, inspect the application process and logs. If local access works but external access fails, check port binding, firewall rules, AWS security groups, and network routes.

Scenario 5: Permission denied for a file

```
ls -l file.txt
namei -l /path/to/file.txt
id
```

Inspect the file's ownership and permissions, along with execute permissions on parent directories. Change only the permissions or ownership that actually need correction.

## 15. Common Linux interview questions

1\. What is the difference between Linux and Unix?Unix refers to a family of operating systems and related systems. Linux is an open-source Unix-like kernel used by many distributions.

2\. What is the difference between absolute and relative paths?An absolute path starts from the root directory, such as /home/ubuntu/app. A relative path is interpreted from the current directory, such as ./app.

3\. What is the difference between hard and symbolic links?A hard link refers to the same underlying filesystem inode. A symbolic link stores a path to another file or directory and can become broken if its target moves.

4\. What is the difference between a process and a thread?A process is a running program with its own process resources and address space. Threads are execution units within a process that generally share its memory.

5\. What is the difference between kill and kill -9?kill normally sends SIGTERM, allowing graceful shutdown. kill -9 sends SIGKILL, which cannot be caught or ignored and prevents graceful cleanup.

6\. What is the difference between grep and find?grep searches text for matching patterns. find searches the filesystem for files and directories matching specified conditions.

7\. What is the difference between df and du?df reports filesystem space usage. du estimates disk usage for specified files and directories.

8\. What is the root user?Root is the superuser with broad administrative privileges on Linux.

9\. What is a daemon?A daemon is a background process that provides a service or performs tasks, often without direct user interaction.

10\. What does chmod 755 mean?The owner receives read, write, and execute permissions. The group and others receive read and execute permissions.

11\. How do you check a running service?Use systemctl status service-name on systems managed by systemd, then inspect service logs if necessary.

12\. How do you find which process uses a port?Use sudo ss -ltnp for TCP listening sockets, or sudo lsof -i :8080 if lsof is installed.

13\. What is a zombie process?A zombie is a process that has exited but whose parent has not yet collected its exit status. It usually cannot be terminated like a running process.

14\. What is the PATH variable?PATH is a colon-separated list of directories searched by the shell when resolving executable commands.

15\. How do you troubleshoot high CPU usage?Use top or ps to identify processes consuming CPU, then examine their logs and workload before taking corrective action.

