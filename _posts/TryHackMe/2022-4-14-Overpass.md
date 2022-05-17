---
title: "TryHackMe Overpass WriteUp"

categories:
   - CTF
   - TryHackMe
tag:
   -TryHackMe
---

Selamlar bu yazımda TryHackMe'de bulunan Overpass makinesinin çözümünden bahsedeceğim. Umarım öğretici bir yazı olur, keyifli okumalar.

## Nmap

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/nmap.png){: .align-center}

Gobuster çıktısı bize bir /admin dizini göstermekte.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/gobuster.png){: .align-center}

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/adminarea.png){: .align-center}

Sqlmap ile bir kaç deneme yapılmaktadır fakat herhangi bir sonuç alınamamaktadır. Sonrasında ipucuna bakıldığında OWASP Top 10'i incelenmesini ve brute force uygulanmasını söylemektedir. Daha sonrasında session token sıfırlandığında parola gerekmeksizin hedef sisteme erişilebildiği tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/sessiontoken.png){: .align-center}

James adlı kullanıcının SSH key'i için bir RSA Private Key tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/rsa.png){: .align-center}

Tespit edilen RSA ssh2john kullanılarak hash haline getirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/ssh2john.png){: .align-center}

Daha sonrasında ise john kullanılarak parola tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/parola.png){: .align-center}

ve hedef sisteme giriş sağlanmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/giris.png){: .align-center}

user.txt

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/user.png){: .align-center}

Yetki yükseltmek için SUID bitlerine baktığımda işe yarar bir şey bulunmamaktadır fakat crontab'da kullanılabilir bir dosya olduğu tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/cron.png){: .align-center}

buildscript.sh dosyasını çekerek bash'e gönderip çalıştıran bir cronjobs tespit edilmektedir. Ip'yi saldırgan makine olarak göstererek oluşturulan buildscript.sh adlı revershell ile root yetkileri elde edilmektedir.

/etc/host dosyası incelendiğinde overpass.thm'nin adresi 127.0.0.1 olarak görüntülenmekte. Bu değeri saldırgan makine ip'sine değiştirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/ip.png){: .align-center}

Reverse Shell oluşturulup hosts dosyasında saldırgan ip olarak düzenlendikten sonra ilgili port dinlenerek root yetikili bir shell ede edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/Overpass/shel.png){: .align-center}
