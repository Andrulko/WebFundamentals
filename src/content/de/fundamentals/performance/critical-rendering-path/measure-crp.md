project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Sie können nicht optimieren, was Sie nicht messen können. Mit dem Navigation Timing API verfügen wir jedoch über alle notwendigen Tools zum Messen der einzelnen Schritte des kritischen Rendering-Pfads (Critical Rendering Path, CRP)!

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Den kritischen Rendering-Pfad mit Navigation Timing messen {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Sie können nicht optimieren, was Sie nicht messen können. Mit dem Navigation Timing API verfügen wir jedoch über alle notwendigen Tools zum Messen der einzelnen Schritte des kritischen Rendering-Pfads (Critical Rendering Path, CRP)!

* Navigation Timing liefert hochauflösende Zeitstempel für die Messung des CRP.
* Der Browser gibt Serien verarbeitbarer Ereignisse aus, die verschiedene Phasen des CRP erfassen.

Die Grundlagen einer soliden Leistungsstrategie sind gute Messungen und eine geeignete Instrumentierung. Das Navigation Timing API stellt genau das zur Verfügung.

## Auditing a page with Lighthouse {: #lighthouse }

Lighthouse is a web app auditing tool that runs a series of tests against a given page, and then displays the page's results in a consolidated report. You can run Lighthouse as a Chrome Extension or NPM module, which is useful for integrating Lighthouse with continuous integration systems.

Jede der Beschriftungen im obigen Diagramm entspricht einem hochauflösenden Zeitstempel, den der Browser für jede einzelne geladene Seite erfasst. In diesem speziellen Fall ist tatsächlich nur ein Bruchteil der unterschiedlichen Zeitstempel dargestellt. Wir ignorieren zunächst alle netzwerkbezogenen Zeitstempel, kommen aber später darauf zurück.

Was bedeuten diese Zeitstempel nun?

![Lighthouse's CRP audits](images/lighthouse-crp.png)

Das obige Beispiel erscheint vielleicht ein wenig einschüchternd, ist in Wirklichkeit jedoch ziemlich einfach. Die Navigation Timing API erfasst alle relevanten Zeitstempel und unser Code wartet einfach auf den Start des `onload`-Ereignisses und berechnet die Differenz zwischen den verschiedenen Zeitstempeln. Beachten Sie, dass das onload-Ereignis nach domInteractive, domContentLoaded und domComplete ausgelöst wird.

## Instrumenting your code with the Navigation Timing API {: #navigation-timing }

Wir verfügen nun über einige spezifische zu verfolgende Wegmarken und eine einfache Funktion zur Ausgabe dieser Messungen. Beachten Sie, dass Sie den Code so modifizieren können, dass anstatt der Anzeige dieser Messdaten auf der Seite diese an einen Analyse-Server gesendet werden ([Google Analytics tut dies automatisch](https://support.google.com/analytics/answer/1205784)). Das ist eine großartige Möglichkeit, die Leistung auf Ihren Seiten zu überwachen und Seiten zu identifizieren, die von einer Optimierung profitieren würden.

<img src="images/dom-navtiming.png"  alt="Navigation Timing" />

Each of the labels in the above diagram corresponds to a high resolution timestamp that the browser tracks for each and every page it loads. In fact, in this specific case we're only showing a fraction of all the different timestamps &mdash; for now we're skipping all network related timestamps, but we'll come back to them in a future lesson.

So, what do these timestamps mean?

* **domLoading:** Dies ist der Zeitstempel am Beginn des gesamten Prozesses. Der Browser startet gleich mit dem Parsen der ersten empfangenen Bytes des HMTL- Dokuments.
* **domInteractive:** Markiert den Zeitpunkt, an dem der Browser mit dem Parsen des gesamten HTML-Codes fertig und die DOM-Erstellung abgeschlossen ist.
* `domContentLoaded`Markiert den Zeitpunkt, an dem das DOM einsatzbereit ist und keine Stylesheets die JavaScript-Ausführung blockieren, d. h., wir könnten die Rendering-Baumstruktur jetzt erstellen. 
    * Viele JavaScript-Frameworks warten auf dieses Ereignis, bevor sie mit der Ausführung ihrer eigenen Logik beginnen. Aus diesem Grund erfasst der Browser die Zeitstempel *EventStart* und *EventEnd* und wir können nachverfolgen, wie lange diese Ausführung dauerte.
* **domComplete:** Die gesamte Verarbeitung ist abgeschlossen und alle Ressourcen (Bilder usw.) auf der Seite sind heruntergeladen, d. h., der Ladekreisel dreht sich nicht mehr.
* **loadEvent:** Als finalen Schritt beim Laden jeder Seite gibt der Browser ein `onload`-Ereignis aus, das weitere Anwendungslogik starten kann.

The HTML specification dictates specific conditions for each and every event: when it should be fired, which conditions should be met, and so on. For our purposes, we'll focus on a few key milestones related to the critical rendering path:

* **domInteractive** markiert die Einsatzbereitschaft des DOM.
* `domContentLoaded` markiert typischerweise den Zeitpunkt, zu dem [sowohl das DOM als auch das CSSOM einsatzbereit sind](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/). 
    * Wenn kein Parser vorhanden ist, der JavaScript blockiert, dann wird *DOMContentLoaded* unmittelbar nach *domInteractive* gestartet.
* **domComplete** markiert den Zeitpunkt, zu dem die Seite und alle ihre Unterressourcen einsatzbereit sind.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp.html){: target="_blank" .external }

The above example may seem a little daunting on first sight, but in reality it is actually pretty simple. The Navigation Timing API captures all the relevant timestamps and our code simply waits for the `onload` event to fire &mdash; recall that `onload` event fires after `domInteractive`, `domContentLoaded` and `domComplete` &mdash; and computes the difference between the various timestamps.

<img src="images/device-navtiming-small.png"  alt="NavTiming demo" />

All said and done, we now have some specific milestones to track and a simple function to output these measurements. Note that instead of printing these metrics on the page you can also modify the code to send these metrics to an analytics server ([Google Analytics does this automatically](https://support.google.com/analytics/answer/1205784)), which is a great way to keep tabs on performance of your pages and identify candidate pages that can benefit from some optimization work.

## What about DevTools? {: #devtools }

Although these docs sometimes use the Chrome DevTools Network panel to illustrate CRP concepts, DevTools is currently not well-suited for CRP measurements because it does not have a built-in mechanism for isolating critical resources. Run a [Lighthouse](#lighthouse) audit to help identify such resources.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}