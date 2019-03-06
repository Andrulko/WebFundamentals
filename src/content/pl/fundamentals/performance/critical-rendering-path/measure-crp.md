project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Jeśli czegoś nie da się zmierzyć, nie można tego zoptymalizować. Na szczęście interfejs API Navigation Timing zapewnia nam wszystkie narzędzia do pomiarów na każdym etapie krytycznej ścieżki renderowania.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Pomiar krytycznej ścieżki renderowania za pomocą interfejsu Navigation Timing {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Jeśli czegoś nie da się zmierzyć, nie można tego zoptymalizować. Na szczęście interfejs API Navigation Timing zapewnia nam wszystkie narzędzia do pomiarów na każdym etapie krytycznej ścieżki renderowania.

* Interfejs Navigation Timing udostępnia sygnatury o wysokiej rozdzielczości czasowej, umożliwiające pomiar krytycznej ścieżki renderowania.
* Na różnych etapach krytycznej ścieżki renderowania przeglądarka generuje serię zdarzeń, które można następnie przechwycić.

Podstawą każdej solidnej strategii optymalizacji wydajności jest poprawny pomiar i dobre narzędzia. Właśnie to zapewnia interfejs API Navigation Timing.

## Auditing a page with Lighthouse {: #lighthouse }

Lighthouse is a web app auditing tool that runs a series of tests against a given page, and then displays the page's results in a consolidated report. You can run Lighthouse as a Chrome Extension or NPM module, which is useful for integrating Lighthouse with continuous integration systems.

Każda z etykiet na powyższym diagramie odpowiada sygnaturze czasowej o wysokiej rozdzielczości. Przeglądarka generuje te sygnatury dla każdej wczytywanej strony. W zasadzie przedstawiamy tutaj tylko niewielką część wszystkich sygnatur czasowych &ndash; na razie pomijamy te związane z siecią, ale powrócimy do nich w jednej z następnych lekcji.

Co oznaczają te sygnatury czasowe?

![Lighthouse's CRP audits](images/lighthouse-crp.png)

See [Critical Request Chains](/web/tools/lighthouse/audits/critical-request-chains) for more information on this audit's results.

## Instrumenting your code with the Navigation Timing API {: #navigation-timing }

Na pierwszy rzut oka powyższy przykład może wydawać się nieco zniechęcający, ale w rzeczywistości jest całkiem prosty. Interfejs API Navigation Timing przechwytuje wszystkie odpowiednie sygnatury czasowe, a nasz kod po prostu czeka na wyzwolenie zdarzenia `onload` &ndash; przypominamy, że zdarzenie onload jest generowane po zdarzeniu domInteractive, domContentLoaded i domComplete &ndash; następnie obliczana jest różnica czasu między różnymi sygnaturami czasowymi.

<img src="images/dom-navtiming.png"  alt="Navigation Timing" />

Each of the labels in the above diagram corresponds to a high resolution timestamp that the browser tracks for each and every page it loads. In fact, in this specific case we're only showing a fraction of all the different timestamps &mdash; for now we're skipping all network related timestamps, but we'll come back to them in a future lesson.

So, what do these timestamps mean?

* **domLoading:** jest to sygnatura czasowa, od której rozpoczyna się cały proces &ndash; przeglądarka właśnie ma rozpocząć parsowanie pierwszych odebranych bajtów dokumentu HTML.
* **domInteractive:** sygnalizuje moment, w którym przeglądarka kończy parsowanie wszystkich znaczników HTML i tworzenie modelu DOM.
* `domContentLoaded`sygnalizuje moment, w którym model DOM jest gotowy, arkusze stylów blokujące wykonywanie skryptów JavaScript nie występują, co oznacza, że teraz można (potencjalnie) utworzyć drzewo renderowania. 
    * Wiele platform JavaScript czeka z wykonywaniem własnego kodu do pojawienia się tego zdarzenia. Z tej przyczyny przeglądarka przechwytuje sygnatury czasowe *EventStart* i *EventEnd*, umożliwiając śledzenia czasu wykonywania.
* **domComplete:** jak wskazuje nazwa, ukończono wszystkie procedury przetwarzania i pobieranie wszystkich zasobów na stronie (obrazów itd.) - tzn. wskaźnik postępu wczytywania przestał się obracać.
* **loadEvent:** ostatnim etapem wczytywania strony jest wygenerowanie zdarzenia `onload`, które może wyzwolić wykonywanie dodatkowego kodu aplikacji.

The HTML specification dictates specific conditions for each and every event: when it should be fired, which conditions should be met, and so on. For our purposes, we'll focus on a few key milestones related to the critical rendering path:

* **domInteractive** sygnalizuje, kiedy gotowy jest model DOM.
* `domContentLoaded` zazwyczaj sygnalizuje, kiedy [zarówno model DOM, jak i model CSSOM są gotowe](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/){: .external }. 
    * Jeśli nie występują skrypty JavaScript blokujące parsowanie, zdarzenie *DOMContentLoaded* jest wyzwalane bezpośrednio po zdarzeniu *domInteractive*.
* Zdarzenie **domComplete** informuje o gotowości strony i wszystkich skojarzonych z nią zasobów.

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