# Ölçme Testi: Compute (DVA-C02)

**What is the purpose of the launch template in AWS Auto Scaling?**

 - [ ] To configure default storage volume for auto-scaling group instances
 - [ ] To identify the best instance type for an auto-scaling group
 - [x] To build a standard configuration to launch instances for your auto-scaling groups
 - [ ] To configure security groups within an existing auto-scaling group

Otomatik ölçekleme grubunu yapılandırırken başlatma şablonunun birincil amacı, otomatik ölçekleme gruplarınız için instance'leri başlatmak üzere standart bir yapılandırma oluşturmaktır. Depolama hacimlerini yapılandırmak otomatik ölçeklemenin genel bir işlevidir ve en iyi instance türünü belirlemek ile mevcut bir otomatik ölçekleme grubundaki güvenlik gruplarını yapılandırmak, başlatma şablonunun birincil amaçları arasında yer almaz.

---

**An Amazon EC2 instance store provides temporary block-level storage for your instance. Ephemeral storage is ideal for ________________.**

- [ ] persistent data
- [ ] storing critical system files
- [ ] high-performance storage of user files
- [x] non-persistent data

Amazon EC2 Bulut Sunucusu Deposu, bulut sunucunuz için geçici blok düzeyinde (temporary block-level storage) depolama sağlar. Instance buffer, arabellekler, önbellekler, karalama verileri (scratch data) ve diğer geçici içerikler gibi sık sık değişen bilgilerin geçici olarak depolanması için idealdir. Geçici depolama, kalıcı olmayan (non-persistent) veriler için idealdir.

---

**A company is using AWS Lambda for an event-driven application. The developer has observed high latency for invocations occurring during peak periods. Which of the following actions can the developer take to reduce latency?**

- [ ] Increase function timeout
- [ ] Configure reserved concurrency
- [x] Configure provisioned concurrency
- [ ] Decrease function memory

Provisioned concurrency, kullanıcı tarafından tanımlandığı gibi yürütme ortamlarını başlatarak gecikmeyi azaltmak için kullanılabilir; bu ortamlar, herhangi bir işlev çağrısına hemen yanıt verebilir.

Provisioned concurrency'nin bir maliyeti vardır ve bu ayarlar belirli bir Lambda işlevinin sürümüne veya bir takma adı (alias) uygulamak için kullanılabilir.

Kalan seçenekler aşağıdaki nedenlerle yanlıştır:

-   İşlev zaman aşımını artırmak, bir yürütme süresini azaltabilir, ancak gecikmeyi azaltmaz.
-   Reserved concurrency, kullanıcı tarafından tanımlanan instance'ların bir işlev için aynı anda ve münhasır olarak mevcut olmasını sağlar. Ayrıca, işlevlerin rezerve edilmemiş concurrency kullanmalarını sınırlayarak, bir işlevin belirli bir AWS bölgesinde bir hesaba sağlanan concurrency miktarını aşmasını engeller.
-   İşlev belleğini azaltmak, gecikmenin azalmasına neden olmaz.

---

**What is an environment in Elastic Beanstalk?**

- [ ] A section of deployable code that points to Amazon S3
- [x] A collection of AWS resources running an application version
- [ ] An EC2 instance containing an application's code
- [ ] A combination of components in which you can build your application

Bir AWS Elastic Beanstalk ortamı, bir uygulama sürümünü çalıştıran AWS kaynaklarının bir koleksiyonudur. Birden fazla uygulama sürümünü çalıştırmanız gerektiğinde birden fazla ortam dağıtabilirsiniz. Instance'ın, geliştirme (development), entegrasyon (integration) ve üretim (production) ortamlarınız olabilir. Bir uygulama sürümü, dağıtılabilir kodun belirli bir bölümüne çok özel bir referanstır. Uygulama sürümü, ortamdan ziyade genellikle dağıtılabilir kodun bulunabileceği S3'e işaret eder. Bir ortam, AWS kaynaklarına dağıtılan uygulama sürümüdür ve sadece yüklenmiş kodunuzla bir EC2 instance'ı değildir. Platform, Elastic Beanstalk kullanarak uygulamanızı oluşturabileceğiniz bileşenlerin bir birleşimidir.

