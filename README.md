# 🌐 跨平台远程访问配置教程 (OpenSSH + 防火墙 + Tailscale)

> 支持系统：Windows 11 / Ubuntu 24.04+ / MacOS M系列

---

## 目录

1. 🔧 环境准备
2. 🛠 安装并配置 OpenSSH

   * Windows
   * Ubuntu
   * macOS
3. 🔥 编写防火墙规则
4. 🌐 通过 Tailscale 设置内网互联
5. ✅ 测试远程连接
6. 🧰 常见问题与故障排查

---

## 1. 🔧 环境准备

需要的工具：

* GitHub 账号(或者其他账号用于Tailscale登陆)
* 经常用的终端工具 (Terminal / PowerShell / zsh)

---

## 2. 🛠 安装并配置 OpenSSH

### 🪠 Windows 11

#### 检查 OpenSSH 是否已安装(如果不确定是否安装)：

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
#如果需要设置开机自启
Set-Service -Name sshd -StartupType 'Automatic'
```

---

### 🤓 Ubuntu

```bash
sudo apt update
sudo apt install openssh-server
#设置开机自启
sudo systemctl enable ssh
sudo systemctl start ssh
#手动启动
sudo service ssh start
```

---

### 🍏 macOS

⚠️MacOS一般自带Openssh服务，可以使用
```bash
which ssh
#或者
ssh -V
```

使用Homebrew 安装Open-ssh service

⚠️安装Homebrew(如果已安装请跳过)
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

```bash
brew install openssh
```

---

## 3. 🔥 防火墙配置

### 🪠 Windows Defender 防火墙（未完善）

打开端口 22:(通常默认端口为22)

```powershell
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Port 22' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```
当然也可以通过图形化界面自行配置防火墙规则
---

### 🤓 Ubuntu UFW

默认端口22

```bash
#允许防火墙通过ssh协议
sudo ufw allow ssh
#启用防火墙
sudo ufw enable
```

---

### 🔒 macOS 防火墙说明

macOS 默认允许 SSH 远程连接，无需额外配置防火墙。

但如果你开启了系统防火墙或使用第三方防火墙，请确认允许 `sshd` 服务接收入站连接：

```bash
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --add /usr/sbin/sshd
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --unblockapp /usr/sbin/sshd
```

---


## 4. 🌐 Tailscale 内网互联配置

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

---

### 💡替换方法
进入官网使用.pkg方式下载


#### Windows
🤔使用官网的安装包下载即可

### 📡 查看 IP

```bash
#查看tailscale分配的ip
tailscale ip
#查看当前tailscale账户下连接的设备
tailscale status
```

---

## 5. ✅ 测试连接

示例：

```bash
#输出ip
tailscale status
#尝试ping通ip
ping ip
#ssh连接
ssh 用户名@ip
```

---

## 6. 🧰 常见问题与故障排查

### Linux篇

输入
```bash
tailscale status
```
显示
```bash
100.0.0.0    user-ubuntu          user@ linux   -
```
并且ssh无法连接的情况
可以尝试输入
```bash
ping ip
```
然后返回重新查看。若返回
```bash
100.0.0.0    user-ubuntu          user@ linux   active
```
则可以正常开始ssh连接

### MacOS篇

### Window篇

---

## 👨‍💻 作者

* GitHub：[@Alex-Lee-morning](https://github.com/Alex-Lee-morning)
