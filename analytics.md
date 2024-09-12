# Analytics (DVA-C02)

Bu başlık altında, geliştiriciler için AWS'deki analiz hizmetlerine giriş yapmaktır. Bu hizmetler arasında:

-   Amazon Athena,
-   Amazon Kinesis,
-   Amazon OpenSearch Service, yer almaktadır.

## Amazon Kinesis

Amazon Kinesis, veri akışını AWS bulutuna aktarmanın karmaşıklığını ve maliyetlerini ele almak için tasarlandı. Kinesis, event logs, sosyal medya akışları, tıklama akışı verileri, uygulama verileri ve IoT sensör verileri gibi çeşitli veri akışlarını gerçek zamanlı veya gerçek zamana yakın bir şekilde toplamanıza, işlemenize ve analiz etmenize olanak tanır.

Kinesis'e erişim, AWS Identity and Access Management (IAM) kullanılarak kontrol edilir. AWS Key Management Service (KMS) kullanılarak veri, hem akış içinde hem de Amazon S3 veya Redshift gibi bir AWS depolama hizmetine yerleştirildiğinde otomatik olarak korunur. Veri aktarımı, Transport Layer Security Protocol (TLS) kullanılarak korunur. Amazon Kinesis dört hizmetten oluşur: 
- Kinesis Video Streams, 
- Kinesis Data Streams, 
- Kinesis Data Firehose,
- Kinesis Data Analytics.

**Kinesis Video Streams**, ses ve video gibi ikili kodlanmış veriler üzerinde akış işlemleri yapmak için kullanılır.

**Kinesis Data Streams**, **Kinesis Data Firehose** ve **Kinesis Data Analytics** ise base64 metin kodlu verileri akıtmak için kullanılır. Bu metin tabanlı bilgiler, log kayıtları, tıklama akışı verileri, sosyal medya akışları, finansal işlemler, oyun içi oyuncu etkinlikleri, coğrafi hizmetler ve IoT cihazlarından gelen telemetri gibi kaynakları içerir.

Genel olarak konuşursak, veri akışı çerçeveleri beş katman olarak tanımlanır; 
- Kaynak (Source), 
- Akış Alma (Stream Ingestion),
- Akış Depolama (Stream Storage), 
- Akış İşleme (Stream Processing),
- Hedef (Destination).

Kinesis Data Streams kullanıldığında süreç şu şekilde işler:

Veriler, mobil cihazlar, akıllı evlerdeki ölçüm cihazları, tıklama akışları, IoT sensörleri veya loglar gibi bir  kaynak tarafından üretilir.

Stream Ingestion katmanında, veriler bir veya daha fazla üretici (Producers) tarafından toplanır, veri kayıtları (Data Records) olarak biçimlendirilir ve bir akışa konur.

Kinesis Data Stream, bir Stream Storage katmanıdır. Verileri en az 24 saatten, Kasım 2020 itibarıyla 365 güne kadar saklayan yüksek hızlı bir tampondur. 24 saat saklama varsayılan değerdir.

Kinesis Data Streams içinde, veri kayıtları değiştirilemezdir. Bir kez saklandığında, değiştirilemezler. Verilerde güncelleme yapmak için yeni bir kaydın akışa konulması gerekir. Veri akıştan kaldırılmaz, yalnızca süresi dolabilir.

Stream Processing katmanı, tüketiciler (consumers) tarafından yönetilir. Tüketiciler aynı zamanda Amazon Kinesis Data Streams uygulamaları olarak da bilinir ve bir akış içinde yer alan verileri işlerler. Tüketiciler, veri kayıtlarını Destination katmanına gönderirler. Bu, bir data lake, veri ambarı, kalıcı depolama veya başka bir akış gibi bir şey olabilir.

Amazon Kinesis Streaming hizmetlerinin her birini hızlıca açıklayalım.

**📌 Amazon Kinesis Video Streams**, milyonlarca kaynaktan AWS'ye ikili kodlu verileri akıtmak için tasarlanmıştır. Geleneksel olarak bu, ses ve video verileridir ancak herhangi bir türde ikili kodlanmış zaman serisi verisi olabilir. Adında video olmasının nedeni, birincil kullanım alanı olmasıdır.

**AWS SDK**'ları, verilerin AWS'ye güvenli bir şekilde akıtılmasını, oynatılmasını, depolanmasını, analiz edilmesini, makine öğrenimi ve diğer işlemler için kullanılmasını mümkün kılar. Veriler, akıllı telefonlar, güvenlik kameraları, uç cihazlar, RADAR, LIDAR, dronlar, uydular ve araç içi kameralar gibi cihazlardan alınabilir.

Kinesis Video Streams, **WebRTC** adlı açık kaynaklı projeyi destekler. Bu, web tarayıcıları, mobil uygulamalar ve bağlı cihazlar arasında çift yönlü, gerçek zamanlı medya akışına olanak tanır.

**📌 Amazon Kinesis Data Streams**, AWS tarafından sunulan son derece özelleştirilebilir bir akış çözümüdür. Son derece özelleştirilebilir demek, veri alımı, izleme, ölçeklendirme, esneklik ve tüketim gibi akış işlemlerinin tüm parçalarının, bir akış oluşturulurken programatik olarak yapılması anlamına gelir. AWS, yalnızca talep edildiğinde kaynakları sağlayacaktır.

Burada önemli bir çıkarım, **Kinesis Data Streams'in Auto Scaling** yapabilme yeteneğine sahip **olmamasıdır**. Eğer akışlarınızı ölçeklendirmeniz gerekiyorsa, bunu çözümünüzde sizin oluşturmanız gerekmektedir. Kinesis Data Streams'in geliştirilmesini, yönetilmesini ve kullanılmasını kolaylaştırmak için AWS, API'ler, AWS SDK'ları, AWS CLI, Linux için Kinesis Agent ve Windows için Kinesis Agent sağlar.

Üreticiler yani consumer, veri kayıtlarını bir veri akışına ekler. Kinesis Üreticileri, AWS SDK'ları, Kinesis Agent, Kinesis API'leri veya Kinesis Üretici Kütüphanesi (KPL) kullanılarak oluşturulabilir. Başlangıçta Kinesis Agent yalnızca Linux içindi. Ancak, AWS Windows için de Kinesis Agent'ı piyasaya sürmüştür. Bir Kinesis Data Stream, bir dizi Shard'dan oluşur. Bir shard, bir veri kaydı dizisi içerir. Veri kayıtları bir sıra numarası (sequence number), bir bölüm anahtarı (partition key) ve bir veri bloğundan (data blob) oluşur ve bunlar değiştirilemez bir bayt dizisi olarak saklanır.

Amazon Kinesis'te, Kinesis Data Streams **bir akış depolama katmanı**dır. Bir Kinesis veri akışındaki veri dayıtları değiştirilemez yani güncellenemez veya silinemez. Akışta 24 saat ile, Kasım 2020 itibarıyla 8.760 saat arasında (365 güne denk gelir) sınırlı bir süre boyunca mevcut olur. Başlangıçta, varsayılan süre 24 saatti ve bu varsayılan süre ek bir ücret karşılığında 7 güne kadar uzatılabiliyordu. 24 saatten uzun süre ve 7 güne kadar saklanan veri kayıtları, her shard saati başına ek bir ücretle faturalandırılır. Kasım 2020 güncellemesiyle, artık 7 günden sonra veri gigabayt başına ayda faturalandırılır. Saklama süresi bir akış oluşturulurken yapılandırılır ve `IncreaseStreamRetentionPeriod()` ve `DecreaseStreamRetentionPeriod()` API çağrıları kullanılarak güncellenebilir. Kinesis veri akışından `GetRecords()` API çağrısını kullanarak 7 günden daha eski verileri almak için de bir ücret alınır. `SubscribeToShard()` API'sini kullanan Enhanced Fanout Consumer'ı ile uzun vadeli veri alma işlemi için herhangi bir ücret alınmaz.

Tüketiciler yani consumer'lar, Kinesis Data Streams'ten kayıtları alır ve işleyerek hedefe gönderirler. Bir tüketicinin her saniye bir shard'dan çekebileceği veri miktarı ve sayısı sınırlıdır. Bir shard'a bir tüketici uygulaması eklemek, mevcut veri yolunu paylaşmaları gerektiği anlamına gelir.

KCL sürüm 2 itibarıyla, Enhanced Fanout adı verilen bir Push yöntemi de bulunmaktadır. Enhanced Fanout ile, tüketiciler bir shard'a abone olabilirler. Bu, verilerin otomatik olarak shard'dan bir tüketici uygulamasına itilmesiyle sonuçlanır. Tüketiciler verileri almak için shard'ı sorgulamadığından, shard'daki paylaşılan sınırlar kaldırılır ve her tüketici shard başına 2 megabaytlık sağlanan veri yoluna sahip olur.

**📌 Amazon Kinesis Data Firehose**, AWS'nin Kinesis Data Streams'e benzer bir veri akış hizmetidir. Ancak, Kinesis Data Streams son derece özelleştirilebilirken, Data Firehose **tamamen yönetilen** bir akış teslimat hizmetidir. Alınan veriler dinamik olarak dönüştürülebilir, otomatik olarak ölçeklenebilir ve otomatik olarak bir veri deposuna teslim edilir. Bu durumda, Kinesis Data Firehose, Kinesis Data Streams'in olduğu şekilde bir akış depolama katmanı değildir.

Kinesis Data Firehose, üreticileri kullanarak verileri partiler halinde akışlara yükler ve akışa girdikten sonra veriler bir veri deposuna teslim edilir. Data Firehose akışındaki verileri işlemek için tüketici uygulamaları geliştirmenize ve özel kod yazmanıza gerek yoktur.

Kinesis Data Streams'ten farklı olarak, Amazon Kinesis Data Firehose, gelen akış verilerini hedefe teslim etmeden önce tamponlar. Tampon boyutu ve tampon aralığı, bir teslimat akışı oluştururken seçilir. Tampon boyutu megabayt cinsindendir ve hedefe bağlı olarak farklı aralıklara sahiptir. Tampon aralığı 60 saniyeden 900 saniyeye kadar değişebilir.

Özünde, veriler akış içinde tamponlanır ve tampon ya dolduğunda ya da tampon aralığı sona erdiğinde tampondan çıkar. Bu nedenle, Kinesis Data Firehose yakın gerçek zamanlı bir akış çözümü olarak kabul edilir.

Başlangıçta, Kinesis Data Firehose verileri dört veri deposuna teslim edebilirdi; 
- Amazon S3, 
- Amazon Redshift, 
- Amazon Elasticsearch,
- Splunk.

2020 yılında, bu HTTP uç noktalarının yanı sıra 3. taraf sağlayıcılar olan Datadog, MongoDB Cloud ve New Relic için genel HTTP uç noktalarını içerecek şekilde genişletildi.

Kinesis Data Streams ve Kinesis Data Firehose arasındaki bir diğer fark, Kinesis Data Firehose'un gerektiğinde otomatik olarak ölçeklenmesidir. Kinesis Data Firehose, giriş verilerinizin formatını Amazon S3'e depolamadan önce JSON'dan Apache Parquet veya Apache ORC'ye dönüştürebilir. Parquet ve ORC, JSON gibi satır tabanlı formatlara kıyasla alan tasarrufu sağlayan ve daha hızlı sorgulara olanak tanıyan sütun bazlı veri formatlarıdır.

Kinesis Data Firehose, Lambda işlevlerini çağırarak gelen kaynak verilerini dönüştürebilir ve dönüştürülen verileri hedefe teslim edebilir. Örneğin, veri JSON dışındaki bir formatta ise (örneğin, virgülle ayrılmış değerler), önce AWS Lambda'yı kullanarak bunu JSON'a dönüştürebilirsiniz.

Kinesis Data Firehose'u kullanmak için ücretsiz bir katman yoktur. Ancak, maliyetler yalnızca veriler bir Firehose akışında olduğunda ortaya çıkar. Sağlanan kapasite için değil, yalnızca kullanılan kapasite için faturalandırılırsınız.

**📌 Kinesis Data Analytics**, akıştan gerçek zamanlı olarak veri okuyabilir ve veriler hareket halindeyken toplama ve analiz yapabilir. Bunu SQL sorgularını kullanarak veya Apache Flink ile Java veya Scala kullanarak zaman serisi analizleri gerçekleştirmek, gerçek zamanlı panolar beslemek ve gerçek zamanlı metrikler oluşturmak için yapar.

Kinesis Data Firehose'u Kinesis Data Analytics ile kullanırken, veri kayıtları yalnızca SQL kullanılarak sorgulanabilir. Java ve Scala ile Apache Flink uygulamaları yalnızca Kinesis Data Streams için mevcuttur. Kinesis Data Analytics, verileri ölçeklendirme, dönüştürme, toplama ve analiz etme gibi yaygın işlem fonksiyonları için yerleşik şablonlar ve operatörlere sahiptir.

Kullanım durumları arasında ETL, sürekli metrik oluşturma ve duyarlı gerçek zamanlı analizler bulunur. ETL'i daha önce duymadıysanız; ETL Extract, Transform, Load'ın kısaltmasıdır. ETL'nin birincil amaçlarından biri, verileri zenginleştirmek, düzenlemek ve bir Veri Gölü veya Veri Ambarı şemasına uyacak şekilde dönüştürmektir.

Sürekli metrik oluşturma uygulamaları, verilerin zaman içinde nasıl trend oluşturduğunu izler ve raporlar. Gerçek zamanlı analiz uygulamaları, belirli metrikler önceden tanımlanmış eşiklere ulaştığında veya—daha gelişmiş durumlarda—bir uygulama makine öğrenimi algoritmalarını kullanarak anormallikler tespit ettiğinde alarmlar tetikler veya bildirimler gönderir.

Son olarak ücretlendirmelerin nasıl olduğuna bakalım. **Amazon Kinesis ile ücretsiz bir katman olmadığını lütfen unutmayın.**

- Kinesis Video Streams fiyatlandırması, alınan veri hacmine, tüketilen veri hacmine ve bir hesaptaki tüm video akışları boyunca depolanan veriye dayanmaktadır.

- Kinesis Data Streams fiyatlandırması biraz daha karmaşıktır. Bir Kinesis Veri Akışındaki shard sayısına dayalı olarak saatlik bir maliyet vardır. Bu ücret, veriler aslında akışta olsun ya da olmasın alınır. Üreticiler verileri akışa koyduğunda ayrı bir ücret alınır. İsteğe bağlı genişletilmiş veri saklama etkinleştirildiğinde, akışta saklanan veriler için shard başına saatlik bir ücret alınır. Tüketiciler için, ücretler Gelişmiş Fanout'un kullanılıp kullanılmadığına bağlıdır. Eğer kullanılıyorsa, ücretler veri miktarına ve tüketici sayısına dayanmaktadır.

- Firehose ücretleri, bir teslimat akışına konulan veri miktarına, Data Firehose tarafından dönüştürülen veri miktarına ve veriler bir VPC'ye gönderildiğinde teslim edilen veri miktarına ve kullanılabilirlik bölgesi başına saatlik ücrete dayanmaktadır.

- Amazon Kinesis Data Analytics, bir akış uygulaması çalıştırmak için kullanılan Amazon Kinesis Processing Units—veya KPU'ların—sayısına dayalı olarak saatlik bir ücret alır. Bir KPU, 1 sanal CPU ve 4 gigabayt bellekten oluşan bir akış işleme kapasitesi birimidir.


### Akış İşlemenin Temelleri (Fundamentals of Stream Processing)

Stream processing, Machine Learning ve Artificial Intelligence, cloud computing içinde popüler konulardır. Giderek daha fazla şirket modern stream processing araçlarını kullanmaya başlamaktadır. AWS gibi cloud sağlayıcıları daha iyi ve güçlü streaming ürünleri piyasaya sürüyor ve böylelikle talep gittikçe artıyor.

Peki, stream processing tam olarak nedir? Cevap karmaşık ama zor değil. Veriler önem kazandı çünkü tüm veriler eşit yaratılmamıştır ve değerleri zamanla değişir. Bazı bilgilerin değeri yıllarla ölçülebilir. Diğer verilerin ise sadece üretildikleri anda büyük değeri olabilir.

Stream processing ortaya çıkmadan önce, büyük hacimli veriler genellikle bir veritabanında veya enterprise-class bir sunucuda depolanır ve aynı anda işlenirdi. Bu verilerin analizi, şimdi **batch processing** olarak adlandırdığımız yöntemle yapılırdı çünkü tek bir "batch" halinde gerçekleştirilirdi.

Batch processing'de veriler belirli bir boyutta sabit parçalar halinde toplanır, depolanır ve düzenli bir programa göre analiz edilir. Program, veri toplama sıklığına ve elde edilen içgörünün ilgili değerine bağlıdır. Stream processing'in merkezinde de işte bu değer vardır.

Daha önce gördüğümüz gibi, bazı bilgiler neredeyse zamansızdır. Alfabeyi öğrenmeyi, cebirdeki işlem sırasını, kişi ve yer isimlerini düşünün. Bu veriler çok yavaş değişir ve değerleri nispeten sabit kalır. Ancak bazı bilgiler sadece erişildiği ve işlendiği anda değerlidir. Zaman açısından kritik veriler, önleyici bakım için veya bir ve daha fazla olaya gerçek zamanlı tepki vermek için kullanılır.

