## Redhat HTTP & HTTPS configurantion. (www.enta.pt)

First we need to install the apache package:
``  yum install httpd ``

Then you need to put the certificate and the key in `` /etc/pki/tls/private `` (if you dont have certificates check [this](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/Debian/Certificates.md)

Next we create a htmls directory: ``  cp -r /var/www/html /var/www/htmls``

Go to `` /etc/httpd/conf.d/ssl.conf ``
And change:
```
<VirtualHost _default_:443>

# General setup for the virtual host, inherited from global configuration
DocumentRoot "/var/www/htmls"
ServerName www.enta.pt:443
 ```
 Then comment the following lines:
 ```
#SSLProtocol all -SSLv3
#SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5:!SEED:!IDEA
```
After that edit the certificates:
```
SSLCertificateFile /etc/pki/tls/certs/www.control.enta.crt
SSLCertificateKeyFile /etc/pki/tls/private/www.control.enta.key
```
If everyting looks good, run ``  systemctl restart httpd ``
