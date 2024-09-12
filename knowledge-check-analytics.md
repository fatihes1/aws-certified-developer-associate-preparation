# Ölçme Testi: Analytics (DVA-C02)

**A news organization is using Amazon Kinesis Data Streams to consume a newswire subscription. The organization is expecting increased data flow ahead of a significant news event.**
**Which solution will improve the data stream performance while minimizing costs?**

- [ ] Split every shard in the stream to double capacity.
- [x] Identify hot shards in the stream and split them.
- [ ] Identify cold shards in the stream and merge them.
- [ ] Merge all shards in the stream for increased throughput.

Hot shard'ın bölünmesi, daha az veri akışına sahip hash anahtarların kapasitesinin iki katına çıkarılması masrafına maruz kalmadan, popüler hash anahtarlar için akış kapasitesini artırabilir.

---

**Amazon Athena supports which methods for encryption at rest? (Choose 3 answers)**

- [x] Server-side encryption with an S3-managed key (SSE-S3)
- [x] Server-side encryption with a KMS key (SSE-KMS)
- [x] Client-side encryption with a KMS key (CSE-KMS)
- [ ] Server-side encryption with a customer-provided key (SSE-C)

Amazon Athena, müşteri tarafından sağlanan anahtarla sunucu tarafı şifreleme (SSE-C) hariç listelenen şifreleme yöntemlerini destekler.

Aşağıdaki şifreleme seçenekleri desteklenmez:

-   Müşteri tarafından sağlanan anahtarlarla SSE (SSE-C)
-   İstemci tarafından yönetilen bir anahtar kullanarak istemci tarafı şifreleme
-   Asimetrik anahtarlar

---

**Which of the following is a method for filtering and querying data with Amazon Athena?**

- [x] Write SQL queries directly against data stored in S3 buckets.
- [ ] Use Amazon Redshift to load and transform data, then query using SQL.
- [ ]  Use the AWS Management Console to filter data based on tags and attributes.
- [ ] Use AWS Glue to crawl and transform data, then query using Python scripts.

Amazon Athena, Amazon S3'te depolanan verileri standart SQL kullanarak kolayca analiz etmenizi sağlayan sunucusuz bir interaktif sorgu hizmetidir. Athena ile, ek veri ambarı veya ETL araçlarına ihtiyaç duymadan, S3 bucket'larında depolanan verilere karşı doğrudan SQL sorguları yazabilirsiniz. Bu, tarih aralıkları, belirli özellikler veya değerler ve daha fazlası dahil olmak üzere çok çeşitli kriterlere dayalı olarak verileri filtrelemeyi ve sorgulamayı kolaylaştırır.

Diğer seçenekler aşağıdaki nedenlerden dolayı yanlıştır:

-   Redshift ayrı bir veri ambarı hizmetidir ve Athena ile veri sorgulamak için gerekli değildir.
-   Etiketlere ve özelliklere dayalı filtreleme, Athena'nın temel bir işlevi değildir.
-   Glue veri dönüşümü için kullanılabilse de, Athena ile veri sorgulamak için gerekli değildir.

---

**Which of the following best describes how shard capacity and scaling work in AWS Kinesis Data Streams?**

- [ ] Shard capacity can be adjusted manually as needed, and scaling is automatic based on usage.
- [x] Shard capacity and scaling are both manual processes that require configuration changes.
- [ ] Shard capacity and scaling are both automatic processes that are managed by AWS.
- [ ] Shard capacity is fixed and cannot be adjusted, and scaling is manual using additional stream instances.

AWS Kinesis Data Streams'de, her stream bir veya daha fazla shard'dan oluşur ve stream'in kapasitesi içerdiği shard sayısı tarafından belirlenir. Kullanıcılar, stream'lerinin kapasitesini ölçeklendirmek için manuel olarak shard eklemeli veya çıkarmalıdır ve bu, yapılandırma değişikliklerini içerir.

Diğer seçenekler aşağıdaki nedenlerden dolayı yanlıştır:

