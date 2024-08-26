# Ölçme Testi: Databases (DVA-C02)

**Which of the following best describes on-demand capacity mode? **

 - [ ] This mode enables you to specify how much read and write capacity you need for your application.
 - [ ] This mode enables you to use auto scaling to control your read and write capacity.
 - [x] This mode enables DynamoDB, the service, to adjust how many reads and writes are provided for the application
 - [ ] This mode enables you to calculate the appropriate amount of reads and writes for your application.

On-demand (isteğe bağlı) modu seçtiğinizde DynamoDB, daha önce ulaşılan herhangi bir trafik düzeyine doğru artan veya azalan iş yüklerinize anında uyum sağlar. Bir iş yükünün trafik düzeyi yeni bir zirveye ulaşırsa DynamoDB iş yüküne uyum sağlamak için hızla işe koyulur. Bu modda otomatik ölçeklendirmeye veya hesaplamalara gerek yoktur.

---

**In which of the following cases would you choose to use a relational database over a non-relational database?**

- [x] When you have data that is highly structured. 
- [ ] When you need a managed database service. 
- [ ] When you need transactional processing support. 
- [ ] When you have read-heavy workloads. 

İlişkisel veritabanları yüksek düzeyde yapılandırılmış verileri yönetmek için kullanılır. Geriye kalan seçenekler yanlıştır çünkü:

- Transactional processing, DynamoDB ve DocumentDB gibi ilişkisel olmayan veritabanlarına ihtiyaç olduğunu gösterir.
- Yönetilen veri tabanları ve yönetilmeyen veri tabanları, ilişkisel bir veri tabanına mı yoksa ilişkisel olmayan bir veri tabanına mı ihtiyacınız olduğunu göstermez.
- Okuma ağırlıklı iş yükleri her iki veritabanı türünde de yönetilebilir.

---

**Which of the following cache engines does Amazon ElastiCache support?**

- [ ] Amazon ElastiCache supports Memcached only.
- [x] Amazon ElastiCache supports Memcached and Redis. 
- [ ] Amazon ElastiCache supports Memcached and Hazelcast. 
- [ ] Amazon ElastiCache supports Memcached and Hazelcast.

Amazon ElastiCache tarafından desteklenen önbellek motorları (engine) Memcached ve Redis'tir.

---

**Which Amazon RDS database engine was built by AWS and is entirely cloud-native?**

- [ ] PostgreSQL
- [ ] MySQL
- [ ] Redshift
- [x] Aurora

Amazon Aurora, sıfırdan bulutta yerel olacak şekilde tasarlanıp oluşturulduğundan yüksek performanslı bir veritabanı hizmetidir. Bu nedenle, RDS'de Aurora hizmetini seçmenin hız ve kullanılabilirlik avantajları vardır.

---

**What choice best describes how Amazon Aurora maintains current data between its master database and each database replica?**

- [ ] The master database sends updates to the replica databases using synchronous replication. 
- [ ] The master database sends updates to the replica databases using asynchronous replication.
- [ ] The master database sends updates to the replica databases using either synchronous or asynchronous replication, depending on the database configuration.
- [x] The master database and the replicas share the same storage layer, so no replication is needed.

Depolama katmanı (storage layer), hesaplama katmanına (compute layer) tek bir mantıksal birim (volume) olarak sunulur. Bu aynı tek mantıksal birim, ister ana isterse okuma kopyası olsun, bilgi işlem katmanında yer alan tüm işlem örnekleri arasında paylaşılır; böylece okuma kopyaları, ana kopyanın kendisiyle neredeyse aynı sorgu performansını elde edebilir.

RDS ile karşılaştırıldığında, verilerin çoğaltma açısından yönetimi temel olarak farklıdır. RDS ile verilerin ana sunucudan kopyalarının her birine kopyalanması gerekir. Öte yandan Aurora, tüm işlem instance'lerin arasında tek bir mantıksal birim kullandığından ve paylaştığından çoğaltmaya gerek yoktur.


---

**Amazon Aurora replicates data _____ ways across multiple availability zones.**

- [ ] three
- [ ] five
- [x] six
- [ ] ten

Aurora, verilerinizi birden fazla kullanılabilirlik bölgesinde altı kere çoğalttığı için yüksek düzeyde kullanılabilir, hataya dayanıklı ve kendi kendini iyileştirebilecek şekilde tasarlanmıştır.

---

**Which feature optimizes your RDS database to ensure it is capable of meeting the fluctuations of your load?**

- [ ] automatic backup
- [ ] parameter groups
- [ ] encryption using your own customer master key
- [x] autoscaling