Hayatınızda birinin sizi cevap veremez halde bıraktığı anları düşünün. Hazırcevap bir yanıt veya karşılık vermek için mükemmel bir andır ve söyleyecek hiçbir şeyiniz yoktur. Uzaklaştıktan sonra, o mükemmel cevabı düşünürsünüz ama artık çok geçtir. An sonsuza dek kaybolmuştur. İngilizcede bu fenomeni tanımlayan bir kelime bilmiyorum ama Fransızcada buna "l'esprit de l'escalier", Almancada ise "Treppenwitz" denir. Çeviriye bağlı olarak, kelimeler "merdiven ruhu" veya "merdiven espri(si)" anlamına gelir. Yani, durumdan uzaklaşıp bir merdivenden inmeye başladıktan sonra, söylenecek tam doğru şeyi düşünürsünüz. Komik, zeki veya ilgi çekici olmak için çok geçtir.

Bu, bazı verilerin zamanla nasıl değer kaybettiğini açıklar. İşlemler gerçekleşirken, tam o anda, veriler değerlidir. Bu, ek bir ürün öneren bir öneri motoru, bir kişinin bir ürün hakkında ne hissettiğini belirleyen duygu analizi veya IoT donanım arızaları için anomali tespiti olabilir. Bu tür verileri dakikalar, saatler hatta günler sonra işlemek bir tür l'esprit de l'escalier veya Treppenwitz haline gelir. Ancak komik değildir; satışlar kaybedilir, insanlar öfkelenir veya hayal kırıklığına uğrar ve cihazlar arızalanır.

L'esprit de l'escalier sorunu aslında bir latency ve verinin zaman içindeki değeri meselesidir. Latency'den sonra, batch processing ile ilgili veri değerini etkileyen iki sorun daha var.

Bahsedeceğimiz ilk konu **session state**'lerle ilgilidir. Bu bağlamda, bir session'ı birbiriyle ilişkili olaylar veya işlemler topluluğu olarak düşünebiliriz. Batch processing sistemleri verileri tutarlı ve eşit aralıklı zaman dilimlerine böler. Bu, öngörülebilir, istikrarlı bir iş yükü oluşturur. Buradaki sorun, öngörülebilir olmasına rağmen, hiçbir zekaya sahip olmamasıdır. Bir batch'te başlayan session'lar farklı bir batch'te sonlanabilir. Bu, ilişkili işlemlerin analizini zorlaştırır.

Batch processing sistemleriyle ilgili ikinci sorun, işleme başlamadan önce **belirli bir miktarda verinin birikmesini bekleyecek** şekilde tasarlanmış olmalarıdır. Batch mimarileri, tek seferde büyük miktarda veriyi işlemek için optimize edilmiştir. Bu nedenle, bir analiz işi, işleme başlayabilmek için kuyruğun dolması gerektiğinden, uzun süreler boyunca beklemek zorunda kalabilir. Batch job'ın boyutu standart olsa da, her veri batch'indeki zaman periyodu tutarsızdır.

Steam processing, işte bu latency, session sınırları ve tutarsız yük sorunlarını çözmek için oluşturulmuştur. Streaming terimi, başı sonu olmadan sürekli akan bilgiyi tanımlamak için kullanılır. Asla bitmez ve önce indirilmesine gerek kalmadan üzerinde işlem yapılabilecek sürekli bir event akışı sağlar. Basit bir benzetme, suyun bir nehir veya dereden akışı gibidir. Su çeşitli kaynaklardan, farklı hız ve hacimlerde gelir ve tek, sürekli, birleşik bir akıntıya dönüşür. Benzer şekilde, data stream'ler çeşitli kaynaklar tarafından, farklı format ve hacimlerde üretilir. Bu kaynaklar uygulamalar, network cihazları, sunucu log dosyaları, website aktivitesi, bankacılık işlemleri ve konum verileri olabilir. Tümü, tek bir doğruluk kaynağından analiz yapmak ve yanıt vermek için gerçek zamanlı olarak birleştirilebilir.

O halde stream processing, hareket halindeki veriler üzerinde işlem yapmak veya onlara bir tepki oluşturma süreci olarak düşünülebilir. Hesaplama, veri üretildiği veya alındığı anda gerçekleşir. Stream'den bir event alındığında, bir Stream processing uygulaması buna tepki verir. Bu tepki bir eylem tetiklemek, bir toplam, benzer bir istatistiği güncellemek veya gelecekte referans olması için event'i önbelleğe almak olabilir. Birden fazla data stream eşzamanlı olarak işlenebilir ve **Consumer**'lar, yani bir stream'den veri işleyen uygulamalar, yeni data stream'ler oluşturabilir.

Steam processing'e bir örnek, kredi kartı dolandırıcılığı uyarılarıdır. Evinizden uzak bir şehirde kredi kartınızı kullandıktan sonra, telefonunuza son işlemin meşru olup olmadığını soran bir SMS aldığınız zamanlar olmuş olabilir. Bu tür bir işlem, verilerin gerçek zamanlı değerlendirilmesini gerektirir. Buna işlem, uyarı gönderme, yanıt alma ve yanıta göre hareket etme dahildir.

Sabit boyutlu batch'ler yerine, bir data stream ilişkili event'ler veya işlemler koleksiyonudur. Tipik olarak, bir stream uygulamasının üç ana parçası vardır; **Producer**'lar, bir **Data Stream** ve **Consumer**'lar.

- **Producer**'lar event'leri veya işlemleri toplar ve bir Data Stream'e koyar. 
- **Data Stream**'in kendisi verileri depolar. 
- **Consumer**'lar stream'lere erişir, verileri okur ve sonra bunlar üzerinde işlem yapar.

Streaming veri framework'lerini kullanmanın bir dizi avantajı vardır. Bazı veriler doğal olarak sonsuz bir event akışı şeklinde gelir ve en iyi durumda hala _hareket halindeyken_ işlenir. Batch processing, durağan veri mimarisi üzerine kurulmuştur. İşleme başlamadan önce, toplama durdurulmalı ve veriler depolanmalıdır. Sonraki veri batch'leri, birden fazla batch üzerinde toplam oluşturma ihtiyacını doğurur. Buna karşılık, streaming mimarileri sonsuz veri akışlarını doğal ve zarif bir şekilde işler. Stream'ler kullanılarak, kalıplar tespit edilebilir, sonuçlar incelenebilir ve birden fazla stream eşzamanlı olarak incelenebilir.

Bazen veri hacmi mevcut depolama kapasitesinden daha büyüktür. Evet, cloud'da kullanılabilir depolamaya bir sınır yok gibi görünebilir, ancak bu depolama bir fiyat etiketiyle birlikte geldiğini de unutmayalım. Ayrıca bu, insani bir zaafla da birlikte gelebilir. Bazen insanlar, _daha sonra_ ihtiyaç duyabilecekleri için verileri silmekten korkuyorlar.

Stream'leri kullanarak, ham veriler gerçek zamanlı işlenir ve sadece faydalı olan bilgi ve içgörüyü saklarsınız. Stream processing, zaman serisi verileri ve zaman içindeki kalıpların tespiti için doğal olarak uygundur. Örneğin, sürekli bir veri akışında web oturumunun uzunluğu gibi bir sırayı tespit etmeye çalışırken, bunu batch'ler halinde yapmak zor olur. IoT sensörleri tarafından üretilen zaman serisi verileri gibi veriler süreklidir ve doğal olarak bir streaming veri mimarisine uyar. Event'lerin gerçekleşmesi, içgörülerin elde edilmesi ve eylemlerin gerçekleştirilmesi arasında neredeyse hiç gecikme yoktur. Eylemler ve analizler günceldir. Bununla beraber, veriler hala taze, anlamlı ve değerliyken veri durumunu yansıtır.

Streaming, büyük ve pahalı paylaşımlı veritabanlarına olan ihtiyacı azaltır. Bir streaming framework kullanırken, her stream processing uygulaması kendi verisini ve durumunu korur ve bu nedenle stream processing doğal olarak bir microservices mimarisine uyar.

Bahsedilmesi gereken önemli bir konu ise, batch processing'in hala gerekli olduğudur. Stream processing, batch computing'i tamamlar. Ay sonu faturalandırması hala bir tür batch process kullanılarak en iyi şekilde yapılır. Faturalandırma verilerinin değeri önemli ve öngörülebilir kalır. Büyük ölçekli raporlama, pahalı, yüksek hızlı, düşük latency'li compute engine'lere ihtiyaç duymaz. Sadece, bir tüketici olarak, olası bir dolandırıcılığı öğrenmek için 30-45 gün beklemek istemezsiniz.

Stream processing, anomalileri tespit etmek, farkındalık oluşturmak veya içgörü elde etmek için verileri gerçek zamanlı veya gerçek zamana yakın bir şekilde toplamak, işlemek ve sorgulamak için kullanılır. Gerçek zamanlı veri işleme gereklidir çünkü bazı bilgi türleri için veriler toplandığı anda eyleme geçirilebilir bir değere sahiptir ve değeri zaman içinde hızla azalır. Stream processing, kaydedilen bir event'ten milisaniyeler ile saniyeler içinde eyleme geçirilebilir içgörüler sağlayabilir.

Peki stream processing ne kadar önemlidir? Belki de daha iyi bir soru, işletmenin nasıl çalıştığı, müşterilerin nasıl hissettiği veya hangi cihazların çevrimiçi ve kullanımda olduğu hakkında anında içgörü sahibi olmanın ne kadar önemli veya faydalı olduğudur? Emtialarda gerçek zamanlı ticareti düşünün; saniyenin bir kesrindeki avantaj, milyonlarca kâr veya zarar anlamına gelebilir.

Milyonlarca insanın aynı anda satın almak için giriş yaptığı ürünlerin küresel lansmanlarını yapan büyük tüketici ürünleri şirketleri hakkında ne düşünüyorsunuz? Black Friday veya Cyber Monday gibi günlerde insanlar hızlı ve tutarlı yanıtlar bekler. Her işlem anında yanıt gerektirmez, ancak birçoğu gerektirir. E-ticaret, finans, sağlık hizmetleri ve güvenlik alanında uzmanlaşmış işletmeler anında yanıt gerektirir ve **stream processing'in hedef pazarı** budur.

Sorun, şirketlerin önemli bir şeyin olduğunu fark etme yeteneğine ihtiyaç duymaları ve buna anlamlı ve anında tepki verebilmeleri gerektiğidir. Anındalık (Immediacy) önemlidir çünkü veriler son derece bozulabilir olabilir ve raf ömrü milisaniyelerle ölçülebilir.

### Streaming Framework

Bu başlık altında, Amazon Kinesis'i bir streaming framework olarak ele alalım. Amazon Kinesis ve özellikleri, gerçek zamanlı veya gerçek zamana yakın veri işlemek için birlikte çalışan parçalar koleksiyonudur. Öncelikle, streaming verilerin neden var olduğunu hatırlayalım. Streaming veriler için bir dizi yaygın kullanım senaryosu vardır. Bunlar arasında endüstriyel otomasyon, akıllı şehirler, akıllı evler, data lake'ler, log analitiği ve IoT analitiği yer alır. En popüler iki kullanım senaryosu, data lake'lere beslenen **log analitiği** ve **IoT analitiğidir**.

**IoT**, temelde geniş bir cihaz kategorisidir. IoT cihazlarını basitçe telefon, tablet veya akıllı hoparlör gibi bağlı bir cihaz olarak düşünebiliriz. Bunlar neredeyse her zaman veri gönderen connected cihazlardır. 

**Event**'ler arama sonuçları, finansal işlemler, kullanıcı aktivitesi, IoT cihazlarından gelen telemetri verileri, log dosyaları ve uygulama metrikleri vb. olabilir. Stream'deyken, veriler hareket halindeyken dinamik olarak işlenir. Bu işleme, machine learning ile gerçek zamanlı analitik, uyarılar (alerts) veya bir veya daha fazla eylemin tetiklenmesi olabilir.

Bu kapsamda unutulmaması gereken önemli bir detay vardır. Bu detay, stream'deyken verilerin işlenebileceği ancak değiştirilemeyeceğidir. Veri kayıtları değişmezdir. Bir stream'deki bilgilerin güncellenmesi gerekiyorsa, başka bir kayıt eklenir.

**Consumer**'lar stream'e bağlıdır ve gelen verileri toplayabilir, uyarılar gönderebilir ve diğer consumer'lar tarafından işlenebilecek yeni data stream'ler oluşturabilir. Veri akışına uyan stream tabanlı bir mimarinin, batch tabanlı işlemeye göre birkaç avantajı vardır. Bu avantajlardan biri düşük latency'ye sahip olmasıdır. Streaming sistemleri event'leri işleyebilir ve bunlara gerçek zamanlı olarak tepki verebilir.

Stream processing'in bir diğer avantajı, stream'lerin insanların uygulamaları nasıl kullandığını yansıtacak şekilde tasarlanabilmesidir. Bu, stream'lerin gerçek dünya süreçleriyle eşleştiği anlamına gelir. Başka bir deyişle, stream processing, insanların etraflarındaki verilerle nasıl etkileşime girdiğiyle eşleşir. Sonsuz bir event akışına sahip uygulamalar stream processing için idealdir.

Batch sistemlerinde, işleme başlamadan önce verilerin birikmesi gerektiğine değinmiştik. Stream processing kullanırken ise, hesaplama veriler gelir gelmez gerçekleşir. Data streaming, çeşitli kaynaklardan gelen yüksek hacimli, yüksek hızlı verileri gerçek zamanlı olarak almaya, işlemeye ve analiz etmeye olanak tanıyabilir.

Genel olarak, gerçek zamanlı veri streaming'in beş katmanı vardır. Bunlar **source layer**, **stream ingestion layer**, **stream storage layer** ve **stream processing layer**'dır.

**Source layer**, verilerin kaynaklarının bulunduğu yer olarak düşünülebilir. IoT sensörlerinden gelen veriler, mobil cihazlardan ve web sitelerinden gelen click-stream verileri veya uygulama logları gibi bir şey olabilir.

**Stream ingestion layer**, kaynak verilerini toplayan, uygun şekilde formatlayan ve **Data Record**'ları **stream storage layer**'a yayınlayan bir **Producer** uygulama katmanıdır. Stream storage layer, veriler için yüksek hızlı bir tampon görevi görür.

**Stream processing layer**, **Consumer** adı verilen bir veya daha fazla uygulamayı kullanarak stream storage layer'a erişir. **Consumer**'lar streaming verileri gerçek zamana yakın bir şekilde okur ve işler. Bu işleme ETL--Extract, Transform, Load--operasyonları, veri toplama, anomali tespiti veya analiz içerebilir.
 
Consumer'lar Data Record'ları beşinci katman olan **hedefe (destination)** iletir. Bu, bir Data Lake veya Data Warehouse gibi bir depolama, Amazon S3 gibi dayanıklı bir depolama veya bir tür veritabanı olabilir.

Clickstream analitiği, bir öneri motoru olarak hareket ederek kişiselleştirilmiş kupon ve indirimler oluşturmak, arama sonuçlarını özelleştirmek ve hedefli reklamları yönlendirmek için kullanılan eyleme geçirilebilir içgörüler sağlar. Tüm bunlar perakendecilerin çevrimiçi alışveriş deneyimini geliştirmesine, satışları artırmasına ve geri dönüş oranlarını iyileştirmesine yardımcı olur.

Kısa bir not olarak, eğer satış elemanlarıyla çalışmaya yeni başladıysanız, cloud'da çalışmaya başladığınızda **dönüşüm (conversion)** terimine yeni olabilirsiniz. Veri formatları yerine, potansiyel müşterileri ödeme yapan müşterilere dönüştürmek anlamına gelir.  Yani, şirketler ürünlere göz atan kişileri yani ürünlere bakan insanları, daha fazla satış yapmak için geri dönen (conversion) müşterilere dönüştürmek isterler.

Kullanım senaryolarına geri dönersek, önleyici bakımla ilgili streaming çabaları, ekipman üreticilerinin ve hizmet sağlayıcıların hizmet kalitesini izlemesine, sorunları erken tespit etmesine, destek ekiplerini bilgilendirmesine ve kesintileri önlemesine olanak tanır.

- Streaming verileri, bankaları ve hizmet sağlayıcıları şüpheli dolandırıcılıklar konusunda uyarmak, sahte işlemleri durdurmak ve etkilenen hesapları hızla bildirmek için kullanılabilir.

- Duygu analitiği ile streaming verileri, mutsuz kullanıcıları tespit edebilir ve bu mutsuzluk öfkeye dönüşmeden önce müşteri hizmetlerinin yanıtını güçlendirmesine ve tırmanmayı önlemesine yardımcı olabilir.

- Dinamik fiyatlandırma motoru ile streaming verilerini kullanmak, mevcut müşteri talebi, ürün mevcudiyeti ve bölgedeki rekabetçi fiyatlar gibi faktörlere dayalı olarak bir ürünün fiyatını otomatik olarak ayarlayabilir.

