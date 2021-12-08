### Table of Contents
- [Linux](#linux)
  - [Formatting and Re-Partitioning Drive on Ubuntu](#formatting-and-re-partitioning-drive-on-ubuntu)
  - [Auto-connect to Monitor when HDMI Plugged In](#auto-connect-to-monitor-when-hdmi-plugged-in)
  - [Mount SD Card](#mount-sd-card)
- [Security](#security)
  - [Generate Ed25519 SSH Key](#generate-ed25519-ssh-key)
- [Homelab](#homelab)
  - [Setting up Automatic Disk Health Checks](#setting-up-automatic-disk-health-checks)
  - [Restore Mediaserver/Nextcloud Backup](#restore-mediaservernextcloud-backup)
- [Misc](#misc)
  - [Reinstall Deleted Android System App](#reinstall-deleted-android-system-app)

<br/>
<br/>
<br/>

<!----------------------------------------------------------------------------->

# Linux

## Formatting and Re-Partitioning Drive on Ubuntu
https://www.tecklyfe.com/how-to-partition-format-and-mount-a-disk-on-ubuntu-20-04/

## Auto-connect to Monitor when HDMI Plugged In
Before plugged in for the first time:
```sh
autorandr --save mobile
```

When plugged in for the first time (for laptop display eDP-1 and external display HDMI-2)
```sh
xrandr --output eDP-1 --off --output HDMI-2 --auto --primary
autorandr --save docked
```

## Mount SD Card
```sh
sudo fdisk -l   # find sd card device name here
sudo mkdir /mnt/sd
sudo mount <<device_name>> /mnt/sd

# after you're done...

sudo unount /mnt/sd
sudo rm -r /mnt/sd
```

<!----------------------------------------------------------------------------->

# Security

## Generate Ed25519 SSH Key
```sh
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "john@example.com"
```

<!----------------------------------------------------------------------------->

# Homelab

## Setting up Automatic Disk Health Checks
https://help.ubuntu.com/community/Smartmontools#Server

## Restore Mediaserver/Nextcloud Backup
```sh
sudo su
gpg -d --pinentry-mode=loopback your_archive.tgz.gpg | tar xz
# cd into created directory, find docker-compose file
docker network create shared
docker-compose up -d

# for nextcloud only
docker exec -it nextcloud php occ preview:generate-all -vvv
docker exec -it nextcloud php occ files:scan --all
```

<!----------------------------------------------------------------------------->

# Misc

## Reinstall Deleted Android System App
```sh
adb shell
pm install-existing --user 0 <pkg_name>
```
