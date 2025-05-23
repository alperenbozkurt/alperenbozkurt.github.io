---
layout: post
title: "Ruby'de Aspect Oriented Programming: Cross-Cutting Concern'leri Yönetmek"
date: 2025-05-23
excerpt: "Ruby'de Aspect Oriented Programming (AOP) yaklaşımlarını keşfedin. Module, Prepend ve Method Wrapping teknikleriyle loglama, hata yönetimi ve performance monitoring gibi cross-cutting concern'leri nasıl zarif bir şekilde kodunuza entegre edebileceğinizi öğrenin. Pratik örnekler ve gerçek dünya senaryolarıyla AOP'nin gücünü Ruby'de kullanmaya başlayın."
tags: [ruby aop, aspect-oriented-programming, cross-cutting-concerns, ruby-modules, method-wrapping, error-handling, logging, performance-monitoring, ruby-metaprogramming, design-patterns, clean-code]
---

Ruby geliştiricileri olarak, kod yazerken sıklıkla karşılaştığımız bir durum var: aynı fonksiyonaliteyi farklı class'larda tekrar tekrar yazmak zorunda kalmak. Loglama, hata yönetimi, performance monitoring, authentication gibi **cross-cutting concern'ler** uygulamanın her yerinde bulunur ve genellikle iş mantığını kirletir.

İşte tam bu noktada **Aspect Oriented Programming (AOP)** devreye giriyor. AOP, bu tür cross-cutting concern'leri ana iş mantığından ayırarak, kodun daha temiz, sürdürülebilir ve test edilebilir olmasını sağlar.

## Aspect Oriented Programming Nedir?

AOP, yazılım geliştirmede separation of concerns prensibini destekleyen bir programlama paradigmasıdır. Temel amacı, uygulamanın farklı bölümlerinde kullanılan ortak fonksiyonaliteleri (cross-cutting concerns) merkezi bir yerden yönetmektir.

Klasik OOP'da bir class'ın birden fazla sorumluluğu olabilir:
- Kendi iş mantığı
- Hata yönetimi  
- Loglama
- Performance monitoring
- Security kontrolü

AOP ile bu sorumlulukları ayırır ve **aspect'ler** olarak adlandırdığımız ayrı birimler halinde yönetiriz.

## Ruby'de AOP Yaklaşımları

Ruby'nin metaprogramming yetenekleri sayesinde AOP'yi farklı şekillerde uygulayabiliriz. En yaygın yaklaşımları inceleyelim:

### 1. Module ve Include Yaklaşımı

En basit AOP implementasyonu için Module'ları kullanabiliriz:

```ruby
module Loggable
  def self.included(base)
    base.extend(ClassMethods)
  end
  
  module ClassMethods
    def logged_method(method_name)
      alias_method "#{method_name}_without_logging", method_name
      
      define_method(method_name) do |*args, &block|
        puts "#{self.class}##{method_name} başladı"
        result = send("#{method_name}_without_logging", *args, &block)
        puts "#{self.class}##{method_name} tamamlandı"
        result
      end
    end
  end
end

class PaymentService
  include Loggable
  
  def process_payment(amount)
    # Payment logic
    puts "#{amount} TL ödeme işleniyor..."
  end
  
  logged_method :process_payment
end
```

### 2. Method Missing ile Dynamic Wrapping

Daha dinamik bir yaklaşım için `method_missing` kullanabiliriz:

```ruby
module AutoLogger
  def self.included(base)
    base.extend(ClassMethods)
  end
  
  module ClassMethods
    def with_logging
      Module.new do
        def method_missing(method_name, *args, &block)
          if respond_to_missing?(method_name)
            puts "[LOG] #{self.class}##{method_name} çağrıldı"
            result = super
            puts "[LOG] #{self.class}##{method_name} tamamlandı"
            result
          else
            super
          end
        end
        
        def respond_to_missing?(method_name, include_private = false)
          super
        end
      end.tap { |mod| prepend mod }
    end
  end
end
```

### 3. Gerçek Dünya Örneği: Error Handler Aspect

Şimdi gerçek bir cross-cutting concern olan hata yönetimini AOP yaklaşımıyla implement edelim:

