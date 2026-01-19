# Ubuntu Developer Cheatsheet

Useful Ubuntu OS commands, workflows, and tips for software developers.

---

## Assumptions

- OS: Ubuntu 22.04+ (tested on 24.04 LTS)
- Shell: bash
- User has sudo access
- Target audience: developers using CLI daily

---

## Table of Contents

1. System Information  
2. Package Management  
3. File & Directory Operations  
4. Networking  
5. Processes & Performance  
6. Development Environment  
7. Docker  
8. Git  
9. Archives & Compression  
10. Disk & Storage  
11. Permissions & Ownership  
12. Debugging & Troubleshooting  
13. Productivity Tips  

---

## 1. System Information

Check Ubuntu version  
lsb_release -a  

Kernel and architecture  
uname -a  

System load and uptime  
uptime  

Current user  
whoami  

Host details  
hostnamectl  

Reboot / shutdown  
sudo reboot  
sudo shutdown now  

---

## 2. Package Management (APT)

Update package index  
sudo apt update  

Upgrade system  
sudo apt upgrade -y  

Install packages  
sudo apt install curl git -y  

Remove packages  
sudo apt remove package-name  
sudo apt purge package-name  

Search packages  
apt search nginx  
apt show nginx  

Fix broken packages  
sudo apt --fix-broken install  

---

## 3. File & Directory Operations

Current directory  
pwd  

List files (human-readable)  
ls -lah  

Change directory  
cd ~/projects  

Create nested directories  
mkdir -p a/b/c  

Delete files or folders  
rm file.txt  
rm -rf folder  

Find files  
find . -name "*.js"  

Disk usage  
du -sh *  
df -h  

---

## 4. Networking

List network interfaces  
ip a  

Routing table  
ip route  

Check connectivity  
ping google.com  

Public IP  
curl ifconfig.me  

Open ports  
ss -tulpn  
lsof -i :3000  

---

## 5. Processes & Performance

Running processes  
top  
htop  

Memory usage  
free -h  

CPU cores  
nproc  

Kill process  
kill PID  
kill -9 PID  
pkill node  

---

## 6. Development Environment

Build tools  
sudo apt install build-essential -y  

Node.js via NVM  
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash  
nvm install --lts  

Python  
sudo apt install python3 python3-pip python3-venv -y  

Java  
sudo apt install openjdk-17-jdk -y  

---

## 7. Docker

Install Docker  
sudo apt install docker.io docker-compose -y  
sudo usermod -aG docker $USER  

Verify  
docker --version  
docker ps  

Cleanup  
docker system prune -af  

---

## 8. Git

Global config  
git config --global user.name "Your Name"  
git config --global user.email "you@email.com"  

Common commands  
git status  
git pull  
git push  
git checkout -b feature-x  
git log --oneline --graph  

Fix wrong author  
git commit --amend --reset-author  

---

## 9. Archives & Compression

Create tar.gz  
tar -czvf file.tar.gz folder  

Extract tar.gz  
tar -xzvf file.tar.gz  

Zip / unzip  
zip -r file.zip folder  
unzip file.zip  

---

## 10. Disk & Storage

List disks  
lsblk  

Mount disk  
sudo mount /dev/sdb1 /mnt  

Disk health (NVMe/SATA)  
sudo smartctl -a /dev/nvme0n1  

---

## 11. Permissions & Ownership

Make executable  
chmod +x script.sh  

Set permissions recursively  
chmod -R 755 folder  

Change owner  
chown user:user file  

Fix dev folder permissions  
sudo chown -R $USER:$USER .  

---

## 12. Debugging & Troubleshooting

System logs  
journalctl -xe  

Service logs  
journalctl -u docker  

Service management  
systemctl status nginx  
systemctl restart nginx  

Kill process using a port  
sudo fuser -k 3000/tcp  

---

## 13. Productivity Tips

Common aliases  
alias gs="git status"  
alias ll="ls -lah"  

Reload shell  
source ~/.bashrc  

Clipboard (X11)  
xclip -sel clip < file.txt  

Watch command output  
watch -n 1 free -h  

---

## Common Failure Modes

- command not found → package not installed or PATH issue  
- permission denied → missing execute bit or wrong ownership  
- docker permission denied → user not in docker group (log out/in)  
- apt locked → another apt process running  

---

## Verification Checklist

- sudo apt update runs clean  
- docker ps works without sudo  
- git --version returns expected output  
- node -v, python3 --version, java -version valid  

---

## License

MIT
