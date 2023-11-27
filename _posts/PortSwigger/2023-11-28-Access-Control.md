---
title: "PortSwigger Access Control Vulnerabilities" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

Access control (yada Authorization), eylemlerin veya kaynakların kimin veya neyin ulaşabileceğine ilişkin kısıtlamaların uygulanmasıdır. Web uygulamalarında Access control, authentication ve session management’e bağlıdır.

- **Authentication**, kullanıcıyı tanımlar ve söylediği kişi olduğunu onaylar.
- **Session Management**, aynı kullanıcı tarafından hangi HTTP istekleri yaptığını tanımlar.
- **Access control**, kullanıcının gerçekleştirmeye çalıştığı eylemi gerçekleştirmesine izin verilip verilmediğini belirler.

Broken acess controls, yaygın olarak karşılaşılan ve genellikle kritik bir güvenlik zafiyetidir. Erişim kontrollerinin tasarımı ve yönetimi dinamik bir sorundur. Erişim kontrolü tasarım kararları teknoloji tarafından değil insan tarafından verilmelidir ve hata potansiyeli yüksektir.

Kullanıcı açısından bakıldığında access controls aşağıdaki kategorilere ayrılabilir:

- Vertical access controls
- Horizontal access control
- Context-depentent access controls

**Vertical Access Control**

Dikey erişim kontrolleri ile farklı tipteki kullanıcılar farklı uygulama işlevlerine erişebilir. Örneğin, sıradan bir kullanıcının bu işlemlere erişimi yokken bir yönetici herhangi bir kullanıcının hesabını değiştirebilir veya silebilir. Dikey erişim denetimleri görevler ayrılığı ve en az ayrıcalık gibi iş ilkelerini uygulamak için tasarlanmış güvenlik modellerinin daha ayrıntılı uygulamaları olabilir.

**Horizontal Access Control**

Horizontal access control, kaynaklara erişimi, bu kaynaklara erişmelerine özel olarak izin verilen kullanıcılarla sınırlayan mekanizmalardır. Yatay erişim denetimleriyle farklı kullanıcılar aynı türdeki kaynakların bir alt kümesine erişebilir. Örneğin, bir bankacılık uygulaması bir kullanıcının işlemleri görüntülemesine ve kendi hesaplarıdan ödeme yapmasına izin verir ancak başka bir kullanıcının hesaplarına izin vermez.

**Context-dependent access control**

Context-dependent access controls, uygulamanın durumuna veya kullanıcının onunla etkileşimine bağlı olarak işlevlere ve kaynaklara erişimi kısıtlar. Context dependent acces controls, kullanıcının eylemleri yanlış sırada gerçekleştirmesini engeller. Örneğin kullanıcıların ödeme yaptıktan sonra alışveriş sepetlerinin içeriğini değiştirmesini engelleyebilir.


[Lab  1](#lab-1-unprotected-admin-functionality){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-unprotected-admin-functionality-with-unpredictable-url){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-user-role-controlled-by-request-parameter){: .btn .btn--danger} [Lab 4](#lab-4-user-role-can-be-modified-in-user-profile){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-user-id-controlled-by-request-parameter){: .btn .btn--danger}{: .text-left}  [Lab 6](#lab-6-user-id-controlled-by-request-parameter-with-unpredictable-user-ids){: .btn .btn--danger}{: .text-left}  [Lab 7](#lab-7-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect){: .btn .btn--danger}{: .text-left} [Lab  8](#lab-8-user-id-controlled-by-request-parameter-with-password-disclosure){: .btn .btn--danger}{: .text-right} [Lab  9](#lab-9-insecure-direct-object-references){: .btn .btn--danger}{: .text-right} [Lab  10](#lab-10-url-based-access-control-can-be-circumvented){: .btn .btn--danger}{: .text-right} [Lab  11](#lab-11-method-based-access-control-can-be-circumvented){: .btn .btn--danger}{: .text-right} [Lab  12](#lab-12-multi-step-process-with-no-access-control-on-one-step){: .btn .btn--danger}{: .text-right} [Lab  13](#lab-13-referer-based-access-control){: .btn .btn--danger}{: .text-right} 



## Lab-1 Unprotected admin functionality

Bu laboratuvarda korunmasız bir admin paneli bulunmaktadır. Lab’ın tamamlanması için carlos kullanıcısının silinmesi gerekmektedir.

robots.txt dosyasında admin panel endpoint’i görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/1/1.png){: .align-center}

