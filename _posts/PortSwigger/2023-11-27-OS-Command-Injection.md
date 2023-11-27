---
title: "PortSwigger OS Command Injection" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

OS command injection saldırganların, hedef işletim sistemi üzerinde keyfi komut çalıştırabilmesine izin veren bir web güvenlik zafiyetidir. Saldırganlar OS command injection güvenlik zafiyetini istismar ederek organizasyonun güvenilen diğer sistemlerine erişebilmektedir.


[Lab  1](#lab-1-os-command-injection-simple-case){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-blind-os-command-injection-with-time-delays){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-blind-os-command-injection-with-output-redirection){: .btn .btn--danger} [Lab 4](#lab-4-blind-os-command-injection-with-out-of-band-interaction){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-blind-os-command-injection-with-out-of-band-data-exfiltration){: .btn .btn--danger}{: .text-left}  


## Lab-1 OS command injection, simple case

OS command injection saldırganların, hedef işletim sistemi üzerinde keyfi komut çalıştırabilmesine izin veren bir web güvenlik zafiyetidir. Saldırganlar OS command injection güvenlik zafiyetini istismar ederek organizasyonun güvenilen diğer sistemlerine erişebilmektedir.

Bu laboratuvarın stock check fonksiyonunda OS command injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için hedef işletim sistemi üzerinde whoami komutu çalıştırılmalıdır.

Yakalanan requestte storeID değerinden sonra pipe karakteri ile istenen whoami komutu eklenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/1/1.png){: .align-center}

whoami komutunu içeren request gönderildiğinde current user bilgisi görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/1/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/1/3.png){: .align-center}


## Lab-2 Blind OS command injection with time delays

Bu laboratuvardaki feedback fonksiyonunda OS command injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için hedef işletim sisteminde 10 saniyelik bir delay oluşturmalıdır.

Feedback fonksiyonu kullanılırken yakalanan requestte localhost’a ping attıracak bir payload yazarak response’un delaylı gelmesi sağlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/2/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/2/2.png){: .align-center}

## Lab-3 Blind OS command injection with output redirection

Bu laboratuvardaki feedback fonksiyonunda blind OS command injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için whoami komutunun çalıştırılıp çıktı almak gerekmektedir.

E-posta parametresine whoami komutunu çalıştıracak ve çıktısını /var/www/images/ klasörü altına yazacak bir payload eklenerek istek gönderilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/3/1.png){: .align-center}

Daha sonra uygulamadaki görselleri GET isteği ile alan bir istek manipüle edilerek daha önce gönderdiğimiz istekteki dosya adı olarak değiştirip current user tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/3/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/3/3.png){: .align-center}



## Lab-4 Blind OS command injection with out-of-band interaction

Bu laboratuvardaki feedback fonksiyonunda blind OS command injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için blind OS command injection saldırısı gerçekleştirerek Collaborator’a bir  DNS lookup yapılması gerekmektedir.

Feedback requesti üzerindeki e-posta parametresine nslookup komutu eklenmektedir ve çıktısının collaborator’a gitmesi sağlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/4/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/4/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/4/3.png){: .align-center}

## Lab-5 Blind OS command injection with out-of-band data exfiltration

Bu laboratuvardaki feedback fonksiyonunda blind OS command injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için whoami komutunun çalıştırılarak çıktının DNS query yoluyla Burp Collaborator sunucusuna çıkartılması sağlanmalıdır.

Feedback requesti yakalanarak e-posta parametresi Burp Collaborator’a whoami komutunun çıktısın DNS query olarak gönderecek komut eklenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/5/1.png){: .align-center}

Elde edilen current user bilgisi girildiğinde lab tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/5/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/10-Os-Command/5/3.png){: .align-center}

