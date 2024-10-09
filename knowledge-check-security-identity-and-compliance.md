# Ölçme Testi: Security, Identity and Compliance (DVA-C02)

**What is the first step to implement cross-account access using AWS IAM?**

- [ ] Create a role from within the trusted account.
- [ ] Test the configuration by switching to the new role.
- [x] Create a role from within the trusting account.
- [ ] Specify the permissions attached to the newly created role, which the users in the trusted account would assume to carry out their required actions and tasks.

Süreci adım adım açıklayalım. İlk olarak, güvenen (trusting) hesaptan (örneğimizde bu production hesabı olacaktır) bir rol oluşturmalısınız. Bu, iki hesap arasında bir güven ilişkisi kurmak içindir. Bu rol, development hesabını güvenilir (trusted) bir varlık olarak tanımlayacaktır. Ardından, development hesabındaki kullanıcıların gerekli eylem ve görevleri yerine getirmek için üstlenecekleri bu yeni oluşturulan role bağlı izinleri belirtmelisiniz. Sonra, güvenilen hesaba (bu senaryoda development hesabı) geçiş yaparak, geliştiricilerinize güvenilen hesaptaki yeni oluşturulan rolü üstlenmelerine izin vermek için izinler vermelisiniz. Son olarak, role geçiş yaparak konfigürasyonu test edebilirsiniz.


---

**Which of the following statements about AWS IAM Identity Center (formerly AWS SSO) is false?**

- [ ] It supports multi-factor authentication.
- [ ] It provides support for Okta Universal Directory.
- [ ] It provides support for Microsoft Entra ID.
- [x] It can use AWS Web Application Firewall (WAF) to manage access to resources.

AWS IAM Identity Center ayrıca Microsoft Entra ID ve Okta Universal Directory'yi destekler. Ek güvenlik koruması için multi-factor authentication'ı da destekler ve kaynaklara erişimi yönetmek için IAM fine-grained kontrollerini kullanma yeteneğine sahiptir.


---

**You are implementing cryptographic best practices on an application that you developed. You enabled automatic key rotation for a KMS key.**

**Which of the following applies to your data encryption?**

- [ ] Data that is already encrypted will be re-encrypted as soon as you enable KMS rotation.
- [x] AWS will generate new cryptographic material for your KMS every year.
- [ ] Existing KMS data keys will be replaced by new data keys as soon as you enable KMS rotation.
- [ ] When you enable KMS rotation, data that you encrypted with old cryptographic material will be decrypted with the new cryptographic material.

KMS için key rotation'ı etkinleştirdiğinizde, AWS anahtarlarınızı key rotation'ı etkinleştirdiğiniz tarihten itibaren 365 gün sonra ve sonrasında her 365 günde bir otomatik olarak döndürür. Diğer seçenekler şu nedenlerle yanlıştır: AWS, CMK rotation'ı etkinleştirdikten sonra her ay yeni bir kriptografik anahtar oluşturmaz. Halihazırda şifrelenmiş veriler, KMS rotation'ı etkinleştirdiğinizde yeniden şifrelenmez. AWS, KMS rotation'ı etkinleştirdiğinizde mevcut anahtarınızı silmez veya değiştirmez. KMS rotation'ı etkinleştirdiğinizde, eski kriptografik materyalle şifrelediğiniz veriler, şifrelendiği aynı kriptografik materyalle çözülecektir.

---

**In AWS Web Application Firewall, _____ are a list of IP addresses that are allowed to access the resource.**

- [ ] rule groups
- [ ] denylisted IPs
- [ ] bad signatures
- [x] allowlisted IPs

Allowlisted IP adresleri, kaynağa erişmesine izin verilen IP adreslerinin bir listesidir.

---

**How can you configure rotation for a Customer Managed Key (CMK)? Select two possible choices. **

