---
layout: dvwa
---

# Low Level

## Brute Force

Brute Force kısmına gelince bizi böyle bir ekran karşılamaktadır.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/1-Brute.png)

Burp suite ile araya girerek username ve password'e Brute Force uygulayarak id ve pw bulunmaktadır.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/2-brute.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/5-brute.png)

## Command Injection

Command Injection kısmında bizden ping atmak için bir ip istemektedir. Alınan ip herhangi bir kontrolden geçmeden doğrudan çalıştırıldığından istenilen komut eklenilerek bu zafiyet sömürülebilir.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/7-command.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/8-command.png)

## CSRF

Get request'inde cookie bilgileriyle PHPSSESID taşındığını gördüm. CSRF için çok elverişli.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/9-csrf.png)

İçeriği aşağıdaki gibi olan bir zararlı web sitesine giriş yapıldığında resmi kullanıcının haberi olmadan kullanıcının şifresi değiştirilir.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/11-csrf.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/12-csrf.png)

Zararlı içerik barındıran web siteyi ziyaret ettikten sonra hedeflenen kullanıcı sadece "hileliPassword" ile giriş yapabilmektedir.

# LFI

Herhangi bir ayıklama olmadığından dosyalar-dizinler arasında rahatça gezebildim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/13-fileinc.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/14-fileinc.png)

## File Upload

File Upload sekmesine geldiğimde benden bir görsel seçmemi istiyor daha sonra Upload butonu yardımıyla sisteme yükleyebiliyorum.

Bu sekmeyi reverse shell alabilmek için kullanıcam. Reverse Shell'in gerekli ayarlarını yaptıktan sonra sisteme yükledim

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/15-fileUpload.png)

Daha sonra dosyanın yüklendiği end-point'e giderken 4444 portunu dinliyordum ve reverse shell aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/16-fileUpload.png)

## SQLi 

ıd ile sorgulattığım zaman kullanıcının adını soy adını aldığım bir sistemle karşılaştım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/17-SQLi.png)

3'' denediğimde sonuç aldığım zaman arka tarafta iyi bir input kontrolü olmadını fark ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/18-SQLi.png)

Daha sonra gerekli payload ile veritabanından isim ve soy isimleri alabildim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/19-SQLi.png)

## SQLi (Blind)

User ID olarak 3 girdiğim zaman ıd=3'e karşılık gelen bir kullanıcı olduğu bildirimini aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/20-SQLiblind.png)

```bash
$ 5' AND ascii(substring((select schema_name from information_schema.schemata limit 0,1),1,!)) >= 90 #

```
Ascii tablosunun da yardımıyla harf harf kazıyarak veritabanı isimlerini buldum daha sonra veritabanındaki tabloları en son olarak da kullanıcı adı ve paroları elegeçirebildim.

## XSS (DOM)

Sayfanın kaynak kodlarını incelediğimde seçilen değerlerin script olarak yansıtıldığını gördüm ve kendi scriptimi yazdım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/29-xss.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/30-xss.png)

# XSS Reflected

Kullanıcıdan aldığı değeri dinamik olarak geri web sitesinde göstermeye çalışırken oluşan zafiyet yararlandım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/31-xss.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/31.1-xss.png)

## XSS Stored

Burp suite ile araya girdikten sonra istek üzerinde değişiklilik yaparak XSS zafiyetinden yararlanabildim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/32-xss.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/33-xss.png)

# MEDİUM Level

## Brute Force

Brute force kısmında kaynak kodları incelediğimde sleep(2) fonksiyonu brute force hızından kayıp yaratmak için yazılmıştı. Eğer bir giriş işlemi başarızı olursa bahsi geçen fonksiyondaki zaman kadar beklenmesi gerekiyor bir sonraki giriş teşebbüsü için.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/34-Mbrute.png)

Bu yüzden giriş denemeleri arasındaki süreyi bir 2000 ms'den 3000 ms'ye çıkardım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/35-Mbrute.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/36-Mbrute.png)

## Command Injection

Kaynak kodlar incelendiğinde blacklist de noktalı virgül ve "&&" işaretinin olduğunu gördüm.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/37-Mcommand.png)

Fakat benim kullandığım komutlar ve işaretler bahsi geçen blacklist'de yok.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/38-Mcommand.png)

## CSRF

