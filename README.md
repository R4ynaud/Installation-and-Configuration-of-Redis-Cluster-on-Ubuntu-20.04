# Redis-cluster-installing-and-configuring-on-Ubuntu-20.04
Ubuntu 20.04 üzerinde Redis cluster kurulumu ve konfigürasyonu / Installing and configuring Redis cluster on Ubuntu 20.04

![image](https://user-images.githubusercontent.com/93924485/225926085-9bdce1db-27c4-43e6-919c-8292a70f4f88.png)

# Redis Nedir ? 

* Redis, açık kaynaklı, hafıza-tabanlı (in-memory) bir veritabanı yönetim sistemidir. Ana amaçlarından biri, yüksek performanslı ve ölçeklenebilir veritabanları sağlamaktır. Redis, anahtar-değer (key-value) mağazası olarak çalışır ve hızlı bir şekilde erişim sağlamak için verileri bellekte tutar. Ayrıca, Redis verilerini disk üzerinde saklamak için de kullanılabilir. Redis, özellikle önbellek, oturum yönetimi, sıralama ve gerçek zamanlı uygulamalar gibi alanlarda popüler bir veritabanı çözümüdür.

# What is Redis ?

* Redis is an open-source, in-memory database management system. One of its main goals is to provide high-performance and scalable databases. Redis works as a key-value store and keeps data in memory for fast access. It can also be used to store data on disk. Redis is a popular database solution, especially in areas such as caching, session management, sorting, and real-time applications.

### 1-)
• Kurulumdan önce işletim sistemimizin paketlerini güncellemek için aşağıdaki komutları çalıştırıyoruz. 

• We run the following commands to update our operating system's packages before installation.

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


.
