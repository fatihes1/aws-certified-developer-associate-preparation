# Ölçme Testi: Networking and Content Delivery (DVA-C02)

**Which of the following statements about AWS security groups is false?**

- [x] They are stateless.
- [ ] If there isn't a rule that explicitly permits a particular packet, it will simply be dropped.
- [ ] The rule set is made up of two rule sets, inbound and outbound.
- [ ] Any rule that authorizes traffic into an EC2 instance will allow any response to be returned without an explicit rule in the outbound ruleset.

NACL'lerden farklı olarak güvenlik gruplarında bir kural için 'Deny' eylemi yoktur. Bunun yerine, eğer belirli bir pakete açıkça izin veren bir kural yoksa, o paket basitçe kaldırılacaktır. Kural seti, gelen (inbound) ve giden (outbound) olmak üzere iki kural setinden oluşur. Güvenlik grupları stateful'dur; yani stateless olan NACL'lerin aksine, yanıt trafiği için hem gelen hem de giden trafik için aynı kurallara ihtiyacınız yoktur. Bu nedenle, bir EC2 bulut sunucusuna giden trafiğe izin veren herhangi bir kural, giden kural kümesinde açık bir kural olmadan herhangi bir yanıtın döndürülmesine izin verecektir.

----------

**What is Amazon CloudFront's first step in processing file requests?**

- [ ] The request is routed back to the origin for file transfer.
- [ ] The request is routed back to the origin, and then to the Edge location for file transfer.
- [ ] The request is routed to the edge location closest to the origin. 
- [x] The request is routed to the edge location that can deliver the file with the least latency.

Amazon CloudFront, uç konumlardan (edge locations) oluşan ağ aracılığıyla statik ve dinamik içeriğinizin dağıtımını hızlandırır. Bir dosya için istek yapıldığında CloudFront, dosyanın aktarımı için isteği web sunucusuna yönlendirmez. İstek, dosyayı en az gecikmeyle teslim edebilecek en yakın edge location'a yönlendirilir. İsteği en son dosya sürümü için web sunucusuna geri yönlendirmeden önce dosya için önbelleğini kontrol eder. İstek başlangıçta web sunucusuna yönlendirilmez, bunun yerine web sunucusu isteği yapılmadan önce edge location'daki önbellek kontrol edilir. İstek edge location'da önbelleğe alınmaz, edge location'da ön belleğe alınan yapı dosyalardır.

---

**Within AWS, what is an accurate description of a Virtual Private Cloud (VPC)?**

- [x] An AWS user's isolated network segment within the AWS cloud
- [ ] An AWS user's connection between two remote networks via the public internet
- [ ] An AWS user's connection between two remote networks via private AWS infrastructure
- [ ] An isolated segment of an AWS network that cannot receive inbound traffic from the internet

Virtual Private Cloud, bir AWS kullanıcısının AWS bulutunun ağ segmenti olarak tanımlanabilir. Diğer ifadeler bir virtual private network'u, AWS Direct Connect aracılığıyla bir bağlantıyı ve bir VPC içindeki özel bir alt ağı açıklar.

---

**What does Amazon Route 53 Application Recovery Controller uses to manage failovers?**

- [x] routing controls
- [ ] a control group
- [ ] a configuration control group
- [ ] an email

Trafiği Amazon Route 53 Application Recovery Controller'daki uygulama replikalarına devretmek için yönlendirme kontrollerini (routing controls) kullanılır. Routing Controls belirli bir tür durum denetimiyle entegre edilmiştir. Hazırlanmamış bir kopyaya yük devretmeyi önlemenin bir yolu olarak yönlendirme kontrollerine güvenlik kuralların (rules of routing) da kullanabilir.

---

**Which of the following statements about Amazon Route 53 is false?**

- [ ] It is a global service.
- [ ] It uses edge locations in the AWS global infrastructure.
- [x] You are required to specify a region when configuring Route 53 resources.
- [ ] AWS offers a 100% available SLA for Route 53.

Amazon Route 53, AWS küresel altyapısındaki edge location'ları kullanır ve küresel bir hizmettir. Route 53 kaynaklarını yapılandırırken bölge belirtmenize gerek yoktur. AWS, Route 53 için %100 kullanılabilir SLA sunar.

--- 

**In which case does AWS recommend selecting an Application Load Balancer?**

- [x] When an application requires load balancing of HTTP requests
- [ ] When an application requires load balancing of TCP or UDP protocol traffic
- [ ] When an application requires the use of a third-party virtual appliance
- [ ] Application Load Balancers are not recommended because the related network is being retired

