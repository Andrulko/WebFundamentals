project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Typografie is een fundamenteel onderdeel van goed design, merkbekendheid, leesbaarheid en toegankelijkheid. Met weblettertypen kunt u al deze criteria en meer realiseren: de tekst kan worden geselecteerd, er kan in de tekst worden gezocht, de tekst kan worden in- en uitgezoomd en is geschikt voor hoge resoluties waarbij de tekst consistent en scherp wordt weergegeven, ongeacht de schermomvang en -resolutie.

{# wf_updated_on: 2014-09-29 #} {# wf_published_on: 2014-09-19 #}

# Optimalisatie van weblettertypen {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*This article contains contributions from [Monica Dinculescu](https://meowni.ca/posts/web-fonts/), [Rob Dodson](/web/updates/2016/02/font-display), and Jeff Posnick.*

De optimalisatie van lettertypen is een essentieel onderdeel van het algehele resultaat. Elk lettertype is een extra hulpbron en sommige lettertypen kunnen weergave van de tekst verhinderen, maar het feit alleen dat de pagina weblettertypen gebruikt, wil nog niet zeggen dat de weergave langzamer moet zijn. Een geoptimaliseerd lettertype in combinatie met een goed afgewogen strategie voor het laden en toepassen op de pagina kan eraan bijdragen om de totale pagina-omvang te reduceren en pagina`s sneller weer te geven.

Een weblettertype is een verzameling gliefen, en elke glief is een vectorvorm die staat voor een letter of een symbool. De omvang van een lettertypebestand is daarom afhankelijk van twee variabelen: de complexiteit van de vectorpaden van elke glief en het aantal gliefen in het lettertype. Open Sans bijvoorbeeld, een van de populairste weblettertypen, bestaat uit 897 gliefen, waaronder tekens uit het Latijnse, Griekse en Cyrillische alfabet.

## Anatomie van een weblettertype

### TL;DR {: .hide-from-toc }

* Unicode-lettertypes kunnen duizenden gliefen bevatten
* Er zijn vier bestandsindelingen voor lettertypen: WOFF2, WOFF, EOT, TTF
* Voor sommige lettertype-indelingen is compressie met GZIP nodig

A *webfont* is a collection of glyphs, and each glyph is a vector shape that describes a letter or symbol. As a result, two simple variables determine the size of a particular font file: the complexity of the vector paths of each glyph and the number of glyphs in a particular font. For example, Open Sans, which is one of the most popular webfonts, contains 897 glyphs, which include Latin, Greek, and Cyrillic characters.

<img src="images/glyphs.png"  alt="Font glyph table" />

Lettertypen op internet moeten dus zorgvuldig worden ontwikkeld om ervoor te zorgen dat de typografie geen belemmering vormt voor de weergave van pagina`s. Internet biedt gelukkig alle noodzakelijke standaardbenodigdheden en later in deze leidraad zullen we bespreken hoe we het beste van beide kanten kunnen combineren.

Er bestaan tegenwoordig vier containerindelingen op internet: [EOT](http://en.wikipedia.org/wiki/Embedded_OpenType){: .external }, [TTF](http://nl.wikipedia.org/wiki/TrueType), [WOFF](http://en.wikipedia.org/wiki/Web_Open_Font_Format) en [WOFF2](http://www.w3.org/TR/WOFF2/). Ondanks de uitgebreide keuzemogelijkheid, bestaat er geen enkele universele indeling die werkt voor alle oude en nieuwe browsers: EOT is [alleen IE](http://caniuse.com/#feat=eot), TTF heeft [gedeeltelijke IE-ondersteuning](http://caniuse.com/#search=ttf), WOFF heeft de breedste ondersteuning, maar is [niet beschikbaar in sommige oudere browsers](http://caniuse.com/#feat=woff), en ondersteuning voor WOFF 2.0 is [voor de meeste browsers nog in ontwikkeling](http://caniuse.com/#feat=woff2).

### Bestandsindelingen voor lettertypen

Wat houdt dit voor ons in? Er is geen enkele indeling die in alle browsers werkt, wat betekent dat we meerdere indelingen moeten aanbieden voor een consistent resultaat:

Note: Er is in principe ook nog de [SVG `font container`](http://caniuse.com/svg-fonts), maar deze werd nooit ondersteund in IE of Firefox, en wordt nu ook afgewezen in Chrome. Deze is daarom van weining praktisch nut en wordt in deze leidraad weggelaten.

* Bied WOFF 2.0 aan als variant voor browsers die deze indeling ondersteunen
* Bied WOFF aan als variant voor het merendeel van de browsers
* Bied TTF aan als variant voor verouderde Android-browsers (lager dan 4.4)
* Bied EOT aan als variant voor verouderde IE-browsers (lager dan IE9) ^

Een lettertype is een verzameling gliefen, die elk bestaan uit paden die de vorm van de letter beschrijven. De afzonderlijke gliefen zijn natuurlijk verschillend, maar ze bevatten toch veel vergelijkbare informatie die kan worden gecomprimeerd met GZIP of een andere geschikte compressor:

### De omvang van lettertypen verlagen door compressie

Sommige lettertype-indelingen bevatten extra metadata, zoals informatie voor [hinten](http://nl.wikipedia.org/wiki/Hinten){: .external } en [overhang](http://nl.wikipedia.org/wiki/Overhang_(typografie)) die niet noodzakelijk is op sommige platformen, waardoor de bestandsgrootte verder kan worden geoptimaliseerd. Controleer uw lettertypecompressor op beschikbare optimalisatie-opties en zorg dat u over de juiste infrastructuur beschikt om deze geoptimaliseerde lettertypen aan elke afzonderlijke browser te leveren. Google Fonts bevat bijvoorbeeld meer dan 30 geoptimaliseerde varianten voor elk lettertype en ontdekt en levert automatisch de optimale variant voor elk platform en elke browser.

* De indelingen EOT en TTF worden standaard niet gecomprimeerd. Zorg ervoor dat uw servers zijn geconfigureerd voor toepassen van [GZIP-compressie](/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text-compression-with-gzip) wanneer deze bestandsindelingen worden aangeboden.
* WOFF heeft ingebouwde compressie - controleer of uw WOFF-compressor optimaal is ingesteld.
* WOFF2 maakt gebruik van aangepaste voorverwerkings- en compressie-algoritmen waarmee een ongeveer 30% grotere reductie wordt behaald in vergelijking tot andere bestandsindelingen - zie [rapport](http://www.w3.org/TR/WOFF20ER/){: .external }.

Note: Gebruik [Zopfli-compressie](http://en.wikipedia.org/wiki/Zopfli) voor de bestandsindelingen EOT, TTF en WOFF. Zopfli is een compressor die geschikt is voor zlib en biedt een ongeveer 5% grotere reductie in bestandsgrootte dan Gzip.

Met de @font-face CSS-regel kunnen we de locatie van een bepaalde lettertype-hulpbron definiëren, samen met de stijleigenschappen en de Unicode-codepoints waarvoor de hulpbron dient te worden gebruikt. Een combinatie van dergelijke @font-face-regels kan worden gebruikt om een `lettertypefamilie` samen te stellen die de browser gebruikt om vast te stellen welke lettertype-hulpbronnen moeten worden gedownload en toegepast op de actuele pagina. Laten we hier nader op ingaan.

## De lettertypefamilie definiëren met @font-face

### TL;DR {: .hide-from-toc }

* Gebruik de hint format() om meerdere lettertype-indelingen aan te geven
* Deel grote unicode-lettertypen in in deelverzamelingen en houd een handmatig in te stellen reservedeelverzameling achter de hand voor oudere browsers
* Verlaag het aantal stilistische lettertypen, zodat pagina''s en tekst beter kan worden weergegeven

Elke @font-face-regel bevat de naam van de lettertypefamilie, die fungeert als logische groep met meerdere regels [eigenschappen van het lettertype](http://www.w3.org/TR/css3-fonts/#font-prop-desc), zoals stijl, gewicht en stretch, en de [src-descriptor](http://www.w3.org/TR/css3-fonts/#src-desc) die een lijst met voorrangslocaties definieert voor de lettertype-hulpbron.

### Selectie van een lettertype

In bovenstaand voorbeeld wordt een enkele *Awesome Font-familie weergegeven met twee verschillende stijlen (normal en _italic*). Beide verwijzen naar een andere verzameling lettertype-hulpbronnen. Elke `src`-descriptor bevat een door een komma gescheiden lijst met voorrangsvarianten van hulpbronnen:

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
    

^ Note: Tenzij u verwijst naar een van de standaard systeemlettertypen, komt het zelden voor dat gebruikers Zopfli hebben geïnstalleerd, met name op mobiele apparaten waarop het onmogelijk is om extra lettertypen te installeren. U moet daarom altijd een lijst verstrekken met de locatie van externe lettertypen.

* de regel `local()` laat ons lokaal opgeslagen lettertypen laden, gebruiken en ernaar verwijzen.
* de regel `url()` laat ons externe lettertypen laden die de optionele hint `format()` kunnen hebben om de bestandsindeling van het lettertype aan te geven waarnaar in de URL wordt verwezen.

Wanneer de browser vaststelt dat het lettertype nodig is, gaat deze de lijst met hulpbronnen in de aangegeven volgorde af en probeert de browser de juiste hulpbron te laden. Zoals in bovenstaand voorbeeld:

Dankzij de combinatie van lokale en externe regels met hints voor de juiste bestandsindeling kunnen we alle beschikbare lettertype-indelingen specificeren en doet de browser de rest: de browser stelt vast welke hulpbronnen nodig zijn en selecteert voor ons de optimale bestandsindeling.

1. Browser geeft de lay-out van de pagina weer en bepaalt welke varianten van de lettertypes nodig zijn om de tekst op de pagina weer te geven.
2. Voor elk benodigd lettertype controleert de browser of het lettertype lokaal aanwezig is.
3. Als het bestand niet lokaal is opgeslagen, zoekt de browser naar externe definities: 
    * Als een hint aanwezig is, controleert de browser vóór de download of deze het aanbevolen lettertype ondersteunt en gaat anders door naar het volgende lettertype.
    * Als er geen hints voor bestandsindelingen zijn, downloadt de browser de hulpbron.

Note: De volgorde waarin de verschillende lettertypen worden genoemd is van belang. De browser kiest namelijk het eerste lettertype dat wordt ondersteund. Wanneer u wilt dat nieuwe browsers WOFF2 gebruiken, moet u WOFF2 boven WOFF plaatsen, enzovoort.

Naast eigenschappen van het lettertype als stijl, gewicht en stretch kunnen we aan de hand van de @font-face-regel ook een verzameling Unicode-codepoints definiëren die door elke afzonderlijke hulpbron worden ondersteund. Hierdoor kunnen we een groot Unicode-lettertype opsplitsen in kleinere deelverzamelingen (bijv. Latijnse, Griekse en Cyrillische deelverzamelingen) en hoeven we alleen de gliefen te downloaden die nodig zijn om de tekst op een bepaalde pagina weer te geven.

### Deelverzameling Unicode-bereik

De [descriptor voor het Unicode-bereik](http://www.w3.org/TR/css3-fonts/#descdef-unicode-range) laat ons een lijst met door een komma gescheiden waarden specificeren. Elke waarde kan een van drie vormen aannemen:

We kunnen onze _Awesome Font-familie bijvoorbeeld opsplitsen in Latijnse en Japanse deelverzamelingen die elk door de browser worden gedownload wanneer dit nodig is:

* Enkel codepoint (bijv. U+416)
* Intervalbereik (bijv. U+400-4ff): bevat de codepoints voor begin en eind van het bereik
* Wildcard (bijv. U+4??): '?' de tekens staan voor hexadecimale getallen

Note: Deelverzamelingen voor unicode-lettertypen zijn met name belangrijk voor Aziatische talen. Het aantal gliefen is namelijk een stuk groter dan in Westerse talen en een volledig lettertype neemt al gauw enkele megabytes in beslag, in plaats van enkele tientallen kilobytes.

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
    

Omdat we deelverzamelingen voor Unicode-bereik en aparte bestanden gebruiken voor elke stilistische variant van het lettertype, kunnen we een lettertypefamilie gebruiken die zowel sneller, als efficiënter kan worden gedownload - de bezoeker downloadt alleen de varianten en deelverzamelingen die nodig zijn en hoeft geen deelverzamelingen te downloaden die hij of zij toch nooit ziet of gebruikt op de pagina.

Er is een klein nadeel met Unicode-bereik: [het wordt nog niet door alle browsers ondersteund](http://caniuse.com/#feat=font-unicode-range). Sommige browsers negeren de hint voor Unicode-bereik en downloaden alle varianten, andere verwerken de @font-face-regel helemaal niet. Om dit op te lossen, moeten we voor oudere browsers terugvallen op de "handmatige deelverzameling".

Oudere browsers zijn niet slim genoeg om alleen de benodigde deelverzamelingen te selecteren en kunnen geen samengesteld lettertype maken. We moeten daarom een enkele lettertype-hulpbron gebruiken die alle benodigde deelverzamelingen bevat, en de rest moeten we voor de browser verbergen. Als de pagina bijvoorbeeld alleen Latijnse tekens gebruikt, kunnen we andere gliefen verwijderen en kunnen we die deelverzameling aanbieden als alleenstaande hulpbron.

Elke lettertypefamilie bestaat uit meerdere stilistische varianten (normaal, vet, cursief) en meerdere gewichten voor elke stijl die elk zeer verschillende gliefvormen kunnen hebben - bijv. verschillende spaties, grootte, of een geheel andere vorm.

1. **Hoe bepalen we welke deelverzamelingen nodig zijn?** 
    * Wanneer deelverzamelingen voor Unicode-bereik worden ondersteund door de browser, selecteert deze automatisch de juiste deelverzameling. De pagina hoeft alleen de bestanden van de deelverzameling te verstrekken en het juiste Unicode-bereik te specificeren in de @font-face-regels.
    * Wanneer Unicode-bereik niet wordt ondersteund, moet de pagina alle onnodige deelverzamelingen verbergen - d.w.z. de ontwikkelaar moet de vereiste deelverzamelingen specificeren.
2. **Hoe genereren we deelverzamelingen van lettertypen?** 
    * Gebruik de open source [pyftsubset-tool](https://github.com/behdad/fonttools/blob/master/Lib/fontTools/subset.py#L16) om deelverzamelingen te maken en uw lettertypen te optimaliseren.
    * Bij sommige lettertypeservices kunt u handmatig deelverzamelingen maken aan de hand van aangepaste zoekparameters. Op deze manier kunt u handmatig de vereiste deelverzameling voor uw pagina specificeren. Raadpleeg de documentatie van de leverancier van uw lettertypen.

### Selectie en synthese van lettertypen

Each font family is composed of multiple stylistic variants (regular, bold, italic) and multiple weights for each style, each of which, in turn, may contain very different glyph shapes&mdash;for example, different spacing, sizing, or a different shape altogether.

<img src="images/font-weights.png"  alt="Font weights" />

Iets vergelijkbaars geldt voor varianten van *italic*. De ontwerper van het lettertype bepaalt welke varianten worden gemaakt, en wij bepalen welke varianten we op onze pagina gebruiken - elke variant moet apart worden gedownload, dus het is een goed idee om het aantal varianten beperkt te houden. We kunnen bijvoorbeeld twee varianten definiëren voor onze _Awesome Font-familie:

> Wanneer een gewicht wordt opgegeven waarvoor geen lettertype bestaat, wordt het gewicht gebruikt dat het meest in de buurt komt. Grote gewichten corresponderen over het algemeen met lettertypen met grotere gewichten en lichte gewichten met lettertypen met lichtere gewichten.
> 
> > [Algoritme voor corresponderende CSS3-lettertypen](http://www.w3.org/TR/css3-fonts/#font-matching-algorithm)

Bovenstaand voorbeeld beschrijft de _Awesome Font-familie die is samengesteld uit twee hulpbronnen met dezelfde verzameling Latijnse gliefen (U+000-5FF) maar die twee verschillende "gewichten" heeft: normaal (400) en vet (700). Wat gebeurt er als een van onze CSS-regels een ander lettertypegewicht specificeert, of de eigenschap van de lettertypestijl instelt op cursief?

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

* Als geen lettertype wordt gevonden dat exact overeenkomt, kiest de browser de overeenkomst die het meest in de buurt komt.
* Als er geen stilistische overeenkomst wordt gevonden (bijv. we hebben in bovenstaand voorbeeld geen cursieve varianten beschreven), synthetiseert de browser zijn eigen lettertypevariant.

<img src="images/font-synthesis.png"  alt="Font synthesis" />

Note: Voor de beste consistentie en visuele resultaten kunt u beter geen gebruikmaken van lettertype-synthese. Beperk het aantal gebruikte lettertypen en geef de locatie van de lettertypen op, zodat de browser de lettertypen kan downloaden wanneer deze op de pagina worden gebruikt. In sommige gevallen kan een synthetische variant [een optie zijn](https://www.igvita.com/2014/09/16/optimizing-webfont-selection-and-synthesis/) - gebruik deze met zorg.

Een volledig weblettertype met alle stilistische varianten, die we mogelijk niet nodig hebben, plus alle gliefen, die we mogelijk niet gebruiken, kan al snel oplopen tot een download van meerdere megabytes. De @font-face CSS-regel is speciaal ontwikkeld om de lettertypefamilie op te splitsen in een verzameling hulpbronnen: Unicode-deelverzamelingen, stijlvarianten, enzovoort.

Aan de hand van deze regels bepaalt de browser welke deelverzamelingen en varianten nodig zijn en downloadt vervolgens het kleinste aantal benodigde gegevens om de tekst weer te geven. Dit is erg nuttig, maar als we niet oppassen, kan dit problemen opleveren met het kritieke render-pad en kan dit leiden tot vertraagde tekstweergave - iets dat we absoluut willen vermijden.

## Laden en renderen optimaliseren

### TL;DR {: .hide-from-toc }

* Verzoeken om lettertypen worden pas behandeld wanneer de render-boom is opgebouwd, wat tot vertraagde tekstweergave kan leiden
* In de Font Loading API kunnen we strategieën voor aangepaste lettertypen en rendering implementeren die voorrang krijgen boven standaard 'lazyload' laden van lettertypen

Wanneer we lettertypen `lazyloaden`, is er een belangrijk verborgen aspect dat de tekstweergave kan vertragen: de browser moet [de render-boom samenstellen](/web/fundamentals/performance/critical-rendering-path/render-tree-construction), die afhankelijk is van de DOM- en CSSOM-bomen. Pas dan weet de browser welke lettertype-hulpbronnen nodig zijn om de tekst weer te geven. Verzoeken om lettertypen worden dus pas na andere kritieke hulpbronnen behandeld, en de browser wordt mogelijk verhinderd om de tekst weer te geven tot de benodigde hulpbron is opgehaald.

Given these declarations, the browser figures out the required subsets and variants and downloads the minimal set required to render the text, which is very convenient. However, if you're not careful, it can also create a performance bottleneck in the critical rendering path and delay text rendering.

### Weblettertypen en het kritieke render-pad

Door de volgorde waarin de pagina wordt ingevuld en weergegeven, van de eerste inhoud die kort na samenstelling van de render-boom wordt weergegeven, tot het verzoek om het lettertype, veroorzaakt `lege tekst`, wanneer de browser de lay-out gereed heeft, maar nog moet wachten om de tekst in te vullen. Dit gedrag verschilt per browser:

<img src="images/font-crp.png"  alt="Font critical rendering path" />

1. Browser verzoekt om een HTML-document
2. Browser begint HTML-antwoord te parsen en de DOM samen te stellen
3. Browser ontdekt CSS, JS en andere hulpbronnen en verstuurt verzoeken
4. Browser stelt de CSSOM samen wanneer alle CSS-inhoud is ontvangen en combineert dit met de DOM-boom om de render-boom samen te stellen 
    * Verzoeken om lettertypen worden verstuurd wanneer de render-boom aangeeft welke lettertypevarianten nodig zijn om de gespecificeerde tekst op de pagina weer te geven
5. Browser geeft lay-out weer en vult scherm op met inhoud 
    * Als het lettertype nog niet beschikbaar is, kan de browser geen tekstpixels weergeven
    * Wanneer het lettertype beschikbaar is, vult de browser de tekstpixels in

[Font Loading API](http://dev.w3.org/csswg/css-font-loading/){: .external } biedt een script-interface waarin CSS-lettertypen kunnen worden gedefinieerd en aangepast, de downloadvoortgang kan worden bijgehouden en het standaard "lazyload"-gedrag kan worden gewijzigd. Als we bijvoorbeeld zeker weten dat een bepaalde lettertypevariant nodig is, kunnen we deze definiëren en de browser instrueren om de lettertype-hulpbron onmiddellijk op te halen:

Voorts kunnen we de status van het lettertype controleren (via [check()](http://dev.w3.org/csswg/css-font-loading/#font-face-set-check)) en de downloadvoortgang bijhouden, waardoor we ook een aangepaste strategie kunnen definiëren voor de tekstweergave op onze pagina's:

### Tekstweergave optimaliseren met Font Loading API

We kunnen bovenstaande strategieën bovendien ook combineren voor verschillende inhoud op de pagina - bijv. tekstweergave onderbreken in sommige onderdelen tot het lettertype beschikbaar is, een handmatig lettertype gebruiken en de tekst opnieuw weergeven wanneer het nieuwe lettertype is gedownload, verschillende time-outs opgeven, enzovoort.

Note: Font Loading API is [voor sommige browsers nog in ontwikkeling](http://caniuse.com/#feat=font-loading). Gebruik de [FontLoader polyfill](https://github.com/bramstein/fontloader), of de [webfontloader-bibliotheek](https://github.com/typekit/webfontloader) om vergelijkbare functionaliteit te leveren, met als nadeel dat u afhankelijk bent van JavaScript.

Een eenvoudige alternatieve strategie voor Font Loading API om lege tekst op te lossen, is de inhoud van het lettertype invoeren in een CSS-stylesheet:

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

De invoerstrategie is niet zo flexibel en we kunnen geen aangepaste time-outs instellen of weergavestrategieën maken voor verschillende inhoud, maar het is een eenvoudige en degelijke oplossing die werkt voor alle browsers. Voor het beste resultaat werkt u ingevoerde lettertypen uit op zelfstandige stylesheets en biedt u deze aan met een lange `max-age` - zo dwingt u uw bezoekers niet om de lettertypen opnieuw te downloaden wanneer u uw CSS bijwerkt.

Note: Maak selectief gebruik van `inlining`. De reden dat @font-face gebruikmaakt van `lazyload` is om downloaden van overbodige lettertypevarianten en deelverzamelingen te voorkomen. Als u de omvang van uw CSS via agressieve `inlining` vergroot, heeft dit negatieve gevolgen voor uw [kritieke render-pad](/web/fundamentals/performance/critical-rendering-path/) - de browser moet alle CSS downloaden voordat deze de CSSOM en render-boom kan opstellen, en pagina-inhoud op het scherm kan weergeven.

### Tekstweergave optimaliseren met `inlining`

Lettertype-hulpbronnen zijn meestal statische hulpbronnen die weinig updates krijgen. Ze zijn daarom ideaal voor een lange `max-age` - zorg ervoor dat u zowel een [voorwaardelijke ETag-header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), als een [optimaal Cache-Control-beleid](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) specificeert voor alle lettertype-hulpbronnen.

#### Browser behaviors

U hoeft lettertypen niet op te slaan in het lokale geheugen of via andere methoden - deze hebben elk op hun beurt enige nadelen voor het resultaat. De HTTP-cache van de browser, samen met Font Loading API of de webfontloader-bibliotheek bieden de beste en degelijkste methode om lettertype-hulpbronnen aan de browser te leveren.

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

* Safari wacht met de tekstweergave tot het lettertype is gedownload.
* Chrome en Firefox wachten 3 seconden, waarna ze terugvallen op het handmatige lettertype. Wanneer het lettertype is gedownload, wordt de tekst opnieuw weergegeven in het nieuwe lettertype.
* IE geeft de tekst direct met het handmatige lettertype weer als het opgevraagde lettertype nog niet beschikbaar is, en geeft de tekst weer met het nieuwe lettertype wanneer dit is gedownload.

Het is een misverstand dat weblettertypen per definitie de paginaweergave vertragen of negatieve gevolgen hebben voor andere resultaten. Goed geoptimaliseerd gebruik van lettertypen kan een veel gebruiksvriendelijker resultaat opleveren: geweldige merkbekendheid, verbeterde leesbaarheid, gebruiksgemak en zoekfunctie. Daarnaast levert u een schaalbare oplossing met meerder resoluties die zich aanpast aan schermen van alle afmetingen en resoluties. Wees niet bang om weblettertypen te gebruiken.

#### The font display timeline

Gebruik ze wel met verstand, anders krijgt u te maken met grote downloads en onnodige vertragingen. We moeten de browser helpen door de lettertype-items zelf te optimaliseren, en daarnaast te optimaliseren hoe ze worden opgehaald en gebruikt op onze pagina`s.

1. **Controleer en monitor hoe u lettertypen gebruikt:** gebruik niet te veel lettertypen op uw pagina`s en beperk het aantal varianten voor elk lettertype. Op die manier kunt u uw pagina`s consistenter en sneller aan uw gebruikers leveren.
2. **Splits uw lettertype-hulpbronnen op in deelverzamelingen:** veel lettertypen kunnen worden ingedeeld in deelverzamelingen of in meerdere Unicode-bereiken, zodat u precies de gliefen levert die voor een bepaalde pagina nodig zijn. Hierdoor beperkt u de bestandsgrootte en verbetert u de downloadsnelheid van de hulpbron. Als u deelverzamelingen maakt, kunt u deze het beste direct optimaliseren voor hergebruik: u wilt niet op elke pagina een andere verzameling met overlappende tekens downloaden. Een goede werkwijze is om deelverzamelingen te maken op basis van alfabetten - bijv. Latijns, Cyrillisch, enzovoort.
3. **Lever lettertypen voor elke browser in geoptimaliseerde bestandsindelingen:** elk lettertype moet worden geleverd in de indelingen WOFF2, WOFF, EOT en TTF. Comprimeer EOT- en TTF-bestanden met GZIP, want deze bestandsindelingen worden standaard niet gecomprimeerd.

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

### Hergebruik van lettertypen optimaliseren met HTTP-caching

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

* CSS-stylesheets met corresponderende zoekopdrachten voor media worden automatisch met hoge prioriteit gedownload door de browser, omdat deze vereist zijn om de CSSOM samen te stellen.
* Door de gegevens van het lettertype in te voeren in een CSS-stylesheet wordt de browser gedwongen om het lettertype met hoge prioriteit te downloaden, zonder te wachten op de render-boom - dit werkt als een handmatige ingreep waarmee het standaard `lazyload`-gedrag wordt overschreven.
* You can use the fallback font to unblock rendering and inject a new style that uses the desired font after the font is available.

Best of all, you can also mix and match the above strategies for different content on the page. For example, you can delay text rendering on some sections until the font is available, use a fallback font, and then re-render after the font download has finished, specify different timeouts, and so on.

Note: The Font Loading API is still [under development in some browsers](http://caniuse.com/#feat=font-loading). Consider using the [FontLoader polyfill](https://github.com/bramstein/fontloader) or the [webfontloader library](https://github.com/typekit/webfontloader) to deliver similar functionality, albeit with even more overhead from an additional JavaScript dependency.

### Proper caching is a must

Font resources are, typically, static resources that don't see frequent updates. As a result, they are ideally suited for a long max-age expiry - ensure that you specify both a [conditional ETag header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), and an [optimal Cache-Control policy](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) for all font resources.

If your web application uses a [service worker](/web/fundamentals/primers/service-workers/), serving font resources with a [cache-first strategy](/web/fundamentals/instant-and-offline/offline-cookbook/#cache-then-network) is appropriate for most use cases.

You should not store fonts using [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) or [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API); each of those has its own set of performance issues. The browser's HTTP cache provides the best and most robust mechanism to deliver font resources to the browser.

## Optimalisatiechecklist

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