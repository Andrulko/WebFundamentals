project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Heute kann über eine extreme Vielfalt an Geräten auf das Web zugegriffen werden, von Telefonen mit sehr kleinen Displays bis hin zu Fernsehern mit riesigen Bildschirmdiagonalen. Erfahren Sie, wie Sie eine Website erstellen, die auf allen Geräten gut funktioniert.

{# wf_updated_on: 2014-01-05 #} {# wf_published_on: 2013-12-31 #}

# Ihre erste Website für verschiedene Geräte {: .page-title }

Caution: This article has not been updated in a while and may not reflect reality. Instead, check out the free [Responsive Web Design](https://www.udacity.com/course/responsive-web-design-fundamentals--ud893) course on Udacity.

{% include "web/_shared/contributors/paulkinlan.html" %}

<img src="images/finaloutput-2x.jpg" alt="many devices showing the final project" class="attempt-right" />

Creating multi-device experiences is not as hard as it might seem. In this guide, we will build a product landing page for the [CS256 Mobile Web Development course](https://www.udacity.com/course/mobile-web-development--cs256) that works well across different device types.

Das Erstellen von Websites für verschiedene Geräte mit unterschiedlichem Funktionsumfang und stark abweichender Bildschirmgröße kann als große Herausforderung oder gar unmöglich erscheinen.

Vollständig responsive Websites zu erstellen ist nicht so schwer, wie Sie vielleicht denken. Dieser Leitfaden führt Sie durch die ersten Schritte. Wir haben den Einstieg in zwei einfache Schritte unterteilt:

1. die Informationsarchitektur, oft als IA bezeichnet, und die Struktur der Seite definieren und 2. Designelemente hinzufügen, um sie auf allen Geräten responsiv und ansprechend zu machen.
2. Adding design elements to make it responsive and look good across all devices.

## Inhalte und Struktur erstellen

Inhalte sind der wichtigste Aspekt jeder Website. Erstellen wir das Design für die Inhalte und lassen wir das Design nicht die Inhalte bestimmen. In diesem Leitfaden ermitteln wir zunächst die Inhalte, die wir benötigen, anschließend erarbeiten wir eine Seitenstruktur auf Grundlage dieser Inhalte und präsentieren die Seite schließlich in einem einfachen linearen Layout, das sowohl mit schmalen als auch breiten Darstellungsbereichen gut funktioniert.

### Seitenstruktur erstellen

Wir kommen zu dem Schluss, dass wir die folgenden Inhalte benötigen:

1. einen Bereich, in dem eine allgemeine Beschreibung unseres Produkts verfügbar ist, Kurs `CS256: Für das mobile Web entwickeln`,
2. ein Formular, über das Informationen zu Nutzern erfasst werden können, die an unserem Produkt interessiert sind,
3. eine ausführliche Beschreibung und ein Video,
4. Bilder des Produkts in Aktion und
5. eine Datentabelle mit Informationen zur Untermauerung unserer Behauptungen.

#### Titel und Formular erstellen

* Inhalte ermitteln, die Sie zuerst benötigen
* Informationsarchitektur(IA)-Entwurf für schmale und breite Darstellungsbereiche erarbeiten
* Seitenstrukturansicht mit Inhalten und ohne Stile erstellen

Darüber hinaus haben wir eine Rohversion der Informationsarchitektur und ein grobes Layout für schmale und breite Darstellungsbereiche ausgearbeitet.

<div class="attempt-left">
  <figure>
    <img src="images/narrowviewport.png" alt="Narrow Viewport IA">
    <figcaption>
      Narrow Viewport IA
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img src="images/wideviewport.png" alt="Wide Viewport IA">
    <figcaption>
      Wide Viewport IA
     </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

This can be converted easily into the rough sections of a skeleton page that we will use for the rest of this project.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addstructure.html" region_tag="structure" adjust_indentation="auto" %}
</pre>

Diese können einfach in die vorläufigen Abschnitte einer Seitenstruktur ohne Stile umgewandelt werden, auf die wir im weiteren Verlauf dieses Projekts zurückgreifen werden.

### TL;DR {: .hide-from-toc }

Die Grundstruktur der Website ist fertig. Wir wissen, welche Abschnitte wir brauchen, welche Inhalte in diesen Abschnitten erscheinen und wo sie in der gesamten Informationsarchitektur positioniert werden sollen. Nun können wir mit der Erweiterung der Website beginnen.

Note: Styling kommt später

### Der Seite Inhalte hinzufügen

Der Titel und das Anfragebenachrichtigungsformular sind die kritischen Komponenten unserer Seite. Diese müssen dem Nutzer umgehend angezeigt werden.

Geben Sie als Titel einen einfachen Text zur Beschreibung des Kurses ein:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addheadline.html" region_tag="headline" adjust_indentation="auto" %}
</pre>

Darüber hinaus müssen wir das Formular ausfüllen. Das Formular soll einfach sein und die Namen und Telefonnummern der Nutzer und einen geeigneten Zeitpunkt zum Rückruf erfassen.

Alle Formulare sollten über Beschriftungen und Platzhalter verfügen, damit Nutzer Elemente einfach erkennen, sofort verstehen, was eingetragen werden soll und Eingabehilfetools die Struktur des Formulars erfassen können. Das Namensattribut sendet den Formularwert nicht nur an den Server: Es wird auch dazu verwendet, dem Browser wichtige Signale darüber zu geben, wie das Formular für den Nutzer automatisch ausgefüllt werden kann.

Wir fügen semantische Typen hinzu, damit Nutzer Angaben auf Mobilgeräten schnell und einfach eingeben können. Beispielsweise sollte dem Nutzer beim Eingeben einer Telefonnummer lediglich eine Wähltastatur angezeigt werden.

Der Video- und Informationsabschnitt der Inhalte ist etwas ausführlicher. Er enthält eine Liste der Funktionen unserer Produkte und darüber hinaus einen Videoplatzhalter, in dem Nutzern die Funktionsweise unseres Produkts vorgeführt wird.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addform.html" region_tag="form" adjust_indentation="auto" %}
</pre>

Videos kommen oft zum Einsatz, um Inhalte auf eine interaktivere Art zu beschreiben. Meistens finden Sie Verwendung, um eine Demonstration eines Produkts oder Konzepts zu geben.

#### Video- und Informationsabschnitt erstellen

Orientieren Sie sich an den Best Practices, um Videos einfach in Ihre Website zu integrieren:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section1" adjust_indentation="auto" %}
</pre>

Websites ohne Bilder können etwas langweilig sein. Es gibt zwei Bildtypen:

Im Bildabschnitt unserer Seite befindet sich eine Anzahl an Bildern zum Inhalt.

Bilder zum Inhalt sind sehr wichtig, um die Bedeutung der Seite zu vermitteln. Stellen Sie sich diese wie die Bilder in Zeitungsartikeln vor. Die Bilder, die wir verwenden, sind Bilder der Lehrer dieses Projekts: Chris Wilson, Peter Lubbers und Sean Bennet.

* Fügen Sie ein `controls`-Attribut hinzu, mit dem sich Nutzer das Video einfach ansehen können.
* Fügen Sie ein `poster`-Bild hinzu, um Nutzern eine Vorschau des Inhalts zu geben.
* Add multiple `<source>` elements based on supported video formats.
* Fügen Sie Hinweistext hinzu, über den Nutzer das Video herunterladen können, falls die Wiedergabe im Fenster nicht möglich ist.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addvideo.html" region_tag="video" adjust_indentation="auto" %}
</pre>

Die Bilder sind so eingestellt, dass sie sich über 100 % der Bildschirmbreite erstrecken. Dieser Ansatz funktioniert gut auf Geräten, die einen schmalen Darstellungsbereich haben, allerdings weniger gut auf solchen mit einem breiten Darstellungsbereich, etwa auf Desktopcomputern. Dieses Problem wird im Abschnitt für responsives Design behandelt.

#### Bildabschnitt erstellen

Viele Nutzer können sich Bilder nicht ansehen und greifen auf Hilfstechnologien wie eine Bildschirmsprachausgabe zurück, die die Daten auf der Seite analysieren und anschließend in verbaler Form ausgeben. Vergewissern Sie sich daher, dass alle Ihre Bilder zum Inhalt ein beschreibendes `alt`-Tag aufweisen, die die Sprachausgabe dem Nutzer vorlesen kann.

* Bilder zum Inhalt: Bilder, die sich im Text im Dokument befinden und dazu dienen, zusätzliche Informationen zum Inhalt zu vermitteln.
* Stilistische Bilder: Bilder, die dazu dienen, das Erscheinungsbild zu verbessern. Solche Bilder sind häufig Hintergrundbilder, Muster und Farbverläufe. Weitere Informationen hierzu finden Sie im [nächsten Artikel](#).

Achten Sie beim Hinzufügen von `alt`-Tags darauf, den Text so prägnant wie möglich zu halten und das Bild vollständig zu beschreiben. In unserer Demonstration formatieren wir das Attribut zum Beispiel einfach als `Name: Rolle`. Diese Informationen reichen aus, dass der Nutzer versteht, dass sich dieser Abschnitt um die Autoren und ihre Aufgaben dreht.

Der letzte Abschnitt ist eine einfache Tabelle mit bestimmten Produktdaten.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addimages.html" region_tag="images" adjust_indentation="auto" %}
</pre>

Tabellen sollten nur für Tabellendaten verwendet werden, also Informationsmatrizen.

Für die meisten Websites ist eine Fußnote erforderlich, in der sich Inhalte wie Nutzungsbedingungen, Haftungsausschlüsse und sonstige Informationen befinden, die nicht in der Hauptnavigation oder im Hauptinhaltsbereich der Seite erscheinen sollen.

Auf unserer Website verlinken wir einfach auf die Nutzungsbedingungen, eine Kontaktseite und unsere Profile in den sozialen Medien.

Wir haben einen Entwurf der Website ausgearbeitet und die wichtigsten strukturellen Elemente ermittelt. Darüber hinaus haben wir uns vergewissert, dass alle relevanten Inhalte fertig sind und für unsere geschäftlichen Anforderungen bereitstehen.

#### Abschnitt mit Datentabelle hinzufügen

The final section is a simple table that is used to show specific product stats about the product.

Sie werden nun vielleicht denken, dass die Seite gar nicht gut aussieht. Das ist jedoch Absicht. Inhalte sind der wichtigste Aspekt jeder Website und wir mussten sichergehen, dass wir eine gute und solide Informationsarchitektur und Informationsdichte haben. Anhand dieses Leitfadens haben wir uns eine ausgezeichnete Grundlage geschaffen, auf der wir nun aufbauen können. Die Formatierung unserer Inhalte wird im nächsten Leitfaden behandelt.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section3" adjust_indentation="auto" %}
</pre>

Heute kann über eine extreme Vielfalt an Geräten auf das Web zugegriffen werden, von Telefonen mit sehr kleinen Bildschirmen bis hin zu Fernsehern mit riesigen Bildschirmdiagonalen. Jedes dieser Geräte bringt eigene Vorteile, jedoch auch Einschränkungen mit sich. Als Webentwickler wird von Ihnen erwartet, sämtliche Geräte zu unterstützen.

#### Fußnote hinzufügen

Wir erstellen eine Website, die auf Bildschirmen verschiedener Größe und verschiedenen Gerätetypen funktioniert. Im [vorherigen Artikel](#) haben wir die Informationsarchitektur der Seite entworfen und eine Grundstruktur erstellt. In diesem Leitfaden nehmen wir unsere Grundstruktur mit Inhalten und verwandeln diese in eine schöne Seite, die auf einer breiten Palette an Bildschirmgrößen responsiv ist.

Gemäß den Prinzipien der "Mobile First"-Webentwicklung, wonach zuerst für Mobilgeräte entwickelt wird, beginnen wir mit einem schmalen Darstellungsbereich, ähnlich dem Bildschirm eines Mobiltelefons. Anschließend skalieren wir für größere Geräteklassen nach oben. Dazu verbreitern wir den Darstellungsbereich und überprüfen, ob Design und Layout anschließend noch passen.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="footer" adjust_indentation="auto" %}
</pre>

Wir haben zuvor verschiedene allgemeine Designs dazu erstellt, wie unsere Inhalte dargestellt werden sollen. Nun müssen wir dafür sorgen, dass sich unsere Seite an diese verschiedenen Layouts anpasst. Dazu müssen wir uns auf Grundlage dessen, wie die Inhalte auf den jeweiligen Bildschirm passen, entscheiden, wo unsere Übergangspunkte liegen sollen. An diesen Punkten ändern sich Layout und Stile.

### Zusammenfassung

Selbst bei sehr einfachen Seiten ist es *obligatorisch*, ein Darstellungsbereich-Meta-Tag einzufügen. Der Darstellungsbereich ist die wichtigste Komponente zur Realisierung geeigneter Mehr-Geräte-Erlebnisse. Ohne ihn funktioniert Ihre Website auf Mobilgeräten nicht besonders gut.

<div class="attempt-left">
  <figure>
    <img src="images/content.png" alt="Content">
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-without-styles.html">Content and structure</a>
    </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img  src="images/narrowsite.png" alt="Designed site" style="max-width: 100%;">
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-with-styles.html">Final site</a>
    </figcaption>
  </figure>
</div>

Der Darstellungsbereich signalisiert dem Browser, dass die Seite skaliert werden muss, damit sie auf den Bildschirm passt. Es gibt viele verschiedene Konfigurationen, die Sie für Ihren Darstellungsbereich zur Steuerung der Seitendarstellung festlegen können. Wir empfehlen folgenden Standard:

## Responsiv machen

Der Darstellungsbereich befindet sich in der Kopfzeile des Dokuments und muss nur einmal deklariert werden.

Unser Produkt und unser Unternehmen haben bereits ein ganz bestimmtes Branding und Richtlinien für die Schriftart in einem Styleguide.

Ein Styleguide stellt eine gute Möglichkeit dar, ein umfassendes Verständnis für die visuelle Darstellung der Seite zu bekommen, und hilft dabei, ein konsistentes Design auf die Beine zu stellen.

Im vorherigen Leitfaden haben wir inhaltliche Bilder hinzugefügt, die wichtig für die Beschreibung unseres Produkts waren. Ansprechende Bilder sind Bilder, die zwar nichts zu den Kerninhalten beitragen, jedoch visuelle Eleganz verleihen oder dabei helfen, die Aufmerksamkeit des Nutzers auf bestimmte Inhalte zu lenken.

### TL;DR {: .hide-from-toc }

* Immer einen Darstellungsbereich verwenden
* Immer mit einem schmalen Darstellungsbereich beginnen und nach oben skalieren
* Übergangspunkte außerhalb platzieren, wenn Inhalte angepasst werden müssen
* Allgemeinen Entwurf Ihres Layouts über alle primären Übergangspunkte hinweg erstellen

### Darstellungsbereich einfügen

Ein gutes Beispiel hierfür ist ein Titelbild für den Inhalt, der ohne Scrollen sichtbar ist. Dieses wird häufig dazu genutzt, Nutzer dafür zu gewinnen, mehr über das Produkt zu lesen.

The viewport indicates to the browser that the page needs to be scaled to fit the screen. There are many different configurations that you can specify for your viewport to control the display of the page. As a default, we recommend:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/viewport.html" region_tag="viewport" adjust_indentation="auto" %}
</pre>

Solche Bilder können sehr einfach hinzugefügt werden. In unserem Fall fügen wir eines als Hintergrund für die Kopfzeile ein und wenden es mithilfe von ein wenig einfachem CSS-Code an.

Wir haben ein einfaches Hintergrundbild ausgewählt, das unscharf ist, damit es nicht vom Inhalt ablenkt. Wir haben es so konfiguriert, dass es sich stets über das gesamte Element erstreckt und dabei immer das richtige Seitenverhältnis aufweist.

### Einfaches Styling anwenden

Das Design sieht ab 600 Pixeln in der Breite unvorteilhaft aus. In unserem Fall übersteigt die Länge einer Zeile sieben Wörter - die optimale Länge zum Lesen - und hier möchten wir es ändern.

#### Styleguide

600 Pixel stellen eine gute Stelle für den ersten Übergangspunkt dar, da wir nun genug Platz haben, Elemente neu zu positionieren, damit sie besser auf den Bildschirm passen. Wir können dies mithilfe einer Technologie namens [Medienabfragen](/web/fundamentals/design-and-ux/responsive/#use-css-media-queries-for-responsiveness) tun.

#### Ansprechende Bilder hinzufügen

<div class="styles" style="font-family: monospace;">
  <div style="background-color: #39b1a4">#39b1a4</div>
  <div style="background-color: white">#ffffff</div>
  <div style="background-color: #f5f5f5">#f5f5f5</div>

  <div style="background-color: #e9e9e9">#e9e9e9</div>
  <div style="background-color: #dc4d38">#dc4d38</div>
</div>

#### Das Formularelement frei verschieben

<img  src="images/narrowsite.png" alt="Designed site"  class="attempt-right" />

Note: Sie müssen nicht alle Elemente auf einmal verschieben, sondern können bei Bedarf kleinere Anpassungen vornehmen.

Im Kontext unserer Produktseite müssen wir Folgendes tun:

Wir haben uns entschieden, nur zwei Hauptlayouts zu verwenden: einen schmalen und einen breiten Darstellungsbereich, wodurch der Erstellungsprozess stark vereinfacht wird.

<div style="clear:both;"></div>

    #headline {
      padding: 0.8em;
      color: white;
      font-family: Roboto, sans-serif;
      background-image: url(backgroundimage.jpg);
      background-size: cover;
    }
    

Darüber hinaus möchten wir randlose Abschnitte für den schmalen Darstellungsbereich erstellen, die auch im breiten Darstellungsbereich randlos bleiben. Das bedeutet, dass wir die maximale Breite der Anzeige so begrenzen sollten, dass sich Text und Absätze auf extrem breiten Bildschirmen nicht zu einer einzigen langen Zeile ausdehnen. Diese Begrenzung soll bei uns bei 800 Pixeln liegen.

### Legen Sie Ihren ersten Übergangspunkt an

Dazu müssen wir die Breite einschränken und die Elemente zentrieren. Wir müssen einen Container um die einzelnen Hauptabschnitte erstellen und `margin: auto` verwenden. Damit bleiben die Inhalte zentriert und werden maximal 800 Pixel breit, auch wenn die Bildschirmgröße weiter steigt.

<video controls poster="images/firstbreakpoint.png" style="width: 100%;">
  <source src="videos/firstbreakpoint.mov" type="video/mov"></source>
  <source src="videos/firstbreakpoint.webm" type="video/webm"></source>
  <p>Ihr Browser unterstützt keine Videos.
     <a href="videos/firstbreakpoint.mov">Video herunterladen</a>
  </p>
</video>

Bei dem Container handelt es sich um ein einfaches `div`-Tag-Paar, das die folgende Form aufweist:

    @media (min-width: 600px) {
    
    }
    

Bei schmalen Darstellungsbereichen haben wir nur wenig Platz, den wir zur Darstellung von Inhalten nutzen können. Daher werden Größe und Breite der Typografie oft stark reduziert, damit sie auf den Bildschirm passt.

Bei größeren Darstellungsbereichen müssen wir beachten, dass Nutzer zwar eine größere Anzeige haben, sich dabei jedoch weiter weg davon befinden. Wir können die Lesbarkeit der Inhalte verbessern, indem wir die Größe und Breite der Typografie erhöhen und darüber hinaus die Abstände verändern, sodass bestimmte Bereiche auffälliger sind.

Auf unserer Produktseite erhöhen wir die Abstände für die Abschnittselemente, indem wir sie darauf einstellen, stets 5 % der Breite einzuhalten. Darüber hinaus vergrößern wir die Kopfzeilen der Abschnitte.

* die maximale Breite des Designs einschränken,
* die Abstände der Elemente ändern und die Textgröße reduzieren,
* das Formular verschieben, damit es sich auf gleicher Höhe mit den Inhalten der Kopfzeile ausrichtet,
* das Video so konfigurieren, dass es sich um die Inhalte herum ausrichtet, und
* die Größe der Bilder reduzieren und sie in einem optisch ansprechenderen Raster erscheinen lassen.

### Die maximale Breite des Designs einschränken

Unser schmaler Darstellungsbereich war eine vertikale, lineare Anzeige. Alle Hauptabschnitte und die entsprechenden Inhalte darin wurden der Reihe nach von oben nach unten angezeigt.

Bei einem breiten Darstellungsbereich haben wir mehr Platz, den wir dazu nutzen können, die Inhalte auf optimale Weise für diesen Bildschirm darzustellen. Für unsere Produktseite bedeutet das, dass wir gemäß unserer IA Folgendes tun können:

Bei einem schmalen Darstellungsbereich haben wir sehr viel weniger Platz in der Horizontalen, um Elemente bequem auf dem Bildschirm zu platzieren.

Damit wir die Horizontale effektiver nutzen können, müssen wir die lineare Darstellung der Kopfzeile auflösen und das Formular und die Liste verschieben, damit sie nebeneinander erscheinen.

    <div class="container">
    </div>
    

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="containerhtml" adjust_indentation="auto" %}
</pre>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="container" adjust_indentation="auto" %}
</pre>

Das Video auf der Oberfläche für den schmalen Darstellungsbereich ist so eingestellt, dass es die volle Breite des Bildschirms ausfüllt und nach den wichtigsten Funktionen erscheint. Auf einem breiten Darstellungsbereich erscheint das Video durch die Skalierung zu groß und zudem unpassend, wenn es neben unserer Liste mit Funktionen platziert wird.

### Abstände ändern und Textgröße reduzieren

Das Videoelement muss aus dem vertikalen Fluss des schmalen Darstellungsbereichs herausgenommen werden und auf einem breiten Darstellungsbereich neben der Inhaltsliste erscheinen.

Die Bilder sind für schmale Darstellungsbereiche, in der Regel die Bildschirme von Mobilgeräten, so eingestellt, dass Sie die Breite des Bildschirms ausfüllen und vertikal gestapelt erscheinen. Eine Skalierung dieses Prinzips für breite Darstellungsbereiche ist nicht ratsam.

Damit die Bilder in breiten Darstellungsbereichen richtig erscheinen, werden sie auf 30 % der Container-Breite skaliert und horizontal statt vertikal ausgerichtet, wie es in schmalen Darstellungsbereichen der Fall ist. Darüber hinaus fügen wir eine Umrandung und Schlagschatten hinzu, um die Optik der Bilder zusätzlich zu verbessern.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/alterpadding.html){: target="_blank" .external }

### Elemente an breiten Darstellungsbereich anpassen

Berücksichtigen Sie bei der Verwendung von Bildern die Größe des Darstellungsbereichs und die Pixeldichte der Anzeige.

Das Web wurde für Bildschirme mit 96 DPI geschaffen. Mit der Einführung von Mobilgeräten hat sich die Pixeldichte von Bildschirmen sprunghaft erhöht, ganz abgesehen von Laptopbildschirmen der Retina-Klasse. Aus diesem Grund sehen Bilder, die auf 96 DPI codiert sind, häufig sehr schlecht auf Geräten aus, die hohe DPI-Werte unterstützen.

* das Formular um die Kopfzeileninformationen herum verschieben,
* das Video rechts neben die wichtigsten Punkte platzieren,
* die Bilder gekachelt darstellen und
* die Tabelle erweitern.

#### Das Videoelement frei verschieben

Dafür haben wir eine Lösung, die noch nicht sehr weit verbreitet ist. In Browsern, die dies unterstützen, können Sie Bilder mit hoher Pixeldichte anzeigen lassen, wenn sie auf Bildschirmen mit hoher Pixeldichte aufgerufen werden.

Die korrekte Darstellung von Tabellen auf Geräten mit schmalem Darstellungsbereich ist extrem schwierig und verdient daher besondere Aufmerksamkeit.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="formfloat" adjust_indentation="auto" %}
</pre>

