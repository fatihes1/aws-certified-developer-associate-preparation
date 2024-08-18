# Ölçme Testi: Storage (DVA-C02)

**How could you configure a resource-based policy for an S3 bucket? Select two choices.**

- [ ] Attach an S3 permissions policy to an IAM user that allows the user to access the designated S3 bucket
- [X] Create a bucket policy and attach it to the bucket to grant access
- [X] Edit the bucket's Access Control List (ACL)
- [ ] Attach the IAM users that require access to the bucket to the S3 bucket's configured IAM role

Kaynak tabanlı politikalar (resource-based policy) bir kimlikten ziyade bir kaynakla ilişkilendirilir. Amazon S3 perspektifinden bakıldığında kaynak tabanlı politikalar, Erişim Kontrol Listeleri (ACL) ve bucket politikaları biçiminde kullanılabilir. Her ikisi de bir S3 grubu için kaynak tabanlı erişim politikasını uygulamak için kullanılabilir.

----------

**What is a canned access control list in Amazon S3?**

- [X] A predefined grant that contains both grantees and permissions
- [ ] A policy that is associated with a resource rather than an identity
- [ ] A policy that is associated with an identity rather than a resource
- [ ] A list of anyone who has uploaded an object to a bucket, and thus become the owner of that object

Hazır (canned) ACL, hem yetki alan kişileri hem de izinleri içeren önceden tanımlanmış bir yetkidir.

Lütfen unutmayın: S3 Bucket ACL'leri artık varsayılan olarak etkin değildir ancak S3 verilerinin güvenliğini sağlamaya yönelik geçerli seçenekler olmaya devam etmektedir.


----------

**Where does Amazon EFS store its data?**

- [ ] A bucket
- [ ]  A volume
- [ ]  A cache
- [X] A file system

Amazon EFS, şirket içi ağa bağlı depolamaya benzer; bunun aksine, sanal olarak doğrudan bağlı depolama sunan EBS ve EC2 bulut sunucusu deposu ile düz bir yapıya sahip, internet üzerinden erişilebilen nesne depolama sunan Amazon S3'tür.

----------

**How can a user upload objects of size greater than 5 GB to Amazon S3?**

- [ ] Using the REST Application Programming Interfaces (API) to upload the objects
- [ ] Using QTP scripts to application lifecycle management (ALM)
- [X] Using the multipart upload Application Programming Interfaces (API) 
- [ ] Using S3 SDK and UPLOAD as a single operation 

Boyutu 5 GB'a kadar olan nesneleri tek bir işlemle yükleme şansınız vardır. 5 GB'tan büyük nesneler için çok parçalı yükleme API'sini kullanmanız gerekir.

----------

**Which of the following must you do to enable object lock on an S3 bucket?**

- [ ] Disable versioning
- [x] Enable versioning
- [ ] Select Governance Mode 
- [ ] Select Compliance Mode

Bir S3 klasöründe nesne kilidini (object lock) etkinleştirmek için öncelikle sürüm oluşturmanın etkinleştirilmesi gerekir.

----------

**What is an ideal use case for EBS HDD volume storage?**

- [x] HDD volumes are ideal when throughput is essential.
- [ ] HDD volumes are faster than SSD volumes.
- [ ] HDD volumes are better suited for use as boot volumes for EC2 instances.
- [ ] HDD volumes are better suited for scenarios that work with smaller blocks of data.

HDD destekli birimler, büyük verilerin işlenmesi ve bilgilerin günlüğe kaydedilmesi gibi daha yüksek bir aktarım hızı gerektiren daha iyi iş yükleri için tasarlanmıştır. Yani aslında daha büyük veri bloklarıyla çalışmak üzere tasarlandılar. SSD destekli depolama, işlemsel iş yüklerini kullanan veritabanları gibi daha küçük bloklarla veya genellikle EC2 bulut sunucularınız için önyükleme birimleri olarak çalışan senaryolar için daha uygundur.

----------

**Which statements about allows and denials in S3 bucket policies are correct? (Choose 3 answers)**

- [x] By default, access is denied to an object, even without an explicit Deny within any policy.
- [x] If there is both a Deny and an Allow associated with the principal to a specific object, then access will be denied.
- [x] If there is no Deny associated with the principal to a specific object, but there is an Allow, then access will be authorized. 
- [ ] An Allow always takes precedence over a Deny.

