# 🐧 Linux Commands Cheatsheet & System Administration Guide

Essential Linux commands for system administrators, DevOps engineers, and developers.

---

## 📂 1. System Navigation & Information

```bash
# Print working directory
pwd 

# List directory contents with detailed permissions and hidden files
ls -la

# Change directory
cd /var/log

# Print kernel, architecture, and system information
uname -a

# Print hostname
hostname

# Display current user info
whoami
id
```

---

## 📄 2. File & Directory Operations

```bash
# Create new directory (with parent directories if missing)
mkdir -p /path/to/directory

# Create empty file or update timestamp
touch filename.txt

# Copy file / directory recursively
cp source.txt destination.txt
cp -r /source/dir /destination/dir

# Move or rename file/directory
mv oldname.txt newname.txt

# Delete file / directory recursively
rm file.txt
rm -rf directory/

# Search for files by name or modified date
find /var/log -name "*.log"
find . -type f -mtime -7
```

---

## 👁️ 3. File Viewing & Text Processing

```bash
# View entire file content
cat file.txt

# View file page by page
less file.txt

# View top 10 lines of a file
head -n 10 file.txt

# Monitor logs in real time (follow output)
tail -f /var/log/syslog

# Search for patterns in files
grep -rn "ERROR" /var/log/

# Count lines, words, and characters
wc -l file.txt

# Stream editor (replace text pattern in stream or file)
sed -i 's/old_text/new_text/g' config.conf

# Pattern scanning and text processing language
awk '{print $1, $3}' access.log
```

---

## 🔐 4. Permissions & Ownership

Linux file permissions structure: `rwx rwx rwx` (Owner, Group, Others).

```bash
# Change file permissions
chmod 755 script.sh      # rwxr-xr-x
chmod +x script.sh       # Add execution permission

# Numerical Permission Reference:
# 4 = Read (r), 2 = Write (w), 1 = Execute (x)
# 7 = rwx, 6 = rw-, 5 = r-x, 4 = r--

# Change file owner and group
chown user:group filename.txt
chown -R user:group /directory/

# View current umask (default creation permission mask)
umask
```

---

## ⚡ 5. Process Management & Monitoring

```bash
# List running processes
ps aux

# Find specific running process by name
ps aux | grep nginx
pgrep -l nginx

# Interactive real-time process viewer
top
htop    # (if installed)

# Terminate process by PID
kill 1234
kill -9 1234    # Force kill SIGKILL

# Terminate process by name
pkill nginx

# View system resource usage (memory)
free -h

# View disk usage by filesystem
df -h

# View directory size summary
du -sh /var/log/*

# Check system uptime and load average
uptime
```

---

## ⚙️ 6. Systemd Service Management

```bash
# Check service status
systemctl status nginx

# Start / Stop / Restart service
systemctl start nginx
systemctl stop nginx
systemctl restart nginx

# Enable service to start automatically on boot
systemctl enable nginx

# Disable service auto-start
systemctl disable nginx

# View system logs using journalctl
journalctl -u nginx --since "1 hour ago"
journalctl -f -u docker
```

---

## 🌐 7. Networking & Connectivity

```bash
# Check network connectivity to a host
ping -c 4 8.8.8.8

# Fetch web contents or test HTTP APIs
curl -Iv https://example.com
wget https://example.com/file.tar.gz

# Display listening ports and network connections
netstat -tulpn
# or modern alternative:
ss -tulpn

# Display network interfaces and IP addresses
ip addr show
ip route show

# DNS Lookup
dig example.com
nslookup example.com

# Secure Shell (SSH) connection
ssh user@remote-server-ip -i ~/.ssh/id_rsa
```

---

## 📦 8. Package Management

### Ubuntu / Debian (APT)
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y nginx curl git
sudo apt remove nginx
```

### RHEL / CentOS / Fedora (DNF / YUM)
```bash
sudo dnf check-update
sudo dnf install -y nginx curl git
sudo dnf remove nginx
```

---

## 🧠 Useful One-Liners for DevOps Engineers

- **Find top 10 largest directories/files:**
  ```bash
  du -a /var | sort -n -r | head -n 10
  ```
- **Check open connections count by IP:**
  ```bash
  ss -tan | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr
  ```
- **Recursively search and replace string in all config files:**
  ```bash
  grep -rl "http://" /etc/nginx/ | xargs sed -i 's/http:\/\//https:\/\//g'
  ```
