
# Storage (DVA-C02)

Bu başlık altındaki bilgiler, özellikle AWS Certified Developer - Associate sınavını geçmenize yardımcı olmak için hazırlanmıştır ve sınava hazırlık kapsamında AWS'deki çeşitli depolama hizmetleri hakkında daha fazla bilgi edinmek isteyen herkes için idealdir.

Reponun bu bölümünün amacı, geliştiriciler için AWS'deki depolama hizmetlerine giriş sağlamaktır. Bu hizmetler şunları içerir:

-   Amazon Elastic Block Store (Amazon EBS),
-   Amazon Elastic File System (Amazon EFS),
-   Amazon S3,
-   Amazon S3 Glacier.


AWS Certified Developer - Associate sertifikası, AWS hizmetlerini kullanarak uygulamaları geliştirme, test etme, dağıtma ve hata ayıklama konusunda bilgi ve deneyime sahip geliştirici rolündeki kişiler için tasarlanmıştır.

## Amazon S3'e Genel Bakış (Overview of Amazon S3)

Bu başlık altında, Amazon Simple Storage Service'i, yaygın adıyla S3'ü tanıyacağız.  Amazon S3, AWS tarafından sağlanan muhtemelen en yoğun kullanılan depolama hizmetidir. Bunun nedeni, birçok farklı kullanım senaryosuna uygun olması ve birçok AWS hizmetiyle entegre çalışabilmesidir. Amazon S3, tam yönetilen, nesne tabanlı, yüksek kullanılabilirliğe ve dayanıklılığa sahip, oldukça uygun maliyetli ve geniş çapta erişilebilir bir depolama hizmetidir.

Hizmetin kendisi, sınırsız depolama kapasitesine sahip olarak tanıtılmaktadır. Bu, Amazon S3'ü son derece ölçeklenebilir (scalable) kılar. Amazon S3, kendi yerel depolama çözümünüzden çok daha ölçeklenebilirdir. Ancak, destekleyebileceği tek bir dosyanın boyutu konusunda bazı sınırlamalar vardır. Desteklediği en küçük dosya boyutu **sıfır bayt**, en büyük dosya boyutu ise **beş terabayttır**. Bu maksimum dosya boyutu sınırlaması olsa da, çoğumuz bunu çoğu kullanım senaryosunda süregelen bir engel olarak algılamayacağız.

S3 hizmeti, nesne depolama hizmeti (object storage system) olarak çalışır. Bu, yüklenen her nesnenin bir dosya sistemi gibi hiyerarşik bir veri yapısına uymadığı, bunun yerine mimarisinin düz bir adres alanında var olduğu ve benzersiz bir URL ile referans edildiği anlamına gelir. Bunu, verilerinizin bir dizi dizin içinde ayrı dosyalar olarak depolandığı ve tıpkı kendi bilgisayarınızda olduğu gibi bir veri yapısı hiyerarşisi oluşturan dosya depolama ile karşılaştırırsanız, S3 çok farklıdır. S3 **bölgesel bir hizmettir** ve bu nedenle veri yüklerken, müşteri olarak sizin bu verinin yerleştirileceği bölgesel konumu belirtmeniz gerekir.

Verileriniz için bölgenizi belirttiğinizde, Amazon S3 daha sonra yüklenen verilerinizi hem dayanıklılığını (durability) hem de kullanılabilirliğini (availability) artırmak için o bölge içindeki birden fazla kullanılabilirlik bölgesinde (AZ) birden fazla kez depolar ve çoğaltır.

S3'te depolanan nesneler, on bir dokuzluk dayanıklılık olarak bilinen %99,999999999 dayanıklılığa sahiptir ve bu nedenle veri kaybetme olasılığı son derece düşüktür. Bu, S3'ün aynı verilerin kopyalarını farklı kullanılabilirlik bölgelerinde saklaması sayesindedir. S3 veri nesnelerinin kullanılabilirliği (availability), kullanılan depolama sınıfına bağlıdır ve bu %99,5 ile %99,99 arasında değişebilir.

Kullanılabilirlik ile dayanıklılık arasındaki fark şudur: Kullanılabilirlik açısından AWS, depolanan verilerinize erişmenizi sağlamak için Amazon S3'ün çalışma süresinin depolama sınıfına bağlı olarak %99,5 ile %99,99 arasında olmasını sağlar. Dayanıklılık yüzdesi, verilerinizin bozulma, veri degradasyonu veya diğer bilinmeyen potansiyel zararlı etkiler nedeniyle kaybolmadan korunma olasılığını ifade eder.

Amazon S3'e nesneler yüklerken, verilerinizi sabit bir adreste bulmak için spesifik bir yapı kullanılır. S3'te nesneleri depolamak için önce bir bucket (kova) tanımlamanız ve oluşturmanız gerekir. Bucket'ı verileriniz için bir konteyner olarak düşünebilirsiniz. Bu bucket adı sadece belirttiğiniz bölgede değil, küresel olarak var olan diğer tüm S3 bucket'larına karşı benzersiz (unique) olmalıdır çünkü oluşturduğunuz bucket gibi milyonlarcası vardır. Bu, sabit adres (flat address) alanından kaynaklanır. Basitçe açıklamak gerekirse, yinelenen bir ada sahip olamazsınız. Bucket'ınızı oluşturduktan sonra içine verilerinizi yüklemeye başlayabilirsiniz. Varsayılan olarak hesabınız en fazla 100 bucket'a sahip olabilir, ancak bu kesin bir sınır değildir. AWS'ye bu sayının artırılması için talepte bulunulabilir. Bucket'larınıza yüklenen her nesneye onu tanımlamak için benzersiz bir nesne anahtarı verilir.

Bucket'ınıza ek olarak, gerekirse nesnelerinizin kategorilendirmesine yardımcı olmak ve daha kolay veri yönetimi sağlamak için bucket içinde klasörler oluşturabilirsiniz. Klasörler veri organizasyonu açısından ek yönetim sağlayabilse de, Amazon S3'ün bir **dosya sistemi olmadığını** ve Amazon S3'ün birçok özelliğinin **belirli bir klasör seviyesinde değil, bucket seviyesinde çalıştığını** tekrar vurgulamak isterim. Bu nedenle, her nesne için benzersiz nesne anahtarı bucket'ı, mevcut klasörleri ve dosyanın kendisinin adını içerir.

## Amazon S3 Demo

Şimdi size Amazon S3 konsolunun hızlı bir use case sunacağım. Yeni bir bucket nasıl oluşturulur ve bu bucket'a nasıl bir nesne yüklenir, bunu göreceğiz. Ardından, bu nesnenin benzersiz nesne anahtarına bakacağız.

İlk olarak AWS yönetim konsoluna gidelim ve sonrasında, S3 servisine ait konsola geçelim. Burada varsa hesabınızda daha önce oluşturduğunuz bucket'ların listesini görebilirsiniz. Listelenen bucket tablosundaki sütunlara bakarsanız bucket adı için benzersiz bucket tanımlayıcısı olan bulunduğu bölge için sütunlarımız vardır. Bir önceki başlıkta dediğimiz gibi S3 bölge (regiosn) bazlı bir servistir. Eğer daha önce bazı bucket'lar oluşturduysanız ve bunlar farklı kullanılabilirlik alanlarında olabilir. Örneğin biri us-east-1'de, birinin us-west-2'de ve birinin de eu-west-2'de olduğunu görebilirsiniz. Hemen diğer sütunda, bucket'lar için erişim izinleri var. Bu konulara bucket erişimi ve güvenliği konusuna çok fazla değineceğiz, şimdilik sadece konsola genel bir bakış sağlayalım. Son olarak, bucket'ın ne zaman oluşturulduğunu gösteren bir sütunumuz var.

Şimdi, bu bucket'lardan birine girerseniz burada eğer oluşturduysanız bir veya daha fazla klasör görebiliriz. Yanındaki küçük klasör simgesinden bunun bir klasör olduğunu anlayabiliriz. Sonrasında içerisinde görsel olduğunu varsaydığımız bir klasöre girerseniz, bu klasörün içinde bir PNG dosyası olan bir nesne görebilmeniz muhtemeldir. Yani bu görsel, S3'teki seçtiğiniz bucket'a yüklediğim bir nesnedir.

Şimdi, bu görseli seçerseniz, hakkında bazı bilgiler görebilirsiniz. İndirebilir veya açabilirsiniz. Ayrıca en son ne zaman değiştirildiğini görebilirsiniz ve sayfada aşağı doğru kaydırırsanız, ait olduğu depolama sınıfını (storage class) ve herhangi bir server-side şifreleme etkinleştirilip etkinleştirilmediğini de görebilirsiniz. Eğer sayfada yukarı kaydırırsanız, dosya boyutunu ve dosya türünü de görebiliriz. Object review bağlığı altında 'key' adında bir değer göreceksiniz, işte bu değer nesnenin benzersiz tanımlayıcısı veya anahtarı olarak düşünülebilir. Görüldüğü gibi, anahtar bucket içindeki klasörden ve sonuna eklenen nesne adından oluşur. Sağ tarafta Object URL değeri göreceksiniz, bu değer ise nesnenin S3'teki benzersiz tanımlayıcısını gösterir.

Şimdi bu nesneyi açmak isterseniz, sadece sağ üstte bulunan 'Open' butonuna tıklamanız yeterlidir.  Ya da dilerseniz, nesneyi indirmeyi de seçebilirsiniz. Şimdi bucket'ınızın içinde nesneleri depolamak için klasörleri nasıl kullanabileceğinizi ve nesne anahtarının nasıl göründüğünü de biliyoruz. O halde devam edelim.

Şimdi S3 yönetim konsoluna dönüp bucket listenize giderseniz, size hızlıca yeni bir bucket nasıl oluşturulur onu aktarmak istiyorum. Ve bunu yapmak çok basit. Sayfanın sağ üstünde bulunan 'Create bucket' butonuna tıklayın. İlk olarak, bucket'ınıza bir isim vermeniz gerekiyor. Unutmayın, bu ismin küresel olarak benzersiz olması gerekiyor, bu yüzden buna 'fatihescertifiedprep' diyeceğim. Sonra, bu bucket'ın bulunmasını istediğiniz bölgeyi seçebilirsiniz. Bu bucket için us-east-2 bölgesini seçeceğim.

İsterseniz, mevcut bir bucket'tan ayarları kopyalamayı da seçebilirsiniz, ancak bu aşamada yeni bir bucket oluştururken sahip olduğunuz farklı seçenekleri gözden geçirebilirsiniz. Görüyorsunuz, bucket'ınıza yüklenen nesnelere kimin erişim kontrolü sağlayabileceğini belirleyen Erişim Kontrol Listelerini (Access Control List) veya ACL'leri etkinleştirebilir veya devre dışı bırakabiliriz. Hemen aşağıda bulunan 'Block Public Access settings fot this bucket' başlığı altındaki seçeneği kullanarak, tüm genel yani public erişimi engelleyebilirsiniz ki S3 bucket'larınız için neredeyse her zaman yapmak isteyeceğiniz bir durumdur bu. Bu yüzden bu varsayılan olarak her zaman 'Block all public access' işaretli olacaktır. Bu, VPC'niz dışından herhangi birinin bucket'ınızdaki herhangi bir veriye erişmesini engelleyecektir. Ayrıca sayfa da biraz daha aşağı inerek, aynı bucket'ta bir nesnenin tüm sürümlerini tutan sürüm oluşturmayı  (Bucket Versioning) etkinleştirebilirsiniz. Dilerseniz anahtar-değer çifti etiketleri ekleyebilir ve nesnelerimi varsayılan olarak şifreleyebilirsiniz.  Biraz daha aşağı inerseniz gelişmiş ayarları (advanced settings) başlığını görürsünüz. Bu alanda, nesnelerimin silinmesini veya üzerine yazılmasını engelleyebilecek nesne kilidini (object lock) de etkinleştirebiliriz. Şimdilik bunu varsayılan hali olan disable olarak bırakalım ve 'Create bucket' butonuna tıklayacağım.

İşte az önce oluşturduğum bucket olan fatihescertifiedprep listeleniyor. Ve bu bucket'ı seçerseniz, içinde henüz herhangi bir klasör veya nesne olmadığını görebiliriz. Bir klasör oluşturmak için, sağ üstte bulunan 'Create folder' butonunu kullanabiliriz. Bu klasörün adını 'Demo' olarak tanımlayalım. Burada benzersiz bir şifreleme anahtarı belirtebilir veya varsayılan şifreleme için bucket ayarını kullanacak olan varsayılanı bırakabiliriz. Tüm bunlardan sonra sayfanın altında bulunan buton yardımıyla klasörü oluşturabiliriz. Bu klasöre, yeni bir nesne eklemek istersek, ya doğrudan oluşturduğumuz bucket altına ekleyebiliriz ya da o klasöre girip oraya yükleyebiliriz. Bu kapsamda direkt bucket'a yükleyelim.

Bir nesne yüklemek için sadece sağ üstte gelip 'Upload' butonunu kullanıyoruz. Açılan sayfada, sayfanın üst kısmında konumlanan 'Add files' butonunu kulllanmanız yeterli olacaktır. Dosyanızı seçtikten sonra sayfada aşağıya doğru kaydırırsak, bucket'ın sürüm oluşturma, varsayılan şifreleme ve nesne kilidi ayarlarının yeni yüklenen nesnemizi nasıl etkileyeceğini görebiliriz. Ayrıca izinlere (permissions) daha detaylı bakabiliriz. Özellikler'i (proporties) başlığını genişletirseniz, depolama sınıflarının tam listesini görebilirsiniz. Depolama sınıfı seçiminiz, nesnenizin dayanıklılığını (durability), kullanılabilirliğini (availabity) ve ayrıca depolama maliyetini (cost store) etkileyecektir. Bir sonraki başlıkta farklı depolama sınıflarına daha derinlemesine gireceğiz. Bu demo kapsamında, bu alanı sadece standart depolama sınıfı olarak bırakabiliriz.

Sayfada daha da aşağı kaydırarak, sunucu tarafı şifreleme (server-side encryption), etiketler (tags) ve meta veriler (meta data) için tüm bu varsayılan ayarları olduğu gibi bırakalım ve sonra 'Upload' butonuna tıklayalım. 

Tüm bu işlemlerden sonra, bucket'ımızda yüklenen bu yeni nesneyi görebiliyoruz. Bu başlığın başında bahsettiğimiz gibi nesneye tıklarsanız, nesne anahtarını ve ayrıca benzersiz nesne URL'sini de görebilirsiniz.

### S3 Depolama Sınıfları (S3 Storage Classes)

Yeni nesneleri S3'e yüklerken, yüklediğiniz nesne için depolama sınıfını seçme seçeneğiniz vardır. Amazon S3, performans özellikleri ve maliyetler temelinde bir sınıf seçmenize olanak tanımak için bu farklı depolama sınıflarını sunar. Verileriniz için en uygun depolama sınıfını seçmek size kalmış.

Bu depolama sınıfları şu şekilde sıralanabilir:

-   S3 Standard
-   S3 Intelligent-Tiering
-   S3 Standard-Infrequent Access
-   S3 One Zone-Infrequent Access
-   S3 Glacier Instant Retrieval
-   S3 Glacier Flexible Retrieval
-   S3 Glacier Deep Archive

Hadi bu depolama sınıflarını daha ayrıntılı olarak inceleyelim. S3 Standard depolama sınıfı ile başlayalım.

**S3 Standard**, genel amaçlı bir depolama sınıfı olarak kabul edilir. Yüksek işlem gücü ve düşük gecikme süresi gerektiren, ancak verilerinize sık erişmeniz gereken durumlarda idealdir. Verileri birden fazla kullanılabilirlik bölgesine kopyalayarak, S3 tek bir kullanılabilirlik bölgesinin arızasına karşı koruma sağlayan %99,9999999 dayanıklılık (on bir dokuzluk) sunar. Ayrıca, yılda %99,99 kullanılabilirlik sunmaktadır. Güvenlik açısından, bu depolama sınıfı, depolanmış verilerin şifrelenmesi için farklı şifreleme seçenekleri sunan, aktarım sırasında şifreleme için SSL desteğine de sahiptir.

Yaşam döngüsü kuralları gibi yönetim özellikleri ile, S3 Standard'daki nesneler otomatik olarak bir depolama sınıfından başka bir depolama sınıfına taşınabilir. Yaşam döngüsü kısaca, verilerinizin Amazon S3 üzerinde depolanması sırasında verilerin yaşam dönemini otomatik olarak yönetmenize olanak tanırlar. Bir bucket'a bir yaşam döngüsü kuralı ekleyerek, nesnelerinizi bir sınıftan diğerine taşımak veya Amazon S3'ten tamamen silmek için belirli kriterler ayarlayabilirsiniz. Yaşam döngülerini, verilerinizi belirli bir süre sonra daha ucuz bir depolama sınıfına taşıyarak maliyet tasarrufu yapmak için yapabilirsiniz.

Bir sonraki sınıfımız, **S3 Intelligent-Tiering**'e bakalım. Bu depolama sınıfı, S3'te depoladığınız nesnelere ne sıklıkla erişeceğinizi bilmediğiniz durumlarda idealdir. Veri erişim modellerinin öngörülemez olduğu durumlarda, S3 Intelligent-Tiering'i kullanmak depolama maliyetlerinizi optimize etmeye yardımcı olabilir. 

S3 Intelligent-Tiering, nesnelerin erişim sıklığına bağlı olarak şu üç katmana otomatik olarak taşıyabilir:

- Sık erişim (Frequent access)
- Seyrek erişim (Infrequent access)
- Arşiv anlık erişimi (Archive instant access)

Bu katmanlar, daha önce listelediğim diğer S3 depolama sınıflarından bağımsızdır. Nesneler ilk kez S3 Intelligent-Tiering'e depolandığında, en pahalı olan sık erişim katmanına yerleştirilir. Nesne 30 gün boyunca erişilmezse, AWS onu daha ucuz olan seyrek erişim katmanına otomatik olarak taşır. Nesne 90 gün boyunca erişilmezse, AWS onu daha da ucuz olan arşiv anlık erişim katmanına taşır. Anlık erişimin gerekli olmadığı durumlarda, S3 Intelligent-Tiering'i 180 gün boyunca erişilmeyen nesneleri en ucuz olan deep archive access katmanına otomatik olarak taşıyacak şekilde yapılandırabilirsiniz.

Infrequent access veya archive instant access katmanlarındaki bir nesne yeniden erişildiğinde, otomatik olarak Frequent access katmanına geri taşınır. S3 Standard gibi, S3 Intelligent-Tiering de tek bir kullanılabilirlik bölgesinin kaybına karşı korumak için birden fazla kullanılabilirlik bölgesinde on bir dokuzluk dayanıklılık sunuyor. Ancak %99,9'luk kullanılabilirliği, %99,99 kullanılabilirliği olan S3 Standard'dan biraz daha düşüktür.

Bir sonraki sınıf, **S3 Standard-Infrequent Access**'dir. Bu, Intelligent Tiering sınıfındaki seyrek erişim katmanına eşdeğerdir, çünkü Standard sınıfındaki veriler kadar sık erişilmesi gerekmeyecek veriler için tasarlanmıştır, ancak yine de yüksek işlem gücü ve düşük gecikme süresi erişimi sunar. Diğer tüm S3 depolama sınıflarında olduğu gibi, birden fazla kullanılabilirlik bölgesine kopyalanarak kullanılabilirlik bölgesi arızalarına karşı koruma sağlayan on bir dokuzluk dayanıklılığa sahiptir. Intelligent-Tiering ile aynı %99,9 kullanılabilirliğe sahiptir. Sonuç olarak, bu depolama sınıfı S3 Standard'dan daha ucuzdur. Diğer sınıflaarda gördüğümüz gibi yine, aktarım sırasında şifreleme için SSL ve depolanan veriler için şifreleme seçenekleri desteklenir, ayrıca gereksinimlerinize göre nesneleri otomatik olarak başka bir depolama sınıfına taşımak için yaşam döngüsü kuralları gibi yönetim kontrolleri de mevcuttur.

