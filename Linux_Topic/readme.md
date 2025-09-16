# 🔹 Linux Interview Preparation Guide

## 1. **Basic Linux Concepts**

* Difference between Unix, Linux, Windows.
* Linux architecture: **Kernel, Shell, User space**.
* File system hierarchy (`/bin`, `/etc`, `/home`, `/var`, `/usr`, `/tmp` etc).
* File types: Regular, Directory, Block, Character, FIFO, Socket, Links (hard vs soft).

👉 Practice:

```bash
ls -l
file /bin/ls
```

---

## 2. **Essential Commands**

* File/Directory:

  * `ls`, `cd`, `pwd`, `cp`, `mv`, `rm`, `find`, `locate`
* File content:

  * `cat`, `less`, `head`, `tail`, `grep`, `awk`, `sed`
* File permissions:

  * `chmod`, `chown`, `umask`
* Disk/Process:

  * `df`, `du`, `ps`, `top`, `htop`, `kill`, `free`
* Networking:

  * `ping`, `curl`, `wget`, `netstat`, `ss`, `ip`, `traceroute`, `scp`, `rsync`
* Archiving/Compression:

  * `tar`, `gzip`, `bzip2`, `zip/unzip`

👉 Interview Tip: Many interviewers give **small shell problems** like:

```bash
# Find all .log files > 100MB
find /var/log -name "*.log" -size +100M

# Replace 'foo' with 'bar' in a file
sed -i 's/foo/bar/g' file.txt
```

---

## 3. **Users, Groups, and Permissions**

* User management: `useradd`, `usermod`, `passwd`, `id`
* Groups: `groupadd`, `groups`, `gpasswd`
* SUID, SGID, Sticky bit → interview favorite!
* SSH key authentication setup.

👉 Example:

```bash
chmod 4755 script.sh  # SUID
chmod 2775 dir/       # SGID
chmod +t /tmp         # Sticky bit
```

---

## 4. **Processes & Services**

* Foreground vs Background (`&`, `jobs`, `fg`, `bg`).
* `systemd`, `systemctl`, `journalctl`.
* Runlevels / Targets.
* Crontab (`crontab -e`, `/etc/cron.*`).

👉 Sample Q:

* How to restart a service if it crashes?
  → Use `systemctl restart <service>` or set `Restart=always` in systemd unit file.

---

## 5. **Networking**

* IP config: `ip addr`, `ifconfig`, `nmcli`.
* Check open ports: `netstat -tulnp`, `ss -tulnp`.
* Firewall basics: `iptables`, `firewalld`, `ufw`.
* DNS: `/etc/hosts`, `/etc/resolv.conf`, `dig`, `nslookup`.

👉 Example Q:

* How to check if port 8080 is listening?
  → `ss -tulnp | grep 8080`

---

## 6. **Storage & File Systems**

* Mounting/Unmounting: `mount`, `umount`, `/etc/fstab`.
* Disk usage: `df -h`, `du -sh`.
* Partitioning: `lsblk`, `fdisk`, `parted`.
* LVM basics: `pvcreate`, `vgcreate`, `lvcreate`.
* Swap memory management.

---

## 7. **Logs & Troubleshooting**

* Log files: `/var/log/messages`, `/var/log/syslog`, `/var/log/secure`, `/var/log/dmesg`.
* Troubleshooting boot issues, service failures.
* Checking performance: `iostat`, `vmstat`, `sar`.

👉 Example Q:

* How to check which process is using the most memory?
  → `top` or `ps aux --sort=-%mem | head`

---

## 8. **Shell Scripting**

* Variables, loops, conditions.
* Redirections: `>`, `>>`, `2>`, `&>`.
* Pipes (`|`).
* Write a script to monitor disk space and send an alert.

👉 Example:

```bash
#!/bin/bash
threshold=80
usage=$(df / | tail -1 | awk '{print $5}' | tr -d '%')
if [ $usage -gt $threshold ]; then
  echo "Disk usage high: $usage%"
fi
```

---

## 9. **Security**

* File permissions, ACLs (`getfacl`, `setfacl`).
* SSH security (`sshd_config`).
* Firewalls (`ufw`, `iptables`).
* Fail2ban, SELinux/AppArmor basics.

---

## 10. **Interview-Style Scenarios**

* A server is slow — how will you debug? (CPU, RAM, Disk, Network checks).
* Service is not starting — steps to debug (`systemctl status`, logs).
* Disk full — how to identify the culprit (`du -sh *`).
* User cannot SSH — what to check (`/etc/passwd`, `/etc/ssh/sshd_config`, permissions).

---

✅ **Pro Tips for Interview:**

* Always explain **“why” not just “how”**.
* If unsure, tell interviewer your **approach to troubleshoot**.
* Practice on a real Linux VM (Ubuntu/Debian/CentOS).

---
# 🔹 Linux Interview Q\&A Cheat Sheet

## 1. Basics

**Q1. What is the difference between Linux and Unix?**
👉 Unix is proprietary; Linux is open-source Unix-like OS. Linux runs on many platforms, Unix is vendor-specific.

**Q2. What is the Linux kernel?**
👉 Core of the OS; manages hardware, memory, processes, security, networking.

