### Formatting and re-partitioning a new drive on Ubuntu
https://www.tecklyfe.com/how-to-partition-format-and-mount-a-disk-on-ubuntu-20-04/

### Setting up automatic disk health checks with email notifications on failure
#### go to Server section
https://help.ubuntu.com/community/Smartmontools

### xRandR auto-connect when HDMI is plugged in on laptop
Before plugged in for the first time:
```sh
autorandr --save mobile
```

When plugged in for the first time (for laptop display eDP-1 and external display HDMI-2)
```sh
xrandr --output eDP-1 --off --output HDMI-2 --auto --primary
autorandr --save docked
```



### Reinstall Android app that you deleted
```sh
adb shell
pm install-existing --user 0 <pkg_name>
```

### Restore mediaserver/nextcloud backup
```sh
sudo su
gpg -d --pinentry-mode=loopback your_archive.tgz.gpg | tar xz
# cd into created directory, find docker-compose file
docker network create shared
docker-compose up -d
```

### Generate an ed25519 SSH key
```sh
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "john@example.com"
```

### Mount an SD card
```sh
sudo fdisk -l   # find sd card device name here
sudo mkdir /mnt/sd
sudo mount <<device_name>> /mnt/sd

# after you're done...

sudo unount /mnt/sd
sudo rm -r /mnt/sd
```
