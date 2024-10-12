# Management and Governance (DVA-C02)

Bu başlık altında bulunan konular, geliştiriciler için AWS'deki yönetim ve denetim hizmetlerine bir giriş sağlamaktır. Başlığın genel kapsamında, aşağıda listelenen servislere değineceğiz:

-   AWS AppConfig,
-   AWS Cloud Development Kit (AWS CDK),
-   AWS CloudFormation,
-   AWS CloudTrail,
-   Amazon CloudWatch,
-   Amazon CloudWatch Logs,
-   AWS Command Line Interface (AWS CLI), 
-   AWS Systems Manager.

## CloudWatch

Bu başlık altında, Amazon CloudWatch'un ne olduğu ve ne yaptığı hakkında üst düzey bir genel bakışa sahip olacaksınız.

Amazon CloudWatch, uygulamalarınızın ve altyapınızın sağlığını ve operasyonel performansını gözlemlemeniz için tasarlanmış global bir servistir. Kaynaklarınızdan anlamlı operasyonel verileri bir araya getirip sunarak onların performansını izlemenize ve gözden geçirmenize olanak tanır. Bu, CloudWatch'un sunduğu içgörülerden yararlanma fırsatı verir, bu da otomatik yanıtları tetikleyebilir veya gerekirse altyapınızı optimize etmek için manuel operasyonel değişiklikler yapma ve kararlar alma fırsatı ve zamanı sağlar.

Ortamınızın sağlığını ve performansını anlamak, olayları, kesintileri ve hataları en aza indirmenize yardımcı olabilecek temel operasyonlardan biridir. Sonuç olarak, Amazon CloudWatch operasyonel rolde olanlar ve site güvenilirlik mühendisleri tarafından yoğun olarak kullanılmaktadır.

Amazon CloudWatch'un çeşitli bileşenleri vardır ve bu onu son derece güçlü bir servis haline getirir. Şimdi size bu özelliklerin bazılarını ve neler yapmanıza olanak tanıdığına değinelim. Bunlar özellikler, arasında CloudWatch Dashboards, CloudWatch Metrics ve Anomaly Detection, CloudWatch Alarms, CloudWatch EventBridge, CloudWatch Logs, CloudWatch Insights bulunmaktadır.

AWS Management Console, AWS CLI veya PutDashboard API'sini kullanarak, kaynaklarınızla ilgili metrikleri ve alarmları gösteren farklı görsel widget'lar kullanarak birleşik bir görünüm oluşturmak için bir sayfa oluşturabilir ve özelleştirebilirsiniz. Bu dashboard'lar daha sonra AWS Management Console'dan görüntülenebilir.

İşte dashboard'unuzu oluşturmak için seçebileceğiniz farklı widget türlerine bir örnek.

Özelleştirilmiş dashboard'unuzdaki kaynaklar birden fazla farklı bölgeden olabilir, bu da onu çok kullanışlı bir özellik haline getirir. Kendi görünümlerinizi oluşturabildiğiniz için, iş ve operasyonel perspektiften görmeniz gereken verileri temsil eden farklı dashboard'ları hızlı ve kolay bir şekilde tasarlayıp yapılandırabilirsiniz. Örneğin, belirli bir projeyle veya belirli bir müşteriyle ilgili tüm performans metriklerini ve alarmlarını görüntülemeniz gerekebilir. Ya da belirli bir bölge veya uygulama dağıtımı için farklı bir dashboard oluşturmak isteyebilirsiniz. Önemli nokta, verilerinizi nasıl temsil etmek istediğinize göre tamamen özelleştirilebilir olmalarıdır.

<div align="center">

