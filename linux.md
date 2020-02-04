# Linux
- sudo command
- df [-h] *..disk information*
- yum install htop
- updates CentOS, check-update
- updates Ubuntu
  - apt update, apt upgrade, [apt full-upgrade]
  - apt autoremove (removes unused packages, cleans boot partition)
  - systemctl restart apache2
- reboot -h now
- systemctl start/restart/status/stop/enable mariadb/waagent
- tab+tab auto completition
- midnight commander - Norton like Commander
- ls -la list details
- ip addr show
- sudo -s (on Azure Ubuntu to switch to root)
- putty - Shift + Insert - Paste text
- swapon -s check swap file, if it doesn't exist, the waagent.conf might have been replaced
  - ResourceDisk.Format=y
  - ResourceDisk.EnableSwap=y
  - ResourceDisk.SwapSizeMB=1024

## Vi Editor
 - sudo vi /var/log/mariadb/mariadb.log 
 - G - end of file
 - :q! - type to exit type
 - ZZ - exit & save
 - x - delete selected character
 - dd - delete line
 
 ## Nano editor
 - nano
 
## cron
```30 1 * * * root /bin/systemctl stop httpd.service && (/opt/letsencrypt/letsencrypt-auto renew | tee -a /var/log/letsencrypt-renew.log) && /bin/systemctl start httpd.service```
