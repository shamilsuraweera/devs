# Windows 10 Pro — Developer & Business Admin Commands and Tips

## Assumptions
- OS: Windows 10 Pro (22H2)
- Shells: PowerShell 5.1+, Windows Terminal
- Privileges: Standard user + Admin when stated
- Tools optionally installed: WSL2, Docker Desktop, Git, VS Code

---

## 1. Core Shortcuts (Productivity)
- Win + X → Admin tools menu  
- Win + R → Run dialog  
- Win + E → File Explorer  
- Win + L → Lock  
- Win + V → Clipboard history  
- Alt + Tab → App switch  
- Ctrl + Shift + Esc → Task Manager  

---

## 2. PowerShell Basics (Admin when noted)
Check version  
$PSVersionTable.PSVersion

List execution policy  
Get-ExecutionPolicy -List

Set execution policy (Admin)  
Set-ExecutionPolicy RemoteSigned -Scope LocalMachine

List services  
Get-Service

Start/Stop service  
Start-Service Name  
Stop-Service Name

List processes  
Get-Process

Kill process  
Stop-Process -Name processname -Force

---

## 3. File & Disk Management
List disks (Admin)  
Get-Disk

List partitions  
Get-Partition

Disk cleanup  
cleanmgr

Check disk errors (Admin, reboot required)  
chkdsk C: /f

Format drive (Admin)  
format D: /fs:NTFS /q

---

## 4. Networking & Troubleshooting
IP configuration  
ipconfig /all

Flush DNS  
ipconfig /flushdns

Ping  
ping google.com

Trace route  
tracert google.com

Open ports & listeners  
netstat -ano

Firewall rules (Admin)  
Get-NetFirewallRule  
New-NetFirewallRule -DisplayName "App Port" -Direction Inbound -Protocol TCP -LocalPort 8080 -Action Allow

---

## 5. Package Management (winget)
Search package  
winget search nodejs

Install  
winget install Git.Git  
winget install Microsoft.VisualStudioCode

Upgrade all  
winget upgrade --all

List installed  
winget list

---

## 6. WSL2 (Linux on Windows)
List distros  
wsl -l -v

Install Ubuntu  
wsl --install -d Ubuntu

Set default version  
wsl --set-default-version 2

Shutdown WSL  
wsl --shutdown

Access Windows files from WSL  
/mnt/c/Users/YourUser

---

## 7. Docker Desktop (WSL2 Backend)
Check status  
docker info

Run container  
docker run -it ubuntu bash

List containers  
docker ps -a

Remove unused data  
docker system prune -a

---

## 8. Environment Variables
List user variables  
Get-ChildItem Env:

Set user variable  
setx JAVA_HOME "C:\Java\jdk17"

Refresh current shell  
$env:JAVA_HOME="C:\Java\jdk17"

---

## 9. Task Manager & Performance
Open advanced view  
Ctrl + Shift + Esc → More details

Startup apps  
Task Manager → Startup

Resource Monitor  
resmon

Performance Monitor  
perfmon

---

## 10. System Configuration
System info  
msinfo32

Services manager  
services.msc

Local users & groups (Pro only)  
lusrmgr.msc

Group Policy Editor (Pro only)  
gpedit.msc

---

## 11. Security & Updates
Windows Defender status  
Get-MpComputerStatus

Quick scan  
Start-MpScan -ScanType QuickScan

Force Windows Update check  
wuauclt /detectnow /updatenow

---

## 12. Remote & Admin Tools
Remote Desktop  
mstsc

Computer Management  
compmgmt.msc

Event Viewer  
eventvwr.msc

---

## 13. Backup & Recovery
File History  
control /name Microsoft.FileHistory

System Restore  
rstrui

Create recovery drive  
recoverydrive

---

## 14. Common Failure Modes & Fixes
- Script blocked → Set execution policy to RemoteSigned
- WSL slow → Ensure virtualization enabled in BIOS
- Docker not starting → Restart WSL + Docker Desktop
- Port in use → netstat -ano → kill PID
- winget broken → Update App Installer from Microsoft Store

---

## 15. Verification Checklist
- PowerShell runs scripts without error
- winget installs packages
- WSL2 shows Version 2
- Docker runs containers
- Firewall rules applied and visible

---

## 16. Recommended Defaults
- Use Windows Terminal
- Enable clipboard history
- Enable Hyper-V + Virtual Machine Platform
- Keep OS and drivers updated
- Use standard user account; elevate only when required
