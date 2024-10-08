# Security, Identity and Compliance (DVA-C02)

Bu başlık altında, geliştiriciler için AWS'deki güvenlik, kimlik ve uyumluluk hizmetlerine yakından bakacağız. Bu hizmetler aşağıda listelenmiştir:

-   AWS Certificate Manager (ACM),
-   AWS Certificate Manager Private Certificate Authority,
-   Amazon Cognito,
-   AWS Identity and Access Management (IAM),
-   AWS Key Management Service (AWS KMS),
-   AWS Secrets Manager,
-   AWS Security Token Service (AWS STS),
-   AWS Web Application Firewall (AWS WAF).

## IAM: Identity and Access Management

Bu başlık altında, Identity & Access Management hizmetinin ne olduğuna ve IAM'in aslında ne anlama geldiğine dair genel bir giriş yapacağız.

İlk olarak, Identity & Access Management'ın ne anlama geldiğini tanımlamayalım ve bu kapsamı iki parçaya ayıralım. **Identity Management** kısmı ile başlamak istersek; AWS kullanıcı adları gibi kimlikler, AWS hesabınıza kimlik doğrulaması yapmak için gereklidir ve bu kimlik doğrulama süreci 2 aşamada yönetilir:

1.  Bu sürecin ilk kısmı, kim olduğunuzu tanımlamaktır, yani temelde etkin bir şekilde kimliğinizi sunmaktır. Örneğin AWS kullanıcı adınız. Bu tanımlama, hesabınız için IAM içinde benzersiz bir değerdir, yani IAM aynı AWS hesabı içinde aynı ada sahip 2 özdeş kullanıcı hesabına sahip olmanızı engeller.
2.  Kimlik doğrulama sürecinin ikinci kısmı, söylediğiniz kişi olduğunuzu doğrulamaktır. Bu, ek veriler sağlayarak gerçekleştirilir ve AWS kullanıcı adlarımızı kullanırken bunu bir şifre eşleştirerek doğrulayabiliriz.

**Access Management** olan ikincı kısım ise ise yetkilendirme ve erişim kontrolü ile ilgilidir. Yetkilendirme, bir kimliğin kimlik doğrulaması yapıldıktan sonra AWS hesabınız içinde neye erişebileceğini belirler. Bu yetkilendirmeye bir örnek vermek gerekirse, kullanıcının belirli AWS kaynaklarına erişmek için izin listesidir, örneğin EC2'ye *Full Access* veya RDS'ye *Read Only* erişimi olabilir.

Erişim Kontrolü, güvenli bir kaynağa erişim mekanizması olarak sınıflandırılabilir. Örneğin, aşağıdakileri kullanarak:

-   Kullanıcı adı ve şifre (Kimlik Doğrulama ve Doğrulama - Authentication and Verification)
-   Çok Faktörlü Kimlik Doğrulama (MFA, geçerli bir şifreden sonra ek bir doğrulama adımı olarak kullanılır)
-   Federated Erişim, AWS dışındaki kullanıcıların geçerli bir IAM kullanıcı hesabından AWS kullanıcı kimlik bilgilerini sağlamak zorunda kalmadan kaynaklara güvenli bir şekilde erişmesine olanak tanır. Bu kimlik bilgileri kimlik sağlayıcılardan temin edilir. 

Yani özünde **IAM**, AWS Hesabınızdaki kaynaklarınıza kimliklerin kimlik doğrulama, yetkilendirme ve erişim kontrol mekanizmalarını yönetme, kontrol etme ve yönetme yeteneği ile tanımlanabilir. Kimlik doğrulama ve yetkilendirme perspektifinden farklı güvenlik kontrollerini anlamak, altyapınız için doğru güvenlik seviyesini tasarlamanıza yardımcı olabilir.

### IAM Özellikleri (IAM Features)

IAM'in nasıl çalıştığını ve bu hizmet aracılığıyla neler yapılabileceğini anlamak çok önemlidir, ancak özelliklerini nasıl uygulayacağınızı bilmek daha da önemlidir. IAM olmadan, hem dahili hem de harici olarak kaynaklarınıza kimin veya neyin erişebileceğini ve bunlarla neler yapabileceğini kontrol etmenin veya güvenliğini sağlamanın bir yolu olmazdı. IAM, bu erişim yönetimini sürdürmek için bileşenler sağlar, ancak yalnızca sizin yapılandırdığınız kadar güçlü ve güvenlidir.

IAM kullanarak AWS hesabınızda güvenli, sağlam ve sıkı güvenlik uygulamanın sorumluluğu size aittir, siz AWS hesabının sahibisiniz. Erişim kontrol prosedürlerinizin ne kadar güvenli olması gerektiğini, kullanıcıların belirli kaynaklara erişimini ne kadar kısıtlamak istediğinizi, şifre politikasının ne kadar karmaşık olması gerektiğini ve kullanıcıların çok faktörlü kimlik doğrulama kullanması gerekip gerekmediğini siz tanımlamalısınız. Tüm bunlar ve çok daha fazlası sizin tasarlamanız ve uygulamanız gereken şeylerdir ve çoğu muhtemelen Bilgi Güvenliği Yönetim Sisteminiz (ISMS: Information Security Management System) içindeki kendi güvenlik standartlarınıza ve politikalarınıza bağlı olacaktır.

AWS Management Console'dan IAM hizmeti 'Security, Identity & Compliance' kategorisi altında bulunabilir ve erişildiğinde sizi IAM Dashboard'a götürecektir.

IAM Console'un başlangıç panosu aşağıdaki bilgileri gösterecektir:

