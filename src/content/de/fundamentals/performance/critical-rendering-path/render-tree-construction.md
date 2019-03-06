project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Die CSSOM- und DOM-Baumstrukturen werden in einer Rendering-Baumstruktur zusammengefasst, die dann zur Berechnung des Layouts eines jeden sichtbaren Elements verwendet wird und als Eingabe für den Paint-Prozess dient, der die Pixel auf dem Bildschirm darstellt. Die Optimierung jedes einzelnen dieser Schritte ist für die bestmögliche Rendering-Leistung entscheidend.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Erstellung der Rendering-Baumstruktur, Layout, und Paint {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Die CSSOM- und DOM-Baumstrukturen werden in einer Rendering-Baumstruktur zusammengefasst, die dann zur Berechnung des Layouts eines jeden sichtbaren Elements verwendet wird und als Eingabe für den Paint-Prozess dient, der die Pixel auf dem Bildschirm darstellt. Die Optimierung jedes einzelnen dieser Schritte ist für die bestmögliche Rendering-Leistung entscheidend.

Im vorherigen Abschnitt über die Erstellung des Objektmodells erstellten wir die DOM- und CSSOM-Baumstrukturen basierend auf den HTML- und CSS-Eingaben. Allerdings handelt es sich bei letzteren um unabhängige Objekte, die verschiedene Aspekte des Dokuments abdecken: eines beschreibt den Inhalt und das andere die Formatierungsregeln, die auf das Dokument anzuwenden sind. Wie werden beide Objekttypen zusammengeführt und bewirken, dass der Browser Pixel auf dem Bildschirm darstellt?

### TL;DR {: .hide-from-toc }

* Die DOM- und CSSOM-Baumstrukturen bilden gemeinsam die Rendering-Baumstruktur.
* Die Rendering-Baumstruktur enthält nur die Knoten, für das Rendern der Seite erforderlich sind.
* Beim Layout werden die exakte Position und Größe eines jeden Objekts berechnet.
* Beim Paint als letztem Schritt werden die Pixel auf dem Bildschirm basierend auf der finalen Rendering-Baumstruktur dargestellt.

Zunächst werden im Browser die DOM- und CSSOM-Elemente in einer Rendering-Baumstruktur zusammengefasst, die alle sichtbaren DOM-Inhalte auf der Seite und zusätzlich alle CSSOM-Formatinformationen für jeden Knoten abdeckt.

<img src="images/render-tree-construction.png" alt="Die DOM- und CSSOM-Elemente werden in der Rendering-Baumstruktur zusammengefasst." />

Bei der Erstellung der Rendering-Baumstruktur geht der Browser grob wie folgt vor:

1. Starting at the root of the DOM tree, traverse each visible node.
    
    * Manche Knoten sind überhaupt nicht sichtbar, z. B. Skript-Tags, Metatags usw., und andere werden weggelassen, weil sie in der gerenderten Ausgabe nicht erscheinen.
    * Manche Knoten sind per CSS ausgeblendet und erscheinen ebenfalls nicht in der Rendering-Baumstruktur. So fehlt hier beispielsweise der span-Knoten aus dem obigen Beispiel, da eine explizite Regel dafür die Eigenschaft `display: none` vorgibt.

2. For each visible node, find the appropriate matching CSSOM rules and apply them.

3. Die sichtbaren Knoten werden mit ihrem Inhalt und ihren berechneten Styles ausgegeben.

Note: Wir weisen darauf hin, dass `visibility: hidden` sich von `display: none` unterscheidet. Ersteres macht das Element unsichtbar, aber das Element belegt weiterhin Platz im Layout, d. h., es wird als leeres Feld wiedergegeben, während letzteres das Element vollständig aus der Rendering-Baumstruktur entfernt, sodass das Element unsichtbar und nicht mehr Teil des Layouts ist.

Das finale Rendering enthält sowohl die Inhalte als auch die Formatinformationen aller sichtbaren Inhalte auf dem Bildschirm - wir haben es fast geschafft! **Da die Rendering-Baumstruktur nun eingerichtet ist, können wir mit der `Layout`-Phase fortfahren.**

Bis zu diesem Punkt haben wir analysiert, welche Knoten sichtbar sein sollten und ihre Formatierung umgesetzt, allerdings haben wir ihre exakte Position und Größe im [Anzeigebereich](/web/fundamentals/design-and-ux/responsive/#set-the-viewport) des Geräts nicht berechnet - dies geschieht in der Layout-Phase, die gelegentlich auch als `Reflow` bezeichnet wird.

Mit der Ermittlung der exakten Größe und Position der einzelnen Objekte beginnt der Browser im Stammverzeichnis der Rendering-Baumstruktur und arbeitet diese ab, um die Geometrie eines jeden Objekts auf der Seite zu berechnen. Sehen wir uns ein einfaches praktisches Beispiel an:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/nested.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Im Textkörper (body) der obigen Seite sind zwei verschachtelte div-Einträge vorhanden: Das erste (übergeordnete) div-Element legt die Anzeigegröße des Knotens auf 50 % der Breite des Anzeigebereichs fest und mit dem zweiten div-Element innerhalb des übergeordneten Elements wird dessen Breite auf 50 % des übergeordneten Elements gesetzt, d. h. auf 25 % der Breite des Anzeigebereichs!

The body of the above page contains two nested div's: the first (parent) div sets the display size of the node to 50% of the viewport width, and the second div\---contained by the parent\---sets its width to be 50% of its parent; that is, 25% of the viewport width.

<img src="images/layout-viewport.png" alt="Calculating layout information" />

Da wir nun wissen, welche Knoten sichtbar sind und deren berechnete Styles und ihre Geometrie kennen, können wir diese Informationen jetzt in der finalen Phase nutzen, wo jeder Knoten in der Rendering-Baumstruktur in reale Pixel auf dem Bildschirm konvertiert wird - dieser Schritt wird häufig als `Painting` oder `Rastern` bezeichnet.

Konnten Sie bisher allem folgen? Jeder dieser Schritte bedeutet für den Browser einen nicht unerheblichen Arbeitsaufwand, der häufig längere Zeit in Anspruch nimmt. Glücklicherweise können wir mit Chrome DevTools Einblicke in alle drei oben beschriebenen Phasen gewinnen. Wir wollen nun die Layoutphase für unser ursprüngliches `Hallo Welt`-Beispiel untersuchen:

This can take some time because the browser has to do quite a bit of work. However, Chrome DevTools can provide some insight into all three of the stages described above. Let's examine the layout stage for our original "hello world" example:

<img src="images/layout-timeline.png" alt="Measuring layout in DevTools" />

* Die Erstellung der Rendering-Baumstruktur und die Berechnung der Position und Größe werden mit dem Ereignis `Layout` in der Zeitleiste (Timeline) erfasst.
* Nach Fertigstellung des Layouts gibt der Browser die Ereignisse `Paint Setup` und `Paint` aus, mit denen die Rendering-Baumstruktur in reale Pixel auf dem Bildschirm umgewandelt wird.

Wir haben es geschafft: Unsere Seite wird im Anzeigebereich dargestellt!

The page is finally visible in the viewport:

<img src="images/device-dom-small.png" alt="Rendered Hello World page" />

Unsere Demo-Seite sieht vielleicht sehr simpel aus, aber es steckt jede Menge Arbeit darin! Können Sie sich vorstellen, was es bedeutet, wenn die DOM- oder CSSOM-Elemente verändert werden? Derselbe Prozess müsste wiederholt werden, um festzustellen, welche Pixel erneut auf dem Bildschirm darzustellen sind.

1. HTML-Markup verarbeiten und DOM-Baumstruktur erstellen.
2. CSS-Markup verarbeiten und CSSOM-Baumstruktur erstellen.
3. DOM und CSSOM in eine Rendering-Baumstruktur zusammenführen.
4. Das Layout für die Rendering-Baumstruktur ausführen, um die Geometrie der einzelnen Knoten zu ermitteln.
5. Die einzelnen Knoten auf dem Bildschirm darstellen.

**Der kritische Rendering-Pfad wird über die Minimierung der Gesamtdauer der Schritte 1 bis 5 in der obigen Abfolge optimiert.** Auf diese Weise können wir die Inhalte so schnell wie möglich auf dem Bildschirm darstellen und ebenso die Zeit zwischen Bildschirmaktualisierungen nach dem ersten Rendern verkürzen, d. h., wir erzielen eine höhere Aktualisierungsrate für interaktive Inhalte.

***Optimizing the critical rendering path* is the process of minimizing the total amount of time spent performing steps 1 through 5 in the above sequence.** Doing so renders content to the screen as quickly as possible and also reduces the amount of time between screen updates after the initial render; that is, achieve higher refresh rates for interactive content.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}