Zamanla veritabanı iş yükünüz dalgalanacaktır. Depolama otomatik ölçeklendirme (storage autoscaling) adı verilen bir özellik aracılığıyla depolama alanınızı ölçeklendirerek, hem depolama hem de bilgi işlem açısından yükünüzün taleplerini karşılayabildiğinden emin olmak için RDS veritabanınızı optimize edebilirsiniz.

--- 

**Which type of Amazon Aurora connection endpoint load balances connections across the read replica fleet within the cluster?**

- [ ] Cluster Endpoints
- [ ] Custom Endpoints
- [x] Reader Endpoints
- [ ] Instance Endpoints

Amazon Aurora'nın sunduğu 4 farklı bağlantı endpoint türü vardır. Hangisini kullanacağınız gereksinimlerinize bağlıdır. Şimdi hızlıca her birini gözden geçirelim:

-   **Cluster Endpoint:** Cluster endpoint, mevcut master veritabanı instance'ı ile eşlenir. Cluster endpoint'ini kullanmak, uygulamanızın master instance üzerinde okuma ve yazma gerçekleştirmesine olanak tanır.
    
-   **Reader Endpoint:** Reader endpoint, cluster içindeki read replica instance'ları arasında bağlantıları load balance eder.
    
-   **Custom Endpoint:** Custom endpoint, seçtiğiniz ve custom endpoint içinde kaydettiğiniz bir dizi cluster instance'ı arasında bağlantıları load balance (yük dengeleme) eder. Custom endpoint'ler, instance'ları instance boyutuna göre veya belki belirli bir veritabanı parameter group'una göre gruplandırmak için kullanılabilir. Daha sonra custom endpoint'i organizasyonunuz içindeki belirli bir rol veya görev için kullanabilirsiniz. Örneğin, ay sonu raporları oluşturma gereksiniminiz olabilir, bu nedenle özellikle bu görev için kurulmuş bir custom endpoint'e bağlanabilirsiniz.
    
-   **Instance Endpoint:** Instance endpoint, doğrudan bir cluster instance'ına eşlenir. Her cluster instance'ının kendi instance endpoint'i vardır. İsteklerinize hangi instance'ın hizmet etmesi gerektiği konusunda ince ayarlı bir kontrole ihtiyaç duyduğunuzda bir instance endpoint kullanabilirsiniz. Genel bir kural olarak, okuma işleminin yoğun olduğu workload'lar reader endpoint üzerinden bağlanmalıdır.


---

**How do cloud database-as-a-service (DBaaS) options like Amazon RDS and DynamoDB differ from on-premise databases? (Choose 3 answers)**

- [x] AWS database services offer Incremental license costs instead of full license costs.
- [x] AWS database services manage the underlying infrastructure.
- [x] AWS database services offer a pay-for-use pricing model.
- [ ] AWS database services offer faster processing speeds than on-premise databases.

AWS veritabanı hizmetleri ile şirket içi veritabanları arasındaki fark, belirli veritabanı performansıyla değil, kurulum ve yönetim açısından kullanım kolaylığı ve sınırlı başlangıç ​​yatırımıyla ilgilidir.


---

**Which of the following AWS NoSQL database services is a managed key-value store?**

- [x] Amazon DynamoDB
- [ ] Amazon Keyspaces
- [ ] Amazon Neptune
- [ ] Amazon DocumentDB

Anahtar-Değer Veritabanları genellikle NoSQL veritabanının en basit türü olarak kabul edilir. Genellikle ilişkisel veritabanlarından daha esnektirler ve okuma ve yazma işlemleri için hızlı performans sunarlar. Anahtar-Değer deposu olan AWS tarafından yönetilen NoSQL veritabanı DynamoDB'dir. Anahtar-Değer depoları, ilişkisel dizileri depolamak, almak ve yönetmek için tasarlanmıştır ve büyük miktarlarda verilerle çalışmaya çok uygundur.

---

**Amazon Aurora replicates data across how many availability zones by default?**

- [ ] 1
- [ ] 2
- [x] 3
- [ ] 4

Aurora, verilerinizin birden fazla kopyasını **üç** Availability Zone'da tuttuğu için, bir disk arızası sonucunda veri kaybetme olasılığı büyük ölçüde azaltılmıştır. Aurora, cluster volume'u oluşturan disk volume'lerindeki arızaları otomatik olarak tespit eder. Bir disk volume'ünün bir segmenti arızalandığında, Aurora bu segmenti hemen onarır. Aurora disk segmentini onardığında, onarılan segmentteki verilerin güncel olduğundan emin olmak için cluster volume'u oluşturan diğer volume'lerdeki verileri kullanır. Sonuç olarak, Aurora veri kaybını önler ve bir disk arızasından kurtulmak için point-in-time restore yapma ihtiyacını azaltır.

