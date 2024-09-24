# Ölçme Testi: Management and Governance (DVA-C02)

**Which Amazon CloudWatch feature allows CloudWatch to implement machine learning algorithms against your metric data to help detect any activity that sits outside of the normal baseline parameters?**

- [ ] CloudWatch Alarms
- [x] CloudWatch Anomaly Detection
- [ ] Amazon EventBridge
- [ ] CloudWatch Logs

CloudWatch metrics ayrıca anomaly detection (anomali tespiti) olarak bilinen bir özelliği etkinleştirmenize olanak tanır. Bu, CloudWatch'un makine öğrenimi algoritmalarını metrik verilerinize uygulayarak, genellikle beklenen normal temel parametre sınırları dışında kalan herhangi bir etkinliği tespit etmeye yardımcı olmasını sağlar.

---

**In AWS Systems Manager Patch Manager, patches are defined using patch _____.**

- [ ] rules
- [ ] templates
- [ ] documents
- [x] baselines

Patch Manager'ı, yönetilen instance'larda çalışan hem işletim sistemine hem de uygulamalara güncellemeleri uygulamak için kullanabilirsiniz. Peki bu patch'ler nerede tanımlanır? Patch Manager, patch'leri patch baseline'ı kullanarak tanımlar.

---

**If an AWS CloudFormation template fails to create a resource, what action does CloudFormation take by default?**

- [ ] The CloudFormation engine sends a notification and requests you to take a look at the failed resource before continuing. 
- [ ] The template continues to retry creating the resource until you manually stop it. 
- [ ] The template maintains the created resources but stops the creation of any new resources. 
- [x] The template rolls back and reverts the stack to its last known stable configuration.

Varsayılan olarak, "automatic rollback on error" (hatada otomatik geri alma) özelliği etkindir. Bu, CloudFormation'a yalnızca tüm bireysel işlemler başarılı olursa stack'inizdeki tüm kaynakları oluşturmasını veya güncellemesini yönlendirir. Başarısız olurlarsa, CloudFormation stack'i bilinen son kararlı yapılandırmaya geri döndürür. Gerekirse bu varsayılan davranışı değiştirebileceğinizi unutmayın.

---

**What is the function of Amazon CloudWatch subscriptions?**

- [x] To access a real-time feed of log events from CloudWatch Logs and deliver those logs to other services for custom processing and analysis
- [ ] To collect default metrics from more than 70 AWS services and evaluate those metrics in the CloudWatch dashboard
- [ ] To collect and aggregate curated metrics and container logs by collecting compute performance metrics from each container as performance events
- [ ] To create reusable graphs and visualize your cloud resources and applications within the CloudWatch dashboard

Amazon CloudWatch Subscriptions, CloudWatch loglarından gerçek zamanlı log olayları akışına erişim sağlar. Bu akış, özel işleme ve analiz için Kinesis streams, Firehouse veya Lambda gibi diğer servislere iletilebilir.

---

**Where are VPC flow logs stored?**

 - [x] CloudWatch logs
 - [ ] Amazon EBS volumes
 - [ ] Within CloudTrail logs 
 - [ ] DynamoDB tables

S3 erişim logları ve CloudFront erişim loglarından farklı olarak, VPC Flow Logs tarafından oluşturulan log verileri S3'te depolanmaz. Bunun yerine, yakalanan log verileri CloudWatch loglarına gönderilir. VPC Flow Logs, VPC'nizdeki network arayüzlerine giden ve gelen IP trafiği hakkında bilgi yakalamanızı sağlayan bir özelliktir. Flow log verileri şu konumlara yayınlanabilir: Amazon CloudWatch Logs, Amazon S3 veya Amazon Data Firehose.

---

**Which of the following are requirements for a managed instance with AWS Systems Manager? (Choose 2 answers.)**

- [ ] The instance is using the Amazon SSM Managed Instance Core policy.
- [ ] The instance has access to the public Systems Manager access point.
- [x] A role with the permissions needed has been defined and associated with the instance.
- [x] Systems Manager Agent has been installed and configured.

