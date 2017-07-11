---
layout: post
title: "Gnome Kurulumu ve Ayarları"
date: 2017-06-24
excerpt: "Gnome Shell yükleme ve daha fazlası.."
tags: [Pencere düğmeleri, gnome nasıl yüklenir, gnome üst tarafı gizleme, gnome tema yükleme]
---
Gnome kullandıktan sonra farkettim ki adamlar tam benim istediğim gibi bir masaüstü ortamı sunmuşlar tabi biraz eksiklikleri var onları ayarlayacağız. Pencere Yönetim tuşlarının yeri, üst taraftaki barı otomatik gizlenir yapma, icon takımı seçimi shell seçimi gibi işlemleri yapacağız.

Gnome Yüklemek
---
Gnome yüklenirken yanında birçok program yükleniyor biz bunu en aza indirmek istiyoruz bu yüzden sadece gnome-shell yükleyeceğiz.

    sudo apt-get install gnome-shell

yazarak gnome un yüklenmesini bekliyoruz. Herhangi bir soru sorar ise **gdm**'yi seçmenizi tavsiye ederim. Başlangıç ekranınız biraz değişecektir. :) 

Tema Kurulumu
---
`sudo apt-get install gnome-tweak-tool`  yazarak **Gnome Tweak Tool(İnce Ayar Aracı)**'u yükleyelim. Birkaç eklenti yükleyeceğiz. [Gnome](https://extensions.gnome.org/)'un sitesine girerek gerekli chrome/firefox eklentisinin kurulumunu yapalım.
`sudo apt-get install chrome-gnome-shell` komutunu da çalıştıralım.
Tweak tool uygulamasından indirdiğimiz eklentiyi etkinleştirelim. Aşağıdaki adımları izleyelim

    sudo add-apt-repository ppa:snwh/pulp
    # update repository info
    sudo apt-get update
    # install icon theme
    sudo apt-get install paper-icon-theme
    # install cursor theme
    sudo apt-get install paper-cursor-theme
    # install gtk theme
    sudo apt-get install paper-gtk-theme

Tweak tool'dan temamızı seçebiliriz.

Üst Barı gizleme
---
[Bu linkten](https://extensions.gnome.org/extension/740/hide-top-panel/) indirmeyi gerçekleştirebilirsiniz. İndirme bittiğinde tweak tooldan etkinleştirebilirsiniz.

Kapatma-Küçültme Tuşları
---

Pencere yönetim düğmelerinin yeri gnome yüklediğimde sağ tarafa geliyor. tabi ben sevmiyorum bunu bu tuşların sol tarafta olması daha kolayıma geliyor. Bu yüzden bu tuşları sola alacağız tabi isteyen içinde ubuntu'da nasıl sağa alınır onu yazacağız. Tabi ben tam ekran düğmesini de  gereksiz buluyorum bu arada onuda kaldıralım. (İşte Linux'un en sevdiğim yanı bu) 

    gsettings set org.gnome.shell.overrides button-layout close,minimize:

Burada ´:´ önemli bize ne tarafa yaslayacağımızı belirtiyor. Sola koyarsak butonlar sağ tarafta, sağa koyarsakta butonlar sol tarafta oluyor. ´close´, ´minimize´, ´maximize´, ´menu´ kullanılabilen komutlar.

Chrome Pencere Yönetim Tuşları
---
Yukarıdaki kod tüm sistemde çalıştı ama Chrome'da bi gariplikler var. Onu da düzeltmek için, önce chrome'un ayar sayfasından **sistemin başlık ve kenarlığını kullan** checkbox'ının tikini kaldıralım. Sonrada konsola 

    gconftool-2 --set /apps/metacity/general/button_layout --type string "close,minimize"

kodunu yazalım.
Şimdi daha iyi oldu.


Nautilus Fotoğraf Önizleme
---
Ubuntu yüklendiğinde resim dosyalarını açmadan göremiyoruz. Hazır tüm ayarları yapıyorken bunu da ekleyelim. Konsola 

    gsettings set org.gnome.nautilus.preferences show-image-thumbnails always

yazarak fotografları her zaman göstertebiliriz.