```ruby
# frozen_string_literal: true

# ErrorHandler modülü, sınıflardaki tüm metodlar için otomatik hata yakalama özelliği sağlar.
# Bu modülü include eden sınıflar, tüm metodlarında oluşan hataları merkezi bir şekilde yönetebilir.
#
# Kullanım:
#   class MyClass
#     include ErrorHandler
#
#     on_error :handle_error
#
#     def some_method
#       # Burada hata oluşursa handle_error metodu çağrılır
#     end
#
#     private
#
#     def handle_error(exception, message, method_name)
#       # Hata yönetim mantığı
#     end
#   end
module ErrorHandler
  extend ActiveSupport::Concern

  included do
    # Her sınıf için hata yakalama metodunu tutar
    class_attribute :error_handler_method, default: nil

    class << self
      # Hata yakalama metodunu belirler
      # @param method_name [Symbol] Hata yakalama işlemi yapacak metodun adı
      def on_error(method_name)
        self.error_handler_method = method_name
      end

      # Alt sınıflara hata yakalama ayarlarını miras alır
      # @param subclass [Class] Miras alan alt sınıf
      def inherited(subclass)
        super
        subclass.error_handler_method = error_handler_method
      end

      # Her yeni metod tanımlandığında çağrılır ve hata yakalama özelliği ekler
      # @param method_name [Symbol] Yeni tanımlanan metodun adı
      def method_added(method_name)
        # Sonsuz döngüyü ve özel metodları atla
        return if @wrapping_method || method_name == error_handler_method || method_name.to_s.start_with?('_wrapped_')

        @wrapping_method = true

        # Orijinal metodu yeni bir isimle sakla
        wrapped_method_name = "_wrapped_#{method_name}"
        alias_method(wrapped_method_name, method_name)

        # Metodu hata yakalama özelliğiyle yeniden tanımla
        define_method(method_name) do |*args, **kwargs, &block|
          send(wrapped_method_name, *args, **kwargs, &block)
        rescue => e
          # Hata oluştuğunda belirlenen hata yakalama metodunu çağır
          if self.class.error_handler_method
            error_message = "#{self.class.name}##{method_name} EXCEPTION ERROR"
            send(self.class.error_handler_method, e, error_message, method_name)
          end
          raise e
        end

        @wrapping_method = false
      end
    end
  end
end
```

Bu error handler'ı kullanmak çok basit:

```ruby
class PaymentGateway
  include ErrorHandler
  
  on_error :send_error_notification
  
  def process_payment(amount)
    # Bu metod hata verirse send_error_notification çağrılır
    raise "API hatası" if amount < 0
    puts "Ödeme başarılı: #{amount} TL"
  end
  
  def refund_payment(payment_id)
    # Bu metod da otomatik olarak korunur
    raise "Ödeme bulunamadı" unless payment_id
    puts "İade işlemi tamamlandı"
  end
  
  private
  
  def send_error_notification(error, message, method_name)
    puts "HATA: #{message}"
    puts "Detay: #{error.message}"
    # Mail gönder, Slack'e mesaj at, vb.
  end
end
```

## AOP'nin Avantajları

- **Separation of Concerns**: İş mantığı ile cross-cutting concern'ler ayrılır. Payment logic sadece ödeme ile ilgilenir, hata yönetimi ayrı bir katmanda yer alır.

- **DRY Prensibi**: Hata yönetimi kodunu her metoda yazmak yerine, bir kez yazıp tüm class'ta kullanırız.

- **Sürdürülebilirlik**: Hata yönetimi stratejisini değiştirmek istediğinizde tek bir yeri güncellemeniz yeterli.

- **Test Edilebilirlik**: Cross-cutting concern'leri ayrı ayrı test edebilirsiniz.

## Dikkat Edilmesi Gerekenler

- **Performance Overhead**: Method wrapping performans kaybına neden olabilir. Kritik performance gerektiren yerlerde dikkatli kullanın.

- **Debugging Zorluğu**: Stack trace'lerde ekstra katmanlar görünür, debugging zorlaşabilir.

- **Magic Behavior**: Kod okuyucusu için açık olmayan "sihirli" davranışlar oluşabilir.

## Sonuç

Ruby'de AOP, metaprogramming yetenekleri sayesinde oldukça güçlü ve esnek bir şekilde uygulanabilir. Module, prepend, method_missing gibi teknikleri kullanarak cross-cutting concern'leri temiz bir şekilde yönetebilirsiniz.

Error handling, logging, performance monitoring gibi konularda AOP yaklaşımı kodunuzu daha temiz, sürdürülebilir ve test edilebilir hale getirecektir. Ancak performans ve kod okunabilirliği açısından dikkatli kullanmak önemlidir.

Bu tür advanced Ruby teknikleri ve mimari kararlar hakkında daha detaylı geri bildirim almak, kod review yaptırmak isterseniz [erbab.dev](https://erbab.dev) platformunu kullanabilirsiniz. **BLOG50** kupon koduyla %50 indirimli fiyatlandırmadan yararlanabilirsiniz. 
