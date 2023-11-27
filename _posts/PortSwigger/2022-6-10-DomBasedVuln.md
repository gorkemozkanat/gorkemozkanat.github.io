---
title: "PortSwigger DOM-based Vulnerabilities Labs" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

Document Object Model (DOM), web tarayıcısının sayfa üzerinde elementlerin hiyerarşik sunumudur. Web siteleri DOM üzerinde değişiklilik  yapmak için JavaScript kullanabilir. Modern web sitelerinin ayrılmaz bir parçasıdır. Fakat verileri güvensiz bir şekilde işeyen JavaScript çelitli saldırılara olanak sağlamaktadır. DOM-based güvenlik zafiyetleri source olarak bilinen kullanıcının kontrol edebildiği ve sink olarak bilinen tehlikeli fonksiyona geçiren JavaScript içerdiğinde ortaya çıkmaktadır.

Yaygın Source’lar

```jsx
document.URL
document.documentURI
document.URLUnencoded
document.baseURI
location
document.cookie
document.referrer
window.name
history.pushState
history.replaceState
localStorage
sessionStorage
IndexedDB (mozIndexedDB, webkitIndexedDB, msIndexedDB)
Database
```

Yaygın Sink’ler

```jsx
DOM XSS LABS	                    document.write()
Open redirection                    window.location
Cookie manipulation                 document.cookie
JavaScript injection	            eval()
Document-domain manipulation	    document.domain
WebSocket-URL poisoning	            WebSocket()
Link manipulation	            element.src
Web message manipulation	    postMessage()
Ajax request-header manipulation    setRequestHeader()
Local file-path manipulation	    FileReader.readAsText()
Client-side SQL injection	    ExecuteSql()
HTML5-storage manipulation	    sessionStorage.setItem()
Client-side XPath injection	    document.evaluate()
Client-side JSON injection	    JSON.parse()
DOM-data 			    element.setAttribute()
Denial of service	            RegExp()
```


[Lab  1](#lab-1-dom-xss-using-web-messages){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-dom-xss-using-web-messages-and-a-javascript-url){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-dom-xss-using-web-messages-and-jsonparse){: .btn .btn--danger} [Lab 4](#lab-4-dom-based-open-redirection){: .btn .btn--danger}{: .text-left}  [Lab 5](#lab-5-dom-based-cookie-manipulation){: .btn .btn--danger}{: .text-left} [Lab 6](#lab-6-exploiting-dom-clobbering-to-enable-xss){: .btn .btn--danger}{: .text-left} [Lab 7](#lab-7-clobbering-dom-attributes-to-bypass-html-filters){: .btn .btn--danger}

## Lab-1 DOM XSS using web messages

Bu laboratuvarda basit bir web mesaj güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için hedef sistemde print() fonksiyonunu tetiklenmesine neden olan bir mesaj göndermek için exploit sunucusu kullanılmalıdır.

Hedef sistemde kaynak kod incelendiğinde web mesajını dinleyen bir addEventListener görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/1/1.png){: .align-center}

iframe yüklendiğinde postMessage() metodu ana sayfaya gönderilmektedir. Eventlistener web mesajının içeriğini alır ve div’e ekler. src attribute’unda hata olduğundan onerror çalışır ve print yazdırılır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/1/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/1/3.png){: .align-center}

## Lab-2 DOM XSS using web messages and a JavaScript URL

Bu laboratuvarda web mesajlaşması tarafından tetiklenen Dom-based güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için zafiyeti sömüren ve print () fonksiyonunu çağıran bir HTML sayfası tasarlanmalıdır.

JS web mesajının herhangi bir yerinde http ve https aramaktadır. Fakat hatalı bir indexOf denetimi içerir. Ayrıca sink location.href dosyasını da içermektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/2/1.png){: .align-center}

Aşağıdaki komut http ile birlikte bir JS payload’u içermektedir. İkinci argüman ise web mesajı için herhangi bir taregtOrigin’e izin verildiğini belirtmektedir. iframe yüklendiğinde postMessage() JS payload’u ana sayfaya göndermektedir. Eventlistener http dizesini tespit eder ve yükü print() işlevinin çağırıldığı location.href sink’ine gönderir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/2/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/2/3.png){: .align-center}

## Lab-3 DOM XSS using web messages and JSON.parse

Bu laboratuvar web mesajlaşması kullanmaktadır ve mesajları JSON olarak parse etmektedir. Lab’ın tamamlanması için exploit sunucusunda güvenlik zafiyetinden yararlanan ve print() fonksiyonunu tetikleyen bir HTML sayfası oluşturulmalıdır.

Event listener JSON.parse kullanılarak ayrıştırılan bir string beklemektedir. JS’te event listener bir type beklemektedir ve görüldüğü üzere load-channel case’i ifram scr attribute’unu değiştirmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/3/1.png){: .align-center}

src’ye erişebildiğimizi fark ettikten sonra aşağıdaki payload’u hazırlayarak beklenen load-channel kısmına print() fonksiyonu gönderilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/3/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/3/3.png){: .align-center}

## Lab-4 DOM-based open redirection

Bu laboratuvarda Dom-based open-redirection güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için güvenlik zafiyetinin sömürülerek kurbanı exploit servere yönlendirilmesi sağlanmalıdır.