Low kısmındaki örneğe ek olarak bir http kontrolü bulunmaktadır. 

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/39-Mcsrf.png)

stripos fonksiyonu ile kaynak sunucunun adresi istekteki HTTP PREFER değerinin içinde olup olmadığı kontrol ediliyor. Eğer istek farklı bir kaynaktan geliyorsa istek değerlendirilmiyor. Fakat isteğin kaynağı "saldırgansunucu/hedefsunucu.html" şeklinde olursa kaynak sunucu HTTP PREFER'in içinde olduğundan bu ayıklamayı atlatabilmektedir.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/40-Mcsrf.png)

## LFI

LFI sekmesinde kaynak koda göz attığımda kullanışlı bazı değerlerin ayıklandığını fark ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/41-LFI.png)

Fakat onları kullanmadan da kritik bilgilere ulaşılabiliyor.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/42-LFI.png)

## File Upload

Yüklenen dosya formatında sınırlamalar olduğunu fark ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/45-uploadDıkkat.png)

Burp suite ile araya girdikten sonra Content-Type'ı php görünümünden jpeg görünümüne çevirdiğimde doğrulamayı atlatabildim ve reverse shell'i başarıyla yükledim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/46-uploadDıkkat.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/43-upload.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/44-upload.png)

## SQLi

mysqli_real_escape_string fonksiyonu inputta sql'in değerli görebileceği herhangi bir karaktere ters slash "\" koymaktadır.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/47-sqli.png)

Yukarıdaki SQLi engelleme yöntemi çok kullanışlıdır fakat 7. satırda id değeri tırnak kullanılmadan alındığından halen zafiyetlidir. Tırnak koymadan istediğimiz sql komutunu çalıştırabiliriz.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/48-sqli.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/49-sqli.png)

## SQLi Blind

Sqlmap kullanarak bilgi almayı denedim.

```bash
$ sqlmap -u http://192.168.44.129/dvwa/vulnerabilities/sqli_blind/# --cookie="security=medium; PHPSESSID=2b67tokvrfiaoup184hoshdhi0" --data"id=1&Submit=Submit00" --dbs
$ sqlmap -u http://192.168.44.129/dvwa/vulnerabilities/sqli_blind/# --cookie="security=medium; PHPSESSID=2b67tokvrfiaoup184hoshdhi0" --data"id=1&Submit=Submit00" -D dvwa --tables
$ sqlmap -u http://192.168.44.129/dvwa/vulnerabilities/sqli_blind/# --cookie="security=medium; PHPSESSID=2b67tokvrfiaoup184hoshdhi0" --data"id=1&Submit=Submit00" -D dvwa -T users --columns
$ sqlmap -u http://192.168.44.129/dvwa/vulnerabilities/sqli_blind/# --cookie="security=medium; PHPSESSID=2b67tokvrfiaoup184hoshdhi0" --data"id=1&Submit=Submit00" -D dvwa -T users -C user,password -- dump

```
Önce veritabanı isimlerini öğrendim daha sonra veritabanları içindeki tabloları, kullanıcıları en son olarak veritabanındaki değerli verilere eriştim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/52-blind.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/53-blind.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/54-blind.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/55-blind.png)

## XSS Dom

Sayfanın kaynak kodlarında default değeri içerisinde script etiketini engelledikleri görülmektedir.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/56-xssdom.png)

Gerekli payload'ı default değeri dışında çalıştırdım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/57-xssdom.png)

## XSS Reflected

Kaynak kodunda yine script değerinin engellendiğini gördüm.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/58-reflected.png)

Büyük-küçük harflerle oynayarak bunu da atlatabildim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/59-reflected.png)

## XSS Stored

Script etiketi göründüğünde siliniyordu. İç içe script kullanarak bu durumu atlattım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/60-xss.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/61-xss.png)

# High Level

## Brute Force

High levelde token bulunmaktadır. Her denemede token değeri değişmektedir ve token değerini silip denediğimizde hata almaktayım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/1-BruteForce.png)

Her login denemesi ile birlikte yeni bir token oluşturarak istekle birlikte yollamayı denedim. Bu yüzden tokeni algılayıp yeniden oluşturması için burp suite ile bir macro  oluşturdum. Sleep fonksyonu bu senaryoda da bulunmakta.

Project Options --> Sessions 

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/2-Bruteforce.png)