![dbnn3y7.md.png](https://iili.io/dbnn3y7.md.png)

#### IAM Kaynakları (IAM Resources) 

 Bu bölüm, IAM içinde yapılandırdığınız kullanıcıların, kullanıcı gruplarının, rollerin, müşteri tarafından yönetilen politikaların ve kimlik sağlayıcıların sayısının basit bir sayımını kullanarak IAM kaynaklarınızın özet bir genel bakışını sunar.

#### En İyi Uygulamalar (Best Practices)

AWS'nin uygulamanızı önerdiği IAM güvenlik en iyi uygulamalarının bir listesi ve bunların nasıl uygulanacağına dair bağlantılarla doldurulur. Bu en iyi uygulamaları en kısa sürede benimsemenizi AWS tarafından şiddetle tavsiye edilir. Sıkı güvenliği korumak, bulut ortamında çalışırken çok önemlidir.

Bu gösterge panosuna ek olarak, sol menüde her birinin altında bir dizi bileşen listeleyen Erişim Yönetimi (Access Management) ve Erişim Raporları (Access Reports) olmak üzere 2 kategori de göreceksiniz. Erişim yönetimi başlığı altındaki bu bileşenlerin her birini gözden geçirerek başlayalım. 

**Kullanıcılar (Users)**

Kullanıcı nesneleri bir kimliği temsil etmek için oluşturulur. Kullanıcı, AWS ortamınızı çalıştırmak ve sürdürmek için erişim gerektiren organizasyonunuzdaki gerçek bir kişi olabilir veya AWS kaynaklarınızla programlı olarak etkileşimde bulunmak için bir uygulama tarafından kullanılan bir kimlik olabilir. Kullanıcılar basitçe AWS hesabınıza kimlik doğrulama sürecinde kullanılan kimlikleri temsil eden nesnelerdir. Benzersiz değerler olarak, her kullanıcının referans alınabileceği bir Amazon kaynak adı (ARN: Amazon Resource Name) değeri vardır. Bir kullanıcının ARN'si şu değere benzer olacaktır: `arn:aws:iam::accountId:user/Fatih`

Kullanıcılarınızı yapılandırırken, kullanıcıları çok faktörlü kimlik (multi-factor authentication) doğrulama için ayarlayabilirsiniz. MFA'yı yapılandırmak, ek bir doğrulama düzeyinin uygulanmasına izin verir, kullanıcının normal şifresinden sonra bağlı bir MFA cihazından rastgele 6 haneli bir sayı girmesi gerekecektir. MFA, AWS hesap sahibi ve yükseltilmiş ayrıcalıklara sahip diğer kullanıcılar için kullanılmalıdır.

**Gruplar (Groups)**

IAM Kullanıcı Grupları, kullanıcı nesneleri gibi nesnelerdir, ancak herhangi bir kimlik doğrulama sürecinde kullanılmazlar, bunun yerine grubun üyelerine AWS kaynaklarına erişim yetkisi vermek için kullanılırlar.

IAM kullanıcı grupları, IAM kullanıcılarını içerir ve bu grupların AWS kaynaklarına erişime izin veren veya reddeden IAM politikaları ile ilişkilendirilmiştir. Bu politikalar ya önceden var olan AWS tarafından yönetilen politikalarıdır, ya da sizin yani müşteri tarafından oluşturulan politikalardır.

**Roller (Rols)**

IAM Rolleri, kullanıcıların, diğer AWS hizmetlerinin ve uygulamaların AWS kaynaklarına erişmek için geçici IAM izinleri kümesini benimsemesine olanak tanır. Roller esasen kullanıcı nesneleri gibi çalışır, yani farklı kaynaklara erişmesine izin veren ilişkili izinlere sahip bir kimliktir. Ancak fark, rollerin tek bir kişiyi tanımlamamasıdır, geçici olarak bir dizi izin edinmesi gereken herhangi bir kimlik veya hizmet tarafından üstlenilmek üzere tasarlanmıştır. Ek olarak, rollerin kimlikleriyle ilişkili şifreleri yoktur, bunun yerine, az önce belirttiğimiz gibi, roller üstlenili; ancak rolleri üstlenmek için doğru izinlere sahip olmanız gerekir.

**Politikalar (Policies)**

IAM içinde kullanılan politikalar JSON belgeleri olarak yazılır. Politikalar, neye erişilebileceğini ve erişilemeyeceğini tanımlar. Bu politikalar kullanıcılara, kullanıcı gruplarına veya rollere eklenebilir.

Politikalarla çalışırken, AWS tarafından yönetilen politikaları veya satır içi (in-line) politikaları kullanabilirsiniz. Yönetilen politikalar, kullanılabilir politikaların bir kütüphanesi olarak görülür ve birden çok kullanıcı, kullanıcı Grubu ve role uygulanabilen 2 farklı türde gelir:

- AWS Tarafından Yönetilen Politikaları (AWS Managed Policies): Bunlar, farklı AWS hizmetlerine çeşitli erişim sağlayan önceden tanımlanmış politikaların bir listesidir.
- Müşteri Tarafından Yönetilen Politikaları (Customer Managed Policies): Bunlar, müşteri olarak sizin tarafınızdan oluşturulan ve yazılan politikalardır.

Yönetilen politikaların aksine, satır içi politikalar bir kütüphanede saklanmaz, bunun yerine bir kullanıcı, kullanıcı grubu veya rol içine açıkça gömülü olarak yazılmalıdır. Sonuç olarak, aynı politika yönetilen politikalar gibi kolayca başka bir kimliğe uygulanamaz.

**Kimlik Sağlayıcı (Identity Providers)**

AWS kaynaklarınıza federated erişim sağlamak istiyorsanız, bir kimlik sağlayıcı eklemelisiniz.

Federated Erişim, AWS dışındaki kimlik bilgilerinin AWS kaynaklarınıza kimlik doğrulama aracı olarak kullanılmasına olanak tanır. Örneğin, yerinde Active Directory Federation sunucunuz ile AWS hesabınız arasında bir güven oluşturabilirsiniz; veya AWS hesabınız ile Google hesabınız arasında bir güven oluşturabilirsiniz. Her iki durumda da, bu örneklerde ADFS sunucunuz veya Google hesabınız, Active Directory veya Google tarafından kimliği doğrulanmış olanların AWS hesabınızdaki kaynaklarınıza erişmesine izin veren kimlik sağlayıcılar olarak yapılandırılabilir. Bu, ayrı IAM kullanıcı hesapları oluşturmanızı önler.

**Hesap Ayarları (Account Settings)**

Hesap ayarları altında bir şifre politikası (password policies) uygulayabilirsiniz ve bunu yapmak her zaman en iyi yöntemdir. Şifre politikası, kuruluşunuzun uymak zorunda olabileceği herhangi bir şifre standardı için karşılanması gereken minimum güvenlik gereksinimlerini uygulamak için kullanılmalıdır. Şifre politikası hesabınızdaki tüm IAM kullanıcılarına uygulanır.

**Güvenlik Token Hizmeti (Security Token Service)**

STS hizmeti, hem IAM kullanıcıları hem de federated kullanıcılar için geçici, sınırlı ayrıcalıklı kimlik bilgileri talep etmenize olanak tanımak için kullanılır. STS uç noktaları, STS için etkinleştirilmiş veya devre dışı bırakılmış bölgelerin bir listesini sağlar. Varsayılan olarak, tüm bölgeler etkinleştirilmiştir, ancak kullanmayı düşünmediğiniz bölgeleri devre dışı bırakmanız önerilir.

Varsayılan olarak STS [https://sts.amazonaws.com](https://sts.amazonaws.com) genel uç noktasını kullanır, ancak bölgesel uç noktaları kullanırken gecikmeyi azaltmaya yardımcı olabilir.

Bu aşamaya kadar, Erişim Yönetimi (Access Management) başlığı altında kapsanan bileşenlere genel bir bilgi sağladık. Şimdi Erişim Raporları (Access Reports)  kategorisine giren özelliklere sırayla bakalıö

**Erişim Analizcisi (Access Analyzer)**

Access Analyzer, güven alanınız içindeki bir kaynaktaki bir politika, güven alanınız dışından erişime izin verdiğinde bulgular oluşturmak için kullanılır. Bu, çapraz hesap erişimine izin veren bir IAM rolü veya farklı bir hesabın bucket içine nesneler yüklemesine izin veren bir bucket gibi çeşitli kaynaklardan olabilir, temel olarak kaynaklarınıza harici erişime izin veren herhangi bir kaynak işaretlenecek ve vurgulanacaktır. Böylece erişimden haberdar olduğunuzdan emin olunur. Bu, siz veya ekibinizin istemeden bir kaynağa erişim izni vermiş olabileceğiniz güvenlik risklerini ve tehditlerini azaltmaya yardımcı olan harika bir araçtır. Herhangi bir sorun, erişimi incelemenize olanak tanıyan bir bulgu olarak rrişim analizcisi tarafından kaydedilir.

**Kimlik Bilgisi Raporu (Credential Report)**

Kimlik bilgisi raporu, tüm IAM kullanıcılarınızın ve kimlik bilgilerinin bir listesini içeren bir *.csv* dosyası oluşturmanıza ve indirmenize olanak tanıyan harika bir araçtır. Bu, hesaplarınızı ve son kullanıldıkları zamanı hızlı ve kolay bir şekilde gözden geçirmenin yanı sıra bir kullanıcının şifresinin en son ne zaman değiştirildiğini ve çok faktörlü kimlik doğrulamasının etkin olup olmadığını belirlemenin hızlı ve kolay bir yolunu sağlar.

** Organizasyon Aktivitesi (Organizational Activity)**

AWS Organizations kullanıyorsanız, son 365 gün içindeki hizmet aktivitesini görüntülemek için bir organizasyon birimi (OU) veya hesap seçmenize olanak tanır. Bu bölümdeki hesaplara inerek hangi kullanıcıların aktivite gösterdiğini ve ayrıca belirli bir zaman diliminde hangi AWS hizmetlerine eriştiklerini görebilirsiniz.

**Hizmet Kontrol Politikaları (Service Control Policies)**

Hizmet kontrol politikaları, önceki bileşen olan 'Organizasyon Aktivitesi' ile bağlantılıdır. Hesap için geçerli olan hizmet kontrol politikalarını ve etkilenen kimliklerin sayısını listeler. Hizmet kontrol politikaları , kullanıcılara, kullanıcı gruplarına ve rollere izin veren kimlik tabanlı politikalardan farklıdır çünkü SCP'ler aslında kendileri izin vermez. Bunun yerine, SCP'ler AWS Organizations ile AWS hesapları için izin sınırını uygulamak ve belirlemek için kullanılır.

Örneğin, bir AWS hesabı içindeki bir kullanıcının IAM'deki kimlik tabanlı bir politika aracılığıyla S3, RDS ve EC2'ye tam erişimi olduğunu varsayalım. Eğer o AWS hesabıyla ilişkili SCP, S3 hizmetine erişimi reddederse, o kullanıcı S3'e tam erişimi olmasına rağmen yalnızca RDS ve EC2'ye erişebilir. SCP, o hizmetin AWS hesabı içinde kullanılmasını engelleyecek ve böylece geçersiz kılma önceliğine sahip olacaktır. Böylelikle, izin verilen maksimum izin düzeyini belirleyecektir.

### IAM Kullanarak Hesaplar Arası Erişim (Cross-Account Access Using IAM)

Çapraz hesap erişiminin ne olduğunu hızlıca tanımlayayım. Basitçe söylemek gerekirse, bir AWS hesabındaki IAM kullanıcılarının IAM rolleri aracılığıyla farklı bir AWS hesabındaki hizmetlere erişmesine olanak tanır. IAM rolleri, kullanıcıların, diğer AWS hizmetlerinin ve uygulamalarının AWS kaynaklarına erişmek için geçici bir dizi IAM izni edinmesine izin verir.

Peki, neden çapraz hesap erişimi uygulamak istersiniz? Temel olarak, bu mimari ve güvenlik en iyi uygulamalarına dayanır. Örneğin, AWS kullanan çoğu kuruluşun muhtemelen bir production ve bir geliştirme veya test ortamı olacaktır. Yönetim kolaylığı ve izolasyon için, bu ortamların her biri muhtemelen ayrı AWS hesaplarında olacaktır. Bunu göz önünde bulundurarak, öncelikle geliştirme hesabında çalışan geliştirme ekibinizin zaman zaman production hesabındaki kaynaklara erişmesi gerekebilir. Production ortamında yalnızca nadir durumlarda gerekli olacak ek kullanıcı hesapları oluşturmak yerine, bunun yerine gerekli izinlere sahip roller kullanılabilir. Geliştirme ekibine daha sonra production ortamındaki kaynaklara erişmek için bu rolü üstlenme izni verilebilir. Bu, en az ayrıcalık ilkesine uymaya yardımcı olur.

Bu erişim her zaman gerekli olmadığından, geliştirici diğer kaynaklara erişmek için bilinçli olarak role geçiş yapmalı ve rolü üstlenmelidir. Ek güvenlik için, üstlenilmeden önce role çok faktörlü kimlik doğrulama da ekleyebilirsiniz, böylece başka bir doğrulama adımı eklemiş olursunuz.

Şimdi çapraz hesap erişimini uygulamak için gereken temel adımlara bakalım. Bu süreci anlamak için, kullanacağımız iki temel terimin farkında olmanız gerekiyor. Bunlar _güvenilen_ (trusted) hesap ve _güvenen_ (trusting) hesaptır. Önceki production ve geliştirme örneğimizi kullanırsak, production hesabı güvenen hesap olacak ve geliştirme hesabı güvenilen hesap olacaktır, çünkü geliştirme kullanıcılarına üretim hesabındaki kaynaklara erişme güveni verilecektir.

Süreci parçalara ayırayım. 
1. İlk olarak, örneğimizde üretim hesabı olacak olan güvenen hesaptan bir rol oluşturmanız gerekir. Bu adım, iki hesap arasında bir güven oluşturmak içindir. Bu rol, geliştirme hesabını güvenilir bir varlık olarak tanımlayacaktır. 
2. Sonra, geliştirme hesabındaki kullanıcıların gerekli eylemlerini ve görevlerini yerine getirmek için üstlenecekleri bu yeni oluşturulan role ekli izinleri belirtmeniz gerekir. 
3. Ardından, geliştiricilerinize güvenilen hesapta yeni oluşturulan rolü üstlenme izni vermek için güvenilen hesaba, bu senaryoda geliştirme hesabına geçmeniz gerekir.
4.  Son olarak, role geçiş yaparak yapılandırmayı test edebilirsiniz.

### IAM AWS Politika Türleri (IAM AWS Policy Types)

Bu başlık altında, AWS IAM ile çalışırken karşılaşabileceğiniz dört farklı policy türüne daha yakından bakalım.

1.  **Identity-based policies:** Policy'ler IAM içerisindeki kullanıcılara, user group'lara veya role'lere eklenebilir. Temel olarak, bir kimliği temsil eden herhangi bir varlığa eklenebilir.
2.  **Resource-based policies:** Bir kimlikle ilişkilendirilmek yerine, bu policy'ler doğrudan kaynakların kendisine inline olarak eklenir. IAM'de resource-based policy'ye bir örnek, policy'nin role'ü üstlenebilecek principal'ı tanımladığı bir role trust policy'sidir.
3.  **Permission boundaries:** Policy'ler bir role veya kullanıcıyla ilişkilendirilebilir, ancak kendileri izin vermezler. Bunun yerine, bir varlığa verilebilecek maksimum izin seviyesini tanımlarlar.
4.  **Organization service control policies (SCPs):** İzin vermemeleri açısından permission boundaries'e çok benzerler. Maksimum izinleri belirleyen bir sınır tanımlarlar. Ancak, bu service control policy'leri AWS Organizations ile çalışırken bir AWS hesabı veya organizational unit (OU) ile ilişkilendirilir ve bu hesapların üyelerine verilen maksimum izinleri yönetir.

Tüm bu policy türleriyle, bazı kişilerin hangi tür policy'nin kullanıldığı veya kullanılması gerektiği konusunda neden kafalarının karışabileceğini tahmin etmek zor değildir. Bu nedenle, bu policy'lerin her birini daha ayrıntılı olarak ele alalım. 

**Identity-based policy**'lerle başlayalım. Daha önce de belirttiğimiz gibi, bu policy'ler kullanıcılara, user group'lara veya role'lere eklenebilir ve bu varlıkların her birinin hangi izinlere sahip olduğunu kontrol eder. Identity-based policy'ler managed veya inline policy'ler olabilir, peki bu ne anlama geliyor?

Managed policy'ler, IAM policy kütüphanesi içinde kaydedilir ve gerektiğinde herhangi bir kullanıcıya, user group'a veya role'e eklenebilir. Aynı policy birden fazla varlığa eklenebilir.

Managed policy'ler de iki farklı türde gelir: 

- AWS managed policy'ler ve customer managed policy'ler. AWS managed policy'ler, AWS tarafından önceden yapılandırılmış ve atamak isteyebileceğiniz en yaygın izinleri yönetmenize yardımcı olmak için size sunulan policy'lerdir.

- Customer managed policy'ler, kendinizin oluşturduğu ve ardından bir kullanıcı, user group veya role ile ilişkilendirilebilen policy'lerdir. AWS managed policy'ler güvenlik gereksinimlerinizi karşılamadığında customer managed policy'ler oluşturmak isteyebilirsiniz.

Şimdi inline policy'lerin managed policy'lerden nasıl farklı olduğunu açıklayalım. Inline policy'ler doğrudan varlığa (kullanıcı, user group veya role) gömülüdür. Policy, IAM kütüphane policy'sinde kaydedilmez ve saklanmaz, varlıkla ilişkilendirilen tek varoluşu budur. Sonuç olarak, diğer varlıklara kolayca kopyalanamaz, o bir kullanıcıya, user group'a veya role'e özgüdür ve bire bir bir ilişki oluşturur.

Şimdi **resource-based policy**'lere geçelim. Resource-based policy'ler, bir kimlik yerine bir kaynakla ilişkilendirilen etkili inline policy'lerdir. Amazon S3'ü uzun süredir kullanıyorsanız, S3 bucket policy'leriyle karşılaşmış olabilirsiniz. Bu, yaygın bir resource-based policy'dir ve bucket'a erişim izinleri kaynak düzeyinde tanımlanır. Bununla beaber, hangi principal veya principal'ların farklı eylemler temelinde bucket'a erişebileceğini belirler.

Sırada **permission boundaries** bulunur. Permission boundaries yalnızca bir kullanıcı veya role ile ilişkilendirilebilir. Bir group'a sınır eklemek mümkün değildir. Hem identity-based hem de resource-based policy'lerden farklı olarak, permission boundaries'in kendileri izin vermez. Kullanıcıya veya role'e verilebilecek maksimum izin seviyesini sınırlayan bir kılavuz görevi görürler.

Son olarak, **SCPs yani service control policy**'lerden bahsedelim. Service control policy'ler, AWS Organizations tarafından kullanılır ve ilişkili hesabın veya OU'nun üyeleri için izin verilen maksimum izinleri tanımlamak üzere AWS hesaplarına veya organizational unit'lara (OU'lar) eklenir. Bir bakıma, hesap düzeyinde permission boundaries'e benzer şekilde çalışırlar ve o hesapların tüm üyelerini etkilerler.

IAM'de, AWS hesabınız için geçerli olan herhangi bir SCP'yi, policy ifadesinin kendisini, ARN'sini, etkilediği varlık sayısını görüntüleme yeteneğine sahipsiniz. Organizasyon içindeki bir principal'ın en son ne zaman bir hizmete eriştiğini öğrenmek için erişim etkinliğini inceleyebilirsiniz. Ancak, SCP'yi güncellemek veya düzenlemek isterseniz, bunu AWS Organizations hizmetinin içinden yapmanız gerekir. SCP, IAM içinden düzenlenemez.

### JSON Politika Yapısı (JSON Policy Structure)

Bu başlık altında, IAM politikalarının sözdizimi ve yapısına derinlemesine bakacağız. Hem identity-based hem de resource-based politikalar için sözdizimi aynıdır, ancak birkaç farklı parametreler bulunabilir. Daha önce IAM ile çalıştıysanız, muhtemelen bir IAM politikasıyla karşılaşmışsınızdır. IAM politikalar temelde, bir kimliğin neye erişip erişemeyeceğini tanımlayan bileşendir. Peki bu politikalar gerçekte neye benzer? IAM politikaları JSON (JavaScript Object Notation) belgeleri olarak biçimlendirilir ve her politikanın en az bir statement'ı olacaktır; yapı aşağıdaki gibidir:

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Stmt1494509737040",
			"Effect": "Allow",
			"Principal": "AWS": "arn:aws:iam:730739171055:user/Fatih",
			"Action": "cloudtrail:*",
			"Resource": "*",
			"Condition": {
				"IpAddress": {
					"aws:SourceUp": "10.10.0.0/16"	
				}	
			}
		}
	]
}
```

Yukarıdaki politikanın yapısını her bir öğeyi anlamanıza olanak tanıyacak şekilde parçalara ayırayım:

- Version: Bu değer, politika dili sürümünü belirtir ve kullanılan dil sözdizimini belirler. Bu repo'nun yazıldığı sırada, mevcut politika sürümü 2012-10-17'dir.

- Statement: Bu değer, politikanın ana öğesini tanımlar ve Sid, Effect, Action, Resource ve Condition gibi diğer alt öğeleri içerir. Bu öğeler, verilen veya reddedilen erişim düzeyini ve hangi kaynağa olduğunu belirler. Bir politika en az bir statement içermelidir, ancak bir dizi statement de içerebilir. Her statement bloğu küme parantezleri içine alınmalıdır, ancak bir dizi statement kullanıyorsanız, tüm diziyi köşeli parantezler içine almalısınız.

	- Sid: Bu değer, statement ID'sidir ve statement içinde benzersiz bir tanımlayıcı ayarlamanıza olanak tanıyan isteğe bağlı bir parametredir. Politikanıza daha fazla dizi ekledikçe, her biri için bir Sid eklemek iyi bir fikir olabilir, böylece onları uygun şekilde adlandırabilir ve daha kolay tanımlanabilir hale getirebilirsiniz. Örneğin, `AllowGetObjectForS3`. Statement'ın geri kalanını okumadan, bu statement'ın hangi izinlere izin verdiği hakkında iyi bir fikir edinebilirsiniz.

	- Effect: Bu öğe 'Allow' veya 'Deny' olarak ayarlanabilir ve statement'ta tanımlanan eylemler için erişimi ya verir ya da kısıtlar. Varsayılan olarak, kaynaklarınıza tüm erişim reddedilir, dolayısıyla bu Allow olarak ayarlanırsa, varsayılan reddedilen erişimin yerini alır. Benzer şekilde, bu Deny olarak ayarlanırsa, önceki herhangi bir Allow'un üzerine yazacaktır. Bir politikadaki açık bir Deny, herhangi bir Allow'dan her zaman öncelikli olacaktır.

	- Principal: Bu parametre, politikanın hangi principal'a ilişkin olduğunu tanımlar. Principal yalnızca resource-based politikalar için kullanılır, örneğin S3 Bucket'larına eklenen politikalar. Identity-based politikaları kullanırken, politikanın kendisi principal ile ilişkili olduğu ve bir kaynakla ilişkili olmadığı için bu parametre politika içinde gerekli değildir. Principal parametresine alternatif olarak, bu **NotPrincipal** parametresi ile değiştirilebilir, bu da ilişkili kaynağa erişimine izin verilmeyen veya reddedilen kullanıcı, role veya AWS hesabını belirtir.

	- Action: Bu öğe, Effect parametresi için girilen değere bağlı olarak izin verilecek veya reddedilecek eylemdir. Eylemler etkili bir şekilde farklı hizmetler için API çağrılarıdır. Sonuç olarak, her hizmet için farklı eylemler kullanılır. Örneğin, `DeleteBucket` eylemi S3 için mevcuttur ancak EC2 için değildir ve benzer şekilde, `CreateKeyPair` eylemi EC2 için mevcuttur ancak S3 için değildir. Eylem, ilişkili AWS hizmetiyle önek alır. Bu örnek, CloudTrail hizmeti için iki eylemi, `CreateTrail` ve `DeleteTrail`'i tanımlar. Başka bir örnek olarak, yukarıdaki politika'da yıldız işaretiyle gösterilen bu örneği görebilirsiniz ve bu, CloudTrail hizmetinin tüm eylemlerini temsil eden bir joker karakter görevi görür, esasen hizmete tam erişim sağlar.

	- Principal parametresinde olduğu gibi, Action parametresini de **NotAction** ile değiştirebiliriz. Bu sayede, eşleşmemesi gereken sınırlı bir eylem kümesini listeleyerek daha kısa bir versiyon oluşturarak politikanızı optimize etmenize yardımcı olabilir.

	- Resource: Bu öğe, Action ve Effect'in uygulanmasını istediğiniz gerçek kaynağı belirtir. AWS, belirli kaynakları belirtmek için ARN'ler (Amazon Resource Names) olarak bilinen tanımlayıcılar kullanır. Genellikle ARN'ler şu sözdizimini izler: arn, partition, service, region, account-id ve ardından resource (**arn:partition:service:region:account-id:resource**). Partition öğesi, kaynağın bulunduğu bölümle ilgilidir. Standart AWS bölgeleri için bu AWS olacaktır. Service, bu belirli AWS hizmetini yansıtır, örneğin S3 veya EC2. Sonraki part, region yani kaynağın bulunduğu bölgedir. Bazı hizmetler belirtilen bir bölgeye ihtiyaç duymaz, bu nedenle bu bazen boş bırakılabilir. Sonra gelen alan ise; Account-ID değeridir; tire olmadan AWS hesap kimliğinizdir. Yine, bazı hizmetler bu bilgiye ihtiyaç duymaz ve bu nedenle boş bırakılabilir. Resource, bu alanın değeri kullandığınız AWS hizmetine bağlı olacaktır. Örneğin, `Action:s3:PutObject` eylemini kullanıyorsam, o zaman bu izinlerin uygulanmasını istediğim bucket adını kullanabilirim. Yine, belirtilen kaynaklar dışındaki tüm diğer kaynaklarla açıkça eşleşmek için kullanılabilen bir NotResource parametremiz var. Örneğin, bu politika NotResource parametresi tarafından belirtilenler dışındaki tüm S3 bucket'larına erişime izin verecektir.

		- Condition: Bu parametre, belirli kriterlere dayalı olarak izinlerin ne zaman etkili olacağını kontrol etmenize olanak tanıyan isteğe bağlı bir öğedir. Öğenin kendisi bir koşul ve bir anahtar değer çiftinden oluşur ve izinlerin aktif olması için koşulun tüm öğeleri karşılanmalıdır. Yukarıdaki örnek koşula bakalım. Örnekte, IP adresi, anahtar değer çiftinin etkili olacağı koşulun kendisidir. `aws:SourceIp` anahtardır ve 10.10.0.0/16 anahtar'ın değer öğesidir. Yani etkin olarak söylediği şey şudur: Eğer politika aracılığıyla erişim isteyen kullanıcının kaynak IP adresi 10.10.0.0/16 ağ aralığı içindeyse, o zaman politika ifadesindeki izinleri uygula.

Şimdi bir IAM politikasının temel parametrelerini gözden geçirdiğimize göre, politika içinde sunulan izinleri anlayabildiğimizden emin olmak için bir çift örneğe bakalım. Daha önce de belirttiğim gibi, bir statement içinde her biri farklı bir erişim düzeyi veren birden fazla Sid'e sahip olabilirsiniz. 


```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "StatementBlock1",
			"Effect": "Allow",
			"Action": "cloudtrail:*",
			"Resource": "*",
			"Condition": {
				"IpAddress": {
					"aws:SourceIp": "10.10.0.0/16"	
				}
			}
		},
		{
			"Sid": "StatementBlock2",
			"Effect": "Allow",
			"NotAction": "rds:DeleteDBInstance",
			"Resource": "*",
		},
		{
			"Sid": "StatementBlock3",
			"Effect": "Allow",
			"Action": ["s3:CreateBucket", "s3:DeleteBucket"],
			"Resource": "arn:aws:s3::cloudacademy"
		}
	]
}
```

Bu örnek politikaya bakarak, hangi erişimin verildiğini belirleyelim:

StatementBlock1: Bu, herhangi bir kaynağa, kaynak IP adresleri 10.10.0.0/16 ağ aralığı içinde olması koşuluyla CloudTrail'e tam erişim sağlar.

StatementBlock2: Bu, NotAction parametresi Action yerine kullanıldığı için `rds:DeleteDBInstance` dışındaki tüm API çağrılarını kullanarak tüm RDS veritabanlarına tam erişim sağlar.

StatementBlock3: Bu, S3'teki cloudacademy bucket'ı içinde S3 Bucket'larının oluşturulmasına ve silinmesine izin verir.

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid:" "S3AccessWithMFAEnabled",
			"Effect": "Allow",
			"Principal": {
				"AWS": ["arn:aws:iam::730739171055:user/Fatih"]
			}
			"Action": [
				"s3:CreateBucket",
				"s3:DeleteBucket",
				"s3:DeleteBucketPolicy",
				"s3:DeleteObject",
				"s3:PutOBject"
			],
			"Resource": "arn:aws:s3::fe-bucket-tr"
			"Condition": {
				"Bool": {"aws:MultiFactorAuthPresent": "True"}	
			}
		},
	]
}
```

