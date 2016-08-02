# BroIDS Kurulum ve Kullan�m Klavuzu
***
### 1) BroIDS Nedir?
**BroIDS** ilk ba�larda **Vern Paxson** taraf�ndan C++ dilinde geli�tirilmeye ba�lanan, daha sonra a��k kaynak hale gelerek herkesin katk�da bulundu�u Linux tabanl� bir **a� trafi�i analiz ve m�dahale** yaz�l�m�d�r. **Bro**'nun ad�, "Big Brother" yani "B�y�k Karde�"ten (Big Brother, siber g�venlik d�nyas�nda genel olarak NSA'in lakab�d�r) gelmektedir. "B�y�k Karde�"in bizi hep izledi�i varsay�m�ndan esinlenilerek bu isim verilmi�tir. BroIDS'in piyasada benzer i�levlere sahip di�er yaz�l�mlardan ay�rt edici �zelli�i, y�ksek "throughput" seviyelerinde (Gigabitler) �al��abilmesidir.

Bro yaz�l�m� :
* Sald�r� Tespit Sistemi
* Sald�r� Engelleme Sistemi
* A� trafi�i izleme

ama�lar� do�rultusunda kullan�labilmektedir.

Fakat biz bu yaz�da **Sald�r� Tespit Sistemini** inceleyece�iz.
***
### 2) Kurulum

BroIDS kurmak i�in �ncelikle bir Linux sisteminde olmal�s�n�z. 

Kuruluma ba�lamadan �nce sisteminizin **g�ncel** olmas� gerekir. Dolay�s�yla, komut sat�r�n�za : 

> sudo apt-get update && apt-get upgrade

yazarsan�z sisteminiz g�ncellenir. Daha sonra, Bro'nun bilgisayar�n�zda �al��abilmesi i�in �nceden y�kl� olmas� gereken paketleri y�klemelisiniz. Bunun i�in :

> sudo apt-get install cmake make gcc g++ flex bison libpcap-dev libssl-dev python-dev swig zlib1g-dev

yazarsan�z b�t�n gerekli paketler indirilecektir. Art�k Bro'yu indirmek i�in sistemimiz haz�r.

�imdi, Bro'yu indirmek istedi�iniz dizine gidin. �rne�in, 

> cd ~/ids/

Bro yaz�l�m� a��k kaynakl� oldu�undan a��k kaynak yaz�l�mlar�n bulundu�u Git sitesinde de bir deposu (repository) bulunmaktad�r. Dolay�s�yla yaz�l�m� indirmek i�in sadece o depodaki dosyalar� bilgisayar�m�za indirmemiz yeterlidir. Bunun i�in komut sat�r�n�za :

> git clone --recursive git://git.bro.org/bro

yaz�n. Bro, otomatik olarak bulundu�unuz dizine indirlecektir. Dosyalar elimizde oldu�una g�re �imdi kuruluma ge�ebiliriz. �ncelikle indirdi�imiz Bro klas�r�ne gitmemiz gerekiyor. E�er hala ayn� dizinde iseniz komut sat�r�na :

> cd bro

girin. Do�ru klas�rde olup olmad���n�z� anlamak i�in "_ls_" komutu girin. A�a��dakine benzer bir ��kt� alman�z gerekir : 

```sh
root@kali:~/bro# ls
aux              build    CMakeLists.txt  doc       man   README      src
bro-config.h.in  CHANGES  configure       INSTALL   NEWS  README.rst  testing
bro-path-dev.in  cmake    COPYING         Makefile  pkg   scripts     VERSION
```

Daha sonra kuruluma ba�lamak i�in :

> ./configure && make && make install

yaz�n. Sonra kurulumunuz ba�lar. Tercihen, Bro'yu kuraca��n�z dizini belirlemek istiyorsan�z "- -prefix" parametresi eklemeniz gerekir. �rnek : 

> ./configure --prefix=/etc/bro && make && make install

Eklemezseniz otomatik olarak "/usr/local/bro" klas�r�ne kurar.

Kurulumunuz bitince Bro kullan�ma haz�rd�r.

Son olarak Bro'yu her dizinden komut sat�r�ndan �al��t�rabilmek i�in "PATH"e ekleriz (e�er - -prefix parametresini eklemediyseniz) :

> export PATH=/usr/local/bro/bin:$PATH

### 3) Kullan�m
***
##### a) broctl

Bro, yaz�l�m� kontrol etmek i�in bir kontrol beti�ine (script) sahiptir ; **broctl**

Komut sat�r�na :

> broctl

girin ve Bro aray�z�yle kar��la�acaks�n�z. Burada :

> help

komutu girerseniz size yard�mc� olacak, hangi komutlar� girebilece�inizi yazan ekran gelir. Komut sat�r�na :

> start 

girin. Bro bu komut sonras�nda �al���r duruma ge�er. Emin olmak i�in : 

> status

komutunu girin. Bro'nun prosesi (process) hakk�nda bilgi alacaks�n�z.

Bro �u anda paketleri yakalay�p i�erisinde zaten bulunmakta olan imzalara y�nelik alarmlar �retmekte.

Birka� komut �rne�i :

```sh
stop        # Bro'yu durdurur
update      # Konfig�rasyon dosyalar�n� g�nceller
restart     # Bro'yu yeniden ba�lat�r
exit        # broctl'den ��kar
diag        # Herhangi bir sorun ��k�nca loglar� bu komutla g�rebilirsiniz
```
##### b) Loglar

