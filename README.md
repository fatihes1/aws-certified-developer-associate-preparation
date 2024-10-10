# AWS Certified Developer - Associate Exam Preparation

Merhaba! AWS Certified Developer - Associate sertifika sınavına hazırlanmanıza ve bu sınavı geçmenize yardımcı olacak bu GitHub reposuna hoş geldiniz.

Ben Fatih Es, 2023 yılından bu yana AWS Certified Solutions Architect – Associate sertifikasyonuna sahip Full Stack Softwate Engineer'ım. Bu repo kapsamında AWS Certified Developer - Associate sınavına hazırlanırken aldığım notlar yer almaktadır. Umarım bu sınavlara hazırlanan veya bulut dünyasına girmek isteyen kişilere yol gösterici bir kaynak olur.

AWS Certified Developer - Associate sertifikası, AWS hizmetlerini kullanarak uygulamalar geliştirme, test etme, dağıtma ve hata ayıklama konusunda bilgi ve aynı zamanda deneyime sahip bir geliştirici rolündeki bireyler için tasarlanmıştır. AWS, bu sınava girecek adayların AWS hizmetlerini kullanarak uygulama geliştirme ve bakım yapma konusunda en az 1 yıllık gerçek hayat deneyime sahip olmasını önermektedir. Bu repo, Şubat 2023'te yayımlanan AWS Certified Developer - Associate sertifikasyon sınavının (DVA-C02) en son sürümüne hazırlanırken ihtiyaç duyacağınız bilgileri size sağlamayı hedeflemektedir.

Doğruca içindekiler alanında gitmek için [buraya tıklayınız](./contents)

Sertifikasyon dört farklı alanına ayrılmıştır:

1.  AWS Hizmetleri ile Geliştirme (Development with AWS Services),
2.  Güvenlik (Security),
3.  Dağıtım (Deployment),
4.  Hata Ayıklama ve Optimizasyon (Troubleshooting and Optimization)

Her bir alan, sınavda belirli bir yüzde ağırlığı taşır. Her alan ayrıca belirli bilgi ve becerilerin gerekli olduğunu belirten bir dizi görev ifadesi içerir. Şimdi, bu alanlardan her birini daha ayrıntılı inceleyerek sınavda ele alınacak konular hakkında daha detaylı bilgi edinelim.

İlk olarak, 
**Alan 1: AWS Hizmetleri ile Geliştirme** ile başlayacağız. Bu alan, sınav içeriğinin %32'sini oluşturur, yani sınav içeriğinin neredeyse üçte biridir. Bu alan, 3 ana konuya odaklanır:

1.  AWS'de barındırılan uygulamalar için kod geliştirme,
2.  AWS Lambda için kod geliştirme ve
3.  Uygulama geliştirmede veri depolarını (data stores) kullanma

Bu alan tamamen AWS'de uygulama geliştirmeyle ilgilidir. Açık olmak gerekirse, burada iki ana konu "kod geliştirme" ile başlasa da, bu sınavda herhangi bir kod yazmanız veya programlama dilleri hakkında sorular cevaplamanız beklenmez. En fazla, daha verimli veya güvenli hale getirilebilecek bir algoritmayı açıklayan bazı sözde kodlar (pseudocode) görebilirsiniz. Bu nedenle kaynak kodu üzerinde odaklanmak yerine, modern uygulamalarda yaygın olarak kullanılan tasarım ve mimari desenleri anlamalısınız. Bu kapsam, gevşek bağlı bileşenlere (loosely coupled components) duyulan ihtiyacı ve olay odaklı (event-driven) ve mikroservis mimarilerini içermektedir. Ayrıca, **AWS Lambda** kullanarak sunucusuz (serverless) uygulamaları anlamalı, Lambda işlevlerini diğer AWS hizmetleriyle ve bir **Sanal Özel Bulut (VPC)** içindeki özel kaynaklarla nasıl yapılandıracağınızı, ölçeklendireceğinizi ve entegre edeceğinizi bilmelisiniz. Lambda ile ilgili olarak, kaynak tahsisi, özel kütüphanelerin eklenmesi ve veritabanı bağlantı dizeleri (database connection strings) ile harici bağımlılıkların nasıl yönetileceğini anlamalısınız. Ve son olarak, uygulamalarınızda farklı türde veri depolarının kullanım durumlarını bilmeniz gerekecek. Bunlar; önbellekler, önbellekleme stratejileri, **Amazon S3** kullanarak nesne depolama ve ilgili yaşam döngüsü yönetimi, hem ilişkisel hem de ilişkisel olmayan veritabanları gibi veri depolarıdır. Özellikle **DynamoDB**'ye, DynamoDB anahtarlarına ve indekslemeye dikkat etmenizi tavsiye ederim; ayrıca sorgu (query) ve tarama (scan) işlemleri arasındaki farkları anlamalısınız.

Sonra,
**Alan 2: Güvenlik.** Bu alan sınav içeriğinin %26'sını oluşturur ve 3 ilgi alanına odaklanır:

1.  Uygulamalar ve AWS hizmetleri için kimlik doğrulama ve/veya yetkilendirme uygulama (Implement authentication and/or authorization for applications and AWS services),
2.  AWS hizmetlerini kullanarak şifreleme uygulama (Implement encryption by using AWS services),
3.  Uygulama kodunda hassas verileri yönetme (Manage sensitive data in application code)

