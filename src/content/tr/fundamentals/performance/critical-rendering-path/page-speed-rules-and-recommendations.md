project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Baglamla ilgili PageSpeed Insights kurallari: Kritik Olusturma Yolunu optimize neye önem verilmeli ve neden.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# PageSpeed Kurallari ve Önerileri {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Baglamla ilgili PageSpeed Insights kurallari: Kritik Olusturma Yolunu optimize neye önem verilmeli ve neden.

## Olusturmayi engelleyen JavaScript'i ve CSS'yi çikarma

Ilk olusturmada en hizli süreyi saglamak için sayfadaki kritik kaynaklarin sayisini en aza indirmek ve (mümkün oldugunda) bunlari çikarmak, indirilen kritik bayt sayisini en aza indirmek ve kritik yol uzunlugunu optimize etmek istersiniz.

## JavaScript Kullanimini Optimize Etme

JavaScript kaynaklari, *async* olarak isaretlenmezlerse veya bir özel JavaScript snippet'i araciligiyla eklenmezlerse varsayilan olarak ayristiriciyi engellerler. Ayristiriciyi engelleyen JavaScript, tarayiciyi CSSOM'yi beklemeye ve DOM yapimini duraklatmaya zorlar; buna karsilik, ilk olusturmanin süresi önemli ölçüde gecikebilir.

### Prefer asynchronous JavaScript resources

Zaman uyumsuz kaynaklar doküman ayristiricisinin engellemesini kaldirir ve tarayicinin, komut dosyasini yürütmeden önce CSSOM'yi engellemekten kaçinmasina olanak tanir. Genellikle komut dosyasi zaman uyumsuz yapilabilirse, bu komut dosyasinin ilk olusturma için gerekli olmadigi anlamina da gelir. Zaman uyumsuz komut dosyalarini ilk olusturmadan sonra yüklemeyi düsünebilirsiniz.

### Avoid synchronous server calls

Tarayicinin sayfayi olustururken gerçeklestirmesi gereken is miktarini en aza indirmek üzere ilk olusturmada görünür içerigin olusturulmasi için kritik ve gerekli olmayan komut dosyalari ertelenmelidir.

    <script>
      function() {
        window.addEventListener('pagehide', logData, false);
        function logData() {
          navigator.sendBeacon(
            'https://putsreq.herokuapp.com/Dt7t2QzUkG18aDTMMcop',
            'Sent by a beacon!');
        }
      }();
    </script>
    

Uzun çalisan JavaScript, tarayicinin DOM, CSSOM ve sayfayi olusturmasini engeller. Sonuç olarak, ilk olusturmada gerekli olmayan baslatma mantigi ve islevleri daha sonrasina ertelenmelidir. Uzun bir baslatma sirasinin çalistirilmasi gerekiyorsa tarayicinin diger olaylari da arada islemesine olanak tanimak için bunu, birkaç asamaya bölmeyi düsünebilirsiniz.

    <script>
    fetch('./api/some.json')  
      .then(  
        function(response) {  
          if (response.status !== 200) {  
            console.log('Looks like there was a problem. Status Code: ' +  response.status);  
            return;  
          }
          // Examine the text in the response  
          response.json().then(function(data) {  
            console.log(data);  
          });  
        }  
      )  
      .catch(function(err) {  
        console.log('Fetch Error :-S', err);  
      });
    </script>
    

CSS, olusturma agacini olusturmak için gerekir ve JavaScript genellikle sayfanin ilk yapimi sirasinda CSS'yi engeller. Gerekli olmayan CSS'nin (ör. yazdirma ve diger medya sorgulari) kritik degil seklinde isaretlendiginden ve kritik CSS miktari ile bunu saglamak için gereken sürenin mümkün oldugunca küçük tutuldugundan emin olmalisiniz.

    <script>
    fetch(url, {
      method: 'post',
      headers: {  
        "Content-type": "application/x-www-form-urlencoded; charset=UTF-8"  
      },  
      body: 'foo=bar&lorem=ipsum'  
    }).then(function() { // Additional code });
    </script>
    

### Defer parsing JavaScript

Tüm CSS kaynaklari, HTML dokümani içinde mümkün oldugunca erken belirtilmelidir. Böylece, tarayici `<link>` etiketlerini kesfedebilir ve CSS'ye iliskin istegi mümkün olan en kisa sürede gönderir.

### Avoid long running JavaScript

CSS içe aktarma (@import) yönergesi, bir stil sayfasinin kurallari baska bir stil sayfasi dosyasina içe aktarabilmesini saglar. Ancak, bu yönergeler kritik yolda ek gidis gelislere neden olacagindan bu yönergelerden kaçinilmalidir: Içe aktarilan CSS kaynaklari, yalnizca @import kuralini içeren CSS stil sayfasinin kendisi alindiktan ve ayristirildiktan sonra kesfedilir.

## CSS Kullanimini Optimize Etme

En iyi performans için kritik CSS'yi dogrudan HTML dokümaninda satir içine yerlestirmek isteyebilirsiniz. Bu, kritik yoldaki ek gidis gelisleri ortadan kaldirir ve dogru yapilirsa, yalnizca HTML'nin engelleyen kaynak oldugu bir `tek gidis gelislik` kritik yol uzunlugu saglamak için kullanilabilir.

### Put CSS in the document head

Specify all CSS resources as early as possible within the HTML document so that the browser can discover the `<link>` tags and dispatch the request for the CSS as soon as possible.

### Avoid CSS imports

The CSS import (`@import`) directive enables one stylesheet to import rules from another stylesheet file. However, avoid these directives because they introduce additional roundtrips into the critical path: the imported CSS resources are discovered only after the CSS stylesheet with the `@import` rule itself is received and parsed.

### Inline render-blocking CSS

For best performance, you may want to consider inlining the critical CSS directly into the HTML document. This eliminates additional roundtrips in the critical path and if done correctly can deliver a "one roundtrip" critical path length where only the HTML is a blocking resource.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}