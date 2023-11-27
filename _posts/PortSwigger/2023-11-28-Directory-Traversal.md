---
title: "PortSwigger Directory Traversal" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

Directory traversel ( Path traversal olarak da bilinir), saldırganların hedef sunucuda keyfi dosya okumasına izin veren bir güvenlik zafiyetidir. Bu okunan dosyalar, kod, data, back-end bilgileri veya önemli işletim sistemi dosyaları olabilir. Bazı durumlarda saldırgan sunucu üzerindeki dosyalara yazma eylemi gerçekleştirerek hedef sunucuda full kontrole sahip olabilir.


[Lab  1](#lab-1-file-path-traversal-simple-case){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-file-path-traversal-traversal-sequences-blocked-with-absolute-path-bypass){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-file-path-traversal-traversal-sequences-stripped-non-recursively){: .btn .btn--danger} [Lab 4](#lab-4-file-path-traversal-traversal-sequences-stripped-with-superfluous-url-decode){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-file-path-traversal-validation-of-start-of-path){: .btn .btn--danger}{: .text-left}  [Lab 6](#lab-6-file-path-traversal-validation-of-file-extension-with-null-byte-bypass){: .btn .btn--danger}{: .text-left} 

## Lab-1 File path traversal, simple case

Bu laboratuvardaki product images görüntülenmesinde file path traversal güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için /etc/passwd dosyasının görüntülenmesi gerekmektedir.

Ürün görsellerini incelemek için yapılan request üzerinde “/etc/passwd” payload’ı gönderilerek hedeflenen dosya içeriği görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/1/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/1/2.png){: .align-center}


## Lab-2 File path traversal, traversal sequences blocked with absolute path bypass

Bu laboratuvardaki ürün görsellerini görüntülemede file path traversal güvenlik zafiyeti bulunmaktadır.

Uygulama traversal sequence’leri engellemektedir. Lab’ın tamamlanması için /etc/passwd dosyasının görüntülenmesi gerekmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/2/1.png){: .align-center}


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/2/2.png){: .align-center}


## Lab-3 File path traversal, traversal sequences stripped non-recursively

Bu laboratuvardaki ürün görsellerini görüntülemede file path traversal güvenlik zafiyeti bulunmaktadır. Uygulama kullanıcı tarafından sağlanan dosya adından path traversal dizinini çıkartır. Lab’ın tamamlanması için /etc/passwd dosyasının okunması gerekmektedir.

Uygulama alınan girdideki nokta ve eğik çizgi karakterlerini çıkartmaktadır fakat çıkartma işlemi sadece bir kere yapılmaktadır. Payload ….//….//….//etc/passwd olarak değiştirildiğinde hedeflenen dosyaya erişilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/3/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/3/2.png){: .align-center}



## Lab-4 File path traversal, traversal sequences stripped with superfluous URL-decode

Bu laboratuvardaki ürün görsellerini görüntülerken path traversal güvenlik zafiyeti oluşmaktadır. Uygulama path traversal işlemi için gerekli olan karakterleri engellemektedir. Lab’ın tamamlanması için /etc/passwd dosyasının okunması gerekmektedir.

Payload URL-encoded olarak gönderildiğinde hedeflenen dosya görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/4/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/4/2.png){: .align-center}


## Lab-5 File path traversal, validation of start of path

Bu laboratuvardaki ürün görsellerini görüntülemede path traversal güvenlik zafiyeti bulunmaktadır. Uygulama girilen path’in beklenen klasörde başladığını doğrular. Lab’ın tamamlanması için /etc/passwd dosyasının görüntülenmesi gerekmektedir.

Uygulama girdi olarak aldığı yolun “/var/www/images/” olduğunu doğrulamaktadır. Bu yüzden payload bu doğrulama ile başlamaktadır. Sonrasında dizinlerden geri gidilmektedir ve /etc/passwd dosyası görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/5/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/5/2.png){: .align-center}

## Lab-6 File path traversal, validation of file extension with null byte bypass

Bu laboratuvardaki görseller görüntülenirken path traversal güvenlik zafiyeti oluşmaktadır. Uygulama hedeflenen dosyanın uzantısını doğrulamaktadır. Lab’ın tamamlanması için /etc/passwd dosyasının görüntülenmesi gerekmektedir.

Uygulama girdinin .png .jpg gibi bir uzantı ile bitmesini istemektedir. Null byte kullanılarak path’in uzantıdan sonra bitmesi sağlanabilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/6/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/12-Directory-Traversal/6/2.png){: .align-center}