Post içerisinde bulunan “Back to Blog” bağlantısı ana sayfaya döneye yaramaktadır.

```jsx
<div class="is-linkback">
    <a href='#' onclick='returnUrl = /url=(https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "/"'>Back to Blog</a>
</div>
```

url parametresi kullanıcının nereye yönlendirileceğini değiştirmeye olanak tanıyan open-redirection güvenlik zafiyeti barındırmaktadır. Aşağıdaki payload’u yazıp tıkladığımızda url değişkeni bizim exploit sunucusu olarak güncellenecek ve back to blog linkine tıkladığımızda exploit sunucusuna erişilecektir.

```jsx
https://acc21f691f57a22ac08368dc0093005f.web-security-academy.net/post?postId=4&url=https://exploit-ac2c1f611f29a2dcc09568e901f30013.web-security-academy.net
```

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/4/1.png){: .align-center}

## Lab-5 DOM-based cookie manipulation

Bu laboratuvarda Dom-based client-side cookie manipülasyonu bulunmaktadır. Lab’ın tamamlanması için XSS’e neden olan ve print() fonksiyonunu çağıran bir cookie enjeksiyonu gerçekleştirilmelidir.

Bir ürünü ziyaret ettikten sonra uygulama son görüntülenen ürünü bir cookie değerinde saklamaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/5/1.png){: .align-center}

iframe yüklendiğinde tarayucu zararlı URL’i açar ve daha sonra onu lastViewedProduct cookie bilgisinin içerisine yerleştirir. Onload ise bu durumdan habersiz olarak ana sayfaya yönlendirilmesini sağlar. Kurbanın tarayıcısında zararlı cookie dururken ana sayfanın yüklenmesi yükün yürütülmesine neden olur.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/5/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/5/3.png){: .align-center}

## Lab-6 Exploiting DOM clobbering to enable XSS

DOM clobbering, sayfa üzerindeki JavaScript davranışını kalıcı olarak değiştirmek ve DOM üzerinde manipülasyonlar yapmak için HTML enjekte etme tekniğidir. Özellikle XSS’in mümkün olmadığı durumlarda kullanışlıdır. DOM clobbing’in en yaygın biçimi global bir değişkenin üzerine yazmak için bir bağlantı öğesi kullanır ve bu daha sonra uygulama tarafında dinamik bir komut doslyası URL’i oluşturmak gibi güvenlik olmayan bir şekilde kullanılır.

Bu laboratuvarda DOM-clobbering güvenlik zafiyeti bulunmaktadır. Yorum yapma fonksiyonu güvenli HTML’lere izin vermektedir. Lab’ın tamamlanması için alert() fonksiyonunu tetikleyen bir HTML enjekte edilmelidir.

Hedef uygulamaya gittikten sonra aşağıdaki payload’u içeren bir yorum yapılır.

```jsx
<a id=defaultAvatar><a id=defaultAvatar name=avatar href="cid:&quot;onerror=alert(1)//">
```

Daha sonra herhangi bir veri içeren başka bir yorum yapılır ve yorumlara göz atmak için geri gidilir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/6/1.png){: .align-center}

Uygulama DOM-based güvenlik zafiyetlerini önleme amacıyla DOMPurify filtresini kullanmaktadır. DOMPurify, çift tırnakları URL kodlamayan cid: protokolünü kullanılmasına izin vermektedir. Bu çalışma zamanında kodun çözülecek kodlanmış bir çift alıntı eklenebileceği anlamına gelmektedir. Sonuç olarak yukarıda açıklanan enjeksiyon sayfanın bir sonraki yüklenmesinde defaultAvatar değişkenine clobbered özelliğinin {avatar: ‘cid:"onerror=alert(1)//’} atanmasına neden olmaktadır.

## Lab-7 Clobbering DOM attributes to bypass HTML filters

Bu laboratuvar HTMLJanitor kütüphanesi kullanmaktadır ve DOM clobbering güvenlik zafiyetine olanak sağlamaktadır. Lab’ın tamamlanması için filtrelerin atlatılması ve print() fonksiyonunu çağıran bir DOM clobbering vektörü enjekte edilmelidir.

Bir paylaşıma gidilip aşağıdaki HTML yorum olarak yazılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/7/1.png){: .align-center}

Daha sonra exploit server’a gidilerek aşağıdaki payload yazılmaktadır. Uygulama HTML attribute’lerini filtrelemek için attributes özelliğini kullanmaktadır. Ancak attribute’ün kendisini clobber’lamak mümkün olmaktadır. Bu durum form öğesine istediğimiz herhangi bir özelliği enjekte etmemizi sağlamaktadır.

iframe yüklendiğinde 500 ms lik bir gecikmeden sonra sayfa URL’sinin sonuna # parçasını eklemektedir. JS yürütülmeden önce enjeksiyonu içeren yorumun yüklendiğinden emin olmak için bu gecikme gereklidir. Bu tarayıcının yorum içerisindeo luşturduğumuz form olan “x” kimliğine sahip öğeye odaklanmasına neden olur. onfocus event handler daha sonra print() fonksiyonunu çağırır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/7/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/5-DOM-based-vulnerabilities/7/3.png){: .align-center}