Bu sefer resource-based bir politika ve bu durumda bir S3 bucket'ından örnek politikayı inceleyelim. Bu politikada, Principal parametresi tarafından vurgulanan Fatih adlı kullanıcıya, bucket oluşturma ve silme ve ayrıca bucket politikasını silme izni verilir. Fatih ayrıca bucket içindeki nesneleri silebilir, güncelleyebilir ve bu politikanın atıfta bulunduğu bucket, Resource parametresi tarafından tanımlanan fe-bucket-tr adlı bucket'tır. Ancak, bu politikaya bağlı bir koşul vardır ve bu, Fatih'in bunu yalnızca Multi-Factor Authentication (MFA) yoluyla kimlik doğrulaması yapılmışsa yapabileceğini belirtir. Bu, JSON IAM politikalarının nasıl çalıştığı ve neye benzediği konusunda size daha fazla anlayış sağlamalıdır.

### Politika Değerlendirme Mantığı (Policy Evaluation Logic)

AWS içindeki bir kaynağa erişmeye çalışan herkes için, istek bir dizi adımdan geçerek işlenir. Bu adımlardan biri, kullanılan politikalara dayalı olarak izin düzeyinin değerlendirilmesini içerir. Erişimin nasıl verildiğini veya reddedildiğini anlamak için tüm sürece bakalım. Basit bir dört adımlı süreçle başlayabiliriz:

1.  Authentication (Kimlik Doğrulama): Öncelikle, isteği gönderen principal'ın geçerli bir kullanıcı olarak kimliğinin doğrulandığından emin olmalıyız.
2.  Context (Bağlam): Principal'ın kimlik doğrulaması yapıldıktan sonra, AWS istenen isteğin bağlamını belirlemelidir, örneğin hangi hizmet veya eylemin istendiği gibi. Bu, isteğe dayalı olarak ilgili politikaların vurgulanmasını sağlar.
3.  Policy evaluation (Politika Değerlendirmesi): Bu, ilgilendiğimiz kısımdır. İsteğe bağlı olarak, erişim düzeyini belirlemek için incelenmesi gereken birden fazla politika türü olabilir.
4.  Result (Sonuç): AWS, kullanılan tüm politikaların değerlendirilmesine dayanarak erişime izin verilip verilmediğini belirleyecektir.

Bu başlık altında, üçüncü noktaya, yani politika değerlendirmesine ve bu sürecin nasıl yürütüldüğüne odaklanalım. Tek bir hesaptaki birden fazla politikadaki izinleri inceleme kuralları aslında oldukça basittir ve şöyle özetlenebilir:

-   Varsayılan olarak, bir kaynağa tüm erişim reddedilir.
-   Erişime yalnızca principal ile ilişkili bir politika içinde Allow belirtilmişse izin verilir.
-   Aynı principal ile ilişkili herhangi bir politikada aynı kaynağa karşı tek bir Deny varsa, o Deny aynı kaynak ve eylem için var olabilecek önceki herhangi bir Allow'u geçersiz kılar.
-   Yani tekrar etmek gerekirse, açık bir Deny her zaman bir Allow'dan öncelikli olacaktır.

Şimdi, politikaların değerlendirildiği bir sıra vardır ve aşağıdaki politika listesi değerlendirme sırasına göre gösterilmiştir:

