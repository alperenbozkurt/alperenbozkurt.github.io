---
layout: post
title: "Dokku Nedir, Kurulumu ve Kullanımı"
date: 2020-01-18
excerpt: "Dokku nedir, nasıl kullanılır, neler yapılabilir, ve bir çok kullanışlı komutun bulunduğu blog yazısıdır."
tags: [dokku nedir, dokku kurulumu, dokku ile rails uygulamasının kurulumu, dokku kod çalıştırma, dokku çevre değişkenleri, dokku yedekleme, dokku geri tükleme, dokku domain ekleme, dokku alanadı ekleme, dokku alanadı değiştime, dokku ssl sertifikası ekleme, dokku komut çalıştırma]
---

Geliştirmekte olduğum [PriviaHub](https://priviahub.com/)'ın güncellemeleri benim için büyük bir zulümdü. Sürekli o uzun adımları uygulayıp en sonunda uygulamanın çalışmaması. Üzerimdeki baskı 10 katına çıkarırken, hızlı bir şekilde acaba hangi adımı atladım sorusunu düşünmeye çalışıyordum. Ve bu sürede **[PriviaHub](https://priviahub.com/)**'ın yayında olmaması da ayrı bir sorundu. Her ne kadar 2.0.10 versiyonundan sonra bu adımları hata yapmadan yapabilecek tecrübeyi edinsem de çok fazla vakit kaybediyordum. Bu adımları bir şekilde optimize etmeliydim.

Bu güncelleme adımlarını optimize etmek için bir bash script yazarken zaten **dokku**'nun bu işe yaradığı geldi aklıma. Sunucuya dokku kurarak artık güncelleme çilesinden de kurtuldum.

 **Dokku** için aldığım notlarımı hekesin yararlanması için blog yazısı haline getirdim.

## Dokku Nedir

**Dokku**, web uygulamalarınızı hızlı bir şekilde *deploy* etmeye(yayınlamaya) yarayan bir uygulamadır. Arkaplanda kullandığı **docker** uygulaması ile minimum düzeyde kaynak tüketen *container*lar oluşturarak istediğiniz kadar web uygulamasını yayınlamınıza imkan verir. 
Dokku ile sadece *git push* komutunu kullanarak saniyeler içerisinde uygulamanızı yayınlayabilirsiniz. Dokku otomatik olarak kullandığınız dili ve kullandığınız kütüphaneleri algılayıp kurulumları gerçekleştirecektir.
Gerçekten mükemmel değil mi :)

