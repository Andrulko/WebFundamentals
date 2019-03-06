project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Kritik olusturma yolu performans tikanikliklarinin tanimlanmasi ve çözülmesi, yaygin görülen güçlüklerle ilgili iyi bilgi sahibi olunmasini gerektirir. Simdi baslangiç turumuzu gerçeklestirelim ve sayfalarinizi optimize etmenize yardimci olacak, yaygin görülen performans kaliplarini çikartalim.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Kritik Olusturma Yolu Performansini Analiz Etme {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Kritik olusturma yolu performans tikanikliklarinin tanimlanmasi ve çözülmesi, yaygin görülen güçlüklerle ilgili iyi bilgi sahibi olunmasini gerektirir. Simdi baslangiç turumuzu gerçeklestirelim ve sayfalarinizi optimize etmenize yardimci olacak, yaygin görülen performans kaliplarini çikartalim.

Kritik olusturma yolunu optimize etmenin amaci, tarayicinin sayfayi mümkün oldugunca hizli bir sekilde boyamasina olanak tanimaktir: Sayfalarin hizli olmasi, etkilesim ve görüntülenen sayfa sayisinin daha fazla olmasi ve [dönüsümün iyilestirilmesi](http://www.google.com/think/multiscreen/success.html) anlamina gelir. Sonuç olarak, hangi kaynaklarin ne sirada yüklendigini optimize ederek ziyaretçinin bos bir ekrana bakarak geçirdigi süreyi en aza indirmek isteriz.

Bu süreci göstermeye yardimci olmasi açisindan, olabilecek en basit örnekle baslayalim ve ek kaynaklari, stilleri ve uygulama mantigini ekleyerek sayfamizi kademeli bir sekilde olusturalim. Süreç içinde, islerin nerelerde yanlis gidebilecegini ve bu örneklerin her birini nasil optimize edebilecegimizi de görecegiz.

Son olarak, baslamadan önce son bir noktayi belirtmek istiyoruz... Simdiye kadar, özellikle kaynak (CSS; JS veya HTML dosyasi) islenmek için hazir olduktan sonra tarayicida neler olduguna odaklanmis ve kaynagin önbellekten veya agdan getirilmesi için gereken süreyi göz ardi etmistik. Bir sonraki derse uygulamamizin ag iletisimi unsurlarini nasil optimize edecegimizi ayrintili olarak görecegiz, ancak bu arada (yaptiklarimizi daha gerçekçi kilmak için) asagidaki kosullarin saglandigini varsayacagiz:

* Sunucuya ag gidis gelisi (yayilma gecikmesi) 100 ms olacaktir
* Sunucu yanit süresi, HTML dokümani için 100 ms ve diger tüm dosyalar için 10 ms olacaktir

## Herkese Merhaba deneyimi

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Mümkün olan en basit örnek olmasi için temel HTML biçimlendirmesi ve tek bir resimle baslayacagiz, CSS veya JavaScript olmayacak. Simdi, Chrome DevTools'ta Ag zaman çizelgemizi açip kaynak selalemizi inceleyim:

We'll start with basic HTML markup and a single image; no CSS or JavaScript. Let's open up our Network timeline in Chrome DevTools and inspect the resulting resource waterfall:

<img src="images/waterfall-dom.png" alt="CRP" />

HTML içerigi kullanilabilir hale geldikten sonra, tarayicinin baytlari ayristirmasi, belirteçlere dönüstürmesi ve DOM agacini olusturmasi gerekir. DevTools'un, DOMContentLoaded olayi için geçen süreyi kullanisli bir sekilde alt kisimda bildirdigine (216 ms), bunun da mavi dikey çizgiye karsilik geldigine dikkat edin. HTML indirme isleminin sonu ile mavi dikey çizgi (DOMContentLoaded) arasindaki bosluk, tarayicinin DOM agacini olusturdugu süredir. Bu örnekte, bu süre yalnizca birkaç milisaniyedir.

Son olarak, ilginç bir seye dikkat edin: `Harika fotografimiz` domContentLoaded olayini engellemedi! Görünüse göre, sayfadaki her ögeyi beklemeden olusturma agacini yapabilir, hatta sayfayi boyayabiliriz: **Tüm kaynaklar ilk hizli boyanin saglanmasi açisindan önemli degildir**. Aslinda, ileride görecegimiz gibi, kritik olusturma yolundan söz ederken genellikle HTML biçimlendirmesi, CSS ve JavaScript'ten konusuyoruz. Resimler, sayfanin ilk olusturmasini engellemez. Elbette, resimlerin de mümkün oldugunca hizli boyanmalarini saglamayi da denemeliyiz!<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

Bununla birlikte, `yükleme` olayi (`onload` olarak da bilinir) resimde engellenmistir: DevTools, onload olayini 335 ms olarak bildirir. Onload olayinin, sayfanin gerektirdigi **tüm kaynaklarin** indirilip islendigi noktayi isaret ettigini unutmayin. Bu, tarayicidaki yükleme deger degistiricisinin dönmeyi durdurabilecegi noktadir ve bu nokta, selalede kirmizi dikey çizgiyle isaretlenmistir.

`Herkese Merhaba deneyimi` sayfamiz, yüzeyde basit görünebilir, ancak bunun olmasini saglamak için sahne arkasinda birçok sey olup bitmektedir! Bununla birlikte, uygulamada yalnizca HTML'den daha fazlasina da ihtiyacimiz olacaktir: Muhtemelen bir CSS stil sayfamiz ve sayfamiza biraz etkilesim eklemek için bir veya daha fazla komut dosyamiz olur. Simdi bunlarin her ikisini de karisimimiza ekleyip neler oldugunu görelim:

That said, the `load` event (also known as `onload`), is blocked on the image: DevTools reports the `onload` event at 335ms. Recall that the `onload` event marks the point at which **all resources** that the page requires have been downloaded and processed; at this point, the loading spinner can stop spinning in the browser (the red vertical line in the waterfall).

## Karisima JavaScript ve CSS ekleme

Our "Hello World experience" page seems simple but a lot goes on under the hood. In practice we'll need more than just the HTML: chances are, we'll have a CSS stylesheet and one or more scripts to add some interactivity to our page. Let's add both to the mix and see what happens:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_timing.html){: target="_blank" .external }

*Before adding JavaScript and CSS:*

<img src="images/waterfall-dom.png" alt="DOM CRP" />

*With JavaScript and CSS:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

Adding external CSS and JavaScript files adds two extra requests to our waterfall, all of which the browser dispatches at about the same time. However, **note that there is now a much smaller timing difference between the `domContentLoaded` and `onload` events.**

What happened?

* Sade HTML örnegimizden farkli olarak, artik CSSOM'yi yapmak için CSS dosyasini getirip ayristirmamiz da gerekiyor ve olusturma agacini olusturmak için hem DOM'ye hem de CSSOM'ye ihtiyacimiz oldugunu biliyoruz.
* Sayfamizda ayristirici engelleyen bir JavaScript dosyamiz da oldugundan, domContentLoaded olayi CSS dosyasi indirilip ayristirilincaya kadar engellenir: JavaScript, CSSOM'yi sorgulayabilir, dolayisiyla JavaScript'i yürütebilmemiz için bunu engelleyip CSS'yi beklememiz gerekir.

**What if we replace our external script with an inline script?** Even if the script is inlined directly into the page, the browser can't execute it until the CSSOM is constructed. In short, inlined JavaScript is also parser blocking.

That said, despite blocking on CSS, does inlining the script make the page render faster? Let's try it and see what happens.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Ayristirici engelleyen (harici) JavaScript:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, JS" />

We are making one less request, but both our `onload` and `domContentLoaded` times are effectively the same. Why? Well, we know that it doesn't matter if the JavaScript is inlined or external, because as soon as the browser hits the script tag it blocks and waits until the CSSOM is constructed. Further, in our first example, the browser downloads both CSS and JavaScript in parallel and they finish downloading at about the same time. In this instance, inlining the JavaScript code doesn't help us much. But there are several strategies that can make our page render faster.

First, recall that all inline scripts are parser blocking, but for external scripts we can add the "async" keyword to unblock the parser. Let's undo our inlining and give that a try:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Alternatif olarak, farkli bir yaklasim deneyip hem CSS'yi hem de JavaScript'i satir içine yerlestirebilirdik:

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

Mümkün olan en basit sayfa yalnizca HTML biçimlendirmesinden olusur: CSS, JavaScript veya baska kaynak türlerini içermez. Bu sayfayi olusturmak için tarayicinin istegi baslatmasi, HTML dokümaninin ulasmasini beklemesi, bunu ayristirmasi, DOM'yi olusturmasi ve son olarak, sayfayi ekranda olusturmasi gerekir:

Alternatively, we could have inlined both the CSS and JavaScript:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**T<sub>0</sub> ile T<sub>1</sub> arasindaki süre, ag ile sunucu isleme sürelerini yakalar.** En iyi durumda (HTML dosyasi küçükse), tüm ihtiyacimiz olan belgenin tamamini getirmek için yalnizca bir ag gidis gelisidir. TCP aktarim protokollerinin çalisma seklinden dolayi, büyük dosyalar daha fazla gidis gelis gerektirebilir. Bu, ileride göreceginiz baska bir derste geri dönecegimiz bir konudur. **Sonuç olarak, yukaridaki sayfanin en iyi durumda bir gidis gelis (minimum) kritik olusturma yolunun oldugunu söyleyebiliriz.**

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" />

Notice that the `domContentLoaded` time is effectively the same as in the previous example; instead of marking our JavaScript as async, we've inlined both the CSS and JS into the page itself. This makes our HTML page much larger, but the upside is that the browser doesn't have to wait to fetch any external resources; everything is right there in the page.

Bir kez daha, HTML dokümanini getirmek için bir ag gidis gelisi gerçeklestiririz ve daha sonra, alinan biçimlendirme bize CSS dosyasina da ihtiyacimiz oldugunu bildirir: Bu, tarayicinin sayfayi ekranda olusturmadan önce sunucuya geri gitmesi ve CSS'yi almasi gerektigi anlamina gelir. **Sonuç olarak, bu sayfa, sayfanin görüntülenmesinden önce en az iki gidis gelis gerçeklestirir.** Bir kez daha, CSS dosyasinin birden çok gidis gelis gerektirebilecegini belirtelim; burada vurgu `en az` kelimesindedir.

Simdi, kritik olusturma yolunu açiklarken kullanacagimiz terimleri tanimlayalim:

## Performans Kaliplari

Simdi, bunu yukaridaki HTML + CSS örneginin kritik yol özellikleriyle karsilastiralim:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

<img src="images/analysis-dom.png" alt="Hello world CRP" />

Tamam, simdi karisima fazladan bir JavaScript dosyasi ekleyelim!

Sayfada harici bir JavaScript varligi olan app.js dosyasini ekledik ve simdiye kadar ögrendigimiz gibi bu ayristirici engelleyen (ör. kritik) bir kaynaktir. Daha da kötüsü, JavaScript dosyasinin yürütülebilmesi için islemi engelleyip CSSOM'yi beklememiz gerekir. JavaScript'in CSSOM'yi sorgulayabildigini, dolayisiyla tarayicinin `style.css` indirilip CSSOM olusturuluncaya kadar bekleyecegini hatirlayin.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

Artik toplam 11 KB kritik bayt olan üç kritik kaynagimiz vardir, ancak kritik yol uzunlugumuz hâlâ iki gidis gelistir. Bunun nedeni, CSS ve JavaScript'i paralel bir sekilde aktarabiliyor olmamizdir! **Kritik olusturma yolunun özelliklerinin belirlenmesi, nelerin kritik kaynaklar oldugunu tanimlayabilmek ve tarayicinin, getirme islemlerini nasil planlayacagini anlamak anlamina gelir.** Örnegimizle devam edelim...

Site gelistiricilerimizle sohbet ettikten sonra, JavaScript'i engellenmesi gerekmeyen sayfamiza ekledigimizi fark ettik: Burada, sayfamizin olusturulmasini engellemesi gerekmeyen bazi analizlerimiz ve baska kodlarimiz bulunuyor. Bu bilgiyle, ayristiricinin engellemesini kaldirmak için script etiketine `async` özelligini ekleyebiliriz:

* **Kritik Kaynak:** Sayfanin ilk olusturmasini engelleyebilecek kaynak.
* **Kritik Yol Uzunlugu:** Tüm kritik kaynaklari getirmek için gereken gidis gelis sayisi veya toplam süre.
* **Kritik Baytlar:** Sayfayi ilk kez olusturmak için alinmasi gereken toplam bayt miktari; tüm kritik kaynaklarin aktarim dosyasi boyutlarinin toplamidir. Tek HTML sayfasi içeren ilk örnegimizde tek bir kritik kaynak (HTML dokümani) bulunuyordu. Kritik yol uzunlugu da 1 ag gidis gelisine esitti (dosyanin küçük oldugu varsayimiyla) ve toplam kritik bayt sayisi, HTML dokümaninin kendisinin aktarim boyutuydu.

Now let's compare that to the critical path characteristics of the HTML + CSS example above:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** kritik kaynak
* En düsük kritik yol uzunlugu için **2** veya daha fazla gidis gelis
* **9** KB kritik bayt

Sonuç olarak, optimize edilmis sayfamiz simdi tekrar iki kritik kaynaktir (HTML ve CSS), en düsük kritik yol uzunlugu iki gidis gelistir ve toplam 9 KB kritik bayt içerir.

Son olarak, CSS stil sayfasinin yalnizca yazdirma için gerektigini düsünelim. Nasil görünürdü?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

style.css kaynagi yalnizca yazdirma için kullanildigindan, tarayicinin sayfayi olusturmak için bunu engellemesine gerek yoktur. Dolayisiyla, DOM yapimi tamamlanir tamamlanmaz tarayici sayfayi olusturmak için yeterli bilgiye sahip olur! Sonuç olarak, bu sayfada yalnizca tek bir kritik kaynak (HTML dokümani) vardir ve en düsük kritik olusturma yolu uzunlugu bir gidis gelistir.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

That said, in practice if we look at this page's "network waterfall," you'll see that both the CSS and JavaScript requests are initiated at about the same time; the browser gets the HTML, discovers both resources, and initiates both requests. As a result, the above page has the following critical path characteristics:

* **3** kritik kaynak
* En düsük kritik yol uzunlugu için **2** veya daha fazla gidis gelis
* **11** KB kritik bayt

We now have three critical resources that add up to 11KB of critical bytes, but our critical path length is still two roundtrips because we can transfer the CSS and JavaScript in parallel. **Figuring out the characteristics of your critical rendering path means being able to identify the critical resources and also understanding how the browser will schedule their fetches.** Let's continue with our example.

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

An asynchronous script has several advantages:

* Komut dosyasi artik ayristiriciyi engellemez ve kritik olusturma yolunun bir parçasi degildir
* Baska kritik komut dosyasi olmadigindan, CSS'nin de domContentLoaded olayini engellemesi gerekmez
* domContentLoaded olayi ne kadar çabuk etkinlesirse, diger uygulama mantiginin yürütülmesine de o kadar çabuk baslanabilir

As a result, our optimized page is now back to two critical resources (HTML and CSS), with a minimum critical path length of two roundtrips, and a total of 9KB of critical bytes.

Finally, if the CSS stylesheet were only needed for print, how would that look?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

Because the style.css resource is only used for print, the browser doesn't need to block on it to render the page. Hence, as soon as DOM construction is complete, the browser has enough information to render the page. As a result, this page has only a single critical resource (the HTML document), and the minimum critical rendering path length is one roundtrip.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}