project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Większość stron internetowych nie jest zoptymalizowana do działania na różnych rodzajach urządzeń. Poznaj podstawy, dzięki którym Twoja witryna będzie działać na komputerach, urządzeniach mobilnych i wszystkich innych, które mają ekran.

{# wf_updated_on: 2014-04-29 #} {# wf_published_on: 2000-01-01 #}

# Podstawy elastycznego projektowania witryn {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Liczba urządzeń mobilnych używanych do surfowania po sieci rośnie w astronomicznym tempie, jednak większość stron internetowych nie jest zoptymalizowana do działania na tych urządzeniach. Częstym ograniczeniem urządzeń mobilnych jest rozmiar wyświetlacza, dlatego wymagają one stosowania innych sposobów rozmieszczania treści na ekranie.

{% include "web/_shared/udacity/ud893.html" %}

<video autoplay muted loop controls>
  <source src="videos/resize.webm" type="video/webm">
  <source src="videos/resize.mp4" type="video/mp4">
</video>

Ekrany mogą być w wielu różnych rozmiarach &ndash; na telefonach, `fabletach`, tabletach, komputerach, konsolach do gier, telewizorach, a nawet urządzeniach do noszenia. Zawsze będą pojawiać się nowe, dlatego witryna powinna dostosowywać się do każdego rozmiaru ekranu &ndash; teraz i w przyszłości.

Elastyczne projektowanie witryn, pierwotnie zdefiniowane przez [Ethana Marcotte`a w czasopiśmie A List Apart](http://alistapart.com/article/responsive-web-design/){: .external } to odpowiedź na potrzeby użytkowników i ich urządzeń. Układ strony zmienia się w zależności od rozmiaru i możliwości urządzenia. Na przykład na telefonie użytkownik widzi treści w jednej kolumnie. Z kolei na tablecie te same treści są już wyświetlane w dwóch kolumnach.

## Ustawianie tagu viewport

Strony zoptymalizowane pod kątem działania na rozmaitych urządzeniach muszą w nagłówku dokumentu zawierać metatag viewport. Przekazuje on przeglądarce instrukcje, jak sterować wymiarami i skalowaniem strony.

### TL;DR {: .hide-from-toc }

- Użyj metatagu viewport, by sterować szerokością i skalowaniem widocznego obszaru w przeglądarkach.
- Dołącz tag `width=device-width`, by dopasować stronę do szerokości ekranu w pikselach niezależnych od urządzenia.
- Dołącz tag `initial-scale=1`, by utworzyć relację 1:1 między pikselami CSS a pikselami niezależnymi od urządzenia.
- Nie wyłączaj skalowania strony przez użytkownika, by nie ograniczać jej dostępności.

Aby strona działała jak najlepiej, przeglądarki mobilne renderują ją w szerokości ekranu komputera (zwykle około 980&nbsp;pikseli, choć zdarzają się też inne wartości), a potem próbują poprawić wygląd treści, zwiększając czcionki i skalując zawartość, by pasowała do ekranu. W takiej sytuacji rozmiary czcionek mogą być niespójne, a użytkownik musi kliknąć dwukrotnie lub inaczej zmienić powiększenie, by zobaczyć treści i wejść z nimi w interakcję.

    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    

Wartość metatagu viewport `width=device-width` powoduje, że strona dopasowuje się do szerokości ekranu w pikselach niezależnych od urządzenia. Dzięki temu jej zawartość może zostać ułożona odpowiednio do danego rozmiaru ekranu &ndash; zarówno małego w telefonie komórkowym, jak i dużego w monitorze komputera.

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

Niektóre przeglądarki utrzymują stałą szerokość strony podczas obrotu do trybu poziomego i powiększają widok zamiast na nowo ułożyć zawartość na ekranie. Atrybut `initial-scale=1` poleca przeglądarce ustanowić relację 1:1 między pikselami CSS a pikselami niezależnymi od urządzenia, bez względu na jego orientację. To pozwala wykorzystać pełną szerokość strony w trybie poziomym.

### TL;DR {: .hide-from-toc }

Note: Atrybuty rozdziel przecinkami, by starsze przeglądarki analizowały je prawidłowo.

- `minimum-scale`
- `maximum-scale`
- `user-scalable`

Oprócz `initial-scale` możesz też ustawić te atrybuty tagu viewport:

## Ułatwianie dostępu do treści w widocznym obszarze

Po ustawieniu mogą one uniemożliwić użytkownikowi powiększanie widocznego obszaru, powodując problemy z dostępnością.

### TL;DR {: .hide-from-toc }

- Nie używaj dużych elementów o stałej szerokości.
- Prawidłowe renderowanie treści nie powinno zależeć od konkretnej szerokości widocznego obszaru.
- Użyj zapytań o media CSS, by zastosować różne style na małych i dużych ekranach.

Zarówno na komputerach, jak i urządzeniach mobilnych użytkownicy są przyzwyczajeni do przewijania stron w pionie, ale nie w poziomie. Zmuszanie użytkownika do przewijania w poziomie lub pomniejszania widoku, gdy chce zobaczyć pozostałą część strony, obniża wygodę obsługi.

Podczas tworzenia witryny mobilnej z metatagiem `viewport` łatwo przypadkowo dodać do strony treści, które nie pasują do określonego widocznego obszaru. Na przykład obraz szerszy niż widoczny obszar powoduje konieczność przewijania w poziomie. Elementy tego typu trzeba dopasować do szerokości widocznego obszaru, tak by użytkownik nie musiał przewijać ich w bok.

Wymiary ekranu i szerokość w pikselach CSS mogą bardzo się różnić na poszczególnych urządzeniach (np. telefonach i tabletach czy nawet różnych telefonach), dlatego prawidłowe renderowanie treści nie powinno zależeć od konkretnej szerokości widocznego obszaru.

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

## Dostosowywanie rozmiaru treści do widocznego obszaru

Ustawienie dużych bezwzględnych szerokości CSS elementów strony (tak jak w przykładzie poniżej) spowoduje, że element `div` będzie zbyt szeroki dla widocznego obszaru na węższym urządzeniu (np. iPhonie, który ma 320&nbsp;pikseli szerokości CSS). Zamiast tego użyj względnych wartości szerokości, na przykład `width: 100%`. Podobnie pamiętaj, by nie używać dużych bezwzględnych wartości pozycji, które mogą spowodować, że element znajdzie się poza widocznym obszarem na małym ekranie.

### Stosowanie zapytań o media na podstawie rozmiaru widocznego obszaru

- Zapytań o media możesz używać, by stosować style na podstawie cech urządzenia.
- Użyj `min-width` zamiast `min-device-width`, by interfejs był jak najszerszy.
- Użyj względnych rozmiarów elementów, by uniknąć zniekształcenia układu.

For example, you could place all styles necessary for printing inside a print media query:

    <link rel="stylesheet" href="print.css" media="print">
    

Zapytania o media to proste filtry, które można zastosować do stylów CSS. Ułatwiają zmianę stylów na podstawie cech urządzenia, które renderuje treści, takich jak typ wyświetlacza, szerokość, wysokość, orientacja, a nawet rozdzielczość.

    @media print {
      /* print style sheets go here */
    }
    
    @import url(print.css) print;
    

Na przykład możesz umieścić wszystkie style potrzebne do drukowania w zapytaniu o media `print`:

### Uwaga o `min-device-width`

Istnieją jeszcze dwa inne sposoby (oprócz atrybutu `media` w linku arkusza stylów) użycia zapytań o media stosowane w pliku CSS: `@media` i `@import`. Ze względu na wydajność zamiast instrukcji `@import` zalecamy dwie pierwsze metody (przeczytaj sekcję [Unikanie importu CSS](/web/fundamentals/performance/critical-rendering-path/page-speed-rules-and-recommendations)).

    @media (query) {
      /* CSS Rules used when query matches */
    }
    

Reguły logiczne stosowane przy zapytaniach o media nie wykluczają się nawzajem i każdy filtr, który spełnia kryteria danego bloku CSS, zostanie zastosowany zgodnie ze standardowymi regułami pierwszeństwa w CSS.

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Atrybut</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="attribute"><code>min-width</code></td>
      <td data-th="Result">Reguły obowiązują w każdej przeglądarce, której szerokość przekracza wartość podaną w zapytaniu.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>max-width</code></td>
      <td data-th="Result">Reguły obowiązują w każdej przeglądarce, której szerokość jest poniżej wartości podanej w zapytaniu.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>min-height</code></td>
      <td data-th="Result">Reguły obowiązują w każdej przeglądarce, której wysokość przekracza wartość podaną w zapytaniu.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>max-height</code></td>
      <td data-th="Result">Reguły obowiązują w każdej przeglądarce, której wysokość jest poniżej wartości podanej w zapytaniu.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>orientation=portrait</code></td>
      <td data-th="Result">Reguły obowiązują w każdej przeglądarce, której wysokość jest równa szerokości lub większa.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>orientation=landscape</code></td>
      <td data-th="Result">Reguły obowiązują w każdej przeglądarce, której szerokość jest większa niż wysokość.</td>
    </tr>
  </tbody>
</table>

Zapytania o media umożliwiają tworzenie elastycznych interfejsów, w których wybrane style są stosowane na małych, dużych i wszystkich pośrednich ekranach. Składnia zapytań o media pozwala opracowywać reguły stosowane w zależności od cech urządzenia.

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

Jest kilka różnych elementów, których może dotyczyć zapytanie, jednak przy elastycznym projektowaniu witryn najczęściej używa się `min-width`, `max-width`, `min-height` i `max-height`.

- Gdy przeglądarka ma szerokość od **0** do **640&nbsp;pikseli**, stosujemy plik `max-640px.css`.
- Gdy przeglądarka ma szerokość od **500** do **600&nbsp;pikseli**, stosujemy style z bloku `@media`.
- Gdy przeglądarka ma szerokość **co najmniej 640&nbsp;pikseli**, stosujemy plik `min-640px.css`.
- Gdy przeglądarka ma **szerokość większą niż wysokość**, stosujemy plik `landscape.css`.
- Gdy przeglądarka ma **wysokość większą niż szerokość**, stosujemy plik `portrait.css`.

### Korzystanie z jednostek względnych

Przyjrzyjmy się przykładowi:

Zapytania o media można też tworzyć, korzystając z atrybutu `*-device-width`, jednak **zdecydowanie odradzamy** tę metodę.

Różnica jest niewielka, ale bardzo ważna: `min-width` zależy od rozmiaru okna przeglądarki, a `min-device-width` &ndash; od rozmiaru ekranu. Niektóre przeglądarki (w tym starsza przeglądarka w Androidzie) mogą nie zgłaszać prawidłowo szerokości urządzenia &ndash; zamiast oczekiwanej szerokości widocznego obszaru podają rozmiar ekranu w pikselach urządzenia.

### TL;DR {: .hide-from-toc }

Oprócz tego użycie `*-device-width` może uniemożliwiać dostosowywanie układu treści na komputerach i innych urządzeniach, które pozwalają zmieniać wymiary okien. Powodem jest to, że zapytanie dotyczy rzeczywistej szerokości urządzenia, a nie rozmiaru okna przeglądarki.

### Określ główne punkty graniczne, zaczynając od małego rozmiaru i stopniowo go powiększając

Podstawowa zaleta, która odróżnia projektowanie elastyczne od układów o stałej szerokości, to płynność i proporcjonalność. Stosowanie jednostek względnych miary pomaga uprościć układy i zapobiega przypadkowemu tworzeniu komponentów, które wychodzą poza widoczny obszar.

Na przykład ustawienie `width: 100%` w elemencie div najwyższego poziomu gwarantuje, że rozciągnie się on na szerokość widocznego obszaru i nigdy nie będzie zbyt duży ani zbyt mały. Element div będzie zawsze pasować &ndash; zarówno na iPhonie o szerokości 320&nbsp;pikseli, Blackberry Z10 o szerokości 342&nbsp;pikseli, jak i na Nexusie 5 o szerokości 360&nbsp;pikseli.

Jednostki względne pozwalają też przeglądarkom renderować treści zgodnie z poziomem powiększenia ustawionym przez użytkownika, bez potrzeby dodawania poziomych pasków przewijania strony.

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
    

## Zwiększanie elastyczności dzięki zapytaniom o media CSS

Podczas definiowania punktów granicznych można kierować się klasami urządzeń, jednak lepiej zachować ostrożność. Utrzymanie punktów granicznych utworzonych pod konkretne, używane obecnie urządzenia, produkty, marki czy systemy operacyjne, może okazać się wyjątkowo pracochłonne. Sposób dostosowywania układu do kontenera powinien zależeć od samych treści.

### W razie potrzeby określ dodatkowe punkty graniczne

- Utwórz punkty graniczne na podstawie treści, nigdy pod konkretne urządzenia, produkty czy marki.
- Projektowanie zacznij od najmniejszego urządzenia mobilnego, a potem stopniowo powiększaj interfejs wraz ze wzrostem rozmiaru ekranów.
- Postaraj się, by długość wierszy tekstu nie przekraczała 70-80&nbsp;znaków.

### Zoptymalizuj tekst do czytania

<figure class="attempt-right">
  <img src="imgs/weather-1.png" srcset="imgs/weather-1.png 1x, imgs/weather-1-2x.png 2x" alt="">
  <figcaption>
    <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/weather-1.html">
      Preview of the weather forecast displayed on a small screen.
    </a>
  </figcaption>
</figure>

Najpierw zaprojektuj treści tak, by pasowały do ekranu o małym rozmiarze, a potem powiększaj go, aż trzeba będzie utworzyć punkt graniczny. W ten sposób zoptymalizujesz punkty pod kątem treści i uzyskasz najmniejszą ich liczbę.

Opracujmy przykład pokazany na początku &ndash; [prognozę pogody](/web/fundamentals/design-and-ux/responsive/). W pierwszej kolejności postaraj się, by prognoza dobrze wyglądała na małym ekranie.

<div style="clear:both;"></div>

<figure class="attempt-right">
  <img src="imgs/weather-2.png" class="center" srcset="imgs/weather-2.png 1x, imgs/weather-2-2x.png 2x" alt="Preview of the weather forecast as the page gets wider.">
  <figcaption>
    <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/weather-1.html">
      Preview of the weather forecast as the page gets wider.
    </a>
  </figcaption>
</figure>

Następnie powiększaj okno przeglądarki, aż między poszczególnymi elementami będzie tyle pustego miejsca, że prognoza przestanie wyglądać dobrze. Ocena jest dość subiektywna, ale ponad 600&nbsp;pikseli szerokości to z pewnością za dużo.

<div style="clear:both;"></div>

Aby wstawić punkt graniczny przy 600&nbsp;pikselach, utwórz dwa nowe arkusze stylów: jeden stosowany wtedy, gdy szerokość przeglądarki nie przekracza 600&nbsp;pikseli, a drugi &ndash; powyżej tej wartości.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-2.html" region_tag="mqweather2" adjust_indentation="auto" %}
</pre>

Na koniec popraw style CSS. W tym przykładzie umieściliśmy wspólne style (takie jak czcionki, ikony, podstawowe pozycjonowanie oraz kolory) w pliku `weather.css`. Konkretne układy na mały ekran są w pliku `weather-small.css`, a style na duży &ndash; w pliku `weather-large.css`.

<figure class="attempt-right">
  <img src="imgs/weather-3.png"  srcset="imgs/weather-3.png 1x, imgs/weather-3-2x.png 2x" alt="Preview of the weather forecast designed for a wider screen.">
  <figcaption>
    <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/weather-2.html">
      Preview of the weather forecast designed for a wider screen.
    </a>
  </figcaption>
</figure>

Oprócz głównych punktów granicznych, przy których układ znacznie się zmienia, warto też wyznaczyć miejsca wprowadzania drobnych zmian. Na przykład między głównymi punktami granicznymi możesz dostosowywać marginesy lub odstępy elementu czy zwiększać rozmiar czcionki, by wyglądała ona bardziej naturalnie w układzie.

<div style="clear:both"></div>

### Nigdy nie ukrywaj zupełnie treści

Zacznijmy od zoptymalizowania układu na małym ekranie. Gdy szerokość widocznego obszaru przekroczy 360&nbsp;pikseli, powiększymy czcionkę. Jeśli na ekranie będzie dość miejsca, rozdzielimy najniższą i najwyższą temperaturę &ndash; będą w tym samym wierszu zamiast jedna nad drugą. Powiększymy też nieco ikony symbolizujące warunki pogodowe.

Let's start by optimizing the small screen layout. In this case, let's boost the font when the viewport width is greater than 360px. Second, when there is enough space, we can separate the high and low temperatures so that they're on the same line instead of on top of each other. And let's also make the weather icons a bit larger.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-small.css" region_tag="mqsmallbpsm"   adjust_indentation="auto" %}
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

Podobnie na dużych ekranach warto ograniczyć maksymalną szerokość panelu prognozy, by nie zajął całej szerokości ekranu.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-large.css" region_tag="mqsmallbplg"   adjust_indentation="auto" %}
</pre>

### Optimize text for reading

Według klasycznych zasad gwarantujących czytelność tekstu idealna szpalta powinna zawierać 70-80&nbsp;znaków w wierszu (około 8-10&nbsp;wyrazów). Dlatego za każdym razem, gdy wiersz w bloku tekstu przekroczy 10&nbsp;wyrazów, należy rozważyć utworzenie punktu granicznego.

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
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/reading.html" region_tag="mqreading"   adjust_indentation="auto" %}
</pre>

Przyjrzyjmy się dokładniej powyższemu przykładowi posta na blogu. Na mniejszych ekranach czcionka Roboto o rozmiarze 1&nbsp;em działa idealnie, dając 10&nbsp;wyrazów w wierszu, ale na większych wymaga punktu granicznego. Jeśli szerokość przeglądarki przekroczy 575&nbsp;pikseli, idealna szerokość treści to 550&nbsp;pikseli.

### Never completely hide content

Przy podejmowaniu decyzji, które treści ukryć lub pokazać w zależności od rozmiaru ekranu, zachowaj ostrożność. Nie ukrywaj treści tylko dlatego, że nie mieszczą się na ekranie. Rozmiar ekranu nie jest ostatecznym wyznacznikiem oczekiwań użytkownika. Na przykład usunięcie liczby pyłków z prognozy pogody może być bardzo kłopotliwe dla osób, które wiosną cierpią na alergie i potrzebują informacji, czy mogą bez obaw wyjść z domu.

## Jak wybierać punkty graniczne

Once you've got your media query breakpoints set up, you'll want to see how your site looks with them. You *could* resize your browser window to trigger the breakpoints, but there's a better way: Chrome DevTools. The two screenshots below demonstrate using DevTools to view how a page looks under different breakpoints.

![Example of DevTools' media queries feature](imgs/devtools-media-queries-example.png)

To view your page under different breakpoints:

[Open DevTools](/web/tools/chrome-devtools/#open) and then turn on [Device Mode](/web/tools/chrome-devtools/device-mode/#toggle).

Use the [viewport controls](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#viewport-controls) to select **Responsive**, which puts DevTools into responsive mode.

Last, open the Device Mode menu and select [**Show media queries**](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#media-queries) to display your breakpoints as colored bars above your page.

Click on one of the bars to view your page while that media query is active. Right-click on a bar to jump to the media query's definition. See [Media queries](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#media-queries) for more help.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}