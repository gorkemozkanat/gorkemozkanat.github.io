---
title: "PortSwigger Insecure Deserialization" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

**Serialization**, nesneler ve bunların alanları gibi karmaşık veri yapılarını sıralı bir bayt akışı olarak gönderip alınabilen “daha düz” bir biçime dönüştürme işlemidir. Verileri serializing etmek şunları daha kolay hale getirmektedir:

- Inter-process memory, bir dosyaya veya bir veritabanına karmaşık verilerin yazılmasına
- Bir ağ üzerinden, uygulamanın farklı bileşenleri arasında veya bir API çağrısında karmaşık verilerin gönderilmesinde

En önemlisi, nesne serializing edilirken durumu kalıcı olmaktadır yani nesnenin nitelikleri atanan değeriyle birlikte korunur.

**Deserialization,** serialized durumdan çıkarma, orijinal nesnenin tam olarak işlevsel bir kopyasına serialized hale getirilmeden önceki haline geri yükleme işlemidir.

Insecure deserialization, kullanıcı tarafından kontrol edilebilen verilerin bir web sitesi tarafından seri durumdan çıkarılmasıdır. Saldırganın zararlı verileri uygulama koduna geçirmek için serileştirilmiş nesneleri değiştirmesine olanak tanımaktır. 

Serileştirilmiş bir nesneyi tamamen farklı bir sınıftan bir nesneye değiştirmek bile mümkündür. Web sitesinde mevcut olan herhangi bir sınıfın nesneleri hangi sınıftan beklendiğine bakılmaksızın seri hale getirilecek ve somutlaştırılacaktır. Bu nedenle ınsecure deserialization bazen bir “object injection” güvenlik zafiyetine neden olabilmektedir.


[Lab  1](#lab-1-modifying-serialized-objects){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-modifying-serialized-data-types){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-using-application-functionality-to-exploit-insecure-deserialization){: .btn .btn--danger} [Lab 4](#lab-4-arbitrary-object-injection-in-php){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-exploiting-java-deserialization-with-apache-commons){: .btn .btn--danger}{: .text-left}  [Lab 6](#lab-6-exploiting-php-deserialization-with-a-pre-built-gadget-chain){: .btn .btn--danger}{: .text-left}  [Lab 7](#lab-7-exploiting-ruby-deserialization-using-a-documented-gadget-chain){: .btn .btn--danger}{: .text-left} 

## Lab-1 Modifying serialized objects

Bu laboratuvar, serialization-based session mekanizması kullanmaktadır ve privilege escalation güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için cookie bilgisi üzerinde oynamalar yaparak Carlos kullanıcısının silinmesi gerekmektedir.

Id-pw ile giriş yaptıktan sonra set edilen cookie bilgisi base64 ile encode yapılmıştır. Decode edip “admin” bilgisini 0 dan 1 e getirerek yetki yükseltme gerçekleştirilmektedir. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/1/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/1/2.png){: .align-center}

## Lab-2 Modifying serialized data types

Bu laboratuvar, serialization-based session mekanızmasına sahiptir ve authentication bypass güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için administrator hesabına erişip carlos kullanıcısının silinmesi gerekmektedir.

Session bilgisi base64 ile decode edilerek incelendiğinde access_token bilgisi string değer olarak sunucu tarafına gönderilmekte. Access token değeri string (s) değerinden integer(i) değerine değiştirilerek ve basamak olarak 0 ayarlanarak sunucu tarafına gönderildiğinde, authentication bypass gerçekleştirilmiş ve admin yetkilerine erişilmiş olmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/2/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/2/2.png){: .align-center}

## Lab-3 Using application functionality to exploit insecure deserialization

Bu laboratuvbarda serialization-based session mekanizması bulunmaktadır. Belirli bir fonksiyon serileştirilmiş bir nesnede sağlanan veriler üzerinde tehlikeli bir metot çağırmaktadır. Lab’ın tamamlanması için carlos kullanıcısının home dizininde bulunan morale.txt’nin silinmesi gerekmektedir.

Uygulamaya avatar yüklenebilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/3/1.png){: .align-center}

Account silinmek istendiğinde account ile ilişkili avatar görseli de dizin üzerinden silinebilmektedir ve bu bilgi serialization halde session bilgisinde tutulmaktadır. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/3/2.png){: .align-center}