Karmaşıklığı nedeniyle, veri streaming iş akışlarının oluşturulmasının bir dizi zorluğu vardır. Tarihsel olarak, streaming uygulamaları, onları tutarsız ve otomatikleştirmesi zor kılan büyük miktarda insan etkileşimi içeren high touch sistemler olmuştur.

Data streaming uygulamalarını kurmak zor olabilir. Streaming uygulamaları, bozulma olma eğiliminde olan bir dizi hareketli parçaya sahiptir.

Source layer, ingestion layer ile iletişim kurabilmelidir. Ingestion layer, verileri stream storage layer'a koyabilmelidir. Consumer uygulamaları, stream-storage layer'daki verileri işler ve ya yeni bir stream'e koyar ya da nihai hedefine gönderir.

On-premises veri merkezlerinde oluşturulan streaming çözümlerini oluşturmak, sürdürmek ve ölçeklendirmek, hem insan hem de hesaplama maliyetleri açısından pahalıdır. Streaming uygulamaları oluşturmayla ilgili sorunlar, ölçekleme operasyonlarıyla devam eder. IoT sensör verileri, kasırgalı mevsimlerde hava hızını izlemek gibi mevsimsel olabilir. Toplanan verileri depolamak ve tüketmek için gereken kaynak sayısını artırıp azaltabilmek önemlidir.

AWS, özel streaming framework'leri ve AWS cloud'a veri akışı sağlayan uygulamalar oluşturmanın zorluklarını ele almak için Amazon Kinesis'i tanıtmıştır. Amazon Kinesis'i geliştirirken, AWS mühendisleri yüksek kullanılabilirlik ve dayanıklılığın hizmetin gerekli bir parçası olduğunu fark etti ve veri kaybı olasılığını en aza indirecek şekilde inşa edilmiştir.

Yönetilen bir hizmet olarak AWS, istek üzerine hesaplama, depolama ve bellek kaynaklarını otomatik olarak sağlar. Streaming uygulamaları, Amazon Kinesis'e ve Kinesis'ten veri yayınlamak ve tüketmek için API'ler kullanır. Kinesis tam olarak ölçeklenebilir ve elastiktir. Yani, bir iş yükünün ihtiyaçlarını karşılamak için büyüyebilir ve kaynakları israf etmeyi önlemek için küçülebilir, bu da para israfını önler.

Amazon Kinesis, çeşitli AWS hizmetleriyle entegre olur. Bunun bir avantajı, ölçekli stream processing yapan az kodlu veya kodsuz iş akışları oluşturmanın mümkün olmasıdır.

Özetle şunu hatırlayalım, streaming'in kendi başına bir şey değildir. Gerçek zamanlı veya gerçek zamana yakın veri işlemek için birlikte çalışan sistemler koleksiyonudur. AWS'den tam yönetilen bir framework'e sahip olmak, bir streaming veri sistemi oluşturmak için gereken işlerin çoğunun önceden yapıldığı anlamına gelir. Streaming altyapısı hakkında endişelenmek yerine, işinizi veya organizasyonunuzu geliştirmek için ne tür içgörülere ve analizlere ihtiyaç duyulduğuna odaklanabilirsiniz.

### (Kinesis Veri Akışının Öğeleri) The Elements of a Kinesis Data Stream

Birisi Amazon Kinesis Data Stream'den bahsettiğinde, genellikle büyük miktarda verinin yüksek hızda bir yerden başka bir yere nasıl hareket ettiğinden bahsediyordur. Bunu önceki başlıklarda yeterince gördük. Ancak gerçek şu ki, bu olanların bir genellemesidir. Streaming data, birkaç parçası olan karmaşık bir süreçtir. Streaming kelimesi açıklayıcıdır ancak tamamen doğru değildir. Bazı yönlerden, uçmayı hava yolculuğu olarak tanımlamak gibidir. Kimse sadece bir uçağa binip uçup gitmez. 

Yolcular için, uçma kısmı gerçekleşmeden önce bilet satın almak, biniş kartı almak, havaalanına gitmek, bagaj ve kabin eşyalarını halletmek ve uçağa binmek gibi her şeyi içeren bir süreç vardır.

Bu sadece yolcu deneyiminin yarısıdır. Uçak indikten ve uçaktan indikten sonra, bu süreç tersine işler, ancak elbette tam olarak aynı değildir çünkü farklı bir yerdesinizdir ve havaalanı güvenlik kontrolleri çıkan değil, giren insanlara odaklanır.

Yani, streaming sadece çok miktarda veriyi yüksek hızda taşımaktan daha fazlasıdır. Stream'lerin tamamiyle sağlanması gerekir. Bilgilerin toplanması ve doğru şekilde formatlanması gerekir. Verilerin stream'e konulması ve aynı derecede önemli olarak, geri alınması gerekir. Bu süreç karmaşık bir süreç olabilir.

Bu başlık altında, bir stream'in ne olduğunu ve verilerin nasıl içine ve dışına alınacağını ayrıntılı olarak inceleyeceğiz. Bununla beraber bir noktada kendi başınıza pratik yapmaya karar verirseniz, hizmet için ücretsiz bir kademe olmadığından Amazon Kinesis'i kullanırken ücret ödeyemek zorunda kalırsınız. Kinesis Data Streams, açık shard'lar ve bir stream'de depolanan Data Record'lar için ücret alır. Küçük ölçekte, maliyetler düşüktür.

#### Stream Oluşturma (Creating A Stream)

Bir Kinesis Data Stream oluşturmak nispeten basit bir süreçtir. AWS Console'dan, programatik olarak veya AWS CLI kullanılarak yapılabilir. Örneğin, AWS CLI kullanarak 3 shard'lı myStream adlı bir Kinesis Data Stream oluşturmak için komut şu şekildedir: 

```bash
aws kinesis create-stream --stream-name myStream --shard-count 3
```

AWS CLI komutu olan `describe-stream`, stream'in ayrıntılarını görüntüleyecek ve mevcut shard'ları hakkında bilgi içerecektir. `describe-stream` çıktısını JSON olarak görerek stream'in iç yapısını anlamak için şu komut kullanılabilir:

```bash
aws kinesis describe-stream --stream-name myStream --output jspn
```

Üretilen JSON çıktısında shard ayrıntıları en üstte ve en altta stream'in kendisi hakkında ayrıntılar bulunur. Shard ayrıntıları görmek istemeden bir Kinesis Data Stream hakkında bilgi almak istiyorsanız kullanacağınız AWS CLI komutu `describe-stream-summary`'dir.


Komut sonunda dönen bilgileri biraz inceleyelim. İlk olarak, her shard'ın kendi benzersiz kimliği vardır bu değer `ShardId` olarak karşımıza çıkacaktır. Ardından, listelenen ilk shard'ın değeri 0 olan `StartingHashKey`'i ve uzun bir sayıdan oluşan `EndingHashKey`'i olduğunu fark edebilirsiniz. Sonraki shard, ilk shard'ın `EndingHashKey`'inden bir fazla olan uzun bir sayıyla başlayan bir `StartingHashKey` aralığına sahiptir. Benzer şekilde, üçüncü shard, ikinci shard'ın `EndingHashKey`'inden bir fazla olan bir `StartingHashKey`'e sahiptir. 

Bu hash key'in shard içinde önemli bir rolü vardır. Bir Data Record bir Kinesis Data Stream'e konulduğunda, payload'ının bir parçası olarak bir `Partition Key` içerir. Kinesis Data Streams, Partition Key'i işler ve ondan bir hash değeri oluşturur. Data Record'un **hangi shard'a yazılacağını belirleyen** bu hash değeridir. Bu nedenle, tek bir kaynaktan veya tek bir konumdan gelen veriler gibi benzer veriler her zaman aynı shard veya shard'lara gidecektir.

#### Stream'a Yazma (Writing to a Stream)

Tekrarlamak gerekirse, bir Producer uygulaması verileri bir dizi Data Record olarak stream'e koyar. Bir Data Record, bir stream'de depolanan bir bilgi birimidir. Bununla beraber, bir Partition Key, bir Sequence Number ve bir Data Blob'dan oluşur. Data Record'un **Partition Key**'i, Data Record'un **yazılacağı stream içindeki shard'ı belirler**. Kinesis, Partition Key'in bir **MD5** hash değerini hesaplar ve bu değere dayanarak kaydın hangi shard'a yazılacağına karar verir.

Data Record'un **Sequence Number**'ı, her shard içinde **benzersiz** olan bir tanımlayıcıdır. Bu, verilerin süresi dolana kadar yazıldığı sırayla depolanmasını sağlar. Her shard'ın kapasitesi sınırlıdır. Bir shard'a saniyede 1 megabayta kadar 1.000 kayıt koyabilirsiniz.

Throughput'u en üst düzeye çıkarmak için, Data Record'ları mevcut tüm shard'lara dağıtmak en iyisidir. Bunu yapmak için, AWS önerisi rastgele partition key'ler oluşturan bir mantık kullanmaktır. Bu, kayıtların bir stream'deki shard'lara eşit olarak dağıtılmasını sağlamaya yardımcı olacaktır.

Çok fazla kayıt tek bir shard'a atandığında, diğer shard'lar soğuk (cold) kalırken o shard sıcak (hot) hale gelir. Bahsedilmeye değer diğer bir şey ise, partition key'in boyutunun tek bir kaydın payload'ı için 1 megabayt sınırına dahil edilmemesidir, ancak throughput sınırının bir parçası olarak sayılır.

Son olarak, data record'un Data Blob'u, 1 megabayta kadar olabilen Base64 kodlu bayt dizisidir. Bu blob, Kinesis Data Streams için hem opak hem de değişmezdir. Bu, Kinesis'in verileri herhangi bir şekilde incelemediği, yorumlamadığı veya değiştirmediği anlamına gelir.

Az önce bahsettiğimiz gibi, Data Record'lar bir Data Stream'e saniyede en fazla 1.000 kayıt, toplamda saniyede 1 megabayta kadar bir maksimum hızda konulabilir. Bu sınır, stream başına kaç shard'ın sağlanması gerektiğini belirler. Daha fazla throughput gerekiyorsa, bir stream'e daha fazla shard eklemek mümkündür. Bu sürece **resharding** denir.

Resharding işlemi AWS Console'dan, komut satırından veya programatik olarak yapılabilir. Saniyede 3.000 veri kaydı aktaran bir kullanım senaryosu için 3 shard gereklidir.

Bir Kinesis Data Stream'e yazarken, bir Data Record depolama ve işleme için tam olarak bir shard'a yerleştirilir. Süresi dolana kadar orada kalır. Bir shard içindeki **Data Record'lar değiştirilemez; düzenlenemez veya silinemezler**. Eğer bir Data Record'un güncellenmesi gerekiyorsa, onu değiştirmek için stream'e yeni **bir kayıt** eklenebilir.

#### Veri Saklama ve Faturalandırma (Data Retention and Billing Considerations)

Bir stream oluştururken, varsayılan **retention period 24 saat** olarak ayarlanır. Bu aynı zamanda retention period'un **minimum boyutu**dur. 2020 Kasım'ına kadar, bir stream'deki veriler için maksimum retention period 168 saatti. Bu 7 gün demektir. 2020 Kasım'ındaki değişiklikle birlikte, Data Record'lar bir Kinesis Data Stream'de 8.760 saate kadar (365 gün) tutulabilir.

Başlangıçta, varsayılan süre sonu 24 saatti ve ek bir ücret karşılığında 7 güne kadar uzatılabilirdi. Bu hala geçerlidir ve güncelleme öncesindeki gibi aynı şekilde çalışır. 24 saatten fazla ve 7 güne kadar saklanan Data Record'lar, her shard saati için ek bir ücrete tabi tutulur.

Artık 7 günden sonra, bir stream'de saklanan veriler, bir yıla kadar her ay gigabyte başına ücretlendirilir. Retention period bir stream oluştururken yapılandırılır ve `IncreaseStreamRetentionPeriod()` ve `DecreaseStreamRetentionPeriod()` API çağrıları kullanılarak güncellenebilir.

Ayrıca, `GetRecords()` API çağrısını kullanarak bir Kinesis Data Stream'den 7 günden eski verileri almak için de bir ücret alınır. `SubscribeToShard()` API çağrısını kullanan Enhanced Fanout Consumer kullanılırken uzun vadeli veri alımı için ise ücret alınmaz.

Bir data record tüketildikten sonra, süresi dolana kadar stream'de kalır. Bu, stream'in gerektiğinde yeniden işlenmesine veya tekrarlanmasına olanak tanır. Ayrıca, birden fazla uygulamanın aynı stream'i işleyen tüketicilere sahip olabileceği anlamına gelir.

#### Kinesis Veri Akışı Sınırları (Kinesis Data Streams Limits)

Data Stream'in limitleri hakkında daha detaylı bilgilere bakalım. Her data record'un maksimum boyut limiti 1 megabyte'tır, her shard saniyede 1.000 kayıt kabul edebilir ve varsayılan retention period 24 saattir. Data record'un boyutu artırılamaz, ancak retention period ek bir ücret karşılığında 7 güne kadar ve sonra tekrar bir yıla kadar uzatılabilir.

Producer ve Consumer uygulamalarının bazı sınırlamaları da vardır. Producer'lar, kayıtları bir stream'e koyarken, shard başına saniyede 1 megabyte veya shard başına saniyede 1.000 yazma işlemiyle sınırlıdır. 5 shard varsa, toplamda 5 MB veya 5.000 mesajlık bir throughput mevcut olacaktır. Ancak, tek bir shard için yazma limitlerini aşmak, `ProvisionedThroughputExceededException` hatasını döndürecektir.

#### Consumers

Amazon Kinesis Data Streams için 2 tür consumer vardır: **Orijinal Shared-Throughput Consumer** ve **Enhanced Fan-Out**. Bazı kişiler orijinal Consumer'a Classic Consumer veya Standard Consumer olarak da atıfta bulunur. Orijinal shared-throughput Consumer ile, her shard saniyede 2 megabyte okuma throughput'unu destekler. Ayrıca, TÜM consumer'lar arasında shard başına saniyede 5 API çağrısı limiti vardır. Bu, shard başına saniyede maksimum 10 megabyte okuma throughput'una karşılık gelir. Her shard için saniyede 5 API çağrısıyla ilgili olarak, her okuma isteği toplamda 10.000 kayda kadar dönebilir. Bu limitler aşılırsa, sonraki 5 saniye içinde yapılan sonraki istekler bir exception fırlatacak ve throttle'lanacaktır. Bunu düşünüp hesapladığımızda, bu mantıklıdır. Eğer bir istek 10 megabyte dönüyorsa, bu bir shard'dan 5 saniyelik veriye eşittir.

Kinesis Data Streams'in avantajlarından biri, aynı stream'e birden fazla consumer bağlamanın mümkün olmasıdır. Dahası, bu Consumer uygulamaları farklı olabilir. Bir uygulama, data stream'deki kayıtları toplayabilir, bunları batch haline getirebilir ve uzun süreli saklama için batch'i S3'e yazabilir. İkinci bir uygulama ise, kayıtları zenginleştirebilir ve bunları bir Amazon DynamoDB tablosuna yazabilir. Aynı zamanda, üçüncü bir uygulama stream'i filtreleyebilir ve verilerin bir alt kümesini farklı bir Kinesis Data Stream'e yazabilir.

Standard Consumer kullanırken, uygulamalar aynı saniyede 2 megabyte okuma throughput'unu paylaşırdı. Bu nedenle, aynı anda veri stream'ine verimli bir şekilde en fazla iki veya üç fonksiyon bağlanabilirdi. Standard Consumer kullanarak birden fazla uygulama arasında daha yüksek outbound throughput elde etmek için, veriler birden fazla stream arasında dağıtılmalıdır. Eğer bir geliştirici beş farklı uygulamayı desteklemek için saniyede 10 gigabyte okuma throughput'una ihtiyaç duyuyorsa, çoğaltılmış verilerle birden fazla stream'e ihtiyaç duyacaktır.

Bu sorunu çözmek için AWS, Kinesis Data Streams'e iki özellik eklemiştir: **Enhanced Fan Out** ve **HTTP/2** veri alma API'si. HTTP/2, istemciler ve sunucular arasında veri çerçeveleme ve taşıma için yeni bir yöntem sunan HTTP ağ protokolünün büyük bir revizyonudur. Gecikmeyi azaltmak ve throughput'u artırmak için tasarlanmış yeni özellikler sunan binary bir protokoldür. 

Her Enhanced Fan-Out Consumer, shard başına saniyede 2 megabyte throughput alır. Bu, standard Consumer'a benzer görünse de farklıdır. Standard Consumer, `GetRecords()` API isteği yaparak veri alır. Data Record'lar shard'dan çekilir. Tüm standard Consumer'lar aynı saniyede 2 megabyte throughput'u paylaşır. Enhanced Fan-Out ile, Consumer uygulaması önce kendisini Kinesis Data Streams'e kaydeder. Kayıt olduktan sonra, bir `SubscribeToShard()` isteği yapabilir ve veriler uygulamaya beş dakika boyunca saniyede 2 megabyte hızında gönderilir. Beş dakika sonra, uygulama kayıtları almaya devam etmek için başka bir `SubscribeToShard()` isteği yapması gerekecektir. Shard'a abone olduktan sonra, Data Record'lar otomatik olarak shard'dan consumer'a gönderilir. Her Enhanced Fan-Out Consumer, tam saniyede 2 megabyte throughput alır.