Admin paneline ulaştıktan sonra herhangi bir güvenlik önlemi olmadığından carlos kullanıcısı silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/1/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/1/3.png){: .align-center}


## Lab-2 Unprotected admin functionality with unpredictable URL

Bu laboratuvarda korunmasız admin paneli bulunmaktadır. Panelin konumu tahmin edilebilir bir konumda değildir ancak uygulama admin panelini bir yerde ifşalamaktadır. Lab’ın tamamlanması için carlos kullanıcısının silinmesi gerekmektedir.

Anasayfanın kaynak kodları incelendiğinde admin panelinin ifşası görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/2/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/2/2.png){: .align-center}

## Lab-3 User role controlled by request parameter

Bu laboratuvarın admin paneli /admin dizininde bulunmaktadır ve forgeable cookie kullanılarak kullanıcıları tanımlamaktadır. Lab’ın tammalanması için carlos kullanıcısının silinmesi gerekmektedir.

admin paneline gidildiğinde giriş yapılamadığı gözlemlenmektedir çünkü cookie verisinde admin değeri false’dur. Cookie verisi değiştirip true yapıldığında admin paneline ulaşılabilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/3/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/3/2.png){: .align-center}



## Lab-4 User role can be modified in user profile

Bu laboratuvardaki admin paneli /admin dizininde bulunmaktadır. roleid 2 olan oturum açmış kullanıcılar tarafından erişilebilir. Lab’ın tamamlanması için carlos kullanıcısının silinmesi gerekmektedir.

E-posta değiştirme fonksiyonundaki response’da “roleid” değeri atanmaktadır. Request yakalanıp “roleid”:””2” verisi eklenerek gönderildiğinde response’da id değerinin 2 olarak ayarlandığı gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/4/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/4/2.png){: .align-center}

## Lab-5 User ID controlled by request parameter

Bu laboratuvardaki user account sayfasında horizontal privesc güvenlik zafiyeti bulunmaktadır. Lab’ın tammalanması için Carlos kullanıcısının API bilgisi submit edilmelidir.

Wiener kullanıcısı ile giriş yaptıktan sonra my account linkine tıklandığındaki oluşan requestte id değerinde wiener kullanıcısının gittiği görüntülenmektedir. Bu id değeri carlos ile değiştirildiğinde carlos kullanıcısının account sayfasına gitmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/5/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/5/2.png){: .align-center}

## Lab-6 User ID controlled by request parameter, with unpredictable user IDs

Bu laboratuvardaki user account sayfasında horizontal privesc güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için carlos’un API key’i girilmelidir.

Blogpostlar incelendiğinde carlos kullanıcısına ailt bir post görüntülenmektedir ve incelendiğinde URL’de carlos kullanıcısının id değeri görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/6/1.png){: .align-center}

My account sayfasına gitmek istendiğinde id değerine göre sayfa görüntülenmektedir. Daha önce tespit edilen carlos kullanıcısının id değeri yazılarak carlos kullanıcısının account sayfasına erişilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/6/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/6/3.png){: .align-center}

## Lab-7 User ID controlled by request parameter with data leakage in redirect

Bu laboratuvarda yeniden yönlendirme yanıtında hassas bilgilerin sızdırdığı bir access control güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için carlos kullanıcısının API bilgisi girilmelidir.

My account sayfasını için request oluşturulduğunda id değeri carlos olarak değiştirip gönderilmektedir. Fakat giriş sağlanamamaktadır uygulama redirect yaparak login sayfasına geri yönlendirmektedir. Bu sırada carlos kullanıcısının API verisi saldırgana sızmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/7/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/7/2.png){: .align-center}


## Lab-8 User ID controlled by request parameter with password disclosure

Bu laboratuvar current kullanıcının parolasını içeren bir user account sayfasına sahiptir. Lab’ın tamamlanması için carlos kullanıcısının silinmesi gerekmektedir.

My account requestindeki id parametresi administrator olarak dğeiştirildiğinde gelen response’da kullanıcının parolası ifşa olmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/8/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/8/2.png){: .align-center}


## Lab-9 Insecure direct object references

