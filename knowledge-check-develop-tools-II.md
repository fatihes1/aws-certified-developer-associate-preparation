# Ölçme Testi II: Developer Tools (DVA-C02)

**A developer is configuring a two-stage pipeline in AWS CodePipeline to deploy a web application on an Amazon EC2 instance.**
**The source stage is a CodeCommit repository and the Deploy Stage is a CodeDeploy application and deployment group. All necessary IAM roles and permissions are correctly configured.**
**What additional step is required to successfully complete the CodePipeline configuration?**

- [ ] Add a Test Stage to the AWS CodePipeline.
- [ ] Configure an AWS CodeBuild project to move the web application code from AWS CodeCommit and AWS CodeDeploy.
- [ ] Add a Build Stage to the AWS CodePipeline.
- [x] Install the CodeDeploy Agent in the EC2 instance.

Bu pipeline'ın düzgün çalışması için, web uygulamasının dağıtılacağı EC2 instance'ına CodeDeploy Agent'ı yüklemeniz gerekir. CodeDeploy Agent, bu durumda EC2 instance'ının bir dağıtım hedefi olarak kullanılmasını sağlayan ve AWS CodeDeploy tarafından bir dağıtım işlemi gerçekleştirildiğinde kullanılacak bir dizi işlevsellik içeren bir yazılım paketidir.

---

**Which of the following can provide continuous deployment? (Choose 3 answers)**

- [x] Puppet
- [x] Chef
- [x] Docker
- [ ] Terraform

Puppet, Chef ve Docker, hepsi Continuous Deployment sağlayan araçlardır. Terraform, altyapı oluşturmak için kullanılan bir dildir (Infrastructure as Code).

---

**Which of the following is the best and most complete definition of continuous integration?**

- [ ] a practice in which developers continuously meet and integrate their ideas to promote a shared understanding of the system under development
- [ ] a practice in which developers continuously deliver new versions of a system to users
- [ ] a practice in which developers continuously comment their code with insightful explanations about design decisions
- [x] a practice in which developers continuously integrate their code changes with the current code in source control, and then build and test each commit

Continuous integration, insanlar ve yazılım tarafından desteklenen bir uygulamadır. Bu uygulama, geliştiricilerin kod değişikliklerini sürekli olarak kaynak kontroldeki mevcut kodla entegre etmelerini sağlar. Continuous integration sürecinin bir parçası, her şeyin doğru çalıştığını doğrulamak için her commit'i oluşturmak ve test etmektir.

---

**If you have a CICD Pipeline that needs a Unit testing step added to it, which one of these AWS services can help with that?**

- [ ] It's a built-in feature of AWS CodePipeline.
- [x] AWS CodeBuild.
- [ ] AWS CodeDeploy.
- [ ] AWS CodeCommit.

CodeBuild bu harika araç pipeline'ınızın ihtiyaç duyabileceği her türlü hesaplama ile ilgili görevi çalıştırmanıza yardımcı olabilir - uygulamanız için unit test'ler dahil!

---

**You have finished configuring a new CodePipeline pipeline. It consists of three stages, a Source stage, Build stage, and Deploy stage.**
**Given these pipeline characteristics, which of the following statements are known to be true? (Choose 2 answers)**

- [ ] The pipeline has three transitions.
- [x] The pipeline has two transitions.
- [x] All transitions are enabled by default.
- [ ] The pipeline has four transitions.

"The pipeline has two transitions." ve "All transitions are enabled by default." cevapları doğrudur. Geçişler, pipeline aşamaları arasındaki devre dışı bırakılabilen veya etkinleştirilebilen bağlantılardır. Varsayılan olarak etkindir. Devre dışı bırakılmış bir geçişi yeniden etkinleştirdiğinizde, otuz günden fazla zaman geçmemişse en son revizyon pipeline'ın kalan aşamalarından geçecektir. Otuz günden fazla devre dışı bırakılmış bir geçiş için, yeni bir değişiklik tespit edilmedikçe veya pipeline'ı manuel olarak yeniden çalıştırmadıkça pipeline yürütmesi devam etmeyecektir.

---

**In terms of version control, code development is often thought about or referred to as which of the following?**

- [ ] Rivers
- [x] Trees
- [ ] Flow
- [ ] Highway and Streets

Versiyon kontrolünden bahsederken, sıklıkla ağaç (Trees) benzetmesi kullanılır.

---

**Which of these is a valid source for AWS CodePipeline? Select two choices. **

- [ ] AWS CloudFormation
- [x] AWS Elastic Container Registry (ECR)
- [x] GitLab
- [ ] Circle CI

AWS CodePipeline, ECR'ye yeni bir container image gönderdiğinizi tespit edebilir ve buna dayalı olarak uygun eylem veya eylemler alabilir.

---

**Mutable and immutable servers each have their own benefits. What benefits do mutable servers offer? (Choose 2 answers)**

- [x] low overhead
- [x] the ability to rollback changes through version control
- [ ] the ability to use cloud platform scaling features with minimal application redesign
- [ ] no additional testing required for operating system level changes

