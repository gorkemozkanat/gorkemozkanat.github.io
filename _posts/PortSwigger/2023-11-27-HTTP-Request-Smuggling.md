---
title: "PortSwigger HTTP Request Smuggling" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

HTTP request smuggling, bir web sitesinin bir veya birden fazla alınan HTTP requestlerinin işlenme biçimini manipüle etmektdir. Doğası gereği kritik bir güvenlik zafiyetidir. Saldırganın güvenlik denetimlerini atlatmasına, hassas verileri ele geçirmesine ve diğer uygulama kullanıcılarını istismar etmesine yol açabilmektedir.


[Lab  1](#lab-1-http-request-smuggling-basic-clte-vulnerability){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-http-request-smuggling-basic-te-cl-vulnerability){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-http-request-smuggling-obfuscating-the-te-header){: .btn .btn--danger} [Lab 4](#lab-4-http-request-smuggling-confirming-a-clte-vulnerability-via-differential-responses){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-http-request-smuggling-confirming-a-te-cl-vulnerability-via-differential-responses){: .btn .btn--danger}{: .text-left}  [Lab 6](#lab-6-exploiting-http-request-smuggling-to-bypass-front-end-security-controls-cl-te-vulnerability){: .btn .btn--danger}{: .text-left}  [Lab 7](#lab-7-exploiting-http-request-smuggling-to-bypass-front-end-security-controls-te-cl-vulnerability){: .btn .btn--danger}{: .text-left} [Lab  8](#lab-8-exploiting-http-request-smuggling-to-reveal-front-end-request-rewriting){: .btn .btn--danger}{: .text-right} [Lab  9](#lab-9-exploiting-http-request-smuggling-to-capture-other-users-requests){: .btn .btn--danger}{: .text-right} [Lab  10](#lab-10-exploiting-http-request-smuggling-to-deliver-reflected-xss){: .btn .btn--danger}{: .text-right} [Lab  11](#lab-11-response-queue-poisoning-via-h2te-request-smuggling){: .btn .btn--danger}{: .text-right} [Lab  12](#lab-12-h2-cl-request-smuggling){: .btn .btn--danger}{: .text-right} [Lab  13](#lab-13-http2-request-smuggling-via-crlf-injection){: .btn .btn--danger}{: .text-right} [Lab  14](#lab-14-http2-request-splitting-via-crlf-injection){: .btn .btn--danger}{: .text-right} 



## Lab-1 HTTP request smuggling, basic CL.TE vulnerability

Bu laboratuvar, front-end ve back-end sunucusu içermektedir ve front-end chunked encoding’i desteklememektedir. Front-end sunucusu GET veya POST metotlarını kullanmayan istekleri reddetmektedir. Lab’ın tamamlanması için back-end sunucusu tarafından işlenen bir sonraki isteğin GPOST yöntemini kullanıyor gibi görünmesi için back-end sunucusuna bir request gönderilmelidir.

Front-end Content-length değerine bakmaktadır ve “G” harfine kadar olan bütün veriyi back-end’e gönderir. Back-end Transfer-Encoding değerine bakmaktadır ve mesaj body’sini chunked encoding olarak inceler. 0 uzunlukta olduğu belirtilen ilk chunk’ı işler ve isteği sonlandırır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/1/1.png){: .align-center}

Aynı istek ikince kez yapıldığında “G” harf bir sonraki request’in başına ekleneceğinden sunucu GPOST diye bir metotu bilmediğini söylemektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/1/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/1/3.png){: .align-center}


## Lab-2 HTTP request smuggling, basic TE-CL vulnerability

Bu laboratuvar, bir front-end ve back-end sunucusu içermektedir. Back-end sunucusu chunked encoding değerini desteklememektedir. Front-end GET ve POST metotlarını kullanmayan requestleri reddetmektedir. Lab’ın tamamlanması için back-end sunucusu tarafından işlenen bir sonraki isteğin GPOST metodunu kullanıyor gibi görünmesi için sunucuya bir request gönderilmelidir.

Front-end chunked encoding değerini almaktadır ve aşağıdaki request back-end’e gönderilmektedir.Back-end content length değerini aldığından 5c değerinden sonraki verileri işemez. Geri kalan veriler bir sonraki requestin başına eklenir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/2/1.png){: .align-center}

