====  My Jenkins Master  =====

sudo apt update

sudo apt upgrade -y

clear

<hr/>
-->sudo yonetici hakki almak icindeir. nano, yazi yazmak
sudo nano /etc/hostname

My-Jenkins-Master  
Bu ismi makineye ver.

Ctrl + x 'e bas.
Onaylamak için Y harfine bas.
En sonda ise Enter'a bas.


Makineyi yeniden başlat.

sudo init 6
sudo reboot
->
<hr/>

AWS web tarayıcısından Security groups'a gidip
8080 portunu dış dünyaya aç.

<hr/>


Java'yı kuracağız Jenkins için.

java -version

sudo apt install openjdk-21-jre -y

java --version



<hr/>


Jenkis'i kuracağız.
https://www.jenkins.io/doc/book/installing/linux/

<hr/>


sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
https://pkg.jenkins.io/debian binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins  -y

<hr/>


sudo systemctl enable jenkins

sudo systemctl start jenkins

sudo systemctl status jenkins

<hr/>


http://My-Jenkins-Master_makinesinin_public_IPsi_Public_IPv4_address:8080/


<hr/>

Jenkins'in admin parolası buradadır.
-->cat, okuma yapar.
sudo cat  /var/lib/jenkins/secrets/initialAdminPassword


<hr/>



====  My Jenkins Agent  =====

sudo apt update

sudo apt upgrade -y

clear

<hr/>


sudo nano /etc/hostname

My-Jenkins-Agent

Bu ismi makineye ver.


Ctrl + x 'e bas.
Onaylamak için Y harfine bas.
En sonda ise Enter'a bas.


Bu komutla da isimlendirmeyi yapabiliriz.

hostnamectl set-hostname My-Jenkins-Agent


<hr/>

Makineyi yeniden başlat.

sudo init 6         OR      sudo reboot

<hr/>

Java'yı kuracağız Jenkins için.

sudo apt install openjdk-21-jre -y

java --version

<hr/>

Docker'ı kuracağız.

sudo apt  install docker.io  -y

OR

sudo apt-get  install docker.io  -y

sudo usermod -aG docker $USER

sudo reboot


<hr/>





My Jenkins Agent için
--> bazi configleri degistirmek icindir. iki makinayi birbirine baglamak icin
-->master i takip eden agent olacak ve masterin dediklerini yapacak
sudo nano /etc/ssh/sshd_config

<hr/>

Bunların önündeki # sembolü kaldır.

PubkeyAuthentication yes

AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2

<hr/>

Ctrl + x 'e bas.
Onaylamak için Y harfine bas.
En sonda ise Enter'a bas.


sudo service ssh reload


<hr/>





My Jenkins Master için

sudo nano /etc/ssh/sshd_config


Bunların önündeki # sembolü kaldır.

PubkeyAuthentication yes

AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2


Ctrl + x 'e bas.
Onaylamak için Y harfine bas.
En sonda ise Enter'a bas.


sudo service ssh reload

--> ssh sifre uretme ve anahtar olusturma islemi
ssh-keygen

OR

ssh-keygen -t ed25519
--> sonra ll yaptigin zaman icerikler gozukuyor renkli olanlar folder digerleri file

Master makinedeki id_ed25519.pub dosyasına çift tıkla
içindekini kopyala

Agent makinedeki authorized_keys dosyasına çift tıkla
bunun içine yapıştır ve kaydet
--> sonuc asagida
ubuntu@My-Jenkins-Agent:~$ cd .ssh
ubuntu@My-Jenkins-Agent:~/.ssh$ cat authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCslhapU/8syVRLrVjXkX2DkCz+I33QqFua5zupNVS+7ijfv85EAlqzUiU8/nJkkeN7cS8WWWjp86TH+kwz96/goB1BPhLxlxWcqTkVUK01lwaMR9zxHinRtQWBARB7J2FxETcPlUW3cLC8A/C079S+do5iLR1hS16GXoPJTk7I9JtpiTW++l8E6GAc9m57/WUrIgDv1C/E3aLpr3gPxYL7N6CrgWVN8fGHnAZC/KwcJVbrtVQV3A4Yj64kh8mTHk2wtL1UdxE4wmEKsR0OCuNuwA35jsgxi7c8pl4VCkbBsz/ELSWUkqi3USC7aHAmdhvdEgJAvh5c0WRzm+0CzkTr My-Key-Pair-2024

ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIH0xBFw59G2HlXr6ELtuVECfGPzTP3eQpN45+0uo/U8/ ubuntu@My-Jenkins-Master
ubuntu@My-Jenkins-Agent:~/.ssh$


Master ve Agent makinelerini yeniden başlat.

sudo reboot

<hr/>


Master makinede
Jenkins'in admin parolası buradadır.

sudo cat  /var/lib/jenkins/secrets/initialAdminPassword


<hr/>
--> daha once iki makineyi birbbirine baglamistik. 
--> Master in hash kodunu agent a verdigim icin artik birbirleri ile anlasabiliyorlar

