project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: JavaScript, sayfanin neredeyse her yönünü degistirmemize olanak tanir: içerik, stil ve kullanici etkilesimlerine karsi davranisi. Bununla birlikte , JavaScript DOM yapimini engelleyebilir ve sayfanin olusturulma zamanini geciktirebilir. En iyi performansi sunmak için JavaScript'inizi zaman uyumsuz yapin ve önemli olusturma yolundaki gereksiz JavaScript kodlarini kaldirin.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2013-12-31 #}

# JavaScript ile Etkilesim Ekleme {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

JavaScript, sayfanin neredeyse her yönünü degistirmemize olanak tanir: içerik, stil ve kullanici etkilesimlerine karsi davranisi. Bununla birlikte , JavaScript DOM yapimini engelleyebilir ve sayfanin olusturulma zamanini geciktirebilir. En iyi performansi sunmak için JavaScript'inizi zaman uyumsuz yapin ve önemli olusturma yolundaki gereksiz JavaScript kodlarini kaldirin.

### TL;DR {: .hide-from-toc }

* JavaScript; DOM ve CSSOM'yi sorgulayip degistirebilir.
* JavaScript yürütmesi CSSOM'de engelleme yapar.
* JavaScript, açik bir sekilde zaman uyumsuz oldugu açiklanmazsa DOM yapisini engeller.

JavaScript, tarayicinin içinde çalisan dinamik bir dildir ve sayfa davranisiyla ilgili neredeyse her unsuru degistirmemize olanak tanir: DOM agacindan ögeler ekleyerek veya bu ögeleri kaldirarak sayfadaki içerigi degistirebiliriz, her bir ögenin CSSOM özelliklerini degistirebiliriz, kullanici girisini isleyebiliriz ve daha birçok islem yapabiliriz. Bunu çalisirken göstermek için önceki `Herkese Merhaba` örnegimizi basit bir satir içi komut dosyasiyla genisletelim:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/script.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/script.html){: target="_blank" .external }

* JavaScript, DOM'ye erismemize ve gizli span dügümüne referansi çikarmamiza olanak tanir. Dügüm, olusturma agacinda görünmeyebilir, ancak DOM'de bulunmaya devam eder! Daha sonra, referansi aldigimizda metnini (.textContent araciligiyla) degistirebilir, hatta hesaplanmis görüntü stili özelligini geçersiz kilip `none` yerine `inline` degerine ayarlayabiliriz. Her seyin sonunda, sayfamiz artik `**Merhaba etkilesimli ögrenciler!**` mesajini görüntüleyecek.

* JavaScript, yeni ögeler olusturmamiza, biçimlendirmemize, bunlari DOM'ye eklememize ve DOM'den kaldirmamiza da olanak tanir. Aslinda, teknik olarak sayfamizin tamami, ögeleri tek tek olusturan ve biçimlendiren bir büyük JavaScript dosyasi olabilirdi. Bu ise yarardi, ancak HTML ve CSS ile çalismak pratikte çok daha kolaydir. JavaScript islevimizin ikinci bölümünde, yeni bir div ögesi olusturuyoruz, metin içerigini ayarliyoruz, biçimlendiriyoruz ve bunu gövdeye ekliyoruz.

<img src="images/device-js-small.png"  alt="page preview" />

Bununla birlikte, altta yatan büyük bir performans riski vardir. JavaScript bize birçok güç saglar, ancak sayfanin nasil ve ne zaman olusturulacagiyla ilgili çok sayida ek sinirlama da olusturur.

Öncelikle, yukaridaki örnekte satir içi komut dosyamizin sayfanin alt kismina yakin bir yerde olduguna dikkat edin. Neden? Bunu kendiniz denemelisiniz, ancak komut dosyasini *span* ögesinin üzerine tasirsak komut dosyasinin basarisiz olacagini fark edip dokümanda herhangi bir *span* ögesine yapilmis basvuru bulamadigindan sikayet edersiniz. Örnegin, *getElementsByTagName('span')*, *null* degerini döndürür. Bu, önemli bir özelligi gösterir: Komut dosyamiz, tam olarak dokümana ekledigimiz noktada yürütülür. HTML ayristirici bir komut dosyasi etiketiyle karsilastiginda, DOM yapim islemini duraklatir ve denetimi JavaScript motoruna verir; JavaScript motorunun çalismasi bittiginde, tarayici kaldigi yerden devam eder ve DOM olusturma islemini sürdürür.

