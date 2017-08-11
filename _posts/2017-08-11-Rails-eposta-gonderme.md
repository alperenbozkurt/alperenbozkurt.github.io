---
layout: post
title: "MIPS Nedir"
date: 2017-06-15
excerpt: "Mips ve assembly kodları giriş."
tags: [MIPS, bilgisayar organizasyonu, register, assembly]
---
Rails uygulamalarında e-posta göndermek için ActionMailler diye bir yapı var.  Bu yazıda, bu yapıyı kullanarak bir e-posta göndereceğiz. Hızlıca başlayalım. 

    $ rails new mail
    $ rails g scaffold user name:string email:string
    $ rake db:migrate 

Şimdi kullanıcıların kayıt olabildiği, basit bir web uygulamamız oldu. Rails generator kullanarak hızlı bir şekilde dosyalarımızı oluşturalım.

    $ rails g mailer kayit_mailer

Oluşan dosyalardan `app/mailers/kayit_mailer.rb` dosyasına gönderici eposta adresini ve mail gönderecek metodu oluşturmamız gerekiyor. 
```rails
class KayitMailer < ApplicationMailer
  default from: "gonderici@gmail.com"

  def sample_email(user)
    @user = user
    mail(to: @user.email, subject: 'Sample Email')
  end
end
```
Burada `sample_email` metodu parametre olarak bir user nesnesi alıyor ve user 'ın eposta adresine bir mail gönderiyor. Göndereceği epostanın içeriğini ise `app/views/kayit_mailer/sample_email.html.erb` dosyasında ve `app/views/kayit_mailer/sample_email.text.erb` dosyasında düzenlememiz gerekiyor.

`app/views/kayit_mailer/sample_email.html.erb`  dosyasına 
```erb
<!DOCTYPE html>
<html>
  <head>
    <meta content='text/html; charset=UTF-8' http-equiv='Content-Type' />
  </head>
  <body>
    <h1>Hi <%= @user.name %></h1>
    <p>
      Sample mail sent using smtp.
    </p>
  </body>
</html>
```

`app/views/kayit_mailer/sample_email.text.erb`  dosyasına 
```erb
Hi <%= @user.name %>
Sample mail sent using smtp.
```
Html render edemeyen mail istemcileri için bu dosyayı oluşturmamız gerekiyor.  Gmail ayarlarımızı da yapalım.

`/config/environments/production.rb` dosyamıza alt taraftaki kodları ekleyelim.

```rails
config.action_mailer.delivery_method = :smtp
# SMTP settings for gmail
config.action_mailer.smtp_settings = {
	:address              => "smtp.gmail.com",
	:port                 => 587,
	:user_name            => 'gonderici@gmail.com',
	:password             => 'gondericiSifresi',
	:authentication       => "plain",
	:enable_starttls_auto => true
}
```
Tüm hazırlıkları tamamladık şimdi kısık ateşte 20 dk pişirelim. Piştikten sonra eposta gönderme işlemini tetikletmek için `user_controller.rb` nin create metoduna `KayitMailer.sample_email(@user).deliver` satırını eklememiz gerekiyor. 
```rails 
def create
  @user = User.new(user_params)

  respond_to do |format|
    if @user.save

      # Sends email to user when user is created.
      KayitMailer.sample_email(@user).deliver

      format.html { redirect_to @user, notice: 'User was successfully created.' }
      format.json { render :show, status: :created, location: @user }
    else
      format.html { render :new }
      format.json { render json: @user.errors, status: :unprocessable_entity }
    end
  end
end
```

Bu şekilde örnek bir eposta gönderme uygulaması yapmış olduk. Eğer bir yanlışım, eksiğim olmuş ise yorum yazabilirsiniz veya pull request yollayabilirsiniz. Kendinize iyi davranın.
