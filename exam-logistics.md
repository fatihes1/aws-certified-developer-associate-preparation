# Sıvav Yapısı

AWS Developer - Associate sınavı 65 sorudan oluşur ve tamamlamak için 130 dakikanız vardır. Bu da iki saat on dakikaya denk gelmektedir. Basit bir matematik yaparsanız, her soru için yaklaşık iki dakikanız olacaktır. Bazı sorular diğerlerinden daha hızlı okunur. Bununla beraber cevaplaması kolay sorular olacaktır. Soru başına iki dakika sadece bir ortalamadır. Ancak, sınava hazırlıklıysanız, soruların hiçbirinin bu kadar uzun sürmesi gerekmeyebilir.

65 sorunun her biri giriş kısmında bahsettiğimiz dört temel alandan biri hakkındadır. Geçme notu, 1.000 üzerinden 720 puandır.

Bu, ölçeklendirilmiş bir puandır. Yani, AWS bazı sorulara diğerlerinden daha fazla ağırlık verir. Bu da bazı soruların diğerlerinden daha fazla puan getirdiği anlamına gelecektir. Ayrıca, sınav kılavuzunda **puanlanmamış içerik (unscored content)** başlıklı bir paragraf bulunur.

Bu paragrafta şu yazılıdır:

"Sınavınız, istatistiksel bilgi toplamak amacıyla teste yerleştirilen puanlanmamış sorular içerebilir. (Your examination may include non-scored questions that are placed on the test to gather statistical information.)"

Puanlanmamış, tam olarak bu anlama gelir? Doğru ya da yanlış, bu sorular sınavı geçip geçmeyeceğinizi etkilemeyecektir. Hangi soruların bu olduğunu, neden dahil edildiklerini veya doğru yanıtlayıp yanıtlamadığınızı bilmeyeceksiniz.

AWS'de herhangi bir süre geçirdiyseniz re:Invent'in videolarına katılmış veya videolarını izlemiş olabilirsiniz. Yeni hizmetler genellikle MVP olarak anılır; Minimum Viable Product. Zamanla, bu hizmetler daha fazla özellik kazanır ve daha sağlam hale gelir. AWS Certified Developer sınavı için 720 geçme notu benzerdir. Ancak, minimum viable (minimal düzeyde geçerli) yerine, minimum nitelikli (minimally qualified) gibidir.

Minimum nitelikli, işi yapmak için yeterli becerilere sahip olduğunuz ve bu şekilde tanındığınız anlamına gelmektedir.

Sınav, güncel ve geçerli AWS bilgilerini ve kavramlarını test etmek için tasarlanmıştır. Düğme yerleşimi gibi önemsiz bilgileri veya gerektiğinde belgelerle başvurulması gereken şeyleri içermez. Bunun yerine, bir geliştirici rolü için geçerli ve önemli görevleri test eder. Bunlar; sorunları çözme, performansı iyileştirme, hata ayıklama veya güvenliği artırma yeteneğini değerlendirme gibi konulardır.

Özünde, sınav yetkinliği ve teorik veya önemsiz bilgileri test etmez. Önemsiz bilgilerle, teknik jargon, yerel terminoloji, iş yeri spesifik terimler ve karmaşık dili kastediyoruz. Bu tarz bilgiler şeyler yeteneği ölçmez ve haliyle sınavda bulunmaz.

Peki, eğer sınav yetkinliği test ediyorsa, bunu nasıl yapar?

Yanıt, aslında sınav kılavuzunda bulunur. Sınav kılavuzunu gözden geçirmek ve anlamak önemlidir çünkü her soru, belirli bir beceriyi sınav kılavuzundaki bir hedefe göre ölçmek için hazırlanmıştır. Çoğu kişinin test sorusu olarak düşündüğü şey aslında üç bölüme sahiptir: bir senaryo, bu senaryo hakkında belirli bir soru ve olası cevapların bir seti.

