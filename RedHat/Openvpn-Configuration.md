## Openvpn in redhat (control.enta.pt)

First we need to install the DNS package:
```
yum install openvpn -y
```
For this domain we are going to use 2 certificates, (created [here](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/Debian/Certificates.md). A client and server one.

## Server_ss
``cd /etc/openvpn/``
Then creat a ta.key and a dh key, ``openvpn --genkey --secret ta.key and openssl dhparam -out dh2048.pem 2048``
Get [this](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/RedHat/Server_ss) example for the server_ss conf.
``nano server_ss.conf`` and change:
```
remote: Public ip from server2
route: private net and mask from server2
Certificates: ❗use the client certificates❗
push “route eth0 MASK”
push “route eth1 MASK”
push “route eth2 MASK”
```
## Server_ra
Get [this](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/RedHat/Server_ra)example for the server_ra conf.
Change:
```
Certificates: ❗use the server certificates❗
push “route eth0 MASK”
push “route eth1 MASK”
push “route eth2 MASK”
```
