# Linux_cmd_informations
Information on Linux command

# File actions
```
ls -als           // list all files
mkdir titi        // make directory "titi"
rmdir -rf  titi   // remove all file on the "titi" directory
cd                // Go to your home directory
cd                // Go to the upper directory
```

# update linux with root profile
```
sudo apt-get update       //update all the package on your distribution
sudo atp-get upgrade      //upgrade all package installed on your distribution
sudo apt-get install gimp //exemple to install gimp software for example you can replace gimp by other software....
sudo apt-get remove gimp  //remove gimp software for example. You can replace gimp by other software for removing....
```

# network information
```
iwconfig //see status of wireless card.
ifconfig //see adress ip of each network card.

sudo iwlist scan  //scan les connexion wifi

//arret demmarage carte wifi
sudo ifconfig wlp5s0 up    //connexion de la carte wlp5s0
sudo ifconfig wlp5s0 down  //deconnexion de la carte wlp5s0
```

# network configuration example
```
sudo nano /etc/network/interfaces //edit the network conf file.
config example
STATIC
 iface wlp5s0 inet static
 address 192.168.1.10
 netmask 255.255.255.0
 gateway 192.168.1.254
 wpa-psk "replace by your wpa password securit"
 wpa-driver wext
 wpa-key-mgmt WPA-PSK
 wpa-proto WPA2
 wpa-ssid "replace by your SSID"
 auto wlp5s0

DYNAMIC
 iface wlp5s0 inet dhcp
 auto wlp5s0

After saving file run these commands :
 sudo service networking force-reload
 sudo service networking restart
 sudo iwconfig  //for controlling if your connexion was OK
```
# Disable power management wifi

This procedure is to allow wifi to stay permanently on.

```
To disable the Wi-Fi power management at boot time, follow these steps:

Create a file in the /lib/systemd/system directory named wifi-power-management-off.service with the following content:

[Unit]
Description=Disable power management for wlan0
Requires=sys-subsystem-net-devices-wlan0.device
After=network.target

[Service]
Type=oneshot
ExecStart=/sbin/iwconfig wlan0 power off

[Install]
WantedBy=multi-user.target

Type the following command:
# systemctl enable wifi-power-management-off.service

To verify that the service is functioning properly, reboot the board and type the following command:
# iwconfig wlan0

You should see Power Management: off on the screen.

Power Management: off

To disable the service type the following command:

# systemctl disable wifi-power-management-off.service

Source : https://www.debian-fr.org/t/wifi-se-mettre-en-power-off-au-demarrage/71825
```

if don't work you can test this:

```
Open this file with your favorite text editor, I use nano here:
sudo nano /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf

By default there is:
[connection]
wifi.powersave = 3

Change the value to 2. Reboot for the change to take effect.

Possible values for the wifi.powersave field are:
 NM_SETTING_WIRELESS_POWERSAVE_DEFAULT (0): use the default value
 NM_SETTING_WIRELESS_POWERSAVE_IGNORE  (1): don't touch existing setting
 NM_SETTING_WIRELESS_POWERSAVE_DISABLE (2): disable powersave
 NM_SETTING_WIRELESS_POWERSAVE_ENABLE  (3): enable powersave
```

# dns configuration
```
sudo nano /etc/resolv.conf
add dns as well :
 nameserver 212.27.40.240
```

# Main Issues for my specifics problems
## ISSUE 1 disconnect wifi card intel PRO wireless 3945ABG on LUBUNTU
```
sudo rmmod iwl3945     //Software disconnect the wifi board 
sudo modprobe iwl3945  //Software reconnect the wifi board
```

## ISSUE 2 connect on windows remote desktop
```
install rdesktop
  sudo apt-get install rdesktop
Run rdesktop with rdp to IP.IP.IP.IP adress with screen size 1440x900 with 16bits color and sound stay on the distant computer.
  rdesktop -r sound:remote -k fr -g 1440x900 -a 16 IP.IP.IP.IP
  rdesktop -k fr -a 8 IP.IP.IP.IP  //color 8bit full screen keyboard FR
```