Request ikinci defa gönderildiğinde bir önceki requesten kalan “GPOST” metodu en başta olmaktadır ve back-end GPOST metodunu tanımadığını response da bildirmektedir..

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/2/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/2/3.png){: .align-center}

## Lab-3 HTTP request smuggling, obfuscating the TE header

Bu laboratuvar, bir front-end ve bir back-end sunucusuna sahiptir. HTTP request başlıklarını her ikiside farklı şekilde işlemektedir. Front-end GET ve POST olmayan istekleri kabul etmemektedir. Lab’ın tamamlanması için back-end tarafında işlenen bir sonraki requestin GPOST metodu gibi görünmesi için back-end’e bir request gönderilmelidir.

Front-end ve back-end sunucularının ikiside Transfer-Encoding header’ını desteklemektedir. Fakat sunuculardan biri header’a obfuscate uygulayarak işlenmemesine sebep olabilmektedir.

Potansiyel Transfer-Encoding obfuscate yöntemleri:

```
Transfer-Encoding: xchunked

Transfer-Encoding : chunked

Transfer-Encoding: chunked
Transfer-Encoding: x

Transfer-Encoding:[tab]chunked

[space]Transfer-Encoding: chunked

X: X[\n]Transfer-Encoding: chunked

Transfer-Encoding
: chunked
```

Bir TE.TE güvenlik zafiyetini ortaya çıkarmak için front-end ve backend sunucularından yalnızca birinin TE verisini işleyip diğerinin yok sayması gibi varyasyonlar gerekmektedir.

Obfuscated işleminden sonra saldırının geri kalanı CL.TE veya TE.CL güvenlik zafiyetleriyle aynı hali alacaktır.

Front-end bütün body verisini back-end’e göndermektedir(chunked). Fakat back-end GPOST ile başlayan veriyi işlememektedir.(cow)

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/3/1.png){: .align-center}

Request ikinci defa tekrarlandığında GPOST ile başlayan veriler requestlerin en başında olacağından sunucu GPOST metodu hatası verecektir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/3/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/3/3.png){: .align-center}



## Lab-4 HTTP request smuggling, confirming a CL.TE vulnerability via differential responses

Bu laboratuvarda front-end ve back-end sunucusu bulunmaktadır. Front-end sunucusu chunked encoding değerini desteklememektedir. Lab’ın tamamlanması için “/” için 404 döndürecek bir request back-end’e iletilmelidir.

CL.TE güvenlik zafiyetinin olup olmadığı farklı yöntemler ile de tespit edilebilmektedir. Örneğin aşağıdaki görselde bulunan payload sunucuya iletildiğinde front-end chunked değerini desteklemediğinden bütün body’i back-end’e iletecektir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/4/1.png){: .align-center}

Back-end transfer-encoding değerini desteklediğinden “0” karakterinden sonra gelen bütün verileri bir sonraki request olarak değerlendirecektir. İkinci request iletildiğinde ise 404 Not Found response’u alınarak zafiyetin tespiti gerçekleştirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/4/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/4/3.png){: .align-center}

## Lab-5 HTTP request smuggling, confirming a TE-CL vulnerability via differential responses

Bu laboratuvarda front-end ve back-end sunucusu bulunmaktadır. Back-end sunucusu chunked encoding’i desteklememektedir. Lab’ın tamamlanması için bir sonraki requestte 404 Not Found döndüren bir request back-end sunucusuna gönderilmelidir.

Aşağıdaki payload ilk requeste gönderildiğinde front-end tarafı chunked encoding’i desteklediğinden bütün body’deki veriler back-end’e gönderilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/5/1.png){: .align-center}

Request ikinci defa gönderildiğinde ise back-end chunked encoding değerini desteklemediğinden POST /404 verisi bir sonraki requestin başına geçeceğinden sunucudan 404 Not Found response’u alınmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/5/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/5/2.png){: .align-center}

## Lab-6 Exploiting HTTP request smuggling to bypass front-end security controls, CL-TE vulnerability

Bu laboratuvarda front-end ve back-end sunucusu bulunmaktadır. Fron-end chunked encoding’i desteklememektedir. /admin de bir admin paneli bulunmaktadır fakat fron-end bunu engellemektedir. Lab’ın tamamlanması için admin panelile erişen ve carlos kullanıcısı silen bir request back-end’e gönderilmelidir.

