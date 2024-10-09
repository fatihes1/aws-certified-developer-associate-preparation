# Ölçme Testi: Application Integration (DVA-C02)

**A solutions architect is reviewing the cost of a company's scheduled nightly maintenance. The solutions architect notices that three Amazon EC2 instances are being run to perform nine scripted tasks that take less than 5 minutes each to complete. The scripts are all written in Python.**

**Which action should the company take to optimize the costs of the nightly maintenance? **

- [ ] Consolidate the scripts from the three EC2 instances to run on one EC2 instance. 
- [x] Convert the scripts to AWS Lambda functions and schedule them with Amazon EventBridge. 
- [ ] Purchase a Compute savings plan for the running EC2 instances.
- [ ] Create a Spot Fleet to replace the running EC2 instances for running the scripts. 


EventBridge çözümü en maliyet etkin seçenektir. Her biri beş dakika süren dokuz görevle bu çözüm, AWS Free Tier kullanımı için uygun olacaktır. Free Tier kullanımı, her ay 1 milyon ücretsiz istek ve her ay 400.000 GB-saniye hesaplama süresi içerir. Ayrıca, bu oranda EventBridge (CloudWatch Events) olayları için ücretler nominal düzeydedir.

Diğer seçenekler aşağıdaki nedenlerle yanlıştır:

-   Üç EC2 instance'ından scriptleri birleştirip tek bir EC2 instance'ında çalıştırmak bazı maliyet tasarrufları sağlayabilir, ancak başka bir çözüm daha maliyet etkindir. Bu yaklaşım esnekliği azaltır ve Python scriptlerinin tek bir işletim sistemi altında çalışmasını gerektirerek gereksiz karmaşıklık ekleyebilir. Lambda ile şirket mevcut kodunu getirebilir ve çalıştırabilir.
-   Compute Savings Plan bazı maliyet tasarrufları sağlayabilir, ancak başka bir çözüm daha maliyet etkindir. EC2 instance'ları için Compute Savings Plan, az sayıda çağrı ve scriptleri çalıştırmak için gereken az sayıda saat göz önüne alındığında Lambda ve EventBridge (CloudWatch Events) olaylarının kullanımından daha pahalıya mal olacaktır.
-   Spot instance'lar bazı maliyet tasarrufları sağlayabilir, ancak başka bir çözüm daha maliyet etkindir. Bu yaklaşımla, Spot Fleet çoğu zaman boşta kalacaktır. Spot Fleet, mevcut EC2 konfigürasyonundan daha az maliyetli olacaktır, ancak scriptler için Lambda kullanımından daha pahalıya mal olacaktır.

---

**Which of the following is NOT a use case for Amazon EventBridge? **

- [ ] Re-architect for speed by decoupling services and limiting dependencies 
- [ ] Extend functionality via SaaS integrations 
- [ ] Customize SaaS with AI/ML using events 
- [x] None of the above

Yukarıdakilerin tümü Amazon EventBridge için kullanım durumlarıdır:

-   Ayrık hizmetler ve uygulamalarla mimarinizi modernleştirmeyi ve yeniden düzenlemeyi hızlandırmak için EventBridge'i kullanın. EventBridge ile, olay üreten ve tüketen uygulamalar veya hizmetler arasında ağır koordinasyona gerek yoktur. Sistemler arasında açık bağımlılıklar olmadan ekiplerin özellikler üzerinde çalışmasına izin vererek kuruluşunuzun geliştirme sürecini hızlandırabilirsiniz.
-   EventBridge aracılığıyla uygulamalarınızı kolayca diğer SaaS uygulamalarına bağlayarak işlevselliğini genişletebilirsiniz. Örneğin, ücretsiz kademede yeni bir kullanıcı oluşturulduğunda EventBridge'e özel olaylar gönderebilir ve bu olayı API Destinations aracılığıyla Zendesk CRM'ye gönderebilirsiniz.
-   EventBridge'i Lambda ve diğer hizmetlerle bağlayabilirsiniz.
-   AWS Yapay Zeka/Makine Öğrenimi hizmetlerini kullanarak SaaS uygulamalarından gelen olaylarınızı zenginleştirebilir ve değerli içgörüler elde edebilirsiniz. Örneğin, Shopify'dan verilerinizi EventBridge'e yükleyerek bir iş akışını tetikleyebilir ve yeni perakende ürünlerinin görüntü etiketlemesi için Amazon Comprehend gibi AI hizmetlerini kullanabilirsiniz. 


---

**Which AWS service can help you add a layer of timeout-and-retry logic to your AWS Lambda functions to assist with managing failures with zero code?**

- [x] AWS Step Functions
- [ ] Amazon Managed Workflows for Apache Airflow
- [ ] Amazon EventBridge
- [ ] Amazon CloudWatch

Step Functions, AWS'deki iş akışlarını yönetmek için (bu iş akışları başarısız olsa bile) kod gerektirmeyen çözümler sunar. Bu durumda, olayı birkaç kez yeniden deneme ve tamamlanması çok uzun sürüyorsa iptal etme seçenekleriniz vardır.


---

**Which AWS Step Functions state is often a sub-state within other states where a resource is selected to run as well as a timeout period?**

- [x] The Task State
- [ ] The Choice State
- [ ] The Parallel State
- [ ] The Map State

State machine'iniz herhangi bir zamanda sekiz durumdan birinde olabilir.

Task State. İşin gerçekten gerçekleştiği yer burasıdır. Bir görevle, Step Functions'ın çalıştırmasını istediğiniz bir kaynak ve bir zaman aşımı süresi tanımlarsınız. Örneğin, burada bazı kodları çalıştırmak için Lambda fonksiyonunuzu ekleyebilirsiniz. Bu durum genellikle diğer durumlar içinde bir alt durum (veya eylem) olarak kullanılır.


