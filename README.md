# Search-Guard
#Подготовка к установки ELK
yum install tzdata
sudo yum install apr
sudo yum install telnet
sudo yum install java
sudo yum install elasticsearch
sudo yum install kibana
sudo yum install logstash

#elastic
sudo chkconfig --add elasticsearch
sudo -i service elasticsearch start
sudo -i service elasticsearch stop
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service
vi /etc/elasticsearch/jvm.options
`-Xms4g`
`-Xmx4g`


#Скачать дистрибутив Search Guard необходимой(своей) версии для elastic и Kibana#
https://docs.search-guard.com/latest/search-guard-versions

#Устанавливаем плагины для Кибаны и эластика#
/usr/share/kibana/bin/kibana-plugin install file:///tmp/search-guard-kibana-plugin-6.5.4-17.zip



####Минимально необходимая защита реализуется через TLS
#1.Скачиваем TLS утилиту 
https://docs.search-guard.com/latest/offline-tls-tool
https://search.maven.org/search?q=a:search-guard-tlstool
#2.Распаковываем
gunzip /opt/ELK/search-guard-tlstool-1.6.tar.gz
tar xvf /opt/ELK/search-guard-tlstool-1.6.tar
#3.Создаем свой конфиг файл для генерации ключей
cp /opt/ELK/config/example.yml /opt/ELK/config/SecPower.yml


2.Генерируем ключи утилитой 
