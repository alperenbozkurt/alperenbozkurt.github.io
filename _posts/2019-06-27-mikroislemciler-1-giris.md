---
layout: post
title: "Mikroişlemciler #1 - Giriş"
date: 2019-06-27
excerpt: "Fırat Üniversitesi, Mikroişlemciler dersinde öğretilen PIC16F877A mikroişlemcisinin iç yapısı ve nasıl kodlanacağı hakkındaki notlarımın internet ortamına geçirilmiş halidir."
tags: [PIC16F877A, PIC16F877A örnekleri, Mikroişlemciler Ders Notları, Hayrettin Can Ders Notları, Fırat Üniversitesi, Mikroişlemciler]
---

Fırat üniversitesi'nde aldığım en zor derslerden birisi olan, Hayrettin CAN'ın verdiği, mikroişlemciler dersi notlarımı başka kişilerin de yararlanması için seri şeklinde paylaşıyorum.

# PIC16F877A

Mikroişlemciler dersinde kullandığımız mikroişlemci **PIC16F877A**'dır. Bu çipin özellikleri aşağıdaki gibidir. Her bir maddenin detaylarına yazının ileriki bölümlerinden erişebilirsiniz.
- RISC Mimarisinde bir işlemcidir.
- Mikrochip firmasının bir üyesi olan bu çipde 40 pin vardır. Bu pinlerin 33 tanesi giriş/çıkış pinidir. 
- 20 MHz de çalışır. Bu hızda her bir komutun çalışması 200 ns sürer. Aynı zamanda 4 ve 16 MHz ile çalışan versyonları da vardır. Aksi belirtilmediği sürece biz 4 MHz de kullandık. Her bir komut 4 aşamada çalıştığı için  bir komutun çalışma süresi daha kolay hesaplanabiliyor. (Fetch Decode Execute Writeback) 4 MHz hızda çalışan işlemcide her bir komut 1 µs sürüyor. 
- 3 adet belleği vardır:
  --8K * 14 bitlik Flash Memory'si vardır. Bu bölüme program kodları yazılır.
  --368 Byte Data Memory'si vardır. Bu alanda ayar registerları ve özel amaçlı registerlar bulunur. 
  --256 Byte EEPROM belleği vardır. Bu alan kalıcı hafıza olarak kullanılır.
- 3 Tane Timer vardır.
- 2 tane Compare, Capture, PWM modülü vardır.
- 10 Bit Analog/Dijital dönüştürücüsü vardır.
- 35 tane komutu vardır.

Bir sonraki yazımda komutlardan ve pinlerden bahsedeciğim.
