## Redhat DNS configurantion. (control.enta.pt)

First we need to install the easy-rsa package:
```
yum install bind
```
Then go to ``/etc/named.conf`` change the lines:
```
  options {
          listen-on port 53 { any; };
          listen-on-v6 port 53 { any; };
          ...
          allow-query     { any; };
  
  And add the forward zone
  
    zone "enta.pt" {
  type master;
  file "/var/named/db.enta.pt";
  };
```
Go to `` /var/named/´´ and create the forward zone `` nano db.enta.pt``
```
$TTL    86400
@               IN SOA  enta.pt. root (
                        42              ; serial
                        3H              ; refresh
                        15M             ; retry
                        1W              ; expiry
                        1D )            ; minimum
         IN NS  ns
ns       IN A   172.31.0.10
teleport IN A   172.31.0.10
control  IN A   172.31.0.10
wazuh    IN A   172.31.128.12
sales    IN A   172.31.128.13
markting IN A   172.31.128.14
central  IN A   172.31.128.11
www      IN A   172.31.96.11
```

After creating the forward zone restart the service:
```
  systemctl restart named.service 
  systemctl status named.service   
  ```