---

**Some developers created an app where users can use their social identities from authentication providers such as Twitter, Google, or Amazon. Now they want to synchronize user data across mobile devices and the web.**

**How can they synchronize user profile data across devices? (Choose 2 answers)**

- [x] Synchronize user profile data across devices using AWS AppSync
- [ ] Synchronize user profile data across devices using AWS Directory Service
- [x] Synchronize user profile data across devices using Amazon Cognito Sync
- [ ] Synchronize user profile data across devices using AmazonAPI Gateway

AWS AppSync, geliştiricilerin kullanıcı mobil uygulama verilerini cihazlar arasında gerçek zamanlı olarak senkronize etmesine ve yönetmesine olanak tanıyan bir hizmettir. Ayrıca, mobil cihaz internete bağlı olmadığında verilerin değiştirilmesine izin verir.

Amazon Cognito Sync, geliştiricilerin kendi backend'lerini kullanmadan kullanıcı mobil uygulama ve web verilerini cihazlar arasında senkronize etmelerini sağlar.

Diğer seçenekler aşağıdaki nedenlerle yanlıştır:

- Amazon Cognito Event, Amazon Cognito'daki bir olaya yanıt olarak bir AWS Lambda fonksiyonunu çalıştırmanıza olanak tanır. Kullanıcı profillerinin cihazlar arasında senkronizasyonuna izin vermez.

- AWS Directory Service, Microsoft Active Directory'yi Amazon EC2, Amazon RDS for SQL Server, FSx for Windows File Server ve AWS IAM Identity Center (AWS Single Sign-On'un halefi) gibi diğer AWS hizmetleriyle kurmanın ve çalıştırmanın birden çok yolunu sağlar.

- AWS API Gateway, geliştiricilerin herhangi bir ölçekte API'ler oluşturmasına, sürdürmesine ve güvenliğini sağlamasına olanak tanıyan tam yönetilen bir hizmettir.


---

**In Amazon EventBridge, the _____ is where all of the events will land once they have been triggered.**

- [ ] event target
- [ ] event store
- [ ] event warehouse
- [x] event bus

Event bus, tüm olaylarımızın tetiklendikten sonra ineceği yerdir. Hizmetlerinizin oluşturduğu tüm verileri yakalayan huni gibidir.

---

**Which of the following best describes how to handle duplicate messages in an Amazon Simple Queue Service (SQS) queue?**

- [ ] Use content-based deduplication to prevent duplicate messages from being sent to the queue.
- [ ] Configure the queue to automatically delete duplicate messages based on their content.
- [x] Use a unique message identifier to deduplicate messages within the queue.
- [ ] Handle duplicate messages in the application code that processes messages from the queue. 

Amazon Simple Queue Service (SQS), bileşenlerin ayrıştırılmasını ve uygulamaların ölçeklenebilirliğini sağlayan dağıtılmış bir mesaj kuyruğu hizmetidir. Bir SQS kuyruğundaki yinelenen mesajları işlemek için, kuyruk içindeki mesajları yinelemeden çıkarmak amacıyla benzersiz bir mesaj tanımlayıcısı kullanabilirsiniz. Kuyruğa bir mesaj gönderirken, uygulama mesaj için benzersiz bir tanımlayıcı ekleyebilir ve bu tanımlayıcı yinelemeleri tanımlamak ve kaldırmak için kullanılabilir. Bu yaklaşım, yinelenen mesajların uygulama tarafından işlenmeden önce kaldırılmasını sağlar.

Diğer seçenekler aşağıdaki nedenlerle yanlıştır:

-   İçerik tabanlı yineleme önleme, kuyruğa yinelenen mesajların gönderilmesini önlemek için kullanılır, ancak kuyrukta zaten bulunan yinelenenleri işlemez.
-   AWS SQS kuyrukları, yinelenen mesajları otomatik olarak silmek için yerleşik bir özelliğe sahip değildir.
-   Yinelemeleri uygulama kodunda işlemek karmaşık ve hataya açık olabilir ve yinelenen mesajların gereksiz yere işlenmesine neden olabilir.


---

**Which of the following best describes how to decouple applications using Amazon Simple Queue Service (SQS) in AWS?**

- [x] Applications should send messages directly to the SQS queue, then retrieve and process messages as they become available.
- [ ] Applications should send messages to an SNS topic, which then forwards messages to the SQS queue for processing.
- [ ] Applications should use AWS Lambda functions to retrieve and process messages from the SQS queue.
- [ ] Applications should use AWS Step Functions to manage the workflow and decouple processing steps.

Amazon Simple Queue Service (SQS), bileşenlerin ayrıştırılmasını ve uygulamaların ölçeklenebilirliğini sağlayan bir mesaj kuyruğu hizmetidir. SQS kullanarak uygulamaları ayrıştırmak için, uygulamalar mesajları doğrudan SQS kuyruğuna göndermeli, ardından mesajlar kullanılabilir hale geldiğinde bunları almalı ve işlemelidir. Bu, asenkron işlemeyi mümkün kılar, çünkü uygulamalar diğer bileşenlerin kullanılabilirliğine bağlı olmak yerine, işlemeye hazır olduklarında kuyruktan mesajları alabilirler.

Diğer seçenekler aşağıdaki nedenlerle yanlıştır:

-   SNS topic'i eklemek ek bir karmaşıklık katmanı ekler.
-   AWS Lambda, bir SQS kuyruğundan gelen mesajları işlemek için kullanılabilirken, SQS kullanarak uygulamaları ayrıştırmak için gerekli değildir.
-   AWS Step Functions, işleme adımlarının iş akışını yönetmek için kullanılabilirken, SQS kullanarak uygulamaları ayrıştırmak için gerekli değildir.

