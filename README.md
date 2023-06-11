# Installation and Configuration of Redis Cluster on Ubuntu 20.04
Ubuntu 20.04 üzerinde Redis cluster kurulumu ve konfigürasyonu / Installing and configuring Redis cluster on Ubuntu 20.04

![image](https://user-images.githubusercontent.com/93924485/225926085-9bdce1db-27c4-43e6-919c-8292a70f4f88.png)

# Redis Nedir ? 

* Redis, açık kaynaklı, hafıza-tabanlı (in-memory) bir veritabanı yönetim sistemidir. Ana amaçlarından biri, yüksek performanslı ve ölçeklenebilir veritabanları sağlamaktır. Redis, anahtar-değer (key-value) mağazası olarak çalışır ve hızlı bir şekilde erişim sağlamak için verileri bellekte tutar. Ayrıca, Redis verilerini disk üzerinde saklamak için de kullanılabilir. Redis, özellikle önbellek, oturum yönetimi, sıralama ve gerçek zamanlı uygulamalar gibi alanlarda popüler bir veritabanı çözümüdür.

# What is Redis ?

* Redis is an open-source, in-memory database management system. One of its main goals is to provide high-performance and scalable databases. Redis works as a key-value store and keeps data in memory for fast access. It can also be used to store data on disk. Redis is a popular database solution, especially in areas such as caching, session management, sorting, and real-time applications.

### 1-) Kurulumdan önce işletim sistemimizin paketlerini güncellemek için aşağıdaki komutları çalıştırıyoruz.
### 1-) We run the following commands to update our operating system's packages before installation.

> sudo apt-get update  
> sudo apt-get upgrade

### 2-) Redis Cluster kurulumu ve konfigürasyonu / Redis Cluster installation and configuration. 

![image](https://user-images.githubusercontent.com/93924485/225933102-eb5f2f6a-e3db-4f4a-b180-87c10fcf8081.png)

• Redis kurulumu yapmak için aşağıdaki komutu çalıştırmamız gerekiyor. 

• We need to run the following command to install Redis.

> sudo apt-get install redis-server

> sudo systemctl disable redis-server.service

### 3-) Kurulumu tamamladıktan sonra aşağıdaki komutları çalıştırarak portlara izin vermemiz gerekiyor.  
### 3-) After completing the installation, we need to run the following commands to allow access to the ports.

> sudo ufw allow 7000 
   
> sudo ufw allow 7001
  
> sudo ufw allow 17000
  
> sudo ufw allow 17001
        
![image](https://user-images.githubusercontent.com/93924485/225950468-b8c31ff3-d03f-4355-a106-5f38d201d06c.png)


### Eğer işletim sisteminizde Vim kurulu değilse bu işlemi gerçekleştirmeden önce Vim editörünü kurmanız gerekmektedir. Vim editörünü kurmak için aşağıdaki komutu çalıştırabilirsiniz.

### If Vim is not installed on your operating system, you need to install the Vim editor before performing this process. To install the Vim editor, you can run the following command.


> sudo apt-get install vim


### 4-) Aşağıdaki komutu çalıştırarak /etc/rc.local dosyasını oluşturun ve düzenleyin.
### 4-) Create and edit the /etc/rc.local file by running the following command.

> vim /etc/rc.local 

``` bash
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.
echo never > /sys/kernel/mm/transparent_hugepage/enabled
sysctl -w net.core.somaxconn=65535
exit 0 ' '
```


### 5-) Oluşturduğumuz dosyayı çalıştırılabilir hale getirmek için aşağıdaki komutu çalıştırmalıyız.
### 5-) To make a file executable, you should run the following command.


> chmod +x /etc/rc.local


• Dosyayı kaydedip çıkın.

• Save the file and exit.

>:wq!


### 6-) Aşağıdaki komutu çalıştırarak /etc/sysctl.conf dosyasını düzenleyin.
### 6-) Edit the /etc/sysctl.conf file by running the following command.


> vim /etc/sysctl.conf


``` bash 

vm.overcommit_memory=1

``` 

 ![image](https://user-images.githubusercontent.com/93924485/225975468-5845e2fa-bfd3-461f-8138-cc34a556c695.png)



• Dosyayı kaydedip çıkın.

• Save the file and exit.


>:wq!


### 7-) Aşağıdaki komutları çalıştırarak bazı gerekli klasörleri oluşturun.
### 7-) Create some necessary folders by running the following commands.


> sudo mkdir /etc/redis/cluster 

> sudo mkdir /etc/redis/cluster/7000

> sudo mkdir /var/lib/redis/7000

> sudo mkdir /etc/redis/cluster/7001

> mkdir /etc/redis/cluster/7001



### 8-) Aşağıdaki komutu çalıştırarak /etc/redis/cluster/7000/redis_7000.conf dosyasını oluşturun ve düzenleyin.
### 8-) Create and edit the /etc/redis/cluster/7000/redis_7000.conf file by running the following command.



> vim /etc/redis/cluster/7000/redis_7000.conf



``` bash 

port 7000
dir /var/lib/redis/7000/
appendonly no
protected-mode no
cluster-enabled yes
cluster-node-timeout 5000
cluster-config-file /etc/redis/cluster/7000/nodes_7000.conf
pidfile /var/run/redis/redis_7000.pid
logfile /var/log/redis/redis_7000.log
loglevel notice
requirepass judajudajudajudajuda
masterauth judajudajudajudajuda


``` 


![image](https://user-images.githubusercontent.com/93924485/230792462-2670c7e2-ff12-4ade-a756-23aecc9097c7.png)





• Dosyayı kaydedip çıkın.

• Save the file and exit.


>:wq!









### 9-) Aşağıdaki komutu çalıştırarak /etc/redis/cluster/7001/redis_7001.conf dosyasını oluşturun ve düzenleyin.
### 9-) Create and edit the /etc/redis/cluster/7001/redis_7001.conf file by running the following command.



>  vim /etc/redis/cluster/7001/redis_7001.conf


``` bash 


port 7001
dir /var/lib/redis/7001
appendonly no
protected-mode no
cluster-enabled yes
cluster-node-timeout 5000
cluster-config-file /etc/redis/cluster/7001/nodes_7001.conf
pidfile /var/run/redis/redis_7001.pid
logfile /var/log/redis/redis_7001.log
loglevel notice
masterauth judajudajudajudajuda
requirepass judajudajudajudajuda


``` 


![image](https://user-images.githubusercontent.com/93924485/230792337-83e57a72-d4a2-401a-bb0f-c55ea451a401.png)





• Dosyayı kaydedip çıkın.

• Save the file and exit.


>:wq!



### 10-) Redis Server servisleri için bir redis kullanıcısı ve bir redis grubu oluşturun ve aşağıdaki komutları çalıştırarak onlara doğru izinleri verin.
### 10-) Create a redis user and a redis group for the Redis Server services and give them the correct permissions by running the following commands.


> sudo chown redis:redis -R /var/lib/redis 

> sudo chmod 770 -R /var/lib/redis

> sudo chown redis:redis -R /etc/redis



### 11-) Aşağıdaki komutu çalıştırarak /etc/systemd/system/redis_7000.service dosyasını oluşturun ve düzenleyin.
### 11-) Create and edit the /etc/systemd/system/redis_7000.service file by running the following command.



> vim /etc/systemd/system/redis_7000.service



``` bash

[Unit]
Description=Redis key-value database on 7000
After=network.target
[Service]
ExecStart=/usr/bin/redis-server /etc/redis/cluster/7000/redis_7000.conf --supervised systemd
ExecStop=/bin/redis-cli -h 127.0.0.1 -p 7000 shutdown
Type=notify
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target

```


![image](https://user-images.githubusercontent.com/93924485/225981237-d3624786-bc61-4507-8bae-e2dc68c8417f.png)




• Dosyayı kaydedip çıkın.

• Save the file and exit.


>:wq!


### 12-) Aşağıdaki komutu çalıştırarak /etc/systemd/system/redis_7001.service dosyasını oluşturun ve düzenleyin.
### 12-) Create and edit the /etc/systemd/system/redis_7001.service file by running the following command.



> vim /etc/systemd/system/redis_7001.service


``` bash

[Unit]
Description=Redis key-value database on 7001
After=network.target
[Service]
ExecStart=/usr/bin/redis-server /etc/redis/cluster/7001/redis_7001.conf --supervised systemd
ExecStop=/bin/redis-cli -h 127.0.0.1 -p 7001 shutdown
Type=notify
User=redis
Group=redis
RuntimeDirectory=/etc/redis/cluster/7001
RuntimeDirectoryMode=0755
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target


```



![image](https://user-images.githubusercontent.com/93924485/225982032-2222835e-f639-4e65-9070-b6cedb125ded.png)





• Dosyayı kaydedip çıkın.

• Save the file and exit.


>:wq!




### 13-) systemd'nin redis_7000.service ve redis_7001.service hizmetlerini otomatik olarak başlatmasını sağlamak için aşağıdaki komutları çalıştırın.
### 13-)  Run the following commands to make systemd automatically start the redis_7000.service and redis_7001.service services.


> sudo systemctl enable /etc/systemd/system/redis_7000.service

> sudo systemctl enable /etc/systemd/system/redis_7001.service



### 14-) Aşağıdaki komutu çalıştırarak sunucuyu yeniden başlatın.
### 14-) Reboot the server by running the following command.


> sudo reboot



### 15-) Aşağıdaki komutu çalıştırarak 7000 numaralı bağlantı noktasında çalışan redis-server hizmeti için log dosyasının içeriğini (son 100 satır) kontrol edin.
### 15-)  Run the following command to check the contents of the log file (last 100 lines) for the redis-server service running on port 7000.


> tail -n 100 /var/log/redis/redis_7000.log


![image](https://user-images.githubusercontent.com/93924485/225987547-b9c6ccfb-ae54-4b6d-927c-e83cb9bb3385.png)




### 16-) Aşağıdaki komutu çalıştırarak 7001 numaralı bağlantı noktasında çalışan redis-server hizmeti için log dosyasının içeriğini (son 100 satır) kontrol edin.
### 16-)  Run the following command to check the contents of the log file (last 100 lines) for the redis-server service running on port 7001.



> tail -n 100 /var/log/redis/redis_7001.log



![image](https://user-images.githubusercontent.com/93924485/225987929-05ed9277-3bf4-49ba-be7f-e2acbbd84fe4.png)




### 17-) Aşağıdaki komutu çalıştırarak 7000 numaralı çalışan redis-server hizmetinin durumunu kontrol edin.
### 17-) Run the following command to check the status of the redis-server service running on port 7000.



> systemctl status redis_7000.service



![image](https://user-images.githubusercontent.com/93924485/225988697-b84c7dca-d014-4b3e-890e-8fb8fec1eea4.png)





### 18-) Aşağıdaki komutu çalıştırarak 7001 numaralı çalışan redis-server hizmetinin durumunu kontrol edin.
### 18-) Run the following command to check the status of the redis-server service running on port 7001.



> systemctl status redis_7001.service



![image](https://user-images.githubusercontent.com/93924485/225988786-6f4973c8-c3f7-4ab4-b33c-d7cc01a6b1a5.png)




# Redis Cluster Konfigürasyonu / Redis Cluster Configuration 



### • 3 Redis sunucusunun IP adreslerinin aşağıdaki gibi olduğunu düşünün.
### • Consider that the IP addresses of three Redis servers are as follows.



> 1-) 192.168.152.136

> 2-) 192.168.152.134

> 3-) 192.168.152.135 


### • Ve son olarak aşağıdaki komutu çalıştırarak Redis clusterı oluşturun.
### • Finally, create the Redis cluster by running the following command.


> redis-cli -a judajudajudajudajuda --cluster create 192.168.152.136:7000 192.168.152.134:7000 192.168.152.135:7000 192.168.152.136:7001 192.168.152.134:7001 192.168.152.135:7001 --cluster-replicas 1



![image](https://user-images.githubusercontent.com/93924485/225991885-a297a031-d318-4cec-abe1-3b7de2f26712.png)




``` bash 
 sudo chown -R redis:redis /run/redis

 systemctl start redis-server.service

 systemctl restart redis-server.service

 systemctl start redis_7001.service

 systemctl restart redis_7001.service

 systemctl start redis_7000.service

 systemctl restart redis_7000.service

``` 


> vim /etc/redis/redis.conf 


> pidfile /var/run/redis/redis-server.pid




![image](https://user-images.githubusercontent.com/93924485/226220877-94b764c9-f31b-4266-95e7-e6a6035939b2.png)





### • Redis'i kontrol etmek için SET ve GET komutlarını çalıştırıyoruz.
### • We are running the SET and GET commands to check Redis.


> redis-cli -c -h 192.168.152.136 -p 7000 -a judajudajudajudajuda 

> redis-cli -c -h 192.168.152.136 -p 7001 -a judajudajudajudajuda




``` bash 

set a 1 

set b 2 

set c 3 

set d 4 

get b

get a 

get c 

get d 

```


![image](https://user-images.githubusercontent.com/93924485/226222958-af3ba98f-af49-49a1-ad55-a29227110296.png)





### • Aşağıdaki komutu çalıştırarak 40 saniye boyunca Sunucu 1'de Master'ın yük devretmesini simüle edin.
### • Simulate a failover of the Master on Server 1 for 40 seconds by running the following command.


> redis-cli -c -h 192.168.152.136 -p 7000 -a judajudajudajudajuda DEBUG sleep 40 


• Bu komutun çıktısı olarak "OK" almalısınız.

• You should get OK as the output of this command.


![image](https://user-images.githubusercontent.com/93924485/226224526-531514e2-ae88-4614-a6ea-730f61120fe5.png)




![image](https://user-images.githubusercontent.com/93924485/226225276-63e8b271-1765-49b0-8ad5-7e258f99dfef.png)