Bazı uygulamalarda front-end sunucusu hangi requestlerin gönderilip hangilerinin gönderilmeyeceğini belirlemek gibi bazı guvenlik kontrollerine sahip olmaktadır. Örneğin, front-end gelen requesti sadece yetkisi varsa yönlendiren bir access control güvenliğine sahiptir. Back-end ise gelen bütün requestleri kontrol etmeden gerçekleştirmektedir. Bu durumda bir HTTP request smuggling saldırısı gerçekleştirilir ve front-end bypass edilirse back-end istenilen endpoint’i saldırgana iletecektir.

CL.TE bulunduğundan aşağıdaki payload sunucuya gönderilirken sorun oluşmamaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/6/1.png){: .align-center}

Request ikinci defa tekrarlandığında ise Admin arayüzüne sadece local erişim olduğu görüntülenmektedir. Yani smuggling işlemi başarılı olmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/6/2.png){: .align-center}

localhost:80 ifadesini parser kullanıcı adı parola olarak algılamaktadır ve  “stock.weliketoshop” ifadesini domain olarak tanımlamaktadır. Böylelikle whitlelist bypass edilmektedir. Daha sonra double-url encoding işlemi uygulanan ”#” karakteri işin içine girdiğinde uygulama bundan sonrasını location hash olarak algılamaktadır ve domain’i localhost olarak değerlendirmektedir. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/6/3.png){: .align-center}

Admin arayüzüne ulaşmak için bir “host: localhost” değeri eklenmektedir. 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/6/4.png){: .align-center}

Request ikinci defa gönderildiğinde iki tane host değeri olacağından sunucu hata vermektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/6/5.png){: .align-center}

Bu yüzden bir x= değeri atanarak ikinci defa request gönderildiğinde gelecek verilerin header olarak değilde body de bulunması sağlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/6/6.png){: .align-center}

Böylelikle iki adet host değeri olmamaktadır ve Admin panele erişim sağlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger9-HTTP-Request-Smuggling/6/7.png){: .align-center}

Daha sonra carlos kullanıcısını silecek request hazırlanarak iki defa gönderilmektedir ve carlos kullanıcısı silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/6/8.png){: .align-center}

## Lab-7 Exploiting HTTP request smuggling to bypass front-end security controls, TE-CL vulnerability

Bu laboratuvarda front-end ve back-end sunucusu bulunmaktadır. Back-end sunucusu chunked encoding’i desteklememektedir. Uygulamada bir admin paneli bulunmaktadır fakat front-end bu panele erişimi engellemektedir. Lab’ın tamamlanması için front-end’i bypass edecek ve back-end’e admin panelini göstermesi için bir HTTP requesti gönderilerek carlos kullanıcısı silinmelidir.

TE.CL bulunduğundan front-end aşağıdaki payload’ı back-end’e iletmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/7/1.png){: .align-center}

Request ikinci defa iletildiğinde response olarak admin arayüzüne sadece local kullanıcıların erişebildiği bilgisi dönmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/7/2.png){: .align-center}

Daha sonra aşağıdaki payload hem local kullanıcı gibi görünecek hem de host header’larının çakışmasını engelleyecek şekilde ayarlanarak back-end’e gönderilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/7/3.png){: .align-center}

Request ikinci defa gönderildiğinde admin paneline erişildiği gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/7/4.png){: .align-center}

Carlos kullanıcısı silecek eklemeler yaparak request back-end’e gönderilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/7/5.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/7/6.png){: .align-center}

## Lab-8 Exploiting HTTP request smuggling to reveal front-end request rewriting

Bu laboratuvarda front-end ve back-end sunucusu bulunmaktadır. Front-end chunked encoding değerini desteklememektedir. Uygulamada bir admin paneli bulunmaktadır fakat sadece 127.0.0.1 ip adresine sahip kullanıcılar bu panele erişebilmektedir. Front-end requestlere X-Forwarded-For header’ına benzer bir Header ekleyerek request sahibinin IP verisini yazmaktadır. Lab’ın tamamlanması için front-end tarafından eklenen header’ın tespit edilmesi ve tespit edilen header kullanılarak admin panelinden carlos kullanıcısını silen bir request oluşturulmalıdır.

