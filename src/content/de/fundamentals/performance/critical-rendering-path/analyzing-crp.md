project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Zur Erkennung und Behebung von Leistungsengpässen beim kritischen Rendering-Pfad müssen Sie die häufigen Probleme kennen. Bei unserer interaktiven Tour picken wir häufige Leistungsmuster heraus, die Ihnen bei der Optimierung Ihrer Seiten helfen.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Leistung des kritischen Rendering-Pfads analysieren {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Zur Erkennung und Behebung von Leistungsengpässen beim kritischen Rendering-Pfad müssen Sie die häufigen Probleme kennen. Bei unserer interaktiven Tour picken wir häufige Leistungsmuster heraus, die Ihnen bei der Optimierung Ihrer Seiten helfen.

Durch die Optimierung des kritischen Rendering-Pfads soll erreicht werden, dass der Browser die Seite so schnell wie möglich zeichnet, denn ein schnellerer Seitenaufbau führt zu größerem Interesse, einer höheren Anzahl aufgerufener Seiten und einer [besseren Conversion-Rate](http://www.google.com/think/multiscreen/success.html). Aus diesem Grund möchten wir den Zeitraum, in dem ein Besucher auf einen leeren Bildschirm starren muss, auf ein Minimum beschränken. Dazu nehmen wir Optimierungen im Hinblick darauf vor, welche Ressourcen in welcher Reihenfolge geladen werden.

Zur Veranschaulichung dieses Prozesses beginnen wir mit dem einfachsten möglichen Fall und bauen unsere Seite nach und nach anhand von zusätzlichen Ressourcen, Stilen und Anwendungslogik auf. Dabei sehen wir auch, was schieflaufen kann und wie sich jedes dieser Fallbeispiele optimieren lässt.

Noch eine letzte Anmerkung, bevor es losgeht: Bisher haben wir ausschließlich darüber gesprochen, was im Browser passiert, sobald die Ressource (CSS-, JS- oder HTML-Datei) verarbeitet werden kann, und die Zeit ignoriert, die für den Abruf aus dem Cache oder von einem Netzwerk erforderlich ist. Details zur Optimierung der Netzwerkaspekte unserer Anwendung finden Sie im nachfolgenden Modul. Bis dahin setzen wir für ein realistischeres Szenario Folgendes voraus:

* Die Netzwerkumlaufzeit (Gatterlaufzeit) zum Server beträgt 100 ms.
* Die Antwortzeit des Servers beträgt 100 ms für das HTML-Dokument und 10 ms für alle anderen Dateien.

## Das `Hallo Welt`-Erlebnis

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Wir beginnen ganz unkompliziert mit grundlegendem HTML-Markup und einem einzelnen Bild - ohne CSS oder JavaScript. Nun öffnen wir unsere Netzwerkzeitachse in den Chrome-Entwicklertools und sehen uns den daraus resultierenden Ressourcenwasserfall an:

We'll start with basic HTML markup and a single image; no CSS or JavaScript. Let's open up our Network timeline in Chrome DevTools and inspect the resulting resource waterfall:

<img src="images/waterfall-dom.png" alt="CRP" />

Sobald der HTML-Inhalt verfügbar ist, muss der Browser die Bytes parsen, in Token konvertieren und die DOM-Struktur erstellen. Beachten Sie, dass die Entwicklertools praktischerweise die Dauer des DOMContentLoaded-Ereignisses unten angeben (216 ms), was ebenfalls der blauen vertikalen Linie entspricht. Die Lücke zwischen dem Ende des HTML-Downloads und der blauen vertikalen Linie (DOMContentLoaded) ist die Zeit, die der Browser für die Erstellung der DOM-Struktur benötigt hat. In diesem Fall sind das nur wenige Millisekunden.

Zuletzt noch etwas Interessantes: Unser `awesome photo` hat das domContentLoaded-Ereignis nicht blockiert! Es hat sich also herausgestellt, dass wir die Renderstruktur erstellen und sogar die Seite zeichnen können, ohne auf jede einzelne Ressource der Seite warten zu müssen: **Für eine schnelle erste Zeichnung sind nicht alle Ressourcen erforderlich**. Vielmehr sprechen wir, wenn wir über den kritischen Rendering-Pfad sprechen, in der Regel über das HTML-Markup, CSS und JavaScript. Bilder blockieren das erste Rendern der Seite nicht, obwohl wir natürlich versuchen sollten, die Bilder ebenfalls schnellstmöglich zu zeichnen!<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

Das Ladeereignis - auch bekannt als `onload` - für das Bild wird blockiert: Laut Entwicklertools erfolgt das Onload-Ereignis bei 335 ms. Noch einmal zur Erinnerung: Das Onload-Ereignis markiert den Punkt, an dem **alle Ressourcen**, die die Seite benötigt, heruntergeladen und verarbeitet wurden. Das ist der Punkt, an dem die Ladeanimation im Browser aufhört, sich zu drehen. Dies wird durch die rote vertikale Linie im Wasserfall gekennzeichnet.

Unsere `'Hallo Welt'-Erlebnis`-Seite sieht zwar auf den ersten Blick einfach aus, es steckt jedoch eine Menge dahinter! In der Praxis benötigen wir mehr als nur HTML: Wahrscheinlich haben wir ein CSS-Stylesheet und ein oder mehrere Skripts, um unsere Seite interaktiver zu gestalten. Fügen wir beides hinzu und sehen, was passiert:

That said, the `load` event (also known as `onload`), is blocked on the image: DevTools reports the `onload` event at 335ms. Recall that the `onload` event marks the point at which **all resources** that the page requires have been downloaded and processed; at this point, the loading spinner can stop spinning in the browser (the red vertical line in the waterfall).

## Mischung aus JavaScript und CSS hinzufügen

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

* Im Gegensatz zu unserem reinen HTML-Beispiel müssen wir zur Erstellung des CSSOM nun zusätzlich die CSS-Datei abrufen und parsen. Außerdem wissen wir, dass wir sowohl das DOM als auch das CSSOM zur Erstellung der Renderstruktur benötigen.
* Da sich auf unserer Seite auch eine JavaScript-Datei befindet, die das Parsen blockiert, wird das domContentLoaded-Ereignis blockiert, bis die CSS-Datei heruntergeladen und geparst wurde: JavaScript fragt das CSSOM möglicherweise ab, sodass wir dies blockieren und auf CSS warten müssen, bevor wir JavaScript ausführen können.

**What if we replace our external script with an inline script?** Even if the script is inlined directly into the page, the browser can't execute it until the CSSOM is constructed. In short, inlined JavaScript is also parser blocking.

That said, despite blocking on CSS, does inlining the script make the page render faster? Let's try it and see what happens.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Externes JavaScript mit Parser-Blockierung:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, JS" />

We are making one less request, but both our `onload` and `domContentLoaded` times are effectively the same. Why? Well, we know that it doesn't matter if the JavaScript is inlined or external, because as soon as the browser hits the script tag it blocks and waits until the CSSOM is constructed. Further, in our first example, the browser downloads both CSS and JavaScript in parallel and they finish downloading at about the same time. In this instance, inlining the JavaScript code doesn't help us much. But there are several strategies that can make our page render faster.

First, recall that all inline scripts are parser blocking, but for external scripts we can add the "async" keyword to unblock the parser. Let's undo our inlining and give that a try:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Alternativ hätten wir einen anderen Ansatz versuchen und CSS und JavaScript inline bereitstellen können:

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

Die einfachste Seite besteht lediglich aus dem HTML-Markup: weder CSS noch JavaScript oder andere Arten von Ressourcen. Zum Rendern dieser Seite muss der Browser die Anforderung initiieren, auf das Eintreffen de HTML-Dokuments warten, es parsen, das DOM erstellen und es schließlich auf dem Bildschirm rendern:

Alternatively, we could have inlined both the CSS and JavaScript:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**Die Zeit zwischen T<sub>0</sub> und T<sub>1</sub> erfasst die Verarbeitungsdauer von Netzwerk und Server.** Bestenfalls (wenn die HTML-Datei klein ist), benötigen wir nur einen Netzwerkumlauf zum Abrufen des gesamten Dokuments. Aufgrund der Funktionsweise der TCP-Transportprotokolle sind bei größeren Dateien möglicherweise mehr Umläufe nötig. Darauf kommen wir später noch einmal zurück. **Folglich lässt sich sagen, dass der (minimale) kritische Rendering-Pfad der oben stehenden Seite bestenfalls aus einem Umlauf besteht.**

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" />

Notice that the `domContentLoaded` time is effectively the same as in the previous example; instead of marking our JavaScript as async, we've inlined both the CSS and JS into the page itself. This makes our HTML page much larger, but the upside is that the browser doesn't have to wait to fetch any external resources; everything is right there in the page.

Wieder benötigen wir einen Netzwerkumlauf, um das HTML-Dokument abzurufen, und das abgerufene Markup verrät uns, dass wir auch die CSS-Datei benötigen. Das bedeutet: Der Browser muss die CSS vom Server abrufen, bevor er die Seite auf dem Bildschirm rendern kann. **Infolgedessen muss diese Seite mindestens zwei Umläufe durchführen, bevor die Seite angezeigt werden kann**. Noch einmal zur Erinnerung: Im Falle der CSS-Datei sind unter Umständen mehrere Umläufe nötig, daher die Betonung auf `mindestens`.

Klären wir die Begriffe, mit denen wir den kritischen Rendering-Pfad beschreiben:

## Leistungsmuster

Vergleichen wir das nun mit den Charakteristiken des kritischen Pfads für das HTML- und CSS-Beispiel oben:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

<img src="images/analysis-dom.png" alt="Hello world CRP" />

Nun bringen wir noch eine zusätzliche JavaScript-Datei ins Spiel!

Wir haben `app.js` hinzugefügt, eine externe JavaScript-Ressource auf der Seite, und wie wir jetzt wissen, handelt es sich hierbei um eine Ressource, die den Parser blockiert, also eine kritische Ressource. Schlimmer noch: Zum Ausführen der JavaScript-Datei müssen wir auch das CSSOM blockieren und darauf warten. Noch einmal zur Erinnerung: JavaScript kann das CSSOM abfragen, wodurch der Browser solange pausiert, bis `style.css` heruntergeladen und das CSSOM erstellt wurde.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

Nun haben wir drei kritische Ressourcen und insgesamt 11 KB kritische Bytes, während die Länge unseres kritischen Pfads weiterhin zwei Umläufe beträgt, weil CSS und JavaScript parallel übertragen werden können! **Die Charakteristiken unseres kritischen Rendering-Pfads herauszufinden bedeutet, die kritischen Ressourcen identifizieren zu können und zu verstehen, wie der Browser deren Abrufe plant.** Fahren wir mit unserem Beispiel fort...

Bei einem Gespräch mit unseren Website-Entwicklern haben wir herausgefunden, dass der JavaScript-Inhalt, den wir auf unserer Seite eingefügt haben, keine Blockierungsfunktion übernehmen muss: Der darin befindliche Analyse- und sonstige Code erfordert keine Blockierung des Renderings unserer Seite. Vor diesem Hintergrund können wir dem Skript-Tag das Attribut `async` hinzufügen, um die Blockierung des Parsers aufzuheben:

* **Kritische Ressource:** die Ressource, die unter Umständen das erste Rendern der Seite blockiert.
* **Länge des kritischen Pfads:** die zum Abrufen alle kritischen Ressourcen erforderliche Anzahl an Umläufen oder die erforderliche Dauer.
* **Kritische Bytes:** die Anzahl der Bytes, die für das erste Rendern der Seite erforderlich sind, also die Summe der Transfer-Dateigrößen aller kritischen Ressourcen. Unser erstes Beispiel mit einer einzigen HTML-Seite enthielt eine kritische Ressource, nämlich das HTML-Dokument. Die Länge des kritischen Pfads entsprach einem Netzwerkumlauf, vorausgesetzt, es handelt sich um eine kleine Datei, und die Gesamtzahl der kritischen Bytes belief sich lediglich auf die Transfergröße des HTML-Dokuments selbst.

Now let's compare that to the critical path characteristics of the HTML + CSS example above:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** kritische Ressourcen
* **2** oder mehr Umläufe als minimale Länge des kritischen Pfads
* **9** KB kritische Bytes

Infolgedessen ist unsere optimierte Seite wieder bei zwei kritischen Ressourcen (HTML und CSS) mit einer minimalen Länge des kritischen Pfads von zwei Umläufen und insgesamt 9 KB kritischen Bytes.

Nehmen wir zuletzt an, das CSS-Stylesheet wurde nur zum Drucken benötigt. Wie würde das aussehen?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

Da die Ressource `style.css` nur zum Drucken verwendet wird, muss der Browser diese zum Rendern der Seite nicht blockieren. Somit verfügt der Browser nach Abschluss der DOM-Erstellung über ausreichend Informationen zum Rendern der Seite! Das bedeutet, dass die Seite lediglich eine kritische Ressource (das HTML-Dokument) aufweist und die minimale Länge des kritischen Rendering-Pfads einem Umlauf entspricht.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

That said, in practice if we look at this page's "network waterfall," you'll see that both the CSS and JavaScript requests are initiated at about the same time; the browser gets the HTML, discovers both resources, and initiates both requests. As a result, the above page has the following critical path characteristics:

* **3** kritische Ressourcen
* **2** oder mehr Umläufe als minimale Länge des kritischen Pfads
* **11** KB kritische Bytes

We now have three critical resources that add up to 11KB of critical bytes, but our critical path length is still two roundtrips because we can transfer the CSS and JavaScript in parallel. **Figuring out the characteristics of your critical rendering path means being able to identify the critical resources and also understanding how the browser will schedule their fetches.** Let's continue with our example.

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

An asynchronous script has several advantages:

* Das Skript blockiert den Parser nicht mehr und ist kein Bestandteil des kritischen Rendering-Pfads.
* Da keine anderen kritischen Skripts vorhanden sind, muss das CSS auch nicht das domContentLoaded-Ereignis blockieren.
* Je eher das domContentLoaded-Ereignis ausgelöst wird, desto eher können andere Anwendungslogiken ausgeführt werden.

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