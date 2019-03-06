project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Tipografi; iyi tasarim, marka bilinci olusturma, okunabilirlik ve erisilebilirlik özelliklerinin temelini olusturur. Web yazi tipleri yukaridakilerin tümünü ve daha fazlasini saglar: Metin seçilebilir, aranabilir, zum yapilabilir ve yüksek DPI dostudur, ekran boyutu ve çözünürlükten bagimsiz olarak tutarli ve net metin olusturmasi saglar. Web yazi tipleri iyi tasarim, UX ve performans için kritik öneme sahiptir.

{# wf_updated_on: 2014-09-29 #} {# wf_published_on: 2014-09-19 #}

# Web yazi tipi optimizasyonu {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*This article contains contributions from [Monica Dinculescu](https://meowni.ca/posts/web-fonts/), [Rob Dodson](/web/updates/2016/02/font-display), and Jeff Posnick.*

Web yazi tipi optimizasyonu, genel performans stratejisinin kritik bir parçasidir. Her yazi tipi bir ek kaynaktir ve bazi yazi tipleri metnin olusturulmasini engelleyebilir; ancak sayfanin web yazi tiplerini kullanmasi daha yavas olusturulmasi gerektigi anlamina gelmez. Tam tersine, optimize edilmis bir yazi tipi yüklenme sekilleriyle ilgili makul bir stratejiyle birlestirilip sayfaya uygulandiginda, toplam sayfa boyutunu azaltmaya ve sayfa olusturma sürelerini iyilestirmeye yardimci olabilir.

Web yazi tipi, gliflerden olusan bir koleksiyondur ve her bir glif, bir harfi veya sembolü açiklayan bir vektör seklidir. Sonuç olarak, belirli bir yazi tipi dosyasinin boyutu iki basit degisken tarafindan belirlenir: Her bir glifin vektör yollarinin karmasikligi ve belirli bir yazi tipindeki gliflerin sayisi. Örnegin, en popüler web yazi tiplerinden olan Open Sans; Latin, Yunan ve Kiril karakterlerini de kapsayan 897 glif içerir.

## Web yazi tipinin anatomisi

### TL;DR {: .hide-from-toc }

* Unicode yazi tipleri binlerce glif içerebilir
* Dört yazi tipi biçimi vardir: WOFF2, WOFF, EOT, TTF
* Bazi yazi tipi biçimleri GZIP sikistirmasinin kullanilmasini gerektirir

A *webfont* is a collection of glyphs, and each glyph is a vector shape that describes a letter or symbol. As a result, two simple variables determine the size of a particular font file: the complexity of the vector paths of each glyph and the number of glyphs in a particular font. For example, Open Sans, which is one of the most popular webfonts, contains 897 glyphs, which include Latin, Greek, and Cyrillic characters.

<img src="images/glyphs.png"  alt="Font glyph table" />

Açik bir biçimde, tipografinin performansi engellememesi için web'de yazi tiplerini kullanirken dikkatli bir mühendislik çalismasi gerekir. Neyse ki web platformu gerekli tüm temel ögeleri saglar ve bu kilavuzun geri kalaninda, her ikisinden de en iyi sekilde nasil yararlanacagimiza uygulamali bir sekilde bakacagiz.

Bugün web'de dört yazi tipi kapsayici biçimi kullanilmaktadir: [EOT](http://en.wikipedia.org/wiki/Embedded_OpenType), [TTF](http://en.wikipedia.org/wiki/TrueType), [WOFF](http://en.wikipedia.org/wiki/Web_Open_Font_Format) ve [WOFF2](http://www.w3.org/TR/WOFF2/){: .external }. Ne yazik ki çok çesitli seçenekler olmasina ragmen, tüm eski ve yeni tarayicilarda çalisan tek bir evrensel biçim yoktur: EOT [yalnizca IE'de çalisir](http://caniuse.com/#feat=eot), TTF [IE'de kismen desteklenir](http://caniuse.com/#search=ttf), WOFF en genis destegi alir, ancak [bazi eski tarayicilarda kullanilamaz](http://caniuse.com/#feat=woff) ve WOFF 2.0 destegi [üzerinde birçok tarayicinin çalismalari devam etmektedir](http://caniuse.com/#feat=woff2).

### Web yazi tipi biçimleri

Peki bu bizim açimizdan ne anlama geliyor? Tüm tarayicilarda çalisan tek bir biçimin olmamasi, tutarli bir deneyim saglamak için birden çok biçim saglamamiz gerektigi anlamina gelir:

Note: Teknik olarak, [SVG yazi tipi kapsayicisi](http://caniuse.com/svg-fonts) da vardir, ancak bu kapsayici hiçbir zaman IE veya Firefox tarafindan desteklenmemistir ve simdi Chrome'da kullanimdan kaldirilmistir. Böyle olunca da kullanimi sinirli olmustur. Bu kapsayiciyi, bu kilavuzda da özellikle atliyoruz.

* WOFF 2.0 çesidini destekleyen tarayicilara bu çesidi sunun
* Tarayicilarin çogunluguna WOFF çesidini sunun
* Eski Android (4.4 altindaki) tarayicilara TTF çesidini sunun
* Eski IE (IE9 altindaki) tarayicilara EOT çesidini sunun ^

Yazi tipi, gliflerden olusan bir koleksiyondur. Gliflerin her biri, harf biçimini açiklayan bir yol kümesidir. Elbette bagimsiz glifler farklidir, ancak yine de GZIP veya uyumlu bir sikistiriciyla sikistirilabilecek birçok benzer bilgiler içerirler:

### Sikistirmayla yazi tipi boyutunu küçültme

Son olarak, bazi yazi tiplerinin bazi platformlar için gerekli olmayabilecek [yazi tipi ipucu](http://en.wikipedia.org/wiki/Font_hinting) ve [aralik birakma](http://en.wikipedia.org/wiki/Kerning) bilgileri içerdigini unutmamakta fayda vardir. Bu, daha fazla dosya boyutu optimizasyonuna olanak tanir. Kullanabileceginiz optimizasyon seçenekleri için yazi tipi sikistiriciniza danisin ve bu yolu seçerseniz, optimize edilen bu yazi tiplerini test edecek ve her bir ilgili tarayiciya saglayacak uygun altyapiya sahip oldugunuzdan emin olun. Örnegin, Google Yazi Tipleri her bir yazi tipi için 30'un üzerinde optimize edilmis çesit saglar ve her bir platform ve tarayici için en uygun çesidi otomatik olarak algilayip teslim eder.

* Varsayilan olarak EOT ve TTF biçimleri sikistirilmaz: Sunucularinizin bu biçimleri saglarken [GZIP sikistirmasi](/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text-compression-with-gzip) uygulayacak sekilde yapilandirildigindan emin olun.
* WOFF yerlesik sikistirmaya sahiptir. WOFF sikistiricinizin en uygun sikistirma ayarlarini kullanmakta oldugundan emin olun.
* WOFF2, diger biçimlere göre yaklasik %30 daha küçük dosya boyutu saglamak için özel ön isleme ve sikistirma algoritmalari kullanir. [Rapora](http://www.w3.org/TR/WOFF20ER/){: .external } bakabilirsiniz.

Note: EOT, TTF ve WOFF biçimleri için [Zopfli sikistirmasini](http://en.wikipedia.org/wiki/Zopfli) kullanmayi düsünebilirsiniz. Zopfli, gzip'e göre yaklasik olarak %5 dosya boyutu küçültmesi saglayan, zlib uyumlu bir sikistiricidir.

@font-face CSS kuyruklu a kurali, belirli bir yazi tipi kaynaginin konumunu, stil özelliklerini ve yazi tipi için kullanilacak Unicode kod noktalarini tanimlamamiza olanak tanir. Bu tür @font-face bildirimlerinin bir kombinasyonu kullanilarak bir `yazi tipi ailesi` olusturulabilir. Daha sonra, tarayici hangi yazi tipi kaynaklarinin indirilmesi ve geçerli sayfaya uygulanmasi gerektigini degerlendirir. Bunun sahne arkasinda nasil çalistigina daha yakindan bakalim.

## Yazi tipi ailesini @font-face ile tanimlama

### TL;DR {: .hide-from-toc }

* Birden çok yazi tipi biçimi belirtmek için format() ipucunu kullanin
* Performansi iyilestirmek için genis unicode yazi tiplerinin alt kümelerini olusturun: Unicode araligi alt kümesi olusturun ve eski tarayicilar için bir manuel alt küme yedegi saglayin
* Sayfa ve metin olusturma performansini iyilestirmek için biçimsel yazi tipi çesitlerinin sayisini azaltin

Her bir @font-face bildirimi, yazi tipi ailesinin adini saglar. Yazi tipi ailesi birden çok bildirimin, stil, agirlik ve uzatma gibi [yazi tipi özelliklerinin](http://www.w3.org/TR/css3-fonts/#font-prop-desc) ve yazi tipi kaynagina iliskin öncelikli bir konum listesini belirten [src tanimlayicisindan](http://www.w3.org/TR/css3-fonts/#src-desc) olusan bir mantiksal grup gibi hareket eder.

### Biçim seçimi

Ilk olarak, yukaridaki örnekte iki stili (normal ve *italic*) olan tek bir *Awesome Font* ailesinin tanimlandigina ve her bir stilin farkli bir yazi tipi kaynak kümesini isaret ettigine dikkat edin. Dolayisiyla, her bir `src` tanimlayicisi öncelikli, virgülle ayrilmis bir kaynak çesitleri listesi içerir:

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome.woff2') format('woff2'), 
           url('/fonts/awesome.woff') format('woff'),
           url('/fonts/awesome.ttf') format('ttf'),
           url('/fonts/awesome.eot') format('eot');
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: italic;
      font-weight: 400;
      src: local('Awesome Font Italic'),
           url('/fonts/awesome-i.woff2') format('woff2'), 
           url('/fonts/awesome-i.woff') format('woff'),
           url('/fonts/awesome-i.ttf') format('ttf'),
           url('/fonts/awesome-i.eot') format('eot');
    }
    

^ Note: Varsayilan sistem yazi tiplerinden birine basvuruda bulunmuyorsaniz, pratikte kullanicinin, özellikle de ek yazi tiplerinin `yüklenmesinin` gerçekten imkansiz oldugu mobil cihazlarda bunu yerel olarak yüklemis olmasi nadir görülen bir durumdur. Sonuç olarak, her zaman harici yazi tipi konumlarinin bir listesini saglamaniz gerekir.

* `local()` yönergesi, yerel olarak yüklenmis yazi tiplerine basvuruda bulunmamiza, bunlari yüklememize ve kullanmamiza olanak tanir.
* `url()` yönergesi, harici yazi tiplerini yüklememize olanak tanir ve saglanan URL'nin basvuruda bulundugu yazi tipinin biçimini belirten, istege bagli bir `format()` ipucu içermesine izin verilir.

Tarayici yazi tipinin gerektigini belirlediginde, saglanan kaynak listesini belirtilen sirada tekrarlar ve uygun kaynagi yüklemeye çalisir. Örnegin, yukaridaki örnegi izlersek:

Uygun biçim ipuçlariyla yerel ve harici yönergelerin kombinasyonu, kullanilabilir tüm yazi tipi biçimlerini belirtebilmemizi ve geri kalani tarayicinin gerçeklestirmesini saglar: Tarayici, hangi kaynaklarin gerekli oldugunu belirler ve en uygun biçimi bizim adimiza seçer.

1. Tarayici sayfa yer paylasimini gerçeklestirir ve sayfada belirtilen metnin olusturulmasi için gereken yazi tipi çesitlerini belirler.
2. Gereken her bir yazi tipi için tarayici, yazi tipinin yerel olarak mevcut olup olmadigini kontrol eder.
3. Dosya yerel olarak bulunamazsa, harici tanimlari tekrar eder: 
    * Bir biçim ipucu varsa tarayici, indirme islemini baslatmadan önce biçimi destekleyip desteklemedigini kontrol eder; aksi halde bir sonrakine ilerler.
    * Herhangi bir biçim ipucu yoksa tarayici kaynagi indirir.

Note: Yazi tipi çesitlerinin belirtildigi sira önemlidir. Tarayici, destekledigi ilk biçimi seçer. Dolayisiyla, yeni tarayicilarin WOFF2 kullanmasini istiyorsaniz, WOFF2 bildirimini WOFF üzerine yerlestirmeniz gerekir.

Stil, agirlik ve uzatma gibi yazi tipi özelliklerine ek olarak, @font-face kurali her bir kaynak tarafindan desteklenen bir Unicode kod noktalari kümesi tanimlamamiza olanak tanir. Bu, genis bir Unicode yazi tipini küçük alt kümelere bölebilmemizi (ör. Latin, Kiril, Yunan alt kümeleri) ve yalnizca ilgili sayfadaki metni olusturmak için gereken glifleri indirebilmemizi saglar.

### Unicode araligi alt kümesi olusturma

[Unicode araligi tanimlayici](http://www.w3.org/TR/css3-fonts/#descdef-unicode-range), aralik degerlerinin virgülle ayrilmis bir listesini belirtmemize olanak tanir. Bu degerlerin her biri üç farkli biçimden birinde olabilir:

Örnegin, *Awesome Font* ailemizi Latin ve Japonca alt kümelerine bölebiliriz. Daha sonra, bunlarin her biri tarayici tarafindan gerektiginde indirilir:

* Tek kod noktasi (ör. U+416)
* Uzaklik araligi (ör. U+400-4ff): Bir araligin baslangiç ve bitis kod noktalarini belirtir
* Joker karakter araligi (ör. U+4??): `?` karakterler herhangi bir onaltilik sayiyi belirtir

Note: Unicode araligi alt kümesi özellikle Asya dilleri için önemlidir. Bu dillerdeki glif sayisi, bati dillerinden çok daha fazladir ve tipik bir `tam` yazi tipi genellikle onlarca kilobayt yerine megabaytlarla ölçülür!

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-jp.woff2') format('woff2'), 
           url('/fonts/awesome-jp.woff') format('woff'),
           url('/fonts/awesome-jp.ttf') format('ttf'),
           url('/fonts/awesome-jp.eot') format('eot');
      unicode-range: U+3000-9FFF, U+ff??; /* Japanese glyphs */
    }
    

Unicode araligi alt kümelerinin ve her bir biçimsel yazi tipi çesidi için ayri dosyalarin kullanilmasi, hem daha hizli hem de daha verimli bir sekilde indirilen bir bilesik yazi tipi ailesi tanimlamamiza olanak tanir. Ziyaretçi yalnizca gereksinim duydugu çesitleri ve alt kümeleri indirir ve sayfada hiç görmeyecegi veya kullanmayacagi alt kümeleri indirmeye zorlanmaz.

Bununla birlikte, unicode araligiyla ilgili küçük bir nokta söz konusudur: Henüz [tüm tarayicilar tarafindan desteklenmemektedir](http://caniuse.com/#feat=font-unicode-range). Bazi tarayicilar unicode araligi ipucunu göz ardi edip tüm çesitleri indirirken, digerleri @font-face bildirimini hiç isleyemez. Bu konuyu ele almak üzere eski tarayicilar için "manuel alt küme" yedegi saglamamiz gerekir.

Eski tarayicilar gerekli alt kümeleri seçecek kadar akilli olmadiklari ve bir bilesik yazi tipi olusturmayacaklari için gerekli tüm alt kümeleri içeren tek bir yazi tipi kaynagi saglamak ve geri kalani tarayicidan gizlemek için bir yedegimizin olmasi gerekir. Örnegin, sayfa yalnizca Latin karakterleri kullaniyorsa, diger glifleri çikarabilir ve bu alt kümeyi bagimsiz bir kaynak olarak sunabiliriz.

Her bir yazi tipi ailesi birden çok biçimsel çesit (normal, kalin, italik) ve her bir stil için her biri çok farkli glif sekilleri içerebilecek birden çok agirliktan (ör. farkli aralik, boyutlandirma veya tamamiyla farkli bir sekil) olusur.

1. **Hangi alt kümelerin gerektigini nasil belirleriz?** 
    * Tarayici unicode araligi alt kümesini destekliyorsa, dogru alt kümeyi otomatik olarak seçer. Sayfanin yalnizca alt küme dosyalarini saglamasi ve @font-face kurallarinda uygun unicode araliklarini belirtmesi gerekir.
    * Unicode araligi desteklenmiyorsa sayfanin gereksiz tüm alt kümeleri gizlemesi gerekir. Gereken alt kümeleri gelistirici belirlemelidir.
2. **Yazi tipi alt kümelerini nasil olustururuz?** 
    * Alt küme olusturmak ve yazi tiplerinizi optimize etmek için açik kaynakli [pyftsubset aracini](https://github.com/behdad/fonttools/blob/master/Lib/fontTools/subset.py#L16) kullanin.
    * Bazi yazi tipi hizmetleri, özel sorgu parametreleri araciligiyla manuel alt küme olusturmaya olanak tanir. Sayfaniz için gereken alt kümeyi kendiniz manuel olarak belirtirken bunu kullanabilirsiniz. Yazi tipi saglayicinizin dokümanlarina bakin.

### Yazi tipi seçimi ve sentez

Each font family is composed of multiple stylistic variants (regular, bold, italic) and multiple weights for each style, each of which, in turn, may contain very different glyph shapes&mdash;for example, different spacing, sizing, or a different shape altogether.

<img src="images/font-weights.png"  alt="Font weights" />

Benzer mantik *italic* çesitler için de geçerlidir. Yazi tipi tasarimcisi hangi çesitleri olusturacaklarini, biz de sayfada kullanacagimiz çesitleri kontrol ederiz. Her bir çesit ayri bir indirme oldugundan çesit sayisinin az tutulmasi iyi bir fikirdir! Örnegin, *Awesome Font* ailemiz için iki kalin çesit tanimlayabiliriz:

> When a weight is specified for which no face exists, a face with a nearby weight is used. In general, bold weights map to faces with heavier weights and light weights map to faces with lighter weights.
> 
> > [CSS3 font matching algorithm](http://www.w3.org/TR/css3-fonts/#font-matching-algorithm)

Yukaridaki örnekte, ayni Latin glif kümesini kapsayan, ancak iki farkli `agirlik`ta iki kaynaktan olusan *Awesome Font* ailesi bildirilmektedir (U+000-5FF): normal (400) ve kalin (700). Bununla birlikte, CSS kurallarimizdan biri farkli bir yazi tipi agirligi belirtir veya yazi tipi stil özelligini italige ayarlarsa ne olur?

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 700;
      src: local('Awesome Font'),
           url('/fonts/awesome-l-700.woff2') format('woff2'), 
           url('/fonts/awesome-l-700.woff') format('woff'),
           url('/fonts/awesome-l-700.ttf') format('ttf'),
           url('/fonts/awesome-l-700.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    

The above example declares the *Awesome Font* family that is composed of two resources that cover the same set of Latin glyphs (`U+000-5FF`) but offer two different "weights": normal (400) and bold (700). However, what happens if one of your CSS rules specifies a different font weight, or sets the font-style property to italic?

* Bir tam yazi tipi eslesmesi yoksa tarayici, bunu en yakin eslesmeyle degistirir.
* Herhangi bir biçimsel eslesme bulunamazsa (ör. yukaridaki örnekte herhangi bir italik çesit bildirmediysek), tarayici kendi yazi tipi çesidini sentezler.

<img src="images/font-synthesis.png"  alt="Font synthesis" />

Note: En iyi tutarlilik ve görsel sonuçlar için yazi tipi sentezine güvenmemeniz gerekir. Bunun yerine, kullanilan yazi tipi çesitlerinin sayisini en aza indirin ve konumlarini belirtin. Bu sekilde, tarayici sayfada kullanildiklarinda bunlari indirebilir. Bununla birlikte, bazi durumlarda sentezlenmis bir çesit [uygun bir seçenek olabilir](https://www.igvita.com/2014/09/16/optimizing-webfont-selection-and-synthesis/); bunu kullanirken dikkatli olun.

Ihtiyacimiz olmayabilecek tüm biçimsel çesitleri ve kullanilmayabilecek tüm glifleri içeren bir `tam` web yazi tipi, kolayca megabaytlarca büyüklükte bir indirmeyle sonuçlanabilir. Bunu ele almak için @font-face CSS kurali, özellikle yazi tipi ailesini bir kaynak koleksiyonuna bölmemize olanak taniyacak sekilde tasarlanmistir: Unicode alt kümeler, ayri stil çesitleri vb.

Bu bildirimler saglandiginda, tarayici gereken alt kümeleri ve çesitleri belirleyip metni olusturmak için gereken en küçük kümeyi indirir. Bu davranis çok kullanislidir, ancak dikkatli olmazsak kritik olusturma yolunda bir performans sorununa ve metin olusturmasinin gecikmesine neden olabilir. Bunun olmasini kesinlikle istemeyiz!

## Yükleme ve olusturmayi optimize etme

### TL;DR {: .hide-from-toc }

* Yazi tipi istekleri, olusturma agaci olusturuluncaya kadar geciktirilir. Bu, metin olusturmasinin gecikmesine neden olabilir
* Yazi Tipi Yükleme API''si, varsayilan yazi tipinin geç yüklenmesi yöntemini geçersiz kilan özel yazi tipi yükleme ve olusturma stratejileri uygulamamiza olanak tanir

Yazi tiplerinin geç yüklenmesi, metin olusturmayi geciktirebilecek önemli bir gizli etki tasir: Tarayicinin, metni olusturmak için ihtiyaç duyacagi yazi tipi kaynaklarini ögrenmeden önce DOM ve CSSOM agaçlarina bagimli olan [olusturma agacini olusturmasi gerekir](/web/fundamentals/performance/critical-rendering-path/render-tree-construction). Sonuç olarak, yazi tipi istekleri diger kritik kaynaklarin sonrasina ertelenir ve kaynak getirilinceye kadar tarayicinin metni olusturmasi engellenebilir.

Given these declarations, the browser figures out the required subsets and variants and downloads the minimal set required to render the text, which is very convenient. However, if you're not careful, it can also create a performance bottleneck in the critical rendering path and delay text rendering.

### Web Yazi Tipleri ve Kritik Olusturma Yolu

Olusturma agaci olusturulduktan kisa bir süre sonra yapilabilen sayfa içeriginin ilk boyamasi ile yazi tipi kaynaginin istenmesi arasindaki `yaris`, tarayicinin sayfa yer paylasimini olusturabildigi ancak metni atladigi `bos metin sorunu`nu ortaya çikarir. Gerçek davranis, çesitli tarayicilar arasinda farklilik gösterir:

<img src="images/font-crp.png"  alt="Font critical rendering path" />

1. Tarayici HTML dokümanini ister
2. Tarayici, HTML yanitini ayristirmaya ve DOM'yi olusturmaya baslar
3. Tarayici; CSS, JS ve diger kaynaklari kesfeder ve istekler gönderir
4. Tarayici, tüm CSS içerigi alindiktan sonra CSSOM'yi olusturur ve olusturma agaci yapimi için bunu DOM agaciyla birlestirir 
    * Yazi tipi istekleri, olusturma agaci sayfada belirtilen metni olusturmak için gereken yazi tipi çesitlerini belirttikten sonra gönderilir
5. Tarayici yer paylasimini gerçeklestirir ve içerigi ekranda boyar 
    * Yazi tipi henüz mevcut degilse, tarayici hiç metin pikseli olusturamaz
    * Yazi tipi kullanilabilir duruma geldiginde tarayici metin piksellerini boyar

[Yazi Tipi Yükleme API'si](http://dev.w3.org/csswg/css-font-loading/), CSS yazi tipi yüzlerinin tanimlanmasi ve kullanilmasi, indirme isleme ilerleme durumunun izlenmesi ve varsayilan geç yükleme davranisinin geçersiz kilinmasi için bir komut dosyasi arayüzü saglar. Örnegin, belirli bir yazi tipi çesidinin gerekeceginden eminsek bunu tanimlayabilir ve tarayiciya, yazi tipi kaynagini hemen getirmeye baslamasini söyleyebiliriz:

Bunun ötesinde, yazi tipi durumunu ([check()](http://dev.w3.org/csswg/css-font-loading/#font-face-set-check)) yöntemi araciligiyla kontrol edebildigimiz ve indirme islemi ilerleme durumunu izleyebildigimizden, sayfalarimizdaki metnin olusturulmasina yönelik özel bir strateji de tanimlayabiliriz:

### Yazi Tipi Yükleme API'si ile yazi tipi olusturmayi optimize etme

En iyisi, sayfadaki farkli içerik için yukaridaki stratejileri karistirip eslestirebiliriz; diger bir deyisle, bazi bölümlerde yazi tipi kullanilabilir duruma gelene kadar metin olusturmayi bekletip bir yedek kullanabilir ve yazi tipi indirme islemi bittikten sonra yeniden olusturabilir, farkli zaman asimlari belirleyebilir ve baska islemler yapabiliriz.

Note: Yazi Tipi Yükleme API'si [bazi tarayicilarda hâlâ gelistirme asamasindadir](http://caniuse.com/#feat=font-loading). Benzer bir islevsellik saglamak için [FontLoader çoklu dolgusunu](https://github.com/bramstein/fontloader) veya [webfontloader kitapligini](https://github.com/typekit/webfontloader) kullanmayi düsünebilirsiniz; ancak bunlar, bir JavaScript bagimliligi ek yükü de getirir.

`Bos metin sorunu` nu ortadan kaldirmak üzere Yazi Tipi Yükleme API'sini kullanmaya alternatif basit bir strateji de yazi tipi içerigini bir CSS stil sayfasinda satir içinde kullanmaktir:

```html
var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
  style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
});

font.load(); // don't wait for render tree, initiate immediate fetch!

font.ready().then(function() {
  // apply the font (which may rerender text and cause a page reflow)
  // once the font has finished downloading
  document.fonts.add(font);
  document.body.style.fontFamily = "Awesome Font, serif";

  // OR... by default content is hidden, and rendered once font is available
  var content = document.getElementById("content");
  content.style.visibility = "visible";

  // OR... apply own render strategy here... 
});
```

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

Satir içinde kullanma stratejisi çok esnek degildir ve farkli içerik için özel zaman asimlari veya olusturma stratejileri tanimlamamiza olanak tanimaz, ancak tüm tarayicilarda çalisan basit ve saglam bir çözümdür. En iyi sonuçlar için satir içinde kullanilan yazi tiplerini bagimsiz bir stil sayfasina ayirin ve bunlari uzun bir max-age degeriyle sunun. Bu sekilde, CSS'nizi güncellediginizde ziyaretçileri bu yazi tiplerini yeniden indirmeye zorlamamis olursunuz.

Note: Satir içi kullanirken seçici olun! @font-face kuralinin geç yükleme davranisini kullanma nedeninin, gereksiz yazi tipi çesitlerini ve alt kümelerini indirmekten kaçinmak oldugunu unutmayin. Ayrica, CSS dosyanizin boyutunu agresif satir içi kullanimla büyütmek [kritik olusturma yolunuzu](/web/fundamentals/performance/critical-rendering-path/) olumsuz bir sekilde etkiler. Tarayicinin CSSOM'yi, olusturma agacini ve ekranda sayfa içerigini olusturmadan önce tüm CSS'yi indirmesi gerekir.

### Satir içi kullanimla yazi tipi olusturmayi optimize etme

Yazi tipi kaynaklari genellikle sik güncellenmeyen statik kaynaklardir. Sonuç olarak, uzun bir max-age süre sonu için çok uygundurlar. Tüm yazi tipi kaynaklari için hem bir [kosullu ETag üstbilgisi](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags) hem de bir [en iyi Cache-Control politikasi](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) belirttiginizden emin olun.

#### Browser behaviors

Yazi tiplerinin localStorage içinde veya baska mekanizmalar araciligiyla depolanmasina gerek yoktur. Bunlarin her birinin kendi performans avantajlari vardir. Tarayicinin HTTP önbellegi, Yazi Tipi Yükleme API'si veya webfontloader kitapligiyla birlikte, yazi tipi kaynaklarini tarayiciya teslim etmek için en iyi ve en saglam mekanizmayi saglar.

<table>
  <thead>
    <tr>
      <th data-th="Browser">Browser</th>
      <th data-th="Timeout">Timeout</th>
      <th data-th="Fallback">Fallback</th>
      <th data-th="Swap">Swap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Browser">
        <strong>Chrome 35+</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Opera</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Firefox</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Internet Explorer</strong>
      </td>
      <td data-th="Timeout">
        0 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Safari</strong>
      </td>
      <td data-th="Timeout">
        No timeout
      </td>
      <td data-th="Fallback">
        N/A
      </td>
      <td data-th="Swap">
        N/A
      </td>
    </tr>
  </tbody>
</table>

* Safari, yazi tipi indirilinceye kadar metin olusturmayi bekletir.
* Chrome ve Firefox, yazi tipi olusturmayi 3 saniyeye kadar bekletir; sonrasinda bir yedek yazi tipi kullanirlar ve yazi tipi indirildikten sonra, metni indirilen yazi tipiyle bir kez daha yeniden olustururlar.
* IE, istenen yazi tipi henüz mevcut degilse metni hemen yedek yazi tipiyle olusturur ve yazi tipi indirme islemi tamamlandiktan sonra metni yeniden olusturur.

Popüler düsüncenin aksine, web yazi tiplerinin kullanilmasinin sayfa olusturmayi geciktirmesi gerekmez veya diger performans ölçümleri üzerinde olumsuz etkisi yoktur. Yazi tiplerinin iyi bir sekilde optimize edilerek kullanilmasi, çok daha iyi bir genel kullanici deneyimini saglayabilir: Harika marka bilinci olusturma, iyilestirilmis okunabilirlik, kullanilabilirlik ve aranabilirlik özelliklerinin tümü, tüm ekran biçimlerine ve çözünürlüklerine iyi bir sekilde uyum saglayan ölçeklenebilir çok çözünürlüklü bir çözümle saglanir. Web yazi tiplerini kullanmaktan korkmayin!

#### The font display timeline

Bununla birlikte, denenmemis bir uygulama büyük indirmelere ve gereksiz gecikmelere yol açabilir. Bu noktada, optimizasyon araç takimimizin tozunu almamiz ve yazi tipi varliklarini ve sayfalarimizda nasil getirilip kullanilacaklarini kendimiz optimize ederek tarayiciya yardimci olmamiz gerekir.

1. **Yazi tipi kullaniminizi denetleyin ve izleyin:** Sayfalarinizda çok sayida yazi tipi kullanmayin ve her bir yazi tipi için kullanilan çesitlerin sayisini en aza indirin. Bu, kullanicilariniz için daha tutarli ve daha hizli bir deneyim saglamaniza yardimci olur.
2. **Yazi tipi kaynaklarinizin alt kümesini olusturun:** Yalnizca belirli bir sayfa için gereken glifleri saglamak üzere birçok yazi tipinin alt kümesi olusturulabilir veya yazi tipleri birden çok unicode araligina bölünebilir. Bu, dosya boyutunu küçültür ve kaynagin indirme hizini iyilestirir. Ancak, alt kümeleri tanimlarken yazi tipi yeniden kullanimi optimizasyonuna dikkat edin. Her sayfada farkli, ancak örtüsen bir karakter kümesini indirmek istemezsiniz. Iyi bir uygulama yazili metne dayali alt küme olusturmaktir (ör. Latin, Kiril vb.).
3. **Her bir tarayiciya optimize edilmis yazi tipi biçimleri saglayin:** Her bir yazi tipi WOFF2, WOFF, EOT ve TTF biçimlerinde saglanmalidir. EOT ve TTF biçimleri varsayilan olarak sikistirilmadigindan, bu biçimlere GZIP sikistirmasi uyguladiginizdan emin olun.

Understanding these periods means you can use `font-display` to decide how your font should render depending on whether or when it was downloaded.

#### Using font-display

To work with the `font-display` property, add it your `@font-face` rules:

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  font-display: auto; /* or block, swap, fallback, optional */
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

`font-display` currently supports the following range of values: `auto | block | swap | fallback | optional`.

* **`auto`** uses whatever font display strategy the user-agent uses. Most browsers currently have a default strategy similar to `block`.

* **`block`** gives the font face a short block period (3s is recommended in most cases) and an infinite swap period. In other words, the browser draws "invisible" text at first if the font is not loaded, but swaps the font face in as soon as it loads. To do this the browser creates an anonymous font face with metrics similar to the selected font but with all glyphs containing no "ink." This value should only be used if rendering text in a particular typeface is required for the page to be usable.

* **`swap`** gives the font face a zero second block period and an infinite swap period. This means the browser draws text immediately with a fallback if the font face isn’t loaded, but swaps the font face in as soon as it loads. Similar to `block`, this value should only be used when rendering text in a particular font is important for the page, but rendering in any font will still get a correct message across. Logo text is a good candidate for **swap** since displaying a company’s name using a reasonable fallback will get the message across but you’d eventually use the official typeface.

* **`fallback`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a short swap period (three seconds is recommended in most cases). In other words, the font face is rendered with a fallback at first if it’s not loaded, but the font is swapped as soon as it loads. However, if too much time passes, the fallback will be used for the rest of the page’s lifetime. `fallback` is a good candidate for things like body text where you’d like the user to start reading as soon as possible and don’t want to disturb their experience by shifting text around as a new font loads in.

* **`optional`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a zero second swap period. Similar to `fallback`, this is a good choice for when the downloading font is more of a "nice to have" but not critical to the experience. The `optional` value leaves it up to the browser to decide whether to initiate the font download, which it may choose not to do or it may do it as a low priority depending on what it thinks would be best for the user. This can be beneficial in situations where the user is on a weak connection and pulling down a font may not be the best use of resources.

`font-display` is [gaining adoption](https://caniuse.com/#feat=css-font-rendering-controls) in many modern browsers. You can look forward to consistency in browser behavior as it becomes widely implemented.

### HTTP Önbellege Alma özelligiyle yazi tipi yeniden kullanimini optimize etme

Used together, `<link rel="preload">` and the CSS `font-display` give developers a great deal of control over font loading and rendering, without adding in much overhead. But if you need additional customizations, and are willing to incur with the overhead introduced by running JavaScript, there is another option.

The [Font Loading API](https://www.w3.org/TR/css-font-loading/) provides a scripting interface to define and manipulate CSS font faces, track their download progress, and override their default lazyload behavior. For example, if you're sure that a particular font variant is required, you can define it and tell the browser to initiate an immediate fetch of the font resource:

    var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
      style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
    });
    
    // don't wait for the render tree, initiate an immediate fetch!
    font.load().then(function() {
      // apply the font (which may re-render text and cause a page reflow)
      // after the font has finished downloading
      document.fonts.add(font);
      document.body.style.fontFamily = "Awesome Font, serif";
    
      // OR... by default the content is hidden, 
      // and it's rendered after the font is available
      var content = document.getElementById("content");
      content.style.visibility = "visible";
    
      // OR... apply your own render strategy here... 
    });
    

Further, because you can check the font status (via the [check()](https://www.w3.org/TR/css-font-loading/#font-face-set-check)) method and track its download progress, you can also define a custom strategy for rendering text on your pages:

* Eslesen medya sorgularina sahip CSS stil sayfalari, CSSOM'yi olusturmak için gerekli olduklarindan tarayici tarafindan yüksek öncelikle otomatik olarak indirilir.
* Yazi tipi verilerinin CSS stil sayfasinda satir içinde kullanilmasi tarayiciyi, yazi tipini yüksek öncelikle ve olusturma agacini beklemeden indirmeye zorlar. Bu yaklasim, varsayilan geç yükleme davranisini el ile geçersiz kilar.
* You can use the fallback font to unblock rendering and inject a new style that uses the desired font after the font is available.

Best of all, you can also mix and match the above strategies for different content on the page. For example, you can delay text rendering on some sections until the font is available, use a fallback font, and then re-render after the font download has finished, specify different timeouts, and so on.

Note: The Font Loading API is still [under development in some browsers](http://caniuse.com/#feat=font-loading). Consider using the [FontLoader polyfill](https://github.com/bramstein/fontloader) or the [webfontloader library](https://github.com/typekit/webfontloader) to deliver similar functionality, albeit with even more overhead from an additional JavaScript dependency.

### Proper caching is a must

Font resources are, typically, static resources that don't see frequent updates. As a result, they are ideally suited for a long max-age expiry - ensure that you specify both a [conditional ETag header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), and an [optimal Cache-Control policy](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) for all font resources.

If your web application uses a [service worker](/web/fundamentals/primers/service-workers/), serving font resources with a [cache-first strategy](/web/fundamentals/instant-and-offline/offline-cookbook/#cache-then-network) is appropriate for most use cases.

You should not store fonts using [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) or [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API); each of those has its own set of performance issues. The browser's HTTP cache provides the best and most robust mechanism to deliver font resources to the browser.

## Optimizasyon kontrol listesi

Contrary to popular belief, the use of webfonts doesn't need to delay page rendering or have a negative impact on other performance metrics. The well-optimized use of fonts can deliver a much better overall user experience: great branding, improved readability, usability, and searchability, all while delivering a scalable multi-resolution solution that adapts well to all screen formats and resolutions. Don't be afraid to use webfonts.

That said, a naive implementation may incur large downloads and unnecessary delays. You need to help the browser by optimizing the font assets themselves and how they are fetched and used on your pages.

* **Audit and monitor your font use:** don't use too many fonts on your pages, and, for each font, minimize the number of used variants. This helps produce a more consistent and a faster experience for your users.
* **Subset your font resources:** many fonts can be subset, or split into multiple unicode-ranges to deliver just the glyphs that a particular page requires. This reduces the file size and improves the download speed of the resource. However, when defining the subsets, be careful to optimize for font re-use. For example, don't download a different but overlapping set of characters on each page. A good practice is to subset based on script: for example, Latin, Cyrillic, and so on.
* **Deliver optimized font formats to each browser:** provide each font in WOFF2, WOFF, EOT, and TTF formats. Make sure to apply GZIP compression to the EOT and TTF formats, because they are not compressed by default.
* **Give precedence to `local()` in your `src` list:** listing `local('Font Name')` first in your `src` list ensures that HTTP requests aren't made for fonts that are already installed.
* **Customize font loading and rendering using `<link rel="preload">`, `font-display`, or the Font Loading API:** default lazyloading behavior may result in delayed text rendering. These web platform features allow you to override this behavior for particular fonts, and to specify custom rendering and timeout strategies for different content on the page.
* **Specify revalidation and optimal caching policies:** fonts are static resources that are infrequently updated. Make sure that your servers provide a long-lived max-age timestamp and a revalidation token to allow for efficient font reuse between different pages. If using a service worker, a cache-first strategy is appropriate.

## Automated testing for web font optimization with Lighthouse {: #lighthouse }

[Lighthouse](/web/tools/lighthouse) can help automate the process of making sure that you're following web font optimization best practices. Lighthouse is an auditing tool built by the Chrome DevTools team. You can run it as a Node module, from the command line, or from the Audits panel of Chrome DevTools. You tell Lighthouse what URL to audit, and then it runs a bunch of tests on the page, and gives you a report of what the page is doing well, and how it can improve.

The following audits can help you make sure that your pages are continuing to follow web font optimization best practices over time:

* [Enable text compression](/web/tools/lighthouse/audits/text-compression)
* [Preload key requests](/web/tools/lighthouse/audits/preload)
* [Uses inefficient cache policy on static assets](/web/tools/lighthouse/audits/cache-policy)
* [All text remains visible during webfont loads](/web/updates/2016/02/font-display)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}