Bazı uygulamalarda front-end sunucusu gelen isteğe bazı header bilgileri ekleyerek back-end tarafına göndermektedir. Bazı durumlarda normalde front-end sunucusu tarafından eklenen header’lar saldırgan tarafından eklendiğinde back-end bu durumu düzgün işlemeyebilir.

Bir front-end rewriting işlemini tespit etmek için basit bir yol mevcuttur ve aşağıdaki gibidir.

- Kullanıcıdan bir parametre ile aldığı veriyi yansıtan bir POST requesti bulunmalı.
- Parametre mesaj body’sinin en alt kısmına getirilmeli.
- Back-end sunucusuna request smuggling saldırısı gerçekleştirerek gönderilmeli ve response’ta front-end’in eklediği headerlar görüntülenmelidir.

Örneğin aşağıdaki request iki kez gönderildiğinde (email parametresi kullanıcıya yansıtılmaktadır.)


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/8/1.png){: .align-center}

Aşağıdaki gibi bir response alınmaktadır. Böylelikle front-end sunucusunun hangi header’ları eklediği tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/8/2.png){: .align-center}

Lab’ın search fonksiyonu post metodu ile çalışmaktadır ve aşağıdaki payload iki kere gönderildiğinde kullanıcıya front-end sunucusunun eklediği başlıklar yansımaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/8/3.png){: .align-center}

Front-end sunucusu “X-wXsAVO-Ip:” header’ı ile ip verisini ilettiği gözlemlenmektedir. Admin paneline erişmek için 127.0.0.1 verisi, ilgili header bilgisi ile girilmektedir. Front-end sunucusunun tekrar aynı başlığı eklemesi ile oluşacak çakışma engellenmesi için “x=1” değeri ile message body haline getirerek request gönderildiğinde admin paneline erişim sağlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/8/4.png){: .align-center}

Carlos kullanıcısının silinmesi için request aşağıdaki gibi düzenlenip iki kere gönderilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/8/5.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/8/6.png){: .align-center}

## Lab-9 Exploiting HTTP request smuggling to capture other users' requests

Uygulamada metin bazlı veriler alınıyor veya saklanmasına izin veriliyorsa HTTP smuggling saldırısı ile diğer kullanıcıların istekleri ele geçirilebilir. Token, session ve diğer kullanıcıya ait hassas veriler saldırganın eline geçebilmektedir.

Saldırı gerçekleştirmek için istekte en son konumlanan verileri içeren parametle ile depolama işlevine veri gönderen bir istek kullanılmalıdır. Back-end tarafından işlenen bir sonraki istek diğer kullanıcının ham isteği iteğinin depolanmaıyla sonuçlanacaktır ve smuggled isteğe eklenecektir.

Örneğin aşağıdaki request, blogda depolanacak ve blog yazısı yorumu olarak görüntülecek bir requesttir.

```jsx
POST /post/comment HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 154
Cookie: session=BOe1lFDosZ9lk7NLUpWcG8mjiwbeNZAO

csrf=SmsWiwIJ07Wg5oqX87FfUVkMThn9VzO0&postId=2&comment=My+comment&name=Carlos+Montoya&email=carlos%40normal-user.net&website=https%3A%2F%2Fnormal-user.net
```

Aşağıdaki payload kullanılarak veri depolama isteğini back-end sunucusuna iletip saldırı gerçekleştirilebilinir.

```jsx
GET / HTTP/1.1
Host: vulnerable-website.com
Transfer-Encoding: chunked
Content-Length: 324

0

POST /post/comment HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 400
Cookie: session=BOe1lFDosZ9lk7NLUpWcG8mjiwbeNZAO

csrf=SmsWiwIJ07Wg5oqX87FfUVkMThn9VzO0&postId=2&name=Carlos+Montoya&email=carlos%40normal-user.net&website=https%3A%2F%2Fnormal-user.net&comment=
```

Valid bir kullanıcı uygulamaya bir istek yaptığında yukarıdaki payload’ın sonundaki comment’e bütün request eklenecek ve yorum olarak kullanıcının hassas bilgileri görüntülenecektir.