---

**Which of the following best describes the Elastic Beanstalk health agent?**

- [ ] A separate EC2 instance that runs health checks
- [ ] A CloudWatch custom alarm
- [ ] An object-oriented database that stores health checks
- [x] A daemon process that runs on each EC2 instance in your environment

Elastic Beanstalk sağlık ajanı, ortamınızdaki her EC2 instance'ında çalışan bir daemon (arka planda çalışan) işlem olarak en iyi şekilde tanımlanır. Bu ajan, işletim sistemi ve uygulama seviyesindeki sağlık metriklerini izler ve sorunları Elastic Beanstalk'a rapor eder.

---

**Amazon EC2 provides virtual computing environments known as _____.**

- [x] instances
- [ ] containers
- [ ] clusters
- [ ] microservices

Amazon EC2, sanal bilgisayar ortamları olarak bilinen instance'kar sağlar.

Bir instance başlattığınızda, belirttiğiniz instance türü, instance'niz için kullanılan ana bilgisayarın donanımını belirler. Her instance türü, farklı hesaplama, bellek ve depolama yetenekleri sunar ve bu yeteneklere göre instance ailelerine gruplandırılmıştır. Instance'nizi çalıştırmayı planladığınız uygulamanın veya yazılımın gereksinimlerine göre bir instance türü seçin.

---

**You are planning the development of a serverless application. You know that you have many Lambda functions to write and test. You want to develop and test your code quickly and locally using scripts and command lines.
Which one of the following tools will you choose?**

- [ ] AWS Serverless Application Repository
- [ ] Amazon Aurora Serverless
- [ ] AWS Step Functions
- [ ] AWS Serverless Application Model (SAM) Command Line Interface CLI

AWS Serverless Application Model (SAM) Command Line Interface (CLI), geliştiricilerin sunucusuz uygulamalar oluşturmak, test etmek ve yönetmek için betikler kullanabileceği bir ortam oluşturmak için kullanılan bir araçtır. AWS SAM CLI, geliştiricilerin daha hızlı bir şekilde geliştirme yapmasını sağlar.

Kalan seçenekler aşağıdaki nedenlerle yanlıştır:

-   AWS Serverless Application Repository, sunucusuz uygulamalar için yönetilen bir havuzdur; geliştirme ve test aracı değildir.
-   Amazon Aurora Serverless, Amazon Aurora için talep üzerine otomatik ölçeklenen bir yapılandırmadır. Bir veritabanıdır.
-   AWS CloudFormation, öngörülebilir dağıtımlar için AWS altyapısını oluşturmanıza ve sağlamanıza olanak tanıyan bir hizmettir. Lambda işlevlerinizin hızlı geliştirilmesini ve test edilmesini sağlar.
-   AWS Step Functions hizmeti, geliştiricilerin dağıtılmış uygulamalar oluşturmak veya süreçleri otomatikleştirmek için kullandığı düşük kodlu görsel bir iş akışıdır. AWS Step Functions, girişler alıp çıktılar üreten bağımlı durumlar koleksiyonları olan durum makineleri temelindedir. Durumlar, belirli görevleri yerine getirmek için kullanılır.

---

**In Elastic Beanstalk, which of the following refers to a specific, labeled iteration of deployable code for a web application?**

- [ ] An environment
- [x] An application version
- [ ] A configuration template
- [ ] A copyright

Elastic Beanstalk'ta uygulama sürümü (application version), bir web uygulaması için dağıtılabilir kodun belirli, etiketli bir yinelemesini ifade eder.

---

**Which statements about dynamic scaling in AWS autoscaling are correct? (Choose 3 answers)**

- [x] The CloudWatch alarms that target-tracked autoscaling creates will be automatically removed if you ever delete the scaling policy.
- [x] You should not delete any of the CloudWatch alarms that target tracked autoscaling creates.
- [x] When using target tracking, you should scale back slowly in order to ensure the best availability and experience for users.
- [ ] Using target tracking with larger autoscaling groups makes it much harder for the system to keep the metrics at the tracked level than it is with smaller autoscaling groups.

