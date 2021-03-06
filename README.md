
## Dockerizing Laravel Application With Nginx,Mysql on Ubuntu

### Technologies used;

- Laravel
- Docker for containerization
- Mysql
- HTML5,CSS3,Javascript

<img src="./images/laravel-docker.png" width="350" height="200"/>

Bir laravel projesini sanallaştırma teknolojilerini kullanarak nasıl ayağa kaldırırız?

Bu projede bir tane docker composer oluşturup bir container'da web sunucusu olarak ngnix, bir başka container'da db olarak mysql, bir başka container'da da uygulamamız'ın koşmasını hedefliyoruz.Sonuç olarak projemizde docker composer kullanarak toplam 3 tane ayrı container kullanmış olacağız ve bunlar bir birleri ile etkilşim halinde olmuş olacaklar.

## 1-Laravel Kütüphanesini Projeye İmport Edelim

<img src="./images/laravel-logo.jpg" width="350" height="200"/>

Aşağıdaki komut satırı ile laravel kütüphanesini Projemize clone edelim.

    - $ git clone https://github.com/laravel/laravel.git Dockerizing-laravel-App

## 2-Docker Composer'ı Projemize Kuralım

Composer'ı kullanabilmek için projemizin ana dizininde kurulumu yapalım.
    
    - $ cd ~/laravel-app
    
    - $ docker run --rm -v $(pwd):/app composer install
    
## 3-Docker Compose File Oluşturalım.

Projemizde kullacağımız image 'leri tanımlamak için Docker-Compose dosyası oluşturalım.

    - $ nano ~/laravel-app/docker-compose.yml

## 4-Docker Compose Dosyasını Projemizin Niteliklerine Göre Düzenleyelim.

Projemizde kullanmamız gereken 3 tane container var.Bu container'ları oluşturucağımız docker-compose.yml dosyasında tanımlayıp sonra build edeceğiz.Eğer bir den fazla container kullanmak istiyorsak compose kullanılmalıdır.Eğer sadece bir tane container kullacaksak Dockerfile da tanımlamaları yapmak yeterli olacaktır.

## 5- Docker File dosyasını OLuşturalım.

<img src="./images/docker-file.jpeg" width="100" height="100"/>

Docker ayağa kalkacağı zaman proje path'inin belirlenmesi ,sistem paketlerinin güncellenmesi,Composer'ın kurulumu,cache 'in temizlenmesi projedeki dosyaların root izni olmadan yürütülmesi için gerekli tanımlamaların ve yetkilerin verildiği dosyadır.

![Docker](images/docker-build.png)

Aslında Dockerfile içersinde oluşturduğum her bir component(mysql,apache,ngnix..) Dockerfile derlendikten sonra adı,id'si gibi özellikleri olan birer image 'dir.

## 6- Projemize php klasörü oluşturalım.

<img src="./images/php-file-logo.png" width="100" height="100"/>

- $ mkdir ~/Dockerizing-Laravel-App-On-Ubuntu/php

Proje dizinine dosya upload etmek için izin verilen maximum boyutlar set edilmiştir.

- nano ~/laravel-app/php/local.ini

    upload_max_filesize=40M;
    post_max_size=40M

## 7- Nginx Web Server Yapılandırma

<img src="./images/nginx.png" width="100" height="100"/>

İlk önce /nginx/conf.d dizinini oluşturalım.

    - $ mkdir -p ~/Dockerizing-Laravel-App-On-Ubuntu/nginx/conf.d

Bu dizinin altına nginx web server'ı yapılandırmak için app.conf dosyasını oluşturdum.

    - $ nano ~/laravel-app/nginx/conf.d/app.conf

## 8- Mysql Yapılandırma

<img src="./images/mysql.jpg" width="350" height="200"/>

Mysql dizinini oluşturalım.

    - $ mkdir ~/laravel-app/mysql

Bu dizinin altına mysql'i yapılandırmak my.cnf dosyasını oluşturalım.

## 9- Dockerfile build edelim.

![Docker](images/docker-build-composer.png)

Eğer projenizde docker-compose 'ı ilk kez build ediyorsanız bu biraz zaman alıcaktır.

    - $ docker-compose up -d

## 10- Projemizde Oluşturduğumuz containerlar'ı ve image'leri listeleyelim.

![Docker](images/docker-container.png)

    - $ docker container list //Containerları listeleme

    - $ docker image list  //İmageleri listeleme

## 11- Uygulamamızı çalıştırıp kontrol ediyoruz.

Uygulamamız oluşturmuş olduğumuz 3 tane container ile etkileşim halinde çalışıyor.

![Docker](images/laravel-localhost.png)

## 12- Oluşturduğumuz Container'larda root olma.

Oluşturuduğumuz her container'ın birer ip adresi vardır.Bu ip adresiyle birlikte sannallaştırdığımız sunuculara 22 portu ile ssh bağlantısı kurabiliriz.Ancak bu noktada bir sanal switch oluşturulup bu sanal switch üzerinden ssh yapılmalıdır.

Sunucuda root iken containerlara aşağıdaki komut satırı ile root olabiliriz.

    - $ docker-compose exec db bash

Burada db sanal sunucuların isimleri aslında.Eğer sadece uygulamanın koştuğu sanal sunucuya bağlanmak istersek 

    - $ doceker-compose exec app bash

yazmamız yeterli olacaktır.






















 
