---
title: "PortSwigger Authentication" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

Authentication, belirli bir kullanıcı veya istemcinin kimliğini doğrulama işlemidir. Web siteleri internete bağlanan herkese açık olduğundan güvenilir kimlik doğrulama mekanizmaları, web güvenliğinin ayrılmaz bir yönüdür.

Kimlik doğrulama mekanizmalarında güvenlik zafiyeti genel olarak iki yoldan açığa çıkmaktadır.

- Kimlik doğrulama mekanizmaları brute-force saldırılarına karşı yeterince koruma sağlamaz.
- Uygulamadaki mantık kusurları veya zayıf kodlama, kimlik doğrulama mekanizmalarının bir saldırgan tarafından tamamaen atlanmasına izin verir. Bu bazen “broken authentication” olarak adlandırılır.


[Lab  1](#lab-1-username-enumeration-via-different){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-2fa-simple-bypass){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-password-reset-broken-logic){: .btn .btn--danger} [Lab 4](#lab-4-username-enumeration-via-subtly-different-responses){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-username-enumeration-via-response-timing){: .btn .btn--danger}{: .text-left}  [Lab 6](#lab-6-broken-brute-force-protection-ip-block){: .btn .btn--danger}{: .text-left}  [Lab 7](#lab-7-username-enumeration-via-account-lock){: .btn .btn--danger}{: .text-left} [Lab  8](#lab-8-2fa-broken-logic){: .btn .btn--danger}{: .text-right} [Lab  9](#lab-9-brute-forcing-a-stay-logged-in-cookie){: .btn .btn--danger}{: .text-right} [Lab  10](#lab-10-offline-password-cracking){: .btn .btn--danger}{: .text-right} [Lab  11](#lab-11-password-reset-poisoning-via-middleware){: .btn .btn--danger}{: .text-right} [Lab  12](#lab-12-password-brute-force-via-password-change){: .btn .btn--danger}{: .text-right} [Lab  13](#lab-13-broken-brute-force-protection-multiple-credentials-per-request){: .btn .btn--danger}



## Lab-1 Username enumeration via different 

Bu laboratuvarın username enumeration ve password brute-force saldırılar karşı güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için geçerli bir kullanıcı tespit edilip sisteme giriş yapılması gerekmektedir. 

Kullancı adı ve parola denendiğinde uygulama “Invalid username” hatası döndürmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/1/1.png){: .align-center}

İstek yakalanarak Kullanıcı adını tespit etmek için bir saldırı gerçekleştirilmektedir. Doğru kullanıcı adı ile login olma denendiğinde Incorrect password hatası vereceğinden username tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/1/2.png){: .align-center}

Valid username sabit tutularak potansiyel passwordler dennemektedir. Doğru password ile login olma denendiğinde Length olarak diğerlerinden farklı bir sonuç döndüreceğinden valid password tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/1/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/1/4.png){: .align-center}

## Lab-2 2FA simple bypass

Bu laboratuvardaki 2FA authentication atlatılabilinmektedir. Lab’ın tamamlanması için Carlos kullanıcısının hesabına giriş yapılmalıdır.

Valid kullanıcı ile giriş yapıp 2FA işlemi gerçekleştirildikten sonra Account sayfasına gidilmektedir ve url not edilmektedir. Daha sonra carlos kullanıcısı ile deneme yapıldığında 2FA sorduğu sırada my-account?id=carlos” yazılarak direkt account sayfasına geçilmektedir 2FA atlatılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/2/1.png){: .align-center}

## Lab-3 Password reset broken logic

Bu laboratuvardaki parola reset fonksiyonunda güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanamsı için Carlos kullanıcısının hesabına giriş yapılmalıdır.

Valid kullanıcı ile parola sıfırlama yaparken yeni parola belirlediğimiz esnada sunucu tarafına kullanıcı adının da gittiği gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/3/1.png){: .align-center}

İlgili requestteki username kısmı carlos olarak değiştirilerek yeni parolası belirlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/3/2.png){: .align-center}


## Lab-4 Username enumeration via subtly different responses