1.  Organizational Service Control Policies (SCP'ler)
2.  Resource-based policies
3.  IAM permission boundaries
4.  Identity-based policies

## AWS Identity Federation

Temel olarak, iki farklı sağlayıcının, kullanıcıların birinden kimlik doğrulaması yapmasına ve diğerindeki kaynaklara erişmelerine izin veren bir güven düzeyi oluşturmasını sağlayan bir yöntemdir. Federation süreci sırasında, bir taraf Identity Provider (IdP) olarak hareket eder ve diğeri Service Provider (SP) olur. Identity provider kullanıcının kimliğini doğrular ve service provider, IdP'nin kimlik doğrulamasına dayalı olarak kendi hizmetine veya kaynaklarına erişimi kontrol eder.

Muhtemelen hepiniz bir giriş sayfası sunan web sitelerine gitmişsinizdir, burada ya o hizmet için yerel mevcut kimlik bilgilerinizi kullanarak giriş yapabilirsiniz ya da Facebook, Google, Twitter veya LinkedIn gibi 3. taraf bir sağlayıcının kimlik bilgilerini kullanarak kimlik doğrulaması yapma seçeneğiniz olabilir.

Örneğin, medium.com'un giriş sayfasını düşünelim. Google, Facebook, Apple, Twitter ve Gmail dahil olmak üzere kimlik sağlayıcılardan mevcut kimlik bilgilerini kullanarak yeni bir Medium hesabı oluşturabilir veya zaten bir hesap oluşturduysanız Medium'a tekrar giriş yapabilirsiniz. Bu senaryoda, Medium Service Provider olacaktır.

Dolayısıyla, Medium'da henüz bir hesabınız yoksa, bir tane oluşturmak için bu sağlayıcılardan birini seçebilirsiniz. Örneğin, halihazırda bir Google hesabınız varsa ve bununla giriş yapmışsanız ve 'Google ile Giriş Yap'ı seçebilirsiniz.

Eğer seçtiğiniz Google hesabı ile bir medium.com hesabınız yok ise; burada Medium, mevcut Google hesabımla ilişkili bir Medium hesabımın olmadığını tespit eder, "Üzgünüz, bu hesabı tanımadık" yazdığını göreceksiniz ve bu nedenle bir tane oluşturmak isteyip istemediğiniz sorulur. Google ile Kaydol'u seçerek, Medium Google kimlik bilgilerim aracılığıyla erişimimi doğrular ve böylece Google IdP'ye güvenerek, bu kimlik doğrulamasına dayalı olarak yeni bir Medium hesabı oluşturmamı sağlar.

Bu örnekte, Google, sizi yeni bir hesapla Medium'a doğrulamak için Identity Provider olarak kullanıldı. Bir sonraki Medium'a giriş yapmam gerektiğinde, tek yapmam gereken 'Google ile Giriş Yap'ı seçmek olacak ve Google hesabınızla zaten giriş yapmış olduğum sürece, 1 tıklamayla doğrudan Medium'a giriş yapacaksınız ve bu doğrudan 'Single-Sign on' veya SSO terimiyle ilişkilidir. SSO, bir sisteme giriş yapma yöntemiyle ilgilidir, bu da ek kimlik bilgileri sağlamanıza gerek kalmadan başka bir sisteme kimlik doğrulamanız için kullanılabilir.

Bu sürecin arkasındaki işlem oldukça basittir, identity provider seçildiğinde ve kullanıcı o sağlayıcı tarafından doğrulandığında, service provider'a, bu durumda Medium'a bir assertion gönderilir. Bu assertion, kullanıcının kullanıcı adı gibi metadata ve öznitelikleri içeren bir mesajdır. Bu assertion, service provider'ın hizmetlerine erişimi yetkilendirmesine olanak tanır.

Dolayısıyla identity federation, kullanıcılar ve hizmet sağlayıcılar için esneklik ve kolaylık sağlayan erişim kontrol sistemlerini kolayca kurmanın harika bir yolunu sunar.

AWS, aynı ilkeyi kullanarak, AWS hizmetlerinize federated access yoluyla erişmenizi sağlayan çeşitli hizmetler ve yöntemler sunmaktadır. Bu, Identity Provider olarak kullanılabilecek başka bir yerde yönetilen bir kullanıcı dizininiz varsa, AWS'de özel bir Identity & Access Management kullanıcısı yapılandırmanıza gerek olmadığı anlamına gelir.

OAuth 2.0, OIDC (OpenID Connect) ve SAML 2.0 (Security Assertion Markup Language) dahil olmak üzere birçok farklı kimlik standardını kullanan AWS, yükleniciler ve diğer 3. taraflar için erişimi kolayca yapılandırmanızın yanı sıra, mobil ve web uygulamalarınıza kimlik doğrulaması için ölçeklenebilir erişim sağlamanıza olanak tanır.

Bu standartların Wikipedia'ya göre tanımlarına hızlıca bakalım:

"OAuth, erişim delegasyonu için açık bir standarttır ve genellikle İnternet kullanıcılarının web sitelerine veya uygulamalara diğer web sitelerindeki bilgilerine erişim izni vermesinin bir yolu olarak kullanılır, ancak onlara şifreleri vermeden." - Wikipedia

"OpenID Connect, OAuth 2.0 protokolünün üzerinde basit bir kimlik katmanıdır ve bilgisayar istemcilerinin bir yetkilendirme sunucusu tarafından gerçekleştirilen kimlik doğrulamaya dayalı olarak bir son kullanıcının kimliğini doğrulamasına ve son kullanıcı hakkında temel profil bilgilerini elde etmesine olanak tanır." - Wikipedia

"Security Assertion Markup Language 2.0 (SAML 2.0), güvenlik alanları arasında kimlik doğrulama ve yetkilendirme kimliklerini değiştirmek için bir standarttır. SAML 2.0, bir SAML yetkilisi (Identity Provider olarak adlandırılır) ile bir SAML tüketicisi (Service Provider olarak adlandırılır) arasında bir principal (genellikle bir son kullanıcı) hakkında bilgi geçirmek için assertionlar içeren güvenlik belirteçleri kullanan XML tabanlı bir protokoldür." - Wikipedia

Şimdi federation hakkında daha fazla bilgi sahibi olduğumuza göre, AWS tarafından sunulan bazı hizmetlere ve bunların AWS Identity Federation'a nasıl uyduğuna yüksek düzeyde bakmak istiyorum, bunlar arasında:

- AWS Single Sign-On (SSO olarak bilinir)
- AWS Identity & Access Management (IAM)
- Amazon Cognito


Bu hizmet öncelikle kullanıcıların AWS Organization'larındaki birden fazla hesaba kolayca erişmelerini sağlamak için tasarlanmıştır ve her hesap için kimlik bilgileri sağlama ihtiyacını ortadan kaldıran tek bir oturum açma yaklaşımı sağlar. AWS Organizations'a aşina olmayanlar için, bu hizmet, sahip olduğunuz birden fazla AWS hesabını merkezi olarak yönetmenizi ve kategorize etmenizi sağlar, onları tek bir organizasyonda bir araya getirerek AWS ortamınızı güvenlik, uyumluluk ve hesap yönetimi açısından korumanıza yardımcı olur.

AWS SSO'nun kendi yerleşik kullanıcı dizinini kullanabilirsiniz, bu size e-posta adreslerine dayalı benzersiz kullanıcılar oluşturma imkanı verir, veya alternatif olarak AWS SSO'yu desteklenen kimlik sağlayıcılardan birine bağlayabilirsiniz, örneğin kendi kurumsal MS Active Directory'nizi kullanıyorsanız.

Her iki durumda da, AWS SSO farklı gruplar oluşturmanızı sağlarken IAM rollerinin ve izinlerinin gücünden yararlanarak, kullanıcıların veya grupların organizasyonunuz içindeki belirli AWS Hesaplarında hangi erişime sahip olduğunu kontrol etmenizi sağlar.

Kullanıcılar yapılandırıldığında, erişimleri olan tüm AWS hesaplarını, benimseme erişimine sahip oldukları rolleri ve Yönetim konsoluna erişmek mi yoksa AWS CLI erişimi için geçici kimlik bilgileri kullanmak mı istediklerini listeleyecek kendi portalları üzerinden bağlanabilirler.

AWS SSO ayrıca Office 365 ve Salesforce gibi SAML 2.0 kullanan bulut tabanlı uygulamalara erişimi yönetmek için de kullanılabilir. Daha önce de belirttiğimiz gibi, Identity federation, IAM'de bir kullanıcı hesabınız olmasa bile AWS kaynaklarına erişmenizi ve bunları yönetmenizi sağlar.

AWS SSO, yerleşik kullanıcı dizinini veya MS-AD'yi kullanarak AWS organizasyonunuzdaki birden fazla hesaba tek oturum açma yaklaşımı oluşturmanıza olanak tanırken, AWS IAM, her AWS hesabı için farklı kimlik sağlayıcıları kullanarak AWS hesaplarınıza ve kaynaklarınıza federated access yapılandırmanıza olanak tanır. Örnekler arasında MS-AD kullanarak kurumsal federation için SAML 2.0 ve Google, PayPal ve Amazon gibi web kimliği federation olarak sınıflandırılan OpenID Connect bulunur. Sonuç olarak, AWS IAM'de yeni bir kimlikle kullanıcılar oluşturmak yerine bu Identity Provider'ları kullanarak ortamınıza erişime izin verebilirsiniz.

Bunun faydası iki yönlüdür. IAM içinde gerekli yönetim miktarını en aza indirir ve tek oturum açma çözümüne olanak tanır.

Federated authentication'ı uygulamak için yapılandırma sürecinin bir parçası olarak, kimlik sağlayıcı ile AWS hesabınız arasında bir güven ilişkisi kurulur. AWS iki tür kimlik sağlayıcıyı destekler: OpenID Connect (genellikle web kimliği federation olarak da adlandırılır) ve SAML 2.0.

Amazon Cognito,  web veya mobil uygulamalarınıza erişen yeni ve mevcut kullanıcılar için güvenli kimlik doğrulama ve erişim kontrolünü etkinleştirmeyi basitleştirmek için özel olarak oluşturulmuştur. Sadece SAML 2.0 ile iyi entegre olmakla kalmaz, aynı zamanda web kimliği federation ile de iyi entegre olur. Amazon Cognito'nun en büyük özelliklerinden biri, mobil uygulamalarla çalışırken harika olan milyonlarca yeni kullanıcıya ölçeklenebilme yeteneğine sahip olmasıdır.

Cognito ayrıca kişiselleştirilmiş bir oturum açma sayfası eklemenize, markalama ve kendi logonuzu eklemenize olanak tanıyan özel bir portal kullanmanıza izin verir.

Amazon Cognito'nun 'User Pools' ve 'Identity Pools' olmak üzere 2 ana bileşeni vardır ve bunlar farklı işlemler gerçekleştirir.

User Pools esasen ölçeklenebilir bir kullanıcı dizinidir ve yeni kullanıcıların kaydolmasına veya mevcut kullanıcıların mobil uygulamanıza user pool'dan yerel kimlik bilgilerini kullanarak giriş yapmasına olanak tanır. Bununla beaber, alternatif olarak bir web veya kurumsal IdP aracılığıyla erişimlerini federe edebilirler.

Identity Pools, user pool'lardan farklıdır çünkü aslında size web veya mobil uygulamanız tarafından çağrılan AWS kaynaklarına geçici kimlik bilgileri ve Security token service kullanarak erişme yeteneği sağlar.

Şimdi AWS'ye federated access uygulamak için bu 3 AWS seçeneği arasındaki temel farkı özetleyeyim:

- AWS SSO, tümü için tek bir kimlik sağlayıcı kullanarak bir AWS Organization içindeki birden fazla AWS hesabına erişmek için tek oturum açma yaklaşımı oluşturmanıza olanak tanır.
- AWS IAM, AWS hesaplarınızın her biri için farklı OpenID veya SAML kimlik sağlayıcıları yapılandırmanıza olanak tanır.
- Amazon Cognito, hem SAML 2.0 hem de web kimliği federation kullanarak web veya mobil uygulamalarınıza güvenli kimlik doğrulama sağlar.

## AWS Cognito

Amazon Cognito, mobil veya web uygulamaları oluşturmanın en can sıkıcı kısımlarından biri kullanıcı kimlik doğrulamasıyla uğraşmaktır. Uygulamada belirli servisleri veya özellikleri kimin kullanıp kimin kullanamayacağını belirlemek son derece önemlidir. Ancak, bu işlemi kurmak zahmetli ve zaman alıcı olabilir. Geçmişte, tüm bu önemli kullanıcı bilgileri sıradan bir user database'inde saklanırdı. Bu database in-promise veya cloud'da barındırılabilirdi. Her iki durumda da, muhtemelen kullanıcı adlarını ve ilişkili izinleri tutan relational bir database olurdu. AWS dünyasında çalışırken, bu database'in ya RDS (Relational Database Service) üzerinde barındırılması ya da kendi database'inizi EC2 instance'larında çalıştırmanız anlamına gelirdi. Bu yöntemlerin herhangi birini kullanmanın zorluğu, her şeyi kurmak ve sistemi sürdürmek için çok fazla çalışma gerektirmesidir.

Ayrıca, kurumsal ortamda çalışan insanların tanıdık senaryosuna da sahibiz. Tüm bilgileri zaten Microsoft Active Directory gibi bir directory service'te saklanmıştır ve yeni özel uygulamamız için bir kez daha login ve password oluşturmalarını istemeyiz. Günlük kurumsal username ve password'leri ile giriş yapabilmelerini tercih ederiz.

İşte tüm bu sıkıntılı noktalar, oldukça küçük ama çok iş yapabilen bir servis olan Amazon Cognito kullanılarak çözülebilir.

Özünde, Amazon Cognito bir authentication ve user management servisidir. Apple, Facebook, Google ve Amazon gibi üçüncü taraf identity provider'larla güçlü bir entegrasyona sahiptir. Ayrıca, kendi Active Directory kullanıcılarınızın kendi harici web ve mobil uygulamalarınıza erişebilmesi için kendi Active Directory servislerinizden identity'leri federe etmenize olanak tanır.

Amazon Cognito'nun özellikleri kolayca iki farklı konuya ayrılabilir: **Amazon Cognito User Pools** ve **Amazon Cognito Identity Pools**. Servis ile yapabileceğiniz diğer her şey bu iki şeyden türetilmiştir.

### User Pools

Amazon Cognito User Pools'un temel amacı, mobil veya web uygulamalarınız için bir user directory oluşturmak ve yönetmektir. Bu yetenek, yeni ve mevcut kullanıcılarınızın hem sign up hem de sign in işlemlerini yönetmek anlamına gelir. Yeni kullanıcıları kaydederken, Cognito sizin ve uygulamanız için önemli olan şeyleri özelleştirmenize olanak tanır. Potansiyel kullanıcılarınızın sign up sırasında sunabileceği çok sayıda bilgi vardır.

Yeni kullanıcılarınızın e-postalarını, adreslerini, fotoğraflarını veya istediğiniz herhangi bir bilgiyi sunmalarını istiyorsanız, user pool'unuzu oluştururken tüm bunları ayarlayabilirsiniz. Ayrıca, kullanıcılarınızdan belirli bir şeye ihtiyacınız varsa custom attribute'ler de oluşturabilirsiniz. Custom attribute bir string veya number olabilir ve kabul edeceğiniz minimum ve maksimum değerleri belirlemenize olanak tanır.

Bu bilgilerin tümü Cognito User Pool içinde saklanır ve uygulamanız tarafından ihtiyaç duyulduğunda erişilebilir. Kullanıcıların oluşturabileceği password'lerin ne kadar katı olmasını istediğinizi de belirleyebilirsiniz. Cognito, minimum uzunluk, sayı, özel karakter, büyük ve küçük harf gerektirme gibi tüm normal password işlevselliğini sunar.

Amazon Cognito ayrıca multi-factor authentication (MFA) gerektirme işlevselliğine de sahiptir. Bu özelliği, herhangi bir finansal hizmet veya tıbbi, kredi kartı bilgileri gibi yüksek değerli bilgiler için veya kullanıcının önemli miktarda para yatırmış olabileceği in-app satın almalar içeren herhangi bir uygulama için önerilir.

Servis, kendi başınıza kurmanın oldukça can sıkıcı olabileceği ve normalde sizin için başka bir backend servisinin ele almasını gerektiren account recovery özelliklerini de içerir. Ayrıca bu özelliğe, e-posta ve telefon dahild,r. Ek olarak, kullanıcılarınızın tüm bu zahmetlerden geçmesini istemiyorsanız, Cognito User Pools size social sign-in imkanı sunar. Bu, kullanıcılarınızın uygulamanız için üçüncü taraf ID provider'ları kullanarak da giriş yapabilecekleri anlamına gelir.

Bu yol, (uygulama geliştiricisi olarak) sizin önce bu harici üçüncü taraf provider'larla bir developer account oluşturmanızı ve uygulamanızı onlarla ayarlamanızı gerektirir. Özellikle zor bir görev değildir, ancak zaman alıcı olabilir.

Son olarak, herhangi bir SAML (Security Assertion Markup Language) identity provider ile de giriş yapabilirsiniz. SAML'den haberdar değilseniz, security assertion'lar için bir XML tabanlı markup language'dir. Web üzerinden single sign-on için önemli bir araçtır. Örneğin, SAML ID provider'ınız bir active directory federation service olabilir. Bu provider, yerinde AD'niz veya bir EC2 sunucusunda barındırdığınız bir AD olabilir. Bu yolu kullanırsanız, sahip olduğunuz bir domain name'e ihtiyacınız olacağını lütfen unutmayın.

Servis ayrıca Sign in ve sign up hizmetlerini yönetmek için kendi özelleştirilebilir web UI'ınızı oluşturmanın bir yolunu da sunar. Bu özelleştirilebilir UI'ı kullanmak, size OAuth 2.0 uyumlu bir authorization server sağlar. OAuth, sunucuların veya servislerin birbirlerine SSO kimlik bilgilerini paylaşmadan nasıl güvenli bir şekilde authenticated erişim sağlayabileceklerini düzenleyen açık standart bir authorization protokolüdür. Sağlanan web UI'ın user experience'ı özelleştirilebilir ve kendi marka logolarınızı eklemenize ve web sayfasının görünümünü değiştirmenize olanak tanır. Bunu kullanmak zorunda değilsiniz ve kendi UI'ınızı oluşturabilirsiniz. Sadece servis için uygun API çağrılarını yapmaktan ve kendi OAuth sunucunuzu çalıştırmaktan sorumlu olursunuz ki bu bazı insanlar için zor olabilir ve ulaşmaya çalıştığınız hedefin kapsamı dışında olabilir.

User pool'ları ayrıca AWS Lambda ile entegrasyonlara sahiptir ve user flow'una dayalı olarak fonksiyonları tetikleme seçeneği sunar. Örneğin, bir kullanıcı başarıyla kaydolduktan hemen sonra, bir e-posta göndermek veya o kullanıcı için bazı backend işlevselliği oluşturmak için bir lambda fonksiyonunun tetiklenmesini istiyorsanız, bu yeteneğe sahipsiniz. Veya bir kullanıcı başarıyla giriş yaptığında, lambda'nın o kullanıcı hakkında bazı backend bilgilerini kontrol etmesini ve ortamlarını buna göre hazırlamasını sağlayabilirsiniz.

Diğer bir konu ise eğer bu bilgiler zaten mevcutsa, bir CSV dosyası aracılığıyla tüm kullanıcı ve hesap listesini de ekleyebilirsiniz. Bence bu servis hakkında dikkat edilmesi gereken en önemli şey, tüm ekstra engelleri ortadan kaldırmaya ve sadece uygulamanızı geliştirmeye odaklanmanıza olanak sağlamaya çalışmasıdır. Modern uygulamalarla basit sign ve authentication işlemlerinin bile çalışması için gereken birçok adım vardır, bu yüzden tüm bu ekstra şeylerin sizin için halledilmesi oldukça güçlü bir özelliktir.

Şimdi User Pool'lar hakkında biraz daha bilgi sahibi olduğumuza göre, authentication flow'unun uygulamanız ve servis tarafından nasıl ele alındığını hızlıca inceleyelim.

Kullanıcınıza, uygulamanızın içinden bir login ekranı veya terminal sunulacaktır. Doğrudan bir hesap oluşturarak veya üçüncü taraf bir provider'a giriş yaparak kimlik bilgilerini göndereceklerdir. Uygulama, bu kimlik bilgileriyle **InitiateAuth** operasyonunu çağıracaktır. Bu API çağrısı, authentication flow'unu başlatır. Amazon Cognito'ya doğrudan authenticate etmek istediğinizi belirtir.

Çağrı başarılı olursa, Cognito ya bir token ile ya da bir challenge ile yanıt verecektir. Challenge, CAPTCHA'ları veya Dinamik challenge sorularını içerebilir. Bunlar genellikle bot'ları taramaya yardımcı olmak için kullanılır. İsterseniz kendi özel challenge'larınızı ekleyebilirsiniz. Bu, client'a geri gönderilecek ve artık _onların problemi_ haline gelecektir.

Client server'a (Cognito'ya) yanıt vermeye hazır olduğunda, **RespondToAuthChallenge** ile yanıt verebilir ve challenge'ın gerektirdiği bilgileri geri gönderebilir. Kullanıcı challenge'da başarısız olursa, Cognito'yu yeni bir challenge göndermek üzere ayarlayabilirsiniz. Bu, kullanıcı başarılı olana veya başarısız olana kadar birden fazla tur içerebilir.