## Dokku Kurulumu
Not: sunucumun ip adresi *104.197.120.156* dır. Kodlardaki ip kısımlarını değiştirmeyi unutmayın :) 

 1. Sunucunuza bağlantı sağlayın. ``` ssh root@104.197.120.156 ```
 2. [Dokku](http://dokku.viewdocs.io/dokku/) web sitesinde yer alan son versiyon kurulum scriptini indirin ve çalıştırın. 
```
wget https://raw.githubusercontent.com/dokku/dokku/v0.19.12/bootstrap.sh
sudo DOKKU_TAG=v0.19.12 bash bootstrap.sh 
```
3. Artık sunucunuza dokku yüklenmiş olacak. Konfigrasyon için ip adresinizi web tarayıcınızdan ziyaret ediniz. Aşağıdaki gibi bir ekran ile kaşılaşacaksınız. 
![Screenshot from 2020-01-18 14-21-49](https://user-images.githubusercontent.com/19302254/72662947-ff3e3f00-39fd-11ea-8d9a-80f0eb28ffbb.png) **Public SSH Keys** yazan kısma herkese açık ssh anahtarınızı vemeniz gerekiyor. ```cat ~/.ssh/id_rsa.pub``` komutunu kendi bilgisayarınızda çalışırak public keyinizi öğrenebilirsiniz. Daha detaylı bilgi için **SSH Key** üretimini de anlattığım [git ve github ayarları](https://alperenbozkurt.net/Git-ve-github-ayarlari/) isimli blog yazımı ziyaret edebilirsiniz.
**Hostname** kısmı uygulamalarınızı yayınlayacağınız domain adresini istiyor.
**Use VirtualHost naming for apps** kısmında ise uygulamanıza subdomain ile mi yoksa port ile mi erişeleceğini seçiyorsunuz.

Dokku kurulumu bu kadardı. Kendi bilgisayarınıza **git**den başka birşey yüklemenize gerek yok. 
Not: Domaininize istek geldiğinde bu sunucudan cevap dönmesi için DNS yönlendirmelerinizi girmeyi unutmayın.

## İlk Uygulamanın Yayınlanması
Dokku kurulumunu başarıyla gerçekleştirdik. Şimdi örnek bir uygulamayı *deploy* edelim. Örnek olarak, **Ruby on Rails** ile yazdığım **[GitBuff](https://github.com/alperenbozkurt/gitbuff)** uygulamamı seçtim.

 1. **Dokku**'da uygulamamızı oluşturmamız gerekiyor. **Sunucuda** ``` dokku apps:create gitbuff ``` şeklinde uygulamamızı oluşturabiliriz. 
**Not:** Uygulamanın adı aynı zamanda uygulamanın yayınlanacağı subdomaindir. Ayrıntılı bilgi için yazının sonlarındaki Domain ekleme ve düzenleme başlığına bakabilirsiniz.
 2. Eğer uygulamanızda bir veritabanı var ise uygun veritabanı eklentisini uygulamanıza bağlamanız gerekiyor. 
```
# Postgres eklentisinin sisteme yükleyelim.
sudo dokku plugin:install https://github.com/dokku/dokku-postgres.git
# gitbuff-database isminde bir postgres container'ı oluşturalım.
dokku postgres:create gitbuff-database
# gitbuff uygulamasını gitbuff-database container'ına bağlayalım.
dokku postgres:link gitbuff-database gitbuff
```
3. Şimdi *local*'imizde(kendi bilgisayarımız) projemizin olduğu dizine girelim.
4. Projeye yeni **git remote repository**(uzak git deposu) eklememiz gerekiyor. Ekleyeceğimiz bu repo dokku kurduğumuz sunucuyu temsil edecek.
``` git remote add dokku dokku@104.197.120.156:gitbuff ```
bu komut dokku isminde bir uzak git deposunu tarif ediyor 104.197.120.156 ipsinde gitbuff isimli repo olarak açıklanabilir.
5. Artık kodları gönderebiliriz.
``` git push dokku master ```
6. Olası bir hata almanız durumunda logları şu şekilde inceleyebilirsiniz.
``` dokku logs gitbuff ``` Ayrıca `-t` parametresi ile akış modunu etkinleştirebilirsiniz. 
![Screenshot from 2020-01-18 16-12-37](https://user-images.githubusercontent.com/19302254/72664369-00776800-3a0e-11ea-89d3-ebe31d609f8c.png)
Dokku kurulumdan sonra veritabanı *migrationlarını* yapmaz. Bu yukarıdaki gibi bir hata sayfası ile karşılaşabilirsiniz. Bu hatayı çözmek için aşağıdaki app.json ile post-deploy olayını inceleyebilirsiniz.

## Pre-Deploy ve Post-Deploy

**Dokku** size *deployment* öncesinde ve sonrasında komut çalıştırabilmeniz için bir dosya atamıştır. `app.json` ismindeki bu dosyaya kurulum öncesinde ve sonrasında komut çalıştırabiliriz. Benim uygulamam **Ruby on Rails** uygulaması olduğu için kurulumdan sonra sunucuya girip manuel olarak `rails db:migrate` komutunu çalıştırmam gerekir. app.json dosyasının içerisindeki **post-deploy** anahtarı ile bu süreci otomatikleştirebiliriz.
```json
{
  "scripts": {
    "dokku": {
      "predeploy": "touch ./predeploy.test",
      "postdeploy": "bundle exec rails db:migrate"
    }
  }
}
```

`app.json` ile ilgili ayrıntılı dökümantasyona [buradan](https://devcenter.heroku.com/articles/app-json-schema) erişebilirsiniz.

*Post deploy* komutu için *app.json* dosyasını düzenledikten sonra tekrar *commit* atarsanız uygulamanın sorunsuz bir şekilde yüklendiğini göreceksiniz.

Artık sadece ``` git push dokku master `` uygulamanızı güncelleyebilirsiniz. Reponuzun adresini değiştirerek de yeni sunuculara kurulum yapabilirsiniz.

## Dockerfile Entegrasyonu

Bazı durumlarda app.json dosyası yetersiz gelebilir. Ya da tüm kurulum adımlarını kendiniz yönetmeniz gerekebilir bu gibi durumlarda projenizin ana dizinine Dockerfile isminde bir dosya oluşturabilirsiniz. 

## Environment Variables (Çevre Değişkenleri)

Uygulamanızın DATABASE_URL gibi çevre değişkenleri, uygulamalarınızı eklentilere bağladığınızda otomatik olarak kurulumda oluşturulur. Fakat ekstra çevre değişkenleriniz varsa dokku bu değişkenleri tanımlamanıza izin verir.
 ```
dokku config:keys uygulama-adi # Tüm Çevre değişkenlerini gösterir
dokku config:set uygulama-adi ADMIN_PASSWORD=secret # yeni bir çevre değişkeni oluşturur.
dokku config:get uygulama-adi ADMIN_PASSWORD # çevre değişkeninin değerini görüntüler.
```

## Yedekleme ve Geri Yükleme

Orjinal postgres eklentisi gibi eklentilerin **yedekleme** (**backup**) ve **geri yükleme** (**recovery**) işlemleri için komutları vardır.
```
dokku postgres:export gitbuff-database > backup.dump
dokku postgres:import gitbuff-database < backup.dump
```

### Amazon S3'e otomatik yedekleme

Aynı zamanda bu tarz eklentiler, verilerinizi uzak bir sunucuda yedeklemenize de imkan sağlar. Örneğin belirlediğiniz zaman aralıklarında, veritabanı yedeğinizi amazon s3 sunucunuza gönderen bir **cronjob** oluşturabilir.
```bash
dokku postgres:backup-auth gitbuff-database AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY
dokku postgres:backup-schedule gitbuff-database CRON_SCHEDULE BUCKET_NAME
```
CRON_SCHEDULE değişkeninin *syntax*'ı crontab syntaxındadır. Eğer bu syntax'ı bilmiyorsanız [bu](https://crontab.guru/) siteden kolayca oluşturabilirsiniz.

## Domain Ekleme ve Düzenleme

Oluşturduğunuz uygulamanın adı aynı zamanda uygulamanın yayınlanacağı subdomaindir. Uygulamanızın herhangi bir subdomainde değil de direkt olarak kökdizininde yayınlanmasını istiyorsanız, uygulamanıza alperenbozkurt.net gibi domaininizin adını vermelisiniz.
Ve ya aşağıdaki komutu kullanarak direkt olarak ayarlayabilirsiniz.
```dokku domains:set gitbuff alperenbozkurt.net```


```
# Tüm uygulamaların domainlerini listelemek için
dokku domains:report

# Uygulamaya yeni bir domain eklemek için
dokku domains:add gitbuff gitbuff.io

# Uygulamadan bir domaini silmek için
dokku domains:remove gitbuff alperenbozkurt.net

# komutları kullanılabilir.
```
## Yetersiz RAM'ler İçin Hafıza Problemi

Bu sorun bir swap alanı oluşturularak düzeltilebilir. Swap allanı, cihazın RAM'i dolduğunda harddisk'i RAM gibi kullanarak cihazın donmasını önlemektedir.
```
# Swap alanı için 2038 megabyte'lık bir dosya oluşturmalısın.
sudo fallocate -l 2048m /mnt/swap_file.swap  
# Yetkileri düzenle
sudo chmod 600 /mnt/swap_file.swap  
# Disk swap alanı olarak formatla  
sudo mkswap /mnt/swap_file.swap  
# Swap alanını etkinleştir
sudo swapon /mnt/swap_file.swap

# Swap alanı oluşturulmuş mu
sudo swapon -s  
df

# Yeniden başlatmadan sonra swap alanı mount edilecek şekilde ayarlak gerekiyor.
sudo nano /etc/fstab
# Açılan dosyanın en altına aşağıdaki satır eklenerek swap alanı oluşturulmuş olur.
/mnt/swap_file.swap none swap sw 0 0
```

## Dil Dosyaları İle İlgili Uyarı Mesajları

Bu mesajları almamak için `sudo dpkg-reconfigure locales` komutunu çalıştırılmalısın.
 
## Cloudflare Kullanarak SSL Sertifikası Eklemek

Öncelikle `certificate.pem` ve `certificate.key` dosyalarına ihtiyacınız var bu dosyaları **cloudflare** *dashboard*'ından edinebilirsiniz.

```bash
tar cvf certificates.tar certificate.pem certificate.key
# Bu komutla iki dosyayı certificates.tar olarak sıkıştırıyoruz. 
dokku certs:add gitbuff < certificates.tar
# Ardından bu komutla sertifikaları uygulamaya ekliyoruz.
```
Bu da bu kadar basit. :)

## Log Analizi

Loglara bakmak için ise `dokku logs gitbuff` komutu kullanılır.  Eğer logların akış şeklinde olması için is `-t`.  

`dokku nginx:access-logs gitbuff` ile Nginx erişim logları,
`dokku nginx:error-logs gitbuff`  ile de Nginx hata logları görüntülenebilir.

## Bazı Kullanışlı Kodlar

Run komutu ile *container*'da komut çalıştırılabilir.
`dokku run gitbuff rails c`
`dokku run gitbuff bash`
Bir uygulamayı yeniden başlatmak için ise 
`dokku ps:restart gitbuff` komutu ile bir uygulama yeniden başlatılabilir.

Bu yazımda dokku hakkında kısa bir bilgi verdim ve bazı özelliklerinden bahsettim. Bu uygulamanın, geliştireceğiniz projelerde sizlere katkı sağlamasını dilerim. 
