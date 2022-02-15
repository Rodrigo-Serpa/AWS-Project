## Redhat certificates configurantion. (control.enta.pt)

First we need to install the easy-rsa package:
```
amazon-linux-extras install epel
yum install easy-rsa -y
```
Then we need to coppy the easy-rsa folder to /etc/
```
cp -r /usr/share/easy-rsa/3/ /etc/easy-rsa
```
cd /etc/3/

Now we are going to change some information in the vars folder ```nano vars```

Go down to the end of the file and change to following lines:
```
set_var EASYRSA_DN     "org"
set_var EASYRSA_REQ_COUNTRY    "PT"
set_var EASYRSA_REQ_PROVINCE   "Azores"
set_var EASYRSA_REQ_CITY       "São Miguel"
set_var EASYRSA_REQ_ORG        "ENTA CERTIFICATES"
set_var EASYRSA_REQ_EMAIL      "wemail@inovapt"
set_var EASYRSA_REQ_OU         "GRSI"
```
Then ```./easyrsa init-pki```
``./easyrsa build-ca``
After doing the first steps in [inova](https://github.com/Rodrigo-Serpa/AWS-Project/edit/main/Debian/Certificates.md) paste the inova ca in: ``nano /etc/easy-rsa/pki/reqs/subca.crt``
``./easyrsa sign-req ca subca``
``cat /etc/easy-rsa/pki/ca.crt >> /etc/easy-rsa/pki/issued/subca.crt`` This will make a chained CA
``/etc/easy-rsa/pki/issued/subca.crt``
Copied at take is back to INOVA
🛑 This is not the optimal way to do these steps but for the porpuse of this project ( we have access to both domains ) only 1 domain did all the certificates.🛑
