## Ubuntu DNS configurantion.

First we need to install the DNS package:
```
apt install bind9 -y 
```

Then proceed to cd /var/lib/bind/
```
nano db.inova.pt
```
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     inova.pt. root.inova.pt. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      inova.pt.
@       IN      AAAA    ::1

control  IN A   192.168.0.10
wazuh    IN A   192.168.2.12
sales    IN A   192.168.2.13
markting IN A   192.168.2.15
central  IN A   192.168.2.11
wwww     IN A   192.168.1.11
central  IN MX 10 192.168.2.11
```
After the dns set-up head over to ```cd /etc/bind```
```nano named.conf.options```
* Uncomment 
```
forwarders  {
        8.8.8.8;
};
```
After that go to ``` nano named.conf.local```
And paste:
```
zone "inova.pt" {
        type master;
        file "/etc/bind/db.inova.pt";
};
```
``` systemctl restart bind9 && systemctl status bind9 ```
If everything look's good now we need to change our machine dns in ```nano /etc/netplan/50-cloud-init.yaml```
```
nameservers:
    addresses: [ 192.182.0.10 ]
```
Don't forget to do ``` netplan try ``` to aplly the changes.
