---
title: "PortSwigger Server-side template injection" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

Server-side template injection, saldırganın bir template’e zararlı bir komut enjekte etmek için template’in syntax’ini kullanmasıdır. 

Template engine’ler sabit template’leri değişken verilerle değiştirerek web sayfaları oluşturmak için tasarlanmıştır. Server-side template injection saldırıları, kullanıcıdan alınan inputları doğrulamadan direkt template ile birleştiği durumlarda gerçekleşir. Saldırgan template engine’i manipüle ederek sunucunun tam kontrolünü ele geçirebilir.


[Lab  1](#lab-1-basic-server-side-template-injection){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-basic-server-side-template-injection-code-context){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-server-side-template-injection-using-documentation){: .btn .btn--danger} [Lab 4](#lab-4-server-side-template-injection-in-an-unknown-language-with-a-documented-exploit){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-server-side-template-injection-with-information-disclosure-via-user-supplied-objects){: .btn .btn--danger}{: .text-left}  [Lab 6](#lab-6-server-side-template-injection-in-a-sandboxed-environment){: .btn .btn--danger}{: .text-left}  [Lab 7](#lab-7-server-side-template-injection-with-a-custom-exploit){: .btn .btn--danger}{: .text-left}

## Lab-1 Basic server-side template injection

Bu laboratuvarda ERB template’inin doğru yapılandırılmamasından kaynaklı server-side template injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için Carlos’un homa dizininden morale.txt silinmelidir.

Uygulamadaki “message” parametresi alınan inputu kullanıcıya geri yansıtmaktadır. Potansiyel bir SSTI zafiyeti oluşabilir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/1/1.png){: .align-center}

Uygulama ERB template kullandığı bilindiğinden ilgili payload parametre ile gönderilmektedir. 49 yanıtı, template engine’in inputu işleyerek çıkan sonucu yazdırdığını göstermektedir. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/1/2.png){: .align-center}

Böylelikle istenilen komut yürütülebilir ve carlos dizini altındaki morale.txt silinebilir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/1/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/1/3.png){: .align-center}


## Lab-2 Basic server-side template injection (code context)

Bu laboratuvarda Tornado template’inin güvenli olmayan şekilde kullanılmasından kaynaklı server-side template injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için Carlos dizini altındaki morale.txt dosyasını silen bir SSTI saldırısı gerçekleştirilmelidir.

Kullanıcı profilinde nickname veya isim olarak gezinme ayarı bulunmakta. Bu ayar değiştirildiğinde input kullanıcıya geri yansıtılmaktadır. Aşağıdaki görselde zafiyetin tespiti için gerekli payload görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/2/1.png){: .align-center}

Payload sunucu tarafında hata vermektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/2/2.png){: .align-center}

Template engine python tabanlı olduğundan süslü parantezler ile bir ifade yazıldıktan sonra payload yazılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/2/3.png){: .align-center}

Bu deneme başarı olup 49 değerini görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/2/4.png){: .align-center}

Sunucu üzerinde işletim sistemi komutu çalıştıracak ve carlos dizini altındaki morale.txt dosyasını silecek payload gönderilerek lab tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/2/5.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/2/6.png){: .align-center}


## Lab-3 Server-side template injection using documentation

Bu laboratuvarda server-side template injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için template engine’in tespit edilmesi ve keyfi komut çalıştırarak hedef sunucuda bulunan Carlos dizini altındaki morale.txt dosyasının silinmesi gerekmektedir.

Uygulamadaki ürünlerin açıklama kısmını değiştirebilmekteyiz. ${deneme} gibi bir tanım denendiğinde uygulama hata vermektedir ve hata mesajında template engine’in FreeMarker olduğu görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/3/1.png){: .align-center}

FreeMarker dökümanlarında okuma yaparak hedef işletim sistemi üzerinde morale.txt dosyasını silmeye yarayan komut oluşturulmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/3/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/3/4.png){: .align-center}