```jsx
POST /post/comment HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 400
Cookie: session=BOe1lFDosZ9lk7NLUpWcG8mjiwbeNZAO

csrf=SmsWiwIJ07Wg5oqX87FfUVkMThn9VzO0&postId=2&name=Carlos+Montoya&email=carlos%40normal-user.net&website=https%3A%2F%2Fnormal-user.net&comment=GET / HTTP/1.1
Host: vulnerable-website.com
Cookie: session=jJNLJs2RKpbg9EQ7iWrcfzwaTvMw81Rj
...
```

Bu laboratuvarda front-end ve back-end sunucusu bulunmaktadır. Front-end sunucusu chunked encoding’i desteklememektedir. Lab’ın tamamlanması için bir sonraki kullanıcının requestini çalmaya yarayacak bir requesti back-end’e iletilmelidir.

Uygulamanın yorum yapma fonksiyonu POST request ile çalışmaktadır. Aşağıdaki istek hedef sunucuya gönderildiğinde yorum yapma requesti bir sonraki request olarak kalacaktır ve kurban uygulamaya request yaptığında request olarak algılanmayıp yorum olarak algılanacaktır ve ilgili paylaşımın yorumuna post olarak düşecektir.

```jsx
POST / HTTP/1.1
Host: your-lab-id.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 256
Transfer-Encoding: chunked

0

POST /post/comment HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 400
Cookie: session=your-session-token

csrf=your-csrf-token&postId=5&name=Carlos+Montoya&email=carlos%40normal-user.net&website=&comment=test
```

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/9/1.png){: .align-center}

Front-end sunucusunun eklediğin session bilgisi kullanılarak carlos hesabına girip yapılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/9/2.png){: .align-center}

## Lab-10 Exploiting HTTP request smuggling to deliver reflected XSS

Bu laboratuvarda front-end ve back-end sunucusu bulunmaktadır. Front-end sunucusu chunked encoding değerini desteklememektedir. Ayrıca uygulamanın User-Agent header’ında Reflected XSS bulunmaktadır. Lab’ın tamamlanması için bir sonraki kullanıcının isteği ile alert(1) çalıştıracak XSS’li requestin back-end sunucusuna iletilmesi gerekmektedir.

Eğer bir uygulama hem HTTP request smuggling hem Reflected XSS güvenlik zafiyetlerine sahipse bu iki zafiyet kombinlenerek kullanılabilir. XSS’i kendi başına kullanmaktan daha iyi bir saldırıdır çünkü kullanıcı etkileşimi gerekmez.

Aşağıdaki hazırlanmış request back-end’e gönderilir ve kurban kullanıcının bir request yapması beklenir. Kullanıcı uygulamaya request yapınca User-Agent header’ındaki payload çalışarak lab tamamlanır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/10/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/10/2.png){: .align-center}

## Lab-11 Response queue poisoning via H2.TE request smuggling

Queue poisoning saldırısının etkisi çok kritiktir. Saldırı yapıldıktıktan sonra saldırgan sadece rastgele requestler göndererek kullanıcıların response yakalayabilmektedir. Response’lar kullanıcıların hassas verileri ve erişim tokenlerını barındırabilmektedir.

Başarılı bir queue saldırısı için aşağıdakilere ihtiyaç bulunmaktadır.

- Çoklu request/response döngüsü için front-end ve back-end arasındaki TCP bağlantısı gerekmektedir.
- Saldırgan back-end sunuusundan kendi yanıtı alan bağımsız bir isteği başarıyla ve gizlice göndermesi gerekmektedir.
- Saldırı her iki sunucunun da TCP bağlantısını kapatmasına neden olmaz. Sunucular genellikle geçersiz bir istek aldıklarında gelen bağlatınlarını kapatırlar çünkü isteğin nerede bitmesi gerektiğini belirleyemezler.

Bu laboratuvar, request smuggling saldırılarına karşı savunmazısdır. Front-end sunucusu belirsiz uzunluğa sahip olsa bile HTTP/2 isteklerini downgrade yapar. Lab’ın tamamlanması için /admin endpoint’inden carlos kullanıcısını silmeye yarayan response queue poisoning saldırısı yapılması gerekmektedir.

