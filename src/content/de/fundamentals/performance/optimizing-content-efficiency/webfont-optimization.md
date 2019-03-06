project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Die Typografie ist eine grundlegende Voraussetzung für gutes Design und Branding sowie Lesbarkeit und Zugänglichkeit. Webschriftarten erfüllen die obigen Bedingungen und bieten noch mehr: Der Text ist skalierbar, kann durchsucht, vergrößert und verkleinert werden und unterstützt hohe DPI-Werte. Außerdem liefern diese Schriftarten eine konstante und scharfe Textdarstellung unabhängig von der Bildschirmgröße und -auflösung.

{# wf_updated_on: 2014-09-29 #} {# wf_published_on: 2014-09-19 #}

# Optimierung von Webschriftarten {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*This article contains contributions from [Monica Dinculescu](https://meowni.ca/posts/web-fonts/), [Rob Dodson](/web/updates/2016/02/font-display), and Jeff Posnick.*

Die Optimierung von Webschriftarten ist ein entscheidender Bestandteil der gesamten Leistungsstrategie. Jede Schriftart ist eine zusätzliche Ressource und einige Schriftarten blockieren möglicherweise das Rendern des Textes, aber nur weil die Seite Webschriftarten verwendet, heißt dies nicht, dass sie langsamer gerendert werden muss. Im Gegenteil: Eine optimierte Schriftart in Kombination mit einer sinnvollen Strategie für das Laden und Anwenden auf der Seite kann zur Verringerung der gesamten Seitengröße beitragen und die Rendering-Zeiten verkürzen.

Eine Webschriftart ist eine Sammlung von Glyphen und jede Glyphe ist eine Vektorform, die einen Buchstaben oder ein Symbol repräsentiert. Dies führt dazu, dass die Größe einer bestimmten Schriftartdatei durch zwei einfache Variablen bestimmt wird: die Komplexität des Vektorpfads der einzelnen Glyphen und die Anzahl der Glyphen in einer bestimmten Schriftart. Open Sans, eine der beliebtesten Webschriftarten, enthält beispielsweise 897 Glyphen, u. a. lateinische, griechische und kyrillische Zeichen.

## Aufbau einer Webschriftart

### TL;DR {: .hide-from-toc }

* Unicode-Schriftarten können Tausende von Glyphen enthalten.
* Es gibt vier Schriftformate: WOFF2, WOFF, EOT und TTF.
* Einige Schriftformate erfordern die GZIP-Komprimierung.

A *webfont* is a collection of glyphs, and each glyph is a vector shape that describes a letter or symbol. As a result, two simple variables determine the size of a particular font file: the complexity of the vector paths of each glyph and the number of glyphs in a particular font. For example, Open Sans, which is one of the most popular webfonts, contains 897 glyphs, which include Latin, Greek, and Cyrillic characters.

<img src="images/glyphs.png"  alt="Font glyph table" />

Es liegt auf der Hand, dass die Verwendung von Schriftarten im Internet eine sorgfältige Codierung erfordert, um sicherzugehen, dass die Typografie nicht die Leistung beeinträchtigt. Auf der Webplattform finden Sie alle erforderlichen Grundlagen und im weiteren Verlauf des Leitfadens wird praktisch dargestellt, wie das Beste von beidem erreicht werden kann.

Derzeit werden vier Formate von Schriftartcontainern im Internet verwendet: [EOT](http://en.wikipedia.org/wiki/Embedded_OpenType), [TTF](http://de.wikipedia.org/wiki/TrueType), [WOFF](http://de.wikipedia.org/wiki/Web_Open_Font_Format) und [WOFF2](http://www.w3.org/TR/WOFF2/). Trotz der großen Auswahl gibt es leider kein einzelnes universelles Format, das in allen alten und neuen Browsern funktioniert: EOT eignet sich [nur für den IE](http://caniuse.com/#feat=eot), TTF bietet [partielle IE-Unterstützung](http://caniuse.com/#search=ttf), WOFF verfügt über die breiteste Unterstützung, ist aber [in einigen älteren Browsern nicht verfügbar](http://caniuse.com/#feat=woff) und die WOFF 2.0-Unterstützung [ist für viele Browser noch nicht implementiert](http://caniuse.com/#feat=woff2).

### Formate von Webschriftarten

Was bedeutet das für uns? Es gibt kein einziges Format, das in allen Browsern funktioniert, d. h., es müssen mehrere Formate bereitgestellt werden, um eine einheitliche Erfahrung zu bieten:

Note: Eigentlich gibt es auch den [SVG-Schriftart-Container](http://caniuse.com/svg-fonts), aber dieser wurde nie von IE oder Firefox unterstützt und mittlerweile auch nicht mehr von Chrome. Deshalb hat er nur eingeschränkten Nutzen und wird in diesem Leitfaden nicht behandelt.

* Liefern Sie die WOFF 2.0-Variante für Browser, die diese unterstützen.
* Liefern Sie die WOFF-Variante für die Mehrzahl der Browser.
* Liefern Sie die TTF-Variante für alte Android-Browser (vor 4.4).
* Liefern Sie die EOT-Variante für alte IE-Browser (vor IE9). ^

Eine Schriftart ist eine Sammlung von Glyphen, von denen jede eine Reihe von Pfaden zur Beschreibung der Buchstabenform darstellt. Die einzelnen Glyphen unterscheiden sich natürlich, aber ungeachtet dessen enthalten sie eine Vielzahl ähnlicher Informationen, die mit GZIP oder einem kompatiblen Komprimierungsprogramm minimiert werden können:

### Schriftgröße per Komprimierung reduzieren

Darüber hinaus ist es beachtenswert, dass einige Schriftartformate zusätzliche Metadaten wie zum Beispiel Informationen zum [Hinting](http://http://de.wikipedia.org/wiki/Hint) und zur [Unterschneidung](http://http://de.wikipedia.org/wiki/Unterschneidung_%28Typografie%29) beinhalten, die auf manchen Plattformen nicht benötigt werden, was eine weitere Optimierung der Dateigröße ermöglicht. Informieren Sie sich über die verfügbaren Optimierungsoptionen Ihres Schriftart-Komprimierungsprogramms und achten Sie darauf, wenn Sie diesen Weg verfolgen, dass Sie über die geeignete Infrastruktur zum Testen und zur Übermittlung dieser optimierten Schriftarten an die einzelnen Browser verfügen, z. B. stellt Google Fonts mehr als 30 optimierte Varianten für jede Schriftart bereit und erkennt und liefert die optimale Variante für jede Plattform und jeden Browser.

* Die Formate EOT und TTF werden standardmäßig nicht komprimiert: Achten Sie darauf, dass Ihre Server für die Anwendung der [GZIP-Komprimierung](/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text-compression-with-gzip) konfiguriert sind, wenn Sie diese Formate bereitstellen.
* WOFF verfügt über eine integrierte Komprimierung. Achten Sie darauf, dass Ihr WOFF-Komprimierungsprogramm die optimalen Komprimierungseinstellungen verwendet.
* WOFF2 nutzt spezifische Vorverarbeitungs- und Komprimierungsalgorithmen für eine Verringerung der Dateigröße um ca. 30 % gegenüber anderen Formaten, siehe [Bericht](http://www.w3.org/TR/WOFF20ER/).

Note: Ziehen Sie die Verwendung der [Zopfli-Komprimierung](http://en.wikipedia.org/wiki/Zopfli) für die Formate EOT, TTF und WOFF in Betracht. Zopfli ist ein mit ZLIB kompatibles Komprimierungsprogramm, das die Dateigröße gegenüber GZIP um ca. weitere 5 % reduziert.

Die CSS-at-Regel @font-face gestattet es, den Speicherort einer bestimmten Schriftartressource, deren Style-Eigenschaften und die Unicode-Codepoints festzulegen, für die sie verwendet werden soll. Über eine Kombination solcher @font-face-Deklarationen kann eine `Schriftartfamilie` erstellt werden, die der Browser zur Beurteilung heranzieht, welche Schriftartressourcen herunterzuladen sind und auf die aktuelle Seite angewendet werden müssen. Wir wollen uns nun genauer ansehen, wie das intern vor sich geht.

## Schriftartfamilie mit @font-face definieren

### TL;DR {: .hide-from-toc }

* Verwenden Sie den Hinweis `format()`, um mehrere Schriftformate anzugeben.
* Unterteilen Sie große Unicode-Schriftarten, um die Leistung zu verbessern: Nutzen Sie die Unicode-Bereichsunterteilung und sehen Sie eine manuelle Ausweichlösung zur Unterteilung für ältere Browser vor.
* Reduzieren Sie die Zahl der stilistischen Schriftvarianten, um die Leistung bei der Seiten- und Textwiedergabe zu verbessern.

Jede @font-face-Deklaration beinhaltet den Namen der Schriftartfamilie, die eine logische Gruppe aus mehreren Deklarationen darstellt, außerdem die [Schriftarteigenschaften](http://www.w3.org/TR/css3-fonts/#font-prop-desc) wie Stil, Stärke und Streckung sowie den [SRC-Descriptor](http://www.w3.org/TR/css3-fonts/#src-desc), der eine priorisierte Liste der Speicherorte für die Schriftartressource angibt.

### Format auswählen

Beachten Sie zunächst, dass in den obigen Beispielen eine einzige Schriftartfamilie *Awesome Font* mit zwei Schriftstilen (normal und *italic*) definiert wird, die jeweils auf eine andere Gruppe an Schriftartressourcen verweisen. Jeder SRC-Descriptor enthält wiederum eine priorisierte, kommagetrennte Liste mit Ressourcenvarianten:

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome.woff2') format('woff2'), 
           url('/fonts/awesome.woff') format('woff'),
           url('/fonts/awesome.ttf') format('ttf'),
           url('/fonts/awesome.eot') format('eot');
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: italic;
      font-weight: 400;
      src: local('Awesome Font Italic'),
           url('/fonts/awesome-i.woff2') format('woff2'), 
           url('/fonts/awesome-i.woff') format('woff'),
           url('/fonts/awesome-i.ttf') format('ttf'),
           url('/fonts/awesome-i.eot') format('eot');
    }
    

Note: Wenn Sie nicht auf einen der Standard-Systemschriftarten zurückgreifen, haben die Nutzer in der Praxis die entsprechenden Schriftarten selten lokal installiert, insbesondere auf Mobilgeräten, wo es faktisch unmöglich ist, zusätzliche Schriftarten zu `installieren`. Aus diesem Grund sollten Sie stets eine Liste mit externen Speicherorten für Schriftarten bereitstellen.

* Mithilfe der Anweisung `local()` können wir lokal installierte Schriftarten referenzieren, laden und verwenden.
* Mithilfe der Anweisung `url()` können wir externe Schriftarten laden und einen optionalen Hinweis `format()` aufnehmen, der das Format der Schriftart angibt, auf die die vorgesehene URL verweist.

Wenn der Browser feststellt, dass die Schriftart benötigt wird, arbeitet er die bereitgestellte Ressourcenliste in der angegebenen Reihenfolge ab und versucht, die entsprechende Ressource zu laden. Gemäß dem obigen Beispiel geschieht das wie folgt:

Die Kombination aus lokalen und externen Anweisungen mit entsprechenden Formathinweisen ermöglicht es uns, alle verfügbaren Schriftartformate anzugeben und dem Browser die übrige Arbeit zu überlassen: Der Browser ermittelt, welche Ressourcen erforderlich sind und wählt das optimale Format aus.

1. Der Browser führt das Seitenlayout aus und bestimmt, welche Schriftartvarianten für die Darstellung des vorgegebenen Texts auf der Seite benötigt werden.
2. Für jede erforderliche Schriftart prüft der Browser, ob diese lokal verfügbar ist.
3. Wenn die Datei lokal nicht verfügbar ist, arbeitet der Browser die externen Definitionen ab: 
    * Ist ein Formathinweis vorhanden, prüft der Browser, ob er das Format unterstützt, bevor er den Download einleitet, andernfalls geht er zur nächsten Schriftart über.
    * Ist kein Formathinweis vorhanden, lädt er die Ressource herunter.

Note: Es kommt auf die Reihenfolge an, in der die Schriftvarianten angegeben werden. Der Browser wählt das erste unterstützte Format aus. Wenn Sie also wünschen, dass die neueren Browser WOFF2 verwenden, ist die WOFF2-Deklaration vor WOFF anzuordnen und so weiter.

Neben den Schrifteigenschaften wie Stil, Stärke und Streckung können wir mit der @font-face-Regel ein Gruppe von Unicode-Codepoints definieren, die von jeder Ressource unterstützt werden. Auf diese Weise ist es möglich, eine große Unicode-Schriftart in kleinere Untergruppen, z. B. für lateinische, kyrillische und griechische Zeichen, aufzuteilen und nur die Glyphen herunterzuladen, die für die Darstellung des Texts auf einer bestimmten Seite erforderlich sind.

### Unterteilung in Unicode-Bereiche

Mit dem [Unicode-Bereichs-Descriptor](http://www.w3.org/TR/css3-fonts/#descdef-unicode-range) können wir eine kommagetrennte Liste von Bereichswerten angeben, von denen jeder in einer von drei verschiedenen Formen vorliegen kann:

Wir können unsere Schriftart *Awesome Font* zum Beispiel in lateinische und japanische Untergruppen aufteilen, die vom Browser je nach Bedarf heruntergeladen werden:

* Einzelner Codepoint, z. B. U+416)
* Intervallbereich, z. B. U+400-4ff): kennzeichnet die Start- und End-Codepoints eines Bereichs
* Platzhalterbereich, z. B. U+4??): Die Zeichen `?` zeigen eine hexadezimale Ziffer an .

Note: Die Unterteilung in Unicode-Bereiche ist besonders für asiatische Sprachen wichtig, bei denen die Anzahl der Glyphen wesentlich höher ist als in westlichen Sprachen und eine typische `komplette` Schriftart oftmals in Megabyte anstatt in Kilobyte angegeben wird!

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-jp.woff2') format('woff2'), 
           url('/fonts/awesome-jp.woff') format('woff'),
           url('/fonts/awesome-jp.ttf') format('ttf'),
           url('/fonts/awesome-jp.eot') format('eot');
      unicode-range: U+3000-9FFF, U+ff??; /* Japanese glyphs */
    }
    

Mithilfe von Unicode-Bereichsuntergruppen und separaten Dateien für jede stilistische Variante der Schriftart können wir eine kombinierte Schriftartfamilie definieren, die schneller und effizienter heruntergeladen werden kann. Die Besucher laden dann nur die Varianten und Untergruppen herunter, die diese Schriftartfamilie benötigt, und sie sind nicht gezwungen, Untergruppen herunterzuladen, die sie womöglich auf der Seite niemals sehen oder verwenden.

Es soll jedoch nicht verschwiegen werden, dass Unicode-Bereiche einen kleinen Haken haben: [Sie werden noch nicht von allen Browsern unterstützt](http://caniuse.com/#feat=font-unicode-range). Einige Browser ignorieren den Unicode-Bereichshinweis einfach und laden alle Varianten herunter, während andere die @font-face-Deklaration überhaupt nicht verarbeiten. Wir lösen dieses Problem, indem wir bei älteren Browsern auf die `manuelle Unterteilung` ausweichen.

Da alte Browser nicht ausreichend intelligent sind, um nur die benötigten Untergruppen auszuwählen und keine kombinierte Schriftart erstellen können, weichen wir auf die Bereitstellung einer einzelnen Schriftartressource aus, die alle notwendigen Untergruppen enthält und verbergen den Rest vor dem Browser. Wenn die Seite zum Beispiel nur lateinische Schriftzeichen verwendet, können wir andere Glyphen entfernen und diese spezielle Untergruppe als eigenständige Ressource übermitteln.

Jede Schriftartfamilie besteht aus mehreren stilistischen Varianten (normal, fett, kursiv) und mehreren Schriftstärken für jeden Stil, von denen jeder wiederum sehr unterschiedliche Glyphenformen aufweisen kann, z. B. unterschiedliche Abstände, Größen oder eine komplett andere Form.

1. **Wie bestimmen wir, welche Untergruppen benötigt werden?** 
    * Wenn die Unterteilung in Unicode-Bereiche vom Browser unterstützt wird, wählt dieser die richtige Untergruppe automatisch aus. Die Seite muss lediglich die Untergruppendateien bereitstellen und die passenden Unicode-Bereiche in den @font-face-Regeln angeben.
    * Wenn der Unicode-Bereich nicht unterstützt wird, muss die Seite alle unnötigen Untergruppen ausblenden, d. h., es ist Aufgabe des Entwicklers, die erforderlichen Untergruppen anzugeben.
2. **Wie erzeugen wir Schriftart-Untergruppen?** 
    * Verwenden Sie das Open-Source-Tool [pyftsubset](https://github.com/behdad/fonttools/blob/master/Lib/fontTools/subset.py#L16) für die Unterteilung und Optimierung Ihrer Schriftarten.
    * Einige Schriftartdienste gestatten die manuelle Unterteilung über benutzerdefinierte Abfrageparameter, mit denen die erforderliche Untergruppe für Ihre Seite manuell angegeben werden kann - schlagen Sie in der Dokumentation Ihres Schriftartanbieters nach.

### Schriftartauswahl und -synthese

Each font family is composed of multiple stylistic variants (regular, bold, italic) and multiple weights for each style, each of which, in turn, may contain very different glyph shapes&mdash;for example, different spacing, sizing, or a different shape altogether.

<img src="images/font-weights.png"  alt="Font weights" />

Die entsprechende Logik gilt für kursive Varianten. Der Schriftart-Designer steuert, welche Varianten erstellt werden, und wir steuern, welche Varianten wir auf der Seite verwenden. Da jede Variante einen separaten Download darstellt, empfiehlt es sich, die Zahl der Varianten zu begrenzen. Wir können zum Beispiel zwei Fettschrift-Varianten für unsere Schriftartfamilie *Awesome Font* festlegen:

> Wenn eine Stärke angegeben ist, für die keine Schrift vorhanden ist, wird eine Schrift mit ähnlicher Stärke verwendet. Grundsätzlich werden Fettformatierungen Schriften mit höherer Stärke und helle Formatierungen Schriften mit geringerer Stärke zugeordnet.
> 
> > [Algorithmus für CSS3-Schriftartenzuordnung](http://www.w3.org/TR/css3-fonts/#font-matching-algorithm)

Mit dem obigen Beispiel wird die Schriftartfamilie *Awesome Font* deklariert, die aus zwei Ressourcen mit demselben Satz an lateinischen Glyphen (U+000-5FF) besteht, aber zwei verschiedene Stärken anbietet: normal (400) und fett (700). Wie verhält es sich jedoch, wenn eine unserer CSS-Regeln eine andere Schriftstärke angibt oder die Schriftstileigenschaft auf kursiv setzt?

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 700;
      src: local('Awesome Font'),
           url('/fonts/awesome-l-700.woff2') format('woff2'), 
           url('/fonts/awesome-l-700.woff') format('woff'),
           url('/fonts/awesome-l-700.ttf') format('ttf'),
           url('/fonts/awesome-l-700.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    

The above example declares the *Awesome Font* family that is composed of two resources that cover the same set of Latin glyphs (`U+000-5FF`) but offer two different "weights": normal (400) and bold (700). However, what happens if one of your CSS rules specifies a different font weight, or sets the font-style property to italic?

* Wenn eine genau übereinstimmende Schriftart nicht verfügbar ist, ersetzt der Browser diese durch diejenige mit der ähnlichsten Übereinstimmung.
* Wird keine stilistische Übereinstimmung gefunden, z. B. weil wir wie im obigen Beispiel keine kursiven Varianten deklariert haben, erstellt der Browser eine eigene Schriftartvariante.

<img src="images/font-synthesis.png"  alt="Font synthesis" />

Note: Im Sinne einer optimalen Konsistenz und bestmöglicher visueller Resultate sollten Sie sich nicht auf die Schriftartsynthese verlassen. Minimieren Sie stattdessen die Anzahl der verwendeten Schriftvarianten und geben Sie deren Speicherort an, damit der Browser diese herunterladen kann, wenn sie auf der Seite benötigt werden. Unter Berücksichtigung der obigen Ausführungen [kann in manchen Fällen eine synthetisierte Variante eine gangbare Option darstellen](https://www.igvita.com/2014/09/16/optimizing-webfont-selection-and-synthesis/). Dies ist allerdings sorgfältig zu prüfen.

Eine `komplette` Webschriftart, die alle stilistischen Varianten und alle Glyphen umfasst, die wir möglicherweise nicht brauchen, kann ohne Weiteres einen Download von mehreren Megabytes bedeuten. Aus diesem Grund wurde eigens die CSS-Regel @font-face konzipiert, mit der wir die Schriftartfamilie in eine Sammlung von Ressourcen aufteilen können, die Unicode-Untergruppen, spezielle Stilvarianten usw. umfasst.

Anhand dieser Deklarationen bestimmt der Browser die erforderlichen Untergruppen und Varianten und lädt die minimalen Bestandteile, die für die Wiedergabe des Texts benötigt werden. Dieses Verhalten ist zwar sehr komfortabel, kann aber bei mangelnder Sorgfalt zu einem Leistungsengpass im kritischen Rendering-Pfad führen und die Textwiedergabe verzögern - und das wollen wir ja unbedingt vermeiden!

## Laden und Rendern optimieren

### TL;DR {: .hide-from-toc }

* Schriftartanforderungen werden verzögert, bis die Rendering-Baumstruktur erstellt ist, was zu einer verzögerten Textwiedergabe führen kann.
* Das Font Loading API ermöglicht die Implementierung von Strategien zum Laden und Rendern von Schriftarten, die das standardmäßige langsame Laden (Lazy Loading) von Schriftarten umgehen.

Das Lazy Loading von Schriftarten hat eine wichtige verborgene Auswirkung, die die Textwiedergabe verzögern kann: Der Browser muss [die Rendering-Baumstruktur erstellen](/web/fundamentals/performance/critical-rendering-path/render-tree-construction), die von den DOM- und CSSOM-Baumstrukturen abhängt, bevor klar ist, welche Schriftartressourcen für das Rendern des Texts benötigt werden. Das hat zur Folge, dass Schriftarten wesentlich später als andere kritische Ressourcen angefordert werden und der Browser eventuell an der Textwiedergabe gehindert wird, bis die jeweilige Ressource abgerufen wurde.

Given these declarations, the browser figures out the required subsets and variants and downloads the minimal set required to render the text, which is very convenient. However, if you're not careful, it can also create a performance bottleneck in the critical rendering path and delay text rendering.

### Webschriftarten und kritischer Rendering-Pfad

Das `Rennen` zwischen dem ersten Rendern des Seiteninhalts, das kurz nach der Erstellung der Rendering-Baumstruktur erfolgen kann, und der Anforderung der Schriftartressource ist für das `Leertextproblem` verantwortlich, bei dem der Browser das Seitenlayout ohne Text darstellt. Das tatsächliche Verhalten unterscheidet sich bei den verschiedenen Browsern:

<img src="images/font-crp.png"  alt="Font critical rendering path" />

1. Der Browser fordert ein HTML-Dokument an.
2. Der Browser beginnt mit dem Parsen der HTML-Antwort und mit der Erstellung des DOM.
3. Der Browser erkennt CSS-, JS- und andere Ressourcen und versendet Anforderungen.
4. Der Browser erstellt nach dem Erhalt aller CSS-Inhalte das CSSOM und verbindet es mit der DOM-Baumstruktur, um die Rendering-Baumstruktur zu erzeugen. 
    * Die Schriftartanforderungen werden ausgegeben, sobald durch die Rendering-Baumstruktur angezeigt wird, welche Schriftartvarianten für die Wiedergabe des vorgegebenen Texts auf der Seite benötigt werden.
5. Der Browser führt das Layout aus und stellt die Inhalte auf dem Bildschirm dar. 
    * Wenn die Schriftart noch nicht verfügbar ist, kann der Browser keine Textpixel rendern.
    * Sobald die Schriftart verfügbar ist, rendert der Browser die Textpixel.

Das [Font Loading API](http://dev.w3.org/csswg/css-font-loading/) bietet eine Scripting-Oberfläche für die Definition und Bearbeitung von CSS-Schriftarten, die Verfolgung des Downloadfortschritts und die Umgehung des standardmäßigen Lazy Loading-Verhaltens. Wenn wir beispielsweise sicher sind, dass eine bestimmte Schriftvariante benötigt wird, können wir diese definieren und den Browser anweisen, den sofortigen Abruf der Schriftartressource einzuleiten:

Weil wir den Schriftstatus mit der Methode [check()](http://dev.w3.org/csswg/css-font-loading/#font-face-set-check)) überprüfen und den jeweiligen Downloadfortschritt verfolgen können, ist es darüber hinaus möglich, eine angepasste Strategie für die Wiedergabe von Text auf unseren Seiten festzulegen:

### Schriftart-Rendering mit dem Font Loading API optimieren

Das Beste ist: Wir können die obigen Strategien auch mischen und an unterschiedliche Seiteninhalte anpassen, z. B. das Text-Rendering in bestimmten Bereichen bis zur Verfügbarkeit der Schriftart anhalten, eine Ausweich-Schriftart nutzen und dann erneut rendern, sobald der Schriftartdownload abgeschlossen ist, verschiedene Zeitlimits festlegen und so weiter.

Note: Das Font Loading API [ist für manche Browser noch in der Entwicklungsphase](http://caniuse.com/#feat=font-loading). Ziehen Sie die Verwendung des [FontLoader-Polyfills](https://github.com/bramstein/fontloader) oder der [Webfontloader-Bibliothek](https://github.com/typekit/webfontloader) in Betracht, um eine ähnliche Funktionalität bereitzustellen, allerdings mit dem Nachteil einer zusätzlichen JavaScript-Abhängigkeit.

Eine einfache alternative Strategie, um mithilfe des Font Loading API das `Leertext-Problem` zu umgehen, besteht darin, die Schriftartinhalte inline in ein CSS-Stylesheet einzubetten:

```html
var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
  style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
});

font.load(); // don't wait for render tree, initiate immediate fetch!

font.ready().then(function() {
  // apply the font (which may rerender text and cause a page reflow)
  // once the font has finished downloading
  document.fonts.add(font);
  document.body.style.fontFamily = "Awesome Font, serif";

  // OR... by default content is hidden, and rendered once font is available
  var content = document.getElementById("content");
  content.style.visibility = "visible";

  // OR... apply own render strategy here... 
});
```

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

Zwar ist die Inlining-Strategie nicht so flexibel und gestattet es uns nicht, benutzerdefinierte Zeitlimits oder Rendering-Ansätze für unterschiedliche Inhalte zu definieren, aber es ist eine einfache und robuste Lösung, die mit allen Browsern funktioniert. Isolieren Sie für optimale Resultate die Inline-Schriftarten in ein eigenständiges Stylesheet und stellen Sie sie mit einer langen Ablaufdauer (max-age) bereit. Auf diese Weise zwingen Sie die Besucher bei der Aktualisierung Ihres CSS nicht dazu, die Schriftarten erneut herunterzuladen.

Note: Nutzen Sie die Inline-Ersetzung selektiv! Vergegenwärtigen Sie sich, dass der Grund, warum @font-face ein Lazy Loading-Verhalten nutzt, die Vermeidung des Downloads unnötiger Schriftvarianten und Untergruppen ist. Ebenso wird das Anwachsen Ihres CSS-Codes über eine aggressive Inline-Ersetzung sich negativ auf den [kritischen Rendering-Pfad](/web/fundamentals/performance/critical-rendering-path/) auswirken - der Browser müssen den gesamten CSS-Code herunterladen, bevor er das CSSOM erstellt, die Rendering-Baumstruktur erzeugt und den Seiteninhalt auf dem Bildschirm darstellt.

### Schriftart-Rendering durch Inline-Ersetzung optimieren

Schriftartressourcen sind typischerweise statische Ressourcen, die nicht häufig aktualisiert werden. Aus diesem Grund eignen sie sich hervorragend für eine lange Ablaufdauer (max-age). Geben Sie sowohl einen [bedingten ETag-Header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags) als auch eine [optimale Cache-Control-Richtlinie](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) für alle Schriftartressourcen an.

#### Browser behaviors

Es ist nicht nötig, Schriftarten im localStorage oder mit anderen Methoden zu speichern - diese führen alle zu Leistungseinbußen. Der HTTP-Cachespeicher des Browsers stellt in Kombination mit dem Font Loading API bzw. der Webfontloader-Bibliothek das beste und stabilste Verfahren zur Übermittlung von Schriftartressourcen an den Browser dar.

<table>
  <thead>
    <tr>
      <th data-th="Browser">Browser</th>
      <th data-th="Timeout">Timeout</th>
      <th data-th="Fallback">Fallback</th>
      <th data-th="Swap">Swap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Browser">
        <strong>Chrome 35+</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Opera</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Firefox</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Internet Explorer</strong>
      </td>
      <td data-th="Timeout">
        0 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Safari</strong>
      </td>
      <td data-th="Timeout">
        No timeout
      </td>
      <td data-th="Fallback">
        N/A
      </td>
      <td data-th="Swap">
        N/A
      </td>
    </tr>
  </tbody>
</table>

* Safari hält das Rendern des Texts an, bis der Schriftartdownload abgeschlossen ist.
* Chrome und Firefox halten die Schriftartdarstellung bis zu 3 Sekunden an und nutzen dann eine Ausweich-Schriftart. Sobald der Schriftartdownload abgeschlossen ist, rendern sie den Text erneut mit der heruntergeladenen Schriftart.
* IE stellt sofort die Ausweich-Schriftart dar, wenn die angeforderte Schriftart noch nicht verfügbar ist und rendert diese erneut, sobald der Schriftartdownload abgeschlossen ist.

Im Gegensatz zur gängigen Meinung verzögern Webschriftarten nicht notwendigerweise die Seitendarstellung oder wirken sich negativ auf andere Leistungsmesswerte aus. Ein optimierter Einsatz von Schriftarten kann zu einer wesentlich besseren Nutzererfahrung beitragen: hervorragendes Branding sowie verbesserte Lesbarkeit, Nutzbarkeit und Durchsuchbarkeit bei gleichzeitiger Bereitstellung einer Lösung mit multiplen Auflösungen, die sich an alle Bildschirmformate anpasst. Schrecken Sie nicht vor der Verwendung von Webschriftarten zurück!

#### The font display timeline

Allerdings kann eine naive Implementierung zu großen Downloads und unnötigen Verzögerungen führen. Deshalb ist es erforderlich, das Optimierungs-Toolkit zu überarbeiten und den Browser bei der Optimierung der Schriftartressourcen und deren Abruf sowie bei der Verwendung auf unseren Seiten zu unterstützen.

1. **Schriftartnutzung überprüfen und überwachen:** Verwenden Sie nicht zu viele Schriftarten auf Ihren Seiten und minimieren Sie für jede Schriftart die Zahl der verwendeten Varianten. Dies ermöglicht die Bereitstellung einer einheitlicheren und schnelleren Nutzererfahrung.
2. **Schriftartressourcen unterteilen:** Viele Schriftarten können unterteilt oder in mehrere Unicode-Bereiche aufgeteilt werden, damit nur die Glyphen übertragen werden, die von einer bestimmten Seite benötigt werden. Damit wird die Dateigröße reduziert und die Downloadgeschwindigkeit der Ressource erhöht. Achten Sie bei der Festlegung der Untergruppen darauf, eine Optimierung für die erneute Schriftartverwendung durchzuführen, z. B., wenn Sie keinen anderen, überlappenden Zeichensatz auf den einzelnen Seiten herunterladen möchten. Es hat sich bewährt, eine Unterteilung auf Skriptbasis, z. B. für lateinische, kyrillische Zeichensätze usw., durchzuführen.
3. **Optimierte Schriftartformate für jeden Browser bereitstellen:** Jede Schriftart sollte in den Formaten WOFF2, WOFF, EOT und TTF geliefert werden. Wenden Sie die GZIP-Komprimierung auf die Formate EOT und TTF an, da diese standardmäßig nicht komprimiert werden.

Understanding these periods means you can use `font-display` to decide how your font should render depending on whether or when it was downloaded.

#### Using font-display

To work with the `font-display` property, add it your `@font-face` rules:

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  font-display: auto; /* or block, swap, fallback, optional */
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

`font-display` currently supports the following range of values: `auto | block | swap | fallback | optional`.

* **`auto`** uses whatever font display strategy the user-agent uses. Most browsers currently have a default strategy similar to `block`.

* **`block`** gives the font face a short block period (3s is recommended in most cases) and an infinite swap period. In other words, the browser draws "invisible" text at first if the font is not loaded, but swaps the font face in as soon as it loads. To do this the browser creates an anonymous font face with metrics similar to the selected font but with all glyphs containing no "ink." This value should only be used if rendering text in a particular typeface is required for the page to be usable.

* **`swap`** gives the font face a zero second block period and an infinite swap period. This means the browser draws text immediately with a fallback if the font face isn’t loaded, but swaps the font face in as soon as it loads. Similar to `block`, this value should only be used when rendering text in a particular font is important for the page, but rendering in any font will still get a correct message across. Logo text is a good candidate for **swap** since displaying a company’s name using a reasonable fallback will get the message across but you’d eventually use the official typeface.

* **`fallback`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a short swap period (three seconds is recommended in most cases). In other words, the font face is rendered with a fallback at first if it’s not loaded, but the font is swapped as soon as it loads. However, if too much time passes, the fallback will be used for the rest of the page’s lifetime. `fallback` is a good candidate for things like body text where you’d like the user to start reading as soon as possible and don’t want to disturb their experience by shifting text around as a new font loads in.

* **`optional`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a zero second swap period. Similar to `fallback`, this is a good choice for when the downloading font is more of a "nice to have" but not critical to the experience. The `optional` value leaves it up to the browser to decide whether to initiate the font download, which it may choose not to do or it may do it as a low priority depending on what it thinks would be best for the user. This can be beneficial in situations where the user is on a weak connection and pulling down a font may not be the best use of resources.

`font-display` is [gaining adoption](https://caniuse.com/#feat=css-font-rendering-controls) in many modern browsers. You can look forward to consistency in browser behavior as it becomes widely implemented.

### Wiederverwendung von Schriftarten mit HTTP-Caching optimieren

Used together, `<link rel="preload">` and the CSS `font-display` give developers a great deal of control over font loading and rendering, without adding in much overhead. But if you need additional customizations, and are willing to incur with the overhead introduced by running JavaScript, there is another option.

The [Font Loading API](https://www.w3.org/TR/css-font-loading/) provides a scripting interface to define and manipulate CSS font faces, track their download progress, and override their default lazyload behavior. For example, if you're sure that a particular font variant is required, you can define it and tell the browser to initiate an immediate fetch of the font resource:

    var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
      style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
    });
    
    // don't wait for the render tree, initiate an immediate fetch!
    font.load().then(function() {
      // apply the font (which may re-render text and cause a page reflow)
      // after the font has finished downloading
      document.fonts.add(font);
      document.body.style.fontFamily = "Awesome Font, serif";
    
      // OR... by default the content is hidden, 
      // and it's rendered after the font is available
      var content = document.getElementById("content");
      content.style.visibility = "visible";
    
      // OR... apply your own render strategy here... 
    });
    

Further, because you can check the font status (via the [check()](https://www.w3.org/TR/css-font-loading/#font-face-set-check)) method and track its download progress, you can also define a custom strategy for rendering text on your pages:

* CSS-Stylesheets mit passenden Medienabfragen werden vom Browser mit hoher Priorität automatisch heruntergeladen, weil sie für die Erstellung des CSSOM benötigt werden.
* Die Inline-Einbettung von Schriftartdaten in ein CSS-Stylesheet zwingt den Browser dazu, die Schriftart mit hoher Priorität herunterzuladen, ohne auf die Rendering-Baumstruktur zu warten und stellt somit eine manuelle Umgehung des standardmäßigen Lazy Loading-Verhaltens dar.
* You can use the fallback font to unblock rendering and inject a new style that uses the desired font after the font is available.

Best of all, you can also mix and match the above strategies for different content on the page. For example, you can delay text rendering on some sections until the font is available, use a fallback font, and then re-render after the font download has finished, specify different timeouts, and so on.

Note: The Font Loading API is still [under development in some browsers](http://caniuse.com/#feat=font-loading). Consider using the [FontLoader polyfill](https://github.com/bramstein/fontloader) or the [webfontloader library](https://github.com/typekit/webfontloader) to deliver similar functionality, albeit with even more overhead from an additional JavaScript dependency.

### Proper caching is a must

Font resources are, typically, static resources that don't see frequent updates. As a result, they are ideally suited for a long max-age expiry - ensure that you specify both a [conditional ETag header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), and an [optimal Cache-Control policy](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) for all font resources.

If your web application uses a [service worker](/web/fundamentals/primers/service-workers/), serving font resources with a [cache-first strategy](/web/fundamentals/instant-and-offline/offline-cookbook/#cache-then-network) is appropriate for most use cases.

You should not store fonts using [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) or [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API); each of those has its own set of performance issues. The browser's HTTP cache provides the best and most robust mechanism to deliver font resources to the browser.

## Optimierungscheckliste

Contrary to popular belief, the use of webfonts doesn't need to delay page rendering or have a negative impact on other performance metrics. The well-optimized use of fonts can deliver a much better overall user experience: great branding, improved readability, usability, and searchability, all while delivering a scalable multi-resolution solution that adapts well to all screen formats and resolutions. Don't be afraid to use webfonts.

That said, a naive implementation may incur large downloads and unnecessary delays. You need to help the browser by optimizing the font assets themselves and how they are fetched and used on your pages.

* **Audit and monitor your font use:** don't use too many fonts on your pages, and, for each font, minimize the number of used variants. This helps produce a more consistent and a faster experience for your users.
* **Subset your font resources:** many fonts can be subset, or split into multiple unicode-ranges to deliver just the glyphs that a particular page requires. This reduces the file size and improves the download speed of the resource. However, when defining the subsets, be careful to optimize for font re-use. For example, don't download a different but overlapping set of characters on each page. A good practice is to subset based on script: for example, Latin, Cyrillic, and so on.
* **Deliver optimized font formats to each browser:** provide each font in WOFF2, WOFF, EOT, and TTF formats. Make sure to apply GZIP compression to the EOT and TTF formats, because they are not compressed by default.
* **Give precedence to `local()` in your `src` list:** listing `local('Font Name')` first in your `src` list ensures that HTTP requests aren't made for fonts that are already installed.
* **Customize font loading and rendering using `<link rel="preload">`, `font-display`, or the Font Loading API:** default lazyloading behavior may result in delayed text rendering. These web platform features allow you to override this behavior for particular fonts, and to specify custom rendering and timeout strategies for different content on the page.
* **Specify revalidation and optimal caching policies:** fonts are static resources that are infrequently updated. Make sure that your servers provide a long-lived max-age timestamp and a revalidation token to allow for efficient font reuse between different pages. If using a service worker, a cache-first strategy is appropriate.

## Automated testing for web font optimization with Lighthouse {: #lighthouse }

[Lighthouse](/web/tools/lighthouse) can help automate the process of making sure that you're following web font optimization best practices. Lighthouse is an auditing tool built by the Chrome DevTools team. You can run it as a Node module, from the command line, or from the Audits panel of Chrome DevTools. You tell Lighthouse what URL to audit, and then it runs a bunch of tests on the page, and gives you a report of what the page is doing well, and how it can improve.

The following audits can help you make sure that your pages are continuing to follow web font optimization best practices over time:

* [Enable text compression](/web/tools/lighthouse/audits/text-compression)
* [Preload key requests](/web/tools/lighthouse/audits/preload)
* [Uses inefficient cache policy on static assets](/web/tools/lighthouse/audits/cache-policy)
* [All text remains visible during webfont loads](/web/updates/2016/02/font-display)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}