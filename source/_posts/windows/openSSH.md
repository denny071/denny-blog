---
title: openSSH
categories: windows
date: 2021-12-07 10:30:02
tags: openSSH
---

下载地址：**https://github.com/PowerShell/Win32-OpenSSH/releases**


   ![OpenSSH-Win32.zip](openSSH/OpenSSH-Win32.zip.png)



下载之后解压，打开cmd，进入解压目录，执行安装服务命令

**powershell.exe -ExecutionPolicy Bypass -File install-sshd.ps1**



打开防火墙过滤

**New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22**



此命令是windows 2012（含）以上的服务器使用，如果较低版本server

**netsh advfirewall firewall add rule name=sshd dir=in action=allow protocol=TCP localport=22**



启动服务：

**net start sshd**