Elastic Load Balancing (ELB) dört tür yük dengeleyiciyi destekler. Uygulama ihtiyaçlarınıza göre uygun yük dengeleyiciyi seçebilirsiniz. HTTP isteklerini dengelemek istiyorsanız, Application Load Balancer (ALB) kullanmanızı öneririz. Ağ/taşıma protokolleri (katman 4 - TCP, UDP) için yük dengeleme ve aşırı performans/düşük gecikme süreli uygulamalar için Network Load Balancer kullanmanızı öneririz. Uygulamanız Amazon Elastic Compute Cloud (Amazon EC2) Classic ağı içinde yer alıyorsa, Classic Load Balancer kullanmalısınız. Üçüncü taraf sanal cihazları dağıtmak ve çalıştırmak istiyorsanız, Gateway Load Balancer kullanabilirsiniz.

---

**When an AWS resource is deployed in a VPC public subnet, what function does a public IP address provide?**

- [ ] A central target where route tables direct all VPC egress traffic to the Internet
- [x] Set a specific resource's network location to support communication with the Internet  
- [ ] Allow or restrict external traffic from entering the subnet based on a series of rules
- [ ] Serve as an anchor on the customer side of a VPN connection to a VPC

Genel bir IP adresi, dış mesajların VPC alt ağınızdaki kaynaklara ulaşması için gereken konumdur. Bu olmadan, dış kaynaklar size nasıl ulaşacaklarını bilemezler. Diğer seçenekler ise internet ağ geçitleri, ağ ACL'leri ve müşteri ağ geçitleri tarafından sağlanan hizmetlerdir.

---

**Which of the following describes the role of an NACL for a subnet?**

- [ ] It is stateful and monitors all types of traffic into the subnet.
- [ ] It is stateful and monitors inbound traffic into the subnet.
- [ ] It is stateless and responds only to outbound traffic from the subnet.
- [x] It is stateless and permits or denies inbound traffic based on the rules for outbound traffic.

Alt ağlar için NACL'ler durumsuzdur ve izin verilen gelen trafiğe, giden trafik için belirlenmiş kurallara göre yanıt verir. Güvenlik grupları ise durumsaldır ve yalnızca örnek düzeyinde uygulanabilir, alt ağ düzeyinde değil.

---

**What task do AWS content delivery networks (CDNs) accomplish?**

- [ ] Unlimited amounts of data storage, regardless of the variety of data types
- [ ] Long-term, durable storage for each specific virtual machine in AWS
- [ ] Transferring petabytes of data via physical hard drive to the desired storage site
- [x] Improved response times for customers accessing your data online from numerous, distant geographic locations

İçerik dağıtım ağları (content delivery networks), içeriği ve diğer uygulama verilerini kullanıcılara yakın depolayarak uygulama yanıt sürelerini iyileştirir. Büyük bir coğrafi alana sahip web uygulamalarının yanı sıra video akışı veya yazılım indirmeleri gibi yüksek hacimli büyük veri dosyalarıyla ilgilenen web uygulamaları tarafından sıklıkla kullanılır.

---

**Identify a true statement about route tables.**

- [x] A subnet can only be associated with one route table at a time, but you can associate multiple subnets with the same route table.
- [ ] A subnet can be associated with multiple route tables at a time, but you can associate only one subnet for each route table.
- [ ] A subnet can be associated with multiple route tables at a time, but you cannot associate multiple subnets for each route table.
- [ ] None of these are true.

Bir alt ağ aynı anda yalnızca bir yönlendirme tablosuyla ilişkilendirilebilir ancak birden fazla alt ağı aynı yönlendirme tablosuyla ilişkilendirebilirsiniz.

---

**Which API Gateway protocol creates a persistent connection between an application's backend and a client, and is ideal for chat applications or real-time streaming services?**

- [ ] REST APIs
- [ ] HTTP APIs
- [x] WebSocker APIs
- [ ] Private APIs

WebSocket API, arka uç ile istemci arasında kalıcı bir bağlantı sağlar. Buna Çift Yönlü İletişim (Duplex Communication) denir. Bu, bir WebSocket API'nin uygulamalar arasında gerçek zamanlı iletişim oluşturmak için ideal olduğu anlamına gelir. Bu, Sohbet Uygulamaları veya akış hizmetlerini içerebilir. Bu sayede sunucuları yönetmek zorunda kalmadan gerçek zamanlı iletişim uygulamaları oluşturabilirsiniz.