Mutable server'lar düşük yük sunar ve versiyon kontrolü sürdürüldüğü sürece değişiklikleri geri alma yeteneği sağlar. Ad-hoc komutlar mutable server'larda nispeten basittir ve mevcut çok çeşitli konfigürasyon yönetimi araçlarıyla mutable server'lar nispeten kolay yönetilebilir. Immutable server'lar genellikle cloud ölçeklendirme özellikleriyle çalışma yeteneği sunarken, mutable server'lar çoğunlukla sunmaz. Immutable server'larda, OS seviyesindeki değişiklikler için ek test gerekmezken, mutable server'larda bu gereklidir.

---

**What is continuous delivery?**

- [ ] Continuously meeting customers to ensure a product meets their needs
- [x] Building production-ready software to be deployed at any time
- [ ] Steadily update code in source control with a process of builds and tests
- [ ] Steadily update code in source control with a process of builds and tests

Continuous delivery'nin revize edilmiş tanımımız şudur: Continuous delivery, yazılımı istediğiniz zaman belirli bir ortama dağıtılabilecek ve yalnızca en yüksek kaliteli sürümleri üretime dağıtacak şekilde oluşturmanın bir yoludur.

---

**For CodeBuild to properly use your Buildspec file, what has to be true? **

- [ ] The file must be named appsepc.yml and placed in the root directory 
- [x] The file must be named buildspec.yml and placed in the root directory
- [ ] The file must be named buildspec.yml and placed in a folder called Buildspec/
- [ ] The file must be named appspec.yml and placed in a folder called Buildspec/

CodeBuild'in Buildspec dosyanızı düzgün kullanması için, önce dosyayı buildspec.yml olarak adlandırmanız ve ardından kaynak kodunuzun kök dizinine yerleştirmeniz gerekir.

----

**_____** **are responsible for determining whether your classes, functions, and methods behave the way they're expected to.**

- [ ] Linters
- [x] Unit tests
- [ ] Integration tests
- [ ] System tests

Unit test'ler, kodun tek bir birimini test ettikleri için en ayrıntılı test düzeyidir. Sınıflarınızın, fonksiyonlarınızın ve metodlarınızın beklendiği gibi davranıp davranmadığını belirlemekten sorumludurlar.


----

**The opposite of a microservices architecture is a(n) _____ architecture.**

- [ ] event-based
- [ ] distributed
- [ ] data-centric
- [x] monolithic

Yazılım endüstrisi uzun yıllar boyunca her özelliğin sıkı bir şekilde birbirine bağlı olduğu büyük, monolitik uygulamalar oluşturuyordu. Sistemleri bir kerede oluşturmak ve bunları ayrı bileşenler yerine bir birim olarak inşa etmek doğaldı. Bugünlerde bu özellikleri ayrı servislere ayırmak istiyoruz. Monolitik uygulamamızı daha küçük uygulamalara veya yaygın olarak adlandırıldığı şekliyle microservice'lere ayırmak istiyoruz.


----

**Which choice best describes a rolling deployment strategy?**

- [ ] You deploy the latest version of your software into a production environment, have a router select a group of users to route to it, and see how it behaves before deciding to roll it out further
- [ ] You have two environments and a routing mechanism to choose which one is live, based on which one it sends traffic to.
- [x] The application is gradually deployed to the machines one at a time or in batches.
- [ ] You turn all servers offline, install any updates, and turn all servers back on.

Rolling deployment'ta, yeni kodu gruplar halinde bir veya daha fazla sunucuya dağıtırsınız. Bunu zaten yapmış olabilirsiniz, bu özellikle egzotik bir dağıtım yöntemi değildir, ancak normalde dağıtım yapabileceğiniz yöntemden farklıdır; normalde bir sunucuyu kapatıp üzerine yeni kod koyup tekrar açarsınız, ancak burada işleri AutoScaling launch configuration kullanarak yapıyoruz.

---

**Which statement best describes a canary deployment strategy?**

- [x] You deploy the latest version of your software into a production environment, have a router select a group of users to route to it, and see how it behaves before making the decision to roll it out further.
- [ ] You have two environments and a routing mechanism to choose which one is live, based on which one it sends traffic to.
- [ ] The application is gradually deployed to the machines one at a time or in batches.
- [ ] You do a partial deployment with only specific artifact versions.

Canary deployment'larda, yazılımınızın en son sürümünü bir production ortamına dağıtırsınız ve bir router'ın seçtiği bir grup kullanıcıyı buna yönlendirirsiniz. Daha fazla yaygınlaştırma veya tamamen kaldırma kararını vermeden önce nasıl davrandığını görebilirsiniz.

----

**Which of the following statements about immutable servers is true?**

- [ ] Operating system changes require additional testing.
- [ ] Scaling is usually a long and complicated process.
- [x] They can use deployment and scaling features available via cloud platforms.
- [ ] They can be useful for small projects or small teams that don't want the extra overhead of managing virtual machine images.

Immutable server'ların avantajlarından biri, genellikle cloud platformumuzla birlikte gelen dağıtım ve ölçeklendirme özelliklerini kullanabilmemizdir, örneğin AWS ile auto-scaling gibi.