Varsayılan olarak AWS, herhangi bir politikada açık bir 'Deny' seçeneği olmasa bile bir nesneye erişimin reddedildiğini belirtir.

Erişim elde etmek için, asıl öğenin bir bucket politikası veya ACL içinde ilişkilendirildiği veya tanımlandığı bir politika içinde bir 'Allow' özelliğinin olması gerekir.

Tanımlanmış bir 'Deny' yoksa ancak bir politika içinde 'Allow' varsa erişime izin verilir. Bununla birlikte, belirli bir nesnenin sorumlusuyla ilişkili tek bir 'Deny' varsa, o zaman bir 'Allow mevcut olsa bile, bu açık reddetme her zaman öncelikli olacak, 'Allow' seçeneğini geçersiz kılacak ve erişime izin verilmeyecektir.

Bildiğimiz gibi, Reddet her zaman 'Allow' seçenğine göre önceliklidir; bu da Stuart'ın, klasörü veya içindeki herhangi bir nesneyi silmek dışında tüm S3 eylemlerini gerçekleştirmek için s3deepdive klasörüne erişebileceği anlamına gelir.

----------

**A set of Amazon S3 objects are configured with the following S3 object lifecycle:**

**For the first 30 days, the objects are frequently accessed. After 30 days, the objects are infrequently accessed for the next 90 days. After this period of infrequent access, the objects are no longer needed. You may choose to archive or delete them.**

**Which of the following actions defined by lifecycle configuration can be used in this scenario?**

- [ ] Versioning and transition
- [ ] Expiration and versioning
- [x] Expiration and transition
- [ ] Authentication and versioning

İyi tanımlanmış bir yaşam döngüsüyle oluşturulmuş nesneleri düşünün. Başlangıçta nesnelere 30 günlük bir süre boyunca sıklıkla erişilir. Başlangıç ​​döneminden sonra, nesnelere nadiren erişilen durumlarda erişim sıklığı 90 güne kadar azalır. Bundan sonra nesnelere artık ihtiyaç duyulmaz. Bunları arşivlemeyi veya silmeyi seçebilirsiniz. Bu örnek senaryoyla eşleşen nesnelerin geçişini ve sona ermesini tanımlamak için bir yaşam döngüsü yapılandırması kullanabilirsiniz (oluşturulduktan 30 gün sonra STANDARD_IA'ya geçiş ve oluşturulduktan 90 gün sonra GLACIER'a geçiş ve belki de belirli sayıda gün sonra nesnelerin sona ermesi).

----------

**A startup is developing an application to help small businesses manage customer identification documents that are uploaded through an API. The documents are stored in Amazon S3 buckets where data is encrypted at rest. The startup wants to control its encryption keys but does not want to manage the encryption operations.**

**Which encryption method should this startup implement?**

- [ ] Use a client-side encryption and store the encryption keys in your application codes
- [x] Server-Side Encryption with customer-provided encryption keys (SSE-C)
- [ ] Server-Side Encryption for the Amazon S3 buckets with Amazon S3-managed keys (SSE-S3)
- [ ] Use a client-side encryption and store the encryption keys in AWS KMS

Amazon S3 klasörlerindeki verileri korurken üç seçeneğiniz vardır:

- Amazon S3 tarafından yönetilen anahtarlarla sunucu tarafı şifreleme (SSE-S3)

- AWS KMS'de (SSE-KMS) depolanan KMS anahtarlarıyla Sunucu Tarafı Şifreleme

- Müşteri tarafından sağlanan anahtarlarla sunucu tarafı Şifreleme (SSE-C)

Başlangıç, şifreleme anahtarlarını yönetmek istediğinden ancak şifreleme ve şifre çözme işlemlerini yönetmek istemediğinden, bu senaryonun çözümü SSE-C'dir.

Geri kalan seçenekler aşağıdaki nedenlerden dolayı yanlıştır:

AWS KMS tarafından yönetilen anahtarlara sahip sunucu tarafı şifreleme sayesinde anahtarlar KMS tarafından yönetilir; başlangıç, anahtarları yönetmek istiyor ancak şifrelemeyi gerçekleştirmek istemiyor. Bu seçenek başlangıç ​​planına uymuyor.

Şifrelemenin istemci tarafında gerçekleşmesini istediğinizde istemci tarafı şifrelemeyi kullanırsınız. Bu senaryoda veriler, S3 klasörüne aktarılmadan önce şifrelenmez, kullanımda değilken şifrelenir.

