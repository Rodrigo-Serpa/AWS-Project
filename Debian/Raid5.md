## Raid 5 in ubuntu (central.inova.pt)

For this part we are going to need to add 3 volumes to the aws machine.
After that, do ``lsblk`` and check the names of the volumes and run `` gdisk /dev/(name of the disk)``
Do the follwing order:
```
o
w
n
enter
enter
enter
fd00
w
y
```
This will clean previus partitions and create new ones.

After run ``mdadm --create /dev/md0 --level 5 --raid-devices 3 /dev/xvdf1 /dev/xvdg1 /dev/xvdh1 /dev/xvdi1``
To check if everything is ok run ``mdadm --detail /dev/md0``
And it should look like this:
```
 State : clean
	Active Devices : 4
	Working Devices : 4
	Failed Devices : 0
	Spare Devices : 0
```

### Lucks configuration

``cryptsetup luksFormat --hash=sha512 --key-size=512 --cipher=aes-xts-plain64 --verify-passphrase /dev/md0`` Yes and type a password you would like.

``cryptsetup luksOpen /dev/md0 md0_crypt`` (md0_crypt can be any name you want it to be)
### LVM

```
pvcreate /dev/mapper/md0_crypt
vgcreate vg0 /dev/mapper/md0_crypt
lvcreate -n lv0 -l +100%FREE vg0
```
Add a file system: ``mkfs.xfs /dev/vg0/lv0``
Create a directory where you want to mount mkdir ``/mnt/userinfo``
``mount /dev/vg0/lv0 /mnt/raid5``
❗Do this if you want it to be mounted everytime you reboot the machine❗
```
cat /etc/mtab
copy the last 2 lines and paste it in /etc/fstab and ❗add nofail❗