Bant genişliği paylaşımı yoktur. Bu, 5 consumer'ın shard başına saniyede 10 megabyte throughput, 10 consumer'ın shard başına saniyede 20 megabyte throughput alacağı anlamına gelir ve bu böyle devam eder. Shard başına saniyede 2 megabyte limiti ortadan kalkar. Amazon Kinesis Data Streams, HTTP/2 kullanarak verileri consumer'lara gönderdiği için bunu yapabilir. Verileri consumer'lara göndermek ayrıca shard başına saniyede 5 API çağrısı limitini de kaldırır. Bu, consumer uygulamalarının ölçeklenebilme yeteneğini artırır ve gecikmeyi önemli ölçüde azaltır.

Standard Consumer ile, shard başına saniyede 5 istek limiti vardır. Bir saniye 1.000 milisaniye olduğundan, bu her istek arasında yaklaşık 200 milisaniye olduğu anlamına gelir. Bir shard için altıncı bir consumer, ek bir saniye gecikme ekleyecektir. Bu, standard Consumer ile gecikmenin 200 ile 1.000 milisaniye arasında olduğu anlamına gelir.

Enhanced Fan-Out ile push modeli, bunu ortalama 70 milisaniyeye düşürür. Bu önemli ölçüde daha hızlıdır. Perspektif için, ortalama insan gözünün göz kırpması yaklaşık 100 milisaniye sürer. Bu performans artışı için artan bir maliyet vardır bunu unutmamak önemlidir. Ayrıca, varsayılan olarak, stream başına kaydedilebilecek 20 consumer uygulaması soft-limiti vardır. Ancak bu sayı AWS'ye bir destek bileti (support ticket) oluşturarak artırılabilir. Her consumer uygulaması aynı anda yalnızca bir veri stream'ine kaydedilebilir.

Hem Standard Consumer hem de Enhanced Fan-Out için kullanım senaryoları vardır. 200 milisaniyelik bir gecikmeyi tolere edebilen beşten az consuming uygulaması varsa Standard Consumer'ı kullanın.

Göz önünde bulundurulması gereken bir diğer faktör, Standard Consumer'ın Enhanced Fan-Out'tan daha az maliyetli olmasıdır. Maliyetleri minimize etmek önemliyse, Standard Consumer'ı kullanın. Beş ila on uygulama aynı anda aynı stream'i tüketiyorsa ve yaklaşık 70 milisaniyelik gecikme önemliyse Enhanced Fan-Out'u kullanın. Ayrıca Enhanced Fan-Out, yalnızca daha yüksek maliyetleri tolere edebiliyorsanız çalışır.

#### Özet (Summary)

Stream'ler AWS Console kullanılarak, programatik olarak bir SDK kullanarak veya AWS CLI'dan oluşturulabilir.

Bir stream sağlandığı anda ücretler birikmeye başlar. Bir stream oluştururken, benzersiz bir ada ve en az bir shard'a sahip olması gerekir. Bir steam'deki her shard'ın bir Shard ID'si, bir Hash Key aralığı ve bir başlangıç Sequence Number'ı vardır.

Shard ID shard'a özgüdür, Hash Key aralıkları shard'lar arasında çakışmaz ve Sequence Number, Data Record'ları sıralamak için kullanılır. Bir Producer bir Data Record'u bir stream'e yazdığında, Partition Key'e dayalı bir MD5 Hash oluşturulur. Bu hash key değeri, Data Record'un hangi shard'a yazılacağını belirler.

Producer'lar, kayıtları bir stream'e koyarken, bir çift limite sahiptir. Veriler shard başına saniyede 1 megabyte hızında yazılabilir veya shard başına saniyede 1.000 yazma işlemi olabilir. Bir Kinesis Data Stream'i okumak için kullanılabilen iki tür Consumer vardır; Standard Consumer ve Enhanced Fan-Out Consumer.

Standard Consumer, bir stream'den veri almak için bir polling yöntemi kullanır. Okumalar için, her shard saniyede 5 API çağrısına kadar destekleyebilir, saniyede 2 megabyte'a kadar dönebilir, toplamda saniyede 10.000 kayda kadar.

Enhanced Fan-Out, verileri Consumer'lara göndermek için bir push mekanizması kullanır. Bir stream'e en fazla 20 Consumer Uygulaması kaydedilebilir ve her Consumer shard başına saniyede 2 megabyte throughput alır. Bu throughput bir maliyetle gelir bunu unutmamak gerekir.

### Shard Kapasitesi ve Ölçeklendirme (Shard Capacity and Scaling)

Bu başlık altında, Amazon Kinesis Data Streams ile Shard Kapasitesi ve Ölçeklendirme konusunu inceleyeceğiz. Başlık altındaki içeriğe, Kinesis Data Streams'in bazı limitlerini gözden geçirerek başlayalım.

Önceki başlıkta birkaç kez üzerinde dursak da tekrardan hatırlayalım, yazma işlemleri için, Kinesis Data Streams'in kesin bir limiti vardır. Shard başına, saniyede maksimum 1 megabyte'a kadar ve 1.000 kayda kadar bir yazma hızını destekler. Okumalar için Standard Consumer kullanırken, her shard saniyede 5 işleme ve maksimum 2 megabyte'lık bir okuma hızına kadar destekleyebilir.

Bir shard üzerinde bu limitlerden herhangi biri aşılırsa, bir **ProvisionedThroughputExceededException** döndürülür ve stream throughput'u geçici olarak throttle'lanır. Hot shard'lar saniyede çok sayıda okuma ve yazma işlemine sahipken, cold shard'lar bunun tam tersidir, yani saniyede düşük sayıda okuma ve yazma işlemine sahiptirler.

Bir hot shard, tüm bir stream'in throttle'lanmasına neden olabilir. Cold shard'larla ilgili sorun ise para israf etmeleridir. Bir shard nasıl hot hale gelir? Hatırlayalım, her saniye, her shard maksimum 1 megabyte'a kadar 1.000 yazmayı destekleyebiliyordu.

Bu, eğer shard başına saniyede ortalama 1.000 yazma varsa, Data Record boyutunun **bir kilobyte'tan büyük olamayacağı** anlamına gelir. Bu değer daha büyük olursa, saniyede 1 megabyte limiti aşılır. Bir stream'e yazarken, ortalama Data Record boyutu 2 kilobyte ise, hot shard olmaktan ve throttling riskinden kaçınmak için, shard başına saniyede 500'den fazla yazma olamaz.

Bir stream'in toplam throughput'u, shard'larının kapasitelerinin toplamıdır. Bu eklemeli bir süreçtir. Shard sayısını artırmak, veri stream'inin daha yüksek işlem hızı ve kapasitesi sağlar. Benzer şekilde, shard'ları kaldırmak bir stream'in işlem hızını ve kapasitesini düşürecektir.

#### Kinesis Veri Akışını Ölçeklendirme (Scaling a Kinesis Data Stream)

Veri throughput'u, okumalar veya yazmalar için mevcut shard kapasitesini aşacak bir noktaya kadar artarsa, istekler throttle'lanacaktır. Kinesis Data Streams gerektiğinde genişleyebilir ve para israfından kaçınmak için küçülebilir, ancak bu otomatik değildir ve AWS tarafından yönetilmez. Örneğin Amazon EC2 instance'ları için olduğu gibi mevcut bir Auto Scaling yoktur. 

#### Resharding

Bir Kinesis Data Stream'i yukarı veya aşağı ölçeklendirmek, **resharding** adı verilen bir süreci içerir. Bir Kinesis Data Stream'e daha fazla throughput eklemek için, bir veya daha fazla shard ekleyebilirsiniz. Buna aynı zamanda **Shard Splitting** de denir. Bu, stream'in kapasitesini shard başına saniyede 1 megabyte artıracaktır. Shard Splitting, bir hot shard'ı bölmek için kullanılabilir. Bir shard bölündüğünde, kapatılır ve mevcut Data Record'lar süreleri dolduğunda silinir. Bir shard'ı kapatmak, Producer'ların ona veri yazmasını engeller ancak Data Record'lar süreleri dolana kadar Consumer'lar için erişilebilir kalır.

Süreç şöyle görünecektir. Örneğin 3 shard'ın olduğu bir yapı düşünelim. Bu shard'ların 1, 2 ve 3 olarak numaralandıralım. Shard 2 hot hale gelirse, bölünmesi gerekir. Bölme işlemi shard 2'yi, yani Parent Shard'ı kapatacak ve Child Shard'lar olarak Shard 4 ve 5'i oluşturacaktır. Shard 4 ve Shard 5, Shard 2'nin yerini alır. Parent Shard olan Shard 2 yazma için kapalı olsa da, Consumer'lar süresi dolana kadar içindeki verilere erişebilir. Bir resharding işleminden sonra, Parent shard'a akan veri kayıtları Child shard'lara yönlendirilir.

#### Merging Shards

Shard'ları bölmenin tersi, shard sayısını azaltmak veya kaldırmak için **Merging Shards** olarak adlandırılır. Merging Shards, mevcut shard sayısını azaltacak ve bir Kinesis Data Stream'den kapasite kaldıracaktır.

Düşük kullanıma sahip shard'lara cold denilir. Maliyetleri azaltmak için cold shard'ları birleştirmek mantıklıdır. Shard splitting'de olduğu gibi, Merging Shards işleminde de Parent Shard'lar ve bir Child Shard vardır. Parent Shard'ların durumu Closed olarak değiştirilir ve Parent Shard'lara gidecek olan Write işlemleri Child Shard'a yönlendirilir. Data Record'lar, süreleri dolana kadar Parent Shard'lardan okuma için kullanılabilir. Shard'ların süresi dolduktan sonra, silineceklerdir. 

Gelin sürece daha yakından bakalım.

Daha önceki shard splitting işlemimden sonra, stream'imiz 4 shard'a sahip. Sırasıyla 1, 4, 5 ve 3 numaralı shard'lar. Shard 5 ve 3 cold hale geldiğini düşünelim ve maliyetleri azaltmak için onları birleştirmek isteyebiliriz. Bunu yaptığımızda, yeni bir shard oluşturulur ve bu Shard 6 olacaktır.