Base64 ile decoding işlemi yapıp sonrasında morale.txt dosyasını hedefleyen path yazılmaktadır.(s:23 değeri path basamağı kadar ayarlanmaktadır.) Daha sonrasında account delete işlemi gerçekleştirildiğinde morale.txt dosyası silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/3/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/3/4.png){: .align-center}


## Lab-4 Arbitrary object injection in PHP

Bu laboratuvar, serialization-based bir session mekanızması kullanmaktadır ve rastgele nesne enjeksiyonuna karşı savunmasızdır. Lab’ın tamamlanması için morale.txt dosyasının silinmesi gerekmektedir. Lab’ın çözülmesi için kaynak kod erişimi elde edilmesi gerekmektedir.

Uygulamada CustomTemplate.php adlı bir dosya bulunmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/4/1.png){: .align-center}

Tilda (~) kullanılarak php dosyası istendiğinde dosyanın içeriği görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/4/2.png){: .align-center}

Dosya incelendiğinde CustomTemplate class’ı çağırıldığında, __destruct magic metodu eğer lock_file_path değişkenine bir değer atandıysa local_file_path değişkenini silmektedir. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/4/3.png){: .align-center}

Session bilgisinde lock_file_path değişkenine morale.txt dosyasının path’i yazılarak gönderilmektedir ve morale.txt silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/4/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/4/5.png){: .align-center}

## Lab-5 Exploiting Java deserialization with Apache Commons

Bu laboratuvar, serialization-based session mekanızması kullanmaktadır ve Apache Commons Collections kütüphanesini yükler. Lab’ın tamamlanması için RCE yükü içeren zararlı bir payload serialized bir nesne oluşturulup morale.txt dosyası silinmelidir.

Uygulama Apache Commons kullanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/5/1.png){: .align-center}

ysoserial aracı kullanılarak hedeflenen payload’ı içeren bir çıktı üretilmektedir base64 encode halinde. Daha sonrasında URL encoding yaparak session parametresinde gönderilerek lab tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/5/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/5/3.png){: .align-center}


## Lab-6 Exploiting PHP deserialization with a pre-built gadget chain

Bu laboratuvar, imzalı bir cookie kullanan serialization-based session mekanızmasına sahiptir. Ayrıca bir PHP framework kullanılmaktadır. Lab’ın tamamlanması için Carlos kullanıcısının morale.txt dosyasının silinmesi gerekmektedir.

Session bilgisinde token verileri serialization şeklinden gönderilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/6/1.png){: .align-center}

Token değeri base64 ile encoding yapınca aşağıdaki gibi görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/6/2.png){: .align-center}

Uygulamanın içerisinde debug sayfasının path’i ifşalanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/6/3.png){: .align-center}

İlgili path’de hmac algoritması için kullanılan Secret Key ifşa olmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/6/4.png){: .align-center}

Phpgcc aracı ile (Symfony framework’u olduğu uygulamada ifşa olmuştu) ilgili payload hazırlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/6/5.png){: .align-center}

Daha sonra secret key ve phpgcc aracı aracılıyla oluşturulan zararlı payload kullanılak uygun bir cookie haline getirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/6/6.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/6/7.png){: .align-center}

Son olarak session bilgisi ile değiştirilerek sunucuya gönderilmektedir morale.txt dosyası silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/6/8.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/6/9.png){: .align-center}


## Lab-7 Exploiting Ruby deserialization using a documented gadget chain

Bu laboratuvarda serialization-based session mekanizması ve Ruby on Rails framework kullanmaktadır. Lab’ın tamamlanamsı için morale.txt dosyasının silinmesi gerekmektedir.

Uygulamanın session verisi serialized haldedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/7/1.png){: .align-center}

Universal Deserialisation Gadget for Ruby 2.x-3.x by vakzz on devcraft.io’dan payload generating kısmındaki son script alınarak morale.txt silinecek şekilde hedeflenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/7/2.png){: .align-center}

Alınan hex çıktısı base64 hale getirilerek session bilgisine yazılmaktadır. Sonuç olarak payload yerleştirilmiş request sunucuya gönderilerek hedeflenen dosya silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/7/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/7/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/17-Insecure-Deserialization/7/5.png){: .align-center}


