# File Transfer Guide: Linux & Windows

A comprehensive guide to securely transfer files between local and remote systems on Linux and Windows platforms.

---

## Table of Contents

- [Linux: SCP (Secure Copy Protocol)](#linux-scp)
- [Linux: SFTP](#linux-sftp)
- [Windows: PowerShell](#windows-powershell)
- [Windows: WinSCP](#windows-winscp)
- [Windows: OpenSSH](#windows-openssh)
- [Cross-Platform Tools](#cross-platform-tools)
- [Troubleshooting](#troubleshooting)

---

## Linux: SCP

SCP (Secure Copy Protocol) is a command-line utility for securely copying files over SSH.

### Basic Syntax

```bash
scp [options] source destination
```

### Copy Local to Remote

```bash
# Copy single file
scp /path/to/local/file username@remote_host:/path/to/remote/

# Copy directory (recursive)
scp -r /path/to/local/directory username@remote_host:/path/to/remote/

# Example
scp myfile.txt user@192.168.1.100:/home/user/
```

### Copy Remote to Local

```bash
# Copy single file
scp username@remote_host:/path/to/remote/file /path/to/local/

# Copy directory (recursive)
scp -r username@remote_host:/path/to/remote/directory /path/to/local/

# Example
scp user@192.168.1.100:/home/user/myfile.txt ~/Downloads/
```

### Copy Between Two Remote Servers

```bash
scp username1@host1:/path/to/file username2@host2:/path/to/destination/
```

### Common Options

| Option | Description |
|--------|-------------|
| `-P port` | Specify custom SSH port (capital P) |
| `-r` | Recursively copy directories |
| `-p` | Preserve file permissions and timestamps |
| `-C` | Enable compression during transfer |
| `-v` | Verbose output (show transfer details) |
| `-l limit` | Limit bandwidth in Kbps |

### Examples

```bash
# Custom SSH port
scp -P 2222 file.txt user@example.com:/home/user/

# Directory with compression
scp -r -C /local/folder user@example.com:/remote/path/

# Verbose transfer with bandwidth limit
scp -v -l 1000 file.txt user@example.com:/home/user/

# Preserve permissions
scp -p important.conf user@example.com:/etc/config/
```

---

## Linux: SFTP

SFTP (SSH File Transfer Protocol) provides an interactive file transfer interface over SSH.

### Connect to Remote Host

```bash
sftp username@remote_host
```

### Common SFTP Commands

```bash
# List remote files
ls

# List local files
lls

# Change remote directory
cd /path/to/directory

# Change local directory
lcd /path/to/directory

# Upload file
put local_file.txt

# Upload directory (recursive)
put -r local_directory

# Download file
get remote_file.txt

# Download directory (recursive)
get -r remote_directory

# Delete file
rm filename

# Create directory
mkdir dirname

# Exit SFTP
exit
```

### Example SFTP Session

```bash
$ sftp user@example.com
Connected to example.com.
sftp> ls
file1.txt  file2.txt  documents/

sftp> cd documents
sftp> put local_report.pdf
Uploading local_report.pdf to /home/user/documents/local_report.pdf
100%                             1024KB   2.1MB/s   00:00

sftp> get remote_data.csv
Fetching /home/user/documents/remote_data.csv to remote_data.csv
100%                             512KB    1.8MB/s   00:00

sftp> exit
```

---

## Windows: PowerShell

Windows 10+ includes OpenSSH via PowerShell, supporting SCP and SSH natively.

### Copy Local to Remote

```powershell
# Single file
scp C:\Users\YourName\file.txt username@remote_host:C:/Users/RemoteUser/

# Directory (recursive)
scp -r C:\Users\YourName\folder username@remote_host:C:/Users/RemoteUser/

# Example
scp C:\Downloads\myfile.txt user@192.168.1.100:C:/Users/user/
```

### Copy Remote to Local

```powershell
# Single file
scp username@remote_host:C:/Users/RemoteUser/file.txt C:\Users\YourName\

# Directory (recursive)
scp -r username@remote_host:C:/Users/RemoteUser/folder C:\Users\YourName\

# Example
scp user@192.168.1.100:C:/Users/user/myfile.txt C:\Downloads\
```

### Check OpenSSH Installation

```powershell
ssh -V
```

If not installed, enable via Settings > Apps > Optional Features > OpenSSH Client.

### Using PowerShell Profile

Create a profile for custom SSH commands:

```powershell
# Edit profile
notepad $PROFILE

# Add alias
function Copy-ToRemote {
    param([string]$LocalPath, [string]$RemotePath)
    scp -P 2222 $LocalPath $RemotePath
}
```

---

## Windows: WinSCP

WinSCP is a GUI-based SFTP/SCP client for Windows.

### Download & Install

- Download from: https://winscp.net/
- Run the installer
- No additional dependencies required

### Connecting to Remote Host

1. **Open WinSCP** → New Site
2. **File Protocol**: Choose SFTP or SCP
3. **Host name**: `remote_host` or IP address
4. **Port**: `22` (default SSH port)
5. **User name**: Your SSH username
6. **Password**: Your SSH password (or use private key)
7. Click **Login**

### File Transfer in WinSCP

**Drag and Drop:**
- Drag files from local panel (left) to remote panel (right) to upload
- Drag files from remote panel (right) to local panel (left) to download

**Manual Transfer:**
- Right-click file → **Copy** to copy to other panel
- Or use the arrow buttons between panels

### WinSCP Command-Line Usage

```powershell
# SCP command-line transfer
"C:\Program Files (x86)\WinSCP\WinSCP.com" /command `
  "open scp://user:password@example.com" `
  "put C:\local\file.txt /remote/" `
  "exit"

# SFTP command-line transfer
"C:\Program Files (x86)\WinSCP\WinSCP.com" /command `
  "open sftp://user@example.com" `
  "cd /remote/path" `
  "put C:\local\file.txt" `
  "exit"
```

---

## Windows: OpenSSH

Modern Windows (10+) includes native OpenSSH support.

### Enable OpenSSH

**Via Settings:**
1. Settings → Apps → Optional Features
2. Click "**View features**"
3. Find "**OpenSSH Client**" → Install
4. (Optional) Install "**OpenSSH Server**" to accept SSH connections

**Via PowerShell (Admin):**

```powershell
# Install OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

### SSH Key Setup on Windows

```powershell
# Generate SSH key pair
ssh-keygen -t rsa -b 4096

# Save to default location (~/.ssh/id_rsa)
# When prompted for passphrase, press Enter (or set one)

# Add public key to remote server
scp $env:USERPROFILE\.ssh\id_rsa.pub user@example.com:.ssh/

# Connect to remote and authorize key
ssh user@example.com
cat .ssh/id_rsa.pub >> .ssh/authorized_keys
exit
```

### Batch File Script for Windows

```batch
@echo off
REM File Transfer Script for Windows
set REMOTE_HOST=user@192.168.1.100
set REMOTE_PATH=/home/user/
set LOCAL_FILE=C:\path\to\file.txt

scp %LOCAL_FILE% %REMOTE_HOST%:%REMOTE_PATH%

if %errorlevel% equ 0 (
    echo File transferred successfully!
) else (
    echo Transfer failed with error code %errorlevel%
)
pause
```

---

## Cross-Platform Tools

### Rsync (Linux/macOS/Windows via WSL or Git Bash)

Rsync is more efficient for large transfers or syncing directories.

```bash
# Upload with rsync
rsync -avz /local/path/ username@remote_host:/remote/path/

# Download with rsync
rsync -avz username@remote_host:/remote/path/ /local/path/

# Options:
# -a: Archive mode
# -v: Verbose
# -z: Compress during transfer
# -P: Partial transfer + progress
```

### Rclone (Cross-platform)

Supports cloud storage and remote servers.

```bash
# Copy file
rclone copy /local/path remote:/remote/path

# Sync directories
rclone sync /local/directory remote:/remote/directory

# List remote files
rclone ls remote:/path
```

### Docker (File Transfer via Container)

```bash
# Copy TO container
docker cp /local/path container_name:/container/path

# Copy FROM container
docker cp container_name:/container/path /local/path
```

---

## Troubleshooting

### Permission Denied (Linux/Mac)

**Problem**: "Permission denied (publickey, password)."

**Solution:**
```bash
# Check SSH permissions
ls -la ~/.ssh/
# id_rsa should be 600
# authorized_keys should be 644

chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/authorized_keys
```

### Connection Timeout

**Problem**: `ssh: connect to host ... port 22: Connection timed out`

**Solution:**
- Verify host is reachable: `ping remote_host`
- Check SSH port: `netstat -an | grep :22` (Linux) or `netstat -an | findstr :22` (Windows)
- Check firewall settings
- Verify SSH service is running

### Port Already in Use (Windows)

**Problem**: Port 22 already in use.

**Solution:**
```powershell
# Find process using port 22
netstat -ano | findstr :22

# Kill process
taskkill /PID <PID> /F

# Or use different port
ssh -p 2222 user@host
```

### Key Permission Issues

**Problem**: "WARNING: UNPROTECTED PRIVATE KEY FILE"

**Solution:**
```bash
# Linux/Mac
chmod 600 ~/.ssh/id_rsa

# Windows PowerShell
icacls $env:USERPROFILE\.ssh\id_rsa /inheritance:r /grant:r "$env:USERNAME`:`(F`)"
```

### Slow Transfer Speed

**Solutions:**
- Use compression: `scp -C file user@host:/path`
- Use rsync: `rsync -avz --partial`
- Check bandwidth limits: `scp -l 0` (unlimited)
- Use local network instead of public internet

### File Permissions Not Preserved

**Problem**: Files arrive with different permissions.

**Solution:**
```bash
# Use -p flag with SCP
scp -p file user@host:/path

# Or use rsync (preserves more metadata)
rsync -av file user@host:/path
```

---

## Best Practices

1. **Use SSH Keys**: Eliminate passwords, improve security
2. **Enable Compression**: Use `-C` flag for large files
3. **Verify Checksums**: Compare file integrity after transfer
4. **Use SFTP for Interactive**: Better for multiple file operations
5. **Use SCP for Automation**: Simpler for scripts
6. **Set Proper Permissions**: `chmod 600` for private keys
7. **Use Custom Ports**: Change SSH port from 22 if possible
8. **Monitor Transfers**: Use `-v` flag for debugging
9. **Test Connectivity First**: Verify SSH access before transferring
10. **Backup Important Files**: Always have backups before large transfers

---

## Quick Reference

| Task | Linux | Windows |
|------|-------|---------|
| Copy to remote | `scp file user@host:/path` | `scp file user@host:path` |
| Copy from remote | `scp user@host:/path/file .` | `scp user@host:path/file .` |
| Interactive transfer | `sftp user@host` | WinSCP, PowerShell remoting |
| Batch script | Bash script | Batch file, PowerShell script |
| GUI interface | Nautilus, Dolphin | WinSCP, Explorer extension |

---

## Additional Resources

- [OpenSSH Documentation](https://man.openbsd.org/ssh)
- [WinSCP Documentation](https://winscp.net/eng/docs/)
- [Rsync Manual](https://linux.die.net/man/1/rsync)
- [SFTP Documentation](https://tools.ietf.org/html/draft-ietf-secsh-filexfer-extensions)

---

**Last Updated**: February 2026  
**License**: MIT  
**Contributing**: Feel free to submit issues and pull requests
