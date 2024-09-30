# Ölçme Testi I: Developer Tools (DVA-C02)

**Which of the following statements describes the blue/green deployment strategy?**

- [ ] The new version is slowly rolled out to replace the old version.
- [ ] The old version is terminated, and then the new version is rolled out.
- [ ] The new version is released to a subset of users, then proceeds to a full rollout.
- [x] The new version is released alongside the old version, and then the traffic is switched to the new version.

Mavi/yeşil dağıtım, uygulamanın iki versiyonunun aynı anda çalıştırılmasını ifade eder. Bir versiyon yükseltilirken, eski versiyonu durdurmaya gerek yoktur. Yeni versiyon başarıyla çalıştıktan sonra, servis yeni versiyona geçirilir, böylece servis kesintisiz çalışmaya devam edebilir.


---

**Which of the following are phases of AWS CodeBuild? (Choose 2 answers)**

- [x] Install
- [ ] Un-build
- [x] Post-build
- [ ] Uninstall

BuildSpec dosyası, CodeBuild'in derleme sırasında kullanacağı komutları içerir. Birkaç aşaması vardır: kurulum (install), ön derleme (pre-build), derleme (build) ve derleme sonrası (post-build).

---

**Which of the following is a deployment configuration in AWS CodeDeploy?**

- [ ] A set of instances associated with an application that you target for a deployment
- [ ] The pre-build scripts that can be used to bootstrap an agent during a code launch
- [x] A constraint that determines how a deployment progresses through the instances in a deployment group
- [ ] A specific version of deployable content, such as source code, post-build artifacts, web pages, executable files, and deployment scripts

Bir dağıtım yapılandırması, bir dağıtımın bir dağıtım grubundaki örnekler arasında nasıl ilerleyeceğini belirleyen bir kısıtlamadır.

---

**Jenkins is primarily developed in _____.**

- [ ] Groovy
- [x] Java
- [ ] Python
- [ ] Javascript

Jenkins öncelikle Java'da geliştirilmiştir ve bu nedenle bir Java çalışma zamanı olan JRE, temel gerekliliktir.

---

**Which actions should you take when configuring a new deployment for the EC2 platform in AWS CodeDeploy? (Choose 2 answers.)**

- [ ] Specify deployment target EC2 instances or Auto Scaling groups.
- [x] Select blue/green or in-place deployment.
- [x] Specify OneAtATime, HalfAtATime, or AllAtOnce deployment configurations.
- [ ] Configure deployment target groups for ECS services.

EC2/On-premises platform OneAtATime, HalfAtATime ve AllAtOnce yapılandırmalarını destekler. EC2 platformu için ayrıca dağıtım türünü yani kodunuzu mevcut örneklere yerinde mi dağıtmak istiyorsunuz yoksa en son sürümle ayrı bir ortam oluşturmak için mavi/yeşil strateji mi kullanmak istiyorsunuz belirtmeniz gerekecektir.

---

**A developer wants to run a React web application on an AWS Lambda function. The developer has created an AWS CodeDeploy project to deploy the application.**
**Which storage option is the correct repository type for CodeDeploy's required files?**

- [x] Amazon S3 
- [ ] Bitbucket
- [ ] GitHub
- [ ] Amazon EFS

AWS CodeDeploy, bir uygulamayı dağıtmak için gereken paketleri veya dosyaları depolamak için bir repository kullanır. Eepository'ler S3 bucket'ları, Github ve Bitbucket depoları olabilir. AWS Lambda fonksiyonları söz konusu olduğunda, izin verilen tek repository bir S3 bucket'ıdır. AWS CodeBuild, uygulama paketini oluşturmanıza ve bir ZIP dosyası oluşturmanıza, ardından bunu AWS CodeDeploy projesinin kaynağı olarak kullanılmak üzere bir S3 bucket'ına yüklemenize olanak tanır. 

---

**Which of these statements about AWS Cloud9 is correct? (Choose 2 answers)**

- [ ] Cloud9 provides an inbuilt terminal but comes without full admin privileges to the underlying instance (sudo rights).
- [x] Cloud9 provides an inbuilt terminal, which comes with full admin privileges to the underlying instance (sudo rights).
- [ ] Cloud9 does not support debugging and stepping through your source code.
- [x] Cloud9 supports debugging and stepping through your source code.

AWS Cloud9 IDE'sinin CLI komutlarını interaktif olarak çalıştırabilen yerleşik bir terminal penceresi barındırır. Ayrıca, örnekte tam yönetici ayrıcalıklarına (sudo hakları) sahipsiniz, bu da geliştirme için gerekli olan veya uygulamanızı barındırmak için gereken ek araçları yüklemenize olanak tanır. AWS Cloud9 IDE, birçok dil için kodu oluşturma, çalıştırma ve hata ayıklama için yerleşik destek sağlar.

---

**Which of the following statements about AWS CodeCommit is false?**

- [ ] You can use any existing Git client tools with AWS CodeCommit.
- [ ] AWS CodeCommit can act as the source control system for your software projects.
- [ ] Any commits into AWS CodeCommit can trigger downstream activity, such as ticking off a new build of your source code.
- [x] AWS CodeCommit is the ending component within a CI/CD setup.

