---
title: "PortSwigger WebSockets" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

WebSocket’ler, kullanıcı işlemlerini gerçekleştirmek ve hassas bilgileri iletmek de dahil bir çok amaçla kullanılmaktadır. HTTP ile ortaya çıkan hemen hemen her web güvenlik zafiyeti WebSockets işlemleriyle de ortaya çıkabilmektedir. (XSS, CSRF, SQLi, XXE, SSRF)


[Lab  1](#lab-1-manipulating-websocket-messages-to-exploit-vulnerabilities){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-manipulating-the-websocket-handshake-to-exploit-vulnerabilities){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-cross-site-websocket-hijacking){: .btn .btn--danger} 


## Lab-1 Manipulating WebSocket messages to exploit vulnerabilities

Bu laboratuvardaki “live chat” uygulaması WebSockets kullanmaktadır. Gönderilen mesajlar bir destek temsilcisi tarafından anlık olarak görüntülenmektedir. Lab’ın tamamlanması için popun olarak alert() yayınlanmalıdır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/15-WebSockets/1/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/15-WebSockets/1/2.png){: .align-center}


## Lab-2 Manipulating the WebSocket handshake to exploit vulnerabilities

Bu laboratuvardaki “live chat” uygulaması WebSockets kullanmaktadır.  Lab’ın tamamlanması için alert() fonksiyonu çağırılmalıdır.

WebSockets de 
```
<script>alert(1)</script>
```
gibi bir payload denendiğinde uygulama bizi direkt blacklist’e almaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/15-WebSockets/2/1.png){: .align-center}

X-Forwarded-For: 1.1.1.1 yaparak bağlantı tekrar kurulmaktadır ve obfuscated XSS payload’ı gönderilerek uyglama atlatılıp lab tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/15-WebSockets/2/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/15-WebSockets/2/3.png){: .align-center}

## Lab-3 Cross-site WebSocket hijacking

Bu laboratuvardaki “live chat” uygulaması WebSockets kullanmaktadır. Lab’ın tamamlanması için cross-site WebSocket hijacking saldırısı gerçekleştirilerek kurbanın sohbet geçmişi ele geçirilmelidir. Sonrasında kurbanın hesabına giriş yapılmalıdır.

Uygulamanın Chat kısmına gelindiğinde WebSockets sunucuya READY mesajı atmaktadır ve sunucu geri dönüt olarak sohbet geçmişini göndermektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/15-WebSockets/3/1.png){: .align-center}

WebSockets requestleri incelendiğinde CSRF token görüntülenmemektedir. Aşağıdaki payload kurbana iletildiğinde kurbanın sohbet geçmişi exploit sunucusuna gönderilmesi beklenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/15-WebSockets/3/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/15-WebSockets/3/3.png){: .align-center}


