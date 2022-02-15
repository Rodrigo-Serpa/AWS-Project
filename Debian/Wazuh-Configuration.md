## WAZUH configuration in linux (wazuh.inova.pt)(debian)


First we need to update & upgrade the machine ``apt update && apt -y upgrade``

Then install filebeat: ``apt-get install filebeat``
After that install the preconfigured Filebeat configuration: ``curl -so /etc/filebeat/filebeat.yml https://packages.wazuh.com/resources/4.2/open-distro/filebeat/7.x/filebeat_all_in_one.yml``

Install the elasticsearch alerts template ``curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/4.2/extensions/elasticsearch/7.x/wazuh-template.json
chmod go+r /etc/filebeat/wazuh-template.json``

Install the Wazuh module for Filebeat ``curl -s https://packages.wazuh.com/4.x/filebeat/wazuh-filebeat-0.1.tar.gz | tar -xvz -C /usr/share/filebeat/module``

Coppy the certificates to /etc/filebeat/certs:
```
mkdir /etc/filebeat/certs
cp ~/certs/root-ca.pem /etc/filebeat/certs/
mv ~/certs/filebeat* /etc/filebeat/certs/
```
Enable the service:
```
systemctl daemon-reload
systemctl enable filebeat
systemctl start filebeat
```
To check if everything is ok run ``filebeat test output``

It should like this:
```
Output

 elasticsearch: https://127.0.0.1:9200...
   parse url... OK
   connection...
     parse host... OK
     dns lookup... OK
     addresses: 127.0.0.1
     dial up... OK
   TLS...
     security: server's certificate chain verification is enabled
     handshake... OK
     TLS version: TLSv1.3
     dial up... OK
   talk to server... OK
   version: 7.10.2
   ```
   Now we need to isntall kibana package: ``apt-get install opendistroforelasticsearch-kibana``
   
   Install the kibana configurantion file: ``curl -so /etc/kibana/kibana.yml https://packages.wazuh.com/resources/4.2/open-distro/kibana/7.x/kibana_all_in_one.yml``
   
   Create the /usr/share/kibana/data directory:
   ```
mkdir /usr/share/kibana/data
chown -R kibana:kibana /usr/share/kibana/data
```
Install the Wazuh Kibana plugin.
❗ The instalation must be done in kibana home directory❗
```
cd /usr/share/kibana
sudo -u kibana /usr/share/kibana/bin/kibana-plugin install https://packages.wazuh.com/4.x/ui/kibana/wazuh_kibana-4.2.5_7.10.2-1.zip
```
Coppy the certificates to /etc/kibana/certs:
```
mkdir /etc/kibana/certs
cp ~/certs/root-ca.pem /etc/kibana/certs/
mv ~/certs/kibana* /etc/kibana/certs/
chown kibana:kibana /etc/kibana/certs/*
```
Link Kibana socket to port 443: `` setcap 'cap_net_bind_service=+ep' /usr/share/kibana/node/bin/node`` 
```
systemctl daemon-reload
systemctl enable kibana
systemctl start kibana
```
To access the web interface:
```
URL: https://<wazuh_server_ip>
user: admin
password: admin
```
