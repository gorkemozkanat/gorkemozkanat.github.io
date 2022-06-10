---
title: "PortSwigger Clickjacking Labs" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

Clickjacking, kurbanların kandırılarak sahte bir web sitesindeki tıklanabilir içeriğe tıklamasından sonra gizli bir web sitesindeki başka bir eylemi gerçekleştirmek için tasarlanan arayüz tabanlı bir saldırıdır.

Örneğin kurban ödül kazanmak için bir butona tıklar ve bilmeden saldırgan tarafından gizlenmiş başka bir web sitesinde ödeme yapar. Teknik bir iframe içinde bir düğme veya gizli bağlantı içeren görünmez işlem yapılabilir web sayfasının dahil edilmesiyle alakalıdır.


[Lab  1](#lab-1-basic-clickjacking-with-csrf-token-protection){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-clickjacking-with-form-input-data-prefilled-from-a-url-parameter){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-clickjacking-with-a-frame-buster-script){: .btn .btn--danger} [Lab 4](#lab-4-exploiting-clickjacking-vulnerability-to-trigger-dom-based-xss){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-multistep-clickjacking){: .btn .btn--danger}{: .text-left}


## Lab-1 Basic clickjacking with CSRF token protection

Bu laboratuvardaki oturum açma fonksiyonu ve CSRF token tarafından korunan bir hesap sil butonu içermektedir. Lab’ın tamamlanması için hesap silmeye neden olan bir Clickjacking saldırısı gerçekleştirilmelidir.

wiener:peter bilgileri ile giriş yaptıktan sonra My Account kısmında Delete Account butonu görüntülenmektedir. iframe kullanarak ilgili sayfanın üzerine sahte bir web sayfası hazırlanmaktadır ve tam Delete account butonunun üzerine kurbanın tıklaması için tıklanılabilir bir nesne oluşturulmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/1/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/1/2.png){: .align-center}

Daha sonrasında sahte web sitesi kurbana gönderilerek Click Me adlı sahte butona tıklanması beklenmektedir. Kurban butona tıklayınca hesabı silinerek tab tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/1/3.png){: .align-center}

## Lab-2 Clickjacking with form input data prefilled from a URL parameter

Bu lab bir önceki lab’ın devamıdır. Lab’ın amacı URL parametresini kullanarak bir formu öncedne doldurup, kullanıcıyı “E-posta güncelle” butonuna tıklatmaktır.

Verilen bilgilerle giriş yaptıktan sonra Update email butonu görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/2/1.png){: .align-center}

Kurbanın Update email butonuna tıklaması için sahte iframe hazırlanmaktadır. Kurban butona tıkladığında eposta zararlı web sitesinde ayarlanmış olan e-postaya dönüşmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/2/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/2/3.png){: .align-center}

## Lab-3 Clickjacking with a frame buster script

Bu lab web sitesinin frame’lenmesini önleyen bir frame buster kullanmaktadır. Lab, frame buster’ı atlatıp e-posta değiştirmeye neden olan bir Clickjacking saldırısı düzenlenmesini istemektedir.

Verilen bilgiler ile hedef sisteme giriş yapıldığında aşağıdaki görsel görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/3/1.png){: .align-center}

Sahte bir web sitesi ayarlayıp kurbana tıklatarak e-posta verisini değiştirmeyi hedeflemekteyiz. Fakat frame buster bu süreci zorlaştırmaktadır. Buna çözüm iframe attribute olan sandbox değerini allow-forms olarak değiştirmektir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/3/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/3/3.png){: .align-center}

## Lab-4 Exploiting clickjacking vulnerability to trigger DOM-based XSS

Bu lab tıklama ile tetiklen bir XSS güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için kullanıcının “Click Me” butonuna basması sonucu oluşan bir alert() komutu çalıştırılmalıdır.

Hedef web sitesindeki geri bildirim fonksiyonunda bulunan “name” değerinde XSS güvenlik zafiyeti bulunmaktadır. Kurban “Click me” butonuna tıkladıktan sonra payload geribildirim fonksiyonuna giderek XSS’i tetiklemektedir ve bu durum da print() fonksiyonunu çalıştırmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/4/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/4/2.png){: .align-center}

## Lab-5 Multistep clickjacking

Bu lab CSRF token tarafından korunan bazı fonksiyonlara ve Clickjacking’e karşı koruma sağlamak için onay iletişim kutusuna sahiptir. Lab’ın tamamlanması için “Click me first” and “Click me next” sahte butonlarına kurbanın tıklaması gerekmektir. Bu durum gerçekleştiğinde kurbanın hesabı silinmektedir ve lab tamamlanmaktadır.

Hesap silme fonksiyonu iki adımlıdır önce Delete Account dedikten sonra Yes butonuna tıklanması gerekmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/5/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/5/2.png){: .align-center}

Bu yüzden sahte web sitesinde iki adımlı bir tıklama süreci bulunması gerekmektedir. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/5/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/5/4.png){: .align-center}

Kurban Test me first butonuna tıkladıktan sonra açılan pencerede Test me next butonuna tıklamaktadır. Paylodan kurban kullanıcıya gönderilmeden önce butonlardaki “Text” yazıları “Click” yazısıyla değiştirilmelidir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/5/5.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/4-Clickjacking/5/6.png){: .align-center}