Seyrek erişilen verileriniz için diğer bir ek seçenek ise, **S3 One Zone-Infrequent Access** sınıfıdır. **Önceden bahsettiğimiz sınıflar temelinde bu depolama sınıfının nasıl çalıştığını varsayabilirsiniz.** Seyrek erişimli bir depolama sınıfı olduğundan, sık erişilmesi beklenmeyecek nesneler için tasarlanmıştır. Aynı zamanda, S3 Standard-Infrequent Access ile aynı işlem gücü ve düşük gecikme süresine sahiptir. Ancak, on bir dokuzluk dayanıklılığına rağmen, yalnızca **tek bir kullanılabilirlik bölgesi** genelinde bulunmaktadır. Sınıfın adından da anlaşılacağı gibi, tek bir kullanılabilirlik bölgesini kullanır. Dolayısıyla nesneler, birden fazla kullanılabilirlik bölgesi genelinde değil, aynı kullanılabilirlik bölgesi içindeki farklı depolama konumlarına birkaç kez kopyalanır. Bu, S3 Standard'a kıyasla %20 daha ucuz depolama maliyeti sağlar. Ancak, verilerinizin yalnızca tek bir kullanılabilirlik bölgesinde depolanması nedeniyle,  %99,5 ile en düşük kullanılabilirlik düzeyini sunar. Verilerinizin depolandığı kullanılabilirlik bölgesi kullanılamaz hale gelirse, verilerinize erişemezsiniz. Hatta kullanılabilirlik bölgesinin yıkıcı bir olay sonucu tamamen yok olması durumunda, verileriniz tamamen kaybedilebilir. Tüm bunlarla beraber, aktarım sırasında ve depolanırken verilerinizi korumak için yaşam döngüsü kuralları ve şifreleme mekanizmaları mevcuttur.

Şimdi de **S3 Glacier** ile arşiv depolama seçeneklerimize bakalım. Ele alacağımız sonraki üç depolama sınıfı, S3 Glacier ile ilişkilidir. S3 Glacier, Amazon S3 servisinden ayrı olarak erişilebilir olsa da, onunla yakından çalışır. S3 Glacier depolama sınıfları, daha önce bahsettiğim Amazon S3 yaşam döngüsü kurallarıyla doğrudan etkileşim içindedir. Ancak, S3 Glacier depolama sınıflarının temel farkı, aynı verileri non-Glacier S3 depolama sınıflarında depolamaya kıyasla çok daha düşük maliyetli olmalarıdır. Peki neden?

Glacier, Amazon S3 kadar tüm özellikleri sunmaz, ama daha da önemlisi, verilerinize her zaman anlık erişim sağlamaz.

Peki S3 Glacier sınıfları tam olarak ne sunuyor? Uzun vadeli yedekleme ve arşivleme gereksinimleri için idealdir, uzun süreli ve son derece düşük maliyetli dayanıklı bir depolama çözümü sunarlar. Amazon S3'te depolanabilen tüm veri türlerini depolayabilir. Ancak, daha önce belirttiğim gibi, verilerinize her zaman **anlık erişim sağlamaz.** Hizmetin kendisi on bir dokuzluk dayanıklılığa sahiptir, bu da Amazon S3 kadar dayanıklıdır. Ve yine, tek bir bölgedeki birden fazla kullanılabilirlik bölgesine veri çoğaltılarak bunu başarır.  Amazon S3'e kıyasla depolamayı çok daha düşük maliyetle sağlar. Bunun nedeni, Glacier'de depolanan verilerin geri alınması işleminin her zaman anlık erişim sağlayan bir süreç olmamasıdır. Verilerinizi geri alırken, belirli kriterlere bağlı olarak **birkaç saate** kadar sürebilir.

Glacer içindeki veri yapısı, kasalar (vaults) ve arşivler (archives) etrafında merkezlenmiştir. Bucket'ler ve klasörler kullanılmaz. Bu iki yapı, yalnızca S3 için kullanılır. Bir Glacier vault'u, Glacier arşivleri için bir kap görevi görür. Bu kasalar bölgeseldir, dolayısıyla yeni bir kasa oluştururken bulunacağı bölgeyi belirtmeniz istenecektir. Bir kasanın içinde, verileriniz bir arşiv olarak saklanacaktır. Bu arşivler herhangi bir nesne olabilir. Bir Glacier kasasında sınırsız arşiv saklayabilirsiniz, yani kapasite açısından S3'te olduğu gibi verileriniz ve kasalarınız için sınırsız depolama alanına erişebilirsiniz. Ancak S3'ün bucket'ler ve klasörler aracılığıyla verilerinizi görüntülemenize, yönetmenize ve almak için mükemmel bir grafik kullanıcı arabirimi sunmasına karşılık, Amazon Glacier bu yeteneği sunmaz.

AWS yönetim konsolundaki Glacier kontrol paneli, kasalar oluşturmanızı, veri alma ilkelerini ayarlamanızı ve olay bildirimleri yapılandırmanızı sağlar. Verilerinizi ilk kez S3 Glacier'a taşımak, iki adımlı bir süreçtir. İlk olarak, arşivlerinizin toplanacağı bir kasa (vault) oluşturmanız gerekir. Bu, Glacier konsolu kullanılarak tamamlanabilir. Daha sonra verilerinizi, mevcut API'ler veya SDK'lar kullanarak kasanıza taşıyabilirsiniz. Verilerinizi Glacier'a taşımanın başka bir yolu da, daha önce bahsettiğimiz S3 yaşam döngüsü kurallarını kullanmaktır.

Arşivlerinizi geri almak için, yine kod yani API'ler, SDK'lar veya AWS CLI kullanmanız gerekir. Önce bir arşiv alma işi (archive retrieval job) oluşturacak, ardından tümünü veya bir kısmına erişim talep edeceksiniz.

S3 Glacier depolama sınıflarının üçünü de sırayla gözden geçirelim. İlk olarak, **S3 Glacier Instant Retrieval**'a bakalım. Bu sınıf, nadiren erişilen veriler için idealdir ve düşük maliyetli, dayanıklı ve yüksek güvenlikli bir depolama çözümü sunar. Diğer S3 depolama sınıflarıyla aynı dayanıklılığa sahiptir, çoklu kullanılabilirlik bölgelerinde (multi AZ) 11 dokuz, %99,9 kullanılabilirliğe sahiptir. S3 PUT API'lerini veya S3 yaşam döngüsü kurallarını kullanarak bu depolama sınıfına veri ekleyebilirsiniz. En iyisi, S3 Glacier Instant Retrieval sınıfını kullandığınızda, S3 Standard'da saklanan nesneler kadar hızlı - milisaniyeler içinde ve aynı verimlilikle - S3 Glacier'da saklanan nesneleri geri alabilirsiniz, bu da verilerinize hemen ihtiyaç duyduğunuz arşiv kullanım durumları için idealdir. Ancak, bu Glacier sınıfının **hızlı veri alımına izin veren tek sınıf** olduğunu unutmayın. Burada bahsedeceğim,z diğer tüm Glacier sınıfları, verilerinizi dakikalar-saatler içinde almanıza imkan sunar.

**S3 Glacier Flexible Retrieval** sınıfı, yılda yaklaşık bir kez erişilmesi gereken arşiv verilerinin daha düşük maliyetli depolanmasını sağlar. Verilerinizi ne kadar acil geri almak istediğinize bağlı olarak üç farklı geri alma seçeneği sunar, her biri farklı bir fiyat seviyesindedir.

- Bunların ilki **Expedited**'dir. Verilerinizi acilen almanız gerektiğinde, ancak talebinizin 250 megabayttan az olması gerektiğinde kullanılır. Verileriniz **bir ila beş dakika içinde** size sunulur. Beklendiği gibi, bu üç seçenek arasında en pahalı geri alma seçeneğidir.

- Sonraki seçenek **Standard**'tır. Arşivlerinizden herhangi birini geri almak için kullanılabilir, ancak verileriniz **üç ila beş saat içinde** kullanıma hazır olmayacaktır, yani expedited seçeneğine göre çok daha uzun sürecektir. Bu, üç seçenek arasında ikinci en pahalı seçenektir.

- Son olarak, **Bulk** seçeneği var. Bu seçenek, **petabaytlarca veriyi** aynı anda geri almak için kullanılabilir; ancak, bu genellikle **beş ila on iki saat arası** bir süre alır. Bu, geri alma seçenekleri arasında en ucuz olanıdır, ancak hangi seçeneği seçeceğiniz, ihtiyaç duyduğunuz veri miktarına ve ne kadar hızlı ihtiyaç duyduğunuza bağlı olacaktır, çünkü geri alma hızı ve maliyet, geri alma seçeneğinize göre belirlenecektir.

S3'ün sunduğu son depolama sınıfı **S3 Glacier Deep Archive**'dir. S3'ün sunduğu tüm depolama sınıfları arasında, Glacier Deep Archive **en ucuz** olanıdır. Glacier sınıfı olduğu için, uzun vadeli arşiv depolamaya odaklanır. Bu, finansal veya sağlık sektörleri gibi, verilerin yasal olarak yedi yıl veya daha uzun süre tutulması gereken durumlarda, minimum - hatta hiç - erişim ihtiyacı olduğu durumlarda idealdir. Çoklu AZ'ler genelinde %99,99 kullanılabilirlikle 11 dokuz dayanıklılık sağlar.

S3 Glacier Deep Archive'e veri ekleme, S3 PUT API'leri ve S3 yaşam döngüsü kuralları kullanılarak S3 Glacier ile aynı şekilde gerçekleştirilir. Glacier Deep Archive, verilerinizi ne kadar hızlı geri almanız gerektiğine bağlı olarak iki farklı geri alma seçeneği sunar. 
- Varsayılan **standart geri alma (Standard retrieval)** seçeneğini kullanırken, AWS, verilerinizin geri alınmasının 12 saat veya daha kısa sürede tamamlanacağını belirtir. 
- Daha düşük maliyetli **Bulk geri alma (Bulk retrieval)** seçeneği de var, ancak bu 48 saate kadar sürecektir.

Dolayısıyla, S3'te verileriniz için bir depolama sınıfı seçerken, kendinize şu soruları sormalısınız: 
- Verilerim ne kadar kritik? 
- Yeniden üretilebilir mi? 
- Gerekirse kolayca yeniden oluşturulabilir mi? 
- Ne sıklıkta erişmem gerekecek? 

<div align="center">