Senaryo resmi olarak kök (stem) olarak bilinir ve sorunun hazırlığını yapar. Amacı, değerlendirilen kavramı tanıtmaktır. Kökler, doğru cevabı vermek için gereken tüm detayları içerir. Kökten sonra gelen soru, sınav kılavuzuyla doğrudan ilgili olan AWS'nin tek bir kavramı veya yönü üzerine odaklanır. Sadece bir şey test edilir. Yani, bir soru yüksek derecede erişilebilir veya maliyet etkin bir çözüm isteyebilir, ancak ikisini birden istemez.

Yaygın soru temaları arasında yüksek erişilebilirlik, maliyet etkinliği, en güvenli olanı, performans iyileştirmesi ve en az kesinti süresi bulunur. En iyi veya en kolay çözüm aynı anda sorulmaz çünkü sizin için en iyi olan şey başkası için en kötü olabilir.

Bunun yerine, sorular genellikle bir durumu belirleyen niteleyicilere sahiptir. Örneğin: 
- Bu seçeneklerden hangisi maliyet açısından en etkin? 
- Hangi işlem hiç kesinti yapmaz? 
- Hangisi en az gecikmeye sahiptir?

AWS sınav soruları, hizmetlerinin ve özelliklerinin kullanımına odaklanır. Yani, çözümleri uygulamak için gereken bilgi ve becerilere sahip olup olmadığınız test edilir.

Diğer bir deyişle, EC2 örnekleri için performans metriklerini, hizmet limitlerini, AWS CLI için sözdizimini ve çeşitli hizmetlerin fiyatlarını ezberlemenize gerek olmadığı anlamına gelir. Daha önce değindiğimiz gibi, bu önemsiz bilgiler ve uygulanması sırasında kolayca bulunabilecek şeylerdir. Sınavda yer almaya değmezler.

Yanıtlara daha detaylı bakacak olursak, yanıtların diğer yanıtların kombinasyonları olduğu sorular olmayacak. Yani şu gördüğünüz gibi aşina olduğunuz şıklar karşınıza çıkmayacaktır.
- A, C ve D, ancak B değil. 
- A ve D, ancak C veya B değil.
- Yalnızca B ve C.

Şimdi yanıt seçeneklerine geçelim. Olası yanıtlar listesi, doğru yanıtı ve yanlış yanıtları içerir. Sınav oluşturma dünyasında, doğru yanıt anahtar olarak adlandırılır ve yanlış yanıtlar yanıltıcılar olarak adlandırılır. Yanıltıcılar, sorunun bağlamında yanlıştır. AWS sınavlarına girdikçe, bu seçeneklerin yanlış olmasına rağmen yine de makul olduğunu göreceksiniz. Yani, bir şıkta biraz doğruluk vardır. Başka bir bağlamda doğru bile olabilirler. Ancak sorunun bağlamında yanlış değerlendirilecektir. Bazen, yanıltıcıyı makul kılan bir kelime veya ifade, doğru gibi görünmesidir. Ancak, test edilen konuyu biliyorsanız, yanlış olduğunu anlamak eğlenceli olabilir. Bir tür tuzak gibidir. Tam bilgiye sahip olmayanlar için çekici olacaktır. Bu tür sorular, minimum nitelikli bir kişinin becerilerine sahip olmayan birini tuzağa düşürecektir.

Sınavda iki tür soru vardır; çoktan seçmeli ve çoklu yanıt. Çoktan seçmeli sorular bir doğru yanıt ve üç yanlış yanıt içerir. Çoklu yanıt sorularında beş olası seçenekten iki doğru yanıt vardır. Sınavda yalnızca bu tür sorular vardır. Doğru/Yanlış, Eşleştirme, Sürükle ve Bırak, Kısa Yanıt veya Boşluk Doldurma soruları yoktur.