Session Handling Rules kısmında Add dedim ve çıkan pencerede kural ismini belirtip yapacağı eylemi macro olarak gösterdim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/3-Bruteforce.png)

Daha sonra açılan pencerede Add diyerek token bilgisinin olduğu bir isteği gösterdim. 

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/5-Brute.png)

Configure İtem dedikten sonra Add diyerek user_token bilgisini macroya gösterdim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/6-UserToken.png)

Session handling action editor sekmesindeki bu kutucuğu doldurdum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/8-Brute.png)

Daha sonra Scope kısmında aşağıdaki kutucukların seçili olduğundan emin oldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/9-Brute.png)

Son olarak login işlemini yakalayıp parola kısmını işaretleyerek uygun bir wordlist ile brute force saldırısına başladım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/10-Brute.png)

Parolayı buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/11-BruteForceSon.png)

## Command Injection

Kaynak koda baktığımda bir blacklist hazırlandığını gördüm fakat bir hata vardı. Pipe karakteri blacklist'e eklenirken bir boşluk unutulmuş.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/12-COmmand.png)

Bu hatayı kullanarak Command Injection'ı gerçekleştirdim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/13-CommandInjectionSon.png)

## CSRF

Kaynak kodu incelediğimde her bir parola değişirme talebimde bir token istediğini fark ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/14-CSRF.png)

Zafiyeti sömürebilmek id ve password bilgilerini "admin" olarak değiştirecek bir javascript dosyası oluşturdum ve istemcinin ulaşabileceği bir yere koydum. High seviye DOM XSS güvenlik açığını kullanarak id ve pw'yi değiştirdim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/15-CSRF.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/16-CSRF.png)

Sonuç olarak CSRF zafiyetini de sömürerek id ve pw'yi değiştirdim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/17-CSRF.png)

## LFI

Kaynak kodlara baktığım zaman dosya isminin ya include.php olması gerektiğini yada file ile başlaması gerekliliğini fark ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/18-LFI.png)

```bash
?page=file:///../../../../etc/passwd

```

Üsteki yaklaşım ile istenilen gereklilikleri yakalayarak passwd dosyasını okudum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/19-LFI.png)

## File Upload

Kaynak kodlara baktığımda yüklenecek dosyaların "jpeg", "jpg" veya "png" olması gerektiğini fark ettim. Daha sonra boş bir dosyanın içine php reverse shell komutlarını yazdım fakat komutları yazmadan önce kontrolden geçmesi adına GIF98 yazdım ve shell.jpeg olarak kaydettim. Sisteme yüklemeyi gerçekleştirdim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/21-Fileupload.png)

Hedef sistemde reverse shell kodları mevcut fakat kullanabilmek için .jpeg uzantısından php uzantısına çevirmem gerekiyor daha önce kullandığım command injection zafiyetinden yararlanarak dosya ismini ve uzantısını değiştirdim.
 
```bash
1|mv /var/www/html/dvwa/hackable/uploads/shell.jpeg /var/www/html/dvwa/hackable/uploads/exploit.php

```
Daha sonra 4444 portunu dinlemeye bırakarak reverse shell'in yüklendiği yere gittim ve shell aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/22-FileUploadSon.png)

## SQLi

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/23-SQli.png)

## SQLi Blind

Tarayıcıdan PHPSESSID aldıktan sonra sırasıyla ilerledim. İlk olarak veritabanı isimlerini aldım

```bash
sqlmap -u http://192.168.44.129/dvwa/vulnerabilities/sqli_blind/# --cookie="security=high; PHPSESSID=ovq8gbpmclqfmn6k4us3r67ij5" --data="id=1&Submit=Submit00" --dbs  
```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/24-SqliBlind.png)


Daha sonra tablo isimlerini

```bash
sqlmap -u http://192.168.44.129/dvwa/vulnerabilities/sqli_blind/# --cookie="security=high; PHPSESSID=ovq8gbpmclqfmn6k4us3r67ij5" --data="id=1&Submit=Submit00" -D dvwa --tables  
```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/25-Sqlblind.png)


Sütun isimlerini aldım

```bash
sqlmap -u http://192.168.44.129/dvwa/vulnerabilities/sqli_blind/# --cookie="security=high; PHPSESSID=ovq8gbpmclqfmn6k4us3r67ij5" --data="id=1&Submit=Submit00" -D dvwa -T users --columns
```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/26-Sqlblind.png)