Bu laboratuvar, username enum ve password brute-force saldırısı karşısında savunmasızdır. Lab’ın tamamlanması için geçerli bir kullanıcı adı ve parola ile giriş yapılmalıdır.

Uygulama girilen hatalı bilgileri için “Invalid username or password.” hatası döndürmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/4/1.png){: .align-center}

Potansiyel kullanıcı adıları ile username enum. denendiğinde bir username verisinde hatanın farklı olduğu görüntülenmektedir. “Invalid username or password” (Cümle sonunda nokta yok)

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/4/2.png){: .align-center}

Tespit edilen valid username ile potansiyel parolalar denenerek valid bir hesaba giriş yapılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/4/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/4/4.png){: .align-center}

## Lab-5 Username enumeration via response timing

Bu laboratuvar response time kullanarak kullanıcı adı tespit etme güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için geçerli bir kullanıcı ile uygulamaya giriş yapılması gerekmektedir.

Uygulama kullanıcı adını onayladıktan sonra parolayı onaylamaya çalışmaktadır. Yani geçersiz kullanıcı adları denendiğinde direkt hata verirken geçerli bir kullanıcı adı denendiğinde parolayı da kontrol edeceğinden response timing diğerlerine göre farklı olacaktır.

Uygulama çok fazla yanlış deneme yaptığında blokladığından X-Forward-For header kullanılarak bu atlatılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/5/1.png){: .align-center}

Diğer response timing değerlerinden fazla olduğundan kullanıcı adı mysql olarak tespit edilmiştir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/5/2.png){: .align-center}

Tespit edilen kullanıcı adı ile potansiyel parolalar denenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/5/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/5/4.png){: .align-center}

## Lab-6 Broken brute-force protection, IP block

Bu laboratuvar, brute-force korumasındaki bir mantık hatası nedeniyle korunmasızdır. Lab’ın tamamlanması için carlos kullanıcısı ile giriş yapılmalıdır.

Bir denemede valid id:pw diğer denemede valid id: unknown pw taktiği.

## Lab-7 Username enumeration via account lock

Bu laboratuvarda username enum güvenlik zafiyeti bulunmaktadır. Uygulama koruma için account locking yapmaktadır. Lab’ın tamamlanması için geçerli bir kullanıcı ile giriş yapılmalıdır.

Belirli bir kullanıcı ile birden fazla hatalı parola giriş denemesi yapıldığında hesap 1 dakikalığına kitlenmektedir. Bu kitlenme durumu response’da belirtildiği için diğer response’lardan daha farklı bir length’e sahip olmaktadır. Böylelikle valid bir username tespit edilebilir. Aşağıdaki görselde her bir potansiyel kullanıcı adına 5’er kere aynı parola denenmiştir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/7/1.png){: .align-center}

Tespit edilen valid username ile potansiyel parolalar eşleştiğinde önce parola hatası daha sonra çok fazla deneme yapıldı hatası görüntülenmektedir. Fakat valid bir parola denk geldiğinde uygulama herhangi bir hata döndürmemektedir. Böylelikle parola tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/7/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/7/3.png){: .align-center}

## Lab-8 2FA broken logic

Bu laboratuvardaki 2FA authentication’da güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için Carlos hesabı ile giriş yapılmalıdır.

Valid ıd:pw ile 2FA işlemi yapılmaktadır ve workflow tespit edilmektedir. Aşağıdaki görsel ile carlos kullanıcısı adına 2FA için bir mfa-code istenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/8/1.png){: .align-center}

Daha sonra wiener kullanıcısı için yapılan request alınarak verify parametresi carlos olarak değiştirilir ve mfa-code brute-force ile tespit edilir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/8/2.png){: .align-center}

Tespit edilen mfa-code ile carlos kullanıcısının hesabına erişim sağlanır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/8/3.png){: .align-center}


## Lab-9 Brute-forcing a stay-logged-in cookie

Bu laboratuvar, kullanıcıların tarayıcı oturumlarını kapattıktan sonra bile oturlmarın açık kalmasına olanak tanır. Bu işlevi sağlamak için kullanılan cookie brute-force’a karşı savunmasızdır. Lab’ın tamamlanması için Carlos kullanıcısının hesabına giriş sağlanmalıdır.