Bu laboratuvar kullanıcı log kayıtlarını doğrudan sunucunun dosya sisteminde depolar ve bunları statil URL’ler kullanarak alır. Lab’ın tamamlanması için carlos kullanıcısının parolasını bulunması ve hesabına giriş yapılması gerekmektedir. 

Live chat uygulamasının log kayıtları indirilebilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/9/1.png){: .align-center}

İndirme requestı yakalanıp, 2.txt değişkeni 1.txt’ye değiştirilerek sunucuya gönderilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/9/2.png){: .align-center}

Sunucu bize 1.txt göndermektedir ve içerisinde carlos kullanıcısının parolası bulunmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/9/3.png){: .align-center}


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/9/4.png){: .align-center}


## Lab-10 URL-based access control can be circumvented

Bu laboratuvarın /admin dizininde unauthenticated admin paneli bulunmaktadır. Fakat front-end sistem harici erişimi engellemektedir. Back-end uygulaması X-Original-Url header bilgisini destekleyen bir framework üzerine kurulmuştur. Lab’ın tamamlanması için carlos kullanıcısının silinmesi gerekmektedir.

Bazı uygulamalar, başlık değerinde belirtilenle isteklerde hedef URL'nin geçersiz kılınmasına izin vermek için X-Original-URL veya X-Rewrite-URL gibi standart olmayan başlıkları destekler. /admin dizini erişimi reddettiği için X-Origin-URL header’ı /admin olarak ayarlanıp carlos kullanıcısı silinebilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/10/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/10/2.png){: .align-center}

## Lab-11 Method-based access control can be circumvented

Bu laboratuvar, kismi bir HTTP istek yönetimine dayalı erişim kontrolü uygulamaktadır. Lab’ın tamamlanması için administrator kullanıcı yetkisi elde edilmelidir.

Administator olarak giriş yaptıktan sonra carlos kullanıcısını admin kullanıcısına yükseltmek için oluşturulan request yakalanmaktadır. Daha sonrasında administrator kullanıcısının session bilgisi weiner kullanıcısının session bilgisi ile değiştirilip sunucuya istek yapıldığında kullanıcının yetkisiz olduğu response’u alınmıştır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/11/1.png){: .align-center}

POST metodu POSTX olarak değiştirip istek tekrarlandığında “missing parametre” hatası görüntülenmektedir. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/11/2.png){: .align-center}

Bu hata ile GET isteği ile sunucu tarafına istekte bulunabileceğimizi anlamaktayız. Requestin metodunu GET olarak değiştirip parametreler ayarlanarak sunucuya istek yapılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/11/3.png){: .align-center}

Yetkisiz session bilgileri ile yetkisiz şekilde bir kullanıcıyı admin yetkilerine yükseltme yapılarak lab tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/11/4.png){: .align-center}

## Lab-12 Multi-step process with no access control on one step

Bu laboratuvarda, kullanıcının rolünü değiştirmek için kusurlu çok adımlı bir sürece sahip admin paneli bulunmaktadır. Lab’ın tamamlanması için güvenlik zafiyeti istismar edilerek administrator yetkileri elde edilmelidir.

Administrator kullanıcısı ile carlos kullanıcısının admin yetkilerine yükseltme requesti iki adımdan oluşmaktadır ve onay parametresi belirten ikinci request yakalanmaktadır. Session bilgileri wiener kullanıcısının yetkisiz session bilgileri ile değiştirilmektedir. Daha sonra username parametresi wiener olarak ayarlanarak istek yapıldığında lab tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/12/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/12/2.png){: .align-center}

## Lab-13 Referer-based access control

Bu laboratuvar, Referer header’ına dayalı olarak belirli yönetici işlevlerine erişimi kontrol etmektedir. Lab’ın tamamlanması için administrator kullanıcı yetkilerine yükseltme yapmak gerekmektedir.

Administator hesabı ile giriş yapıp carlos kullanıcısını admin yetkilerine upgrade eden request yakalanmaktadır. Daha sonrasında wiener kullanıcısının yetkisiz session bilgileri admin session bilgileri ile değiştirilmektedir. Username= wiener ve action=upgrade olarak ayarlandıktan sonra istek gönderildiğinde işlem gerçekleşmemekte “yetkisiz” hatası dönmektedir.

Referer header’ı aşağıdaki görseldeki gibi ayarlanarak tekrar request yapıldığında işlem gerçekleşmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/13/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/13-Access-Control/13/2.png){: .align-center}



