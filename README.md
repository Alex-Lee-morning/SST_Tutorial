# 🌐 Cross-Platform Remote Access Setup (OpenSSH + Firewall + Tailscale)

> Supported systems: Windows 11 / Ubuntu 24.04+ / macOS (Apple Silicon)

---

## Table of Contents

1. 🔧 Prerequisites
2. 🛠 Install and Configure OpenSSH

   * Windows
   * Ubuntu
   * macOS
3. 🔥 Firewall Rules
4. 🌐 Connect Devices with Tailscale
5. ✅ Test the Remote Connection
6. 🧰 Troubleshooting
7. 🚨 Remote Access and Debugging with VS Code Remote-SSH and Tailscale

---

## 1. 🔧 Prerequisites

You will need:

* A GitHub account (or another account you can use to sign in to Tailscale)
* A terminal you use regularly (Terminal / PowerShell / zsh)

---

## 2. 🛠 Install and Configure OpenSSH

### 🪠 Windows 11

#### Check whether OpenSSH is already installed (if you are not sure):

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
```

#### Install the client and the server:

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

#### Start the service:

```powershell
Start-Service sshd
# Start automatically at boot
Set-Service -Name sshd -StartupType 'Automatic'
```

---

### 🤓 Ubuntu

```bash
sudo apt update
sudo apt install openssh-server
# Enable the service at boot
sudo systemctl enable ssh
sudo systemctl start ssh
# Start the service manually
sudo service ssh start
```

---

### 🍏 macOS

⚠️ macOS usually ships with an OpenSSH client. You can check with:

```bash
which ssh
# or
ssh -V
```

Install the OpenSSH server with Homebrew.

⚠️ Install Homebrew first (skip this if it is already installed):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

```bash
brew install openssh
```

---

## 3. 🔥 Firewall Configuration

### 🪠 Windows Defender Firewall (incomplete)

Open port 22 (the default SSH port):

```powershell
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Port 22' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

You can also create the firewall rule through the graphical interface.

---

### 🤓 Ubuntu UFW

Default port: 22

```bash
# Allow SSH through the firewall
sudo ufw allow ssh
# Enable the firewall
sudo ufw enable
```

---

### 🔒 macOS Firewall Notes

macOS allows SSH remote connections by default, so no extra firewall setup is required.

If you have turned on the system firewall or use a third-party firewall, make sure `sshd` is allowed to receive inbound connections:

```bash
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --add /usr/sbin/sshd
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --unblockapp /usr/sbin/sshd
```

---

## 4. 🌐 Connect Devices with Tailscale

### 📅 Install Tailscale

Download page: [https://tailscale.com/download](https://tailscale.com/download)

#### Ubuntu

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

#### macOS

```bash
brew install --cask tailscale
open /Applications/Tailscale.app
```

---

### 💡 Alternative install method

Download the `.pkg` installer from the official website.

#### Windows

🤔 Download and run the official installer from the website.

### 📡 Check the IP address

```bash
# Show the IP address assigned by Tailscale
tailscale ip
# List devices on the current Tailscale account
tailscale status
```

---

## 5. ✅ Test the Connection

Example:

```bash
# Print the IP address
tailscale status
# Try to ping the IP
ping ip
# Connect over SSH
ssh username@ip
```

---

## 6. 🧰 Troubleshooting

### Linux

Run:

```bash
tailscale status
```

If the output looks like this:

```bash
100.0.0.0    user-ubuntu          user@ linux   -
```

and SSH cannot connect, try:

```bash
ping ip
```

Then check the status again. If it now shows:

```bash
100.0.0.0    user-ubuntu          user@ linux   active
```

you can start the SSH connection.

### macOS

### Windows

---

## 🚨 Remote Access and Debugging with VS Code Remote-SSH and Tailscale

### Example: macOS and Linux

#### ⚠️ Prerequisites

Both machines need VS Code installed, and both need to be signed in to Tailscale.

On macOS (the client), install the Remote-SSH extension in VS Code.

1. Press `Shift+Cmd+P` to open the Command Palette.
2. Type: `Remote-SSH: Connect to Host...`
3. Enter `ssh user@ip`.
   * The first time, VS Code installs the VS Code Server on the remote machine.
   * A connection log is shown.
4. After a successful connection, the SSH address appears in the lower-left corner.

🤔 How do you disconnect?

1. Closing VS Code disconnects the SSH session by default.
2. Click the green arrow or the SSH address in the lower-left corner and choose **Close Remote Connection**.

## 👨‍💻 Author

* GitHub: [@Alex-Lee-morning](https://github.com/Alex-Lee-morning)
