## Ubuntu HTTP&HTTPS configuration (nginx). (control.inova.pt)

First we need to install the nginx package:
```apt install -y nginx ssl-cert```
Now go to ```/etc/nginx/snipperts``` , do ```cat snakeoil.conf``` and coppy these lines:
```
ssl_certificate /etc/ssl/certs/ssl-cert-snakeoil.pem;
ssl_certificate_key /etc/ssl/private/ssl-cert-snakeoil.key;
```
Now go to ``` cd /etc/nginx/sites-available/```
``` cp default defaultsafe ``` You can use any name you want for the defaultsafe file.
Now ``` nano defaultsafe```
```
Comment these 2 lines
* listen 80 default_server;
* listen [::]:80 default_server;

Then paste the snaikeoil.conf lines:
And change the certificates.
* ssl_certificate /etc/ssl/certs/www.controlinova.pt.crt;
* ssl_certificate_key /etc/ssl/private/www.control.inova.pt.key;

Then search for root /var/www/html and add a s in the end

root /var/www/htmls;

``` 
```
cd /var/www

And create a htmls file
 cp -r html htmls
Edit the htmls file as you like
```
`` systemctl restart nginx.service``
