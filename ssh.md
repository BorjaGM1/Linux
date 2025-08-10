# SSH Key Authentication Setup Guide

This guide walks you through setting up SSH key authentication for secure, password-free server access.

## Overview

SSH key authentication involves three main steps:
1. Create an SSH key pair on your local computer
2. Copy your public key to the server
3. Configure the server to use keys and disable password authentication

## Step 1: Create SSH Key Pair

### Generate the Key Pair

Open your terminal and run:

```bash
ssh-keygen -t rsa -b 4096
```

- `-t rsa`: Specifies RSA key type
- `-b 4096`: Creates a 4096-bit key for enhanced security

### Configuration Prompts

**File location**: Press Enter to accept the default location (`~/.ssh/id_rsa`)

**Passphrase**: Choose based on your security needs:
- **High security**: Enter a strong passphrase
- **Convenience**: Leave empty (press Enter)

This creates two files:
- `id_rsa` (private key - keep secret)
- `id_rsa.pub` (public key - goes on server)

## Step 2: Copy Public Key to Server

### Method 1: ssh-copy-id (Recommended for macOS/Linux)

```bash
ssh-copy-id username@your_server_ip
```

Enter your VPS password when prompted. This will be the last time you need it.

### Method 2: Manual Copy (Windows or if ssh-copy-id unavailable)

1. **Display your public key locally:**
   ```bash
   # macOS/Linux
   cat ~/.ssh/id_rsa.pub
   
   # Windows PowerShell
   Get-Content ~/.ssh/id_rsa.pub
   ```

2. **Copy the entire output to your clipboard**

3. **Log into your VPS:**
   ```bash
   ssh username@your_server_ip
   ```

4. **Create SSH directory and set permissions:**
   ```bash
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   touch ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

5. **Add your public key:**
   ```bash
   nano ~/.ssh/authorized_keys
   ```
   Paste your public key, save (Ctrl+X, Y, Enter)

## Step 3: Test and Secure

### Test Key Authentication

⚠️ **Important**: Test before disabling passwords to avoid lockout.

1. Open a **new terminal window** (keep your current session open)
2. Test SSH key login:
   ```bash
   ssh username@your_server_ip
   ```
3. Should log in without password prompt

### Disable Password Authentication

Once key authentication works:

1. **Edit SSH configuration:**
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

2. **Find and modify these lines:**
   ```
   PasswordAuthentication no
   PermitRootLogin no
   ```
   (Remove `#` if present to uncomment)

3. **Check for additional configuration files** (some systems use includes):
   ```bash
   # Look for Include directives in main config
   grep -i "^Include" /etc/ssh/sshd_config
   
   # Common additional locations to check
   ls /etc/ssh/sshd_config.d/
   ls /etc/ssh/conf.d/
   ```

4. **If additional config files exist**, edit them too:
   ```bash
   sudo nano /etc/ssh/sshd_config.d/50-cloud-init.conf
   # Or whatever files were found above
   ```

5. **Verify your configuration:**
   ```bash
   sudo sshd -t
   ```
   (Should return no errors)

6. **Restart SSH service:**
   ```bash
   sudo systemctl restart sshd
   ```

## Security Benefits

- **Enhanced security**: Private keys are much stronger than passwords
- **Faster logins**: No password typing required
- **Brute force protection**: Password attacks become impossible
- **Audit trail**: Key-based access is easier to track

Your server is now secured with SSH key authentication!