![daydJTb.md.png](https://iili.io/daydJTb.md.png)

</div>

Bu tablo, tüm S3 depolama sınıflarının ortak özelliklerini özetliyor. Gördüğünüz gibi, aralarındaki temel farklılıklar, kullanım durumu, kullanılabilirlik yüzdesi ve minimum depolama süresi, fiyatlandırmanın yanı sıra budur. 

### Versiyonlama - Sürüm Oluşturma (Versioning)

Bu başlık altında S3 Bucket özelliklerinin versiyonlama'yı (versioning) inceleyeceğiz.

Versiyonlama, aynı nesnenin birden fazla sürümünün var olmasına izin veren bir bucket özelliğidir. Bu, bir dosyanın önceki sürümlerini almanıza veya bir nesnenin kazara silinmesi / kasıtlı kötü niyetli silinmesi durumunda dosyayı kurtarmanıza olanak tanıması açısından faydalıdır.

Sürümleme etkinleştirilmiş bir bucket'da bir nesnenin üzerine yazdığınızda veya sildiğinizde nesnelere karşı otomatik olarak yönetilir. Bir nesnenin en güncel sürümüyle çalıştığınızdan emin olmak için, yönetim konsol içinde yalnızca nesnenin en son sürümü görüntülenir. Ancak, ihtiyaç duymanız halinde bir nesnenin tüm sürümlerini inceleyebilirsiniz.

Sürümleme varsayılan olarak etkinleştirilmemiştir, ancak etkinleştirdikten sonra **devre dışı bırakamazsınız.** Bunun yerine, sadece bucket'da bu özelliği **askıya alabilirsiniz (suspend)** ki bu da nesnelerinizin sürümünün daha fazla  oluşturulmasını engelleyecek, ancak askıya alınma noktasına kadar olan tüm mevcut nesne sürümlerini koruyacaktır. Bunu göz önüne alırsak, bucker'ınız şu 3 durumdan birinde olabileceği anlamına gelir: 
- Sürümsüz (unversionrd) (bu, bucket'ler için varsayılan durumdur), 
- sürümleme-etkin (versioning-enabled),
- sürümleme-askıya (versioning-suspended) alınmış.

Sürümleme etkinleştirildiğinde, aynı dosyanın birden fazla sürümünü depolayacağınız için ek depolama maliyetleri oluşacağını da unutmamalısınız ve S3'ün maliyet metriklerinden biri de ne kadar veri depoladığınızdır.

Şimdi sürümlemenin ne olduğuna dair daha net bir anlayışa sahip olduğumuza göre, sürece ve yapılandırmasına biraz daha derinlemesine bakalım.

Sürümlemeyi etkinleştirmek çok kolaydır ve bunu yapmanın birkaç yolu vardır. İlk olarak AWS Yönetim Konsolundan, bir bucket oluştururken etkinleştirebilir veya mevcut bir bucket'de etkinleştirebilirsiniz.

Yeni bir bucket oluştururken, 'seçenekleri yapılandır' (configure options) sırasında etkinleştirebilirsiniz. Yeni oluşturmakta olduğunuz bucket'inizin için etkinleştirmek için sadece onay kutusunu seçin.

Mevcut bir bucket'de etkinleştirmek için, S3 kontrol panelinden bucket'i seçin ve özellikler (properties) alt başlığına geçin. Bucket'in farklı özelliklerine ilişkin bir dizi kutucuk göreceksiniz, sürümleme (versioning) seçeneği devre dışı olarak görüntülenecektir. Kutucuğu seçin ve ardından sürümlemeyi etkinleştir yani 'enable versioning' seçenği tikleyin.

Bucket'inizde sürümlemeyi başarıyla etkinleştirdikten sonra, bucket'inize girdiğinizde yeni bir parametre değeri görünür olur, bu da 'versions' ve iki seçenek gösterir: 'Show' yani göster veya 'Hide' yani gizle. Varsayılan olarak gizle seçeneği seçilidir, ancak göster yapmanız durumunda nesnelerin versiyonlarını da görebilecek hale gelirsiniz.

Sürümleme etkinleştirildikten sonra yüklenen yeni nesneler için, nesne  yeni bir sürüm kimliği (Version ID) alacaktır. Eğer içinde zaten nesneler bulunan mevcut bir bucket'de sürümlemeyi etkinleştirdiyseniz, bu nesnelerin sürüm kimliği değeri değiştirilene veya silinene kadar 'null' olarak görüntülenecektir. Aynı nesne her değiştirildiğinde yeni bir sürüm kimliği alacak ve en son değişiklik bucket içinde görüntülenen tek nesne olacaktır. En güncel sürüm en üstte ve parantez içinde en son sürüm (latest version) notu ile tanımlanmıştır, yani nesnenin üzerine yazılmamıştır. Yeni bir sürüm kimliği ile yeni bir nesne oluşturulmuştur. Yukarıda bulunan versionu gösterip gizleme seçeneklerinden gizleyi seçerseniz, yalnızca bu sürüm görüntülenecektir.

Birden fazla sürümü gösteren bu görünümden, eski bir dosyadan kurtarmam gerekirse veya neyin değiştiğini görmek isterseniz, bu önceki sürümlerden herhangi birini açıp yükleyebilirsiniz. Bu görünüm, sürümleme kullanmanın, hızla değişen binlerce dosyanız varsa depolama maliyetlerinizin önemli ölçüde artabileceğini göstermeye yardımcı olur.

Sürüm etkinleştirilmiş bir bucket'de nesneleri sildiğinizde ne olduğunu açıklayayım. Bucket'deki sürümleri gizlediğinizi varsayarsak, yani sadece mevcut sürümleri gösteriyorsanız, nesne silinecek ve bucket'de görünmeyecektir. Ancak, nesne silinmemiştir. Sürümleri göstere geri tıklarsanız tıklarsam, şunu göreceksiniz ki delete marker ile tanımlanan yeni bir sürümünüz var.

Peki burada ne oldu? Temel olarak, bu silme işareti sürümü nesnenin mevcut sürümü haline gelir ve nesneye erişmek için yapılan herhangi bir GET isteğinin '404 Not Found' döndürmesini sağlar. Ancak, silinmek üzere gönderilen dosyanın aynı sürümü de dahil olmak üzere tüm diğer sürümler hala mevcuttur, dolayısıyla tüm sürümler gösterildiğinde konsoldan bu dosyaları hala görüntüleyebilir ve açabilirsiniz. Bununla beraber, belirli bir sürümü çağırmak için bir GET API çağrısında `versionID` değerini belirterek yapabilirsiniz.

Sürümlendirilmiş bir bucket'de bir nesneyi kalıcı olarak silmek istiyorsanız, tam olarak hangi sürümü silmek istediğinizi belirterek bir AWS SDK'sında `DELETE Object versionId` komutunu kullanmalısınız. Silme işareti olan sürümü silerseniz, nesne mevcut en son nesne sürümünü kullanarak bucket'inizde yeniden görünecektir.

### Sunucu Erişimi Log Kayıtları (Server-Access Logging)

Bu başlık altında server-access loggin'in ne olduğuna detaylı olarak bakacağız. Kısaca söylemek gerekirse, bir bucket'de server-access logging etkinleştirildiğinde, o bucket'e ve nesnelerine yapılan isteklerin ayrıntılarını yakalanır. Günlük tutma yani logging; güvenlik, olayları takiben kök neden analizi açısından önemlidir ve ayrıca belirli denetim ve yönetişim sertifikalarına uyum için gerekli olabilir.

Ancak, server-access logging garantili değildir. Log kayıtarı her birkaç saatte bir veya potansiyel olarak daha erken toplanır, sonrasında ise gönderilir. Her isteğin yakalanacağını ve belirli bir istek için belirli bir zaman dilimi içinde bir log kaydı alacağınızı dikte eden kesin bir kural yoktur.

Bucket'inizde access logging'i etkinleştirmek, S3 yönetim konsolunu kullanarak oldukça basit bir işlemdir.

Mevcut bir bucket'de server-access logging'i etkinleştirme öncelikle bucket'inizi seçin. Özellikler (properties) sekmesinden 'Server-access logging' kutucuğunu göreceksiniz. Varsayılan olarak, bu ayar devre dışıdır. Etkinleştirmek için sadece kutucuğu seçin. 'enable logging' seçeneğini seçin. Bu işlem size yapılandırmayı tamamlamak için 2 seçenek sunar. İlk olarak bir hedef bucket (target bucket) seçmeniz gerekiyor. Bu hedef bucket, kaynak bucket'inizde server-access logging'i etkinleştirerek oluşturulan günlükleri depolamak için kullanılacaktır. Log kayıtlarının depolanacağı bucket ile erişim kayıtlarını almak istediğiniz bucket **aynı bölgede** olmalıdır. Yönetim ve organizasyon için, ek olarak S3'ün kaynak (source) bucket'inizden gelen log kayıtlarına ekleyeceği bir hedef önek (prefix) ekleyebilirsiniz. Hedef bucket'inizi seçtiğinizde ve isteğe bağlı bir önek eklediğinizde, kaydet diyerek tüm işlemleri tamamlayabilirsiniz.

Ek olarak, bucket'inizin oluşturulması sırasında da günlük tutmayı etkinleştirebilirsiniz. Yine, bir hedef bucket ve isteğe bağlı bir önek (prefix) seçmelisiniz.

S3'ün hedef bucket'e log kayıtlarını yazmasına izin vermek için, elbette belirli izinler gerekecektir. Bu izinler, hedef bucket'lerinize log dosyaları teslim etmek için kullanılan önceden tanımlanmış bir Amazon S3 grubu olan Log Delivery grubu için yazma erişimi gerektirecektir. Erişim log kayıtlarının yapılandırması yönetim konsolu kullanılarak yapılandırılmışsa, log tutmanın etkinleştirilmesi otomatik olarak Log Delivery grubunu hedef kovanın ACL'sine (Erişim Kontrol Listesi) ekler ve ilgili erişime izin verir. Ancak, erişim günlüğünü S3 API'si veya AWS SDK'ları kullanarak yapılandırırsanız, bu izinleri manuel olarak yapılandırmanız gerekir.

Devam etmeden önce, sunucu erişim loglama yapılandırılmasıyla ilgili bilmeniz gereken bazı noktalar var. İlk olarak ve zaten bahsettiğim gibi, **hem kaynak hem de hedef bucket'ler** aynı bölgede olmalıdır ve her biri için farklı bucket'lerin kullanılması en iyi uygulamadır. Ayrıca, S3 Access Log Group izinleri yalnızca erişim kontrol listeleri (ACLs) aracılığıyla atanabilir ve bucket politikaları aracılığıyla atanmaz, bu nedenle bir SDK aracılığıyla bunun için izinleri manuel olarak ayarlarken ACL'yi güncellemeniz gerekir. Son olarak, hedef bucket'inizde şifreleme etkinleştirdiyseniz, erişim log kayıtları yalnızca bu SSE-S3 (S3 tarafından yönetilen sunucu tarafı şifreleme) olarak ayarlanmışsa teslim edilecektir, çünkü KMS-Key Management Service (Anahtar Yönetim Hizmeti) ile şifreleme desteklenmemektedir.

Günlükler hedef bucket'e gelmeye başladığında, isimler standart bir adlandırma modelini takip ederek sunulacaktır. S3 erişim log kayıtlarının bu örneğinde, 'logs' hedef öneki girdiğimizi düşünürsek, tüm log kayıtları bu önekle başlayacak. Ayarlanan herhangi bir öneki takiben, adlandırma kuralı şöyledir: YYYY-aa-GG-SS-DD-SS-BenzersizDizi/. Bu adlandırma; yılı, ardından ayı, ardından günü, ardından saat, dakika ve saniye cinsinden zaman ve son olarak günlük adlarının çoğaltılmasını önlemek için benzersiz bir string değerini temsil eder.

Şimdi bu log dosyalarından birinin içeriğine bakarak içerdikleri bilgileri anlamaya çalışayım.

<div align="center">

![dcO9kSn.md.png](https://iili.io/dcO9kSn.md.png)

</div>

Bu görsel, log kayıtlarından birindeki tek bir input örneğidir. Log kaydı boşlukla ayrılmış alanlarla sahiptir.

Boşlukları göz önüne alarak her bir bölüme ayırabilirsiniz, böylece nasıl oluşturulduğunu ve her bir öğenin neyi temsil ettiğini görebilirsiniz:

- **Bucket sahibi (Bucket Owner):** Kaynak bucket'in sahibinin kanonik kullanıcı kimliğini temsil eder. Kanonik kullanıcı kimliği, bucket politikaları (policy) aracılığıyla çapraz hesap erişimi (cross-account access) için kullanılır.

- **Kova (Bucket):** Bu, istekle ilgili bucket'in adını gösterir.

- **Zaman (Time):** Bu, UTC-Coordinated Universal Time (Koordineli Evrensel Zaman) cinsinden isteğin zaman damgasıdır.

- **Uzak IP Adresi (Remote IP Address):** İsteği gerçekleştiren kimliğin internet adresini temsil eder.

- **İstekte bulunan (Requester):** Kimliği doğrulanmış kullanıcılar için, bu alan IAM kimliğini gösterecektir. Kimliği doğrulanmamış herhangi bir kullanıcı için bunun yerine bir kısa çizgi (-) görüntülenecektir.

- **İstek Kimliği (Request ID):** Her isteği tanımlamak için rastgele bir dizi.

- **İşlem (Operation):** Bu, gerçekleştirilen isteğin işlemini gösterecektir.

- **Anahtar (Key):** İsteğin "anahtar" kısmı, URL kodlanmış, veya hiçbir anahtar parametresi kullanılmıyorsa bu örnekte olduğu gibi bir kısa çizgi görüntülenecektir. İsteğin herhangi bir alanındaki bir kısa çizgi, mevcut verinin bilinmediğini veya istek için geçerli olmadığını gösterir.

- **İstek URI (Request URI):** Bu, HTTP isteğinin request-uri öğesini temsil eder.

- **HTTP Durumu (HTTP Status):** Bu, istekten dönen HTTP durumunu sayısal bir değer olarak gösterir.

- **Hata Kodu (Error Code):** Bir hata yaşandıysa, S3 alınan hata kodunu döndürecektir.

- **Gönderilen Bayt (Bytes Sent):** Yanıt olarak gönderilen bayt sayısı.

- **Nesne Boyutu (Object Size):** İstekteki söz konusu nesnenin boyutu.

- **Toplam Süre (Total Time):** Milisaniye cinsinden ölçülür, isteği aldıktan sonra yanıtın son baytını göndermek için ne kadar süre geçtiğini temsil eder.

- **Dönüş Süresi (Turn-Around Time):** Bu, S3'ün isteği işlemesinin ne kadar sürdüğünü gösterir.

- **Referans (Referer):** Değer HTTP referans başlığından alınır, ancak bu örnek log kaydında bulunmadığı için, değer olarak bir kısa çizgi temsil edilir.

- **Kullanıcı Ajanı (User-Agent):** Bu, HTTP kullanıcı ajanı başlığından alınan değeri gösterir.

- **Sürüm Kimliği (Version ID):** Varsa, bu isteğin sürüm kimliğini gösterecektir.

- **Ana Bilgisayar Kimliği (Host Id):** x-amz-id-2 veya Amazon S3 genişletilmiş istek kimliği. x-amz-id-2 başlığı, AWS'nin sorunları gidermesine yardımcı olmak için x-amz-request-id başlığı ile birlikte kullanılan bir belirteçtir.

- **İmza Sürümü (Signature Version):** Bu, isteği doğrulamak için hangi imza sürümünün kullanıldığını gösterecektir.

- **Şifre Paketi (Cipher Suite):** SSL kullanıldıysa hangi şifre paketinin kullanıldığını gösterecektir. HTTP kullanıldıysa, bunun yerine bir kısa çizgi gösterilecektir.

- **Kimlik Doğrulama Türü (Authentication Type):** Bu, istek için kullanılan kimlik doğrulama türünü gösterir.

- **Ana Bilgisayar Başlığı (Host Header):** İstekte Amazon S3'e bağlanmak için kullanılan uç noktaları (end-points) temsil eder.

- **TLS Sürümü (TLS Version):** Bu, istemci tarafından hangi TLS sürümünün kullanıldığını gösterir.


### Nesne Düzeyinde Log Kaydı (Object-Level Logging)

Bu başlık altında, S3 bucket'larınızla ilgili object-level logging özelliklerine bakacağız.

Bu özellik aslında bir bakıma S3'ten çok **AWS CloudTrail** servisiyle ilgilidir çünkü Amazon S3 data event'lerine karşı logging aktivitelerini gerçekleştiren AWS CloudTrail'dir. Bu data event'leri, S3'te kullanılan `GetObject`, `DeleteObject` ve `PutObject` gibi spesifik API çağrılarıdır.

Peki CloudTrail nedir? CloudTrail, temel işlevi yapılan tüm AWS API isteklerini kaydetmek ve izlemek olan bir servistir. Bu API çağrıları, bir kullanıcının SDK kullanarak başlattığı programatik istekler, AWS command-line interface'i, AWS management console içinden veya başka bir AWS servisi tarafından yapılan bir istek olabilir.

Bir API isteği başlatıldığında, AWS CloudTrail isteği bir event olarak yakalar ve bu event'i S3'te depolanan bir log dosyasına kaydeder. Her API çağrısı, log dosyasında yeni bir event'i temsil eder. CloudTrail ayrıca tüm event'lerle ilişkili diğer tanımlayıcı metadata'ları da kaydeder. Örneğin, çağrıyı yapanın kimliği, isteğin başlatıldığı timestamp ve kaynak IP adresi gibi.

S3 data event'lerini yakalamak 2 şekilde yapılandırılabilir: İlk olarak, tüm veya bazı S3 bucket'larınız için data event'leri yakalamak istiyorsanız, bu işlevi AWS CloudTrail konsolunun kendisini kullanarak Trail'lerinizden birinin içinde yapılandırabilirsiniz. İkinci olarak, eğer AWS CloudTrail aracılığıyla bucket'ınız için zaten etkinleştirilmemişse, bunu Properties sekmesini kullanarak bucket seviyesinde yapılandırabilirsiniz. Object-level logging bölümünü seçmek, bunu yapılandırmak için size seçenekler sunacaktır.

AWS CloudTrail ile entegrasyonu nedeniyle, bir bucket için S3 data event'lerinizi yakalamak üzere aynı bölgeden mevcut bir trail seçmeniz istenecektir.  Ayrıca hangi tür event'leri yakalamak istediğinizi seçmelisiniz, sadece Read event'leri veya Write event'leri ya da her ikisini de seçerek ilerleyebilirsiniz. Seçiminizi yaptıktan sonra, Create'i seçin ve Object-level logging etkinleştirilecek ve AWS CloudTrail bu bucket'la ilişkili tüm S3 Data event'lerini yakalayacaktır.

### Transfer Hızlandırma (Transfer Acceleration)

Bu başlık altında, uzak mesafeden S3 veri transferlerinizi Transfer Acceleration kullanarak nasıl hızlandırabileceğinize değineceğim.

Uzak bir client'tan Amazon S3'e veya S3'ten dışarı ya da başka bir AWS bölgesine veri transfer ederken, transfer acceleration başka bir AWS servisi olan **Amazon CloudFront**'u kullanarak süreci önemli ölçüde hızlandırabilir.

Amazon CloudFront, temel olarak edge location'lar aracılığıyla dünya çapında trafik dağıtımı sağlayan bir **içerik dağıtım ağı (Content Delivery Network - CDN)** servisidir. AWS edge location'ları, dünya genelindeki büyük şehirlere ve yoğun nüfuslu bölgelere konuşlandırılmış sitelerdir.

Bucket seviyesinde transfer acceleration etkinleştirilmiş olarak client'ınızdan S3'e veri transfer ederken, istek CloudFront Edge Location'larından biri üzerinden geçecektir. Buradan transfer isteği, Amazon S3'e yüksek hızlı optimize edilmiş bir AWS ağ yolu üzerinden yönlendirilecektir.

Transfer acceleration kullanırken bir maliyet olduğunun farkında olmalısınız. Normal şartlarda internetten Amazon S3'e veri transferi ücretsizken, transfer acceleration ile hangi edge location'ın kullanıldığına bağlı olarak GB başına bir maliyet söz konusudur. Ayrıca, S3'ten dışarı, internete veya başka bir bölge'ye (region) transfer edilen veriler için de, yine edge location acceleration nedeniyle artan bir maliyet vardır.

Transfer acceleration'ı etkinleştirmek çok basittir. S3 konsolunda bucket'ınızı seçin, 'Properties'i seçin ve ardından Transfer Acceleration bölümünü seçin.

Daha sonra gerektiği gibi 'Enable' veya 'Suspend' edebilirsiniz. Ayrıca size bir Endpoint verileceğini de fark edeceksiniz.

Sonuç olarak, transfer acceleration'ı etkinleştirmek için bucket adınızın DNS uyumlu olması ve hiç nokta içermemesi gerekir. Ayrıca, transfer acceleration özelliğinin kendisinden yararlanmak için, bucket'a yapılan GET veya PUT gibi herhangi bir istek, bu yeni transfer acceleration endpoint'ini kullanmalıdır.

Transfer acceleration ile ilgili son bir konu ise, desteklemediği birkaç S3 işleminin olmasıdır. Bunlar: 
- GET Service (bucket'ları listeleme), 
- PUT Bucket (bucket oluşturma), 
- DELETE Bucket (bucket silme),
- PUT Object - Copy (bölgeler arası kopyalama).

### Erişimi Denetlemek İçin Poliçeleri Kullanma (Using Policies to Control Access)

Merhaba ve S3 kaynaklarına (hem S3 bucket'ları hem de bu bucket'ların içindeki nesneler dahil) erişim sağlamak için politikaları ve izinleri kullanmanın farklı yöntemlerini inceleyeceğimiz bu derse hoş geldiniz. S3 kaynaklarına erişimi hem **kimlik tabanlı politikalar (identity-based)** hem de **kaynak tabanlı (resource-based)** politikalar kullanarak kontrol edebiliriz.

**Kimlik tabanlı politikalar (identity-based policy)**, erişim gerektiren IAM kimliğine eklenir. Ardından in-line veya yönetilen (managed) IAM izin politikaları kullanılarak **kullanıcıya**, kullanıcının ait olduğu bir **gruba** veya bir **role** ilişkilendirilebilir. Kimlik tabanlı politikalar, politikadaki kaynağı tanımlar.

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Stmt1604440042841",
			"Action": "s3:*",
			"Effect": "Allow",
			"Resource": "arn:aws:s3:::s3deeplive"
		}
	]
}
```

Örneğin, burada gösterilen örnek politikada gördüğünüz gibi bucket adı'dır. Yani bu kimlik tabanlı politika bir kullanıcıyla ilişkilendirilebilir ve onlara s3deepdive bucket'ında tüm S3 eylemlerini kullanma izni verecektir.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1604440042841",
      "Action": [
        "s3:CreateBucket",
        "s3:DeleteBucket",
        "s3:PutObject"
        ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::s3deeplive",
      "Condition": {
       "IpAddress": {
         "aws:SourceIp": "10.1.0.0/16"
       } 
      }
    }
  ]
}
```
Elbette, kaynaklarınıza erişimi daha da yönetmek ve iyileştirmek için politikalarınızda koşullar kullanabilirsiniz. Örneğin, bu politikada kimlik, kaynak IP adresleri 10.1.0.0/16 CIDR bloğu aralığında olması koşuluyla s3deepdive bucket'ında `CreateBucket`, `DeleteBucket` ve `PutObject` eylemlerini gerçekleştirebilecektir.

**Kaynak tabanlı politikalar (source-based-policy)** ise, politikanın kimlik yerine bir kaynakla ilişkilendirilmesi bakımından farklılık gösterir. Yani Amazon S3 perspektifinden bakıldığında, kaynak tabanlı politikalar Access Control List'ler ve bucket politikaları şeklinde gelir. Bu nedenle, politikada kimin erişime izin verileceğini veya reddedileceğini tanımlamanız gerekir. Bu tanımlamalar, bucket politikaları mı yoksa Access Control List'ler mi kullandığınıza bağlı olarak farklı şekilde yönetilir.

Önce bucket politikalarına (bucket policies) odaklanayım. Bucket politikası JSON formatında yazılır, erişim kontrolünü sağlamanın ve kısıtlamanın başka bir yolu olarak doğrudan bucket'ınıza eklenir. Varsayılan olarak, bir bucket oluşturduğunuzda **hiçbir bucket politikası** yoktur.

Bir bucket politikası eklemek veya değiştirmek için bucket'ı seçmeli, izinlere gitmeli ve ardından bucket politikası bölümündeki düzenle butonunu kullanmalısınız. Buradan üç farklı seçeneğiniz vardır. JSON editor aracaılığıyla kendi bucket politikanızı serbestçe yazabilir, bazı politika örneklerini görüntüleyebilir veya bucket politikanızı oluşturmanıza yardımcı olması için AWS politika oluşturucuyu (policy generator) kullanabilirsiniz.

Bucket politikalarıyla çalışırken, ifadede tanımlanan eylem ve etkiyle ilişkili principal'ı tanımlayan bir temel öğeyi belirtmelisiniz. Daha net olması için bir örnek bucket politikasına bakalım. 

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1604440042841",
      "Action": [
        "s3:DeleteBucket",
        "s3:PutObject"
        ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::s3deeplive",
      "Principal": {
        "AWS": [
          "arn:aws:iam:730739171055:user/Fatih"  
        ]
      }
    }
  ]
}
```
Bu bucket politikası, s3deepdive bucket'ı için s3 DeleteObject ve s3 PutObject eylemlerine Fatih principal'ı için izin verir.

Daha önce gösterilen kimlik tabanlı politikada principal öğesini eklemem **gerekmediğini** fark edeceksiniz. Bucket politikasının ifadelerinde tanımlanan izinlerin bucket'a ve bucket içindeki nesnelere uygulandığını unutmayınız.

Şimdi, S3 verilerine erişimi kontrol etmek için hem IAM politikalarını hem de bucket politikalarını kullanabileceğimizin farkındayız, birini diğerine göre ne zaman kullanmanız gerektiğini merak ediyor olabilirsiniz. Buna daha ayrıntılı bir göz atalım. 

Erişim kontrol yöntemlerinizi tek bir serviste, yani IAM'de merkezi olarak yönetmek istiyorsanız IAM'i kullanmak isteyebilirsiniz. Ayrıca, çok sayıda bucket'a uygulanacak birden fazla izniniz varsa, bunu IAM erişimi üzerinden bir veya iki politika içinde yapmak, her bir bucket'ınıza eklemeniz gereken ayrı bucket politikaları oluşturmaktan daha kolay olabilir. Ayrıca IAM, politikaları içinde bir defada birden fazla servis için erişimi kontrol edebilme avantajına sahipken, bucket politikaları yalnızca S3 bucket'ı ve nesnelerine erişimi kontrol eder.

Öte yandan, güvenlik politikalarınızı yalnızca S3 içinde tutmak istiyorsanız bucket politikalarını kullanmak isteyebilirsiniz. Ayrıca, IAM içinde oluşturulan ve üstlenilen rolleri oluşturmak ve üstlenmek zorunda kalmadan bucket politikalarını kullanarak cross-account erişimi sağlayabilirsiniz. Tek yapmanız gereken bir bucket politikası oluşturmak ve bunu bucket'ınıza, örneğin s3deepdive'a eklemek, principal içindeki harici hesapla değiştirmektir. Harici hesapta, kullanıcıların bucket'a erişebilmek için sadece aynı bucket'a erişim yetkisi verilmesi gerekir. Ayrıca, bucket politikalarını kullanmak istemenizin bir başka nedeni de IAM politikalarının kullanıcılar için maksimum iki kilobayt, gruplar için beş kilobayt ve roller için 10 kilobayt boyutunda olabilmesidir. Ancak bucket politikaları 20 kilobayt boyutuna ulaşabilir.

Şimdi, elbette erişimi kontrol etmek için hem IAM politikalarını hem de bucket politikalarını kullanabilirsiniz. Bunlar birbirini dışlamaz. İkisi de birlikte erişimi kontrol etmek için kullanılabilir. S3 Access Control List'lere (ACL'ler) baktıktan sonra, daha önce de bahsettiğim gibi başka bir kaynak tabanlı erişim kontrol yöntemi olan, birden fazla erişim kontrolünün nasıl değerlendirildiğine ve mantığına daha yakından bakalım.

S3 Access Control List'ler veya ACL'ler, gruplama ve AWS hesapları aracılığıyla bucket'lara ve ayrıca bir bucket içindeki belirli nesnelere erişimi kontrol etmenizi sağlar. ACL'leri kullanabilmenin bir avantajı, nesne başına farklı izinler ayarlayabilmenizdir.

ACL'ler, IAM ve bucket politikaları tarafından tanımlanan politikalarla aynı JSON formatını izlemez. Bunun yerine, çok daha az ayrıntılıdır ve bucket seviyesinde veya nesne seviyesinde bir ACL uygulayıp uygulamadığınıza bağlı olarak farklı izinler uygulanabilir.

Bir ACL'nin temel yapısı nedeniyle, ACL'leri kullanarak erişimi üstü kapalı olarak (implicity) reddetmek mümkün değildir. Ayrıca, daha önce kimlik tabanlı erişimden bahsederken gördüğümüz gibi koşullu öğeler de uygulayamazsınız. Bir bucket için varsayılan bir ACL örneği gösterildiği gibi görünür.

<div align="center">

![d0USCrX.md.png](https://iili.io/d0USCrX.md.png)

</div>

Yukarıdaki görselde gördüğünüz gibi, Grantee sütunu tarafından tanımlanan farklı önceden tanımlanmış S3 grupları için farklı izinler belirleyebilirsiniz. İzinler, dört tane olan grantee'ye dayalıdır:

- **Bucket sahibi (bucket owner):** Bu, kendi AWS hesabınız olacak ve tüm nesneler ve bucket'ın kendisi üzerinde tam kontrole sahip olacaktır.
- **Herkes - genel erişim (public access):** Bu grantee'ye karşı ayarlanan izinler, nesne genel hale getirilmişse, herkesin uygulanan izinleri kullanarak erişebileceği anlamına gelir. Bu istekler imzalı, kimliği doğrulanmış veya imzasız, kimliği doğrulanmamış olabilir.
- **Kimliği doğrulanmış kullanıcılar (anyone with an AWS account):** Bu seçenek, herhangi bir AWS hesabından IAM kullanıcılarının imzalı istek (kimliği doğrulanmış) aracılığıyla nesneye erişmesine izin verecektir.
- **S3 log delivery grubu (S3 log delivery group):** Bu, sunucu erişim günlükleri yapılandırıldığında ve bucket log dosyalarını depolamak ve yazmak için kullanıldığında log dosyalarını teslim etmek için kullanılan bir gruptur.

ACL'i bucket seviyesinde düzenleyerek size şunları sunulacaktır. Bu, bucket'a uygulanabilecek farklı izin seviyelerini gösterir. Bucket için grantee gruplarına verilen izinleri gösterecektir, bunlar ya List ya da Write'dır. Ayrıca grantee'nin bucket ACL'sine karşı okuma veya yazma erişimini etkinleştirmek için hangi izinlerin verildiğini de görebilirsiniz.

Bu izinlerin aslında ne sağladığına dair açıklık getirmek gerekirse, şöyledir. 
- List, grantee'nin bucket'taki nesneleri listelemesine izin verir. 
- Write, bir grantee'nin bucket'ta nesne oluşturmasına, üzerine yazmasına ve silmesine izin verir. 
- Bucket ACL Read, grantee'nin bucket'ın ACL'sini okumasına izin verir. 
- Bucket ACL Write, grantee'nin bucket için ACL yazmasına izin verir.

Ayrıca bir grantee olarak başka bir hesap için erişim ekleyebileceğinizi fark etmiş olabilirsiniz. Bu erişimi yapılandırmak için, erişim sahibi olmasını istediğiniz AWS hesabının canonical ID'sini girmeniz gerekecektir. Bucket seviyesinde bir ACL'nin neye benzediğini gördük.

Şimdi de nesne seviyesinde neye benzediğine hızlıca bakalım. İşte bir bucket içindeki bir nesnenin ACL'si. Gördüğünüz gibi, yine varsayılan olarak kaynak sahibi nesne üzerinde tam kontrole sahiptir. Ayrıca, başka bir hesap için erişim ekleyebileceğinizi de fark edeceksiniz.

<div align="center">

![d0Ugrgt.md.png](https://iili.io/d0Ugrgt.md.png)

</div>

Yine, uygun canonical AWS hesap kimliğini ve ilgili izinleri eklemeniz gerekecek. Ayrıca, daha sonra daha fazla açıklayacağım genel erişimi etkinleştirdiyseniz genel erişimi de belirtebilirsiniz. Bu örnekte, herkes (everyone) grubunun nesneye okuma erişimi olduğunu görebiliyoruz.

Bu izinleri değiştirmek için grubun yanındaki radyo düğmesini tıklayıp ayarları gösterildiği gibi değiştirebilirim. Bucket ACL'lerinin aksine, nesne ACL'leriyle çalışırken yazma izni olmadığını unutmayın.

Yine, nesne için seçilen seçeneğe bağlı olarak hangi gerçek izinlerin uygulandığına daha yakından bakalım. 
- Read object, grantee'nin nesne verilerini ve meta verilerini okumasına izin verir. 
- Read object permissions, grantee'nin nesne ACL'sini okumasına izin verir. 
- Write object permissions, grantee'nin nesne için ACL yazmasına izin verir.

Böylece S3 bucket'larınıza ve nesnelerinize çeşitli izinlerle erişim kontrollerini uygulamak için bir dizi farklı yöntemi inceleme fırsatı bulduk. IAM politikalarına, S3 bucket politikalarına ve son olarak S3 Access Control List'lere baktık. Peki ya tüm bu erişim kontrol yöntemlerini kullansaydınız? Politika değerlendirme mantığı nasıl çalışırdı? Hangisi öncelikli olurdu?

Diyelim ki birisi, S3 ACL'lere ek olarak bucket izinleri eklenmiş ve ayrıca kendi IAM izinleri olan bir bucket'taki bir nesneye erişmeye çalışıyor. Erişmeye çalıştıkları bucket'taki nesnede çelişen izinler varsa, bu erişim nasıl yönetilir? Tüm bu politikalar birlikte görüntülenecek ve sonuçta ortaya çıkan erişimi belirleyecek ve en az ayrıcalık ilkesine uygun olarak herhangi bir izin çatışmasını ele alacaktır.

Temel olarak, varsayılan olarak AWS, herhangi bir politika içinde açık bir 'Deny' olmasa bile bir nesneye erişimin reddedildiğini belirtir. Erişim kazanmak için, principal'ın ilişkili olduğu veya bir bucket politikası veya ACL içinde tanımlandığı bir politika içinde bir 'Allow' olmalıdır. Tanımlanmış bir 'Deny' yoksa, ancak bir politika içinde bir Allow varsa, o zaman erişime izin verilecektir. Bununla birlikte, belirli bir nesneye principal ile ilişkili tek bir 'Deny' varsa, bir 'Allow' mevcut olsa bile, bu açık reddetme her zaman öncelik alacak ve Allow'u geçersiz kılacak ve erişime izin verilmeyecektir.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1604440042841",
      "Action": "s3:*",
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::s3deeplive"
    }
  ]
}
```
Örneğimize geri dönersek, Fatih adında bir kullanıcımız var ve aşağıdaki kimlik tabanlı politika ile ilişkilendirilmiş. s3deepdive bucket'ının da eklenen bir bucket politikası var. s3deepdive bucket'ındaki ACL izinleri, bucket sahibine bucket ve nesneleri üzerinde tam kontrol veriyor. Kullanıcı Fatih, bucket sahibiyle aynı AWS hesabının bir parçası.