Für schmale Darstellungsbereiche sollten Sie Ihre Tabelle in zwei Zeilen gliedern, wobei Kopfzeile und Zellen zur Darstellung der Spalte in eine Zeile verschoben werden.

<video controls poster="images/floatingform.png" style="width: 100%;">
  <source src="videos/floatingform.mov" type="video/mov"></source>
  <source src="videos/floatingform.webm" type="video/webm"></source>
  <p>Ihr Browser unterstützt keine Videos.
     <a href="videos/floatingform.mov">Video herunterladen</a>
  </p>
</video>

#### Bilder gekachelt darstellen

Bei unserer Website mussten wir für den Inhalt der Tabelle einen zusätzlichen Übergangspunkt erstellen. Wenn Sie zuerst für Mobilgeräte entwickeln, ist es schwieriger, angewendete Stile rückgängig zu machen. Daher müssen wir den CSS-Code für die Tabelle auf schmalen Darstellungsbereichen vom CSS-Code für breite Darstellungsbereiche trennen. Dadurch sorgen wir für einen klaren und konsequenten Übergang.

** Wir gratulieren!** Wenn Sie diesen Text lesen, haben Sie Ihre erste einfache Produktzielseite erstellt, die auf vielen verschiedenen Geräten, Formfaktoren und Bildschirmgrößen richtig angezeigt wird.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding" adjust_indentation="auto" %}
</pre>