Diger bir deyisle, komut dosyasi blogumuz, henüz islenmedikleri için sayfanin sonraki kisimlarinda geçen hiçbir ögeyi bulamaz! Biraz daha farkli da açiklayabiliriz: **satir içi komut dosyamizi yürütmemiz, DOM yapimini engelleyerek baslangiç olusturmasini da geciktirir.**

Sayfamizda komut dosyalari kullanmamizin sagladigi hemen fark edilmeyen bir baska özellik de bunlarin yalnizca DOM'yi degil, ayni zamanda CSSOM özelliklerini de okuyup degistirebilmeleridir. Aslinda, örnegimizde span ögesinin none olan görüntü özelligini inline olarak degistirdigimizde yapmakta oldugumuz sey tam olarak da budur. Sonuç? Artik bir yaris kosulumuz var.

Komut dosyamizi çalistirmak istedigimizde tarayici CSSOM'yi indirip olusturmayi bitirmediyse ne olur? Yanit basit ve performans için hiç de iyi degil: **Tarayici, CSSOM'yi indirme ve olusturma islemini bitirinceye kadar komut dosyasi yürütmesini geciktirir ve biz beklerken, DOM yapimi da engellenir!**

Kisacasi JavaScript; DOM, CSSOM ve JavaScript yürütmesi arasina yeni birçok bagimlilik ekler ve tarayicinin, sayfamizi ekranda isleyip olusturma hizi açisindan da önemli gecikmelere yol açabilir:

'Kritik olusturma yolunu optimize etmek'ten söz ettigimizde, genis bir ölçekte HTML, CSS ve JavaScript arasindaki bagimlilik grafigini anlamaktan ve optimize etmekten bahsediyoruz.

* The location of the script in the document is significant.
* When the browser encounters a script tag, DOM construction pauses until the script finishes executing.
* JavaScript can query and modify the DOM and the CSSOM.
* JavaScript execution pauses until the CSSOM is ready.

Varsayilan olarak, JavaScript yürütmesi `ayristirici engellemesi`dir: Tarayici, dokümanda bir komut dosyasiyla karsilastiginda DOM yapimini duraklatmasi, denetimi JavaScript çalisma zamanina devretmesi ve DOM yapimina devam etmeden önce komut dosyasinin yürütülmesini beklemesi gerekir. Bunu, önceki örnegimizde bir satir içi komut dosyasiyla uygulamada zaten gördük. Aslinda, satir içi komut dosyalari, siz özel olarak dikkat etmez ve yürütmelerini geciktirmek için ek kod yazmazsaniz her zaman ayristirici engellemesi yapar.

## Ayristirici Engellemesi ve Zaman Uyumsuz JavaScript

Bir script etiketi araciligiyla eklenecek komut dosyalarina ne dersiniz? Önceki örnegimizi alalim ve kodumuzu ayri bir dosyaya çikartalim:

What about scripts included via a script tag? Let's take our previous example and extract the code into a separate file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/split_script.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**app.js**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/app.js" region_tag="full"   adjust_indentation="auto" %}
</pre>

Ancak iyi bir haberimiz var, bunun için bir imdat çikisimiz bulunuyor! Varsayilan olarak tüm JavaScript ayristirici engellemesi yapar ve tarayici, komut dosyasinin sayfada ne yapmayi planladigini bilmez; dolayisiyla, en kötü senaryoyu düsünmeli ve ayristiriciyi engellemelidir. Ancak, tarayiciya bir sinyal verip komut dosyasinin, tam olarak dokümanda basvuruda bulunuldugu noktada yürütülmesine gerek olmadigini söyleyebilseydik ne olurdu? Bunu yapmamiz, tarayicinin DOM yapimina devam etmesine ve komut dosyasini hazir olduktan sonra, yani dosya önbellekten veya bir uzak sunucudan getirildikten sonra yürütmesine olanak tanirdi.

Peki, bu hileyi nasil gerçeklestirecegiz? Oldukça basit, komut dosyamizi *async* olarak isaretleyebiliriz:

async (zaman uyumsuz) anahtar kelimesinin komut dosyasina eklenmesi tarayiciya, komut dosyasinin kullanilabilir hale gelmeyi beklerken DOM yapimini engellememesi gerektigini bildirir. Bu, büyük bir performans kazanci saglar!

To achieve this, we mark our script as *async*:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/split_script_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/split_script_async.html){: target="_blank" .external }

Adding the async keyword to the script tag tells the browser not to block DOM construction while it waits for the script to become available, which can significantly improve performance.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}