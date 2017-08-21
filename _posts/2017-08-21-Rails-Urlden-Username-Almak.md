---
layout: post
title: "Rails Urlden Username Almak"
date: 2017-08-21
excerpt: "Ruby on rails'de get parametresi ile id yerine username nasıl alınır."
tags: [Ruby, Rails, devise, get ile username almak, profil linkini username ile oluşturmak, profil sayfasına username ile girmek]
---

Google, Twitter, İnstagram ve diğerleri artık user_id yi urlden almıyor.Daha anlamlı daha akılda kalıcı şeyler kullanıyorlar. **Username**. Artık **username**'lerimiz bizim **benzersiz idlerimiz** olmaya başladı. Peki internetin öncüleri böyle yapıyor da biz neden eski sitilde devam ediyoruz ? 

`http://site.com/users/121331231` olarak girdiğimiz kullanıcı sayfasına `http://site.com/alperenbozkurt` olarak girmek daha kolay ve akılda kalıcı olmaz mıydı? Tabi ki olurdu.O zaman bizde örnek bir Rails uygulaması ile bunu yapalım.Olay gayet basit. routes.rb dosyasında oynama yapacağız ve show actionunda biraz değişiklik yapacağız.

**Name**, **surname**, **email** ve **username** alanları olan bir users modelimiz olduğunu düşünelim. **routes.rb** bu model routes.rb dosyasında `resources :users` şeklinde  çağırılıyor bu yapıya göre kayıt olan bir kişi **REST mimarisine** uygun bir şekilde `/users/1` olarak kendi profil sayfasına gidecek. **routes.rb** dosyamızı şu şekilde düzenleyelim;
```ruby
Rails.application.routes.draw do
  resources :users, except: [:show]
  get '/:username', to: 'users#show', as: 'profile'
end
```
Tamam ama şimdi biz burada ne yazdık. Sitemize bir **'/birşey'** şeklinde bir **get** isteği gelirse onu **UsersController**'ın **Show** action'ına yönlendir. **profile_path** şeklinde yazarak da railsin içerisinde kullanabileceğiz. **Except** ile de gereksiz kalabalıktan kurtulduk. Show action'ını kendimiz yazdığımız için, resources'deki yönlendirmeye gerek kalmadı.

Peki, linke bir kullanıcı adı verdiğimizde show action'ını çalışıyor burada ne yapmamız gerekiyor. 
```ruby
  def show
    @user = User.find_by(username: params[:username])
  end
```

Bu şekilde artık **@user** değişkeni, veritabanında kayıtlı olan username'in bilgileri ile doluyor. Artık view ve controller katmanındaki linkleri **profile_path** şeklinde vermeliyiz. Örneğin link_to metodunu kullanıcaksak;
```erb
<% @users.each do |user| %>
  <tr>
    <td><%= user.name %></td>
    <td><%= link_to 'Profili', profile_path(user.username) %></td>
  </tr>
<% end %>
```
Gibi kullanabiliriz. Kontrol katmanında da aynı şekilde, mesela bir kayıt işleminde;
```ruby
  if @user.save
    redirect_to profile_path(@user.username), notice: 'Kullanıcı başarıyla oluşturuldu.'
  else
```
Şeklinde kullanabiliriz. Username'in eşsiz olduğuna dikkat edilmeli yoksa ortalık karışabilir. Username'i eşsiz yapmak için user model dosyasın'a alttaki satırı ekleyebilirsiniz.
```ruby
class User < ApplicationRecord
  validates :username, uniqueness: {message: 'zaten kullanıyor Lütfen başka bir kullanıcı adı seçiniz.'}
end
```

Bu yazı da bu kadar. Bir yanlışım olmuşsa yada klavyem sürçmüşse, yorum kısmından veya e-posta ile bana bildirebilirsiniz. 
