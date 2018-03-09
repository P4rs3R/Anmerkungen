## Install ELK stack (elasticsearch)
(Installed as `root` - tested on Debian 9.x x64). 
[Suggested order](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html#install-order-elastic-stack):
![Elastic and x-pack](https://www.elastic.co/guide/en/elasticsearch/reference/6.2/setup/images/ElasticsearchFlow.jpg)
![Kibana and x-pack](https://www.elastic.co/guide/en/kibana/6.2/setup/images/KibanaFlow.jpg)
![Logstash and x-pack](https://www.elastic.co/guide/en/logstash/6.2/setup/images/LogstashFlow.jpg)

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-6.x.list
apt-get update && apt-get install elasticsearch
cd /usr/share/elasticsearch && bin/elasticsearch-plugin install x-pack
service elasticsearch start && update-rc.d elasticsearch defaults 95 10
curl -XGET 'localhost:9200/?pretty'
```
The last command is a test: if it works, showing you info such as version and cluster name, everything's fine with elasticsearch
```
apt-get install kibana
cd /usr/share/kibana && bin/kibana-plugin install x-pack
```
Now, edit `kibana.yml` config file, at least:
```
elasticsearch.username: "kibana"
elasticsearch.password: "kibanapassword"
```
...details on [Configuring /etc/kibana/kibana.yml](https://www.elastic.co/guide/en/kibana/6.2/settings.html)

```
service kibana start && update-rc.d kibana defaults 95 10

apt-get install logstash
cd /usr/share/logstash && bin/logstash-plugin install x-pack
systemctl start logstash.service
```
## Extra: Beats (monitoring internal networks)
This is an example, depending on beat & version you want to install
```curl -L -O https://artifacts.elastic.co/downloads/beats/heartbeat/heartbeat-6.2.2-amd64.deb && sudo dpkg -i heartbeat-6.2.2-amd64.deb
```