Front-end sunucusu HTTP/2 olan isteği back-end sunucusuna 1.1 olarak tekrar yazarak göndermektedir. Back-end sunucusu chunkend encoding değerini desteklediğinden Get isteği bir sonraki istek olarak algılanacaktır. Get isteğine dönülmesi gereken response için bir request olmadığından ilgili response gönderilmek için bekletilecektir. Kurban bir request yaptığında bekletilen response kullanıcıya gönderilecektir ve saldırgan bir request yaptığında kurbanın response’u saldırgana gönderilecektir. Saldırgan böylece kurbanın response’unu ele geçirecektir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/11/1.png){: .align-center}

Aşağıdaki görseldeki request gönderilerek kuyruk zehirlenmektedir. Admin uygulamaya request yaptığında response olarak 404 alırken gerçek response saldırgana dönmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/11/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/11/3.png){: .align-center}

Admin’in session verisi ele geçirildikten sonra admin panelindeki carlos kullanıcısı silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/11/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/11/5.png){: .align-center}

## Lab-12 H2-CL request smuggling

Bu laboratuvar request smuggling saldırısına karşı savunmasızdır. Front-end sunucusu gelen HTTP/2 isteklerini  downgrade etmektedir. Lab’ın tamamlanması için request smuggling saldırısı gerçekleştirerek alert(document.cookie) çağırılmalıdır. 

Uygulamada GET /resources isteği yapıldığında academy.net/resources adresine yönlendirmektedir. Exploit sunucusuna giderek bir xss payload’u body’nin içerisine yazılmaktadır ve erişim için /exploit —> /resources haline getirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/12/1.png){: .align-center}

Daha sonra aşağıdaki request hazırlanarak sunucuya iletildiğinde kurban exploit sunucusunun /resources endpointine redirect olacaktır ve xss’e maruz kalacaktır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/12/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/12/3.png){: .align-center}

## Lab-13 HTTP/2 request smuggling via CRLF injection

Uygulamalar H2.CL ve H2.TE saldırılarını önlemek için CL veya TE başlıklarını çıkartmak gibi savunma mekanizmaları geliştirse de HTTP/2 nin binary formatı bu tarz front-end savunma mekanizmalarını atlatmak için yollar sağlamaktadır.

Bu laboratuvar request smuggling güvenlik açığına sahiptir. Front-end HTTP/2 isteklerini downgrade eder ve gelen başlıkları yeterince filtrelemez. Lab’ın tamamlanması için HTTP/2-exclusive request smuggling gerçekleştirerek başka bir kullanıcının hesabı ele geçirilmelidir.

Search fonksiyonu bir Post isteği göndermektedir ve sonucu getirmekle birlikte son arananlar olarak tutmaktadır. HTTP/2 formatında bir TE chunked ekleyip body kısmına da aşağıdaki payload yazıldığında her iki istekten ikincisi 404 Not Found vermektedir. Burdan back-end tarafında zafiyetin çalıştığı anlaşılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/13/1.png){: .align-center}

Daha sonra aşağıdaki request oluşturularak kurbanın oluşturacağı request bilgisi search değişkeninde tutulup uygulamada görüntülenmesi hedeflenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/13/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/13/3.png){: .align-center}

Uygulamanın anasayfasına kurbanın session bilgisi ile istek yapıldığında lab tamamlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/13/4.png){: .align-center}

## Lab-14 HTTP/2 request splitting via CRLF injection

Bu laboratuvarda request smuggling güvenlik zafiyeti bulunmaktadır. Front-end sunucusu HTTP/2 isteklerini downgrade eder ve headerları filtrelemez. Lab’ın tamamlanması içi queue poisoning gerçekleştirilerek admin panelinden carlos kullanıcısının silinmesi gerekmektedir.

Anasayfaya yapılan isteğe aşağıdaki header bilgileri eklenmektedir. Daha sonrasında istek back-end e gönderildiğinde X adında bir endpoint olmadığından çoğunlukla 404 Not Found dönecektir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/14/1.png){: .align-center}

Queue poisoning gerçekleştiğinden uygulmaya istek yapan adminin session bilgisi yakalanarak bize dönmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/14/2.png){: .align-center}

Bu session bilgisi kullanılarak admin panelden carlos kullanıcısı silinmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/14/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/9-HTTP-Request-Smuggling/14/4.png){: .align-center}


