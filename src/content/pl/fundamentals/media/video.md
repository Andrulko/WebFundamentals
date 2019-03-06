project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Przeczytaj, jak w prosty sposób umieścić film na stronie i upewnić się, że użytkownicy będą mogli wygodnie go oglądać na dowolnym urządzeniu.

{# wf_updated_on: 2014-04-28 #} {# wf_published_on: 2000-01-01 #}

# Wideo {: .page-title }

{% include "web/_shared/contributors/samdutton.html" %}

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="j5fYOYrsocs"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Użytkownicy lubią filmy, bo zwykle są one ciekawe i treściwe. Na urządzeniach mobilnych filmy pozwalają przystępnie prezentować wiele informacji. Obciążają jednak łącze i nie zawsze działają tak samo na każdej platformie. Oczekiwanie na załadowanie filmu irytuje użytkowników, podobnie jak brak reakcji na kliknięcie przycisku odtwarzania. Przeczytaj, jak w prosty sposób umieścić film na stronie i upewnić się, że użytkownicy będą mogli wygodnie go oglądać na dowolnym urządzeniu.

## Dodawanie filmu

### TL;DR {: .hide-from-toc }

* Użyj elementu video, by wczytywać, dekodować i odtwarzać filmy w swojej witrynie.
* Przygotuj film w wielu formatach, by można było oglądać go na wielu platformach mobilnych.
* Ustaw prawidłowy rozmiar filmu. Upewnij się, że nie będzie on wystawać poza swój kontener.
* Ułatwienia dostępu są ważne. Dodaj element track jako podrzędny elementu video.

### Dodawanie elementu video

Przeczytaj, jak w prosty sposób umieścić film na stronie i upewnić się, że użytkownicy będą mogli wygodnie go oglądać na dowolnym urządzeniu.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4" type="video/mp4">
  <p>Ta przeglądarka nie obsługuje elementu video.</p>
</video>

    <video src="chrome.webm" type="video/webm">
        <p>Twoja przeglądarka nie obsługuje elementu video.</p>
    </video>
    

### Określanie wielu formatów plików

Dodaj element video, by wczytywać, dekodować i odtwarzać filmy w swojej witrynie:

Nie wszystkie przeglądarki obsługują te same formaty wideo. Element `<source>` pozwala określić wiele formatów zastępczych, jeśli przeglądarka nie obsługuje tego zamierzonego. Na przykład:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/video-main.html" region_tag="sourcetypes" adjust_indentation="auto" %}
</pre>

Podczas analizowania tagów `<source>` przeglądarka korzysta z opcjonalnego atrybutu `type`, by ustalić, który plik ma pobrać i odtworzyć. Jeśli przeglądarka obsługuje WebM, odtworzy plik chrome.webm. W przeciwnym razie sprawdzi, czy może odtworzyć film w formacie MPEG-4. Więcej o działaniu plików wideo i dźwiękowych w internecie dowiesz się z filmu [A Digital Media Primer for Geeks](//www.xiph.org/video/vid1.shtml "Ciekawy i pouczający przewodnik wideo po cyfrowych filmach").

To rozwiązanie ma kilka zalet w porównaniu do wykonywania różnego kodu HTML lub używania skryptów po stronie serwera, zwłaszcza na urządzeniach mobilnych:

Wszystkie te punkty są szczególnie ważne w kontekście urządzeń mobilnych, na których przepustowość sieci i czas oczekiwania mają duże znaczenie, a cierpliwość użytkownika jest zwykle ograniczona. Brak atrybutu type może wpłynąć na wydajność, gdy wiele typów plików źródłowych nie jest obsługiwanych.

Użyj narzędzi dla programistów w przeglądarce mobilnej, by porównać aktywność sieci, gdy w kodzie [są atrybuty type](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html) i gdy [ich nie ma](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/notype.html). Przejrzyj w tych narzędziach także nagłówki odpowiedzi, by [upewnić się, że serwer zgłasza właściwy typ MIME](//developer.mozilla.org/en/docs/Properly_Configuring_Server_MIME_Types). W przeciwnym razie sprawdzanie typu pliku źródłowego filmu nie będzie działać.

* Programista może wymienić formaty w preferowanej kolejności.
* Natywne przełączanie się po stronie klienta zmniejsza czas oczekiwania. Przeglądarka wysyła tylko jedno żądanie, by pobrać treści.
* Umożliwienie przeglądarce wyboru formatu jest prostsze, szybsze i prawdopodobnie bardziej niezawodne niż stosowanie bazy danych obsługi po stronie serwera i wykrywanie klienta użytkownika.
* Określenie typu każdego pliku źródłowego zwiększa wydajność sieci. Przeglądarka może wybrać plik wideo bez pobierania fragmentu filmu i rozpoznawania formatu.

Możesz zmniejszyć obciążenie łącza i poprawić elastyczność strony &ndash; użyj interfejsu API Media Fragments, by dodać czas rozpoczęcia i zakończenia do elementu video.

Aby umieścić na stronie fragment multimediów, wystarczy dodać `#t=[start_time][,end_time]` do ich adresu URL. Jeśli np. użytkownik ma zobaczyć fragment filmu od 5 do 10&nbsp;sekundy, użyj takiego elementu:

Interfejs API Media Fragments pozwala udostępniać wiele widoków tego samego filmu (podobnie do wyboru scen na płycie DVD) bez konieczności kodowania i przesyłania wielu plików.

### Określanie czasu rozpoczęcia i zakończenia

Note: - Interfejs API Media Fragments działa na większości platform z wyjątkiem iOS.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm#t=5,10" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4#t=5,10" type="video/mp4">
  <p>Ta przeglądarka nie obsługuje elementu video.</p>
</video>

Użyj narzędzi dla programistów w przeglądarce, by znaleźć ciąg `Accept-Ranges: bytes` w nagłówkach odpowiedzi:

    <source src="video/chrome.webm#t=5,10" type="video/webm">
    

You can also use the Media Fragments API to deliver multiple views on the same video&ndash;like cue points in a DVD&ndash;without having to encode and serve multiple files.

Dodaj atrybut poster do elementu video, by od razu po wczytaniu strony użytkownicy mogli zorientować się w treści filmu, bez potrzeby jego pobierania czy uruchamiania odtwarzania.

Plakat może też być obrazem zastępczym, gdy atrybut `src` elementu video jest nieprawidłowy lub żaden z dostępnych formatów wideo nie jest obsługiwany. Jedyna wada obrazów plakatu to dodatkowe żądanie wyświetlenia pliku, które zajmuje łącze i wymaga renderowania. Więcej informacji znajdziesz w artykule [Optymalizacja obrazów](../../performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html#image-optimization).

<img class="center" alt="Chrome DevTools screenshot: Accept-Ranges: bytes"
src="images/Accept-Ranges-Chrome-Dev-Tools.png" />

### Dołączanie obrazu plakatu

Add a poster attribute to the `video` element so that your users have an idea of the content as soon as the element loads, without needing to download video or start playback.

    <video poster="poster.jpg" ...>
      ...
    </video>
    

Nie wszystkie formaty wideo działają na każdej platformie. Sprawdź, które formaty są obsługiwane na głównych platformach, i upewnij się, że Twój film będzie można odtwarzać na każdej z nich.

Aby dowiedzieć się, które formaty wideo działają, użyj metody `canPlayType()`. Przyjmuje ona argument w postaci ciągu znaków, który zawiera `mime-type` i opcjonalne kodeki, po czym zwraca jedną z tych wartości:

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

## Alternatywne rozwiązania na starsze platformy

Poniżej znajdziesz kilka przykładowych argumentów metody `canPlayType()` i wartości zwracane po uruchomieniu jej w Chrome:

### Sprawdzanie obsługiwanych formatów

Jest wiele narzędzi, które pozwalają zapisać dany film w różnych formatach:

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Zwracana wartość</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Return value">(pusty ciąg znaków)</td>
      <td data-th="Description">Kontener i/lub kodek nie jest obsługiwany.</td>
    </tr>
    <tr>
      <td data-th="Return value"><code>maybe</code></td>
      <td data-th="Description">
        Kontener i kodeki mogą być obsługiwane, ale przeglądarka
        musi pobrać fragment filmu, by to sprawdzić.
      </td>
    </tr>
    <tr>
      <td data-th="Return value"><code>probably</code></td>
      <td data-th="Description">Format prawdopodobnie jest obsługiwany.
      </td>
    </tr>
  </tbody>
</table>

Chcesz dowiedzieć się, który format wideo wybrała przeglądarka?

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Typ</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Type"><code>video/xyz</code></td>
      <td data-th="Response">(pusty ciąg znaków)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response">(pusty ciąg znaków)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="nonsense, noise"</code></td>
      <td data-th="Response">(pusty ciąg znaków)</td>
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

### Przygotowanie filmu w wielu formatach

Użyj w JavaScripcie właściwości `currentSrc` elementu video, by odczytać nazwę odtwarzanego pliku źródłowego.

* Upewnij się, że Twój serwer odpowiada na żądania zakresu. Na większości serwerów ta funkcja jest domyślnie włączona, ale niektórzy administratorzy usług hostingowych ją wyłączają.
* GUI applications: [Miro](http://www.mirovideoconverter.com/), [HandBrake](//handbrake.fr/), [VLC](//www.videolan.org/)
* Online encoding/transcoding services: [Zencoder](//en.wikipedia.org/wiki/Zencoder), [Amazon Elastic Encoder](//aws.amazon.com/elastictranscoder)

### Sprawdzanie użytego formatu

Aby zobaczyć, jak to działa, skorzystaj z [tej strony demonstracyjnej](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html). Chrome i Firefox wybierają `chrome.webm` (to pierwszy obsługiwany przez nie plik źródłowy na liście dostępnych), a Safari &ndash; `chrome.mp4`.

Dla użytkowników wielkość ma znaczenie.

## Nadawanie filmom prawidłowych rozmiarów

Faktyczny rozmiar klatki wideo określony przy kodowaniu może być inny niż wymiary elementu video (tak jak obraz może być wyświetlany w rozmiarze innym niż rzeczywisty).

### Sprawdzanie rozmiaru wideo

* Narzędzia na komputer: [FFmpeg](//ffmpeg.org/)
* Aplikacje z interfejsem graficznym: [Miro](//www.mirovideoconverter.com/), [HandBrake](//handbrake.fr/), [VLC](//www.videolan.org/)
* Usługi kodowania/transkodowania online: [Zencoder](//en.wikipedia.org/wiki/Zencoder), [Amazon Elastic Encoder](//aws.amazon.com/elastictranscoder)

### Zapobieganie wychodzeniu filmów poza kontener

Aby sprawdzić rozmiar, w którym film został zakodowany, użyj właściwości `videoWidth` i `videoHeight` elementu video. Właściwości `width` i `height` zwracają wymiary elementu video, które można zmieniać, korzystając z CSS lub wbudowanych atrybutów width i height.

Gdy elementy video nie mieszczą się w widocznym obszarze, mogą wyjść poza swój kontener, uniemożliwiając użytkownikowi obejrzenie filmu i skorzystanie z elementów sterujących.

### Jak działa wykrywanie orientacji na różnych urządzeniach

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

Do kontrolowania wymiarów elementu video możesz używać JavaScriptu lub CSS. Biblioteki i wtyczki JavaScript takie jak [FitVids](//fitvidsjs.com/) pozwalają zachować odpowiedni współczynnik proporcji i rozmiar nawet w przypadku filmów Flash z YouTube lub innych źródeł.

Użyj [zapytań o media CSS](../../layouts/rwd-fundamentals/#use-css-media-queries-for-responsiveness), by określić wymiary elementów w zależności od wielkości widocznego obszaru. Świetnie sprawdza się `max-width: 100%`.

{# include shared/related_guides.liquid inline=true list=page.related-guides.media #}

W przypadku treści multimedialnych w elementach iframe (np. filmów z YouTube) zastosuj rozwiązanie elastyczne &ndash; takie jak [proponowane przez Johna Surdakowskiego](//avexdesigns.com/responsive-youtube-embed/)).

**CSS:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="styling"   adjust_indentation="auto" %}
</pre>

**CSS:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="markup"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/responsive_embed.html)

Porównaj [przykład strony elastycznej](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/responsive_embed.html) z [wersją nieelastyczną](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/unyt.html).

## Dostosowywanie odtwarzacza wideo

Na poszczególnych platformach filmy są odtwarzane w różny sposób. W rozwiązaniach mobilnych trzeba wziąć pod uwagę orientację urządzenia. Użyj interfejsu API Fullscreen, by sterować wyświetlaniem treści wideo na pełnym ekranie.

### Wyświetlanie w treści strony lub na pełnym ekranie

Na poszczególnych platformach filmy są odtwarzane w różny sposób. W rozwiązaniach mobilnych trzeba wziąć pod uwagę orientację urządzenia. Użyj interfejsu API Fullscreen, by sterować wyświetlaniem treści wideo na pełnym ekranie.

Orientacja urządzenia nie jest problemem na monitorach komputerowych ani laptopach, ale ma ogromne znaczenie przy projektowaniu stron internetowych na tablety i inne urządzenia mobilne.

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

Safari na iPhonie dobrze sobie radzi z przełączaniem się między orientacją pionową i poziomą:

<img alt="Zrzut ekranu z filmem odtwarzanym w Safari na iPhonie, orientacja pionowa"
src="images/iPad-Retina-landscape-video-playing.png" />

Na iPadzie oraz w Chrome na Androida orientacja urządzenia może stanowić problem. Na przykład niedostosowany film odtwarzany na iPadzie w orientacji poziomej wygląda tak:

## Ułatwienia dostępu są ważne

<img class="attempt-right" alt="Zrzut ekranu z filmem odtwarzanym w Safari na iPadzie Retina, orientacja pozioma"
src="images/iPhone-video-with-poster.png" />

Aby rozwiązać wiele problemów z układem związanych z orientacją urządzenia, przypisz elementowi video styl CSS z ustawieniem `width: 100%` lub `max-width: 100%`. Możesz też zastanowić się nad wykorzystaniem pełnego ekranu.

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Chrome on Android,
portrait" src="images/Chrome-Android-video-playing-portrait-3x5.png" />

On Android, users can request request fullscreen mode by clicking the fullscreen icon. But the default is to play video inline:

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Safari on iPad
Retina, landscape" src="images/iPad-Retina-landscape-video-playing.png" />

Safari on an iPad plays video inline:

<div style="clear:both;"></div>

### Sterowanie wyświetlaniem treści na pełnym ekranie

Safari na iPadzie odtwarza film w treści strony:

To full screen an element, like a video:

    elem.requestFullScreen();
    

Platformy, które nie wymuszają odtwarzania filmów w trybie pełnoekranowym, powszechnie obsługują [interfejs API Fullscreen](//caniuse.com/fullscreen). Pozwala on sterować wyświetlaniem treści lub całej strony na pełnym ekranie.

    document.body.requestFullScreen();
    

Aby w trybie pełnoekranowym wyświetlić element, np. video:

    video.addEventListener("fullscreenchange", handler);
    

Aby w trybie pełnoekranowym wyświetlić cały dokument:

    console.log("In full screen mode: ", video.displayingFullscreen);
    

Możesz też śledzić zmiany stanu wyświetlania na pełnym ekranie:

<video autoplay muted loop class="attempt-right">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.mp4" type="video/mp4">
  <p>Ta przeglądarka nie obsługuje elementu video.</p>
</video>

Jeśli chcesz sprawdzić, czy element jest obecnie w trybie pełnoekranowym:

Pseudoklasa CSS ":fullscreen" pozwala zmieniać sposób wyświetlania elementów w trybie pełnoekranowym.

Na urządzeniach, które obsługują interfejs API Fullscreen, możesz użyć obrazu miniatury jako elementu zastępczego filmu:

<div style="clear:both;"></div>

## Materiały referencyjne

Aby zobaczyć, jak to działa, skorzystaj ze [strony demonstracyjnej](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/fullscreen.html).

### Dodawanie napisów ułatwiających dostęp

<img class="attempt-right" alt="Screenshot showing captions displayed using the
track element in Chrome on Android" src="images/Chrome-Android-track-landscape-5x3.jpg" />

Ułatwienia dostępu to nie dodatkowa funkcja. Użytkownicy, którzy nie widzą lub nie słyszą, potrzebują opisów głosowych lub napisów, by zapoznać się z filmem. Koszt w postaci czasu poświęconego na dodanie tych elementów do filmu jest znacznie mniejszy niż negatywne skutki niezadowolenia użytkowników. Wszyscy użytkownicy powinni mieć dostęp przynajmniej do podstawowych elementów strony.

<div style="clear:both;"></div>

### Dodawanie elementu track

Aby poprawić dostępność multimediów na urządzeniach mobilnych, dodaj napisy i opisy, korzystając z elementu track.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/track.html" region_tag="track"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/track.html)

Tak wyglądają napisy z elementu track:

### Definiowanie napisów w pliku ścieżki

A track file consists of timed "cues" in WebVTT format:

    WEBVTT
    
    00:00.000 --> 00:04.000
    Mężczyzna siedzi na gałęzi drzewa i korzysta z laptopa.
    
    00:05.000 --> 00:08.000
    Gałąź się łamie i mężczyzna zaczyna spadać.
    
    ...
    

## Quick Reference

### Atrybuty elementu video

Do swojego filmu możesz bardzo łatwo dodać napisy &ndash; wystarczy dołączyć element track jako podrzędny elementu video:

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
      <td data-th="Description">Wszystkie przeglądarki</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>poster</code></td>
      <td data-th="Description">Wszystkie przeglądarki</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>preload</code></td>
      <td data-th="Description">Wszystkie przeglądarki mobilne go ignorują. </td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>autoplay</code></td>
      <td data-th="Description">Nie działa na iPhonie ani w Androidzie. Działa we wszystkich przeglądarkach na komputerze i iPadzie oraz w Firefoksie i Operze na Androida.</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>loop</code></td>
      <td data-th="Description">Wszystkie przeglądarki</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>controls</code></td>
      <td data-th="Description">Wszystkie przeglądarki</td>
    </tr>
  </tbody>
</table>

### JavaScript

Atrybut `src` elementu track wskazuje lokalizację pliku ścieżki.

Plik ścieżki zawiera teksty z sygnaturą czasową w formacie WebVTT:

* Transmisja danych może być droga.
* Uruchomienie pobierania i odtwarzania multimediów bez zgody użytkownika może nagle obciążyć łącze i procesor, spowalniając renderowanie strony.
* Użytkownik może być w miejscu, w którym odtwarzanie filmu lub dźwięku jest niestosowne.

Poniżej znajdziesz krótki przegląd właściwości elementu video.

### Preload {: #preload }

Pełną listę atrybutów elementu video i ich definicji znajdziesz w [specyfikacji elementu video](//www.w3.org/TR/html5/embedded-content-0.html#the-video-element).

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Wartość</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Value"><code>none</code></td>
      <td data-th="Description">Użytkownik może nie zechcieć obejrzeć filmu &ndash; nie należy wczytywać wstępnie żadnych danych.</td>
    </tr>
    <tr>
      <td data-th="Value"><code>metadata</code></td>
      <td data-th="Description">Należy wczytać metadane (czas trwania, wymiary, ścieżki tekstowe), ale z minimalnym fragmentem filmu.</td>
    </tr>
    <tr>
      <td data-th="Value"><code>auto</code></td>
      <td data-th="Description">Zalecane jest pobranie od razu całego filmu.</td>
    </tr>
  </tbody>
</table>

Na komputerze `autoplay` nakazuje przeglądarce, by jak najszybciej rozpoczęła pobieranie i odtwarzanie filmu. W iOS oraz Chrome na Androida `autoplay` nie działa. Użytkownik musi kliknąć ekran, by obejrzeć film.

### JavaScript

Nawet w przypadku platform, na których autoodtwarzanie jest możliwe, zastanów się, czy warto je włączać:

#### Autoodtwarzanie

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Property &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Property"><code>currentTime</code></td>
      <td data-th="Description">Odczytuje lub ustawia pozycję odtwarzania w sekundach.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>volume</code></td>
      <td data-th="Description">Odczytuje lub ustawia bieżący poziom głośności filmu.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>muted</code></td>
      <td data-th="Description">Sprawdza lub ustawia wyciszenie dźwięku.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>playbackRate</code></td>
      <td data-th="Description">Odczytuje lub ustawia tempo odtwarzania. 1&nbsp;to normalna szybkość do przodu.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>buffered</code></td>
      <td data-th="Description">Informacja o tym, jaka część filmu jest zbuforowana i gotowa do odtworzenia (zobacz <a href="http://people.mozilla.org/~cpearce/buffered-demo.html" title="Strona demonstracyjna ze wskaźnikiem buforowania filmu umieszczonym w elemencie canvas">stronę demonstracyjną</a>).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>currentSrc</code></td>
      <td data-th="Description">Adres odtwarzanego filmu.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoWidth</code></td>
      <td data-th="Description">Szerokość filmu w pikselach (może być inna niż elementu video).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoHeight</code></td>
      <td data-th="Description">Wysokość filmu w pikselach (może być inna niż elementu video).</td>
    </tr>
  </tbody>
</table>

#### Wstępne wczytywanie

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Method &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Method"><code>load()</code></td>
      <td data-th="Description">Wczytuje lub odświeża źródło filmu bez rozpoczynania odtwarzania. Na przykład wtedy, gdy zostało ono zmienione za pomocą kodu JavaScript.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>play()</code></td>
      <td data-th="Description">Odtwarza film od bieżącego miejsca.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>pause()</code></td>
      <td data-th="Description">Wstrzymuje film w bieżącym miejscu.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>canPlayType('format')</code></td>
      <td data-th="Description">Sprawdza, które formaty są obsługiwane (przeczytaj sekcję Sprawdzanie obsługiwanych formatów).</td>
    </tr>
  </tbody>
</table>

Działanie autoodtwarzania można skonfigurować w Android WebView, korzystając z [interfejsu API WebSettings](//developer.android.com/reference/android/webkit/WebSettings.html#setMediaPlaybackRequiresUserGesture(boolean)). Domyślnie jest ono włączone, ale w aplikacji WebView można je wyłączyć.

#### Właściwości

Atrybut `preload` podpowiada przeglądarce, ile informacji lub treści należy wstępnie wczytać.

<table class="responsive">
  <thead>
  <tr>
    <th colspan="2">Event &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Event"><code>canplaythrough</code></td>
      <td data-th="Description">Wywoływane, gdy na podstawie ilości dostępnych danych przeglądarka uznaje, że może odtworzyć cały film bez zakłóceń.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>ended</code></td>
      <td data-th="Description">Wywoływane po zakończeniu odtwarzania filmu.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>error</code></td>
      <td data-th="Description">Wywoływane po wystąpieniu błędu.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>playing</code></td>
      <td data-th="Description">Wywoływane po pierwszym rozpoczęciu odtwarzania filmu, wstrzymaniu lub ponownym rozpoczęciu.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>progress</code></td>
      <td data-th="Description">Wywoływane okresowo, by wskazać postępy pobierania.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>waiting</code></td>
      <td data-th="Description">Wywoływane, gdy działanie zostaje opóźnione z powodu oczekiwania na zakończenie innego działania.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>loadedmetadata</code></td>
      <td data-th="Description">Wywoływane, gdy przeglądarka zakończy wczytywanie metadanych filmu &ndash; czasu trwania, wymiarów i ścieżek tekstowych.</td>
    </tr>
  </tbody>
</table>

## Feedback {: #feedback }

Atrybut "preload" ma różne działanie na poszczególnych platformach. Na przykład Chrome na komputerze buforuje 25&nbsp;sekund filmu, a w iOS oraz Androidzie nie pobiera żadnych danych. To oznacza, że na urządzeniach mobilnych rozpoczęcie odtwarzania może się opóźnić, co nie zdarza się na komputerach. Szczegółowe informacje znajdziesz na [stronie testowej Steve`a Soudersa](//stevesouders.com/tests/mediaevents.php).