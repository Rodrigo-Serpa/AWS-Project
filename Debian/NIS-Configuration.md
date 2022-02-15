## Nis server in ubuntu (central.inova.pt)
❗Add the server to the ``/etc/hosts in order for this work smothly❗
First we need to install the nis package:
``apt -y install nis`` and add the domain (in my case inova.pt)

In ``/etc/default/nis``
Change:
```
NISSERVER=master
NISCLIENT=false
```
In ``/etc/ypserv.securenets``
```
Comment 0.0.0.0         0.0.0.0
ADD     255.255.255.0  192.168.2.0
```
Go to ``nano /var/yp/Makefile``
And change:
```
MERGE_PASSWD=true
MERGE_GROUP=true
```
`` systemctl restart rpcbind nis``
After restart do ``/usr/lib/yp/ypinit -m`` CTRL + d  and then Y

Everytime you add an user u need to update the server. Go to ´´ cd /var/yp`` and run ``make``

## Nis client
❗Add the server to the ``/etc/hosts in order for this work smothly❗

``apt install nis -y``
In ``nano /etc/yp.conf`` add:
``domain inova.pt server sales.inova.pt``
Go to ``nano /etc/nsswitch.conf`` and it should look like this:
```
passwd:         files systemd nis
group:          files systemd nis
shadow:         files nis
gshadow:        files

hosts:          files dns nis
```
In ``nano /etc/pam.d/common-session`` add ``session optional pam_mkhomedir.so skel=/etc/skel umask=077``
``systemctl restart nis``