---

**Which of these allows Amazon RDS to provide failover support for DB instances?**

- [x] Multi-AZ deployments
- [ ] DB snapshots
- [ ] Security Groups
- [ ] Performance Insights

Amazon RDS, Multi-AZ dağıtımlarını kullanarak DB instance'ları için yüksek kullanılabilirlik ve failover desteği sağlar. Oracle, PostgreSQL, MySQL ve MariaDB DB instance'ları için Multi-AZ dağıtımları Amazon teknolojisini kullanırken, SQL Server DB instance'ları SQL Server Mirroring'i kullanır.

---

**A company will migrate its Amazon RDS MySQL v5.7 databases to Amazon Aurora clusters to increase the resilience of business-critical applications.**

**The new database must offer continuous availability with zero downtime in the event that a primary database fails.**

**Which Amazon Aurora database configuration will be most effective based on the company's requirements?**

- [x] A Global Database configuration
- [ ] An Aurora Serverless v2 configuration
- [ ] A cross-region read replica configuration
- [ ] Multi-AZ deployments with Aurora Replicas

Amazon Aurora, yüksek kullanılabilirlik ve dayanıklılık sunmak üzere tasarlanmıştır. Aurora, veritabanı volume'ünüzü otomatik olarak birçok diske yayılmış 10GB'lık segmentlere böler. Veritabanı volume'ünüzün her 10GB'lık parçası, üç Availability Zone'a yayılmış şekilde altı kez çoğaltılır. Amazon Aurora, veritabanı yazma kullanılabilirliğini etkilemeden iki adete kadar veri kopyasının kaybını ve okuma kullanılabilirliğini etkilemeden üç adete kadar veri kopyasının kaybını otomatik olarak ele alacak şekilde tasarlanmıştır. Aurora Replica'ları, Amazon Aurora'nın yüksek kullanılabilirlik modelinin ayrılmaz bir parçasıdır.

İşte bu konfigürasyonun şirketin gereksinimleri için neden en etkili olduğu:

-   Yüksek Kullanılabilirlik ve Dayanıklılık: Amazon Aurora'yı birden fazla Availability Zone'da (Multi-AZ) dağıtarak, veritabanı bir bölge içindeki farklı fiziksel konumlara yayılır. Bu, doğal afetler, elektrik kesintileri veya sistem arızaları gibi olaylar nedeniyle tam bir hizmet kesintisi riskini önemli ölçüde azaltır.
-   Otomatik Failover: Multi-AZ dağıtımında, birincil instance arızalanırsa Aurora otomatik olarak Aurora Replica'larından birine failover gerçekleştirir ve kesintisiz hizmet sağlar. Bu süreç sorunsuz bir şekilde gerçekleşir ve manuel müdahale gerektirmez, bu da iş açısından kritik uygulamaların sürekli kullanılabilirliğini sağlamak için idealdir.
-   Okuma Ölçeklendirme: Failover yeteneklerinin yanı sıra, Aurora Replica'ları başka bir kritik işlevi yerine getirir - okuma ölçeklendirmesine olanak tanır. Bu, özellikle yüksek okuma talebi olan uygulamalar için faydalıdır, çünkü okuma yükünü dağıtmaya ve uygulamanın genel performansını artırmaya yardımcı olur.
-   MySQL ile Geriye Dönük Uyumluluk: Şirket Amazon RDS MySQL v5.7'den geçiş yaptığı için, Amazon Aurora'nın MySQL veritabanlarıyla tamamen uyumlu olduğunu belirtmek de önemlidir. Bu, geçiş sürecini basitleştirir ve mevcut uygulamaların önemli değişiklikler gerektirmeden çalışmaya devam etmesini sağlar.


---

**Which of these databases does not use Elastic Block Store volumes?**

- [x] Amazon Aurora
- [ ] MySQL
- [ ] MariaDB
- [ ] PostgreSQL

Amazon Aurora, paylaşılan bir cluster depolama mimarisi kullanır ve EBS'yi kullanmaz.

--- 

**What are the use cases for Amazon ElastiCache? (Choose 3 answers)**

- [ ] persistent data storage
- [x] in-memory data storage
- [x] improving read access performance
- [x] caches using secure, network-attached RAM

ElastiCache, veri kayıtlarınızın tek versiyonunu depolamak için asla kullanılmamalıdır, çünkü bir önbellek geçici bir veri deposu olarak tasarlanmıştır. Bu nedenle, birincil veri kayıtlarıyla çalışırken olduğu gibi veri kalıcılığı gerekli olduğunda veya okuma performansı yerine yazma performansına ihtiyaç duyduğumuzda, ElastiCache yerine kalıcı bir veri deposu kullanılmalıdır.

 


















