## Raid 6 in ubuntu (wazuh.inova.pt)

For this part we are going to need to add 4 volumes to the aws machine.

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

Then run ``mdadm --create /dev/md0 --level 6 --raid-devices 4 /dev/xvdf1 /dev/xvdg1 /dev/xvdh1 /dev/xvdi1``

To check if everything is ok run ``mdadm --detail /dev/md1``

It should look like this:
```
  State : clean
	Active Devices : 4
	Working Devices : 4
	Failed Devices : 0
	Spare Devices : 0
  ```
### Lucks

``cryptsetup luksFormat --hash=sha512 --key-size=512 --cipher=aes-xts-plain64 --verify-passphrase /dev/md1``

Yes and type the password you wish.

### LVM

```
pvcreate /dev/mapper/md1_crypt
vgcreate vg1 /dev/mapper/md1_crypt
lvcreate -n lv1 -l +100%FREE vg1
```
Attach the file system you want to use: ``mkfs.xfs /dev/vg0/lv0``

Make a file you want to mount ``mkdir /mnt/raid6`` and then mount it ``mount /dev/vg1/lv1 /mnt/raid6

If you want to be mount everytime you reboot do this steps:
```
cat /etc/mtab
copy last 2 lines
paste it in /etc/fstab and add ❗nofail❗
```