-   Shard kapasitesi gerektiğinde manuel olarak ayarlanabilir ve ölçeklendirme kullanıma göre otomatiktir. - Bu yanlıştır çünkü shard kapasitesi gerçekten manuel olarak ayarlanabilse de, ölçeklendirme kullanıma göre otomatik değildir. Kullanıcılar manuel olarak shard eklemeli veya çıkarmalıdır.
-   Shard kapasitesi ve ölçeklendirme, AWS tarafından yönetilen otomatik süreçlerdir. - Bu yanlıştır çünkü AWS Kinesis Data Streams, shard kapasitesini ve ölçeklendirmeyi otomatik olarak yönetmez. Kullanıcılar, stream'lerini ölçeklendirmek için shard sayısını manuel olarak yapılandırmalıdır.
-   Shard kapasitesi sabittir ve ayarlanamaz, ve ölçeklendirme ek stream örnekleri kullanılarak manuel yapılır. - Bu yanlıştır çünkü shard kapasitesi sabit değildir; shard ekleyerek veya çıkararak manuel olarak ayarlanabilir. Ölçeklendirme, ek stream örnekleri kullanmayı değil, tek bir stream içindeki shard sayısını ayarlamayı içerir.

---

**You have a set of unstructured data in an S3 bucket that you would like to analyze with custom queries. The overall project has the following requirements:**

**1.  Minimal administrative overhead**
**2.  Once the analysis is complete, no AWS resources such as tables or indexes should be retained.**
**3.  You are very familiar with SQL query syntax, and being able to apply it to this project would be extremely efficient.**

**Which AWS service should you use?**

- [x] Amazon Athena
- [ ] Amazon RDS
- [ ] Amazon Kinesis Data Stream
- [ ] Amazon Data Pipeline

Amazon Athena, minimum kurulumla yapılandırılmış, yarı yapılandırılmış veya yapılandırılmamış verileri hızlı bir şekilde sorgulamanıza olanak tanımak üzere tasarlanmıştır. SQL sorgu sözdizimini kullanır, bu nedenle bu durumda en iyi şekilde çalışacaktır.

---

**Which of the following security measures are available in AWS Kinesis Data Streams?**

- [ ] Encryption at rest using AWS KMS.
- [ ] Network isolation using VPC.
- [ ] Fine-grained access control using IAM.
- [x] All of the above.

AWS Kinesis Data Streams, verilerinizi korumaya yardımcı olmak için çeşitli güvenlik önlemleri sunar. AWS KMS kullanarak dinlenme halindeki verilerin şifrelenmesi, verilerinizin diske yazılmadan önce şifrelendiğini ve yalnızca yetkili kullanıcıların şifreleme anahtarlarına erişimi olduğunu garanti eder. VPC kullanarak ağ izolasyonu, veri akışlarınızın genel internete erişilemez olmasını ve yalnızca yetkili kullanıcıların akışa erişimini sağlar. IAM ile ince ayarlı erişim kontrolü, farklı kullanıcılar veya gruplara farklı erişim düzeyleri tanımanıza olanak tanır, böylece yalnızca yetkili kullanıcılar akıştan okuma veya yazma işlemi yapabilir.

---

**An online travel agency is developing an application in AWS to allow its customers to search for and book transportation services and hotel accommodations in cities worldwide. Their customers want to see up-to-date availability and pricing for these services, so the application needs to collect and process large amounts of data from many real-time data sources.**

**Which Amazon service should the solutions architect choose to build an application capable of ingesting large amounts of data in real-time?**

- [ ] Amazon Athena
- [ ] Amazon Elasticsearch
- [x] Amazon Kinesis
- [ ] Amazon Redshift

Amazon Kinesis, gerçek zamanlı veri toplama, işleme ve analiz için kullanılan bir analiz hizmetidir. Kinesis, dört hizmet sunar:

-   **Kinesis Data Streams**: Yüksek hacimli veri akışlarını gerçek zamanlı olarak sağlayan bir hizmettir. Bu veri akışları, yüz binlerce kaynaktan saniyede gigabaytlarca veri alabilir. KDS'den gelen veriler genellikle gerçek zamanlı panolar oluşturmak için kullanılır.
-   **Kinesis Data Firehose**: Birçok veri kaynağından veri alımı için tamamen yönetilen bir çözüm sunar. Kinesis Data Firehose ile verileri (1) Amazon Managed Service for Apache Flink, (2) S3 gibi depolama hizmetleri, (3) Amazon Elasticsearch veya (4) Amazon Redshift gibi çeşitli hedeflere yükleyebilirsiniz.
-   **Kinesis Video Streams**: Birçok video akış cihazından yüksek hacimli video verilerini almanıza olanak tanır.
-   **Amazon Managed Service for Apache Flink** (önceden Kinesis Data Analytics olarak biliniyordu): Gelen veri akışları üzerinde SQL sorguları yazmanıza yardımcı olur.

Bu senaryoda Kinesis en iyi seçimdir çünkü büyük miktarda gerçek zamanlı veriyi alabilme yeteneğine sahiptir.

Diğer bahsedilen hizmetlere bir göz atalım. Gördüğünüz gibi, bu hizmetlerin hiçbiri gerçek zamanlı veri almak için kullanılmaz, bu yüzden bu senaryo için doğru çözüm olmazlar:

-   **Amazon Redshift**: Hızlı, tamamen yönetilen, petabayt ölçeğinde bir veri ambarıdır. Redshift, yüksek performanslı veri analizi için tasarlanmış olup petabaytlarca veri saklayabilir ve işleyebilir. Redshift'te saklanan verilere mevcut iş zekası araçlarınız veya standart SQL ile erişebilirsiniz. İlişkisel veritabanı yönetim sistemi olarak çalışır ve diğer RDBMS uygulamalarıyla uyumludur.
-   **Amazon Athena**: Amazon S3'te saklanan bilgileri sorgulamak için standart SQL kullanmanıza olanak tanıyan sunucusuz bir analiz hizmetidir.
-   **Amazon Elasticsearch**: Elasticsearch'ü çalıştırmanıza yardımcı olan bir yönetilen hizmettir. 

---

**___________ is a service that makes it easy to analyze data in Amazon S3 using standard SQL.**

- [ ] Amazon Kinesis
- [ ] Amazon S3
- [x] Amazon Athena
- [ ] Amazon Redshift

Eğer verilerinizi etkileşimli olarak gözden geçirmek istiyorsanız, standart SQL kullanarak Amazon S3'teki verileri analiz etmeyi kolaylaştıran özel olarak tasarlanmış bir hizmet olan Amazon Athena hizmeti tam size göre.


---

**Which of the following statements about AWS OpenSearch Service is false? **

- [ ] Amazon OpenSearch Service is the successor to the Amazon Elasticsearch Service.
- [ ] AWS OpenSearch can ingest streaming data from Amazon CloudWatch Logs, Kinesis Data Firehose, and S3, and DynamoDB.
- [ ] OpenSearch offers tiered storage, including hot, UltraWarm and cold storage tiers. 
- [x] An AWS OpenSearch cluster can only be deployed across a maximum of 2 Availability Zones.

OpenSearch, yüksek kullanılabilirlik sağlamak için 3 AZ'ye dağıtılabilir.

---

**Which AWS solution capability enables you to build custom applications that process or analyze streaming data for specialized needs?**

- [x] Amazon Kinesis Data Streams
- [ ] Amazon Data Firehose (formerly Amazon Kinesis Firehose)
- [ ] Amazon Kinesis Video Streams
- [ ] Amazon Kinesis Elasticsearch

Amazon Kinesis üç farklı çözüm yeteneği sunar. Amazon Kinesis Streams, akış verilerini işleyen veya analiz eden özel uygulamalar oluşturmanıza olanak tanır. Amazon Data Firehose (önceden Amazon Kinesis Firehose olarak biliniyordu), akış verilerini Amazon S3, Amazon Redshift ve Amazon Elasticsearch hizmetlerine yüklemenizi sağlar.


