Agent makineyi node olarak Jenkins Master'a ekliyoruz.
-->  ssh keylerden id_ed25519 iicndeki BEGIN OPENSSH PRIVATE KEY kopyalayip
--> Jenkins Credentials Provider: Jenkins de Private Key Enter directly yapistiriyoruz
Dashboard ->  Manage Jenkins -> Nodes  -> New Node add node


<hr/>

Bu adresi aç.
https://github.com/settings/tokens

GitHub Token oluştur.

<hr/>

MyGitHubTokenForAWS2024
ghp_123456789


Token'ı Jenkins'e tanıt.

Dashboard -> Manage Jenkins -> Credentials



<hr/>

====  My SonarQube  =====

sudo apt update

sudo apt upgrade -y

clear




sudo nano /etc/hostname

My-SonarQube  
Bu ismi makineye ver.

Ctrl + x 'e bas.
Onaylamak için Y harfine bas.
En sonda ise Enter'a bas.


Bu komutla da isimlendirmeyi yapabiliriz.

hostnamectl set-hostname My-SonarQube


Makineyi yeniden başlat.

sudo init 6

OR

sudo reboot



<hr/>


PostgreSQL kurulumu

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null


sudo apt update

sudo apt-get -y install postgresql postgresql-contrib

sudo systemctl enable postgresql


sudo passwd postgres

parola: 123456789



su - postgres

parola: 123456789

createuser sonar

psql

ALTER USER sonar WITH ENCRYPTED password 'sonar';

CREATE DATABASE sonarqube OWNER sonar;

grant all privileges on DATABASE sonarqube to sonar;

\q

exit





<hr/>

-->Linux un icindeki uygulamalari indirmek icin google store benziyor
Adoptium repository
--> kok dizine gider root a gider
sudo bash

wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc

echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list





<hr/>
--> https://adoptium.net/de/marketplace/ adresini ziyaret edebilirsin
Adoptium repository

sudo bash

wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc

echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list


sudo apt update

sudo apt install openjdk-17-jre -y

OR

sudo apt install temurin-17-jdk -y


sudo update-alternatives --config java

java --version
-->burda java 21 calismiyor 17 yi seciyoruz


<hr/>

Linux kernel

sudo vim /etc/security/limits.conf

Bir şey eklemek için önce klavyeden i tuşuna bas.

sonarqube   -   nofile   65536
sonarqube   -   nproc    4096


çıkış için ESC tuşuna bas.
:wq  yaz



<hr/>

sudo vim /etc/sysctl.conf

Bir şey eklemek için önce klavyeden i tuşuna bas.
Eklenecek bilgi aşağıdaki satır.

vm.max_map_count = 262144


Çıkış için ESC tuşuna bas.
:wq  yaz
-->enter a bas
<hr/>

Makineyi yeniden başlat.

sudo init 6

OR

sudo reboot

<hr/>


Sonarqube kurulumu

sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.6.0.92116.zip

sudo apt install unzip

sudo unzip sonarqube-10.6.0.92116.zip -d/opt

pwd

sudo mv   /opt/sonarqube-10.6.0.92116    /opt/sonarqube


<hr/>

sonar kullanıcı oluşturulacak ve haklar verilecek


sudo groupadd sonar

sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar

sudo chown sonar:sonar /opt/sonarqube -R

<hr/>

veritabanıyla bu kullanıcıyı konuştur

sudo vim /opt/sonarqube/conf/sonar.properties

sonar.jdbc.username=sonar
sonar.jdbc.password=sonar

sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube



<hr/>




Sonar servisini oluşturacağız.

sudo vim /etc/systemd/system/sonar.service
--> burasi vim cikis icin, ESC :wq enter ile cikis yapilir
<hr/>
Aşağıdaki kodları olduğu gibi bu dosyanın içine yapıştır.

<hr/>

