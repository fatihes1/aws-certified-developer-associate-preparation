# Containers (DVA-C02)

Bu başlığın amacı, geliştiriciler için AWS'deki container hizmetlerine bir giriş sağlamaktır. Bu hizmetler şunları içerir:

-   AWS Copilot,
-   Amazon Elastic Container Registry (Amazon ECR)
-   Amazon Elastic Container Service (Amazon ECS)
-   Amazon Elastic Kubernetes Services (Amazon EKS)


## Mikro Hizmetlere, Konteynerlere ve ECS'ye Giriş (Introduction to Microservices, Containers, and ECS)

### Monolitik uygulamanın sorunları

Bu başlık altındaki konuların daha iyi anlaşılabilmesi için, monolitik uygulamanın sorunları ve bu konuda neler yapabileceğimiz konusuna değinerek başlayalım.

Uzun yıllar boyunca yazılım endüstrisi, her özelliği sıkıca birbirine bağlı olan büyük, monolitik uygulamalar oluşturuyordu. Sistemleri bir kerede oluşturmak ve bireysel bileşenler yerine bir bütün olarak inşa etmek doğaldı. Bu fikir bir süre oldukça iyi işledi, ancak iş temposu arttıkça ve uygulamaların boyutu şişmeye başladıkça, büyük sorunlar ortaya çıkmaya başladı.

Kullanıcıların hesap oluşturabildiği, yeni güncellemeler paylaşabildiği, bu gönderilere yorum yapabildiği ve dahili bir sohbet özelliğine sahip bir çevrimiçi forum web sitesine sahip olduğumuzu hayal edin. Bu web sitesinin tüm bu bileşenleri tek bir hizmet olarak yazılmış olması muhtemeldir. Şimdi, bir anda birçok kişinin yeni içerik paylaşmaya başladığını hayal edin, tüm uygulamayı ölçeklendirmek zorunda kalırdık, sadece kritik parçaları değil. Bu israf yaratırdı, çünkü sadece ihtiyaç duyulan yere değil, ihtiyaç duyulmayan yerlere de daha fazla güç eklemiş olurduk.

Günümüzde bu özellikleri ayrı hizmetlere ayırmak isteriz. Monolitik uygulamamızı, daha küçük uygulamalara veya yaygın olarak adlandırıldığı şekliyle microservice'lere ayırmaya başladık. Her microservice bir işi iyi yapar ve diğer ilgili hizmetler hakkında çok fazla şey bilmesi gerekmez. Microservice'lerin açıkça tanımlanmış bir API aracılığıyla erişilmesi amaçlanmıştır. Ayrıca bağımsız olarak yönetilir ve güncellenir. Bu konulara değindiğimize göre, container fikrine giriş yapabiliriz.

### Container nedir?

Container, bir uygulamayı çalıştırmak için gereken tüm kodu ve bağımlılıklarını (framework'leriniz, binary'ler, konfigürasyonlar) içeren tam paketlenmiş bir yazılım paketidir. Bu container'lar, bir container orchestration hizmeti çalıştıran bir sunucuda barındırılır. Container orchestration hizmeti, daha önce bahsedilen tüm bağımlılık bilgilerini içeren bir başlangıç image'ını referans alarak yeni container'lar oluşturabilir.

Yazılımınızı container'laştırarak, çalışan kodu tekrar tekrar dağıtmanın bir yolunu bulmuş olursunuz, bu kod birçok platformda çalışır ve ihtiyaçları için altta yatan sisteme bağımlı değildir. Bu durum, uygulamanızın platform bağımsız olmasını sağlar, yani kendi ortamınızda çalışan bir proje geliştirirseniz, seçtiğiniz container engine'i destekleyen herhangi bir ortamda çalışacak demektir.

Container engine, aslında container'larınızın orchestration'ı ile ilgilenen yapıdır. Gerektiğinde container'ları başlatmaya ve kapatmaya yardımcı olur. Tüm bunlar, bir virtual machine'in yaptığı işlere çok benziyor gibi gelebilir, ancak birkaç önemli fark vardır. Virtual machine veya VM, paketi içinde bir konuk işletim sistemi (Windows veya Linux) içerir. Uygulamanız bu işletim sisteminin üzerinde konumlanır ve gerektiğinde özelliklerini kullanabilir.