---

**What are the basic benefits of creating more than one subnet within a VPC? (Choose 3 answers)**

- [x] Network segmentation
- [x] Security
- [x] High availability
- [ ] Reduced latency

VPC'nizi alt ağlara bölmek, VPC kaynaklarınızı mantıksal olarak organize etmenizi, bu daha düzenli kaynak grupları için ideal güvenlik ayarlarını yapılandırmanızı sağlar ve ayrıca daha yüksek erişilebilirlik sunar çünkü bir alt ağ iki farklı kullanılabilirlik bölgesine yayılamaz.

---

**In which case does AWS recommend selecting a Network Load Balancer?**

- [ ] When an application requires load balancing of HTTP requests
- [x] When an application requires load balancing of TCP or UDP protocol traffic
- [ ] When an application requires the use of a third-party virtual appliance
- [ ] Network Load Balancers are not recommended because AWS retired the related network

Elastic Load Balancing (ELB) dört tür yük dengeleyiciyi destekler. Uygulama ihtiyaçlarınıza göre uygun yük dengeleyiciyi seçebilirsiniz. HTTP isteklerini dengelemek için Application Load Balancer (ALB) kullanmanızı öneririz. Ağ/taşıma protokolleri (katman 4 – TCP, UDP) için yük dengeleme ve yüksek performans/düşük gecikme süreli uygulamalar için Network Load Balancer kullanmanızı önerilir. Uygulamanız Amazon Elastic Compute Cloud (Amazon EC2) Classic ağı içinde inşa edilmişse, Classic Load Balancer kullanmalısınız. Üçüncü taraf sanal cihazları dağıtmanız ve çalıştırmanız gerekiyorsa, Gateway Load Balancer kullanabilirsiniz.

---

**What action does a listener perform as a part of the Elastic Load Balancing service?**

- [x] It checks for connection requests.
- [ ] It launches Amazon Elastic Compute Cloud (Amazon EC2) instances.
- [ ] It removes unhealthy instances.
- [ ] It sets the default connection timeout.

Dinleyici yani listener, bağlantı isteklerini kontrol eden bir süreçtir. Ön uç (istemciden yük dengeleyiciye) bağlantılar için bir protokol ve bir bağlantı noktasıyla ve arka uç (yük dengeleyiciden arka uç instance'ına) bağlantılar için bir protokol ve bir bağlantı noktasıyla yapılandırılmıştır.

---

**Which record type for DNS is used to map a hostname to another hostname?**

- [ ] Name Server
- [x] CNAME
- [ ] TEXT
- [ ] MX

Bir host adını başka bir hostadıyla eşlemek için canonical name yani CNAME kaydı kullanılır.

----

**Network Access Control Lists (NACLs) are _______.**

- [x] stateless
- [ ] stateful
- [ ] synchronous
- [ ] asynchronous

Network ACL'leri durum bilgisine sahip değildir yani stateless'dır. İzin verilen gelen trafiğe verilen yanıtlar, giden trafiğe ilişkin kurallara tabidir (ve bunun tersi de geçerlidir).

---

**Which API Gateway Endpoint type for APIs would serve customers in a localized area who connect over the public internet, and as a result may not need a content delivery network solution at all?**

- [ ] Edge-Optimized API endpoint
- [x] Regional API endpoint
- [ ] Private API endpoint
- [ ] Regional or Private API endpoints would meet this requirement.

Regional API endpoint, kendi içerik dağıtım ağınızı (CDN), yani CloudFront dışındaki bir hizmeti kullanmayı planlıyorsanız iyi çalışır. Ayrıca, AWS dışı bir CDN kullanmak istiyorsanız veya hiç CDN kullanmak istemiyorsanız bu doğru seçenektir. Genel olarak, tüm API çağrılarınızın tek bir AWS Bölgesi içinden gelmesini bekliyorsanız, bu uç nokta iyi çalışır. Bu durum, API'nizin yalnızca belirli bir bölgede var olan diğer uygulamalar veya kendi hizmetleriniz tarafından kullanılması durumunda meydana gelebilir.

---

**What can you manage using routing integrated with health checks and application component verification?**

- [x] failovers
- [ ] elastic load balancers
- [ ] routing policy
- [ ] nodes

Amazon Route 53'ün uygulama kurtarma denetleyicisi özelliği, yönlendirmeyi sağlık kontrolleri ve uygulama bileşeni doğrulaması ile entegre ederek yedeklemeleri yönetmenizi sağlar.




