Alan 1'deki hedefler üzerine koyarak, geliştiricilerin bir uygulamanın mimarisi, kodu ve dağıtımı boyunca güvenliği uygulamalarına dahil etmeleri kritik öneme sahiptir. Bu, en az ayrıcalık erişimi (privilege access) ilkesini anlamayı ve belirli bir kullanıcı, rol veya hizmet için yalnızca gerekli olan en az izin alt kümesini veren politikaları tanımlamak için **AWS Kimlik ve Erişim Yönetimi (IAM)** servisini kullanmayı hedefler. Ayrıca, hem aktarım sırasında hem de depolama sırasında hassas verileri şifreleme ve **AWS Anahtar Yönetim Hizmeti (KMS)**, **AWS Sertifika Yöneticisi (AWS Certificate Manager)**, **Secrets Manager** ve **Systems Manager Parameter Store** gibi hizmetleri kullanarak hassas verileri, kimlik bilgilerini ve secret değerleri korumayı bilmeyi içermektedir. Bazı insanlar yalnızca güvenliği şifreleme (encryption) ile eşleştirse de, bu alanda kapsanan hizmetler ve özellikler şifrelemenin ötesine geçer. Veriye erişimi güvence altına alma, erişimi yalnızca ihtiyaç duyan kişiler ve uygulamalarla sınırlama ve gerektiğinde erişimi ölçeklendirme konularını anlamanız gerekmektedir.

Devam edersek, 
**Alan 3: Dağıtım.** Bu alan sınav içeriğinin %24'ünü oluşturur ve aşağıdaki 4 maddeye odaklanır:

1.  AWS'ye dağıtılacak uygulama eserlerini hazırlama (Prepare application artifacts to be deployed to AWS),
2.  Geliştirme ortamlarında uygulamaları test etme (Test applications in development environments),
3.  Dağıtım testini otomatikleştirme (Automate deployment testing),
4.  AWS CI/CD hizmetlerini kullanarak kod dağıtma (Deploy code by using AWS CI/CD services)

Bu alan, AWS'de uygulama dağıtmayı nasıl yapacağınızı ve bu dağıtımların doğru test edilmesini, ayrıca sürekli entegrasyon ve sürekli teslimat (CI/CD) hatlarını kullanarak dağıtım yaşam döngüsünün farklı aşamalarını otomatikleştirmeyi bilmenizi beklemektedir. Tüm bunları, **CodeCommit**, **CodeBuild**, **CodeDeploy** ve **CodePipeline** dahil olmak üzere çeşitli AWS kod hizmetlerini anlamanız gerektiği anlamına gelir. Ayrıca **CloudFormation** veya **AWS Sunucusuz Uygulama Modeli (AWS Serverless Application Model)** kullanarak altyapı kodu şablonlarını da içerir. Ve bu dağıtımlar, konteyner tabanlı uygulamalardan Lambda işlevlerine ve hatta API'lere kadar her şeyi içerebilir. Ayrıca daha geleneksel çok katmanlı uygulamaları bile içerebilir. Farklı ortamlara (geliştirme, test ve üretim gibi) otomatik dağıtım testlerinin sonuçlarına dayalı olarak dağıtabilecek onaylar, dallar ve eylemlerle eksiksiz bir CI/CD hattı oluşturmak için AWS hizmetlerini nasıl kullanacağınızı anlamalısınız.

Ve son olarak, 
**Alan 4: Hata Ayıklama ve Optimizasyon.** Bu alan sınav içeriğinin %18'ini oluşturur ve 3 alanda sizi değerlendirecektir:

1.  Bir kök neden analizine yardımcı olma (Assist in a root cause analysis),
2.  İzlenebilirlik için cihaz kodu (Instrument code for observability),
3.  AWS hizmetlerini ve özelliklerini kullanarak uygulamaları optimize etme (Optimize applications by using AWS services and features)

Bu alan, mevcut bir uygulamayı inceleyebilme ve kullanılabilirliğini veya performansını etkileyebilecek temel sorunları belirleme yeteneğinizi kapsar. Bu, uygulama hata günlüklerini (error logs) sorgulama ve yorumlama, özel metrikler uygulama veya potansiyel arıza noktalarını proaktif olarak belirlemek için izleme panoları (monitoring dashboards) ve içgörüler kullanmayı içerebilir. Ayrıca uygulama gereksinimlerini değerlendirerek mimarinizde dahil edilecek en uygun AWS özelliklerini belirlemeniz gerekecektir. Bu alan için bilmeniz gereken önemli hizmetler **Amazon CloudWatch**, **AWS CloudTrail** ve **AWS X-Ray**'dir. Bir Lambda işlevinin nasıl performans gösterdiğini görmek istediğinizde CloudWatch'u kullanırsınız. Bir API çağrısını izlemek gerektiğinde CloudTrail'i kullanırsınız. Ve bir dağıtılmış sistemde kodu izleme verileriyle takip etmeniz gerektiğinde X-Ray'i kullanırsınız.

Bu repo hakkında geri bildirim, hem beni hem de gelecekte bu repoyu takip edecek kişiler için önem arz etmektedir. Herhangi bir geri bildiriminiz, olumlu veya olumsuz değerlendirmelerinizi fatihesdev@gmail.com adresi üzerinden e-posta ile göndermeniz beni çok memnun edecektir.

Repo tanıtımın sonuna geldiğimize göre, şimdi başlayalım! Sertifikasyon yolculuğunuzda bol şans dilerim!