Peki sonuçta ortaya çıkan izinler neler? IAM izinleri Fatih'e s3deepdive bucket'ı üzerinde tüm eylemleri gerçekleştirme yetkisi veriyor, bucket politikası Fatih'in s3deepdive bucket'ı içindeki bucket'ları ve nesneleri silme erişimini reddediyor. ACL ise, Fatih bucket sahibiyle ilgili hesabın bir parçası olduğu için erişime izin veriyor.

Bildiğimiz gibi, bir **Deny her zaman bir Allow'dan öncelikli olur**, yani Fatih s3deepdive bucket'ına erişebilecek ve bucket'ı veya herhangi bir nesnesini silmek dışında tüm S3 eylemlerini gerçekleştirebilecektir.

### S3 ile Çapraz Kaynak Kaynak Paylaşımı (CORS: Cross Origin Resource Sharing with S3)

Genel olarak, CORS bir web sayfasındaki belirli kaynakların kendi etki alanından farklı bir etki alanından istenmesine izin verir. Böylelikle CORS, client-side web uygulamaları oluşturmanıza olanak tanır. Ardından, gerekirse S3'te depolanan kaynaklara erişmek için CORS desteğinden yararlanabilirsiniz.

Tahmin edebileceğiniz gibi politikaların kullanımını içeren bir bucket için CORS'u nasıl yapılandıracağımıza bakalım. Bu politikalar, izinler sekmesi altında bulunabilen bucket'ın kendisinin CORS yapılandırmasına gömülüdür.