Şimdi, bunu söylemek garip gelebilir, ancak bir soruyu doğru yanıtlamak için, ifadeyi en iyi tamamlayan veya soruyu yanıtlayan seçeneği seçmeniz gerekir. Bu cümledeki anahtar kelime **en iyisi**dir. AWS sınavlarında, doğru yanıtların bazen gerçek dünyada nasıl yapılacağından çok farklı olduğunu görebilirsiniz. Bir hile varsa, mümkün olanı tanımak ve imkansız veya saçma olan yanıtları elemek işinizi kolaylaştıracaktır.

Örneğin, S3'te saklanan bir dosyanın güncellenmesi hakkında bir soru, S3'teki nesnelerin değiştirilemez (immutable) olduğunu anlamanızı gerektirir; yalnızca yer değiştirilebilir. Bu nedenle, doğru yanıt yeni bir nesne oluşturma kavramını içermelidir. Başka bir seçenek yanlış olur ve elenebilir.

Bir soruyu atlar ve yanıtlamazsanız ne olacağını merak edebilirsiniz. Yanıtlanmamış sorular ve yanlış yanıtlar aynı şekilde puanlanır; yanlış kabul edilirler. Çoklu yanıt soruları hakkında, ya %100 doğru ya da yanlıştırlar. Kısmi kredi yoktur. Soruları gözden geçirmek için işaretleyebilirsiniz. Sınavı bitirdiğinizde, işaretlediğiniz soruları gözden geçirmeniz istenecektir.

AWS sertifikasyon sınavlarını oluşturan kişiler, sınavda öğretici ifadeler bulunmamasını sağlamak için çok çalışır. Yani, sınav sorularında herhangi bir AWS hizmetini veya özelliğini tanımlayan bir dil yoktur. Eğer olsaydı, bir soru başka bir soruyu aslında yanıtlayabilir.

Örneğin, bu bir soruda: "Kuruluşunuz AWS Lambda'yı kullanıyor, bu, sunucu sağlamadan veya yönetmeden kod çalıştırmanıza olanak tanıyan bir hesaplama hizmetidir." gibi bir öğretici ifade olması durumunda; aynı sınavda başka bir soru, hangi hizmetin sunucu sağlamadan veya yönetmeden kod çalıştırmanıza izin vereceğini sorsa, yanıtı bilmeyen kişiler bile sınavın kendisi yanıtı verdiği için doğru yanıtlayabilir. AWS sınavlarında bu tür öğretici ifadeler görmek neredeyse imkansızdır. Nadiren, herhangi bir soru başka birine yardımcı olmuştur; hatta bir hafızayı tetikleyerek bile. Bittiğinde ise, sınavı bitir butonunu kullanırsınız.

Bittiğinizde, emin olup olmadığınız sorulacak. Sınav sona ermeden önce sanırım üç kez size uyarı verecektir.
- "Gerçekten emin misiniz?" Evet, eminim. <tıkla>
- Sonra tekrar soracak, "Gerçekten emin misiniz? Geri dönüş yok."
- Evet, eminim. Neden bana işkence ediyorsun? <tıkla>
- Son olarak, "Gerçekten, gerçekten emin misiniz?"
- Evet, eminim. Lütfen alay etmeyi bırak. Bitmesini istiyorum. <tıkla>

Tamam, belki bunlar gerçek istemler değil. Ancak her sınav deneyiminden sonra böyle hissedebilirsiniz 😅 O üçüncü uyarıdan sonra, sınavı geçip geçmediğinizi öğreneceğinizi düşünebilirsiniz. Ancak üzgünüm ki yanılırsınız.

Sonrasında bir anket doldurmanız istenecektir. Geçip geçmediğimi bilmek istiyorsunuz. Ancak, AWS'deki insanlar sınavın yeteneklerimi doğru bir şekilde temsil edip etmediğini bilmek istiyorlar.

Anketten sonra, sınavı geçip geçmediğiniz söylenir. Ancak puanınızı öğrenemezsiniz. Bu işlem bir gün (bazen daha uzun) sürebilir. En uzun beklediğim süre üç gündü. Son birkaç sınav için sonuçları 24 saat içinde aldım.
