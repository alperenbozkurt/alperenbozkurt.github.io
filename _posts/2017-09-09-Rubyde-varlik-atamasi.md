---
layout: post
title: "Ruby'de ||=, Varlık Ataması"
date: 2017-09-09
excerpt: "Rubyde varlık ataması nedir, ||= operatörü ve örnek kullanımları"
tags: [rubyde varlık ataması, varlık ataması, ruby operatörler, varlık ataması operatörü, or equals, veya eşittir operatörü]
---

||= operatörü eğer bir a değişkeni tanımlanmamış ise ona atama yapmak için kullanılır. Bu komutu [`x || (x = y)`][1] gibi düşünün. Eğer bir x değeri tanımlanmamış ( `false` veya `nil` de olabilir ) ise x'e atama işlemi yapar.

Biraz Örnek yapalım;
```ruby
a = nil
b = 10
a ||= b 	# a' nın değeri nil olduğu için a = b ataması yaptı.
puts a 		# 10
#--------------------------------------

a = 10
b = 5
a ||= b 	# a'nın değeri olduğu için bir atama olmuyor.
puts a		# 10
#--------------------------------------

t = 10
e ||= t		# e değişkeni tanımlanmadığı için e = 10 ataması oldu.
puts e		# 10
#--------------------------------------


```

[1]: https://stackoverflow.com/a/3800986/7244925 "Stackoverflow"