Indem Sie sich an die folgenden Richtlinien halten, sorgen Sie für einen guten Start:

#### Bilder für DPI-Werte responsiv machen

<img src="images/imageswide.png" class="attempt-right" />

The images in the narrow viewport (mobile devices mostly) interface are set to be the full width of the screen and stacked vertically. This doesn't scale well on a wide viewport.

To make the images look correct on a wide viewport, they are scaled to 30% of the container width and laid out horizontally (rather than vertically in the narrow view). We will also add some border radius and box-shadow to make the images look more appealing.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="floatvideo" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/tiletheimages.html){: target="_blank" .external }

#### Tabellen

When using images, take the size of the viewport and the density of the display into consideration.

The web was built for 96dpi screens. With the introduction of mobile devices, we have seen a huge increase in the pixel density of screens not to mention Retina class displays on laptops. As such, images that are encoded to 96dpi often look terrible on a hi-dpi device.

We have a solution that is not widely adopted yet. For browsers that support it, you can display a high density image on a high density display.

    <img src="photo.png" srcset="photo@2x.png 2x">
    

#### Tables

Tables are very hard to get right on devices that have a narrow viewport and need special consideration.

We recommend on a narrow viewport that you transform your table by changing each row into a block of key-value pairs (where the key is what was previously the column header, and the value is still the cell value). Fortunately, this isn't too difficult. First, annotate each `td` element with the corresponding heading as a data attribute. (This won't have any visible effect until we add some more CSS.)

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/updatingtablehtml.html" region_tag="table-tbody" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/updatingtablehtml.html){: target="_blank" .external }

