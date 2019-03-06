project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Lees wat de eenvoudigste manieren zijn om video toe te toevoegen aan uw website en gebruikers op alle apparaten de meest optimale ervaring te bieden.

{# wf_updated_on: 2014-04-28 #} {# wf_published_on: 2000-01-01 #}

# Video {: .page-title }

{% include "web/_shared/contributors/samdutton.html" %}

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="j5fYOYrsocs"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Gebruikers houden van video's; ze kunnen leuk en informatief zijn. Op mobiele apparaten kunt u informatie soms gemakkelijker tot u nemen door middel van een video. Video's verbruiken echter wel bandbreedte en werken niet altijd op alle platforms hetzelfde. Gebruikers haken af als ze moeten wachten tot een video is geladen of er niets gebeurt als ze op 'afspelen' drukken. Lees meer informatie over de eenvoudigste manieren om video toe te voegen aan uw website en gebruikers op alle apparaten de meest optimale ervaring te bieden.

## Een video toevoegen

### TL;DR {: .hide-from-toc }

* Gebruik het video-element om video op uw site te laden, decoderen en af te spelen.
* Maak video's in meerdere indelingen zodat ze op allerlei verschillende mobiele platforms kunnen worden afgespeeld.
* Maak video's met het juiste formaat; zorg ervoor dat ze niet overlopen tot buiten de container.
* Toegankelijkheid is belangrijk; voeg het track-element toe als onderliggend element van het video-element.

### Het video-element toevoegen

Bekijk informatie over hoe u video aan uw site toevoegt en ervoor zorgt dat gebruikers op elk apparaat een optimale ervaring hebben.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4" type="video/mp4">
  <p>Het video-element wordt niet ondersteund door deze browser.</p>
</video>

    <video src="chrome.webm" type="video/webm">
         <p>Het video-element wordt niet ondersteund door uw browser.</p>
    </video>
    

### Meerdere bestandsindelingen opgeven

Voeg het video-element toe om video op uw site te laden, decoderen en af te spelen.

Niet alle browsers ondersteunen dezelfde video-instellingen. Met het element `<source>` kunt u meerdere formaten opgeven als oplossing voor het geval de browser van de gebruiker een van de formaten niet ondersteunt. Bijvoorbeeld:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/video-main.html" region_tag="sourcetypes" adjust_indentation="auto" %}
</pre>

Als de browser de `<source>` tags parseert, wordt via het optionele kenmerk `type` bepaald welk bestand moet worden gedownload en afgespeeld. Als de browser WebM ondersteunt, wordt chrome.webm afgespeeld. In het andere geval wordt er gecontroleerd of MPEG-4-video`s kunnen worden afgespeeld. Bekijk [A Digital Media Primer for Geeks](//www.xiph.org/video/vid1.shtml "Highly entertaining and informative video guide to digital video") voor meer informatie over hoe video en audio op het web werken.

Deze benadering heeft diverse voordelen vergeleken bij het aanbieden van verschillende HTML- of serverscripts, vooral op mobiele telefoons:

Al de genoemde punten zijn met name van belang voor mobiele telefoons, waar bandbreedte en wachttijd een grote rol spelen en de gebruiker snel wil worden bediend. Het niet opgeven van een type-kenmerk kan de prestaties beïnvloeden wanneer er meerdere bronnen met niet-ondersteunde typen zijn.

Vergelijk terwijl u uw developertools voor mobiele browsers gebruikt de netwerkactiviteit [met type-kenmerken](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html) en [zonder type-kenmerken](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/notype.html). Controleer ook de reactieheaders in uw browser-developertools om u ervan te [verzekeren dat uw server het juiste MIME-type rapporteert](//developer.mozilla.org/en/docs/Properly_Configuring_Server_MIME_Types); anders zullen controles van videobrontypen niet werken.

* Ontwikkelaars kunnen formaten vermelden in volgorde van voorkeur.
* Systeemeigen switching van de client vermindert de wachttijd; er wordt slechts één verzoek gedaan om inhoud op te halen.
* De browser een formaat laten kiezen is eenvoudiger, sneller en mogelijk betrouwbaarder dan een server-ondersteuningsdatabase te gebruiken met gebruikersagentdetectie.
* Door het opgeven van het type van elke bestandsbron wordt het netwerk sneller. De browser kan dan een videobron selecteren zonder een gedeelte van de video te hoeven downloaden om het indelingstype vast te kunnen stellen.

Spaar bandbreedte en zorg ervoor dat uw site sneller reageert: gebruik de Media Fragments API om een start- en eindtijd toe te voegen aan het video-element.

Als u een mediafragment wilt toevoegen, hoeft u slechts `#t=[start_time][,end_time]` toe te voegen aan de media-URL. Als u bijvoorbeeld de video wilt afspelen tussen seconde 5 en seconde 10, geeft u het volgende op:

U kunt de Media Fragments API ook gebruiken voor het leveren van meerdere weergaven op dezelfde video &ndash; zoals cue points in een dvd &ndash; zonder meerdere bestanden te hoeven coderen en uitvoeren.

### Een start- en eindtijd opgeven

Note: - De Media Fragments API wordt door de meeste platforms ondersteund, maar niet door iOS. Controleer of bereikaanvragen door uw server worden ondersteund. Bereikaanvragen worden op de meeste servers standaard ingeschakeld, maar ze kunnen door bepaalde hostingservices worden uitgeschakeld.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm#t=5,10" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4#t=5,10" type="video/mp4">
  <p>Het video-element wordt niet ondersteund door deze browser.</p>
</video>

Controleer met uw browser-developertools `Accept-Ranges: bytes` in de reactieheaders:

    <source src="video/chrome.webm#t=5,10" type="video/webm">
    

You can also use the Media Fragments API to deliver multiple views on the same video&ndash;like cue points in a DVD&ndash;without having to encode and serve multiple files.

Voeg een posterkenmerk aan het video-element toe zodat uw gebruikers een indruk van de inhoud krijgen zodra het element wordt geladen, zonder dat de video hoeft te worden gedownload of het afspelen hoeft te worden gestart.

Een poster kan ook een noodoplossing zijn als de video `src` defect is of als geen van de geleverde video-indelingen worden ondersteund. Het enige nadeel van posterafbeeldingen is dat er een extra bestandsverzoek moet worden gedaan, wat enige bandbreedte kost en rendering vereist. Zie voor meer informatie [Afbeeldingsoptimalisatie](../../performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html#image-optimization).

<img class="center" alt="Chrome DevTools screenshot: Accept-Ranges: bytes"
src="images/Accept-Ranges-Chrome-Dev-Tools.png" />

### Een posterafbeelding toevoegen

Add a poster attribute to the `video` element so that your users have an idea of the content as soon as the element loads, without needing to download video or start playback.

    <video poster="poster.jpg" ...>
        ...
    </video>
    

Niet alle video-indelingen worden op alle platforms ondersteund. Controleer welke indelingen worden ondersteund op de belangrijkste platforms en zorg ervoor dat uw video in al deze indelingen correct kan worden afgespeeld.

Gebruik `canPlayType()` om te controleren welke video-indelingen worden ondersteund. De methode kijkt naar een tekenreeksargument die bestaat uit een `mime-type` en optionele codecs en retourneert een van de volgende waarden:

<div class="attempt-left">
  <figure>
    <img alt="Android Chrome screenshot, portrait: no poster"
    src="images/Chrome-Android-video-no-poster.png">
    <figcaption>
      Android Chrome screenshot, portrait: no poster
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Android Chrome screenshot, portrait: with poster"
    src="images/Chrome-Android-video-poster.png">
    <figcaption>
      Android Chrome screenshot, portrait: with poster
     </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

## Alternatieven voor verouderde platforms

Hier volgen enkele voorbeelden van `canPlayType()`-argumenten en retourwaarden bij uitvoering in Chrome:

### Ondersteunde indelingen controleren

Er bestaan veel hulpprogramma`s waarmee u dezelfde video in verschillende indelingen kunt opslaan:

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Retourwaarde</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Return value">(lege tekenreeks)</td>
      <td data-th="Description">De container en/of codec worden niet ondersteund.</td>
    </tr>
    <tr>
      <td data-th="Return value"><code>misschien</code></td>
      <td data-th="Description">
        De container en codec(s) worden mogelijk ondersteund, maar de browser
        moet een video downloaden om dit te controleren.
      </td>
    </tr>
    <tr>
      <td data-th="Return value"><code>misschien</code></td>
      <td data-th="Description">De indeling lijkt te worden ondersteund.
      </td>
    </tr>
  </tbody>
</table>

Wilt u weten welke video-indeling de browser heeft gebruikt?

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Type"><code>video/xyz</code></td>
      <td data-th="Response">(lege tekenreeks)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response">(lege tekenreeks)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="nonsens, ruis"</code></td>
      <td data-th="Response">(lege tekenreeks)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/mp4; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response"><code>waarschijnlijk</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm</code></td>
      <td data-th="Response"><code>misschien</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm; codecs="vp8, vorbis"</code></td>
      <td data-th="Response"><code>waarschijnlijk</code></td>
    </tr>
  </tbody>
</table>

### Video`s maken in meerdere indelingen

In JavaScript kunt u de gebruikte bron achterhalen met de eigenschap `currentSrc` van de video.

* Desktopprogramma`s: [FFmpeg](//ffmpeg.org/)
* GUI-toepassingen: [Miro](//www.mirovideoconverter.com/), [HandBrake](//handbrake.fr/), [VLC](//www.videolan.org/)
* Online coderings-/transcoderingsservices: [Zencoder](//en.wikipedia.org/wiki/Zencoder), [Amazon Elastic Encoder](//aws.amazon.com/elastictranscoder)

### De gebruikte indeling controleren

Bekijk [dit voorbeeld](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html) om te zien hoe dit in de praktijk werkt: Chrome en Firefox gebruiken `chrome.webm` (omdat deze boven aan de lijst staat van bronnen die door deze browsers worden ondersteund) terwijl Safari `chrome.mp4` gebruikt.

Als u uw gebruikers tevreden wilt houden, is het formaat zeker belangrijk.

## Juiste videoformaat

Het feitelijk gecodeerde frameformaat van de video kan afwijken van de afmetingen in het video-element (net zoals een afbeelding niet altijd wordt weergegeven op het werkelijke formaat).

### Videoformaat controleren

* Het dataverbruik kan kostbaar zijn.
* Door zonder vragen meteen te beginnen met downloaden en afspelen kan onverwacht te veel beslag worden gelegd op de bandbreedte en de processor, waardoor pagina`s minder snel worden geladen.
* Gebruikers kunnen zich in een situatie bevinden waarin het afspelen van beeld of geluid als hinderlijk wordt ervaren.

### Zorg ervoor dat videocontainers niet te vol raken

Met de eigenschappen `videoWidth` en `videoHeight` van het video-element kunt u controleren wat het gecodeerde formaat is van een video. De eigenschappen `width` en `height` retourneren de afmetingen van het video-element, waarvan het formaat kan zijn gecreëerd met CSS- of inline breedte- of hoogtekenmerken.

Als een video-element te groot is voor de viewport, kan de videocontainer te vol raken. Hierdoor kan de gebruiker de inhoud niet meer bekijken of de bedieningselementen niet meer gebruiken.

### Hoe apparaatoriëntatie werkt voor verschillende apparaten

When video elements are too big for the viewport, they may overflow their container, making it impossible for the user to see the content or use the controls.

<div class="attempt-left">
  <figure>
    <img alt="Android Chrome screenshot, portrait: unstyled video element overflows
    viewport" src="images/Chrome-Android-portrait-video-unstyled.png">
    <figcaption>
      Android Chrome screenshot, portrait: unstyled video element overflows viewport
    </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Android Chrome screenshot, landscape: unstyled video element overflows
    viewport" src="images/Chrome-Android-landscape-video-unstyled.png">
    <figcaption>
      Android Chrome screenshot, landscape: unstyled video element overflows viewport
    </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

U kunt de afmetingen van uw video regelen met JavaScript of CSS. Met behulp van JavaScript-bibliotheken en plugins zoals [FitVids](//fitvidsjs.com/) kunt u de juiste beeldverhouding behouden, zelf voor Flash-video`s van YouTube of andere bronnen.

Gebruik [CSS-mediaquery's](/web/fundamentals/design-and-ux/responsive/#use-css-media-queries-for-responsiveness) om het formaat van elementen op te geven afhankelijk van de afmetingen van de viewport; `max-width: 100%` is uw vriend.

{# include shared/related_guides.liquid inline=true list=page.related-guides.media #}

Probeer voor media-inhoud in iframes (zoals YouTube-video's) een responsieve aanpak te hanteren (zoals de aanpak [van John Surdakowski](//avexdesigns.com/responsive-youtube-embed/)).

**CSS:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="styling" adjust_indentation="auto" %}
</pre>

**CSS:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="markup" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/responsive_embed.html)

Vergelijk het [responsieve voorbeeld](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/responsive_embed.html) met de [niet-responsieve versie](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/unyt.html).

## De videospeler aanpassen

Video wordt op elk platform anders weergegeven. Bij mobiele oplossingen moet rekening worden gehouden met de oriëntatie van het apparaat. Gebruik Fullscreen API om video-inhoud in volledig scherm weer te geven.

### Inline- of fullscreenweergave

Video wordt op elk platform anders weergegeven. Bij mobiele oplossingen moet rekening worden gehouden met de oriëntatie van het apparaat. Gebruik Fullscreen API om video-inhoud in volledig scherm weer te geven.

Apparaatoriëntatie is niet van toepassing voor desktopmonitors of laptops, maar is wel heel belangrijk wanneer u webpagina`s wilt ontwerpen voor mobiele telefoons en tablets.

<div class="attempt-left">
  <figure>
    <img  alt="Screenshot of video playing in Safari on iPhone, portrait"
    src="images/iPhone-video-playing-portrait.png">
    <figcaption>Screenshot of video playing in Safari on iPhone, portrait</figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Screenshot of video playing in Safari on iPhone, landscape"
    src="images/iPhone-video-playing-landscape.png">
    <figcaption>Screenshot of video playing in Safari on iPhone, landscape</figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Safari op iPhone kan uitstekend schakelen tussen de staande en liggende oriëntatie:

<img alt="Screenshot of video playing in Safari on iPad Retina, landscape"
src="images/iPad-Retina-landscape-video-playing.png" />

De apparaatoriëntatie op een iPad en bij Chrome op Android kan lastig zijn. Zonder aanpassingen ziet bijvoorbeeld een video die op een iPad in liggende modus wordt afgespeeld er zo uit:

## Toegankelijkheid is belangrijk

<img class="attempt-right" alt="Screenshot van video die wordt afgespeeld in Safari op de iPad Retina, in liggende modus"
src="images/iPhone-video-with-poster.png" />

Door in CSS `width: 100%` of `max-width: 100%` voor een video in te stellen, kunnen veel lay-outproblemen met apparaatoriëntatie worden opgelost. U kunt ook beeldvullende alternatieven overwegen.

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Chrome on Android,
portrait" src="images/Chrome-Android-video-playing-portrait-3x5.png" />

On Android, users can request request fullscreen mode by clicking the fullscreen icon. But the default is to play video inline:

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Safari on iPad
Retina, landscape" src="images/iPad-Retina-landscape-video-playing.png" />

Safari on an iPad plays video inline:

<div style="clear:both;"></div>

### Beeldvullende weergave van inhoud instellen

Bij Safari op een iPad wordt video inline afgespeeld:

To full screen an element, like a video:

    elem.requestFullScreen();
    

Voor platforms die het beeldvullend afspelen van video niet afdwingen, wordt de Fullscreen API [breed ondersteund](//caniuse.com/fullscreen). Gebruik deze API om de beeldvullende weergave van inhoud, of de pagina, in te stellen.

    document.body.requestFullScreen();
    

Een element, zoals een video:, beeldvullend weergeven:

    video.addEventListener("fullscreenchange", handler);
    

Het hele document beeldvullend weergeven:

    console.log("In full screen mode: ", video.displayingFullscreen);
    

U kunt ook luisteren naar wijzigingen in de beeldvullende status:

<video autoplay muted loop class="attempt-right">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.mp4" type="video/mp4">
  <p>Het video-element wordt niet ondersteund door deze browser.</p>
</video>

Of controleren of het element momenteel in beeldvullende modus is:

U kunt ook de CSS-pseudoklasse `:fullscreen` gebruiken om de manier te veranderen waarop elementen in de beeldvullende modus worden weergegeven.

Op apparaten die de Fullscreen API ondersteunen, kunt u miniatuurafbeeldingen gebruiken als placeholders voor video:

<div style="clear:both;"></div>

## Snelle naslaggids

Bekijk de demo [](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/fullscreen.html) om te zien hoe dit in het echt werkt.

### Ondertiteling toevoegen voor meer toegankelijkheid

<img class="attempt-right" alt="Screenshot showing captions displayed using the
track element in Chrome on Android" src="images/Chrome-Android-track-landscape-5x3.jpg" />

Toegankelijkheid is geen functie. Gebruikers met een visuele beperking of beperking van het gehoor hebben zonder ondertitels of beschrijvingen helemaal niets aan een video. Hoewel het tijd kost om deze elementen aan uw video toe te voegen, hoeft u op deze manier niemand teleur te stellen. Zorg ervoor dat iedere gebruiker iets aan uw video heeft.

<div style="clear:both;"></div>

### Track-element toevoegen

Als u media toegankelijker wilt maken op mobiele telefoons, kunt u ondertitels of beschrijvingen toevoegen.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/track.html" region_tag="track"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/track.html)

Als u het track-element gebruikt, worden ondertitels als volgt weergegeven:

### Ondertitels in trackbestand definiëren

A track file consists of timed "cues" in WebVTT format:

    WEBVTT
    
    00:00.000 --> 00:04.000
    Man zit met een laptop op een boomtak.
    
    00:05.000 --> 00:08.000
    De tak breekt af en de man valt.
    
    ...
    

## Quick Reference

### Kenmerken van video-elementen

Het toevoegen van ondertitels aan uw video is erg gemakkelijk &ndash; voeg een track-element gewoon toe als onderliggend element van het video-element:

<table>
  <thead>
    <tr>
      <th>Attribute</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Attribute"><code>src</code></td>
      <td data-th="Description">Alle browsers</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>poster</code></td>
      <td data-th="Description">Alle browsers</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>preload</code></td>
      <td data-th="Description">In alle mobiele browsers wordt het kenmerk `preload` genegeerd. </td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>autoplay</code></td>
      <td data-th="Description">Niet ondersteund voor iPhone of Android; ondersteund voor alle desktopbrowsers, iPad, Firefox en Opera voor Android.</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>loop</code></td>
      <td data-th="Description">Alle browsers</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>controls</code></td>
      <td data-th="Description">Alle browsers</td>
    </tr>
  </tbody>
</table>

### JavaScript

Het `src`-kenmerk van het track-element geeft de locatie van het trackbestand.

Een trackbestand bestaat uit getimede `cues` in WebVTT-indeling:

* Data usage can be expensive.
* Causing media to download and playback to begin without asking first, can unexpectedly hog bandwidth and CPU, and thereby delay page rendering.
* Users may be in a context where playing video or audio is intrusive.

Een handig overzicht van eigenschappen in het video-element.

### Preload {: #preload }

Ga voor een volledig overzicht van de kenmerken van video-elementen en de beschrijving hiervan naar [specificaties video-element](//www.w3.org/TR/html5/embedded-content-0.html#the-video-element).

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Waarde</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Value"><code>none</code></td>
      <td data-th="Description">De gebruiker bekijkt de video mogelijk niet eens&ndash; er wordt niets geladen bij het openen van de pagina</td>
    </tr>
    <tr>
      <td data-th="Value"><code>metadata</code></td>
      <td data-th="Description">Metadata (duur, formaat, tekstsporen) moeten worden geladen bij het openen van de pagina, maar met zo min mogelijk beeld.</td>
    </tr>
    <tr>
      <td data-th="Value"><code>auto</code></td>
      <td data-th="Description">Het is wenselijk dat de video meteen in zijn geheel wordt gedownload.</td>
    </tr>
  </tbody>
</table>

In een desktopomgeving zorgt `autoplay` (automatisch afspelen) ervoor dat de browser onmiddellijk start met het downloaden van de video en deze afspeelt zodra dit mogelijk is. In iOS en Chrome voor Android werkt `autoplay` niet; gebruikers moeten op het scherm tikken om de video af te spelen.

### JavaScript

Ook op platforms waarop automatisch afspelen mogelijk is, kunt u zich afvragen of het een goed idee is om deze optie te activeren:

#### Autoplay

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Property &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Property"><code>currentTime</code></td>
      <td data-th="Description">Afspeelpositie in seconden ophalen of instellen.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>volume</code></td>
      <td data-th="Description">Huidige volume voor video ophalen of instellen.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>muted</code></td>
      <td data-th="Description">Geluiddemping ophalen of instellen.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>playbackRate</code></td>
      <td data-th="Description">Afspeelsnelheid ophalen of instellen; 1 is normale snelheid vooruit.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>buffered</code></td>
      <td data-th="Description">Informatie over welk gedeelte van de video in de buffer is opgeslagen en klaar is om te worden afgespeeld (zie <a href="http://people.mozilla.org/~cpearce/buffered-demo.html" title="Demo displaying amount of buffered video in a canvas element">demo</a>).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>currentSrc</code></td>
      <td data-th="Description">Het adres van de video die wordt afgespeeld.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoWidth</code></td>
      <td data-th="Description">Breedte van de video in pixels (deze kan afwijken van de breedte in het video-element).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoHeight</code></td>
      <td data-th="Description">Hoogte van de video in pixels (deze kan afwijken van de hoogte in het video-element).</td>
    </tr>
  </tbody>
</table>

#### Preload

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Method &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Method"><code>load()</code></td>
      <td data-th="Description">Een videobron laden of opnieuw laden zonder dat de video wordt gestart: bijvoorbeeld als de videobron wordt gewijzigd met JavaScript.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>play()</code></td>
      <td data-th="Description">De video afspelen vanaf diens huidige locatie.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>pause()</code></td>
      <td data-th="Description">De video pauzeren op diens huidige locatie.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>canPlayType('format')</code></td>
      <td data-th="Description">Overzicht van ondersteunde indelingen (zie Ondersteunde indelingen controleren).</td>
    </tr>
  </tbody>
</table>

Automatisch afspelen kan worden geconfigureerd in Android WebView via de [API WebSettings](//developer.android.com/reference/android/webkit/WebSettings.html#setMediaPlaybackRequiresUserGesture(boolean)). Standaard staat deze optie ingesteld op `true`, maar een WebView-app kan ervoor kiezen dit uit te schakelen.

#### Eigenschappen

Het kenmerk `preload` vertelt de browser in hoeverre informatie of inhoud moet worden geladen bij het openen van de pagina.

<table class="responsive">
  <thead>
  <tr>
    <th colspan="2">Event &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Event"><code>canplaythrough</code></td>
      <td data-th="Description">Wordt actief als de browser voldoende gegevens heeft ontvangen om de video in zijn geheel zonder onderbrekingen af te spelen.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>ended</code></td>
      <td data-th="Description">Wordt actief als de video helemaal is afgespeeld.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>error</code></td>
      <td data-th="Description">Wordt actief als er een fout optreedt.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>playing</code></td>
      <td data-th="Description">Wordt actief als de video voor de eerste keer wordt afgespeeld, opnieuw wordt gestart na te zijn gepauzeerd, of opnieuw wordt gestart.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>progress</code></td>
      <td data-th="Description">Wordt met regelmatige tussenpozen actief om de downloadvoortgang aan te geven.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>waiting</code></td>
      <td data-th="Description">Wordt actief als een actie wordt uitgesteld in afwachting van voltooiing van een andere actie.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>loadedmetadata</code></td>
      <td data-th="Description">Wordt actief als de browser klaar is met het laden van de metadata voor de video: duur, formaat en tekstsporen.</td>
    </tr>
  </tbody>
</table>

## Feedback {: #feedback }

Afhankelijk van het platform heeft het kenmerk `preload` verschillende effecten. In Chrome bijvoorbeeld wordt in een desktopomgeving 25 seconden beeld gebufferd, in iOS of Android niets. Dit betekent dat op mobiele platforms soms sprake is van een opstartvertraging bij het afspelen die niet voorkomt in een desktopomgeving. Zie [Steve Souders` testpagina](//stevesouders.com/tests/mediaevents.php) voor volledige informatie.