Başarılı olursa, Cognito client'ın kullanması için bazı token'lar gönderecektir. Bu aşamadan sonra, authentication başarılı bir şekilde tamamlanmış olur.

### Identity Pools

Amazon Cognito Identity Pool'ları (aynı zamanda Federated Identity'ler olarak da bilinir) kullanıcılarınız veya AWS servislerine erişim ihtiyacı olan misafirleriniz için geçici AWS Credential'ları sağlamaya yardımcı olur.

Identity Pool'lar, Amazon Cognito User Pool'larla birlikte çalışarak, kullanıcılarınızın AWS'den ihtiyaç duydukları belirli özelliklere erişmesine ve bunları kullanmasına olanak tanır. Ayrıca, User Pool'larda olduğu gibi, Amazon, Facebook ve Google gibi public provider'larla da federe olabilirsiniz.

Identity Pool'lar iki tür identity tanımlamaya yardımcı olur: Authenticated identity'ler ve unauthenticated identity'ler. Identity Pool'unuzdaki her identity bu iki durumdan birinde olmalıdır.

Authenticated durumuna ulaşmak için, bir kullanıcının public bir login provider tarafından authenticate edilmesi gerekir. Bu, daha önceki Amazon Cognito User Pool'unuz olabilir veya Amazon, Apple, Facebook, Google, SAML ve hatta bir Open ID connect provider gibi diğer public ID provider'lardan herhangi biri olabilir.

Unauthenticated identity'lere de sahip olabilirsiniz. Bu, bir dizi nedenden dolayı faydalı olabilir, ancak temel nedenler, kullanıcıların tamamen giriş yapmadan önce çeşitli AWS kaynaklarını görmelerine izin vermek olabilir. Örneğin, dashboard'lara bir miktar görünürlük sağlayarak, böylece bir bakışta bir şeylerin yanlış olup olmadığını görebilirler.

Unauthenticated identity'leri, insanların bazı temel servislere erişmesini istediğinizde ve daha sonra onları sign in veya sign up yapmaya teşvik ettiğinizde bir tür misafir girişi olarak da kullanabilirsiniz.

Her identity türü, beraberinde gelen bir role'e sahiptir. Role'lere, kullanıcının AWS içinde yapmasına izin verilen şeylerin izinlerini belirleyen policy'ler eklidir. Role'ler, sınırları tanımlamaya yardımcı olur ve authenticated veya unauthorized bir kullanıcının neyi değiştirebileceğini veya görebileceğini açıkça belirtmenize olanak tanır.


Identity Pool'lar ve User Pool'lar arasındaki farkı düşünürken aklınızda tutmanız gereken en önemli şey, Identity Pool'ların authentication ve access control için kullanıldığıdır (özellikle AWS servisleri için). User Pool'lar ise sign-up ve sign-in türü işlemler için tasarlanmıştır.

Pekala, şimdi Identity Pool'larla authentication'ın nasıl çalıştığına bir göz atalım.

İlk kısma zaten aşina olmalıyız. Uygulamanızın, kullanıcının Cognito User Pool'ları ile giriş yapmasını sağlaması gerekir ve bu, User Pool'un kendisini, bir social sign-in'i, SAML destekli authentication servisinizi veya bu türden bir şeyi kullanarak gerçekleşebilir.

Bu aşamada, IDP tarafından Cognito User Pool'a bir Identity Token gönderilir. Şimdi Cognito, IDP tarafından sağlanan credential'ları saklamaz veya bunu mobil uygulamanıza iletmez, bunun yerine IDP token'ı standart bir token'a, Cognito User Pool Token veya CUP token olarak adlandırılan bir token'a normalize edilecek ve bu Cognito tarafından kullanılacak ve saklanacaktır. Bu, esasen kullanıcının User Pool'daki bir hesap üzerinden mi yoksa federated access üzerinden mi authenticate olduğunun önemli olmadığı anlamına gelir, uygulamanıza geri gönderilen tüm token'lar standartlaştırılacaktır.

Kullanıcının uygulamanızda gerçekleştirdiği işlemlerin ayrıca oluşturduğunuz backend servislerine veya API'lere erişmesi gerekebilir, örneğin bu CUP token'larını kabul eden API Gateway veya Lambda kullanıyor olabilirsiniz, bu nedenle Cognito, API Gateway vb. ile bu API'leri kullanmanız için sizi authenticate ve authorize etmek için aynı CUP token'ı kullanacaktır.

Şimdi bazı servisler, authentication için CUP token'larını kullanmanıza izin vermez. Mobil uygulamanızın kullanıcı adına S3 veya DynamoDB gibi servislere erişmesi gerekiyorsa, o zaman authenticate olmak için Identity Pool'u kullanmanız gerekecektir.

CUP Token, Identity Pool'a gönderilebilir, burada CUP token'ınıza dayalı olarak bir STS Token (Security Token Service) oluşturulacak ve bu uygulamanıza geri gönderilecektir. Bu AWS credential'ları ile uygulamanızın artık diğer AWS servislerini çağırmasına izin verilecektir. Bu credential'lar, Identity Pool içindeki kullanıcılarınızla ilişkilendirdiğiniz bir AWS role'üne bağlı olacaktır.

### Anazon Cognito Sync

Amazon Cognito, web ve mobil uygulama geliştiricilerinin sıklıkla karşılaştığı bir soruya çözüm sunuyor: uygulama kullanıcı verilerini çeşitli platformlar arasında nasıl senkronize edebiliriz?

Bu veriler profil bilgileri, uygulama durumu, daha önce görüntülenen içerik, konum takibi ve benzeri öğeleri içerebilir. Bu tür kullanıcı deneyimini artıran özellikler, kullanıcıları memnun etmek için önemlidir. Kullanıcıların cihaz değiştirirken kaldıkları yerden devam etmelerini sağlar.

Amazon Cognito Sync, bu veri noktalarını sizin için yönetebilir. Böylece kendinizin oluşturması, bakımını yapması ve yönetmesi gereken bir backend'e ihtiyaç duymazsınız. Sync özelliği ayrıca, cihazın internet erişimi olmadığı durumlar için bu verileri yerel olarak önbelleğe almanıza olanak tanır. Cihaz tekrar çevrimiçi olduğunda, sunucu ile yeniden senkronizasyon yapabilir.

Tüm veri noktaları bir dataset içinde saklanır ve key-value pair sistemi ile kaydedilip alınır. Her dataset maksimum 1MB boyutunda olabilir (çok sayıda key-value pair için yeterlidir), roman yazmaya çalışmadığınız sürece.

Bu dataset'ler, en fazla 20 dataset'e sahip olabilen bir Cognito identity ile ilişkilendirilir. Bilgileri Cognito'ya geri senkronize ederken, işlem yapabileceğiniz en küçük veri birimi tüm bir dataset'tir. Tüm genel okuma ve yazma işlemleri yerel olarak gerçekleştirilir ve bilgileri senkronize etmeye karar verdiğinizde, tüm dataset bir kerede senkronize edilir.

Pratikte, dataset tıpkı bir dictionary gibi çalışır. Basit bir key-value sistemidir. Bu key'ler normal bir dictionary gibi okunabilir, eklenebilir veya değiştirilebilir. Aşağıda bir JavaScript senkronizasyon kodu örneği görebilirsiniz.

```js
dataset.get('myKey', function(err, value) {
	console.log('myRecord: ', value);
});

dataset.put('newKey','newValue', function(err, record) {
	console.log(record);
});

dataset.put('oldKey', function(err, record) {
	console.log(record);
});

```

Tabii ki, uygulamanızın hataları ve başarılı işlemleri uygun şekilde ele alabilmesi için servisten gelen callback'leri yönetmek istersiniz. Aşağıda callback seçenekleriyle aynı senkronizasyon fonksiyonunu görebilirsiniz.

```js
dataset.synchronize({
	onSuccess: function(dataset, newRecords) {
		// ...
	},
	onFailure: function(err){
		// ...
	},
	onConflict: function(dataset, conflicts, callback){
		// ...
	},
	onDatasetDeleted: function(dataset, datasetName, callback){
		// ...
	},
	onDatasetMerged: function(dataset, datasetNames, callback){
		// ...
	},
})
```

Bu noktada, Cognito sync hakkında bir uyarı yapmakta fayda var. AWS şu anda bu işlevselliği AWS AppSync'e taşımaya çalışıyor. AppSync, Cognito versiyonunun tüm özelliklerini barındırıyor, ancak ek olarak birden fazla kullanıcının aynı verilere erişmesine olanak tanıyor. Bu, kullanıcılara paylaşılan veriler üzerinde gerçek zamanlı senkronizasyon ve işbirliği yapma gücü veriyor. Bu yüzden, bu özellikleri geliştirmeye karar verdiğinizde her iki servis seçeneğini de incelediğinizden emin olun.

## Şifreleme Nedir? (What is Encryption?)

AWS Key Management Service'in kendisine ve AWS'de verilerinizi nasıl şifreleyebileceğine derinlemesine bakmadan önce, şifreleme konusunda yeni olanlar için KMS tarafından kullanılan şifreleme yöntemlerinin temellerini anlamanın faydalı olacaktır.

Şifrelenmemiş veri, veri at rest (durağan) veya iki veya daha fazla konum arasında transit halindeyken erişimi olan herkes tarafından okunabilen ve görülebilen veridir. Bu şifrelenmemiş veri, genellikle 'plain text' veya 'clear text' veri olarak bilinir çünkü veri açıkça görülebilir ve herhangi bir alıcı tarafından okunabilir. Veri hassas veya gizli bilgiler içermediği ve kısıtlanması gerekmediği sürece şifrelenmemiş olmasında bir sorun yoktur. Ancak, müşteri verileri veya finansal kayıtlar gibi hassas bilgiler içeren verileriniz varsa, o dosyanın içeriğinin yalnızca yetkili kişiler tarafından görüntülenebilir olmasını sağlamanız gerekir. Nesne çevresindeki veri güvenliğini artırmak için o veriye bir şifreleme seviyesi eklemelisiniz.

Veri şifreleme, bilginin matematiksel algoritmalar ve şifreleme anahtarları kullanılarak değiştirildiği ve plain text verileri okunamaz hale getiren mekanizmadır. Şifrelendiğinde, orijinal plain text artık okunamaz olan cipher text olarak bilinir. Veriyi deşifre etmek için, cipher text'i tekrar okunabilir plain text formatına döndürmek için bir şifreleme anahtarı gereklidir.

Şifreleme anahtarı, basitçe bir şifreleme algoritması ile birlikte kullanılan bir karakter dizisidir ve anahtar ne kadar uzunsa şifreleme o kadar güçlü olur. Anahtarları içeren şifreleme yöntemleri, simetrik kriptografi veya asimetrik kriptografi olarak kategorize edilebilir. AWS KMS bu yöntemlerin her ikisini de kullanır.

Gelin bu yöntemlerin nasıl farklılaştığına bir göz atalım.

**Simetrik şifrelemede**, veriyi hem şifrelemek hem de deşifre etmek için tek bir anahtar kullanılır. Örneğin, birisi simetrik şifreleme yöntemi kullanıyorsa, veriyi bir anahtarla şifreler ve daha sonra o veriye erişmesi gerektiğinde, veriyi şifrelemek için kullandığı aynı anahtarı kullanarak veriyi deşifre eder. Bu, şifrelenmiş veri farklı bir alıcı tarafından okunuyorsa, o alıcıya aynı anahtarın verilmesi gerektiği anlamına gelir. Unutmayın, veriyi deşifre etmek için şifrelemek için kullanılan aynı anahtar gereklidir. Sonuç olarak, bu anahtar alıcılar arasında güvenli bir şekilde gönderilmelidir ve burada bu yöntemde potansiyel bir zayıflık ortaya çıkar. Eğer anahtar, transit halinde şifreleme yöntemi kullanılmadan gönderilmiş ve bu iletim sırasında herhangi biri tarafından ele geçirilirse, o üçüncü taraf o anahtarla ilişkili herhangi bir veriyi kolayca deşifre edebilir. AWS KMS, merkezi bir depo görevi görerek, gerekli anahtarları yöneterek ve depolayarak ve yalnızca yeterli izinlere sahip olanlara deşifreleme anahtarlarını vererek bu sorunu çözmeye yardımcı olur.

Kullanılan bazı yaygın simetrik kriptografi algoritmaları AES (Advanced Encryption Standard), DES (Digital Encryption Standard), Triple DES ve Blowfish'tir.

Şimdi bu şifreleme türünü, biri veriyi şifrelemek ve ayrı bir anahtar veriyi deşifre etmek için kullanılan iki ayrı anahtarı içeren asimetrik şifreleme ile karşılaştıralım. Bu anahtarlar aynı anda oluşturulur ve matematiksel bir algoritma aracılığıyla bağlantılıdır. Bir anahtar private anahtar olarak kabul edilir ve tek bir taraf tarafından tutulmalı ve asla başkalarıyla paylaşılmamalıdır. Diğer anahtar public anahtar olarak kabul edilir ve bu anahtar herhangi biriyle verilebilir ve paylaşılabilir.