Bro'nun �retti�i loglar� g�rebilmek i�in �nce **broctl**'den ��k�n :

> exit

�imdi, Bro'nun �retti�i loglara g�zatmak i�in a�a��daki komut ile internetten bir dosya indirin : 

> wget www.testmyids.com

Sonra Bro'yu kurdu�unuz dizindeki "logs" klas�r�ne gidin :

> cd /usr/local/bro/logs/

Burada :

> ls

komutunu �al��t�r�n. ��kt� olarak �u ana kadar �retilen loglar�n tarih olarak ay�rt edilmi� olaca��n� ve kaydetmekte oldu�unuz trafik loglar�n� "current" klas�r� i�inde g�receksiniz :

> cd current

Sonra :

> ls

Burada kategorilere ayr�lm�� log dosyalar�n� g�receksiniz (http.log, dns.log, weird.log, conn.log... vb.). Herhangi birini incelemek i�in, �rne�in :

> less http.log

komutunu girin. Burada Bro'nun �retti�i http loglar�n� g�rmektesiniz. Bro herhangi bir zararl� veya dikkate de�er paket yakalarsa bunu notice.log i�erisine kaydeder.

##### c) �mzalar

Bro, imzalar� i�in kendine �zg� bir dile sahiptir. �rnek bir imza dosyas�n�n i�eri�ine bakmak istiyorsan�z a�a��daki komutu girin :

> less /usr/local/bro/share/bro/base/protocols/dhcp/dpd.sig

Kar��n�za buna benzer bir ekran gelir :

```
signature dhcp_cookie {
  ip-proto == udp
  payload /^.*\x63\x82\x53\x63/
  enable "dhcp"
}
```

Bu imza dhcp paketleri i�in yaz�lm��t�r. �mzan�n anlam� :
* Protokol� UDP olan
* paketin i�erik (payload) k�sm�nda "/^.*\x63\x82\x53\x63/" yani "cSc" gibi bir �ey varsa
* Bro'nun i�inde var olan dhcp protokol analizcisini etkinle�tir

�mzalar�n payload k�sm�, iki tane "/" karakteri aras�ndaki "regular expression"lar� alg�lar.

Kendi imzam�z� yazmay� ��renmek istiyorsak Bro'nun https://www.bro.org/sphinx/frameworks/signatures.html sitesini ziyaret edebiliriz.

Bro, imzalar�n� /usr/local/bro/share/bro/base/protocols/ dizininin alt klas�rlerinde tutuyor. Bu klas�re gidersek :

> cd /usr/local/bro/share/bro/base/protocols/

ve 

> ls

�al��t�r�rsak, baz� protokolleri g�r�r�z (http, ftp, dchp vb.) bunlardan herhangi birine girince :

> cd http

ve

> ls

burada http protokol�n� analiz etmek i�in kullan�lan dosyalar var. Bunlardan "sig" uzant�l� dosya imzalar�n (signature) oldu�u dosyad�r. Basit bir imza yazacak olursak :

> nano dpd.sig

dosyada g�rebilece�iniz gibi �nceden yaz�lm�� imzalar var. Bir tane basit bir imza ekleyecek olursak :

```
signature imza {

  ip-proto == tcp
  dst-port == 80
  event "basarili"

}
```

Bu imza :
* Protokol TCP ise
* Hedef portu 80 ise
* Alarm �retir ve mesaj�n� "basarili" olarak belirler

�imdi imzam�z� g�rmesi i�in Bro'yu ba�tan ba�latmam�z gerekli : 

> broctl restart

E�er Bro ba�larken hata olu�ursa yaz�m hatalar�n� kontrol edin.

Daha sonra alarm �retebilecek bir paket al��-veri�inde bulunal�m :

> wget www.google.com

�mzam�z�n �al���p �al��mad���n� g�rebilmek i�in daha �nce bakt���m�z http.log dosyas�na bakal�m : 

> cat /usr/local/bro/logs/current/notice.log && grep -i --color basarili

Bu komutu giridi�inizde kar��n�za �retilen alarm ve detaylar� ��kmal�d�r.

Bu arada dikkat ettiyseniz "notice.log" dosyas� olu�mu�. �ncesinde log dosyalar�na bakt���m�zda yoktu. Bunun nedeni Bro'nun bir imzaya uyan paket g�r�p tehdit olarak "notice.log"a eklemesidir.

Ba�ka bir imza yazacak olursak :

```
signature karaliste {

  ip-proto == tcp
  dst-ip == 104.154.89.105
  event "saldiri"

}
```

Bu imzada :
* Protokol TCP ise
* Hedef IP karalisteye (blacklisting) ald���m�z IP'lerden biriyse
* Alarm �retir ve mesaj�n� "saldiri" olarak tan�mlar

Bu imzay� a�a��daki komutla yakalabiliriz :

> wget www.badssl.com

Ba�ka bir imza �rne�i :

```
signature LAND-sig {

  same-ip
  event "LAND saldirisi!"

}
```

Bu imzada ise sistem, LAND sald�r�s�n� yakal�yor. Hedef ve kaynak IP adresi e�it ise "LAND saldirisi!" mesajl� bir alarm �retir.

Son bir imza �rne�i ise :

```
signature attack-sig {
    ip-proto == tcp
    dst-port == 80
    payload /.*root/
    event "root"
}
```

Bu imza :
* TCP kullan�yorsa
* Hedef port 80 ise
* ��eri�inde ".*root" olan
* Paket g�rd���nde mesaj� "root" olan bir alarm �retir.


