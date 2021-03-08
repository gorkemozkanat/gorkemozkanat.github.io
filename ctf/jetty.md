---
layout: typhoon
---
# NMAP Çıktısı

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/1-nmap.png)

nmap çıktısında sadece 21 ve 80 portlarının açık olduğunu gördüm. 80 portunda bir http servisi çalışıyordu ve beni böyle bir ekran karşıladı.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/2-anasayfa.png)

Kaynak koduna da baktım fakat işe yarar pek bir şey bulamadım bü yüzden dirb aracı ile gizli dizin olup olmadığını kontrol ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/3-dirb.png)

dirb çıktısında robots.txt yi gördüm ve incelediğimde yine işe yarar bir şey çıkmadı.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/4-robot.png)

ftp servisinde bir şeyler bulmaya çalıştım ve sadece anonymous olarak girebileceğimi fark ettim içerde README.txt ve sshpass.zip dosyalarını buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/5-ftp.png)

Dosyaları kendi makineme çektim ve README.txt'yi incelediğimde .zip dosyasında Henry adlı kullanıcının ssh parolasını olduğunu öğrendik fakat şifrelenmişti.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/6-ReadMe.png)

.zip dosyası içerisindeki parolayı alabilmek için fcrackzip aracı ile .zip dosyasının şifresini buldum.

```bash
$ fcrackzip -v -u -D -p /usr/share/wordlists/rockyou.txt sshpass.zip 

```

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/7-zipK%C4%B1rma.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/8-sshPass.png)
 
Elimde artık bir ssh parolası var. Daha önce VulnHub'da makinenin açıklamasında bize şüpheli kişinin kullanıcı adı verilmişti. Çehov'un silahı diyerek bu kullanıcı adı ve bulduğum parolayı denedim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/9-sshIptal.png)

id, pw var fakat bir ssh servisi ortada yok. Daha önce nmap çıktısında da görmüştüm olmadığını. Bir müddet etrafta gezinip boş boş baktıktan sonra nmap'in default olarak çok kullanılan 1000 portu taradığını hatırladım ve tekrardan nmap kullanarak tüm portları taradım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/10sshNmap.png)

Evet gerekli olan ssh portu burda.

Şüpheli kullanıcı adıyla sistemde shell aldığımda, shell'in sınırlı bir shell olduğunu fark ettim sadece belirli komutları çalıştırabiliyordum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/11-sshgiris.png)

Eğer iznim olmayan komutları çalıştırmaya çalışırsam 3 denemeden sonra girdiğim oturumdan atılıyordum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/12-sshatt%C4%B1.png)

Python kullanabileceğim komutlar arasındaydı ve bu büyük bir avantaj yarattı. Python kullanarak sınırlı shellden kaçabildim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/13-etkile%C5%9Fimli.png)

_sudo -l_ komutunu kullanarak find komutunun parola istemeden root yetkileriyle çalıştığını gördüm. Biraz araştırma yaptığımda find komutunu kullanarak root yetkilerine erişebileceğimi buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/14-find'e%20bakt%C4%B1k.png)

Ve yetkimi yükselttim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/15-RootOlduk.png)

Makinenin bizden istediği kanıtı okudum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/jetty/16-andProofFlag.png)



### Teşekkürler, Başka Makinelerde Görüşmek Üzere.