Simetrik şifrelemenin aksine, bu genel anahtarın güvenli bir iletim üzerinden gönderilmesi gerekmez. Bu genel anahtara kimin erişimi olduğu önemli değildir çünkü özel anahtar olmadan, onunla şifrelenmiş herhangi bir veriye erişilemez. Peki nasıl çalışır?

Başka bir taraf size şifrelenmiş bir mesaj veya veri göndermek isterse, serbestçe erişilebilir olan kendi genel anahtarınızı kullanarak mesajı şifreler. Mesaj size gönderilir ve siz genel anahtarınızla matematiksel ilişkiye sahip olan kendi özel anahtarınızı kullanarak veriyi deşifre edersiniz. Bu, özel anahtarınızı açığa çıkarma riski olmadan şifrelenmiş veri almanıza olanak tanır ve simetrik şifrelemede vurgulanan sorunu çözer.

Simetrik şifrelemenin asimetrik üzerindeki avantajı, şifreleme ve deşifre etme hızıdır. Simetrik, performans açısından çok daha hızlıdır. Ancak, belirtildiği gibi ek bir risk taşır. Asimetrik kriptografi algoritmalarının bazı yaygın örnekleri RSA, Diffie-Hellman ve Digital Signature Algorithm'dır.

## AWS KMS

Key Management Service (KMS), çoğu AWS servisinin veri şifreleme işlemlerinde kullandığı şifreleme anahtarlarını depolamak ve oluşturmak için kullanılan yönetilen bir servistir. Örneğin, Amazon S3'ü KMS tarafından üretilen anahtarları kullanarak nesnelerinize karşı veri şifrelemesi gerçekleştirmek üzere yapılandırabilirsiniz, bu KMS kullanan Sunucu Taraflı Şifreleme (SSE-KMS) olarak bilinir. Ya da RDS veritabanlarınızda depolanan verilerinizi şifrelemek için KMS'i kullanabilirsiniz. Esasen, şifreleme yetenekleri sunan herhangi bir servis, bu şifrelemeyi gerçekleştirmek için büyük olasılıkla KMS ile arayüz oluşturur.

Bu servisin doğası gereği, kriptografik işlemleri gerçekleştirmek için kullanılan KMS anahtarları son derece güvenli kalmalıdır. Sonuç olarak, AWS yöneticileri ve çalışanlarının KMS içindeki anahtarlarınıza erişimi olmadığını ve onları silmeniz durumunda anahtarlarınızı sizin için kurtaramayacaklarını bilmelisiniz. AWS'nin sorumluluğu, sadece KMS'nin üzerinde çalıştığı altta yatan işletim sistemlerini ve donanım güvenlik modüllerini (HSM'ler) yönetmektir.

AWS'nin anahtarlarınıza erişimi olmadığından, KMS servisinin müşterileri ve kullanıcıları olarak kendi şifreleme anahtarlarımızı yönetmek ve bu anahtarların kendi ortamımızda korumak istediğimiz verilere karşı nasıl dağıtıldığını ve kullanıldığını kısıtlamak bizim sorumluluğumuzdur.

KMS'nin kendisi, organizasyonunuzda başarılı ve etkili bir şifreleme stratejisi uygulamak istiyorsanız aşina olmanız gereken bir dizi temel bileşenden oluşur. Bu bileşenlerin özü, mevcut olan farklı anahtarlardır. İşte bunlardan bazılarına hızlı bir genel bakış:

-   **AWS KMS Anahtarları (AWS KMS Keys):** Bunlar veriyi şifrelemek ve deşifre etmek için kullanılır, ayrıca KMS dışında kullanılabilecek anahtarlar üretmek için de kullanılır.
-  ** Müşteri Yönetimli Anahtarlar (Customer Managed Keys):** Bunlar müşteriler olarak bizim tarafımızdan oluşturulur ve bu anahtarlar ve izinleri üzerinde tam kontrole sahibiz.
-   **AWS Yönetimli Anahtarlar (AWS Managed Keys):** Bu anahtarlar diğer AWS servisleri tarafından oluşturulur ve ilgili entegre AWS servisi tarafından yönetilir.
-   **AWS'ye Ait Anahtarlar (AWS Owned Keys):** Bu anahtarlar AWS tarafından sahiplenilir ve yönetilir ve birden fazla hesap genelinde kullanılır. Sonuç olarak, bu anahtarlar AWS hesabınızın kendisine bağlı değildir.
-   **Veri Anahtarları (Data Keys):** Bunlar, KMS dışında veriyi ve diğer anahtarları şifrelemek için kullanılan ve bir AWS KMS Anahtarı tarafından oluşturulan anahtarlardır.
-   **HMAC Anahtarları (HMAC Keys):** Bu, esasen hash tabanlı mesaj kimlik doğrulama kodları (HMAC) oluşturmanıza ve doğrulamanıza olanak tanıyan simetrik bir anahtardır.

KMS servisinin bu anahtarları kullanarak yalnızca at rest (durağan) şifrelemeyi uygulayabildiğini anlamak önemlidir, KMS transit veya hareket halindeki veri için şifreleme gerçekleştirmez. Transit halindeki veriyi şifrelemek istiyorsanız, SSL gibi farklı bir yöntem kullanmanız gerekir. Ancak, veriniz KMS kullanılarak at rest şifrelenmişse, iki taraf arasında gönderilirken bu veri cipher text'te olacak ve yalnızca ilgili anahtarla plain text'e dönüştürülebilecektir.

Production ortamınızda uyumluluk, yönetişim ve diğer düzenlemeleri sürdürürken, şifreleme genellikle güvenlik stratejinizin temel bir unsuru olarak gereklidir. Sonuç olarak, KMS, şifreleme anahtarlarınızın nasıl ve kim tarafından kullanıldığını denetlemek ve izlemek için AWS CloudTrail ile sorunsuz bir şekilde çalışır, ayrıca kaynak IP adresi gibi API'ler tarafından yakalanan diğer metadata'ları da kaydeder. S3'te depolanan CloudTrail günlükleri, Decrypt, Encrypt, GenerateDataKey, GetKeyPolicy ve daha fazlası gibi KMS API çağrılarını kaydeder.

### KMS Servisinin Bileşenleri (Component of the KMS Service)

Bu başlık altında, KMS servisinin bileşenlerinden bazılarına daha derinlemesine odaklanarak, nasıl bir araya getirildiğini ve nasıl çalıştığını inceleyeceğiz.

İlk olarak, **AWS KMS Keys** ile başlayalım. Bu anahtarlar,  KMS servisindeki temel anahtarlardır ve verilerinizi şifrelemek ve şifresini çözmek gibi kriptografik işlemleri gerçekleştirmek için kullanılabilir. Bu anahtar ayrıca, KMS dışında veri şifrelemek için kullanılan data key'leri oluşturmak için de kullanılabilir.

Varsayılan olarak, bir KMS key oluşturulduğunda, simetrik 256-bit AES-GCM şifreleme anahtarı olarak oluşturulur. Hatırlatmak gerekirse, simetrik şifreleme, verilerin şifresini çözmek için kullanılan anahtarın, şifrelemek için kullanılan anahtarla aynı olması gerektiği anlamına gelir. Sonuç olarak, simetrik bir KMS key oluşturulduğunda, anahtar her zaman güvenli kalmasını ve altta yatan hardware security module'lerde (HSM'ler) saklanmasını sağlamak için KMS içinde kalacak ve servisten asla çıkmayacaktır.

Adınıza şifreleme sağlayan herhangi bir AWS servisinin, gerekli şifreleme/şifre çözme işlemlerini gerçekleştirmek için KMS servisi içindeki KMS key'lerle doğrudan entegre olarak simetrik şifreleme kullandığını göreceksiniz. Ancak, ilginç bir şekilde, şifreleme ve imzalama için kullanılabilen asimetrik KMS key'ler de oluşturabilirsiniz, ancak ikisi birden olmaz! Simetrik şifreleme sadece 1 anahtar kullanırken, asimetrik şifreleme 2 anahtar kullanır, biri private ve diğeri public. Bu nedenle asimetrik bir KMS key oluşturulduğunda elbette hem bir public hem de preivate KMS key çifti oluşturacaktır. Çiftin açık anahtarı KMS dışında kullanılabilir ve KMS operasyon çağrıları aracılığıyla serbestçe erişilebilir ve ayrıca indirilebilir. Ancak private asimetrik KMS key, önce şifrelenmeden KMS'den asla çıkmayacaktır. Genellikle, asimetrik KMS key'lerin kullanımı, AWS dışında şifreleme gerektiğinde veya veriyi şifrelemek için doğrudan KMS'i çağıramayan kullanıcılar tarafından gereklidir.

Müşteri olarak bu asimetrik KMS key'leri oluşturma yeteneğine sahibiz, bu da bize anahtarların kendileri üzerinde izin ve yönetim perspektifinden daha fazla kontrol sağlar.

Customer, AWS Managed Keys ve AWS Owned keys, KMS ile çalışırken sahiplik perspektifinden 3 farklı anahtar türü vardır, bunlar:

- Customer managed
- AWS managed
- AWS owned

**Customer managed keys:** KMS ile çalışırken kendi KMS key'lerimizi oluşturabiliyoruz, bunlara Customer Managed Keys denir ve asimetrik anahtarlardır. Oluşturulduktan sonra, müşteri olarak hem izinler hem de yönetim açısından anahtar üzerinde çok daha fazla kontrole sahibiz. Bu yetenek, anahtara yalnızca izin verdiğimiz kişiler tarafından erişilebilmesini sağlamak için anahtar politikaları ve IAM politikaları ve izinler gibi diğer erişim kontrol yöntemlerini oluşturup ilişkilendirebileceğimiz anlamına gelir. Ayrıca anahtar üzerinde aşağıdaki gibi yönetim işlevlerine ve yeteneklerine sahibiz:

- Anahtarı etkinleştirme veya devre dışı bırakma
- Anahtar rotasyonunu yönetme
- Etiket ekleme
- Takma adlar oluşturma
- Anahtar silmeyi yönetme

Bazı AWS Servisleri ile çalışırken, verileri şifrelemek için kullanılacak bir customer managed KMS key belirtebiliriz.

**AWS managed keys:** AWS Managed keys, verilerinizi korumaya yardımcı olmak ve Customer managed keys ile aynı işlevi yerine getirmek için KMS ile entegre olan AWS servisleri tarafından otomatik olarak oluşturulan anahtarlardır.

KMS entegre servisleri, kaynağınızı ve verilerinizi korumak için şifreleme seçeneklerinizi yapılandırırken bir AWS managed KMS key veya bir Customer managed KMS key seçme seçeneği sunabilir. Hangi seçeneği istediğinize karar vermek size bırakılmıştır. AWS managed key'i seçerseniz, o anahtarın yönetimi AWS tarafından ele alınır ve sizin herhangi bir yönetim göreviniz olmaz, bunun yerine hepsi AWS tarafından yönetilir. Bu AWS managed key'ler, ilişkili AWS servisi tarafından açıkça oluşturulur ve yalnızca o servis tarafından kullanılır.

AWS managed key'ler KMS içinden görüntülenebilir ve isimle kolayca tanımlanabilir. Bu key'ler, şu formata sahiptirler: `aws/servicename`, örneğin `aws/rds`. AWS managed key'leri görüntüleyebilir ve ilişkili anahtar politikası gibi bilgileri görüntüleyebilseniz de, anahtarı herhangi bir şekilde yönetemezsiniz. Örneğin, anahtar rotasyon ayarlarını yönetmek, anahtar politikasını değiştirmek veya anahtarın kendisini silmek mümkün değildir.

İlk olarak, Hash-based authentication codes'un, yani HMAC'in ne olduğunu tanımlayalım. Yüksek düzeyde şöyle tanımlanabilir:

"....bir kriptografik hash fonksiyonu ve gizli bir kriptografik anahtar içeren belirli bir mesaj kimlik doğrulama kodu (MAC) türüdür. Herhangi bir MAC gibi, bir mesajın hem veri bütünlüğünü hem de özgünlüğünü aynı anda doğrulamak için kullanılabilir." - Wikipedia

Peki nasıl çalışır? HMAC algoritmaları, mesajınızı veya verilerinizi alır. Veriyle ilişkilendirilen benzersiz bir sabit boyutlu etiket oluşturmak için HMAC anahtarının anahtar materyali ile birleştirir. Şifrelenmiş verileri görüntülerken, orijinal veriden veya gizli anahtardan tek bir karakterin bile değişip değişmediğini tespit edebilecektir çünkü ortaya çıkan sabit boyutlu etiket farklı olacaktır. Bu, verilerin bütünlüğünü ve özgünlüğünü kontrol etmenize, böylece orijinal şifreleme noktasından herhangi bir şekilde değiştirilmediğinden emin olmanıza olanak tanır.

KMS içinde, bir HMAC key RDS 2104'te belirtilen standartlara uyar ve çeşitli uzunluklarda simetrik bir anahtar olarak oluşturulur ve HMAC kodlarını oluşturmak ve doğrulamak için kullanılır. HMAC key'de gömülü olan anahtar materyali, ilişkili HMAC algoritmalarının kullandığı gizli anahtarı içerdiği için önce şifrelenmeden KMS servisinden asla çıkmaz. HMAC key'lerle çalışırken, verilerinizin bütünlüğünü ve özgünlüğünü aşağıdaki operasyonları kullanarak doğrulamanıza yardımcı olacak farklı operasyonlar kullanmanız gerekecektir: GenerateMac ve VerifyMac.

Data key'ler KMS Keys tarafından oluşturulur ve verileri şifrelemek ve şifresini çözmek için kullanılır, ancak KMS'in kendisi verileri şifrelemek için Data Key'leri kullanmaz. Bunun yerine, şifrelemeyi basitleştirmeye yardımcı olan bir client-side şifreleme kütüphanesi olan AWS Encryption SDK ile çalışırken gibi durumlarda data key'ler KMS dışında veri şifrelemek için kullanılmak üzere tasarlanmıştır.

Bir data key oluşturmak için, mevcut bir KMS key kullanarak `GenerateDataKey` operasyonunu kullanabilirsiniz. Bu, Data Key'in şifrelenmiş bir versiyonunun yanı sıra bir Plaintext Data Key de oluşturacaktır.

