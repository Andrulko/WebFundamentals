project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Voordat de inhoud op het scherm kan worden weergegeven, moet de browser de DOM- en CSSOM-boomstructuren opbouwen. Daarom moeten we ervoor zorgen dat zowel de HTML als het CSS zo snel mogelijk aan de browser worden geleverd.

{# wf_updated_on: 2014-09-11 #} {# wf_published_on: 2014-03-31 #}

# Het Objectmodel opbouwen {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Voordat de inhoud op het scherm kan worden weergegeven, moet de browser de DOM- en CSSOM-boomstructuren opbouwen. Daarom moeten we ervoor zorgen dat zowel de HTML als het CSS zo snel mogelijk aan de browser worden geleverd.

### TL;DR {: .hide-from-toc }

- Bytes → tekens → tokens → nodes → objectmodel.
- HTML-opmaak wordt verwerkt in een Documentobjectmodel (DOM), CSS-opmaak wordt verwerkt in een CSS-objectmodel (CSSOM).
- DOM en CSSOM zijn onafhankelijke gegevensstructuren.
- De Timeline (Tijdlijn) in Chrome DevTools biedt ons de mogelijkheid om de opbouw en verwerking van het DOM en CCSOM vast te leggen en te controleren.

## Documentobjectmodel (DOM)

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Laten we beginnen met het meest eenvoudige geval: een platte HTML-pagina met een beetje tekst en één afbeelding. Wat heeft de browser nodig om deze eenvoudige pagina te verwerken?

Let’s start with the simplest possible case: a plain HTML page with some text and a single image. How does the browser process this page?

<img src="images/full-process.png" alt="DOM-boomstructuur" />

1. **Conversie:** de browser leest de onbewerkte bytes van de HTML van de schijf of van het netwerk en vertaalt deze naar individuele tekens op basis van de codering die in het bestand wordt opgegeven (bijvoorbeeld UTF-8).
2. **Tokens maken:** de browser converteert tekenstrings naar specifieke tokens, zoals deze door de [W3C HTML5-standaard](http://www.w3.org/TR/html5/){: .external } worden voorgeschreven (bijvoorbeeld `<html>`, `<body>` en andere strings in `punthaken`). Elke token heeft een speciale betekenis en een eigen set met regels.
3. **Lexeren:** de ontstane tokens worden geconverteerd in `objecten` waarmee de eigenschappen en regels worden gedefinieerd.
4. **DOM-opbouw:** omdat de HTML-opmaak de relatie tussen de verschillende tags (bepaalde tags staan binnen andere tags) definieert, worden tot slot de gemaakte objecten aan elkaar verbonden in een boomstructuur waarin ook de ouder-kindrelatie van de oorspronkelijke opmaak wordt gedefinieerd: het object *HTML* is een ouder van het object *body*, *body* is een ouder van het object *paragraph* enzovoorts.

<img src="images/dom-tree.png"  alt="DOM tree" />

**The final output of this entire process is the Document Object Model (DOM) of our simple page, which the browser uses for all further processing of the page.**

Every time the browser processes HTML markup, it goes through all of the steps above: convert bytes to characters, identify tokens, convert tokens to nodes, and build the DOM tree. This entire process can take some time, especially if we have a large amount of HTML to process.

<img src="images/dom-timeline.png"  alt="Tracing DOM construction in DevTools" />

Als u Chrome DevTools opent en een tijdlijn opneemt terwijl de pagina wordt geladen, kunt u de werkelijke tijd zien die nodig is om deze stap uit te voeren: in het bovenstaande voorbeeld kostte het ongeveer 5 ms om een stuk HTML-bytes te converteren naar een DOM-boomstructuur. Als de pagina natuurlijk groter was geweest, zoals de meeste pagina`s zijn, kan dit proces aanzienlijk langer duren. In de volgende onderdelen zult u zien hoe u vloeiendere animaties kunt maken, aangezien dit al snel een knelpunt kan worden wanneer de browser grote hoeveelheden HTML moet verwerken.

Hebben we genoeg informatie om de pagina op het scherm weer te geven wanneer de DOM-boomstructuur klaar is? Nog niet. De DOM-boomstructuur legt de eigenschappen en relaties van de documentopmaak vast, maar het zegt niets over hoe de elementen eruit moeten komen te zien wanneer deze worden weergegeven. Dat is de verantwoordelijkheid van het CSSOM. Dit onderwerp gaan we nu behandelen.

Terwijl de browser de DOM-boomstructuur van onze eenvoudige pagina heeft opgebouwd, kwam deze een linktag in het hoofdgedeelte van het document tegen, die verwees naar een extern CSS-stijlblad: style.css. De browser ging ervan uit dat deze bron nodig was om de pagina weer te geven en heeft daarom gelijk een aanvraag voor dit document uitgezonden. Deze aanvraag kwam met de volgende inhoud terug:

## CSS-objectmodel (CSSOM)

We hadden onze stijlen direct in de HTML-opmaak (inline) kunnen opgeven, maar door het CSS onafhankelijk van de HTML te houden, kunnen we de inhoud en het ontwerp als afzonderlijke taken behandelen: ontwerpers kunnen werken aan het CSS en ontwikkelaars kunnen zich richten op de HTML.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/style.css" region_tag="full" adjust_indentation="auto" %}
</pre>

Net als bij de HTML moeten de ontvangen CSS-regels worden geconverteerd in iets waarmee de browser kan werken. Daarom wordt een vergelijkbaar proces als het HTML-proces herhaald:

As with HTML, we need to convert the received CSS rules into something that the browser can understand and work with. Hence, we repeat the HTML process, but for CSS instead of HTML:

<img src="images/cssom-construction.png"  alt="CSSOM construction steps" />

The CSS bytes are converted into characters, then tokens, then nodes, and finally they are linked into a tree structure known as the "CSS Object Model" (CSSOM):

<img src="images/cssom-tree.png"  alt="CSSOM tree" />

Bekijk de CSSOM-boomstructuur hierboven voor een concreet voorbeeld. Alle tekst binnen de tag *span* die binnen het body-element wordt geplaatst, heeft een lettertypegrootte van 16 pixels en heeft rode tekst: de richtlijn voor de lettertypegrootte wordt vanaf de body trapsgewijs toegepast tot de span-tag. Als een span-tag echter het kind is van een paragraaftag (p), wordt de inhoud ervan niet weergegeven.

Merk ook op dat de bovenstaande boomstructuur niet de gehele CSSOM-boomstructuur is en alleen de stijlen weergeeft die we hebben overschreven in ons stijlblad. Elke browser levert een standaardset met stijlen. Dit wordt het `stijlblad gebruikersagent` (user agent style sheet) genoemd. Dit zijn de stijlen die u ziet wanneer u geen eigen stijlblad opgeeft. Onze eigen stijlen overschrijven gewoon deze standaardstijlen (bijvoorbeeld [standaard IE-stijlen](http://www.iecss.com/){: .external }). Heeft u ooit de `computed styles` (berekende stijlen) in Chrome DevTools bekeken en u afgevraagd waar al deze stijlen vandaan komen? Dat weet u dan nu.

Bent u ook nieuwsgierig hoe lang de verwerking van het CSS duurde? Neem een tijdlijn op in Chrome DevTools en zoek de gebeurtenis `Recalculate Style` (Stijl herberekenen): anders dan bij DOM-parsering geeft de tijdlijn geen aparte invoer `Parse CSS` (CSS parseren), maar legt de tijdlijn parseren en opbouwen van de CSSOM-boomstructuur, plus de recursieve berekening van de berekende stijlen onder deze enkele gebeurtenis vast.

To find out how long the CSS processing takes you can record a timeline in DevTools and look for "Recalculate Style" event: unlike DOM parsing, the timeline doesn’t show a separate "Parse CSS" entry, and instead captures parsing and CSSOM tree construction, plus the recursive calculation of computed styles under this one event.

<img src="images/cssom-timeline.png"  alt="Tracing CSSOM construction in DevTools" />

Our trivial stylesheet takes ~0.6ms to process and affects eight elements on the page&mdash;not much, but once again, not free. However, where did the eight elements come from? The CSSOM and DOM are independent data structures! Turns out, the browser is hiding an important step. Next, lets talk about the [render tree](/web/fundamentals/performance/critical-rendering-path/render-tree-construction) that links the DOM and CSSOM together.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}