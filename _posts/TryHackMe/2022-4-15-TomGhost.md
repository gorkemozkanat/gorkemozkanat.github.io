---
title: "TryHackMe TomGhost WriteUp"
header:
 
categories:
   - CTF
   - TryHackMe
tag:
   -TryHackMe
---

## Nmap 

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/1.png){: .align-center}

Apache Tomcat 6x 7x 8x 9x sürümlerinde Ghostcat zafiyeti bulunmaktadır. Msf kullanılarak zafiyetten yararlanılmaktadır ve ssh bağlantısı için kullanılan bilgiler elde edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/2.png){: .align-center}

Ssh bağlantısı

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/3.png){: .align-center}

User.txt

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/4.png){: .align-center}

Skyfuck kullanıcısında credential.pgp ve tryhackem.asc dosyaları tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/5.png){: .align-center}

Credential bilgilerine ulaşmaya çalışıldığında passphrase gerektiği gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/6.png){: .align-center}

tryhackme.asc içeriğini saldırı makinesine kopyaladıktan sonra gpg2john kullanılarak hash'e çevrilmektedir ve daha sonrasında john ile parola tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/7.png){: .align-center}

Parola kullanılarak merlin kullanıcısının parolası tespit edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/8.png){: .align-center}

Merlin kullanıcısında zip'in sudo yetkileri ile çalıştığı gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/9.png){: .align-center}

GTFOBins incelendiğinde zip ile yetki yükseltme gerçekleşebildiği gözlemlenmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/10.png){: .align-center}

Yetki yükseltme gerçekleştirilerek root yetkileri elde edilmektedir.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/11.png){: .align-center}

Root.txt

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/TomGhost/12.png){: .align-center}