VM, yazılımınızın ihtiyaç duyduğu tüm bağımlılıkları içerebilir ve genel olarak oldukça bağımsızdır (bir bakıma container'a benzer şekilde). Ancak, VM'lerde paket içinde konuk işletim sistemini depoladığımız için, son ürün gigabyte'larca büyüklükte olabilir. Virtual machine bu kadar büyük olduğu için, bir VM'i yüklemek oldukça uzun zaman alabilir. Bir virtual machine başlatılırken, uygulamanızı çalıştırmaya başlamadan önce konuk işletim sisteminin başlatılmasını beklemesi gerekir. 

VM'ler, sorumluluğundaki tüm VM'leri yönetmekten sorumlu olan bir hypervisor üzerinde konumlanır (container engine'e benzer şekilde). Hypervisor, tüm VM'lerin altta yatan donanımla etkileşim kurmak için yaklaşık olarak eşit zaman almasını sağlar (CPU, Bellek ve disk depolama kullanımına yardımcı olur).

Bir not olarak: EC2 instance'ları oluştururken boyut seçimi sırasında; medium, large, 2xlarge gibi değerlerden birini seçeriz. Aslında perde arkasındaki süreç, instance'a (o VM'e) daha fazla veya tercihli CPU, RAM ve Networking zamanı verilmesidir.

Konunun özüne geri dönersek. VM ve container arasındaki temel fark, işletim sisteminin konumudur. Container, uygulamanın çalışacağı işletim sistemini içermediği için oldukça derecede küçüktür. Bir container, dağıtımdan sonra saniyeler içinde başlatılabilir ve çalışmaya başlayabilir. Hız ve çevikliğin çok önemli olduğu senaryolarda, container'lar gerçekten cazip bir seçenek gibi görünmeye başlar.

Daha önceki örneğimizi hayal edebilirsiniz, kullanıcıları, gönderileri, yorumları ve canlı sohbeti olan forum web sitesi; bu bileşenlerin her birinin monolitik programdan ayrılabileceğini ve bir container'a yerleştirilebileceğini düşünebilirsiniz. Tüm uygulamanın bu bireysel parçaları, microservice'lerimiz, talebe göre yukarı ve aşağı ölçeklendirilebilir. Ayrıca çok hızlı bir şekilde başlatıldıkları için, ölçekleme ile ilgili sorunlarla uğraşırken büyük bir hız ve çeviklik kazanıyoruz.

### AWS içinde container'ları nasıl kullanırız?

Containerized workload'lar, bulutta çalışanlar için çok sayıda fayda sunar. Ancak container'ları kullanmanın en zor kısımlarından biri, her şeyi ilk başta kurmaktır. Her şeyi sıfırdan kendiniz oluşturup dağıtabilirsiniz, ancak bu çalışır hale getirmek için çok zaman ve bilgi gerektirir. İşte AWS'nin devreye girdiği ve tam da bu amaçla bir hizmet oluşturduğu yer burasıdır.

**Amazon Elastic Container Service**, containerized workload'larınızı dağıtmanıza, yönetmenize ve ölçeklendirmenize yardımcı olabilen tam yönetilen bir container orchestration platformudur. Tüm bunları endüstri standardı olan ve birçok harika kullanım kolaylığı özelliği sunan Docker container'larını kullanarak yapar. Amazon ECS'nin container'larınızı dağıtmak için iki yolu vardır: 

- İlk ve en geleneksel yol, ECS'nin Docker Engine'in yüklü olduğu bir ec2 instance'ları kümesi oluşturmasıdır. ECS, kümenin sağlanmasını yönetmenize ve otomatik ölçeklendirme ihtiyaçlarıyla ilgilenmenize yardımcı olacaktır. Gerekli container sayısına bağlı olarak küme içindeki instance sayısını artıracak ve azaltacaktır. Genel olarak, her EC2 instance'ı üzerinde birden fazla container dağıtılmış olacaktır. Her container, ihtiyaçlarını karşılamak için belirli bir miktarda bellek ve CPU ek yükü gerektirecektir ve bu sınırlar içinde kalmaya çalışacaktır. Instance'lar içinde başka bir container dağıtmak için kullanılabilir alan kalmadığında küme ölçeklenecektir. Bu, işletim belleği veya CPU ek yükü tükendiğinde olabilir.

- ECS'nin container'ları dağıtmanıza izin verdiği ikinci yol, bunu serverless bir şekilde yapmaktır. Endişelenmeniz gereken instance'lar olmayacak, sadece container'ın kendisini kurmanız gerekecek. ECS, serverless container'larınızı yönetmeye yardımcı olan AWS Fargate adlı bir kardeş hizmet kullanır. Bu container'lar, servered alternatiflerinin tüm gücüne ve işlevselliğine sahiptir. Ana fark, bu container'ların her zaman açık olmaması, yani sadece onları çağırdığınızda var olmaları ve görevlerini tamamladıklarında kapanmalarıdır.

Hangi yöntemin (serverless veya servered) sizin için en iyisi olduğunu belirlerken düşünmeniz gereken birçok metrik bulunur.

### Amazon Elastic Container Registry

İlk olarak, ECS'de kullanmak üzere containerlarımızı oluşturmak ve depolamak için bir yola ihtiyacımız var. Amazon, Amazon ECS'ye entegre edilmiş tam yönetilen bir Docker container registry'si olan Elastic Container Registry (ECR)'yi hizmete sunmuştur.

Registry, image'larınızı yüksek kullanılabilirlik ve ölçeklenebilirlik sağlayan bir şekilde barındırmanıza yardımcı olur. Bu hizmet tam özellikli ve docker image'larınız için sağlam güvenlik sağlar. ECR ile ayrıca container image'larınızı ve artifact'lerinizi uygun gördüğünüz şekilde private veya public olarak paylaşma yeteneğine sahipsiniz. Geliştiriciler hatta containerlarını dünya çapında keşif ve indirme için Amazon ECR public gallery aracılığıyla halka sunabilirler.

Registry'nizi oluşturmak çok basittir ve tamamlandığında size şuna benzer temel bir URI sağlayacaktır: `278915031458.dkr.ecr.us-west-2.amazonaws.com/ca-container-registry`

Bu, elastic container service ile kullanmak üzere container image'larını göndermeye başlayabileceğimiz konumdur. Ayrıca uygulamalarınız için containerlarınızı geliştirirken, Amazon ECR bu container image'ları için lifecycle policy'leri yapılandırma yeteneğine sahiptir. Muhtemelen herhangi bir zamanda geçmiş image'larınızdan birkaçından fazlasını saklamanıza gerek olmayacaktır, bu yüzden belirli bir sayı aşıldıktan sonra geçmiş sürümleri kaldırabilirsiniz. Bu yapılandırılabilir ve tamamen size bağlıdır.

### Task Definition

Containerımızı ECS üzerinde çalıştırmak için ayrıca bazı task definition'lar tanımlamamız gerekiyor, böylece ECS containerlarınızın temel gereksinimlerini anlar. Task definition'lar ECS'ye hangi Docker image'larının container instance'ları için kullanılacağını, ne tür kaynakların tahsis edileceğini, ağ özelliklerini ve diğer detayları söyler. Örneğin bu tanımlamalar; bellek sınırlarını yapılandırma, port mapping'leri ve dağıtılan containerlar için isimleri ve image'ları ayarlamayı içerir.

Bir task definition oluştururken, serverless workflow (AWS Fargate kullanarak) için bir tane oluşturmak ile servered workflow (ECS Clusters) için bir tane oluşturmak arasında küçük farklar vardır, ancak aksi takdirde çok benzerlerdir. Bu görevin hangi container image'ını kullanmasını istediğimizi, önceki Amazon Elastic Container Registry deposundan URI'yi referans alarak tanımlarız: `278915031458.dkr.ecr.us-west-2.amazonaws.com/ca-container-registry:testblue`

Tüm bu tanımlamalar ayarlandıktan sonra, görevlerinizi bir EC2 cluster'ına veya AWS Fargate'e (serverless container hizmetine) dağıtmaya başlayabileceksiniz. Ne yazık ki, bir Service oluşturmazsak bu dağıtımı elle yapmanız gerekecekti.

### ECS Services

Bir ECS Service, bir Amazon ECS cluster'ı içinde belirli sayıda task definition'ı yönetmenize, çalıştırmanıza ve sürdürmenize olanak tanır. Bir ECS service oluşturduğunuzda, yaşam döngüleri sırasında herhangi bir görev başarısız olursa görevlerinizi yeniden dağıtmanıza yardımcı olur. ECS service'leri, self-healing containerların önemli bir parçasıdır.

ECS service'leri oluşturmanın ve kullanmanın bir diğer önemli faydası, containerlarınızı bir application load balancer arkasına yerleştirmenize olanak tanımasıdır. Application load balancer, OSI modelinin 7. katmanında (HTTP/HTTPS) çalışır. ECS service'imizi bir target group içine yerleştirebilir ve trafiğimizi containerlarımız arasında load-balance edebiliriz. Application load balancer'ı kullandığımız için, bu aynı zamanda containerlarımızın farklı versiyonlarına web sayfamızın URL'si üzerinden erişebileceğimiz anlamına gelir.

Daha önceki microservice ve monolitik forum web sitesini hatırlıyor musunuz? O örnekte, hesap oluşturma, yeni güncellemeler paylaşma, bu gönderilere yorum yapma ve dahili bir sohbet özelliği vardı. İşte, bunların her biri kendi containerlarına ayrılabilir, kendi service'lerine yerleştirilebilir ve Application load balancer aracılığıyla şu şekilde erişilebilir:

- `example.com/api/CreateAccount`

- `example.com/api/PostNewUpdates`

- `example.com/api/Comment`

- `example.com/api/Chat`

gibi.


### Severed ve serverless containerları ne zaman kullanmalıyız?

Bu soruya verilecek cevap, container tabanlı bir mimari tasarlarken erken aşamada yapılması önemli olan bir konudur. Hangi yolun sizin için en iyisi olduğuna karar vermenizde yardımcı olabilecek birkaç spesifik konu bulunur.

### Maliyet

Genel olarak, serverless compute, 1'e 1 ortamda servered compute'dan daha pahalıdır. Beş dakikalık serverless, beş dakikalık  servered güçten daha pahalıya mal olacaktır. Bu yüzden her zaman servered bir seçeneği seçmenin daha iyi olduğunu varsayabilirsiniz...

Daha önce bahsettiğimiz gibi, yalnızca containerlarınızı çalıştıran kaynak makinedeki toplam kullanılan kaynakları maksimize ettiğinizde doğrudur. Diyelim ki EC2 instance'larında çalışan tüm containerlarınız, alttaki instance'ın CPU ve belleğinin yalnızca %50'sini kullanıyor. Bu senaryoda, muhtemelen serverless bir workload üzerinde çalışmak daha iyi olurdu. Aslında, Fargate'i (serverless seçenek) kullanmak neredeyse %40 daha maliyet etkin olurdu. Bunun nedeni, serverless'ın her zaman %100 etkili CPU ve bellek kullanımında çalışmasıdır.

Instance tabanlı containerlarınızı optimize etmek, verimliliğinizi gerçekten maksimize etmek için çok fazla uğraşma ve deneme yanılma gerektirebilir. Ancak ortalama CPU ve bellek rezervasyonunu %100'e çıkarabilseydiniz, EC2 tabanlı bir yaklaşım (servered seçenek) kullanarak %20 daha maliyet verimli olurdunuz.

![dDe260v.md.png](https://iili.io/dDe260v.md.png)

Başka bir senaryoya bakalım. Çok ani yükselen, hızla yukarı ve aşağı giden veya sadece tahmin edilemeyen zamanlarda gelen ve uzun düşük veya ölü zaman (dead time) periyotları olabilen trafiğiniz varsa; bu senaryo, serverless'ın kazandığı başka bir durumdur.

Bu tür bir trafikle EC2 cluster'ı ile uğraşırken, hala tüm o ölü zaman için ödeme yapmanız gerekiyor. Hiçbir şey olmadığında bile cluster'ı çalışır durumda tutmak hala para harcar, otomatik olarak küçültseniz bile. Çünkü o trafik trafik hazır containerlar ile her zaman minimum sayıda instance'ınız olmalıdır.

Dolayısıyla, işlerin maliyet tarafını özetlemek gerekirse, alttaki instance'ların CPU ve Belleğini tam olarak kullanan, uzun süreli operasyonları destekleyebilen bir workload'unuz varsa; maliyet açısından muhtemelen en iyi eylem planı servered bir dağıtımdır.

### Bakım

ECS kullanmanın harika yanlarından biri, containerlarınızın bulunduğu ortam üzerinde tam kontrole sahip olmanızdır. Ayrıca containerlarınızın üzerinde çalıştığı instance türlerini daha ayrıntılı bir şekilde kontrol etme yeteneğine sahipsiniz (ağ optimize edilmiş veya GPU optimize edilmiş instance'ları seçmek gibi). Ancak, tüm bunların dezavantajı, aynı zamanda tüm bu öğelerin bakımından da sorumlu olmanızdır. Bu, instance'ların yamalanmasını, ağ ile uğraşmayı ve tüm cluster'ın genel güvenliğini içerir. Fargate genel olarak, tüm yukarıdakiler yerine sadece uygulamanın kendisinin geliştirilmesine ve bakımına odaklanmanıza olanak tanır. AWS sizin için diğer her şeyi halleder.

### Ağ Seçenekleri

ECS ve Fargate arasında hangi seçeneği seçeceğini belirlemeye çalışan biri için bir başka anlaşmazlık noktası da ağ seçeneklerinin mevcudiyetidir. ECS ile üç ağ modu arasında seçim yapabilirsiniz:

- **Host Mode:** Containerın ağını doğrudan containerı çalıştıran alttaki host'a bağlar.

- **Bridge Mode:** Host ile containerın ağı arasında bir katman oluşturmak için sanal bir ağ köprüsü kullandığınız yer. Bu şekilde, bir host portunu bir container portuna yeniden eşleyen port mapping'leri oluşturabilirsiniz. Mapping'ler statik veya dinamik olabilir.

- **AWSVPC Mode:** awsvpc ağ moduyla, Amazon ECS her görev için bir Elastic Network Interface (ENI) oluşturur ve yönetir ve her görev VPC içinde kendi özel IP adresini alır. Bu ENI, alttaki host'un ENI'sinden ayrıdır. Bir Amazon EC2 instance'ı birden fazla görev çalıştırıyorsa, her görevin ENI'si de ayrıdır.

Ancak AWS Fargate ile sadece AWSVPC modunu kullanmak zorundasınız. İyi haber şu ki, AWSVPC modu geniş bir çözüm yelpazesini kapsar; ancak containerlarınız için daha özel bir ağ moduna ihtiyacınız varsa, Fargate yerine ECS'yi kullanmak zorunda kalabilirsiniz.

### Özet

Containerlar ve Amazon ECS ile çalışmak, daha sağlam bir uygulama oluşturmanıza yardımcı olabilir. Bunu, monolitik programlarımızı belirli bir görevi iyi yapabilen ayrı microservice'lere ayırarak yapabiliriz.

Bu görevler, containerlar içine yerleştirilebilir (tüm kodu, bağımlılıkları ve görevin çalışması için ihtiyaç duyduğu her şeyi barındırır) ve hızlı bir şekilde dağıtılmak ve tüm ortamlarda tekrarlanabilir olmak üzere tasarlanmıştır.

Container image'larımız, lifecycle policy'leri ile yönetilebilecekleri Amazon elastic container registry içinde barındırılabilir. Her image'ın ECS içinde dağıtım için kullanmanıza olanak tanıyan benzersiz bir URI'si vardır. Ek olarak, containerlarınızı dünya ile paylaşma veya Amazon ECR public gallery aracılığıyla başkalarının containerlarını görme seçeneğiniz vardır.

ECS ile yönetilen containerlar, altta yatan altyapıyı tanımlamanıza olanak tanıyan EC2 instance'larına yerleştirilebilir. Bu, yönetmeniz gereken karmaşık ağ kurulumlarınız olduğunda iyidir; ancak aynı zamanda onları bir serverless framework içine yerleştirme seçeneğiniz de var (AWS Fargate kullanarak), bu durumda bazı doğrudan kontrolü kaybedersiniz, ancak AWS'den gözetim kazanırsınız.

Serverless ve servered containerlar arasında seçim yapmak kolay bir görev değildir, yine de serverless framework ile ilişkili bazı belirgin maliyet avantajları vardır. Servered containerlar, yalnızca altta yatan instance'ın tüm kapasitesini kullanabildiğinizde serverless'dan daha maliyet etkindir.


## ECS: Elastic Container Service

ECS hizmeti, karmaşık ve yönetimsel olarak ağır bir cluster yönetim sistemini yönetmenizi gerektirmeden, Docker destekli uygulamaları container'lar olarak paketlenmiş şekilde EC2 instance'larından oluşan bir cluster genelinde çalıştırmanıza olanak tanır. Kendi cluster yönetim sisteminizi yönetme yükü, bu sorumluluğu AWS'ye, özellikle AWS Fargate kullanımı yoluyla devrederek Amazon ECS hizmeti ile soyutlanır.

Docker, container'lar ve AWS Fargate gibi bazı bu terimlere yeniyseniz, bu hizmeti biraz daha kolay anlamanıza yardımcı olmak için bunların ne olduğunu hızlıca, tek bir cümlede tanımlayalım. **AWS Fargate**, ECS'nin container'lar için instance'ları ve cluster'ları yönetmek ve sağlamak zorunda kalmadan container'ları çalıştırmasını sağlayan bir motordur. **Docker**, uygulamaların Linux Container'ları içinde kurulumunu ve dağıtımını otomatikleştirmenizi sağlayan bir yazılım parçasıdır. Peki container'lar nedir? Bir **Container**, bir uygulamanın izole container paketi içinden çalışmasını sağlamak için gereken her şeyi içerir. Bu, sistem kütüphanelerini, kodu, sistem araçlarını, çalışma zamanını vb. içerebilir; ancak bir sanal makine gibi bir işletim sistemi içermez ve böylece container'ın kendisinin gerçek yükünü azaltır.

Container'lar, alttaki işletim sisteminden ayrılmıştır, bu da Container uygulamalarını bir bulut ortamında çok taşınabilir, hafif, esnek ve ölçeklenebilir hale getirir. Bu, uygulamanın dağıtım konumundan bağımsız olarak her zaman beklendiği gibi çalışacağını garanti eder. Bunu akılda tutarak, zaten Docker kullanıyorsanız veya yerel olarak paketlenmiş mevcut container'laştırılmış uygulamalarınız varsa, bunlar Amazon ECS'de sorunsuz bir şekilde çalışacaktır. Docker ve Container'lar hakkında daha fazla bilgi için, lütfen burada bulunan mevcut içeriğimize bakın. Şimdi EC2 Container Service'e ve sağladığı bazı ek işlevlere daha yakından bakalım.

Daha önce de belirttiğimiz gibi, EC2 Container Service, AWS Fargate ile etkileşimleri sayesinde kendi cluster yönetim sisteminizi yönetme ihtiyacını ortadan kaldırır. Hangi instance türünü kullanacağınızı bile belirtmeniz gerekmez. Bu çok zaman alıcı olabilir ve izlemeye, sürdürmeye ve ölçeklendirmeye devam etmek için çok fazla ek yük gerektirir. Amazon ECS ile cluster'ınız için herhangi bir yönetim yazılımı yüklemenize gerek yoktur, ayrıca herhangi bir izleme yazılımı yüklemenize de gerek yoktur. Tüm bunlar ve daha fazlası hizmet tarafından halledilir. Böylece harika uygulamalar oluşturmaya ve bunları ölçeklenebilir cluster'ınız genelinde dağıtmaya odaklanabilirsiniz.

ECS cluster'ınızı başlatırken iki farklı dağıtım modeli seçeneğiniz vardır: Fargate başlatma ve EC2 başlatma. 

- **Fargate başlatması** çok daha az yapılandırma gerektirir ve sadece gerekli CPU ve belleği belirtmenizi, ağ ve IAM politikalarını tanımlamanızı ve uygulamalarınızı container'lara paketlemenizi gerektirir. 
- **EC2 başlatmasıyla** çok daha geniş bir özelleştirme ve yapılandırılabilir parametre kapsamına sahip olursunuz. Örneğin, instance'larınızı yamalamak ve ölçeklendirmekten siz sorumlusunuz ve hangi instance türlerini kullandığınızı ve bir cluster'da kaç container olması gerektiğini belirtebilirsiniz.

Her iki mod için de kullanım durumları vardır. Güvenlik ve uyumluluk kontrolleri nedeniyle bazı cluster'larınızla daha fazla ayrıntıya ve kontrole ihtiyaç duyabilirsiniz. İzleme, container'larınızı ve cluster'ınızı metriklere karşı izleyecek olan AWS CloudWatch kullanılarak halledilir. CloudWatch'u daha önce kullananlarınız, bu metriklere dayalı olarak kolayca alarmlar oluşturabileceğinizi ve cluster boyutunuzun yukarı veya aşağı ölçeklenmesi gibi belirli olaylar meydana geldiğinde size bildirim sağlayabileceğinizi bileceksiniz. Bir Amazon ECS cluster'ı, EC2 instance'larının bir koleksiyonundan oluşur. Bu nedenle, bu kursta daha önce tartıştığımız işlevsellik ve özelliklerin bazıları bu instance'larla kullanılabilir. Örneğin, port ve protokol düzeyinde instance düzeyinde güvenliği uygulamak için Security Group'lar, Elastic Load Balancing ve Auto Scaling ile birlikte kullanılabilir. Bu EC2 instance'ları bir cluster oluştursa da, hala tek bir EC2 instance'ı gibi çalışırlar. Yani yine, örneğin, instance'larınızdan birine bağlanmanız gerekirse, hala SSH bağlantısı başlatmak gibi aynı tanıdık yöntemleri kullanabilirsiniz.

Cluster'ların kendileri, CPU ve bellek gibi kaynakları birleştiren bir kaynak havuzu olarak işlev görür. Cluster dinamik olarak ölçeklendirilebilir, yani cluster'ınızı tek bir küçük instance olarak başlatabilirsiniz, ancak dinamik olarak binlerce daha büyük instance'a ölçeklenebilir. Gerekirse cluster içinde birden fazla instance türü kullanılabilir. Cluster dinamik olarak ölçeklendirilebilir olsa da, yalnızca tek bir bölge içinde ölçeklenebileceğini belirtmek önemlidir. Amazon ECS bölgeye özeldir, bu nedenle birden fazla availability zone'u kapsayabilir, ancak birden fazla bölgeyi kapsayamaz. ECS ile container'larınızı, kaynak gereksinimleri veya birden fazla availability zone kullanımı yoluyla özel kullanılabilirlik gereksinimleri gibi farklı gereksinimlere dayalı olarak cluster'ınız genelinde dağıtmak için planlama yapabilirsiniz. Amazon ECS cluster'ındaki instance'larda ayrıca bir Docker daemon ve bir ECS agent yüklüdür. Bu agent'lar birbiriyle iletişim kurarak Amazon ECS komutlarının Docker komutlarına çevrilmesine olanak tanır.


## ECR: Elastic Container Registry

ECR hizmeti, daha önce bahsedilen EC2 Container Service ile yakından bağlantılıdır, çünkü uygulamalarınız genelinde dağıtılabilen ve konuşlandırılabilen docker image'larınızı güvenli bir şekilde depolamak ve yönetmek için bir konum sağlar.

ECR, tamamen yönetilen bir hizmettir ve sonuç olarak, bu docker image'larının kaydını oluşturmanıza olanak tanıyan herhangi bir altyapıyı tedarik etmeniz gerekmez. Tüm bunlar AWS tarafından tedarik edilir ve yönetilir. Bu hizmet öncelikle geliştiriciler tarafından kullanılır ve docker image'larının kütüphanesini merkezi ve güvenli bir konumda push, pull ve yönetme imkanı sağlar.

Hizmeti daha iyi anlamak için, kullanılan bazı bileşenlere bakalım. Bunlar: registry, authorization token, repository, repository policy ve image. Önce **registry**'ye bakalım. ECR registry, docker image'larınızı barındırmanıza ve depolamanıza, aynı zamanda image repository'leri oluşturmanıza olanak tanıyan nesnedir.

Hesabınız, registry içinde oluşturduğunuz herhangi bir image'a ve herhangi bir repository'ye varsayılan olarak hem okuma hem de yazma erişimine sahip olacaktır. Registry'nize ve image'larınıza erişim, daha sıkı ve katı güvenlik kontrolleri uygulamak için repository policy'lerin yanı sıra IAM policy'leri aracılığıyla da kontrol edilebilir. Docker komut satırı arayüzü, kullanılan farklı AWS kimlik doğrulama yöntemlerini desteklemediğinden, docker client'ınız registry'nize erişmeden önce bir AWS kullanıcısı olarak kimlik doğrulaması yapması gerekir, bu da client'ınızın hem push hem de pull image'ları yapmasına olanak tanır. Ve bu, bir authorization token kullanılarak yapılır. Docker client'ınızın varsayılan registry ile iletişim kurmasına izin vermek için yetkilendirme sürecini başlatmak üzere, AWS CLI kullanarak get login komutunu şu şekilde çalıştırabilirsiniz:

`aws ecr get-login --region region --no-include-email`

Bu komutdaki kırmızı "region" metni kendi bölgenizle değiştirilmelidir. Bu komut, bir docker login komutu olacak bir çıktı yanıtı üretecektir. Çıktı aşağıdakine benzerdir:

`docker login -u AWS -p password https://aws_account_id.dkr.ecr.region.amazonaws.com`

Ardından bu komutu kopyalamanız ve docker terminalinize yapıştırmanız gerekir, bu da client'ınızı kimlik doğrulaması yapacak ve bir docker CLI'yi varsayılan registry'nize ilişkilendirecektir. Bu işlem, registry içinde 12 saat boyunca kullanılabilecek bir authorization token üretir, bu noktada aynı süreci takip ederek yeniden kimlik doğrulaması yapmanız gerekecektir. Repository, registry'nizdeki farklı docker image'larını gruplandırmanıza ve güvence altına almanıza olanak tanıyan nesnelerdir. Registry içinde birden fazla repository oluşturabilir, böylece docker image'larınızı farklı kategorilerde düzenleyebilir ve yönetebilirsiniz.

Hem IAM hem de repository policy'lerini kullanarak, belirli kullanıcıların push veya pull gibi belirli eylemleri gerçekleştirmesine izin veren izinleri her repository'ye atayabilirsiniz. Az önce bahsettiğim gibi, repository'nize ve image'larınıza erişimi hem IAM policy'leri hem de repository policy'leri kullanarak kontrol edebilirsiniz. ECR'ye erişimi kontrol etmenize yardımcı olmak için bir dizi farklı IAM managed policy vardır, bunlar aşağıda listelenen üç tanesidir:

- `AmazonEC2ContainerRegistryFullAccess`

- `AmazonEC2ContainerRegistryPowerUser`

- `AmazonEC2ContainerRegistryReadOnly`

Repository policy'leri kaynak tabanlı policy'lerdir, bu da kimin erişime sahip olduğunu ve hangi izinlere sahip olduklarını belirlemek için policy'ye bir principal eklemeniz gerektiği anlamına gelir. Bir AWS kullanıcısının registry'ye erişim kazanması için ecr get authorization token API çağrısına erişiminin olması gerektiğinin farkında olmak önemlidir. Bu erişime sahip olduktan sonra, repository policy'leri bu kullanıcıların her bir repository üzerinde hangi eylemleri gerçekleştirebileceğini kontrol edebilir. Bu kaynak tabanlı policy'ler ECR'nin içinde ve sahip olduğunuz her bir repository'nin içinde oluşturulur. Registry'nizi, repository'lerinizi ve güvenlik kontrollerinizi yapılandırdıktan ve docker client'ınızı ECR ile kimlik doğrulaması yaptıktan sonra, docker image'larınızı gerekli repository'lerde depolamaya başlayabilir, gerektiğinde tekrar çekmeye hazır hale getirebilirsiniz.

ECR'ye bir image push etmek için docker push komutunu kullanabilirsiniz ve bir image'ı almak için docker pull komutunu kullanabilirsiniz. Hem push hem de pull işlemlerinin nasıl yapılacağı hakkında daha fazla bilgi için aşağıdaki bağlantıları kullanabilirsiniz:

Docker Push: [https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html)

Docker Pull: [https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-pull-ecr-image.html](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-pull-ecr-image.html)

### EKS: Elastic Kubernetes Service

Öncelikle, Kubernetes'e aşina olmayanlar için, basit düzeyde ne olduğunu kısaca açıklayayım. Kubernetes, konteynerleştirilmiş uygulamaları otomatikleştirmek, dağıtmak, ölçeklendirmek ve çalıştırmak için tasarlanmış açık kaynaklı bir konteyner orkestrasyon aracıdır. Onlarca, binlerce, hatta milyonlarca konteynere kadar büyüyecek şekilde tasarlanmıştır. Kubernetes ayrıca konteyner-runtime agnostic'tir, yani Kubernetes'i rocket ve docker konteynerlerini çalıştırmak için kullanabilirsiniz.

EKS'ye dönersek, EKS ile AWS, kontrol plane olarak adlandırılan Kubernetes yönetim altyapısını tedarik etmek ve çalıştırmakla uğraşmadan Kubernetes'i AWS altyapınız genelinde çalıştırmanıza olanak tanıyan yönetilen bir hizmet sunar. Siz, AWS hesap sahibi olarak, yalnızca worker node'ları tedarik etmek ve bakımını yapmakla sorumlusunuz.

#### Control plane nedir ve worker node'lar nelerdir?

**Kubernetes Control Plane:**

Control plane'i oluşturan bir dizi farklı bileşen vardır ve bunlar bir dizi farklı API, kubelet süreçleri ve Kubernetes Master'ı içerir ve bunlar kubernetes'in ve cluster'larınızın birbiriyle nasıl iletişim kurduğunu belirler. Control plane'in kendisi master node'lar üzerinde çalıştırılır.

Control plane, container'ları node'lara planlar. Planlama terimi bu bağlamda zamana atıfta bulunmaz. Bu durumda planlama, container'ları bildirilen hesaplama gereksinimlerine göre node'lara yerleştirme karar sürecini ifade eder. Control Plane ayrıca nesneleri sürekli olarak izleyerek tüm kubernetes nesnelerinin durumunu takip eder. Yani EKS'de AWS, ek dayanıklılık için birden çok availability zone kullanarak control plane'i tedarik etmek, ölçeklendirmek ve yönetmekten sorumludur.

**Worker node'lar:**

Kubernetes cluster'ları node'lardan oluşur ve cluster terimi tüm node'ların toplamını ifade eder. Node, Kubernetes'te bir worker makinedir ve on-demand EC2 instance olarak çalışır ve Kubernetes control plane tarafından yönetilen container'ları çalıştırmak için yazılım içerir. Oluşturulan her node için, güvenlik kontrolleri için docker ve kubelet'in yanı sıra AWS IAM authenticator'ın da yüklendiğinden emin olan belirli bir AMI kullanılır. Bu node'lar, müşteri olarak bizim EKS içinde yönetmekten sorumlu olduğumuz şeylerdir. Worker node'lar tedarik edildikten sonra bir endpoint kullanarak EKS'ye bağlanabilirler.

EKS hizmetini kullanmaya başlamak için gerekenleri şu şekilde listeleyelim:

1.  **EKS Service Role Oluşturun:** EKS ile çalışmaya başlamadan önce, EKS'nin belirli kaynakları tedarik etmesine ve yapılandırmasına izin veren bir IAM service-role yapılandırmanız ve oluşturmanız gerekir. Bu rolün yalnızca bir kez oluşturulması gerekir ve ileriye dönük olarak oluşturulan diğer tüm EKS cluster'ları için kullanılabilir. Rolün şu permissions policy'leri eklenmiş olması gerekir: `AmazonEKSServicePolicy` ve `AmazonEKSClusterPolicy`
2.  **EKS Cluster VPC Oluşturun:** AWS CloudFormation kullanarak, aşağıdaki şablona dayalı bir CloudFormation stack'i oluşturmanız ve çalıştırmanız gerekir: [https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/amazon-eks-vpc-sample.yaml](https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/amazon-eks-vpc-sample.yaml) Bu, EKS ile kullanmanız için yeni bir VPC yapılandıracaktır.
3.  **kubectl ve AWS-IAM-Authenticator'ı yükleyin: **Kubectl, Kubernetes için bir komut satırı yardımcı programıdır ve burada verilen ayrıntılar izlenerek yüklenebilir. IAM-Authenticator, EKS cluster'ı ile kimlik doğrulaması yapmak için gereklidir. Client OS'unuza bağlı olarak (Linux, MacOS veya Windows) buradan indirilebilir:
4.  **EKS Cluster'ınızı Oluşturun:** EKS konsolunu kullanarak, adım 1 ve 2'de oluşturulan VPC'den gelen bilgileri ve ayrıntıları kullanarak EKS cluster'ınızı oluşturabilirsiniz.
5. ** kubectl'i EKS için yapılandırın:** AWS CLI aracılığıyla update-kubeconfig komutunu kullanarak EKS cluster'ınız için bir kubeconfig dosyası oluşturmanız gerekir.
6.  **Worker Node'ları Oluşturun ve Yapılandırın:** EKS cluster'ınız 'Active' durumunu gösterdiğinde, aşağıdaki şablona dayalı olarak CloudFormation kullanarak worker node'larınızı başlatabilirsiniz: [https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/amazon-eks-nodegroup.yaml](https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/amazon-eks-nodegroup.yaml)
7.  **Worker Node'u EKS Cluster'a katılmak üzere yapılandırın:** Buradan indirilen bir configuration map kullanarak bu işlemi yapabilirsiniz:

	```
	curl -O [https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/aws-auth-cm.yaml](https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/aws-auth-cm.yaml)
	```

Bunu düzenlemeniz ve <ARN of instance role (not instance profile)> kısmını adım 6'daki NodeInstanceRole değeri ile değiştirmeniz gerekir.

EKS Cluster'ınız ve worker node'larınız artık uygulamalarınızı Kubernetes ile dağıtmaya hazır olarak yapılandırılmıştır.


## AWS Copilot

AWS Copilot, altyapı kurulumu ve dağıtımı için en iyi uygulama rehberliğini de içerirken, AWS üzerinde konteynerleştirilmiş uygulamaların geliştirilmesini, dağıtımını ve yönetimini basitleştiren bir komut satırı arayüzü veya CLI aracıdır. Copilot, Windows, Linux ve macOS için kullanılabilir. Copilot'u, Dockerfile'ı olan herhangi bir konteyner uygulamasını oluşturmak ve dağıtmak için kullanabilirsiniz. Copilot ile geliştiriciler, load balancer'lar gibi altyapıyı manuel olarak yapılandırmaya veya yönetmeye gerek kalmadan, sağlam, üretime hazır konteynerleştirilmiş uygulamaları Fargate üzerinde ECS'ye kolayca oluşturabilir, yapılandırabilir ve dağıtabilir.

Copilot'u kullanmak için, komut satırından copilot init komutunu çağırmanız yeterlidir. Ardından, uygulamanızın adı ve uygulamanızın özel bir backend servisi mi yoksa trafiğe hizmet etmesi gereken bir uygulama mı olduğu gibi birkaç basit soruyu yanıtlayın, Dockerfile'ınızın konumunu belirtin ve Copilot, hizmetinizi yönetmek için gerekli AWS altyapısını kuracaktır. Bundan sonra, uygulamanızı dağıtmayı seçebilirsiniz. Dağıtım sürecinin bir parçası olarak Copilot, image'ınızı oluşturacak, Elastic Container Registry'ye gönderecek ve Fargate üzerinde ECS'ye dağıtmaya başlayacaktır. Dağıtımınız tamamlandığında, Copilot üretime hazır hizmetinizin URL'sini çıktı olarak verecektir.

Özetlemek gerekirse, AWS Copilot, geliştiricilerin altyapıyı yönetmelerine gerek kalmadan üretime hazır konteynerleştirilmiş uygulamaları kolayca dağıtmalarını sağlayan bir CLI'dır.



