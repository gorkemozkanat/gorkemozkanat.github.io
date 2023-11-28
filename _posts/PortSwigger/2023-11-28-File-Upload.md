---
title: "PortSwigger File Upload Vulnerabilities" 
header:
  image: /assets/images/dan.jpg
categories:
   - PortSwigger
tag:
   -PortSwigger   
---

File upload güvenlik zafiyetleri, kullanıcıların dosya sistemi üzerine yeterince doğrulama yapılmadan dosya yükleyebildiği durumlarda ortaya çıkmaktadır.


[Lab  1](#lab-1-remote-code-execution-via-web-shell-upload){: .btn .btn--danger}{: .text-right}  [Lab 2](#lab-2-web-shell-upload-via-content-type-restriction-bypass){: .btn .btn--danger}{: .text-center} [Lab 3](#lab-3-web-shell-upload-via-path-traversal){: .btn .btn--danger} [Lab 4](#lab-4-web-shell-upload-via-extension-blacklist-bypass){: .btn .btn--danger}{: .text-left} [Lab 5](#lab-5-web-shell-upload-via-obfuscated-file-extension){: .btn .btn--danger}{: .text-left} [Lab 6](#lab-6-remote-code-execution-via-polyglot-web-shell-upload){: .btn .btn--danger}{: .text-left} [Lab 7](#lab-7-web-shell-upload-via-race-condition){: .btn .btn--danger}{: .text-left} 


## Lab-1 Remote code execution via web shell upload

Bu laboratuvardaki görsel yükleme fonksiyonunda güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için basic PHP web shell kullanılarak /home/carlos/secret dosyasının içeriği görüntülenmelidir.

secret verisini okuyacak php kodu hazırlanarak bir php dosyası halinde kaydedilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/1/1.png){: .align-center}

Daha sonrasında avatar yükleme fonksiyonu kullanılarak bahsi geçen php dosyası yüklenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/1/2.png){: .align-center}

Yüklenen dosyanın path’ine gidildiğinde secret bilgisi görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/1/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/1/4.png){: .align-center}


## Lab-2 Web shell upload via Content-Type restriction bypass

Bu laboratuvardaki görsel yükleme fonksiyonunda güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için carlos kullanıcısının secret verisi ele geçirilmelidir.

Uygulama, aldığı dosyanın Content-Type bilgisine bakarak filtreleme gerçekleştirmektedir. PHP dosyalarını engellemektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/2/1.png){: .align-center}

Content-Type verisi değiştirilerek request gönderildiğinde uygulama kabul etmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/2/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/2/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/2/4.png){: .align-center}

## Lab-3 Web shell upload via path traversal

Bu laboratuvardaki görsel yükleme fonksiyonunda güvenlik zafiyeti bulunmaktadır. Lab’ın tamamlanması için Carlos kullanıcısının secret dosyası görüntülenmelidir.

Diğer lablardaki gibi yaptığımızda uygulamanın savunma mekanizması çalışmaktadır ve payload çalışmamaktadır.


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/3/1.png){: .align-center}

Upload requestinde file’ın lokasyonu belirlenirken “../” denenmektedir fakat uygulama onu da engelleyip ilgili klasöre kayıt etmektedir dosyayı.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/3/2.png){: .align-center}

Payload “..%2f” olarak değiştirildiğinde uygulamanın filtresi bu durumu engellemez ve webshell file dosyasına yüklenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/3/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/3/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/3/5.png){: .align-center}

## Lab-4 Web shell upload via extension blacklist bypass

Bu laboratuvardaki görsel yükleme fonksiyonunda güvenlik zafiyeti bulunmaktadır. Uygulama belirli uzantıları blacklist yöntemiyle engellemektedir. Bu engelleme atlatılabilmektedir. Lab’ın tamamlanması için carlos kullanıcısının secret bilgisi ele geçirilmelidir.

Uygulamaya .php olarak payload yüklenmeye çalışıldığında uygulama izin vermemektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/4/1.png){: .align-center}

php5 dosyasına izin vermektedir ancak payloadı çalıştırmamaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/4/2.png){: .align-center}

“..%2f” olarak denendiğinde uygulama onu da engellemektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/4/3.png){: .align-center}

Filnename, content-type ve content ayarlanarak bir .htaccess dosyası upload edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/4/4.png){: .align-center}

Daha sonra ilgili payload uzantısı “.l33t” olarak değiştirilerek uygulamaya upload edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/4/5.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/4/6.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/4/7.png){: .align-center}

## Lab-5 Web shell upload via obfuscated file extension

Bu laboratuvardaki görsel yükleme fonksiyonunda güvenlik zafiyeti bulunmaktadır. Belirli uzantılar blacklist tarafından engellenmektedir. Ancak obfuscation teknikleriyle bu durum atlatılabilmektedir. Lab’ın tamamlanması için carlos kullanıcısının secret bilgisi ele geçirilmelidir.

Uygulama sadece jpg dosyası kabul etmektedir. Payload içeren php dosyasının sonuna %00 null byte ekleyip .jpg yapınca blacklist .jpg dizisini tespit edip hata vermemektedir ancak null byte “.jpg” dizesini atarak sunucuya sadece “php.php” olarak dosyayı eklemektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/5/1.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/5/2.png){: .align-center}

## Lab-6 Remote code execution via polyglot web shell upload

Bu laboratuvardaki görsel yükleme fonksiyonunda güvenlik zafiyeti bulunmaktadır.  Uygulama orijinal bir görüntü olduğunu doğrulamak için dosyanın içeriğini kontrol etmesine rağmen sunucu tarafına payload yüklemek mümkündür. Lab’ın tamamlanması için carlos kullanıcısının secret verisi ele geçirilmelidir.

Uygulama dosya içeriğini kontrol ettiğinden gerçek bir görsel olmayan bütün dosyaları engellemektedir. exiftool yardımıyla valid bir görselin yorum kısmına php payload eklenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/6/1.png){: .align-center}

File type php’dir ancak içerik bir görsel olduğundan uygulama sorun yaratmamaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/6/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/6/3.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/6/4.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/6/5.png){: .align-center}

## Lab-7 Web shell upload via race condition

Bu laboratuvardaki görüntü yükleme fonksiyonunda güvenlik zafiyeti bulunmaktadır. Uygulama yüklenen dosyalar üzerinde sağlam bir kontrol sağlamaktadır ancak bunları işleme biçimindeki bir race condition durumu doğrulamayı atlatmayı mümkün kılmaktadır. Lab’ın tamamlanması için carlos kullanıcısının secret verisi elde edilmelidir.

Uygulamaya dosya yüklerken denenen her türlü saldırı başarısız olmaktadır. Uygulama görseli bir klasöre almaktadır zararlı kod kontrolü yapmaktadır ve tespit ettikten sonra silmektedir. Hedefimiz bu kontrol sırasındaki süreden faydalanarak çok fazla istekte bulunmak ve dosyanın zararlı kod kontrolünü atlatmasını sağlamaktır.

Bir adet POST requesti, içinde payload bulunan php dosyası, gönderilmektedir ve 5 adet ilgili dosyayı çağıran GET requesti yapılmaktadır. (Turbo Intruder Yardımıyla) Dosya sunucuya yüklendikten sonra payload kontrolü tamamlanmadan GET istekleri sunucuya gittiğinden payload barındıran dosya sunucudan silinmeden GET isteklerine geri dönmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/7/1.png){: .align-center}

Yapılan GET isteklerinden bazıları zararlı kod kontrolünü atlatarak bize carlos kullanıcısının secret verisini iletmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/7/2.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/PortSwigger/22-File-Upload/7/3.png){: .align-center}