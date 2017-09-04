---
layout: post
title: "Sass Ve Scss Nedir ?"
date: 2017-09-05
excerpt: "Sass ve Scss in nedir, Neler yapılabilir, Nasıl yüklenir gibi soruların cevapları burada"
tags: [Sass nedir, Sass Nasıl kullanılır, Sass Nasıl kurulur, Scss Nedir, Scss nasıl kurulur, scss Nasıl kullanılır]
---

Daha önce Haml'ın ne olduğundan ve nasıl kullanıldığından söz etmiştik. Bu yazı [Haml Nedir ?](http://alperenbozkurt.net/Haml-ve-sass-nedir/) Yazısının devamı niteliğinde olacaktır. 

Sass Nedir ?
---

**Sass**, css'in karmaşıklığını ortadan kaldırmak amacıyla yazılan bir **Ruby Gem**'idir. Css'de At koşturmaya yarar. Syntax'ını rubyceye yakınlaştırır. .sass uzantılı dosyayı derleyerek css dosyası haline getirir. 
Önce syntax'ı nasıl ona bakalım.
```css
.deneme{
    margin: 0 auto;
    width: 150px
}
```
Gibi bir css kodumuz olduğunu düşünelim. Bu kodu sass ile yazıldığında ise; 
```sass
.deneme
    margin: 0 auto
    width: 150px
```
Şeklinde oluyor. Gördüğünüz gibi gereksiz ; ve {  } yazmadık.

Sass Nasıl Kullanılır ?
---

 - **;** işaretini satırların sonlarına koymak zorunda değiliz.
 - **{** işaretini kullanmak zorunda değiliz. ( Aynı şekilde **}** işaretini de kullanmak zorunda değiliz. )  
 - **$** işareti ile değişken oluşturabiliriz.
 - Matematik operatörlerini kullanabiliyoruz. 
 - İf - Else gibi karar yapılarını kullanabiliyoruz.
 - While, Each gibi döngüleri kullanabiliyoruz.
 - Fonksiyon yazıp cevabını alabiliyoruz.

Scss Nedir ?
---
Scss, sass geliştiricilerinin biraz daha front-end geliştiricilerini düşünerek oluşturduğu syntax'dır. Bu syntax css'e benziyor. {, } ve ; burada kullanılıyor. Diğer her şey sass ile aynıdır. 

Sass Nasıl Kurulur ?
---
Terminale '`gem install sass-rails`' yazarak veya gemfile dosyasına `gem 'sass-rails'` satırını ekleyip, terminale '`bundle install`' yaparak kurabilirsiniz. Bu kurulumla sass ile scss de kurulmuş olacaktır. Dosyalarınızın uzantısı .sass veya .scss şeklinde yapmalısınız. 

Daha ayrıntılı bilgiyi [Bu](https://scotch.io/tutorials/getting-started-with-sass) yazıda bulabilirsiniz.

*Ruby <3 Ben *