- [x] Enable automatic key rotation
- [x] Manually rotate your CMK by creating a new CMK-ID and updating your alias target to point to the new CMK-ID
- [ ] You cannot rotate CMKs
- [ ] AWS will automatically rotate CMKs for you without any additional necessary configuration

Otomatik key rotation etkinleştirilmişse, AWS anahtarlarınızı yıllık olarak manuel olarak döndürecektir. Manuel key rotation süreci, otomatik süreçten farklı olarak yeni bir CMK oluşturulmasını gerektirir. Böylece, yeni bir CMK-ID ve bir backing key oluşturulur. Sonuç olarak, eski anahtarın CMK-ID'sine başvuran herhangi bir uygulamanız varsa, bunları güncellemeniz ve yeni CMK-ID'ye yönlendirmeniz gerekecektir. Bu süreci kolaylaştırmak için, anahtarlarınız için alias isimleri kullanabilir ve ardından alias hedefinizi yeni CMK-ID'ye işaret edecek şekilde güncelleyebilirsiniz. Bunu yapmak için update alias API'sini kullanabilirsiniz.

---

**What setting must be changed to allow IAM policy permissions to control who can use and administer an AWS KMS Key?**

- [ ] The user performing cryptographic operations must have administrator privileges
- [x] The ‘Enable IAM User Permissions’ must be configured in the corresponding key policy
- [ ] The user must be listed as an Administrator within the corresponding key policy
- [ ] The ‘Enable IAM User Permissions’ must be configured in the AWS Organization Policy

KMS anahtarınıza erişmek ve kullanmak için izinler yalnızca IAM kullanılarak verilemez ve oluşturulamaz, IAM izinlerinin Key policy'nizden izin verilmesi gerekir. Örnek bir Key policy'nin ilk "Sid"ine bakarsak bunun nasıl gerçekleştirildiğini görebiliriz:

```json
{

	"Sid": "Enable IAM User Permissions",
	"Effect": "Allow",
	"Principal": {
		"AWS": "arn:aws:iam::730739171055:root"
		},
	"Action": "kms:*",
	"Resource": "*"
},
```

Gördüğünüz gibi, bu ifade anahtarın sahibi olan AWS hesabını temsil eden root hesabına KMS'e tam erişim izni verir. Bunun aslında herhangi bir principal'a Anahtarın kendisini kullanma izni vermediğini, sadece IAM politikaları kullanarak Anahtara erişim izni vermenize olanak sağladığını anlamak önemlidir. Key policy'nizde bu ifade olmadan, erişimi reddetmek için kullanılmadıkça anahtara erişime izin veren tüm IAM ifadeleri göz ardı edilecektir.

----

**Which choices are AWS IAM best practices? (Choose 3 answers)**

- [x] Configure Strong Password policies.
- [x] Multi-factor Authentication
- [ ] EC2 key pair login
- [x] Principle of Least Privilege

