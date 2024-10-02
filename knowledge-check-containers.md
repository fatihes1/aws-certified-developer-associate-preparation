# Ölçme Testi: Containers (DVA-C02)

**Which of the following use cases are appropriate for AWS Copilot? (Choose 3 answers)**

- [x] Develop and release production-ready containerized applications on ECS and Fargate 
- [x] Stand up required infrastructure to host containerized applications 
- [x] Troubleshoot containerized services by checking health logs
- [ ] View suggestions on how to containerize monolithic applications that are currently hosted on EC2

AWS Copilot, containerized uygulamalar oluşturmak ve yönetmek için kullanılabilen bir CLI aracıdır. Monolitik uygulamaları containerize etmek için öneriler üreten akıllı bir araç değildir.

---

**What is a node group in Amazon Elastic Kubernetes Service?**

- [ ] An IAM access point that is configured within the Control Plane
- [ ] An endpoint used for connecting to EKS
- [x] A group of EC2 instances that include software to run containers
- [ ] A cluster of AMIs within an EKS configuration map

Bir node group, bir Amazon EC2 Auto Scaling grubunda dağıtılan bir veya daha fazla Amazon EC2 instance'ıdır. Bir node group'taki tüm instance'lar şu özelliklere sahip olmalıdır: Aynı instance türü olmalı Aynı Amazon Machine Image (AMI) üzerinde çalışmalı Aynı Amazon EKS node IAM rolünü kullanmalı

---

**Amazon EKS allows customers to deploy managed node groups on which EC2 instance purchase options? (Choose 2 answers)**

- [x] On-Demand instances
- [ ] Reserved instances
- [ ] Dedicated instances
- [x] Spot instances

Managed bir node group oluştururken, On-Demand veya Spot kapasite türünü seçebilirsiniz. Amazon EKS, yalnızca On-Demand veya yalnızca Amazon EC2 Spot Instance'larını içeren bir Amazon EC2 Auto Scaling grubu ile bir managed node group dağıtır. Tek bir Kubernetes cluster'ı içinde, hata toleranslı uygulamalar için pod'ları Spot managed node group'larına ve hata toleranssız uygulamaları On-Demand node group'larına zamanLayabilirsiniz. Varsayılan olarak, bir managed node group On-Demand Amazon EC2 instance'larını dağıtır.

---

**What do ECR repository policies control? **

- [x] User access and action within ECR repositories
- [ ] User actions and access with deployed ECS containers
- [ ] Authenticate AWS users to communicate with Docker CLI
- [ ] User actions and access with AWS CodeCommit repositories

Amazon ECR repository politikaları, bireysel Amazon ECR repository'lerine erişimi kontrol etmek için kullanılan ve kapsamı belirlenmiş IAM politikalarının bir alt kümesidir. IAM politikaları genellikle tüm Amazon ECR hizmetine izinler uygulamak için kullanılır, ancak belirli kaynaklara erişimi kontrol etmek için de kullanılabilir. Hem Amazon ECR repository politikaları hem de IAM politikaları, belirli bir kullanıcının veya rolün bir repository üzerinde hangi eylemleri gerçekleştirebileceğini belirlerken kullanılır. Eğer bir kullanıcı veya role bir repository politikası aracılığıyla bir eylemi gerçekleştirme izni verilmişse ancak bir IAM politikası aracılığıyla reddedilmişse (veya tam tersi), eylem reddedilecektir.

---

**What is the relationship between clusters, nodes, and pods within Amazon EKS?**

- [x] A cluster may contain one or more nodes that host one or more pods.
- [ ] A cluster may contain one or more pods that contain one or more nodes.
- [ ] A pod may contain one or more clusters that host one or more nodes.
- [ ] A node may contain one or more clusters that host one or more pods.

Bir cluster, bir veya daha fazla node'dan oluşan bir gruptur. Bir node, bir veya daha fazla pod'u barındırabilen bir veya daha fazla instance'tan oluşan bir gruptur. Bir pod, Amazon EKS içindeki en küçük hesaplama birimidir.

---

**Which of the following statements best describes Amazon Elastic Container Service (ECS)?**

- [x] It is a highly scalable, high performance container management service that supports Docker containers.
- [ ] It is an AWS software performance management support package for EC2 instances.
- [ ] It is a cloud monitoring tool that utilizes Docker vaults for better performance.
- [ ] It is an AWS feature that allows you to contain the cost of running your EC2 instances. 

Amazon Elastic Container Service (ECS), yüksek ölçeklenebilir, yüksek performanslı bir container yönetim hizmetidir. Ayrıca, uygulamaları yönetilen bir Amazon EC2 instance'ları cluster'ında kolayca çalıştırmanıza olanak tanıyan Docker container'larını destekler.

---

**Which statement about serverless vs. server-hosted compute in Amazon ECS is correct?**

- [ ] If you have traffic that is very spiky, quickly going up and down, or just comes in at unpredictable times, and can have long periods of low or dead time, you should choose servered.
- [x] If you have a workload that can support long-running operations, that fully utilizes the CPU and memory of the underlying instances, a servered deployment is probably the best course of action cost-wise.
- [ ] Servered compute is always running at 100% effective CPU and memory utilization.
- [ ] In general, serverless compute is cheaper than servered compute in a 1 to 1 environment.

Genel olarak, serverless hesaplama, 1'e 1 ortamda servered hesaplamadan daha pahalıdır. Bunun nedeni, serverless'ın her zaman %100 etkin CPU ve bellek kullanımıyla çalışmasıdır. Eğer çok dalgalı, hızlı bir şekilde yükselip alçalan veya sadece tahmin edilemeyen zamanlarda gelen ve uzun süreli düşük veya ölü zamanları olabilen bir trafiğiniz varsa, bu serverless'ın kazandığı bir durumdur. Özetle, maliyet tarafını özetlemek gerekirse, eğer uzun süreli operasyonları destekleyebilen, altta yatan instance'ların CPU ve belleğini tam olarak kullanan bir iş yükünüz varsa, maliyet açısından muhtemelen en iyi eylem planı servered bir dağıtımdır.

---

**What is the related service Amazon ECS to help manage serverless containers?**

- [x] AWS Fargate
- [ ] AWS Copilot
- [ ] Red Hat OpenShift
- [ ] AWS Lambda

ECS, serverless container'larınızı yönetmenize yardımcı olan AWS Fargate adlı bir kardeş hizmet kullanır.