Amazon S3 tarafından yönetilen anahtarlarla sunucu tarafı şifreleme kullanıldığında, Amazon S3 her nesneyi diske kaydetmeden önce şifreler. Nesneyi aldığınızda Amazon S3 sizin için nesnenin şifresini çözer. S3 anahtarları yönetir, böylece başlangıç ​​anahtarları kontrol etmez.

----------

**In Amazon S3 server access logging, which of the following is true of the source and the target bucket?**

- [ ] A bucket must be selected
- [x] They must be in the same region.
- [ ] A target prefix is added
- [ ] They must conform to specific audit certifications.

Sunucu erişim loglamayı etkinleştirmek için hem kaynak hem de hedef bucket aynı bölgede olmalıdır. Bucket seçimi yalnızca hedef bucket için geçerliyken, hedef öneki yalnızca hedef bucket'e eklenir ve bucket'lerin kendisi yerine sunucu erişimi log kaydı işleminin denetim spesifikasyonlarına uygun olması gerekir.

----------

**You need to encrypt data containing personal data that is stored within Amazon S3. Currently, you are encrypting all data stored in S3 using SSE-S3, but are researching options. Several AWS users who should not have access to the encrypted S3 data have been able to gain access to the data. You would prefer an encryption option that allows you to grant or deny access to S3 using specific policies.**

**Which encryption can provide this level of control with minimal administrational overhead?**

- [ ] SSE-S3
- [ ] SS3-C
- [ ] CSE-C
- [x] SSE-KMS

KMS'yi kullanmak, anahtarlarınızın nasıl yönetileceği konusunda size çok daha fazla esneklik sağlar. Örneğin, KMS anahtarına erişim denetimlerini devre dışı bırakabilir, döndürebilir, uygulayabilir ve AWS Cloud Trail'i kullanarak bunların kullanımını denetleyebilirsiniz. SSE-S3 bu durumda daha az uygun bir seçenektir çünkü anahtarları sizin için yönetir; benzer şekilde, SSE -S3'ün kullanılması şifreleme sürecini son kullanıcı için görünmez hale getirir ve böylece şifreleme anahtarı sorununu anlama veya azaltma yeteneğinizi sınırlandırır. KMS, şifreleme ve şifre çözme işlemlerinin kullanıcıdan bağımsız olarak değil, kullanıcının bu işlemlere erişmesini sağlayarak farklı şekillerde izlenmesine olanak sağlar.

----------

**Which of the following is a use case for block storage in AWS?**

- [ ] Backing up data storage devices like hard drives or server file systems
- [ ] Backing up large, unstructured data
- [ ] Backing up data that is stored in files
- [x] Backing up S3

Blok depolama, sabit sürücüler, sunucu dosya sistemleri ve depolama alanı ağları veya SAN'lar gibi veri depolama aygıtlarıyla kullanılır. Bu cihazlar verileri bloklar halinde saklar. Amazon Elastic Block Store veya EBS, AWS'de EC2 bulut sunucularıyla sorunsuz bir şekilde entegre olan kalıcı blok depolama birimleri sağlayan hizmettir. EC2 bulut sunucularınızı barındıran sunuculara fiziksel olarak bağlı diskler olan bulut sunucusu depolama birimleri de vardır. Bunlar çok hızlı ancak yalnızca geçici blok depolama sağlayabilir.

