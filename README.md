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

## Docker takes a long time to start at boot
[Solution: remove the dependency on network-online.target for docker.service](https://superuser.com/questions/1356698/docker-service-takes-1-minute-and-30-seconds-causing-slow-boot)
`sudo vim /lib/systemd/system/docker.service`

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

## Set up NUT (Network UPS Tools)
https://wiki.archlinux.org/title/Network_UPS_Tools

## Set up remote desktop with XRDP
https://wiki.archlinux.org/title/Xrdp

## Disable built-in bluetooth receiver so that bluetooth dongle takes priority
To disable the built-in Bluetooth receiver on your motherboard and ensure that your Linux system always uses the Bluetooth dongle, you can use the `udev` rules to control device behavior based on specific criteria, such as the device's hardware address (MAC address). Here are the steps to achieve this:

1. Identify the MAC address of your built-in Bluetooth receiver:

   You can use the `hcitool` command to list the available Bluetooth devices and their MAC addresses:

   ```
   hcitool dev
   ```

   Note down the MAC address of your built-in receiver. It should be listed alongside the device name.

2. Create a `udev` rule:

   You'll need to create a `udev` rule that will prevent the built-in Bluetooth device from loading its driver when it's connected.

   Open a terminal and create a new `udev` rule file using your preferred text editor. For example:

   ```
   sudo nano /etc/udev/rules.d/99-disable-built-in-bluetooth.rules
   ```

   In this file, add the following rule. Replace `XX:XX:XX:XX:XX:XX` with the MAC address of your built-in Bluetooth receiver:

   ```
   SUBSYSTEM=="usb", ATTRS{idVendor}=="1d6b", ATTRS{idProduct}=="024e", ATTR{product}=="Intel(R) Wireless Bluetooth(R)", ATTR{serial}=="XX:XX:XX:XX:XX:XX", RUN+="/bin/sh -c 'echo -n $kernel >/sys/bus/usb/drivers/usb/unbind'"
   ```

   Save the file and exit the text editor.

3. Reload `udev` rules:

   To apply the new `udev` rule, reload the `udev` rules with the following command:

   ```
   sudo udevadm control --reload-rules && sudo udevadm trigger
   ```

4. Disconnect and reconnect your built-in Bluetooth receiver:

   You may need to disconnect and reconnect the built-in Bluetooth receiver physically or use a system reboot to ensure the rule takes effect.

After completing these steps, your Linux system should prioritize the Bluetooth dongle over the built-in receiver, as the rule will prevent the built-in Bluetooth device from loading its driver. This should ensure that your system always chooses the dongle when you connect it.


<!----------------------------------------------------------------------------->

# Misc

## Reinstall Deleted Android System App
```sh
adb shell
pm install-existing --user 0 <pkg_name>
```
