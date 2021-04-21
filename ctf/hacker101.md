---
layout: hacker101
---
_Eksik **CTF**'leri vakit buldukça çözüp ekleyeceğim._

# A little something to get you started 

## (1 / 1 flag)

Sayfanın kaynak kodlarını incelediğimde bir endpoint gördüm. 

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/1-1.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/1-2.png)

# Micro-CMS v1 

## ( 1 / 4 flag)

Create Page kısmına geldiğimde başlık ve içerik girebileceğim bir sayfa ile karşılaştım. Script'in desteklenmediğini gördüm.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/2-1.png)

Script tag'i kullanmadan onafterprint kullanarak XSS'i gerçekleştirdim ve flag'i aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/2-2.png)

## ( 2 / 4 flag)

Bir yorum oluşturduktan sonra sayfaların id değeri ile çekildiğini gördüm.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/2-3.png)

Page 4'e gelince diğerlerinin aksine burada bir sayfa olduğunu anladım fakat erişim için iznim yoktu.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/2--4.png)

Oluşturulan post'u editlemek istediğimde yüksek bir yetkiye sahip olduğumu fark ettim ve bununla iznim olmayan sayfaya ulaşıp içeriği okudum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/2-5.png)

## ( 3 / 4 flag)

Hangi içeriğin editleneceğini belirlemek için bir SQL sorgusu çağırabileceğini düşündüm ve hata almak için bir şeyler denediğimde flag'e ulaştım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/2-6.png)

## ( 4 / 4 flag)

Script tag'i yorum oluştururken engelleniyordu fakat title'da engellenmediğini gördüm.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/2-7.png)

Daha sonra sayfanın kaynak kodlarına bakınca flag'i buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/2-8.png)

# Micro-CMS v2

## ( 1 / 3 flag)

Yeni bir sayfa oluşturmak istediğimde beni bir login ekranına yönlendirdi.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/3-1.png)

SQLi kontrolü yaparken zafiyetin var olduğunu buldum ve sorgunun yapısına ulaştım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/3-2.png)

Username kısmına,

```bash
admin' UNION SELECT "hehe" AS password FROM admins WHERE '1'='1 

```

yazarak ve Password kısmına "hehe" yazarak sisteme giriş yaptım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/3-3.png)

ve flag'i buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/3-4.png)

# Encrypted Pastebin

## ( 1 / 4 flag)

Bir post oluşturduğumda URL'in garip olduğunu düşündüm. 

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/4-1.png)

Link'in şifreli olduğunu ve üzerinde oynamamın bir sakincası olmadığın hint'ler sayesinde öğrendim ve flag'i elde ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/4-2.png)

# Cody's First Blog

## ( 1 / 3 flag)

Blogdaki ilk gönderide bir ipucu yakaladım include() fonksiyonunun kullanıldığını anladım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/5-1.png)

```bash
<?php system($_GET [ 'c' ] ); ?>

```

komutunu enjekte ederek flag'i buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/5-2.png)

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/5-3.png)

## ( 2 / 3 flag)

Daha sonra sayfanın kaynak kodunu incelediğimde Admin Login barındıran bir endpoint olduğunu gördüm.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/5-4.png)

Bir kaç farklı yolla girip yapmaya çalıştım fakat başarısız oldum. URL'de bulunan auth(authentication) ibaresini silerek admin sayfasına ulaşmaya çalıştım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/5-5.png)

ve flag'i aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/5-6.png)

# Postbook

## ( 1 / 7 flag)

Beni bir login sayfası karşıladı.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-1.png)

Hint'lere baktığımda kullanıcı adının "user" olduğu ve parolasının çok kolay olduğunu belirtmiş. username= user password= password bilgileri ile sisteme giriş yaparak flag'i aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-2.png)

## ( 2 / 7 flag) 

Daha önceki bir flag'i bulmak için id değerini değiştirmiştim. Bunda da id değeri gördüm ve değiştirerek admin profil sayfasına ulaştım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-3.png)

Admin sayfasında bulunan gizli blog'u okuduğumda flag'i elde ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-4.png)

## ( 3 / 7 flag)

Yeni post oluştururken kaynak kodlarına baktığımda gizli bir şekilde user_id değerinin barındığını fark ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-5.png)

Daha sonra post oluştururken user_id değerini değiştirdim ve post oluşturdum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-6.png)

ve flag'i buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-7.png)

## ( 4 / 7 flag)

Dördüncü bayrak için hint'lerden yararlandım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-8.png)

945 değerini id olarak denediğimde flag'i buldum.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-9.png)

## ( 5 / 7 flag)

Post'u editlerken yetkim olabileceğini düşündüm ve id değeri 2 olan postun içeriğini değiştirdim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-10.png)

ve flag'i aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-11.png)

## ( 6 / 7 flag)

Burp Suite ile araya girdiğimde Cookie bilgisinde id değerinin md5 şifreli haliyle taşındığını fark ettim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-12.png)

Bu id değerini manipüle ederek admin hesabına geçip flag'i aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-13.png)

## ( 7 / 7 flag)

Kendi postumu silerken burp suite ile araya girdiğimde id değerinin GET methodunda iletildiğini gördüm.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-14.png)

ve bu değeri manipüle ederek flag'i aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/6-15.png)

# Petshop Pro

## ( 1 / 3 flag)

Beni aşağıdaki gibi bir sayfa karşıladı.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/7-1.png)

Daha sonra her iki üründen de birer tane alarak sepetime ekledim.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/7-2.png)


Check Out yaparken Burp Suite ile araya girdiğimde "cart=" verisinde ürün bilgileri ve ücretinin iletildiğini gördüm.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/7-3.png)

Daha sonra bu verideki price değerlerini 0 olarak değiştirip forward'ladığımda ürünleri para vermeden alabildim ve flag'i aldım.

![Branching](https://raw.githubusercontent.com/gorkemozkanat/gorkemozkanat.github.io/master/assets/images/hacker101/7-4.png)





### Teşekkürler, Başka Makinelerde Görüşmek Üzere.
