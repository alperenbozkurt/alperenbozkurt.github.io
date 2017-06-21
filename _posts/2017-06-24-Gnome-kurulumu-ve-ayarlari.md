---
layout: post
title: "Gnome Kurulumu ve Ayarları"
date: 2017-06-24
excerpt: "Gnome Shell yükleme ve daha fazlası.."
tags: [Pencere düğmeleri, gnome nasıl yüklenir, gnome üst tarafı gizleme, gnome tema yükleme]
---
Gnome kullandıktan sonra farkettim ki adamlar tam benim istediğim gibi bir masaüstü ortamı sunmuşlar tabi biraz eksiklikleri var onları ayarlayacağız. Pencere Yönetim tuşlarının yeri, üst taraftaki barı otomatik gizlenir yapma, icon takımı seçimi shell seçimi gibi işlemleri yapacağız.

Kapatma Küçültme Tuşları
---

Pencere yönetim düğmelerinin yeri gnome yüklediğimde sağ tarafa geliyor. tabi ben sevmiyorum bunu bu tuşların sol tarafta olması daha kolayıma geliyor. Bu yüzden bu tuşları sola alacağız tabi isteyen içinde ubuntu'da nasıl sağa alınır onu yazacağız. Tabi ben tam ekran düğmesini de  gereksiz buluyorum bu arada onuda kaldıralım. (İşte Linux'un en sevdiğim yanı bu) 

     gsettings set org.gnome.desktop.wm.preferences button-layout ':close,minimize'

Burada ´:´ önemli bize ne tarafa yaslayacağımızı belirtiyor.sonra sırasıyla butonları koyuyoruz ´close´, ´minimize´, ´maximize´, ´menu´ kullanılabilen komutlar.

Chrome Pencere Yönetim Tuşları
---
Yukarıdaki kod tüm sistemde çalıştı ama Chrome hala eskisi gibi. Onu da düzeltmek için,

`gconftool-2 --set /apps/metacity/general/button_layout --type string "close,minimize"`

kodunu yazalım.
