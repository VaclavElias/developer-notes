# Linux

## wsl

- wsl --install Ubuntu-26.04
- sudo apt update && sudo apt upgrade -y
- sudo apt install -y dotnet-sdk-10.0
- wsl --shutdown
- wsl --export Ubuntu-26.04 D:\Data\ubuntu-26-dotnet10.tar
- wsl --unregister Ubuntu-26.04
- wsl --import Ubuntu-26.04 C:\WSL\Ubuntu-26.04 D:\Data\ubuntu-26-dotnet10.tar

## stride.cli

- dotnet tool install stride.cli
- dotnet stride sdk install 4.4.0-beta6
- dotnet stride new stride-game -n mygame01
- cd mygame01
- dotnet build
- dotnet run --project mygame01.Linux/mygame01.Linux.csprojc

## issues
- copy to .nuget\packages\stride.assetcompiler\4.4.0-beta6\tools\net10.0\
- Stride.Physics: libbulletc -> libbulletc.so
- Stride.Assets: stride_vhacd -> stride_vhacd.so
- Stride.Graphics: freetype -> libfreetype.so
- Stride.TextureConverter: libastcenc.so
- Stride.TextureConverter: stride_directxtex -> stride_directxtex.so
    - sudo apt install libgomp1
    - document libgomp1 as a Linux prerequisite
      
## Other

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
  - /etc/waagent.conf
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