Shard 5 ve Shard 3 (Parent Shard'lar) yazma işlemleri için kapatılır ve içlerindeki Data Record'ların süresi dolduğunda silinecektir. Shard 6 (Child Shard) Parent Shard'lara yönelik Data Record'ları ve kendi Data Record'larını alacaktır.

Resharding, programatik olarak yönetilen bir süreçtir. Shard sayısını değiştirmek için API çağrıları `UpdateShardCount`, `splitShard` ve `mergeShards`'dır. `UpdateShardCount`, stream seviyesinde bir API çağrısıdır ve belirtilen bir stream'in shard sayısını seçilen bir shard sayısına güncelleyecektir.

Shard sayısını güncellemek, bir stream aktif olarak kullanılırken gerçekleşebilen **asenkron** bir işlemdir. Kinesis Data Streams, stream'in durumunu **UPDATING** olarak ayarlar. Güncelleme tamamlandığında, Kinesis Data Streams stream'in durumunu tekrar **ACTIVE** olarak ayarlar.

Bir Data Stream **UPDATING** durumundayken Data Record'lar **yazılabilir ve okunabilir**. Shard sayısını güncellemek için, Kinesis Data Streams istendiği gibi bireysel shard'lar üzerinde bölme veya birleştirme işlemleri gerçekleştirir. Bu, geçici shard'ların oluşturulmasına neden olabilir. Bu geçici shard'lar bir hesap için toplam shard limitine dahildir.

`UpdateShardCount` kullanırken, AWS'nin önerisi %25'in katı olan bir hedef shard sayısı belirtmektir. %25, %50, %75 veya %100 gibi bir hedef yüzde değeri seçmek, resharding sürecinin diğer hedef yüzdelerden daha hızlı tamamlanmasını sağlayacaktır.

`UpdateShardCount` kullanmanın bazı sınırlamaları vardır. 24 saatlik bir süre içinde bir stream'i **ondan fazla kez** yeniden **shardlayamazsınız**. Ölçeklendirme yaparken, istenebilecek maksimum shard sayısı, şu anda stream'de bulunanın iki katıdır. Düşünürseniz, bu mantıklıdır. Bir shard-splitting işlemi bir shard'ı ikiye bölecektir böylelikle bir shard'dan iki shard elde etmiş olacağız.

Benzer şekilde, shard'ları kaldırırken, bir stream'deki mevcut shard sayısının yarısının altına ölçeklendiremezsiniz. Ayrıca, bir hesabın shard limitini aşacak şekilde ölçeklendirmek mümkün değildir. Bunlar soft limitlerdir ve AWS teknik desteğine ulaşılarak artırılabilir.

`UpdateShardCount`'un mevcut limitlerinden biri maksimum toplam 10.000 shard olduğu için, bunun AWS'nin hard limiti olduğunu çıkarabiliriz. Bununla birlikte, AWS online dokümantasyonu, bir stream'in 10.000'den fazla shard'ı varsa, shard sayısı 10.000'den az olduğunda sadece aşağı ölçeklendirilebileceğini belirten başka bir sınırlama listelemektedir.

10.000 shard'ın Kinesis Data Streams için kesin bir limit olduğundan emin olabiliriz. Bu limit hakkında bir an düşünürsek; 10.000 shard önemli miktarda streaming throughput'udur.

Ölçeklendirme işlemlerine geri dönelim. Eğer bir Kinesis Data Stream'in çok fazla veya çok az shard olduğu için yukarı veya aşağı ölçeklendirilmesi gerekiyorsa, stream'in yeniden shardlanması gerekir. Bir resharding işleminden sonra, Parent Shard'a yönelik verilerin Child Shard'a yönlendirildiğini belirtmiştik.

#### ReshardingSonrası Veri Yönlendirme, Veri Kalıcılığı ve Shard Durumu (Data Routing, Data Persistence and Shard State after Resharding)

Kinesis Data Streams gerçek zamanlı bir veri streaming servisi olduğundan, uygulamalar verilerin bir stream'deki shard'lardan sürekli olarak aktığını varsaymalıdır. Yeniden sharding işleminden önce parent shard'larda bulunan Data Record'lar bu shard'larda kalır ve Consumer'lar tarafından okunabilir.

Resharding sırasında, bir parent shard OPEN durumundan CLOSED durumuna ve ardından EXPIRED durumuna geçer. Bir reshard işleminden önce, bir parent shard OPEN durumundadır. Veri kayıtları hem shard'a eklenebilir hem de shard'dan alınabilir.

Bir reshard işleminden sonra, parent shard CLOSED durumuna geçer ve veri kayıtları artık shard'a eklenmez. Ancak, Data Record'lar süreleri dolana kadar hala erişilebilir durumdadır. Parent shard'a eklenecek olan veri kayıtları artık bunun yerine bir child shard'a eklenir.

Parent stream'in retention period'u sona erdiğinde, parent shard'daki Data Record'lara artık erişilemez. Bu noktada, shard'ın kendisi EXPIRED durumuna geçer. Reshard gerçekleştikten sonra, child shard'lardan hemen veri okumak mümkündür. Ancak, resharding'den sonra kalan parent shard'lar, henüz bir Consumer tarafından işlenmemiş veriler içerebilir.

Parent shard tamamen tüketilmeden önce child shard'dan veri okunursa, **veriler sıra dışı** olabilir. Bu, AWS tarafından yönetilmeyen ve Consumer oluştururken dikkate alınması gereken bir şeydir.

Tekrar belirtmek gerekirse, verilerin sırası önemliyse, **child shard'lardan okumaya başlamadan önce parent shard'ları tamamen** tükettiğinizden emin olun. 


#### Ölçeklendirme Sınırlamaları (Scaling Limitations)

Aktif bir stream'de shard'ları bölmek ve birleştirmek mümkündür. Ancak, bunu yaparken dikkat edilmesi gereken bir dizi sınırlama vardır. Aynı anda yalnızca bir bölme veya birleştirme işlemi gerçekleşebilir. **Birden fazla resharding** işlemini **eşzamanlı** olarak çalıştırmak **mümkün değildir**. Her bölme veya birleştirme isteği tamamlanması için biraz zaman alır.

Amazon dokümantasyonuna göre, bir Kinesis Data Stream'in 1.000 shard'ı varsa ve kapasitenin toplam 2.000 shard'a iki katına çıkarılması gerekiyorsa, tamamlanması 30.000 saniye sürecektir. Bu 8 saatten fazladır. Buradaki ana konu şudur ki, büyük miktarda kapasite gerekecekse, bunu önceden sağlamak daha avantajlıdır.

Auto Scaling, Kinesis Data Streams'in bir özelliği değildir. AWS Lambda'yı Amazon CloudWatch ve AWS Application Auto Scaling ile birlikte kullanarak programatik olarak uygulanabilir.

#### Provizyon ve Maliyet Hesaplamaları (Provisioning and Cost Calculations)

Bir stream oluşturmadan önce, kaç shard sağlayacağınızı belirlemeniz gerekecektir. Dikkate alınması gereken bir dizi değişken vardır, ancak genel olarak, kaç shard sağlanması gerektiğini belirleyen iki değer vardır. Bu sayılar, megabyte cinsinden, alınan veri miktarı ve tüketilen veri miktarıdır. Bu sayılardan büyük olanı, shard sayısını belirleyecektir. Bu değerleri belirlemek için, toplanması gereken başka veriler vardır.

İlk olarak, veri stream'ine yazılacak Data Record'un ortalama boyutunu tahmin etmeliyiz. Bu değer kilobyte cinsindendir ve en yakın kilobyte'a yuvarlanır. Verilerimize bakarak, ortalama boyut yaklaşık 500 byte olduğu bir senaryo düşünelim. En yakın kilobyte'a yuvarlamak, değeri 1.024 byte yani 1 kilobyte yapar. Bu, kilobyte cinsinden verilerin ortalama boyutudur.

Ardından, veri stream'ine saniyede yazılan kayıt sayısını tahmin öngörmeyi deneyin. Bu senaryoda bu değerin, stream'imiz için bu 5.000 olduğunu düşünelim. Bu, saniyede kayıt sayısıdır. Daha sonra, stream'den veri işleyecek Kinesis Consumer sayısına karar vermemiz gerekir. Bu Consumer'lar stream'e eşzamanlı ve bağımsız olarak erişiyor. Bu senaryo kapsamında için 10 tane olduğunu varsayalım. Bu değer, consumer sayısıdır.

Bir stream tarafından alınacak bant genişliğini hesaplamak için, saniyedeki kayıt sayısını kilobyte cinsinden verilerin ortalama boyutuyla çarpın. 5.000 çarpı 1, 5.000'dir. Bu, gelen yazma bant genişliğinin kilobyte cinsinden 5.000 olduğu anlamına gelir. Bu, her saniye alınan veri miktarıdır.

Stream'den tüketilen veri miktarını hesaplamak için, kilobyte cinsinden gelen yazma bant genişliğini consumer sayısıyla çarpın. 5.000 çarpı 10, 50.000'dir. Kilobyte cinsinden giden okuma bant genişliği 50.000'dir. Bu, ne kadar veri tüketildiğidir.

Gerekli shard sayısını belirlemek için, kilobyte cinsinden gelen yazma bant genişliğini 1.000'e bölün ve kilobyte cinsinden giden okuma bant genişliğini 2.000'e bölün. Bu iki sayıdan büyük olanı 25'tir. Dolayısıyla, gerekli toplam shard sayısı 25'tir.

Neyse ki AWS, bu hesaplamayı sizin için yapabilecek bir araca sahip. Ayrıca maliyetler hakkında da ekstra bilgi içermektedir. Şu anda, link [https://aws.amazon.com/kinesis/data-streams/pricing/](https://aws.amazon.com/kinesis/data-streams/pricing/) şeklindedir, eğer linke erişmede sorun yaşarsanız bir tarayıcı üzerinden: 'kinesis data streams pricing calculator' bu şekilde aratabilirsiniz.

Fiyatlar zaman içinde değişebilir. Şu anda, fiyatlandırma hesaplayıcısı 24.4 shard'a ihtiyacımız olduğunu gösteriyor ve bu toplam 25 shard'a yuvarlanıyor.

Faturalandırma amaçları için, bir ayda 730 saat vardır. 25 shard çarpı bir aydaki 730 saat, 18.250 shard saati sonucunu verir. Shard saati başına bir buçuk sent maliyetle, 18.250 çarpı $0.015, ABD doları cinsinden $273.75 çıkar. PUT payload'ı, 1 kilobyte boyutunda 5.000 kayıt kullanılarak hesaplanır.

Stream'lerime veri koymanın maliyeti ABD doları cinsinden $183.96'dır. 1 kilobyte boyutunda 5.000 kayıt, 25 shard'a sahip bir Kinesis Data Stream gerektirir ve ayda yaklaşık $457.71 ABD doları tutarında maliyeti olacaktır.

Unutmayın, bir Kinesis Data Stream'de verileri 7 günden fazla saklamak mümkündür ve bu ücretlere eklenecektir. Bu başlık altında ele aldığımız konuları özetleyelim:

-   Kinesis Data Streams throughput'u, sahip olduğu shard sayısına dayanır.
-   Shard başına yazma için limit, saniyede maksimum 1 megabyte'a kadar saniyede 1.000 kayıttır.
-   Standard Consumer kullanırken, her shard saniyede maksimum 2 megabyte'lık bir okuma hızına kadar 5 işleme kadar destekleyebilir.
-   Hot shard'lar saniyede çok sayıda okuma ve yazma işlemine sahipken, cold shard'lar bunun tam tersidir; saniyede düşük sayıda okuma ve yazma işlemine sahiptirler.
-   Bir hot shard **throttling**'e neden olabilir.
-   Cold shard'lar **para israf** eder.
-   Bir stream'in toplam kullanılabilir throughput'u, shard'larının kapasitelerinin toplamıdır.
-   Kinesis Data Streams ölçeklenebilir ancak bu süreç **gerçek zamanlı değildir**.
-   Ölçeklendirme sürecine **resharding** denir.
-   Shard ekleme, **Shard Splitting** adı verilen bir süreç kullanılarak yapılır.
-   Shard kaldırma **Shard Merging**'dir.
-   Bir shard bölündüğünde veya birleştirildiğinde **Parent Shard**'lar ve **Child Shard**'lar vardır. Parent Shard'lar yazma için kapatılır ancak süreleri dolana kadar okuma için kullanılabilir.
-   Bir Parent Shard kapatıldığında, ona yönelik yazma işlemleri Child Shard'a yönlendirilir.
-   Bir stream'deki shard sayısını genel olarak artırmak veya azaltmak için kullanılan süreç, `UpdateShardCount` API çağrısı kullanılarak yapılır.
-   Bireysel shard'ları bölmek veya birleştirmek mümkündür. Bu API çağrıları sırasıyla `splitShard` ve `mergeShards`'dır.
-   Shard'lar üç durumdan birinde olabilir: **Open**, **Closed** ve **Expired**.
-   Shard splitting, aktif olan bir stream üzerinde yapılabilir.
-   Ancak, aynı anda yalnızca bir bölme veya birleştirme işlemi gerçekleşebilir ve her işlemin tamamlanması zaman alır.
-   Auto Scaling, Kinesis Data Streams'in bir özelliği değildir.
-   Maliyetler, bir stream'deki shard sayısına ve bir stream'e PUT edilen veri miktarına dayanır.


### Kinesis Veri Akışındaki Veriler (Data in a Kinesis Data Stream)

Bu başlık altında, Kinesis Data Stream içinde depolanan verilere yakından bakacağız. Bahsedeceğimiz konu, bir Kinesis Data Stream'in bir tür **streaming storage** olduğudur. Bu kavramın anlaşılması için bazı bağlamlar gereklidir. Bir Kinesis Data Stream'in yüksek hızlı geçici bir depolama hizmeti türü olduğunu anlarsak, data stream'leri nasıl verimli kullanacağımızı kavramsallaştırmamamıza yardımcı olacaktır. 

Genel olarak, Amazon Kinesis Data Streams, AWS'den gerçek zamanlı bir veri alım hizmetidir. Ancak, bir Kinesis Data Stream'i bir **stream storage layer** olarak tanımlamak daha doğrudur. Bu, stream işleme sürecinin yüksek düzeyde nasıl çalıştığına bakıldığında daha anlamlı hale gelir.

Genel olarak, gerçek zamanlı veri akışının beş katmanı vardır. Bunlar **source layer**, **stream ingestion layer**, **stream storage layer**, **stream processing layer** ve **destination**'dır.

**Source layer**, çeşitli kaynaklardan toplanan verilerdir. **Stream ingestion layer**, kaynak verilerini toplayan ve üçüncü bileşen olan **stream storage layer**'a yayınlayan bir uygulama katmanıdır. **Stream processing layer**, bir veya daha fazla consumer'ın stream storage layer'daki verileri okuduğu ve işlediği yerdir. Bu işleme ETL, toplama, anomali tespiti veya analizi içerir. Son katman ise Data Warehouse, Data Lake veya ilişkisel veritabanı gibi bir **destination**'dır. Bunlar arasında Data Lake'ler en yaygın olanıdır.

Asıl veri alımı, yani Kinesis Data Stream'e veri koymak, bir **Producer** veya **Producer Application** kullanılarak yapılır. Producer'lar arasında Amazon Kinesis Agent ve Amazon Kinesis Producer Library veya bir AWS SDK ile oluşturulmuş özel uygulamalar bulunur.

Veriler, Producer'lar tarafından gerçek zamanlı olarak toplanır ve bir Kinesis Data Stream'de depolanır. Consumer Applications'a milisaniyeler içinde sunulur. Ancak, veriler bir Kinesis Data Stream'de süresi dolana kadar kalır. Varsayılan olarak, bir Kinesis Data Stream'de depolanan Data Record'lar 24 saat sonra sona erer. Bu saklama süresi, bir Kinesis Data Stream'in stream işleme için bir depolama katmanı olduğu anlamına gelir.

#### Verileri Akışa Yazma (Writing Data to a Stream)

**Producer**'lar, gerçek zamanlı işleme için verileri yakalayan ve **Kinesis Data Streams**'e gönderen uygulamalardır. AWS SDK'ları, Amazon Kinesis Producer Library, Kinesis Agent ve üçüncü taraf araçları kullanarak Producer'lar oluşturmak mümkündür. Kinesis Data Streams ile Kinesis Data Firehose arasındaki temel fark, Kinesis Data Streams'in genellikle stream'den veri almak için özel kod gerektirmesidir. Data Firehose ise Producer'ları, Stream'i ve Consumer'ları içeren tamamen yönetilen bir hizmettir. Kinesis Data Streams ile ve bunu bir depolama katmanı olarak düşünürsek, veri stream'ine farklı olan birden fazla Consumer Application ile erişmek mümkündür. Kinesis Data Firehose'un bu yeteneği **yoktur**. Bu bir dizi ödünleşmedir. Kinesis Data Streams otomatik ölçeklenmezken, Firehose stream'leri gerektiğinde otomatik olarak ölçeklenir. Ayrıca, verileri doğrudan tüketmek için özel uygulamalar oluşturmak mümkün değildir. Bunun yerine, veriler Firehose Data Stream'e girmeden önce dönüştürmek için bir AWS Lambda fonksiyonu kullanılabilir. Eğer sadece verileri S3 gibi dayanıklı bir depolamaya aktarmak istiyorsanız, Firehose geliştirme açısından daha kolay olacaktır çünkü bu amaçla tasarlanmıştır.

#### AWS CLI, SKD ve API'ler (The AWS CLI, the SKD, and APIs)

Özel bir Kinesis Producer uygulaması oluştururken, Kinesis SDK programlı olarak doğrudan bir Data Stream'e veri göndermek için kullanılabilir. API'ler ayrıca AWS komut satırı arayüzünden de çağrılabilir.

```bash
aws kinesis create-stream --streat-name myStream --shard-count 1
```

Örnek olarak, AWS CLI'dan tek bir shard ile myStream adlı bir stream oluşturmanın yöntemi yukarıdaki komut satırı kodudur. `--shard-count` parametresi gereklidir çünkü bir **shard**, bir **Kinesis Data Stream**'deki bir **ölçek** birimidir ve her Data Record bir shard'a yerleştirilir.

Bir shard, bir veri akışındaki bir bölüm olarak da düşünülebilir, ancak bu terminoloji konusunda bazı karışıklıklara neden olabilir çünkü bir Kinesis Data Stream'e veri koyarken gerekli parametrelerden biri **Partition Key**'dir. Partition Key parametresi, Kinesis tarafından veri depolanırken stream içinde hangi shard'ın kullanılacağını belirlemek için kullanılır. Bu nedenle, shard'ları bir ölçek birimi olarak düşünmek mümkündür.

Mevcut throughput (bir stream'de depolanabilecek veri miktarı dahil) ve bir stream'in maliyeti, sağlanan shard sayısı tarafından belirlenir. Daha fazla shard daha fazla throughput anlamına gelir. Daha fazla shard aynı zamanda daha yüksek bir maliyetle birlikte gelir.

Komut satırını kullanarak bir stream'in durumunu görmek için, AWS CLI komutu `describe-stream-summary` kullanılır. Bu, stream'in durumunu verecek ancak shard'lar hakkında herhangi bir ayrıntı sağlamayacaktır. Örneğin, daha önce myStream adıyla bir stream oluşturan komutun durumunu görmek için, `aws kinesis describe-stream-summary --stream-name myStream json` komutu kullanılabilir. Bu komutun çıktısı aşağıdakine benzer olacaktır.

```json
{
  "StreamDescriptionSummary": {
    "StreamName": "myStream",
    "StreamARN": "arn:aws:kinesis:us-west-2:1111aaa:stream/myStream",
    "StreamStatus": "ACTIVE",
    "RetentionPeriodHours": 24,
    "StreamCreationTimestamp": "2024-10-05T20:38:26-07:00",
    "EnhancedMonitoring": [
      {
        "ShardLevelMetrics": []
      }
    ],
    "EncryptionType": "NONE",
    "OpenShardCount": 3,
    "ConsumerCount": 0
  }
}
```

Bu örnekte, StreamStatus ACTIVE'dir. Sağlanma sürecindeyken, StreamStatus CREATING'dir. AWS CLI'yi kullanırken ve sonuçları jq'ya yönlendirirken, bu komut yalnızca durumu döndürecektir.

Jq, hafif ve esnek bir komut satırı JSON işlemcisidir. Yapılandırılmış verileri dilimlemek, filtrelemek, eşlemek ve dönüştürmek için kullanılabilir. Jq için man sayfası uzundur, ancak çevrimiçi bulunabilecek bir dizi öğretici vardır ve dokümantasyon kapsamlıdır. Örneğin aşağıdaki komut ile birlikte çıktıyı jq'ya yönlendirebilirsiniz.

```bash
aws kinesis describe-stream-summary \
	--stream-name myStream \
	--output json | \
	jq -r '.StreamDescriptionSummary.StreamStatus'
	ACTIVE
```

`-r`, ham çıktı istediğimiz anlamına gelir. Tırnak işaretleri olan JSON formatında çıktı istemiyoruz. _Bu sadece kişisel bir tercihtir_. Tek tırnak işaretleri içinde istediğimiz veri var. StreamDiscriptionSummary'den StreamStatus'u istiyoruz.

AWS CLI'yi önemli miktarda zaman harcayacaksanız veya JSON ile çalışmaya başlayacaksanız, jq istediğiniz bilgileri hızlı ve kolay bir şekilde almanıza yardımcı olacaktır. Ayrıca, bu kodu daha okunabilir hale getirmek ve ekrana güzel bir şekilde sığdırmak için bir satır devam karakteri (\) kullandık. Linux'ta, satır devam karakteri bir ters eğik çizgidir. PowerShell kullanırken, satır devam karakteri olarak işlev gören iki karakter vardır; ters tırnak ( ` ) veya boru ( | ) sembolü.

Kinesis Producer Library (KPL) ve Command Line Interface (CLI), hizmetle etkileşim kurmak için aynı API'leri kullanır. Bunun amacı, Kinesis Data Stream Producer Application'ların Data Record'ları nasıl bir Stream'e koyduğunu göstermek ve Data Stream'in Consumer Application'lar tarafından nasıl erişildiğini göstermektir.

#### Verileri Kinesis Veri Akışına Yerleştirme (Putting Data in a Kinesis Data Stream)

Bir Kinesis Data Stream'e Data Record'ları yazmak için iki farklı API çağrısı mevcuttur: `putRecord()` ve `putRecords()`. `putRecord()` metodu, bir Kinesis Data Stream'e tek bir Data Record yazar. Alternatif olarak, yazılması gereken bir kayıt grubu olduğunda `putRecords() `kullanılır.

Aynı anda birden fazla kayıt yazarken, `putRecords()` en fazla 500 kayıt veya 5 megabayt veri içeren grupları destekler. Genel olarak, `putRecords()` kullanmak `putRecord()` kullanmaktan daha tercih edilir.  

`putRecords()`'un daha fazla tercih edilme nedenleri bakalım. Throughput perspektifinden bakıldığında, Kinesis Data Streams, kayıtları gruplandırmayı bireysel isteklerle aynı şekilde ele alır. Bir grupta, her kayıt ayrı ayrı değerlendirilir ve bir shard için genel throughput sınırlarına karşı sayılır. Bununla birlikte, bir Kinesis Data Stream'e tek tek kayıt koymak, önemli miktarda istek yükü oluşturma potansiyeline sahiptir. Kayıtları gruplara koymak, AWS'ye yapılan HTTP isteklerinin boyutunu ve sayısını azaltır. Kayıtları gruplandırmak, bir uygulamanın performansını iyileştirebilir. Bir uygulama her bireysel isteğin tamamlanmasını beklemek zorundaysa, çok sayıda istek bir üretici uygulamanın gecikme süresine eklenecektir. Genel bir kural olarak, mümkün olduğunda toplu işlemi tercih edin.

#### Kinesis Veri Akışı Tüketicileri (Kinesis Data Streams Consumers)

API'leri veya SDK'yı kullanarak birden çok thread'li bir consumer uygulaması oluştururken; her thread, `getShardIterator()` API'si ile birlikte `GetRecords()` API'sini kullanarak bir shard'dan veri alır. Bir shard iterator'ı, stream'de Data Record'ları okumaya başlanacak konumu belirtir. Bu konum bir sıra numarası, zaman damgası, bir stream'deki ilk kayıt veya en son kayıt olabilir.

Veriler, shard başına saniyede 2 megabayt hızında döndürülür. Bu, hizmetin bir sınırlamasıdır ve değiştirilemez. Bir stream'de mevcut olan throughput, sağlanan shard'ların toplamıdır. Örneğin, 6 shard varsa, stream saniyede 12 MB'lık bir toplam throughput'a sahip olacaktır, ancak her shard'ın kendi 2 MB/saniye limiti olacaktır. `GetRecords()` API çağrısı, tek bir shard üzerinde saniyede yalnızca 5 kez yapılabilen bir polling işlemidir. Bu da hizmetin bir sınırlamasıdır ve değiştirilemez. Bu, consumer'ların stream'den veri isteyebileceği en hızlı sürenin 200 ms olduğu anlamına gelir çünkü saniyede 5 istek eşiğinin altında kalması gerekir.

Eğer bir consumer 100 ms'lik bir polling penceresi ile `GetRecords()` çağırırsa, bu saniyede 10 istek yapacağı anlamına gelir. İsteklerin 5'i throttle edilecek ve Kinesis hizmeti bir exception döndürecektir. Consumer daha sonra exception'ı işlemek, bir tür geri çekilme işlemi yapmak ve ardından isteği yeniden denemek zorunda kalacaktır.

Bir stream'den veri isterken dikkat edilmesi gereken iki maksimum vardır. Tek bir `GetRecords()` API isteğiyle döndürülebilecek maksimum kayıt sayısı 10.000'dir. Tek bir `GetRecords()` isteğinin döndürebileceği maksimum veri miktarı 10 MB'dır. Eğer bir istek bu kadar veri döndürürse, sonraki 5 saniye içinde yapılan çağrılar bir **ProvisionedThroughputExceededException** döndürecektir. Bu, throughput'u saniyede 2 MB'da tutar. Polling penceresi örneğinde olduğu gibi, 10 MB'lık bir veri kaydını işleyen bir consumer'ın sonraki istekleri ele almak için bir tür mantığa sahip olması gerekecektir.

Bu sınırlamalar shard başınadır. Birden fazla consumer tek bir shard'a erişirse, _saniyede 2 MB'lık_ limit **ve** _200 ms erişim hızını_ **paylaşırlar**. Bu değerleri yuvarlarsak, bu, tek bir shard'ın onu sorgulayan 5 consumer'ı varsa, her bir consumer'ın shard'a saniyede bir kez erişebileceği anlamına gelir. Bu biraz karmaşık bir matematik işlemine döndü, gelin adım adım daha yakından bakalım.

Bir saniyede 1.000 milisaniye vardır. Bu tek saniye 200 milisaniyelik limite bölünürse, 1.000'in 200'e bölümü 5'tir. 5 consumer bir shard'a saniyede bir kez erişebilir. Benzer şekilde, shard'ın throughput'u saniyede 2 MB ile sınırlı olduğundan, bu miktarı 5 consumer arasında bölmek, her birinin saniyede yaklaşık 400 kilobayt alabileceği anlamına gelir. Buradaki çıkarım, bir shard'a consumer eklemek, her consumer için mevcut throughput'u düşürür. Bazı kullanıcılar için için bu sınırlama bir sorundur. Neyse ki, AWS bunu ele almak için **Enhanced Fan-Out** adında yeni bir tüketici yani consumer türü yarattı. 

#### Tüketici Uygulamaları Nasıl Çalışır (How Consumer Applications Work) ?

Consumer Application'ların nasıl çalıştığının basitleştirilmiş bir versiyonu şöyle düşünülebilir: Bir döngü ile başlar. Data Stream'de mevcut olan ilk veri kaydının konumunu almak için `getShardIterator()` kullanılır. Bir stream'deki en eski kayıt trim horizon'dadır.

Aslında, bir Kinesis Data Stream'den Data Record'ları almanın beş olası başlangıç pozisyonu vardır:

- AT_SEQUENCE_NUMBER, Data Stream'de belirli bir pozisyondan okumaya başlar.
- AFTER_SEQUENCE_NUMBER, belirtilen pozisyondan hemen sonra okumaya başlar.
- AT_TIMESTAMP, Data Stream'den belirtilen zamandan itibaren okumaya başlar.
- TRIM_HORIZON - az önce bahsettiğim gibi - shard'daki en eski kayıttan başlar.
- LATEST, shard'a konan en son kayıtla başlar.

`getRecords()` tarafından birkaç öğe döndürülür. Bunlardan biri **NextShardIterator** değeridir. Bu, shard'daki bir sonraki sıra numarasıdır. Bir sonraki Data Record'u almak için bu değeri `getRecords()` ile kullanmalısınız.

İstek bir `null` bir değer döndürene kadar döngüye devam edin. Null olduğunda, shard kapatılmış ve istenen iterator artık veri döndürmeyecektir.

#### Hata Yönetimi (Managing Failure)

Bilgisayarın ilk günlerinde  kod yazarken odak noktası, hataların meydana gelmesini önlemekti. Bu hala önemlidir. Sorunları verimli ve uygun maliyetli bir şekilde çözen kaliteli kod yazmak hala çok önemlidir.

Bununla birlikte, bir sistem ne kadar dağıtık olursa, hataların meydana gelebileceği yerler o kadar çok olur. Uygulama geliştiricilerinin kontrolü dışındaki hatalar. Hataların meydana geleceğini beklemek, onları genel bir şekilde öngörebileceğim anlamına gelir. Bu, Amazon Kinesis Data Streams için de geçerlidir.

Bir istek, yeniden denenebilir bir hata nedeniyle başarısız olursa, AWS SDK varsayılan olarak isteği üç kez yeniden dener. Bunun bir örneği, ServiceUnavailable veya benzer geçici 500 seviyesi bir hata nedeniyle meydana gelen bir başarısızlık olabilir. Yeniden denemeler, üstel geri çekilme (exponential backoff) adı verilen bir şey kullanır. Varsayılan gecikme 100 milisaniyedir ve sonraki yeniden denemeler arasındaki süre üstel olarak artar. Bir stream sağlarken, yeniden deneme sayısı ve temel gecikme yapılandırılabilir ve değiştirilebilir.

AWS SDK'nın içinde hata yönetimi olduğu görünse de, bunun doğru olmadığı durumlar vardır. 200 yanıtı almak mümkündür (istek başarılı oldu),  burada hizmet kullanılabilir durumdaydı ancak sağlanan throughput aşıldığı için bir veya daha fazla kayıt işlenmediğini düşünelim. **FailedRecordCount** değeri 0'dan büyükse, bu bir isteğin kısmi başarısızlığı olduğu anlamına gelir. Bazı kayıtlar başarıyla bir stream'e yazılmış ancak bir veya daha fazlası başarısız olmuştur.

Toplu işlemler atomik değildir. Atomik bir işlem, ya tamamen başarılı olan ya da tamamen başarısız olan bir işlemdir. Kinesis Data Streams için AWS SDK, kısmi başarısızlıkları başarı olarak değerlendirecek ve ardından bir sonraki işleme geçecektir.

Bu, AWS SDK'yı kullanarak Producer uygulamaları oluştururken, 200 yanıt koduna güvenmenin veri kaybına neden olabileceği anlamına gelir. Bu tür bir başarısızlığın ana nedeni, bir stream'in veya bireysel bir shard'ın throughput'unu aşmaktır. Bu, throughput gereksinimlerini dikkatli bir şekilde hesapladıktan ve bir stream'i uygun şekilde sağladıktan sonra bile, trafik artışları ve ağ gecikmesi nedeniyle meydana gelebilir. Bu şeyler, kayıtların stream'e düzensiz bir şekilde konmasına neden olabilir. Bu da throughput'ta artışlara yol açar.

#### Ağ Gecikmesi (Network Latency)

VPC içinde çalışan Producer Application'lar için ağ gecikmelerini azaltmak amacıyla, her zaman bir Interface VPC Endpoint kullanmak faydalıdır. Bu, Amazon VPC ile Kinesis Data Streams arasındaki trafiği Amazon ağı içinde tutacaktır. Aksi takdirde, VPC'den Kinesis Data Streams'e giden trafik bir Internet Gateway üzerinden yönlendirilmelidir.

Bir endpoint kullanarak, Kinesis Data Streams'e giden trafik VPC içinde kalır. Bu, gecikmeyi en aza indirir ve genel olarak verilerin public internet'e maruz kalma riskini azaltır.

Dalgalı trafikle başa çıkmak söz konusu olduğunda, producer uygulamanızda bir tür geri basınç uygulamayı deneyebilirsiniz. Yazılım geliştirmede, geri basınç akışkanlar dinamiğinden ödünç alınan bir kavramdır. Bu, bir boru içinden geçen istenen gaz veya sıvı akışı miktarına karşı dirençtir. Kinesis bağlamında, geri basınç genellikle verilerin bir stream boyunca istenen akışını engelleyen direnci ifade eder. Kinesis Data Streams, bir Producer Application kullanarak bir kaynaktan veri alır, onu bir stream'e koyar ve bir Consumer Application kullanarak bir hedefe iletir. Geri basınç, bu verileri stream boyunca taşıma sürecinin dirençle karşılaştığı durumdur. Bir şey onu yavaşlatır. Bu, yetersiz hesaplama kaynakları, throughput sınırları veya mimari tasarım nedeniyle olabilir.

Özetle, kısmi başarısızlıklar için uygun hata işleme ve yeniden denemelere sahip olmak önemlidir. FailedRecordCount 0'dan büyük olduğunda, yeniden denemeleri manuel olarak yapmak mümkündür. Doğru kayıtları almak için, PutRecords() isteğinin yanıtından gelen kayıt dizisini kullanın. Bu dizi, giden istekle tam olarak aynı sırada olan bireysel kayıt yanıtlarını içerir. İki diziyi karşılaştırın ve `RecordId` yerine `ErrorCode` ve `ErrorMessage` içeren kayıtları seçin. Yeniden denemeler için bir üst sınır belirlemek önemlidir. Bir noktada, süreç vazgeçmeli ve SQS dead letter queue eşdeğerine bir istek göndermelidir.

#### Kinesis Producer Library (KPL)
 
Bir Kinesis Data Stream'e kayıt koymak oldukça çaba gerektirir. Verilerin doğru şekilde biçimlendirilmesi gerekir ve hata işleme gibi eforlar ortaya çıkar. Bunu kendi başınıza yapmak tamamen mümkündür. Ancak, AWS'de Kinesis'i yaratan kişiler bunun yaygın ve düzenli bir ihtiyaç olacağını fark ettiler. Veri alımı ve tüketiminin karmaşıklıklarını ele almak için, işi kolaylaştırmak amacıyla bir çift kütüphane oluşturdular ve yayınladılar.

**Kinesis Producer Library (KPL)** AWS'den temin edilebilen, bir Kinesis Data Stream'e veri koyan kütüphanedir. Kinesis producer uygulamalarının geliştirilmesini basitleştirmek için oluşturulmuştur ve programcıların bir Kinesis data stream'e yüksek yazma throughput'u elde etmesine olanak tanırken, aynı zamanda istisna ve hata işlemeyi yönetmelerini sağlar.

SDK'yı kullanarak, `putRecord()` ve `putRecords()` metodları bir veya daha fazla Data Record'u bir Kinesis Data Stream'e koyacaktır. Bu metodlar, verilerin bölümlenmesi ve tutarlı bir şekilde birden fazla shard'a iletilmesi gerektiğinde doğru shard'a yazmayı bile ele alır.

Peki, neden KPL ile uğraşalım? SDK'ları kullanırken, ilk kullanımda tamamen açık olmayan bazı sınırlamalar vardır. Bir Amazon Kinesis shard'ı saat başına faturalandırılır ve saniyede maksimum 1 megabayt hızında saniyede 1.000 kayda kadar yazma işlemini destekler.

İkili ve onlu taban matematiği karıştırıldığında, hayal kırıklığına uğratan ve kafa karıştıran bazı ilginç yuvarlama hataları oluşturabilir. Şimdilik, işi basit tutalım ve detaylıca bakalım. 1.000 kilobayt bir megabayttır. Bu Onlu tabandadır. Eğer ikili (binary) taban olsaydı, 1.024 kilobayt bir megabayt olurdu. Fark küçük görünüyor ama büyük bir etkisi olabilir.

Şimdilik, Base10’da 1.000 kilobyte’ın bir megabyte olduğu fikrini kullanalım. Kinesis Data Stream’in giriş limitine ulaşmanın bir yolu, saniyede 1.000 adet 1 kilobyte boyutunda kayıt yazmaktır. Bu yaklaşık olarak 1 megabyte’a denk gelir.

Gerçek dünyada, bu pek olası değildir çünkü çoğu akış verisi çok daha küçük kayıt boyutlarından oluşur. Örneğin, başarısız giriş denemelerini izleyen ve her birini bir Kinesis Data Stream’e gönderen bir sistemi ele alalım. Her bir Data Record (Veri Kaydı) muhtemelen yalnızca bir URL, bir IP adresi ve bir kullanıcı adı içerir ve boyutu muhtemelen 50-60 bayt civarında olur, 1.000 bayt değil.

Bir shard’ın tam kapasitesini kullanmazsanız, temelde Amazon Web Services'e (AWS) boşuna para veriyorsunuz demektir çünkü kullanmadığınız throughput (geçiş kapasitesi) için ödeme yapıyorsunuz. Harcanan para açısından en kötü senaryo, bir shard’a tek bir bayt yazmak ve saniyede 1.000 bayt kullanım ücreti ödemektir. Bunun bir çözümü, bir Kinesis Data Stream’e yazmadan önce tek bir Data Record içine birden fazla kaydı koymaktır.

Birden fazla kaydı tek bir Data Record içinde yönetmek için AWS, **Kinesis Producer Library**’yi (KPL) oluşturdu, bu da Data Record toplama işlemi içindir. Bununla birlikte çalışan bir diğer kütüphane ise **Kinesis Client Library** (KCL), bu kütüphane de Data Record ayıklama işlemini yapar. Amaç, mevcut throughput’un olabildiğince fazlasını kullanmaktır. Data Record’lar, Amazon Kinesis için şeffaf değildir. Hizmetin kendisi, içinde ne olduğunu görmez. Bu yüzden verilerin akışa girmeden önce ve çıktıktan sonra yönetilmesi gerekir. Bu manuel olarak yapılabilir. Ancak AWS, bu tür bir işlevselliğe ihtiyaç duyan yeterli sayıda insan olduğunu fark etti ve bu kütüphaneleri yayınladı.

KPL, Amazon Software License altında lisanslanmıştır ve bu lisans size şu hakları tanır: **"...Eserini ve türevlerini çoğaltmak, türev çalışmalar hazırlamak, kamuya açık olarak sergilemek, kamuya açık olarak icra etmek, alt lisans vermek ve dağıtmak için sürekli, dünya çapında, münhasır olmayan, telif ücreti gerektirmeyen bir telif hakkı lisansı..."**

Kinesis Data Stream’e küçük kayıtlar yazarken, KPL kullanmak throughput’u iyileştirebilir ve maliyetlerinizi düşürebilir. KPL, Kinesis Data Streams ile çalışacak şekilde tasarlanmıştır, **Kinesis Data Firehose ile değil.** KPL yalnızca bir Kinesis Data Stream’e veri koyabilir.

Firehose, veri akışları için kendi üreticilerine sahiptir. Ancak ilginç olan şey, Firehose’un olası üreticilerinden birinin Kinesis Data Streams olmasıdır. Çözüm, Data Record’ları KPL ile toplamak ve bunları bir Kinesis Data Stream’e koymaktır. Ardından, Data Firehose’un bu Data Stream’i bir üretici olarak kullanmasını sağlamak.

Firehose, KPL tarafından toplanan veriyi tanıyacak kadar akıllıdır ve Data Record’ları hedeflerine göndermeden önce uygun şekilde açabilir.

KPL’nin içinde yeniden deneme (retry) mantığı vardır. Bir `putRecords()` çağrısından bir hata veya istisna döndüğünde, KPL otomatik olarak yeniden deneyecektir. Yeniden denemelerin uygun zamanda yapılması için tam bir hata yönetim mekanizması vardır. KPL kullanarak bir akışa konulan kayıtlar her zaman en az bir kez Kinesis Data Stream’e görünmelidir.

#### Kinesis Client Library (KCL)

Kinesis Client Library (KCL), Kinesis Producer Library (KPL) tarafından oluşturulan bir Kinesis Data Stream’den veri okumak için oluşturulmuştur. KPL, shard ve yazma kullanımını en üst düzeye çıkarmak için kayıtları birleştirir. Buna karşılık, KCL bu kayıtları ayırır. Ancak, KCL yalnızca KPL tarafından oluşturulan kayıtları işleyen bir uygulama çerçevesinden fazlasıdır. KCL, akış ve dağıtık hesaplama ile ilgili birçok karmaşık görevi halleder.

KCL, birden fazla tüketici yani consumer uygulama örneği arasında yük dengelemesi yapabilir, tüketici uygulama örneği arızalarına yanıt verebilir ve kayıtları kontrol noktası oluşturarak resharding (shard yeniden boyutlandırma) işlemlerine tepki verebilir. Temel olarak, kayıt işleme mantığı ile Kinesis Data Streams arasında bir aracı görevi görür. Bir Tüketici uygulaması oluşturmak için gerekli olan tüm mantığı manuel olarak kodlamak yerine, KCL işin çoğunu otomatik olarak yapar. KCL, AWS SDK’larında bulunan Kinesis Data Streams API’lerinden farklıdır.

Kinesis Data Streams API’leri, akışları oluşturma, shard yeniden boyutlandırma, kayıt ekleme ve alma gibi işlemleri yapmanızı sağlar. KCL, bu yaygın görevlerin tümü için bir soyutlama katmanı sağlar, böylece her Consumer Application'ı oluşturmanız gerektiğinde bunları tekrar tekrar oluşturmanız gerekmez.

KCL’in burada bahsedilmeye değer birkaç özelliği var. KCL ile bir **checkpoint (kontrol noktası)** özelliği mevcuttur, bu sayede bir uygulama kapandığında tüketici kaldığı yerden devam edebilir. Uygulama yeniden açıldığında, kaldığı yerden işlemeye devam edebilir.

Checkpoint’ler, Data Record'un sıra numarası ile ilişkili alt sıra numarasını izlemek için DynamoDB kullanır. DynamoDB'de, bir Kinesis Data Stream'deki her shard için bir satır vardır. Bunu burada belirtmeye değer çünkü Checkpoint özelliğiyle, DynamoDB'nin sağladığı kapasiteyle ilgili throttling (kısıtlama) yaşamak mümkündür.

Bu, Kinesis Data Stream’in doğru şekilde yapılandırılmış olmasına rağmen, DynamoDB yeterli Okuma Kapasitesi Birimlerine veya Yazma Kapasitesi Birimlerine sahip değilse, throughput’un kısıtlanacağı anlamına gelir. Bunun bir çözümü, DynamoDB için talep üzerine sağlama (on-demand provisioning) kullanmaktır.

Sonuç olarak, yetersiz yapılandırılmış bir DynamoDB tablosu, KCL performansını kısıtlayabilir.

#### Özet (Summary)

Özetlemek gerekirse; 
- Bir veri akış hizmetinin beş katmanı vardır: kaynak katmanı, akış alım katmanı, akış depolama katmanı, akış işleme katmanı ve hedef.

- Amazon Kinesis Data Streams, AWS’den gelen tüm akış hizmetini tanımlarken, Kinesis Data Stream (tekil), akış verileri için bir depolama katmanıdır.

- AWS CLI’dan bir akış oluşturmak için `create-stream` API çağrısını kullanabilirsiniz.

- Bir akışın durumunu AWS CLI’dan görmek için `describe-stream` veya `describe-stream-summary` API çağrısını kullanın.

- Bir Data Record’u bir akışa eklemek için `putRecord()` ve `putRecords()` API çağrılarını kullanabilirsiniz.

- Bir shard’da veri okumaya nereden başlayacağınızı belirlemek için `getShardIterator()` API çağrısı kullanılır. Belirli bir sıra numarası, belirli bir sıra numarasından sonraki Data Record, bir zaman damgası, en eski Data Record veya en yeni Data Record geçerli seçeneklerdir.

- AWS, Producer ve Consumer oluşturmayı hızlı ve verimli hale getirmek için Kinesis Producer Library (KPL) ve Kinesis Client Library (KCL) yayınladı.

- KPL kayıtları birleştirir ve KCL kayıtları ayırır. KCL ayrıca Tüketici uygulamaları oluşturmaya yönelik yaygın görevleri otomatik olarak halledebilir.

### Kinesis Veri Akışı Güvenliği (Kinesis Data Streams Security)

Bir Kinesis Data Stream'in nasıl güvence altına alınacağı konusuna geçmeden önce güvenlik prensiplerini tartışmakla başlayalım.

Bir üretim (production) hesabında, **En Az Ayrıcalık İlkesi**ni (The Principle of Least Privilege) takip etmek önemlidir. Bunu yapmanın bir dizi nedeni vardır ve genellikle üç kategoriye ayrılırlar; **kurumsal (organizational)**, **teknik (technical)** ve **kişisel (personal)**.

Genel olarak, bizler verilerimizin yöneticileriyiz. **Kurumsal** açıdan, insanlara işlerini yapmak için gereken en az erişimi vermek, verilerin **yetkisiz erişim** ve **kullanım** **riskinin** **sınırlı** olmasını sağlar.

**Teknik** açıdan, modüler olan bir veri yöneticiliği kontrolleri seti oluşturmak, bir organizasyon içindeki uyumluluğu yönetmeyi, uymayı ve uygulamayı kolaylaştırır.

**Kişisel** nedenleri ve insan doğasını göz önünde bulundurarak, insanlara zamanla daha fazla ayrıcalık vermek, erişimi geri almaktan **her zaman** **daha kolaydır**. Birinin sizden erişim ayrıcalıklarını aldığı bir zamanı hatırlayabiliyor musunuz? Bunlara ihtiyacınız olmasa bile veya bu yetkilere sahip olmak hayatınıza istenmeyen stres eklese bile, duygusal bir etkisi vardır.

Kendinizi bir sürü dertten kurtarın ve bakımınızdaki verileri korumak için **En** **Az** **Ayrıcalık** **İlkesi** ni kullanın.

AWS içinde Kimlik ve Erişim Yönetimi (Identity and Access Management - IAM) servisinin nasıl çalıştığına dair hızlıca bir göz atalım.

AWS bulutunda yeterince zaman geçirdikten sonra, tüm hizmetler için varsayılan iznin **REDDET** olduğunun acı verici şekilde farkına varacaksınız. Yani, AWS bulutu içinde, herhangi bir şey yapabilmek için, izinin açıkça verilmesi gerekir.

Bunu yapmak için, bir hizmete veya kaynağa erişim ihtiyacı olan kullanıcı, grup veya rol, bir poliçe belgesi oluşturulur ve eklenir. Bir IAM politikası içinde üç temel öğe vardır: Effect (Etki), Action (Eylem) ve Resource (Kaynak).

- **Effect** ya İzin Ver (Allow) ya da Reddet (Deny)'tir. Bir **Reddet** izni **geçersiz kılınamaz**.

- **Action**, bir cümlenin fiili gibidir. Ne yapılabileceğini tanımlar. Örneğin burada, `DescribeStreamSummary` ve `ListStreams` gibi tanımlar kullanılabilir. Get kelimesinden sonra bir yıldız işareti kullanmak, **Get** kelimesiyle başlayan herhangi bir eyleme izin verildiği anlamına gelir. Bu, `GetShardIterator` ve `GetRecords` gibi ifadeleri içerecektir.

**Resource**, stream'in Amazon Kaynak Adı (Amazn Resource Name yani ARN)'dır.

ARN'lere yeniyseniz, bu Amazon Resource Name'in kısaltmasıdır ve AWS içindeki kaynakları benzersiz şekilde tanımlar. Bireysel alanlar iki nokta üst üste ile ayrılır.

arn** ile başlayarak her alanı açıklayalım.

- **arn** - daha önce de belirttiğimiz gibi gibi, Amazon Resource Name anlamına gelir. Temel olarak kendi kendini tanımlamadır.

- **aws** her ARN'de bulunur.

- Üçüncü alan hizmettir, bu durumda **Kinesis**.

- Dördüncüsü **bölge**dir.

- Beşinci alan **hesap numarası**dır.

- Son olarak stream gelir. **stream** öneki ve ardından belirli bir **stream adı**nın geldiği bir ileri eğik çizgi vardır.

Asterisk karakterleri ARN'lerde kullanılabilir ve joker karakter görevi görürler.

```json
{
  "Version": "2024-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "kinesis:PutRecord*"
      ],
      "Resource": [
        "arn:aws:kinesis:us-east-1:111122223333:stream/*"
      ]
    }
  ]
}
```

Yukarıda bir hesapta, herhangi bir stream'e veri eklenmesine izin veren bir IAM Policy örneğini görebilirsiniz. Bu IAM policy'sinde `PutRecord` değerimde sonraki asterisk, `PutRecord()` ve `PutRecords()` API çağrılarının çalışacağı anlamına gelir.

```json
{
  "Version": "2024-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "kinesis:Get*",
        "kinesis:DescribeStreamSummary"
      ],
      "Resource": [
        "arn:aws:kinesis:us-east-1:111122223333:stream/stream1"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "kinesis:ListStreams"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

Yukarıdaki json formatında, daha uzun bir policy dökümanı görebilirsiniz. Bu, bir kullanıcı veya grubun belirli bir stream üzerinde (bu poliçe kapsamında **stream1**) `DescribeStreamSummary`, `GetShardIterator` ve `GetRecords` işlemlerini gerçekleştirmesine ve herhangi bir stream üzerinde `ListStreams` yapmasına izin verir. Bu policy'de Resource'un bir asterisk (*) olduğuna dikkat edin. Bu, bu policy'nin kullanıldığı hesaptaki herhangi bir stream anlamına gelir. Unutmadan hatırlamakta fayda var, kendi hesabınız dışında bir hesaba erişim izni veren bir policy oluşturamazsınız.

Tekrar belirtmek gerekirse, production hesaplarında en az ayrıcalık ilkesini kullanmak önemlidir. Veriye sahip değiliz, sorumluluğumuz altındaki verinin emanetçileriyiz. Bize emanet edilen bilgileri korumak önemlidir.

Bununla birlikte, demolarımız ve deneylerimiz için, tam yönetici ayrıcalıklarına sahip bir hesap kullanmak veya Amazon Kinesis Data Streams için tam ayrıcalıklar vermek çok daha kolaydır.


```json
{
  "Version": "2024-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "kinesis:*"
      "Resource": [
        "arn:aws:kinesis:*:111122223333:stream/*"
      ]
    }
  ]
}
```

Yukarıdaki policy, seçilen bir hesaptaki tüm stream'lere tam erişim sağlar. 

**Bu policy tehlikelidir ve production hesabının yakınında bile olmamalıdır.**

Bunu production'da kullanmak bir RBE'ye (Resume Building Event), yol açabilir. 

Güvenlik konusunda, Kinesis Data Streams ile çalışan IAM policy'leri oluştururken en iyi uygulamalar, rollere dayalı 4 farklı policy oluşturmayı içerir.

**Administrators**, **Stream Resharding**, yazma işlemleri için **Producers** ve okuma işlemleri için **Consumers** için ayrı policy'ler olmalıdır.

Örnek olarak şu şekilde düşünebiliriz.

- **Administrators**'ın `CreateStream`, `DeleteStream`, `AddTagsToStream` ve `RemoveTagsFromStream` gibi IAM Action'larına ihtiyacı vardır.

- **Stream Resharding** isteyenler için IAM action'ları `MergeShards` ve `SplitShard`'ı içerir.

- **Producers**'in veri kayıtları yazabilmek için `DescribeStream`, `PutRecord` ve `PutRecords` IAM action'larına ihtiyacı vardır.

- Benzer şekilde, **Consumers**'in bir Kinesis Data Stream'den okuyabilmek için `GetRecords` ve `GetShardIterator` IAM action'larına ihtiyacı vardır.

Bu rollerin her biri için gerekli olacak başka action'lar da olacaktır.

Örneğin, düşündüğümüzde, Consumers  için olan policy muhtemelen `DescribeStream`'i de kullanabilir.

Herhangi bir iş tanımı için tek bir en iyi policy yoktur. Bu, organizasyonunuzda hangi diğer servislerin ve action'ların gerekli olduğuna bağlıdır. Neyin mümkün olduğunu görmek ve uygun action'ları gerektiği gibi eşleştirmek için mutlaka dokümantasyona başvurun.

Streaming verinin farkında olmadığınız bir özelliğini keşfedebilir veya mevcut bir süreci yönetmek için daha iyi bir yol öğrenebilirsiniz. Ayrıca, uygun olduğunda geçici güvenlik kimlik bilgilerini IAM rolleri şeklinde kullanmanın genel bir AWS en iyi uygulaması olduğunu belirtmeliyim. Gerektiğinde bir rolü üstlenin ve işiniz bittiğinde bırakın.

IAM policy'lerini erişimi kontrol etmek için kullanmanın yanı sıra, Kinesis Data Streams, **HTTPS endpoint'leri** kullanarak veriyi _**uçuşta**_ şifreleyebilir.

Amazon Key Management Service (KMS) kullanılarak, durağan veriler için şifreleme mevcuttur. KMS kullanarak, Data Records, Kinesis stream depolama katmanına yazılmadan önce şifrelenir ve alındıktan sonra şifresi çözülür. Sonuç olarak, Data Records'ı Kinesis Data Streams servisi içinde durağan haldeyken şifrelenir, bu da düzenleyici gereksinimleri karşılar ve veri güvenliğini artırır.

Veriyi client tarafında şifrelemek mümkündür. Verinin şifresinin de client tarafında çözülmesi gerekir. Bu manuel bir süreçtir ve client tarafı şifrelemeyi uygulamaya yardımcı olacak AWS'den bir şey olmadığı için KMS kullanmaktan çok daha zorlayıcıdır.

FIPS 140-2 şifrelemesi gerektiriyorsanız, FIPS endpoint'leri mevcuttur. FIPS'in ne olduğunu bilmiyorsanız, muhtemelen bir FIPS endpoint'i kullanmanıza gerek yoktur. Ancak, referans olması açısından, FIPS **Federal Information Processing Standard** (Federal Bilgi İşleme Standardı) anlamına gelir. Askeri olmayan devlet kurumlarında ve bu kurumlarla çalışan devlet yüklenicileri ve satıcıları tarafından kullanılmak üzere belge işleme, şifreleme algoritmaları ve diğer bilgi teknolojisi standartlarını tanımlayan bir standartlar kümesidir.

Eğer Kinesis Uygulamaları bir VPC içindeyse, Kinesis Data Streams'e erişmek için VPC Endpoint'lerini kullanın. Kinesis ve uygulama arasındaki tüm ağ trafiği VPC içinde kalacaktır.

Esasen, VPC Endpoint'leri, public Internet'e erişmeden bir VPC içinden AWS Servislerine bağlanmanızı sağlar. Bir private subnet'e bir Internet Gateway veya NAT Gateway bağlama ihtiyacını ortadan kaldırırlar.

Endpoint'ler AWS tarafından yönetilir ve otomatik olarak yatay olarak ölçeklenir ve yüksek kullanılabilirliğe sahiptir.

Oluşan senaryo şöyle düşünülebilir: VPC içindeki bir subnet'te özel bir IP adresi kullanılarak bir ENI (Elastic Network Interface) sağlanır. Bu ENI'ye erişimi kısıtlamak için bir security group kullanılır ve Kinesis Data Streams gibi istenen servise trafik göndermek için kullanılır.

Özet olarak, bu başlık altında değindiklerimizi maddeleyelim:

- AWS içinde kaynakları sağlarken En Az Ayrıcalık İlkesini kullanmayı unutmayın.

- Bu, bir geliştirme ortamında daha az önemli olabilir, ancak bir production ortamında, kişisel ve profesyonel hedeflerinizden biri **her zaman** bir RBE, yani Resume Building Event yaratmaktan kaçınmak olmalıdır.

- Kinesis Data Streams için En İyi Uygulamalar, **Administrators**, **Stream Resharding**, yazma işlemleri için **Producers** ve okuma işlemleri için **Consumers** için ayrı güvenlik policy'leri bulundurmayı içerir.

- **Uçuşta (In-flight)** olan streaming veriler için HTTPS endpoint'leri mevcuttur.

- Bir Kinesis Data Stream içinde, Data Records KMS kullanılarak şifrelenebilir ve şifresi çözülebilir.

- Kinesis Data Streams ve VPC'lerle çalışırken, ağ trafiğini VPC içinde tutmak ve Public Internet'ten uzak tutmak için bir VPC Endpoint kullanın.


### Gerçek Zamanlı Mesajlaşma ve Kinesis Veri Akışları (Real-Time Messaging and Kinesis Data Streams)

Gerçek zamanlı işleme (real-time processing) fikri ile başlayalım. Gerçek zaman yani real-time, bilgi işlemde sonuçların anlamlı olabilmesi için veri transferleri ve işlemlerin çok kısa sürede tamamlanması gereken bir performans seviyesini tanımlar. Örneğin borsa, trader'ler buna göre hareket edebilmesi için hisse senedi fiyatlarını yayınlarken gerçek zamanlı çalışır. Nesnelerin İnterneti (IoT - Internet of Things) alanındaki çeşitli mevcut cihazlar, eyleme geçirilebilir sonuçların mümkün olması için sürekli olarak alınacak ve işlenecek veriler gönderir. Tatildeyken ev güvenlik sisteminin ev içinde bir hareket dedektörünün etkinleştirildiğine dair bir uyarı göndermesini düşünün. Bu uyarı, siz veya müdahale eden kurum mümkün olduğunca hemen almıyorsa anlamsız olacaktır. Bu tür gerçek zamanlı işlevler, bilgi işlem sistemleri ve çözümleri gelişmeye devam ettikçe daha popüler ve gerekli hale gelmiştir. Streaming uygulamaları, veri transferlerinin hızının müşteri deneyimi üzerinde doğrudan etkisi olduğu yaygın bir kullanım durumudur.

Stream edilen müzik veya video akıcı bir şekilde oynamıyorsa, müşterinin dikkatini korumanın zor olacağını tahmin edebilirsiniz. Hem veri transferinin hızı hem de hacmi, gerçek zamanlı veri işleme uygulamalarında kritik faktörler haline gelir. Kuyruklar, topic'ler veya bunların kombinasyonu ile uygulanan geleneksel mesajlaşma, nadiren gerçek zamanlı veri transferleri gerçekleştirebilir. Eğer ki veriniz, 256 KB'den büyük yüksek hacimli ise veri transferlerinde gerçek zamanı yakalamak zor olacaktır. Bu tür bir uygulama için, büyük veri kümelerinin alınmasına ve depolanmasına izin veren yeni ve farklı bir tür mesajlaşma sistemine ihtiyacımız var; Amazon Kinesis Data Streams'in amacı budur. Kinesis Data Streams, varsayılan olarak 24 saat boyunca ve uygun şekilde yapılandırılırsa 365 güne kadar alınan tüm verilerin bir kopyasını alındığı sırayla tutan gerçek zamanlı bir veri toplama mesajlaşma servisidir. Bu işlem, **increase stream retention period** ve **decrease stream retention period** operasyonları kullanılarak yapılır. Saklama süresi saat cinsinden belirtilir ve mevcut saklama süresini describe stream operasyonunu kullanarak öğrenebilirsiniz. Kinesis Data Streams, büyük veri kümelerinin gerçek zamanlı işlenmesini ve kayıtların birden çok tüketici uygulamaya okunmasını ve yeniden oynatılmasını sağlar.

Bir stream, bir veya daha fazla shard'dan oluşur ve her shard veri kayıtlarını sırayla saklar. Veri kayıtları bir partition key, bir sequence number ve bir megabayta kadar gerçek veriden oluşur. Üreticiler Kinesis Data Streams'e veri koyar ve tüketiciler Kinesis Client Library'yi kullanarak veriyi işler. Tüketiciler yani consumers EC2 instance filosunda çalışan uygulamalar olabilir. Bu durumda Kinesis Client Library uygulamanıza derlenir ve Kinesis Data Stream'den kayıtların ayrık, tam toleranslı ve yük dengeli tüketimini sağlar. Kinesis Client Library, AWS SDK'da bulunan Kinesis Data Streams API'sinden farklıdır. Kinesis Client Library, her shard için bir kayıt işlemcisinin çalıştığını ve shard'ı işlediğini garanti eder. Bu, veri stream'inden bağlantı ve okuma işlemini kayıt işleme mantığınızdan ayırarak bir stream'den veri okumayı basitleştirir. Kinesis Client Library'nin kontrol verilerini saklamak için bir DynamoDB tablosu kullandığını ve bir stream'den veri işleyen her uygulama için bir tablo oluşturduğunu da unutmamalıyız. Library, gerekirse EC2 instance'larında, Elastic Beanstalk'ta ve hatta kendi veri merkezinizde çalıştırılabilir.

Kinesis Data Stream'da bir üretici (producer) veriyi bir stream'e ilettiğinde otomatik olarak şifreleyebilir. Şifreleme için AWS KMS master key'lerini kullanır. Bu, uygulama günlük verileri, sosyal medya verileri, gerçek zamanlı kazanç panoları ve lider tabloları, borsa veri akışları, web sitelerinden tıklama sırası verileri ve nesnelerin interneti için veri işleme uygulamalarının çoğu gibi gerçek zamanlı veri toplamak ve bir araya getirmek için geçerlidir. Bir kaydın bir stream'e konulması ve alınmaya hazır olması arasındaki süreye **put-to-get** gecikmesi denir ve Kinesis Data Stream'de bu bir saniyeden azdır. Yani bir Kinesis Data Stream uygulaması, veriler gelmeye başladıktan hemen sonra stream'den veri tüketmeye başlayabilir.

Kinesis Data Streams, üreticilerden veri yazmak ve almak için bir veya daha fazla shard'dan oluşur. Bir shard saniyede 1.000 kayıt işleyebilir. Bir stream'in veri kapasitesi, kullanılan shard sayısıyla orantılıdır. Ve yeterli sayıda shard ile, on binlerce kaynaktan saniyede gigabayt'larca veri toplayabilirsiniz. Bir shard, bir stream'deki kayıtların bir dizisidir. Bir stream, her biri sabit kapasiteye sahip bir veya daha fazla shard'ı temsil eder. Her shard, saniyede en fazla bir megabayt'lık maksimum hıza kadar saniyede 1.000 kayıt hızında yazılabilir. Okuma için, bir shard saniyede en fazla iki megabayt'a kadar bir hızı sürdürebilir. Bir stream için veri hızını artırmak, içindeki shard sayısını artırma meselesidir.

Bir shard içinde, veriler kayıt olarak yazılır ve her kayıt bir megabayt'a kadar olabilir. Bir kaydın anatomisi üç bölümden oluşur. İlki, bir stream'deki bir shard'da veriyi gruplamak için kullanılan bir **partition key**'dir. Partition key, kaydın ait olduğu shard'ı tanımlar. İkincisi, bir shard içindeki her partition key için benzersiz olan bir **sequence number**'dır. Aynı partition key için sequence number'lar, kayıtların geliş sırasını korumaya yardımcı olur. Sequence number'lar aynı partition key için zamanla artar. Üçüncüsü, bir megabayt'a kadar olan kayıt için **gerçek veridir**. Bir kayıttaki gerçek verinin Kinesis Data Streams servisi tarafından herhangi bir şekilde incelenmediğini, yorumlanmadığını veya değiştirilmediğini belirtmek önemlidir, bu işlemlerin herhangi birini gerçekleştirmek gerekirse tüketici uygulamaya bağlıdır.

Kinesis Data Stream'deki bir shard'ın kapasitesi, stream'iniz için talep üzerine kapasite (on demand capacity) veya sağlanan kapasite (preovisioned capacity) modu olarak yapılandırılabilir. Talep üzerine kapasite kullanarak, Kinesis Data Streams iş yükünüz tarafından ihtiyaç duyulan throughput'u sağlamak için shard sayısını otomatik olarak yönetir ve ayarlar. Throughput, uygulamanızın ihtiyaçlarına göre yukarı ve aşağı ayarlanır; gerçekte kullandığınız throughput için ücretlendirilirsiniz. Sağlanan kapasite modunda, veri stream'i için shard sayısını belirtmeniz gerekir. Gerektiğinde bir veri stream'indeki shard sayısını artırabilir veya azaltabilirsiniz. Bununla beraber, saatlik bir oran üzerinden shard sayısı için ücretlendirilirsiniz. Yeniden sharding işlemleri; gerektiğinde bir stream'deki shard sayısını artırmak ve azaltmak için shard'ları bölmeyi ve birleştirmeyi içerir. Stream'in toplam kapasitesinin kullanılan tüm shard'ların kapasitesinin toplamı olduğunu unutmayın.

Kısacası, Kinesis Data Streams, akan büyük verinin gerçek zamanlı işlenmesine ve kayıtların birden çok Amazon Kinesis uygulamasına yeniden oynatılmasına olanak tanır. Amazon Kinesis Client Library, belirli bir partition key için tüm kayıtları aynı kayıt işlemcisine ileterek, sayma, toplama ve filtreleme amacıyla aynı Amazon Kinesis stream'inden okuyan birden çok uygulama oluşturmayı kolaylaştırır.

### Kinesis Data Firehose

Bir Kinesis veri stream'inin ikinci tür consumer'ı, bir Amazon Kinesis Data Firehose teslimat stream'i olabilir. Adından da anlaşılacağı gibi, bir firehose teslimat stream'i büyük veri kümelerini alabilir, dönüştürebilir ve Amazon S3, DynamoDB, Amazon Elastic Map Reduce, OpenSearch, Splunk, Data Dog, New Relic, Dynatrace, Sumologic, LogicMonitor, MongoDB, HTTP uç noktaları ve Amazon Redshift gibi hedeflere yükleyebilir. Kinesis Firehose, verinizi bir hedefe almak ve depolamak için gereken tüm altyapıyı, depolamayı, ağı ve yapılandırmayı yönetir. Tam yönetimlidir, yani süreci yönetmek için herhangi bir donanım, yazılım sağlamanız, dağıtmanız, bakımını yapmanız veya herhangi bir uygulama yazmanız gerekmez. Otomatik olarak ölçeklenir ve diğer birçok AWS depolama hizmeti gibi, verileri bir bölgedeki üç tesise çoğaltır. Kinesis Firehose, hedeflere yüklemeden önce giriş stream'ini önceden tanımlanmış bir boyuta ve önceden tanımlanmış bir süreye kadar tamponlar.

Tampon (buffer) boyutu megabayt cinsindendir ve S3 için 1 megabayt'tan 128 megabayt'a, OpenSearch için 1 megabayt'tan 100 megabayt'a ve lambda fonksiyonları için 0.2 megabayt'tan 3 megabayt'a kadar değişir. Buffer aralığı saniye cinsindendir ve 60 saniyeden 900 saniyeye kadar değişir. Kinesis Firehose, teslimat hedefi kullanılamıyorsa verileri 24 saate kadar saklayacaktır, ancak kaynak bir Kinesis veri stream'i ise, bu durumda veriler veri firehose yapılandırmasına göre değil, veri stream'i yapılandırmasına göre saklanacaktır. Amazon Redshift'e veri koyma durumunda, Kinesis Firehose, Redshift kümenize veri yüklemeden önce ilk adım olarak Amazon S3'ü kullanır. Kinesis Data Firehose shard'ları kullanmaz ve ölçeklenebilirlik açısından tamamen otomatiktir. Kinesis Firehose, depolama hedeflerine teslim etmeden önce verileri sıkıştırabilir ve şifreleyebilir. Amazon S3, OpenSearch ve Splunk hedefleri için, veriler dönüştürülürse, isteğe bağlı olarak kaynak verileri başka ve farklı bir S3 bucket'ına yedekleyebilirsiniz. Firehose hızlı çalışır, ancak gerçek zamanlı değildir. Kinesis Firehose'u hedeflere depolamak için kullanırken 60 saniye veya daha fazla gecikme beklemelisiniz.

Ayrıca, Kinesis Firehose için, içinden geçen veri miktarı için ödeme yaparsınız. Kinesis Data Firehose genellikle Kinesis Data Stream kayıtlarını AWS depolama hizmetlerine almak için kullanılan teslimat hizmetidir. Kinesis Data Firehose'a mesaj üreticileri Kinesis Data Streams ile sınırlı değildir ve herhangi bir uygulama Kinesis Firehose'un AWS Depolama hizmetlerine teslim etmesi için mesajlar üretebilir. Kinesis Agent, bir kez kurulup yapılandırıldığında, teslimat stream'inize veri toplayan ve gönderen önceden hazırlanmış bir Java uygulamasıdır. Kinesis Agent'ı web sunucuları, günlük sunucuları ve veritabanı sunucuları için Linux sistemlerine kurabilirsiniz. Agent ayrıca GitHub'da da mevcuttur. Amazon Linux, Red Hat Linux ve Microsoft Windows işletim sistemleri desteklenmektedir. Hem Kinesis Data Streams hem de Kinesis Firehose, Kinesis Data Streams, Kinesis Data Firehose, Kinesis Data Analytics ve Kinesis Video Streams'i içeren Kinesis streaming veri platformunun bir parçasıdır.

### Amazon OpenSearch Service

Bu başlık altında, Amazon OpenSearch Service'ine yakından bakacağız. Bu hizmet hem AWS Certified Developer - Associate hem de AWS Certified DevOps Engineer - Professional sınavları için yeni kapsama alındı, bu yüzden biz de bu hizmet ve ne yaptığı hakkında hızlı bir genel bakış yapabiliriz.

Amazon OpenSearch Service, Amazon Elasticsearch Service'in halefidir. AWS'den tam yönetilen bir arama hizmetidir ve OpenSearch kümelerini dağıtmanıza olanak tanır. OpenSearch, Amazon tarafından oluşturulan, Elasticsearch ve Kibana'ya dayalı açık kaynaklı bir arama, analitik ve görselleştirme paketidir. Bu nedenle, ELK stack'i (Elasticsearch, Logstash ve Kibana) ile aşina olan geliştiriciler OpenSearch ile çalışırken kendilerini çok rahat hissetmelidir. OpenSearch ile büyük veri koleksiyonlarını kolayca indeksleyebilir ve arayabilirsiniz, bu da web sitesi arama işlevselliğinden gerçek zamanlı uygulama izlemeye ve interaktif günlük analizine kadar her şeyi uygulamanıza olanak tanır.

OpenSearch hem Elasticsearch hem de OpenSearch API'lerini sağlar, bu da uygulamalarınıza özel arama işlevselliği eklemeyi kolaylaştırır. Ayrıca Kibana ve OpenSearch Dashboards gibi görselleştirme araçlarıyla da entegre olur.

OpenSearch ayrıca şu özellikleri de destekler:

-   Yüksek kullanılabilirlik için bir OpenSearch kümesini 2 veya 3 kullanılabilirlik bölgesine dağıtma yeteneği,
-   Amazon CloudWatch Logs, Kinesis Data Firehose ve S3 ve DynamoDB dahil diğer AWS hizmetlerinden akan verileri alma yeteneği,
-   Hem sık güncellenen hem de salt okunur arşiv verileri için uygun maliyetli depolama seçenekleri sunan hot, UltraWarm ve cold depolama katmanları dahil olmak üzere katmanlı depolama.

Amazon OpenSearch Service'in ayrıca Amazon OpenSearch Serverless olarak bilinen sunucusuz bir seçeneği de vardır. Bu, bir OpenSearch kümesi sağlamak veya yönetmek zorunda kalmadan OpenSearch çalıştırmanıza olanak tanır. Amazon S3 ile aynı veri dayanıklılığını sunar ve yalnızca kullandığınız kaynaklar için ödeme yaparak maliyetleri kontrol etmenizi sağlar.

### Amazon Athena

Bu başlık altında Amazon Athena'yı göz atacağız. Bu hizmet, AWS Certified Developer - Associate sınavı için yeni kapsama alındı, bu yüzden size bu hizmet ve ne yaptığı hakkında bilgi sahibi olmak faydalı olacaktır.

Amazon Athena, standart **Structured Query Language (Yapılandırılmış Sorgu Dili)** yani **SQL** sözdizimini kullanarak Amazon S3 data lake'inde, veri ambarları ve büyük veri depoları dahil olmak üzere diğer birçok veri kaynağında depolanan verileri interaktif olarak sorgulamanızı sağlayan tam yönetilen bir hizmettir. Bu, yeni bir sorgu dili öğrenmek veya herhangi bir altyapı sağlamak ve yönetmek zorunda kalmadan petabayt büyüklüğündeki veri setleri üzerinde analiz yapmayı kolaylaştırır. Athena sunucusuz olduğundan, yalnızca çalıştırdığınız sorgular için ödeme yaparsınız.

Athena, tabloları tanımlamak için **Data Definition Language (Veri Tanımlama Dili)** kısa adıyla **DDL** kullanır ve CSV, JSON ve Parquet dahil olmak üzere çeşitli dosya formatlarında depolanan verileri sorgulamayı destekler.

Athena ayrıca veri analizi ve sorgulamanıza yardımcı olmak için birçok başka özellik de sunar:

-   Büyük veri setlerini herhangi bir sütuna göre sorgulamanıza olanak tanıyan bölümleme anahtarları belirleme yeteneği,
-   Data lake analitiği yapmanın yanı sıra güçlü analitik pipeline'ları oluşturmak için QuickSight ve Redshift Spectrum ile entegrasyonu kolaylaştıran EMR ve Glue ile entegrasyon,
-   Depolama maliyetlerini azaltmaya yardımcı olabilecek S3'te depolanan sıkıştırılmış verileri sorgulama desteği.

Başlamak için AWS SDK'larını, Athena API'sini veya Athena ile etkileşim kurmak için bir ODBC veya JDBC sürücüsü kullanabilirsiniz.







