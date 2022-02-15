## Redhat ipatbles configurantion. (control.inova.pt)

Install the iptables package: ``yum install iptables-services``
```
systemctl enable --now iptables
```

### HTTPS & HTTPS 
```
-A PREROUTING -i eth0 -p tcp -m multiport --dports 80,443 -j DNAT --to-destination 172.31.96.14
```
### FTP 
```
-A PREROUTING -i eth0 -p tcp -m tcp --dport 5000:5100 -j DNAT --to-destination 172.31.96.14
-A PREROUTING -i eth0 -p tcp -m tcp --dport 20:21 -j DNAT --to-destination 172.31.96.14
-A PREROUTING -i eth0 -p tcp -m tcp --dport 990 -j DNAT --to-destination 192.168.1.101
```
-A POSTROUTING -o eth0 -j MASQUERADE

```
service iptables restart
service iptables reload
service iptables save
```