Akılda tutulması gereken bir şey, daha küçük otomatik ölçekleme gruplarınız (çok fazla instance'ın bulunmadığı gruplar) olduğunda, sistemin metrikleri izlenen seviyede tutmasının çok daha zor hale gelmesidir. Sistem, kullanıcılar için en iyi kullanılabilirlik ve deneyimi sağlamak amacıyla yavaşça geri ölçeklenir. Son olarak, hedef izlemeli otomatik ölçeklemenin oluşturduğu CloudWatch alarmlarını silmemeniz önemlidir; aksi takdirde düzgün çalışmayacaktır. Ölçekleme politikasını sildiğinizde bu alarmlar otomatik olarak kaldırılacaktır.

---

**What is the configuration template within Elastic Beanstalk?**

- [ ] The programming language you use to build your application
- [ ] A collection of different elements, such as application versions
- [x] A baseline for creating a new, unique environment
- [ ] The collection of components in which you can build your application

Yapılandırma şablonu (configuration template), Elastic Beanstalk'ı kullanarak yeni ve benzersiz bir ortam oluşturmaya yönelik bir temel (baseline) oluşturur.

---

**When you launch an Amazon EC2 instance from an AMI, a(n) ____ is created for that instance, which contains all the information necessary to boot the instance.**

- [ ] EC2 configuration
- [ ] instance manifest
- [x] root storage device
- [ ] virtual boot loader

Bir AMI'den her bulut sunucusu başlattığınızda, bu bulut sunucusu için bir kök depolama cihazı (root storage device) oluşturulur. Kök depolama aygıtı, örneği başlatmak için gerekli tüm bilgileri içerir. Bir AMI oluşturduğunuzda veya blok cihaz eşlemesini kullanarak bir örneği başlattığınızda, kök cihaz birimine ek olarak depolama birimlerini de belirleyebilirsiniz.

---

**What is the purpose of the Lambda handler?**

- [ ] It is an event source for Lambda.
- [x] It is the name of the function in your code and the service’s entry point to your code.
- [ ] It manages all configurations for the Lambda function behind the scenes.
- [ ] It is the poller that Lambda uses for stream-based invocations.

Lambda işleyicisi (lambda handler) yazdığınız kodun içindeki işlevin adıdır. Lambda, bir hizmet olarak bu adı kodunuza giriş noktası olarak kullanır. İşlev adını varsayılan lambda_handler yerine değiştirirseniz Lambda konsolunda da bunu güncellemeniz gerekeceğini unutmayın, böylece Lambda kodunuzu düzgün şekilde çalıştırabilir.

---

**In AWS autoscaling, _____ scaling uses machine learning to understand your average loads and provisions your instances based on training data.**

- [x] predictive
- [ ] dynamic
- [ ] scheduled
- [ ] manual

Bir otomatik ölçeklendirme grubu içindeki instance'lerin sayısını değiştirmenin temel olarak dört farklı yolu vardır. Bunlardan biri tahmine dayalı ölçeklendirmedir. Bu, ortalama yüklerinizi anlamak ve instance'lerinizi eğitim verilerine göre hazırlamak için makine öğrenimini kullanır.

--- 

**Which Amazon EC2 instance family is ideal for applications that manage real-time unstructured data processing, or distributed web cache stores?**

- [x] Memory-optimized
- [ ] Compute-optimized
- [ ] Storage-optimized
- [ ] General Purpose

Bellek için optimize edilmiş instance türleri öncelikle, yapılandırılmamış verilerin gerçek zamanlı işlenmesini gerçekleştirmek gibi büyük ölçekli, kurumsal sınıf bellek içi uygulamalar veya SAP HANA gibi bellek içi veritabanları için kullanılır.

---

**Which statement best describes an Amazon Machine Image (AMI)?**

- [ ] A temporary virtual machine created during horizontal scaling
- [ ] A virtual machine backup file on a local server hard drive
- [x] A preconfigured template for your EC2 instances
- [ ] A VMware configuration file for any network deployment

Amazon EC2, bulut sunucularınız için önceden yapılandırılmış şablonlar olan Amazon Machine Images (AMI'ler) sunar.

--- 

**Which of the following instance purchasing options would be recommended for a large insurance company that has a long-term, predictable workload of processing claim data files?**

- [x] Reserved
- [ ] On-demand
- [ ] Scheduled
- [ ] Spot

Reserved Instances, Amazon EC2 maliyetlerinizde On-Demand Instance fiyatlarına kıyasla önemli tasarruflar sağlar ve üç yıla kadar önemli bir indirimle satın alınabilir. Bu süre zarfı, düzenli ve tahmin edilebilir iş yüklerine sahip büyük bir şirket için idealdir, instance'ın toplu işlem yapmak gibi. Planlanmış Instance'lerde, günlük, haftalık veya aylık olarak tekrar eden bir kapasiteyi belirli bir başlangıç zamanı ve süresi ile bir yıllık bir süre için rezerve edebilirsiniz; bu, bu kullanım senaryosunda yeterince uzun değildir. Spot Instances, uygulamalarınızın ne zaman çalışacağı konusunda esnek olabiliyorsanız ve uygulamalarınız kesintiye uğrayabiliyorsa maliyet etkin bir seçenektir. Instance'ın, Spot Instances arka plan işlemleri ve isteğe bağlı görevler için uygun bir seçenektir.

---

**Amazon EC2 Auto Scaling offers what service?**

- [ ] It manages the vertical scaling of EC2 instances
- [x] It manages the horizontal scaling of EC2 instances
- [ ] It manages the software patching of EC2 instances
- [ ] It manages the rightsizing of EC2 instances

Otomatik Ölçeklendirme, kullanıcıların EC2 kaynaklarını koşullara göre veya manuel müdahaleyle otomatik olarak içe veya dışa veya yatay olarak ölçeklendirmesine olanak tanıyan bir hizmettir. EC2 bulut sunucularını ölçeklendirmek sorunsuz bir süreçtir. Mevcut bir örneğin boyutunun değiştirilmesini gerektirecek şekilde instance'leri dikey olarak ölçeklendirmez.

---

**Your development team is helping with migrating your company to the cloud. They are working with configuring EC2 instances with bootstrap scripts using user data. What specifically can you do with user data? (Choose 2 answers)**

- [x] You can specify parameters for configuring your instance.
- [ ] You can retrieve the local IP address of the instance using http://169.254.169.254/latest/user-data.
- [ ] You can retrieve data about the user of the EC2 instance.
- [x] You build generic AMIs that can be modified by configuration files supplied at launch time.

Instance'nizi başlatırken sağladığınız kullanıcı verilerine erişebilir ve örneğinizi yapılandırmak için parametreler belirleyebilir veya basit bir komut dosyası ekleyebilirsiniz. Bu verileri, başlatma sırasında sağlanan yapılandırma dosyaları tarafından değiştirilebilecek daha genel AMI'ler oluşturmak için de kullanabilirsiniz.

---

**Lambda functions are ______ so that Lambda can rapidly launch as many copies of the function as needed to scale to the rate of incoming events.**

- [ ] recursive
- [ ] stateful
- [x] stateless
- [ ] dynamic

AWS Lambda'da çalıştırdığınız koda Lambda işlevi denir. Lambda işlevleri durum bilgisizdir (stateless) ve temel altyapıyla hiçbir yakınlığı yoktur, böylece Lambda, gelen olayların hızına göre ölçeklendirmek için işlevin gerektiği kadar kopyasını hızlı bir şekilde başlatabilir.

---

**How is the worker environment different from the webserver environment in Elastic Beanstalk?**

- [x] The worker environment applies mainly to backend processes, while the webserver environment is associated with HTTP requests.
- [ ] The worker environment is used for authenticating security groups, while the webserver environment is used for routing traffic.
- [ ] The worker environment is used to process SQS queues, while the webserver environment is used for authenticating IAM service roles.
- [ ] The worker environment applies to the configuration of services, while the webserver environment stores the code used to create applications.

Elastic Beanstalk kullanıldığında, çalışan ortamı esas olarak arka uç işlemlerine uygulanırken web sunucusu ortamı HTTP istekleriyle ilişkilendirilir.

---

**Considering long-term total cost savings for predictable workloads on Amazon EC2, which payment option offers the lowest cost?**

- [x] Reserved instances with all upfront payments selected
- [ ] Reserved instances with partial upfront payments selected
- [ ] Reserved instances with no upfront payments selected
- [ ] Reserved instances with variable payments selected

Rezerve bulut sunucuları, isteğe bağlı bulut sunucularına kıyasla daha düşük bir maliyet karşılığında belirli bir süre için belirlenen ölçütlere sahip bir bulut sunucusu türü için indirim satın almanıza olanak tanır. Bu azalma %75’e kadar çıkabilmektedir. Bulut sunucularına yönelik bu rezervasyonların bir veya üç yıllık zaman dilimlerinde satın alınması gerekir.

Seçtiğiniz ödeme yöntemlerine bağlı olarak ayrılmış bulut sunucularıyla daha fazla indirim elde edilebilir. Öncelikle önünüzde üç seçenek var:

Hepsi peşin (All upfront). Bir veya üç yıllık rezervasyona ilişkin ödemenin tamamı ödenir. Bu, en büyük indirimi sunar ve bulut sunucusunun kullanıldığı saat sayısına bakılmaksızın başka bir ödeme yapılmasına gerek yoktur.

Kısmi peşin (Partial upfront), burada daha küçük bir ön ödeme yapılır ve ardından dönem boyunca bulut sunucusu tarafından kullanılan saatlere indirim uygulanır.

Peşin ödeme yapılmaz (No upfront), ön ödeme yapılmaz veya kısmi ödeme yapılmaz ve bulut sunucusu tarafından kullanılan saatlere üç modelden en küçük indirim uygulanır.

Ayrılmış bulut sunucularında değişken ödeme seçeneği yoktur.

---

**Which of the following is a benefit of using asynchronous invocation?**

- [ ] It processes and filters events in streams and queues.
- [ ] It waits for a response to be received.
- [x] It uses a built-in queue to process events sent to your function.
- [ ] It sounds cool.

Eşzamansız (asynchronous) çağrı kulağa hoş gelse de, bu çağrı yöntemini kullanmanın gerçek yararı, işlevinize gönderilen olayları işlemek için yerleşik bir kuyruğa sahip olmasıdır. Eşzamansız çağırma aynı zamanda yeniden denemeleri, atılacak ileti kuyruklarını ve Lambda hedeflerini de destekler. Yanıt beklemek, eşzamanlı çağırmanın bir avantajıdır ve akış tabanlı hizmetlerde olayların işlenmesi, çekme tabanlı (poll-based) modelin bir avantajıdır.

---

**Which Amazon EC2 instance family provides better performance for disk throughput by delivering dedicated bandwidth for I/O operations?**

- [ ] general purpose
- [ ] I/O burst
- [ ] VPC
- [x] EBS-optimized

Amazon EBS için optimize edilmiş bir bulut sunucusu, Amazon EBS I/O için ek, özel kapasite sağlamak amacıyla optimize edilmiş bir yapılandırma yığını kullanır. Bu optimizasyon, Amazon EBS I/O ile bulut sunucunuzun diğer trafiği arasındaki çekişmeyi en aza indirerek EBS birimleriniz için daha iyi performans sağlar. EBS için optimize edilmiş bulut sunucuları, kullandığınız bulut sunucusu türüne bağlı olarak 500 Mb/sn ile 10.000 Mb/sn arasındaki seçeneklerle Amazon EBS'ye özel bant genişliği sunar.

---

**With enhanced health, the Elastic Beanstalk health agent reports metrics to Elastic Beanstalk. How often does this happen?**

- [x] Every ten seconds
- [ ] Every five seconds
- [ ] Every seconds
- [ ] Every minute


Geliştirilmiş sağlıkla (enhanced health), Elastic Beanstalk sağlık aracısı ölçümleri her on saniyede bir Elastic Beanstalk'a bildirir. Daha sonra ortamdaki her bir örneğin sağlık durumunu belirlemek için sistem durumu aracısı tarafından sağlanan ölçümleri kullanır.
