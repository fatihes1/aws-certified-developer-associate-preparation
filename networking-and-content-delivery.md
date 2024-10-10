# Networking and Content Delivery (DVA-C02)

Bu başlık altında, şu anda AWS'de mevcut olan çeşitli **ağ iletişimi ve içerik dağıtım** hizmetlerini tanıtan bir dizi alt başlık bulunmaktadır.

Bu başlığın temel amacı, geliştiriciler için AWS'deki ağ iletişimi ve içerik dağıtım hizmetlerine bir giriş sağlamaktır. Bu başlık altında değineceğimiz hizmetler şu şekilde listelenebilir:

-   Amazon API Gateway,
-   Amazon CloudFront,
-   Elastic Load Balancing,
-   Amazon Route 53, ve
-   Amazon VPC.

## VPC nedir (What is a VPC) ?

Bu başlık altında Virtual Private Clouds'lere (VPC) yakından bakacağız. VPC'nin ne olduğunu anlamak için, hadi AWS altyapısına bir göz atalım.

![djuZgN1.md.png](https://iili.io/djuZgN1.md.png)

Bir VPC, AWS Cloud'unun içinde yer alır ve esasen AWS Cloud'unun kendisinin izole edilmiş kendi segmentinizdir.

Varsayılan olarak VPC'nizi oluşturduğunuzda bu VPC'ye erişimi olan tek kişi kendi AWS hesabınızdır, yani yalnızca sizsiniz. Tamamen izole edilmiştir ve kendi AWS hesabınız dışında kimse VPC'nize erişim sağlayamaz. Tabi ki, dünya genelinde diğer müşteriler tarafından oluşturulan ve AWS ağı içinde konuşlanan milyonlarca başka VPC bulunmaktadır . Yani temelde, milyonlarca müşteri VPC'si var. Ancak, onların sizin VPC'nize erişimi yok ve aynı şekilde sizin de onların VPC'sine erişiminiz yoktur.

Peki, bir VPC'yi ne için kullanırsınız? Esasen, VPC'niz içinde kaynakları dağıtmaya başlamanıza olanak tanır. Örneğin, farklı compute kaynakları, depolama, veritabanı veya diğer ağ altyapısı servislerini dağıtabilirsiniz. Bulut içinde çözümlerinizi oluşturmaya ve dağıtmaya başlamanıza olanak tanır.

Varsayılan olarak, bir sınırlama perspektifinden baktığımızda; AWS, bölge başına en fazla **beş** VPC'ye izin verir ve bir VPC oluşturmak çok basittir. Tek yapmanız gereken, VPC'nizi oluştururken ona bir ad vermek ve ayrıca VPC'nin kullanabileceği bir IP adres aralığı tanımlamaktır. Bu IP aralığı, Classless Inter-Domain Routing (CIDR) bloğu  (Sınıfsız Alan-Arası Yönlendirme) şeklinde yapılır. Diğer başlıklar altında, alt ağlar (subnet) konusunu daha fazla detaylandırdığımızda CIDR hakkında daha fazla bilgi sahibi olacağız.

Basitçe söylemek gerekirse, bir VPC, AWS genel bulutunun izole edilmiş bir segmentidir. Kaynakları güvenli bir şekilde oluşturmanızı ve dağıtmanıza olanak tanır.


### Alt Ağlar (Subnets)

VPC'nin ne olduğunu öğrendikten sonra, şimdi subnet'lere bir göz atalım. Subnet'ler VPC'nizin içinde yer alır ve VPC altyapınızı birden fazla farklı ağa bölmenize olanak tanır. Bunu, kaynaklarınızı daha iyi yönetmek, belirli kaynakları diğerlerinden izole etmek veya altyapınızda yüksek kullanılabilirlik ve dayanıklılık oluşturmak için kullanmak isteyebilirsiniz. O halde subnet'lere daha yakından bakalım.

VPC'ler hakkında konuşurken bahsettiğim gibi, VPC'nizi oluştururken iki bilgiye ihtiyacınız var. Ona bir isim ve bir CIDR block adresi vermeniz gerekiyor. CIDR block adresi, bir IP adresleri aralığıdır ve bu CIDR block aralığı /16'dan /28'e kadar bir subnet mask'e sahip olabilir.  Örneğimiz için, VPC'mizi 10.0.0.0/16 CIDR block'u ile oluşturduğumuzu varsayalım. Bu önemlidir çünkü VPC'miz içinde oluşturduğumuz herhangi bir subnet bu CIDR block aralığı içinde yer almalıdır. Şimdi birkaç subnet'e bakalım.

![djvKDSs.md.png](https://iili.io/djvKDSs.md.png)

Subnet tanımlarken iki subnet bulunur: private ve public. Görselde, sarı olan public subnet'imiz olsun, yeşil olan da private subnet'imiz olsun. VPC oluştururken yaptığımız gibi, subnet'lere de bir CIDR block aralığı vermemiz gerekiyor. Örneğin, bu 10.0.1.0/24 olsun. Private subnet de 10.0.2.0/24 olabilir. Yine, bu CIDR block da daha büyük VPC CIDR block'u içinde yer acaktır.

Peki bir subnet'i public yapan ve bir subnet'i private yapan nedir? Temel olarak, public subnet VPC'nizin dışından, yani internet üzerinden erişilebilir. Public subnet'iniz içinde oluşturulan herhangi bir kaynak, örneğin web sunucuları, internet üzerinden erişilebilir olacaktır. Bu web sunucularının internet üzerinden erişilebilir olmasını istediğimiz için, iki IP adresine sahip olurlar. Subnet aralığında olan kendi internal IP adreslerine sahip olurlar, ki bu değer public subnet için 10.0.1.0/24'tür. Ayrıca bu kaynaklara ve hizmetlere bir de public IP adresi atayanacaktır, çünkü Internet'ten erişilebilir olması için instance'ın kendisinin bir public IP adresine sahip olması gerekir.

Private subnet'iniz içinde oluşturulan herhangi bir kaynak, örneğin backend veritabanlarınız, varsayılan olarak private olarak kabul edilir ve internet üzerinden erişilemez. Peki bir subnet'i nasıl public ve nasıl private yaparsınız? Bir subnet oluşturduğunuzda, ikisini de tam olarak aynı şekilde oluşturursunuz. Bir subnet'in public mi yoksa private mı olacağını belirleyen şey, daha sonra yapacağınız konfigürasyondur.

Bir subnet'i public yapmak için altyapınızda iki değişiklik yapmanız gerekir:

İlki bir **Internet gateway** eklemektir. Internet gateway, VPC'nize bağlanan ve VPC'niz ile dış dünya arasında bir geçit görevi gören AWS tarafından yönetilen bir bileşendir. Yani temelde internettir denilebilir. Bu internet gateway daha sonra Internet'e bağlanır. Artık izole VPC'miz ile Internet arasında, AWS tarafından yönetilen Internet gateway aracılığıyla bir köprümüz var.

Şimdi, bir Internet Gateway olduğu için public subnet'in artık internete erişimi olduğunu düşünebilirsiniz. Ancak, public subnet Internet'e erişebilmeden önce, public subnet'in route table'ına bir route eklememiz gerekiyor. Oluşturulduğunda, her subnet ile ilişkilendirilmiş bir route table olacaktır. Aynı route table'ı birden fazla subnet ile ilişkilendirebilirsiniz. Bu bir sorun değil. Bununla beraber, tek bir subnet'e birden fazla route table ilişkilendiremezsiniz.

Varsayılan olarak, subnet'iniz oluşturulduğunda içinde bir default route olacaktır ve bu bir local route'tur. Route table'ınız bir destination alanı ve bir de target alanı içerecektir. Destination alanı, ulaşmaya çalıştığınız hedef adresidir. Target ise temel olarak o hedefe giden yolu belirtir. Oluşturulan her route table içinde, target değeri *local* route olacaktır. Bu, subnet'lerinizin birbiriyle konuşmasını sağlar. Yani VPC'nizdeki herhangi bir subnet, herhangi bir route yapılandırmanıza gerek kalmadan birbirleriyle iletişim kurabilir. Her route table'da bir local route vardır. Silinemez ve sadece VPC'nizdeki tüm subnet'lerin birbiriyle iletişim kurmasına izin verir.

Yapmamız gereken şey, public subnet ile ilişkili olan bu route table'a bir route eklemektir. Route table'a eklenen bu yeni route'un destination'ı 0.0.0.0/0'dır. Bu, temel olarak route table'da zaten bilinmeyen herhangi bir IP adresi için, onu bu target'a gönder anlamına gelir. Target alanında ise oluşturulan Internet gateway gösteren değer referansı bulunmalıdır. Bu değer referansı Internet gateway'in ID'sidir. Route table'a bu route'u ekleyerek, bu public subnet artık route table'da gösterildiği gibi Internet gateway üzerinden internete nasıl ulaşacağını tam olarak biliyor.

Özetle bu iki bileşen temelde bir subnet'i public yapan şeydir: VPC'ye bağlı bir Internet gateway'in olması ve bu subnet'in nereden geldiğini/nasıl ulaşacağını bilmediği herhangi bir trafik (0.0.0.0/0) için Internet gateway'e işaret eden bir route'a sahip olması.

Şimdi, private subnet'in route table'ını karşılaştırırsak, private subnet sadece local route'a sahip olduğunu görebiliriz. Yani Internet gateway'e giden bir route'u yoktur. Bir Internet gateway'in varlığından bile haberdar değildir, bu yüzden public internete çıkış yolu yoktur. Bu nedenle bu bir private subnet olarak kabul edilir. Şimdi, public subnet'e geri dönelim, IGW'yi içeren route'un olmadığı bir senaryoda, subnet private bir subnet'tir. Çünkü Internet gateway'e giden bir route'u yoktur. Bunu akılda tutalım her subnet oluşturduğunuzda, başlangıçta private bir subnet'tir ve VPC'nize bir Internet gateway ekleyip bu ek route'u ekleyene kadar da öyle kalır.

![djvTFf4.md.png](https://iili.io/djvTFf4.md.png)

Artık public subnet'lere ve private subnet'leri tanıdığımıza göre, şimdi yüksek kullanılabilirlik ve dayanıklılık için VPC'niz genelinde birden fazla subnet'i tasarlamaya bakalım. Bu sefer VPC altında, üç subnet'imiz olduğunu düşünelim. Bir public subnet'imiz ve iki private subnet'imiz olacak. Public subnet'imiz, web katmanımız olsun. Diğer iki private subnet'imiz ise sırasıyla web katmanımız ve bu da veritabanı katmanımız olsun.

Public subnet'imizde bazı web sunucularımız olacaktır. Uygulama katmanımızda sadece bazı EC2 instance'larımız olacaktır. Ve veritabanı katmanımızda sadece bazı veritabanlarımız olacaktır. İşte dağıtımımızın üç katmanı burada. Bildiğimiz gibi, her subnet'te bir local route vardı. Bu yüzden bunların her birinde local route tanımlaması olacaktır, çünkü local route'u kaldıramazsınız. Bu, tüm bu subnet'lerin birbiriyle iletişim kurmasını sağlar. Bildiğimiz gibi, public subnet'in ayrıca Internet Gateway'e giden bir route'u olmalıdır.

Örneğin, bu subnet'i oluşturduğumuzda, onu availability zone bir'de oluşturduk diyelim ve geri kalan subnet'lerimiz için de aynısını yaptık ve hepsini aynı availability zone'a yerleştirdik. Bu tamamen normal, altyapımızı aynı availability zone içinde dağıtabiliriz ve çözümümüz iyi çalışıyor olurdu. Ancak, AWS'nin bu bölgedeki availability zone ile bir sorunu olursa, örneğin bir sel, yangın veya doğal afet yaşanırsa ve availability zone bir'deki hizmetleri devre dışı bırakırsa, kaynaklarımıza ne olacak?

Elbette, bunlar da devre dışı kalırdı çünkü hepsi availability zone 1'de çalışıyor. Bu ideal değil senaryo değildir. Tüm kaynaklarınızı tek bir bölge içindeki aynı availability zone'a dağıtmak best practice değildir, çünkü yüksek kullanılabilirlik ve dayanıklılık sunmaz. Peki bu durumda ne yapmalısınız? Yüksek kullanılabilirliği sağlamak için yapılacak en iyi şey, dayanıklılık için ek subnet'ler eklemektir. Bu demek oluyor ki; ek bir web katmanı, ek bir uygulama ve ek bir veritabanı ekleyeceğiz.

Bu kapsamda altı subnet'imiz oluşmuş olacaktır. Şimdi, bu sefer altyapımızı dağıtacağımız availability zone'lara bakalım.

![djv5Fj9.md.png](https://iili.io/djv5Fj9.md.png)

Örneğin, public subnet'i AZ-1 ve private subnet'leri sırasıyla AZ-2 ve AZ-3'de konumlandırdığımızı düşünelim. Benzer şekilde aynı işlemi az önce bahsettiğimiz ve subet sayısını altıya çıkaran üç subnet için de yapmamız gerekcektir. Örneğin, public subnet'i AZ-3'te, private subnet'leri ise sırasıyla AZ-1 ve AZ-2'de konuşlandıralım. Şimdi senaryoyu tekrar gözden geçirelim. AZ-1'in bir arıza yaşadığını hayal edelim. Bu durumda ne olurdu? Bir public subnet devre dışı kalırdı. Bununla beraber uygulama katmanının bulunduğu private subnet de erişilemez olacaktır. Bu durumda, altyapımızın her katmanında hala bir subnet'imiz mevcut olarak çalışmaya devam edecektir. Yani availability zone bir'de bir arıza yaşarsak, hizmetlerimiz çalışmaya devam edecek.

Subnet'inizi oluşturduğunuzda, VPC CIDR block'una uyan bir CIDR block aralığı atamanız gerektiğini görmüştük. Örneğin, VPC altında bir subnet oluşturduğunuzu ve subnet'e 10.0.1.0/24 adresini verdiğinizi düşünelim. Atanan, /24 mask ile, bu subnet'e toplam 256 IP adresi verir. Aslında sadece 251 IP adresini kullanabilirsiniz. Nedenini hemen açıklayalım. Bu subnet'teki ilk IP adresi 10.0.1.0'dır ve bu **ağ adresi** olarak bilinir. Bunu host adreslerinize atamak için bir IP adresi olarak kullanamazsınız. Bu ağ için ayrılmıştır. Ağ adresinden sonraki ilk kullanılabilir IP adresi **10.0.1.1**'dir. Bu değer ise **AWS yönlendirmesi** için ayrılmıştır.  Bu adresi subnet'inizde bir host ağı olarak kullanamazsınız. Şimdi, bir sonraki kullanılabilir IP adresi **10.0.1.2**'dir. Ve yine, bu **AWS tarafından DNS için ayrılmıştır**. Yani bu IP adresini kullanamazsınız. Bu subnet'te kullanamayacağınız dördüncü IP adresi **10.0.1.3**'tür. Bu değer ise, temelde **AWS tarafından gelecekteki kullanım** için ayrılmıştır. Şimdi, bir AWS subnet'inde kullanamayacağınız beşinci ve son adresin ne olduğuna bakalım. Subnet'e atanan mask değerine göre **son kullanılabilir** adrestir. Bu durumda (10.0.1.0/24 için)  **10.0.1.255** olur. Herhangi bir subnet'teki son adres **yayın adresi (broadcast address)** olarak bilinir ve elbette yine, bunu host kaynakları için kullanamazsınız.

Yani subnet'inizdeki TCP/IP adresleriyle çalışırken, herhangi bir subnet'teki ilk dört adres ayrılmıştır ve host adresleri için kullanamazsınız. Bununla beraber en son adres de ayrılmıştır. Bu yüzden bu CIDR block'u örneği kapsamında  host kaynaklarınıza atamak için kullanabileceğiniz sadece 251 IP adresi vardır.

### Ağ Erişim Kontrol Listeleri (NACLs: Network Access Control Lists)

AWS içinde güvenlik, herhangi bir dağıtımın kilit bir parçasıdır ve sanal özel bulutunuz (VPC) etrafındaki güvenliği yönetmek de temelde farklı değildir.

NACL'ler yani ağ erişim kontrol listelerine bakalım. NACL'ler esasen her bir subnet'e ilişkilendirilmiş sanal ağ düzeyinde güvenlik duvarlarıdır. VPC'niz içinde ve dışında ve subnet'leriniz arasında hareket eden hem giriş hem de çıkış trafiğini kontrol etmeye yardımcı olur.

![dOEIVUB.md.png](https://iili.io/dOEIVUB.md.png)

Yukarıda gördüğünüz VPC'de bulunan bileşenlere bir göz gezdirelim. İlk olarak bir public subnet bulunuyor. VPC'mize bağlı internet gateway'imiz var ve tabi ki gateway'e giden bir route tanımlı ki bu da internet ile iletişim kuruyor.

Güvenliği sağlamaya yardımcı olmak için burada yapabileceğimiz ilk şey, bu subnet'e ilişkilendirilmiş network access control list'i yapılandırmaktır. Route table'lar gibi, yeni bir subnet oluşturduğunuzda, aynı zamanda bir network access control list de ilişkilendirecektir. Varsayılan olarak, bir NACL hem gelen (inbound) hem de giden (outbound) tüm trafiğe izin verecektir. Bu yüzden varsayılan hali çok güvenli değildir. Bu nedenle NACL'lerinizi yalnızca subnet'inize girmesini ve çıkmasını istediğiniz trafiğe izin verecek şekilde yapılandırmak güvenliği sağlamak açısından iyi bir uygulamadır.

Bu bir public subnet olduğundan, burada HTTP ve HTTPS üzerinden konuşan bazı web sunucularımız olabilir. Bu yüzden bu subnet'e ilişkilendirilebilecek gelen network access control list'e bakalım.

![dOERLpS.md.png](https://iili.io/dOERLpS.md.png)

Örneğin yukarıdaki örnek listeye bakarsak, bir dizi farklı alan olduğunu görebiliriz. Rule number, type, protocol, port range, source ve allow / deny alanlarımız var. Rule number'lar, kuralların NACL içinde hangi sırayla görüneceğini belirlemenize olanak tanır ve trafik bu kurallardan birine ulaştığında, tüm type, protocol, port range ve source vb. eşleşirse, sonundaki eylemi gerçekleştirecektir.

Şimdi bu kuralla eşleşmesi gereken gereksinimlere bakalım. Rule number değeri 10 olan kayda bakalım. Trafik türü, TCP protokolünü kullanan 80 numaralı port altında HTTP olmalıdır. Port aralığı ise HTTP için kullanılan 80'dir. Kaynak herhangi bir IP adresi olabilir, yani subnet'imize gelen HTTP çalıştıran herhangi bir IP adresine izin verilecektir. Dolayısıyla, bu protokolü çalıştırdıkları sürece, trafik public subnet'imize girmesine izin verilecektir.

Şimdi ikinci kurala bakalım. İkinci kural, TCP protokolünü kullanarak 443 numaralı portu kullanan HTTPS'yi kullanıyor ve yine, herhangi bir kaynak ve eylem izin verilmek üzere tanımlanmış.

Son kural daha özel bir kural tanımlamasıdır. Bu her network access control list'in sonunda uygulanan varsayılan bir kuraldır. Bu yüzden bir kural numarası yoktur ve herhangi bir protokol kullanan tüm trafiğin herhangi bir port aralığında herhangi bir IP adresinden geldiğini belirtir, ardından bu erişimi reddeder. Yani bu kural bir tür kapsayıcı kuraldır. Temel olarak, bu kuralın üzerinde tanımladığınız kurallara uymayan herhangi bir trafiğin silinmesini ve subnet'inize erişiminin reddedilmesini sağlamanıza olanak tanır.

Bunu göz önünde bulundurarak, public subnet'imize izin verilen tek trafik esasen HTTP ve HTTPS'dir, ki bu da web sunucularımız için tam olarak istediğimiz şeydir ve diğer tüm trafik reddedilecektir.

Şimdi bir de giden (outbound) trafiğe daha yakından bakalım bakalım. Alan türleri, bir değer hariç hepsi inbound rules ile  tamamen aynıdır. Sadece outbound kuralların bir hedefi varken inbound trafiğin ise kaynağı vardı. Yani giden trafikte, trafiği hedefine göre kısıtlamamıza izin verilmektedir.

![dOE01cu.md.png](https://iili.io/dOE01cu.md.png)

Burada sahip olduğumuz ilk kural, herhangi bir protokol kullanan herhangi bir trafiğin herhangi bir port aralığında herhangi bir hedefe giderse, o trafiğe izin ver diyor. Başka her şey reddedilmelidir, ancak bu durumda başka bir şey olmayacaktır çünkü bu giden kural esasen herhangi bir protokolü kullanarak istediğiniz herhangi bir trafiği bu subnet'ten herhangi bir hedefe gönderimine izin vermek üzerine tanımlı.

NACL'ler hakkında belirtilmesi gereken önemli bir nokta şudur. NACL'ler durumsuz yani stateless'dır. Bu, bir istekten oluşturulan herhangi bir yanıt trafiğinin, yanıtın nereden geldiğine bağlı olarak, gelen veya giden kural setinde açıkça yapılandırılması gerektiği anlamına gelir.

Route table'lar da gördüğümüz gibi, aynı NACL'yi birçok subnet'e uygulayabilirsiniz, ancak bir subnet'e yalnızca tek bir NACL ilişkilendirilebilir. Dolayısıyla, network access control list'leri belirli bir subnet'e giren ve çıkan trafiği kontrol etmenin harika bir yoludur.

NACL'ler üzerine bilgi birikimimiz olduğuna göre, sonraki başlıkta security group'lardan bahsetmeye başlayabiliriz. Security group'lar trafiği kontrol etmenin başka bir yöntemidir. Ancak bu sefer NACL'lerin yaptığı gibi ağ düzeyinde değil, instance düzeyinde çalışırlar.

### Güvenlik Grupları (Security Groups)

Güvenlik konusunda devam ederek, şimdi de security group'lara yakından bakalım. Security group'lar, hem gelen hem de giden trafiği filtreledikleri için network access controller'lara oldukça benzerler. Ancak NACL'ler subnet seviyesinde çalışırken, security group'lar instance seviyesinde çalışır.

Aşağıdaki şemayı göz önüne alarak, diyelim ki üç private subnet'imiz bulunuyor. Her birinin IP adresleri olmalıdır, ilki için 10.0.1.0/24 tanımlayalım. Sırasıyla iki ve üçüncü subnet için değerlerimiz ise 10.0.2.0/24 ve 10.0.3.0/24 olsun. Sonrasında senaryomuzu detaylandıralım, ilk subnet'te EC2 instance'ları olsun. İkinci subnet'te MySQL veya Aurora çalıştıran RDS instance'ları olabilir ve son subnet'te de yine EC2 instance'larının olduğu bir senaryo düşünelim.

![dOEvciu.md.png](https://iili.io/dOEvciu.md.png)

Bu üç subnet'in her biri aynı network access control list ile ilişkilendirilmiş durumdadır. Network access control list ise aşağıdaki görseldeki gibi olduğunu düşünelim. 

![dOESKCX.md.png](https://iili.io/dOESKCX.md.png)

Bu NACL basitçe şu tanımlamayı içerir: herhangi bir port aralığında TCP Protokolü kullanan, herhangi bir kaynaktan gelen trafiğe izin ver ve diğer tüm trafiği reddet. Yani bu subnet'ler arasında, herhangi bir port üzerinde herhangi bir TCP Protokolü kullanılabilir ve basitlik açısından, aynı NACL kuralları hem gelen hem de giden trafik için kullanılmış.

Şimdi yapmak istediğimizin, hangi instance'ların buradaki RDS ve Aurora veritabanlarımızla konuşabileceğine erişimi kısıtlamak olduğunu düşünelim. Sadece birinci subnet'ten erişime izin vermek ve üçüncü subnet'ten erişimi reddetmek isteyebiliriz. Bu amaca ulaşmak için, security group'ları kullanabiliriz. Öyleyse, veritabanlarımızın bulunduğu subnet için security group'a bir göz atalım.

![dOEgCml.md.png](https://iili.io/dOEgCml.md.png)

Security group'lar NACL'lere benzer alanlar içerir, sadece birkaç tane daha az alanı bulunur. Security group'ta kural numarası bulunmaz. Bu durumda da security group içindeki tüm kuralların, eylem hakkında bir karar verilmeden önce değerlendirileceği anlamına gelir. Bununla beraber, izin ver (allow) veya reddet (deny) seçeneği olmadığını da fark edebilirsiniz. Security group'larda, eğer bir kural tanımlı ise bunun izin verildiği (Allow) anlamına gelir. Rğer kural yoksa, varsayılan olarak tüm trafik düşürülür. Şidmi yukarıdaki security group'a bakalım. 3306 portunda TCP Protokolü kullanan herhangi bir MySQL veya Aurora trafiği, 10.0.1.0 kaynağından geliyorsa (ki bu değer ilk subnet için tanımlı olan değerdir), izin verildiği kabul edilir. Bu security group'ta 10.0.3.0/24 kaynağı için (ki bu senaryoda üçüncü subnet'i temsil eder) başka bir kuralımız olmadığından, reddedildiği kabul edilir. 

NACL'ler subnet ve ağ katmanı için kullanılırken, security group'lar instance katmanında kullanılır.  Security group'lar hakkında bilinmesi gereken son bir şey var: Tasarım gereği stateless olan NACL'lerin aksine, security group'lar stateful'dur. Bu  durum da NACL'lerde yapmak zorunda olduğunuz gibi, isteklerden gelen dönüş trafiğine izin vermek için özel kurallar yapılandırmanız gerekmediği anlamına gelir.

### NAT Gateway

Sırada başka bir VPC bileşeninden, NAT Gateway'e göz gezidirelim. Aşağıdaki görseli inceleyelim, çok basit bir VPC'miz var ve bu VPC'de iki subnet bulunuyorç. Bu subnet'lerden bir public subnet ve bir de private subnet olarak tanımlanmış. Bir de elbette, VPC'mize bağlı bir Internet gateway olacak, IGW de ağımızın internete bağlanağı köprü görevini görecektir. 

![dOrZHPa.md.png](https://iili.io/dOrZHPa.md.png)

Senaryomuzu düşünelim, private subnet'imizde uygulamalarımızı çalıştıran bir dizi EC2 instance'ımız olduğunu düşünelim. Bununla beraber, public subnet'imizde de bir dizi web sunucumuzu konuşlandıralım. Bildiğimiz gibi, bu subnet'lerin her birine tanımlanmış route table bulunuyor. Public route table, Internet gateway'e ve diğer private subnet'e erişime sahip olacaktır.

Şimdi yine güvenlik hakkında düşünmeye başlayalım. Private subnet'teki EC2 instance'larımıza baktığımızda, AWS Shared Responsibility Model'in bir parçası olarak, her bir EC2 instance'ımızda çalışan işletim sistemlerini güncellemek bizim sorumluluğumuzdadır. AWS Shared Responsibility Model, tüm AWS dağıtımlarınız için kritik önem taşır ve esasen bulut içinde güvenliği uygulamadaki rollerinizin ve sorumluluklarınızın neler olduğunu, AWS'nin bulutun güvenliğini sağlama sorumluluğunun ne olduğunu tanımlar.

Tamam, bu konu cepte; devam edelim. EC2 instance'larımızın işletim sistemlerini sürdürme sorumluluğumuz varsa, gerektiğinde güncellemeleri indirebilmemiz gerekir. Ancak bu subnet private olarak tanımlı. Yani Internet gateway'e ve dolayısıyla internete erişimi bulunmuyor. O halde bu güncellemeleri nasıl indirebiliriz? Yapabileceğimiz en basit şey, bir NAT gateway eklemek olacaktır.

NAT gateway, public subnet içinde yer alır. Public subnet içinde yer aldığı için, bir EIP (Elastic IP address) şeklinde public bir IP adresine sahip olması gerekir. Bu IP adresi, instance'ın kendisine atanır. Public subnet içinde yer aldığı için, Internet gateway'e ve haliyle internete bir route kaydı vardır. NAT Gateway'imizi kurup yapılandırdıktan sonra, private subnet'imizin route table'ını güncellemamiz gerekir. Varsayılan olarak, private subnet'imizdeki route table'da sadece tüm route table'ların sahip olduğu local route'un tanımlı olacağını görmüştük. Ancak bunu NAT Gateway'e bir route sağlayacak şekilde güncellersek, istediğimize ulaşabiliriz. Eklenecek olan route kaydı, Internet gateway üzerinden internete erişim sağlamak için public subnet'e eklediğimiz route kaydına oldukça benzer hatta aynısıdır. Yani 0.0.0.0/0'ı CIDR kaydı ekleyeceğiz, bu temelde route table'da herhangi bir IP adresine yönelik bir hedefi tanımlamak için kullanılır. Sonra, route kaydının hedef alanını NAT gateway olarak belirleriz.

![dO6x1Pn.md.png](https://iili.io/dO6x1Pn.md.png)

Kaydı oluşturduktan sonra bu kayıt temelde şunu açıklar: Eğer bu subnet içindeki herhangi bir kaynak bir güncelleme yapmak için internete erişmesi gerekiyorsa, bunu buradaki NAT üzerinden yapabilir. Bu NAT gateway daha sonra isteği alacak, Internet gateway üzerinden gidecek ve gerekli olan uygun yazılımı indirecektir ve sonrasında bu isteği yollayan EC2 instance'ına geri gönderecektir. NAT gateway ile ilgili önemli bir konu ise, internetten gelen hiçbir iletişimi kabul etmeyecek olmasıdır. Sadece VPC'nizin içinde oluşturulan giden (outbound) iletişimleri kabul edecektir. Yani internetten instance'a gelen tüm trafiği reddedecektir.

NAT Gateway'in kendisi AWS tarafından yönetilir, yani NAT Gayewat için instance sağlamanız gerekmez. Uygulamak çok kolaydır, sadece NAT Gateway'i oluşturursunuz, hangi subnet'te bulunması gerektiğini belirtirsiniz ve bir Elastic IP adresi ilişkilendirirsiniz. Sonrasındaki tüm yapılandırmayı, AWS yönetir. Varsayılan olarak yönetildiği için, AWS dayanıklılık için birden fazla NAT gateway kuracaktır, ancak hesabınızda yalnızca ilişkili ID'ye sahip tek bir NAT gateway göreceksiniz.

Daha önce kaynaklarınızı Multi-Availability Zone'lar genelinde yapılandırmaktan bahsetmiştik. Yani farklı Availability Zone'larda birden fazla public subnet'iniz varsa, o subnet'te de başka bir NAT gateway kurmanız gerekecektir. **AWS, her bir public subnet'inize otomatik olarak bir NAT gateway dağıtmayacaktır.**

Kısaca özetlemek gerekirse, NAT Gateway, private subnet içindeki instance'ların internete erişmesine izin verir, ancak NAT Gateway'in kendisi internetten gelen istekleri ve iletişimi engelleyecektir. Böylece private subnet'i bu şekilde korumaya devam eder. EC2 instance'larınızın güvenliğini sürdürmenizi, işletim sistemlerinin güncel tutulmasını ve patch yönetiminin de halledilmesini sağlamanıza olanak tanır.

## Amazon CloudFront

Bu başlık altında, Amazon CloudFront'un amacını ve temel özelliklerini inceleyeceğiz. CloudFront'un ana rolü içerik önbelleğe almadır. Önbelleğe alma (caching), içeriğimizi ona ihtiyaç duyan kullanıcılara daha yakın depolamamıza olanak tanır. Örneğin, EU-West-2 bölgesinde barındırılan bir web siteniz var, ancak müşterilerinizin çoğu ABD veya Avustralya'daysa, İngiltere'deki kullanıcılara kıyasla daha yüksek gecikmeye (latency) sahip olacaklardır. Ancak içeriği ABD ve Avustralya'daki edge location'larda onlara daha yakın bölgelerde önbelleğe alırsak, gecikmeleri azalacaktır. Amazon CloudFront, müşterilerin düşük gecikme ve yüksek hızla içerik dağıtmasına olanak tanır. Amazon CloudFront, kullandıkça öde hizmetidir. CloudFront kullanırken, dosyalar son kullanıcılara global bir edge location ağı üzerinden teslim edilir.

CloudFront hem statik hem de dinamik içerikle çalışır. Örneğin, Amazon S3 Bucket'larında depolanan statik içerikleri depolar, dosyaların kesin sürümünü tutar. Amazon EC2'de depolanan veya Lambda fonksiyonları kullanılarak sunulan dinamik içerikler ile de çalışır. Bu dinamik içerikler, compute kaynağında oluşturulur ve Amazon CloudFront aracılığıyla dağıtılır. CloudFront ile çalışırken, önce bir CloudFront dağıtımı oluşturursunuz. Bu süreç sırasında, bu dağıtımın istemcilere sunacağı içerik için bir veya daha fazla kaynak belirlersiniz. Ayrıca HTTP veya HTTPS gibi kullanılabilecek protokolleri kontrol eden seçenekleri, önbellek yaşam sürelerini, özel başlıkları, bir fiyat sınıfını (tüm edge location'ları mı yoksa location'ların bir alt kümesini mi kullanacağı), AWS WAF web ACL ilişkilendirmelerini, alternatif alan adlarını, özel SSL sertifikalarını ve daha fazlasını yapılandırırsınız. Bir CloudFront dağıtımı oluştururken, size bir alan adı (domain name) atanır.

Örneğin, 1234.cloudfront.net gibi bir alan adı olabilir. Bu alan adını kullanabilseniz de, Amazon CloudFront müşterilerinin çoğu dağıtımlarına alternatif bir alan adı ekleyecektir. fatihes.com gibi bir ad, DNS kullanılarak CloudFront'un atadığı ada eşlenir. 

Şimdi senaryomuzu düşünelim, web sitemize trafiği dengeleyen bir Internet Application Load Balancer'ımız zaten var. Web site içeriğimizi global olarak önbelleğe almak için bir CloudFront dağıtımı oluşturacağız. CloudFront dağıtım merkezi kontrol paneline geçelim. Bir dağıtım oluşturmak için Create distribution'ı seçeriz. Ardından kaynak domain'imizi seçiyoruz. Kaynak bir AWS kaynağı olabilir veya sadece CloudFront'un önbelleğe almasını istediğimiz kaynağın alan adını yazarız. 'Choose origin' domain kutusuna tıklarsak, S3 bucket'ları ve Application Load Balancer'ımız dahil geçerli ADS kaynaklarının bir listesini görebiliriz.

Senaryomuz gereği, Application Load Balancer'ı seçeriz. Load balancer seçildiğinde, dağıtımın load balancer'a bağlanırken kullanacağı protokol bilgisini ve port bilgisini seçme şansınız olur. Biraz aşağı kaydırırsanız, bir kaynak adı seçme şansımız bulunmaktadır. Kaynak için varsayılan adı kabul edebilir veya bizim için daha anlamlı olan bir ad seçebiliriz. Biraz daha aşağı kaydırırsak, önbellek davranışını yapılandırmamıza izin veren ayarları bulabiliriz. Biraz daha aşağı kaydırırsak, fiyatlandırma sınıfını buluruz. Fiyatlandırma sınıfı, dağıtımımızın içeriği önbelleğe almak için kullanacağı edge location gruplarını seçmemize olanak tanır. WAF ACL'mizi dağıtımımızla ilişkilendirmeyi seçebilir ve dağıtımımız için alternatif bir alan adı seçebiliriz.

Çoğu dağıtım, CloudFront DNS adına güvenmek zorunda kalmak yerine kendi güzel, kullanıcı dostu DNS adlarımızı kullanabilmemiz için alternatif bir alan adı kullanacaktır. Alternatif bir alan adı kullanmayı seçerseniz, ayrıca bir dijital sertifika seçmeniz gerekecektir. Dijital sertifika (Custom SSL certificate) alanında ise  kendi sertifika depolarınızdan içe aktarılabilir veya Amazon certificate manager'dan sertifika arayabilirisiniz. Biraz daha aşağı kaydırırsak, görüntüleyici isteklerini bir S3 Bucket'ına kaydedebilmemiz için standart log kayıt dosyası dağıtımını etkinleştirebiliriz. Ayrıca IPv6 desteğini açıp kapatabilirsiniz. Seçimlerimizden memnunsanız, Create distribution'ı seçin. Dağıtımı seçtikten sonra, dağıtımınızın dağıtılmakta olduğunu söyleyen bir mesaj görmelisiniz.

CloudFront'u genellikle tek bir önbellek olarak tartışsak da, aslında CloudFront'un üç önbellek katmanı vardır. Cloudfront dağıtımları,  global olarak 300'den fazla **Amazon Edge Location**'da bulunur. **Regional Edge Cache**'ler ve yazıldığı sırada 13 regional edge cache vardır. **AWS Origin Shield**, regional edge cache'leriniz ile kaynaklar arasında ek bir önbellek katmanı olarak düşünülebilir. Origin shield varsayılan olarak etkinleştirilmemiştir. Oluşturduğunuz dağıtımlardaki her kaynak için bunu etkinleştirmeniz gerekir. Birden fazla önbellek katmanına sahip olarak, daha fazla içeriği daha uzun süre önbelleğe alabilirsiniz. Regional cache'leri ve origin shield'ı kullanarak, daha iyi önbellekleme deneyimi elde edersiniz. Daha fazla içeriğiniz önbelleğe alındığı için, müşterilerinizin ihtiyaç duyduğu içeriğin önbellekten alınma şansı çok daha yüksektir. 

- **Azaltılmış kaynak yükü (Reduced origin load):** Daha fazla içerik önbellekten sunulduğundan, kaynaklara daha az istek gönderilir. Origin shield kullanırken, önbellekte olmayan aynı nesne için yapılan istekler birleştirilir, böylece kaynağa yalnızca tek bir istek gönderilir. 
- **Daha iyi ağ performansı (Better network performance):** Birden fazla katman kullanarak, içerik AWS Lola into ağında daha uzun süre kalabilir. 

CloudFront'un uzun bir güvenlik özellikleri listesi vardır. Bunlar arasında;
-  CloudFront'un, verilerinizi beklerken koruyan şifrelenmiş SSD'leri kullanması, 
- Belirli kullanıcılar için tasarlanmış içeriğe erişimi kısıtlamak için signed URL'ler ve çerezler (cookies) kullanılabilmesi. 
- İçeriğe erişimi kısıtlamak için web ACL'leri oluşturmak üzere AWS WAF'ı kullanabiliriz ve belirli bölgelerdeki kullanıcıların içeriğe erişmesini engellemek için coğrafi kısıtlamalar kullanabiliriz.

 Amazon CloudFront'un kendisi, CloudFront'a yönetici erişimini kontrol etmek için kullanabileceğimiz identity and access management ile entegre olur. CloudFront listelenen AWS servisleriyle entegrasyon yoluyla izlenebilir: 
 - Amazon CloudWatch alarmları, 
 - AWS CloudTrail Logs,
 - CloudFront gerçek zamanlı IAM standart logları.

## Amazon Route 53

Bu başlık altında, Amazon Route 53 ile tanışacak ve bu hizmetin bir domain adı kaydetmenize ve dünya çapında yönetmenize nasıl yardımcı olduğunu göreceğiz. Route 53, **domain adı** kaydı ve **Domain Name System (DNS) yönetimi** sağlar. Route 53 ayrıca, domain'iniz için kaynaklara internet **trafiğini yönlendirerek** ve **sağlık kontrolleri** kullanarak kaynaklarınızın beklendiği gibi çalıştığını doğrulayarak trafik yönetimini sağlar.

Amazon Route 53, AWS global altyapısındaki edge location'ları kullanır ve global bir hizmettir. Route 53 kaynaklarını yapılandırırken bir bölge belirtmeniz gerekmez. AWS, Route 53 için %100 kullanılabilir bir SLA sunar. Bu, DNS sisteminin dağıtık doğası ve AWS uygulamasının yüksek yedekliliği nedeniyledir.

**Routing policy**'leri, domain'inizdeki kaynaklara internet trafiğinin nasıl yönlendirileceğini tanımlar. Failover routing ve latency routing dahil olmak üzere çeşitli routing policy'leri arasından seçim yapabilirsiniz. Route 53'ün **Traffic flow** özelliği, kaynaklarınız için bir veya daha fazla routing policy'sini birleştirerek karmaşık routing konfigürasyonları oluşturmanıza olanak tanır.

Route 53'ün **application recovery controller** özelliği, sağlık kontrolleri ve uygulama bileşeni doğrulaması ile entegre routing kullanarak failover'ları yönetmenize olanak tanır.

**Amazon Route 53 Resolver** hizmeti VPC'ler içindir ve veri merkezinizdeki DNS ile kolayca entegre olur. Temel olarak, VPC'lere giren ve çıkan DNS sorguları için endpoint'ler yapılandırmanıza olanak sağlar.

Son olarak, **Route 53 Resolver DNS Firewall**, VPC'nizde başlayan DNS sorguları için yönetilen bir hizmettir. Belirli DNS sorgularına izin veren veya engelleyen kural grupları oluşturabilirsiniz.

Kısacası, Amazon Route 53 ile kayıt ve ad çözümlemenin ötesine geçen, trafiğin global olarak nasıl yönlendirileceğini kontrol etmenize olanak tanıyan özelliklere sahip bir domain adı yönetim hizmeti elde edersiniz.

### Amazon Route 53 ve DNS Kayıtları (DNS Records)

Amazon Route 53, AWS tarafından sağlanan domain adı yönetim hizmetidir. Domain Name System yani DNS, interneti her kullandığımızda domain adlarını IP adreslerine çevirmekten sorumludur. Bunu, tıpkı bir kişiden aranacak gerçek bir numaraya çeviren bir telefon rehberi gibi düşünebilirsiniz. Bu nedenle temelde DNS, interneti bir arada tutan temel yapının bir parçasıdır.

Amazon Route 53'ü bir domain kaydetmek için kullandığınızda, hizmet domain için yetkili DNS sunucusu haline gelir ve public bir hosted zone oluşturur. Public zone, trafiğin public internet üzerinde nasıl yönlendirileceğini tanımlar. Bununla beraber private zone ise, trafiğin bir virtual private cloud veya VPC içinde nasıl yönlendirileceğini tanımlar. Private Zone'lar ile kullanılması amaçlanan VPC'lerin konfigürasyonlarında DNS Hostname ve DNS Support'un etkinleştirilmiş olması gerekir.

Private ve Public Hosted Zone'lar kayıtlardan oluşur. Çeşitli kayıt tipleri vardır. Bu iki önemli kayıt tipi, **Name Server (NS)** kayıt tipi ve **Start of Authority (SOA)** kayıt tipi olarak bilinir. Amazon Route 53, oluşturulan her hosted zone'da **4 benzersiz NS** kaydı ve **1 SOA** kaydı oluşturur.

- **Name Server (NS) kayıtları**, belirli bir hosted zone için DNS sunucularını tanımlamak için kullanılır.

- **Start of Authority (SOA) kaydı**, bireysel bir DNS zone'u için yetkili DNS sunucularını tanımlamak için kullanılır.

Bu iki kayıt, domain'inizi mevcut DNS sistemine entegre etmek için çok önemlidir. Route 53, aşağıda listelenen kayıt tipleri dahil DNS'in yaygın kayıt tiplerini destekler:

- **A kaydı**, bir hostname'i bir IP adresine eşlemek için kullanılır. A kaydı **IPv4** adresleri için kullanılır.

- **AAAA kaydı** da bir hostname'i bir IP adresine eşlemek için kullanılır. AAAA kaydı **IPv6** adresleri için kullanılır.

- **Mail exchange (MX)** kaydı, belirli bir domain için e-posta sunucularını tanımlamak için kullanılır. Birden fazla olabilir ve bir numara kullanarak önceliği ayarlayabilirsiniz. Örneğin, 10 öncelikli bir birincil e-posta sunucunuz ve 20 öncelikli bir ikincil e-posta sunucunuz olabilir. En düşük numaralı kayıt ilk olarak kullanılır.

- **Text (TXT) kaydı**, domain'inizin dışındaki sistemlere metin formatında bilgi sağlamak için kullanılır. Birçok kullanım senaryosu vardır.

- **Canonical name (CNAME)**, bir hostname'i başka bir hostname'e eşlemek için kullanılır. Bu, birden fazla adı aynı host'a eşlemek için kullanılabilir. Örneğin, bir sunucunun aynı anda WWW hostname'ini kullanarak web sunucusu ve MAIL hostname'ini kullanarak mail sunucusu olarak yanıt vermesi gerektiğinde.

DNS'in yukarıda listelenenlerden daha fazla kayıt tipini desteklediğini lütfen unutmayın. DNS kapsamı dışında olan bir kayıt tipi **Alias kayıt** tipidir.

**Alias kayıt** tipi Amazon Route 53'e özgüdür ve domain'inizdeki özel bir hostname'i genellikle dahili bir AWS adıyla temsil edilen bir AWS Resource'una eşler. Örneğin, CloudFront dağıtımları, Amazon S3 bucket'ları ve Elastic Load Balancer'lar size **AWS'ye özel bir domain adı** sağlar. Bu kaynağa özel bir ad tanımlamak için bir alias kaydı kullanabilirsiniz. Alias kayıtlarını ayrıca example.com veya fatihes.com gibi bir DNS namespace'inin en üst düğümleri olan **apex kayıtlarına** eşlemek için de kullanabilirsiniz.

Route 53 kullanarak bir kayıt oluşturduğunuzda, kayıt adını, kayıt tipini, gerçek değeri, saniye cinsinden Time-To-Live'ı ve bu kayıt için Routing policy'sini belirtirsiniz. Time to Live, kaydın geçerli kabul edildiği süreyi belirtir. Daha önce elde edilen aynı kayıt sonucu gelecekte kullanılır ve TTL süresi dolana kadar DNS'e tekrar sorgu yapılmaz.

Bir kayıt için Routing policy, bir DNS sorgusunun nasıl cevaplanacağını tanımlar. Her policy tipi, sağlık kontrollerinin olası kullanımı da dahil olmak üzere farklı bir şey yapar. 

### Amazon Route 53 Sağlık Kontrolleri (Health Checks)

Amazon Route 53 sağlık kontrolleri, bir kayıt tanımlanırken çoğu routing policy tarafından kullanılabilen bağımsız kaynaklardır. Bir sağlık kontrolü oluşturduğunuzda, Route 53 **her 30 saniyede bir endpoint'e istekler gönderir** ve yanıtlara dayanarak, Route 53 endpoint'in **Healthy** veya **UnHealthy** olduğuna karar verir. Daha sonra bu bilgiyi sorguya yanıt olarak hangi değeri sağlayacağını belirlemek için kullanır.

Ayrıca gerçek toplam uygulamanın sağlıklı kabul edilmesinden önce uygulamanızın farklı katmanlarını bağımsız olarak doğrulamanıza olanak tanıyan diğer kontroller için bir sağlık kontrolü yapılandırabilirsiniz. Amazon Route 53, sağlıklı kabul edilen sağlık kontrollerinin sayısını toplar ve bu sayıyı belirlediğiniz sağlık eşik değeriyle karşılaştırır.

Route 53 sağlık kontrolleri ile bir **CloudWatch alarmının** durumunu da izleyebilirsiniz. Alarm **OK** durumundayken sağlık kontrolü durumu sağlıklıdır. Alarm durumu **ALARM** durumundayken sağlık kontrolü durumu sağlıksızdır. Alarm **INSUFFICIENT** durumundayken sağlık kontrolü durumunun ne olacağını da seçebilirsiniz. Seçenekler healthy (sağlıklı), unhealthy (sağlıksız) veya last known status (son bilinen durum) şeklindedir.

Route 53 bir sorgu aldığında, routing policy'sine göre bir kayıt seçer. Ardından o kayıt için sağlık kontrolünün durumunu kontrol ederek seçilen kaydın mevcut sağlığını belirler ve sorguya sağlıklı bir kaydın değeriyle yanıt verir. Sağlıksız kayıtlar dikkate alınmaz. Bir kayıtla bir sağlık kontrolü **ilişkilendirmezseniz**, Route 53 bu kayıtları her zaman sağlıklı olarak kabul eder.

Sağlık kontrolü, dünya çapında bulunan bir sağlık kontrolcüsü grubu tarafından gerçekleştirilir. Bölgeye göre önerilen sağlık kontrolcülerinin listesini kullanabilir veya listeyi işletmenize özgü bölgelere göre özelleştirebilirsiniz. Sağlık kontrolleri, siz her 10 saniyede bir olarak belirtmedikçe her 30 saniyede bir gerçekleştirilir.

Endpoint sağlık kontrolleri **IP adresi** veya **domain adı** ile belirtilebilir. Sağlık kontrolü protokolü TCP, HTTP veya HTTPS olabilir. HTTP ile ilgili protokoller için, Route 53'ün yanıt gövdesinde (response body) belirtilen string aramasını belirttiğiniz isteğe bağlı bir string matching kullanabilirsiniz. Route 53, yalnızca belirtilen string yanıt gövdesinin **ilk 5120 baytı** içinde tamamen görünüyorsa endpoint'i sağlıklı kabul eder.

Son olarak, tüm sağlık kontrolleri için, başarısız olduğunda bildirim almayı seçebilirsiniz.


### Amazon Route 53 Yönlendirme Politikaları (Routing Policies)

Bir kayıt için Routing policy, bir DNS sorgusunun nasıl cevaplanacağını tanımlar. Her policy tipinin farklı bir görevi vardır.

- **Simple routing policy**, bir ad ile ilişkili IP adresini sağlar. Simple routing ile bir **A kaydı** bir veya daha fazla IP adresi ile ilişkilendirilir. Rastgele bir seçim hangi IP'nin kullanılacağını belirler. Simple Routing policy'lerinin **sağlık kontrollerini desteklemediğini** bilmek önemlidir. Diğer tüm routing policy'leri destekler.

- **Weighted routing policy**, simple routing'e benzerdir ve IP adresi başına bir ağırlık (weight) tanımlayabilirsiniz. Temel olarak, aynı ada ve tipe sahip kayıtlar oluşturur ve her kayda bir IP adresini diğerine göre tercih eden sayısal bir değer atarsınız. 0 değeri, bir kaydın hiçbir zaman dönmediğini gösterir. Bu, basit yük dağıtımı veya yeni yazılım testi için kullanışlıdır. Her kayıt, tüm kayıtların toplam ağırlığına kıyasla ağırlığına göre döndürülür. Seçilen bir kayıt **Unhealthy** ise, sağlıklı bir kayıt elde edilene kadar işlem **tekrarlanır.**

- **Geolocation routing policy**, kayıtları **Default**, **Kıta** veya **Ülke** olabilen bir konum ile etiketler. Farklı ülkelerdeki veya farklı dillerdeki müşterilere hitap edebilen bir kaynağın IP'sini dağıtmanıza olanak tanır. Ayrıca dağıtım veya lisanslama haklarını korumanıza yardımcı olabilir. Coğrafi bir konuma eşlenmeyen IP adresleri için varsayılan bir kayıt oluşturabilirsiniz. Geolocation routing ile bir IP kontrolü, müşterinin konumunu doğrular ve o konum için ilgili kayıt, ülke, kıta veya varsayılan Location Tag değerine göre döndürülür.

- **Geo-proximity routing policy**, Route 53'ün **Traffic Flow** özelliğini kullanmanızı ve bir **Traffic Policy** oluşturmanızı gerektirir. Traffic policy, bir veya daha fazla routing policy'sini birleştiren bir kaynaktır. Geo-proximity kayıtları bir AWS Region ile veya **enlem ve boylam** koordinatları kullanılarak etiketlenir. Geo-proximity routing, mesafe ve tanımlanmış bir **bias**'a dayanır. -99 ile 99 arasında bir Bias belirleyebilirsiniz. Bu, **pozitif bir değer kullanarak bir endpoint'e daha fazla trafik yönlendirmek veya negatif bir değer kullanarak bir endpoint'e daha az trafik yönlendirmek** için kullanabileceğiniz bir değerdir. Bir endpoint'e en az miktarda trafik yönlendirmek için -99 bias'ını kullanın. Bias'ı, kapsama açısından bir bölge boyutunu artırabilmek veya azaltabilmek olarak düşünebilirsiniz. Bu, trafiği bir konumdan diğerine kaydırmanıza ve kaynaklarınızın konumuna göre trafiği yönlendirmenize olanak tanır.

- **Failover routing policy**, bir sağlık kontrolüne dayalı olarak trafiği birincil bir kaynağa yönlendirebilir ve sağlık kontrolü başarısız olursa trafiği ikincil bir kaynağa yeniden yönlendirebilir. Failover routing kullanarak bir kaydı birincil ve farklı bir kaydı ikincil olarak tanımlarsınız. Ayrıca önceden tanımlanmış bir sağlık kontrolüne sahip olmanız gerekir. Birincil kaydın routing'i, sağlık kontrolü sonucu sağlıklı olduğunda aktiftir. Aksi takdirde, ikincil kayıt kullanılır.

- **Latency routing policy**, müşteriye **en düşük gecikmeye** sahip kaydı seçer. Aynı ada sahip birden fazla kayıt tanımlar ve her kayda bir bölge atarsınız. AWS, kullanıcıların genel konumu ile DNS kayıtlarında **etiketlenen bölgeler arasındaki gecikmeye ilişkin bir veritabanı** tutar. Kullanılan kayıt, kaydedilen en düşük gecikmeye sahip ve sağlıklı olandır. Bu, özellikle en yakın kaynak doygunsa, her zaman en yakın kaynak olmayabilir.

- **Multi value Answer routing policy**, bir sorguya birden fazla IP adresi döndürür. Bir sağlık kontrolüne dayalı olarak sağlıklı kayıtlara karşılık gelen **en fazla 8 IP adresi** döndürülür. Sekiz veya daha az sağlıklı host varsa, yanıt tüm sağlıklı host'ları içerir.

### Amazon Route 53 Trafik Akışı (Traffic Flow)

Traffic flow, büyük ve karmaşık konfigürasyonlarda kayıtların oluşturulmasını ve bakımını kolaylaştırır. Bu, aynı domain için bir web sunucusu grubu gibi, aynı işlemi gerçekleştiren bir kaynak grubunuz olduğunda kullanışlıdır.

Traffic flow görsel düzenleyicisi, karmaşık kayıt setleri oluşturmanıza ve bunlar arasındaki ilişkileri görmenize olanak tanır. Tek bir konfigürasyonda birden fazla routing policy'si ve sağlık kontrolünü birleştirebilirsiniz. Her konfigürasyon bir traffic policy olarak adlandırılır ve otomatik olarak sürümlenir. Böylece konfigürasyonunuz değiştiğinde her şeye yeniden başlamanız gerekmez. Traffic policy'lerinin eski sürümleri, siz silene kadar kullanılabilir olmaya devam eder.

Bir traffic policy yüzlerce kaydı tanımlayabilir. Traffic flow, bir policy kaydı oluşturduğunuzda tüm bu kayıtları otomatik olarak oluşturur. Traffic policy'leri domain veya subdomain adı kayıtlarıyla ilişkilendirmek için policy kayıtları oluşturursunuz. Traffic policy kaydı, hosted zone için kayıtlar listesinde görünür. Aynı traffic policy'yi birden fazla public-hosted zone'da kayıt oluşturmak için kullanabilirsiniz. Bu, aynı host'ları birden fazla domain için kullandığınızda kullanışlıdır.

Son olarak **Geo-proximity routing policy**'nin yalnızca **traffic flow** kullanıyorsanız mevcut olduğunu unutmayın.

### Amazon Route 53 Resolver (Çözümleyici)

Route 53 resolver, **veri merkezinizle entegre olan VPC**'ler için DNS hizmetidir. Veri merkezinizin DNS'i ile AWS arasında bir **Direct Connect (DX)** veya **Virtual Private Network (VPN)** bağlantısı kullanılarak bağlantı kurulması gerekir. VPC'lere gelen ve çıkan DNS sorguları için endpoint'ler yapılandırırsınız. Endpoint'ler, Route 53 Resolver'a ihtiyaç duyan her subnet'te IP adresi ataması yoluyla yapılandırılır.

- **Inbound sorgular**, veri merkezinizde oluşan DNS sorgularının AWS'de barındırılan domain'leri çözmesine olanak tanır.

- **Outbound DNS sorguları**, koşullu iletme kuralları kullanılarak etkinleştirilir. Veri merkezinizde barındırılan domain'ler, Route 53 resolver'da iletme kuralları olarak yapılandırılabilir. Bu domain'lerden birine bir sorgu yapıldığında kurallar tetiklenir ve istek veri merkezinize iletilir. VPC'leriniz için bu özyinelemeli DNS, DNS sorgularının VPC'leriniz ve veri merkeziniz arasında nasıl işlendiğini kontrol eder.

Son olarak, Route 53 Resolver DNS firewall, VPC'lerinizde başlayan DNS sorguları için yönetilen bir firewall hizmetidir. Route 53 Resolver DNS firewall'ın VPC'nizden gelen trafiği nasıl inceleyeceğini ve filtreleyeceğini tanımlamak için bir **firewall kural grubu** kullanırsınız. Her kural, DNS sorgularında incelenecek bir domain listesinden ve bir sorgu eşleşme ile sonuçlandığında alınacak bir eylemden oluşur. **Eşleşen bir sorgunun geçmesine** izin verebilir, bir uyarıyla geçmesine izin verebilir veya engelleyebilir ve varsayılan veya özel bir yanıtla yanıt verebilirsiniz. Filtrelemeyi başlatmak için kural grubunu korumak istediğiniz VPC'lerle ilişkilendirirsiniz. Route 53 resolver DNS firewall, tanımladığınız filtreleme kurallarını çıkan VPC trafiğine uygulayacaktır.

### Uygulama Kurtarma Denetleyicisi (Application Recovery Controller)

Route 53 application recovery controller, bir uygulamanın arızalardan kurtulma (recovery) yeteneğini sürekli olarak izleyen ve uygulama kurtarmayı birden fazla availability zone, bölge ve muhtemelen kendi veri merkezi ortamlarınız genelinde kontrol eden bir dizi yetenektir.

Bir **readiness check** tanımlayarak AWS kaynak konfigürasyonlarını, kapasitesini ve network routing policy'lerini izleyebilirsiniz. Bunlar, diğerleri arasında Auto Scaling Groups, Amazon EC2 instance'ları, Amazon EBS volume'leri, Elastic Load Balancer'lar, RDS instance'ları ve DynamoDB tablolarının konfigürasyonunu kontrol edebilir.

Bu readiness check'ler, recovery ortamının gerektiğinde devralmak için ölçeklendirildiğini ve yapılandırıldığını belirtir. Yeterli kapasitenin dağıtılabileceğini doğrulamak için AWS hizmet limitlerini kontrol edebilirsiniz. Ayrıca, bir failover gerçekleşmeden önce uygulamalar için kapasite ve ölçeklendirme kurulumlarının bölgeler arasında tam olarak aynı olduğunu doğrulayabilirsiniz.

Readiness Check'ler, uygulama metrikleri, kısmi arızalar, artan hata oranları veya gecikme gibi özel koşullara dayalı olarak tüm bir uygulamayı failover etmenin bir yolunu sunmak için **Routing Control**'ler ile birlikte çalışır. Ayrıca manuel olarak failover yapabilirsiniz. Routing Control'ler ile bakım amaçlı veya gerçek bir arıza senaryosu sırasında trafiği kaydırabilirsiniz. Ayrıca, hazırlıksız bir replikaya failover'ı önlemenin bir yolu olarak routing control'lere güvenlik kuralları uygulayabilirsiniz.

Bir control panel, bir uygulama için routing control'lerin bir grubudur. Daha önce belirtildiği gibi, bir routing control, Bölgeler veya Availability Zone'lardaki bireysel hücrelere trafik akışını **ON** veya **OFF** konumuna getirmek için kullanılır.

Bu, failover veya bakım döngüleri sırasında uygulamalarınız için özel trafik yeniden yönlendirmesi tanımlamanıza olanak tanır. Amazon Route 53 Application recovery controller, çok yüksek kullanılabilirlik gerektiren uygulamaları uygulamak için ince taneli failover ve doğrulama adımları yapılandırmanıza olanak tanır.


## API Nedir (What is an API)?

Bir API yani (Application Programming Interface: Uygulama Programlama Arayüzü) , kullanıcıların veya uygulamaların temiz ve iyi belgelenmiş bir sistem aracılığıyla birbirleriyle etkileşime girmesine olanak tanır. Bir API'nin amacı, hem hizmetin kendisi hem de onu çağıran kullanıcı için altta yatan hizmetle etkileşimleri kolaylaştırmaktır. API, ham hizmetin üzerinde başka bir katman olarak görülebilir. Bir tampon görevi görüp, kolay kullanılabilir ve anlaşılır bir arayüz sağlarken, bununla beraber altta yatan bir görevi gerçekleştirmek için gereken minimum bilgiyi sağlar.

API'yi düşünmenin en iyi yolu, onu bir restorandaki garson olarak hayal etmektir. Masaya oturduğunuzda, bir garson size restoranda mevcut olan tüm olası yemeklerin ve hizmetlerin bir menüsünü getirecektir. Bu benzetmede, restoran uygulamayı veya hizmeti temsil ederken garson ve menü API'dir.

Garson ve menüyü kullanarak restoranla (uygulama) etkileşime girebilirsiniz. Restoranın size bir omlet yapmasını istiyorsanız ve elbette bunu menüde bulabiliyorsanız, garson mutfağa gidecek ve personele masanız için bir omlet yapmasını söyleyecektir.

API sizi hizmetin arka ucundan ayırır. Mutfakta kaç aşçı olduğunu bilmeniz gerekmez. Omletinizin pişirilmesinde kaç tava kullanıldığından tamamen habersiz olursunuz. Tek bildiğiniz, menüden (API) bir omlet istediğinizde, kısa süre içinde size bir tane getirileceğidir.

API, bu durumda her iki taraf için de değerlidir. Müşteri, mutfağa bir omleti nasıl pişireceğini ve hangi malzemelerin adım adım girmesi gerektiğini söylemek zorunda kalmak istemez. Kendileri için doğru olan bir seçeneği seçmek ve hazırlanmasını isterler. Ek olarak, restoran omletlerini istedikleri gibi pişirebilmek ister. Pişirme süreci ortasında vardiya değişikliği yapmaları gerekiyorsa, müşterinin bunu bilmesine veya herhangi bir şekilde dahil olmasına gerek yoktur.

Bu benzetme gerçek dünya durumunda nasıl görünür? Restoran AWS gibi bir şirket olabilir. AWS'nin etkileşime girebileceğiniz ve sistemlerinizi ve hizmetlerinizi oluşturmanıza yardımcı olan çok sayıda genel API'si vardır.

Bu API'ler EC2 instance'ları, DynamoDB tabloları oluşturmanıza ve hatta tüm çok katmanlı mimariler oluşturmanıza olanak tanır. Muhtemelen AWS API'sini zaten kullanmışsınızdır ve kullandığınızın farkında bile olmamış olabilirsiniz. Konsolda bir EC2 instance'ı oluşturduğunuzda, aslında yaptığınız şey, web sayfasının o düğmeye tıklandığında sizin için AWS API'sini çağırmasıdır.

Komut satırını (command line) kullanmak da AWS API'sini çağırmanın bir yoludur. AWS'nin bu işlevleri yapmanıza yardımcı olmak için bu API'ye sahip olmasının nedeni, arka uçlarıyla uğraşmak zorunda kalmanızı istememeleridir. Hypervisor'ları kurmaktan veya arka uç ekipmanını yönetmekten sorumlu olmamalısınız. Sadece API'yi kullandığınızda EC2 instance'ının önyüklemeye başlamasını ve işiniz bittiğinde sonlanmasını istersiniz.

### HTTP ve İnternet İletişimine Hızlı Bir Bakış (A Quick Overview of HTTP and Internet Communication)

Artık bir API'nin sadece bir hizmet ile kullanıcı arasındaki iletişimi kolaylaştırmanın bir yolu olduğunu bildiğimize göre, bunun hangi yöntemlerle gerçekleşebileceğini incelemeye başlayabiliriz.

İnternet üzerindeki çoğu iletişim, HyperText Transfer Protocol veya HTTP kullanılarak gerçekleştirilir. Bu protokol, belge, resim, video, script ve benzeri kaynakları internet üzerinden almanıza olanak tanıyan bir sunucu-istemci protokolüdür. HTTP, senkron istek ve yanıt modelini takip eder. Bu model temelde, istekte bulunanın bir sistem tarafından alınan ve işlenen bir istek mesajı gönderdiği ve sonunda bir yanıt göndereceği bir mesaj değişim modelidir.

HTTP protokolü, iletişim kurmak için kullanabileceğiniz birçok farklı metoda sahiptir. Bunların her biri sunucuya belirli bir istekle ne yapması gerektiğini söyler.

Bu metotlardan en yaygın olanları şu şekilde sıralanabilir: GET, POST, PUT ve DELETE'dir.

- **GET** metodu, bir sunucudan belirli bir veri parçası istemenize olanak tanır. Bu, örneğin [www.fatihes.com](http://www.fatihes.com) yazdığınızda tarayıcınızın bir web sayfasını almak için kullandığı şeydir.

- **POST**, bir sunucuya yeni veri eklemek için bir istektir. Örneğin POST metodu, bir YouTube videosuna yorum eklemek için kullanılabilir.

- **PUT**, mevcut bazı verileri güncellemek için kullanılır. Örneğin önceki yorumu düzenlemek gibi.

- **DELETE** oldukça açıklayıcıdır. Bir veriyi silmek için kullanılır örneğin attığınız yorumu kaldırmaya karar verdiğinizde kullanılacaktır.

Ayrıca sunucudan gelen yanıtları tanımlamak için kurallar vardır. Her yanıt, sayısal bir yanıt kodu içerir. Yanıt kodları, istemciye yanıtı nasıl ele alacağını söylemeye yardımcı olur. Kodlar 100 ile 500 arasında numaralandırılmıştır ve bazıları muhtemelen size çok tanıdık gelecektir.

Örneğin, muhtemelen 404 durum koduna aşinasınız, bu sunucunun aradığınız şeyin bulunamadığını size söylemesidir. Ayrıca her şeyin beklendiği gibi gittiği anlamına gelen 200 durum kodunu da görmüş olabilirsiniz.

![deReAWg.md.png](https://iili.io/deReAWg.md.png)

Her farklı kod seviyesinin (200, 300 vb.) farklı bir anlamı vardır. 2xx yanıtları genellikle her şeyin yolunda gittiği anlamına gelir. 500 kodları, sunucunun yanıt oluşturmada bir sorunu olduğu anlamına gelir. 3xx'ler yönlendirmeleri kapsar ve 4xx aralığı istemci tarafında istekle ilgili sorunlar olduğunu gösterir.

### HTTP ve REST Arasındaki Fark (The Difference Between HTTP and REST)

Bir API oluşturmak veya tasarlamak için birçok yol vardır, ancak en yaygın duyacağınız yöntemlerden biri REST'tir. Bu konuya çok derinlemesine girmeyelim, ancak HTTP ve REST arasında bir ayrım yapmak için biraz üzerinde duralım. Bunlar iki farklı şeydir, ancak bazen kazara birbirlerinin yerine kullanılırlar.

HTTP, bir önceki başlıkta üzerinden geçtiğimiz gibi bir iletişim protokolüdür. Yani bir HTTP API, iletişim kurmak için HTTP kullanan ve üzerine başka önemli değişiklikler veya kısıtlamalar konulmamış bir API'dir. API'ın da ne olduğunu hatırlamak gerekirse, API temelde, iki yazılım varlığının birbirleriyle iletişim kurmasına yardımcı olan bir sistemdir.

Bir REST API veya RESTful API, iletişimin nasıl gerçekleşmesi gerektiğini tanımlamaya yardımcı olan iletişim sistemi üzerine yerleştirilmiş bir dizi kısıtlama olarak düşünülebilir.

REST, **RE**presentational **S**tate **T**ransfer'in kısaltmasıdır. REST neredeyse bir mimari stil gibidir, yapı malzemeleriyle ilgilenmez. HTTP, FTP veya diğer herhangi bir iletişim protokolü ile kullanılabilir. REST sadece HTTP ile çok yaygın olarak kullanılmaktadır.

RESTful bir sistem veya API oluştururken gereken oldukça fazla bileşen vardır, ancak temel noktalar şu şekilde listelenebilir:

-   Sunucu ve istemci arasındaki etkileşimler yalnızca hypertext kullanılarak tanımlanmalıdır.
-   İstemci ve sunucu arasındaki bağlantı mümkün olduğunca hafif veya gevşek olmalıdır. Bu, sunucunun veya istemcinin birbirleri hakkında herhangi varsayımda bulunmaması gerektiği anlamına gelir.
-   Sunucu, herhangi istenen bir bilgiyi saklamamalıdır. İstekler birbirinden bağımsız olmalı ve her zaman aynı sonuçları vermelidir.

RESTful sistemlerle ilgili bir diğer önemli mimari kavram, istemcilerin kaynaklara yalnızca bir URI kullanarak erişebilmesidir. Hizmet veya sunucu daha sonra istenen kaynağın durumunun bir temsilini onu çağıran istemciye aktaracaktır. Bu nedenle adı, REpresentational State Transfer (REST)'dir.

Bu durum bilgisi genellikle ayrıştırılabilen ve gerektiği gibi kullanılabilen bir JSON nesnesi olarak iletilir. İşte URI kullanarak bazı bilgiler almanın hızlı bir örneği:

Veritabanımızdaki tüm müşterilerin bir listesini almak istediğimizi hayal edin. Zaten oluşturulmuş bir API'miz varsa, bilgileri almak için API'yi kullanabiliriz.

![deRifSe.md.png](https://iili.io/deRifSe.md.png)
Yukarıdaki görselde, API'nin arka uç veritabanından tüm müşterileri döndürdüğünü görebiliriz. Finn, Jake ve Fire Princess'i görebiliyoruz.

Ancak ID'sini zaten biliyorsanız belirli bir müşteriyi almak isteyebilirsiniz.

Bunun için isteğimiz muhtemelen şu şekilde görünecektir: `www.example.com/api/customers/2`

Genel olarak bir API, işletmeniz için hem içsel hem de dışsal olarak işlevsellik oluşturmanıza gerçekten yardımcı olabilecek oldukça basit bir sistemdir.

## API Gateway

API'lerle çalışmak ve onları oluşturmak, çoğu geliştirme sürecinin büyük bir parçasıdır. API'ler, müşterilerinizin bilmesine gerek olmayan kodu, servisleri, mimarileri ve tüm küçük detayları soyutlamanıza olanak tanıyan faydalı bir aracıdır. AWS içinde bir API oluşturmanın birçok yolu vardır ve bazıları diğerlerine göre daha karmaşıktır. Örneğin, oluşturmak isteyebileceğiniz herhangi bir backend servisi için API görevi görebilecek bir EC2 instance üzerinde bir Node.js sunucusu çalıştırabilirsiniz. Ancak bu, biraz bilgi birikimi gerektirir ve normalde bir AWS Managed Service'in sizin için yapacağı patch, güvenlik ve diğer faydalarla sizin uğraşmanızı gerektirir.

AWS, API'leri AWS içinde oluşturma, yayınlama, izleme, güvence altına alma ve bakımını yapma konularında yardımcı olan **Amazon API Gateway** adında tamamen yönetilen bir servis oluşturdu. Bu servis her ölçekte oldukça iyi çalışır ve backend'de serverless, genel web uygulamaları ve hatta container'lı iş yüklerini destekleyebilir. API'lerinizi genel kullanım, özel kullanım veya hatta üçüncü taraf geliştiriciler için oluşturabilirsiniz. En iyi yanı ise tamamen serverless olması, herhangi bir altyapı yönetmenizi gerektirmemesi ve sadece kullandığınız kadar ödeme yapmanızdır. Servis ayrıca yüz binlerce eşzamanlı isteği kabul edip işleyebilir, işler kontrolden çıkmaya başlarsa API Gateway tüm trafiği izleyebilir ve istekleri istenildiği gibi throttle'layabilir yani kısıtlayabilir.

API'nizi oluşturmanın farklı yolları vardır. Bir API'ye ihtiyaç duyduğunuzu belirlediniz ve API Gateway gibi yönetilen bir servis kullanarak bir tane oluşturmak istediğinizi düşünelim. Bir sonraki soru, müşterilerinizin API'nizle nasıl etkileşime geçeceğidir. Bu soruyla kastedilen şey, bu API'nin nerede kullanılabilir olacağıdır. API Gateway, trafiğin API'ye farklı şekillerde gelmesine izin veren, farklı hedefleri olan üç farklı endpoint türüne sahiptir. İlk seçenek **Edge-Optimized API Endpoint**'tir. Bu seçenek, coğrafi olarak farklı ve dağınık birçok müşteriniz olduğunda en iyisidir. Bu endpoint türü için, tüm istekler en yakın **CloudFront point of presence**'a yönlendirilir, böylece API'nizi müşterinize yakınlaştırır. Bu tür bir endpoint kullanmak, TLS bağlantı yükünü azaltmaya yardımcı olabilir. Bu azalma sayesinde de genel olarak istek yanıtını hızlandırır. Bu mimariyi web uygulamaları, mobil uygulamalar ve bir API ile konuşması gereken olası IoT cihazları için önerilir.

İkinci seçenek **Regional API Endpoint**'tir. Bu seçenek, kendi içerik dağıtım ağınızı (CDN) kullanmayı planladığınızda iyi çalışır. Bu seçenek, aynı zamanda AWS olmayan bir CDN kullanmak istediğinizde veya hiç CDN kullanmak istemediğinizde de doğru seçenektir. Genel olarak, bu endpoint tüm API çağrılarınızın tek bir AWS Region'undan gelmesini beklediğinizde iyi çalışır. Bu, API'nizin yalnızca belirli bir bölgede bulunan diğer uygulamalar veya kendi servisleriniz tarafından kullanıldığında uygun olacaktır.

Son seçenek ise, **Private API Endpoint**'tir. Bu endpoint'lere yalnızca bir **Interface VPC Endpoint** kullanılarak VPC'nizin içinden erişilebilir. Bu, VPC'nizde oluşturduğunuz basit bir **ENI**'dir. Bu tür endpoint'ler dahili API'ler için kullanılmalıdır. Buna, yalnızca kuruluşunuzun bilmesi gereken mikroservisler veya dahili uygulamalar örnek verilebilir. Private API Endpoint, API'nizin güvenliği için ağ alanı üzerinde size çok fazla kontrol sağlar.

#### Desteklenen Protokoller (Supported Protocols)

 API Gateway, API oluşturabileceğiniz iki destekleyici protokole sahiptir. Bunlar **HTTP REST Endpoint**'leri ve **WebSocket Endpoint**'leridir. HTTP tipi endpoint iki versiyondan oluşur. Orijinal versiyon REST API olarak adlandırılırken, daha yeni versiyon sadece HTTP API'dir. Bu versiyonların her ikisi de GET, POST, PUT ve DELETE gibi HTTP metodlarını kullanarak HTTP İsteklerine hizmet vermek için kullanılır. Ayrıca, kafa karıştırıcı isimlendirme kuralına rağmen, her ikisi de bunu RESTful bir şekilde yapar.

Bu iki versiyon arasında bir dizi fark var. Ancak, daha yeni versiyon olan HTTP API'nin yavaş yavaş eski muadiliyle eşitliğe ulaştığını bilmek önemlidir. Sonunda REST API'yi geçecek ve şu anda API Gateway'in geliştirme yolunu oluşturmuş olacaktır. REST API başlangıçta API yaşam döngüsünün tüm yönleriyle başa çıkmaya yardımcı olmak için oluşturulmuştur. **Usage Plans**, **API Keys** gibi API yönetim özellikleri sunar ve API'lerinizi yayınlama ve paraya çevirme konusunda yardımcı olur.

Öte yandan HTTP API, **AWS Lambda fonksiyonları**na veya genel HTTP backend'lerine proxy olan bir API oluşturmak için optimize edilmiş ve oluşturulmuştur. REST API'nin sahip olduğu yaşam kalitesi özelliklerine sahip değildir, ancak bu HTTP API'ı yaklaşık %70 daha ucuz hale getirir. AWS, hangisini kullanmanız gerektiği konusunda şunları belirtir: 
- HTTP API'ler şunlar için idealdir:AWS Lambda veya herhangi bir HTTP endpoint için proxy API'ler oluşturmak, OIDC ve OAuth 2 yetkilendirmesi ile donatılmış modern API'ler oluşturmak, çok büyüme olasılığı olan iş yükleri ve gecikmeye duyarlı iş yükleri için API'ler.

- REST API'ler şunlar için idealdir: API'lerini oluşturmak, yönetmek ve yayınlamak için gereken tüm özellikleri içeren tek bir fiyat noktası ödemek isteyen müşteriler. 

WebSocket API ise diğer ikisinden tamamen farklıdır. Bu tür bir API, backend'iniz ile istemci arasında kalıcı bir bağlantı sağlar. Buna **Çift Yönlü İletişim (Duplex Communication)** denir. Bu, bir WebSocket API'nin uygulamalar arasında gerçek zamanlı iletişim oluşturmak için iyi olduğu anlamına gelir. Sohbet uygulamaları veya streaming tipi hizmetleri içerebilir. Bu durum, sunucuları yönetmekle uğraşmadan gerçek zamanlı iletişim uygulamaları oluşturabileceğiniz anlamına gelir. Ancak, kesintili bir bağlantınız veya zayıf gecikmeniz varsa, tam olarak beklediğiniz davranışı alamayabilirsiniz. Genel olarak, WebSocket API'ler oldukça kullanım durumuna özgüdür. 

#### API Gateway'i Servislerinizle Entegre Etme (Integrating API Gateway with Your Services)

API Gateway'inizin arkasına gerçekte ne koyabilirsiniz? Bu muhtemelen herhangi bir API'nin en önemli özelliğidir, backend'de neyle bağlantı kurabileceğini bilmek. API Gateway'i önüne koyabileceğiniz bir dizi şey var, ancak önce entegrasyonun nasıl gerçekleşebileceği hakkında tartışsak daha iyi olur. 

**Proxy vs Direct Integrations**

Backend'inize bağlanmanın iki farklı yolu vardır ve bunlar proxy entegrasyonu veya doğrudan entegrasyon yoluyla olur. Proxy entegrasyonu temel olarak sadece bir geçiş kurulumudur, burada API Gateway isteği yolda herhangi bir şeyi değiştirmeye çalışmadan doğrudan backend'e iletir. Doğrudan entegrasyon, API Gateway üzerinden geçerken istekte değişiklikler yapmanıza ve yanıtı istemciye geri dönerken değiştirmenize olanak tanır.

Proxy entegrasyonu kullanmanın avantajı, kurulumunun çok kolay olmasıdır. Neredeyse her şey backend serviste işlenir ve hızlı prototipleme için harikadır. Doğrudan entegrasyon kullanmanın avantajı, API Gateway'i backend'in istek ve yanıt yüklerinden, başlıklarından ve durum kodlarından tamamen ayırmasıdır. Bu, değişiklikler yapmanıza ve backend servisinin yanıtlarına kilitlenmemenize olanak tanır. Ancak, bu entegrasyon yönteminin kurulumunda daha fazla iş vardır.

**Entegrasyon Türleri (Integrations Types)**

Artık proxy ve doğrudan entegrasyon arasındaki farkı anladığımıza göre, servisin backend'de gerçekte neye bağlanabileceğini görme zamanı geldi. 
- **Lambda fonksiyonları:** API'lerinizi bir Lambda fonksiyonuna bağlayabilirsiniz. Bu, ya proxy olarak ya da doğrudan entegrasyon olarak yapılabilir. 
- **HTTP entegrasyonları:** API Gateway'in AWS içinde veya dışında herhangi bir halka açık endpoint'e işaret etmesini de sağlayabilirsiniz. Bu, onu halka açık bir load balancer'ın veya bir EC2 instance'ının önüne veya hatta on-premises'teki bir şeyin önüne koyabileceğiniz anlamına gelir. İstemcinin geçirdiği veriler, istek başlıklarını, sorgu dizelerini, URL yolu değişkenlerini ve bir payload'ı içerecektir. Backend HTTP endpoint'i bu bilgiyi ayrıştıracak ve neyle yanıt vermesi gerektiğine karar verecektir. Bu yöntem, ya proxy entegrasyonu ya da doğrudan entegrasyon olarak mevcuttur. 
- **Mock entegrasyonu:** Bu entegrasyon, API Gateway'in herhangi bir backend entegrasyonuna gerek kalmadan bir yanıt üretmesine olanak tanır. Temel olarak istediğiniz herhangi bir şeyle yanıt vermenizi sağlar. Bu, test için iyi olabilir veya backend servisi entegre edilmeye hazır olmadan önce yer tutucu verilerin geri dönmesine izin verebilir. Bu yöntem, sadece doğrudan entegrasyon olarak mevcuttur.
- **AWS Servis Entegrasyonu:** Bu entegrasyon, bir AWS Servis API yanıtıyla yanıt vermenizi sağlar. Bu entegrasyon çağrıldığında, API Gateway sizin için belirli bir AWS Servis API'sini çağıracaktır. Örneğin, bir Amazon SQS kuyruğuna mesaj göndermek veya bir Step Functions state machine'ini başlatmak için kullanabilirsiniz. Bu entegrasyon, kullanıcılarınızın tam erişime sahip olmadan AWS Servisine erişmesini istediğinizde iyidir. Bu da sadece doğrudan entegrasyon için mevcuttur.
- **VPC Link Entegrasyonları:** Bu entegrasyon türü, VPC'nizdeki özel kaynaklara erişmenizi sağlayacaktır. Bu kaynaklar, Application Load Balancer'lar veya container tabanlı uygulamalar gibi çeşitli varyasyonlarda olabilir. Bu entegrasyon türü de sadece doğrudan entegrasyondur. Ancak tüm bu entegrasyonlarla, müşterilerinizin isteyebileceği her türlü backend servisini hemen hemen ele alabilirsiniz. 

![deaQiXe.md.png](https://iili.io/deaQiXe.md.png)

REST API kullanan API Gateway'ler, beş entegrasyon türünün tümüne erişebilir: 
- Lambda Fonksiyonları, 
- HTTP, 
- Mock, 
- AWS Servisi  
- VPC Link. 

HTTP API'leri kullanan API Gateway'ler şunları kullanabilir: 
- Lambda Proxy Entegrasyonları, 
- AWS Servis Entegrasyonları, 
- HTTP Proxy Entegrasyonları. 

WebSocket API'lerini kullanan API Gateway'ler şunlara erişebilir: 
- Lambda fonksiyonları, 
- HTTP Endpoint'leri,
- AWS Servisler,

#### API Ağ Geçidi Yetkilendiricisi (API Gateway Authorizer)

Amazon API Gateway, HTTP ve REST API'leriniz için bir dizi yetkilendirme seçeneğine sahiptir. Ancak bunların bazıları API'ye özgüdür.

İlk seçenek bir **IAM Authorizer** kullanmaktır, bu yol AWS Identity and Access Management yani IAM'i kullanır. Bu yolu kullanarak, API müşterilerinize dağıtabileceğiniz benzersiz kimlik bilgileri oluşturmanıza olanak tanır. Bu, o müşteriden Signature Version 4 imzalı bir istek gerektirir ve ayrıca müşterinin Execute-API izinlerine sahip olmasını gerektirir. Bu seçenek HTTP API, REST API ve WebSocket API ile kullanılabilir.

İkinci seçenek bir **Lambda Authorizer** kullanmaktır, API Gateway'in özel bir yetkilendirme modeli çalıştırabilen bir Lambda fonksiyonunu yürütmesini sağlayabilirsiniz. Bu, bir tür üçüncü taraf auth sisteminiz veya bazı eski sistemleriniz varsa, bu fonksiyonun oradan ihtiyacınız olan her şeyi döndürebileceği anlamına gelir. Bu seçenek de HTTP, REST ve WebSocket API'leri ile kullanılabilir. 

Üçüncü seçeneğimiz bir **Cognito Authorizer** kullanmaktır. Bu yöntem, tam kullanıcı yönetimi yapmanıza olanak tanıyan cognito user pool'larına doğrudan entegrasyondur. Bu, giriş sayfalarına, çok faktörlü kimlik doğrulamaya, kullanıcı bilgilerine erişim anlamına gelir. Bu seçenek yalnızca REST API için mevcuttur. 

Ve son seçenek ise, bir **JWT Authorizer** kullanmaktır. OpenID Connect, OIDC'ye sahip herhangi bir servisi kullanabilirsiniz. Bu komik, çünkü bu aslında bu authorizer türü aracılığıyla Cognito'yu kullanabileceğiniz anlamına geliyor. Bunu belirtmekte fayda var; çünkü Cognito Authorizer'larının yalnızca REST API'leri için mevcut olduğunu belirtmiştik. Bu yöntemi kullanarak, Cognito'ya bağlanmak için bir JWT Authorizer kullanabilirsiniz. Bu seçenek yalnızca HTTP API'leri için mevcuttur.

#### API Gateway Güvenliği (API Gateway Secuirty)

İnsanların API'nizin işleyişine müdahale etmeye çalışabileceği birçok yol vardır ve bu tehditleri kontrol altında tutmak önemlidir. Herhangi bir çevrimiçi varlığın yaşayabileceği en yaygın sorunlardan biri DDOS saldırısıdır. API Gateway, böyle bir saldırının neden olabileceği hasarı büyük ölçüde azaltabilen **Web Application Firewall**, **AWS WAF** ile derin entegrasyonlara sahiptir. Ek olarak, WAF güvenlik ön cephesinde son derece değerli olan bir dizi başka özellik sunar. Bu özellikler arasında SQL enjeksiyon saldırıları ve cross-site scripting saldırıları gibi yaygın web saldırılarına karşı koruma yer alır.

WAF ile, IP adres aralıklarının API'nize erişmesini engelleme yeteneğine sahipsiniz. Hatta belirli ülkeleri veya bölgeleri bile engelleyebilirsiniz. 


#### API Yönetimi ve Kullanımı (API Management and Usage)

Bir REST API oluştururken, kullanım planları belirleme seçeneğiniz vardır. Bu seçenekler, API'nizi müşterilerinize bir ürün olarak sunmayı planlıyorsanız harikadır. Bu planlar, bir dizi API Anahtarı etrafında yapılandırılabilir, bu da sırayla müşterinizin ilgili API'ye erişmesine izin verir.

API Anahtarları, API'nize erişmek için müşterilerinize dağıtabileceğiniz alfanümerik dizelerdir. API Anahtarları, istekler belirli bir eşiğin üzerine çıkarsa onları throttle'lamanıza (kısıtlamanıza) veya müşterilerinizin kullanabileceği maksimum kotaları belirlemenize olanak tanır. Bu bilgilerle müşterileriniz için fatura beyanları oluşturabilir ve sözleşmelerinizin sınırları içinde kalmalarını sağlayabilirsiniz. API Anahtarları sadece dış müşterileriniz için değil, dahili kullanım için de faydalı olabilir.

Dahili API'niz üzerinde daha sıkı bir kontrol sağlamak istiyorsanız, geliştiricilerinize backend sistemlerinizi çökertme fırsatı vermeyecek API anahtarları verebilirsiniz. 

#### Yanıtlarınızı Önbelleğe Alma (Caching Your Responses)

API'niz daha popüler hale geldikçe, kullanıcılarınıza API'nizin gecikmesini hızlandırmaya yardımcı olmak için bazı yanıtlarınızı önbelleğe almakla ilgilenebilirsiniz. Önbelleğe alma, tekrarlanabilir yanıtları olan bir ürünü büyük ölçüde geliştirmenin çok uygun maliyetli bir yoludur.

Yanıtlarınızı önbelleğe alarak, backend endpoint'lerinize yapılan toplam çağrı sayısını azaltabilirsiniz. Bu sadece yanıt süresini artırmakla kalmaz, aynı zamanda bu backend servisleri üzerindeki yükü de azaltabilir, bu da çok faydalı olabilir. API Gateway, API'nizin o ekstra hız dokunuşuna ihtiyacı olduğunu düşünüyorsanız etkinleştirebileceğiniz yerleşik bir önbellek sistemine sahiptir. Etkinleştirildiğinde, API Gateway backend'inizden gelen yanıtları önbelleğe alacak ve bunları belirli bir yaşam süresi, TTL periyodu boyunca bellekte tutacaktır. API önbelleğinizin varsayılan TTL'si 300 saniyedir. Ancak, verilerinizin zaman hassasiyetine bağlı olarak bunu değiştirebilirsiniz. TTL'yi sıfır ile 3.600 saniye arasında herhangi bir yere ayarlama seçeneğiniz var. Verileriniz hızlı bir şekilde değişiyorsa endişelenmeyin, yeterince yüksek bir talebiniz varsa bir saniyelik bir önbellek bile uygulamanızı büyük ölçüde iyileştirebilir.

Önbelleğinizi oluştururken, öncelikle bir önbellek kapasitesi seçmeniz gerekecektir. Ne kadar büyük bir önbelleğiniz olursa, performans o kadar iyi olur. Ancak bu aynı zamanda maliyeti de artıracaktır. Ne yazık ki, bu servisin birçok yönü gibi, önbellek kullanımı da API Gateway'in REST API versiyonuyla sınırlıdır. 

#### Metrikler ve İzleme (Metrics and Monitoring) 

API'nizin sağlığını belirleyebileceğiniz en önemli yollardan biri, API Gateway'den gelen metrikleri toplamak ve izlemektir.

Genel olarak, API Gateway'in izlediği **yedi anahtar boyut** vardır. 
- İlk olarak **CacheHitCount**'a sahibiz. Bu, belirli bir dönemde API önbelleğinden sunulan istek sayısıdır. 
- Bir sonraki metrik **CacheMissCount**'tur. Bu, API önbelleğe alma etkinleştirildiğinde, belirli bir dönemde backend'den sunulan istek sayısıdır. 
- **Normal Count** değerine de sahibiz, bu belirli bir dönemdeki toplam API isteği sayısıdır. 
- **IntegrationLatency** metriği de bulunmaktadır. Bu metrik API Gateway'in bir isteği backend'e iletmesi ve backend'den bir yanıt alması arasındaki süredir. 
- Ve son olarak, sadece **genel Latency** değerine sahibiz. Bu değer, API gateway'in istemciden istekleri alması ve istemciye bir yanıt döndürmesi arasındaki süredir.

Latency, integration latency ve diğer API Gateway yükünü içerir. IntegrationLatency, toplam API sayısı, toplam CacheHitCount gibi şeyleri göz önünde bulundurmak, API'nizle ilgili bir şeylerin yanlış gittiğini size bildirebilir. API Gateway bu metrikleri her dakika CloudWatch'a gönderir ve bir bakışta bir şeylerin yanlış gittiğini size bildirecek gerçek zamanlı grafikler, panolar ve alarmlar sağlayabilir. 


#### HTTP vs REST API

HTTP ve REST API'ler arasındaki farkı tartışmış olsak da, mimarileriniz için belirleyici olabilecek daha spesifik bazı noktalara geri dönelim. Daha önce tartıştığımız gibi, API'nizin yetkilendirmesine baktığınızda, REST API kullanırken Native OpenIDConnect / OAuth 2.0 kullanamayacaksınız. Ancak, Cognito'nun farklı bir şekilde de olsa bu boşluğu doldurabileceğini düşünüyoruz. Entegrasyon potansiyeline baktığımızda, REST API Application Load Balancer veya Cloud Map ile özel entegrasyon sağlayamıyor. Bunun için alternatifler vardır.

![decspgR.md.png](https://iili.io/decspgR.md.png)

İşte AWS'nin bu konuda önerisi: "Özel Application Load Balancer'lar için, önce özel bir Network Load Balancer'a bağlanmak için API Gateway'in VPC linkini kullanın. Ardından, API Gateway isteklerini özel Application Load Balancer'a iletmek için Network Load Balancer'ı kullanın." Yani direkt olarak çalışmasa da, bir sürüm sonrasında çalışmasını zorlayabilirsiniz.

API Yönetimi konusunun çok kısa da olsa üzerinden geçtik. Ancak bazı insanlar gerçekten burada bir seçim yapmak zorunda kalabilir. API'niz o API Anahtarlarına ve kullanım planlarına ihtiyaç duyacaksa, şimdilik REST API'yi kullanmak zorundasınız. Ek olarak, HTTP API ile özel bir alan adı kullanmak için bir uyarı bulunmaktadır. Bu uyarı da TLS 1.2'nin desteklenen tek TLS sürümü olmasıdır.  API'nizin gelen isteklerin body değerini  değiştirme yeteneğine ihtiyacı varsa, REST API kullanmanız gerekir.

![decboDN.md.png](https://iili.io/decboDN.md.png)
Ek olarak, API önbelleğe almak isterseniz, bu da REST API'ye ait bir özelliktir. Backend'inizden gelen isteği ve yanıtı değiştirmek sizin için önemli değilse, HTTP API'leri oldukça indirimli bir seçenek olabilir. Ancak, bu servislere ihtiyacınız varsa, yine REST API'yi kullanmak zorundasınız. Güvenlik tarafında, REST API tam özellikli ve WAF erişimine sahiptir. Ancak HTTP API'ler  güçlü güvenlik özelliklerinden yoksundur. 

![delB6b9.md.png](https://iili.io/delB6b9.md.png)
Monitoring konusuna bakarsak yine, REST API tüm özelliklere sahipken, HTTP API biraz geride kalmış durumdadır. Ancak, bunların çoğu insan için büyük bir sorun teşkil etmeyebilir. 

#### Fiyatlandırma (Pricing)

Son olarak, aslında çoğu kullanıcı için en önemlisi olabilir. Bu iki API arasındaki fiyat farkından bahsedelim. Bahsettiğimiz gibi, HTP API'yi kullanmak için oldukça fazla şeyden vazgeçmeniz gerekecek. O halde bu tasarrufun ne olacağına bakalım. 

![delGVN1.md.png](https://iili.io/delGVN1.md.png)
Görselde REST API için maliyetleri görebilirsiniz, ilk 300 milyon civarı API çağrısı 3,50 dolar maliyete sahip. Kullanım arttıkça fiyat düşer. Ancak fark etmiş olmalısınız ki, bu fiyatı gerçekten düşürmek için milyarlarca çağrı yapmanız gerekecek. Bunu HTTP API ile karşılaştırırsak, fiyatın zaten REST API'nin en ucuz modelinden bile çok daha düşük olduğunu görebilirsiniz. Yani oldukça fazla şeyden vazgeçiyorsunuz, ancak bununla başa çıkabiliyorsanız ve oldukça büyük bir tasarruf elde edebilirsiniz. Bununla beraber, HTTP API ile daha fazla hacmin sadece %10'luk bir indirim olduğunu belirtmekte fayda var.

## Elastic Load Balancer (ELB)

Elastic Load Balancer'ın (genellikle ELB olarak anılır) ana işlevi, hedeflenen bir kaynak grubuna gelen isteklerin akışını yönetmek ve kontrol etmek için bu istekleri hedef kaynak grubuna eşit bir şekilde dağıtmaktır. Bu hedefler, bir dizi EC2 instance'ı, Lambda fonksiyonları, bir dizi IP adresi ve hatta Container'lar olabilir. ELB içinde tanımlanan hedefler, ek dayanıklılık için farklı Availability Zone'larda (Erişilebilirlik Bölgeleri) bulunabilir veya tek bir Availability Zone içinde yer alabilir.

Şimdi bunu tipik bir senaryodan ele alalım.

Diyelim ki, ortamınızda tek bir EC2 instance üzerinde bulunan yeni bir uygulama oluşturdunuz ve bu uygulamaya bir dizi kullanıcı erişiyor. Bu aşamada, mimariniz mantıksal olarak aşağıda gösterildiği gibi özetlenebilir.

![deWTzlV.md.png](https://iili.io/deWTzlV.md.png)

Eğer mimari tasarım ve en iyi uygulama ilkelerine aşinaysanız, tek bir instance kullanma yaklaşımının ideal olmadığını fark bilirsiniz. Ancak bu yöntem kesinlikle çalışır ve kullanıcılara hizmet sağlar bu bir gerçektir. Ancak, bu altyapı düzeni bazı zorluklar getirir. Örneğin, uygulamanızın bulunduğu tek bir instance donanım veya yazılım arızasından dolayı çalışmayı durdurabilir. Eğer bu gerçekleşirse, uygulamanız kapalı kalır ve kullanıcılarınıza erişilemez hale gelir. Ayrıca, ani bir trafik artışı yaşarsanız, instance performans sınırlamaları nedeniyle bu ek yükü kaldırmakta zorlanabilir. Sonuç olarak, altyapınızı güçlendirmek ve bu tür zorlukları gidermeye yardımcı olmak, beklenmedik trafik artışları ve yüksek kullanılabilirlik gibi durumlar için, tasarıma bir **Elastic Load Balancer** ve uygulamanızı çalıştıran ek bir instance eklemelisiniz.

![deXChQ4.md.png](https://iili.io/deXChQ4.md.png)

Yukarıdaki tasarımda görüldüğü gibi, AWS Elastic Load Balancer, kullanıcıdan gelen trafiği almak ve trafiği daha fazla sayıda instance arasında eşit olarak dağıtmak için bir nokta görevi görecektir. Varsayılan olarak, ELB, AWS tarafından yönetilen bir hizmet olduğundan yüksek düzeyde kullanılabilirliğe sahiptir. Bu nedenle, dayanıklılığını AWS garanti eder. Bu yüzden biz endişelenmek zorunda kalmayız. ELB, tek bir hata noktası gibi görünebilir; ancak aslında AWS tarafından yönetilen birden fazla instance'tan oluşur. Bu senaryoda, artık uygulamamızı çalıştıran üç instance bulunuyor. Önceden tartıştığımız zorluklara tekrar dönelim. Bu üç instance'tan herhangi biri arızalanırsa, ELB tanımlanmış metriklere dayanarak arızayı otomatik olarak tespit eder ve trafiği kalan iki sağlıklı instance'a yönlendirir. Ayrıca, bir trafik artışı yaşarsanız, uygulamanızı çalıştıran ek instance'lar bu ek yükle başa çıkmaya yardımcı olur. ELB kullanmanın birçok avantajından biri, AWS tarafından yönetiliyor olması ve tanımı gereği elastik olmasıdır. Bu, gelen trafik arttıkça veya azaldıkça otomatik olarak ölçekleneceği anlamına gelir.

Eğer kendi load balancer'ınızı kendiniz çalıştıran bir sistem yöneticisi veya DevOps mühendisiyseniz, load balancer'ınızı ölçeklendirmek ve yüksek kullanılabilirliği sağlamak konusunda endişelenmeniz gerekebilir. AWS ELB ile birkaç tıklamayla load balancer oluşturabilir ve dinamik ölçeklendirmeyi etkinleştirebilirsiniz. Trafik dağıtım gereksinimlerinize bağlı olarak, AWS'de seçebileceğiniz üç farklı ELB mevcuttur.

- İlk olarak, **Application Load Balancer**'dan bahsedelim. Application Load Balancer, HTTP veya HTTPS protokollerini kullanan web uygulamalarınız için esnek bir özellik seti sunar. Application Load Balancer, istek düzeyinde çalışır ve gelişmiş yönlendirme, TLS sonlandırma ve uygulama mimarilerine yönelik görünürlük özellikleri sağlar. Bu da trafiği aynı EC2 instance üzerindeki farklı portlara yönlendirmenize olanak tanır.

- Sonraki, **Network Load Balancer**'dır. Bu tür ELB ise, uygulamanız için ultra yüksek performans sağlamak amacıyla kullanılır ve aynı zamanda çok düşük gecikmeleri yönetir. Bağlantı düzeyinde çalışır, trafiği VPC'niz içindeki hedeflere yönlendirir ve saniyede milyonlarca isteği işleme kapasitesine sahiptir.

- Son olarak, **Classic Load Balancer** bulunur. Öncelikle mevcut EC2 classic ortamında inşa edilmiş uygulamalar için kullanılır ve hem bağlantı hem de istek düzeyinde çalışır. 

Şimdi, bir AWS ELB'nin bileşenlerinden ve bunların ardındaki bazı prensiplere yakından bakalım.

#### Dinleyiciler (Listeners) 

Kullanılan yük dengeleyici türünden bağımsız olarak, her bir yük dengeleyici için en az bir listener yapılandırmanız gerekir. Listener, gelen bağlantılarınızın, belirlediğiniz portlar ve protokoller temelinde hedef gruplarınıza nasıl yönlendirileceğini tanımlar. Listener'ın kendi yapılandırması, seçtiğiniz ELB türüne bağlı olarak biraz farklılık gösterir.

#### Hedef gruplar (Target groups) 

Hedef grubu, ELB'nin istekleri yönlendirmesini istediğiniz kaynakların oluşturduğu basit bir gruptur. Örneğin bir dizi EC2 instance'ı hedef grup olarak tanımlanabilir. ELB'nizi, her biri farklı bir listener yapılandırması ve ilgili kurallarla ilişkilendirilmiş birden fazla hedef grubuyla yapılandırabilirsiniz. Bu, istek türüne bağlı olarak trafiği farklı kaynaklara yönlendirebilmenizi sağlar.

#### Kurallar (Rules) 

Kurallar, ELB'niz içinde yapılandırdığınız her bir listener ile ilişkilendirilir ve gelen bir isteğin hangi hedef gruba yönlendirileceğini tanımlamaya yardımcı olur. ELB'niz bir veya daha fazla listener içerebilir, her listener bir veya daha fazla kural içerebilir, her kural birden fazla koşul içerebilir ve kuraldaki tüm koşullar tek bir eyleme eşittir. Bir örnek kural, koşulları temsil eden 'if' ifadesi ve tüm koşullar karşılandığında eylem olarak hareket eden 'then' ifadesi şeklinde olabilir. ELB'nin yanıt verdiği listener isteğine bağlı olarak, bu koşullar ve eylemleri içeren bir öncelik listesine dayalı bir kural ilişkilendirilecektir. Örneğin, istek 10.0.1.0/24 ağ aralığından geliyorsa  ve bir HTTP PUT isteği yapıyorsa, istek Group1 adlı hedef grubuna gönderilsin, şeklinde bir tanımlama bir eylemdir.

![dehL5Tg.md.png](https://iili.io/dehL5Tg.md.png)


#### Sağlık kontrolleri (Health checks)

ELB, hedef grup içinde tanımlanan kaynaklara karşı gerçekleştirilen bir sağlık kontrolü ile ilişkilidir. Bu sağlık kontrolleri, ELB'nin belirli bir protokol kullanarak her hedefe temas etmesine ve bir yanıt almasına olanak tanır. Belirli eşik değerleri içinde yanıt alınmazsa, ELB hedefi sağlıksız (unhealthy) olarak işaretleyecek ve bu hedefe trafik göndermeyi durduracaktır.

#### Internal veya Internet-facing ELBs 

Yük dengeleyicileriniz için iki farklı şema kullanılabilir: Internal (iç) veya Internet-facing (internet yönelimli). Internet-facing, adından da anlaşılacağı gibi, ELB'lerin düğümleri internete erişilebilir ve bu nedenle public bir DNS adına ve public bir IP adresine sahiptir. Bu, ayrıca bir dahili IP adresine de sahip olacağı anlamına gelir. Bu, ELB'nin internetten gelen istekleri karşılayarak trafiği hedef gruplarınıza yönlendirmeden önce sunabilmesine olanak tanır. İnternet yönelimli bir ELB, hedef grubuyla iletişim kurarken yalnızca dahili IP adresini kullanır; bu da hedef grubunuzun public IP adreslerine ihtiyaç duymayacağı anlamına gelir. Internal bir ELB sadece bir dahili IP adresine sahiptir. Bu, yalnızca VPC'niz içinden gelen istekleri karşılayabileceği anlamına gelir. Örneğin, bir iç yük dengeleyici, kamu alt ağı içindeki web sunucularınız ve özel alt ağdaki uygulama sunucularınız arasında oturabilir.

#### ELB Düğümleri (ELB Nodes) 

ELB'lerinizi oluşturma sürecinde, ELB'nizin hangi Availability Zone içinde çalışmasını istediğinizi tanımlamanız gerekir. Seçilen her Availability Zone için, o Availability Zone içinde bir ELB düğümü yerleştirilecektir. Sonuç olarak, trafik yönlendirmek istediğiniz herhangi bir Availability Zone için bir ELB düğümüne sahip olduğunuzdan emin olmanız gerekir. Availability Zone ile ilişkilendirilmediği takdirde, ELB, hedef grupta tanımlanmış olsalar bile o Availability Zone içindeki hedeflere trafik yönlendiremeyecektir. Bunun nedeni, ELB'nin trafiği hedef gruplarınıza dağıtmak için düğümleri kullanmasıdır.

#### Bölgeler Arası Yük Dengeleme (Cross-Zone Load Balancing). 

Hangi ELB seçeneğini seçtiğinize bağlı olarak, çevrenizde Cross-Zone load balancing'i etkinleştirme ve uygulama seçeneğine sahip olabilirsiniz. Diyelim ki ELB'niz için iki Availability Zone etkinleştirildi ve her birine bağlı yük dengeleyici eşit miktarda trafik alıyor. Bir Availability Zone altı hedefe sahipken, diğeri dört hedefe sahip. Cross-Zone load balancing devre dışı bırakıldığında, her ELB ve onunla ilişkili AZ, trafiğini yalnızca o Availability Zone içindeki hedeflere dağıtır. Availability Zones genelinde her hedef için trafiğin dengesiz dağılımına yol açar. Cross-Zone load balancing etkinleştirildiğinde, ilişkili Availability Zone'daki hedef sayısından bağımsız olarak, ELB'ler tüm gelen trafiği tüm hedefler arasında eşit olarak dağıtır, böylece Availability Zones genelinde her hedef için eşit bir dağılım sağlar.


### SSL Sunucu Sertifikaları (SSL Server Certificates) 

Bu başlık altında sunucu sertifikaları ve bunların elastic load balancers içinde nasıl kullanıldığına yakından bakacağız.

Önceki başlıkta de belirtildiği gibi, Application Load Balancer, HTTP veya HTTPS protokollerini kullanan web uygulamalarınız için esnek bir özellik seti sunar. Bu nedenle, ALB'nizi oluştururken kullanılabilen ALB listener seçenekleri, sırasıyla 80 ve 443 numaralı portlarda HTTP veya HTTPS protokolüdür. HTTP port 80 listener'larının yapılandırılması oldukça basit bir süreçtir. Ancak, bazen HTTPS şifreli protokolünü bir listener olarak kullanmanız gerekebilir ve bu da ek bir yapılandırma gerektirir.

Şimdi, HTTPS'yi bir listener olarak kullanırken dikkate almanız gereken bazı noktalar üzerinden geçelim. HTTPS, HTTP protokolünün şifreli bir versiyonudur. Bu durum, isteği başlatan istemciler ile Application Load Balancer'ınız arasında şifreli bir iletişim kanalı kurulmasına olanak tanır. Ancak, ALB'nizin HTTPS üzerinden şifreli trafik almasına izin vermek için bir sunucu sertifikası ve ilişkili bir güvenlik politikası gerekecektir.

SSL tam adıyla Secure Sockets Layer,  TLS (Transport Layer Security) gibi bir kriptografik protokoldür. Hem SSL hem de TLS, Application Load Balancer'ınız için sertifikalar tartışılırken birbirinin yerine kullanılabilir. ALB tarafından kullanılan sunucu sertifikaları, bir Sertifika Otoritesi tarafından sağlanmış olan bir dijital kimlik olan X.509 sertifikalarıdır ve bu Certificate Authority, AWS Certificate Manager (ACM) hizmeti de olabilir. Bu sertifika, uzaktaki istemciden alınan şifreli bağlantıyı sonlandırmak için kullanılır ve bu sonlandırma işleminin bir parçası olarak istek şifre çözülür ve ELB hedef grubundaki kaynaklara iletilir.

HTTPS'yi listener olarak seçtiğinizde, kullanılacak sertifikayı seçmeniz istenir ve bu işlem için dört farklı seçenekten birini kullanabilirsiniz: 
- ACM'den bir sertifika seçmek, 
- ACM'ye bir sertifika yüklemek, 
- IAM'den bir sertifika seçmek, 
- IAM'ye bir sertifika yüklemek.

 İlk iki seçenek ACM ile ilgilidir. ACM, AWS Certificate Manager'dir ve bu hizmet, AWS ortamınızda farklı hizmetlerde kullanılmak üzere SSL/TLS sunucu sertifikaları oluşturmanızı ve sağlamanızı sağlar. ACM ile olan bu entegrasyon, elastic load balancer için yeni bir sertifika uygulama sürecini basitleştirir ve bu nedenle tercih edilen seçenektir.

Son iki seçenek, IAM'i sertifika yöneticiniz olarak kullanarak üçüncü parti bir sertifika kullanmanıza olanak tanır ve bu seçeneği, ELB'lerinizi ACM tarafından desteklenmeyen bölgelerde dağıtırken seçersiniz.  ACM'i sertifika yöneticiniz olarak kullanmak, hem ACM'nin kendisi içinde sertifika oluşturmanıza hem de AWS dışından oluşturulmuş mevcut sertifikaları içe aktarmanıza olanak tanıyarak mevcut üçüncü parti sertifikalarınız için ek esneklik sağlar. ACM yapılandırması bu kursun kapsamı dışındadır. 

### Application Load Balancers (ALB)

Application Load Balancer (ALB) konusundaki bu derse hoş geldiniz. Bu, ele alacağım üç load balancer'dan ilki olacak. Eğer açık sistemler bağlantı modeli olan OSI modeline aşina iseniz, ALB'nin yedi katmanda, yani uygulama katmanında çalıştığını öğrenmek sizi şaşırtmayacaktır. OSI modelinin uygulama katmanı, kullanıcılar ve uygulama süreçlerinin ağ hizmetlerine erişim sağlaması için bir arayüz görevi görür. Bu katmandaki her şey uygulamaya özgüdür. Modelin uygulama katmanı, uygulamalara ağ hizmetleri sağlamaya yardımcı olur ve sunduğu uygulama süreçleri veya hizmetleri örnekleri arasında HTTP, FTP, SMTP ve NFS bulunur.

AWS'in, mikroservisler ve konteynerler gibi uygulama mimarilerine yönelik gelişmiş yönlendirme içeren esnek bir özellik seti sunmanız gerektiğinde Application Load Balancer kullanmanızı önerdiğini görebilirsiniz. Bu durum, özellikle HTTP veya HTTPS kullanıldığında önerilir. ALB'nizi yapılandırmadan önce, hedef gruplarınızı oluşturmak iyi bir uygulamadır. Önceki bir başlıkta değindiğimiz gibi, bir hedef grup, ALB'nizin istekleri yönlendirmesini istediğiniz kaynaklardan oluşan bir gruptur. İsteklerinizin niteliğine bağlı olarak farklı hedef gruplar yapılandırmak isteyebilirsiniz. Örneğin, internet-facing bir ALB'niz varsa, HTTP port 80 isteklerini işlemek için bir hedef grup ve güvenli HTTPS protokolünden gelen istekleri port 443 kullanarak işlemek için farklı bir hedef grup yapılandırmak isteyebilirsiniz. Bu senaryoda, iki farklı hedef grup yapılandırabilir ve ardından listeners ve rules kullanarak, isteğe bağlı olarak trafiği farklı hedeflere yönlendirebilirsiniz.

### Network Load Balancers

ALB ve NLB arasında, genel sürecin nasıl işlediğine dair prensipler aynıdır; yani, gelen trafiği bir kaynaktan yapılandırılmış hedef gruplarına dengelemek için kullanılır. Ancak, ALB trafiği yönlendirmek için HTTP başlığını analiz ederek uygulama seviyesinde çalışırken, ağ yük dengeleyicisi (Network Load Balancer - NLB) OSI modelinin Katman 4'ünde çalışır ve yalnızca TCP ve UDP protokollerine dayalı olarak istekleri dengelemenizi sağlar. Bu nedenle, bir TCP veya UDP bağlantısı açma isteği, hedef gruptaki ana makineyi yük dengelemek için kurulur. NLB tarafından desteklenen dinleyiciler TCP, TLS ve UDP'yi içerir. 

NLB, saniyede milyonlarca isteği işleyebilir, bu da ultra yüksek performansa ihtiyacınız varsa NLB'yi uygulamanız için harika bir seçim yapar. Ayrıca, uygulama mantığınız statik bir IP adresi gerektiriyorsa, o zaman NLB, tercih edeceğiniz esnek yük dengeleyici olmalıdır. Her zaman etkin olan uygulama yük dengeleyicisinin aksine, NLB etkinleştirilebilir veya devre dışı bırakılabilir. NLB'leriniz dağıtıldığında ve farklı erişilebilirlik bölgelerine iliştirildiğinde, bu erişilebilirlik bölgelerinde bir NLB düğümü sağlanacaktır. Düğüm daha sonra, hedefi bu bölgede isteği işlemek için seçmek üzere sıraya, protokole, kaynak porta, kaynak IP'ye, hedef porta ve hedef IP'ye dayalı ayrıntıları kullanan bir algoritma kullanır. Bir hedef ana makine ile bağlantı kurulduğunda, bu bağlantı, istek süresi boyunca o hedefle açık kalacaktır. 


### ELB ve Otomatik Ölçeklendirmeyi Birlikte Kullanma (Using ELB and Auto Scaling Together)

Bu başlık altında, ELB'lerin (Elastic Load Balancers) ve EC2 otomatik ölçeklendirme ile ilişkisini ele alacağız. Her bir hizmet, kendi başına belirli operasyonel zorlukları çözmek için harika bir yol sunar. ELB, hedef gruplar ve kurallar temelinde kaynaklarınız arasındaki yükleri dinamik olarak yönetmenizi sağlarken, EC2 otomatik ölçeklendirme, altyapınıza konulan talebe bağlı olarak bu hedef grupları esnek bir şekilde ölçeklendirmenize olanak tanır. Ancak, biri diğeri olmadan operasyonel bir yük getirebilir.

Örneğin, bir ELB'niz olduğunu ama otomatik ölçeklendirme olmadığını varsayalım. Talebe göre hedefleri manuel olarak eklemeniz ve kaldırmanız gerekecektir. Bu talebi izleyerek gerektiğinde örnekleri manuel olarak eklemeniz veya kaldırmanız gerekir. Şimdi tam tersine bakalım. EC2 otomatik ölçeklendirme yapılandırdığınızı ama bir elastic load balancer'ınız olmadığını varsayalım. Trafiği EC2 grubunuza nasıl eşit olarak dağıtacaksınız?

EC2 işlem kaynaklarınızı hem içe hem de dışa doğru otomatik olarak ölçeklendirmek ve yönetmek için bir ELB ve otomatik ölçeklendirmeyi birleştirmenin faydasını görebiliyorsunuzdur. Bir ELB'yi otomatik ölçeklendirme grubuna bağladığınızda, ELB otomatik olarak örnekleri algılar ve tüm trafiği otomatik ölçeklendirme grubundaki kaynaklara dağıtmaya başlar. Bir uygulama yük dengeleyicisini veya ağ yük dengeleyicisini ilişkilendirmek istediğinizde, otomatik ölçeklendirme grubunu ELB hedef grubuyla ilişkilendirirsiniz. Klasik bir yük dengeleyiciyi bağladığınızda, EC2 grubu doğrudan yük dengeleyici ile kaydedilecektir.

## Gateway Load Balancer

AWS hizmetlerinin ve yeniliklerinin çoğu müşteri odaklıdır. AWS Gateway Load Balancer hizmeti de bu duruma örnek niteliğindedir. Birçok müşteri, AWS ortaklarının ve AWS Marketplace'in sanal cihazlarına güveniyor ve kullanmaktadır. Ancak, sanal cihazların kurulumu ve ölçeklendirilmesi zor olabilir. İlk olarak, bir VPC içindeki belirli bir EC2 örneğinin Elastic Network Interface'ine, bir Internet Gateway veya Virtual Private Gateway'den gelen tüm trafiği, hem gelen hem de giden kurallarını yönlendirebilmemiz gerekiyordu.

bir Internet Gateway için VPC Ingress yönlendirmesi kullanılarak uygulanmıştır. VPC Ingress yönlendirmesi kullanarak, bir VPC içindeki yönlendirme tablolarını güncelleyerek trafiği bir Gateway Load Balancer'a yönlendirebiliriz. Gereken bir sonraki özellik, IP adresleme ile ilgili hataların veya çatışmaların oluşmaması için IP tünellemesiyle başa çıkmaktır. Özetle, VPC için gelen ve giden tüm trafiği alabilmeli ve güvenlik işlemleri için sanal bir ağ cihazına yönlendirebilmeli ve isteğin ve yanıtın normal akışını ve etkileşimlerini kesintiye uğratmamalıyız.

Bu süreç şeffaf olmalıdır. AWS Gateway Load Balancer, tüm gelen ve giden trafik için tek bir erişim noktası kullanır ve diğer Elastic Load Balancer'lar gibi (örneğin, Application Load Balancer) talebe göre sanal cihazınızı ölçeklendirmenize olanak tanır. Gateway Load Balancer, trafiği sağlıklı sanal cihazlar üzerinden yönlendirir ve bir cihaz sağlıksız hale gelirse trafiği göndermeyi durdurur.

Gateway Load Balancer kullanarak, AWS'deki herhangi bir ağ yoluna kendi mantığınızı ekleyebilir, paketleri incelemek ve onlara göre işlem yapmak istediğinizde müdahale edebilirsiniz. AWS Gateway Load Balancer, gelen ve giden trafiği aynı tutarlı rota üzerinden ve aynı hedefi kullanarak şeffaf bir şekilde gönderir. Bu durum şeffaf ve simetrik bir akışı uygular.





