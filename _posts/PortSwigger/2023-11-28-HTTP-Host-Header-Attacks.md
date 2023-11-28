---
title: "PortSwigger HTTP Host Header Attacks" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

HTTP Host header HTTP/1.1 requestlerinde zorunlu başlıklardan biridir. İstemcinin erişmek istediği yeri göstermektedir. Örn: “https://gorkemozkanat.github.io/” url adresine gidilmek istendiğinde request aşağıdaki gibi olmaktadır.

```jsx
GET /web-security HTTP/1.1
Host: gorkemozkanat.github.io
```

Tarihsel olarak her ip sadece bir içerik barındırdığı dönemlerde böyle bir host header bilgisine ihtiyaç bulunmamaktaydı. Ancak Virtual host, loadbalancer, reverse proxy veya CDN gibi teknolojilerden dolayı bir Ip’nin birden fazla domain name bilgisine sahip olabilir ve içeriği de doğal olarak farklı olabilir. Bu durumun önüne geçilmesi için host başlığı erişilmek istenen spesifik domain’i belirtmektedir.

Host header’ın doğru ve yeteri şekilde kontrol edilmemesi bazı güvenlik zafiyetlerine yol açmaktadır. Eğer sunucu host header’ına sonsuz şekilde güveniliyorsa ve host header verisini doğru şekilde kontrol etmiyorsa zararlı payload’lar sunucu tarafını manipüle edebilmektedir.


[Lab  1](#lab-1-basic-password-reset-poisoning){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-host-header-authentication-bypass){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-routing-based-ssrf){: .btn .btn--danger} [Lab 4](#lab-4-ssrf-via-flawed-request-parsing){: .btn .btn--danger}{: .text-left}


## Lab-1 Basic password reset poisoning

Parola sıfırlama requesti gönderirken host bilgisi bizim erişimimizde olan exploit sunucu ile değiştirilmektedir ve parolasının değişmesini istediğimiz kullanıcı carlos olmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/1/1.png){: .align-center}

parola sıfırlama token bilgisini ele geçirerek carlos kullanıcısının parolası değiştirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/1/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/1/3.png){: .align-center}


## Lab-2 Host header authentication bypass

Bu laboratuvar, HTTP host header’a  bağlı olarak ayrıcalık düzeyi hakkında varsayımda bulunmaktadır. Lab’ın tamamlanması için carlos kullanıcısının silinmesi gerekmektedir.

admin paneline sadece yerel ağdan bağlanılabilmektedir. Bu yüzden host başlığı bilgisi localhost olarak değiştirildiğinde erişim sağlanabilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/2/1.png){: .align-center}

carlos kullanıcısını silerken de yine host başlığı değiştirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/2/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/2/3.png){: .align-center}

## Lab-3 Routing-based SSRF

Bu laboratuvarda host header aracılığıyla routing-based SSRF güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanamsı için admin paneline erişilim carlos kullanıcısının silinmesi gerekmektedir.

Anasayfayı çağıran bir requestte host başlığı burp collaborator olarak değiştirildiğinde callaborator’de dns ve http istekleri görüntülenmektedir. Uygulama host header ile bir doğrulama, kontrol yaptığı düşünülmektedir.


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/3/1.png){: .align-center}

Host header’ı 192.168.0.0/24 range arasında ıntruder yardımıyla denendiğinde sadece bir ip’den 302 dönmektedir. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/3/2.png){: .align-center}

/admin paneline erişmek için host değeri 192.168.0.236 olarak değiştirip request gerçekleştirildiğinde hata alınmadan erişilmektedir ve carlos kullanısı silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/3/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/3/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/3/5.png){: .align-center}

## Lab-4 SSRF via flawed request parsing

Bu laboratuvarda, parsing’in zafiyetli olmasından kaynaklı routing-based SSRF bulunmaktadır.  Lab’ın tamamlanması için admin panelinden carlos kullanıcısının silinmesi gerekmektedir.

Request üzerinde host başlığı değiştirildiğinde uygulama hemen engellemektedir. Ancak / yerine sitenin uzun halini yazıp requesti gerçekleştiğimizde uygulama host değerini doğrulamayı bırakıp girilen URL’e bakmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/4/1.png){: .align-center}

Bu durumdan faydalanılarak hedefe URL verisi girilir ve host header’ına da 192.168.0.0/24 range’i verilerek yetkili ip tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/4/2.png){: .align-center}

Yetkili ip bulunduktan sonra host bilgisine eklenir ve /admin değeri url/admin olarak değiştirilerek request tamamlandığında işlem gerçekleşir. Bu durum carlos kullanıcısını silmek için de kullanılarak lab tamamlanır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/4/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/20-HTTP-Host-Header-Attacks/4/5.png){: .align-center}