Tek bir kuralı olan bir örneğe bakalım. Aşağıdaki politika, [www.fatihes.com](https://www.fatihes.com) kaynağından PUT, POST ve DELETE kullanmanıza izin verir. Politikanın AllowedHeaders öğesi, bir ön kontrol isteğinde hangi başlıkların `Access-Control-Request-Headers` başlığı aracılığıyla izin verildiğini belirler. Bu başlık, tarayıcılar tarafından sunucuya, asıl istek yapıldığında client'ın hangi HTTP başlığını gönderebileceğini bildirmek için kullanılır. Bu politika kapsamında, tüm başlıkların bir ön kontrol isteğinde kullanılmasına izin verilecektir.

```json
[
  {
    "AllowedHeaders": [
      "*"
    ],
    "AllowedMethods": [
      "PUT",
      "POST",
      "DELETE"
    ],
    "AllowedOrigins": [
      "https://www.fatihes.com"
    ],
    "ExposeHeaders": []
  }  
]
```

Bu örneği kullanarak, bucket bir tarayıcıdan ön kontrol isteği aldığında, S3 CORS yapılandırması için bucket ile ilişkili politikayı değerlendirecek ve politikadaki ilk eşleşen kuralı işleyecektir. Kuraldaki aşağıdaki koşullar karşılandığında bir eşleşme yapılır.

- İstekte bulunanın Origin başlığı, AllowedOrigins öğesinde yapılan bir girişle eşleşir. 
- İstekte kullanılan yöntem, örneğin bir POST veya DELETE işlemi, AllowedMethods öğesinde eşleştirilir. 
- Son olarak, ön kontrol isteğiyle birlikte isteklerin Access-Control-Request-Headers başlığı içinde kullanılan başlıklar, AllowedHeader öğesindeki bir değerle eşleşir.

Politikadaki ExposeHeader öğesi, müşteri uygulamaları tarafından yapılmasına izin verilen yanıtta bir başlığı tanımlamak için kullanılır. 

CORS politikanız birden fazla kural içerebilir. Örneğin, aşağıdaki politika iki kural içerir. İlk kural daha önce baktığımızla aynıdır ve ikinci kural yalnızca aws.com kaynağını takiben PUT ve POST işlemlerine izin verir.

```json
[
  {
    "AllowedHeaders": [
      "*"
    ],
    "AllowedMethods": [
      "PUT",
      "POST",
      "DELETE"
    ],
    "AllowedOrigins": [
      "https://www.fatihes.com"
    ],
    "ExposeHeaders": [],
    "AllowedHeaders": [
      "*"
    ],
    "AllowedMethods": [
      "PUT",
      "POST",
    ],
    "AllowedOrigins": [
      "https://www.aws.com"
    ],
  }  
]
```

## Sunucu Tarafı Şifreleme Mekanizmalarına Genel Bakış (Overview of Server-Side Encryption Mechanisms)

Amazon S3'ü kullanırken verilerinizi korumak her zaman önceliğiniz olmalıdır. Geçmişte birçok kuruluşun yeterli güvenlik önlemlerini uygulamadığı ve sonuç olarak verilerinin tehlikeye girerek büyük olumsuz etkilere yol açtığı birçok örnek olmuştur.

Gereksinimlerinize bağlı olarak, bir şifreleme yöntemi diğerinden daha uygun olabilir. Sunucu tarafı şifreleme yöntemlerinin nasıl çalıştığına geçmeden önce, her mekanizmanın kısa bir özetine bakalaım.

- **S3 yönetimli anahtarlarla sunucu tarafı şifreleme (SSE-S3):** Bu seçenek minimal yapılandırma gerektirir ve anahtar rotasyonu gibi şifreleme anahtarlarının tüm yönetimi AWS tarafından gerçekleştirilir ve yönetilir. Tek yapmanız gereken verilerinizi yüklemektir ve S3 diğer tüm yönleri halledecektir. Ocak 2023 itibarıyla, yüklenen tüm nesneler varsayılan olarak bu seçenek kullanılarak şifrelenecektir.

- **KMS yönetimli anahtarlarla sunucu tarafı şifreleme (SSE-KMS):** Bu yöntem, Amazon S3'ün bir KMS anahtarı kullanarak veri şifreleme anahtarları oluşturmak için Key Management Service'i kullanmasına olanak tanır. KMS, anahtarınızın nasıl yönetildiği konusunda size çok daha fazla esneklik sağlar. AWS tarafından yönetilen bir KMS anahtarı kullanabilirsiniz (bu durumda rotasyon gibi görevler otomatik olarak yönetilir) veya anahtarınız üzerinde daha ayrıntılı kontrol istiyorsanız, müşteri yönetimli bir KMS anahtarı kullanabilirsiniz. Bunu yaparak, anahtar politikalarını kullanarak KMS anahtarını devre dışı bırakabilir, döndürebilir ve erişim kontrolleri uygulayabilirsiniz. AWS yönetimli (aws managed) bir KMS anahtarı veya müşteri yönetimli (customer managed) bir KMS anahtarı kullanmanızdan bağımsız olarak, anahtarın kullanımı **AWS CloudTrail** kullanılarak izlenebilir ve denetlenebilir.

- **Müşteri tarafından sağlanan anahtarlarla sunucu tarafı şifreleme (SSE-C):** Bu seçenek, AWS dışında zaten kullanıyor olabileceğiniz kendi şifreleme anahtarlarınızı kullanma fırsatı verir. Müşteri tarafından sağlanan anahtarınız verilerinizle birlikte S3'e gönderilir ve S3 şifrelemeyi sizin için gerçekleştirir.

Tamam, şimdi Amazon S3 tarafından sunulan 3 sunucu tarafı şifreleme türünü kısaca ele aldık, şimdi bunların her birinin verileri nasıl şifrelediğine ve şifresini çözdüğüne daha yakından bakalım, **SSE-S3 ile** başlayalım.

#### SSE-S3

Şifreleme süreci şu şekildedir:

1. İlk olarak, bir istemci (client) S3'e yüklenecek nesne verilerini seçer.

2. S3 daha sonra bu nesne verilerini alır ve S3 tarafından sağlanan düz metin (plain text) veri anahtarı kullanarak 256 bitlik Gelişmiş Şifreleme Standardı (Advanced Encryption Standard - AES-256) blok şifresi kullanarak şifreler. Bu, nesne verilerinin şifrelenmiş bir versiyonunu oluşturur ve ardından S3'te kaydedilir ve depolanır.

3. Ardından, S3 düz metin veri anahtarı bir S3 Master Key ile şifrelenir, bu da şifrelenmiş bir S3 Veri Anahtarı (S3 Data Key) oluşturur. Bu şifrelenmiş veri anahtarı da S3'te depolanır ve düz metin veri anahtarı bellekten kaldırılır.

Şifre çözme süreci şu şekildedir:

1. İstemci (client) tarafından S3'e şifrelenmiş nesne verilerini almak için bir istek yapılır.

2. S3, nesne verilerinin ilişkili şifrelenmiş S3 Data Key'i alır ve S3 Master Key ile şifresini çözer.

3. Bu S3 Plaintext Data Key'i daha sonra nesne verilerinin şifresini çözmek için kullanılır.

4. Bu düz metin nesne verileri daha sonra istemciye geri gönderilir.

Güvenlik en iyi uygulaması olarak ve ek güvenlik önlemlerini uygulamanın bir yolu olarak, **Amazon S3 varsayılan olarak** tüm bucket'lar için bir varsayılan **şifreleme ayarına** sahiptir. Daha önce açıklandığı gibi, bu varsayılan ayar için seçilen seçenek **SSE-S3**'tür ve bucket'a yüklenen **tüm nesnelere** uygulanacaktır. Gerekirse, varsayılan şifreleme seçeneğini SSE-S3'ten SSE-KMS'ye değiştirebilirsiniz. Bu, şifreleme anahtarları üzerinde daha fazla esneklik ve yönetim sağlamanıza olanak tanır.

Gördüğünüz gibi, bir bucket'taki varsayılan şifrelemeyi SSE-S3'ten SSE-KMS'ye değiştirmek, mevcut bir KMS anahtarı seçmenize veya yeni bir KMS anahtarı oluşturmanıza olanak tanır. Şimdi **SSE-KMS** şifreleme sürecinin nasıl çalıştığına bakalım.

#### SSE-KMS

Şifreleme süreci şu şekildedir:

1. İlk olarak, bir istemci Amazon S3'e nesne verilerini yükler.

2. S3 daha sonra KMS'ten bir Master Key ister.

3. Belirtilen KMS Key kullanarak KMS iki veri anahtarı oluşturur: bir düz metin veri (plaintext) anahtarı ve aynı Encrypted S3 Data Key.

4. Bu 2 anahtar daha sonra S3'e geri gönderilir.

5. S3 daha sonra nesne verilerini AES-256 ve düz metin veri anahtarı kullanarak şifreler. Bu, nesne verilerinin şifrelenmiş bir versiyonunu oluşturur ve daha sonra şifrelenmiş Data Key ile birlikte S3'te depolanır. Düz metin veri anahtarı (Plaintext Data Key) daha sonra bellekten kaldırılır.

Şifre çözme süreci şu şekildedir:

1. İstemci tarafından S3'e şifrelenmiş nesne verilerini almak için bir istek yapılır.

2. S3, nesne verilerinin ilişkili şifrelenmiş veri anahtarını bir şifre çözme isteği ile KMS'e gönderir.

3. KMS daha sonra şifrelenmiş veri anahtarı (data key) ile ilişkili KMS Key'i kullanarak şifresini çözer ve bir düz metin veri anahtarı (plaintext data key) oluşturur.

4. Bu düz metin veri anahtarı daha sonra Amazon S3'e geri gönderilir.

5. Düz metin veri anahtarı, şifrelenmiş nesne verilerinin şifresini çözmek için AES-256 şifreleme algoritmasını kullanır.

6. Düz metin nesne verileri daha sonra istemciye gönderilir.

SSE-KMS kullanırken, nesnelerinizi şifrelemek için KMS anahtarı için **kms:GenerateDataKey** izni ve ardından tekrar şifresini çözmek için **kms:Decrypt** iznine sahip olmanız gerektiğini anlamanız önemlidir.

Ayrıca, SEE-KMS kullanırken, S3'ten KMS'ye gelen istek ve trafik miktarını azaltarak maliyetlerinizi önemli ölçüde düşürmenize yardımcı olabileceği için bir bucket anahtarı (bucket key) uygulamak isteyebilirsiniz.

Şimdi bunun nasıl başarıldığını anlamak için altyapı içindeki yerine bakalım.

İlk olarak, yeni oluşturulan bir bucket'ta bucket anahtarlarını etkinleştirme sürecine bakalım.

1. Bir bucket oluşturulur ve bucket anahtarları etkinleştirilmiş SSE-KMS şifrelemesi ile yapılandırılır.

2. S3, KMS'ye belirli bir KMS anahtarı kullanarak yeni bir bucket anahtarına ihtiyaç duyduğunu belirten bir istek gönderir.

3. KMS, istekte belirtilen KMS anahtarını kullanarak yeni bir bucket anahtarı oluşturarak yanıt verecektir.

4. KMS, bucket anahtarını S3'e geri gönderecektir.

5. S3 daha sonra bu bucket anahtarını yeni oluşturulan bucket ile ilişkilendirecektir.

Bu aşamada artık bucket'ınızla ilişkilendirilmiş bir bucket anahtarımız var, peki KMS bucket anahtarı etkinleştirilmiş SSE-KMS kullanarak bu bucket'a bir **nesne eklemeye** çalıştığınızda ne olur? Bakalım:

1. Nesne, SSE-KMS kullanılarak S3 bucket'ına yüklenir.

2. S3, ilişkili S3 bucket anahtarını kullanarak 2 veri anahtarı oluşturur: bir düz metin veri anahtarı ve aynı veri anahtarının şifrelenmiş bir versiyonu.

3. S3 daha sonra nesne verilerini ve düz metin veri anahtarını birleştirerek şifrelemeyi gerçekleştirir. Bu, nesne verilerinin şifrelenmiş bir versiyonunu oluşturur ve daha sonra şifrelenmiş veri anahtarı ile birlikte S3'te depolanır.

4. Düz metin veri anahtarı daha sonra bellekten kaldırılır.

Gördüğünüz gibi, bucket anahtarını kullanarak, veri anahtarlarının KMS'deki KMS anahtarından oluşturulmasını istemeye gerek yoktur, bu KMS'ye yapılan istek sayısını azaltır ve dolayısıyla şifreleme maliyetlerinizi %99'a kadar düşürür. Bu, ayda şifrelenmiş milyonlarca nesneye erişiyorsanız çok büyük bir miktar olabilir. Anahtarlar bunun yerine S3 bucket anahtarınız tarafından oluşturulur ve böylece şifreleme süreci S3 içinde kalır.

Şimdi size şifre çözme sürecinin nasıl çalıştığını göstereyim:

1. İstemci tarafından S3'e nesne verilerini almak için bir istek yapılır.

2. S3, nesne verilerinin ilişkili şifrelenmiş veri anahtarını, bucket anahtarı ile birleştirerek bir düz metin anahtar oluşturur.

3. S3 daha sonra düz metin veri anahtarını şifrelenmiş nesne verileri ile kullanarak şifresini çözer.

4. Düz metin anahtar bellekten silinir.

5. Şifresi çözülmüş nesne verileri daha sonra S3 tarafından istemciye geri gönderilir.


Şimdi 3. ve son sunucu tarafı şifreleme seçeneğine, yani **SSE-C**'ye bakalım.

#### SSE-C

Şifreleme süreci şu şekildedir:

1. İlk olarak, bir istemci nesne verilerini müşteri tarafından sağlanan anahtarla birlikte S3'e HTTPS üzerinden yükler, bu **sadece HTTPS bağlantısı ile çalışır**, aksi takdirde S3 isteği reddeder.

2. Amazon S3 daha sonra müşteri tarafından sağlanan anahtarı kullanarak nesne verilerini AES-256 ile şifreler. S3 ayrıca gelecekteki doğrulama istekleri için müşteri tarafından sağlanan anahtarın bir salted HMAC değerini oluşturur. Şifrelenmiş nesne verileri, müşteri anahtarının HMAC değeri ile birlikte S3'te kaydedilir ve depolanır. Müşteri tarafından sağlanan anahtar daha sonra bellekten kaldırılır.

Şifre çözme süreci şu şekildedir:

1. İstemci tarafından S3'e nesne verilerini almak için HTTPS bağlantısı üzerinden bir istek yapılır. Aynı zamanda, müşteri tarafından sağlanan anahtar da istekle birlikte gönderilir.

2. S3, aynı anahtarın HMAC değerini kullanarak istenen nesnenin geçerliliğini onaylar.

3. Müşteri tarafından sağlanan anahtar daha sonra şifrelenmiş nesne verilerinin şifresini çözmek için kullanılır.

4. Nesne verileri daha sonra istemciye (client) geri gönderilir.


## İstemci Tarafı Şifrelemeye Genel Bakış (Overview of Client-Side Encryption)

Bazen, verilerinizi kendi ortamınızdan ayrılmadan önce şifrelemeniz gereken bir iş gereksinimi olabilir. Bu, verileriniz transit halindeyken ek güvenlik sağlar ve nesne Amazon S3'e ulaştığında server-side encryption ihtiyacını ortadan kaldırır. Şifrelenmiş bir nesne S3'e ulaştığında, servisin kendisi nesnenin şifrelenmiş olduğunu algılamaz, bunu sadece depolanması gereken nesne verisi olarak görür.

Client-side encryption kullanımını kolaylaştırmak için, nesne verilerinizi şifrelemek üzere benzersiz bir simetrik plaintext data encryption key üretecek olan Amazon S3 Encryption Client'ı kullanabilirsiniz. Bu plaintext data key daha sonra bir wrapping key kullanılarak şifrelenir, bu işlem envelope encryption olarak bilinir.

Önemli bir nokta, bu wrapping key için seçeneklerinizin olmasıdır; simetrik ve asimetrik anahtarlar kullanabilirsiniz. Ayrıca, wrapping işlemi için AWS KMS servisinden yararlanabilir veya kendi key management store'unuzdan kendi wrapping key'inizi kullanabilirsiniz. Wrapping key olarak KMS kullanmayı seçerseniz, bu şifreleme mekanizması Client-Side Encryption with KMS-Provided Keys veya kısaca CSE-KMS olarak adlandırılabilir. Kendi customer-provided key'inizi kullanmak isterseniz, bu bir raw AES-GCM key veya raw RSA key olmalıdır ve bu mekanizma Client-Side Encryption with Customer-Provided keys veya kısaca CSE-C olarak kabul edilir.

S3 encryption client, müşteriniz ile Amazon S3 arasında aracı görevi görür ve veri anahtarı (data key) oluşturma, nesne verisi şifreleme ve tercih ettiğiniz envelope encryption yöntemini (CSE-KMS/CSE-C) kullanarak data key'i wrapping işlemini sağlar. Bu encryption client'ın Java, GO, PHP ve daha fazlası dahil olmak üzere farklı dil uygulamalarıyla kullanılabileceğini de bilmelisiniz.

Şimdi, bu yöntemlerin her birini inceleyerek şifreleme ve şifre çözme sürecinin yüksek düzeyde nasıl çalışacağını göstereceğim.

Şifreleme süreci şu şekildedir:

1.  S3 encryption client kullanılarak bir nesneyi şifrelemek için bir istek yapılır.
2.  Nesneyi şifrelemek için benzersiz bir data key oluşturulur ve aynı data key'in bir kopyası KMS wrapping key tarafından şifrelenecektir.
3.  S3 encryption client'tan, data key kopyasının `kmsKeyId` builder parametresi tarafından belirtilen bir KMS key ile wrap edilmesi için bir istek gönderilir.
4.  Şifrelenmiş data key, KMS'den encryption client'a gönderilir.
5.  S3 encryption client, plaintext data key kullanarak nesne verisini şifreler.
6.  Benzersiz plaintext data key silinir ve bellekten kaldırılır.
7.  S3 encryption client daha sonra hem şifrelenmiş nesne verisini hem de şifrelenmiş data key'i Amazon S3'e yükler.
8.  S3 daha sonra şifrelenmiş nesne verisini ve ilişkili şifrelenmiş data key'i depolar.

Şifre çözme süreci şu şekildedir:

1.  S3 encryption client tarafından nesne verisini S3'ten almak için bir istek yapılır.
2.  Nesne verisi client'a gönderilir ve şifrelenmiş data key, şifreleme sürecinde kullanılan aynı `kmsKeyId` kullanılarak KMS'deki wrapping key'e gönderilir.
3.  Data key'in plaintext versiyonu client'a geri gönderilir.
4.  Plaintext data key daha sonra nesnenin şifresini çözmek için kullanılır.
5.  Plaintext data key daha sonra silinir ve bellekten kaldırılır.

CSE-C (Client-Side Encryption with Customer-Provided) için şifreleme süreci şu şekildedir:

1.  S3 encryption client kullanılarak bir nesneyi şifrelemek için bir istek yapılır.
2.  Nesneyi şifrelemek için benzersiz bir data key oluşturulur ve aynı data key'in bir kopyası customer-provided wrapping key tarafından şifrelenecektir.
3.  S3 encryption client'tan, data key kopyasının customer-provided key ile wrap edilmesi için bir istek gönderilir.
4.  S3 encryption client, plaintext data key kullanarak nesne verisini şifreler.
5.  Benzersiz plaintext data key silinir ve bellekten kaldırılır.
6.  S3 encryption client daha sonra hem şifrelenmiş nesne verisini hem de şifrelenmiş data key'i Amazon S3'e yükler.
7.  S3 daha sonra şifrelenmiş nesne verisini ve ilişkili şifrelenmiş data key'i depolar.

CSE-C (Client-Side Encryption with Customer-Provided) için şifre çözme:

1.  S3 encryption client tarafından nesne verisini S3'ten almak için bir istek yapılır.
2.  Nesne verisi client'a gönderilir ve şifrelenmiş data key, plaintext data key oluşturmak için customer-provided wrapping key'e gönderilir.
3.  Plaintext data key daha sonra nesnenin şifresini çözmek için kullanılır.
4.  Plaintext data key daha sonra silinir ve bellekten kaldırılır.

## Yaşam Döngüsü Yapılandırmaları (Lifecycle Configurations)

Aşağıdaki senaryoyu düşünün: Çok hızlı ve tutarlı bir şekilde büyüyen, versiyonlu bir S3 bucket'ında bir data lake iş yükünüz var. Her gün çok sayıda üzerine yazma (owerwrite) işlemi gerçekleşiyor ve büyük objeler multipart upload ile yükleniyor.

Büyük ihtimalle olası bir senaryoda, bu durum veri yönetimi perspektifinden, zaman zaman tamamlanmamış multipart upload'larınız ve verilerinizin çok sayıda güncel olmayan eski versiyonları olabileceği anlamına gelir. Bu data lake'i çalıştırmanın ne kadar maliyetli olabileceğini muhtemelen hayal edebilirsiniz.

Bu nedenle, elinizdeki her maliyet aracını değerlendirmeniz önemlidir. Bu araçlardan biri de verilerinizi daha düşük maliyetli storage class'lara taşıyarak veya artık ihtiyacınız olmayan verileri silerek, verilerinizin depolama yaşam döngülerini yönetmektir. Elbette verilerinizi manuel olarak taşıyabilir ve silebilirsiniz, ancak bu büyük ölçekte yönetilmesi zor olabilir. Bu süreci otomatikleştirmek için iki yol vardır:

- İlk yol, **S3 Intelligent-Tiering** depolama sınıfını (storage class) kullanmaktır. Aylık bir obje izleme ve otomasyon ücreti ödeyeceksiniz ve karşılığında S3 Intelligent-Tiering, obje erişim modellerinizi izleyecek ve objelerinizi otomatik olarak üç tier arasında taşıyacaktır: sık kullanılan (frequent), nadir kullanılan (infrequent) ve arşiv (archive). S3 Intelligent-Tiering, bilinmeyen veya öngörülemeyen veri erişim modelleri için önerilir ve veri yaşam döngüsünü yönetmek için daha elinizi taşın altına koymayadığınız bir yaklaşım sunması amaçlanmıştır.

- İkinci yaklaşım, **Yaşam Döngüsü konfigürasyonlarını (Lifecycle configurations)** kullanmaktır. Lifecycle konfigürasyonlarını, verileri daha düşük maliyetli bir S3 storage class'ına geçirmek veya verileri silmek için kullanabilirsiniz. Lifecycle konfigürasyonları ayrıca tamamlanmamış multipart upload'ları temizlemek ve verilerinizin güncel olmayan versiyonlarını yönetmek için seçenekler sunar. Bu hizmet günün sonunda, depolama harcamalarını azaltmaya yardımcı olur.

Lifecycle konfigürasyonlarını kullanmak, objelerinizin ve iş yüklerinizin (workload) tanımlanmış bir lifecycle'ı olduğunda ve öngörülebilir kullanım modellerini takip ettiğinde en maliyet etkin stratejidir.

Örneğin, tanımlanmış bir erişim modeli, S3'ü loglama için kullanmanız ve loglarınıza en fazla bir ay boyunca sık erişmeniz olabilir. Bu aydan sonra, gerçek zamanlı erişime ihtiyaç duymayabilirsiniz, ancak şirket veri saklama politikaları nedeniyle bir yıl boyunca silemezsiniz.

Bu bilgiyle, bu erişim modeline dayalı sağlam bir yaşam döngüsü konfigürasyonu oluşturabilirsiniz. Objeleri 30 gün sonra S3 standard storage class'ından S3 Glacier Flexible Retrieval storage class'ına geçiren bir S3 Lifecycle konfigürasyonu oluşturabilirsiniz. Objelerinizin storage class'ını değiştirerek, genel storage harcamalarınızda önemli maliyet tasarrufları görmeye başlayacaksınız. Ve 365 gün sonra, objeleri silebilir ve maliyetlerden tasarruf etmeye devam edebilirsiniz.

Verilerinizin çoğunun benzer bir erişim modelini izlediğini görebilirsiniz: Mesela, verilerinize gerçek zamanlı erişim ihtiyacınız yavaş yavaş azalır ve belirli bir süre geçtikten sonra verileri silebilirsiniz. Ya da bazı uyumluluk veya yönetişim düzenlemelerini karşılamak için kaydetmeniz gereken veya S3 Standard'dan arşiv depolamasına taşınabilen ve uzun süreler boyunca yalnız bırakılabilen verileriniz olabilir. Veya belki de S3 Standard depolamada çok sayıda objeniz var ve bu objelerin tümünü S3 Intelligent Tiering storage class'ına geçirmek istiyorsunuz.

Bu modeller kullanım senaryonuza benziyorsa, lifecycle konfigürasyonlarını kullanmak iş yükünüz için mantıklıdır.

Özetle, Lifecycle konfigürasyonları, objelerinizin eski kullanılmayan versiyonlarını silmenizi veya geçirmenizi, tamamlanmamış multipart upload'ları temizlemenizi, objeleri daha düşük maliyetli storage tier'larına geçirmenizi ve artık ihtiyaç duyulmayan objeleri silmenizi sağlayabilen önemli bir maliyet aracıdır.

## Yaşam Döngüsü Bileşenleri (Components of a Lifecycle Configuration)

Bir S3 Lifecycle konfigürasyonu teknik olarak bir XML dosyasıdır. AWS konsolunda lifecycle konfigürasyonları oluşturmak için XML bilmenize gerek olmasa da, XML formatı, çeşitli bileşenleri anlamanıza ve bunların perde arkasında nasıl çalıştığını görmenize yardımcı olur. Bu nedenle, bu başlık altında bir lifecycle konfigürasyonunun anatomisini XML kullanarak açıklayacağım.

Her lifecycle konfigürasyonu bir dizi kural içerir. Her kural dört bileşene ayrılır: ID, Filters, Status ve Actions.

```xml
<LifecycleConfiguration>
	<Rule>
		<ID>ProjectBlueRule</ID>
		<Filter>
			<And>
				<Prefix>ProjectBlue</Prefix>
				<Tag>
					<Key>Classification</Key>
					<Value>Secret</Value>
				</Tag>
			</And>
		</Filter>
		<Status></Status>
		Actions
	</Rule>
</LifecycleConfiguration>
```

**ID**, lifecycle kuralını benzersiz bir şekilde tanımlar - bunu kuralın adı olarak düşünebilirsiniz. Bu önemlidir, çünkü bir lifecycle konfigürasyonu 1000'e kadar kural içerebilir ve ID, hangi kuralın ne yaptığını takip etmenize yardımcı olabilir.

**Filters bölümü**, bucket'ınızdaki hangi objelere işlem uygulamak istediğinizi tanımlar. Tüm objelere veya bucket'taki objelerin bir alt kümesine işlemler uygulamayı seçebilirsiniz. Bir alt küme seçerseniz, prefix, obje etiketi veya obje boyutuna göre filtreleme yapabilirsiniz. Veya çok ayrıntılı olmak için, bu özelliklerin bir kombinasyonuna dayalı olarak filtreleme yapabilirsiniz. Örneğin, "ProjectBlue/" prefix'ine sahip tüm objeleri, aynı zamanda "Secret" Classification etiket değerine sahiplerse uygun diyebileceğiniz bir filtre oluşturabilirsiniz. Birden fazla filtreyi birleştirdiğinizde, XML'de "And" anahtar kelimesini kullanmanız gerektiğini unutmayın.

```xml
	<Filter>
		<ObjectSizeGreaterThan>500</ObjectSizeGreaterThan>
		<ObjectSizeLessThan>10000</ObjectSizeLessThan>
	</Filter>
```

Obje boyutu için, belirli bir boyuttan büyük, belirli bir boyuttan küçük veya iki boyut sınırı arasında bir aralıktaysa objeleri alabilir veya silebilirsiniz. Örneğin, objelerim 500 Byte'dan büyük ancak 10.000 Byte'dan küçükse geçiş yaptığım bir filtre oluşturabilirim. Maksimum filtre boyutunun 5TB olduğunu unutmayın.

```xml
<Status>Enabled</Status>
```

Bir sonraki bileşen status'tür yani durum alanı. Status alanı ile her lifecycle kuralını etkinleştirebilir ve devre dışı bırakabilirsiniz. Bu özellik, iş yükünüz için en iyi kuralları belirlerken lifecycle konfigürasyonlarını test ederken yararlı olabilir. Status'ü 'Disabled' olarak değiştirdiğinizde, S3 o kuralda tanımlanan işlemleri çalıştırmaz, temelde lifecycle işlemini durdurur. Ve objeleriniz üzerinde bu işlemleri tekrar çalıştırmaya hazır olduğunuzda, status'ü her zaman tekrar 'Enabled' olarak değiştirebilirsiniz.

Son ve belki de en önemli bileşen 'Actions' bölümüdür. Burada objelerinizi nereye taşımak istediğinizi tanımlarsınız. Örneğin, başka bir storage class'a mı taşıyorsunuz yoksa siliyor musunuz?

```xml
<Rule>
    <Transition></Transition>
    <Expiration></Expiration>
    <NoncurrentVersionTransition></NoncurrentVersionTransition>
    <NoncurrentVersionExpiration></NoncurrentVersionExpiration>
    <ExpiredObjectDeleteMarker></ExpiredObjectDeleteMarker>
    <AbortIncompleteMultipartUpload></AbortIncompleteMultipartUpload>
</Rule>
```

Kullanabileceğiniz altı ana işlem vardır: Transition, Expiration, NoncurrentVersionTransition, NoncurrentVersionExpiration, ExpiredObjectDeleteMarker ve AbortIncompleteMultipartUpload işlemi. İlk iki işlem geçiş işlemleri ve silme işlemleridir. 
- Geçiş işlemleri, verileri S3 storage class'ları arasında otomatik olarak taşımanızı sağlar.

- Silme işlemleri, S3'teki objelerinizin silinmesini otomatikleştirmenizi sağlar. Hem geçiş hem de silme işlemleri için, bu objeleri obje yaşına göre ne zaman taşıyacağınızı veya sileceğinizi tanımlayabilirsiniz. Ve yaş, objenin en son ne zaman oluşturulduğuna veya değiştirildiğine dayanır.

```xml
<LifecycleConfiguration> 
	<Rule> 
		<ID>ProjectBlueRule</ID> 
		<Filter> 
			<Prefix>ProjectBlue</Prefix> 
		</Filter> 
		<Status>Enabled</Status> 
		<Transition> 
			<Days>365</Days>
			<StorageClass>GLACIER</StorageClass> 
		</Transition> 
		<Expiration> 
			<Days>2555</Days> 
		</Expiration> 
	</Rule> 
</LifecycleConfiguration>
```

Yukarıda XML'de nasıl yapılandırıldığına dair bir örnek görüyorsunuz. Bu örnekte, ilk işlem ProjectBlue/ ile prefix'lenen tüm objeleri oluşturulduktan 365 gün sonra S3 Glacier Flexible Retrieval storage class'ına geçirir. İkinci işlem, 2.550 gün sonra - ki bu 7 yıldır, artık ihtiyaç duyulmadığı için ProjectBlue/ ile prefix'lenen objeleri siler.

Bucket'ınız için versiyonlama açıksa, geçiş işlemleri ve silme işlemleri yalnızca objenizin mevcut versiyonu için çalışır. Objenizin güncel olmayan versiyonlarını geçirmek istiyorsanız, NoncurrentVersionTransition işlemini kullanmalısınız.

Benzer şekilde, objenizin güncel olmayan versiyonlarını silmek istiyorsanız, NoncurrentVersionExpiration işlemini kullanmalısınız. Hem NoncurrentVersionTransition hem de NoncurrentVersionExpiration işlemleri için, objeleri iki şeye göre ne zaman taşıyacağınızı veya sileceğinizi tanımlayabilirsiniz.

- İlki, objenin güncel olmadığı günden bu yana geçen gün sayısıdır; Amazon bunu objenin üzerine yazıldığı veya silindiği günden bu yana geçen gün sayısı olarak hesaplar.
- İkincisi ise saklanacak maksimum versiyon sayısıdır. Bu, veri koruması için geri dönmek üzere birkaç versiyon kaydetmek isterken, depolama harcamalarından tasarruf etmek için objenizin eski versiyonlarını kaldırmak istediğinizde yararlıdır.

```xml
<LifecycleConfiguration> 
	<Rule> 
		<ID>ProjectBlueRule</ID> 
		<Filter> 
			<Prefix></Prefix> 
		</Filter> 
		<Status>Enabled</Status> 
		<NoncurrentVersionTransition> 
			<NoncurrentDays>30</NoncurrentDays> 
			<StorageClass>STANDARD_IA</StorageClass> 
		</NoncurrentVersionTransition> 
		<NoncurrentVersionExpiration> 
			<NewerNoncurrentVersions>3</NewerNoncurrentVersions> 
			<NoncurrentDays>730</NoncurrentDays> 
		</NoncurrentVersionExpiration> 
	</Rule> 
</LifecycleConfiguration>
```

Yukarıdaki xml dosyası hem güncel olmayan versiyon geçişi hem de güncel olmayan versiyon silme işlemi kullanan bir lifecycle konfigürasyonu örneğidir. Bu kuralda, tüm güncel olmayan versiyonlar güncel olmayan hale geldikten 30 gün sonra S3 Standard - Infrequent Access'e taşınır. Ek olarak, en son 3 versiyonu saklarken, güncel olmayan hale geldikten 730 gün veya iki yıl sonra tüm güncel olmayan versiyonları siler. 1 ile 100 arasında herhangi bir sayıda versiyon saklayabileceğinizi unutmayın.

Alabileceğiniz ek özel işlemler de vardır. Bunlardan biri Expired object delete marker işlemidir. Bu, sıfır versiyona sahip, yalnızca bir silme işaretçisi kalmış objeleriniz varsa yararlıdır. Bu, süresi dolmuş obje silme işaretçisi olarak adlandırılır ve bunları kaldırmak için bu işlemi kullanabilirsiniz.

Son olarak, AbortIncompleteMultipartUpload işlemi vardır. Temizlemeniz gereken tamamlanmamış çok parçalı yüklemeleriniz varsa, bu işlemi kullanmalısınız. Bu işlemle, çok parçalı yüklemelerinizin devam edebileceği maksimum süreyi gün cinsinden belirleyebilirsiniz. Örneğin, çok parçalı yüklemelerinizin 14 gün boyunca devam edebileceğini belirtebilirsiniz. Yükleme 14 gün içinde tamamlanmazsa, S3 onu silecektir.

### Yaşam Döngüsü Yapılandırma Demosu Oluşturma (Creating a Lifecycle Configuration Demo)

Bu başlık altında, hem AWS Console hem de AWS CLI kullanarak bir S3 Lifecycle konfigürasyonu yapılandıracağız. Bu S3 Lifecycle konfigürasyonu, bir S3 bucket'ındaki objelerin bir alt kümesini geçirecek.

Örnek senaryomuz şu şekilde olabilir; bir S3 bucket'ında, kedi fotoğrafları ve plaj fotoğrafları kombinasyonum var. Kedi fotoğraflarıma şu anda her zaman erişiyorum. Ancak plaj fotoğraflarım için, sadece ilk ay sık eriştiğimi fark ettim. Bu süreden sonra, birisi tatilim hakkında sorarsa diye onları bir yıl boyunca depolamada tutmak istiyorum. Ancak, maalesef ki hiç kimse sormuyor, bu yüzden ilk 30 günden sonra onları düşük maliyetli bir depolama sınıfında (storage class) saklayabilirim. Ve bir yıl sonra, yeni bir tatile çıkacağım, bu yüzden bu eski plaj fotoğraflarını silebilirim. Bu bilgileri kullanarak, bilinen erişim modellerime dayalı bir yaşam döngüsü konfigürasyonu oluşturabilirim.

S3 konsolunda başlayacağız ve lifecycle-configuration-demo-console adlı bucket'ıma tıklayacağım. Eğer böyle bir bucket'ınız yoksa oluşturup kedi ve sahil fotoğrafları yükleyebilirsiniz. Bucket'ınız içinde objelerinizi görebilirsiniz: birkaç kedi fotoğrafları ve sonra adlı bir klasörünüz var. Bu klasöre tıklarsanız, plaj fotoğraflarınızı görebilirsiniz.

<div align="center">

![dEpZl49.md.png](https://iili.io/dEpZl49.md.png)

</div>

Plaj fotoğraflarımı taşımak için bir lifecycle konfigürasyonu oluşturmak için, management sekmesine tıklayabilirsiniz ve lifecycle rules altında "create lifecycle rule" seçeneğine tıklayabilirim.

Açılan sayfada ilk olarak adı gireceğiz, bu durumda BeachPhotosRule olarak adlandırabiliriz. Ardından prefix'e göre sınırlandırmayı veya tüm fotoğraflara uygulamayı seçebilirim. Kedi fotoğraflarıma dokunmadan bırakmak istediğimiz için, bir filtreye göre sınırlandıracağız. Ve prefix altında, klasör adımı `beach/` olarak yazacağız.

Obje etiketine veya minimum ve maksimum obje boyutuna göre de filtreleme yapabileceğimizi fark etmiş olmalısınız. Ancak, bu demo için yalnızca prefix'e göre filtreleme yapacağız. Actions bölümüne geçerek, objelerimin nereye gideceğini seçebiliriz.

Bu durumda, bu bucket'ta versiyonlama açık değil ve çok parçalı yüklemeler kullanmıyoruz, bu yüzden sadece objelerin mevcut versiyonlarını geçirme seçeneğini (Move current versions of objects between storage classes) ve mevcut versiyonları silme (Expire current version of objects) seçeneğini seçmemiz yeterli olacaktır.

Plaj fotoğraflarımızı taşımak için, 30 gün sonra hangi storage class'a taşımak istediğimi seçebiliriz. Fotoğraflarıma nadiren erişeceğim için, bir arşiv depolama sınıfı en iyi seçenektir. Bu senaryo için Glacier Flexible Retrieval'ı seçecebiliriz. Sonrasında bu geçişin obje oluşturulduktan 30 gün sonra gerçekleşmesi gerektiğini belirteceğiz.

Bize, objelerimi Glacier'a geçirmek için obje başına bir ücret ödeyeceğimizi ve ayrıca ek bir obje metadata depolama ücreti ödeyeceğimizi söyleyen bir uyarı verecektir. Çünkü S3 hizmeti, objeler geri yüklenirken kullanılan obje metadata'sı için 32 KB depolama ekliyor. Ek maliyet getirecek olsa da, bu bizin için sorun değildir. Bu yüzden kutuyu işaretleyip devam edeceğiz.

Şimdi objelerimizin ne zaman sileceğimizi seçebiliriz. S3 Glacier Flexible Retrieval'ın 90 günlük bir saklama süresi var, bu yüzden bu süre 90 günden uzun olduğu sürece ek bir ücret ödemeyeceğiz. Tatillerimizi yıllık olarak yaptığımızı düşünürsek, fotoğraflarımızı silmek için 365 gün seçebiliriz.

Tüm tanımlama ve ayarlarımızı yaptığımıza göre, "create rule" butonuna tıklamadan önce tüm doğru seçimleri yaptığımızdan emin olmak için inceleyebileceğimiz bir zaman çizelgesi oluşturuluyor. Burada tüm seçimlerimizi tekrar gözden geçirebiliriz.

<div align="center">

![dEpyupe.md.png](https://iili.io/dEpyupe.md.png)

</div>

Şimdi, AWS konsolunda tıklamayı sevmeyenler için: CLI kullanarak bir yaşam döngüsü konfigürasyonu oluşturmak da aynı derecede kolaydır. CLI kullanarak lifecycle konfigürasyonları yapılandırma hakkında bilmemiz gereken tek şey, konfigürasyonunuzu tanımlayan bir JSON şablon dosyası sağlamanız gerektiğidir. XML'e daha aşina iseniz, her zaman XML olarak yapılandırabilir ve ücretsiz XML'den JSON'a dönüştürme web sitelerinden herhangi birini kullanarak dönüştürmeyi yapabilirsiniz.

```json
{
  "Rules": [
    {
      "Filter": {
        "Prefix": "beach/"
      },
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 30,
          "StorageClass": "GLACIER"
        }  
      ],
      "Expiration": {
        "Days": 365
      },
      "ID": "BeachPhotosRule"
    }  
    
  ]
}
```

Ancak, ben zaten bir JSON dosyası oluşturdum ve hızlıca üzerinden geçelim. BeachPhotosRule değeri kuralın ID'si veya adı olarak düşünülebilir. Sonra beach/ klasörümüz prefix'ini kullanan filtre bulunuyor. Status enabled olarak ayarlı yani şu an bu konfigürasyon dosyamız çalışacaktır. 30 gün sonra S3 Glacier Flexible Retrieval storage class'ına bir geçişimiz var. Ve plaj fotoğraflarımızı 365 gün sonra siliyoruz.

Gördüğünüz gibi, bu tam olarak aynı lifecycle konfigürasyonu. Terminale giderek, aşağıdaki komutu kullanabiliriz.

```bash
aws s3api put-bucket-lifecycle-configuration --lifecycle-configuration file://lifecycle.json --bucket=lifecycle-configuration-demo-cli
```

Doğrulamak için, S3 konsoluna geri dönebiliriz, demo-cli bucket'ımızı bulabiliriz. Management sekmesine tıklayıp, lifecycle configuration'a tıklayıp, detayların doğru olduğunu doğrulayabiliriz. 

### Diğer Konular ve Sınırlamalar (Other Considerations and Limitations)

Bu başlık altında, S3 storage class'larını bir merdiven olarak düşünelim. S3 Standard storage merdivenin en üstünde, S3 Glacier Deep Archive ise merdivenin en altında. Ve diğer tüm storage class'lar bunların arasında konumlanmış durumda olmalıdır.

<div align="center">

![dEyBbQS.md.png](https://iili.io/dEyBbQS.md.png)

</div>

Lifecycle konfigürasyonlarıyla bu merdiven sadece tek yönlü çalışır: yukarıdan aşağı doğru. Verileri merdivenden aşağı, daha düşük maliyetli bir storage class'a geçirdiğinizde, objeleri geri yukarı taşıyamazsınız. Örneğin, diyelim ki verilerimizi S3 Standard-Infrequent Access'e taşıdık. Verilerim bu storage class'a geçtikten sonra, bir lifecycle konfigürasyonu kullanarak verilerimizi S3 Standard'a geri taşıyamayız. Bu, verilerimizi S3 One Zone - IA'ya taşırsak da geçerlidir, onu S3 Intelligent-Tiering, S3 Standard-IA veya S3 Standard'a geri taşıyamayız.

Bu, arşiv depolama kullanıyorsak önemli hale gelir. Verilerimizi en düşük maliyetli storage class olan S3 Glacier Deep Archive'a taşıyarak merdivenin en altına ulaşırsak, verilerimizi başka herhangi bir storage tier'a geri geçiremeyiz.

Lifecycle Configuration maliyetleri benzer bir merdiven modelini takip eder. Bu maliyetleri iki şekilde kategorize edelim: 
- Minimum depolama süresi ücretleri (Minimum Storage Duation Fees),
- Depolama geçiş maliyetleri (Storage Transition Costs) 

Her ikisi de merdivenden **aşağı indikçe artar.** Örneğin, depolama geçiş maliyetlerini ele alalım. Verileri diğer storage class'lara taşıdığınızda ücretlendirilirsiniz ve bu ücret merdivenden aşağı indikçe artar. Örneğin, merdivenin en üstünde, objeler S3 Standard'dan S3 Standard-IA storage class'ına taşındığında her 1.000 lifecycle geçiş isteği için 0,01$ ücretlendirilirsiniz. Merdivenden aşağı, S3 Glacier Deep Archive'a kadar indiğinizde, bu maliyet artar ve her 1000 geçiş isteği için 0,05$'a kadar çıkabilir.

Büyük bir maliyet gibi görünmese de, özellikle sürekli olarak verileri arşiv depolamasına taşıyorsanız, zamanla birikebilir. Örneğin, milyonlarca küçük objeyi arşiv depolamasına geçirmeniz gerekiyorsa, bu geçiş maliyeti çok yüksek olabilir. Bu maliyeti en aza indirmek için, çoğunlukla uzun süreler boyunca saklanması gereken büyük objeleri geçirmeyi düşünmelisiniz. Ayrıca bu ücretten tasarruf etmek için birkaç küçük objeyi bir büyük obje halinde birleştirmeyi de düşünebilirsiniz.

Lifecycle konfigürasyonlarıyla ilgili ikinci maliyet faktörü minimum depolama süresi ücretleridir. Çoğu storage class, verileri silmeden, üzerine yazmadan veya bu objeleri taşımadan önce belirli bir süre boyunca bir storage class'ta tutmanızı gerektiren **minimum bir depolama süresine** sahiptir.

Bu minimum depolama süreleri de merdivenden aşağı indikçe artar. Örneğin, S3 Standard ve S3 Intelligent-Tiering'in minimum depolama süresi yoktur. S3 Standard-IA ve S3 One Zone - IA gibi seyrek erişim tier'ları 30 günlük minimum depolama süresine sahiptir. S3 Glacier Instant Retrieval ve S3 Glacier Flexible Retrieval gibi arşiv depolama tier'ları 90 günlük minimum depolama süresine sahipken, S3 Glacier Deep Archive 180 günlük minimum depolama süresine sahiptir.

Peki, minimum depolama süresine ulaşılmadan bu objeleri siler veya üzerine yazarsanız ne olur? Ücretlendirilirsiniz. Örneğin, diyelim ki bir objeyi 30 gün boyunca S3 Glacier Deep Archive'a geçirdiniz ve sonra sildiniz. Bu durumda, yine de tam 180 günlük depolama için ücretlendirileceksiniz.

Bu yüzden lifecycle konfigürasyonlarınızı ayarlarken, sınırlamaları ve maliyetleri göz önünde bulundurduğunuzdan emin olun. 

## Amazon Elastic Block Store (EBS)

Bu başlık altında, EC2 instance'lerinize EBS birimleri aracılığıyla depolama sağlayan ve bazı EC2 örnekleriyle kullanılan örnek depolama birimlerinden farklı avantajlar sunan Amazon Elastic Block Store hizmetine daha yakından bakacağız.

EBS, kalıcı ve dayanıklı blok düzeyinde depolama sağlar. Sonuç olarak, EBS volume'leri, örnek depolama volume'lerinde depolanan verilerle karşılaştırıldığında veri yönetimi konusunda çok daha fazla esneklik sunar. EBS volume'leri, EC2 örneklerinize eklenebilir. Özellikle hızla değişen ve belirli bir Saniye Başına Giriş/Çıkış İşlemi (IOPS - Input/Output operations Per Second) oranı gerektirebilecek veriler için kullanılır.

EBS volume'leri EC2 instance'sinden bağımsızdır, yani iki farklı kaynak olarak var olurlar. Örnek depolama volume'leri gibi doğrudan bağlı olmak yerine mantıksal olarak instance'a bağlanırlar.

EBS'nin veri kalıcılığını (persistence of data) sağlama yeteneği nedeniyle, instance'lerinizin kasıtlı veya kazara durdurulsa, yeniden başlatılsa ve hatta sonlandırılsa bile, veriler EBS ile yapılandırıldığında bozulmadan kalır. EBS ayrıca, ihtiyaç duyduğunuzda tüm volume'in belirli bir zamandaki yedeklemelerini sağlama yeteneği sunar. Bu yedeklemeler anlık görüntü (snapshots) olarak bilinir. Volume'nin anlık görüntüsünü istediğiniz zaman manuel olarak alabilirsiniz. Dilerseniz, belirli bir tarihte veya saatte tekrarlanan yedeklemelerin otomatik olarak alınması için Amazon CloudWatch events'lerini kullanabilirsiniz.

Anlık görüntüler daha sonra Amazon S3'te depolanır ve bu nedenle çok dayanıklı (durable) ve güvenilirdir (reliable). Ayrıca artımlıdır (incremental), yani her anlık görüntü yalnızca önceki anlık görüntü alındığından beri değişen verileri kopyalar. Bir EBS volume'nin anlık görüntüsüne sahip olduktan sonra, o anlık görüntüden yeni bir volume oluşturabilirsiniz. Yani, herhangi bir nedenle EBS volume'nize bir olay nedeniyle erişimi kaybederseniz, mevcut bir anlık görüntüden veri volume'ni yeniden oluşturabilirsiniz. Ardından bu volume'i yeni bir EC2 instance'ına ekleyebilirsiniz. Ek esneklik ve dayanıklılık sağlamak için, bir anlık görüntüyü bir bölgeden diğerine kopyalamak da mümkündür.

Yüksek kullanılabilirlik ve dayanıklılık konusuna bakarsak, EBS volume'leriniz varsayılan olarak güvenilirlik göz önünde bulundurularak oluşturulur. Bir EBS volume'ne yapılan her yazma işlemi, bölgenizin aynı kullanılabilirlik alanı içinde birden çok kez çoğaltılarak verilerin tamamen kaybını önlemeye yardımcı olur. Bu, EBS biriminizin kendisinin yalnızca tek bir kullanılabilirlik alanında kullanılabilir olduğu anlamına gelir. Sonuç olarak, kullanılabilirlik alanınız başarısız olursa, EBS birimininize erişiminizi kaybedersiniz. Bu durumda, birimi (volume) önceki anlık görüntünüzden yeniden oluşturabilir ve başka bir kullanılabilirlik alanındaki başka bir instance'e ekleyebilirsiniz.

İki tür EBS birimi mevcuttur. Her birinin kendine özgü özellikleri vardır. 
- Bunlar SSD destekli depolama, 
- HDD destekli depolamadır. 

Bu, depolamanızı maliyet-performans perspektifinden gereksinimlerinize uygun hale getirmenize olanak tanır.

**SSD destekli depolama**, işlemsel iş yükleri kullanan veritabanları gibi daha küçük bloklarla çalışan senaryolar için daha uygundur. Genellikle EC2 instance'leriniz için önyükleme birimleri (boot volumes) olarak kullanılır. **HDD destekli birimler** ise büyük veri işleme ve günlük bilgilerini işleme gibi daha yüksek bir verim oranı gerektiren iş yükleri için tasarlanmıştır. Yani, esasen daha büyük veri bloklarıyla çalışır.

Bu hacim türleri daha da ayrıntılı olarak bölünebilir. Aşağıdaki tabloya bakarak hem SSD hem de HDD hacim türleri için farklı birimlerin nasıl kullanılabileceğini görebiliriz.

<div align="center">

![dEywE9n.md.png](https://iili.io/dEywE9n.md.png)

</div>

EBS biriminizin kullanım durumuna bağlı olarak en uygun türü seçebileceğinizi görebilirsiniz. Bu birimlerin her biri ayrıca şu farklı performans faktörlerini sunar: 
- Birim boyutu (Volume Size), 
- Birim başına maksimum IOPS (Max IOPS per volume), 
- Birim başına maksimum verim (Max throughput per volume), 
- Instance başına maksimum IOPS (Max IOPS per instance), 
- Instance başına maksimum verim (Max throughput per instance),
- Baskın performans (Dominant performance)

Sağlanan IOPS (saniye başına giriş/çıkış işlemleri) birimleri (Provisioned IOPS SSD) ile aşina olmayanlar için, bunlar I/O yoğun iş yükleri gerektiren uygulamalar için gelişmiş öngörülebilir performans sunar. Bu birimlerle çalışırken, yeni bir EBS birimi oluşturulurken bir IOPS oranı belirtme yeteneğine de sahipsiniz. Birim EBS optimize edilmiş bir instance'e eklendiğinde, EBS yıl boyunca %99,9 oranında tanımlanan ve gereken IOPS'yi %10 içinde teslim edecektir.

Verim optimize edilmiş HDD birimleri (Throughput Optimized HDD), sık erişilen veriler için tasarlanmıştır. Veri akışı, büyük veri ve günlük işleme gibi verim yoğun iş yükleri gerektiren büyük veri kümeleriyle çalışmak için idealdir. Bu birimler, belirli bir yıl boyunca %99 oranında beklenen verimi sağlayacaktır. Belirtilmesi gereken önemli bir nokta, bu birimlerin instance'leriniz için önyükleme birimleri (boot volume) olarak kullanılamayacağıdır.

Soğuk HDD birimleri (Cold HDD), diğer tüm EBS birim türlerine kıyasla en düşük maliyeti sunar. Büyük boyutlu ve nadiren erişilen iş yükleri için uygundur. Belirli bir yıl boyunca %99 oranında beklenen verimi sağlayacaklardır ve yine, bu tür birimleri EC2 instance'leriniz için önyükleme birimleri (boot volume) olarak kullanmak mümkün değildir.

EBS'nin harika bir özelliği, hem durağan (rest) halde hem de aktarım (transit) sırasında veri şifreleme yoluyla verilerinizin güvenliğini artırma yeteneğidir. Bu, özellikle EBS biriminizde kişisel olarak tanımlanabilir bilgiler gibi hassas veriler depoladığınızda çok kullanışlıdır. Düzenleyici (regulatory) veya yönetişim (governance) perspektifinden bir tür şifrelemeye sahip olmanız gerekebilir. EBS çok basit bir şifreleme mekanizması sunar. Basit, çünkü şifreleme sürecini kendiniz gerçekleştirmek için veri anahtarlarını yönetmekle uğraşmanız gerekmez. Hepsi EBS tarafından yönetilir ve uygulanır. Yapmanız gereken tek şey, oluşturma sırasında bir onay kutusu aracılığıyla birimin şifrelenip şifrelenmeyeceğini seçmektir.

Şifreleme süreci AES-256 şifreleme algoritmasını kullanır ve başka bir AWS hizmeti olan Anahtar Yönetim Hizmeti (KMS-Key Management Service) ile etkileşime girerek şifreleme sürecini sağlar. KMS, bu örnekte EBS gibi çeşitli AWS hizmetlerinde verilerin şifrelenmesini sağlayan müşteri ana anahtarlarını (CMK-Customer Master Key) kullanır.


Şifrelenmiş bir birimden alınan herhangi bir anlık görüntü de şifrelenecektir ve ayrıca bu şifrelenmiş anlık görüntüden oluşturulan herhangi bir birim de şifrelenecektir. Ayrıca, bu şifreleme seçeneğinin yalnızca seçili instance türlerinde kullanılabilir olduğunu bilmelisiniz.

EBS şifrelemesi hakkında değinilecek son bir nokta, oluşturulan tüm EBS hacimlerinin varsayılan olarak şifrelenmesini sağlayan bir varsayılan bölge ayarı oluşturabileceğinizdir.

EBS birimleri EC2 instance'lerinden ayrı olduğundan, yönetim konsolundan bir EBS birimi birkaç farklı şekilde oluşturabilirsiniz. Yeni bir örnek oluşturma sırasında ve başlatma anında ekleyebilir veya gerektiğinde bir instance'e eklenmeye hazır bağımsız bir birim olarak AWS yönetim konsolunun EC2 panosundan oluşturabilirsiniz. Bir EC2 instance'i başlatma sırasında bir EBS birimi oluştururken, size depolama yapılandırma seçenekleri sunulur. Burada yeni bir boş birim oluşturabilir veya mevcut bir anlık görüntüden oluşturabilirsiniz. Ayrıca boyutu ve birim türünü belirleyebilirsiniz. Önemli olarak, instance sonlandığında birime ne olacağına karar verebilirsiniz. Hacmi EC2 instance'inin sonlandırılmasıyla silinecek şekilde ayarlayabilir veya birimi koruyarak verileri muhafaza edip başka bir EC2 instance'ine eklemenize olanak tanıyabilirsiniz. Son olarak, gerekirse verileri şifreleme seçeneğiniz de vardır.

EBS birimini bağımsız bir birim olarak da oluşturabilirsiniz. EC2 panosundaki EBS altındaki volume (brime) seçeneğini seçerek, yönetim konsolundan yeni bir EBS hacmi oluşturabilirsiniz ve size aşağıdaki ekran sunulacaktır:

Burada birçok aynı seçeneğe sahip olacaksınız. Ancak, hacmin hangi kullanılabilirlik alanında var olacağını belirleyebilirsiniz, böylece aynı kullanılabilirlik alanındaki herhangi bir EC2 örneğine ekleyebilirsiniz.

EBS birimleri ayrıca ihtiyaç duyulduğunda esnek bir şekilde yeniden boyutlandırılabilme esnekliği sunar. Belki disk alanınız tükeniyor ve biriminizi büyütmeniz gerekiyor. Bu, konsol içinde veya AWS CLI aracılığıyla birimi değiştirerek gerçekleştirilebilir. Ayrıca, mevcut biriminizi bir anlık görüntüsünü oluşturarak ve ardından bu anlık görüntüden artırılmış bir kapasite boyutuyla yeni bir birim oluşturarak birimin aynı yeniden boyutlandırmasını gerçekleştirebilirsiniz.

Gördüğümüz gibi, EBS, EC2 örnek depolama birimlerine göre bir dizi avantaj sunar. Ancak EBS, tüm depolama gereksinimleri için uygun değildir. Ayrıca, çok yüksek dayanıklılık ve veri depolama kullanılabilirliğine ihtiyaç duyarsanız, Amazon S3 veya EFS'yi (Elastic File System) kullanmak daha uygun olacaktır.

## Amazon Elastic File System (EFS)

EFS hizmetinin AWS depolama dünyasındaki yerine bakarak başlayalım. İlk olarak, AWS depolama tekliflerinin dizisine bakmak ve bunlardan birkaçını karşılaştıralım. AWS'nin bu başlık altında tartışacağımızdan daha fazla depolama çözümü var ve gelecekte muhtemelen daha fazlasını eklemeye devam edecek. Ancak bu aşamada sadece üç farklı hizmete odaklanalım. Bu üçünü seçme nedenimiz, ilk bakışta benzer görünebilmeleri ve birçok kişinin mevcut depolama gereksinimlerine uygun olanı seçmekte kararsız kalabilmesidir.

**Amazon Simple Storage Service veya S3**, bir nesne depolama çözümüdür. Nesne depolama her şeyi tek bir nesne olarak depolar, yani küçük parçalar veya bloklar halinde depolamaz. Bu tür bir depolamada, bir dosya yüklersiniz ve dosya değişirse onu değiştirmek için tüm dosya değiştirilir. Bu tür bir depolama, dosyaların bir kez yazıldığı ve sonra birçok kez erişildiği durumlar için en iyisidir. Aynı anda hem sık okuma hem de yazma erişimi gerektiren durumlar için optimal değildir. Bu nedenle Amazon S3 genellikle video dosyaları, resimler, statik web siteleri ve yedekleme arşivleri gibi büyük dosyaların depolanması için kullanılır. Örneğin, Netflix veri akış hizmeti için S3'ü kullanır. Büyük film dosyalarını bir kez yüklerler ve ardından aboneler filmleri çok çok kez erişir ve oynatır.

Bir sonraki hizmet **Amazon Elastic Block Store veya EBS**'dir ve blok düzeyinde depolamadır. Dosyalar tek nesneler olarak depolanmaz. Küçük blok parçaları halinde depolanırlar, böylece sadece dosyanın değişen kısmı güncellenir. Bu tür depolama, düşük gecikme süreli erişim, hızlı, eşzamanlı okuma ve yazma işlemlerinin gerekli olduğu durumlar için optimize edilmiştir. EBS, tek bir EC2 instance'i ile kullanım için kalıcı blok depolama birimleri (volume) sağlar. Açıklandığı gibi, EBS kalıcıdır, yani EBS kullanan bir EC2 instance'ini durdursanız veya sonlandırsanız bile, EBS birimindeki veriler bozulmadan kalır. Bu tür depolamayı, işletim sistemi dosyalarını, uygulamaları ve EC2 instance'nizle kullanmak için elde etmek istediğiniz diğer dosyaları depoladığınız bir bilgisayar sabit sürücüsü gibi kullanmalısınız.

**Amazon Elastic File System veya EFS**, dosya düzeyinde depolama olarak kabul edilir ve ayrıca düşük gecikme süreli erişim için optimize edilmiştir. Ancak EBS'nin aksine, aynı anda birden fazla EC2 instance'i tarafından erişimi destekler. Kullanıcılara bir dosya yöneticisi arayüzü gibi görünür ve dosyaları kilitleme, dosyaları yeniden adlandırma, dosyaları güncelleme gibi standart dosya sistemi semantiklerini kullanır. Aynı zamanda, hiyerarşik bir yapı kullanır. Bu, standart öncül tabanlı (premise-based) sistemlerde alıştığımız şeye benzer. Bu tür depolama, ağ kaynaklarının erişebileceği dosyaları depolamanıza olanak tanır.

EFS hakkında derinlemesine bilgi vermeden önce, insanların geleneksel olarak ağ dosyalarına ve kaynaklarına nasıl eriştiğini tartışalım. Geleneksel öncül tabanlı ağlarda, kullanıcılar bir sunucuya bağlanan ağ kaynaklarına göz atarak dosyalara erişir. Bu erişim, kendileri için yapılandırılmış bir eşlenmiş sürücü aracılığıyla. Bağlandıklarında mevcut klasörlerin ve dosyaların bir ağaç görünümünde gösterilir. Bu işlevsellik genellikle dosya sunucuları veya depolama alanı ağı (SAN-Storage Area Network) veya ağa bağlı depolama (NAS-Network Attached Storage) gibi çeşitli yerel alan ağı sistemleri tarafından sağlanır.

Şimdi geleneksel öncül tabanlı çözümlerden uzaklaşalım ve bulut tabanlı çözümlerden, özellikle AWS ve Amazon Elastic File System hizmetinden bahsedelim. EFS, Amazon EC2 instance'leriyle kullanım için basit, ölçeklenebilir dosya depolama sağlar. Geleneksel dosya sunucuları veya bir SAN veya NAS gibi, Amazon EFS de kullanıcılara bulut ağ kaynaklarına göz atma yeteneği sağlar. EC2 instance'leri, yapılandırılmış bağlama noktaları kullanarak Amazon EFS instance'lerine erişecek şekilde yapılandırılabilir. Bağlama noktaları (mount points), birden fazla kullanılabilirlik bölgesinde oluşturulabilir ve birden fazla EC2 instance'ine bağlanabilir. Yani, geleneksel LAN sunucularınız gibi, EC2 instance'leri bir ağ dosya sistemine, Amazon EFS'ye bağlanır. Dolayısıyla, kullanıcı açısından sonuç aynıdır. Kullanıcı, her zaman yaptığı gibi ağ kaynaklarına erişir, ancak şimdi bu işlemi bulut kaynakları kullanılarak yapılır.

EFS, düşük gecikme süreli erişimle petabayt boyutuna kolayca ölçeklenebilen paylaşımlı dosya sistemleri oluşturmanıza olanak tanıyan, tam yönetilen, yüksek oranda kullanılabilir ve dayanıklı bir hizmettir. EFS, düşük gecikme süreli erişim yanıtına ek olarak yüksek bir verim düzeyi sağlamak üzere tasarlanmıştır ve bu performans faktörleri EFS'yi çok çeşitli iş yükleri ve kullanım senaryoları için arzu edilen bir depolama çözümü haline getirir. Bununla beraber, onlarca, yüzlerce hatta binlerce EC2 instance'inin eşzamanlı taleplerini karşılayabilir. Yönetilen bir hizmet olduğundan, herhangi bir dosya sunucusu sağlamanıza, depolama öğelerini yönetmenize veya bu sunucuların bakımını yapmanıza gerek yoktur. Bu, ortamınızda dosya düzeyinde depolama sağlamak için çok basit bir seçenek haline getirir. Standart işletim sistemi API'lerini kullanır, bu nedenle standart işletim sistemi API'leri ile çalışmak üzere tasarlanmış herhangi bir uygulama EFS ile çalışacaktır. NFS sürüm 4.1 ve 4.0'ı destekler. Güçlü tutarlılık ve dosya kilitleme gibi standart dosya sistemi semantiklerini kullanır. Tek bir bölgedeki kullanılabilirlik alanları arasında çoğaltılır, bu da EFS'yi yüksek düzeyde güvenilir bir depolama hizmeti haline getirir.

Dosya sistemine birden fazla instance tarafından erişilebildiğinden, birden fazla instance üzerinde ölçeklenen uygulamalar için verilere paralel erişime izin veren çok iyi bir depolama seçeneği haline getirir. EFS dosya sistemi ayrıca bölgeseldir, bu nedenle birden fazla kullanılabilirlik bölgesine yayılan herhangi bir uygulama dağıtımı, uygulama depolama katmanınızın yüksek kullanılabilirlik düzeyi sağlayarak aynı dosya sistemlerine erişebilir.

## Depolama Sınıfları ve Performans Seçenekleri (Storage Classes and Performance Options)

Merhaba ve bu derse hoş geldiniz. Bu derste, EFS'nin sunduğu farklı depolama sınıfı seçeneklerini ve EFS kullanım senaryonuza bağlı olarak belirli performans faktörlerini nasıl değiştirebileceğinizi ve yapılandırabileceğinizi tartışacağız. Amazon EFS, size farklı performans ve maliyet seviyeleri sunan iki farklı depolama sınıfı sunmaktadır: 
- Standard ve Infrequent Access (IA olarak bilinir). 
- Standard depolama sınıfı, EFS kullanırken varsayılan depolama sınıfıdır. 

Ancak, Infrequent Access genellikle EFS'de nadiren erişilen verileri depolamak için kullanılır. Bu sınıfı seçerek, depolama maliyetinizde bir azalma elde edersiniz.

Daha ucuz depolamanın sonucu, Standard ile karşılaştırıldığında bu sınıfta veri okuma ve yazma sırasında artan bir ilk byte gecikme (first-byte latency ) etkisi olmasıdır. Maliyetler de biraz farklı yönetilir. IA kullanırken, kullanılan depolama alanı miktarı için ücretlendirilirsiniz ve bu, Standard'a göre daha ucuzdur. Ancak bununla beraber, IA ile depolama sınıfına yaptığınız her okuma ve yazma işlemi için de ücretlendirilirsiniz. Bu, bu depolama sınıfını yalnızca çok sık erişilmeyen veriler için kullanmanızı sağlar, örneğin denetim amaçlı veya tarihsel analiz için gerekli olabilecek veriler gibi.

Standard depolama sınıfında, yalnızca aylık kullanılan depolama alanı miktarı için ücretlendirilirsiniz. Her iki depolama sınıfı da EFS'nin desteklendiği tüm bölgelerde mevcuttur. Önemli olan, her ikisi de aynı düzeyde kullanılabilirlik ve dayanıklılık sağlar. S3'e aşinaysanız, veri yönetimi için S3 yaşam döngüsü politikalarına da aşina olabilirsiniz. EFS içinde, EFS yaşam döngüsü yönetimi olarak bilinen benzer bir özellik vardır. Etkinleştirildiğinde, EFS otomatik olarak verileri Standard depolama sınıfı ile IA depolama sınıfı arasında taşır. Bu süreç, bir dosyanın belirli bir gün sayısı boyunca okunmadığı veya yazılmadığı durumlarda gerçekleşir. Bu süre için seçenekleriniz 14, 30, 60 veya 90 gündür.

Seçiminize bağlı olarak, EFS, bu süre dolduktan sonra maliyetten tasarruf etmek için verileri IA depolama sınıfına taşır. Ancak, aynı dosyaya tekrar erişildiğinde, zamanlayıcı sıfırlanır ve dosya Standard depolama sınıfına geri taşınır. Tekrar, belirli bir süre boyunca erişilmezse, IA'ya geri taşınır. Bir dosyaya her erişildiğinde, **yaşam döngüsü yönetimi zamanlayıcısı sıfırlanır.** Verilerin IA depolama sınıfına taşınmamasının tek istisnaları, 128K'dan küçük dosyalar ve dosyalarınızın herhangi bir metadatası içindir, bunların tümü Standard depolama sınıfında kalır.

EFS dosya sisteminiz 13 Şubat 2019'dan sonra oluşturulduysa, yaşam döngüsü yönetimi özelliği açılıp kapatılabilir. Şimdi EFS'nin sunduğu farklı performans modlarına bir göz atalım. AWS, EFS'nin çeşitli kullanım senaryoları ve iş yükleri için kullanılabileceği yerdir. Bu nedenle her kullanım senaryosu, throughput, IOPS ve gecikme açısından performans değişikliği gerektirebilir. Sonuç olarak, AWS, EFS dosya sisteminizin oluşturulması sırasında tanımlanabilen iki farklı performans modu sunmuştur. Bunlar;
- General Purpose, 
- Max I/O'dur.

**General Purpose**, varsayılan performans modudur ve genellikle çoğu kullanım senaryosu için kullanılır. Örneğin, ev dizinleri ve genel dosya paylaşım ortamları için. Genel bir performans ve düşük gecikme süreli dosya işlemi sunar. Bu modun EFS dosya sisteminize saniyede yalnızca 7.000 dosya sistemi işlemine izin veren bir sınırlaması vardır. Ancak, EFS dosya sisteminizin aynı anda binlerce EC2 instance'i tarafından kullanılması muhtemel olan ve saniyede 7.000 işlemi aşacak büyük ölçekli bir mimariniz varsa, Max I/O'yu düşünmeniz gerekecektir. Bu mod, neredeyse sınırsız miktarda throughput ve IOPS sunar. Ancak dezavantajı, dosya işlemi gecikmesinin General Purpose'a göre daha kötüdür.

Hangi performans seçeneğine ihtiyacınız olduğunu belirlemenin en iyi yolu, uygulamanızla birlikte testler yapmaktır. Uygulamanız saniyede 7.000 işlem sınırı içinde rahatça çalışıyorsa, General Purpose en uygun seçenek olacaktır ve düşük gecikme süresi ek bir avantaj olacaktır. Ancak, testleriniz saniyede 7.000 işlemin ulaşılabileceğini veya aşılabileceğini doğruluyorsa, Max I/O'yu seçin.

General Purpose işlem modunu kullanırken, EFS, saniyedeki işlemleri 7.000 üst sınırının yüzdesi olarak görüntülemenizi sağlayan bir CloudWatch metriği olan 'percent I/O' limit sunar. Bu, işlemleriniz bu sınıra ulaşıyorsa Max I/O dosya sistemine geçiş yapma kararı almanıza olanak tanır.

İki performans moduna ek olarak, EFS ayrıca iki farklı throughput modu sunar ve throughput, mebibyte cinsinden ölçülür. Sunulan iki mod, **Bursting Throughput** ve **Provisioned Throughput**'tur. Dosya sistemlerindeki veri throughput modelleri genellikle nispeten düşük aktivite dönemlerinden geçer ve zaman zaman burst kullanımında ani artışlar olur. EFS, bu rastgele yüksek aktivite dönemlerini yönetmeye yardımcı olmak için throughput kapasitesi sağlar.

Varsayılan mod olan **Bursting Throughput** modunda, dosya sisteminiz büyüdükçe throughput miktarı ölçeklenir. Yani ne kadar çok depolarsanız, o kadar çok throughput kullanımınıza sunulur. Varsayılan olarak kullanılabilir throughput, saniyede 100 mebibyte'a kadar burst yapabilir, ancak standard depolama sınıfıyla, dosya sistemi içinde kullanılan depolama alanının her tebibyte'ı için saniyede 100 mebibyte'a kadar burst yapabilir.

Örneğin, EFS dosya sisteminizde beş tebibyte depolama alanınız olduğunu varsayalım. Burst kapasiteniz saniyede 500 mebibyte'a ulaşabilir. Throughput burst'ünün süresi, dosya sisteminin boyutuyla yansıtılır. Düşük aktivite dönemlerinde biriken krediler kullanılarak, kullanılan depolama alanının her tebibyte'ı için 50 mebibyte olarak belirlenen temel throughput oranının altında çalışarak, EFS'nin ne kadar süre burst yapabileceği belirlenir. Her dosya sistemi, temel throughput'una zamanın %100'ünde ulaşabilir. Kredi biriktirerek, dosya sisteminiz temel sınırınızın üzerine çıkabilir. Kredi sayısı, bu throughput'un ne kadar süre sürdürülebileceğini belirler ve dosya sisteminiz için burst kredilerinin sayısı, CloudWatch'ın *BurstCreditBalance* metriğini izleyerek görüntülenebilir.

Burst kredilerinizin çok sık tükendiğini fark ederseniz, **Provisioned Throughput** modunu kullanmayı düşünmeniz gerekebilir. Provisioned Throughput, dosya sisteminizin boyutuna bağlı olarak tahsis edilen ödeneğinizin üzerine çıkmanıza olanak tanır. Yani dosya sisteminiz nispeten küçükse ancak dosya sisteminizin kullanım senaryosu yüksek bir throughput oranı gerektiriyorsa, varsayılan bursting throughput seçenekleri isteğinizi yeterince hızlı işleyemeyebilir. Bu durumda, provisioned throughput kullanmanız gerekecektir. Ancak bu seçenek ek ücretler doğurur ve varsayılan bursting throughput seçeneğinin üzerindeki herhangi bir burst için ek maliyetler ödersiniz.

## Blok, Nesne ve Dosya Depolama Karşılaştırması (Block vs. Object vs. File Storage)

Bu başlık altında, blok depolama, nesne depolama ve dosya depolamayı içeren AWS depolama hizmetlerini ve depolama türlerini kısaca tanıyacağız.

Hatırlatmak gerekirse, **blok depolama**, sabit diskler, sunucu dosya sistemleri ve storage-area network'ler (SAN'lar) gibi veri depolama cihazlarında kullanılır. Bu cihazlar veriyi bloklar halinde depolar. Amazon Elastic Block Store veya EBS, AWS'de EC2 instance'leriyle sorunsuz bir şekilde entegre olan kalıcı blok depolama birimleri sağlayan hizmettir. Ayrıca, EC2 instance'lerinizi barındıran sunuculara fiziksel olarak bağlı diskler olan instance store birimleri de vardır. Bunlar çok hızlı ancak yalnızca geçici blok depolama sağlayabilir.

**Nesne depolama**, genellikle neredeyse sınırsız kapasiteye sahip olmakla daha çok ilgilendiğiniz büyük, yapılandırılmamış veriler için kullanacağınız şeydir. Bunun için Amazon S3, hem ölçeklenebilir hem de yüksek oranda dayanıklı nesne depolama sağlayan tercih ettiğimiz hizmettir.

Son olarak, adından da anlaşılacağı gibi, **dosya depolama**, dosyalarda depolanan veriler için kullanılır. Bu dosyalar, klasörlerin hiyerarşisi içinde bulunur. Bunlar genellikle Windows veya Linux ortamında ağa bağlı dosya paylaşımlarınızı barındıran network-attached storage (NAS) cihazlarınızdır. AWS'de, bulut tabanlı dosya depolama hizmetlerimiz olarak hem Elastic File System (EFS) hem de FSx'e sahibiz.

### Blok Depolama (Block Storage)

Bu başlık altında, AWS blok depolama ile ilgili performans faktörlerini tartışacağız. AWS'de, dosya sistemleri ve yapılandırılmış veritabanı depolaması dahil olmak üzere düşük gecikme süreli ve yüksek performanslı iş yüklerimiz için EBS birimleri gibi blok düzeyinde depolama cihazları kullanıyoruz. Peki blok düzeyinde depolama ile tam olarak ne kastediyoruz? Bloku, blok cihazınızın **tek bir I/O isteğinde** okuyabileceği veya yazabileceği veri miktarına dayalı olarak **sabit boyutlu** bir veri parçası olarak düşünebilirsiniz.

EBS biriminizde bu blok boyutundan daha büyük bir veri depolamanız gerekirse, bu veri eşit boyutlu bloklara bölünecek ve alttaki fiziksel birimde bu bloklar içinde depolanacaktır. Bu durum, hem hızlı erişim hem de geri alma için optimize edilmiş bir şekilde yapılır ve her veri bloğuna benzersiz bir tanımlayıcı (identifier) atanır. Bu blokların alttaki birimde sürekli veya anlamlı bir sırada olması gerekmez, çünkü blok depolama sistemi, bir blok yazıldığında bu benzersiz tanımlayıcıların konumlarıyla güncellenen bir arama tablosundan (lookup table) yararlanır. Bu, veri istendiğinde hızlı bir şekilde alınmasını ve birleştirilmesini sağlar.

Blok depolama birimleri birçok farklı işletim sistemi ile entegre edilebilir, ancak öncelikle o belirli işletim sistemi tarafından anlaşılabilecek şekilde formatlanmaları gerekir. Blok depolamanın bir avantajı, mevcut bir dosyada küçük bir güncelleme gibi veri güncellendiğinde, o dosya içindeki **yalnızca yeni veya değiştirilmiş blokların yeniden yazılması** gerektiğidir. Değişmemiş bloklar olduğu gibi bırakılabilir, bu da performansı ve hızı daha da artırmaya yardımcı olur.

AWS'de bir tür özel durum olan başka bir blok depolama türü daha vardır, bu da EC2 instance'lerimizle birlikte gelen instance store birimleridir. Bunlar, EC2 örneklerimizi barındıran AWS veri merkezlerindeki gerçek makinelere fiziksel olarak bağlı disklerdir. Bu nedenle, son derece hızlı performans gösterirler. Ancak EC2 instance'niz durdurulduğunda, hazırda bekletildiğinde veya sonlandırıldığında instance store birimindeki tüm verilerin kaybolacağını unutmayın. Bu nedenle, yalnızca hızlı geçici depolama (fast temporary storage), örneğin bir scratch birimi veya önbellek olarak kullanılması gerçekten faydalıdır.

Şimdi EBS birimi performansı ve onu etkileyebilecek farklı faktörler hakkında biraz daha spesifik olarak inceleyelim. Öncelikle, iş yükleriniz için uygun EBS birim türünü kullandığınızdan emin olmak isteyebilirsiniz. Hızlı bir hatırlatma olarak, EBS throughput-optimized (st1) ve cold (sc1) manyetik sabit disk sürücülerinin yanı sıra general purpose (gp2 veya gp3) ve provisioned IOPS (io1 veya io2) SSD destekli blok depolama sunar. Ancak genel bir kural olarak, **SSD destekli depolama** küçük, rastgele I/O işlemleri için daha iyi performans gösterirken, **manyetik sabit disk sürücüleri** büyük, sıralı işlemler için daha iyi performans gösterecektir.

EBS birimlerinizin EC2 instance'lerine fiziksel olarak bağlı olmadığını unutmayın. Bunun yerine, ağ üzerinden erişilirler. Bu nedenle, EBS birimleriniz için I/O trafiğinin diğer ağ trafiğinizle rekabet edebileceği ve ek gecikmeye neden olabileceği durumlar olabilir. Bunu hafifletmek için, **EBS-optimized** denilen bir instance türü kullanmalısınız. EBS-optimized instance türleri, I/O işlemleri için özel olarak ayrılmış ağ kapasitesi sağlayarak I/O trafiğinizi ayıracak şekilde AWS tarafından yapılandırılmıştır. Bu, EBS birimlerinizin gp2 ve gp3 SSD destekli birim türleri ve tüm manyetik sabit disk sürücüleri için belirli bir yılda zamanın %99'unda sağlanan IOPS performanslarının en az %90'ına ulaşmasına yardımcı olur. Ve bu sayı, io1 ve io2 SSD destekli birim türleri için belirli bir yılda zamanın %99,9'una çıkar.

Blok depolama ve EBS performansı hakkındaki bilgilerimizi tamamlamak için, EBS birimlerinizin performansını etkileyebilecek birkaç diğer faktörden bahsedelim. Bunlar arasında snapshot oluşturmanın etkisi ve snapshot'lardan yeni birimler başlatma yer alır. Bir st1 veya sc1 birimi kullanımdayken bir snapshot oluşturduğunuzda, snapshot işlemi devam ederken o birimin performansı düşecektir. Herhangi bir birim türü için, bir snapshot'a dayalı yeni bir birimi başlattığınızda, birimdeki her bloğa en az bir kez erişilene kadar biraz performans kaybı yaşayacaksınız. Bunun nedeni, EBS snapshot'larınızın S3'te nesne olarak depolanmasıdır.

Bir snapshot'a dayalı yeni bir birimi başlattığınızda, o birim için tüm depolama bloklarının S3'ten indirilmesi ve erişebilmeniz için yeni birime yazılması gerekir. Bu süreç önemli miktarda zaman alabilir. Bunu iki şekilde azaltabilirsiniz: 
- İlk olarak, bir Linux makinesinde cihazdaki tüm blokların okunmasını zorlamak için `dd` veya `fio` yardımcı programlarını çalıştırmayı içeren başlatma (initialization) adı verilen bir işlem kullanarak, 
- İkinci olarak, belirli bir kullanılabilirlik bölgesinde snapshot bazında yapılabilen EBS **fast snapshot** restore olarak bilinen özelliği etkinleştirerek.

I/O throughput'unuzu gerçekten artırmakla ilgileniyorsanız, birden fazla EBS birimini bir **RAID 0** yapılandırmasında birleştirebilirsiniz. Bu, onların tek bir mantıksal sürücü olarak eşzamanlı çalışmasına olanak tanır. Ayrıca okuma ve yazma performansını büyük ölçüde artırma avantajına sahiptir. Ancak RAID 0 kullanmak veri kaybı riski taşır, çünkü birimler arasında veri yansıtma (mirroring) veya yedekleme yoktur. Birimlerinizden biri başarısız olursa, veri kaybedebilirsiniz.

Son olarak, EBS birimlerinizin performansını test edebilir ve doğrulayabilirsiniz. Bu, Linux'ta `fio` gibi bir karşılaştırma aracı kullanarak I/O iş yüklerini simüle etmeyi ve birimleriniz için optimum kuyruk uzunluğu gibi şeyleri belirlemenize yardımcı olmayı içerir.

### Nesne Depolama (Object Storage)

Bu başlık altında, AWS nesne depolaması ile ilgili performans faktörlerine daha yakından bakacağız. Ve tabii ki, nesne depolamamız için Amazon S3'ü kullanacağız. S3 ve genel olarak nesne depolama, büyük miktarlarda yapılandırılmamış veriyle uğraşırken harikadır. Ve önemli bir not, geleneksel anlamda dosya olarak kabul ettiğimiz şeyleri S3'te depolasak bile, bunları hala nesne depolamada bulunan nesneler olarak adlandıracağız.

Blok depolama veriyi verinin boyutuna göre parçalar halinde depolarken, nesne depolama verinizi **tek bir nesne olarak** depolar. Bu, blok depolamada veri güncellemenin yalnızca birçok bloktan birinin yeniden yazılmasını gerektirebileceği durumun aksine, nesne depolamada her zaman **tüm nesnenin yeniden yazılması** gerektiği anlamına gelir, ne olursa olsun. Blok ve nesne depolama arasındaki bir diğer fark, blok depolamanız işletim sisteminiz içinde bir sürücü olarak monte edilebilirken, S3'teki nesnelerinize erişmek için HTTP, HTTPS veya CLI veya SDK aracılığıyla AWS API'lerini kullanmanız gerekecek olmasıdır.

S3 ile nesne depolamanın bir büyük avantajı, doğası gereği yüksek oranda kullanılabilir ve son derece dayanıklı olmasıdır. Ayrıca çok düşük maliyetlidir ve tek bir EBS biriminin sınırlarının çok ötesinde **sonsuz ölçeklenebilirdir**. Bununla beraber felaket kurtarma açısından, verinizin birden fazla AWS bölgesinde depolanmasını sağlamak için bölgeler arası replikasyonu yapılandırmak kolaydır. Bu durum da yedeklemeler gibi şeyleri depolamak için harika kılar.

Nesne depolamada, depoladığınız nesneler yalnızca verilerden veya dosyaların kendilerinden ibaret olmayacaktır. Her nesne ayrıca nesne için bir tür genel olarak benzersiz tanımlayıcı ve bazı ek **metadata** içerecektir. Bu metadata, oluşturma tarihi veya görüntü formatı ve çözünürlüğü gibi sistem tanımlı metadata'dan, herhangi bir anahtar-değer çifti olabilecek kullanıcı tanımlı metadata'ya kadar her şey olabilir. S3'te, nesneler bucket'larda depolanır ve nesnenin anahtarı olarak kabul edilen bir ad atanır. Ve bu anahtar, belirli bir nesnenin sürüm kimliği ile birlikte, o nesneyi S3 içinde benzersiz bir şekilde tanımlamaya yarar. Yani S3'ü esasen büyük bir anahtar-değer deposu olarak düşünebilirsiniz, burada nesnenin adı anahtar ve değeri verinin kendisidir. Ve her nesnenin ilişkili metadata'sı daha sonra o nesneyle ilişkili ayrı bir anahtar-değer deposunda tutulur.

Buraya kadar, S3 ile nesne depolamanın hızlı bir genel bakış yaptık. Ancak bu başlık altında, S3'teki performans faktörlerini ve S3 kullanan uygulamalarınızda mümkün olan en iyi performansı elde etmenize yardımcı olacak bazı tasarım modellerini tartışarak tamamlayalım.

S3 ile performansı iyileştirmekten bahsettiğimizde, genellikle nesneleri S3'e koyma, S3'ten alma hızı ve güvenilirliği ile ilgileniyoruz. S3 ile iletişim kuran EC2 instance'lerine sahip bir uygulamanız varsa, EC2 instance'leriniz S3 bucket'larınızla aynı bölgede bulunması gerektiğini söylemeye gerek yok. Bu zorunluluk, sadece size en iyi performansı vermek için değil, aynı zamanda veri transfer maliyetlerinizi düşük tutmak içindir.

Gereksinimleriniz, coğrafi olarak size yakın olmayan bir bölgedeki S3 bucket'ına veri yüklemenizi dikte ediyorsa, bu ek mesafe gecikmeyi artırabilir ve performans üzerinde önemli bir etkiye sahip olabilir. Bunu azaltmanın en iyi yolu, konumunuz ile S3 bucket'ınız arasındaki en hızlı yükleme yolunu AWS ağ altyapısını kullanarak sağlayan CloudFront edge konumlarını (CloudFront Edge Locations) kullanan S3 Transfer Acceleration'dan yararlanmaktır.

CloudFront'tan bahsetmişken, S3 içinde statik bir web sitesi barındırıyorsanız, önbelleklemeyi etkinleştirmek, gecikmeyi azaltmak ve sitenizin genel performansını iyileştirmek için kesinlikle o web sitesinin önünde bir CloudFront dağıtımı oluşturmalısınız.

Yükleme performansını artırmak için yapabileceğiniz bir başka şey de S3'te çok parçalı yüklemelerden (multipart uploads) yararlanmaktır. Çok parçalı yüklemeler, 5 gigabayt'tan büyük herhangi bir nesne için gereklidir, ancak 100 megabayt'tan büyük herhangi bir nesneyi yüklerken de kullanmak iyi bir fikirdir. Bunun nedeni, throughput'u ve genel performansı artırmak için paralel olarak birden fazla PUT isteği gönderebilmenizdir.

S3'ü, bir disk sürücüsü veya dosya paylaşımı gibi darboğazlara (bottlenecks) tabi olabilecek tek bir uç nokta yerine büyük bir dağıtık sistem olarak düşünmek önemlidir. Ve bu nedenle, çok parçalı yüklemelerimiz için birden fazla eşzamanlı PUT isteği gönderebilmemiz gibi, birden fazla eşzamanlı GET isteği göndererek indirme performansımızı da iyileştirebiliriz. Bunu yapmak için, GET isteklerimize byte-range fetch adı verilen şeyi kullanmak için *Range* adlı bir HTTP başlığı (headers) ekleyebiliriz. Bu kullanım, bir nesnenin birden fazla küçük parçasını aynı anda indirerek S3'ten nesne indirmelerimizde throughput'u büyük ölçüde artırmamıza olanak tanır. S3 bucket'ınıza yapabileceğiniz eşzamanlı bağlantı sayısında herhangi bir sınır olmadığından, bunu yaparak indirme throughput'unuzu gerçekten büyük ölçüde artırabilirsiniz. 

### Dosya Depolama (File Storage)

Bu başlık altında, AWS'deki dosya depolama hizmetlerimiz için hem Amazon EFS hem de FSx'e bakacağız. İsimden de anlaşılacağı üzere, dosya depolaması klasörler içinde yaşayan dosyaların hiyerarşisini korumak için kullanılır. Bu klasörler hem dosyaları hem de potansiyel olarak ek klasörleri içerebilir. Bu durum da iç içe geçmiş bir dosya yolu yapısına yol açar. Dosya depolaması muhtemelen ağa bağlı depolama veya NAS cihazları kullanan ağ paylaşımlı dosya sistemlerimiz için en yaygın kullanılan depolama türüdür.

Dosya depolaması, diğer depolama türlerine göre birçok avantaj sunar. Bunlardan biri, dosyalara sanki yerel bir sabit sürücüdeymiş gibi birden fazla farklı istemciden (client) eşzamanlı olarak erişilebilmesidir. Dosya depolamasının, Network File System (NFS) veya Server Message Block (SMB) gibi bir dosya sistemi kullanarak blok depolamanın üzerinde sadece bir soyutlama (abstraction) olduğunu hatırlamak önemlidir.

Yine AWS'de, Linux tabanlı EC2 instance'ları ve şirket içi sunucuların yanı sıra Lambda fonksiyonları ve ECS, EKS ve Fargate gibi diğer AWS container hizmetleriyle kullanım için tam yönetilen bir dosya depolama hizmeti olan Elastic File System veya EFS'ye sahibiz. Büyük ölçekte ölçeklenebilir ve aynı anda binlerce instance tarafından erişilebilir.

Ayrıca, FSx for Lustre, Windows File Server, OpenZFS ve NetApp ONTAP dahil olmak üzere çeşitli iş yüklerini (workload) destekleyen birkaç farklı teklif sunan Amazon FSx'i de kullanabiliriz.

Şimdi hem EFS hem de FSx için performans faktörlerinden bahsedelim. EFS açısından, dosya sistemimizi ilk oluşturduğumuzda bir storage class, bir performance mode ve bir throughput mode yapılandırma imkanımız var. Storage class seçiminizin (One Zone veya Standard olabilir) performans üzerinde herhangi bir etkisi olmayacaktır, çünkü bu sadece dosya sisteminizin yüksek kullanılabilirliğe sahip olup olmadığını etkiler. Ancak performance mode seçiminizin performans üzerinde kesinlikle bir etkisi olacağı sizin için de sürpriz olmamalı. Performance mode seçenekleriniz General Purpose ve Max I/O'dur. Dosya sisteminiz One Zone storage class kullanıyorsa, General Purpose performance mode'unu kullanmak zorundadır.

Bu mod size okuma işlemleri için mikrosaniye, yazma işlemleri için milisaniye düzeyinde çok düşük gecikme süresi verecektir, ancak okuma işlemleri için maksimum 35.000 IOPS ve yazma işlemleri için 7.000 IOPS ile sınırlıdır. Öte yandan, Max I/O gecikme sürenizi biraz artıracak, ancak maksimum IOPS'unuzu okuma işlemleri için 500.000'in üzerine ve yazma işlemleri için 100.000'in üzerine çıkaracaktır. Bu performance mode'larının hiçbiri dosya sisteminizin maksimum throughput'unu değiştirmez, bu da dosya sistemi başına okuma için 3-5 gigabayt/saniye ve yazma için 1-3 gigabayt/saniye arasında değişecektir.

Throughput açısından, Bursting veya Provisioned throughput mode'larını seçebiliriz. **Bursting** varsayılan moddur ve düşük throughput dönemlerinde burst bucket'lar içinde burst kredileri biriktirmenize olanak tanır. Bu burst kredileri daha sonra daha yüksek throughput talebinin olduğu dönemlerde performansı korumak için kullanılabilir. Bursting genellikle throughput ihtiyaçlarınızın zaman içinde değişeceği çoğu durum için iyi bir seçimdir. Ancak sabit ve öngörülebilir throughput ihtiyaçlarınız varsa, bunun yerine **Provisioned Throughput** mode'unu kullanmayı tercih etmelisiniz. Provisioned Throughput ile, Bursting Throughput mode'unu kullanıyormuş gibi dosya sisteminin temel throughput oranını aşan throughput miktarına göre ücretlendirilirsiniz.

Amazon FSx'e gelince, farklı dosya sistemi tekliflerinin tümü milisaniyenin altında gecikme süresi ve en az 2-3 gigabayt/saniye throughput destekleyecektir. FSx for Lustre durumunda, maksimum throughput aslında 1000 gigabayt/saniye ile çok daha yüksektir ve Lustre dosya sistemleri ayrıca petabayt boyutuna kadar ölçeklenebilir. FSx'in farklı çeşitlerinin performans özelliklerini aşağıdaki tabloda görebilirsiniz:

<div align="center">

![dG7Bu7p.md.png](https://iili.io/dG7Bu7p.md.png)

</div>

FSx dosya sisteminiz için başlangıç throughput kapasitesini oluşturduğunuz sırada yapılandırabilirsiniz. Seçiminiz, dosya sisteminizin genel performansını etkileyecektir. Örneğin, FSx for Windows File Server ile 32'den 2.048 megabayt/saniye'ye kadar değişen throughput kapasiteleri seçebilirsiniz. Seçiminize bağlı olarak, birkaç diğer performans seçeneği de bununla birlikte ölçeklenecektir, bunlar:

- Ağ throughput kapasitesi veya dosya sunucusunun dosyaları ağ üzerinden istemcilere sunma hızı,

- Ağ IOPS,

- Dosya kopyalama ve veri tekilleştirme gibi arka plan işlemlerini desteklemek için kullanılabilen dosya sunucusu belleği,

- Dosya sunucusu disk throughput'u,

- Disk IOPS.

Ancak bu throughput kapasitesini zaman içinde gereksinimlerinizi karşılamak için her zaman yukarı veya aşağı ayarlayabileceğinizi unutmayın.