[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target


<hr/>

Makine açıldığında sonarqube otomatik olarak çalıştırma komutları

sudo systemctl enable sonar

sudo systemctl start sonar

sudo systemctl status sonar

<hr/>
Log takibi

sudo tail -f /opt/sonarqube/logs/sonar.log

<hr/>

Makinenin public ip değerini al ve 9000 portundan giriş yap.
kullanıcı: admin
parola: admin
-->123456789

<hr/>

Jenkins için token oluştur.

Administrator  -> Security

http://MAKINENIN_PUBLIC_IP_DEGERI:9000/account/security

<hr/>

jenkins-sonarqube-token
sqa_123456789


Jenkins içinde tokenı kaydettir.

Pluginleri kur.

Sonar'ın kurulduğu makinenin Private IPv4 addresses değerini kopayla.

<hr/>

Docker Hub Token oluştur.

docker login -u YOUR_USERNAME  -p  dckr_pat_123456789


Jenkinse DockerHub Token'ı tanıt ekle.

<hr/>

Agent makinesi zamanla dolacak. Docker şişecek dolacak. Temizlik yapmanız lazım.
Agent makinede temizlik için yeriniz azalmışsa şu komutları kulanın lütfen.

docker rmi $(docker images --format '{{.Repository}}:{{.Tag}}' | grep 'devops-003-pipeline-aws')

docker container rm -f $(docker container ls -aq)

docker volume prune


====  My EKS (Elastic Kubernetes Service) Server  =====

sudo apt update

sudo apt upgrade -y

clear



sudo nano /etc/hostname

My-EKS-Bootstrap-Server  
Bu ismi makineye ver.

Ctrl + x 'e bas.
Onaylamak için Y harfine bas.
En sonda ise Enter'a bas.


Makineyi yeniden başlat.

sudo init 6
sudo reboot





### AWS CLI kurulumu


WEB PAGE
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#cliv2-linux-install

sudo su

pwd


curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

apt install unzip

unzip awscliv2.zip

sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update







### Kubectl kurulumu

sudo su

pwd

WEB PAGE  
https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.30.4/2024-09-11/bin/linux/amd64/kubectl


ll

chmod +x ./kubectl

ll


mv kubectl /bin


export PATH=$HOME/bin:$PATH

kubectl version --output=yaml





### eksctl

sudo su

pwd


curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

cd /tmp

ll

sudo mv /tmp/eksctl /bin

eksctl version







sudo reboot

kubectl version --client
--> bu kodu calsitirip eksctl in kurulu oldugunun gormeden asagidaki kodlari calistirmayin



### 3 node yapılacak

eksctl create cluster --name my-workspace-cluster4 \
--region us-east-1 \
--node-type t2.small \
--nodes 3

-->silmek icin
kubectl delete cluster my-workspace-cluster



kubectl get nodes

kubectl get pods
kubectl get pod
kubectl get po




### ArgoCD
--> ArgoCd yi kubernetes makinesine kuruyoruz


WEB PAGE
https://argo-cd.readthedocs.io/en/stable/#quick-start

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get pods -n argocd


Yönetici moduna geç.

sudo su

pwd


ArgoCD sürümünü çekeceğiz.


Son sürümü çekmek için bu 3 satır gerekli.

curl -L -s https://raw.githubusercontent.com/argoproj/argo-cd/stable/VERSION

VERSION=$(curl -L -s https://raw.githubusercontent.com/argoproj/argo-cd/stable/VERSION)

curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v$VERSION/argocd-linux-amd64



OR


Sürümün elle yazılmışı

curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.12.3/argocd-linux-amd64


chmod +x /usr/local/bin/argocd



Normal kullanıcı moduna geç

Ctrl+C

OR

Terminale bunu yaz.

exit



kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

kubectl get svc -n argocd

ArgoCD dış dünyaya böyle bir adres ile açlılıyor.
a5b3d196d6343444dbd692184429ca6b-117814533.us-east-1.elb.amazonaws.com


Admin şifresini almak bu komutu kullan

kubectl get secret argocd-initial-admin-secret -n argocd -o yaml


echo     dEJXSzJuVktYY2IxVVpaYw==  | base64 --decode


Size böyle bir sonuç verecek.
tBWK2nVKXcb1UZZcubuntu@My-EKS-Bootstrap-Server:~$

Admin şifresi budur.

tBWK2nVKXcb1UZZc


Giriş yap ve "User Info" menüsünden parolayı hemen güncelle.




Terminalden ArgoCD kullanımı

argocd login a5b3d196d6343444dbd692184429ca6b-117814533.us-east-1.elb.amazonaws.com   --username admin

argocd cluster list




Çalışılan K8s cluster adını alıyoruz.
kubectl config get-contexts


K8s cluster adını ArgoCD'ye veriyoruz.
argocd cluster add i-019044c0a004f6c9f@my-workspace3-cluster.us-east-1.eksctl.io --name my-workspace3-cluster



kubectl get svc



MyGitHubToken
ghp_123456789123456789123456789



JENKINS_API_TOKEN
123456789123456789123456789



minikube service my-application-service --url


kubectl get service

http://a72903a0997564a328c47bc910f75aa8-253506306.us-east-1.elb.amazonaws.com:8083




Agent makinesi zamanla dolacak. Docker şişecek dolacak. Temizlik yapmanız lazım.
Agent makinede temizlik için yeriniz azalmışsa şu komutları kulanın lütfen


docker rmi $(docker images --format '{{.Repository}}:{{.Tag}}' | grep 'devops-003-pipeline-aws')

docker container rm -f $(docker container ls -aq)

docker volume prune




EKS nodelarını silme

https://docs.aws.amazon.com/eks/latest/userguide/delete-cluster.html

eksctl version

Çalışan tüm servisleri listele
kubectl get svc --all-namespaces

Sadece bir servisi silmek için bu komutu kullanıyoruz ama servisleri tek tek silmeyeceğiz.
kubectl delete svc SERVISIN_ADI


Cluster adını ve detaylarını görmek için
kubectl config view


export AWS_DEFAULT_REGION=us-east-1


Nodeları ve servislerin hepsini cluster adını vererek toplu halde sileceğiz.
eksctl delete cluster --name my-workspace3-cluster


