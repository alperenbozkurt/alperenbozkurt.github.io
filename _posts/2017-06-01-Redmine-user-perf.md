---
layout: post
title:  "Redmine User Perf."
image: "https://cloud.githubusercontent.com/assets/19302254/24833583/460131b0-1cd6-11e7-9bd5-7b16c0eec97a.png"
date:   2017-06-14
excerpt: "İş saatini grafiğe çeviren redmine eklentisi."
project: true
technologies: [Ruby, Redmine]
tags: [redmine, kullanıcı performansı eklentisi, user performance plugins, redmine eklentisi]
---

Users performance, kullanıcıların zaman girişlerini (time entries) grafiksel olarak gösteren bir redmine eklentisidir.

## Yükleme

- Redmine kök dizinine geliniz.
`cd {REDMINE_KÖKDİZİNİ}`
- Eklentiyi Github'dan kopyalayınız.
`git clone https://github.com/alperenbozkurt/redmine_users_performance.git plugins/redmine_users_performance`
(Veya depoyu indirin ve `{REDMINE_KÖKDİZİNİ}/plugins/` dizinine çıkartınız. Klasörün ismini `redmine_users_performance` olarak değiştiriniz. )
- Gemfile.lock dosyasını güncelleyiniz.
`bundle install`
- Redmine'ı yeniden başlatın.
- Eklentiyi **Yönetim > Eklentiler** sayfasında görebilirsiniz.
- Grafik tipini **Yönetim > Eklentiler > Yapılandır** sayfasından değiştirebilirsiniz.
- Eklentiyi istediğiniz proje için **Proje > Ayarlar > Modüller** sayfasından etkinleştirebilirsiniz.

<div markdown="0">
  <a href="https://github.com/alperenbozkurt/Kisiler/" class="btn btn-info">Kaynak Kodları</a>
</div>

## Ekran Görüntüleri

![Ekran Görüntüsü-01](https://cloud.githubusercontent.com/assets/19302254/24833583/460131b0-1cd6-11e7-9bd5-7b16c0eec97a.png)
![Ekran Görüntüsü-02](https://cloud.githubusercontent.com/assets/19302254/24833584/4601c5bc-1cd6-11e7-8195-e5349f805f6f.png)
![Ekran Görüntüsü-03](https://cloud.githubusercontent.com/assets/19302254/24833581/45fa3824-1cd6-11e7-8d70-a56cd6acfc8d.png)
![Ekran Görüntüsü-04](https://cloud.githubusercontent.com/assets/19302254/24833582/45ffda68-1cd6-11e7-8401-b58ed324d9d4.png)
![Ekran Görüntüsü-05](https://cloud.githubusercontent.com/assets/19302254/24833585/46032876-1cd6-11e7-84e4-84a940e5326e.png)