**Q3. What are runlevels/targets in Linux?**
👉 Defines system state. Example: `multi-user.target` (no GUI), `graphical.target` (GUI).

---

## 2. File System & Permissions

**Q4. What are hard links and soft links?**
👉 Hard link = points to inode (same file, different name).
👉 Soft link = shortcut (like Windows shortcut).

**Q5. Difference between absolute and relative path?**
👉 Absolute starts from `/`, relative depends on current directory.

**Q6. What is SUID, SGID, and Sticky bit?**
👉 SUID → run with file owner’s permissions.
👉 SGID → run with group’s permissions.
👉 Sticky → only file owner can delete inside dir (e.g., `/tmp`).

---

## 3. User Management

**Q7. How to check current logged-in users?**
👉 `who`, `w`, `users`.

**Q8. How to add a user with home directory and default shell?**
👉 `useradd -m -s /bin/bash username`.

**Q9. How to switch user?**
👉 `su - user` or `sudo -i -u user`.

---

## 4. Processes & Services

**Q10. How to check running processes?**
👉 `ps aux`, `top`, `htop`.

**Q11. How to kill a process?**
👉 `kill -9 <pid>`.

**Q12. How to restart a service in systemd?**
👉 `systemctl restart nginx`.

---

## 5. Networking

**Q13. How to check system IP address?**
👉 `ip addr show`, `ifconfig`.

**Q14. How to check open ports?**
👉 `ss -tulnp`, `netstat -tulnp`.

**Q15. How to test network connectivity?**
👉 `ping`, `curl`, `traceroute`.

---

## 6. Disk & File Systems

**Q16. How to check disk usage?**
👉 `df -h`.

**Q17. How to find biggest files in a directory?**
👉 `du -ah / | sort -rh | head -n 10`.

**Q18. How to mount a filesystem?**
👉 `mount /dev/sdb1 /mnt`.

---

## 7. Logs & Monitoring

**Q19. Where are Linux logs stored?**
👉 `/var/log/` (e.g., `syslog`, `messages`, `secure`).

**Q20. How to check last 100 lines of a log file?**
👉 `tail -n 100 /var/log/syslog`.

**Q21. How to monitor CPU and memory usage?**
👉 `top`, `free -h`, `vmstat`.

---

## 8. Package Management

**Q22. How to install a package in Ubuntu/Debian?**
👉 `apt install package`.

**Q23. How to install in CentOS/RHEL?**
👉 `yum install package` or `dnf install package`.

**Q24. How to check package details?**
👉 `dpkg -l`, `rpm -qa`.

---

## 9. Shell Scripting

**Q25. How to redirect output of a command to a file?**
👉 `ls > output.txt`.

**Q26. How to append output to file?**
👉 `ls >> output.txt`.

**Q27. Write a script to print numbers 1 to 5.**

```bash
for i in {1..5}; do echo $i; done
```

---

## 10. Security

**Q28. How to change file ownership?**
👉 `chown user:group file`.

**Q29. How to set read-only permission for all?**
👉 `chmod 444 file`.

**Q30. How to check SELinux status?**
👉 `sestatus`.

---

## 11. Advanced Topics

**Q31. What is LVM in Linux?**
👉 Logical Volume Manager, allows resizing partitions dynamically.

**Q32. How to create a new partition?**
👉 `fdisk /dev/sdb`.

**Q33. How to add swap space?**
👉 `mkswap /swapfile && swapon /swapfile`.

---

## 12. Interview Scenarios

**Q34. Disk is full, how do you troubleshoot?**
👉 `df -h`, then `du -sh *` inside suspected directory.

**Q35. Service is down, how do you debug?**
👉 `systemctl status <service>`, check logs in `/var/log/`.

**Q36. User cannot SSH into server. What do you check?**
👉 User exists? SSH port open? Permissions on `~/.ssh/authorized_keys`.

**Q37. High CPU usage – how to check?**
👉 `top`, `ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head`.

**Q38. Website is not loading – steps?**
👉 Check `ping`, `curl`, firewall, `ss -tulnp`.

**Q39. File deleted but disk still full – why?**
👉 Process still holding file. Check `lsof | grep deleted`.

**Q40. How to schedule a script to run daily?**
👉 Add entry in `crontab -e`:

```
0 2 * * * /path/script.sh
```

---

## 13. Quickfire (Common Rapid Qs)

**Q41. How to check OS version?** → `cat /etc/os-release`.
**Q42. How to check kernel version?** → `uname -r`.
**Q43. How to reboot system?** → `reboot` or `shutdown -r now`.
**Q44. How to list hidden files?** → `ls -a`.
**Q45. How to find a word inside files?** → `grep -r "word" /path`.
**Q46. How to compare two files?** → `diff file1 file2`.
**Q47. How to count lines in a file?** → `wc -l file.txt`.
**Q48. How to list all environment variables?** → `env`.
**Q49. How to make a script executable?** → `chmod +x script.sh`.
**Q50. How to check system uptime?** → `uptime`.

---

✅ This set of **50 Q\&A** will cover **80–90% of Linux interview rounds**.

Do you want me to also prepare a **hands-on Linux lab practice checklist** (like tasks to try in a VM so you’ll be ready for practical rounds)?