Son olarak veritabanındaki kullanıcı adı şifrelerini aldım.

```bash
sqlmap -u http://192.168.44.129/dvwa/vulnerabilities/sqli_blind/# --cookie="security=high; PHPSESSID=ovq8gbpmclqfmn6k4us3r67ij5" --data="id=1&Submit=Submit00" -D dvwa -T users -C users user,password --dump
```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/27-Sqliblind.png)

## XSS (DOM)

Kaynak kodu incelediğimde girdi 4 değerden biri değilse eğer default olarak İngilizceye ayarlıyor. Hiç dokunmadan atlatılabiliyor.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/28-XssDom.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/29-xssDomSon.png)

## XSS (Reflected)

Kaynak kod aşağıdaki gibi ve script harfleri kontrol ediliyor.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/31-XssReflected.png)

script filtrelenmiş olabilir fakat farklı yollarla alert çıktısı alabiliyorum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/32-xssreflected.png)

## XSS (Stored)

Kaynak kodu incelediğimde yine mesaj kısmında script kısıtlaması olduğunu fark ettim reflected XSS de kullandığım komut burda da çalışıyor. 

```bash
<img src=x onerror="alert(1)">
```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/dvwa/high/33.1-xssStored.png)

# Impossible Level

## Brute Force

görsel1

Bu güvenlik seviyesinde her başarısız login denemesinde bir sonraki login denemesi için beklenmesi gereken süre artmaktadır. Eğer zaman problemi yoksa bir kaç yöntem ile eninde sonunda parola bulunabilir. Birden fazla kullanıcıya şifre denemesi yaparsak eğer bir kullanıcıyı zorlamadığımız için 15 dakika hapis kalınmaz. Fakat sonuca ulaşmak aylarca vakit alır.

## Command Injection

görsel2

Kaynak kodunu incelediğimde girdi değeri ip olması gerektiğinde dörder dörder ayırarak integer bir değer olup olmadığını kontrol ediyor. Eğer integer ise tüm değerler tekrar birleştirerek cmd ye yönlendiriyor. Bu validation yaklaşımını atlatmayı henüz bulamadım.

## CSRF

görsel3

Kaynak kodu incelediğimde parolayı değiştirmek için eski parolanın da istendiğini ve kontrol edildiğini gördüm, eski parola bilinmeden yeni parola oluşturmak imkansız hale geldi. CSRF saldırılarına karşı başarılı bir önlem uygulanmış.

## File Inclusion

görsel4

Impossible güvenlik seviyesinde File Inclusion saldırılarına karşı en iyi güvenlik yaklaşımlardan biri olan beyaz liste kullanılıyor. Herhangi bir sızma tekniği tespit edemedim.

## File Upload

görsel5

Impossible güvenlik seviyesinde yüklenen dosyaların isimleri değiştirilmektedir ve CSRF saldırılarına karşı Anti-CSRF tokeni bulunmaktadır. Ayrıca dosya içeri ise çok sık bir şekilde kontrol edildiğinden zararlı yazılım barındıran dosya hedef sisteme yüklenememektedir. 

## SQLi

görsel6

Impossible güvenlik seviyesi PDO teknolojisini kullanmaktadır. İyi bir şekilde ayıklama yaparak SQLi'a karşı başarılı bir önlem oluşmaktadır. Ek olarak CSRF token bulunduğunda savunma mekanizması bir tık daha artmaktadır.

## SQLi Blind

görsel7

PDO teknolojisi ile birlikte girdideki hangi parçanın kod hangi parçanın veri olduğu ayırt edilebilmektedir. CSRF token ile güvenlik seviyesi daha da arttırılmıştır.

## XSS DOM

görsel8

Koruma istemci tarafından yapıldığından bir XSS DOM zafiyeti yoktur.

## XSS Reflected

görsel9 

Impossible seviyedeki kaynak kodları incelediğimde htmlspecialchars fonksiyonu daha önceden belirlenmiş karakterleri HTML öğeleri olarak kullanılmasını engelleyerek HTML varlıklarına dönüştürür.

## XSS Stored

görsel10

Kaynak kodlarında hem isim hem de mesaj kısmı sıkı bir şekilde filtrelenmiştir. XSS zafiyeti bulunmamaktadır.


### Teşekkürler, Başka Makinelerde Görüşmek Üzere.



























