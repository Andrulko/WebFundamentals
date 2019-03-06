project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Bir seyin ag üzerinden getirilmesi yavas ve pahalidir: Büyük yanitlar, istemci ile sunucu arasinda çok sayida gelis gidis gerçeklestirmesini gerektirdiginden kullanilabilir duruma gelmeleri ve tarayici tarafindan islenebilmeleri gecikir, ayrica ziyaretçi için veri maliyetleri olur. Sonuç olarak, önceden getirilen kaynaklari önbellege alma ve yeniden kullanma yetenegi, performansi optimize etmenin önemli bir unsurudur.

{# wf_updated_on: 2014-01-04 #} {# wf_published_on: 2013-12-31 #}

# HTTP'yi önbellege alma {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Bir seyin ag üzerinden getirilmesi yavas ve pahalidir: Büyük yanitlar, istemci ile sunucu arasinda çok sayida gelis gidis gerçeklestirmesini gerektirdiginden kullanilabilir duruma gelmeleri ve tarayici tarafindan islenebilmeleri gecikir, ayrica ziyaretçi için veri maliyetleri olur. Sonuç olarak, önceden getirilen kaynaklari önbellege alma ve yeniden kullanma yetenegi, performansi optimize etmenin önemli bir unsurudur.

Güzel bir haberimiz var, her tarayici bir HTTP önbellegi uygulamasiyla saglanir! Bizim tüm yapmamiz gereken, her bir sunucu yanitinin ne zaman ve ne kadar süreyle tarayici tarafindan önbellege alinabilecegini tarayiciya bildirmek için dogru HTTP üstbilgi yönergelerinin saglandigindan emin olmaktir.

Note: Uygulamanizdaki web içerigini getirmek ve görüntülemek için bir Webview kullaniyorsaniz HTTP önbelleginin etkinlestirildiginden, boyutunun kullanim örneginizle eslesecek makul bir degere ayarlandigindan ve önbellegin kalici duruma getirildiginden emin olmak için ek yapilandirma isaretleri saglamaniz gerekebilir. Platform dokümanlarini kontrol edin ve ayarlarinizi onaylayin!

<img src="images/http-request.png"  alt="HTTP istegi" />

Sunucu bir yanit döndürdügünde, HTTP üstbilgilerinden olusan ve içerik türünü, uzunlugunu, önbellege alma yönergelerini, dogrulama belirtecini ve baska bilgileri açiklayan bir koleksiyon da yayinlar. Örnegin, yukaridaki degisimde sunucu bir 1024 bayt yaniti döndürerek istemciye, yaniti en fazla 120 saniye için önbellege almasini bildirir ve yanit süresi dolduktan sonra, kaynagin degisip degismedigini kontrol etmesi için kullanabilecegi bir dogrulama belirteci (`x234dff`) saglar.

## Önbellege alinan yanitlari ETag ögeleriyle dogrulama

### TL;DR {: .hide-from-toc }

* Dogrulama belirteci, ETag HTTP üstbilgisi araciligiyla sunucu tarafindan iletilir
* Dogrulama belirteci, verimli kaynak güncelleme kontrolleri saglar: Kaynak degismediyse veri aktarimi gerçeklestirilmez.

Ilk getirmemizden bu yana 120 saniyenin geçtigini ve tarayicinin, ayni kaynak için yeni bir istek baslattigini düsünelim. Ilk olarak tarayici yerel önbellegi kontrol eder ve önceki yaniti bulur. Yanitin artik `süresi doldugu` için ne yazik ki bunu kullanamaz. Bu noktada, yeni bir istek gönderip yeni tam yaniti getirebilir, ancak kaynak degismediyse zaten önbellekte olan ayni baytlari indirmek için bir neden olmadigindan bu verimsiz bir uygulama olur!

ETag üstbilgisinde belirtilen dogrulama belirteçleri tam da bu sorunu çözmek için tasarlanmistir: Sunucu, genellikle dosya içerigine iliskin karma kod veya baska parmak izi olan bir rastgele belirteç olusturur ve bunu döndürür. Istemcinin parmak izinin nasil olusturuldugunu bilmesine gerek yoktur, buna yalnizca sonraki istekte sunucuya göndermek için gereksinim duyar: Parmak izi hâlâ ayniysa kaynak degismemistir ve indirme islemini atlayabiliriz.

<img src="images/http-cache-control.png"  alt="HTTP Cache-Control örnegi" />

Yukaridaki örnekte istemci, ETag belirtecini `If-None-Match` HTTP istegi üstbilgisinde otomatik olarak saglar, sunucu belirteci geçerli kaynaga göre kontrol eder ve degismemisse tarayiciya, önbellegindeki yanitin degismedigini ve 120 saniye daha yenilenebilecegini bildiren bir `304 Degistirilmedi` yaniti döndürür. Yaniti bir kez daha indirmek zorunda kalmadigimizi unutmayin. Bu, zamandan ve bant genisliginden tasarruf saglar.

Bir web gelistiricisi olarak verimli yeniden dogrulamanin avantajini nasil buluyorsunuz? Tarayici, bizim adimiza tüm isi yapar: Önceden bir dogrulama belirtecinin belirtilip belirtilmedigini otomatik olarak algilar, bunu bir giden istege ekler ve sunucudan aldigi yanita göre önbellek zaman damgalarini gereken sekilde günceller. **Bizim yapmamiz için kalan tek sey sunucunun gerçekten gerekli ETag belirteçlerini sagladigindan emin olmaktir: Gerekli yapilandirma isaretleri için sunucu dokümanlarinizi kontrol edin.**

Note: Ipucu: HTML5 Standart Metin projesi, her bir yapilandirma isareti ve ayari için ayrintili açiklamalarla birlikte en popüler sunucularin tümü için [örnek yapilandirma dosyalari](https://github.com/h5bp/server-configs) içerir: Listede favori sunucunuzu bulun, uygun ayarlari arayin ve yapilandirmalari kopyalayip sunucunuzun önerilen ayarlarla yapilandirildigini onaylayin.

## Cache-Control

### TL;DR {: .hide-from-toc }

* Her bir kaynak, Cache-Control HTTP üstbilgisi araciligiyla kendi önbellege alma politikasini tanimlayabilir
* Cache-Control yönergeleri yaniti kimin, hangi kosullar altinda ve ne kadar süreyle önbellege alabilecegini kontrol eder

En iyi istek, sunucuyla iletisim gerektirmeyen istektir: Yanitin bir yerel kopyasi, tüm ag gecikmesini ortadan kaldirmamiza ve veri aktarimi için ödenecek veri ücretlerinden kurtulmamiza olanak tanir. Bunu yapmak için HTTP belirtimi, sunucunun [farkli Cache-Control yönergeleri](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) döndürmesine olanak tanir. Bu yönergeler, bagimsiz yanitin tarayici ve diger araci önbellekleri tarafindan nasil ve ne kadar süreyle önbellege alinabilecegini kontrol eder.

Note: Cache-Control üstbilgisi, HTTP/1.1 teknik özelliginin parçasi olarak tanimlanmistir ve yanit önbellege alma politikalarini tanimlamak için kullanilan önceki üstbilgilerin (Süresi Dolan) yerini alir. Tüm modern tarayicilar Cache-Control protokolünü desteklediginden, tüm ihtiyacimiz olan budur.

<img src="images/http-cache-control-highlight.png"  alt="HTTP Cache-Control örnegi" />

### `no-cache` ve `no-store`

`no-cache`, önce sunucu yanitin degisip degismedigini kontrol etmeden, döndürülen yanitin ayni URL'ye izleyen bir istegin gönderilmesi için kullanilamayacagini belirtir. Sonuç olarak, uygun bir dogrulama belirteci (ETag) varsa no-cache, önbellege alinan yanitin dogrulanmasi için bir gelis gidis gerçeklestirilmesine neden olur, ancak kaynak degismemisse indirme islemini önleyebilir.

Bunun tersine `no-store` çok basittir. Tarayicinin ve hiçbir araci önbelleginin, döndürülen yanitin (ör. gizli kisisel veya bankacilik verilerini içeren bir yanit) hiçbir sürümünü depolamasina izin vermez. Kullanici bu varligi her istediginde, sunucuya bir istek gönderilir ve her defasinda bir tam yanit indirilir.

### `public` ve `private`

Yanit `public` olarak isaretlenmisse, kendisiyle iliskilendirilmis HTTP dogrulamasi olsa ve yanit durum kodu normalde önbellege alinabilir olmasa bile önbellege alinabilir. Açik önbellege alma bilgileri (`max-age` gibi) yanitin önbellege alinabilir oldugunu belirttigi için çogu zaman `public` olarak isaretlemeye gerek duyulmaz.

Bunun tersine `private` yanitlar tarayici tarafindan önbellege alinabilir, ancak genellikle tek bir kullanici için tasarlanmislardir; dolayisiyla, herhangi bir araci önbellegi tarafindan önbellege alinmalarina izin verilmez. Örnegin, gizli kullanici bilgilerinin bulundugu bir HTML sayfasi, söz konusu kullanicinin tarayicisi tarafindan önbellege alinabilir, ancak bir CDN tarafindan önbellege alinamaz.

### "max-age"

Bu yönerge, istek zamanindan baslayarak getirilen yanitin yeniden kullanilmasina izin verilen maksimum süreyi saniye cinsinden belirtir. Örnegin `max-age=60`, yanitin önbellege alinabilecegini ve sonraki 60 saniye için yeniden kullanilabilecegini belirtir.

## En iyi Cache-Control politikasini tanimlama

<img src="images/http-cache-decision-tree.png"  alt="Önbellek karar agaci" />

Belirli bir kaynaga veya uygulamaniz tarafindan kullanilan bir kaynak kümesine iliskin en iyi önbellege alma politikasini belirlemek için yukaridaki karar agacini izleyin. Ideal kosullarda, istemcide mümkün oldugunca çok yaniti mümkün olan en uzun süre için önbellege almayi ve verimli yeniden dogrulama için her bir yanitin dogrulama belirteçlerini saglamayi amaçlamaniz gerekir.

<table class="responsive">
  
<thead>
  <tr>
    <th colspan="2">Cache-Control yönergeleri</th>
  </tr>
</thead>
<tr>
  <td data-th="cache-control">max-age=86400</td>
  <td data-th="explanation">Yanit, tarayici ve araci önbellekleri tarafindan en fazla 1 gün için (60 saniye x 60 dakika x 24 saat) önbellege alinabilir (`public`)</td>
</tr>
<tr>
  <td data-th="cache-control">private, max-age=600</td>
  <td data-th="explanation">Yanit, yalnizca istemcinin tarayicisi tarafindan en fazla 10 dakika için (60 saniye x 10 dakika) önbellege alinabilir</td>
</tr>
<tr>
  <td data-th="cache-control">no-store</td>
  <td data-th="explanation">Yanitin önbellege alinmasina izin verilmez ve her istekte tam olarak getirilmelidir.</td>
</tr>
</table>

HTTP Archive sitesine göre (Alexa siralamasi temelinde) ilk 300.000 site arasinda [indirilen tüm yanitlarin yariya yakini tarayici tarafindan önbellege alinabilmektedir](http://httparchive.org/trends.php#maxage0). Bu, tekrarlanan sayfa görüntülemeleri ve ziyaretler için büyük bir tasarruf saglar! Elbette bu, sizin uygulamanizin kaynaklarinin %50'sinin önbellege alinabilecegi anlamina gelmez: Bazi siteler kaynaklarinin %90'dan fazlasini önbellege alabilirken, digerleri hiç önbellege alinamayan çok sayida gizli veya zamana duyarli veriye sahip olabilir.

**Hangi kaynaklarin önbellege alinabilecegini tanimlamak ve sayfalarinizin uygun Cache-Control ve ETag üstbilgileri döndürdügünden emin olmak için sayfalarinizi denetleyin.**

## Önbellege alinan yanitlari geçersiz kilma ve güncelleme

### TL;DR {: .hide-from-toc }

* Yerel olarak önbellege alinan yanitlar, kaynagin ''kullanim süresi sona erinceye'' kadar kullanilir
* URL''ye bir dosya içerigi parmak izi yerlestirmemiz, istemciyi yanitin yeni bir sürümüne güncellemeye zorlayabilmemizi saglar
* En iyi performans için her bir uygulamanin kendi önbellek hiyerarsisini tanimlamasi gerekir

Tarayici tarafindan olusturulan tüm HTTP istekleri, istegin yerine getirilmesi için kullanilabilecek önbellege alinmis geçerli bir yanitin olup olmadiginin kontrol edilmesi amaciyla öncelikle tarayici önbellegine yönlendirilir. Bir eslesme bulunursa yanit önbellekten okunur ve hem ag gecikmesini hem de aktarimin neden olacagi veri maliyetlerini ortadan kaldirmis oluruz. **Bununla birlikte, önbellege alinmis bir yaniti güncellemek veya geçersiz kilmak istersek ne olur?**

Örnegin, ziyaretçilerimize bir CSS stil sayfasini en fazla 24 saat için (max-age=86400) önbellege almalarini bildirdigimizi, ancak tasarimcimizin tüm kullanicilarimizin kullanimina sunmak isteyecegimiz yeni bir güncelleme gönderdigini düsünelim. CSS'mizin artik 'eskiyen' önbellege alinmis bir kopyasina sahip tüm ziyaretçileri önbelleklerini güncellemeleri için nasil bilgilendiririz? Bu bir tuzak soru. Bunu yapamayiz, en azindan kaynagin URL'sini degistirmeden yapamayiz.

Yanit tarayici tarafindan önbellege alindiktan sonra, önbellege alinmis sürüm max-age veya expires özellikleriyle belirlenen sekilde yeniligini kaybedinceye veya kullanicinin tarayici önbellegini temizlemesi gibi baska bir nedenle önbellekten çikarilincaya kadar kullanilir. Sonuç olarak, farkli kullanicilar sayfa olusturulurken dosyanin farkli sürümlerini kullanabilir; kaynagi yeni getiren kullanicilar yeni sürümü kullanirken, önceki (ancak hâlâ geçerli) bir kopyayi önbelleklerine almis olan kullanicilar yanitin daha eski bir sürümünü kullanir.

**Peki, istemci tarafinda önbellege alma ile hizli güncellemelerin her ikisinden nasil yararlaniriz?** Basit, kaynagin URL'sini degistirebiliriz ve içerigi degistiginde, kullaniciyi yeni yaniti indirmeye zorlariz. Genellikle bu, dosya adina (ör. style) dosyanin bir parmak izi veya bir sürüm numarasi yerlestirilerek yapilir.**x234dff**.css.

<img src="images/http-cache-hierarchy.png"  alt="Önbellek hiyerarsisi" />

Önbellege alma politikalarini kaynak temelinde tanimlama yetenegi, her bir kaynagin ne kadar süreyle önbellege alinacaginin yani sira yeni sürümlerin ziyaretçi tarafindan ne kadar çabuk görünecegini de kontrol etmemize olanak tanir. Örnegin, yukaridaki örnegi analiz edelim:

* HTML `no-cache` koduyla isaretlenir. Bu, tarayicinin her istekte dokümani her zaman yeniden dogrulayacagi ve içerik degistiyse en son sürümü getirecegi anlamina gelir. Ayrica, HTML biçimlendirmesindeki CSS ve JavaScript varliklarinin URL'lerine parmak izleri yerlestiririz: Bu dosyalarin içerigi degisirse sayfanin HTML'si de degisir ve HTML yanitinin yeni kopyasi indirilir.
* CSS'nin tarayicilar ve araci önbellekleri (ör. bir CDN) tarafindan önbellege alinmasina izin verilir ve 1 yil içinde süresinin sona erecegi ayarlanir. Dosya adina dosya parmak izini yerlestirdigimiz için 1 yil gibi `uzak gelecek süre sonlarini` kullanabildigimizi unutmayin: CSS güncellenirse URL de degisir.
* JavaScript de 1 yil içinde süresi dolacak sekilde ayarlanir, ancak CDN'nin önbellege almamasi gereken bazi gizli kullanici verilerini içerdigi için gizli olarak isaretlenir.
* Resim, bir sürüm veya benzersiz parmak izi olmadan önbellege alinir ve süresi 1 günde dolacak sekilde ayarlanir.

ETag, Cache-Control ve benzersiz URL'lerin birlesimi hepsinden de yararlanmamiza olanak tanir: Uzun vadeli süre sonlari, yanitin nerede önbellege alinabilecegiyle kontrol ve istege bagli güncellemeler.

## Önbellege alma kontrol listesi

Tek bir tane en iyi önbellege alma politikasi yoktur. Trafik kaliplariniza, sunulan verilerin türüne ve veri yeniligi açisindan uygulamaya özel gereksinimlere bagli olarak, kaynak temelinde uygun ayarlari ve genel 'önbellege alma hiyerarsisini' tanimlayip yapilandirmaniz gerekir.

Önbellege alma stratejisi üzerinde çalisirken aklinizda bulundurmaniz gereken bazi ipuçlari ve teknikler:

* **Use consistent URLs:** if you serve the same content on different URLs, then that content will be fetched and stored multiple times. Tip: note that [URLs are case sensitive](http://www.w3.org/TR/WD-html40-970708/htmlweb.html).
* **Ensure that the server provides a validation token (ETag):** validation tokens eliminate the need to transfer the same bytes when a resource has not changed on the server.
* **Identify which resources can be cached by intermediaries:** those with responses that are identical for all users are great candidates to be cached by a CDN and other intermediaries.
* **Determine the optimal cache lifetime for each resource:** different resources may have different freshness requirements. Audit and determine the appropriate max-age for each one.
* **Determine the best cache hierarchy for your site:** the combination of resource URLs with content fingerprints and short or no-cache lifetimes for HTML documents allows you to control how quickly the client picks up updates.
* **Minimize churn:** some resources are updated more frequently than others. If there is a particular part of a resource (for example, a JavaScript function or a set of CSS styles) that is often updated, consider delivering that code as a separate file. Doing so allows the remainder of the content (for example, library code that doesn't change very often), to be fetched from cache and minimizes the amount of downloaded content whenever an update is fetched.

## Feedback {: .hide-from-toc }

{% include "web/_shared/helpful.html" %}

<div class="clearfix"></div>