Yönetilen bir instance, bir Systems Manager ile iletişim kurabilen ve şu iki gereksinimi karşılayan bir makinedir: Systems Manager Agent kurulmuş ve yapılandırılmış olmalı ve gerekli izinlere sahip bir rol tanımlanmış ve instance ile ilişkilendirilmiş olmalıdır.

---

**CloudWatch Insights can deliver three different types of insights. Which of the choices below is not an available insight?**

- [ ] Container Insights
- [ ] Lambda Insights
- [ ] Log Insights
- [x] Network Insights

Artık CloudWatch içinde 3 farklı insight türü var: Log Insights, Container Insights ve Lambda Insights. 

---

**How long does it take AWS CloudTrail to deliver new logs to their designated S3 bucket?**

- [ ] 15 minutes
- [x] 5 minutes
- [ ] 5 hours
- [ ] 15 seconds

AWS CloudTrail kullanırken, yaklaşık her beş dakikada bir yeni loglar oluşturulur ve API çağrısından yaklaşık 5 dakika sonra kalıcı depolama için belirlenmiş bir S3 bucket'ına teslim edilir.

---

**Which of the following is a valid section in a CloudFormation template?**

- [ ] Resource:
- [ ] Transitions: 
- [x] Resources:
- [ ] AWSTemplateDate:

Bir CloudFormation şablonunun geçerli bölümleri şunlardır:

- AWSTemplateFormatVersion:
- Description:
- Parameters:
- Mappings:
- Conditions:
- Transform:
- Resources:
- Outputs:

---

**Which of the following statements about CloudWatch Logs and the Unified CloudWatch Agent is true?**

- [ ] CloudWatch Logs can only be used to monitor log data from EC2 instances.
- [ ] The Unified CloudWatch Agent is only available for Linux operating systems.
- [x] The Unified CloudWatch Agent can collect logs and additional metric data from EC2 instances and on-premise services.
- [ ] The CloudWatch Agent configuration file can only be stored on the EC2 instance where the agent is installed.

CloudWatch Logs, kullanıcıların logstream'i gerçek zamanlı olarak izlemesine ve belirli olayları aramak için metrik filtreleri ayarlamasına olanak tanıyarak, log verilerinin gerçek zamanlı izlenmesi için merkezi bir depo görevi görebilir. Unified CloudWatch Agent, Linux ve Windows dahil olmak üzere çeşitli işletim sistemlerine kurulabilir ve kullanıcıların EC2 instance'larından ve şirket içi hizmetlerden logları ve ek metrik verilerini toplamasına olanak tanır.


---

**With dozens of EC2 instances under your watch, you have to come up with a way to aggregate logs in S3 buckets where you can then run analytics on the data.**
**Which approach would you choose to collect logs and send them to S3 with the least effort involved?**

- [x] Install the CloudWatch logs agent to collect all the logs you need, which then can be viewed in CloudWatch and exported to S3.
- [ ] Setup Systems Manager Agent (SSM), which automatically aggregates logs and just needs to be configured for S3 Access
- [ ] Using Python and Boto3 (AWS SDK), write a script that collects the log files and sends them over to S3.
- [ ] Turn on VPC Flow Logs which will collect ALL traffic for ALL servers in your EC2 Fleet and send them to a CloudWatch log group which in turn can be exported to S3.

CloudWatch logs agent, iş için uygun araçtır; veriler yakalandıktan sonra, logları S3'e aktarma seçeneğiniz vardır. VPC Flow logları uygulama veya sunucuya özgü değildir, sadece ağ trafiği içindir. SSM logları toplamaz. AWS SDK ile Python kesinlikle bunu yapabilir, ancak önemli bir çaba ve Python'da kodlama becerisi gerektirecektir.

---

**To manage access to your secrets with the same level of fidelity you have come to expect from AWS in general, AWS Secrets Manager integrates with which service?**

- [x] AWS Identity and Access Management (IAM)
- [ ] Amazon Redshift
- [ ] Amazon RDS
- [ ] AWS KMS

