## Ubuntu nfs configurantion. (central.inova.pt)
First we need to install the nfs package:
```
apt install nfs-kernel-server
```
After installing the package we are going to a directory to export, in this case will be a directory from the [raid5](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/Debian/Raid5.md) and it will be the /mnt/userinfo
Then `` nano /etc/exports `` and add
```
/mnt/userinfo *(rw,sync,no_subtree_check,no_root_squash)
```
Then use the command ``exportfs -a``
After exporting restart nfs ``systemctl restart --now nfs-kernel-server``
To add users do the folling step:
```
adduser   user --home /mnt/userinfo/user
echo xfce4-session > /mnt/userinfo/user/.xsession
chown user:user /mnt/userinfo/user/.xsession
```
## Instalation in client
First we need to update and upgrade the machine and then install the nfs package:
`` apt update && apt -y upgrade && apt install nfs-common -y``
❗ Create a directory with the same name as the server❗ ``mkdir /mnt/userinfo``
Then mount: ``mount -t nfs serverIP:/mnt/userinfo /mnt/userinfo``

To save this mount do ``cat /etc/mtab`` copy the last line to ``/etc/fstab`` and add ``nofail``
