# Redis-cluster-installing-and-configuring-on-Ubuntu-20.04
Ubuntu 20.04 üzerinde Redis cluster kurulumu ve konfigürasyonu / Installing and configuring Redis cluster on Ubuntu 20.04

# Redis Nedir ? / What is Redis ? 
![image](https://user-images.githubusercontent.com/93924485/225926085-9bdce1db-27c4-43e6-919c-8292a70f4f88.png)

Redis, açık kaynaklı, hafıza-tabanlı (in-memory) bir veritabanı yönetim sistemidir. Ana amaçlarından biri, yüksek performanslı ve ölçeklenebilir veritabanları sağlamaktır. Redis, anahtar-değer (key-value) mağazası olarak çalışır ve hızlı bir şekilde erişim sağlamak için verileri bellekte tutar. Ayrıca, Redis verilerini disk üzerinde saklamak için de kullanılabilir. Redis, özellikle önbellek, oturum yönetimi, sıralama ve gerçek zamanlı uygulamalar gibi alanlarda popüler bir veritabanı çözümüdür.

Redis is an open-source, in-memory database management system. One of its main goals is to provide high-performance and scalable databases. Redis works as a key-value store and keeps data in memory for fast access. It can also be used to store data on disk. Redis is a popular database solution, especially in areas such as caching, session management, sorting, and real-time applications.

### 1-)
• Kurulumdan önce işletim sistemimizin paketlerini güncellemek için aşağıdaki komutları çalıştırıyoruz. 

• We run the following commands to update our operating system's packages before installation.

> apt-get update  
> apt-get upgrade
