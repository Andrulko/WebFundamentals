project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Ölçemediginiz bir seyi optimize edemezsiniz. Neyse ki, Gezinme Zamanlamasi API'si bize kritik olusturma yolunun her bir adimini ölçmemiz için gereken tüm araçlari veriyor!

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Kritik Olusturma Yolunu Gezinme Zamanlamasiyla Ölçme {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Ölçemediginiz bir seyi optimize edemezsiniz. Neyse ki, Gezinme Zamanlamasi API'si bize kritik olusturma yolunun her bir adimini ölçmemiz için gereken tüm araçlari veriyor!

* Gezinme Zamanlamasi, CRP ölçümü için yüksek çözünürlüklü zaman damgalari saglar.
* Tarayici, CRP''nin çesitli asamalarini yakalayan bir tüketilir olaylar serisi yayinlar.

Her saglam performans stratejisinin temeli, iyi ölçüme ve araçlarla is yapmaya dayanir. Görünüse göre, Gezinme Zamanlamasi API'sinin tam olarak sagladigi da budur.

## Auditing a page with Lighthouse {: #lighthouse }

Lighthouse is a web app auditing tool that runs a series of tests against a given page, and then displays the page's results in a consolidated report. You can run Lighthouse as a Chrome Extension or NPM module, which is useful for integrating Lighthouse with continuous integration systems.

Yukaridaki semada bulunan etiketlerin her biri, tarayicinin yükledigi her sayfa için izledigi yüksek çözünürlüklü bir zaman damgasina karsilik gelir. Aslinda, bu özel örnekte tüm farkli zaman damgalarinin yalnizca bir kismini gösteriyoruz. Simdilik agla ilgili tüm zaman damgalarini atliyoruz, ancak ileride göreceginiz bir baska derste bunlara geri dönecegiz.

Bu zaman damgalari ne anlama geliyor?

![Lighthouse's CRP audits](images/lighthouse-crp.png)

See [Critical Request Chains](/web/tools/lighthouse/audits/critical-request-chains) for more information on this audit's results.

## Instrumenting your code with the Navigation Timing API {: #navigation-timing }

Yukaridaki örnek ilk bakista biraz ürkütücü görünebilir, ancak gerçekte oldukça basittir. Gezinme Zamanlamasi API'si, ilgili tüm zaman damgalarini yakalar ve bizim kodumuz yalnizca `onload` olayinin etkinlesmesini bekler. Onload olayinin domInteractive, domContentLoaded ve domComplete olaylarindan sonra etkinlestigini hatirlayin. Kodumuz, daha sonra çesitli zaman damgalari arasindaki farki hesaplar.

<img src="images/dom-navtiming.png"  alt="Navigation Timing" />

Each of the labels in the above diagram corresponds to a high resolution timestamp that the browser tracks for each and every page it loads. In fact, in this specific case we're only showing a fraction of all the different timestamps &mdash; for now we're skipping all network related timestamps, but we'll come back to them in a future lesson.

So, what do these timestamps mean?

* **domLoading:** Bu, tüm sürecin baslangiç zaman damgasidir; tarayici aldigi ilk HTML dokümaninin baytlarini ayristirmaya baslamak üzeredir.
* **domInteractive:** Tarayicinin tüm HTML'yi ayristirmayi bitirdigi ve DOM yapimini tamamladigi noktayi isaretler.
* `domContentLoaded`DOM'nin hazir oldugu ve JavaScript yürütmesini engelleyen herhangi bir stil sayfasinin bulunmadigi noktayi isaretler; artik olusturma agacinin yapimini (muhtemelen) gerçeklestirebilecegimiz anlamina gelir. 
    * Birçok JavaScript çerçevesi, kendi mantiklarini yürütmeye baslamak için bu olayi bekler. Bu nedenle, tarayici *EventStart* ve *EventEnd* zaman damgalarini, bu yürütmenin ne kadar sürecegini izleyebilmemiz için yakalar.
* **domComplete:** Adindan da anlasilacagi gibi, islemenin tümü tamamlanmistir ve sayfadaki kaynaklarin (resimler vb.) tamaminin indirme islemi bitmistir (yükleme deger degistiricisinin dönmesi durmustur).
* **loadEvent:** Her sayfa yüklemesinde son bir adim olarak tarayici, ek uygulama mantigini tetikleyebilecek bir `onload` olayini etkinlestirir.

The HTML specification dictates specific conditions for each and every event: when it should be fired, which conditions should be met, and so on. For our purposes, we'll focus on a few key milestones related to the critical rendering path:

* **domInteractive**, DOM'nin hazir oldugu zamani isaretler.
* `domContentLoaded` , genellikle [hem DOM hem de CSSOM'nin hazir oldugu](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/) zamani isaretler. 
    * Ayristiriciyi engelleyen JavaScript yoksa *DOMContentLoaded* olayi, *domInteractive* olayindan hemen sonra etkinlesir.
* **domComplete**, sayfanin ve tüm alt kaynaklarinin hazir oldugu zamani isaretler.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp.html" region_tag="full"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp.html){: target="_blank" .external }

The above example may seem a little daunting on first sight, but in reality it is actually pretty simple. The Navigation Timing API captures all the relevant timestamps and our code simply waits for the `onload` event to fire &mdash; recall that `onload` event fires after `domInteractive`, `domContentLoaded` and `domComplete` &mdash; and computes the difference between the various timestamps.

<img src="images/device-navtiming-small.png"  alt="NavTiming demo" />

All said and done, we now have some specific milestones to track and a simple function to output these measurements. Note that instead of printing these metrics on the page you can also modify the code to send these metrics to an analytics server ([Google Analytics does this automatically](https://support.google.com/analytics/answer/1205784)), which is a great way to keep tabs on performance of your pages and identify candidate pages that can benefit from some optimization work.

## What about DevTools? {: #devtools }

Although these docs sometimes use the Chrome DevTools Network panel to illustrate CRP concepts, DevTools is currently not well-suited for CRP measurements because it does not have a built-in mechanism for isolating critical resources. Run a [Lighthouse](#lighthouse) audit to help identify such resources.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}