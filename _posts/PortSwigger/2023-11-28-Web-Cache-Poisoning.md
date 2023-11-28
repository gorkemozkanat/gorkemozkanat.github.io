---
title: "PortSwigger Web Cache Poisoning" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

Web cache poisoning, saldırganın web sunucusunun ve cache’in davranışından yararlanarak diğer kullanıcılara zararlı bir HTTP yanıtı sunduğu gelişmiş bir tekniktir.

Temel olarak, web cache poisoning iki aşamadan oluşur. İlk olarak, saldırganın back-end sunucusundan  bir zararlı payload yanıtı elde etmesi gerekmektedir. Başarılı olunduktan sonra yanıtların önbelleğe alındığından ve ardından hedeflenen kurbanlara dağıtıldığından emin olunması gerekmektedir.


[Lab  1](#lab-1-web-cache-poisoning-with-an-unkeyed-header){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-web-cache-poisoning-with-an-unkeyed-cookie){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-web-cache-poisoning-with-multiple-headers){: .btn .btn--danger} [Lab 4](#lab-4-targeted-web-cache-poisoning-using-an-unknown-header){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-web-cache-poisoning-via-an-unkeyed-query-string){: .btn .btn--danger}{: .text-left}  [Lab 6](#lab-6-web-cache-poisoning-via-an-unkeyed-query-parameter){: .btn .btn--danger}{: .text-left}  [Lab 7](#lab-7-parameter-cloaking){: .btn .btn--danger}{: .text-left} [Lab  8](#lab-8-web-cache-poisoning-via-a-fat-get-request){: .btn .btn--danger}{: .text-right} [Lab  9](#lab-9-url-normalization){: .btn .btn--danger}{: .text-right}

## Lab-1 Web cache poisoning with an unkeyed header

Bu laboratuvar, enkeyed bir header’dan gelen girdiyi güvenli olmayan bir şekilde işlediğinden web cache posisoning güvenlik zafiyetine neden olmaktadır. Lab’ın tamamlanması için kurbanın tarayıcısında alert(document.cookie) fonksiyonu çalıştırılmalıdır.

Uygulamanın home page’ine gönderilen requestin response’unda X-Cache: hit/miss gibi bir header görüntülenmektedir. Bu header uygulamanın cache kullandığını göstermektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/1/1.png){: .align-center}

Dönen response’da ilgili lab URL’inin de yansıdığı görüntülenmektedir. Bu değeri kontrol edebildiğimiz için bir saldırı gerçekleşebilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/1/2.png){: .align-center}

Eğer requestte X-Forwarded-Host header’ı bulunuyorsa uygulama öncelikle bu header’ı host olarak kabul etmektedir. Bu headerı kontrol edebildiğimiz için saldırı düzenlenebilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/1/3.png){: .align-center}

Exploit sunucusuna görseldeki gibi zararlı payload ayarlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/1/4.png){: .align-center}

Uygulamanın home page’ine X-Forwarded-Host başlığı exploit sunucusunu işaret edecek şekilde ayarlanarak request yapılır ve cache’de tutulduğundan emin olunur.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/1/5.png){: .align-center}

Kurban home page’i ziyaret ettiğinde XSS gerçekleşir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/1/6.png){: .align-center}

## Lab-2 Web cache poisoning with an unkeyed cookie

Bu laboratuvar, cookie cache key’ine dahil edilmediğinden web cache poisoning güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için hedef kullanıcının tarayıcısında alert(1) fonksiyonu yürütülmelidir.

Uygulama Cookie bilgilerini göndermektedir ve response’da tekrar bize yansımaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/2/1.png){: .align-center}

Cookie bilgisi üzerinde oynamalar yaparak web cache poisoning saldırısı gerçekleştirilmektedir. Kurban home page’e gittiğinde alert tetiklenecektir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/2/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/2/3.png){: .align-center}

## Lab-3 Web cache poisoning with multiple headers

Bu laboratuvardaki birden fazla header bilgisi kullanılarak zararlı istekler gerçekleştirip web cache poisoning saldırısı gerçekleştirilebilmektedir. Lab’ın tamamlanması için alert(document.cookie) fonksiyonu kurban tarayıcısında çalıştırılmalıdır.

X-Forwarded-For başlığı kullanıldığında response’da herhangi bir yansıma görüntülenmemektedir. Ancak X-Forwarded-Sheme başlığı girildiğinde response 302 dönmektedir ve dönen response’da bulunan Location başlığı yönlendirilen URL’i göstermektedir. Tek bir fark vardır HTTPS haline gitirmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/3/1.png){: .align-center}

X-Forwarded-Host başlığına exploit sunucusu girilerek request yapıldığında Location header bilgisi exploit sunucusu olarak görüntülenmektedir. Yani isteği exploit sunucumuza redirect yaptıracak şekilde ayarlayabildik.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/3/2.png){: .align-center}

Exploit sunucusundaki endpoint /resources/js/tracking.js olarak ayarlanıp istek o endpoint’e yapılacak şekilde ayarlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/3/3.png){: .align-center}

