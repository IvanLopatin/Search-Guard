:white_check_mark:***Search-Guard настройка***
=====================

#Подготовка к установки ELK
=====================
yum install tzdata

sudo yum install apr

sudo yum install telnet

sudo yum install java

sudo yum install elasticsearch

sudo yum install kibana

sudo yum install logstash



##elastic
=====================
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



##Устанавливаем плагины для Кибаны и эластика
/etc/init.d/elasticsearch stop
/etc/init.d/kibana stop

/usr/share/kibana/bin/kibana-plugin install file:///tmp/search-guard-kibana-plugin-6.5.4-17.zip





###Минимально необходимая защита реализуется через TLS

#1.Скачиваем TLS утилиту
 
https://docs.search-guard.com/latest/offline-tls-tool

https://search.maven.org/search?q=a:search-guard-tlstool

#2.Распаковываем

gunzip /opt/ELK/search-guard-tlstool-1.6.tar.gz

tar xvf /opt/ELK/search-guard-tlstool-1.6.tar

#3.Создаем свой конфиг файл для генерации ключей SecPower.yml(см. в директории)

cp /opt/ELK/config/example.yml /opt/ELK/config/SecPower.yml

#3.1 Изменяем сертификаты под свою фирму



#4.Генерируем ключи утилитой 

#(генерируем сразу все сертификаты)

tools/sgtlstool.sh -c config/SecPower.yml -ca -crt 

#5.Добавляем в elasticsearch.yml пути к нашим сертификатам, перед тем переносим их к директорию к elastic (/etc/elasticsearch)

vi /etc/elasticsearch/elasticsearch.yml


##add

action.auto_create_index: .monitoring*,.watches,.triggered_watches,.watcher-history*,.ml*

searchguard.ssl.transport.pemcert_filepath: out/node1.pem

searchguard.ssl.transport.pemkey_filepath: out/node1.key

searchguard.ssl.transport.pemkey_password: default-scret

searchguard.ssl.transport.pemtrustedcas_filepath: out/root-ca.pem

searchguard.ssl.transport.enforce_hostname_verification: false

searchguard.authcz.admin_dn:

  - CN=admin,OU=client,O=client,L=test, C=de




#5.1 Проверяем правильность заполнения и загрузку сертификатов

/opt/ELK/tools/sgtlsdiag.sh -es /etc/elasticsearch/elasticsearch.yml