## Lab-4 Server-side template injection in an unknown language with a documented exploit

Bu laboratuvarda server-side template injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için template engine’in tespit edilip carlos dizini altında bulunan morale.txt dosyasının silinmesi gerekmektedir.

Uygulamada bulunan message parametresi üzerinde random payload’lar deneyerek bir hata mesajı elde edilmektedir. Hata mesajında template engine’in handlebars olduğu görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/4/1.png){: .align-center}

Handlebars hakkında araştırma yapılarak aşağıdaki görselde görüntüleneceği üzere işletim sistemi üzerinde komut yürütme payload’ına erişilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/4/2.png){: .align-center}

URL encoding yaparak ilgili payload sunucuya gönderildiğinde lab tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/4/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/4/4.png){: .align-center}


## Lab-5 Server-side template injection with information disclosure via user-supplied objects

Bu laboratuvarda server-side template injection güvenlik zafiyeti bulunmaktadır. Bu güvenlik zafiyeti hassas verileri ifşa etmektedir. Lab’ın tamamlanması için framework’ün secret key bilgisi elde edilmelidir.

Çeşitli payload’lar denenerek hata mesajı alınmaktadır ve hata mesajında kullanılan template tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/5/1.png){: .align-center}

Django’da secret key verisini alabilemek için aşağıdaki kod yazılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/5/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/5/3.png){: .align-center}

## Lab-6 Server-side template injection in a sandboxed environment

Bu laboratuvar Freemarker Template engine kullanmaktadır. Laboratuvarda sandbox’un yanlış yapılandırılmasından kaynaklı server-side template injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için sandbox’un atlatılıp carlos dizini altındaki my_password.txt dosyasının okunması gerekmektedir.

Template engine’in Freemarker oldu bilindiğinden araştırma yapıldığında Sandbox bypass adı altında bir payload bulunmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/6/1.png){: .align-center}

Uygulamanın default olarak verdiği text’te daha önce article’ın product olduğu görüntülenmişti ilgili payload düzenlenerek hedef uygulamaya gönderilmektedir ve id verisi okunmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/6/2.png){: .align-center}

Lab’ın istediği parola dosyasının okunması için payload düzenlenerek tekrar gönderilmektedir. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/6/3.png){: .align-center}


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/6/4.png){: .align-center}

## Lab-7 Server-side template injection with a custom exploit

Bu laboratuvarda server-side template injection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için carlos dizinindeki /.ssh/id_rsa dosyasının silinmesi gerekmektedir. 

Uygulamada avatar yükleme fonksiyonu bulunmaktadır ve invalid bir dosya gönderildiğinde aşağıdaki hata mesajı alınmaktadır. Bu hata mesajı bize User.setAvatar() fonksiyonu gibi güzel bir ipucu vermektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/7/1.png){: .align-center}

Daha sonrasında uygulamada bulunan lakap-gerçek isim değiştirme requesti incelendiğinde user.setAvatar’ı kullanılabilecek potansiyel bir input alanı gözlenmektedir. /etc/passwd dosyasının okunması için payload oluşturulmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/7/2.png){: .align-center}

Daha sonra wiener kullanıcısının avatarına gidildiğinde /etc/passwd dosyası görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/7/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/7/4.png){: .align-center}

Carlos kullanıcısının id_rsa verisini okumak için payload düzenlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/7/5.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/7/6.png){: .align-center}

Laboratuvar bizden dosyanın silinmesini istemektedir. İlk görseldeki hata mesajında elde ettiğimiz User.php dosyasının içeriği incelendiğinde gdprDelete() isimli bir fonksiyon tespit edilmiştir. Bu fonksiyon avatar’ı silmektedir. Avatar, carlos kullanıcısının id_rsa dosyası olarak az önceki gibi ayarlanıp gdprDelete() fonksiyonu çağırıldığında id_rsa dosyası silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/7/7.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/11-Server-Side-Template-Injection/7/8.png){: .align-center}