Başlangıç olarak, AWS CodeCommit tüm tanıdık Git komutlarını destekler, böylece mevcut herhangi bir Git istemci aracını veya komut satırından veya terminalden eklenen komutları kullanabilirsiniz. AWS CodeCommit, `clone`, `pull`, `push` ve `fetch` gibi tipik Git işlemlerini destekler. AWS CodeCommit, yazılım projeleriniz için kaynak kontrol sistemi olarak görev yapar. Kod değişikliklerinizi commit ettiğiniz servistir. Özünde, AWS CodeCommit bir CI/CD kurulumu içindeki başlangıç bileşenidir. AWS CodeCommit'e yapılan herhangi bir commit, CI/CD kurulumunuzda kaynak kodunuzun yeni bir derlemesini başlatmak gibi downstream aktiviteyi tetikler.

---

**AWS CodeCommit is based on the _____ version control system.**

- [ ] CSV
- [x] Git
- [ ] SVN
- [ ] RCS

AWS CodeCommit, kendi özel Git repolarınızı AWS tarafından yönetilen tam güvenli bir ortamda barındırmak için kullanılabilir. AWS CodeCommit, Git tabanlı bir kaynak kontrol sisteminden beklediğiniz tüm özelliklere sahip, son derece basit bir şekilde tasarlanmış bir servistir.

---

**Which statements comparing mutable and immutable servers are correct? (Choose 2 answers)**

- [x] Mutable servers _**can**_ be changed after deployment, and immutable servers _**cannot**_.
- [ ] Mutable servers _**cannot**_ be changed after deployment, and immutable servers _**can**_.
- [x] Mutable servers scale _**less effectively**_ than immutable servers.
- [ ] Mutable servers scale _**more effectively**_ than immutable servers.

Mutable altyapı nedir? Bu, sağlandıktan sonra değiştirilen altyapıdır. Yani bu, bir altyapı parçasını dağıttıktan sonra, onun bazı yönlerini değiştirdiğiniz anlamına gelir. Bu altyapıyı değiştirebilirsiniz, bu yüzden adı mutable altyapıdır.

---

**What is the function of a build badge in AWS CodeBuild?**

- [ ] To provide a description of the build project
- [x] To display the status of the latest build for a project
- [ ] To identify the repository that contains the source code
- [ ] To provide a source version for the last commit

AWS CodeBuild, bir projenin en son derlemesinin durumunu gösteren, gömülebilir, dinamik olarak oluşturulan bir image (badge) kullanımına izin verir. Bu görüntüye, CodeBuild projeniz için oluşturulan herkese açık bir URL aracılığıyla erişilebilir.

---

**What is the difference between deployment and release?**

- [ ] To release code means to push changes to an environment, say, production; deployment means that the feature is enabled and usable by your end users.
- [ ] Release means the feature has not been tested yet; deployment means the feature has been tested.
- [ ] Deployment deals with the entire product; a release deals with only one feature.
- [x] Deployment is the act of pushing changes to an environment, say, production; releasing a feature means that the feature is enabled and usable by your end users.

Dağıtım, değişiklikleri bir ortama, diyelim ki üretime gönderme eylemidir. Ancak, özellik değiştirme anahtarları ve akıllı veritabanı geçişleri kullanıyorsanız, kullanıcıya sunulmamış olabilecek kodu dağıtabilirsiniz. Yani, "released" demek, özelliğin etkinleştirildiği ve son kullanıcılarınız tarafından kullanılabilir olduğu anlamına gelir.

---

**AWS Lambda may be integrated with _____ for built-in distributed request tracing.**

- [x] X-Ray
- [ ] Azure
- [ ] DynamoDB
- [ ] CloudWatch

Lambda, yerleşik dağıtılmış istek izleme için X-Ray ile entegre edilebilir.

---

**In which stage of the AWS CodePipeline is the code deployment setup run?**

- [ ] test
- [ ] source
- [x] deploy
- [ ] build

AWS CodePipeline'da, kod dağıtımı genellikle "Deploy" aşamasında kurulur. AWS CodePipeline'daki aşamaların ve dağıtımın nereye uyduğunun kısa bir genel bakışı: 
- Source: Bu, kaynak kodunuzun GitHub, AWS CodeCommit veya Bitbucket gibi bir sürüm kontrol sisteminden çekildiği ilk aşamadır. 
- Build: Bu aşamada, kaynak kod AWS CodeBuild veya Jenkins gibi araçlar kullanılarak bir derleme sürecinden geçer. Bu adım kodunuzu derler ve testleri çalıştırır. 
- Deploy: Bu, kod dağıtımının gerçekleştiği kritik aşamadır. Bu aşamada, uygulamanız AWS Elastic Beanstalk, AWS ECS veya AWS Lambda gibi AWS hizmetleri kullanılarak belirtilen ortamlara dağıtılır. Dağıtım sürecinizi burada kurar ve yapılandırırsınız. 
- Onay/Manuel Adım (İsteğe bağlı): Bazı pipeline'lar, özellikle üretim ortamları için dağıtımdan önce veya sonra manuel bir onay aşaması içerir. 
- Test (İsteğe bağlı): Bazı kurulumlarda, özellikle hazırlık veya üretim ortamında entegrasyon veya kullanıcı kabul testi için dağıtımdan sonra ek bir test aşaması olabilir.

---

**What is AWS Amplify?**

- [ ] a service that allows developers to securely and easily add location data to applications
- [ ] a service that allows developers to test Android, iOS, and web apps on real devices in the AWS cloud
- [x] a mobile and web toolset that AWS offers to help developers create scalable front-end applications
- [ ] a mobile and web toolset that AWS offers to help developers build, deploy, and manage APIs

AWS Amplify, geliştiricilerin ölçeklenebilir ön uç uygulamaları oluşturmasına yardımcı olmak için AWS'nin sunduğu bir mobil ve web araç setidir.


















