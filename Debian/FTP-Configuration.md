## FTP configuration in linux (debian)

First we need to install the ftp package:

```
apt install vsftpd  -y
```
Then open the ```/etc/vsftpd.conf```

And change:
*  listen=NO ➡️ listen=YES
*  Uncomment and change ➡️ listen_ipv6=NO
*  Uncomment and change ➡️ write_enable=YES
*  After that chage your certificates you can learn how to make them [here](https://github.com/Rodrigo-Serpa/AWS-Project/blob/main/Debian/Certificates.md)
```
rsa_cert_file=/etc/ssl/certs/ftp.inova.pt.crt
rsa_private_key_file=/etc/ssl/private/ftp.inova.pt.key
ssl_enable=YES
```
If you want implicit ssl add 
```
          pasv_enable=YES
          pasv_min_port= PORT1
          pasv_max_port= PORT2

          allow_anon_ssl=NO
          force_local_data_ssl=YES
          force_local_logins_ssl=YES
          ssl_tlsv1=YES
          ssl_sslv2=NO
          ssl_sslv3=NO
          require_ssl_reuse=NO
          ssl_ciphers=HIGH

          implicit_ssl=YES
          listen_port=990
```
For explicit, just remove the last 2 lines.

If evrything looks good, do ```systemctl restart vsftpd && systemctl status vsftpd```
