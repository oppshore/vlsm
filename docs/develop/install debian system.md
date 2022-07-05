this article to help to install a `debian` system from zero to one



# prepare debian system iso file

if you don't have a hardware server, you can install the pc virtual software, such as VMware workstation

download the latest `debian` system iso file, notice that, the DVD offline way is very suitable rather than `netinst`

often, the website in there https://cdimage.debian.org/debian-cd/current/amd64/iso-dvd/ , you can visit it and download it



# create virtual machain on vmware workstation

on the VMware workstation

create a new virtual machine

select typical

select iso, the choose someone

type virtual machine name, such as k8s_1

type location

type disk size, choose save type to one

click customize hardware

adjust this virtual machine' memory, such as 4096, adjust process configure, adjust process number to 2



tips:

if you cannot start virtual machine, and your system is windows 11 pro n, you can add the media pack for deal with this problem





# install debian system

on the `debian` install view

type enter to choose `Graphical install` 

click `continue` six times

type `Root password`, such as  `oopshorer`, and click continue

type `new user name`, such as `oppshorer`, and click continue

type `new user password`, such as `oppshorer`, and click continue

click continue to choose Eastern

click continue to choose `Guided - use entire disk` 

click continue to choose `SCSI3 ...`

click continue to choose `All files in one partition`

click continue to choose `Finish partitioning and write changes to disk`

change to default value on `write the change to disks` to yes



uncheck `Debian desktop environment`/`... GNOME`

check `SSH server` and click continue





on the server login view

type username: root, password: `oppshorer`

type ifconfig to look at `ip` for remote server login







we need be some initialize for login/disk/network

for login

```
# allow outer environment to login
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
systemctl restart sshd
```

for network

```
sed -i '/dhcp/d' /etc/network/interfaces
cat >> /etc/network/interfaces<<EOF
auto ens33
iface ens33 inet static
address 192.168.128.143
netmask 255.255.255.0
gateway 192.168.128.2
ns-nameservers 192.168.128.2 192.168.128.2
EOF
cat /etc/network/interfaces
/etc/init.d/networking restart
/etc/init.d/networking status

reboot
```

for disk

```
# delete swap
sed -i '/swap/d'  /etc/fstab
sed -i '/media/d'  /etc/fstab
swapon --show
```