Ekstra güvenlik için, ayrıcalıklı IAM kullanıcıları (hassas kaynaklara veya API'lere erişim izni verilen kullanıcılar) için multi-factor authentication (MFA) etkinleştirin. IAM politikaları oluştururken, en az ayrıcalık verme standart güvenlik tavsiyesini izleyin - yani, yalnızca bir görevi gerçekleştirmek için gerekli izinleri verin. Kullanıcıların ne yapması gerektiğini belirleyin ve ardından onların yalnızca bu görevleri gerçekleştirmelerine izin veren politikalar oluşturun. Kullanıcıların kendi şifrelerini değiştirmelerine izin veriyorsanız, güçlü şifreler oluşturmalarını ve şifrelerini periyodik olarak değiştirmelerini zorunlu kılın. EC2 key pair'leri IAM'in bir parçası değildir. EC2 instance'larını yapılandırabilir ve oluşturduğunuz herhangi bir IAM politikasının dışında bir key pair alabilirsiniz.


----

**Your application provides real-time analytics on commodity trading. You want to encrypt network communications between clients and your app. Your major concern is to promptly renew expiring SSL/TLS certificates. **

**Which service will help you protect data in transit?**

- [ ] AWS DataSync
- [x] AWS Certificate Manager 
- [ ] AWS IQ 
- [ ] AWS Data Exchange 

Bu uygulama için, transit halindeki verileri şifrelemek ve internet üzerinden web sitelerinin kimliğini doğrulamak için SSL/TLS sertifikalarına ihtiyacınız var. AWS Certificate Manager ile SSL/TLS sertifikalarını kolayca tedarik edebilir, yenileme ve dağıtımını yönetebilirsiniz. Bu sizin çözümünüzdür. Diğer seçenekler şu nedenlerle yanlıştır: AWS DataSync, on-premises depolama ve AWS depolama arasında veri transferini optimize etmenizi ve hızlandırmanızı sağlayan bir veri transfer hizmetidir. DataSync, SSL/TLS sertifikalarını yönetmez. AWS DataBrew, herhangi bir kod yazmadan analitik ve makine öğrenimi için verileri temizleyen ve normalleştiren bir veri görselleştirme aracıdır. AWS IQ, üçüncü taraf AWS sertifikalı uzmanların yardımıyla projeleri daha hızlı tamamlamanızı sağlarken, AWS Data Exchange verileri güvenli ve uyumlu bir şekilde kolayca değiş tokuş etmenizi sağlar.


---

**Which sources can provide key material for symmetric keys? (Choose 3 answers)**

- [x] From with AWS KMS
- [ ] From AWS Secrets Manager
- [x] External key managers backed by an external key store
- [x] AWS CloudHSM

Key material, AWS içindeki bir anahtarla ilişkili temel bir parçadır. Kriptografik bir algoritmanın bir parçası olarak kullanılan öğedir ve etkili bir şekilde sadece bir bit dizisidir. Korunması gereken ve simetrik şifreleme ile kullanılan private key material vardır, ardından public key içinde asimetrik şifreleme için yaygın olarak kullanılan public key material vardır.

Simetrik anahtarlar için bu key material'in kaynağı çeşitli güvenli kaynaklardan depolanabilir, örneğin:

- AWS KMS'in kendisinden.
- Karşılık gelen KMS anahtarı harici bir key store tarafından destekleniyorsa, KMS dışından kendi harici anahtar yöneticinizden de kaynaklanabilir.
- CloudHSM key store içinde depolandığında kaynak olarak CloudHSM'i de seçebilirsiniz.
- Kendi key material'inizi de içe aktarabilirsiniz, ancak bununla bu key material'i güvence altına almaktan siz sorumlusunuz.


---

**Which of the following best describes how AWS Cognito works?**

- [x] AWS Cognito is a cloud-based service that provides user authentication and authorization for applications.
- [ ] AWS Cognito is a hardware device that provides secure access to Amazon Web Services.
- [ ] AWS Cognito is a tool for deploying and managing containerized applications in the cloud.
- [ ] AWS Cognito is a database service that stores and retrieves user data for web and mobile applications.

Amazon Cognito, web ve mobil uygulamalarınız için kullanıcı kimlik doğrulama ve yetkilendirmeyi yönetir. User pool'lar ile uygulamalarınıza kolayca ve güvenli bir şekilde kayıt olma ve oturum açma işlevselliği ekleyebilirsiniz. Identity pool'lar (federated identities) ile uygulamalarınız, kullanıcılar ister anonim ister oturum açmış olsun, belirli AWS kaynaklarına erişim izni veren geçici kimlik bilgileri alabilir.

---

**Which of the following is true about user pools in AWS Cognito?**

- [ ] User pools allow you to manage user identities and access to AWS resources.
- [ ] User pools are used for managing groups of virtual machines in the cloud.
- [ ] User pools provide a mechanism for federated identity management.
- [x] User pools provide a directory of users for web and mobile applications.

11.  User pool'lar, AWS Cognito'nun temel bir bileşenidir ve web ve mobil uygulamalar için bir kullanıcı dizini sağlar. Geliştiricilerin uygulamalarına kolayca kayıt olma, oturum açma ve erişim kontrolü yetenekleri eklemelerine olanak tanır ve sosyal kimlik sağlayıcıları ve multi-factor authentication için destek sunar.

Diğer seçenekler şu nedenlerle yanlıştır:

- User pool'lar doğrudan AWS kaynaklarına erişim sağlamaz
- User pool'lar sanal makine gruplarını yönetmek için kullanılmaz
- User pool'lar kimlik doğrulama için federated identity provider'lar (Google ve Facebook gibi) ile birlikte kullanılabilse de, kendileri federated identity yönetimi için bir mekanizma sağlamaz


---

**Which AWS IAM policy type is one that is embedded within the identity object itself?**

- [ ] Customer-managed
- [ ] AWS-managed
- [x] Inline
- [ ] Service control policies

Permissions sekmesinin üstünde, bu kullanıcı için bir inline policy ekleyebiliriz. Bunu yaparsak, bu, kullanıcı nesnesinin içine gömülü bir politika olacaktır.

----

**Which statement is true about AWS WAF rules?**

- [x] Rules are executed in the order they appear in the Web ACL.
- [ ] Rules are added to conditions.
- [ ] All rules that have an action of 'block' are automatically listed first.
- [ ] It is not possible to edit a rule once it has been created.

Kurallar hakkında önemli bir nokta, WAF içinde listelendiği sırayla yürütülmeleridir. Bu nedenle, rule base'iniz için bu sırayı doğru bir şekilde tasarlamaya dikkat edin, tipik olarak bunlar gösterildiği gibi sıralanır: 
- AllowListed IP
- Allow DenyListed IP 
- Block Bad Signatures - Block


---

**Identify a way to use AWS KMS with Amazon Simple Storage Service (S3) in AWS.**

- [ ] Use server-side encryption to protect your data with a customer data key.
- [ ] Use client-side encryption to protect your data with a KMS key.
- [x] Use server-side encryption to protect your data with a KMS key.
- [ ] Use client-side encryption to protect your data with a customer data key.

AWS KMS'i Amazon S3 ile kullanmanın iki yolu vardır: Verilerinizi bir customer master key ile korumak için server-side encryption kullanabilirsiniz. Verilerinizi client tarafında korumak için Amazon S3 encryption client ile bir AWS KMS anahtarı kullanabilirsiniz.

---

**An IAM user is part of an IAM group that has read-only access to Amazon RDS databases within a company's production environment. This user is also part of an organizational unit (OU) which is granted full access to Amazon RDS databases in the company's production environment.**

**If the IAM user attempts to modify the failover settings for a database in the company's production environment, what will happen?**

- [x] The request will be denied because permissions can only be granted by AWS IAM, not AWS Organizations.
- [ ] The request will be granted because permissions can be granted by either AWS IAM or AWS Organizations.
- [ ] The request will be reviewed for approval by the owner of the related AWS account.
- [ ] The request will be reviewed for approval by the master account of the related AWS Organization.

AWS Organizations'ın SCP'leri ve IAM politikaları şu şekilde birlikte çalışır:

Kullanıcılara ve rollere hala uygun IAM izin politikalarıyla izinler verilmelidir. Herhangi bir IAM izin politikası olmayan bir kullanıcı, geçerli SCP'ler tüm hizmetlere ve tüm eylemlere izin verse bile erişime sahip değildir.

Bir kullanıcı veya rolün, geçerli SCP'ler tarafından da izin verilen bir eyleme erişim izni veren bir IAM izin politikası varsa, kullanıcı veya rol bu eylemi gerçekleştirebilir.

Bir kullanıcı veya rolün, geçerli SCP'ler tarafından izin verilmeyen veya açıkça reddedilen bir eyleme erişim izni veren bir IAM izin politikası varsa, kullanıcı veya rol bu eylemi gerçekleştiremez.
