project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Bevor der Browser Inhalte auf dem Bildschirm darstellen kann, müssen die DOM- und CSSOM-Baumstrukturen erstellt werden. Deshalb sind sowohl die HTML- als auch die CSS-Elemente dem Browser unverzüglich zur Verfügung zu stellen.

{# wf_updated_on: 2014-09-11 #} {# wf_published_on: 2014-03-31 #}

# Objektmodell erstellen {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Bevor der Browser die Seite darstellen kann, müssen die DOM- und CSSOM-Baumstrukturen erstellt werden. Deshalb sind sowohl die HTML- als auch die CSS-Elemente dem Browser unverzüglich zur Verfügung zu stellen.

### TL;DR {: .hide-from-toc }

- Bytes → Zeichen → Token → Knoten → Objektmodell
- Das HTML-Markup wird in ein Document Object Model (DOM), das CSS-Markup in ein CSS Object Model (CSSOM) umgewandelt.
- DOM und CSSOM sind unabhängige Datenstrukturen.
- Chrome DevTools Timeline ermöglicht die Erfassung und Kontrolle der Erstellungs- und Verarbeitungskosten von DOM und CSSOM.

## Document Object Model (DOM)

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Beginnen wir mit dem einfachsten Fall: eine reine HTML-Seite mit Text und einem Bild. Was benötigt der Browser, um diese einfache Seite zu verarbeiten?

Let’s start with the simplest possible case: a plain HTML page with some text and a single image. How does the browser process this page?

<img src="images/full-process.png" alt="DOM-Baumstruktur" />

1. **Konvertierung:** Der Browser liest die Rohbytes des HTML-Codes von der Festplatte oder aus dem Netzwerk ein und übersetzt diese basierend auf der angegebenen Dateicodierung, z. B. UTF-8, in einzelne Zeichen.
2. **Tokenisierung:** Der Browser konvertiert Zeichenfolgen in eindeutige Token, die vom [W3C HTML5-Standard](http://www.w3.org/TR/html5/) vorgegeben sind, z. B. `<html>`, `<body>` und andere Strings in `spitzen Klammern`. Jedes Token hat eine spezielle Bedeutung und mehrere Regeln.
3. **Lexing:** Die ausgegebenen Token werden in Objekte umgewandelt, die ihre Eigenschaften und Regeln festlegen.
4. **DOM-Erstellung:** Weil das HTML-Markup Beziehungen zwischen unterschiedlichen Tags definiert (manche Tags sind in anderen Tags enthalten), werden die erstellten Objekte in einer Baumstruktur verknüpft, die die hierarchischen Beziehungen berücksichtigt, die im ursprünglichen Markup vorgegeben sind: *HTML* object ist *body* object übergeordnet, *body* ist *paragraph* object übergeordnet und so weiter.

<img src="images/dom-tree.png"  alt="DOM tree" />

**The final output of this entire process is the Document Object Model (DOM) of our simple page, which the browser uses for all further processing of the page.**

Every time the browser processes HTML markup, it goes through all of the steps above: convert bytes to characters, identify tokens, convert tokens to nodes, and build the DOM tree. This entire process can take some time, especially if we have a large amount of HTML to process.

<img src="images/dom-timeline.png"  alt="Tracing DOM construction in DevTools" />

Wenn Sie Chrome DevTools öffnen und eine Zeitleiste aufzeichnen, während eine Seite geladen wird, können Sie die Zeit sehen, die für die Durchführung dieses Schritts benötigt wird. Im obigen Beispiel dauerte es circa 5 ms, um eine Anzahl von HTML-Bytes in eine DOM-Baumstruktur umzuwandeln. Wenn die Seite größer ist, was in der Regel zutrifft, kann dieser Vorgang erheblich länger dauern. In den nächsten Abschnitten über die Erstellung flüssiger Animationen werden Sie feststellen, wie leicht die Verarbeitung großer HTML-Mengen durch den Browser zu Engpässen führen kann.

Verfügen wir nach Fertigstellung der DOM-Baumstruktur über genügend Informationen, um die Seite auf dem Bildschirm darzustellen? Noch nicht! Die DOM-Baumstruktur enthält zwar die Eigenschaften und Beziehungen des Dokumenten-Markups, sagt jedoch nichts darüber aus, wie das Element auf dem Bildschirm aussehen soll. Das ist die Aufgabe des CSSOM, das wir als Nächstes in Angriff nehmen!

Bei der Erstellung des DOM unserer einfachen Seite im Browser wurde ein Link-Tag im Kopfteil des Dokuments festgestellt, das auf ein externes CSS-Stylesheet verwies: style.css. In der Annahme, dass diese Ressource zur Darstellung der Seite benötigt wird, wurde diese Ressource umgehend angefordert und mit dem folgenden Inhalt zurückgesendet:

## CSS Object Model (CSSOM)

Natürlich hätten wir unsere Styles direkt im HTML-Markup (inline) deklarieren können, aber wenn unser CSS unabhängig von HTML bleibt, ist es möglich, die Inhalte und das Layout getrennt zu behandeln und die Grafiker können am CSS arbeiten, die Entwickler sich auf HTML konzentrieren und so weiter.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/style.css" region_tag="full" adjust_indentation="auto" %}
</pre>

Wie bei HTML müssen die empfangenen CSS-Regeln in ein Format umgewandelt werden, das im Browser verarbeitet werden kann. Der Prozess ähnelt sehr der Vorgehensweise bei HTML:

As with HTML, we need to convert the received CSS rules into something that the browser can understand and work with. Hence, we repeat the HTML process, but for CSS instead of HTML:

<img src="images/cssom-construction.png"  alt="CSSOM construction steps" />

The CSS bytes are converted into characters, then tokens, then nodes, and finally they are linked into a tree structure known as the "CSS Object Model" (CSSOM):

<img src="images/cssom-tree.png"  alt="CSSOM tree" />

Betrachten Sie zur Verdeutlichung die obige CSSOM-Baumstruktur. Sämtlicher Text innerhalb von *span* tag, der im body-Element platziert wird, besitzt eine Schriftgröße von 16 Pixeln und ist rot, weil die Anweisung für die Schriftgröße für den Textkörper (body) und somit auch für das nachrangige span-Tag gilt. Wenn ein span-Tag jedoch einem Absatz-Tag (p) untergeordnet ist, dann wird sein Inhalt nicht angezeigt.

Beachten Sie auch, dass die obige CSSOM-Baumstruktur nicht vollständig ist und nur die Styles aufweist, die wir in unserem Stylesheet überschreiben wollten. Jeder Browser stellt eine Reihe von Standard-Styles bereit, die als `User Agent Styles` bezeichnet und dargestellt werden, wenn wir keine eigenen vorgeben. Mit unseren Styles werden diese Standard-Styles, z. B. [Standard-IE-Styles](http://www.iecss.com/) überschrieben. Wenn Sie sich jemals die `computed Styles` in Chrome DevTools angesehen und sich gewundert haben, wo all diese Styles herkommen, wissen Sie jetzt Bescheid!

Sind Sie neugierig, wie lange die CSS-Verarbeitung gedauert hat? Zeichnen Sie eine Zeitleiste in DevTools auf und suchen Sie das Ereignis `Recalculate Style` (Style neu berechnen): Im Gegensatz zum DOM-Parsing enthält die Zeitleiste keinen Eintrag `Parse CSS` (CSS parsen) und erfasst stattdessen das Parsing und die Erstellung der CSSOM-Baumstruktur sowie die rekursive Berechnung der `computed` (berechneten) Styles im Rahmen dieses einen Ereignisses.

To find out how long the CSS processing takes you can record a timeline in DevTools and look for "Recalculate Style" event: unlike DOM parsing, the timeline doesn’t show a separate "Parse CSS" entry, and instead captures parsing and CSSOM tree construction, plus the recursive calculation of computed styles under this one event.

<img src="images/cssom-timeline.png"  alt="Tracing CSSOM construction in DevTools" />

Our trivial stylesheet takes ~0.6ms to process and affects eight elements on the page&mdash;not much, but once again, not free. However, where did the eight elements come from? The CSSOM and DOM are independent data structures! Turns out, the browser is hiding an important step. Next, lets talk about the [render tree](/web/fundamentals/performance/critical-rendering-path/render-tree-construction) that links the DOM and CSSOM together.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}