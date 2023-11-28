---
title: "PortSwigger Information Disclosure" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

Information disclosure (aka Information Leakage), bir web uygulamasının istemeden hassas bilgileri ifşa etmesidir. İçeriğe bağlı olarak aşağıdakiler de dahil olmak üzere bir çok bilgiyi sızdırabilir:

- Kullanıcı adı, finansal bilgiler gibi kullanıcılar hakkındaki veriler
- Hassas mesleki veya ticari veri
- Web site ve onun yapısı hakkında teknik detay

Kullanıcı bilgi sızıntısı veya iş verilerinin sızıntısının tehlikesi çok barizdir ancak teknik bilgilerin sızıntısı da bazen çok önemli olabilmektedir. Saldırganlar tarafından saldırı yüzeyini arttırma olarak kullanılabilmektedir. Toplanabilen bilgi, karmaşık ve yüksek saldırılar oluşturmaya çalışırken oldukça kullanışlı olmaktadır.

**Bazı Information Disclosure örnekleri:**

- Gizli dizinlerin, yapıların ve içeriklerin, ***robots.txt*** gibi dosya ve dizinler aracılığıyla ifşası
- Geçici backup’lar aracılığıyla kaynak kodların ifşası
- Veritabanı satırı veya sütunu isimlerinin hata mesajlarında bahsedilmesi
- Kredi kartı bilgileri gibi son derece yüksek öneme sahip bilgilerin ifşası
- Hard-coded API keys, IP adresi, veritabanı creds. ve kaynak kodlarının ifşası
- Kullanıcı adı veya kaynakların, uygulamanın farklı davranışlarından kaynaklı kullanıcının (veya saldırganın) tespit edebileceği şekilde ipuclarının ifşası


[Lab  1](#lab-1-modifying-serialized-objects){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-information-disclosure-on-debug-page){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-source-code-disclosure-via-backup-files){: .btn .btn--danger} [Lab 4](#lab-4-authentication-bypass-via-information-disclosure){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-information-disclosure-in-version-control-history){: .btn .btn--danger}{: .text-left}  

## Lab-1 Modifying serialized objects

Bu laboratuvardaki hata mesajının detayları güvenlik zafiyeti bulunan third-part bir framework’un sürüm bilgisini ifşa etmektedir. Lab’ın tamamlanması için framework’un sürüm bilgisi elde edilmelidir.

Uygulamadaki bir ürünü görüntülerken URL’de productId parametresi dikkat çekmektedir. Parametreye “2-1” girdisi verilerek sürüm bilgisini içeren hata mesajı elde edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/1/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/1/2.png){: .align-center}

## Lab-2 Information disclosure on debug page

Bu laboratuvardaki debug sayfası hassas verileri ifşa etmektedir. Lab’ın tamamlanması için SECRET_KEY verisinin elde edilmesi gerekmektedir.

BurpSuite uygulamasında Target—>Site Map —> Engagement Tools —> Find Comments şeklinde ilerleyerek hedef uygulama üzerindeki yorum satırları tespit edilebilmektedir. Tespit edilen uygulamarda gizli dizinler ifşa olmuştur ve bu dizinler uygulama özelinde bilgilere sahip olmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/2/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/2/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/2/3.png){: .align-center}

## Lab-3 Source code disclosure via backup files

Bu laboratuvar gizli dizinde bulunan bir backup dosyası aracılığıyla uygulama hakkında kaynak kod ifşalamaktadır. Lab’ın tamamlanması için kaynak kod içerisinde hard-coded olarak bulunan veritabanı parolası ele geçirilmelidir.

Uygulamanın backup dizininde backup dosyası bulunmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/3/1.png){: .align-center}

Kaynak kod bulunan içerikte veritabanı bağlantısı için gerekli parola bilgisi de ifşa olmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/3/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/3/3.png){: .align-center}


## Lab-4 Authentication bypass via information disclosure

Bu laboratuvardaki admin arayüzü authentication bypass güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için carlos kullanıcısının silinmesi gerekmetkedir.

/admin dizinine bir request yapıldığında yetkisiz olunduğu mesajı alınmaktadır. TRACE HTTP metodu ile denendiğinde uygulamanın X-Custom-IP-Authorization başlığı ile ip bilgisini göderdiği gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/4/1.png){: .align-center}

Uygulama sadece local kullanıcıların admin arayüzüne erişimi olduğundan proxy options match-replace kısmındaki replace değerine ilgili header ve değeri atanarak bu sorun ortadan kaldırılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/4/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/4/3.png){: .align-center}

## Lab-5 Information disclosure in version control history

Bu laboratuvarda, sürüm kontrol geçmişinde hassas veri ifşa olmaktadır. Lab’ın tamamlanması için administrator kullanıcısının parolası ele geçirilerek Carlos kullanıcısı silinmelidir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/18-Information-Disclosure/5/1.png){: .align-center}



