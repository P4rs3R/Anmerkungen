## Install ELK stack (elasticsearch)
(Installed as `root` - tested on Debian 9.x x64) 
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-6.x.list
apt-get update && apt-get install elasticsearch
service elasticsearch start && update-rc.d elasticsearch defaults 95 10
curl -XGET 'localhost:9200/?pretty'
```
The last command is a test: if it works, showing you info such as version and cluster name, everything's fine with elasticsearch
```
apt-get install kibana
service kibana start && update-rc.d kibana defaults 95 10
apt-get install logstash
systemctl start logstash.service
```
## Configuration
[Configuring /etc/kibana/kibana.yml](https://www.elastic.co/guide/en/kibana/6.2/settings.html)