![drVEzib.md.png](https://iili.io/drVEzib.md.png)

</div>

Dashboard'larınızı oluşturduktan sonra, AWS hesabınıza erişimi olmayan kişiler de dahil olmak üzere diğer kullanıcılarla kolayca paylaşabilirsiniz. Bu, CloudWatch tarafından toplanan bulguları, log kayıtları operasyonel rollerinde sonuçları ilginç ve faydalı bulabilecek ancak AWS hesabınıza erişme ihtiyacı olmayan kişilerle paylaşmanıza olanak tanır.

Metrikler, Amazon CloudWatch'un başarısı için temel ve kilit bir bileşendir, bir uygulamanın veya kaynağın belirli bir öğesini bu veri noktalarını takip ederken belirli bir süre boyunca izlemenize olanak tanır. Örneğin, bir EC2 instance'ındaki DiskReads veya DiskWrites sayısı, EC2 ile ilgili izleyebileceğiniz sadece 2 metriktir. Farklı servisler farklı metrikler sunar, örneğin Amazon S3 için DiskReads yoktur çünkü bir compute servisi değildir, bunun yerine servisle ilgili metrikler mevcuttur. Örneğin belirtilen bir bucket'taki nesne sayısını takip eden NumberOfObjects gibi.

Amazon CloudWatch ile çalışırken varsayılan olarak herkes ücretsiz bir Metrik setine erişime sahiptir ve EC2 için bunlar 5 dakikalık bir zaman dilimi üzerinden toplanır. Ancak, küçük bir ücret karşılığında, metriklerdeki verileri her dakika toplayarak daha derin bir içgörü elde etmenizi sağlayacak detaylı izlemeyi etkinleştirebilirsiniz. Detaylı izlemeye ek olarak, uygulamalarınız için ihtiyacınız olan herhangi bir zaman serisi veri noktasını kullanarak kendi özel metriklerinizi de oluşturabilirsiniz, ancak bir metrik oluşturduğunuzda bunların bölgesel olduğunu unutmayın, yani 1 bölgede oluşturulan herhangi bir metrik başka bir bölgede kullanılamaz.

CloudWatch metrikleri ayrıca anomali tespiti olarak bilinen bir özelliği etkinleştirmenize olanak tanır. Bu, CloudWatch'un genellikle beklenen normal temel parametre dışında kalan herhangi bir aktiviteyi tespit etmeye yardımcı olmak için metrik verilerinize karşı makine öğrenimi algoritmaları uygulamasına olanak tanır. Önceden uyarılmalar sayesinde, bir production sorunu haline gelmeden önce bir sorunu tespit etmenize yardımcı olabilir.

Amazon CloudWatch Alarms, az önce bahsettiğim metriklerle sıkı bir şekilde entegre olur ve her metrikle ilgili yapılandırabileceğiniz belirli eşiklere dayalı otomatik eylemler uygulamanıza olanak tanır.

Örneğin, bir EC2 instance'ının CPUUtilization'ı 5 dakikadan fazla %75'e ulaştığında başka bir instance oluşturmak gibi bir auto scaling operasyonunu etkinleştirmek için bir alarm ayarlayabilirsiniz. Aynı şekilde, aynı instance %75 eşiğinin altına düştüğünde bir SNS Topic'e mesaj göndermek için bir alarm yapılandırabilirsiniz, bu da alarm durumundan çıkmasına neden olarak mühendisleri değişiklik konusunda bilgilendirir.

Alarm durumlarından bahsetmişken, bir metrikle ilişkili herhangi bir alarm için 3 farklı durum vardır: 
- **OK** : Metrik, tanımlanan yapılandırılmış eşik dahilindedir, 
- **ALARM** : Metrik, belirlenen eşikleri aşmıştır,
- **INSUFFICIENT_DATA** : Alarm yeni başlamıştır, metrik mevcut değildir veya alarm durumunu belirlemek için metrik için yeterli veri mevcut değildir.

CloudWatch alarmları aynı zamanda dashboard'larınızla da kolayca entegre edilebilir, her alarmın durumunu hızlı ve kolay bir şekilde görselleştirmenize olanak tanır. Bir alarm ALARM durumuna geçtiğinde, dashboard'unuzda kırmızıya dönecek, çok belirgin bir gösterge sağlayacaktır.

CloudWatch EventBridge, Amazon Events adlı mevcut bir özellikten evrilmiş bir özelliktir. Dolayısıyla CloudWatch Events ile daha önceden deneyiminiz varsa, bu size oldukça tanıdık gelebilir. CloudWatch EventBridge, kendi uygulamalarınızı çeşitli hedeflere, genellikle AWS servislerine bağlamanın bir yolunu sunar, böylece gerçek zamanlı bir izleme düzeyi uygulamanıza olanak tanır ve uygulamanızda meydana gelen olaylara gerçekleştikleri anda yanıt vermenizi sağlar.

Peki bir olay yani event nedir? Temel olarak bir olay, ortamınızda veya uygulamanızda bir değişikliğe neden olan herhangi bir şeydir. CloudWatch EventBridge kullanmanın büyük faydası, gerçek zamanlı bir ayrıştırılmış ortamda olay odaklı bir mimari düzeyi uygulama fırsatı sunmasıdır. EventBridge, uygulamalarınız ile belirtilen hedefler arasında bir bağlantı kurar ve olay akışının gönderilmesine olanak tanır. 

Bu özelliğin bazı elementlerine hızlıca göz atalım. bunlar arasında Rules (Kurallar), Targets (Hedefler) ve Event Buses (Olay Veri Yolları) bulunmaktadır.

- Kurallar ile başlayalım. Bir kural, gelen olay trafiği akışları için bir filtre görevi görür ve ardından bu olayları kural içinde tanımlanan uygun hedefe yönlendirir. Kuralın kendisi trafiği birden fazla hedefe yönlendirebilir, ancak hedef aynı bölgede olmalıdır.

- Hedefler, olayların kurallar tarafından gönderildiği yerler, örneğin AWS Lambda, SQS, Kinesis veya SNS gibi. Hedef tarafından alınan tüm olaylar JSON formatında alınır.

- Son olarak, Event Buses. Bir Event Bus, olayı uygulamalarınızdan alan bileşendir ve kurallarınız belirli bir event bus ile ilişkilendirilir. CloudWatch EventBridge, AWS servislerinden olayları almak için kullanılan varsayılan bir Event bus kullanır, ancak kendi uygulamalarınızdan gelen olayları yakalamak için kendi Event Bus'ınızı oluşturabilirsiniz.

CloudWatch Logs, CloudTrail, EC2, VPC Flow logs vb. gibi çıktı olarak log sağlayan farklı AWS servislerinden gelen tüm loglarınızı, ayrıca kendi uygulamalarınızdan gelen logları barındırmak için merkezi bir konum sunar.

Log verileri Cloudwatch Logs'a beslendiğinde, log akışını gerçek zamanlı olarak izlemek ve uyarılmanız veya yanıt vermeniz gereken belirli girişleri ve eylemleri aramak için filtreler yapılandırmak üzere CloudWatch Log Insights'ı kullanabilirsiniz. Bu, CloudWatch Logs'un log verilerinin gerçek zamanlı izlenmesi için merkezi bir depo olarak hareket etmesine olanak tanır.

CloudWatch logs'un ek bir avantajı, Unified CloudWatch Agent'ın kurulumu ile gelir. Bu agent, EC2 instance'larından ve ayrıca Linux veya Windows işletim sistemi çalıştıran şirket içi servislerden logları ve ek metrik verilerini toplayabilir. Bu metrik verileri, CloudWatch'un sizin için otomatik olarak yapılandırdığı varsayılan EC2 metriklerine ek olarak gelir. Agent tarafından toplanan bu ek metriklerin listesi bu linkte bulunabilir.

Artık CloudWatch içinde 3 farklı insight türü vardır: Log Insights, Container Insights ve Lambda Insights.

Peki insights tam olarak nedir? Adından da anlaşılacağı gibi, CloudWatch'un topladığı verilerden daha fazla bilgi alma yeteneği sağlarlar. Öyleyse, her birinin gerçekleştirdiği rolü anlamak için bunlara daha yakından bakalım:

- Log Insights ile başlayalım. Log Insights, CloudWatch Logs tarafından yakalanan loglarınızı saniyeler içinde ölçekli olarak interaktif sorgular kullanarak analiz edebilen ve çubuk, çizgi, pasta veya yığılmış alan grafikleri olarak temsil edilebilen görselleştirmeler sunan bir özelliktir. Bu özelliğin çok yönlülüğü, AWS servislerinin veya uygulamalarınızın kullanabileceği herhangi bir log dosyası formatıyla çalışmanıza olanak tanır. Esnek bir yaklaşım kullanarak, belirli verileri almak için log verilerinizi filtrelemek üzere Log insights'ı kullanabilir, ilgilendiğiniz içgörüleri toplayabilirsiniz. Ayrıca özelliğin görsel yeteneklerini kullanarak, bunları görsel bir şekilde gösterebilir.

- Log insights gibi, Container Insights da AWS içindeki farklı container servislerinden ve uygulamalarından, örneğin Amazon Elastic Kubernetes Service (EKS) ve Elastic Container Service (ECS)'den farklı metrik verilerini toplama ve gruplandırma imkanı sunar. CloudWatch tarafından bu servisler için toplanan standart metriklere ek olarak, Container Insights aynı zamanda tanılama verilerini yakalama ve izleme olanağı sağlayarak, container mimarinizde ortaya çıkan sorunların nasıl çözüleceğine dair ek içgörüler sunar. Bu izleme ve insight verileri cluster, node, pod ve görev düzeyinde analiz edilebilir, bu da container uygulamalarınızı ve servislerinizi anlamanıza yardımcı olacak değerli bir araç haline getirir.

- Lambda Insights, AWS Lambda kullanan uygulamalarınız hakkında daha derin bir anlayış kazanma fırsatı sunar. Önceki 2 insight özelliğinde gördüğümüz prensiplerle çalışarak, serverless uygulamalarınızı izlemenize ve sorun gidermenize yardımcı olmak için AWS Lambda ile ilgili sistem ve tanılama metriklerini toplar ve birleştirir. Lambda Insights'ı etkinleştirmek için, fonksiyonunuzun Monitoring Tools bölümünde her Lambda fonksiyonu için özelliği etkinleştirmeniz gerekir. Bu, fonksiyonunuz için bir CloudWatch uzantısının etkinleştirilmesini sağlar ve bu da fonksiyon her çağrıldığında kaydedilen sistem düzeyindeki metriklerin toplanmasına olanak tanır.

### CloudWatch Kontrol Paneli (CloudWatch Dashboard)

CloudWatch dashboard'ları, her bir servisin detaylarına inmeden AWS içindeki verilerinizi görselleştirmenin harika bir yoludur. İş yükünüz ve süreçleriniz hakkında kararlar verme yeteneği sağlayarak, önemli bilgileri bir bakışta hızlıca görüntülemenize olanak tanır. Bu dashboard'lar, grafikleri oluşturmak ve istediğiniz konular hakkında hızlıca detaylı bilgi sağlamak için bir araya getirebileceğiniz ayrı widget'lardan oluşturulur. Hatta daha detaylı ve spesifik bilgileri görüntülemek için bu widget'lar içinde sorgular çalıştırmanıza bile izin verir.

CloudWatch ayrıca servisin kendisi tarafından sizin için oluşturulan otomatik dashboard'lara da sahiptir. Bu otomatik dashboard'lar servis bazında çalışır ve ilgilenebileceğiniz bazı temel bileşenleri seçer. Örneğin, halihazırda çalışan bir EC2 instance'ınız varsa, EC2 iş yüklerinizi izlemek için muhtemelen otomatik olarak oluşturulmuş bir dashboard vardır. Bu otomatik olarak oluşturulan dashboard'lara bir göz atmanızı öneririm çünkü bu servis ile nelerin mevcut olduğu ve kendiniz için mükemmel dashboard'u oluşturmak için yararlanabileceğiniz metrik türleri hakkında gerçekten iyi bir anlayış sağlarlar.

Bir dashboard oluşturmanın iki yolu vardır. Bunu ya editör aracılığıyla görsel olarak yapabilir ya da programlı olarak dashboard'lar oluşturabilir. Bununla beraber, metrikleri CloudFormation şablonlarının içinde bile kullanabilirsiniz. Her iki yöntem de widget adı verilen birçok farklı medya türü arasından seçim yapmanıza olanak tanır. Şu anda bu widget'ların 8 türü vardırü:

1.  **Çizgi grafikler (Line charts):** Çizgi grafik, bilgiyi düz çizgi segmentleriyle bağlanan bir dizi veri noktası olarak gösteren bir grafik türüdür. Birçok alanda yaygın olan temel bir grafik türüdür.
2.  **Yığılmış alan grafiği (Stacked area charts):** Bu grafik türü, aynı grafik içinde birçok farklı konunun toplamlarını karşılaştırır.
3.  **Sayı (Number) Widget'ı:** Özellikle ilgilendiğiniz belirli bir metrik için değeri anında görmenizi sağlar - bu, mevcut çevrimiçi instance sayısını göstermek kadar basit olabilir.
4.  **Çubuk Grafikler (Bar Chart):** Aynı grafik içinde birden fazla veri türünün değerlerini karşılaştırır.
5.  **Pasta grafikleri (Pie Chart):** Bir daire içine sığdırılmış diğer bilgilerle doğrudan ilişki içindeki oransal veriler.
6.  **Metin (Text) widget'ı:** Markdown formatlaması ile serbest metin, dashboard'larınıza uygun gördüğünüz şekilde faydalı bilgiler eklemenize olanak tanır.
7.  **Log tabloları (Log table):** Log insights'tan gelen sonuçları keşfeder. Logs Insights, Amazon CloudWatch'daki log verilerinizi interaktif olarak aramanıza ve analiz etmenize olanak tanır.
8.  **Alarm durumları (Alarm Status):** Bir şeylerin yanlış gittiğini hemen bu dashboard'da öğrenmek istediğiniz bir alarmınız varsa kullanışlıdır.

CloudWatch dashboard'larının en faydalı özelliklerinden biri, göstermek istediğiniz metrikler üzerinde matematiksel işlemler yapmanıza izin vermesidir. Yani, grafiği çizilen bir metriğin normalizasyon teknikleri veya filtreler uygulandığında nasıl göründüğünü görmek isterseniz, bunu yapma gücüne sahipsiniz.

Ek olarak, dashboard'larla çalışırken, bir auto scaling grubu gibi birden fazla kaynak genelinde veri toplama izniniz de vardır, yani tüm filonuz genelinde CPU yükünün zaman içinde nasıl işlendiğini görmekle ilgileniyorsanız, bunu gösteren bir dashboard oluşturabilirsiniz.

Kendi dashboard'larınızı nasıl oluşturacağınızı merak ediyor olabilirsiniz. Aslında, CloudWatch konsolundaki AWS tarafından sağlanan görsel dashboard oluşturma araçlarıyla dashboard oluşturmak oldukça kolaydır.

Editörde dashboard oluşturmak, boş bir tuval üzerine yeni widget'lar sürükleyip bırakmak ve eklemek kadar basittir. Editör, daha önce bahsedilen farklı medya widget türlerinden herhangi birini seçmenize ve istediğiniz yere yerleştirmenize olanak tanır. Parçalar yeniden düzenlenebilir ve istediğiniz kadar hassas kontrollerle yerleştirilebilir. Tüm widget'lar, belirli boyutlara konumlandırabileceğiniz esnek bir pencere görünümüne sahiptir.

Dashboard'lar aynı zamanda kod olarak da yazılabilir, böylece aynı bilgilere ve araçlara programatik erişim sağlar. Bu, yeni hesaplarda veya projelerde kolay dashboard oluşturma için bu kod parçacıklarını CloudFormation şablonlarının içine de koyabileceğiniz anlamına gelir. Ancak, bu kodlanmış dashboard'ları oluşturmak ilk başta göründüğü kadar kolay değildir. 

Dashboard kodunuz JSON formatında bir dize olarak yazılır ve 0 ile 100 arasında ayrı widget nesnesi içerebilir. Widget'larınızın x,y konumunu ve her öğenin genişliğini ve yüksekliğini özellikle belirtmeniz gerekir. Bu, ilk kez kurmak için biraz sıkıcı olabilir, ancak halihazırda işlevsel bir şablonunuz varsa, bunu oldukça kolayca değiştirebilirsiniz.

Grafiklerinizi oluştururken ve tamamladıktan sonra, grafiklerinize açıklamalar ekleme yeteneğine sahipsiniz. Bu, geçmişte belirli bir olayın ne zaman gerçekleştiğini göstermek için faydalıdır ve ekibinizin diğer üyelerine bilgilerinizdeki belirli yükseliş ve düşüşler hakkında fikir sağlayabilir. İyi kod yazmanın yorumlar gerektirmesi gibi, grafiklerinizin ve çizelgelerinizin de bu avantaja sahip olmasını sağlamak özellikle önemlidir.

Grafiklerinizde hem yatay hem de dikey açıklamalara sahip olabilirsiniz, ki her birinin kendi amacı vardır. Örneğin, yatay açıklamalar bir servisin CPU yükü için makul üst ve alt sınırları belirtebilirken, dikey açıklamalar geçmişte belirli bir olayın ne zaman gerçekleştiğini not etmek için harikadır.

Ayrıca kendi sistemleriniz içindeki veya hatta hesaplar arasındaki diğer dashboard'lara bağlantı verme yeteneğine sahipsiniz. Bu dashboard'ların aynı bölgede olması gerekmez. Bu, uygulamalarınızın durumuna görünürlük sahibi olması gereken operasyon ekiplerini, DevOps'u ve diğer servis sahiplerini merkezileştirmeye yardımcı olan çok güçlü bir araçtır.

Hesaplar arası ve bölgeler arası erişime izin vermek için, hesabınızın CloudWatch ayarlarında ve bağlanmak istediğiniz her bir hesapta bunu etkinleştirmeniz gerekir. Ardından hesaplarınızı birbirine bağlayarak CloudWatch verilerini paylaşabilirsiniz. Bu ayarlar ayrıca AWS SDK ve CLI içinde de etkinleştirilebilir.

Paylaşım yeteneği bu kadarla kısıtlı değildir:

1.  Tek bir dashboard'u paylaşın ve dashboard'u görüntüleyebilecek kişilerin belirli e-posta adreslerini ve şifrelerini belirleyin.
2.  Tek bir dashboard'u herkese açık olarak paylaşın, böylece bağlantıya sahip olan herkes dashboard'u görüntüleyebilir.
3.  Hesabınızdaki tüm CloudWatch dashboard'larını paylaşın ve dashboard erişimi için üçüncü taraf bir tek oturum açma (SSO) sağlayıcısı belirtin. Bu SSO sağlayıcısının listesinin üyesi olan tüm kullanıcılar hesaptaki dashboard'lara erişebilir. Bunu etkinleştirmek için **SSO** sağlayıcısını **Amazon Cognito** ile entegre edersiniz.

CloudWatch Dashboard'ları, her biri ücretsiz olarak 50 metriğe kadar içeren üç dashboard'a sahip olmanıza olanak tanır. Bu, sadece pratik yapan veya izlemek istediği birkaç uygulaması olan herkes için fazlasıyla yeterlidir. Ancak bundan daha fazlası için, oluşturmak istediğiniz her yeni dashboard için ayda 3$ ücretlendirilirsiniz.

Kurumsal bir şirket için bu çok fazla bir harcama değildir. Ancak yeni başlayan bir solo geliştirici veya küçük bir işletmeyseniz - gelişigüzel dashboard'lar oluşturursanız bu küçük 3 dolarlık ücretler birikebilir. Bu yüzden servisleriniz için dashboard'lar oluştururken kaynaklarınızı uygun şekilde kullandığınızdan emin olun.

Dashboard kullanımı için bazı tavsiyeler şu şekildedir:

1.  En önemli gösterge metrikleri için daha büyük grafikler kullanın. Bu oldukça açık bir şey gibi görünebilir, ancak insanların görsel varlıklar olduğunu akılda tutmak önemlidir. Bir şeye dikkat etmelerini istiyorsanız, onu büyük ve belirgin yapın.
2.  Dashboard'larınızı ve grafiğinizi kullanıcılarınızın ortalama minimum ekran çözünürlüğüne göre düzenleyin. Bu, tüm ilgili verilerin bir seferde ekranda olmasını sağlamaya yardımcı olabilir. Bu, kullanıcıların ekran dışında olabilecek önemli bilgileri kaçırmasını önler, ki bu zaman açısından kritik sorunlar veya olaylar durumunda felaket olabilir. Günümüzde çoğu ekran 1920 x 1080'i oldukça iyi idare edebiliyor, ancak destek personelinizin her şeye telefonlarından baktığını biliyorsanız, belki de dashboard'larınızı bunun yerine ona göre tasarlayabilirsiniz.
3.  Zaman tabanlı veriler için grafiklerinizde saat dilimlerini gösterin ve birden fazla operatörün aynı dashboard'u aynı anda kullanması bekleniyorsa saat dilimini UTC'de tutun. Bu, insanların bir bakışta bir olayın ne zaman gerçekleştiğini bilmelerine olanak tanır. Ayrıca bir acil durumda tüm kullanıcıların olayın gerçekleştiği zamanla ilgili aynı öncülde çalışması önemlidir, saat dilimleri arasındaki farkları hesaplamak, müşterilerinizin memnuniyeti ve işiniz söz konusu olduğunda sinir bozucu olabilir.
4.  Zaman aralığınızı ve veri noktası periyodunuzu en yaygın kullanım durumuna göre varsayılan olarak ayarlayın.
5.  Grafiklerinizde çok fazla veri noktası çiziminden kaçının. Çok fazla veri, dashboard yükleme süresini yavaşlatabilir ve anormalliklerin görünürlüğünü azaltabilir.
6.  Grafiklerinize servisleriniz için ilgili alarm eşiklerini ekleyin. Bu alarmlar, kullanıcılarınızın servislerinizden birinin SLA sürelerini aşmak üzere olduğunu veya korkunç bir şeyin olmak üzere olduğunu bir bakışta anlamalarına olanak tanır. Alarmlara sahip olmak harikadır, ancak bir şeylerin önceden yanlış olduğunu bildiğiniz için onları asla tetiklememek çok daha iyidir.
7.  Kullanıcılarınızın her metriğin ne anlama geldiğini bileceğini varsaymayın, metin widget'larını kullanarak dashboard'da etiketleme ve açıklamalara sahip olmak konusunda ısrarcı olun.

### Anormallik Tespiti (Anomaly Detection)

AWS, 2019 yılında Amazon CloudWatch'a CloudWatch Anomaly Detection adlı bir özellik ekledi. Bu özellik makine öğrenimi tarafından desteklenmektedir ve CloudWatch alarmlarının oluşturulmasını ve bakımını otomatikleştirerek onları iyileştirir. Hepsi değil ancak, çoğu manuel müdahaleyi ortadan kaldırmak, alarmları daha etkili ve verimli hale getirir.

Adı anomali tespiti olan bu özellik, aykırı değerleri tespit etmek için oluşturulduğunu ima etse de, aslında CloudWatch alarmlarının oluşturulması ve bakımı sürecini otomatikleştirmek için tasarlanmıştır. Metrik için normal ve anormal davranışın ne olduğunu belirlemek için makine öğrenimini kullanır. Anomali tespiti kullanılırken, Amazon CloudWatch seçilen metrikleri izleyecek ve problemli davranış tespit edildiğinde otomatik olarak **ALARM** durumuna geçecektir.

Makine öğreniminde yeniyseniz, bu oldukça kapsamlı bir konudur. Ancak, ana hatlarıyla açıklaması oldukça kolay olan bazı temel prensipleri bulunur. Süreci anlamanıza yardımcı olmak için bunları kısaca ele alalım. Makine öğrenimi çok fazla matematik özellikle lineer cebir, kullanmasına rağmen, bir model oluşturulduktan sonra yeniden kullanılabilir. Bu, matematikçi olmayan kişilerin onları neredeyse bir emtia gibi kullanabileceği anlamına gelir.

Makine öğrenimi algoritmalarını güçlü kılan şeylerden biri, eğitim amaçlı veri kullanmalarıdır. Makine öğrenimi, belirli bir veriyi aramak yerine, gizli kalabilecek kalıpları bulmak için kullanılabilir.

Makine öğrenimi olarak adlandırılması tesadüf değildir. Algoritma kullanıldıkça öğrenir ve zamanla daha rafine hale gelir yani gelişir. Temelde bakarsanız, daha akıllı hale geldiğini söyleyebilirsiniz. Bu algoritmanın nasıl çalıştığını herhangi bir matematik kullanmadan genel bir şekilde nasıl olduğuna bakalım.

"A" harfini düşünün. Bir görüntüde "a" harfini tespit etmek için bir algoritma oluşturacak olsaydık, önce harfin neye benzediğini matematiksel olarak tanımlamamız gerekirdi.  Bu yöntemdeki pürüz, algoritmanın sadece harfin tam versiyonunu bulacak olmasıdır. Daktilo ile yazılmış "a" harfi, elle yazdığımdan çok farklıdır. İhtiyaçlarıma bağlı olarak, bulmamız gerekeni oldukça hassas bir şekilde tanımlamam gerekecekti.

Makine öğreniminin buna çare olabilir. YZ, çeşitli şekil ve boyutlardaki "a" harfini tanımak için bir algoritma oluşturmak için kullanılabilir. Tek başına bu gerçekten iyi çalışacaktır. Ancak, işaret edilene kadar belli olmayan, insan önyargılarımıza dayalı bir dezavantaj da nulunur.

A harfi ters göründüğünde (∀) ne olur? Bu hala bir "A" mı yoksa artık bir "V" mi?  Demek istediğim şu ki, ne aradığımızı tanımlarken, insanlar genellikle yerleşik bir bakış açısı veya perspektifle başlar ve bundan geriye doğru çalışır. Yapay Zeka kullanırken bile, yazacağım programlar A harfinin belirli bir yönelimini varsayacaktır çünkü bulmayı beklediğim şey budur.

Bu nedenle, bir bilgisayar modeliyle insan perspektifini kullanmak, bazı geçerli eşleşmelerin kaçırılacağı anlamına gelir. Ancak vakine öğrenimi, veriler nasıl yönlendirilmiş olursa olsun veri içindeki kalıpları bulmak için tasarlanmıştır. Yani, makine öğrenimi, A harfini baş aşağı, açılı, büyük harf veya küçük harf olsa bile veri içinde gizlenirken bulacaktır. Mükemmel mi? Hayır. Hiç değil. Sadece eğitim verileri kadar iyidir.

Ayrıca, farklı makine öğrenimi algoritma türleri vardır. Finansal veriler içindeki trendleri analiz etmek için tasarlanmış olanlar, görüntü tanıma içeren görevler için uygun olmayacaktır. Ancak, her kullanım durumu için manuel olarak ayrı algoritmalar oluşturmaktan çok daha verimlidir. Yönetmek ve sürdürmek için daha az insan sermayesi gerektirir.

Bunu CloudWatch Anomali Tespitine geri getirirsek, servisin izlenen şey için zamanla neyin normal olduğunu öğreneceği anlamına gelir. Normal mutlak bir değer değildir. Birden çok faktöre bağlı bir aralıktır.

Yarın herkes parlak pembe ayakkabılar giymeye ve ayaklarında başka hiçbir şey giymemeye başlasaydı, bu normal davranış haline gelirdi. Günlük hayatta bile, birçok insan normalin ne olduğunu veya ne anlama geldiğini belirleme konusunda zorlanıyor.

Amazon CloudWatch Anomali Tespiti için normal, seçilen bir metrik için geçmiş değerleri analiz ederek; saatlik, günlük veya haftalık olarak tekrarlanan öngörülebilir kalıplar arayarak tanımlanır. Geçmiş verileri topladıktan sonra, servis en uygun modeli oluşturur. Bu model, geleceği tahmin etmeye yardımcı olmanın yanı sıra normal ve problemli davranış arasında ayrım yapabilmeyi iyileştirir.

Bunun çalışması için, izlenen metriğin tekrarlanabilir bir kalıba sahip olması gerekir. Bir metrik geniş dalgalanmalara sahipse veya sık sık boşta kalıp aktivite patlamaları yaşıyorsa, Anomali Tespiti pratik bir değere sahip olmayacaktır. Grafiğe döküldüğünde, model gri bir bant olarak gösterilir.

CloudWatch Alarm oluştururken ve Anomaly Detection kullanırken, bir "Anomaly detection threshold" girmeniz istenecektir. Bu, istenen standart sapmaya dayalı bir sayıdır. Bu eşik için daha büyük sayılar daha kalın bir banda, daha küçük sayılar ise daha ince bir banda sahip olacaktır.

Model istenildiği gibi ayarlanabilir ve ince ayar yapılabilir, aynı CloudWatch metriği için birden fazla model kullanılabilir. Alarmlar, metrik bandın dışına çıktığında, bant eşiğinden yüksek olduğunda veya bant eşiğinden düşük olduğunda tetiklenecek şekilde ayarlanabilir.

CloudWatch Anomaly Detection, 12.000'den fazla yerleşik modele sahiptir ve CloudWatch Alarmları oluştururken manuel yapılandırma ve deney yapma ihtiyacını neredeyse tamamen ortadan kaldırır. Belirgin bir trende veya pattern'e sahip olduğu sürece, herhangi bir standart veya özel CloudWatch metriği Anomaly Detection ile kullanılabilir.

Anomaly Detection algoritmaları, metriklerin mevsimsellik ve trend değişikliklerine uyum sağlayabilir. Mevsimsellik değişiklikleri saatlik, günlük veya haftalık olabilir. Model oluşturulduktan sonra, her beş dakikada bir yeni metrik verileriyle güncellenecektir.

Bir model oluşturmak biraz zaman alır ve model oluşturulurken alarm durumu **INSUFFICIENT DATA** olarak görünecektir.

Bir metrik için anomali tespitini etkinleştirdikten sonra, CloudWatch, minimal kullanıcı müdahalesiyle normal temel çizgileri belirlemek ve anomalileri ortaya çıkarmak için sürekli olarak analiz etmek üzere istatistiksel ve makine öğrenimi algoritmalarını uygular.

Algoritmalar bir anomali tespit modeli oluşturur. Bu model daha sonra o metrik için normal davranışı temsil eden beklenen değerler bandı oluşturur. Model tarafından üretilen beklenen değerler iki şekilde kullanılabilir. Alarm oluşturmak için veya bir grafik ile görselleştirmenin bir parçası olarak kullanılabilirler.

Unutmayın, bir anomali tespit alarmı bir metriğin beklenen değerine dayanır. Bu tür alarmların Alarm durumunu belirlemek için statik eşikleri yoktur. Bunun yerine, yaptıkları şey metriğin değerini anomali tespit modeline dayalı beklenen değerle karşılaştırmaktır.

Anomaly Detection bandı gri renkte gösterilir, metriğin kendisi mavidir ve metriğin gerçek değeri Anomaly Detection bandının dışına çıktığında kırmızı olarak gösterilir.

Anomaly Detection, AWS Management Console, AWS CLI veya AWS SDK kullanılarak etkinleştirilebilir. Anomali tespiti, AWS'den mevcut olan yerleşik metrikler ve ayrıca özel metrikler kullanılarak gerçekleştirilebilir.

Genel olarak, Anomaly Detection kullanmak, kendi başıma tahmin etmek ve hangi eşiklerin olması gerektiğini anlamaya çalışmaktan çok daha kolay. Bu, tüm izleme sorunlarınızı ortadan kaldıracak bir sihirli değnek değil. Bunun yerine, izleme yaparken hayatınızdan biraz sürtünmeyi azaltacak başka bir araçtır.

Özetlemek gerekirse, Anomaly Detection, önceki verilere dayanarak bir metriğin beklenen davranışını öğrenebilir ve modelleyebilir ve bu zamanla uyum sağlamaya devam eder. Model tarafından oluşturulan normal aralıklara dayalı olarak bir Anomaly Detection güven bandı oluşturacaktır. Bandın dışına düşen metrik değerleri **anormallikler** olarak kabul edilir.

Bu normal desene dayalı olarak alarmlar oluşturulabilir ve "**Bandın Dışında (Outside the band)**", "**Banttan Büyük (Greater than the band)**" veya "**Banttan Küçük (Lower than the band)**" olduklarında tetiklenebilir. Amazon CloudWatch Anomaly Detection, AWS Console kullanılarak yapılandırılabilir ve ayrıca AWS API desteğine sahiptir. Bu, AWS CLI, AWS SDK'ları ve AWS CloudFormation kullanılarak yapılandırılabileceği anlamına gelir.


### CloudWatch Abonelikleri (CloudWatch Subscriptions)

Bu başlık altında, Amazon CloudWatch'un güçlü bir özelliği olan CloudWatch Subscriptions'ı ele alalım. Bu özellik, CloudWatch loglarından gerçek zamanlı log olayları akışına erişmenizi sağlar ve bu logları özel işleme ve analiz için Kinesis streams, Firehouse veya Lambda gibi diğer servislere iletebilir.

Temel seviyede, Amazon CloudWatch Logs, çeşitli Amazon EC2 instance'larınızdan, CloudTrails'den ve diğer kaynaklardan gelen log dosyalarını izlemenize, depolamanıza ve erişmenize olanak tanır. Bu, tüm farklı sistem ve uygulamalarınızdan gelen logları tek bir yerde merkezileştirmenizi sağlar. Bu bile başlı başına çok kullanışlıdır çünkü buradan loglarımızı kolayca görüntüleyebilir, arayabilir veya belirli kriterlere göre filtreleyebiliriz. Bununla beraber, Amazon CloudWatch subscriptions ile tüm bunları gerçek zamanlı olarak yapmaya başlayabiliriz.

#### Amazon CloudWatch Subscription'a Giriş

Amazon CloudWatch Subscriptions'ı kullanmanın ilk adımı, diğer sistemler tarafından oluşturulan logları alacak olan alıcı kaynağı oluşturmaktır. Bu kaynak, örneğin, hızlı bir şekilde gelen yüksek hacimli veriyi işleyebilen bir Kinesis stream olabilir.

Doğru bilgileri alıcı kaynağa yönlendirmek için , bir subscription filter kurmamız gerekiyor. Subscription filter, loglarda arayacağımız, eşleşen olay verilerini bulacağımız ve bu bilgileri alıcı kaynağımıza ileteceğimiz bir desen tanımlamamıza yardımcı olur. Her subscription filter, kurmanız ve farkında olmanız gereken birkaç temel öğe içerir.

İlk öğe, **Log Group Name**'dir. Bu, subscription filter'ı ilişkilendirdiğiniz log grubudur. Bu log grubunda oluşturulan herhangi bir log olayı filtrelemeye tabi tutulacak ve herhangi bir eşleşen öğe bulunduğunda, hedef servise gönderilecektir.

İkinci öğe **Filter Pattern**'in kendisidir. Bu, CloudWatch logs'un log olaylarındaki herhangi bir veriyle nasıl etkileşime girmesi gerektiğinin ve bu veriyi nasıl filtrelemesi gerektiğinin bir açıklamasıdır. Buna, hedef kaynağa ulaşan verinin kısıtlanması da dahildir. 

Subscription filter'ın üçüncü öğesi **Destination ARN**'dir. Bu, eşleşen tüm verileri göndereceğimiz hedef kaynağın ARN'sidir. Yine bu muhtemelen bir kinesis stream veya belki de bir lambda function'dır.

Dördüncü öğe basit bir **Role ARN**'dir. Bu Role ARN, CloudWatch logs'a filtre verilerini hedef kaynağa göndermek için gereken izinleri verebilen roldür. AWS'de her şeyin birbirine erişmek için izinlere ihtiyacı vardır ve bu da sistemin bir parçasıdır.

Son öğe ise **dağıtım yöntemidir (distribution method)**. Bu öğe, hedef kaynak olarak kinesis kullandığınızda kullanılır. Varsayılan olarak, veriler log stream'e göre gruplandırılacaktır. Daha dengeli bir dağılım için log verilerini rastgele dağıtmasını da sağlayabilirsiniz.

Bir subscription filter aracılığıyla alıcı servise gönderilen loglar **Base64** ile kodlanır ve **gzip** formatıyla sıkıştırılır.

Hesaplar arası log verisi paylaşımı Amazon CloudWatch Subscription log verileri ayrıca hesaplar arasında paylaşılabilir. Farklı AWS hesaplarının sahipleriyle işbirliği yapabilmek, çoklu hesap yapıları ve organizasyonlar için hayati önem taşır. Daha önce bahsettiğimiziz önceki hedef örneğinin aksine, log verilerinizi Lambda, Kinesis Data Firehouse Streams veya Kinesis streams'e gönderebilirdiniz. Hesaplar arası paylaşım yalnızca Kinesis streams ile sınırlıdır, bu nedenle mimarilerinizi bu soruna göre tasarlarken bunu aklınızda bulundurun.

Hesaplar arasında log verisi paylaşmak için bir log verisi gönderici ve alıcı oluşturmanız gerekecektir: Log verisi gönderici, veri kaynağından veriyi alır ve verinin belirtilen hedefe gönderilmeye hazır olduğunda CloudWatch logs'a bildirir. Log verisi alıcı, bir hedef tanımlar ve CloudWatch logs'a alıcının log verisi almak istediğini bildirir.

Bu öğelerin her ikisi de bir CloudWatch Logs Destination oluşturmanızı gerektirir. Bir hedef, bazı tanıdık öğelerden oluşur:

1.  Hedef Adı
2.  Hedef ARN
3.  Rol ARN
4.  Erişim Politikası. Bunlar, daha önce bahsettiğimiz subscription filter'ın öğelerine çok benzerdir. Dikkat edilmesi gereken önemli bir husus, log grubu ve hedefin **aynı AWS bölgesinde olması** gerektiğidir. Ancak, hedefin işaret ettiği kinesis stream farklı bir bölgede olabilir.

Verileriniz beklediğiniz şekilde gelmeye başladığında, ister başka bir hesaptan ister doğrudan kaynaktan, bu bilgileri istediğiniz şekilde manipüle edebilir veya görselleştirebilirsiniz.

Birçok kişi, kullanımı oldukça basit olduğu ve dışarıda çok sayıda örnek olduğu için veri görselleştirmesini anlamak için Kibana Dashboard'larını kullanır. Kibana, hizmetlerini sağlamak için ElasticSearch üzerinde çalışır. CloudWatch Log Subscriptions, verileri ElasticSearch'e göndermeyi doğal olarak desteklemez, ancak daha önce de gösterdiğimiz gibi kinesis'e taşıyabiliriz. Kinesis'ten, CloudWatch Logs Subscription Consumer kullanarak verileri elastic search'e aktarmaya başlayabiliriz.


## CloudTrail

AWS CloudTrail, temel işlevi AWS hesabınız içinde yapılan AWS Application Programming Interface (API) isteklerinin yanı sıra API olmayan istekleri de kaydetmek ve izlemek olan bir servistir. Bu olaylar, bir kullanıcının SDK kullanarak başlattığı programatik istekler, AWS komut satırı arayüzü, AWS yönetim konsolundan veya başka bir AWS servisi tarafından yapılan bir istek olabilir. Örneğin, EC2 auto-scaling yeni bir EC2 instance oluşturmak için tetiklendiğinde, instance'ı başlatmak için servis tarafından bir API isteği yapılır. Bu API istekleri CloudTrail tarafından kaydedilir ve izlenir.

Olaylar CloudTrail'in önemli bir parçası olduğundan, Olayların ne olduğu konusuna daha yakından bakalım. CloudTrail'in kategorize ettiği ve izlediği 3 tür olay vardır, bunlar:

-   Yönetim Olayları (Management Events)
-   Veri Olayları (Data Events)
-   CloudTrail Insight Olayları (CloudTrail Insight Events)

CloudTrail ile çalışırken varsayılan olarak kaydedilen ana olay türü, hesabınız içinde çalışan AWS kaynaklarınıza karşı alınan yönetim operasyonları hakkında bilgi izleyen Yönetim olaylarıdır (kontrol düzlemi operasyonları olarak da bilinir). Örneğin, Amazon RDS `CreateDBInstance` API'sini veya AWS IAM `CreateUser` API'sini çalıştırırsanız, bunlar yönetim olayları olarak CloudTrail üzerinde kaydedilecektir.

Yakalanan olayların çoğu API çağrıları aracılığıyla yakalanmasına rağmen, API olmayan olaylar olarak sınıflandırılan bazı olaylar da yakalanır. Bunun bir örneği, bir KMS anahtarının otomatik anahtar rotasyonu gerçekleştirildiğinde, bir kullanıcının AWS yönetim konsoluna başarılı bir şekilde giriş yaptığında veya aslında başarısız olduğunda olacaktır.

Veri olayları (veri düzlemi operasyonları olarak da bilinir), bir kaynak üzerinde veya içinde gerçekleştirilen kaynak operasyonları hakkında bilgi gösteren olaylardır. Örneğin, `GetObject`, `DeleteObject` ve `PutObject` API operasyonları dahil olmak üzere Amazon S3 nesne düzeyindeki API aktivitesi veya `PutSnapshotBlock`, `GetSnapshotBlock` ve `ListChangedBlocks` API operasyonları gibi AWS EBS Snapshot operasyonları.

Insight olayları, hesabınız içindeki **olağandışı aktivite** tarafından tetiklenen olayları yakalamanıza olanak tanır. Bu olaylar, S3'te yönetim ve veri olaylarından farklı bir klasörde saklanır. Söz konusu olayın zamanı hakkında bilginin yanı sıra, ortamınızdaki potansiyel sorunları belirlemenize yardımcı olmak için hata kodları, ilişkili API'ler ve ek istatistikler içerir.

Tüm yeni AWS hesapları için varsayılan olarak, AWS CloudTrail olayları kaydetmek üzere etkinleştirilmiştir ve bunların tümü AWS yönetim konsolunun servis panosundaki olay geçmişi (Event History) kullanılarak görüntülenebilir. Olay geçmişi, CloudTrail tarafından kaydedilen son 90 günlük aktiviteyi hem aramak hem de indirmek için hızlı ve kolay bir yol sağlar. Ayrıca olay geçmişi içindeki olayları filtreleyebilir, böylece belirli olayları daha verimli bir şekilde hızlıca tanımlayabilirsiniz.

CloudTrail, Trail adı verilen bir bileşeni yapılandırarak, olay geçmişi görüntüleyicisinin yapabildiğinin ötesinde, bu yakalanan olayları depolama, gözden geçirme ve ardından analiz etme yeteneğine de sahipsiniz. Bu izler ayrıca yakalanan belirli olaylara yanıt vermenize olanak tanır, böylece olay odaklı bir mimari oluşturmanızı sağlar. Bu Trails tarafından yakalanan veriler Amazon S3 içinde kaydedilir ve depolanır, ancak aynı zamanda Amazon CloudWatch Logs'a da gönderilebilir. 

Izlerden (Trails) konusunu tamamlamadan önce, oluşturabileceğiniz 3 farklı Trail türüne bakalım:

-   Tüm Bölge İzi (All Region Trail)
-   Tek Bölge İzi (Single Region Trail)
-   AWS Organizasyon İzi (AWS Organization Trail)

All Region Trail, tam olarak adında belirtildiği gibi çalışan bir izdir. AWS hesabınızdaki tüm bölgelere uygulanacak bir izdir. AWS CloudTrail, çalıştığınız her bölgede yapılandırıldığı olayları kaydedecek ve ardından bu olayları log dosyaları aracılığıyla belirttiğiniz S3 bucket'ına teslim edecektir.

Tüm bölge izi, CloudTrail kullanırken AWS tarafından önerilen ve tavsiye edilen seçenektir, tüm hesabınız genelinde veri yakalamanızı ve kaydetmenizi sağlar, bu da sonuçta altyapınız genelinde olayları nasıl izleyebileceğinizi geliştirir. Genellikle, ortamınız hakkında daha fazla bilgi ve veri yakalandıkça, daha fazla görünürlük sayesinde altyapınızı optimize etmek ve güvence altına almak için daha fazla fırsatınız olur.

All Region Trail aksine, Single Region Trail yalnızca AWS komut satırı arayüzünü (CLI) kullanarak oluşturabilirsiniz. Beklendiği gibi, bu izler yalnızca tek bir bölgeden veri yakalar, bu da veri kaydetmek istediğiniz bölgeleri özelleştirmenize olanak tanır. Olay log dosyaları daha sonra tek bir depo görevi gören bir Amazon S3 bucket'ı gibi seçtiğiniz hedefe teslim edilir. Gerekirse, her tek bölge izinin logları ayrı bir bucket'a teslim etmesini sağlayabilirsiniz.

AWS iş yüklerinizde AWS Organizations kullanıyorsanız, bir AWS Organization Trail kullanabilirsiniz.  Bir Organization Trail oluşturulduğunda, AWS Organizasyonuna ait tüm hesaplardan gelen tüm olayları yakalayacaktır. İlginç bir şekilde, bu organizasyon izleri ya sadece tek bir bölgeden ya da tüm bölgelerden olayları yakalamak üzere yapılandırılabilir. Yani hepsini yakala yaklaşımı olarak, AWS CloudTrail'i AWS organizasyonunuzla yapılandırabilir ve tüm bölgelerde yapılandırabilirsiniz.

Trail oluşturmak için yönetim hesabı (management account) kullanılmalıdır ve bu trail daha sonra tüm üye hesaplarınızla ilişkilendirilecek ve onlara uygulanacaktır. Bir kontrol aracı olarak, üye hesaplar yalnızca trail ve oluşturduğu log dosyalarını görüntüleme erişimine sahip olacaktır, bu nedenle izin yapılandırmasını hiçbir şekilde değiştiremezler, bu sadece Yönetim hesabından yapılabilir.

AWS CloudTrail'in bir diğer ilginç bileşeni CloudTrail Lake adı verilen bir bileşendir. CloudTrail lakes, AWS hesabınızdaki kaynaklarınız tarafından oluşturulan farklı olayları toplu olarak depolamanıza, gözden geçirmenize ve ardından sorgulamanıza olanak tanır. Ardından bunları olay veri depoları olarak bilinen şeye biriktirir ve 7 yıla kadar saklar. Bu olay veri depoları, hangi olayların o deponun bir parçası olmasını istediğinizi filtrelemenize olanak tanır, yalnızca gelişmiş olay seçicileri kullanarak analiz etmek istediğiniz ilgili olayları sunar, olayları, güvenlik tehditlerini ve ihlallerini sorun gidermenize yardımcı olur. SQL sorgu dilini kullanarak daha sonra olay veri depolarından belirli verileri çıkarabilir, kullanım durumunuza bağlı olarak ilgili eylemleri gerçekleştirebilirsiniz.

CloudTrail olaylarını kullanarak AWS hesabınızdaki kendi kaynaklarınızdan olayları toplamanın yanı sıra, CloudTrail lakes aynı zamanda AWS Config yapılandırma öğelerinden ve kendi veri merkeziniz dahil olmak üzere dış kaynaklardan log olaylarını yakalama yeteneği de sağlar.

AWS ayrıca, CloudTrail kanallarının kullanımı yoluyla yeni bir CloudTrail Lake'e bu verileri sorunsuz bir şekilde yakalamanıza olanak tanıyan CrowdStrike, CyberArk, GitHub, Nordcloud ve diğerleri gibi sistemlerini AWS CloudTrail Lakes'e entegre eden bir dizi Ortakla çalışır. Bu kanallar, harici kaynağınızı ve hedef CloudTrail Lake'inizi birbirine bağlamanıza olanak tanır.

### CloudTrail Kullanmanın Faydaları (The Benefits of Using CloudTrail)

AWS CloudTrail'in All Region, Single Region veya AWS Organization trail kullanarak büyük miktarda veriyi yakalama kabiliyeti, işletmenize ne gibi faydalar sağlar ve neden gereklidir? Bu sorunun bazı basit ve açık nedenleri var ve muhtemelen bazılarını kendiniz de düşünmüşsünüzdür, ancak AWS CloudTrail'in organizasyonunuz için nasıl faydalı olduğunu görmek için birkaçını gözden geçirelim.

CloudTrail'i bir security tool olarak kullanmak, olmaması gereken olayları tespit etmek için harika bir yoldur. Başarısız bir sign-in girişimi veya belki de kısıtlı bir API'yi çalıştırmaya çalışan bir principal gibi belirli olayları arayabilmek, security ekiplerinizin bu olayların nasıl gerçekleştiğini, kimin başlattığını öğrenmesine ve tekrar oluşmamasını sağlayacak önleyici tedbirler almasına olanak tanır. Bu verilerin analizi, altyapınızdaki zayıf noktaları tespit etmeye de yardımcı olarak mühendislerinizin sınırları ve operasyonel best practice'leri güçlendirmesini ve pekiştirmesini sağlar.

CloudTrail Trail ve CloudTrail Lakes özelliklerini kullanarak, birden fazla region'daki activity kayıtlarını tek bir S3 bucket'ında konsolide edebilir, böylece verileri analiz etmek için uygun bir yol sağlayarak, sorguları kullanarak ölçekli bir şekilde pattern'ları belirlemenize ve activity'leri denetlemenize olanak tanırsınız. Birden fazla kaynaktan ve region'dan gelen verileri bir araya getirmek, ekiplerinizin API activity'sini single-pane-of-glass yaklaşımıyla görüntülemesine olanak tanıyarak veri setlerinin analizini basitleştirir.

CloudTrail, AWS cloud ortamınızda tam olarak ne olduğunu, ne zaman ve kimin tarafından gerçekleştiğini anlamanıza olanak tanıyarak görünürlük kazanmanız için harika bir yol sunar. Ortamınızda artırılmış görünürlüğe sahip olmak, saldırılara karşı farkındalığınızı artırma fırsatı sağlar ve olağandışı davranışlara karşı erken uyarı sağlayabilir. Aslında, CloudTrail'in CloudTrail Insights adında ilginç bir özelliği bulunmaktadır; bu özellik, bir Write API'nin eylemi temelinde hesabınız içinde tespit edilen olağandışı davranışları izlemenize ve tanımlamanıza yardımcı olur. Bu event'ler yalnızca normal operasyonlarınızın kapsamı dışına çıkan API çağrılarının davranış pattern'ına göre yakalanır. Tetiklenen insight'ın arkasındaki nedeni belirlemenize yardımcı olmak için ek metadata da yakalanır.

CloudTrail Insights varsayılan olarak etkinleştirilmemiştir, ancak etkinleştirildiğinde CloudTrail, Write management event'lerindeki düzensizlikleri gözden geçirir, izler ve bunlar hakkında bilgi verir. Yakalandığında, bu event'ler destination trail bucket'ınızda farklı bir klasörde saklanır, bu klasör /CloudTrail-Insight olarak adlandırılır. Bu insight'ları hızlı ve etkili bir şekilde gözden geçirmenize yardımcı olmak için, bunlar AWS management console'daki AWS CloudTrail dashboard'undan da görüntülenebilir.

AWS CloudTrail, API çağrılarını izlemek üzere tasarlandığından, birden fazla hesap ve birden fazla region genelinde AWS kaynaklarınıza karşı gerçekleştirilen action'ların ve configuration değişikliklerinin kaydedilmiş bir audit'ini tutmak için çok etkili bir yöntem olarak kullanılabilir. CloudTrail log'larından toplanan veriler, governance'ı sürdürmenize ve regulatory gereksinimleri karşılamanıza yardımcı olmak için kullanılabilir. Audit perspektifinden bakıldığında, kaydedilen her event şu bilgileri yakalar:

-   API'yi gerçekleştiren principal (ARN dahil)
-   Account ID
-   Username ve session bilgileri
-   Event'in zamanı
-   Kaynak
-   Eventname (API)
-   Region
-   Source IP adresi
-   Ve daha birçok bilgiyi, yakalar.

### CloudTrail İzinlerini Yönetme (Managing CloudTrail Permissions)

Bu başlıkta, hesabınızda trail'ler oluşturmanıza ve ayarlamanıza olanak tanıyan bazı permission'lara daha yakından bakacağız.

AWS'deki tüm resource'larda olduğu gibi, bir service veya feature'ı kullanabilmek için doğru permission'lara ve access'e sahip olmanız gerekir. CloudTrail ile trail'ler oluşturmak veya ihtiyacınız olduğu gibi configure etmek için gerekli permission'lara sahip değilseniz, AWS administrator'ınız veya security ekibinizle konuşmanız gerekecektir. Eğer administrator siz iseniz ve başkalarına access vermek istiyorsanız, gerekli access'i kapsayan kendi IAM policy'lerinizi oluşturabilir veya mevcut bir AWS Managed policy kullanabilirsiniz. Bu repo oluşturulduğu, AWS CloudTrail ile ilgili 3 AWS managed policy bulunmaktadır:

İlk role olan `CloudTrailServiceRolePolicy`, bir CloudTrail ServiceLinkedRole için bir permission policy'sidir ve service'in sizin adınıza belirli operation'ları gerçekleştirmesine izin verir. Bu bir service-linked role'e bağlı olduğundan, bu tür bir policy'yi herhangi bir user, group veya role'e attach etmek mümkün değildir, sadece CloudTrail'in kendisi tarafından inherit edilebilir.

Kullanılabilir bir sonraki policy, `AWSCloudTrail_FullAccess`'tir ve beklendiği gibi AWS CloudTrail'i configure etmenize olanak tanıyan tüm gerekli action'ları sağlar. `AWSCloudTrail_FullAccess` için gerçek policy'ye bakarsanız, bunun aynı zamanda Simple Notification Service (SNS), Amazon S3, Amazon CloudWatch Logs ve ayrıca AWS Key Management Service (KMS) gibi diğer service'ler için permission'ları da içerdiğini göreceksiniz.

Bunun nedeni, Trail'lerinizi configure ederken aşağıda listelenenleri yapmanız gerektiğidir:

- Log dosyalarını depolamak için S3'te bir storage location belirtmek (bucket ve folder name dahil).
- KMS kullanarak log'larınızın encryption'ını seçmek ve configure etmek.
- Log dosyası teslimi için SNS kullanarak notification parameter'larını configure etmek ve ayarlamak.
- Trail log'larınızı izlemek ve belirli bir activity gerçekleştiğinde sizi bilgilendirmek için CloudWatch Logs'u configure etme fırsatına sahipsiniz.

Sonuç olarak, diğer service'lerle entegre olan bu configuration task'larının bazılarını gerçekleştirmek için belirli access gereklidir.

Trail'inizi oluştururken CloudTrail'den log dosyalarınızı depolamak için S3 bucket'ı oluşturmasını isterseniz, o bucket'a otomatik olarak bir bucket policy eklenir ve CloudTrail'in event log'larını yayınlamasına izin verir. Bu policy aşağıdaki gibi görünür ve iki farklı statement ID ekler; ilki CloudTrail'in Bucket ACL'yi döndürmesine izin verir, ikincisi ise CloudTrail'in bucket'a log yazmasına izin verir. Bu statement ID'ler, CloudTrail'in yalnızca belirli Trail için bucket'a yazmasını sağlayan bir aws:SourceArn koşulu içerir, çünkü aws:SourceArn trail'in ARN'si olacaktır.

Benzer şekilde, CloudTrail'den log dosyası teslim bildirimleri için yeni bir SNS topic configure etmesini istiyorsanız, o zaman yine permission'lar otomatik olarak o Topic'e uygulanacaktır.

Diğer bir policy olan `AWSCloudTrail_ReadOnlyAccess` policy'sine bakalım ve bunların nasıl farklılaştığını ve aralarındaki kontrastı görelim. Beklendiği gibi, herhangi bir şekilde Trail oluşturmanıza veya değişiklik yapmanıza izin veren hiçbir permission yok. Aslında, bu senaryoda verilen tek permission'lar burada gösterilmiştir:

Peki bu 4 action'ın her biri size ne yapmanıza izin verir?

- `cloudtrail:Get` - Bu, belirli bir trail için setting bilgilerini döndürmenize izin verecektir.
- `cloudtrail:describe` - Bu permission, mevcut region'daki hesabınızda bulunan farklı trail'ler için ayarları almanıza olanak tanır.
- `loudtrail:List` - Bu, basitçe mevcut hesaptaki tüm trail'lerin bir listesini döndürecektir.
- `cloudtrail:LookupEvents` - Bu, mevcut region'da son 90 gün içindeki hem management hem de insight event'lerini aramanıza yardımcı olmak için kullanılabilir.

Use case'inize ve vermek istediğiniz permission'lara bağlı olarak, mevcut AWS Managed policy'lerini kullanabilir veya en azından bunların bir varyantını kullanıp ihtiyacınıza göre modify edebilirsiniz. Permission'ları atarken her zaman least privilege prensibini benimsemeniz gerektiğini unutmayın, bu nedenle AWS CloudTrail'e erişim gerektiren kullanıcıya bağlı olarak bunları uygun şekilde tahsis ettiğinizden emin olun.

### CloudTrail Olay Log Dosyaları (CloudTrail Event Log Files)

Bu başlık altında, bir CloudTrail trail oluşturmanın sonucu olarak oluşturulan event log'larını inceleyeceğiz.

Trail'ler, AWS hesaplarınız genelinde meydana gelen event'leri yakalamak, izlemek ve saklamak için temel unsurdur. Bu trail'ler olmadan, event'ler yalnızca AWS Management Console'daki CloudTrail dashboard'unun Event History'sinden görüntülenebilir ve yalnızca event kaydedildikten sonra en fazla 90 gün boyunca görüntülenebilir.

Bir event meydana geldiğinde, Trail tarafından yakalanır ve Amazon S3'te depolanabilen bir log dosyasına gönderilir. CloudTrail Lake'teki data store'ları kullanıyorsanız, bu event'ler data store'unuza gönderilir. **CloudTrail Lake**'ler, AWS hesabınızda resource'larınız tarafından oluşturulan farklı event'leri toplu olarak depolamanıza, gözden geçirmenize ve sorgulamanıza, ardından bunları event data store'ları olarak bilinen şeylerde biriktirmenize ve 7 yıla kadar saklamanıza olanak tanır. Bu log dosyalarında yakalanan bilgiler kapsamlı olabilir, bu nedenle kendi ortamınızda bunlardan elde edebileceğiniz potansiyel değeri en üst düzeye çıkarmanıza olanak tanıyacak verileri anlayabilmeniz için bu konuyu incelemek için biraz zaman harcamak istiyorum.

Peki, bir log dosyası nedir ve neye benzer? Log dosyaları, IAM'deki access policy'lere benzer şekilde JSON formatında (JavaScript Object Notation) yazılır ve aynı log içinde birçok farklı event'in kaydını içerir. Her event, hangi action'ın istendiğini, isteğin kaynağını, ne zaman olduğunu, yakalanan API veya event'i ve daha fazlasını belirlemenize olanak tanıyan bilgiler içerir.

![d4ZLemG.md.png](https://iili.io/d4ZLemG.md.png)

Yukarıdak, örneğe bakalım ve log dosyasının her öğesini, neyi ifade ettiğini belirlemek için parçalara ayıralım. En üstten başlayıp aşağıya doğru ilerleyerek bu bilginin bize tam olarak ne sunduğunu görelim.

- eventVersion, AWS tarafından yönetilen ve major ve minor revision'dan oluşan log event format'ının versiyonunu tanımlar. Bu içerik yazıldığı sıradaki mevcut versiyon 1.08'dir, 'major' revision '1' ile, minor versiyon ise '.08' ile temsil edilir.

- userIdentity, yakalanan event'in isteğini kimin gerçekleştirdiği hakkında bilgi bulmamızı sağlar. Bu örnekte, 730739171066 hesabından 'Cloudacademy' adlı bir IAMUser olduğunu görebiliyoruz.

- eventTime, event'in belirlenen region'da kaydedildiği timestamp'i kaydeder.

- eventSource, isteğin hangi service'e karşı yapıldığını gösterir, bu örnekte signin.amazonaws.com URL'si aracılığıyla AWS management console'a giriş ile ilgili olduğunu görebiliyoruz. Bu EC2'ye karşı bir istek olsaydı, ec2.amazonaws.com olarak görünürdü.

- Sırada eventName key'i bulunur, bu da eventSource'a karşı hangi API action'larının olduğunu gösterir. Yani örneğimizde, signin.amazonaws.com üzerinden AWS Management Console'a giriş yapmak için bir istek yapıldığını görebiliyoruz.

- awsRegion açıklamaya gerek yok diye düşünüyorum. Bu değer isteğin yapıldığı region'dır, örneğimizde istek US East (N. Virginia)'ya karşılık gelen us-east-1'de yapılmıştır.

- sourceIPAddress, CloudTrail'in isteği yapan kullanıcının IP Adresini yakalamasına olanak tanır.

Şu anda Event'in yaklaşık yarısındayız ve event log girişinin bu aşamasında, 16 Şubat 2023 tarihinde 14:41'de 86.141.209.159 kaynak IP adresinden US East (N. Virginia) region'daki 730739171066 hesabının AWS management console'una giriş yapmak için Cloudacademy adlı bir IAM user tarafından bir istek yapıldığını tespit ettik. Yani bu istek hakkında zaten çok fazla bilgi belirledik, ancak hala bu event hakkında yakalanan çok fazla veriye sahibiz, bu yüzden Event'teki bir sonraki girişe geçelim.

- userAgent, isteği yapmak için kullanılan agent hakkında bilgi sağlar. Örneğimizde, browser ve kaynak makine hakkında veri sağlıyor. İstek AWS içinden veya bir AWS resource tarafından yapıldıysa, aşağıda listelenenler gibi girişler de görebilirsiniz:

	-   signin.amazonaws.com – Bu, isteğin AWS Management Console ile bir IAM user tarafından yapıldığını gösterir.
	-   console.amazonaws.com – İsteğin AWS Management Console ile bir root user tarafından yapıldığını gösterir.
	-   lambda.amazonaws.com – Bu, isteğin hangi service tarafından yapıldığını gösterir, burada AWS Lambda var.
	-   aws-sdk-java – İstek, Java için AWS SDK ile yapılmıştır.
	-   aws-cli/1.3.23 Python/2.7.6 Linux/2.6.18-164.el5 – İstek, Linux'a kurulu AWS CLI ile yapılmıştır.

- Sırada requestParameters key'i bulunur. Bu değer istek ile gönderilen parametreleri tanımlamak için kullanılır. Bu örnekte, ek parametre yoktu, bu nedenle değer 'null' olarak gösterilir.

- responseElements, isteklerin action'larını gösterecektir, AWS Management Console'a bu login isteğinin yanıtı burada ConsoleLogin öğesi altında 'Success' olarak gösterilmiştir. Bu değerden, access'in başarıyla verildiğini ve CloudAcademy kullanıcısına access verildiğini belirleyebiliriz.

<div align="center">

![d4ZDn4V.md.png](https://iili.io/d4ZDn4V.md.png)

</div>

- Bunu takiben, additionalEventData parametresi bulunur. Bu parametre basitçe log'daki önceki değerler tarafından vurgulanmamış event hakkındaki ek bilgileri vurgular. Bu verilerden, IAM user'ın yönlendirildiği URL'yi, MobileVersion üzerinden erişilmediği gerçeğini ve IAM user Cloudacademy'nin Multi-Factor Authentication (MFA) kullandığını görebiliyoruz. Bu bilgi değerlidir çünkü isteğin veya yanıtının bir parçası olmayan diğer verileri tanımlamaya yardımcı olur.

- eventID, event'i tanımlamak için CloudTrail tarafından oluşturulan globally unique bir identifier'dır, sonuç olarak kaydedilen her event'in farklı bir eventID'si olacaktır.

- Bunu takiben, isteğin read only bir event olup olmadığını belirlemenize olanak tanıyacak readOnly parametresini görebilirsiniz. AWS management console'a bir kullanıcının giriş yapması örneğimizde, bunun false olarak kaydedildiğini görebiliyoruz, bu da operation'ın write-only olduğu anlamına gelir. Eğer operation read-only olsaydı, değer true olarak döndürülürdü.

- eventType değeri, isteğin hangi tür event olarak sınıflandırıldığını belirlemenize olanak tanır. Bu repo'nun yazıldığı sırada aşağıdaki event türleri bulunmaktadır:

	- AwsApiCall - İstekte bir API'nin çağrıldığını temsil etmek için kullanılır
	- AwsServiceClient - İsteğin bir AWS service tarafından yapıldığını gösterir
	- AwsConsoleAction - İsteğin AWS console'dan yapıldığını ve bir API çağrısı olmadığını gösterir
	- AwsConsoleSignIn - İsteğin AWS Management Console'a giriş yapmak için yapıldığını belirtir. Bu, burada çalıştığımız örneğimiz için geçerlidir
	- AwsCloudTrailInsight - Event'in CloudTrail Insights'ın bir parçası olarak kaydedildiğini temsil eder. CloudTrail Insights, bir Write API'nin eylemi temelinde hesabınız içinde tespit edilen olağandışı davranışları izlemenize ve tanımlamanıza yardımcı olur.

<div align="center">

![d4ZmgBj.md.png](https://iili.io/d4ZmgBj.md.png)

</div>

- Ardından, bunun bir management event olup olmadığını belirleyen managementEvent değeri bulunur. Bir management event, hesabınızda çalışan AWS resource'larınıza karşı alınan management operation'ları hakkında bilgi izler.

- recipientAccountId, event'in gerçekleştiği hesap numarasını gösterir.

- eventCategory, LookupEvents'te kullanıldığında bu event'in hangi kategori ile ilişkili olduğunu belirler. Look up'lar, son 90 gün içinde belirli bir region'dan management veya cloudtrail insight event'lerini aramanıza olanak tanır. Örneğimizde bu, management olarak kategorize edilmiştir. Ancak, diğer değerler Data ve Insight'ı içerir.

- Bu örnekteki son parametre tlsDetails'tir. Bu, Transport Layer Security'yi ifade eder ve gördüğünüz gibi, istekte kullanılan TLS versiyonunu, kullanılan cipher suite'i ve istekte kullanılan fully qualified domain name'i gösterir.

Temelde, bir Trail içindeki her event için yakalanan detay miktarı oldukça fazladır ve parametreler yakalanan event'e bağlı olarak biraz değişebilir.

Daha önce de belirttiğim gibi, tek bir event log'u içinde birçok farklı event olacaktır, bunun nedeni yaklaşık her beş dakikada bir yeni log'ların oluşturulmasıdır. Log dosyaları, son işleme tamamlanana kadar CloudTrail service tarafından tutulur. Ancak o zaman S3 bucket'a ve isteğe bağlı olarak AWS CloudWatch log'larına teslim edilecektir. Event'ler log'lara yazıldıktan sonra S3'e teslim edilip kaydedildiğinde, burada gösterildiği gibi standart bir adlandırma formatı verilir.

Bu format açıklayıcıdır, YYYY, MM, DD tarihi yıl, ay ve gündür, T ise HH, mm timestamp'ini gösterir ve 24 saat formatında temsil edilir. Bu tarih ve zaman damgasının sonundaki Z, zamanın UTC olduğunu gösterir.

Dosya yapılarına bakarken, log'larınızın depolandığı Amazon S3 bucket konumundan da bahsedelim. Log'ların hepsinin Trail'inizi oluştururken yapılandırdığınız S3 bucket'ınızda tek bir klasörde depolandığını düşünebilirsiniz. Ancak uzun ama çok kullanışlı bir klasör yapısı vardır:

İlk olarak, Trail'inizi oluştururken seçtiğiniz dedicated S3 bucket name'iniz bulunur. Ardından, yine Trail oluşturma sırasında yapılandırılan ve farklı Trail'lere karşılık gelebilen log'larınız için bir klasör yapısı oluşturmanıza yardımcı olan prefix gelir. Bunu takiben, AWSLogs adlı sabit bir klasör adı, ardından orijinal AWS account ID'si geliyor. Sonra, log'ları hangi service'in teslim ettiğini gösteren CloudTrail adlı başka bir sabit klasör adı var. Bundan sonra, log dosyasının kaynaklandığı region adı geliyor. Bu, birden çok region'a uygulanan Trail'leriniz olduğunda kullanışlıdır. Son üç klasör, log dosyasının teslim edildiği yıl, ay ve günü gösterir. Belirlediğiniz S3 bucket'ınızın altında birden fazla klasör olmasına rağmen, belirli bir log dosyası ararken kolay bir navigasyon yöntemi sağlıyor. Bu klasör yapısı, aynı S3 bucket'a log teslim eden birden fazla AWS hesabınız varsa daha da büyük bir kullanım alanı buluyor.

Bazı organizasyonlar birden fazla AWS hesabı kullanıyor olması muhtemeldir. CloudTrail log'larının birden fazla hesap arasında farklı S3 bucket'larda depolanması, belirli durumlarda uygunsuz olabilir ve yönetmek için ek yönetim gerektirir. Neyse ki CloudTrail, birden fazla hesaptan gelen CloudTrail log'larını bu hesaplardan birine ait tek bir S3 bucket'ında toplama yeteneği sunuyor. S3 bucket'ınızda bir account ID klasörü olmasının nedeni budur.

Tüm hesaplarınızdan gelen tüm log'larınızı sadece bir S3 bucket'a teslim etmek oldukça basit bir 3 adımlı süreçtir ve sonuç olarak esasen tüm CloudTrail log'larınızı yönetmenize olanak tanır. Bu çözümün nasıl yapılandırıldığına bakalım:

1.  İlk olarak, tüm log dosyalarının teslim edilmesini istediğiniz AWS hesabında bir Trail oluşturarak CloudTrail'i etkinleştirmeniz gerekir.
2.  Oluşturulduktan sonra, diğer hesapların CloudTrail log'larını yazmasına izin vermek için hedef bucket'ınızdaki bucket policy'sini güncellemeniz gerekir. Bu policy güncellemeleri hakkında daha fazla bilgi için lütfen buradaki örneğe bakın.
3.  Diğer AWS hesaplarınızda yeni bir Trail oluşturun ve log dosyaları için mevcut bir S3 bucket kullanmayı seçin. İstendiğinde birinci adımda kullanılan bucket adını ekleyin ve uyarıldığında farklı bir AWS hesabından bir bucket kullanmak istediğinizi kabul edin. Bucket seçimini yapılandırırken burada önemli bir nokta, birinci adımda bucket'ı yapılandırırken kullandığınız prefix'in aynısını kullandığınızdan emin olmaktır. Bu, CloudTrail'in kullanmak istediğiniz yeni bir prefix'in konumuna yazmasına izin vermek için bucket policy'sini düzenlemeyi amaçlamadığınız sürece geçerlidir. Trail'inizi yapılandırdığınızda, oluştur'a tıklayın ve yeni Trail'iniz artık log dosyalarını bu sürecin birinci adımında kullanılan AWS hesabınızdaki S3 bucket'a teslim edecektir.

### AWS CloudTrail Log Dosyalarını İzlemek için Amazon CloudWatch'tan Yararlanma (Leveraging Amazon CloudWatch to Monitor AWS CloudTrail Log Files)

Bu başlık altında, AWS CloudTrail'in Amazon CloudWatch ile nasıl etkileşime girdiğine bakacağız. Bu etkileşim, güvenlik risklerini ve anormallikleri belirlemenize yardımcı olan bir izleme çözümü oluşturmanın yanı sıra, CloudTrail log verilerinizden altyapınız genelinde gelişmiş bir içgörü düzeyi sağlamanıza olanak tanır. Trail event log'larınızın CloudWatch'a teslim edilmesini etkinleştirmek isteğe bağlı yapılandırılabilir bir öğedir, bu özelliği ya trail'lerin oluşturulması sırasında ya da mevcut bir trail'i düzenleyip detayları yapılandırarak etkinleştirmeniz gerekir.

CloudWatch'ta mevcut veya yeni bir log group seçebilirsiniz. Yeni bir log group seçmek isterseniz, düzenlenebilir varsayılan bir ad verecektir. Ayrıca CloudTrail'in CloudWatch'taki log group'a iletim yapmasına izin vermek için bir role seçmeniz gerekir. Yine, bu mevcut bir role olabilir veya CloudTrail'den sizin için bir tane oluşturmasını isteyebilirsiniz. 

CloudTrail event log'larını izlemek için CloudWatch kullanmanın birçok faydası vardır, çok sayıda güvenlik izleme kontrolünün kullanılmasını sağlar. Bunun harika bir örneği, VPC'nizdeki security group'larınızda veya network access control list'lerinizde herhangi bir değişiklik isteyen belirli API çağrıları olduğunda veya IAM'deki security policy'lerde değişiklikler olduğunda bildirim almaktır. Güvenlik önlemlerinizde olmaması gereken değişiklikler yapılıyorsa, yetkili kullanıcılar için erişim yanlışlıkla kaldırılabilir ve yetkisiz kullanıcılara erişim verilebilir, bu da operasyonel hizmetleriniz üzerinde büyük ve zararlı bir etkiye sahip olabilir. Bir policy'de yapılan küçük bir değişiklik bile, güvenilmeyen bir kullanıcının hatayı istismar etmesine yol açabilir.

CloudTrail yalnızca doğru yetkilendirmenin kimliği doğrulanmış kimlik tarafından karşılandığı başarılı API çağrılarını izlemekle kalmaz, aynı zamanda başarısız API isteklerini de izler. Bu başarısız girişimlere özel dikkat gösterilmelidir, çünkü bu erişim kazanmaya çalışan kötü niyetli bir kullanıcı olabilir. Ancak, bu aynı zamanda rolü için erişim sahibi olması gereken bir kaynağa erişmeye çalışan meşru bir kullanıcı da olabilir, ancak ilişkili IAM policy'lerinde yanlış permission'lar uygulanmış olabilir.

CloudTrail aracılığıyla bu kadar çok bilgi üretilirken, anlamlı içgörüler elde etmemize yardımcı olacak şekilde bu verileri analiz etmenin yollarını bulmamız gerekiyor. Event log'larımızı Amazon S3'e ek olarak Amazon CloudWatch'a göndererek, **metric filter**'lar, **CloudWatch Log Insights** ve **Contributor insights** gibi bir dizi özelliği kullanarak verileri önemli ve yararlı şekillerde yorumlamamıza yardımcı olabiliriz. Bunların her birine bakalım.

CloudTrail Log group'u ile ilişkili kendi özel metric filter'larınızı oluşturabilirsiniz. Bu, log dosyanızdaki event'lerinizde belirli bir değeri veya terimi aramanıza ve saymanıza olanak tanır, bu da  özelleştirilebilir eşiklerin uygulanmasına izin verir. Bu metric filter'ları oluştururken, CloudWatch'un dosyalarınızdan tam olarak neyi izlemesini ve çıkarmasını istediğinizi belirleyen bir filter pattern oluşturmanız gerekir. Bu filter pattern'leri tamamen özelleştirilebilir dizelerdir, ancak sonuç olarak çok spesifik bir pattern sözdizimi gereklidir. Bu nedenle, bunları ilk kez oluşturuyorsanız, doğru sözdizimini anlamanız gerekir.  Bu filter'lara karşı metric'ler ve eşikler belirlemek, belirli event aktivitesiyle ilgili bildirimlerin otomatik olarak gönderilmesini sağlar, örneğin başarısız Console Login girişimlerini izliyorsanız.

Contributor Insights, CloudTrail loglarınızdan çıkarılan time-series verilerini kullanarak en önemli katkıda bulunanlarınızı görselleştirmek için harika bir yoldur. Kurallar oluşturarak, toplamak istediğiniz içgörülerle en ilgili olaylara odaklanabilirsiniz. Contributor Insights'ı kullanmaya başlamak için, CloudTrail loglarınızdaki bilgileri tanımlamak için kullanılacak kurallar oluşturmanız gerekir. Kendi özel kurallarınızı oluşturabilir veya alternatif olarak yaygın gereksinimlere dayalı önceden tanımlanmış örnek kurallar kümesini kullanabilirsiniz. Burada, internet güvenliği merkezi tarafından tanımlanan en iyi uygulamalara odaklanan AWS CloudTrail merkezli önceden tanımlanmış örnek kuralların listesini görebilirsiniz.

Kural oluşturma süreci sırasında, kaynak IP adresi ve userIdentity gibi kurala uyan en önemli katkıda bulunanları tanımlamanızı sağlayan bir dizi anahtar da belirtmeniz gerekir.

Ayrıca, isteğe bağlı olarak, kurala uygulanacak ek filtreler belirleyebilirsiniz. Bu, loglardaki daha spesifik verilere odaklanmanızı sağlar. Örneğin, Center for Internet Security - VPC Changes adlı önceden tanımlanmış örnek kuralı seçtiğimizi varsayalım. Filtreler bölümü daha sonra burada gösterilen filtrelerle önceden doldurulacaktır.

Kural CloudTrail logundaki listelenen değerlerden herhangi biriyle başlayan herhangi bir Eventname'i arar. Bu, bu eventname değerlerinden herhangi birini içeren herhangi bir olayın kural içinde yakalandığını ve Contributor Insights tarafından vurgulandığını garanti eder, bu da sırayla bu kurala uyan en önemli katkıda bulunanları vurgular. Ayrıca, önceden tanımlanmış kural seçeneği tarafından eklenen önceden doldurulmuş filtrelerin dışında kendi ek filtrelerinizi de ekleyebilirsiniz.

Kurallarınızı yapılandırdıktan sonra, sonuçlar özelleştirilmiş bir time series grafiği üzerinde en önemli katkıda bulunanlarınızı gösterecek şekilde görselleştirilebilir.

CloudWatch Metrics ve Contributor Insights'tan bahsettik, şimdi CloudWatch Log Insights'a bir göz atalım. Tahmin edebileceğiniz gibi, bu, log içindeki algılanabilir alanlara dayalı özel oluşturulmuş sorgu dili kullanılarak CloudTrail Loglarınızdan faydalı bilgiler elde etmek için kullanılabilecek başka bir özelliktir. Bu, ilgilendiğiniz alanları kapsayan, analiz etmeniz gereken verileri filtrelemek için sorgular çalıştırmanızı sağlar.

Başlamak için, kendi sorgularınızı oluşturmak ve yazmak için beceri ve bilgiye sahip olmadan log verilerinizden bilgi toplamanıza olanak tanıyan önceden oluşturulmuş bir dizi örnek sorgu kullanabilirsiniz. Bu örnek sorgular çeşitli kullanım senaryolarını, hizmetleri ve önerileri kapsar.

CloudTrail Log grubumu ve ardından 'Number of log entries by service, event type, and region' örnek sorgusunu seçersek, bu aşağıdaki sorguyla temsil edilecektir.

```sql
stats count (*) by eventSource, eventName, awsRegion
```

Yakalanan verilere bağlı olarak, bu daha sonra çeşitli grafikler olarak görselleştirilebilir veya daha fazla analiz için CSV, JSON veya XLSX dosya formatında dışa aktarılabilir. Ayrıca, bu verinin time series'ini önceden yapılandırılmış aralıklar veya gerektiğinde özelleştirilmiş bir aralık üzerinden ayarlamak mümkündür.

Oluşturduğunuz herhangi bir sorgu kaydedilebilir ve daha sonraki bir tarihte kolayca çalıştırılabilir, bu özellikle belirli verileri tanımlamanıza yardımcı olan bir dizi karmaşık sorgu oluşturduysanız özellikle kullanışlıdır. Sorgularınızın sonuçları daha sonra 7 güne kadar görüntülenebilir.

Öğrendiğimiz gibi, Amazon CloudWatch, CloudTrail log dosyalarınızdan yakalanan verileri analiz etmenizi, tanımlamanızı, izlemenizi ve raporlamanızı sağlayan çeşitli özellikler sunar. Bu, loglarınızdan ek istihbarat ve içgörü toplama yeteneği sunar, böylece güvenlik risklerini proaktif olarak tespit etmenizi, performans sorunlarını belirlemenizi ve potansiyel sorunları çözmenizi sağlar.


## AWS CloudFormation

AWS Console'dan hiç yinelenen şeyler yaptığınızı fark ettiniz mi? Tek bir EC2 instance oluşturmak için AWS Console'da 20 ekran boyunca tıklayarak gününüzü geçirdiniz olmuştır. Sonra user data'yı yanlış girdiğiniz için hayal kırıklığına uğrayıp, düzeltmek için 20 ekran daha tıklamış olmanız da muhtemeldir. 

Bu durumlara çözüm olarak, AWS CloudFormation kullanmanızı önerebilirim. Çünkü kaynakları manuel olarak oluşturmak zaman alıcı ve genellikle hataya açıktır. Bu yüzden bu süreci mümkün olduğunca otomatikleştirmek istersiniz. "Eh, bunu yapmak için CLI'yi kullanabilirim" diye düşünüyor olabilirsiniz. CLI veya API çağrıları ile AWS kaynaklarının oluşturulmasını otomatikleştirebilseniz de, bu kaynakları güncellemek yine de çoğunlukla manuel olacaktır.

CloudFormation kullanarak, altyapınızın ve yapılandırmalarının oluşturulmasını, güncellenmesini ve silinmesini tek bir yerden otomatikleştirebilirsiniz. Yani, shell scriptleri yazıp AWS API çağrıları ile kendi mantığınızı oluşturmak yerine, CloudFormation kullanarak altyapınızı deklaratif olarak kod olarak yazabilirsiniz.

Eğer bu noktada CloudFormation kullanmaya ikna olduysanız, ilk olarak altyapınızı bir CloudFormation template'inde tanımlayarak başlayabilirsiniz. Template, JSON veya YAML formatında yazılır ve tüm AWS kaynaklarınızı ve yapılandırmalarını belgelemek için özel bir yapı kullanır.

Altyapınızı kod olarak tanımlamanın harika yanı, yazılım geliştirme süreciniz için kullandığınız aynı en iyi uygulamaları cloud altyapınızın geliştirilmesine ve dağıtımına uygulayabilmenizdir. Bu, template'lerinize yapılan değişiklikleri takip etmek için Git veya SVN gibi kod versiyonlama araçlarını kullanabileceğiniz anlamına gelir. Sanallaştırılmış testler kullanabilir ve sürekli izleme uygulayabilirsiniz. Hatta CloudFormation template'lerinizi bir CI/CD pipeline üzerinden dağıtabilirsiniz.

Bunun faydası, template'lerinizdeki hata sayısını azaltmanız ve altyapınızın birden fazla örneğini oluşturmak için template'leri kolayca yeniden dağıtabilmenizdir. Bu, dev, test, staging ve prod gibi birden fazla ortamınız olduğunda ve bu ortamların özdeş versiyonlarını hızlıca oluşturmanız gerektiğinde yararlıdır.

Template'inizi oluşturduktan sonra, cloudformation engine bir fonksiyon gibi davranacak, template'inizi input olarak alacak ve output olarak stack adı verilen şeyi oluşturacaktır. Bir stack, tek bir birim olarak yönetebileceğiniz AWS kaynaklarının bir koleksiyonudur.

Her CloudFormation stack'inin benzersiz bir adı ve bağlantılı bir template'i vardır. Yeni bir stack oluşturduğunuzda, altyapı dağıtımınızın canlı durumunu kontrol edebilir, yeni oluşturulan kaynaklarınızı görüntüleyebilir veya kaynaklarınız artık gerekli değilse stack'i silebilirsiniz.

CloudFormation'ın her stack kaynağının düzgün bir şekilde oluşturulup yapılandırıldığını kontrol ettiğini akılda tutmak önemlidir. Template'deki herhangi bir kaynak oluşturulamazsa, CloudFormation varsayılan olarak geri döner ve oluşturulan tüm kaynakları yok eder. Bu varsayılan davranış, stack'lerin "ya hep ya hiç" olmasını sağlar. Yani stack'ler ya tamamen oluşturulur ya da tamamen yok edilir, böylece CloudFormation'ın oluşturduğu herhangi bir başıboş kaynağı takip edip kendiniz silmek zorunda kalmazsınız.

CloudFormation çoğu AWS servisi için kullanılabilir olsa da, hepsini desteklemez. Ancak, Amazon desteklenen AWS kaynakları ve operasyonları listesini aylık olarak sürekli güncellemektedir. CloudFormation'ın desteklemediği ancak sizin kapsama ihtiyacı duyduğunuz bir servis varsa, adından da anlaşılacağı gibi CloudFormation servisine yapılacak yaklaşan eklemelere odaklanan halka açık bir yol haritası olan CloudFormation Public Coverage Roadmap github'ı kontrol edebilirsiniz.

Özetlemek gerekirse, altyapınızı YAML veya JSON kullanarak bir template'de kod olarak yazın, dosyaları console, API veya SDK'lar kullanarak CloudFormation'a yükleyin ve CloudFormation kaynaklarınızı oluşturacaktır. Artık console'da tıklayıp durmanıza gerek kalmayacaktır.

### CloudFormation Şablonunun Anatomisi (The Anatomy of a CloudFormation Template)

CloudFormation'ın temel taşı template dosyasıdır. İster JSON formatında ister YAML formatında bir template kullanıyor olun, template'in anatomisi aynıdır. Her template için birkaç ana bölüm vardır.

Format versiyonu ve tarihi, doküman açıklaması, parametreler, eşlemeler, koşullar, dönüşüm, kaynaklar ve çıktı bilgileri template'i oluşturan bölümlerdir. Gelin bu bölümleri, tek tek inceleyelim.

- AWS template format versiyonu, CloudFormation template'inin versiyonudur. En son template format versiyonu 2010-09-09'dur ve şu an için geçerli olan tek değerdir.

- Description, altyapınızı tanımlamak için yorum bırakmak üzere kullanabileceğiniz isteğe bağlı bir alandır. Bu, template'inizin dokümantasyonudur. Bu sayede, diğer katkıda bulunanlar daha fazla bakmak zorunda kalmadan template'de tam olarak ne olup bittiğini bileceklerdir.

- Resources, bir CloudFormation template'inin tek gerekli bölümüdür, yani en az bir altyapı kaynağı bildirmeniz gerekir. Her kaynak üç parçaya ayrılır: template içinde benzersiz olan bir logical ID, bir resource type ve o kaynak için properties.

	- Logical ID, kaynakları tanımlamak için kullanılır ve template'in diğer bölümlerinde referans gösterilebilir.

	- Resource type, oluşturduğunuz kaynak için CloudFormation tarafından önceden tanımlanmış bir koddur. Bunu CloudFormation dokümantasyonunda aramanız gerekecek. Örneğin, bir EC2 instance için kod "AWS::EC2::Instance" türündedir.

	- Resource properties, bir kaynak için belirtebileceğiniz ek seçeneklerdir. Örneğin, bir EC2 instance oluşturmak için Instance Type, ImageID, user data ve daha fazlası gibi özellikleri bildirebilirsiniz.

- Şimdi, oluşturduğunuz altyapıya bağlı olarak, değerleri hard coding yapmaktan kaçınmanız ve bunun yerine kaynakların belirli özelliklerini dinamik olarak seçmeniz gereken bir durumla karşılaşabilirsiniz. Örneğin, bir EC2 instance oluşturmak için bir template oluşturuyorsanız, hangi tür EC2 instance'ın kullanılması gerektiğini veya kaç instance oluşturulması gerektiğini seçmeniz gerekebilir. Bununla beraber, bu değerler template'i her çalıştırdığınızda değişebilir. Bunu template'inizin parameter bölümünü kullanarak yapabilirsiniz. Parametrelerle, stack oluşturma sırasında kaynaklara değerler iletebilirsiniz. Ayrıca, bir kullanıcı stack'i oluşturduğunda üzerine yazılabilen varsayılan değerlere sahip parametreler de ayarlayabilirsiniz. Bu, çalışma zamanında bir kullanıcının template'i kendi ortamına uyarlamak için instance type, instance key pair name ve daha fazlasını seçebileceği anlamına gelir. Ayrıca, pseudo parameters adı verilen ve bildirmeniz gerekmeyen önceden tanımlanmış parametreler de vardır. Örneğin, hesap kimliğinizi almak için önceden tanımlanmış AWS account ID pseudo parameter'ını veya kaynağın oluşturulduğu AWS bölgesini almak için AWS region pseudo parameter'ını kullanabilirsiniz. Bunları parameters bölümünde kendiniz bildirmeniz gerekmez, template'inizde ihtiyaç duyduğunuzda basitçe referans gösterebilirsiniz. Bir parametreyi, sahte parametreyi veya başka bir kaynağı referans gösterirken, bunların her birini mantıksal ID'lerini kullanarak referans gösterebilirsiniz. Örneğin, bir EC2 Instance kaynağı oluşturuyorsanız ve kullanıcının EC2 key pair adını girmesini sağlayan bir parametreniz varsa, EC2 instance kaynak bloğunuzda "!Ref" anahtar kelimesini kullanabilir ve key pair parametresinin mantıksal ID'sini belirtebilirsiniz. Bu !Ref anahtar kelimesi, intrinsic functions olarak adlandırılan yerleşik fonksiyonların bir parçası olan CloudFormation'a özgü bir anahtar kelimedir. Diğer yaygın intrinsic functions şunlardır:

	-   !GetAtt, şablonunuzdaki bir kaynak özniteliğinden bir değer döndürür
	-   !FindInMap, bir anahtarı kullanarak bir haritada bir değer bulur ve
	-   Join, iki değeri bir ayırıcı kullanarak birleştirir, bu durumda yeni satır ayırıcısı

	Şablonunuzu oluştururken, seçilen parametrelere göre belirli değerleri aramaya ihtiyacınız olabilir. Örneğin, bir EC2 instance türü için doğru AMI'yi nasıl seçeceğinizi düşünün. Aynı OS görüntüsünün genellikle AWS bölgesine bağlı olarak farklı ID'leri olduğundan, bunu hardcode etmek zor olabilir. Bu nedenle, instance'ımız için bir AMI ID'sini hardcode ederseniz, her seferinde aynı bölgede başlatıldığından emin olmanız gerekir.

- Bu sorunu mapping'leri kullanarak çözebilirsiniz. Mapping'ler, bölgeler arası kullanılabilirlik için çok faydalıdır, çünkü böylece bölgeye ve OS mimarisine göre AMI ID için bir arama tablosu oluşturabilir ve instance'ım için doğru AMI ID'sini hızlıca bulabiliriz.

- Conditions bölümü, tanımladığınız koşullara bağlı olarak hangi kaynakların oluşturulacağını kontrol ettiğiniz yerdir. Bunu bir if/else ifadesi olarak düşünebilirsiniz. Eğer koşulum doğruysa, kaynaklarımı oluştur. Yanlışsa, onları oluşturma veya farklı kaynaklar oluştur. Örneğin, stack'in üretim veya test ortamı için olup olmadığına bağlı olarak bir kaynağı koşullu olarak oluşturabilirsiniz. Bunu yapmak için, test ve prod izin verilen değerleriyle environment type adlı bir parametre oluşturabilirsiniz. Buradan, isProduction koşulu, kullanıcının environment type parametresi için girdiği string'in prod'a eşit olup olmadığını değerlendirecektir. Eğer prod'a eşitse, CloudFormation şablonu isProduction koşulunu belirten EC2 instance'ını oluşturacaktır.

- Ardından, şablonunuzda kullanabileceğiniz makroları belirlemenizi sağlayan transform bölümü vardır. Makrolar oldukça ileri düzey olabilir, bu yüzden bu giriş kursunda onlar hakkında fazla konuşmayacağım, ancak size şablonlarınızda daha fazla özel işlem imkanı sağlarlar.

- Son olarak, stack oluşturulduktan sonra kaynak bilgilerini döndürmek için template output bölümünü kullanabilirsiniz. Örneğin, bir EC2 instance oluşturan bir şablonunuz varsa, bu instance'ın Public IP adresini veya instance ID'sini geri döndürmek için outputs bölümünü kullanabilirsiniz. Bu değerleri daha sonra CloudFormation konsolunda görebilir veya isterseniz başka bir stack'e aktarabilirsiniz.


## Logging Faydaları (The Benefits of Logging)

Bu başlık altında, logging'in size ve altyapınıza getirebileceği farklı faydalardan bazılarına yakından bakacağız. Bazıları için, logging'i sonradan düşünülen bir şey olarak görür, çok geç olana kadar uygulanmayan bir şey yani. Bu genellikle, ortamınızın güvenliğinin sağlanmasında ve çözümün gecikmesine neden olan bir olay veya güvenlik ihlalinin meydana geldiği durumlarda söz konusudur. Bu durumların geriye dönük değerlendirmesinde, olayı hızlı ve verimli bir şekilde düzeltmek veya hatta ilk etapta olmasını önlemek için logging'in baştan çalışır ve uygulanmış olması harika bir fikir olurdu.

Peki logging nasıl yardımcı olabilir? Genellikle, loglar hizmetler ve uygulamalar tarafından oluşturulur ve büyük miktarda bilgi içerir. Bu bilgiler, gerektiğinde incelenmek ve analiz edilmek üzere kalıcı depolamada kaydedilir ve saklanır. Bazı loglar gerçek zamanlı olarak izlenebilir ve log'un veri içeriğine bağlı olarak otomatik yanıtların gerçekleştirilmesine olanak tanır. Denetim perspektifinden, bu loglar paha biçilmezdir. Genellikle tarih damgaları, IP adresi veya kullanıcı adları gibi kaynak bilgileri dahil olmak üzere çok sayıda metadata içerirler ve bu özellikle CloudTrail loglarına baktığınızda doğrudur. Bu loglar, izlenebilir ve denetlenebilir eylemlerin kanıtını gerektiren belirli uyumluluk sertifikalarını elde etmenize yardımcı olmak için kullanılabilir.

Bir olayı mümkün olan en kısa sürede çözebilmek, organizasyonunuz içinde çok önemlidir. İster öncelik bir, iki veya üç olsun, olay öncesinde ve hemen sonrasında neler olduğuna dair mümkün olduğunca fazla bilgi edinebilmek, çözüm sürenizi önemli ölçüde azaltabilir. Olay öncesinde, sırasında ve sonrasında ortamınızın durumunu belirlemek için logları kullanmak netlik sağlar ve olayın nerede meydana geldiğini tespit etmenizi sağlayarak, çabalarınızı belirli bir alana yönlendirmenize olanak tanır. Daha hızlı çözüm, organizasyonunuz için daha iyi bir müşteri deneyimi sağlar.

Loglarınızdaki verileri izleyerek, meydana gelir gelmez haberdar olmak istediğiniz potansiyel sorunları hızlı bir şekilde tespit edebilirsiniz. Bu log izlemeyi eşikler ve uyarılarla birleştirerek, potansiyel sorunlar, tehditler ve olaylar üretim sorunu haline gelmeden önce otomatik bildirimler alabilirsiniz. Uygulamalarınız, ağınız ve diğer cloud altyapınızda neler olup bittiğini loglarayarak, performans için bir baseline oluşturabilir ve neyin rutin olduğunu, neyin olmadığını belirleyebilirsiniz. Bu baseline'a sahip olarak, üçüncü taraf araçlar ve yönetim hizmetleri aracılığıyla tehditleri ve anormallikleri daha kolay tespit edebilirsiniz.

Altyapınızda neler olup bittiğine dair kapsamlı bir anlayışa sahip olmak, operasyonel ekiplerinize büyük fayda sağlar. Altyapınızın nasıl performans gösterdiğine ve iletişim kurduğuna dair içeriden bir bakış, daha önce bahsettiğim faydaları elde etmeye yardımcı olur ve ortamınızın nasıl çalıştığı hakkında daha fazla veriye sahip olmak, özellikle olaylar ve güvenlik ihlalleri durumunda işiniz için gerçekten önemli olduğunda, yeterli bilgiye sahip olmamanın dezavantajından çok daha ağır basar.


### CloudWatch Logging Agent

Bu başlık altında CloudWatch Logların ne olduğunu, nasıl çalıştığını ve nasıl yapılandırıldığını yakından bakacağız. Bildiğimiz gibi CloudWatch, AWS hesabınızda çalışan kaynakların metriklerini toplamak ve bir araya getirmek için kullanılan bir izleme servisidir. Bu sayede kaynaklarınızın performansını izleyebilir ve belirlenen eşikleri karşılayan uyarılara yanıt verebilirsiniz. Buna ek olarak, Amazon CloudWatch, uygulamalarınızın ve çeşitli AWS servislerinin loglarını toplamanıza olanak tanıyan güçlü bir araçtır.

Veriler CloudWatch Logs'a beslendiğinde, log akışını gerçek zamanlı olarak izleyebilir ve belirli olayları aramak için metrik filtreleri oluşturabilirsiniz. Bu sayede uyarı almanız veya yanıt vermeniz gereken olayları tespit edebilirsiniz. Bu özellik, CloudWatch Logs'un log verilerinin gerçek zamanlı izlenmesi için merkezi bir depo olarak hareket etmesini sağlar. Bu özelliğin farklı bileşenlerini açıklayarak nasıl bir araya geldiğini daha iyi anlamanızı sağlayacağım. Unified CloudWatch Agent ile başlayalım.

Unified CloudWatch Agent'ın kurulumu ile EC2 örneklerinden ve Linux veya Windows işletim sistemi çalıştıran şirket içi hizmetlerden logları ve ek metrik verilerini toplayabilirsiniz. Ayrıca, bu metrik verileri CloudWatch'un sizin için otomatik olarak yapılandırdığı default metriklerine ek olarak toplanır. Agent, çeşitli işletim sistemlerine kurulabilir. 

Agent'ı kurmak ve yapılandırmak için birkaç farklı adım gereklidir. EC2 örneklerinize agent'ı kurmak için şunları yapmanız gerekir: 
- İlk olarak, CloudWatch'un örneklerden veri toplamasına ve AWS Systems Manager (SSM) ile etkileşime girmesine izin veren yetkilerle bir rol oluşturup bunu örneğe eklemeniz gerekir. 
- Ardından agent'ı EC2 örneğine indirip kurmanız gerekir. 
- Son olarak, CloudWatch Agent'ı yapılandırıp başlatmanız gerekir. Bu kurulum ve yapılandırmayı tamamlamanın en verimli yolu, SSM olarak bilinen EC2 Systems Manager servisini kullanmaktır. İki rol oluşturmanız gerekecek. Bir rol agent'ı kurmak ve toplanan ek metrikleri CloudWatch'a göndermek için kullanılacak; diğer rol ise SSM içindeki Parameter Store ile iletişim kurmak için kullanılacak. Bu, agent'ın yapılandırma dosyasını saklamak ve ardından diğer EC2 örnekleriyle paylaşmak için kullanılır.

Güvenlik açısından, yalnızca bir EC2 örneğinizin Parameter Store'a yazma iznine sahip olması en iyisidir. Agent yapılandırma dosyası SSM'de saklandıktan sonra, diğer EC2 örneklerinin aynı şeyi yapmasına gerek yoktur. Aslında, yazma işlemi tamamlandıktan sonra bu rol EC2 örneğinden ayrılmalı ve sadece agent'ın CloudWatch'a veri göndermesine izin veren diğer rol uygulanmalıdır.

SSM için ek izinlere sahip olan rol, bir rol oluştururken şu şekilde yapılandırılmalıdır. 
- 'Select the type of trusted identity' seçeneği 'AWS service' olmalıdır. 
- 'Chose the service that will use this role' seçeneği 'EC2 Allows EC2 instances to call AWS services on your behalf' olmalıdır.
- 'Attach Permissions Policies' kısmında 'CloudWatch Agent Admin Policy' ve 'Amazon EC2 role for SSM' seçilmelidir.

Sadece agent'ı kurmak ve CloudWatch'a veri göndermek için kullanılan rolün yapılandırması şu şekilde olmalıdır: 
- 'Select the type of trusted identity' seçeneği 'AWS service' olmalıdır. 
- 'Chose the service that will use this role' seçeneği 'EC2 Allows EC2 instances to call AWS services on your behalf' olmalıdır.
-  Son olarak, 'Attach Permissions Policies' kısmında 'CloudWatch Agent Server Policy' ve 'Amazon EC2 Role for SSM' seçilmelidir.

Rollerinizi oluşturduktan sonra, 'CloudWatch Agent Admin Role' yapılandırma dosyasını Parameter Store'da saklayacak olan EC2 örneğinizle ilişkilendirebilirsiniz.

SSM'nin Parameter Store'unda yapılandırma dosyasının kaydedileceği ek izinlere sahip EC2 örneğinden, agent'ı kurmanız gerekir. Bu işlem Systems Manager kullanılarak yapılabilir veya Linux veya Windows için bir S3 genel bağlantısından indirilebilir. CloudWatch Agent kurulumunun ön koşulu olarak, EC2 örneğinizin SSM ve CloudWatch uç noktalarıyla iletişim kurmak için internet erişimine sahip olduğunu doğrulamanız gerekir. Buna ek olarak, SSM agent'ının da kurulu olması gerekir. Bazı AMI'lar için agent zaten kurulu olabilir.

### VPC Akış Log Kayıtları (VPC Flow Logs)

VPC'niz içinde, potansiyel olarak yüzlerce hatta binlerce kaynak bulunabilir. Bu kaynaklar hem public hem de private alt ağlar arasında ve ayrıca VPC peering bağlantıları aracılığıyla farklı VPC'ler arasında iletişim kurar. VPC Flow Logs, VPC'nizdeki kaynaklarınızın ağ arayüzleri arasında akan IP trafiği bilgilerini yakalamanıza olanak tanır. Bu veriler birçok nedenden dolayı faydalıdır. Özellikle ağ iletişimi ve trafik akışıyla ilgili sorunları çözmenize yardımcı olmanın yanı sıra, yasaklanması gereken bir hedefe ulaşan trafiği tespit etmek gibi güvenlik amaçları için de kullanılır.

S3 erişim logları ve CloudFront erişim loglarından farklı olarak, VPC Flow Logs tarafından oluşturulan log verileri S3'te depolanmaz. Bunun yerine, yakalanan log verileri CloudWatch Logs'a gönderilir. VPC Flow Loglarınızı oluşturmadan önce, bunları uygulamanızı veya yapılandırmanızı engelleyebilecek bazı sınırlamaların farkında olmalısınız. VPC peering bağlantısı çalıştırıyorsanız, yalnızca aynı hesap içindeki eşleştirilmiş VPC'lerin flow loglarını görebilirsiniz, ya da hala EC2-Classic ortamında kaynaklar çalıştırıyorsanız, ne yazık ki bunların arayüzlerinden bilgi alamazsınız. Ayrıca bir VPC Flow Log oluşturulduktan sonra değiştirilemez. VPC Flow Log yapılandırmasını değiştirmek için, onu silmeniz ve yeni bir tane oluşturmanız gerekir.

Buna ek olarak, aşağıda listelenen trafik loglar tarafından izlenmez ve yakalanmaz: 
- VPC içindeki DHCP trafiği,
-  Amazon DNS Sunucusuna yönelik örneklerden gelen trafik

Ancak, kendi ortamınızda kendi DNS Sunucunuzu kullanmaya ve uygulamaya karar verirseniz, buna yönelik trafik VPC Flow Log'da kaydedilecektir. Diğer tüm gelen ve giden trafik ağ IP düzeyinde yakalanabilir.

Üç ayrı kaynağa karşı bir flow log kurabilir ve oluşturabilirsiniz. Bunlar, instance'lerinizden birinin ağ arayüzü, VPC'nizdeki bir alt ağ ve VPC'nin kendisidir. Açıkça görüldüğü gibi, VPC'nizdeki bir alt ağ  ve VPC'nin kendisi numaralı seçenekler için bu, bir dizi farklı kaynağı içerecektir. Sonuç olarak, veriler sırasıyla alt ağ veya VPC içindeki tüm ağ arayüzleri için yakalanır. Daha önce de belirttiğimiz gibi, bu veriler daha sonra bir CloudWatch log grubu aracılığıyla CloudWatch Logs'a gönderilir. CloudWatch log grubuna veri yayınlayan her ağ arayüzü farklı bir log akışı kullanacaktır. Bu akışların her birinde, log girişlerinin içeriğini gösteren flow log olay verileri olacaktır. Bu logların her biri yaklaşık 10 ila 15 dakikalık bir pencere sırasında veri yakalar.

Flow log verilerinizin bir CloudWatch log grubuna gönderilmesini sağlamak için, bu işlemi yapma izinlerine sahip bir IAM rolü gereklidir. Bu rol, VPC Flow Log'un kurulum yapılandırması sırasında seçilir. Rolünüz gerekli izinlere sahip değilse, log verileriniz CloudWatch grubuna teslim edilmeyecektir.

İzinler konusundayken, birinin VPC Flow Loglarını incelemesi ve erişmesi veya aslında ilk etapta bir tane oluşturabilmesi için gerekli izinlere bakalım. Bu üç EC2 izni, flow logları oluşturmanıza, silmenize ve açıklamanıza olanak tanır. Bu izinler; `ec2:CreateFlowLogs`, `ec2:DeleteFlowLogs` ve `ec2:DescribeFlowLogs`'tur. `logs:GetLogData` izni, bir veri akışından log olaylarını listelemenizi sağlamak için kullanılır. Flow logları oluşturmak istiyorsanız, ayrıca `iam:passrole` IAM izninin de tanımlanması gerekir. Bu izin, hizmetin daha önce bahsedilen rolü üstlenerek sizin adınıza bu flow loglarını oluşturmasına olanak tanır.

## AWS ile Program Tabanlı Çalışma (Operating Programmatically with AWS)

Bu başlık altında, AWS ile programlı olarak provizyon yaparken ve çalışırken sahip olduğumuz çeşitli seçenekleri inceleyeceğiz, bunlar arasında:

-   Application Programming Interfaces, yani API'ler;
-   Software Development Kits, yani SDK'lar;
-   AWS Command Line Interface, yani CLI; ve
-   Infrastructure as Code, yani IaC bulunmaktadır.

Bu seçeneklerin her biri farklı kullanım durumlarına sahiptir. Muhtemelen tarayıcı tabanlı AWS console'u ve bunun kaynakları sağlamak ve yönetmek için nasıl kullanılabileceği konusunda zaten aşinasınızdır. Console, tek bir EC2 instance başlatmak veya bir S3 bucket oluşturmak gibi basit, tek seferlik görevleri gerçekleştirmek için kullanışlı olabilse de, tekrarlayan görevleri otomatikleştirmesi gereken veya yirmi EC2 instance başlatmak, S3'te elli bucket oluşturmak gibi ölçekli işlemler gerçekleştirmesi gereken geliştiriciler ve sistem yöneticileri için daha sağlam seçenekler mevcuttur. Bu durumlarda, AWS ile programlı olarak kod ve script'ler kullanarak etkileşim kurmak, console üzerinden tıklayarak işlem yapmaktan daha mantıklıdır. Console'da bu tür işlemleri tamamlamak, uzun bir dizi manuel, hataya açık adımın sırayla, tek tek tamamlanmasını gerektirecektir ve toplu olarak tek seferde yapılamaz. O halde, AWS ile programlı olarak çalışma seçeneklerimizi incelemeye API'ler ve SDK'lar ile başlayalım.

**API**'ler, farklı yazılım sistemlerinin ve uygulamaların yayınlanmış, üzerinde anlaşmaya varılmış arayüzler aracılığıyla birbirleriyle iletişim kurmasını sağlar. AWS console'unda gerçekleştirdiğiniz çoğu eylem, aslında arka planda bir veya daha fazla AWS API'sini çağırır. Yani, AWS console'unda bir S3 bucket oluşturmak için bir düğmeye tıkladığınızda, aslında `CreateBucket` adlı bir S3 API eylemi çağrılır. Ve console'a zaten doğru şekilde kimliği doğrulanmış ve yetkilendirilmiş bir kullanıcı olarak giriş yaptığınız için, `CreateBucket` eylemine yapılan bu API çağrısı başarılı olacaktır. Ancak API'lerin doğası nedeniyle, geliştiricilerin özel bir uygulama içinden aynı `CreateBucket API` eylemini doğrudan çağıran kendi kodlarını yazmaları ve aynı sonucu elde etmeleri de mümkündür.

Ancak, geliştiricilerin AWS API'lerini çağıran özel uygulamalar yazdıklarında, yetkilendirme gerektiren her API isteğini bir AWS access key ID ve secret access key ile düzgün bir şekilde imzalamaktan sorumlu olduklarını unutmayın. Bu süreç, geliştiricilerin her API isteğinin bir parçası olarak özel bir HTTP yetkilendirme başlığı veya benzersiz bir imza sorgu dizesi değeri eklemelerini gerektirir. Bu, AWS'nin istekte bulunanın kimliğini doğrulamasına ve istenen işlemi gerçekleştirme yetkisine sahip olduğundan emin olmasına olanak tanır. API istekleri ayrıca, aktarım sırasında gönderilen istek verilerinin bütünlüğünü sağlamak ve herhangi bir yeniden oynatma saldırısına karşı koruma sağlamak için özel hash değerleri veya zaman damgaları içermelidir. Tüm API isteklerinin, ister console'dan, ister özel koddan, isterse birazdan değineceğimiz AWS SDK'larından herhangi birinden gelsin, AWS CloudTrail'de loglandığını belirtmekte fayda var.

AWS API çağrılarını özel koddan doğrudan yürütürken bahsettiğimiz ek karmaşıklık nedeniyle, AWS ayrıca **Software Development Kits** veya **SDK**'lar olarak bilinen AWS hizmetleri için bir dizi dile özgü API geliştirmiştir. Bu SDK'lar birçok yaygın geliştirme dili ve platformuyla entegre olur ve düzgün şekilde imzalanmış geçerli API isteklerini oluşturma sürecini otomatik olarak yönetir. AWS SDK'ları şu programlama dilleri için mevcuttur:

- Python,
- Java,
- PHP,
- .NET vb.

AWS ayrıca JavaScript ve React ve Angular gibi client-side geliştirme çerçeveleri için front-end web SDK'ları sunar. Ek olarak, iOS ve Android için mobil SDK'lar ve Python'dan Embedded C'ye kadar her şey için çok sayıda nesnelerin interneti veya IoT cihaz SDK'sı bulunmaktadır. Kurulduktan sonra, bu SDK'lar geliştiricilerin dile ve platforma özgü araçları ve tanıdık sözdizimini kullanarak öğrenme eğrilerini kısaltmalarına ve özel uygulamalarından AWS hizmetleriyle sorunsuz bir şekilde etkileşime girmelerine olanak tanır.

API'ler ve SDK'lar gibi araçlar yazılım geliştiriciler için kullanışlıdır, peki ya komut satırı araçları ve scriptleri kullanarak AWS ile etkileşimlerini otomatikleştirmek veya kolaylaştırmak isteyen sistem yöneticileri veya başkaları için? Bu kullanıcılar için AWS CLI daha uygun bir çözüm olabilir. AWS CLI'yi kurduktan ve yapılandırdıktan sonra, doğrudan komut satırından AWS hizmetlerine çağrı yapmaya başlayabilirsiniz. İlk yapılandırma sürecinin bir parçası olarak CLI ile kullanılacak bir kimlik bilgisi seti belirlediğiniz için, CLI'dan yapılan tüm çağrılar otomatik olarak bu kimlik bilgileri kullanılarak kimlik doğrulamasından geçirilecektir. Bununla beraber, çağrının başarılı olması için kullanıcının ayrıca izinlere sahip olması ve o eylemi gerçekleştirme yetkisine sahip olması gerektiğini unutmayın. Şimdi, kapsayıcı AWS CLI'ye ek olarak, AWS ayrıca Windows kullanıcıları için PowerShell araçları ile birlikte şu gibi belirli hizmetler için özel CLI'ler dahil olmak üzere ek özel komut satırı araçları sunar. Bunlar;

- Elastic Beanstalk,
- Elastic Container Service veya ECS,
- AWS Serverless Application Model veya SAM'dir.

Tüm bu komut satırı araçları, otomasyonu etkinleştirmek için scriptlerin kullanımını destekler ve bir DevOps ortamında sürekli entegrasyon ve sürekli dağıtım yani CI/CD pipeline'ının bir parçası olarak kullanılabilir. Bu scriptler, zaman içinde değişiklikleri merkezi olarak izlemek ve yönetmek için git gibi bir repository kullanılarak sürüm kontrolüne tabi tutulabilir. Ancak belki de daha önemlisi, yaygın görevleri otomatikleştirmek için script kullanmak, insan hatası olasılığını azaltır ve işletmeniz genelinde daha sağlam, güvenilir ve tekrarlanabilir süreçleri destekler. API'ler ve SDK'larla ilgili belirttiğimiz gibi, komut satırı aracılığıyla AWS ile yapılan her etkileşim, sonuçta denetim amaçlı olarak AWS CloudTrail'de yakalanabilen bir AWS API çağrısına karşılık gelir.

AWS ile program tabanlı olarak çalışma konusunda diğer bir bahsetmemiz gereken yönü, infrastructure as code'dur. Şu ana kadar, kaynakları sağlamak gibi eylemler için AWS ile etkileşimlerimizi otomatikleştirmek için nasıl kod ve script kullanabileceğimize değindik; ancak aynı ilkeleri altyapının kendisini ve yapılandırmasını temsil etmek için de kullanmamız mümkündür. Diyelim ki, tek bir AWS bölgesinde yük dengeleyiciler, EC2 instance'ları, RDS veritabanları ve S3 bucket'ları ile yüksek kullanılabilirliğe sahip bir uygulama kurma görevine girdik. Bu belki de tüm bunları ilk kurduğumuzda console üzerinden manuel olarak yapılandırdık.

Ancak diyelim ki, felaket kurtarma amacıyla tam olarak aynı altyapıyı başka bir AWS bölgesinde kurmamız gerekiyor. Sadece console'u kullanarak, tüm uygulamamızın altyapısını ve tüm yapılandırma ayarlarını başka bir bölgede, her iki bölgedeki kurulumun tamamen aynı olacağı şekilde çoğaltma olasılığımız nedir? Elbette bu mümkün, ancak tüm altyapımızı kod olarak bildirimsel bir şekilde temsil edebilseydik, böylece tüm ortamımızı ihtiyaç duyduğumuz kadar çok kez ve ihtiyaç duyduğumuz kadar çok bölge ve hesap genelinde yeniden üretebilseydik daha kolay olmaz mıydı? İşte CloudFormation burada devreye giriyor.

CloudFormation ile, altyapınızı **stack** adı verilen bir şeyde temsil eden şablonlar oluşturursunuz. Başlamak için AWS, indirmeniz ve özelleştirmeniz için birçok örnek şablon (template) sunar. Daha sonra bu şablonları, bir stack içinde bulunan tüm kaynakları tekrarlanabilir, tutarlı bir şekilde hızlıca sağlamak için kullanabilirsiniz. 

## AWS Systems Manager

Bu başlık altında, özellikleri ve kullanım senaryoları da dahil olmak üzere AWS Systems Manager'a genel bir göz atacağız. Systems Manager, Amazon EC2, kendi veri merkeziniz veya diğer cloud platformlarında çalışan tüm Linux ve Windows instance'larınızda güvenli ve ölçeklenebilir sistem yapılandırmasını, sürekli yönetimini sağlayan tam yönetilen AWS servisler kümesidir. Otomasyona odaklanması, yönetmek istediğiniz instance'ları seçebileceğiniz ve gerçekleştirmek istediğiniz görevleri tanımlayabileceğiniz sistemlerin yapılandırılmasını ve yönetilmesini sağlar.

Ayrıca, bir maintenance window yapılandırarak değişikliklerin ne zaman uygulanacağını da tanımlayabilirsiniz. Sistem imajları oluşturabilir ve güncelleyebilir, software inventory toplayabilir, sistem veya uygulama patch'lerini uygulayabilir, Linux ve Windows işletim sistemlerini yapılandırabilir, ayrıca instance'larınızın durumunu yönetebilirsiniz. Tüm bunları aynı console veya command line interface üzerinden yapabilirsiniz.

Farklı platformlar için farklı araçlar kurmak ve yönetmekle ilgilenmenize gerek kalmaz. Ayrıca, instance'larınıza bağlantı kurmak için secure shell key'leri, secure shell, remote desktop portları ya da bastion host'ları yapılandırmanıza gerek yoktur. Systems Manager, çeviklik ve elastiklik kullanan cloud tipi ölçeklenebilirlik için tasarlanmıştır. Uzun süreli veya geçici workload'ları, bir veya binlerce instance'ı yönetmenize olanak tanır.

Ayrıca, erişim kontrolü için Identity and Access Management, denetim için CloudTrail, event driven otomasyon için CloudWatch events ve diğer birçok AWS yapılandırma ve yönetim aracı gibi AWS'nin geri kalanıyla AWS optimize edilmiş native entegrasyon elde edersiniz. Karmaşık lisanslama modelleri yoktur. Systems Manager'ın işlevselliğinin çoğu ücretsiz olarak sunulmaktadır. 

Kısacası, Systems Manager ile otomasyonu kullanarak geleneksel ve cloud workload'larını yönetebilir, temel kurulum, bakım ve yönetim görevlerini gerçekleştirebilir; aynı zamanda işletim sistemi, konum ve instance sayısından bağımsız olarak tüm makine parkınız üzerinde tam görünürlük ve kontrol sağlayabilirsiniz.

Bununla beraber, Systems Manager mevcut AWS yapılandırma ve yönetim araç setinize iyi bir şekilde uyum sağlar. Size, tüm hesaplarınız ve bölgeler genelinde operasyonel verilerinizi ve iş öğelerinizi gösteren özelleştirilebilir bir operasyon dashboard'u olan Systems Manager Explorer'ı sunar. Explorer'ı ayarları ve tercihleri yapılandırmak için kullanabilirsiniz. Sürükle-bırak ve detaylı inceleme yeteneklerini kullanarak iş öğelerini önceliklendirmek için görüntüyü özelleştirebilirsiniz.

Son olarak, Explorer ile gerektiğinde otomatik runbook'ları kullanarak aksiyonlar alabilirsiniz. Systems Manager ayrıca Amazon CloudWatch dashboard'larınız ve Personal Health dashboard'unuz ile entegre olur. Artık kaynaklarınızı ölçeklenebilir şekilde sağlamanıza ve işletmenize yardımcı olan tam yönetilen AWS Servisleri setine sahip olabilirsiniz. 

### Kaynak Gruplarını Yönetme (Managing Resource Groups)

Systems Manager, her biri kendi yetenekleri ve işlevselliğine sahip 20'den fazla özellik ve entegrasyon içerir. Bunlardan bazıları Fleet Manager, Session Manager, Run Command, Parameter Store, Patch Manager ve State Manager'dır. Bu özelliklerin çoğu, gerçekleştirilecek işlemleri tanımlamak için Systems Manager document'larını kullanır. Ayrıca, bu işlemlerin gerçekleşebileceği tarih ve saati tanımlamak için Maintenance Window'ları kullanırlar. Birlikte, instance'larınızın yaşam döngüsünü kurmak ve yönetmek için kapsamlı bir dashboard ve temel araçlar sağlarlar. Inventory yönetebilir ve asset'leri patch'leyebilir, komutlar çalıştırabilir ve desired state'i yönetebilir, hatta private subnet'lerdeki EC2 instance'larına güvenli bir şekilde bağlanabilirsiniz.

Genel olarak, Systems Manager'ı kullanmak, AWS kaynaklarınızı gruplandırmayı, dashboard'lar aracılığıyla ilgili operasyonel verilerini incelemeyi ve son olarak, bildirilen sorunları hafifletmek için aksiyon almayı içerir. İşlem yapılacak instance'lar üç genel mekanizmadan biri kullanılarak seçilebilir. İlki manuel olarak. Bu, Systems Manager console'unu kullanarak hedef olarak kaydetmek istediğiniz instance'ları tek tek seçtiğiniz yerdir. Ayrıca, bu tag'leri paylaşan instance'ları seçmek için bir veya daha fazla tag key-value çifti belirttiğiniz instance tag'lerini de kullanabilirsiniz. Daha sonra sonuçları bir Resource Group olarak kaydederek gelecekte aynı instance seti üzerinde işlemler gerçekleştirebilirsiniz.

Son olarak, mevcut kaynak tag'lerine dayalı bir sorgu gerçekleştirebileceğiniz veya hedeflemek istediğiniz instance'ları zaten içeren mevcut bir Resource Group'u seçebileceğiniz Resource Group'ları kullanabilirsiniz. Systems Manager, Resource Group'lar aracılığıyla yönetilen instance'ların mantıksal birimleri üzerinde çalışır. Bu, AWS Systems Manager'ın çalışması için hedeflerinizi tanımlamanın en güçlü yoludur. Genel olarak, birbiriyle ilişkili çeşitli AWS kaynakları üzerinde çalışıyorsanız, AWS Resource Group'ları bunları yönetim, sahiplik ve kategoriler açısından daha iyi görünürlük ve toplama için düzenlemenize yardımcı olabilir.

Resource Group'lar, kategorideki öğeleri açıklayan key-value çiftleriyle ortak tag'ler tanımlayarak hayatlarına başlar. Bir Resource Group, aynı bölgedeki belirli bir açıklamaya uyan AWS kaynaklarının bir koleksiyonudur. Resource Group'lar tag tabanlı olabilir, bu da bir kaynak grubunu geliştirme katmanının, üretim katmanının, belirli bir sahibin, bir departmanın veya diğer birçok olası kategori arasında belirli bir projeye adanmış olarak temsil eder. Systems Manager ayrıca CloudFormation stack'lerine dayalı Resource Group'lar üzerinde de çalışabilir. Bu gruplar, belirli bir bölgede aynı CloudFormation stack'i tarafından oluşturulan kaynaklardır. Resource Group, stack ile aynı mantıksal yapıya sahip olacaktır. Systems Manager ve Resource Group'lar, Resource Group'lar hakkında organize ve konsolide edilmiş bilgiler gösteren özel console'lar oluşturmanıza olanak tanır ve operasyon ve yönetim için yardımcı görünürlük sunar.

Varsayılan olarak, AWS Management Console, EC2 Management Console'unda zaten gözlemlemiş olabileceğiniz gibi, kaynakları servis kategorisine göre organize edilmiş şekilde gösterir. Tag Editor, tag'leri ve bir Resource Group haline gelecek şeyi tanımlamanıza olanak tanır. Belirli bir bölgedeki kaynaklara tag'lerin toplu düzenlenmesine ve uygulanmasına izin verir. Tag Policy Editor, belirli bir hesapta veya tüm organizasyonda kaynaklarınız genelinde tag'lemeyi uygulamaya yardımcı olabilir. Resource Group'ları yönetebilir ve Tag Editor'ı AWS Console'unuzun Management Tools bölümlerindeki AWS Resource Group servisi altında bulabilirsiniz. Ayrıca, console'da kaynakları sağlarken, sağlamanın bir bölümü her zaman tag'leri tanımlamanıza izin verecektir.

Fark etmiş olabileceğiniz gibi, kaynaklarınızı tag'lemenin iyi şekilde yapılandırmak, Systems Manager'ın özelliklerini kullanmanız ve bunlardan yararlanmanız için gerekli hale gelir. Instance fleet'inizi oluştururken, bu kaynakları tag'ler kullanarak kataloglamak önemlidir. Daha sonra, bunları gruplamak ve Systems Manager kullanarak üzerlerinde işlem yapmak önemli ölçüde kolaylaşır.

### AWS Systems Manager Gereksinimleri ve Yapı Taşları (AWS Systems Manager Requirements and Building Blocks)

#### SSM Agent

AWS Systems Manager, yönetim hizmeti için bir agent gerektirir. Systems Manager Agent, managed instance olarak adlandırılabilmesi için tüm instance'lara kurulması ve yapılandırılması gereken yazılımdır.

Managed instance, Systems Manager ile iletişim kurabilme ve işlem yapabilme yeteneğine sahip bir instance'dır. Agent, Run Command gibi herhangi bir Systems Manager özelliği aracılığıyla belirlediğiniz görevleri yürütür ve işler. Agent, Amazon Linux AMI'leri, AWS Windows AMI'leri üzerinde varsayılan olarak kurulur ve Amazon Linux repo'sunda mevcuttur. Agent açık kaynaklıdır ve GitHub'da mevcuttur. Agent'ı veri merkezinizdeki veya başka bir cloud sağlayıcısındaki fiziksel bir sunucuya veya sanal makineye kurabilirsiniz. Windows Server 2003 veya daha yeni sürümleri ile Amazon Linux, Ubuntu, Red Hat Enterprise Linux, SUSE ve CentOS gibi Linux dağıtımlarını yönetebilirsiniz.

#### Managed Instance Role'leri

Managed instance'ın, Systems Manager'ın agent ile etkileşime geçebilmesi ve instance'ı Systems Manager Fleet Manager console'unda görünür kılabilmesi için bir instance profile olarak uygulanmış bir Identity and Access Management role'üne ihtiyacı olacaktır. AWS, Systems Manager için önceden tanımlanmış managed policy'ler sağlar. Bunlar genellikle isimlerinin bir parçası olarak SSM kısaltmasını içerir. Bunlardan biri, instance yapılandırmasında size zaman kazandırabilecek Amazon EC2 Role for SSM olarak adlandırılır. Gerekirse kendi özel rolünüzü de oluşturabilir veya mevcut birçok diğer SSM ile ilgili policy'den birini kullanabilirsiniz. Amazon EC2 kapsamı dışındaki veri merkezinizdeki veya diğer cloud sağlayıcılarındaki sunucuları ve sanal makineleri kaydetmek için bir hybrid activation oluşturabilir ve sağlanan activation code ve activation ID'yi kullanarak agent'ı yapılandırabilir ve hybrid ortamınızı ve EC2 instance'larınızı tek bir konumdan merkezi olarak yönetebilirsiniz.

#### Fleet Manager Özelliği

Bir managed instance'ı yapılandırdıktan sonra, Systems Manager console'una gidebilirsiniz. Node Management bölümünün altında Fleet Manager özelliğini göreceksiniz. Tüm managed instance'larınız bu console'da görüntülenecektir. Fleet Manager, her managed instance'ın Instance ID, Platform Type, Instance Type, Operating System name, IP Address ve kurulu olan SSM Agent'ın versiyonu gibi birçok diğer özellik dahil olmak üzere detaylarını görmenizi sağlar. Fleet Manager managed instance console'u hakkında ilginç bir nokta, instance action altında, Systems Manager'ın Session Manager özelliğini kullanarak instance'a bağlanabilmenizdir.

Systems Manager'ın Session Manager özelliği, Linux, Windows ve MacOS instance'ları için etkileşimli bir tarayıcı shell girişi kullanarak herhangi bir managed instance'a bağlanmanıza olanak tanıyan tam yönetilen bir yetenektir. Açık gelen portlara ihtiyaç duymaz ve instance'ınıza bağlanmak için bastion host'ları veya Secure Shell key'leri yönetmeniz gerekmez. Session Manager kullanırken Linux için Secure Shell client'lara veya Windows için Remote Desktop Protocol client'larına da ihtiyacınız yoktur. Session Manager ve instance'lar arasındaki iletişim, Transport Layer Security versiyon 1.2 veya kısaca TLS 1.2 kullanır. İletişimin güvenliği, kendi Key Management Service key'lerinizi kullanarak artırılabilir. Session Manager, bir oturumda üretilen tüm komutları ve çıktıları izler ve ayrıca CloudTrail, CloudWatch veya bir Amazon S3 bucket'ına gönderilebilen tam loglama ve oturum denetimi etkinliği sağlar. Session Manager, Identity and Access Management policy'lerini kullanarak hangi kullanıcıların belirli instance'lara erişebileceğini kontrol edebilir. Etkileşimli tarayıcı shell'i veya AWS Command Line Interface kullanılarak çalışır.

### AWS Systems Manager Operations

Bir managed instance'ı Systems Manager agent'ını yapılandırarak ve bir Systems Manager erişim rolü atayarak yapılandırdıktan sonra, Systems Manager console'una gidebilir ve "Node Management" bölümünün altında "Fleet Manager" özelliğini bulabilirsiniz.

Tüm managed instance'larınız bu console'da görüntülenecektir. Fleet Manager, her managed instance'ın Instance ID, Platform Type, Instance Type, OS Name, IP Address ve SSM Agent versiyonu gibi detaylarını görmenizi sağlayacaktır.
Managed instance fleet'inizi tek bir ekranda görebilmek çok kullanışlıdır.

Systems Manager'ın "Node Management" bölümünde, session manager özelliğini göreceksiniz. Session manager, Linux, Windows ve MacOS instance'ları için etkileşimli bir tarayıcı shell girişi kullanarak herhangi bir managed instance'a bağlanmanıza olanak tanıyan tam yönetilen bir yetenektir. Açık gelen portlara ihtiyaç duymaz ve instance'larınıza bağlanmak için bastion host'ları veya SSH key'leri yönetmeniz gerekmez.

Session Manager kullanırken Linux için SSH client'lara veya Windows için RDP client'larına da ihtiyacınız yoktur. Session manager ve instance'lar arasındaki iletişim güvenlidir ve session manager, bir oturumda üretilen tüm komutları ve çıktıları izler, bunlar sonuç olarak CloudTrail, CloudWatch veya bir Amazon S3 bucket'ına gönderilebilir.

### AWS Systems Manager Run Command

Systems Manager'ın "Run command" özelliği, adından da anlaşılacağı gibi, bir veya daha fazla instance'ınızda bir komut çalıştırmanıza olanak tanır. Çalıştırılacak komut veya komutların karmaşıklığı, bir Systems Manager Document'inde tanımlanır. Document'ler, agent'ın instance'larınızda gerçekleştireceği eylemleri tanımlar ve systems manager console'unda paylaşılan kaynaklardır.

Document'ler JSON veya YAML formatında yazılır. Yeniden kullanılabilirler ve parametre kabul ederler. Bir document ile bir shell script çalıştırabilir veya yönetilen bir instance üzerinde herhangi bir yönetim görevi gerçekleştirebilirsiniz. Klonlayıp değiştirebileceğiniz veya olduğu gibi kullanabileceğiniz düzinelerce hazır document bulunmaktadır. Run command ile yaygın olarak kullanılan bir document, "AWS-RunShellScript" document'idir.

Genel olarak, Run command bir document belirtmenizi ve document'in çalıştırılacağı hedef instance'ları belirlemenizi gerektirir. Herhangi bir instance için, belirli bir uygulamanın durumunu ve o instance'da çalıştırılan belirli bir komutun çıktısını görebilirsiniz.

Instance sayısı fazlaysa, run command ayrıca bir Rate Control tanımlamanıza olanak tanır. Run command için rate control yapılandırması, document'in aynı anda kaç hedefte çalıştırılacağını belirtmek için concurrency kullanır. Ayrıca, belirtilen sayıda instance'da başarısız olduktan sonra görevi durdurmayı belirten bir Error threshold kullanır. Output seçenekleri, komut çıktısını bir Amazon S3 bucket'ına yazar veya Amazon CloudWatch logs'a gönderir. Ayrıca systems manager'ı, Amazon SNS kullanarak komut durumu hakkında bildirimler gönderecek şekilde yapılandırabilirsiniz.

### AWS Systems Manager Parameter Store

Bir Run Command çalıştırdığınızda, hedefler ve komut document'i, iletmek istediğiniz çalışma zamanı parametrelerini kullanacaktır.

AWS Systems Manager Parameter Store, veritabanı bağlantıları veya lisans kodları gibi düz metin verilerini ve parolalar veya diğer uygulama yapılandırma verileri gibi gizli bilgileri yönetmek için merkezi bir depolama sağlar. Parameter Store, gerektiğinde değerleri otomatik olarak şifreleyebilmeniz için AWS Key Management Service (KMS) ile entegredir.

Parameter Store, gizli bilgileri ve yapılandırma verilerini koddan ayırmanıza olanak tanıyarak ve AWS CloudTrail kullanarak entegre denetim özelliklerinden yararlanarak uygulamanızın güvenliğini artırır. Parametreler etiketlenebilir ve kolay yönetim için hiyerarşilere düzenlenebilir; aynı kaynak için Development, QA ve Production gibi farklı katmanlarda aynı parametre için bir hiyerarşiye sahip olabilirsiniz.

Parametrelerdeki değişiklikleri sürümler kullanarak takip edebilirsiniz. Parametre değişiklik bildirimleri oluşturabilir ve AWS Lambda fonksiyonlarını kullanarak kendi özel doğrulama rutinlerinizi oluşturabilirsiniz.

Son olarak, Parameter Store verileri AWS Systems Manager ile sınırlı değildir; parametreler Amazon ECS, AWS Lambda, CloudFormation, CodeDeploy, CodePipeline ve özel uygulamalarınız gibi diğer AWS hizmetleri tarafından da referans alınabilir. Systems manager parameter store kullanarak, bir parametre türü ile yeni bir parametre oluşturur ve karşılık gelen değeri belirlersiniz. Daha sonra, komutlarınızda veya uygulama kodunuzda parametrelere referans verebilirsiniz.


### Maintenance Windows

Potansiyel olarak yıkıcı olan run command document gibi yönetim görevlerini manuel olarak veya önceden tanımlanmış bir maintenance window sırasında çalıştırabilirsiniz. Bir maintenance window, işletim sistemini yamalama, EC2 instance'larınızdaki sürücüleri güncelleme, yazılım yükleme veya desteklenen kaynaklarda görevler planlama gibi işlemleri programlamanıza olanak tanır. Eşzamanlı yürütmeler ve izin verilen hata oranları için sınırlar belirleyebilirsiniz.
Maintenance window, bir Run command document, bir AWS Step Functions veya bir AWS Lambda Function kullanarak karmaşık görevleri tanımlamanıza ve çalıştırmanıza olanak tanıyan bağımsız bir kaynaktır. Ayrıca, gerekirse bir maintenance window'da yürütülen tüm görevlerin geçmişini görüntüleyebilirsiniz.

#### Maintenance window nasıl çalışır?

İlk olarak, potansiyel olarak yıkıcı eylemlerin gerçekleşebileceği zaman aralığını belirten bir program tanımlamanız gerekir. Başlangıç zamanını ve bitiş zamanını belirleyebilirsiniz. Zaman periyodunu tanımlamak için Cron veya Rate ifadelerini de kullanabilirsiniz. Ayrıca maintenance window'un süresini saat cinsinden belirtmeniz gerekir.

Bir maintenance window oluşturulduktan sonra, bir dizi instance'ı maintenance window'unuza atayan hedefleri isme göre kaydedebilirsiniz. Instance tag'lerini belirleyebilir, bir resource group seçebilir veya instance'ları manuel olarak seçebilirsiniz.
Ayrıca bir command document görevi çalıştırmak için kaydolabilir, bir Automation document görevi çalıştırmak için kaydolabilir, bir Lambda Function görevi yürütmek için kaydolabilir veya Step Functions görevi yürütmek için kaydolabilirsiniz.

Maintenance window'lar, yönetilen instance'larınızda herhangi bir sayıda görevi operasyonel kesinti olmadan çalıştırabilir, böylece potansiyel olarak yıkıcı olan yönetim görevlerini, değişikliklerin uygulanabileceği önceden tanımlanmış bir dönemde çalıştırabilirsiniz.

### AWS Systems Manager Document

Systems Manager Belgeleri (Documents), Run command gibi birçok Systems Manager özelliği, gerçekleştirilecek eylem adımlarını belgeler aracılığıyla tanımlar. Belgeler, JSON veya YAML olarak yazılmış bağımsız kaynaklardır. Kullanılacak eylem adımlarını ve parametre değerlerini içermenize olanak tanır. Systems Manager, yarım düzineden fazla belge türünü destekler. Bizim durumumuzda, Systems Manager tarafından kullanılan en yaygın belge türünden bahsetmiştik: Command document (Komut belgesi).

Komut belgeleri, instance'larda yürütülecek eylemleri ve kullanılacak belirli değerleri tanımlamak için run command ile kullanılır. Komut belgeleri ayrıca Systems Manager'ın State Manager özelliği tarafından instance'larınıza yapılandırmalar uygulamak için kullanılır. Son olarak, Maintenance Windows (Bakım Pencereleri), önceden tanımlanmış bir programa göre yapılandırmaları uygulamak için Komut belgelerini kullanır. Bir Komut belgesi ile bir shell script'i çalıştırabilir, CloudWatch'u yapılandırabilir, Docker'ı yapılandırabilir veya yönetilen bir instance üzerinde herhangi bir yönetim görevini gerçekleştirebilirsiniz. Çalışma zamanında parametreler belirleyerek kullanabileceğiniz 100'den fazla önceden yapılandırılmış Systems Manager belgesi vardır. Ayrıca bir belgeyi değiştirebilir veya olduğu gibi kullanabilirsiniz. Run command ile yaygın olarak kullanılan belgeler, Linux için AWS run Shell Scripts ve Windows Sistemleri için AWS run PowerShell Script belgesidir. Belgeler Systems Manager Belge Deposunda (Documents Store) bulunur ve paylaşılan bir kaynaktır. Çoğunlukla, Systems Manager'ın run command ve State Manager özellikleri tarafından kullanılan Komut belgeleriyle ilgileneceğiz.

### AWS System Manager Feature Özet

Önceki başlıklarda değindiğimiz gibi hazırlanan herhangi bir sayıda instance için Systems Manager ile neler yapabiliriz? Yapabileceğimiz ilk şeylerden biri filomuz üzerinde bir göz atmaktır. Systems Manager konsoluna gidebilir ve node management bölümü altında Fleet Manager özelliğini bulabilirsiniz. Tüm yönetilen instance'larınız bu konsolda görüntülenecektir. Fleet Manager, her yönetilen instance'ın detaylarını görmenizi sağlar.

Alabileceğimiz bir diğer aksiyon, Session Manager özelliğini kullanarak herhangi bir instance'a güvenli bir şekilde bağlanmaktır. Systems Manager'ın node management bölümü altında Session Manager özelliği olarak bulabilirsiniz. Session Manager, herhangi bir yönetilen instance'a interaktif bir tarayıcı shell'i kullanarak bağlanmanızı sağlayan tam yönetilen bir özelliktir. Açık inbound portlara, bastion host yönetimine veya instance'larınıza bağlanmak için SSH anahtarlarına ihtiyaç duymaz. Session Manager ve instance'lar arasındaki iletişim güvenlidir ve Session Manager, denetim ve uyumluluk nedenleriyle bir oturumda üretilen tüm komutları ve çıktıları izler.

Yapabileceğimiz üçüncü şey, instance'lardan herhangi birinde, istersek hepsinde bir veya daha fazla komut çalıştırmaktır. Systems Manager Run Command, yönetilen instance'larınızdan bir veya daha fazlasında bir komut çalıştırmanıza izin verecektir. Çalıştırılacak komutun karmaşıklığı, daha önce bahsedildiği gibi komut document'ında tanımlanır. Document'lar, ajanın Systems Manager konsolundaki instance'larınızda veya paylaşılan kaynaklarınızda gerçekleştireceği eylemleri tanımlar.

Genel olarak, Run Command bir document belirtmenizi ve document'ın çalıştırılacağı hedef instance'ları belirlemenizi gerektirir. Herhangi bir instance için, o instance üzerindeki bir komutun durumunu ve çıktısını görebilirsiniz. Instance'larınızda değişikliğe neden olan çoğu özellik gibi, Run Command da bir rate control tanımlayabilir veya filonuzun aynı anda güncellenen yüzdesini, eşzamanlılık için bir değer veya yüzde kullanarak ve gözlemlenen sorunları araştırmak için komutun tamamen durması gereken hata eşiklerini belirleyebilir.

Systems Manager Parameter Store ile konfigürasyon verilerini koddan ayırabiliriz. Parameter Store, veritabanı bağlantıları veya lisans kodları gibi düz metin verilerini ve şifreler gibi gizli bilgileri veya diğer uygulama konfigürasyon verilerini yönetmek için merkezi bir depolama sağlar. Parameter Store, AWS Key Management Service veya KMS ile entegredir.

Gerekirse parametre değerlerini otomatik olarak şifreleyebilmeniz için, sürümleri kullanarak parametre değişikliklerini izleyebilir, parametre değişiklik bildirimleri de oluşturabilir. AWS Lambda fonksiyonlarını kullanarak kendi özel doğrulama rutinlerinizi oluşturabilirsiniz. Parameter Store verilerine erişim AWS Systems Manager ile sınırlı değildir. Parametreler, Amazon ECS, AWS Lambda, CloudFormation, CodeDeploy, CodePipeline ve kendi özel uygulamalarınız gibi diğer AWS servisleri tarafından referans alınabilir.

Systems Manager'ın Maintenance Window özelliği, inceleyeceğimiz bir sonraki öğedir. Maintenance Windows ile potansiyel olarak yıkıcı geçişleri manuel olarak veya önceden tanımlanmış bir zaman diliminde çalıştırabilirsiniz. Bir Maintenance Window, yönetilen instance'larda işletim sistemine patch yapmak, sürücüleri güncellemek veya yazılım yüklemek gibi görevleri planlama yeteneği verir. Eşzamanlı yürütmeler ve izin verilebilir hata oranları için sınırlar belirleyebilirsiniz.

Maintenance Window, bir Run Command document'ı, bir AWS step function veya bir AWS Lambda fonksiyonu kullanarak karmaşık görevleri tanımlamanıza ve çalıştırmanıza olanak tanıyan bağımsız bir kaynaktır. İsterseniz bir Maintenance Window'da yürütülen tüm görevlerin geçmişini de görüntüleyebilirsiniz. Bir Maintenance Window oluşturulduktan sonra, bir dizi instance'ı Maintenance Window'unuza atayan isimlere göre hedefleri kaydedebilirsiniz. Instance etiketlerini belirtebilir, bir kaynak grubu seçebilir veya instance'ları manuel olarak seçebilirsiniz. Maintenance Windows, yönetilen instance'larınızda herhangi bir sayıda görevi çalıştırabilir, operasyonel kesinti süresini önleyerek, potansiyel olarak yıkıcı olan yönetim görevlerini uygulamanızın kullanılabilirliğine çok az veya hiç etki etmeden değişikliklerin uygulanabileceği önceden tanımlanmış bir dönem boyunca çalıştırmanıza olanak tanır.

## AWS AppConfig

AWS AppConfig, AWS Systems Manager'ın bir parçasıdır. AWS'de çalışan uygulamalara feature flag'ler ve diğer güncellemelerin ne zaman ve nasıl dağıtılacağını kontrol etmenize olanak tanırken, aynı zamanda bir uygulamanın sağlığını ve performansını izleme yeteneği sunar. AppConfig ile, uygulamalarınızın her zaman istenen bir durumda çalışmasını ve değişikliklerin kontrollü, öngörülebilir bir şekilde yapılmasını sağlamaya yardımcı olan farklı uygulama konfigürasyonları oluşturabilirsiniz.

AppConfig, uygulamanız için ilgili bir dizi konfigürasyon verisini düzenlemenize ve yönetmenize olanak tanır. Bu veriler daha sonra development, staging veya production gibi bir veya daha fazla ortama uygulanabilir. Bir ortam, hedef instance'lar için bir deployment group görevi görür ve bir ortama yapılan deployment sırasında bir alarm tetiklenirse, herhangi bir konfigürasyon değişikliğini geri almak için CloudWatch Alarms ile entegre olabilir.

AppConfig, uygulamanız için feature flag'ler, connection string'ler veya diğer uygulamaya özel parametreler gibi şeyleri içerebilen configuration profile'ları oluşturmanıza olanak tanır. Configuration profile'ları, güncellemeleri tüm filonuza yaymadan önce belirli bir instance setine veya hatta belirli bir yüzdesine uygulamanıza izin verir. Bu, potansiyel olarak müşteri etkisine veya hatta kesintiye neden olabilecek değişiklikleri, tüm uygulamaya uygulanmadan önce test etmenize ve doğrulamanıza olanak tanır.

AppConfig ayrıca aşağıdaki ek özellikleri sunar:

-   Bir konfigürasyonun formatını ve değerlerini deployment öncesinde doğrulama yeteneği, tüm değişikliklerin doğru formatta olduğundan ve herhangi bir hata içermediğinden emin olunmasını sağlar.
-   Konfigürasyon değişikliklerini izleme ve geri alma yeteneği, bir deployment sırasında oluşabilecek herhangi bir sorundan kurtulmanıza olanak tanır.

Özetlemek gerekirse, AWS AppConfig, geliştiricilerin uygulama güncellemelerini AWS'ye kolayca ve güvenli bir şekilde dağıtmasına yardımcı olmak için tasarlanmıştır. Aynı zamanda bunları yapılandırmak, izlemek ve sorunlarını gidermek için basit ve güçlü bir araç sunar.


## AWS Cloud Development Kit (CDK)

CDK hizmeti, hem AWS Certified Developer - Associate hem de AWS Certified DevOps Engineer - Professional sınavları için yakın zamanda kapsama alındı, bu yüzden CDK'ye ve ne yaptığına hızlıca göz atalım.

CDK, AWS altyapısını ve kaynaklarını kodda tanımlamanıza olanak tanıyan açık kaynaklı bir yazılım geliştirme çerçevesidir. Derleme zamanında otomatik olarak eşdeğer AWS CloudFormation template'lerini oluşturur, bu template'ler daha sonra herhangi bir CloudFormation template'i gibi sağlanabilir. Bu, kendi YAML template'lerinizi yazmak veya bakımını yapmak zorunda kalmadan, geri alma ve drift tespiti gibi CloudFormation'ın tüm avantajlarından yararlanmanızı sağlar. Bunun yerine, CDK Python, C#, Java, JavaScript ve TypeScript gibi tanıdık programlama dillerini destekler. Bu, geliştiricilerin mevcut becerilerinden yararlanmalarına ve fonksiyonlar, döngüler ve koşullu ifadeler gibi yaygın programlama yapılarını kullanmalarına olanak tanır, bu da karmaşık altyapıyı oluşturmayı, inşa etmeyi ve yönetmeyi kolaylaştırır.

CDK, S3 bucket'ları, DynamoDB tabloları, Lambda fonksiyonları ve API Gateway'deki API'ler gibi çeşitli AWS kaynaklarını tanımlamak için kullanılabilen construct olarak bilinen bir dizi kütüphane sağlar. Geliştiriciler bu construct'ları kodları içinde kullanabilir ve hatta kendi construct'larını oluşturup özelleştirebilir, bunlar diğer geliştiricilerle yeniden kullanılabilir ve paylaşılabilir.

CDK ayrıca, CDK ile etkileşimde bulunmak ve yeni CDK uygulamaları oluşturmak ve bunları belirli ortamlara dağıtmak gibi yaygın görevleri gerçekleştirmek için kullanılabilen bir command-line interface sağlar.

Özetlemek gerekirse, CDK, CloudFormation template'leri oluşturmak için tanıdık programlama dillerini kullanarak bulut altyapısını oluşturmak ve yönetmek için güçlü ve esnek bir yol sunar.