## ISSUE 3 ssh connexion
```
ssh user@IP.IP.IP.IP //run ssh connexion with user "user" at adress IP.IP.IP.IP
ssh -X user@IP.IP.IP.IP //run ssh with server X. 
```
## ISSUE 4 test network bandwith
```
nc -lk 2112 >/dev/null  //run on the remote computer 10.10.10.1
dd if=/dev/zero bs=16000 count=625 | nc -v 10.10.10.1 2112  //transfert 10MB to the remote computer and evaluate the bandwith.

Response example on the local computer :
  Connection to 10.10.10.1 2112 port [tcp/*] succeeded!
  625+0 enregistrements lus
  625+0 enregistrements écrits
  10000000 bytes (10 MB, 9,5 MiB) copied, 4,08256 s, 2,4 MB/s
```
## ISSUE 5 : Hard drive informations
```
sudo blkid  //To know informations concerning hard drive.

sudo hdparm -C  /dev/sda      //See the hard drive status
sudo hdparm -y  /dev/sda      //Put the hard drive sda in standby mode

sudo hdparm -S 1 /dev/sda     //le chiffre 1 correspond au temps par multiples de 5 secondes. 
sudo hdparm -S 120 /dev/sda1  //put the disk in standby mode after 120*5s=10 minutes

sudo hdparm -I /dev/sda | grep level    //To verify if the hard drive support low power management. If the disk support low power you will see 254 by default.

```

## ISSUE 6 : Delete shell history
```
Delete specific line
 history -d 505   // delete ligne 505 in the istory command
or edit the local bash history 
 nano ~/.bash_history 
```

## ISSUE 7 : Crontab 

Edit the crontab with this command

**crontab -e**

```
add line for each automated task

mm hh jj MMM JJJ [user] task > log

    mm : minutes (00-59).
    hh : hours (00-23) .
    jj : day of the month (01-31).
    MMM : months (01-12 or three letters : jan, feb, mar, apr, may, jun, jul, aug, sep, oct, nov, dec).
    JJJ : day of the week (1-7 or three letters : mon, tue, wed, thu, fri, sat, sun).
    user (optional) : user name for execute the task.
    task : command to execute.
    > log (optional) : log output to log file. If no log file is specify, an email will be send to the local user.

For each unit use this syntax :

 1. 1-5 : Unit time from 1 to 5.
 2. */6 : reapeat at each 6 unit of time (example for each 6 hour).
 3. 2,7 : time unit 2 and 7.
``` 
**Examples:**
 
Run script.sh at each minute : */1 * * * * /home/user/script.sh > /dev/null 2>&1

Run script1.sh at each 6 hours :  00 */6 * * * /home/user/script1.sh

Run ntpdate at each hours for update the date machine :  00 */1 * * * /usr/sbin/ntpdate fr.pool.ntp.org

## ISSUE 8 : ADD HTACCESS in APACHE

For activate the htaccess support in Apache

edit the apache2.conf file :
``` 
sudo nano /etc/apache2/apache2.conf
``` 
and replace these line

**AllowOverride None**

By

**AllowOverride All**

You can use the nana function ^W for finding these lines
After these changes save the modifications.

Restart the webserver
``` 
sudo service apache2 restart
``` 


## ISSUE 9 : LOG ANALYSIS

install **petit** software [LINK](http://crunchtools.com/software/petit/)

more informations : [tecmint.com](https://www.tecmint.com/petiti-log-analysis-tool-for-linux-sysadmins/)

``` 
sudo apt-get install petit
``` 

for analysis run these command

``` 
# hash log file
petit --hash --fingerprint /var/log/messages

# Finding Number Of Lines Produced by a Daemon
petit --hash --daemon /var/log/syslog

#Graphing a Log File
petit --sgraph /var/log/syslog

#Tracking Particular Words in a Log File
cat /var/log/messages | grep error | petit --mgraph

``` 
