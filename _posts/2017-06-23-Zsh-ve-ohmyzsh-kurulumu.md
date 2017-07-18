---
layout: post
title: "Zsh ve OhMyZsh Kurulumu"
date: 2017-06-23
excerpt: "Terminali daha verimli kullanmak için zsh ve ohmyzsh kuruyoruz."
tags: [zsh kurulumu,ohmyzsh kurulumu, terminal uygulamaları, terminal renklendirme]
---
Terminalimiz için zsh ve ohmyzsh yükleyeceğiz. Bu klasörler arasında daha kolay dolaşmamız ve git v.b programlarımızı daha hızlı kullanmamız için gerekli. Yükledikten bash dan farkını göreceksiniz zaten. :)

    sudo apt-get install zsh

İlk olarak bu komutla zsh'ı yüklüyoruz.

    sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

Bu scripti çalıştırarak ohMyZsh Kurulumunu gerçekleştiriyoruz.

Bir de tema yapalım
---
Temalara [bu linkten](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes) ulaşabilirsiniz. Ben **agnoster** isimli temayı beğendim onun kurulumunu yapacağım.

    gedit ~/.zshrc

ile açılan metin editöründe

    ZSH_THEME="robbyrussell"

satırını

    ZSH_THEME="agnoster"

olarak değiştiriyoruz. Tabi işimiz bitmedi şimdi birde font yüklememiz gerekiyor.
Fontlar
---
Onuda hızlı bir şekilde yükleyelim.

    # Klonlayalım
    git clone https://github.com/powerline/fonts.git
    # Klasöre girelim
    cd fonts
    # Yükleme scriptini çalıştıralım.
    ./install.sh
    # Kurulum dosyalarını temizleyelim.
    cd ..
    rm -rf fonts

Terminal ayarlarından **roboto mono**'yu seçiyorum.
Ve terminalimiz de hazır.
