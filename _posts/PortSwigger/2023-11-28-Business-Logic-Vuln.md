---
title: "PortSwigger Business Logic Vulnerabilities" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

Business logic güvenlik zafiyetleri, saldırganın istenmeyen davranışlar gerçekleştirmesine izin veren uygulamanın tasarım ve uygulamasındaki güvenlik zafiyetidir. Saldırgan, potansiyel olarak kendi çıkarları için valid fonksiyonları kullanabilmektedir. Business logic güvenlik zafiyetleri genel olarak uygulamanın olağan dışı durumları nasıl ele alacağını düşünüp, değerlendirilmediği durumlarda ortaya çıkmaktadır.

Logic zafiyetleri genellikle uygulamanın normal kullanımları sırasında ortaya çıkmayacağından bu zafiyetleri aramayan kimseler tarafından görüntülenemez. Ancak saldırgan bu yaklaşımları ele alarak çıkar sağlayabilmektedir.


[Lab  1](#lab-1-excessive-trust-in-client-side-controls){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-high-level-logic-vulnerability){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-inconsistent-security-controls){: .btn .btn--danger} [Lab 4](#lab-4-flawed-enforcement-of-business-rules){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-low-level-logic-flaw){: .btn .btn--danger}{: .text-left}  [Lab 6](#lab-6-inconsistent-handling-of-exceptional-input){: .btn .btn--danger}{: .text-left}  [Lab 7](#lab-7-weak-isolation-on-dual-use-endpoint){: .btn .btn--danger}{: .text-left} [Lab  8](#lab-8-insufficient-workflow-validation){: .btn .btn--danger}{: .text-right} [Lab  9](#lab-9-authentication-bypass-via-flawed-state-machine){: .btn .btn--danger}{: .text-right} [Lab  10](#lab-10-infinite-money-logic-flaw){: .btn .btn--danger}{: .text-right}



## Lab-1 Excessive trust in client-side controls

Bu laboratuvar, kullanıcıdan aldığı girdiyi yeterince doğrulamamaktadır.  Lab’ın tamamlanması için “Leather Jacket” satın alınmalıdır.

Ürün sepete eklenirken ücreti bilgisi de sepete gitmekte. Eğer bu değer 1 olarak değiştirilerse, sepette ürünün değeri 1 dolar olmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/1/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/1/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/1/3.png){: .align-center}


## Lab-2 High-level logic vulnerability

Bu laboratuvar, kullanıcıdan aldığı girdiği yeteri kadar kontrol etmemektedir. Lab’ın tamamlanması için “Leather Jacket” ürününün satın alınması gerekmektedir.

Ürün sepete eklendiğinde “quantity” parametresi görüntülenmektedir. Bu parametreye negatif bir değer verildiğinde sepet tarafında negatif bir ücret oluşmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/2/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/2/2.png){: .align-center}

Buradan yola çıkarak başka bir ürün negatif şekilde sepete eklenmektedir. Checkout yapabilmek için total’in pozitif bir değer olması gerektiğinden hedeflenen ceket sepete yerleştirilmektedir. Lab. tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/2/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/2/4.png){: .align-center}

## Lab-3 Inconsistent security controls

Bu laboratuvardaki logic zafiyeti, rastgele kullanıcıların sadece çalışanların erişmesi gereken admin yetkilerine erişebilmesine neden olmaktadır. Lab’ın tamamlanması için admin paneline erişip Carlos kullanıcısının silinmesi gerekmektedir.

Uygulamadaki admin arayüzüne ulaşabilmek için çalışan olmak gerekmektedir ve çalışanların dontwannacry adresi ile kayıt olması istenmektedir.


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/3/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/3/2.png){: .align-center}

Kayıt olduktan sonra update email fonksiyonu kullanılarak email sadece çalışan email’i haline getirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/3/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/3/4.png){: .align-center}



## Lab-4 Flawed enforcement of business rules

Bu laboratuvardaki satın alma iş akışında logic güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için “Leather Jacket” ürünün alınması gerekmektedir.

Uygulama yeni müşterilere ve bültene kaydolanlara indirim kuponu vermektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/4/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/4/2.png){: .align-center}

Aynı kuponlar peşpeşe kullanılamamaktadır fakat kuponlar birbiri ardına kullanıldığında uygulama zafiyetli davranıp kabul etmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/4/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/4/4.png){: .align-center}

## Lab-5 Low-level logic flaw

Bu laboratuvar, kullanıcıdan aldığı girdileri yeteri kadar doğrulamamaktadır. Lab’ın tamamlanması için “Leather Jacket” ürününün satın alınması gerekmektedir.

