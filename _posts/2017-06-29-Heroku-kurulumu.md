---
layout: post
title: "Heroku Kurulumu"
date: 2017-06-25
excerpt: "Heroku Kurulumu ve ilk Deploy"
tags: [Heroku, nasıl yayınlanır, ruby, hosting,rails uygulaması yayınlamak]
---
Heroku Ruby, Java, python gibi dillerde yazılmış web projelerinizi hızlıca ve teknik sunucu kurma bilgisi gerektirmeden yayınlamanızı sağlayan bir platformdur. Ücretsiz olarak uygulama yayınlamamıza izin verir. Geliştirme aşamasında olan projelerimiz için heroku'yu kullanabiliriz.

Nasıl Kurulur
---
Websitesinden üyelik açtıktan sonra,

    wget -qO- https://cli-assets.heroku.com/install-ubuntu.sh | sh

Scripti ile kurulumu gerçekleştirelim.
Kurulum gerçekleştiğinde

    heroku login

komutunu girerek giriş yapalım.

Proje Nasıl Yayınlanır
---

Masaüstü dizinine gelip örnek ruby projemizi indirelim ve içerisine girelim.

    git clone https://github.com/heroku/ruby-getting-started.git
    cd ruby-getting-started

    heroku create  

Bu komutla herokuapp.com uzantılı bir web projesi oluşturuyoruz.Otomatik olarak git bağlantılarını yapacak. Bende şöyle bir adres geldi.

    Creating app... done, ⬢ infinite-sierra-90021
    https://infinite-sierra-90021.herokuapp.com/ | https://git.heroku.com/infinite-sierra-90021.git

Artık projemi deploy ettiğimde https://infinite-sierra-90021.herokuapp.com/ adresinden kullanabilirim demektir.

    git push heroku master

komutuyla dosyaları yükleyebiliriz.

    heroku open

 komutuyla websitesini varsayılan tarayıcıyla açabilirsiniz. Logları görmek içinse,

    heroku logs --tail

yazailirsiniz. Sunucuda herhangi bir komut çalıştırmak için ise

    heroku run #komut

şeklinde yazıyoruz. Heroku bu şekilde kullanılıyor. Bu yazı yararlı olduysa yorum kısmına bir gülücük bırakmayı unutmayın :) Eğer yanlışım veya eksiğim varsa bunu yorum kısmından bana iletebilirsiniz.
