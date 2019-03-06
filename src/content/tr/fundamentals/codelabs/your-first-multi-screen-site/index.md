project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Web'e, küçük ekranli telefonlardan genis ekranli televizyonlara kadar çok çesitli cihazlardan erisilebilir. Tüm bu cihazlarda iyi bir sekilde çalisacak bir siteyi nasil olusturacaginizi ögrenin.

{# wf_updated_on: 2014-01-05 #} {# wf_published_on: 2013-12-31 #}

# Ilk Çoklu Cihaz Siteniz {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

Çoklu cihaz deneyimleri olusturmak sanildigi kadar zor degildir. Bu kilavuzu izleyerek [CS256 Mobil Web Gelistirme kursumuz](https://www.udacity.com/course/mobile-web-development--cs256) için farkli cihaz türlerinin tümünde iyi sekilde çalisan bir örnek ürün açilis sayfasi olusturacagiz.

<img src="images/finaloutput-2x.jpg" alt="son projeyi gösteren birçok cihaz" class="attempt-right" />

Farkli yeteneklere, oldukça farkli ekran boyutlarina ve etkilesim yöntemlerine sahip birden çok cihaz için olusturma, baslangiçta imkansiz gibi görünmese de göz korkutucu gelebilir.

Tamamiyla duyarli siteler olusturmak düsündügünüz kadar zor degildir ve size göstermek için bu kilavuzda, baslarken kullanabileceginiz adimlar ayrintili bir sekilde açiklanmaktadir. Bunu iki basit adima ayirdik:

Içerik, bir sitenin en önemli ögesidir. O zaman, içerige göre tasarim yapip tasarimin içerige hükmetmesine izin vermeyelim. Bu kilavuzda, öncelikle ihtiyacimiz olan içerigi tanimlayacagiz, bu içerige dayali bir sayfa yapisi olusturacagiz ve daha sonra, sayfayi, dar ve genis görüntü alanlarinda düzgün çalisan basit bir dogrusal yerlesimde sunacagiz.

1. Sayfanin bilgi mimarisini (genellikle IA olarak bilinir) ve yapisini tanimlama, 2. Sayfanin duyarli olmasi ve tüm cihazlarda iyi görünmesi için tasarim ögeleri ekleme.
2. Adding design elements to make it responsive and look good across all devices.

## Içeriginizi ve Yapinizi Olusturma

Asagidakilere ihtiyacimiz oldugunu belirledik:

### Sayfa yapisini olusturma

Ayrica, hem dar hem de genis görüntü alanlari için kaba bir bilgi mimarisi ve yerlesim de olusturduk.

1. Yüksek bir düzeyde ürünümüz olan `CS256: Mobil web gelistirme` kursunu açiklayan bir alan
2. Ürünümüzle ilgilenen kullanicilardan bilgi toplamak için bir form
3. Ayrintili bir açiklama ve video
4. Ürününün eylem halindeyken resimleri
5. Iddialari destekleyen bilgilerin bulundugu bir veri tablosu

#### Baslik ve formu olusturma

* Öncelikle ihtiyaciniz olan içerigi tanimlayin.
* Dar ve genis görüntü alanlari için Bilgi Mimarisi`ni (IA) tasarlayin.
* Stil olmadan, içerikle birlikte sayfanin bir iskelet görünümünü olusturun.

Bu, projenin geri kalaninda kullanacagimiz iskelet sayfanin kaba bölümlerine kolayca dönüstürülebilir.

<div class="attempt-left">
  <figure>
    <img src="images/narrowviewport.png" alt="Narrow Viewport IA">
    <figcaption>
      Narrow Viewport IA
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img src="images/wideviewport.png" alt="Içerik">
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/getting-started/your-first-multi-screen-site/content-without-styles.html"> Içerik ve yapi</a>
    </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Sitenin temel yapisi tamamlandi. Ihtiyacimiz olan bölümleri, bu bölümlerde görüntülenecek içerigi ve genel bilgi mimarisi içinde bunu nereye yerlestirecegimizi biliyoruz. Artik sitemizi olusturmaya baslayabiliriz.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addstructure.html" region_tag="structure" adjust_indentation="auto" %}
</pre>

Note: Stil daha sonra gelecektir

### TL;DR {: .hide-from-toc }

Baslik ve istek bildirim formu, sayfamizin önemli bilesenleridir. Bunlarin kullaniciya hemen sunulmasi gerekir.

Basliga, kursu açiklamak için basit bir metin ekleyin:

### Sayfaya içerik ekleme

Formu da doldurmamiz gerekiyor. Bu, kullanicilarin adlarini, telefon numaralarini ve onlari geri aramak için uygun olduklari zaman bilgisini toplayan basit bir form olacak.

Kullanicilarin ögelere kolayca odaklanmalarini, içinde ne olmasi gerektigini anlamalarini kolaylastirmak ve ayrica, erisilebilirlik araçlarinin formun yapisini anlamasina yardimci olmak için tüm formlarin etiketleri ve yer tutuculari olmalidir. Ad özelligi yalnizca form degerini sunucuya göndermekle kalmaz, ayni zamanda tarayiciya, formu kullanici adina otomatik olarak nasil dolduracagiyla ilgili önemli ipuçlari da verir.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addheadline.html" region_tag="headline" adjust_indentation="auto" %}
</pre>

Kullanicilarin bir mobil cihazda içerigi hizli ve basit bir sekilde girebilmelerini saglamak için anlamsal türler ekleyecegiz. Örnegin, bir telefon numarasi girerken kullanicinin yalnizca bir tus takimi görmesi gerekir.

{# include shared/related_guides.liquid inline=true list=page.related-guides.create-amazing-forms #}

Içerigin Video ve Bilgi bölümü biraz daha derinlige sahiptir. Ürünlerimizin özelliklerinin madde imli bir listesini ve ayrica, bir kullaniciyi ürünümüzü kullanirken gösteren bir video yer tutucusunu içerir.

Videolar genellikle içerigi daha etkilesimli bir sekilde açiklamak ve siklikla bir ürünün veya kavramin tanitim gösterisini göstermek için kullanilir.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addform.html" region_tag="form" adjust_indentation="auto" %}
</pre>

En iyi uygulamalari izleyerek videoyu sitenize kolayca entegre edebilirsiniz:

#### Video ve Bilgi bölümünü olusturma

{# include shared/related_guides.liquid inline=true list=page.related-guides.video #}

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section1" adjust_indentation="auto" %}
</pre>

Resim bulunmayan siteler biraz sikici olabilir. Iki tür resim vardir:

Sayfamizdaki Resimler bölümü, içerik resimlerinden olusan bir koleksiyondur.

Içerik resimleri, sayfanin anlamini aktarma açisindan önemlidir. Bunlari, gazete makalelerinde kullanilan resimler gibi düsünebilirsiniz. Kullandigimiz resimler, projenin egitmenleri olan Chris Wilson, Peter Lubbers ve Sean Bennet'in resimleridir.

* Kullanicilarin videoyu oynatmalarini kolaylastirmak için bir `controls` özelligi ekleyin.
* Kullanicilara içerigin bir önizlemesini saglamak için bir `poster` resmi ekleyin.
* Desteklenen video biçimlerine göre birden çok `<source>` ögesi ekleyin.
* Kullanicilarin videoyu pencerede oynatamamalari durumunda indirebilmelerini saglamak için yedek metin ekleyin.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addvideo.html" region_tag="video"   adjust_indentation="auto" %}
</pre>

Resimler, ekran genisliginin %100 ölçegine ayarlanmistir. Bu, dar görüntü alani olan cihazlarda iyi bir sekilde çalisir, ancak genis görüntü alanina sahip cihazlarda (masaüstü bilgisayarlar gibi) çok iyi görünmez. Bunu, duyarli tasarim bölümünde ele alacagiz.

#### Resimler Bölümünü Olusturma

{# include shared/related_guides.liquid inline=true list=page.related-guides.images #}

* Içerik resimleri: Dokümanla ayni dogrultuda olan ve içerikle ilgili fazladan bilgileri aktarmak için kullanilan resimler.
* Biçimsel resimler: Sitenin daha iyi görünmesi için kullanilan resimler; bunlar genellikle arka plan resimleri, kaliplar ve renk geçisleri seklindedir. Bu konuyu [sonraki makale](#) baslikli makalede ele alacagiz.

Birçok kisi, resimleri görüntüleyemez ve genellikle, sayfadaki verileri ayristiran ve bunu kullaniciya sözlü olarak aktaran ekran okuyucu gibi bir yardimci teknoloji kullanir. Tüm içerik resimlerinizde, ekran okuyucunun resmi kullaniciya açiklayabilmesi için bir tanimlayici `alt` etiketinin bulundugundan emin olun.

`alt` etiketlerini eklerken, resmi tam olarak açiklamak için alt metnini mümkün oldugunca kisa tuttugunuzdan emin olun. Örnegin, demomuzda özelligi `Ad: Rol` olacak sekilde biçimlendiriyoruz. Bu, kullanicinin bu bölümün yazarlar ve yazarlarin isleriyle ilgili oldugunu anlamasi için yeterli bilgi sunmaktadir.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addimages.html" region_tag="images"   adjust_indentation="auto" %}
</pre>

Son bölüm, ürünle ilgili belirli ürün verilerini göstermek için kullanilan basit bir tablodur.

Tablolar yalnizca bilgi matrisleri gibi tablo seklindeki veriler için kullanilmalidir.

Çogu sitenin Sartlar ve Kosullar, sorumlulugun reddi beyanlari gibi içerigi ve sayfanin ana gezinme veya ana içerik alaninda bulunmamasi gereken baska içerikleri görüntülemek için bir altbilgiye ihtiyaci vardir.

Bizim sitemizde, yalnizca Sartlar ve Kosullar'a, bir Iletisim sayfasina ve sosyal medya profillerimize baglanti verecegiz.

#### Tablolastirilmis Veri Bölümünü Ekleme

Sitenin ana hatlarini olusturduk ve ana yapisal ögelerin tümünü tanimladik. Ayrica, ilgili tüm içerigin is gereksinimlerimizi karsilamak üzere hazir ve yerinde oldugundan emin olduk.

Sayfanin su anda kötü göründügünü fark etmissinizdir; bunu bilinçli olarak böyle yaptik. Içerik, bir sitenin en önemli ögesidir ve saglam bir bilgi mimarisine ve yogunluga sahip oldugumuzdan emin olmamiz gerekiyordu. Bu kilavuz, bize dayanabilecegimiz harika bir temel verdi. Sonraki kilavuzda içerigimizi biçimlendirecegiz.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section3" adjust_indentation="auto" %}
</pre>

Web'e, küçük ekranli telefonlardan genis ekranli televizyonlara kadar çok çesitli cihazlardan erisilebilir. Her bir cihaz kendi benzersiz avantajlarina, ayni zamanda kisitlamalarina sahiptir. Bir web gelistiricisi olarak tüm cihazlari desteklemeniz beklenir.

#### Altbilgi Ekleme

Birden çok ekran boyutunda ve cihaz türünde çalisacak bir site olusturuyoruz. [Önceki makale](#) baslikli makalede, sayfanin Bilgi Mimarisi'ni isledik ve bir temel yapi olusturduk. Bu kilavuzda, içerikle temel yapimizi alip çok sayida ekran boyutuna duyarli güzel bir sayfaya dönüstürecegiz.

Önce Mobil Cihazlar web gelistirme ilkelerini izleyerek bir cep telefonuna benzeyen, dar bir görüntü alaniyla basliyoruz ve önce bu deneyimi olusturuyoruz. Daha sonra, deneyimi daha genis cihaz siniflarina ölçekliyoruz. Bunu, görüntü alanimizi genisleterek ve tasarim ile yerlesimin düzgün görünüp görünmedigine karar vererek yapabiliriz.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="footer" adjust_indentation="auto" %}
</pre>

Daha önce, içerigimizin nasil görünmesi gerektigine dair farkli, üst düzey birkaç tasarim olusturmustuk. Simdi sayfamizin bu farkli yerlesimlere uyarlanmasini saglamamiz gerekiyor. Bunu, içerigi ekran boyutuna sigdirma yöntemimize göre kesme noktalarimizi nereye yerlestirecegimize karar vererek yapiyoruz. Kesme noktalari, yerlesim ve stil degisikliginin gerçeklestigi noktalardir.

### Özet

Temel bir sayfa için bile her zaman bir görüntü alani meta etiketi **kullanmaniz gerekir**. Görüntü alani, birden çok cihaz deneyimi olusturmak için ihtiyaciniz olan en önemli bilesendir. Bu olmadan, siteniz bir mobil cihazda düzgün çalismaz.

<div class="attempt-left">
  <figure>
    <img src="images/content.png" alt="Içerik">
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/getting-started/your-first-multi-screen-site/content-without-styles.html"> Içerik ve yapi</a>
    </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img  src="images/narrowsite.png" alt="Designed site" style="max-width: 100%;">
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/getting-started/your-first-multi-screen-site/content-with-styles.html"> Son site </a>
    </figcaption>
  </figure>
</div>

Görüntü alani, tarayiciya sayfanin ekrana sigmasi için ölçeklenmesi gerektigini bildirir. Görüntü alaninizin sayfanin görüntüsünü kontrol etmesi için belirtebileceginiz birçok farkli yapilandirma vardir. Varsayilan olarak sunlari öneririz:

## Duyarli Olmasini Saglayin

Görüntü alani, dokümanin basinda yer alir ve yalnizca bir kez açiklanmasi gerekir.

{# include shared/related_guides.liquid inline=true list=page.related-guides.responsive #}

Ürünümüzün ve sirketimizin bir stil kilavuzunda saglanmis çok özel marka bilinci olusturma ve yazi tipi yönergeleri zaten vardir.

Stil kilavuzu, sayfanin görsel gösterimini bir üst düzeyde anlamanin yararli bir yoludur ve tasarim genelinde tutarli oldugunuzdan emin olmaniza yardimci olur.

### TL;DR {: .hide-from-toc }

* Her zaman bir görüntü alani kullanin.
* Her zaman önce dar bir görüntü alaniyla baslayip daha sonra bunu genisletin.
* Içerigi uyarlamaniz gerektiginde bunu kesme noktalariniza dayandirin.
* Ana kesme noktalarinizdan yerlesiminizin bir üst düzey görüntüsünü olusturun.

### Bir görüntü alani ekle

Önceki kilavuzda, "içerik resimleri" adli resimleri ekledik. Bunlar, ürünümüzün anlatilmasi açisindan önemli resimlerdi. Biçimsel resimler, temel içerigin parçasi olarak ihtiyaç duyulmayan, ancak görsel gösteris ekleyen veya kullanicinin dikkatini belirli bir içerik parçasina yönlendirmeye yardimci olan resimlerdir.

Buna iyi bir örnek, "ekranin üst kismi"ndaki içerik için bir manset resmi kullanilmasi olabilir. Genellikle, kullaniciyi ürünle ilgili daha fazla bilgi okumaya ikna etmek için kullanilir.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/viewport.html" region_tag="viewport" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/viewport.html){: target="_blank" .external }

Eklenmeleri çok basit olabilir. Bizim örnegimizde, bu basligin arka plani olacaktir ve bazi basit CSS'ler araciligiyla uygulanacaktir.

### Basit stil uygula

Dikkati içerikten uzaklastirmamasi için bulaniklastirilmis, basit bir arka plan resmi seçtik ve bunu tüm ögeyi `kaplayacak` sekilde ayarladik; böylece, dogru en boy oranini korurken her zaman uzatilir.

#### Stil kilavuzu

A style guide is a useful way to get a high-level understanding of the visual representation of the page and it helps you ensure that you are consistent throughout the design.

#### Biçimsel resimler eklemek

<div class="styles" style="font-family: monospace;">
  <div style="background-color: #39b1a4">#39b1a4</div>
  <div style="background-color: white">#ffffff</div>
  <div style="background-color: #f5f5f5">#f5f5f5</div>

  <div style="background-color: #e9e9e9">#e9e9e9</div>
  <div style="background-color: #dc4d38">#dc4d38</div>
</div>

#### Form ögesini hareket ettirme

<img  src="images/narrowsite.png" alt="Designed site"  class="attempt-right" />

600 piksel, ilk kesme noktamizi olusturmak için iyi bir yer gibi görünüyor. Bu, ögeleri ekrana daha iyi sigmalari amaciyla yeniden konumlandirmamiz için gereken alani saglayacak. Bunu, [Medya Sorgulari](/web/fundamentals/design-and-ux/responsive/#use-css-media-queries-for-responsiveness) adli bir teknolojiyi kullanarak yapabiliriz.

Daha genis bir ekranda daha fazla alan vardir, dolayisiyla içerigin nasil görüntülenebilecegi konusunda daha esnek olunabilir.

Note: Tüm ögeleri ayni anda tasimaniz gerekmez, gerektikçe küçük ayarlamalar yapabilirsiniz.

<div style="clear:both;"></div>

    #headline {
      padding: 0.8em;
      color: white;
      font-family: Roboto, sans-serif;
      background-image: url(backgroundimage.jpg);
      background-size: cover;
    }
    

Ürün sayfamiz baglaminda görünüse göre sunlari yapmamiz gerekecek:

### İlk kesme noktamizi ayarlayın

{# include shared/related_guides.liquid inline=true list=page.related-guides.first-break-point #}

<video controls poster="images/firstbreakpoint.png" style="width: 100%;">
  <source src="videos/firstbreakpoint.mov" type="video/mov"></source>
  <source src="videos/firstbreakpoint.webm" type="video/webm"></source>
  <p>Maalesef tarayiciniz videoyu desteklemiyor.
     <a href="videos/firstbreakpoint.mov">Videoyu indirin</a>.
  </p>
</video>

Yalnizca iki ana yerlesimimizin olmasini seçtik: dar bir görüntü alani ve genis bir görüntü alani. Bu seçimimiz, olusturma sürecimizi büyük ölçüde basitlestirecek.

    @media (min-width: 600px) {
    
    }
    

Ayrica, dar görüntü alaninda kenarliksiz bölümler olusturmaya ve bu bölümlerin, genis görüntü alaninda kenarliksiz kalmasina karar verdik. Bu, metin ve paragraflarin ultra genis ekranlarda tek, uzun bir satira uzamamasi için ekranin maksimum genisligini sinirlamamiz gerektigi anlamina gelir. Bu noktanin yaklasik 800 pikselde olmasini seçtik.

Bunu gerçeklestirmek için genisligi sinirlandirmamiz ve ögeleri ortalamamiz gerekir. Her bir ana bölümün çevresinde bir kapsayici olusturup bir `margin: auto` kodu uygulamaliyiz. Bu, ekranin büyümesine, ancak içerigin ortalanmis bir sekilde ve maksimum 800 piksel boyutta kalmasina olanak taniyacak.

Kapsayici, asagidaki biçimde basit bir `div` ögesi olacak:

* Tasarimin maksimum genisligini sinirlama.
* Ögelerin dolgusunu degistirme ve metin boyutunu küçültme.
* Formu, baslik içerigiyle hizali bir sekilde hareket etmesi için tasima.
* Videonun içerik çevresinde hareket etmesini saglama.
* Resimlerin boyutunu küçültme ve daha hos bir tabloda görünmelerini saglama.

### Tasarimin maksimum genisligini sinirlama

Dar görüntü alaninda, içerigi görüntülemek için çok fazla alanimiz olmadigindan tipografinin boyutu ve agirligi, ekrana sigmasi için çogunlukla önemli ölçüde küçültülür.

Daha genis bir görüntü alanimiz oldugunda kullanicinin daha genis bir ekranda, ancak ekrandan biraz daha uzakta bulunma olasiligini göz önünde bulundurmamiz gerekir. Içerigin okunabilirligini artirmak için tipografinin boyutunu ve agirligini artirabilir ve ayri alanlari daha da ayirmak için dolguyu degistirebiliriz.

Ürün sayfamizda, bölüm ögelerinin dolgusunu genisligin %5'inde kalacak sekilde ayarlayarak dolguyu artiracagiz. Ayrica, her bir bölümün basliklarinin boyutunu da artiracagiz.

Dar görüntü alanimiz, yigin seklinde dogrusal bir görüntüydü. Her bir ana bölüm ve içindeki içerik, yukaridan asagiya dogru görüntüleniyordu.

    <div class="container">
    ...
    </div>
    

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="containerhtml"   adjust_indentation="auto" %}
</pre>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="container"   adjust_indentation="auto" %}
</pre>

Genis görüntü alani, bize içerigi ilgili ekran için en uygun sekilde görüntülemek üzere kullanabilecegimiz fazladan alan verir. Ürün sayfamiz için bu, IA'miza göre asagidakileri yapabilecegimiz anlamina gelir:

### Dolguyu degistirme ve metin boyutunu küçültme

Dar görüntü alani, ögeleri ekrana rahatça yerlestirmek için çok daha az bir yatay alani kullanabilecegimiz anlamina gelir.

Yatay ekran alanini daha etkili kullanmak için basligin dogrusal akisindan çikmamiz ve form ile listeyi yan yana olacaklari sekilde tasimamiz gerekir.

Dar görüntü alani arayüzündeki video, ekranin tam genisligini kaplayacak ve önemli özellikler listesinden sonre yerlestirilecek sekilde tasarlanmistir. Genis bir görüntü alaninda, video çok genisleyecek sekilde ölçeklenir ve özellik listemizin yanina yerlestirildiginde yanlis görünür.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding"   adjust_indentation="auto" %}
</pre>

Video ögesinin, dar görüntü alaninin dikey akisinin disina tasinmasi ve genis bir görüntü alaninda içerigin madde imli listesiyle yan yana görüntülenmesi gerekir.

### Ögeleri genis görüntü alanina uyarlama

Dar görüntü alani arayüzündeki resimler (çogunlukla mobil cihazlar), ekranin tam genisligini kaplayacak ve dikey olarak yigilacak sekilde ayarlanir. Bu, genis görüntü alaninda iyi bir sekilde ölçeklenmez.

Resimlerin genis bir görüntü alaninda dogru görünmesini saglamak için kapsayici genisliginin %30'una ölçeklenirler ve (dar görüntü alanindaki gibi dikey yerlestirilmeleri yerine) yatay olarak yerlestirilirler. Ayrica, resimleri daha çekici hale getirmek için biraz sinir yariçapi ve kutu gölgesi de ekleyecegiz.

* Formu, baslik bilgileri çevresinde hareket ettirme.
* Videoyu, önemli noktalarin sagina yerlestirme.
* Resimleri döseme.
* Tabloyu genisletme.

#### Video ögesini hareket ettirme

The narrow viewport means that we have a lot less horizontal space available for us to comfortably position elements on the screen.

Resimleri kullanirken, görüntü alaninin boyutunu ve ekranin yogunlugunu dikkate alin.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="formfloat"   adjust_indentation="auto" %}
</pre>

Web baslangiçta 96 dpi degerindeki ekranlar için olusturulmustu. Mobil cihazlarin kullanilmaya baslanmasiyla ekranlarin piksel yogunlugunda büyük bir artis gördük; dizüstü bilgisayarlarin Retina sinifi ekranlarindan hiç söz etmiyoruz bile. Bunun gibi, 96 dpi degerine kodlanan resimler genellikle yüksek dpi degerine sahip bir cihazda berbat görünür.

<video controls poster="images/floatingform.png" style="width: 100%;">
  <source src="videos/floatingform.mov" type="video/mov"></source>
  <source src="videos/floatingform.webm" type="video/webm"></source>
  <p>Maalesef tarayiciniz videoyu desteklemiyor.
     <a href="videos/floatingform.mov">Videoyu indirin</a>.
  </p>
</video>

#### Resimleri Döseme

Bunun için henüz yaygin bir sekilde benimsenmemis bir çözümümüz var. Bunu destekleyen tarayicilarda, yüksek yogunluga sahip bir ekranda yüksek yogunluga sahip bir resim görüntüleyebilirsiniz.

{# include shared/related_guides.liquid inline=true list=page.related-guides.images #}

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding"   adjust_indentation="auto" %}
</pre>

Tablolarin, dar görüntü alani olan cihazlarda dogru bir sekilde görüntülenmesi çok zordur ve bunlara özel olarak dikkat edilmesi gerekir.

#### Resimleri DPI'ya duyarli yapma

<img src="images/imageswide.png" class="attempt-right" />

Sitemizde, yalnizca tablo içerigi için fazladan bir kesme noktasi olusturmamiz gerekiyordu. Önce bir mobil cihaz için olusturdugunuzda, uygulanan stilleri geri almak daha zor olacagindan, dar görüntü alani tablosu CSS'sini genis görüntü alani CSS'sinden ayirmamiz gerekir. Bu, bize net ve tutarli bir kesme saglar.

**TEBRIKLER.** Bunu okurken, çok sayida cihazda, biçim katsayisinda ve ekran boyutunda çalisan ilk basit ürün açilis sayfanizi olusturmus olacaksiniz.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="floatvideo"   adjust_indentation="auto" %}
</pre>

Bu yönergeleri uygularsaniz iyi bir baslangiç yaparsiniz:

#### Tablolar

When using images, take the size of the viewport and the density of the display into consideration.

The web was built for 96dpi screens. With the introduction of mobile devices, we have seen a huge increase in the pixel density of screens not to mention Retina class displays on laptops. As such, images that are encoded to 96dpi often look terrible on a hi-dpi device.

We have a solution that is not widely adopted yet. For browsers that support it, you can display a high density image on a high density display.

    <img src="photo.png" srcset="photo@2x.png 2x">
    

#### Tables

Tables are very hard to get right on devices that have a narrow viewport and need special consideration.

We recommend on a narrow viewport that you transform your table by changing each row into a block of key-value pairs (where the key is what was previously the column header, and the value is still the cell value). Fortunately, this isn't too difficult. First, annotate each `td` element with the corresponding heading as a data attribute. (This won't have any visible effect until we add some more CSS.)

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/updatingtablehtml.html" region_tag="table-tbody" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/updatingtablehtml.html){: target="_blank" .external }

Now we just need to add the CSS to hide the original `thead` and instead show the `data-th` labels using a `:before` pseudoelement. This will result in the multi-device experience seen in the following video.

<video controls poster="images/responsivetable.png" style="width: 100%;">
  <source src="videos/responsivetable.mov" type="video/mov"></source>
  <source src="videos/responsivetable.webm" type="video/webm"></source>
  <p>Maalesef tarayiciniz videoyu desteklemiyor.
     <a href="videos/responsivetable.mov">Videoyu indirin</a>.
  </p>
</video>

In our site, we had to create an extra breakpoint just for the table content. When you build for a mobile device first, it is harder to undo applied styles, so we must section off the narrow viewport table CSS from the wide viewport css. This gives us a clear and consistent break.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/content-with-styles.html" region_tag="table-css"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-with-styles.html){: target="_blank" .external }

## Wrapping up

Success: By the time you read this, you will have created your first simple product landing page that works across a large range of devices, form-factors, and screen sizes.

If you follow these guidelines, you will be off to a good start:

1. Bir temel IA olusturun ve kodlamadan önce içeriginizi anlayin.
2. Her zaman bir görüntü alani ayarlayin.
3. Temel deneyiminizi önce mobil cihazlar yaklasimiyla olusturun.
4. Mobil deneyiminizi olusturduktan sonra, görüntünün genisligini görüntü bozulmaya baslayana kadar artirin ve kesme noktanizi buraya ayarlayin.
5. Bunu tekrarlamaya devam edin.