Secrets Manager, AWS'nin Identity and Access Management (IAM) ile tam olarak entegredir. Bu, bu sırlara erişimi, AWS'den genel olarak beklediğiniz aynı düzeyde hassasiyetle yönetmenize olanak tanır.

---

**How does the Anomaly Detection feature of Amazon CloudWatch help you to visualize events that are outside of the normal range?**

- [x] By calculating expected confidence values and generating a confidence band based on the normal ranges generated by the model
- [ ] By using values pre-set by the CloudWatch service to predict future events and identify problematic behavior 
- [ ] By establishing thresholds based on metrics with the largest value fluctuations and bursts of activity
- [ ] By auto-generating a graph of expected alarm thresholds based on metrics provided by AWS and comparing those metrics to those generated by a user

Amazon CloudWatch'un Anomaly Detection özelliği, normal aralığın dışındaki olayları görselleştirmenize yardımcı olur. Model tarafından oluşturulan beklenen değerler iki şekilde kullanılabilir. Alarm oluşturmak için kullanılabilirler veya bir grafik ile görselleştirmenin bir parçası olarak kullanılabilirler.


---

**How does CloudTrail deliver events containing API activity to your CloudWatch Logs?**

- [ ] CloudTrail cannot deliver events containing API activity to your CloudWatch Logs.
- [ ] You need MFA to deliver events containing API activity to your CloudWatch Logs.
- [x] CloudTrail assumes the IAM role you specify to deliver API activity to CloudWatch Logs.
- [ ] CloudTrail delivers events containing API activity to your CloudWatch Logs via AWS Kinesis.

CloudTrail, API etkinliğini CloudWatch Logs'a iletmek için belirttiğiniz IAM rolünü üstlenir. IAM rolünü yalnızca olayları CloudWatch Logs log stream'inize iletmek için gerekli izinlerle sınırlarsınız. IAM rol politikasını gözden geçirmek için CloudTrail belgelerinin kullanıcı kılavuzuna gidin.

---

**Which of these CloudWatch features allows you to gather more information about the data that CloudWatch is collecting? (Choose 2 answers)**

- [ ] CloudTrail
- [x] Container Insights
- [ ] Event Bus
- [x] Log Insights

Hem Container Insights hem de Log Insights, CloudWatch'un topladığı veriler hakkında daha fazla bilgi edinmenizi sağlar. Log Insights, CloudWatch Logs tarafından yakalanan logları, çubuk, çizgi, pasta veya yığılmış alan grafikleri olarak temsil edilebilen görselleştirmeler sunan interaktif sorgular kullanarak saniyeler içinde ölçekli olarak analiz etmenizi sağlar. Bu özelliğin çok yönlülüğü, AWS hizmetlerinin veya uygulamalarınızın kullanabileceği herhangi bir log dosyası formatıyla çalışmanıza olanak tanır. Esnek bir yaklaşım kullanarak, Log insights'ı log verilerinizi filtrelemek ve ilgilendiğiniz özel verileri almak için kullanabilirsiniz. Ayrıca özelliğin görsel yeteneklerini kullanarak, bunları görsel bir şekilde görüntüleyebilir.

Container Insights, AWS içindeki farklı container hizmetlerinden ve uygulamalarından, örneğin Amazon Elastic Kubernetes Service (EKS) ve Elastic Container Service (ECS)'den farklı metrik verilerini harmanlayıp gruplandırmanıza olanak tanır. Container Insights ayrıca tanılama verilerini yakalayıp izlemenize olanak tanıyarak, container mimarinizde ortaya çıkan sorunları nasıl çözeceğiniz konusunda ek bilgiler sağlar. Bu izleme ve insight verileri küme, düğüm, pod ve görev düzeyinde analiz edilebilir, bu da container uygulamalarınızı ve hizmetlerinizi anlamanıza yardımcı olan değerli bir araç haline getirir.

---

**You are selecting the option for ‘creating a new Bucket’ for CloudTrail log delivery during a trail creation. When doing this, how will the permission be configured?**

