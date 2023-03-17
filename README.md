# Redis-cluster-installing-and-configuring-on-Ubuntu-20.04
Ubuntu 20.04 üzerinde Redis cluster kurulumu ve konfigürasyonu / Installing and configuring Redis cluster on Ubuntu 20.04

![image](https://user-images.githubusercontent.com/93924485/225926085-9bdce1db-27c4-43e6-919c-8292a70f4f88.png)

# Redis Nedir ? 

* Redis, açık kaynaklı, hafıza-tabanlı (in-memory) bir veritabanı yönetim sistemidir. Ana amaçlarından biri, yüksek performanslı ve ölçeklenebilir veritabanları sağlamaktır. Redis, anahtar-değer (key-value) mağazası olarak çalışır ve hızlı bir şekilde erişim sağlamak için verileri bellekte tutar. Ayrıca, Redis verilerini disk üzerinde saklamak için de kullanılabilir. Redis, özellikle önbellek, oturum yönetimi, sıralama ve gerçek zamanlı uygulamalar gibi alanlarda popüler bir veritabanı çözümüdür.

# What is Redis ?

* Redis is an open-source, in-memory database management system. One of its main goals is to provide high-performance and scalable databases. Redis works as a key-value store and keeps data in memory for fast access. It can also be used to store data on disk. Redis is a popular database solution, especially in areas such as caching, session management, sorting, and real-time applications.

### 1-) Kurulumdan önce işletim sistemimizin paketlerini güncellemek için aşağıdaki komutları çalıştırıyoruz.
###     We run the following commands to update our operating system's packages before installation.

> sudo apt-get update  
> sudo apt-get upgrade

### 2-) Redis Cluster kurulumu ve konfigürasyonu / Redis Cluster installation and configuration. 

![image](https://user-images.githubusercontent.com/93924485/225933102-eb5f2f6a-e3db-4f4a-b180-87c10fcf8081.png)

• Redis kurulumu yapmak için aşağıdaki komutu çalıştırmamız gerekiyor. 

• We need to run the following command to install Redis.

> sudo apt-get install redis-server

> sudo systemctl disable redis-server.service

### 3-)  Kurulumu tamamladıktan sonra aşağıdaki komutları çalıştırarak portlara izin vermemiz gerekiyor.  
###      After completing the installation, we need to run the following commands to allow access to the ports.

> sudo ufw allow 7000 
   
> sudo ufw allow 7001
  
> sudo ufw allow 17000
  
> sudo ufw allow 17001
        
![image](https://user-images.githubusercontent.com/93924485/225950468-b8c31ff3-d03f-4355-a106-5f38d201d06c.png)


### 4-) Eğer işletim sisteminizde Vim kurulu değilse bu işlemi gerçekleştirmeden önce Vim editörünü kurmanız gerekmektedir. Vim editörünü kurmak için aşağıdaki komutu çalıştırabilirsiniz.

### If Vim is not installed on your operating system, you need to install the Vim editor before performing this process. To install the Vim editor, you can run the following command.


> sudo apt-get install vim


### 4-) Aşağıdaki komutu çalıştırarak /etc/rc.local dosyasını oluşturun ve düzenleyin.
###     Create and edit the /etc/rc.local file by running the following command.

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
###     To make a file executable, you should run the following command.


> chmod +x /etc/rc.local


• Dosyayı kaydedip çıkın.

• Save the file and exit.

>:wq!


### 6-) Aşağıdaki komutu çalıştırarak /etc/sysctl.conf dosyasını düzenleyin.
###     Edit the /etc/sysctl.conf file by running the following command.


> vim /etc/sysctl.conf


``` bash 

vm.overcommit_memory=1

``` 

 ![image](https://user-images.githubusercontent.com/93924485/225975468-5845e2fa-bfd3-461f-8138-cc34a556c695.png)



• Dosyayı kaydedip çıkın.

• Save the file and exit.


>:wq!


### 7-) Aşağıdaki komutları çalıştırarak bazı gerekli klasörleri oluşturun.
###     Create some necessary folders by running the following commands.


> sudo mkdir /etc/redis/cluster 

> sudo mkdir /etc/redis/cluster/7000

> sudo mkdir /var/lib/redis/7000

> sudo mkdir /etc/redis/cluster/7001

> mkdir /etc/redis/cluster/7001



### 8-) Aşağıdaki komutu çalıştırarak /etc/redis/cluster/7000/redis_7000.conf dosyasını oluşturun ve düzenleyin.
###     Create and edit the /etc/redis/cluster/7000/redis_7000.conf file by running the following command.



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
requirepass [ACCESSKEY]
masterauth [ACCESSKEY]


``` 


![image](https://user-images.githubusercontent.com/93924485/225979204-462dac88-d88e-42a5-9eb1-7dae574bf173.png)



• Dosyayı kaydedip çıkın.

• Save the file and exit.


>:wq!









### 8-) Aşağıdaki komutu çalıştırarak /etc/redis/cluster/7001/redis_7001.conf dosyasını oluşturun ve düzenleyin.
###     Create and edit the /etc/redis/cluster/7001/redis_7001.conf file by running the following command.



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
masterauth [ACCESSKEY]
requirepass [ACCESSKEY]


``` 


![image](https://user-images.githubusercontent.com/93924485/225979588-73e1d00a-8c33-47d9-937b-c3cd9ac115fe.png)




• Dosyayı kaydedip çıkın.

• Save the file and exit.


>:wq!



### 9-) Redis Server servisleri için bir redis kullanıcısı ve bir redis grubu oluşturun ve aşağıdaki komutları çalıştırarak onlara doğru izinleri verin.
###     Create a redis user and a redis group for the Redis Server services and give them the correct permissions by running the following commands.


> sudo chown redis:redis -R /var/lib/redis 

> sudo chmod 770 -R /var/lib/redis

> sudo chown redis:redis -R /etc/redis




