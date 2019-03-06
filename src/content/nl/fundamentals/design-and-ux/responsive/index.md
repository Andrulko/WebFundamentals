project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Veel websites zijn niet geoptimaliseerd voor ervaringen op meerdere apparaten. Ontdek de basisbeginselen zodat uw website werkt op mobiele apparaten, desktops of iets anders met een scherm.

{# wf_updated_on: 2014-04-29 #} {# wf_published_on: 2000-01-01 #}

# Basisbeginselen voor responsive webdesign {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Het gebruik van mobiele apparaten om op internet te browsen neemt enorm snel toe. Veel websites zijn echter nog niet geoptimaliseerd voor die mobiele apparaten. Mobiele apparaten zijn vaak beperkt door de grootte van het scherm en vereisen een andere aanpak voor de manier waarop de inhoud op het scherm wordt weergegeven.

{% include "web/_shared/udacity/ud893.html" %}

<video autoplay muted loop controls>
  <source src="videos/resize.webm" type="video/webm">
  <source src="videos/resize.mp4" type="video/mp4">
</video>

Er bestaan heel veel verschillende schermformaten op telefoons, `phablets`, tablets, desktops, gameconsoles, tv`s, zelfs wearables. De schermformaten zullen altijd wijzigen en daarom is het belangrijk dat uw website zich aan elk formaat kan aanpassen, nu en in de toekomst.

Responsive webdesign, oorspronkelijk gedefinieerd door [Ethan Marcotte in A List Apart](http://alistapart.com/article/responsive-web-design/){: .external } beantwoordt aan de behoeften van de gebruikers en de apparaten die zij gebruiken. De lay-out verandert op basis van de grootte en mogelijkheden van het apparaat. Op een telefoon zien gebruikers inhoud bijvoorbeeld in één kolom, terwijl een tablet dezelfde inhoud misschien in twee kolommen weergeeft.

## De viewport instellen

Pagina's die voor verschillende apparaten zijn geoptimaliseerd moeten een meta-viewport-element bevatten in de kop van het document. Een metatag voor viewport geeft de browser instructies over de manier waarop de afmetingen en schaalbaarheid van de pagina moeten worden beheerd.

### TL;DR {: .hide-from-toc }

- Gebruik metatags voor de viewport om de breedte en schaling van de browserviewport te bepalen.
- Neem `width=device-width` op om de breedte van het scherm af te stemmen in apparaatonafhankelijke pixels.
- Neem `initial-scale=1` op om een 1:1-verhouding te verkrijgen tussen CSS-pixels en apparaatonafhankelijke pixels.
- Zorg ervoor dat uw pagina toegankelijk is door schaling voor gebruikers niet uit te schakelen.

In een poging om de beste ervaring te leveren geven mobiele browsers de pagina weer met de breedte van een desktopscherm (doorgaans ongeveer 980 pixels, maar dit varieert per apparaat) en proberen ze de inhoud dan beter weer te geven door de tekengrootte te vergroten en de inhoud aan het scherm aan te passen. Gebruikers zien dat de tekengrootten niet consistent zijn en moeten dubbeltikken of knijpen om te zoomen zodat ze de inhoud kunnen zien en er op kunnen reageren.

    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    

Door het gebruik van de metawaarde voor viewport `width=device-width` krijgt de pagina de instructie om de breedte van het scherm af te stemmen in apparaatonafhankelijke pixels. Hierdoor kan de pagina de inhoud dynamisch aanpassen zodat deze in verschillende schermformaten past, zowel op een kleine mobiele telefoon als op een groot desktopscherm.

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

Some browsers keep the page's width constant when rotating to landscape mode, and zoom rather than reflow to fill the screen. Adding the attribute `initial-scale=1` instructs browsers to establish a 1:1 relationship between CSS pixels and device-independent pixels regardless of device orientation, and allows the page to take advantage of the full landscape width.

Wanneer naar de liggende modus wordt geschakeld, behouden sommige browsers de breedte van de pagina en zoomen ze in op de inhoud in plaats van deze dynamisch aan te passen om het scherm te vullen. Het kenmerk `initial-scale=1` geeft browsers de instructie om een 1:1-verhouding in te stellen tussen CSS-pixels en apparaatonafhankelijke pixels, ongeacht de apparaatoriëntatie. De pagina kan dan optimaal gebruikmaken van de volledige liggende breedte.

### TL;DR {: .hide-from-toc }

Note: Gebruik een komma voor het scheiden van kenmerken, zodat ook oudere browsers de kenmerken goed kunnen parseren.

- `minimum-scale`
- `maximum-scale`
- `user-scalable`

Naast het instellen van een `initial-scale` kunt u ook de volgende kenmerken voor de viewport instellen:

## Zorg voor een toegankelijke viewport

Wanneer deze zijn ingesteld, kunnen ze ervoor zorgen dat de gebruiker niet kan in- en uitzoomen op de viewport, waardoor toegankelijkheidsproblemen kunnen ontstaan.

### TL;DR {: .hide-from-toc }

- Gebruik geen grote elementen met een vaste breedte.
- Inhoud mag voor een goede weergave niet afhankelijk zijn van een specifieke viewportbreedte.
- Gebruik CSS-mediaquery`s om voor kleine en grote schermen een andere styling toe te passen.

Op desktops en mobiele apparaten scrollen gebruikers altijd verticaal op websites, niet horizontaal. Als u de gebruiker verplicht om horizontaal te scrollen of uit te zoomen om de hele pagina te kunnen zien, zal de gebruikerservaring negatief zijn.

Wanneer u een mobiele website met een `meta viewport`-tag ontwikkelt, maakt u gemakkelijk pagina-inhoud die niet helemaal binnen de opgegeven viewport past. Een afbeelding die breder wordt weergegeven dan de viewport kan er bijvoorbeeld voor zorgen dat de gebruiker horizontaal moet scrollen in de viewport. U moet deze inhoud aanpassen zodat deze in de breedte van de viewport past, zodat de gebruiker niet horizontaal hoeft te scrollen.

Omdat de afmetingen en breedte van het scherm in CSS-pixels sterk variëren per apparaat (bijv. tussen telefoons en tablets, en zelfs tussen verschillende telefoons), mag inhoud voor een goede weergave niet afhankelijk zijn van een specifieke viewportbreedte.

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

## Inhoud aanpassen aan de viewport

Als u voor pagina-elementen grote absolute CSS-breedtes instelt (zoals het onderstaande voorbeeld), zal de `div` te breed zijn voor de viewport op een smaller scherm (bijv. een apparaat met een breedte van 320 CSS-pixels, zoals een iPhone). U kunt relatieve breedtewaarden gebruiken, zoals `width: 100%`. Wees voorzichtig bij het gebruik van grote absolute positiewaarden die ervoor kunnen zorgen dat het element op kleine schermen buiten de viewport valt.

### Mediaquery`s toepassen op basis van de grootte van de viewport

- Met mediaquery's kunt u stijlen toepassen op basis van apparaatkenmerken.
- Gebruik `min-width` in plaats van `min-device-width` voor het meest brede resultaat.
- Gebruik relatieve grootten voor elementen om te voorkomen dat de lay-out wordt onderbroken.

For example, you could place all styles necessary for printing inside a print media query:

    <link rel="stylesheet" href="print.css" media="print">
    

Mediaquery's zijn eenvoudige filters die op CSS-stijlen kunnen worden toegepast. Hiermee kunt u vlot stijlen wijzigen op basis van de kenmerken van het apparaat waarop de inhoud wordt weergegeven, zoals het schermtype, de breedte, hoogte, oriëntatie en zelfs resolutie.

    @media print {
      /* print style sheets go here */
    }
    
    @import url(print.css) print;
    

U kunt bijvoorbeeld alle stijlen die nodig zijn voor afdrukken in een mediaquery voor afdrukken plaatsen:

### Een opmerking over `min-device-width`

Naast het gebruik van het kenmerk `media` in de link van het CSS-opmaakmodel, zijn er nog twee manieren om mediaquery`s toe te passen die in een CSS-bestand kunnen worden ingesloten:`@media`en`@import`. Met het oog op de prestaties, kunt u beter een van de eerste twee methoden kiezen dan de`@import`-syntax (zie [CSS-importbewerkingen vermijden](/web/fundamentals/performance/critical-rendering-path/page-speed-rules-and-recommendations)).

    @media (query) {
      /* CSS Rules used when query matches */
    }
    

De logica die van toepassing is op mediaquery`s is niet wederzijds exclusief en elk filter dat aan de criteria voldoet, zorgt ervoor dat het uiteindelijke CSS-blok wordt toegepast met behulp van de standaardregels voor bewerkingsvolgorde in CSS.

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">kenmerk</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="attribute"><code>min-width</code></td>
      <td data-th="Result">Regels die worden toegepast voor elke browserbreedte boven de in de query gedefinieerde waarde.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>max-width</code></td>
      <td data-th="Result">Regels die worden toegepast voor elke browserbreedte onder de in de query gedefinieerde waarde.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>min-height</code></td>
      <td data-th="Result">Regels die worden toegepast voor elke browserhoogte boven de in de query gedefinieerde waarde.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>max-height</code></td>
      <td data-th="Result">Regels die worden toegepast voor elke browserhoogte onder de in de query gedefinieerde waarde.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>orientation=portrait</code></td>
      <td data-th="Result">Regels die worden toegepast voor elke browser waarvan de hoogte groter is dan of gelijk is aan de breedte.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>orientation=landscape</code></td>
      <td data-th="Result">Regels die worden toegepast voor elke browser waarvan de breedte groter is dan de hoogte.</td>
    </tr>
  </tbody>
</table>

Met mediaquery`s kunt u een responsieve ervaring creëren, waarbij bepaalde stijlen worden toegepast op kleine en grote schermen en alles daartussenin. Door de syntax van de mediaquery ontstaan regels die vervolgens kunnen worden toegepast, afhankelijk van de kenmerken van het apparaat.

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

Er zijn verschillende items waarvoor een query kan worden uitgevoerd, maar de meestgebruikte in webdesign zijn `min-width`, `max-width`, `min-height` en `max-height`

- Als de browser tussen **0px** en **640px** breed is, wordt `max-640px.css` toegepast.
- Als de browser tussen **500px** en **600px** breed is, worden stijlen binnen de `@media` toegepast.
- Als de browser **640px of breder** is, wordt `min-640px.css` toegepast.
- Als de breedte van de browser **groter is dan de hoogte**, wordt `landscape.css` toegepast.
- Als de hoogte van de browser **groter is dan de breedte**, wordt `portrait.css` toegepast.

### TL;DR {: .hide-from-toc }

Een voorbeeld:

Het is ook mogelijk query`s te maken op basis van de`*-device-width`, maar deze methode wordt **sterk ontraden**.

Het verschil is subtiel, maar erg belangrijk: `min-width` is gebaseerd op de grootte van het browservenster, terwijl `min-device-width` is gebaseerd op de grootte van het scherm. De breedte van het apparaat wordt door enkele browsers, bijvoorbeeld de oude Android-browser, niet correct gerapporteerd. In plaats van dat de breedte van de viewport wordt gerapporteerd, zoals verwacht, wordt de schermgrootte in apparaatpixels weergegeven.

### Kies grote breekpunten door klein te beginnen en deze steeds groter te maken

Bovendien kunt u door het gebruik van `*-device-width` voorkomen dat inhoud wordt aangepast op desktopcomputers of andere apparaten waarop de grootte van vensters kan worden gewijzigd, omdat de query is gebaseerd op de feitelijke grootte van het apparaat en niet op de grootte van het browservenster.

### Kies zo nodig voor kleinere breekpunten

Een cruciaal concept in responsive design zijn de vloeiende en proportionele kwaliteiten ten opzichte van lay-outs met vaste breedte. Het gebruik van relatieve eenheden voor maten kan de lay-out helpen vereenvoudigen en voorkomt dat er ongewild componenten worden gemaakt die te groot zijn voor de viewport.

Door bijvoorbeeld breedte: 100% op een bovenste div in te stellen, weet u zeker dat de breedte van de viewport geheel wordt omvat en dat de waarde nooit te groot of te klein is voor de viewport. De div zal altijd passen, ongeacht of het gaat om een 320 px brede iPhone, 342 px brede Blackberry Z10 of een 360 px brede Nexus 5.

Bovendien kunnen browsers dankzij relatieve eenheden de inhoud weergeven op basis van het zoomniveau van de gebruiker zonder dat horizontale schuifbalken op de pagina nodig zijn.

<span class="compare-worse">Not recommended</span> — fixed width

    div.fullWidth {
      width: 320px;
      margin-left: auto;
      margin-right: auto;
    }
    

<span class="compare-better">Recommended</span> — responsive width

    div.fullWidth {
      width: 100%;
    }
    

## CSS-mediaquery's gebruiken voor responsiveness

Het mag dan wel handig zijn om na te denken over het bepalen van breekpunten op basis van soorten apparaten, u moet voorzichtig zijn. Het bepalen van breekpunten op basis van specifieke apparaten, producten, merknamen of besturingssystemen die tegenwoordig worden gebruikt, kan leiden tot een nachtmerrie op het gebied van onderhoud. De inhoud zelf moet bepalen hoe de lay-out zich aan de container aanpast.

### Optimaliseer tekst voor het lezen

- Maak breekpunten altijd op basis van de inhoud, en niet op basis van apparaten, producten of merken.
- Begin uw ontwerp eerst voor het kleinste mobiele apparaat en breid daarna steeds verder uit naarmate er meer schermruimte beschikbaar is.
- Beperk tekstregels tot een maximum van ongeveer 70 tot 80 tekens.

### Verberg inhoud nooit helemaal

<figure class="attempt-right">
  <img src="imgs/weather-1.png" srcset="imgs/weather-1.png 1x, imgs/weather-1-2x.png 2x" alt="">
  <figcaption>
    <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/weather-1.html">
      Preview of the weather forecast displayed on a small screen.
    </a>
  </figcaption>
</figure>

Ontwerp de inhoud zodat deze eerst op een klein scherm past, breid het scherm steeds uit tot een breekpunt nodig is. Hierdoor kunt u breekpunten optimaliseren op basis van inhoud en moet u zo weinig mogelijk breekpunten onderhouden.

Laten we eens kijken naar het voorbeeld dat we in het begin hebben gezien, de [weersvoorspelling](/web/fundamentals/design-and-ux/responsive/). In de eerste stap moeten we ervoor zorgen dat de weersvoorspelling er goed uitzien op een klein scherm.

<div style="clear:both;"></div>

<figure class="attempt-right">
  <img src="imgs/weather-2.png" class="center" srcset="imgs/weather-2.png 1x, imgs/weather-2-2x.png 2x" alt="Preview of the weather forecast as the page gets wider.">
  <figcaption>
    <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/weather-1.html">
      Preview of the weather forecast as the page gets wider.
    </a>
  </figcaption>
</figure>

Pas vervolgens de grootte van de browser aan tot er te veel witruimte is tussen de elementen en de weersvoorspelling er gewoon niet meer goed uitziet. Deze beslissing is enigszins subjectief, maar vanaf 600 pixels is echt te breed.

<div style="clear:both;"></div>

Als u op 600 pixels een breekpunt wilt invoegen, maakt u twee nieuwe stylesheets, één om te gebruiken wanneer de browser 600 pixels en kleiner is, en één wanneer de browser breder is dan 600 pixels.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-2.html" region_tag="mqweather2" adjust_indentation="auto" %}
</pre>

Ten slotte moet u de CSS herstructureren. In dit voorbeeld hebben we de meest gebruikte stijlen, zoals lettertypen, pictogrammen, basisplaatsing, kleuren in `weather.css` geplaatst. Specifieke lay-outs voor het kleine scherm worden dan in `weather-small.css` geplaatst en stijlen voor grote schermen worden in `weather-large.css` geplaatst.

<figure class="attempt-right">
  <img src="imgs/weather-3.png"  srcset="imgs/weather-3.png 1x, imgs/weather-3-2x.png 2x" alt="Preview of the weather forecast designed for a wider screen.">
  <figcaption>
    <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/weather-2.html">
      Preview of the weather forecast designed for a wider screen.
    </a>
  </figcaption>
</figure>

Wanneer de lay-out aanzienlijk verandert, kunt u voor grote breekpunten kiezen. Bovendien kan het handig zijn om de lay-out gewoon aan te passen voor kleinere wijzigingen. Tussen grote breekpunten kan het bijvoorbeeld nuttig zijn om de marges of vulling op een element aan te passen of het lettertype te vergroten zodat het natuurlijker aanvoelt in de lay-out.

<div style="clear:both"></div>

### Pick minor breakpoints when necessary

Laten we beginnen met het optimaliseren van de lay-out voor het kleine scherm. In dit geval kunnen we het lettertype versterken wanneer de viewport breder is dan 360 pixels. Wanneer er genoeg ruimte is, kunnen we daarna de hoge en lage temperatuur van elkaar scheiden zodat ze zich op dezelfde regel bevinden, in plaats van boven elkaar. Bovendien kunnen we de weerspictogrammen een beetje groter maken.

Let's start by optimizing the small screen layout. In this case, let's boost the font when the viewport width is greater than 360px. Second, when there is enough space, we can separate the high and low temperatures so that they're on the same line instead of on top of each other. And let's also make the weather icons a bit larger.

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

Voor de grote schermen beperkt u de maximale breedte van het paneel met de voorspelling zodat dit niet de hele breedte van het scherm inneemt.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-large.css" region_tag="mqsmallbplg"   adjust_indentation="auto" %}
</pre>

### Optimize text for reading

Volgens de klassieke theorie over leesbaarheid moet een ideale kolom 70 tot 80 tekens per regel bevatten (ongeveer 8 tot 10 woorden in het Engels). Telkens wanneer de breedte van een tekstvak langer wordt dan 10 woorden, is een breekpunt raadzaam.

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

Let's take a deeper look at the above blog post example. On smaller screens, the Roboto font at 1em works perfectly giving 10 words per line, but larger screens require a breakpoint. In this case, if the browser width is greater than 575px, the ideal content width is 550px.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/reading.html" region_tag="mqreading"  adjust_indentation="auto" %}
</pre>

Laten we het bovenstaande blogbericht nader bekijken. Op kleinere schermen werkt het lettertype Roboto perfect: we zien 10 woorden per regel. Op grotere schermen is er echter een breekpunt nodig. Als de browser breder is dan 575 pixels, is de ideale breedte voor de inhoud 550 pixels.

### Never completely hide content

Let goed op wanneer u kiest welke inhoud moet worden verborgen of weergegeven op basis van het schermformaat. Verberg niet gewoon inhoud omdat het niet op het scherm past. Het schermformaat is geen definitieve aanwijzing van wat een gebruiker wil. Als u bijvoorbeeld de hoeveelheid pollen uit de weersvoorspelling verwijdert, kan dit een ernstig probleem zijn voor mensen die in de lente lijden aan een allergie en de informatie nodig hebben om te bepalen of ze naar buiten kunnen of niet.

## Breekpunten kiezen

Once you've got your media query breakpoints set up, you'll want to see how your site looks with them. You *could* resize your browser window to trigger the breakpoints, but there's a better way: Chrome DevTools. The two screenshots below demonstrate using DevTools to view how a page looks under different breakpoints.

![Example of DevTools' media queries feature](imgs/devtools-media-queries-example.png)

To view your page under different breakpoints:

[Open DevTools](/web/tools/chrome-devtools/#open) and then turn on [Device Mode](/web/tools/chrome-devtools/device-mode/#toggle).

Use the [viewport controls](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#viewport-controls) to select **Responsive**, which puts DevTools into responsive mode.

Last, open the Device Mode menu and select [**Show media queries**](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#media-queries) to display your breakpoints as colored bars above your page.

Click on one of the bars to view your page while that media query is active. Right-click on a bar to jump to the media query's definition. See [Media queries](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#media-queries) for more help.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}