Uygulama sepete ürün eklerken tek seferde sadece 99 adet ürün eklemektedir. Burp Intuder ile sonsuz şekilde request gönderecek hale getirmek için null payload kullanıp “continue indefinitely” seçeneği seçilmelidir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/5/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/5/2.png){: .align-center}

Ödenilmesi gereken ürün negatif sonsuza doğru gittikten sonra pozitif sonsuza doğru geri dönme eğilimi göstermektedir. Bu sırada 0 a en yakın negatif yerde ıntruder durdurulmalıdır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/5/3.png){: .align-center}

Total değerinin pozitif olabilmesi gerekmektedir bu yüzden başka ürünler de eklenmelidir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/5/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/5/5.png){: .align-center}

## Lab-6 Inconsistent handling of exceptional input

Bu laboratuvar kullanıcıdan aldığı girdiyi doğru kontrol etmemektedir. Lab’ın tamamlanması için admin paneline erişip Carlos kullanıcısının silinmesi gerekmektedir.

Uygulamaya uzun bir email ile kayıt olunabilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/6/1.png){: .align-center}

Ancak uygulamanın email değeri için belirli bir karakter sınırı bulunmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/6/2.png){: .align-center}

Admin paneline ulaşabilmek için dontwannacry maili ile kayıt olmak gerekmektedir.  “YETERİ-KADAR-UZUN-STRING@dontwannacry.YOUREMAIL-ID.web.security-academy.net “ email bilgisi ile kayıt olunduğunda mail onayı bizim sunucumuza gelmektedir ancak uygulama sadece dontwannacry[.]com kısmına kadar almaktadır. Bu yüzden bizi çalışan olarak değerlendirmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/6/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/6/4.png){: .align-center}


## Lab-7 Weak isolation on dual-use endpoint

Bu laboratuvar, kullanıcı girdilerine dayalı bir ayrıcalık düzeyi hakkında hatalı varsayımda bulunmaktadır. Lab’ın tamamlanması için Carlos kullanıcısının silinmesi gerekmektedir.

Uygulamada parola değiştirme requesti oluşturduğumuzda kullanıcı adı geçerli parola ve yeni parolayı sormaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/7/1.png){: .align-center}

Ancak uygulama yeteri kadar validation yapmadığından current-password parametresi girilmeden de request gönderilebilmektedir. Böylelikle administrator kullanıcı ele geçirilerek carlos kullanıcısı silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/7/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/7/3.png){: .align-center}


## Lab-8 Insufficient workflow validation

Bu laboratuvar, satın alma workflow’u hakkında hatalı varsayımda bulunmaktadır. Lab’ın tamamlanması için “Leather Jacket” ürünü satın alınmalıdır.

Paranın yetebileceği bir ürün alındığında uygulama sunucuya siparişin onaylandığı hakkında bir request göndermektedir. Bu request saklanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/8/1.png){: .align-center}

Daha sonra istenilen çeket ürünü alınmaya çalışıldığında hesaptaki para yetmediğinden sunucuya siparişin onaylanmadığına dair bir request göndermek istemektedir. Bu request yakalanarak droplanmaktadır ve daha önce yakalanan siparişi onaylayan request onun yerine gönderilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/8/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/8/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/8/4.png){: .align-center}


## Lab-9 Authentication bypass via flawed state machine

Bu laboratuvardanın login işleminde, güvenlik zafiyeti barındıran bir varsayımda bulunmaktadır. Lab’ın tamamlanamsı için Carlos kullanıcısının silinmesi gerekmektedir.

Uygulamaya login olunmaya çalışıldığında role-selector seçimini çağırıp yetkimizi seçmemizi istemektedir. Ancak görseldeği request sunucuya gönderilmeden droplanırsa ve home page ziyaret edilirse administrator yetkileri varsayılan olarak ayarlanmış olacaktır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/9/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/9/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/9/3.png){: .align-center}

## Lab-10 Infinite money logic flaw

Bu laboratuvarda bulunan satın alma işleminde logic zafiyet bulunmaktadır. Lab’ın tamamlanması için “Leather Jacket” satın alınmalıdır.

Gift card aldıktan sonra indirip kuponu girildiğinde 10 dolar veren ama maaliyeti 7 dolar olan bir akış gerçekleşmektedir. Bu akış macro yardımıyla otomatize edilerek sonsuz paraya doğru ilerlenebilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/10/1.png){: .align-center}

7 dolar vererek Gift card satın alma + indirim kuponu gerçekleştirme+ checkout+ confirmed+ gift card verisi ile 10 dolar geri alma

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/10/2.png){: .align-center}

Intruder yardımıyla bu akış tekrarlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/10/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/19-Business-Logic/10/4.png){: .align-center}

