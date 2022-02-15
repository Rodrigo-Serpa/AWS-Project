## Redhat ftp configurantion. (control.enta.pt)

First we need to install the ftp package:
```
  sudo amazon-linux-extras install epel
  yum install proftpd
  ```
  After installing get your certificates and put them in: `` /etc/ssl/certs`` (certificates we made them [here](https://github.com/Rodrigo-Serpa/AWS-Project/edit/main/Debian/Certificates.md))
  Go to ``/etc/proftpd.conf``
  ```
  Define the ports you want to use, I used 5000:51000
  Right below (DefaultRoot                     ~ !adm)
  
  PassivePorts    6000    6100
  ```
  Then comment this line: `` #<IfDefine TLS>``
  
  After change the certificates path:
  ```
TLSRSACertificateFile         /etc/ssl/certs/ftp.control.enta.crt
TLSRSACertificateKeyFile      /etc/ssl/certs/ftp.control.enta.key
```
Uncomment this: ``TLSRenegotiate                ctrl 3600 data 512000 required off timeout 300``

And then comment this last lines:
```
         #  <IfModule mod_tls_shmcache.c>
         #    TLSSessionCache            shm:/file=/var/run/proftpd/sesscache
         #  </IfModule>
         #</IfDefine>
```
After that just restart the service ``  systemctl enable --now proftpd``
