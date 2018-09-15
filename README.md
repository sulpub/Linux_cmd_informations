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

# Main Issues for my specific problems
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