- [x] By CloudTrail by applying and configuring a Bucket Policy with the relevant permissions allowing logs to be delivered to the Bucket
- [ ] By the user by applying their Bucket Policy which must allow CloudTrail to write and store logs within it
- [ ] By creating a new IAM Policy allowing access for CloudTrail writes
- [ ] Configured at a later stage using existing bucket policies within the AWS Account

CloudTrail logları bir S3 Bucket'ına teslim edilir. Trail'lerinizi oluştururken, işlendikten sonra logları hangi S3 bucket'ına göndereceğinizi belirtmeniz gereken bir adım vardır. Burada iki seçeneğiniz olacak: İlk seçenek 'Create a new S3 Bucket' (Yeni bir S3 Bucket oluştur) ve ikincisi 'use an existing S3 Bucket' (mevcut bir S3 Bucket kullan).

İlk seçeneği seçerek, CloudTrail, logların Bucket'a teslim edilmesine izin veren ilgili izinlerle bir Bucket Policy uygular ve yapılandırır. Bu sürecin bir parçası olarak, CloudTrail Policy içinde aşağıdaki öznitelikleri yapılandırır:

- İzin verilen Statement ID'ler (SID'ler)
- Log dosyalarının kaydedileceği klasör adı
- Mevcut ve gelecekteki CloudTrail destekli bölgeler için servis principal adı
- Bucket adı
- Belirtildiyse isteğe bağlı önek (prefix)
- Sahibi hesabın ID'si

---

**According to Amazon, what two types of use cases does AWS CloudTrail solve by logging API activity?**

- [ ] hardware and software
- [ ] intranet and extranet 
- [ ] efficiency and performance
- [x] operational and security

Genel olarak, CloudTrail tarafından yakalanan API etkinliğini aramak, AWS hesabınızdaki operasyonel ve güvenlik olaylarını gidermede yardımcı olur.

---

**Which of the following statements about AWS Secrets Manager is false?**

- [ ] It allows you to rotate API keys.
- [ ] It allows you to manage API keys.
- [ ] It allows you to rotate database credentials.
- [x] Secrets can only be created through a CloudFormation template.

Bir servis olarak Secrets Manager, veritabanı kimlik bilgilerini, API anahtarlarını ve diğer sırları yaşam döngüleri boyunca döndürmenize, yönetmenize ve almanıza olanak tanır. AWS'deki çoğu şey gibi, sırlar gibi kaynakları konsolda elle manuel olarak ekleme yeteneği verir.

----

**Which of the following statements about AWS Systems Manager is false?**

- [ ] It allows you to apply system or application patches.
- [ ] It allows you to collect software inventory.
- [x] Most of its functionality charges a fee.
- [ ] It allows you to create and update system images.

Aynı konsoldan veya komut satırı arayüzünden sistem image'larını oluşturabilir ve güncelleyebilir, yazılım envanterini toplayabilir, sistem veya uygulama patch'lerini uygulayabilir ve Linux ve Windows işletim sistemlerini yapılandırabilir, ayrıca instance'larınızın durumunu yönetebilirsiniz. OpsCenter, kullandıkça öde modeline göre fiyatlandırılır.

---

**Which of the following statements about AWS AppConfig is not true? **

- [ ] AppConfig can be used to validate a configuration’s format and values before deployment
- [ ] AppConfig is a part of AWS Systems Manager
- [x] AppConfig can only be used to manage production application environments 
- [ ] AppConfig allows you to rollback configuration changes in the event of a failure 

AppConfig, Dev, Prod ve Test dahil olmak üzere çeşitli ortamlar için yapılandırmayı yönetmek için kullanılabilir.

---

**An operations team manages multiple Amazon EC2 instances and needs to access logs from these systems in a central location.  The team has decided to use Amazon CloudWatch Logs for this centralized log analysis.** 

**What steps does a SysOps administrator need to take to make the various logs on these instances accessible from CloudWatch Logs?**


- [x]     	
    Download and install the CloudWatch Agent on each EC2 instance.
    Create an IAM Role that allows the CloudWatch Agent to write logs to CloudWatch.
    
- [ ]     	
    Download and install the CloudWatch Agent on each EC2 instance.
    Create an IAM User that allows the CloudWatch Agent to write logs to CloudWatch.
    