Nesne depolama, neredeyse sınırsız kapasiteye sahip olma konusunda daha fazla endişe duyduğunuzda genellikle büyük, yapılandırılmamış veriler için kullanacağınız depolamadır. (S3'ü düşünün)

Adından da anlaşılacağı gibi dosya depolama, dosyalarda saklanan veriler için kullanılır. Ve bu dosyalar bir klasör hiyerarşisi içinde bulunur. Yani bunlar genellikle ağa bağlı depolama alanınız veya ağa bağlı dosya paylaşımlarınızı bir Windows veya Linux ortamında barındıran NAS cihazlarınızdır. AWS'de ise bulut tabanlı dosya depolama hizmetlerimiz için hem Elastik Dosya Sistemine (EFS) hem de FSx'e sahibiz. Bu, AWS'de blok, nesne ve dosya depolamanın temellerini kapsar.

----------

**Which of the following is a use case for EBS (Elastic Block Store)?**

- [x] Storing data that requires frequent updates.
- [ ] Saving static images associated with a website.
- [ ] Sharing storage for applications across multiple instances.
- [ ] Storing information in a table that can be queried.

EBS bir EC2 instance'ine eklenmelidir. EBS, özellikle bir web sunucusu için bir tür arka uç olarak iyi çalışır. EBS, üzerinde çalıştığınız uygulama için depolama olarak kullanışlıdır; oyunun durumunu yerel olarak takip etmek veya sunucuya kaç kullanıcının bağlandığını takip etmek gibi şeyler için.

----------

**Which AWS storage solution is best for applications that scale across multiple instances, allowing for parallel access of data?**

- [ ] Amazon RDS
- [ ] Amazon Elastic Book Store (EBS)
- [x] Amazon Elastic File System (EFS)
- [ ] AWS Storage Gateway

Amazon Elastic File System veya EFS, dosya düzeyinde depolama olarak kabul edilir ve aynı zamanda düşük gecikme süreli erişim için optimize edilmiştir, ancak EBS'den farklı olarak aynı anda birden fazla EC2 bulut sunucusunun erişimini destekler. Dosya sistemine birden çok instance tarafından erişilebildiğinden, bu durum onu ​​birden çok instance arasında ölçeklenen uygulamalar için çok iyi bir depolama seçeneği haline getirerek verilere paralel erişime olanak tanır.

EBS Multi-Attach'ın artık bir özellik olarak mevcut olması nedeniyle, bunun en iyi seçim olduğu düşünülebilir ancak uyumluluğu sınırlıdır.

Geri kalan seçenekler aşağıdaki nedenlerden dolayı yanlıştır:

- Amazon S3, herhangi bir zamanda herhangi bir miktarda veriyi depolamak ve almak için uygun olan bir nesne depolama hizmetidir. Web içeriği, medya dosyaları veya yedeklemeler gibi verileri depolamak için mükemmeldir. Ancak birden fazla örnekten yüksek hızlı paylaşımlı erişim için tasarlanmamıştır.

----------

**Which statements regarding Amazon EFS and Amazon S3 are correct? (Choose 2 answers)**

- [x] Amazon S3 charges data retrieval fees while Amazon EFS does not.
- [x] EFS has fewer storage classes than Amazon S3.
- [ ] EFS has no lifecycle management options while Amazon S3 does
- [ ] Amazon EFS offers object and file storage while Amazon S3 provides object storage only.

EFS'nin S3'ten daha iyi bir maliyet seçeneği olabilmesinin ana nedenleri, EFS'de veri alımı için herhangi bir ücret alınmaması ve daha az depolama sınıfının bulunmasıdır.

----------

**What is a "Key" for an Amazon S3 object?**

- [x] A unique identifier for an object in a bucket 
- [ ] An encryption key for an object in a bucket
- [ ] A version ID for an object in a bucket
- [ ] No choices are correct.

Nesne (object), Amazon S3'ün temel varlığıdır. Bir nesnenin dosya verilerine ek olarak meta verileri de vardır. Bucket'in içindeki her nesnenin, nesnenin benzersiz tanımlayıcısı olan tam olarak bir anahtarı (ket) vardır. Bir nesne; paket, anahtar ve sürüm kimliğiyle benzersiz bir şekilde tanımlanır.

----------

**You need to keep track of API calls made to your S3 bucket called MyLogBucket at the object level. However, your administrative team is not concerned with API tracking for other S3 buckets.**

**What step should you take to capture API calls made to MyLogBucket only?**

- [x] Enable object-level logging in the properties of MyLogBucket
- [ ] Enable AWS CloudTrail and create a trail to capture all S3 events  
- [ ] Add a bucket policy to MyLogBucket that captures API calls
- [ ] Enable AWS CloudWatch and create a trail to capture all S3 events

S3 veri olaylarını yakalama 2 şekilde yapılandırılabilir: 
- İlk olarak, S3 klasörlerinizin tümü veya bir kısmı için veri olaylarını yakalamak istiyorsanız, bunu burada gösterildiği gibi AWS CloudTrail konsolunu kullanarak takiplerinizden birinin içinden yapılandırabilirsiniz. 
- İkinci olarak, klasörünüz için AWS CloudTrail aracılığıyla henüz etkinleştirilmemişse özellikler sekmesini kullanarak klasör düzeyinde yapılandırabilirsiniz. Nesne düzeyinde günlük kaydı kutucuğunu seçtiğinizde size bunu yapılandırma seçenekleri sunulur.

Tüm S3 klasörleri için CloudTrail'in etkinleştirilmesi gerekli olmadığından, nesne düzeyinde günlüğe kaydetmeyi etkinleştirmek için MyLogBucket Özelliklerini kullanmak doğru seçimdir.

----------

**When using Amazon S3 Glacier Flexible Retrieval's expedited retrieval option, which of the following is correct?**

- [ ] Amazon S3 Glacier takes 3-5 hours to retrieve data.
- [x] Amazon S3 Glacier takes 1-5 minutes to retrieve data.
- [ ] Amazon S3 Glacier takes 5-12 hours to retrieve data.
- [ ] Amazon S3 Glacier takes 1 hour to retrieve data.  

Amazon S3 Glacier Flexible Retrieval'daki (eski adıyla Amazon Glacier) hızlandırılmış alma seçeneği, verilere hızla ihtiyaç duyulan durumlar için tasarlanmıştır. Bu seçenek, beklenmedik bir şekilde ihtiyaç duyduğunuz arşivlere kısa sürede erişmek için idealdir. Hızlandırılmış alımı seçtiğinizde Amazon S3 Glacier, istenen verileri 1 ila 5 dakika içinde teslim edebilir. Bu hızlı yanıt süresi, zamana daha az duyarlı veri erişim ihtiyaçları için tasarlanmış standart veya toplu alma seçeneklerinden önemli ölçüde daha hızlıdır

----------

**How does the server access logging feature enhance S3 bucket security?**

- [ ] By providing root cause analysis functionality for buckets
- [x] By capturing the details of requests made to buckets and their objects
- [ ] By providing auditing and governance guidance
- [ ] By creating hourly logs that track unusual bucket requests

Sunucu erişim log kaydı (server access logging) özelliği, klasörlere ve nesnelerine yapılan isteklerin ayrıntılarını yakalayarak S3 klasör güvenliğini artırır. Temel neden analizlerini gerçekleştirmek ve denetim ve yönetişim kılavuzuna uygunluğu bildirmek için gereken bilgileri sağlayarak kullanıcılara yardımcı olabilir, ancak bir özellik olarak sunucu erişimi log kaydı bu işlevleri sağlamaz. Sunucu erişim log kaydı, olağandışı paket isteklerini izleyen saatlik log kaydı olarak sağlamaz. Bunun yerine, sunucu erişim log kaydı S3 tarafından en iyi çaba esasına göre gerçekleştirilir. Log kayıtlarının kendileri derlenir ve birkaç saatte bir veya muhtemelen daha erken gönderilir. Her isteğin kaydedileceğini ve belirli bir zaman dilimi içinde belirli bir istek için bir log kaydı alacağınızı belirleyen kesin ve kesin bir kural yoktur.

----------

**A company needs to maintain system access logs for a minimum of 2 years due to financial compliance policies. Logs are rarely accessed, and access must be requested at least 12 hours in advance.**

**What is the most cost-effective method to store logs that satisfies the compliance requirement and delivers logs as requested in a timely manner?**

- [ ] Store the logs in CloudWatch logs and configure a two-year retention period.
- [ ] Store the logs in Amazon S3 in the Intelligent-Tiering storage class. Configure a lifecycle policy to delete the logs after two years.
- [x] Store the logs in Amazon S3 in the S3 Glacier Deep Archive storage class. Configure a lifecycle policy that deletes the logs after two years.
- [ ] Store the logs in Amazon S3 in the S3 One Zone-IA storage class. Configure a lifecycle policy to delete the logs after two years.

Amazon S3 Glacier Deep Archive depolama sınıfı, AWS'deki en ucuz depolama sınıfıdır ve bilgiler varsayılan 12 saatlik süre içinde alınabilir.

Diğer seçenekler, Amazon S3 Standart depolama sınıfına kıyasla bir miktar maliyet tasarrufu sağlar, ancak Akıllı Katmanlama, dosyaları yaklaşık 120 gün boyunca bir arşiv depolama sınıfına dönüştürmez. One Zone-IA depolama sınıfı bir miktar maliyet tasarrufu sağlarken, daha az dayanıklı depolama sunar, dolayısıyla günlükleri kaybetme olasılığı daha yüksektir.


  

 


