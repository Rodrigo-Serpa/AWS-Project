## Ubuntu certificates configurantion. (central.inova.pt)
First we need to install the postfix,dovecote packages:
``apt -y install postfix postfix-doc dovecot-imapd dovecot-pop3d libsasl2-dev sasl2-bin``
Select the second option, and put the domain

``dpkg-reconfigure postfix``
* Second option
* domain
* ubuntu
* Don't change
* No
* Clear everything
* 0
* +
* ipv4

First we need to get our [certificates](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/Debian/Certificates.md) and put them in the correct directory.
Then
``nano /etc/postfix/main.cf``
Change:
```
smtpd_tls_cert_file=/etc/ssl/certs/mail.control.inova.pt.crt
smtpd_tls_key_file=/etc/ssl/private/mail.control.inova.pt.key
```
``nano /etc/postfix/master.cf``
And uncomment:
```
submission inet n       -       y       -       -       smtpd
-o syslog_name=postfix/submission
-o smtpd_tls_security_level=encrypt
-o smtpd_sasl_auth_enable=yes
```
And add right bellow ``-o smtpd_client_restrictions=permit_sasl_authenticated,reject``
uncomment:
```
smtps     inet  n       -       y       -       -       smtpd
-o syslog_name=postfix/smtps
-o smtpd_tls_wrappermode=yes
-o smtpd_sasl_auth_enable=yes
```
add right bellow ``-o smtpd_client_restrictions=permit_sasl_authenticated,reject``
``nano /etc/postfix/sasl/smtpd.conf``
add 
```
pwcheck_method: saslauthd
mech_list: PLAIN LOGIN
```
``nano /etc/default/saslauthd``
Change: 
```
START=yes
OPTIONS="-c -m /var/spool/postfix/var/run/saslauthd"
```
`` postconf -e 'home_mailbox = Maildir/'`` 
`` nano /etc/dovecot/conf.d/10-auth.conf ``
Uncomment and change:
`` disable_plaintext_auth = no ``

`` nano /etc/dovecot/conf.d/10-mail.conf``
```
Uncomment:
mail_location = maildir:~/Maildir

comment:
mail_location = mbox:~/mail:INBOX=/var/mail/%u
```
``nano /etc/dovecot/conf.d/10-ssl.conf``
```
Change:
ssl = yes
Uncomment and change:
ssl_cert = </etc/ssl/certs/mail.control.inova.pt.crt
ssl_key = </etc/ssl/private/mail.control.inova.pt.key
```

Go to ``cd /etc/skel/`` and do ``maildirmake.dovecot Maildir`` with this everytime a new user is added the Maildir folder is created automatically.

```
adduser postfix sasl
adduser dovecot sasl
```
``systemctl restart saslauthd.service postfix dovecot``
