---
title: "PortSwigger OAuth Authentication" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

OAuth, web sitelerinin ve uygulamalarının başka bir uygulamadaki bir kullanıcının hesabına sınırlı erişim talep etmesini sağlayan ve yaygın olarak kullanılan bir authorization frameworküdür.  OAuth, kullanıcının oturum açma bilgilerini istekte bulunan uygulamaya göstermeden bu erişimi vermesine izin verir. Bu durum kullanıcıların hesaplarının tam kontrolünü üçüncü bir tarafa devretmek zorunda kalmadan hangi verileri paylaşmak istediklerine ince ayar yapabilecekleri anlamına gelmektedir.

OAuth 2.0 geçerli standart olmasına rağmen bazı web siteleri halen OAuth 1.0 sürümünü kullanmaktadır. OAuth 2.0, OAuth 1.0 üzerine geliştirilmemiş, sıfırdan geliştirilme yapılmıştır. İki sürüm birbirinden çok farklıdır.


[Lab  1](#lab-1-authentication-bypass-via-oauth-implicit-flow){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-forced-oauth-profile-linking){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-oauth-account-hijacking-via-redirect_uri){: .btn .btn--danger} [Lab 4](#lab-4-stealing-oauth-access-tokens-via-an-open-redirect){: .btn .btn--danger}{: .text-left} [Lab 5](#lab-5-ssrf-via-openid-dynamic-client-registration){: .btn .btn--danger}{: .text-left}


## Lab-1 Authentication bypass via OAuth implicit flow

Bu laboratuvar, sosyal medya accountları ile login olmaya izin veren bir OAuth mekanizması kullanmaktadır. Sunucu tarafındaki bir unsafe validation uygulamayı savunmasız bırakmaktadır. Lab’ın tamamlanması için Carlos hesabı ile giriş yapılmalıdır.

Uygulamada oturum açmak istendiğinde, uygulama bir sosyal medya sitesine yönlendirerek OAuth gerçekleştirmek istemektedir. Wiener kullanıcı bilgileri ile sosyal medyada oturum açtıktan sonra bize, hedef uygulama ile paylaşılacak bilgilerin (scope) iznini istemektedir. İzin verildikten sonra sosyal medya uygulaması, hedef sunucuya bir request aracılığıyla bilgileri göndermektedir. O sırada request yakalanıp e-mail bilgisi hedef kullanıcı olan carlos’un mail bilgileri ile değiştirilerek gönderildiğinde carlos kullanıcısının hesabı ele geçirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/1/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/1/2.png){: .align-center}


## Lab-2 Forced OAuth profile linking

Bu laboratuvar, normal kullanıcı adı ve parola kullanmak yereine OAuth aracılığıyla oturum açmaya olanak sağlamaktadır. Lab’ın tamamlanması için CSRF saldırısı kullanılarak Admin paneline erişip Carlos kullanıcısı silinmelidir.

Uygulamada sosyal medya ile login olma flow’unda hedef uygulamanın istediği bilgilere onay verildikten sonra sosyal medya uygulamasından bir onay kodu çıkmaktadır. Bu onay kodu hedef uygulamaya gittiğinde ilgili hesap ile eşleştirilerek OAuth tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/2/1.png){: .align-center}

Ancak bu onay kodu CSRF saldırısı ile admin bilgileriyle hedef uygulamaya giderse, Admin hesabını wiener sosyal medya hesabıyla bağlamaktadır. Daha sonrasında wiener kullanıcısı sosyal medya hesabıyla bağlanmak istediğinde admin hesabıyla bağlanacaktır..

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/2/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/2/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/2/4.png){: .align-center}

## Lab-3 OAuth account hijacking via redirect_uri

Bu laboratuvar, kullanıcıların sosyal medya hesaplarıyla oturum açmasına izin vermek için bir OAuth mekanizması kullanmaktadır. Lab’ın tamamlanması için admin kullanıcısının authorization kodunun çalınarak carlos kullanıcısı silinmelidir.

