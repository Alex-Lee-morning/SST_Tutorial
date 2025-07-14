# 🌐 跨平台远程访问配置教程 (OpenSSH + 防火墙 + Tailscale)

> 支持系统：Windows 11 / Ubuntu 22.04+ / macOS Ventura+

---

## 目录

1. 🔧 环境准备
2. 🛠 安装并配置 OpenSSH

   * Windows
   * Ubuntu
   * macOS
3. 🔐 设置 SSH 公私钥
4. 🔥 编写防火墙规则
5. 🌐 通过 Tailscale 设置内网互联
6. ✅ 测试远程连接
7. 🧰 常见问题与故障排查

---

## 1. 🔧 环境准备

需要的工具：

* GitHub 账号
* 经常用的终端工具 (Terminal / PowerShell)
* Tailscale 账号

---

## 2. 🛠 安装并配置 OpenSSH

### 🪠 Windows 11

#### 检查 OpenSSH 是否已安装：

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
```

#### 安装客户端和服务端：

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

#### 启用服务：

```powershell
Start-Service sshd
Set-Service -Name sshd -StartupType 'Automatic'
```

---

### 🤓 Ubuntu

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

---

### 🍏 macOS

默认已内置 SSH 服务，启用：

```bash
sudo systemsetup -setremotelogin on
```

查看服务状态：

```bash
sudo systemsetup -getremotelogin
```

---

## 3. 🔐 生成 SSH 公私钥

适用于全系统：

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

常用文件位置：

* 公钥：`~/.ssh/id_ed25519.pub`
* 私钥：`~/.ssh/id_ed25519`

建议添加到 GitHub 使用免密登陆。

---

## 4. 🔥 防火墙配置

### 🪠 Windows Defender 防火墙

打开端口 22:

```powershell
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Port 22' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

---

### 🤓 Ubuntu UFW

```bash
sudo ufw allow ssh
sudo ufw enable
```

---

## 5. 🌐 Tailscale 内网互联配置

### 📅 安装 Tailscale

官网：[https://tailscale.com/download](https://tailscale.com/download)

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

#### Windows

安装后打开 App 登录即可

### 📡 查看 IP

```bash
tailscale ip
tailscale status
```

---

## 6. ✅ 测试连接

示例：

```bash
ssh username@100.x.x.x
```

---

## 7. 🧰 常见问题与故障排查

* `Permission denied (publickey)`：确保公钥已上传
* 无法连接：检查 22 端口是否已打开
* Tailscale 未启动：试试 `sudo tailscale up` 重启

---

## 👨‍💻 作者

* GitHub：[@你的用户名](https://github.com/your-username)
* 博客/网站：自行填充