Kurban home page e geldiğinde otomatik olarak tracking.js dosyasını isteyecektir. Web cache de bulunan exploit sunucusuna yönlenecektir ve payload çalışacaktır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/3/4.png){: .align-center}


## Lab-4 Targeted web cache poisoning using an unknown header

Bu laboratuvarda web cache poisoning güvenlik zafiyeti bulunmaktadır. Kurban kullanıcı gönderilen bütün yorumları görüntülemektedir. Lab’ın tamamlanması için kurbanın tarayıcısında alert(document.cookie) yürütülmelidir.

Param miner uygulaması kullanılarak hedef uygulamanın kabul ettiği header bilgisi X-Host olarak bulunmaktadır. X-Host headerına ne yazılırsa response’da döndüğü gözlemlenmiştir. Ayrıca cache key olarak User-Agent kullanıldığı gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/4/1.png){: .align-center}

Uygulamadaki yorum yapma fonksiyonunda XSS güvenlik zafiyeti bulunmaktadır. Başarılı bir Web cache poisoning saldirisi için XSS kullanılarak kurbanın User-Agent bilgileri ele geçirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/4/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/4/3.png){: .align-center}

Ele geçirilen User-Agent verisi hazırlanan requestte kullanılarak web cache poisoning saldırısı gerçekleştirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/4/4.png){: .align-center}

## Lab-5 Web cache poisoning via an unkeyed query string

Bu laboratuvar, query string’in unkeyed olduğundan web cache poisoning güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için kurbanın tarayıcısında alert(1) fonksiyonu çağırılmalıdır.

Uygulama cache yaparken query string değerini key olarak almadan yapmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/5/1.png){: .align-center}

Payload query string olarak gönderildiğinde ve cache poisoning yapıldığında home page’i ziyaret eden bütün kurbanlar ilgili payload’a maruz kalmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/5/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/5/3.png){: .align-center}


## Lab-6 Web cache poisoning via an unkeyed query parameter

BU laboratuvar, belirli bir parametreyi cache key’i olarak almadığından web cache poisoning güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için kurbanın tarayıcısında alert(1) fonksiyonu çalıştırılmalıdır.

Uygulama aldığı query stringi cache key’e eklemektedir. İlk request miss, ikinci request hit geldiğinden cache’lendiği anlaşılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/6/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/6/2.png){: .align-center}

utm_content header’ı uygulama tarafından cache unkeyed olarak işlendiğinden payload cache tutulur ve kurbanlara dağıtılır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/6/3.png){: .align-center}


## Lab-7 Parameter cloaking

Bu laboratuvar belirli bir parametreyi cache key’e dahil etmediğinden web cache poisoning güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için kurbanın tarayıcısında alert(1) fonksiyonu çalıştırılmalıdır.

utm_content cache key işlemine dahil edilmemektedir. Ayrıca utm_content parametresini “;” karakteri ile ayırıp ikinci bir parametre yazıldığında da uygulama tek bir parametre olarak algılayıp cache key’e dahil etmeyecektir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/7/1.png){: .align-center}

Home page ziyaret edildiğinde uygulama bir js dosyası çağırmaktadır ve bu request cache işlemine dahildir. Çağırılan js dosyasının parametresi de cache key işlemine dahil olduğundan cache poisoning yapılamamaktadır. Ancak utm_contet parametresi cache key’e dahil değildir ve “;” karakteri ile ikinci bir parametre dahil edilebilmektedir. Uygulama iki parametreyi algılamaktadır ancak web cache tek parametre olarak davranmaktadır. Bu yüzden ikinci callback parametresi response dahil edilmektedir ve alert çalışmaktadır.

```jsx
GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert(1)
```

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/7/2.png){: .align-center}


## Lab-8 Web cache poisoning via a fat GET request

Bu laboratuvar web cache poisoning güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için kurbanın tarayıcısında alert(1) fonksiyonu yürütülmelidir.

Query string’de gönderilen callback parametresi body’de de gönderildiğinde uygulama ikinci callback parametresini kabul etmektedir ve sunucudan ikinci parametredeki değer dönmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/8/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/8/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/8/3.png){: .align-center}

## Lab-9 URL normalization

Bu laboratuvar URL encoding nedeniyle doğrudan yararlanılamıyan bir XSS güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için web cache poisoning güvenlik zafiyetinden yararlanılarak XSS saldırısı gerçekleştirilmelidir. 

Uygulamada olmayan bir end pointe GET isteği gönderince response olarak olmayan endpointi yazmaktadır. (Aynı zamanda web cache işlemi de yapmaktadır uygulama) Eğer request ile istenilen endpoint’e bir payload yazılırsa response’da görüntülenecektir ve cache işlemi yapılacağından saldırganlara dağıtılabilecektir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/9/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/9/2.png){: .align-center}

Ancak URL tarayıcıda talep edilirse URL-encoded olacağından alert çalışmayacaktır. Bu yüzden Burp Repeater gibi bir araç kullanılarak cache poisoning hale getirilir ve URL hemen kurbana iletilir. Daha sonrasında cache tarafıdan URL-decode işlemi gerçekleştirileceğinden alert fonksiyonu çalışır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/9/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/16-Web-Cache-Poisoning/9/4.png){: .align-center}