- [ ]     	
    Create a log group in CloudWatch and associate it with each EC2 instance.
    Create an IAM User that allows the CloudWatch Agent to write logs to CloudWatch.

- [ ]     	
    Create a log group in CloudWatch and associate it with each EC2 instance.
    Create an IAM Role that allows the CloudWatch Agent to write logs to CloudWatch.


CloudWatch'u kullanarak AWS içindeki tek bir konumdan tüm loglarınıza erişebilirsiniz. Bu loglar bir EC2 instance'ından veya şirket içi bir sistemden kaynaklanabilir. Bu soruda, loglar bir Amazon EC2 instance'ı tarafından oluşturulmaktadır. Amazon EC2 instance'larınızdan oluşturulan logları CloudWatch'a dahil etmek istiyorsanız, önce şu iki adımı atmanız gerekecek:
Her EC2 instance'ına CloudWatch Agent'ını indirin ve yükleyin.
CloudWatch Agent'ının CloudWatch'a log yazmasına izin veren bir IAM Role oluşturun.

Ancak, şirket içi bir sistemden logları dahil etmek istiyorsanız, adım 1 aynı olacak ancak adım 2 biraz farklı olacaktır. Bir IAM Role oluşturmak yerine, bir IAM user oluşturmanız gerekecektir.

---

**Your company wants to aggregate CloudTrail logs from multiple AWS accounts into a single, shared S3 bucket. What is the most efficient way to allow cross-account access to this specific bucket? (Choose 2 answers)**

- [ ] Create a separate IAM role for each IAM user to assume to access the destination bucket.
- [ ] Apply necessary permissions to all relevant S3 objects, ie CloudTrail logs.
- [x] Apply necessary permissions to the destination S3 Bucket
- [x] Configure cross-account access for CloudTrail

22.  lk olarak, tüm log dosyalarının teslim edilmesini istediğiniz AWS hesabında bir Trail oluşturarak CloudTrail'i etkinleştirmeniz gerekir. Hedef S3 bucket'ına, CloudTrail için hesaplar arası erişime izin veren izinlerin uygulanması gerekir.

Diğer seçenekler - nesnelere izin uygulamak, her kullanıcı için IAM rolleri oluşturmak veya her IAM kullanıcısının atanmış politikasını güncellemek - işe yaramayacak ve çalışsalar bile son derece verimsiz olacaktır.


----

**What is the primary function of Amazon CloudWatch?**

- [ ] To notify you regarding configuration changes to your AWS resources
- [x] To monitor your AWS resources' performance against specific metrics and thresholds
- [ ] To track and record API requests made in AWS
- [ ] To provide feedback on your AWS cloud environment's configuration based on best practices

Amazon CloudWatch'un temel işlevi, kullandığınız her hizmete özgü bir dizi metrik aracılığıyla AWS içinde çalıştırdığınız kaynakları izleme aracı sağlamaktır. Bu, olaylara hızlı bir şekilde tepki vermenizi, tanı koymanızı ve yaşayabileceğiniz herhangi bir kullanılabilirlik veya ölçeklenebilirlik sorununu dinamik olarak ayarlamanızı sağlar.


---

**Which of the following statements best describes the AWS Cloud Development Kit, or CDK?**

- [x] The CDK is an open-source framework that allows you to define AWS infrastructure and resources in code, then generates the equivalent AWS CloudFormation templates at compile time
- [ ] The CDK is a graphical user interface that allows developers to visually create and manage AWS resources and infrastructure without code 
- [ ] The CDK is a command line interface that allows developers to quickly create and manage AWS resources and infrastructure programmatically 
- [ ] The CDK is an open-source library of public developer applications that can be downloaded and launched into your own AWS environment 

AWS Cloud Development Kit (AWS CDK), modern programlama dilleriyle bulut altyapısını kod olarak tanımlamak ve AWS CloudFormation aracılığıyla dağıtmak için açık kaynaklı bir yazılım geliştirme çerçevesidir. Ayrıca bir komut satırı arayüzü de sağlasa da, bu en iyi açıklama değildir.




















 








  









