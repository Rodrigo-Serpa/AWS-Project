## Ubuntu certificates configurantion. (control.inova.pt)

First we need to install the easy-rsa package:
```
apt install easy-rsa -y
```
Then we need to coppy the easy-rsa folder to /etc/
```
cp -r /usr/share/easy-rsa/ /etc/
```
cd /etc/easy-rsa/

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
To build the CA we do the following command ```./easyrsa build-ca subca``` you can either use a password, or if don't want to use a password do the same command and add ```nopass```at the end
🛑
After finishing these steps go to [Enta certificates](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/RedHat/Certificates.md) and do the steps there before continuing!
🛑
After completing the Enta certificates, we can proced to create our certificates.
First we need to get the signed CA from ENTA ```nano /etc/easy-rsa/pki/ca.crt``` paste it there.
Then put it in ```/etc/ssl/certs/ ```
### Now we are going to create the certificates
```
./easyrsa build-server-full serverra.control.inova.pt nopass
./easyrsa build-client-full serverss.control.inova.pt nopass
./easyrsa build-server-full ftp.control.inova.pt nopass
./easyrsa build-server-full www.control.inova.pt nopass

./easyrsa build-server-full vpn.control.enta.pt nopass
./easyrsa build-server-full www.control.enta.pt nopass
./easyrsa build-server-full ftp.control.enta.pt nopass

./easyrsa build-client-full remote.client nopass
```
After creating the certificates copy all inova certificates to :
``` 
The .crt to /etc/ssl/certs/
And the .key to /etc/ssl/private/
```
Then copy the enta certificates to the enta domain [Enta certificates](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/RedHat/Certificates.md)and the
[remote certificates](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/Remote/Openvpn.md).

🛑 This is not the optimal way to do these steps but for the porpuse of this project ( we have access to both domains ) only 1 domain did all the certificates.🛑
