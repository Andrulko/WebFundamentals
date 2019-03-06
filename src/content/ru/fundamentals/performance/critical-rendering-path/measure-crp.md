project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Оптимизация невозможна без анализа данных. С помощью Navigation Timing API вы можете отследить каждый этап визуализации веб-страницы!

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Отслеживание визуализации {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Оптимизация невозможна без анализа данных. С помощью Navigation Timing API вы можете отследить каждый этап визуализации веб-страницы!

* С помощью Navigation Timing API можно получать детальные временные отметки для анализа визуализации.
* Браузер делит визуализацию на несколько этапов, названия которых можно использовать для анализа.

Стратегия оптимизации строится на анализе времени загрузки страниц. Его удобно проводить с помощью Navigation Timing API.

## Auditing a page with Lighthouse {: #lighthouse }

Lighthouse is a web app auditing tool that runs a series of tests against a given page, and then displays the page's results in a consolidated report. You can run Lighthouse as a Chrome Extension or NPM module, which is useful for integrating Lighthouse with continuous integration systems.

При загрузке страницы браузер отслеживает время, затраченное на каждый отмеченный на диаграмме этап. Обратите внимание, что мы пока не учитываем сетевые этапы. К ним вы вернемся в одном из следующих уроков.

А пока давайте выясним, что означают эти названия на диаграмме.

![Lighthouse's CRP audits](images/lighthouse-crp.png)

See [Critical Request Chains](/web/tools/lighthouse/audits/critical-request-chains) for more information on this audit's results.

## Instrumenting your code with the Navigation Timing API {: #navigation-timing }

Этот код на первый взгляд может показаться пугающим, однако на самом деле все очень просто! Navigation Timing API записывает нужные временные отметки, дожидается инициации события `onload` (после завершения этапов domInteractive, domContentLoaded и domComplete) и вычисляет разницу во времени между отметками.

<img src="images/dom-navtiming.png"  alt="Navigation Timing" />

Each of the labels in the above diagram corresponds to a high resolution timestamp that the browser tracks for each and every page it loads. In fact, in this specific case we're only showing a fraction of all the different timestamps &mdash; for now we're skipping all network related timestamps, but we'll come back to them in a future lesson.

So, what do these timestamps mean?

* **domLoading**. Начальная отметка. В этот момент браузер начинает анализировать первые байты HTML- документа.
* **domInteractive**. В это время браузер завершил анализ документа и создал модель DOM.
* `domContentLoaded`. К этому моменту модели DOM и CSSOM уже готовы, поэтому ничто не мешает запуску скриптов JavaScript. Это значит, что браузер может приступать к созданию модели визуализации. 
    * Если ваш фреймворк JavaScript запускается на этом этапе, вы можете отследить, как долго он выполняется, с помощью меток *EventStart* и *EventEnd*.
* **domComplete**. К этому моменту обработка завершена, а контент страницы (например, изображения) полностью скачан. Индикатор загрузки перестает вращаться.
* **loadEvent**. Сигнал полного завершения загрузки. Его можно использовать в качестве триггера функций и скриптов, добавив в HTML-код событие `onload`.

The HTML specification dictates specific conditions for each and every event: when it should be fired, which conditions should be met, and so on. For our purposes, we'll focus on a few key milestones related to the critical rendering path:

* **domInteractive**. Модель DOM готова.
* `domContentLoaded` . Модели [DOM и CSSOM](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/) готовы. 
    * Если в HTML-документе нет встроенного кода JavaScript, событие *DOMContentLoaded* инициируется сразу после *domInteractive*.
* **domComplete**. Страница готова, ее контент загружен.

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