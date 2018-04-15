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
```
Now set credentials by:
```
/usr/share/elasticsearch/bin/x-pack/setup-passwords interactive
```
and test by opening the browser on http://localhost:9200/
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
```
test via browser on localhost:5601
```
apt-get install logstash
cd /usr/share/logstash && bin/logstash-plugin install x-pack
systemctl start logstash.service
```
## Extra: Beats (monitoring internal networks)
This is an example, depending on beat & version you want to install
```
curl -L -O https://artifacts.elastic.co/downloads/beats/heartbeat/heartbeat-6.2.2-amd64.deb && dpkg -i heartbeat-6.2.2-amd64.deb
```

##Running logstash
Make sure the folder `/usr/share/logstash/data` is writable, then you can use logstash to fill the elastic DB. For example, with the RSS plugin, creating a file rssfilter.conf as exaplined [here](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-rss.html), like in this [example](https://www.exensio.de/news-medien/newsreader-blog/rss-feeds-mit-logstash-und-elasticsearch-durchsuchen). Make sure to write elastic's credentials (user and password) when you specify the host in the conf file. And then, run: `bin/logstash -f rssfilter.conf`.
