project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: CSSOM ve DOM agaçlari, bir olusturma agacinda birlestirilir. Bu agaç daha sonra, görünür her bir ögenin yer paylasimini hesaplamak için kullanilir ve ekranda pikselleri olusturan boyama isleminde giris görevi görür. Bu adimlarin her birinin optimize edilmesi, en iyi olusturma performansinin gerçeklestirilmesi açisindan kritik öneme sahiptir.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Olusturma agaci yapimi, Yer paylasimi ve Boyama {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

CSSOM ve DOM agaçlari, bir olusturma agacinda birlestirilir. Bu agaç daha sonra, görünür her bir ögenin yer paylasimini hesaplamak için kullanilir ve ekranda pikselleri olusturan boyama isleminde giris görevi görür. Bu adimlarin her birinin optimize edilmesi, en iyi olusturma performansinin gerçeklestirilmesi açisindan kritik öneme sahiptir.

Nesne modelinin olusturulmasina iliskin önceki bölümde, HTML ve CSS girislerine dayanarak DOM ve CSSOM agaçlari olusturmustuk. Bununla birlikte, bunlarin her ikisi de dokümanin farkli unsurlarini yakalayan bagimsiz nesnelerdir: Biri, içerigi açiklarken digeri, dokümana uygulanmasi gereken stil kurallarini açiklar. Ikisini nasil birlestiririz ve ekranda pikselleri olusturmak için tarayiciya aliriz?

### TL;DR {: .hide-from-toc }

* DOM ve CSSOM agaçlari, olusturma agacini olusturmak üzere birlestirilir.
* Olusturma agaci yalnizca sayfanin olusturulmasi için gereken dügümleri içerir.
* Yer paylasimi, her bir nesnenin tam konumunu ve boyutunu hesaplar.
* Boyama, son olusturma agacini alan ve ekran için pikselleri olusturan son adimdir.

Ilk adim, tarayicinin DOM ve CSSOM'yi, sayfadaki tüm görünür DOM içerigini ve her bir dügüme iliskin CSSOM stil bilgilerinin tümünü yakalayan bir 'olusturma agaci'nda birlestirmesi içindir.

<img src="images/render-tree-construction.png" alt="DOM ve CSSOM, olusturma agacini olusturmak üzere birlestirilir" />

Olusturma agacini olusturmak için tarayici kabaca asagidakileri yapar:

1. Starting at the root of the DOM tree, traverse each visible node.
    
    * Bazi dügümler hiç görünür degildir (ör. script etiketleri, meta etiketleri vb.) ve olusturulan çiktida bunlar yansitilmayacagi için atlanirlar.
    * Bazi dügümler CSS araciligiyla gizlenir ve olusturma agacindan da atlanir. Örnegin, yukaridaki örnekte bulunan span dügümü, `display: none` özelligini ayarlayan açik bir kuralimiz oldugundan olusturma agacinda yoktur.

2. For each visible node, find the appropriate matching CSSOM rules and apply them.

3. Görünür dügümleri, içerik ve hesaplanan stilleriyle yayinlayin.

Note: Kisa bir not olarak, `visibility: hidden` degerinin `display: none` degerinden farkli oldugunu unutmayin. Ilki ögeyi görünmez yapar, ancak öge yer paylasiminda alan kaplamaya devam eder (bos kutu olarak olusturulur), buna karsilik ikincisi (display: none) ögeyi, öge görünmezmis ve yer paylasiminin bir parçasi degilmis gibi olusturma agacindan tamamiyla çikarir.

Son çikti, hem içerigi hem de ekranda görünen tüm içerigin stil bilgilerini içeren bir olusturmadir. Yaklasiyoruz! **Olusturma agaci tamamlandiginda, 'yer paylasimi' asamasina geçebiliriz.**

Bu noktaya kadar, hangi dügümlerin görünür olmasi gerektigini ve bunlarin hesaplanan stillerini hesapladik, ancak cihazin [görüntü alani](/web/fundamentals/design-and-ux/responsive/#set-the-viewport) içindeki tam konumlarini ve boyutlarini hesaplamadik. Bu, 'yer paylasimi' asamasidir; bazen 'yeniden düzenleme' olarak da bilinir.

Her bir nesnenin kesin boyutunu ve konumunu belirlemek için tarayici, olusturma agacinin kökünden baslar ve sayfadaki her bir nesnenin geometrisini hesaplamak için üzerinden geçer. Basit bir uygulama örnegini degerlendirelim:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/nested.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Yukaridaki sayfanin gövdesi iç içe yerlestirilmis iki div ögesi içerir: Ilk (üst) div ögesi, dügümün görüntü boyutunu görüntü alani genisliginin %50'sine ve üst ögenin içerdigi ikinci div ögesi, genisligini üst ögenin %50'sine (görüntü alani genisliginin %25'ine) ayarlar!

The body of the above page contains two nested div's: the first (parent) div sets the display size of the node to 50% of the viewport width, and the second div\---contained by the parent\---sets its width to be 50% of its parent; that is, 25% of the viewport width.

<img src="images/layout-viewport.png" alt="Calculating layout information" />

Son olarak, artik hangi dügümlerin görünür olduklarini, bunlarin hesaplanan stillerini ve geometrilerini bildigimize göre, sonunda bu bilgileri son asamamiza geçirebiliriz. Bu asamada, olusturma agacindaki her bir dügümü ekranda gerçek piksellere dönüstürecegiz. Bu adima genellikle `boyama` veya `tarama` adi verilir.

Tüm bunlari takip edebildiniz mi? Bu adimlarin her biri, tarayici tarafindan azimsanmayacak miktarda is yapilmasini gerektirir. Bu, tüm bunlarin genellikle biraz zaman alabilecegi anlamina da gelir. Neyse ki, Chrome DevTools yukarida asamalarin üçünün de bazi analizlerini almamiza yardimci olabilir. Orijinal `herkese merhaba` örnegimizin yer paylasimi asamasini inceleyelim:

This can take some time because the browser has to do quite a bit of work. However, Chrome DevTools can provide some insight into all three of the stages described above. Let's examine the layout stage for our original "hello world" example:

<img src="images/layout-timeline.png" alt="Measuring layout in DevTools" />

* Olusturma agaci yapimi ile konum ve boyut hesaplamasi, Zaman Çizelgesi'nde 'Yer Paylasimi' olayi ile yakalanir.
* Yer paylasimi tamamlandiktan sonra, tarayici birer 'Boyama Ayarlari' ve 'Boyama' olayi yayinlar. Bunlar, olusturma agacini ekranda gerçek piksellere dönüstürür.

Sonunda, sayfamiz görüntü alaninda görünür hale geldi. Yasasin!

The page is finally visible in the viewport:

<img src="images/device-dom-small.png" alt="Rendered Hello World page" />

Demo sayfamiz çok basit görünebilir, ancak oldukça çalisma gerektiriyor! DOM veya CSSOM degistirilseydi ne olacagini tahmin edebilir misiniz? Ekranda hangi piksellerin yeniden olusturulmasinin gerektigini belirlemek için ayni süreci bastan tekrar etmemiz gerekirdi.

1. HTML biçimlendirmesini isleme ve DOM agacini olusturma.
2. CSS biçimlendirmesini isleme ve CSSOM agacini olusturma.
3. DOM ve CSSOM'yi bir olusturma agacinda birlestirme.
4. Her bir dügüm geometrisini hesaplamak için olusturma agacinda yer paylasimini çalistirma.
5. Ekran için bagimsiz dügümleri boyama.

**Kritik olusturma yolunu optimize etme, yukaridaki sirada 1. ile 5. adim arasindaki adimlarda harcanan toplam süreyi en aza indirme sürecidir.** Bunu yapmamiz içerigi ekran için mümkün olan en kisa zamanda olusturabilmemizi saglar ve ayrica, ilk olusturma sonrasinda ekran güncellemeleri arasindaki süreyi azaltir (etkilesimli içerik için daha yüksek yenileme hizi gerçeklestirmemizi saglar).

***Optimizing the critical rendering path* is the process of minimizing the total amount of time spent performing steps 1 through 5 in the above sequence.** Doing so renders content to the screen as quickly as possible and also reduces the amount of time between screen updates after the initial render; that is, achieve higher refresh rates for interactive content.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}