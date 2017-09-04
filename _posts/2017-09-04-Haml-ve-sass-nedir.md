---
layout: post
title: "Haml Nedir ?"
date: 2017-09-04
excerpt: "Haml nedir, Nasıl kullanılır, Nasıl kurulur?"
tags: [haml nedir, haml nasıl kullanılır, haml nasıl yüklenir, haml kod örnekleri]
---

Ruby'ye geçmemin en büyük sebebi sade ve öz olmasıdır. 

> Sadelik en büyük zenginliktir

Felsefesini benimsemiş **Yukihiro Matsumoto** Ruby dilini yazarken anlaşılabilirliği, temizliği ve sadeliği ön planda tutmuştur. Rails geliştiren bir kaç amca bu sadeliğin html ve css'de de geçerli olması gerektiğini düşünerek haml ve sass'ı ortaya çıkarmışlar. Bu yazımda html ve css in güncellenmiş versiyonları olan haml ve sass dan bahsedeceğim.


Haml Nedir ?
---

**Haml**, html'in karmaşıklığını ortadan kaldırmak amacıyla yazılan bir **Ruby Gem**'idir. .haml uzantılı dosyayı derleyerek html dosyası haline getirir. 
```erb
<div id='content'>
  <div class='left column'>
    <h2>Welcome to our site!</h2>
    <p><%= print_information %></p>
  </div>
  <div class="right column">
    <%= render :partial => "sidebar" %>
  </div>
</div>
```
Gibi bir html ( erb ) kodumuz olduğunu düşünelim. Aynı kod haml ile yazıldığında ise; 
```haml
#content
  .left.column
    %h2 Welcome to our site!
    %p= print_information
  .right.column
    = render :partial => "sidebar"
```
Şeklinde kısalıyor. Haml'ın basit birkaç kuralı var şimdi onlardan bahsedelim.

Haml Nasıl Kullanılır ?
---

 - **%** işareti bir html etiketi açmak için kullanılıyor.
 - **\#** işareti bir html objesine id atamak için kullanılıyor.
 - **.** işareti bir html objesine class belirtmek için kullanılıyor.  
 - **-** işareti ekrana çıktı vermeyen ruby kodları için kullanılıyor. ( Örn: - 1..100.each do )
 - **=** çıktı veren bir ruby kodu çalıştırılacak ise bu kod kullanılıyor. ( Puts methodu gibi düşünülebilir. Örn:  = x.name )
 - Etiketler kapatılmıyor. Etiketlerin etki alanları girintiler ve çıkıntılar ile anlaşılıyor.
 - Eğer div oluşturacaksanız %div.foo demenize gerek yok .foo komutu default olarak div objesi oluşturuyor. Aynı şey id ( # ) için de geçerlidir.

Haml Nasıl Yüklenir ?
---
`gem install haml` yazarak veya gemfile dosyamıza `gem 'haml'` yazıp terminale `bundle install` diyerek kurulumu gerçekleştirebiliriz. Kodlarımızı yazdığımız dosyanın uzantısı da .haml olarak kaydetmeliyiz.


Haml hakkındaki yazım bu kadar oldu. [Sass ve Scss Nedir adlı yazıma da buradan](#) erişebilirsiniz.

*Ruby <3 Ben *