Now we just need to add the CSS to hide the original `thead` and instead show the `data-th` labels using a `:before` pseudoelement. This will result in the multi-device experience seen in the following video.

<video controls poster="images/responsivetable.png" style="width: 100%;">
  <source src="videos/responsivetable.mov" type="video/mov"></source>
  <source src="videos/responsivetable.webm" type="video/webm"></source>
  <p>Ihr Browser unterstützt keine Videos.
     <a href="videos/responsivetable.mov">Video herunterladen</a>
  </p>
</video>

In our site, we had to create an extra breakpoint just for the table content. When you build for a mobile device first, it is harder to undo applied styles, so we must section off the narrow viewport table CSS from the wide viewport css. This gives us a clear and consistent break.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/content-with-styles.html" region_tag="table-css" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-with-styles.html){: target="_blank" .external }

## Wrapping up

Success: By the time you read this, you will have created your first simple product landing page that works across a large range of devices, form-factors, and screen sizes.

If you follow these guidelines, you will be off to a good start:

1. Erstellen Sie eine grundlegende IA und verstehen Sie Ihre Inhalte, bevor Sie mit der Programmierung beginnen.
2. Legen Sie immer einen Darstellungsbereich fest.
3. Erstellen Sie die Grunderfahrung anhand des `Mobile First`-Ansatzes.
4. Wenn Sie die Erfahrung für Mobilgeräte fertiggestellt haben, erhöhen Sie die Breite der Darstellung, bis sie unvorteilhaft aussieht, und setzen Sie davor Ihren Übergangspunkt.
5. Wiederholen Sie diesen Ansatz.