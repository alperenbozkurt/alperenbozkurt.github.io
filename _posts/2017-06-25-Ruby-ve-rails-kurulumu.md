---
layout: post
title: "Ruby On Rails Kurulumu"
date: 2017-06-25
excerpt: "Rbenv, Ruby, Rails, Postgresql kurulumu"
tags: [Rbenv, Ruby, Rails, Postgresql, Ruby on Rails kurulumu, ror yükleme]
---
Ruby kurulumu biraz uğraştırıcı ama işlemler basit. İlk olarak Rbenv kuracağız. Ne işe yarar bu rbenv, bilgisayara birden fazla ruby versyonu kurmamızı sağlıyor. Buda bizi farklı projelerde çalışırken ruby'yi silip tekrar kurmaktan kurtarıyor. Kuruluma başlamadan önce gerekli paketleri kuralım. 

    sudo apt-get update
    sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev nodejs


Rbenv Kurulumu
---

Alttaki kodları sırasıyla çalıştırırsak rbenv yüklenmiş olacak. Tabiki alttaki kodlar zsh kullanıyorsanız çalışacaktır aksi taktirde **.zshrc ** yazan yerleri ** .bashrc** olarak değiştiniz.

    cd
    git clone https://github.com/rbenv/rbenv.git ~/.rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
    echo 'eval "$(rbenv init -)"' >> ~/.zshrc
    exec $SHELL
    
    git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.zshrc
    exec $SHELL


Ruby Kurulumu
---

Rbenv kullanarak ruby kurulumu yapacağız. 

    rbenv install 2.4.0

Yüklemek istediğimiz ruby versyonunu bu şekilde yüklüyoruz.

    rbenv global 2.4.0

Bu şekilde yüklenmiş ruby versyonlarından birisini seçebiliyoruz. Kontrol edelim yüklenmiş mi ?

    ruby -v

Eğer buraya kadar tamamsa bundler yükleyelim.

    gem install bundler

Rails Kurulumu
---

    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    sudo apt-get install -y nodejs
    gem install rails -v 5.1.1
    rbenv rehash
    rails -v

Eğer kurulum başarılı olduysa rails sürümünü ekranda görmemiz gerekiyor. 

Postgresql Kurulumu
---

    sudo sh -c "echo 'deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main' > /etc/apt/sources.list.d/pgdg.list"
    wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | sudo apt-key add -
    sudo apt-get update
    sudo apt-get install postgresql-common
    sudo apt-get install postgresql-9.5 libpq-dev 

Postgres bu şekilde kuruluyor fakat veritabanı oluşturmamız için bir kullanıcıya ihtiyacımız var.

    sudo -u postgres createuser alperen -s
    
    sudo -u postgres psql
    postgres=# \password alperen

Kullanıcının şifresini girdikten sonra bu adım da bitmiş oluyor. Yeni projelere yelken açabilirsiniz.

    rails new yeniproje -d postgresql 
    ;)
