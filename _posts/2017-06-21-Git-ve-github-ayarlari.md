---
layout: post
title: "Git ve Github Ayarları"
date: 2017-06-21
excerpt: "Git Yüklemek ve Github ayarlarını yapmak."
tags: [github, ssh-key, ssh-key nasıl, ubuntu]
---

İlk olarak git yüklemek gerekiyor. Kısaca git’den bahsedersek. Git bir sürüm kontrol sistemidir. Yani projenizin eski yedeklerini tutar. Ne zaman ne değişiklik yaptığınızı bilirsiniz. Yedek, yedek2, yedekson, yedeksonson, yedeksonsonson gibi klasörler kullanmak yerine git kullanmak daha uygun görünüyor. Github ise bizim dosyalarımızı online olarak tutmamızı sağlar.

    sudo apt-get install git
Kurulum tamamlandıktan sonra, commit yaptığımızda çıkacak bilgileri girmemiz gerekiyor.

    git config --global user.name "Ad soyad"
    git config --global user.email "adsoyad@mail.com"
Bu adımı da tamamladıktan sonra **ssh-key** oluşturmamız gerekiyor.

    ssh-keygen
terminale bu komutu girdikten sonra önümüze gelen ilk satır ssh-key'in nerede üretileceğini söylüyor.Bu satıra bir şey yazmadan enter tuşuna basarsak default dizine oluşturacaktır.
2. satır bizden bir anahtar istiyor. Bizim için o anahtara göre şifreli bir kod üretecek.
3. satırda ise şifremizi tekrarlamamızı istiyor. Bunları tamamladıktan sonra github'a bilgisayarımızı tanıtabiliriz.

Terminalden 

    cat ~/.ssh/id_rsa.pub
komutunu çalıştırarak şifremizi kopyalıyoruz.

Githubdan **Settings** sayfasına giriyoruz burada **ssh and gpg keys** menüsüne tıklayarak **New SSH key** butonuna tıklıyoruz. Bir başlık vererek **~/.ssh/id_rsa.pub** dosyasının içeriğini alt tarafa yapıştırıyoruz.


    ssh -T git@github.com
koduyla da bağlantımızı test ediyoruz. 
