## Ubuntu ipatbles configurantion. (control.inova.pt)
First we need to install the easy-rsa package:
``apt install netfilter-persistant iptables-persistant``
After installing the package open `` nano /etc/iptables/rules.v4``

HTTP & HTTPS / FTP  [www.inova.pt](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/Debian/HTTP-HTTPS-Configuration.md)
```
-A PREROUTING -i eth0 -p tcp -m multiport --dports 80,443 -j DNAT --to-destination 192.168.1.11
-A PREROUTING -i eth0 -p tcp -m tcp --dport 990 -j DNAT --to-destination 192.168.1.11
-A PREROUTING -i eth0 -p tcp -m tcp --dport 5000:5100 -j DNAT --to-destination 192.168.1.11
-A PREROUTING -i eth0 -p tcp -m tcp --dport 20:21 -j DNAT --to-destination 192.168.1.11
```
Postfix & Dovecot [central.inova.pt](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/Debian/Mail-Server-Configuration)
```
-A PREROUTING -i eth0 -p tcp -m multiport --dports 25,465,587,143,110,993,995 -j DNAT --to-destination 192.168.2.11
```
Wazuh web page [wazuh.inova.pt]
```
-A PREROUTING -i eth0 -p tcp -m tcp --dport 444 -j DNAT --to-destination 192.168.2.12:443
```
Client in sales.inova.pt
```
-A PREROUTING -i eth0 -p tcp -m tcp --dport 3389 -j DNAT --to-destination 192.168.2.13
```
Client in marketing.inova.pt
```
-A PREROUTING -i eth0 -p tcp -m tcp --dport 3390 -j DNAT --to-destination 192.168.2.15:3389
```
``-A POSTROUTING -o eth0 -j MASQUERADE``