Bu anahtarları kullanarak verilerinizi şifrelemek için, Plaintext data key'i bir şifreleme algoritmasıyla birlikte kullanarak Ciphertext'e dönüştüreceksiniz. Bu aşamada plaintext data key bellekten kaldırılır ve encrypted data key depolanır ve yeni şifrelenmiş veriyle ilişkilendirilir.

Verilerinizin şifresini çözmek için, encrypted data key KMS'e gönderilecek, burada data key'i oluşturmak için kullanılan KMS key, onu çözmek ve anahtarı Plaintext data key olarak döndürmek için kullanılır, bu da orijinal verilerinizin şifresini çözmek için bir şifre çözme algoritması kullanılarak kullanılır.

Ayrıca, `GenerateDataKeyWithoutPlaintext` operasyonunu kullanarak Plaintext data key oluşturmadan şifrelenmiş bir Data Key oluşturmanın da mümkün olduğunu bilmelisiniz. Bu encrypted data key daha sonra kriptografik işlemleriniz için anahtarın plaintext versiyonunu kullanmanız gerektiğinde KMS'e gönderilebilir.

Data Key'leri kullanırken 2 anahtar oluşturabilse de, biri şifrelenmiş ve biri plaintext, hala simetrik şifreleme olarak sınıflandırılır çünkü sadece bir anahtar kullanır, sadece anahtarın 2 versiyonu vardır, biri plaintext ve diğeri tam olarak aynı anahtardır ancak şifrelenmiştir.

Data Key'ler tanım gereği simetrik iken, Data Key pair'leri asimetriktir, yani şifreleme için bir, şifre çözme için bir olmak üzere 2 farklı anahtar kullanırlar, peki Data Key Pair'ler ne için kullanılır? Temel olarak KMS dışında, örneğin client-side kriptografi işlemleri için ve ayrıca imzalama ve doğrulama için tasarlanmıştır.

KMS, aşağıdakileri içeren çeşitli Data Key pair'lerini destekler:

- RSA key pair'leri (Genellikle sertifikalar için kullanılır): RSA_2048, RSA_3072 ve RSA_4096,
- Eliptik eğri key pair'leri (Genellikle dijital imzalar için kullanılır): ECC_NIST_P256, ECC_NIST_P384, ECC_NIST_P521,
- ECC_SECG_P256K1 (Kripto para birimleri),
- SM key pair'leri (Sadece Çin Bölgeleri): SM2,

Asimetrik bir çift olduğu için, Private key beklendiği gibi KMS içinde saklanır ve kendisi seçtiğiniz bir simetrik KMS key tarafından şifrelenir.

Mevcut bir KMS key kullanarak bir Data Key Pair oluştururken, bir Public key'in yanı sıra bir Plaintext Private Key ve Plaintext private key'in Encrypted versiyonu oluşturulur. Data Key'lerde olduğu gibi, ilgili KMS operasyonunu kullanarak plaintext key oluşturmadan da bir Data Key Pair oluşturabilirsiniz.

Data key pair'lerini kullanarak şifreleme söz konusu olduğunda, plaintext verilerinizi alıp verileri ciphertext olarak döndürmek için bir şifreleme algoritmasıyla birlikte public key kullanılır. Nesnenin şifresini çözme söz konusu olduğunda, ciphertext verisini plaintext verisi olarak döndürmek için aynı şifreleme algoritmasına ek olarak data key pair'in plaintext versiyonu kullanılır. Yine, şifresi çözüldükten sonra plaintext key bellekten kaldırılır. Plaintext key'e tekrar bir şifre çözme işlemi için ihtiyaç duyulduğunda, private key'in encrypted versiyonu orijinal KMS key ile birlikte plaintext versiyon döndürmek için kullanılacaktır.

Key material, AWS'de bir anahtarla ilişkilendirilen önemli bir parçadır, kriptografik bir algoritmanın bir parçası olarak kullanılan elementtir ve etkili bir şekilde sadece bir bit dizisidir. Korunması gereken ve simetrik şifreleme ile kullanılan private key material vardır. Sonra asimetrik şifrelemede public key içinde yaygın olarak kullanılan public key material'imiz var.

Simetrik anahtarlar için bu key material'in kökeni çeşitli güvenli kaynaklardan depolanabilir, örneğin:

- AWS KMS'in kendisinden
- İlgili KMS key harici bir key store tarafından destekleniyorsa, KMS dışından kendi harici key manager'ınızdan da kaynaklanabilir
- Bir CloudHSM key store'unda depolandığında kaynak olarak CloudHSM'i de seçebilirsiniz.
- Ayrıca kendi key material'inizi de içe aktarabilirsiniz, ancak bununla birlikte bu key material'i güvence altına almaktan siz sorumlusunuz

Her anahtar için key material benzersizdir, ancak birden fazla bölgede kullanılabilecek bir KMS key oluşturmak istiyorsanız, birden fazla bölgede depolanan bu anahtarların her biri için key material aynı olacaktır. Bu durum, sanki aynı anahtarı kullanıyormuşsunuz gibi birden fazla bölgede depolanan verileri şifrelemenize/şifresini çözmenize yardımcı olur. Çok bölgeli anahtarlar genellikle felaket kurtarma ve geniş coğrafi alanlara yayılan veri yönetimi için kullanılır.

KMS içinde çalışırken bir güvenlik en iyi uygulaması olarak, customer managed key'leriniz için bir seviye Key rotation uygulamalısınız. Key rotation, anahtarlarınız için key material'i yeni key material ile değiştirme sürecidir. Önerilen otomatik anahtar rotasyonunu uygulayabilirsiniz ve AWS KMS bu rotasyonu sizin için yönetecektir. KMS key'in kendisinin değişmediğini, sadece anahtarla ilişkili key material'in değiştiğini anlamak önemlidir, bu da belirli bir anahtara atıfta bulunan uygulamalarınızda herhangi bir şeyi güncellemeniz gerekmediği anlamına gelir. AWS managed veya AWS Owned olan anahtarlar için, KMS bu anahtarların key material'inin rotasyonunu otomatik olarak yönetecektir.

Anahtarlarınızı döndürürken, mevcut tüm key material korunur. Bu durum, KMS'nin daha önce kullanılan key material'in herhangi bir önceki versiyonu ile şifrelenmiş verilerin şifresini çözmesine olanak tanır. Yönetim perspektifinden, verilerinizin hangi key material versiyonu ile şifrelenmesini istediğinizi seçmek mümkün değildir, her zaman şifreleme anında anahtarla ilişkili olan key material ile şifrelenecektir.

**Key policy**, KMS içindeki belirli bir anahtarı kimin kullanabileceğini ve erişebileceğini tanımlamanıza olanak tanıyan bir güvenlik özelliğidir. Bu politikalar KMS key'lere bağlıdır, bu da politikaları kaynak tabanlı politikalar yapar ve farklı KMS key'ler için farklı key policy'ler oluşturulabilir. Bu izinler, tıpkı IAM politikaları gibi JSON tabanlı olan bu key policy belgesinde tanımlanır.

Grant'lar, KMS key'lerin kullanımına erişimi kontrol etmenin başka bir yöntemidir ve öncelikle geçici erişim için kullanılır. Grant'lar kaynak tabanlı bir politikadır, ancak kendi KMS Key erişiminizin bir alt kümesini AWS hesabınızdaki başka bir kullanıcı gibi başka bir principal'a devretmenize olanak tanır. Bunun faydası, birinin o KMS Key için erişim kontrol izinlerini değiştirme riskinin daha az olmasıdır. Herhangi birinin KMS putkey policy iznine sahip olması durumunda, key policy'yi basitçe farklı bir policy ile değiştirebilirler. Grant'ları kullanmak bu olasılığı ortadan kaldırır, çünkü erişim gerektiren her principal için bir grant oluşturulur ve KMS Key'e uygulanır.

### KMS Erişimi: Politika Değerlendirme Mantığı (KMS Access: Policy Evaluation Logic)

Bir KMS anahtarına kimin erişimi olduğunu anlamak biraz kafa karıştırıcı olabilir, çünkü key policy, IAM politikaları ve ayrıca Grant'lar aracılığıyla bir KMS anahtarına erişim kazanmanın ve kullanmanın üç potansiyel yolu vardır.

Doğru erişim seviyesini belirlemek, bu erişim yöntemlerinin birbirleriyle nasıl çalıştığını anlamanız gerektiği anlamına gelir. Öyleyse bazı önemli noktaları anladığımızdan emin olmak için basit bir örneğe bakalım. Bu senaryoda üç KMS anahtarımız ve dört kullanıcımız var.

Burada bu örnek için geçerli olan KMS anahtarlarını, kullanıcıları ve senaryo ifadelerini görebilirsiniz.

Yani üç KMS Key'imiz var: KeyA, KeyB ve KeyC, ve dört Kullanıcımız var: Alana, Danny, Carlos ve Jorge.

![dpmC3mu.md.png](https://iili.io/dpmC3mu.md.png)

Senaryo şu şekilde ifade edelim:

-   Key-A key policy, erişimi yönetmek için IAM kullanıcı izinlerinin kullanılmasını sağlar.
-   Key-B key policy, Danny ve Carlos'un kriptografik işlemler gerçekleştirmesine izin verir. IAM aracılığıyla erişim kontrolü etkinleştirilmemiştir.
-   Key-C key policy, erişimi yönetmek için IAM kullanıcı izinlerinin kullanılmasını sağlar. Danny ve Carlos için erişim açıkça reddedilir, ancak Alana ve Jorge'ye tam kriptografik işlem erişimi verilir. Jorge'nin ayrıca grant oluşturma erişimi vardır.
-   Alana'nın IAM policy izinleri, Key-A ve Key-B için tüm KMS eylemlerine izin verir.
-   Danny'nin IAM policy izinleri yoktur.
-   Carlos'un IAM policy izinleri, Key-A'ya KMS şifreleme erişimine izin verir.
-   Jorge'nin IAM policy izinleri, Key-B ve Key-C için tüm KMS eylemlerine izin verir.

Şimdi bu kullanıcıların her birinin erişimine bakalım ve kriptografik işlemler gerçekleştirip gerçekleştiremeyeceklerini görelim. Alana ile başlayalım:

Alana'nın Key-A'ya erişimi başarılıdır çünkü IAM policy izinleri Key-A'ya karşı tüm KMS eylemlerine izin verir ve Key-A, erişimi yönetmek için IAM politikalarının kullanılmasına izin verir. Key-B'ye erişimi reddedilir çünkü bu Anahtar için key policy, IAM politikalarının kullanılmasına izin vermez. Alana'nın Key-C'ye erişimi başarılıdır çünkü key policy, IAM politikasıyla ilgili izinleri olmamasına rağmen erişime izin verir, erişim tamamen key policy aracılığıyla verilir.

Şimdi Danny'ye bakalım. Key-A'ya erişimi reddedilir çünkü key policy'de Danny'nin erişimi için açık girişler yoktur ve ilişkilendirilmiş IAM policy izinleri yoktur. Key-B'ye erişimi başarılıdır çünkü key policy, IAM policy izinleri olmamasına rağmen Danny'ye erişim izni verir. Danny'nin Key-C'ye erişimi, key policy içindeki açık reddetme eylemleri nedeniyle reddedilir. Açık bir 'reddetme' her zaman diğer herhangi bir izne üstün gelecektir.

Şimdi Carlos'un erişimine bakalım. Key-A için, sadece IAM policy izinleri aracılığıyla verilen 'şifreleme' erişimi vardır ve erişimi yönetmek için IAM policy izinlerinin kullanılmasına izin verilir. Key-B için erişim de başarılıdır çünkü key policy ona erişim izni verir. IAM policy izinleri önemsizdir çünkü key policy, erişimi yönetmek için IAM politikalarının kullanılmasına izin vermez. Ve Key-C'ye erişimi, key policy içindeki açık reddetme eylemleri nedeniyle reddedilir ve açık bir reddetme diğer herhangi bir izne üstün gelecektir.

Ve son olarak Jorge'nin erişimine göz atalım. Key-A'ya erişimi yoktur çünkü ne key policy ne de IAM policy izinleri erişim sağlar. Key-B'ye erişimi yoktur çünkü bu Anahtar için key policy, IAM politikalarının kullanılmasına izin vermez. Yani Jorge için IAM Policy düzeyinde erişim verilmiş olmasına rağmen, Key policy IAM politikalarının kullanılmasına izin vermez ve bu nedenle bu göz ardı edilir. Key C'ye erişim, grant oluşturma yeteneğine ek olarak KMS kriptografik işlemleri için izin verilir.

## Dijital Sertifikalar ve AWS Sertifika Yöneticisi (Digital Certificates and AWS Certificate Manager)

Bu başlık altında, dijital sertifikaların önemi ve AWS Certificate Manager'ın temel özelliklerini ele alacağız. Hepimiz onlarca yıldır dijital sertifikaları kullanıyoruz ve çoğumuz muhtemelen her gün dijital sertifikalardan faydalanıyoruz. HTTPS kullanan bir web sitesine her gittiğimizde, dijital sertifikalardan yararlanmış oluyoruz. Dijital sertifikalar ayrıca site-to-site VPN'lerde yer alan uç noktaların kimlik doğrulaması sırasında da kullanılır; böylece bir VPN tüneli kurulabilir. Bunun yanı sıra, durağan ve transit verilerin bütünlük kontrollerinin bir parçası olarak kullanılan dijital imzaların doğrulanması sırasında, çok faktörlü kimlik doğrulamanın bir parçası olarak da kullanılır.

Temelde, dijital sertifikalar iletişim kurduğumuz web sitesinin, hizmetin veya kullanıcının geçerli olduğuna güvenmemizi sağlar. İletişim kurduğumuz varlığın geçerli olduğuna güveniyorsak, kimlik doğrulama yapılandırması, inkar edilemezlik yapılandırması, bütünlük kontrolleri yapılandırması (configure non-repudiation) ve şifreleme yapılandırması gibi işlemleri gerçekleştirebiliriz. Dijital sertifikalar, içlerine gömülü bir public key ile gelir. Sertifika, public key'i doğrular; yani public key'e güvenebilir, sertifikayı web sunucularıyla güvenli iletişim kurmak ve dijital sertifikaları doğrulamak gibi görevleri gerçekleştirmek için kullanabiliriz. Dijital sertifikaların kendilerine de güvenilmesi gerekir.

Dijital sertifikalara güvenemezsek, onlarla birlikte gelen public key'lere de güvenemeyiz ve dolayısıyla bu public key'leri bağlantılarımızı güvence altına almak için kullanamayız. Güvenebileceğimiz dijital sertifikalar elde etmek için, güvenilir certificate authority'lerden dijital sertifika talep ederiz. Certificate authority'ler public veya private olabilir. Public certificate authority'ler çoğu işletim sistemi tarafından zaten güvenilir kabul edilir. Public certificate authority'ler, işletmelere gömülü public key'lerle birlikte güvenilir sertifikalar verir. Public certificate authority'ler tarafından verilen sertifikalar genellikle web siteleri gibi public hizmetlerde kullanılır. Public certificate authority'den bir sertifika talep ederken, genellikle on-premise bir key pair oluşturursunuz. Private key'inizi gizli tutarsınız ve ardından bir certificate signing request (CSR) oluşturursunuz. CSR, public key'inizi, güvence altına almak istediğiniz DNS adlarını ve dijital imzanızı içerir.

CSR'nizi seçtiğiniz certificate authority'ye gönderirsiniz ve CSR'deki alan adlarının sahibi olduğunuzu doğruladıktan sonra, bir dijital sertifika verilir. Public certificate authority'ler hizmetleri için bir ücret talep eder. Private certificate authority'ler tarayıcılarımız ve işletim sistemlerimiz tarafından otomatik olarak güvenilir kabul edilmez. Tarayıcılarımızı ve işletim sistemlerimizi, private certificate authority'ler tarafından verilen sertifikalara güvenecek şekilde yapılandırmak için, certificate authority'nin root sertifikasını işletim sistemlerimizin güvenilir root sertifika deposuna aktararak yapılandırırız. Bu aktarımı tamamladıktan sonra, private certificate authority tarafından verilen sertifikalara güvenilir ve bunları bağlantıları güvence altına almak için kullanabiliriz. Private CA'lardan sertifika talebinde bulunma şeklimiz, public CA'lardan talep ettiğimiz şekilde, bir CSR göndererek gerçekleşir.

Public CA'lar ile private CA'lar arasındaki en büyük fark, private CA'lar için certificate authority altyapısını bizim kurmamızdır. Certificate authority'nin güvenliğinden, yedeklenmesinden, yüksek kullanılabilirliğinden ve günlük yönetiminden biz sorumluyuz; bu da birden fazla sertifika sunucusunu yönetmek anlamına gelebilir. Altyapıyı siz yönettiğiniz için, private CA'lar tarafından verilen sertifikalar ücretsizdir. Private CA'lar tarafından verilen sertifikalar yalnızca dahili olarak kullanılır, çünkü müşterileriniz veya İnternet'teki diğer hizmetler tarafından güvenilir kabul edilmeyecektir. Dijital sertifikalarla çalışırken, private CA'lar için kendi altyapınızı yönetme zorluğunun yanı sıra birçok başka zorluk da vardır. Ek sertifika zorlukları arasında, sertifika taleplerini yönetmek, süresi dolmak üzere olan dijital sertifikaları yenilemek ve değiştirmek, public CA'lardan alınan sertifikaların maliyeti, certificate authority altyapısının güvenliğini sağlamak ve private CA'larınız için certificate revocation list'leri (CRL'ler) yönetmek sayılabilir.