Uygulama, sosyal medya uygulamasına login olmak için giderken bir redirect işlemi yapmaktadır. Sosyal medya tarafında bütün doğrulamalar gerçekleştiğinde onay kodu ile redirect_url de belirtilen yere dönmesi gerekmektedir uygulama. Ancak redirect_uri değiştirilerek gönderildiğinde uygulama bir validation yapmadığından sorun olmamaktadır. Onay kodu değiştirilmiş url’e gitmektedir.


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/3/1.png){: .align-center}

Bu durum kullanılarak bir request hazırlanır ve hedef kurban olan admine iletilir. Admin linki ziyaret ederek sosyal medya uygulamasında login olup geri kod ile birlikte exploit sunucusuna dönmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/3/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/3/3.png){: .align-center}

Daha sonra saldırgan sosyal medya hesabı ile login olmayı denediği akışta en son basamak olan kendi onay kodunu hedef uygulamaya göndermez, admine ait olan çalınmış onay kodu ile değiştirerek hedef uygulamaya gönderilir. Böylelikle admin kullanıcısı olarak oturum açar.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/3/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/3/5.png){: .align-center}

## Lab-4 Stealing OAuth access tokens via an open redirect

Bu laboratuvar, kullanıcıların sosyal medya hesaplarıyla oturum açmasına izin veren bir OAuth hizmeti kullanmaktadır. Lab’ın tamamlanması için admin hesabı ele geçirilmelidir.

Uygulmadaki blog postlar arasında hareket ederken Open Redirect güvenlik zafiyeti bulunmaktadır. Path parametresi değiştirilerek hedeflenen URL’e gidilebilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/4/1.png){: .align-center}

OAuth doğrulama yaparken redirect_uri değeri validation işlemini path bilgisi de dahil olmak üzere yapmaktadır ancak parametrelere dikkat etmemektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/4/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/4/3.png){: .align-center}

Exploit server’da kurbanın tıkladığında OAuth mekanizmasına gidecek bir payload hazırlanmaktadır. Uygulama “oauth-callback” verisine kadar validation işlemi yapmaktadır. Validation işlemi yapıldıktan sonra, tarayıcı payload’u bir üst dizine (Directory Traversal) çıkartılarak Open Redirect parametlerine eriştirecektir. Daha sonrasında redirect işlemi gerçekleşerek kurbanın token bilgisi saldırganın sunucusuna gelecektir. (Uygulama token bilgisini location.hash’de tutmaktadır.)

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/4/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/4/5.png){: .align-center}

Daha sonrasında wiener kullanıcısı ile OAuth işlemi yaparken token bilgisi çalınan admin token bilgisiyle değiştirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/4/6.png){: .align-center}

Request History incelendiğinde “/me” requestinin response’unda adminin apıkey bilgisi bulunmaktadır. Eğer orda adminin değil de wiener kullanıcısının apı bilgisi görünyorsa, requestin Authorization başlığındaki token bilgisi adminin token bilgisi ile değiştirilerek gönderilmelidir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/4/7.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/4/8.png){: .align-center}

## Lab-5 SSRF via OpenID dynamic client registration

Bu laboratuvar, client uygulamaların, bir endpoint aracılığıyla OAuth işlemine izin vermektedir. Lab’ın tamamlanması için bir SSRF saldırısı gerçekleştirip admin kullanıcısının secret key’i ele geçirilmelidir.

OAuth URL’inde hidden directory araması yapıldığında .well-known/openid-configuration path’i bulunmaktadır. Tespit edilen pathde bir registration uygulaması bulunmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/5/1.png){: .align-center}

/req endpointine yapılan bir post request’i ile birlikte herhangi bir kimlik doğrulama gerektirmeden istemci uygulaması başarılı bir şekilde kaydedilmektedir. Yanıtta client_id dahilş olmak üzere çeşitli datalar dönmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/5/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/5/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/5/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/5/5.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/21-OAuth-Authentication/5/6.png){: .align-center}
