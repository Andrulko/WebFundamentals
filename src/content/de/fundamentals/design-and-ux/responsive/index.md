project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Ein großer Teil des Webs ist nicht für Erfahrungen auf verschiedenen Geräten optimiert. Erlernen Sie die Grundlagen, mit denen Ihre Website auf Mobilgeräten, Desktopcomputern und jeglichen anderen Geräten mit einem Bildschirm funktioniert.

{# wf_updated_on: 2014-04-29 #} {# wf_published_on: 2000-01-01 #}

# Responsives Webdesign: Grundlagen {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Die Nutzung von Mobilgeräten für das Internet steigt auch weiterhin mit ungeheuerlicher Geschwindigkeit, leider ist jedoch ein Großteil des Webs nicht für diese Geräte optimiert. Die Funktionalität von Mobilgeräten ist häufig durch eine geringe Displaygröße eingeschränkt, sodass ein neuer Ansatz bei der Bereitstellung von Inhalten am Bildschirm gefragt ist.

{% include "web/_shared/udacity/ud893.html" %}

<video autoplay muted loop controls>
  <source src="videos/resize.webm" type="video/webm">
  <source src="videos/resize.mp4" type="video/mp4">
</video>

Telefone, Phablets, Tablets, Desktopcomputer, Spielekonsolen, Fernseher und sogar Wearables weisen eine extreme Vielfalt an verschiedenen Bildschirmgrößen auf. Bildschirmgrößen verändern sich ständig, weshalb es besonders wichtig ist, dass sich Ihre Website an diese anpassen kann, ob heute oder in der Zukunft.

Responsives Webdesign, ursprünglich von [Ethan Marcotte in A List Apart](http://alistapart.com/article/responsive-web-design/) definiert, reagiert auf die Bedürfnisse der Nutzer und der Geräte, die sie verwenden. Das Layout verändert sich auf Grundlage der Größe und der Funktionen des Geräts. Wenn Nutzer mit einem Telefon Inhalte zum Beispiel in einer einzelnen Spaltenansicht sehen, könnte der gleiche Inhalt auf einem Tablet in zwei Spalten erscheinen.

## Darstellungsbereich festlegen

Für verschiedene Geräte optimierte Seiten müssen ein Darstellungsbereich-Meta-Element in der Kopfzeile des Dokuments aufweisen. Ein Darstellungsbereich-Meta-Tag gibt dem Browser Anweisungen dazu, wie er Abmessungen und Skalierung der Seite steuern soll.

### TL;DR {: .hide-from-toc }

- Verwenden Sie das Darstellungsbereich-Meta-Tag zur Steuerung der Breite und Skalierung des Darstellungsbereichs im Browser.
- Verwenden Sie `width=device-width` zur Abstimmung auf die Breite des Bildschirms in geräteunabhängigen Pixeln.
- Verwenden Sie `initial-scale=1`, um eine 1:1-Beziehung zwischen CSS-Pixeln und geräteunabhängigen Pixeln zu gewährleisten.
- Stellen Sie sicher, dass Ihre Seite zugänglich ist, indem Sie die Nutzerskalierung beibehalten.

Beim Versuch, die möglichst beste Erfahrung zu gewährleisten, stellen mobile Browser die Seite mit der Breite eines Desktopbildschirms dar, in der Regel etwa 980 Pixel, wobei sich dies von Gerät zu Gerät unterscheiden kann. Anschließend versucht der Browser, die Inhalte optisch ansprechender zu machen, indem er die Schriftgröße erhöht und die Inhalte so skaliert, dass sie den Bildschirm füllen. Für Nutzer bedeutet das, dass die Darstellung der Schriftgrößen inkonsistent ist und sie Inhalte doppelt antippen oder die Finger zusammenziehen müssen, um diese richtig sehen und damit interagieren zu können.

    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    

Wenn Sie den Darstellungsbereich-Meta-Wert `width=device-width` verwenden, geben Sie der Seite die Anweisung, die Breite des Bildschirms in geräteunabhängigen Pixeln zu nutzen. Dies ermöglicht der Seite, Inhalte neu anzuordnen und sich so an verschiedene Bildschirmgrößen anzupassen, egal ob an das kleine Display eines Mobiltelefons oder den großen Bildschirm eines Desktopcomputers.

<div class="attempt-left">
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/vp-no.html">
  <figure>
    <img src="imgs/no-vp.png" srcset="imgs/no-vp.png 1x, imgs/no-vp-2x.png 2x" alt="Page without a viewport set">
    <figcaption>
      Page without a viewport set
     </figcaption>
  </figure>
  </a>
</div>

<div class="attempt-right">
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/vp.html">
  <figure>
    <img src="imgs/vp.png" srcset="imgs/vp.png 1x, imgs/vp-2x.png 2x" alt="Page with a viewport set">
    <figcaption>
      Page with a viewport set
     </figcaption>
  </figure>
  </a>
</div>

Manche Browser behalten die Breite der Seite beim Drehen in das Querformat bei und zoomen, statt die Inhalte zum Füllen des Bildschirms neu anzuordnen. Indem Sie das Attribut initial-scale=1 verwenden, weisen Sie den Browser an, eine 1:1-Beziehung zwischen CSS-Pixeln und geräteunabhängigen Pixeln zu gewährleisten, unabhängig von der Geräteausrichtung. Somit kann die Seite die volle Breite im Querformat nutzen.

Note: Verwenden Sie Kommas zum Trennen von Attributen, damit gewährleistet ist, dass auch ältere Browser diese Attribute richtig verarbeiten können.

### Zugänglichen Darstellungsbereich gewährleisten

Zusätzlich zur Festlegung von `initial-scale` können Sie die folgenden Attribute für den Darstellungsbereich konfigurieren:

- `minimum-scale`
- `maximum-scale`
- `user-scalable`

Wenn festgelegt, können diese verhindern, dass der Nutzer den Darstellungsbereich heranzoomt, wodurch Probleme bei der Zugänglichkeit entstehen können.

## Größe der Inhalte an Darstellungsbereich anpassen

Sowohl auf Desktopcomputern als auch Mobilgeräten sind Nutzer daran gewöhnt, auf Websites vertikal und nicht horizontal zu scrollen. Sie dazu zu zwingen, in der Horizontalen zu scrollen oder herauszoomen zu müssen, um die ganze Seite zu sehen, führt zu einer negativen Nutzererfahrung.

### TL;DR {: .hide-from-toc }

- Verwenden Sie keine großen Elemente mit fester Breite.
- Inhalte sollten für eine gute Darstellung nicht auf eine bestimmte Breite des Darstellungsbereichs ausgerichtet werden.
- Verwenden Sie CSS-Medienabfragen, um verschiedene Stile für große und kleine Bildschirme anzuwenden.

Wenn Sie eine mobile Website mit einem Darstellungsbereich-Meta-Tag entwickeln, ist es sehr gut möglich, dass Sie versehentlich Seiteninhalte erstellen, die nicht gut in den festgelegten Darstellungsbereich passen. So kann etwa ein Bild, das breiter als der Darstellungsbereich dargestellt wird, dazu führen, dass im Darstellungsbereich horizontal gescrollt werden muss. Passen Sie solche Inhalte immer so an, dass sie die Breite des Darstellungsbereichs nicht überschreiten, damit Nutzer nicht horizontal scrollen müssen.

Da sich die Bildschirmabmessungen und die Breite in CSS-Pixeln stark bei Geräten unterscheiden können, etwa zwischen Telefonen und Tablets oder sogar zwischen verschiedenen Telefonen, sollten Inhalte zur guten Darstellung nicht auf einen bestimmen Darstellungsbereich ausgerichtet werden.

Das Festlegen von absoluten CSS-Breiten für Seitenelemente, wie etwa im Beispiel unten, führt dazu, dass der `div`-Bereich für den Darstellungsbereich auf schmaleren Geräten zu breit ausfällt, zum Beispiel bei einer Breite von 320 CSS-Pixeln auf iPhones. Verwenden Sie stattdessen relative Werte für die Breite, zum Beispiel `width: 100%`. Ebenso sollten Sie vorsichtig bei der Verwendung von großen absoluten Positionierungswerten sein, die möglicherweise dazu führen, dass das Element auf kleinen Bildschirmen aus dem Darstellungsbereich fällt.

<div class="attempt-left">
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/vp-fixed.html">
  <figure>
    <img src="imgs/vp-fixed-iph.png" srcset="imgs/vp-fixed-iph.png 1x, imgs/vp-fixed-iph-2x.png 2x" alt="Page with a 344px fixed width element on an iPhone.">
    <figcaption>
      Page with a 344px fixed width element on an iPhone
    </figcaption>
  </figure>
  </a>
</div>

<div class="attempt-right">
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/vp-fixed.html">
  <figure>
    <img src="imgs/vp-fixed-n5.png" srcset="imgs/vp-fixed-n5.png 1x, imgs/vp-fixed-n5-2x.png 2x" alt="Page with a 344px fixed width element on a Nexus 5.">
    <figcaption>
      Page with a 344px fixed width element on a Nexus 5
    </figcaption>
  </figure>
  </a>
</div>

<div class="clearfix"></div>

## CSS-Medienabfragen für Responsivität verwenden

Medienabfragen sind einfache Filter, die auf CSS-Stile angewendet werden können. Sie ermöglichen, Stile ganz einfach auf Grundlage von Charakteristiken des Geräts, das zur Anzeige der Inhalte genutzt wird, zu ändern, einschließlich Anzeigetyp, Breite, Höhe, Ausrichtung und sogar Auflösung.

### TL;DR {: .hide-from-toc }

- Medienabfragen können dazu verwendet werden, Stile auf Grundlage von Gerätecharakteristiken anzuwenden.
- Verwenden Sie `min-width` statt `min-device-width`, um möglichst viele Breiten abzudecken.
- Verwenden Sie relative Größen für Elemente, damit das Layout harmonisch bleibt.

Beispielsweise können Sie alle Stile, die zum Drucken notwendig sind, in eine Druckmedienabfrage einfügen:

    <link rel="stylesheet" href="print.css" media="print">
    

Zusätzlich zur Nutzung des Attributs `media` im Stylesheet-Link gibt es zwei weitere Möglichkeiten, Medienabfragen anzuwenden, die in eine CSS-Datei eingebettet werden können: `@media` und `@import`. Aus Leistungsgründen werden die ersten beiden Methoden statt der `@import`-Syntax empfohlen, siehe [CSS-Importe vermeiden](/web/fundamentals/performance/critical-rendering-path/page-speed-rules-and-recommendations).

    @media print {
      /* print style sheets go here */
    }
    
    @import url(print.css) print;
    

Die Logik, die für Medienabfragen gilt, schließt andere Logiken nicht aus, und der aus der Anwendung von Filtern, die diese Kriterien erfüllen, resultierende CSS-Block wird anhand der CSS-Regeln für die Reihenfolge angewendet.

### Medienabfragen auf Grundlage der Größe des Darstellungsbereichs anwenden

Medienabfragen ermöglichen uns, eine responsive Erfahrung zu schaffen, bei der bestimmte Stile für kleine Bildschirme, große Bildschirme und alle weiteren Möglichkeiten dazwischen angewendet werden. Die Syntax von Medienabfragen erlaubt die Erstellung von Regeln, die abhängig von den Gerätecharakteristiken genutzt werden können.

    @media (query) {
      /* CSS Rules used when query matches */
    }
    

Wir können zwar für viele verschiedene Elemente Abfragen erstellen, diejenigen, die am häufigsten beim responsiven Webdesign zum Einsatz kommen, sind jedoch `min-width`, `max-width`, `min-height` und `max-height`.

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Attribut</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="attribute"><code>min-width</code></td>
      <td data-th="Result">Regeln für alle Browserbreiten angewendet, die über dem in der Abfrage definierten Wert liegen</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>max-width</code></td>
      <td data-th="Result">Regeln für alle Browserbreiten angewendet, die unter dem in der Abfrage definierten Wert liegen</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>min-height</code></td>
      <td data-th="Result">Regeln für alle Browserhöhen angewendet, die über dem in der Abfrage definierten Wert liegen</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>max-height</code></td>
      <td data-th="Result">Regeln für alle Browserhöhen angewendet, die unter dem in der Abfrage definierten Wert liegen</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>orientation=portrait</code></td>
      <td data-th="Result">Regeln für alle Browser angewendet, deren Höhe der Breite entspricht oder größer als diese ist</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>orientation=landscape</code></td>
      <td data-th="Result">Regeln für alle Browser angewendet, deren Breite größer als die Höhe ist</td>
    </tr>
  </tbody>
</table>

Sehen wir uns ein Beispiel an:

<figure>
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/media-queries.html">
    <img src="imgs/mq.png" srcset="imgs/mq.png 1x, imgs/mq-2x.png 2x" alt="Preview of a page using media queries to change properties as it is resized.">
    <figcaption>
      Preview of a page using media queries to change properties as it is resized.
    </figcaption>
  </a>
</figure>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-queries.html" region_tag="mqueries" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/media-queries.html){: target="_blank" .external }

- Wenn der Browser zwischen **0 px** und **640 px** breit ist, kommt `max-640px.css` zum Einsatz.
- Wenn der Browser zwischen **500 px** und **600 px** breit ist, kommt `@media` zum Einsatz.
- Wenn der Browser **640 px breit oder breiter** ist, kommt `min-640px.css` zum Einsatz.
- Wenn der Browser **in der Breite größer ist als in der Höhe**, kommt `landscape.css` zum Einsatz.
- Wenn der Browser **in der Höher größer ist als in der Breite**, kommt `portrait.css` zum Einsatz.

### Ein Hinweis zu `min-device-width`

Es ist auch möglich, Abfragen auf Grundlage von `*-device-width` zu erstellen, von dieser Praxis raten wir jedoch grundsätzlich ab.

Der Unterschied ist zwar klein, jedoch sehr wichtig: `min-width` basiert auf der Größe des Browserfensters, `min-device-width` auf der Bildschirmgröße. Leider können manche Browser, auch ältere Versionen des Android-Browsers, die Breite des Geräts falsch ausgeben. Der Browser meldet die Bildschirmgröße in Gerätepixeln statt der erwarteten Breite des Darstellungsbereichs.

Zudem kann die Verwendung von `*-device-width` verhindern, dass Inhalte richtig für Desktopcomputer oder andere Geräte, auf denen die Größe von Fenstern geändert werden kann, ausgerichtet werden, da die Abfrage auf der tatsächlichen Gerätegröße und nicht auf der Größe des Browserfensters beruht.

### Relative Einheiten verwenden

Ein Schlüsselkonzept von responsivem Design ist Flexibilität und Proportionalität, also das Gegenteil von Layouts mit fester Breite. Die Verwendung relativer Einheiten für Werte kann dazu beitragen, Layouts zu vereinfachen und zu verhindern, dass versehentlich Komponenten erstellt werden, die zu groß für den Darstellungsbereich sind.

### TL;DR {: .hide-from-toc }

So gewährleistet die Verwendung von `width: 100%` für einen `div`-Container auf oberster Ebene, dass er die Breite des Darstellungsbereichs ausfüllt und niemals zu groß oder zu klein wird. Der `div`-Container passt immer, unabhängig davon, ob er auf einem iPhone mit 320 px, einem Blackberry Z10 mit 342 px oder auf Nexus 5 mit 360 px Breite aufgerufen wird.

Hinzu kommt, dass die Nutzung relativer Einheiten Browsern erlaubt, Inhalte auf Grundalge der Zoomstufe des jeweiligen Nutzers darzustellen und das Hinzufügen horizontaler Scrollbalken zur Seite nicht nötig ist.

<span class="compare-worse">Not recommended</span> — fixed width

<span class="compare-better">Recommended</span> — responsive width

    div.fullWidth {
      width: 320px;
      margin-left: auto;
      margin-right: auto;
    }
    

Zwar kann es hilfreich sein, über Übergangspunkte auf Grundlage von Geräteklassen nachzudenken, hierbei ist jedoch Vorsicht geboten. Das Festlegen von Übergangspunkten auf Grundlage von bestimmten Geräten, Produkten, Markennamen oder Betriebssystemen, die heute gängig sind, kann schnell zum Wartungsalbtraum werden. Stattdessen sollte der Inhalt selbst bestimmen, wie sich das Layout an seinen Container anpasst.

    div.fullWidth {
      width: 100%;
    }
    

## Übergangspunkte festlegen

Erstellen Sie Inhalte zunächst so, dass sie auf kleine Bildschirme passen, und erweitern Sie den Bildschirm anschließend, bis ein Übergangspunkt erforderlich wird. So können Sie Übergangspunkte auf Grundlage der Inhalte optimieren und die erforderliche Anzahl dieser Punkte so gering wie möglich halten.

### Bei der Auswahl von primären Übergangspunkten klein beginnen und nach oben arbeiten

- Erstellen Sie Übergangspunkte auf Grundlage der Inhalte und niemals auf Grundlage bestimmter Geräte, Produkte oder Marken.
- Erstellen Sie das Design zuerst für die kleinsten Mobilgeräte und erweitern Sie die Erfahrung anschließend auf den zusätzlichen Platz, der auf größeren Anzeigen verfügbar ist.
- Achten Sie darauf, dass Zeilen immer maximal 70 bis 80 Zeichen enthalten.

### Bei Bedarf sekundäre Übergangspunkte bestimmen

<figure class="attempt-right">
  <img src="imgs/weather-1.png" srcset="imgs/weather-1.png 1x, imgs/weather-1-2x.png 2x" alt="">
  <figcaption>Siehe Beispiel</figcaption>
</figure>

Gehen wir das Beispiel durch, das Sie zu Anfang gesehen haben, die [Wettervorhersage](/web/fundamentals/design-and-ux/responsive/). Im ersten Schritte sorgen wir dafür, dass die Vorhersage auf kleinen Bildschirmen gut aussieht.

Let's work through the example we saw at the beginning: the weather forecast. The first step is to make the forecast look good on a small screen.

<div style="clear:both;"></div>

<figure class="attempt-right">
  <img src="imgs/weather-2.png" class="center" srcset="imgs/weather-2.png 1x, imgs/weather-2-2x.png 2x" alt="Seite mit einem Element mit einer festen Breite von 344 Pixeln auf einem iPhone">
  <figcaption>Seite mit einem Element mit einer festen Breite von 344 Pixeln auf einem iPhone</figcaption>
</figure>

Ändern Sie anschließend die Größe des Browsers, bis zu viel leere Fläche zwischen den Elementen ist und die Vorschau einfach nicht mehr gut aussieht. Diese Entscheidung ist auch eine Geschmacksfrage, ab 600 Pixeln werden die Abstände jedoch definitiv zu groß.

<div style="clear:both;"></div>

To insert a breakpoint at 600px, create two new style sheets, one to use when the browser is 600px and below, and one for when it is wider than 600px.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-2.html" region_tag="mqweather2" adjust_indentation="auto" %}
</pre>

Fügen Sie einen Übergangspunkt bei 600 Pixeln ein, indem Sie zwei neue Stylesheets erstellen. Eines davon kommt zum Einsatz, wenn der Browser 600 Pixel oder weniger in der Breite aufweist, das andere, wenn das Browserfenster über 600 Pixel breit ist.

<figure class="attempt-right">
  <img src="imgs/weather-3.png"  srcset="imgs/weather-3.png 1x, imgs/weather-3-2x.png 2x" alt="Seite mit einem Element mit einer festen Breite von 344 Pixeln auf Nexus 5">
  <figcaption>Seite mit einem Element mit einer festen Breite von 344 Pixeln auf Nexus 5</figcaption>
</figure>

Gestalten Sie die CSS-Datei anschließend um. In diesem Beispiel haben wir die gängigen Stile wie Schriftarten, Symbole, grundlegende Platzierung, Farben usw. in `weather.css` zusammengefasst. Spezielle Layouts für kleine Bildschirme werden anschließend in `weather-small.css`, solche für große Bildschirme in `weather-large.css` platziert.

<div style="clear:both"></div>

### Text zum lesen optimieren

In addition to choosing major breakpoints when layout changes significantly, it is also helpful to adjust for minor changes. For example, between major breakpoints it may be helpful to adjust the margins or padding on an element, or increase the font size to make it feel more natural in the layout.

Zusätzlich zu den primären Übergangspunkten, die zum Einsatz kommen, wenn sich das Layout stark ändert, kann es nützlich sein, Anpassungen für kleinere Veränderungen vorzunehmen. So kann es zwischen primären Übergangspunkten hilfreich sein, die Ränder oder Abstände für einzelne Elemente anzupassen oder die Schriftgröße zu erhöhen, damit der Text im Layout natürlicher wirkt.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-small.css" region_tag="mqsmallbpsm" adjust_indentation="auto" %}
</pre>

<div class="attempt-left">
  <figure>
    <img src="imgs/weather-4-l.png" srcset="imgs/weather-4-l.png 1x, imgs/weather-4-l-2x.png 2x" alt="Before adding minor breakpoints.">
    <figcaption>
      Before adding minor breakpoints.
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img src="imgs/weather-4-r.png" srcset="imgs/weather-4-r.png 1x, imgs/weather-4-r-2x.png 2x" alt="After adding minor breakpoints.">
    <figcaption>
      After adding minor breakpoints.
     </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Beginnen wir mit der Optimierung des Layouts für kleine Bildschirme. Erhöhen wir in diesem Fall die Schriftgröße, wenn die Breite des Darstellungsbereichs 360 Pixel übersteigt. Als Zweites können wir die Höchst- und Mindesttemperatur trennen, wenn genug Platz ist, damit sie in derselben Zeile und nicht übereinander erscheinen. Darüber hinaus können wir die Wettersymbole etwas vergrößern.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-large.css" region_tag="mqsmallbplg" adjust_indentation="auto" %}
</pre>

### Inhalte niemals vollständig ausblenden

Classic readability theory suggests that an ideal column should contain 70 to 80 characters per line (about 8 to 10 words in English). Thus, each time the width of a text block grows past about 10 words, consider adding a breakpoint.

<div class="attempt-left">
  <figure>
    <img src="imgs/reading-ph.png" srcset="imgs/reading-ph.png 1x, imgs/reading-ph-2x.png 2x" alt="Before adding minor breakpoints.">
    <figcaption>Before adding minor breakpoints.</figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img src="imgs/reading-de.png" srcset="imgs/reading-de.png 1x, imgs/reading-de-2x.png 2x" alt="After adding minor breakpoints.">
    <figcaption>After adding minor breakpoints.</figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Für große Bildschirme sollte zudem die maximale Breite des Vorhersagefensters begrenzt werden, damit es nicht die gesamte Breite ausfüllt.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/reading.html" region_tag="mqreading" adjust_indentation="auto" %}
</pre>

Die klassische Theorie zur Lesbarkeit besagt, dass die ideale Spalte 70 bis 80 Zeichen pro Zeile enthalten sollte, was etwa sieben Wörtern in der deutschen Sprache entspricht. Daher sollte immer dann, wenn die Breite eines Textbausteins sieben Wörter übersteigt, ein Übergangspunkt in Betracht gezogen werden.

### Never completely hide content

Be careful when choosing what content to hide or show depending on screen size. Don't simply hide content just because you can't fit it on the screen. Screen size is not a definitive indication of what a user may want. For example, eliminating the pollen count from the weather forecast could be a serious issue for spring-time allergy sufferers who need the information to determine if they can go outside or not.

## View media query breakpoints in Chrome DevTools {: #devtools }

Once you've got your media query breakpoints set up, you'll want to see how your site looks with them. You *could* resize your browser window to trigger the breakpoints, but there's a better way: Chrome DevTools. The two screenshots below demonstrate using DevTools to view how a page looks under different breakpoints.

![Example of DevTools' media queries feature](imgs/devtools-media-queries-example.png)

Lassen Sie bei der Entscheidung, welche Inhalte auf Grundlage der Bildschirmgröße ein- oder ausgeblendet werden sollen, Vorsicht walten. Lassen Sie Inhalte nicht einfach verschwinden, wenn sie nicht auf den Bildschirm passen. Die Bildschirmgröße ist kein zuverlässiger Indikator dafür, was der Nutzer möglicherweise sehen möchte. Beispielsweise wäre es für Frühjahrsallergiker, die wissen möchten, ob sie ins Freie gehen können oder nicht, ein ernstes Problem, wenn der Pollenflug nicht in der Wettervorhersage erscheinen würde.

[Open DevTools](/web/tools/chrome-devtools/#open) and then turn on [Device Mode](/web/tools/chrome-devtools/device-mode/#toggle).

Use the [viewport controls](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#viewport-controls) to select **Responsive**, which puts DevTools into responsive mode.

Last, open the Device Mode menu and select [**Show media queries**](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#media-queries) to display your breakpoints as colored bars above your page.

Click on one of the bars to view your page while that media query is active. Right-click on a bar to jump to the media query's definition. See [Media queries](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#media-queries) for more help.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}