CRL'ler, bir CA tarafından verilmiş ancak artık güvenilmemesi gereken sertifikaların listesidir. Her CA, müşterilerinin erişebileceği bir CRL'de iptal edilmiş sertifikaların bir listesini yayınlamaktan sorumludur; böylece listedeki bir sertifikayla karşılaştıklarında ona güvenmemeleri gerektiğini bilirler. AWS Certificate Manager, güvenilir bir public certificate authority'den ücretsiz olarak SSL/TLS sertifikaları talep etmemize olanak tanır. Bu sertifikalar, Elastic Load Balancers, Amazon CloudFront ve API Gateway gibi AWS hizmetleriyle güvenli bağlantılar kurmak için kullanılabilir. AWS Certificate Manager public certificate authority'leri tarafından verilen sertifikalar yalnızca AWS hizmetleriyle kullanılabilir. Certificate Manager'ı kullanarak bir private certificate authority de kurabilirsiniz. Kurulduktan sonra, private certificate authority'nizi EC2 üzerinde çalışan uygulamalarınızla ve on-premise çalışan uygulamalarınızla iletişimi güvence altına almak için sertifika vermek üzere kullanabilirsiniz.

AWS Certificate Manager'ın faydaları arasında; halka açık güvenilir sertifikalar ücretsiz olarak mevcuttur, key pair oluşturmaya veya CSR vermeye gerek yoktur, bunlar sertifika talebiniz sırasında otomatik olarak oluşturulacaktır, certificate authority altyapısını yapılandırmaya gerek olmaması yer almaktadır. Private bir CA oluşturduğunuzda bile, bu AWS Certificate Manager tarafından yönetilir. Yani AWS, CA'nızı barındıran sunucuların yüksek kullanılabilirliğinden, yedeklenmesinden ve günlük yönetiminden sorumludur. AWS Certificate Manager'ın en iyi özelliklerinden biri, yeni sertifikaları değiştirme yeteneğidir. Kullandıkları sertifikaların süresi dolduğu için kapanan birçok hizmet gördüm. AWS Certificate Manager kullanılırken, bu durum yaşanmamalıdır çünkü süresi dolmak üzere olan uygun sertifikaları değiştirmek için otomatik olarak yeni sertifika talepleri oluşturacak ve ardından süresi dolan sertifikaları korudukları hizmetlerin yapılandırmasında değiştirecektir.

## AWS WAF

Bu başlık altında, AWS WAF servisine giriş yapacağız. CloudFront Distributions, Amazon API Gateway REST APIs, Application Load Balancers veya AWS AppSync GraphQL APIs aracılığıyla herhangi bir web içeriği sunuyorsanız, ek bir güvenlik katmanı olarak AWS Web Application Firewall servisini kullanmanız önerilir.

Web application firewall kullanmadan, web sitelerinizi ve web uygulamalarınızı potansiyel olarak zararlı ve kötü niyetli trafiğe maruz bırakabilirsiniz. Bu da ortamınızda güvenlik risklerine yol açabilir. Bu durum, işletmeniz üzerinde hem finansal hem de itibar açısından önemli olumsuz etkilere neden olabilir. AWS Web Application Firewall, web sitelerinin veya web uygulamalarının yaygın web saldırı modellerine karşı kötü niyetli saldırılardan korunmasına yardımcı olan bir servistir. Bunların çoğu, SQL injection ve cross-site scripting gibi OWASP top 10 listesinde belirtilmiştir.

Buna ek olarak, kaynak IP adresi veya ülke kökeni gibi kriterlere dayalı istekleri filtreleme gibi kendi özel kriterlerinizi de belirleyebilirsiniz. OWASP (Open Web Application Security Project), başkalarının güvenliği ve yazılımı geliştirmelerine yardımcı olmaya adanmış, kâr amacı gütmeyen bir kuruluştur. Uygulama mimarisi ile ilgili en kritik güvenlik açıklarını ve risklerini içeren bir top 10 listesi sunarlar. En güncel OWASP top 10 listesi için lütfen şu bağlantıyı ziyaret edin.

Eğer mimarinizde bu güvenlik açıklarından bazılarını azaltmak için bir WAF uygulayabilirseniz, bu web uygulama mimariniz için büyük bir varlık ve kuruluşunuzdaki güvenlik görevlileri için büyük bir rahatlama olacaktır. AWS WAF'ın uygulama ve yönetim süresini standart bir WAF çözümüyle karşılaştırırsanız, çok daha hızlı olduğunu göreceksiniz. Ayrıca, AWS WAF çok daha basit ve yönetimi kolaydır. Şu anda AWS WAF'ın iki versiyonu bulunmaktadır: AWS WAF classic ve AWS WAF.

AWS WAF classic'i sadece Kasım 2019'dan önce AWS WAF kaynakları oluşturduysanız kullanmalısınız. Bu nedenle, bu başlıkta yeni AWS WAF'a odaklanacağız. AWS WAF servisi, Amazon API Gateway, Amazon CloudFront distributions, Application Load Balancers ve AWS AppSync ile etkileşime girerek, yalnızca belirli koşulları karşılayan filtrelenmiş web isteklerinin bu hizmetler tarafından işlenmek üzere iletilmesini sağlar. WAF, bu diğer hizmetlerle birlikte çalışarak hem HTTP hem de HTTPS isteklerini filtreleyerek, meşru ve zararlı gelen istekleri ayırt eder ve bunların izin verilip verilmeyeceğine karar verir.

Şimdi WAF servisinin nasıl çalıştığını anlamanızı sağlamak için servisin bileşenlerine daha yakından bakalım. WAF ile ilgili bir dizi temel bileşen vardır. Bunlar Web ACLs, Rules ve Rule Groups'tur. Servis içindeki rollerini anlamak için bunları tek tek açıklayalım, konfigürasyonun ana yapı taşı olan Web ACL ile başlayalım.

**Web Access Control Lists (Web ACL):** WAF servisinin ana yapı taşıdır ve hangi web isteklerinin güvenli, hangilerinin güvenli olmadığını belirlemek için desteklenen kaynaklardan biriyle ilişkilendirilen bileşen olarak kullanılır. Bu repo'nun yazıldığı sırada, desteklenen kaynaklar arasında Amazon CloudFront Distributions, Amazon API Gateway REST APIs, Application Load Balancers ve AWS AppSync GraphQL APIs bulunmaktadır. Web ACL, her web isteğinin izin verilmesi veya engellenmesi gerekip gerekmediğini belirlemek için kullanılan belirli kontrolleri ve kriter kontrollerini içeren kuralları barındırır. Ayrıca, her Web ACL için, gelen istek kurallarda belirtilen kriterleri karşılamazsa trafiğin izlemesi gereken varsayılan bir eylem vardır ve bu eylem seçenekleri allow veya block'tur.

**Rules:** Her kural, web isteğinin inceleneceği belirli kriterlere odaklanan ifadeler ve eylemler içerir. İncelenen istek, ifadede belirtilen kriterlere uyuyorsa, bu bir eşleşme olarak kabul edilir. Bu eşleşmenin sonucu daha sonra allow, block veya count eylemlerinden birini izleyebilir. Allow, isteğin kaynağa iletildiği anlamına gelir. Block, isteğin düşürüldüğü ve istekte bulunana isteğin reddedildiğini bildiren bir yanıt gönderildiği anlamına gelir. Count ise sadece eşleşen isteklerin sayısını sayar.

**Rule Groups:** Bir Rule Group, esasen farklı Web ACL'lere ihtiyacınız olduğunda uygulayabileceğiniz bir kural koleksiyonudur. AWS WAF ayrıca, kaynaklarınızı bazı yaygın saldırı modellerine karşı korumak üzere tasarlanmış ve oluşturulmuş bir dizi AWS yönetilebilir grupla önceden yapılandırılmış olarak gelir. Buna ek olarak, AWS marketplace'te diğer satıcılar tarafından oluşturulan ve satılan başka Rule Groups'lara da erişebilirsiniz.

Mantıksal bir mimari perspektifinden bakarsak, WAF'ın kaynağı S3'e işaret eden bir CloudFront Distribution ile ilişkilendirildiğini varsayalım. Ağ diyagramı aşağıdaki gibi görünecektir:

![dyBDr1j.md.png](https://iili.io/dyBDr1j.md.png)

İlk olarak, bir kullanıcı CloudFront Distribution tarafından sunulan web içeriğine bir istek başlatır. Her ne kadar mantıksal olarak AWS WAF, CloudFront'un önünde dursa da, istek önce CloudFront Distribution tarafından alınacaktır. Ardından hemen ilişkili WAF Web ACL'ye iletilir. Dağıtımla ilişkili AWS WAF Web ACL, Rules veya Rule Groups kullanarak gelen web trafiğini filtreler. Böylece, CloudFront ortamınıza ve ağınıza ulaşmadan önce, gelen isteği algılama, analiz etme ve izin verme veya engelleme yeteneğine sahip olursunuz.

Kurallarda belirtilen kriterler, isteğin engellenmesi gerektiğini belirlerse, trafik durdurulur ve daha ileri gitmesi engellenir. İstekte bulunan kişiye isteğin reddedildiği bildirilir. Trafik, geçiş için izin veren kriterleri karşılarsa, istek CloudFront'a iletilir. CloudFront daha sonra gerektiği gibi içeriği sunar. Belki de kuruluşunuzda, WAF'ın ele aldığı bazı aynı riskleri azaltmak için, belki de web sunucusu katmanında, altyapınızın daha derinlerinde çalışan başka güvenlik algılama mekanizmalarınız vardır.

Bu durumda, mevcut çözümüm iyi çalışıyorsa neden WAF'ı uygulamalıyım diye düşünüyor olabilirsiniz. Altyapınızda mevcut algılama sistemleriniz varsa, bu harika. Ancak, mantıksal olarak web uygulamanıza ne kadar yakın uygulanırlarsa, altyapınızın kenarlarına doğru ek güvenlik açıklarının ortaya çıkma riski o kadar yüksek olur. Güvenlik açığı risklerini ağ ortamınızın çevresine mümkün olduğunca yakın bir noktada azaltmak en iyisidir. Bunu yaparak, diğer altyapı ve sistemlerin tehlikeye girme olasılığını azaltırsınız.


## AWS Security Token Service (STS)

Bu başlık altında kısaca, AWS Security Token Service veya kısaca STS'yi tanıyacağız. Bu servis, AWS Certified Developer - Associate sınavı için kapsama yeni alındı.

STS, IAM kullanıcıları veya AWS dışındaki diğer federe kimlik kullanıcıları için geçici, sınırlı ayrıcalıklı kimlik bilgileri talep etmenizi sağlayan bir web servisidir. Bu kullanıcılar, SAML, OAuth veya OpenID Connect ile kimliği doğrulanmış kullanıcıların yanı sıra Amazon Cognito kimlik havuzlarını da içerebilir.

STS'ye programatik olarak global endpoint sts.amazonaws.com üzerinden erişilebilir. Yaygın STS aksiyonları şunları içerir:

-   **AssumeRoleWithSAML:** SAML aracılığıyla kimliği doğrulanmış kullanıcılar için geçici güvenlik kimlik bilgileri döndürür.
-   **AssumeRoleWithWebIdentity:** Facebook veya Google gibi OAuth veya OpenID Connect kimlik sağlayıcıları aracılığıyla kimliği doğrulanmış kullanıcılar için geçici güvenlik kimlik bilgileri döndürür.

STS, AWS CloudTrail'i destekler; burada STS endpoint'lerine yapılan tüm kimliği doğrulanmış ve doğrulanmamış istekler, denetim ve sorun giderme amaçları için kaydedilir.


