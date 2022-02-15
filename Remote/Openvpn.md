## Openvpn in ubuntu (remote.client)

First we need to install the openvpn package:
```
apt install openvpn -y
```
For this domain we are going to use 1 certificate, (created [here](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/Debian/Certificates.md). A client certificate.

### Client1 conf

``cp /usr/share/doc/openvpn/examples/sample-config-files/client.conf .``

``nano client_conf``

Change:

```
my-server-1 -> to the public ip to the server u wanna make a tunnel
Certificates
```

### Client2 conf

``cp client.conf client2.conf``

``nano client2_conf``

Change:

```
my-server-1 -> to the public ip to the server u wanna make a tunnel
Certificates
```
