project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Hier erfahren Sie, wie Sie Videoinhalte ganz einfach zu Ihrer Website hinzufügen und den Nutzern die bestmögliche Nutzererfahrung bieten können - egal auf welchem Gerät.

{# wf_updated_on: 2014-04-28 #} {# wf_published_on: 2014-04-28 #}

# Video {: .page-title }

{% include "web/_shared/contributors/samdutton.html" %}

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="j5fYOYrsocs"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Nutzer mögen Videos. Sie können lustig und gleichzeitig informativ sein. Auf Mobilgeräten lassen sich Informationen in Form von Videos meist einfacher konsumieren. Aber Videos verbrauchen Bandbreite und funktionieren je nach Plattform unterschiedlich gut. Nutzer mögen es nicht, wenn ein Video ewig lädt oder beim Drücken der Wiedergabetaste nichts passiert. Hier erfahren Sie, wie Sie Ihrer Website Videoinhalte ganz einfach hinzufügen und den Nutzern die bestmögliche Nutzererfahrung bieten können - egal auf welchem Gerät.

## Video hinzufügen

### TL;DR {: .hide-from-toc }

* Verwenden Sie das Videoelement zum Laden, Decodieren und Abspielen von Videos auf Ihrer Website.
* Erstellen Sie die Videoinhalte in verschiedenen Formaten, um eine Reihe mobiler Plattformen abzudecken.
* Achten Sie auf die richtige Größe der Videos, damit diese nicht ihre Container sprengen.
* Achten Sie auf Zugänglichkeit. Fügen Sie das Track-Element hinzu und ordnen Sie es dem Videoelement unter.

### Videoelement hinzufügen

Hier erfahren Sie, wie Sie Videoinhalte ganz einfach zu Ihrer Website hinzufügen und den Nutzern die bestmögliche Nutzererfahrung bieten können - egal auf welchem Gerät.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4" type="video/mp4">
  <p>Dieser Browser unterstützt das Videoelement nicht.</p>
</video>

    <video src="chrome.webm" type="video/webm">
        <p>Ihr Browser unterstützt das Videoelement nicht.</p>
    </video>
    

### Mehrere Dateiformate angeben

Nicht alle Browser unterstützen dieselben Videoformate. Mithilfe des `<source>`-Elements können Sie mehrere Formate als Ausweichmöglichkeit angeben, falls der Browser des Nutzers eines der Formate nicht unterstützt. Beispiel:

Beim Parsen der `<source>`-Tags durch den Browser wird anhand des optionalen Attributs `type` ermittelt, welche Datei heruntergeladen und wiedergegeben werden soll. Wenn der Browser WebM unterstützt, spielt er die Datei `chrome.webm` ab, andernfalls wird überprüft, ob MPEG-4-Videos abgespielt werden können. Weitere Informationen dazu, wie Video- und Audioinhalte im Web funktionieren, finden Sie unter [A Digital Media Primer for Geeks](//www.xiph.org/video/vid1.shtml "Highly entertaining and informative video guide to digital video") (Leitfaden zu digitalen Medien für Computerfreaks).

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/video-main.html" region_tag="sourcetypes" adjust_indentation="auto" %}
</pre>

Dieser Ansatz bietet mehrere Vorteile im Vergleich zu verschiedenen HTML-Standards oder serverseitigen Skripts, besonders im Hinblick auf Mobilgeräte:

Diese Punkte spielen vor allem in mobilen Kontexten eine wichtige Rolle, wo Bandbreite und Latenz höchste Priorität haben und die Geduld der Nutzer meist schnell am Ende ist. Der Verzicht auf ein Type-Attribut kann sich im Falle mehrerer Quellen mit nicht unterstützten Typen auf die Leistung auswirken.

Vergleichen Sie anhand der Entwicklertools für Ihren mobilen Browser die Netzwerkaktivität [mit](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html) und [ohne Type-Attribute](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/notype.html). Darüber hinaus sollten Sie die Antwortheader in den Entwicklertools für Ihren Browser überprüfen, um [sicherzustellen, dass Ihr Server den richtigen MIME-Typ zurückgibt](//developer.mozilla.org/en/docs/Properly_Configuring_Server_MIME_Types). Andernfalls lässt sich nicht überprüfen, um welche Art von Videoquelle es sich handelt.

Sparen Sie Bandbreite und machen Sie Ihre Website reaktionsschneller, indem Sie dem Videoelement mithilfe der Media Fragments-API eine Start- und Endzeit hinzufügen.

* Verwenden Sie das Videoelement zum Laden, Decodieren und Abspielen von Videos auf Ihrer Website:
* Native client-side switching reduces latency; only one request is made to get content.
* Letting the browser choose a format is simpler, quicker, and potentially more reliable than using a server-side support database with user-agent detection.
* Specifying each file source's type improves network performance; the browser can select a video source without having to download part of the video to "sniff" the format.

Zum Hinzufügen eines Medienfragments fügen Sie der Medien-URL einfach `#t=[start_time][,end_time]` hinzu. Wenn das Video zum Beispiel von Sekunde 5 - 10 abgespielt werden soll, geben Sie Folgendes an:

Mithilfe der Media Fragments-API können Sie auch mehrere Ansichten desselben Videos bereitstellen - ähnlich wie Cue-Punkte bei einer DVD - ohne mehrere Dateien codieren und bereitstellen zu müssen.

Note: - Die Media Fragments-API wird auf den meisten Plattformen mit Ausnahme von iOS unterstützt.

### Posterbild hinzufügen

Suchen Sie mithilfe der Entwicklertools für Ihren Browser in den Antwortheadern nach `Accept-Ranges: bytes`:

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm#t=5,10" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4#t=5,10" type="video/mp4">
  <p>Dieser Browser unterstützt das Videoelement nicht.</p>
</video>

To add a media fragment, you simply add `#t=[start_time][,end_time]` to the media URL. For example, to play the video between seconds 5 through 10, specify:

    <source src="video/chrome.webm#t=5,10" type="video/webm">
    

Fügen Sie dem Videoelement ein Poster-Attribut hinzu, damit Ihre Nutzer bereits beim Laden des Elements wissen, worum es in dem Video geht, ohne es herunterladen oder die Wiedergabe starten zu müssen.

Ein Poster kann auch eine Ausweichmöglichkeit sein, falls das Attribut `src` des Videos beschädigt ist oder keines der bereitgestellten Videoformate unterstützt wird. Der einzige Nachteil bei Posterbildern ist die zusätzliche Dateianforderung, die etwas Bandbreite verbraucht und Rendering erfordert. Weitere Informationen finden Sie unter [Bildoptimierung](../../performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html#image-optimization).

Hier sehen Sie eine Gegenüberstellung von Videos mit und ohne Posterbild. Wir haben das Posterbild grau dargestellt, um deutlich zu machen, dass es sich nicht um das Video handelt:

<img class="center" alt="Android Chrome-Screenshot, Hochformat: ohne Poster"
src="images/Accept-Ranges-Chrome-Dev-Tools.png" />

### Unterstützte Formate ermitteln

Nicht alle Videoformate werden auf allen Plattformen unterstützt. Informieren Sie sich, welche Formate auf den gängigen Plattformen unterstützt werden, und stellen Sie sicher, dass sich Ihr Video dort abspielen lässt.

    <video poster="poster.jpg" ...>
      ...
    </video>
    

Mit `canPlayType()` können Sie herausfinden, welche Videoformate unterstützt werden. Bei dieser Methode wird auf der Grundlage eines Zeichenfolgenarguments bestehend aus `mime-type` und optionalen Codecs einer der folgenden Werte zurückgegeben:

Im Folgenden finden Sie einige Beispiele für `canPlayType()`-Argumente und zurückgegebene Werte in Chrome:

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

## Start- und Endzeit festlegen

Es gibt viele Tools, mit denen Sie dasselbe Video in unterschiedlichen Formaten speichern können:

### Videos in mehreren Formaten erstellen

Sie möchten wissen, für welches Videoformat sich der Browser letztlich entschieden hat?

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Zurückgegebener Wert</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Return value">(leere Zeichenfolge)</td>
      <td data-th="Description">Der Container bzw. Codec wird nicht unterstützt.</td>
    </tr>
    <tr>
      <td data-th="Return value"><code>maybe</code></td>
      <td data-th="Description">
        Container und Codec(s) werden möglicherweise unterstützt, der Browser
        muss jedoch einen Teil des Videos herunterladen, um dies zu überprüfen.
      </td>
    </tr>
    <tr>
      <td data-th="Return value"><code>probably</code></td>
      <td data-th="Description">Das Format wird anscheinend unterstützt.
      </td>
    </tr>
  </tbody>
</table>

In JavaScript können Sie anhand der Eigenschaft `currentSrc` des Videos feststellen, welche Quelle verwendet wurde.

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Typ</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Type"><code>video/xyz</code></td>
      <td data-th="Response">(leere Zeichenfolge)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response">(leere Zeichenfolge)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="nonsense, noise"</code></td>
      <td data-th="Response">(leere Zeichenfolge)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/mp4; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response"><code>probably</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm</code></td>
      <td data-th="Response"><code>maybe</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm; codecs="vp8, vorbis"</code></td>
      <td data-th="Response"><code>probably</code></td>
    </tr>
  </tbody>
</table>

### Verwendetes Format ermitteln

Wie das in der Praxis aussieht, erfahren Sie in [dieser Demo](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html): Chrome und Firefox haben sich für `chrome.webm` entschieden, weil diese Datei in der Liste der potenziell von diesen Browsern unterstützten Quellen ganz oben steht, während Safari `chrome.mp4` ausgewählt hat.

* Entwickler können Formate in der bevorzugten Reihenfolge anordnen.
* Durch die native clientseitige Umschaltung wird die Latenz verringert. Es wird nur eine Anforderung zum Abrufen des Inhalts gesendet.
* Die Wahl eines Formats durch den Browser ist einfacher, schneller und unter Umständen zuverlässiger als die Verwendung einer serverseitigen Supportdatenbank mit User-Agent-Erkennung.

### TL;DR {: .hide-from-toc }

Für die Zufriedenheit der Nutzer spielt die Größe eine wichtige Rolle.

Die tatsächlich codierte Framegröße des Videos kann von den Abmessungen des Videoelements abweichen, ebenso wie ein Bild möglicherweise nicht in seiner tatsächlichen Größe angezeigt wird.

## Alternativen für veraltete Plattformen anbieten

Die codierte Größe eines Videos lässt sich anhand der Eigenschaften `videoWidth` und `videoHeight` des Videoelements ermitteln. `width` und `height` geben die Größe des Videoelements zurück, die möglicherweise anhand von CSS oder Inline-Breiten- und -Höhenattributen festgelegt wurde.

### Videogröße ermitteln

* Vergewissern Sie sich, dass Ihr Server Bereichsanforderungen unterstützt. Bereichsanforderungen sind auf den meisten Servern standardmäßig aktiviert, einige Hostingdienste können diese jedoch deaktivieren.
* Don't make your videos any longer than they need be.
* Long videos can cause hiccups with download and seeking; some browsers may have to wait until the video downloads before beginning playback.

### Sicherstellen, dass Videos nicht die Größe der Container sprengen

Wenn Videoelemente für den Darstellungsbereich zu groß sind, sprengen sie möglicherweise die Größe der Container. Die Folge: Nutzer können sich weder den Inhalt ansehen noch die Steuerelemente nutzen.

To check the encoded size of a video, use the video element `videoWidth` and `videoHeight` properties. `width` and `height` return the dimensions of the video element, which may have been sized using CSS or inline width and height attributes.

### Wie die Geräteausrichtung geräteübergreifend funktioniert

Die Videogröße lässt sich mit JavaScript oder CSS steuern. JavaScript-Bibliotheken und Plug-ins wie [FitVids](//fitvidsjs.com/) ermöglichen die Beibehaltung der richtigen Größe und des richtigen Formats, selbst für Flash-Videos von YouTube und anderen Quellen.

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

Mithilfe von [CSS-Medienabfragen](../../layouts/rwd-fundamentals/#use-css-media-queries-for-responsiveness) lässt sich die Größe von Elementen je nach Größe des Darstellungsbereichs angeben. Mit `max-width: 100%` liegen Sie nie falsch.

Für Medieninhalte in iframes, zum Beispiel YouTube-Videos, sollten Sie einen responsiven Ansatz wie den von [John Surdakowski](//avexdesigns.com/responsive-youtube-embed/)) versuchen.

Note: Erzwingen Sie keine Größenanpassung von Elementen, wenn das daraus resultierende Seitenverhältnis vom Originalvideo abweicht. Ein gestauchtes oder gestrecktes Bild sieht nicht schön aus.

Caution: Don't force element sizing that results in an aspect ratio different from the original video. Squashed or stretched looks bad.

**HTML:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="styling" adjust_indentation="auto" %}
</pre>

**HTML:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="markup" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/responsive_embed.html)

Videos werden je nach Plattform unterschiedlich dargestellt. Bei Lösungen für Mobilgeräte muss die Geräteausrichtung berücksichtigt werden. Verwenden Sie die Fullscreen-API, um die Vollbildansicht von Videoinhalten zu steuern.

## Videogröße richtig wählen

Bei Desktopmonitoren oder Laptops ist die Geräteausrichtung kein Thema. Anders sieht das jedoch bei Webseiten für Mobilgeräte und Tablets aus.

### Vollbildansicht von Inhalten steuern

Safari auf dem iPhone schaltet gut zwischen Hoch- und Querformat um:

Safari on iPhone does a good job of switching between portrait and landscape orientation:

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

Auf einem iPad und in Chrome unter Android kann die Geräteausrichtung hingegen problematisch sein. Zum Beispiel sieht eine Videowiedergabe ohne jegliche Anpassung auf einem iPad im Querformat so aus:

<img alt="Screenshot einer Videowiedergabe in Safari auf einem iPad mit Retina-Display, Querformat"
src="images/iPad-Retina-landscape-video-playing.png" />

Durch Verwendung von `width: 100%` oder `max-width: 100%` in CSS lassen sich viele Layoutprobleme in Verbindung mit der Geräteausrichtung lösen. Darüber hinaus sollten Sie eventuell auch Vollbildalternativen in Betracht ziehen.

## Videoplayer anpassen

<img class="attempt-right" alt="Screenshot of video element on iPhone, portrait"
src="images/iPhone-video-with-poster.png" />

Different platforms display video differently. Safari on an iPhone displays a video element inline on a web page, but plays video back in fullscreen mode:

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Chrome on Android,
portrait" src="images/Chrome-Android-video-playing-portrait-3x5.png" />

On Android, users can request request fullscreen mode by clicking the fullscreen icon. But the default is to play video inline:

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Safari on iPad
Retina, landscape" src="images/iPad-Retina-landscape-video-playing.png" />

Safari on an iPad plays video inline:

<div style="clear:both;"></div>

### Untertitel für eine bessere Zugänglichkeit hinzufügen

Bei Plattformen, die die Videowiedergabe im Vollbildmodus nicht erzwingen, wird die Fullscreen-API [weitestgehend unterstützt](//caniuse.com/fullscreen). Verwenden Sie diese API, um die Vollbildansicht von Inhalten oder der Seite zu steuern.

Zur Vollbildansicht eines Elements, z. B. video:

    elem.requestFullScreen();
    

Zur Vollbildansicht des gesamten Dokuments:

    document.body.requestFullScreen();
    

Änderungen am Vollbildmodus können Sie auch hören:

    video.addEventListener("fullscreenchange", handler);
    

Oder überprüfen Sie, ob sich das Element derzeit im Vollbildmodus befindet:

    console.log("In full screen mode: ", video.displayingFullscreen);
    

Die Art und Weise, wie Elemente im Vollbildmodus angezeigt werden, können Sie auch mithilfe der CSS-Pseudoklasse `:fullscreen` ändern.

<video autoplay muted loop class="attempt-right">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.mp4" type="video/mp4">
  <p>Dieser Browser unterstützt das Videoelement nicht.</p>
</video>

Auf Geräten, die die Fullscreen-API unterstützen, sollten Sie die Verwendung von Miniaturansicht-Bildern als Platzhalter für Videos in Betracht ziehen:

In dieser [Demo](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/fullscreen.html) erfahren Sie, wie das Ganze in der Praxis aussieht.

Note: `requestFullScreen()` is currently vendor prefixed and may require extra code for full cross browser compatibility.

<div style="clear:both;"></div>

## Inline- oder Vollbildanzeige

Zugänglichkeit ist keine Funktion. Nutzer, die nicht hören oder sehen können, sind nicht in der Lage, sich ein Video ohne Untertitel oder Beschreibungen anzusehen. Der Zeitaufwand für das Hinzufügen solcher Untertitel oder Beschreibungen steht in keinem Verhältnis zu der schlechten Erfahrung, die Sie Ihren Nutzern bieten, wenn Sie darauf verzichten. Für alle Nutzer sollte zumindest ein Grundmaß an Nutzerfreundlichkeit gegeben sein.

### Track-Element hinzufügen

<img class="attempt-right" alt="Screenshot showing captions displayed using the
track element in Chrome on Android" src="images/Chrome-Android-track-landscape-5x3.jpg" />

Note: Das Track-Element wird in Chrome für Android, iOS Safari sowie allen aktuellen Desktop-Browsern mit Ausnahme von Firefox (siehe [caniuse.com/track](http://caniuse.com/track "Track element support status")) unterstützt. Darüber hinaus sind auch mehrere Polyfiller verfügbar. Wir empfehlen [Playr](//www.delphiki.com/html5/playr/ "Playr track element polyfill") oder [Captionator](//captionatorjs.com/ "Captionator track").

<div style="clear:both;"></div>

### Untertitel in Track-Datei definieren

Bei Verwendung des Track-Elements sehen die Untertitel wie folgt aus:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/track.html" region_tag="track" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/track.html)

Es ist ganz einfach, Ihr Video mit Untertiteln zu versehen - Sie müssen lediglich ein Track-Element hinzufügen, das dem Videoelement untergeordnet ist:

### Attribute des Videoelements

Das Attribut `src` des Track-Elements enthält den Speicherort der Track-Datei.

    WEBVTT
    
    00:00.000 --> 00:04.000
    Mann sitzt mit seinem Laptop auf einem Ast.
    
    00:05.000 --> 00:08.000
    Der Ast bricht und der Mann fällt herunter.
    
    ...
    

## Warum Zugänglichkeit wichtig ist

### JavaScript

Eine Track-Datei besteht aus zeitlich festgelegten Cues im WebVTT-Format:

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
      <td data-th="Description">Alle Browser</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>poster</code></td>
      <td data-th="Description">Alle Browser</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>preload</code></td>
      <td data-th="Description">Das Preload-Attribut wird von allen mobilen Browsern ignoriert. </td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>autoplay</code></td>
      <td data-th="Description">Wird auf dem iPhone und in Chrome für Android nicht unterstützt, jedoch in sämtlichen Desktop-Browsern, auf dem iPad sowie in Firefox und Opera für Android.</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>loop</code></td>
      <td data-th="Description">Alle Browser</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>controls</code></td>
      <td data-th="Description">Alle Browser</td>
    </tr>
  </tbody>
</table>

### Autoplay {: #autoplay }

Hier finden Sie eine kurze Übersicht über die Eigenschaften des Videoelements.

Eine vollständige Liste der Attribute des Videoelements samt Definitionen finden Sie in den [Spezifikationen zum Videoelement](//www.w3.org/TR/html5/embedded-content-0.html#the-video-element).

* Desktop-Tools: [FFmpeg](//ffmpeg.org/)
* GUI-Anwendungen: [Miro](//www.mirovideoconverter.com/), [HandBrake](//handbrake.fr/), [VLC](//www.videolan.org/)
* Onlinedienste zur Codierung/Transcodierung: [Zencoder](//en.wikipedia.org/wiki/Zencoder), [Amazon Elastic Encoder](//aws.amazon.com/elastictranscoder)

Bei Verwendung von `autoplay` wird das Video in Desktop-Browsern schnellstmöglich heruntergeladen und abgespielt. Unter iOS und in Chrome für Android funktioniert `autoplay` nicht. Die Nutzer müssen zum Abspielen des Videos auf den Bildschirm tippen.

### Preload {: #preload }

Auch auf Plattformen, auf denen das Autoplay-Attribut verwendet werden kann, sollten Sie überlegen, ob eine Aktivierung sinnvoll ist:

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Wert</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Value"><code>none</code></td>
      <td data-th="Description">Der Nutzer sieht sich das Video möglicherweise überhaupt nicht an, sodass nichts vorab geladen werden sollte.</td>
    </tr>
    <tr>
      <td data-th="Value"><code>metadata</code></td>
      <td data-th="Description">Metadaten wie Dauer, Größe und Texttracks sollten vorab geladen werden, wobei das Video selbst möglichst außen vor gelassen werden sollte.</td>
    </tr>
    <tr>
      <td data-th="Value"><code>auto</code></td>
      <td data-th="Description">Der sofortige Download des gesamten Videos ist wünschenswert.</td>
    </tr>
  </tbody>
</table>

Die automatische Wiedergabe kann in der Android WebView über die [WebSettings-API](//developer.android.com/reference/android/webkit/WebSettings.html#setMediaPlaybackRequiresUserGesture(boolean)) konfiguriert werden. Standardmäßig ist `true` festgelegt, die Option kann jedoch von einer WebView-App deaktiviert werden.

### JavaScript

Das Attribut `preload` gibt dem Browser Auskunft darüber, wie viele Informationen oder Inhalte vorab geladen werden sollten.

#### Automatische Wiedergabe

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Property &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Property"><code>currentTime</code></td>
      <td data-th="Description">Wiedergabeposition in Sekunden abrufen oder festlegen</td>
    </tr>
    <tr>
      <td data-th="Property"><code>volume</code></td>
      <td data-th="Description">Aktuelle Lautstärke für das Video abrufen oder festlegen</td>
    </tr>
    <tr>
      <td data-th="Property"><code>muted</code></td>
      <td data-th="Description">Stummschaltung abrufen oder festlegen</td>
    </tr>
    <tr>
      <td data-th="Property"><code>playbackRate</code></td>
      <td data-th="Description">Wiedergaberate abrufen oder festlegen, wobei 1 die Standardgeschwindigkeit ist, mit der das Video vorwärts abgespielt wird</td>
    </tr>
    <tr>
      <td data-th="Property"><code>buffered</code></td>
      <td data-th="Description">Informationen dazu, inwieweit das Video gepuffert wurde und abgespielt werden kann (siehe <a href="http://people.mozilla.org/~cpearce/buffered-demo.html" title="Demo displaying amount of buffered video in a canvas element">Demo</a>)</td>
    </tr>
    <tr>
      <td data-th="Property"><code>currentSrc</code></td>
      <td data-th="Description">Die Adresse des Videos, das zurzeit abgespielt wird</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoWidth</code></td>
      <td data-th="Description">Breite des Videos in Pixel, die von der Breite des Videoelements abweichen kann</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoHeight</code></td>
      <td data-th="Description">Höhe des Videos in Pixel, die von der Höhe des Videoelements abweichen kann</td>
    </tr>
  </tbody>
</table>

#### Vorab laden

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Method &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Method"><code>load()</code></td>
      <td data-th="Description">Laden oder erneutes Laden einer Videoquelle, ohne dass die Wiedergabe gestartet wird, zum Beispiel, wenn das Videoattribut "src" mit JavaScript geändert wird</td>
    </tr>
    <tr>
      <td data-th="Method"><code>play()</code></td>
      <td data-th="Description">Videowiedergabe an der aktuellen Position starten</td>
    </tr>
    <tr>
      <td data-th="Method"><code>pause()</code></td>
      <td data-th="Description">Videowiedergabe an der aktuellen Position pausieren</td>
    </tr>
    <tr>
      <td data-th="Method"><code>canPlayType('format')</code></td>
      <td data-th="Description">Informationen zu den unterstützten Formaten (siehe `Unterstützte Formate ermitteln`)</td>
    </tr>
  </tbody>
</table>

Das Attribut `preload` wirkt sich je nach Plattform unterschiedlich aus. Auf Desktopgeräten puffert Chrome zum Beispiel 25 Sekunden des Videos, unter iOS oder Android erfolgt hingegen kein Puffern. Das bedeutet, dass die Wiedergabe auf Mobilgeräten im Vergleich zu Desktopgeräten möglicherweise verzögert beginnt. Alle Details hierzu finden Sie auf der [Testseite von Steve Souders](//stevesouders.com/tests/mediaevents.php).

#### Eigenschaften

[Der HTML5 Rocks-Videoartikel](//www.html5rocks.com/en/tutorials/video/basics/#toc-javascript) fasst die JavaScript-Eigenschaften, -Methoden und -Ereignisse, die für die Steuerung der Videowiedergabe verwendet werden können, auf übersichtliche und verständliche Weise zusammen. Diese Informationen, die wir, sofern relevant, durch für Mobilgeräte spezifische Details ergänzt haben, finden Sie im Folgenden.

<table class="responsive">
  <thead>
  <tr>
    <th colspan="2">Event &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Event"><code>canplaythrough</code></td>
      <td data-th="Description">Wird ausgelöst, wenn der Browser aufgrund der verfügbaren Datenmenge der Ansicht ist, dass er das Video vollständig ohne Unterbrechung abspielen kann</td>
    </tr>
    <tr>
      <td data-th="Event"><code>ended</code></td>
      <td data-th="Description">Wird ausgelöst, wenn die Wiedergabe des Videos beendet ist</td>
    </tr>
    <tr>
      <td data-th="Event"><code>error</code></td>
      <td data-th="Description">Wird im Falle eines Fehlers ausgelöst</td>
    </tr>
    <tr>
      <td data-th="Event"><code>playing</code></td>
      <td data-th="Description">Wird ausgelöst, wenn das Video zum ersten Mal abgespielt oder erneut gestartet wird bzw. nachdem es pausiert wurde</td>
    </tr>
    <tr>
      <td data-th="Event"><code>progress</code></td>
      <td data-th="Description">Wird in regelmäßigen Abständen ausgelöst, um den Download-Fortschritt anzugeben</td>
    </tr>
    <tr>
      <td data-th="Event"><code>waiting</code></td>
      <td data-th="Description">Wird ausgelöst, wenn eine Aktion verzögert ist, weil zunächst eine andere Aktion abgeschlossen werden muss</td>
    </tr>
    <tr>
      <td data-th="Event"><code>loadedmetadata</code></td>
      <td data-th="Description">Wird ausgelöst, wenn der Browser die Metadaten des Videos geladen hat: Dauer, Größe und Texttracks</td>
    </tr>
  </tbody>
</table>

## Kurzübersicht

Auf Mobilgeräten werden `playbackRate` ([siehe Demo](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/scripted.html)) und `volume` nicht unterstützt.