wiener kullanıcısı ile giriş yaptıktan sonra stay-loggin cookie verisi incelenmektedir. Cookinin base64 ile encode edildiği gözlemlenmektedir. Aşağıdaki görselde de görüntülendiği üzere stay-loggin cookie bilgisi kullanıcı adı ön eki ve daha sonrasında parolanın md5 halini barındırmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/9/1.png){: .align-center}

Hedeflenen carlos kullanıcısı için /get-myaccount enpointine ön eki carlos olan parolanın md5 olarak hashlendiği ve en sonunda Base64 olarak encode edildiği bir payload gönderilerek parola tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/9/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/9/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/9/4.png){: .align-center}

## Lab-10 Offline password cracking

Bu laboratuvar, kullanıcıların parola karmasını bir cookie bilgisinde saklamaktadır. Lab ayrıca Stored XSS güvenlik zafiyetine de sahiptir. Lab’ın tamamlanması için Carlos kullanıcısının cookie bilgisinin alınması ve parolasının tespit edilerek kullanıcının silinmesi gerekmektedir.

Açıklamada verilen Stored Xss kullanılarak kurbanın cookie bilgisini sahip olunan exploit sunucusuna iletilmektedir. request yapması beklenir. Kullanıcı uygulamaya request yapınca User-Agent header’ındaki payload çalışarak lab tamamlanır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/10/1.png){: .align-center}

Daha sonra kurban Xss bulunan sayfayı ziyaret ettiğinde cookie bilgisi ele geçmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/10/2.png){: .align-center}

Ele geçirilen stay-logged cookie base64 ile decode edilerek parolanın md5 hali elde edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/10/3.png){: .align-center}

Online araçlar sayesinde parola da tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/10/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/10/5.png){: .align-center}

## Lab-11 Password reset poisoning via middleware

Bu lab reset poisoning güvenlik zafiyetine sahiptir. Lab’ın tamamlanması için carlos kullanıcısının hesabına giriş yapılması gerekmektedir.

Parola sıfırlama requestinde X-Forwarded-Host başlığı ekleyerek sıfırlama mail’inin exploit sunucusuna gelmesini sağlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/11/1.png){: .align-center}

temp-forgot-password-token

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/11/2.png){: .align-center}

Daha sonra yeni parola belirleme requestinde token verisi elde edilen carlos kullanıcısının token verisi ile değiştirilerek yeni parola belirlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/11/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/11/4.png){: .align-center}


## Lab-12 Password brute-force via password change

Bu laboratuvarda bulunan parola değiştirme fonksiyonu brute-force saldırılarına karşı savunmasızdır. Lab’ın tamamlanması için carlos kullanıcısı olarak oturum açılması gerekmektedir.

Wiener kullanıcısı olarak parola değiştirme requesti yapılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/12/1.png){: .align-center}

Username carlos olarak değiştirip yeni parolalardan biri hatalı girilmektedir ve potansiyel parola listesini payload list’e eklenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/12/2.png){: .align-center}

Uygulama current-password değerini doğru olduğunda new-password’lere bakmaktadır ve eğer biri hatalıysa “New passwords do not match” hatası döndürmektedir. Bu hata Grep-Match ile kullanılıp current parola elde edilmeye çalışacaktır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/12/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/12/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/12/5.png){: .align-center}

## Lab-13 Broken brute-force protection, multiple credentials per request

Bu laboratuvar, brute-force korumasındaki bir hatadan kaynaklı savunmasızdır. Lab’ın tamamlanması için carlos kullanıcısının parolası tespit edilerek hesaba giriş yapılmalıdır.

Valid kullanıcı ile giriş yapma isteği bulunulduğunda verilerin json halinde gönderildiği gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/13/1.png){: .align-center}

Tek bir istekte birden fazla parolanın denemesi yapılabilmektedir json formatı ile. Bu yüzden kullanıcı adı olarak carlos yazılarak ve potansiyel parolalar eklenerek istek gerçekleştirildiğinde carlos kullanıcısının hesabına erişim sağlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/13/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/14-Authentication/13/3.png){: .align-center}




