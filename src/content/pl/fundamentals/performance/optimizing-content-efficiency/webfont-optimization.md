project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Zagadnienie typografii ma kluczowe znaczenie w dążeniu do poprawności w projektowaniu, budowaniu marki, czytelności i dostępności dla niepełnosprawnych. Czcionki sieci web pozwalają osiągnąć powyższe cele i zwiększyć funkcjonalność tekstu: taki tekst można zaznaczać, wyszukiwać, dowolnie powiększać, jest przyjazny dla urządzeń o wysokiej rozdzielczości, zapewnia ostrość renderowania niezależnie od rozmiaru i rozdzielczości ekranu.

{# wf_updated_on: 2014-09-29 #} {# wf_published_on: 2014-09-19 #}

# Optymalizacja czcionek sieci web {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*This article contains contributions from [Monica Dinculescu](https://meowni.ca/posts/web-fonts/), [Rob Dodson](/web/updates/2016/02/font-display), and Jeff Posnick.*

Optymalizacja czcionek sieci web jest kluczowym elementem strategii zwiększania wydajności. Każda czcionka to dodatkowy zasób zdolny do zablokowania renderowania tekstu, ale korzystanie z czcionek sieci web na stronie wcale nie oznacza, że strona musi renderować się wolniej. Wręcz przeciwnie, zoptymalizowana czcionka w połączeniu z rozsądną strategią wczytywania i wywoływania na stronie może zmniejszyć łączny rozmiar strony i skrócić czas jej renderowania.

Czcionka sieci web to zbiór glifów, z których każdy jest określonym wektorowo kształtem odzwierciedlającym literę lub symbol. Dlatego rozmiar pliku danej czcionki zależy od dwóch zmiennych: złożoności ścieżek wektorowych w obrębie każdego glifu i liczby glifów w obrębie danej czcionki. Na przykład czcionka Open Sans, będąca jedną z najpopularniejszych czcionek sieci web, zawiera 897 glifów, w tym znaki łacińskie, greckie i cyrylicę.

## Budowa czcionki sieci web

### TL;DR {: .hide-from-toc }

* Czcionki Unicode mogą zawierać tysiące glifów
* Stosuje się czcionki w czterech formatach: WOFF2, WOFF, EOT, TTF
* Niektóre formaty czcionek wymagają zastosowania kompresji GZIP

A *webfont* is a collection of glyphs, and each glyph is a vector shape that describes a letter or symbol. As a result, two simple variables determine the size of a particular font file: the complexity of the vector paths of each glyph and the number of glyphs in a particular font. For example, Open Sans, which is one of the most popular webfonts, contains 897 glyphs, which include Latin, Greek, and Cyrillic characters.

<img src="images/glyphs.png"  alt="Font glyph table" />

Jest oczywiste, że korzystanie z czcionek sieci web wymaga zastosowania dobrze przemyślanych zabiegów, by doskonała typografia nie kolidowała z wydajnością. Na szczęście platforma WWW dostarcza wszystkich potrzebnych funkcji podstawowych. W pozostałej części tego przewodnika zajmiemy się praktycznym wykorzystaniem najlepszych cech wszystkich rozwiązań.

Obecnie w Internecie korzysta się z czterech formatów kontenerów czcionek: [EOT](http://en.wikipedia.org/wiki/Embedded_OpenType){: .external }, [TTF](http://pl.wikipedia.org/wiki/TrueType), [WOFF](http://pl.wikipedia.org/wiki/Web_Open_Font_Format) i [WOFF2](http://www.w3.org/TR/WOFF2/). Niestety, mimo wielu możliwości wyboru, nie istnieje jeden uniwersalny format obsługiwany przez wszystkie starsze i nowsze przeglądarki: format EOT obsługuje [tylko przeglądarka IE](http://caniuse.com/#feat=eot), w przeglądarce IE obecna jest [częściowa obsługa formatu TTF](http://caniuse.com/#search=ttf), format WOFF jest najlepiej obsługiwany, ale [niedostępny w niektórych starszych przeglądarkach](http://caniuse.com/#feat=woff), a wdrażanie obsługi formatu WOFF 2.0 w wielu przeglądarkach [ciągle trwa](http://caniuse.com/#feat=woff2).

### Formaty czcionek sieci web

Co to dla nas oznacza? Nie istnieje jeden format obsługiwany przez wszystkie przeglądarki, co oznacza konieczność stosowania wielu formatów, jeśli wrażenia użytkowników mają pozostać spójne:

Note: Prawdę powiedziawszy, istnieje również [kontener czcionek SVG](http://caniuse.com/svg-fonts), ale nigdy nie obsługiwała go ani przeglądarka IE, ani Firefox, a obecnie oznaczono go jako przestarzały w przeglądarce Chrome. Z tego względu ma on ograniczone zastosowanie i nie będzie omawiany w tym przewodniku.

* Przesyłaj wariant WOFF 2.0 do przeglądarek go obsługujących
* Przesyłaj wariant WOFF do większości przeglądarek
* Przesyłaj wariant TTF do starszych przeglądarek na Androidzie (starszych od wersji 4.4)
* Przesyłaj wariant EOT do starszych przeglądarek IE (starszych od wersji 9)

Czcionka to zbiór glifów, z których każdy stanowi kolekcję ścieżek opisujących kształt litery. Poszczególne glify są oczywiście różne, ale mimo to zawierają mnóstwo podobnych informacji, które można poddać kompresji metodą GZIP lub inną kompatybilną:

### Redukcja rozmiaru czcionki dzięki kompresji

Warto również wspomnieć, że niektóre formaty czcionek zawierają dodatkowe metadane, np. odnoszące się do [hintingu](http://pl.wikipedia.org/wiki/Hinting){: .external } i [kerningu](http://pl.wikipedia.org/wiki/Kerning), które mogą być zbędne na niektórych platformach, co umożliwia dalszą optymalizację. Sprawdź w dokumentacji algorytmu kompresji czcionek, jakie opcje optymalizacji są dostępne, i upewnij się, że dysponujesz odpowiednią infrastrukturą do testowania i dostarczania takich zoptymalizowanych czcionek dla każdej przeglądarki &ndash; np. Google Fonts utrzymuje ponad 30 zoptymalizowanych wariantów dla każdej czcionki i automatycznie wykrywa i dostarcza optymalny wariant dla każdej platformy i przeglądarki.

* Domyślnie pliki formatów EOT i TTF nie są kompresowane: upewnij się, że konfiguracja serwerów wymusza stosowanie [kompresji GZIP](/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text-compression-with-gzip) przy przesyłaniu plików w tych formatach.
* Format WOFF posiada wbudowaną kompresję &ndash; upewnij się, że algorytm kompresji formatu WOFF stosuje optymalne ustawienia kompresji.
* Format WOFF2 obejmuje niestandardowe przetwarzanie wstępne i algorytmy kompresji umożliwiające redukcję rozmiaru pliku o ok. 30% w stosunku do innych formatów &ndash; patrz [raport](http://www.w3.org/TR/WOFF20ER/){: .external }.

Note: Zastanów się nad wykorzystaniem [kompresji Zopfli](http://en.wikipedia.org/wiki/Zopfli) do optymalizacji czcionek w formatach EOT, TTF i WOFF. Zopfli to kompresor zgodny z formatem zlib, zapewniający redukcję rozmiaru pliku o ok. 5% w stosunku do kompresji gzip.

@font-face jest regułą CSS umożliwiającą określenie lokalizacji konkretnego zasobu czcionki, jej stylu i kodów Unicode, dla których powinien obowiązywać. Połączenie takich deklaracji reguł @font-face można wykorzystać do utworzenia `rodziny czcionek`, dzięki której przeglądarka może określić, które czcionki trzeba pobrać i zastosować na bieżącej stronie. Przyjrzyjmy się dokładniej, jak to działa.

## Określanie rodziny czcionek regułą @font-face

### TL;DR {: .hide-from-toc }

* Formaty czcionek możesz określić za pomocą wskazówki format()
* Aby zwiększyć wydajność, wydzielaj podzbiory z obszernych zbiorów czcionek Unicode: stosuj wydzielanie zakresów Unicode i ręczne wydzielanie podzbiorów w przypadku starszych przeglądarek
* Ograniczaj liczbę różnych wariantów stylistycznych czcionek, by zwiększyć wydajność wczytywania i renderowania stron

W każdej deklaracji reguły @font-face określa się nazwę rodziny czcionek, dzięki której może ona pełnić rolę logicznej grupy wielu deklaracji, [własności czcionki](http://www.w3.org/TR/css3-fonts/#font-prop-desc), takie jak styl, grubość, rozciągnięcie i [deskryptor źródłowy](http://www.w3.org/TR/css3-fonts/#src-desc) zawierający uporządkowaną według ważności listę lokalizacji tego zasobu czcionki.

### Wybór formatu

Przede wszystkim zwróć uwagę, że w powyższych przykładach określono jedną rodzinę czcionek *Awesome Font* o dwóch stylach (normalny i *kursywa*), z których każdy jest skojarzony z innym zestawem zasobów czcionek. Z kolei każdy deskryptor `src` zawiera uporządkowaną według ważności, rozdzieloną przecinkami, listę wariantów zasobu:

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
    

Note: Jeśli strona nie odwołuje się do jednej z domyślnych czcionek systemowych, w praktyce rzadko są one zainstalowane lokalnie, zwłaszcza w przypadku urządzeń mobilnych, na których instalacja dodatkowych czcionek jest w zasadzie niemożliwa. Z tego powodu należy zawsze udostępniać listę zewnętrznych lokalizacji czcionek.

* Dyrektywa `local()` umożliwia odwoływanie się, wczytywanie i używanie czcionek zainstalowanych lokalnie.
* Dyrektywa `url()` umożliwia wczytywanie czcionek zewnętrznych. Można do niej dołączyć opcjonalną wskazówkę `format()` w celu opisania formatu czcionki opisanej podanym adresem URL.

Gdy przeglądarka ustali, że dana czcionka jest potrzebna, odczytuje w określonej kolejności podaną listę zasobów i próbuje wczytać odpowiedni zasób. Korzystając z powyższego przykładu:

Połączenie dyrektyw lokalnych i zewnętrznych z odpowiednimi wskazówkami opisującymi format pozwala określić wszystkie dostępne formaty czcionek i powierzyć przeglądarce pozostałe czynności: stwierdzenie, które zasoby są wymagane, i wybór ich optymalnego formatu.

1. Przeglądarka wyznacza układ strony i określa, które warianty czcionki są wymagane do zrenderowania pewnego tekstu na stronie.
2. Dla każdej wymaganej czcionki przeglądarka sprawdza, czy czcionka jest dostępna lokalnie.
3. Jeśli plik nie jest dostępny lokalnie, wczytywane kolejno są definicje zewnętrzne: 
    * Jeśli obecna jest wskazówka opisująca format, przed rozpoczęciem pobierania przeglądarka sprawdza, czy go obsługuje, a jeśli nie, przechodzi do następnej pozycji.
    * Jeśli nie podano wskazówki opisującej format, przeglądarka pobiera zasób.

Note: Kolejność podania kolejnych wariantów czcionek ma znaczenie. Przeglądarka wybiera pierwszy obsługiwany format. Dlatego jeśli w nowszych przeglądarkach powinien być używany format WOFF2, deklarację czcionki w tym formacie należy umieścić przed deklaracją czcionki w formacie WOFF i tak dalej.

Oprócz właściwości czcionek, takich jak styl, grubość i rozciągniecie, reguła @font-face pozwala określić, jaki zakres Unicode ma być obsługiwany przez każdy z zasobów. Umożliwia to podział dużych czcionek Unicode na mniejsze podzbiory (np. zbiory czcionki łacińskiej, greckiej, cyrylicy) i pobieranie tylko glifów wymaganych do renderowania tekstu na konkretnej stronie.

### Wydzielanie zakresów Unicode

[Deskryptor zakresu Unicode](http://www.w3.org/TR/css3-fonts/#descdef-unicode-range) umożliwia określenie wartości zakresów na liście rozdzielanej przecinkami, przy czym każdy z zakresów może przyjąć jedną z trzech postaci:

Na przykład naszą rodzinę czcionek *Awesome Font* możemy rozdzielić na podzbiory czcionki łacińskiej i japońskiej, z których każdy przeglądarka będzie mogła pobrać według potrzeby:

* Jedna wartość kodu (np. U+416)
* Zakres wartości (np. U+400-4ff): początek i koniec zakresu kodów
* Zakres określony symbolem wieloznacznym (np. U+4??): znak `?` oznacza dowolną cyfrę heksadecymalną

Note: Wydzielanie zakresów Unicode ma szczególne znaczenie w przypadku języków azjatyckich, ponieważ liczba glifów jest o wiele większa niż w językach europejskich, a typowy pełny zbiór czcionek mierzy się w megabajtach, a nie w dziesiątkach kilobajtów.

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
    

Użycie podzbiorów zakresów Unicode i osobnych plików dla każdego wariantu stylistycznego czcionki pozwala utworzyć złożoną rodzinę czcionek, co zwiększa zarówno prędkość, jak i efektywność pobierania &ndash; użytkownik będzie pobierać tylko potrzebne warianty i podzbiory, a nie te, których być może nigdy nie zobaczy lub nie wykorzysta na stronie.

Pamiętajmy jednak o małym haczyku: [jeszcze nie wszystkie przeglądarki obsługują zakresy Unicode](http://caniuse.com/#feat=font-unicode-range). Niektóre przeglądarki po prostu ignorują wskazówkę z opisem zakresu Unicode i pobierają wszystkie warianty, a inne mogą w ogóle nie przetwarzać deklaracji @font-face. Aby poradzić sobie z tym problemem, musimy uciec się do "ręcznego wydzielania podzbiorów" w starszych przeglądarkach.

Ponieważ starsze przeglądarki nie są na tyle inteligentne, by wybrać tylko niezbędne zasoby i utworzyć czcionkę złożoną, musimy uciec się do dostarczenia pojedynczego zasobu czcionki, który będzie zawierać wszystkie niezbędne podzbiory, a pozostałe ukryć przed przeglądarką. Na przykład, jeśli strona wykorzystuje tylko znaki łacińskie, możemy pozbyć się pozostałych glifów i przesłać tylko określony podzbiór jako samodzielny zasób.

Każda rodzina czcionek składa się z wielu wariantów stylistycznych (normalnego, pogrubionego, kursywy) i wielu stopni pogrubienia dla każdego stylu, z których z kolei każdy może zawierać glify o bardzo różnych kształtach &ndash; np. o różnej wielkości rozstrzelenia, różnym rozmiarze albo w ogóle o odmiennym kształcie.

1. **W jaki sposób określić, które podzbiory są potrzebne?** 
    * Jeśli przeglądarka obsługuje wydzielanie zakresów Unicode, wybierze automatycznie właściwy podzbiór. Strona będzie wymagać tylko dostarczenia plików podzbiorów i określenia odpowiednich zakresów Unicode w regułach @font-face.
    * W przypadku braku obsługi zakresów Unicode strona będzie musiała ukryć wszystkie zbędne podzbiory &ndash; tzn. programista będzie musiał samodzielnie określić wymagane podzbiory.
2. **Jak wygenerować podzbiory czcionek?** 
    * Do określenia podzbiorów i optymalizacji czcionek można użyć narzędzia typu open source [pyftsubset](https://github.com/behdad/fonttools/blob/master/Lib/fontTools/subset.py#L16).
    * Niektóre serwisy z czcionkami umożliwiają wydzielanie podzbiorów z użyciem własnych parametrów zapytań, dzięki czemu można ręcznie określić podzbiór wymagany przez stronę &ndash; szczegóły możesz sprawdzić w dokumentacji serwisu.

### Wybór i synteza czcionek

Each font family is composed of multiple stylistic variants (regular, bold, italic) and multiple weights for each style, each of which, in turn, may contain very different glyph shapes&mdash;for example, different spacing, sizing, or a different shape altogether.

<img src="images/font-weights.png"  alt="Font weights" />

Podobna procedura obowiązuje dla różnych wariantów *kursywy*. Projektant czcionek ma kontrolę nad tym, które warianty zostaną utworzone, a my mamy kontrolę nad tym, których wariantów użyjemy na stronie &ndash; a ponieważ każdy wariant wymaga osobnego pobierania, dobrze jest ograniczyć ich liczbę do minimum. Na przykład możemy określić dwa warianty naszej rodziny czcionek *Awesome Font*:

> Po określeniu grubości, dla której nie istnieje dedykowana czcionka, używana jest czcionka o najbliższej grubości. Generalnie warianty pogrubione są mapowane do wariantów o większych grubościach, a warianty normalne do wariantów o mniejszych grubościach.
> 
> > [Algorytm dopasowywania czcionek CSS3](http://www.w3.org/TR/css3-fonts/#font-matching-algorithm)

W powyższym przykładzie zadeklarowano rodzinę czcionek *Awesome Font* składającą się z dwóch zasobów, które obejmują ten sam zestaw glifów łacińskich (U+000-5FF), ale cechują je inne grubości: normalna (400) i pogrubienie (700). Jednak co się stanie, jeśli w jednej z naszych reguł CSS zostanie użyta inna grubość czcionki lub jako styl czcionki zostanie wybrana kursywa?

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

* Jeśli dokładnie pasująca czcionka jest niedostępna, przeglądarka wybierze najbliższe dopasowanie.
* Jeśli brak dopasowania względem stylu (np. brak zadeklarowanych wariantów z kursywą, jak w przykładzie powyżej), przeglądarka przeprowadzi syntezę własnego wariantu czcionki.

<img src="images/font-synthesis.png"  alt="Font synthesis" />

Note: Aby uzyskać jak najlepszy wygląd i zachować spójność wyświetlania, nie dopuszczaj do syntezy czcionek. Zamiast tego zmniejsz liczbę używanych wariantów czcionek i określ ich lokalizacje, dzięki czemu przeglądarka będzie mogła je pobrać, gdy okażą się potrzebne. Pamiętając o tym ostrzeżeniu, trzeba wiedzieć, że w niektórych przypadkach warianty uzyskane w drodze syntezy [mogą spełniać wymagania](https://www.igvita.com/2014/09/16/optimizing-webfont-selection-and-synthesis/) &ndash; jednak należy stosować je z rozwagą.

Pełna wersja czcionki sieci web ze wszystkimi wariantami stylistycznymi, których możemy nie potrzebować, i ze wszystkimi glifami, których zazwyczaj się nie wykorzystuje, może skutkować koniecznością pobrania wielu megabajtów danych. Rozwiązaniem tego problemu jest reguła @font-face kodu CSS, którą opracowano specjalnie w celu umożliwienia podziału rodziny czcionek na kolekcję zasobów: podzbiorów Unicode, osobnych wariantów stylistycznych i tak dalej.

Uwzględniając te deklaracje, przeglądarka wyznacza wymagane podzbiory i warianty, a następnie pobiera minimalny ich zestaw wymagany do zrenderowania tekstu. Takie zachowanie jest bardzo dla nas wygodne, ale jeśli nie zachowamy ostrożności, może wystąpić wąskie gardło w krytycznej ścieżce renderowania, co opóźni renderowanie tekstu &ndash; a tego chcielibyśmy uniknąć.

## Optymalizacja wczytywania i renderowania

### TL;DR {: .hide-from-toc }

* Żądania pobrania czcionek są odkładane do zakończenia tworzenia drzewa renderowania, co może prowadzić do opóźnienia renderowania tekstu
* Interfejs API Font Loading pozwala wdrożyć własne strategie wczytywania i renderowania czcionek zastępujące domyślne procedury leniwego wczytywania czcionek

Leniwe wczytywanie czcionek niesie ze sobą pewną ukrytą konsekwencję, która może prowadzić do opóźnienia renderowania tekstu: przeglądarka musi na podstawie drzew DOM i CSSOM [utworzyć drzewo renderowania](/web/fundamentals/performance/critical-rendering-path/render-tree-construction), zanim ustali, których zasobów będzie potrzebować do renderowania tekstu. Z tej przyczyny żądania pobierania czcionek są odkładane na moment po pobraniu innych krytycznych zasobów, a przeglądarka może zostać zablokowana i nie być zdolna do renderowania tekstu do chwili pobrania tych zasobów.

Given these declarations, the browser figures out the required subsets and variants and downloads the minimal set required to render the text, which is very convenient. However, if you're not careful, it can also create a performance bottleneck in the critical rendering path and delay text rendering.

### Czcionki sieci web i krytyczna ścieżka renderowania

Chęć jak najszybszego pierwszego odmalowania treści strony (co może nastąpić od razu po utworzeniu drzewa renderowania) i wysyłanie żądania o zasób czcionki dopiero po pewnym czasie prowadzą razem do `problemu pustego tekstu`. Objawia się on renderowaniem samego układu strony, ale nie tekstu. W rzeczywistości zachowanie zależy od rodzaju przeglądarki:

<img src="images/font-crp.png"  alt="Font critical rendering path" />

1. Przeglądarka wysyła żądanie dokumentu HTML
2. Przeglądarka rozpoczyna parsowanie odpowiedzi HTML i tworzy drzewo DOM
3. Przeglądarka rozpoznaje kod CSS i JS oraz inne zasoby, a następnie wysyła odpowiednie żądania
4. Przeglądarka tworzy drzewo CSSOM po odebraniu wszystkich zasobów CSS, a następnie łączy je z drzewem DOM w jedno drzewo renderowania 
    * Żądania czcionek są wysyłane po wykryciu w drzewie renderowania, których wariantów czcionek potrzeba do zrenderowania tekstu na stronie
5. Przeglądarka wyznacza układ strony i maluje treść na ekranie 
    * W przypadku niedostępności czcionki przeglądarka może nie zrenderować żadnych pikseli tekstu
    * Gdy czcionka stanie się dostępna, przeglądarka wykona malowanie pikseli tekstu

[Interfejs API Font Loading](http://dev.w3.org/csswg/css-font-loading/){: .external } to interfejs do wczytywania skryptów umożliwiających określanie i manipulację czcionkami w kodzie CSS, śledzenie postępu ich pobierania i zastępowanie domyślnego sposobu leniwego wczytywania. Na przykład, jeśli mamy pewność, że pewien wariant czcionki będzie wymagany, możemy określić ten wariant i polecić przeglądarce niezwłoczne pobranie zasobu czcionki:

Ponadto (dzięki metodzie [check()](http://dev.w3.org/csswg/css-font-loading/#font-face-set-check)) możemy zweryfikować stan czcionki i śledzić postęp jej pobierania, możemy również określić własną strategię renderowania tekstu na stronach:

### Optymalizacja renderowania czcionek z użyciem interfejsu API Font Loading

Najlepsze jest jednak to, że dla różnych typów treści na stronie można łączyć powyższe strategie &ndash; np. wstrzymanie renderowania tekstu w niektórych sekcjach, aż czcionka stanie się dostępna; użycie czcionki zastępczej i ponowne renderowanie po zakończeniu pobierania czcionek; określenie różnych limitów czasu oczekiwania i tak dalej.

Note: W przypadku niektórych przeglądarek interfejs API Font Loading jest wciąż w fazie [prac rozwojowych](http://caniuse.com/#feat=font-loading). Rozważ użycie biblioteki [FontLoader polyfill](https://github.com/bramstein/fontloader) lub [webfontloader](https://github.com/typekit/webfontloader), by zapewnić podobną funkcjonalność, chociaż za cenę dołączenia dodatkowego zasobu JavaScript.

Jeśli nie można skorzystać z interfejsu API Font Loading, `problem pustego tekstu` da się w prosty sposób wyeliminować: zamieścić zawartość czcionki w arkuszu stylów CSS.

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

Strategia zamieszczania w kodzie nie jest aż tak elastyczna i nie umożliwia określania niestandardowych czasów oczekiwania oraz strategii renderowania dla różnych typów treści, ale jest to proste i wszechstronne rozwiązanie, działające we wszystkich przeglądarkach. Najlepsze wyniki daje umieszczenie czcionek zamieszczonych w kodzie w osobnych arkuszach stylów i określenie ich długiego okresu ważności max-age &ndash; dzięki temu w przypadku aktualizacji arkusza CSS użytkownicy nie będą musieli ponownie pobierać czcionek.

Note: Z umiarem zamieszczaj czcionki w kodzie strony. Pamiętaj, że przyczyną leniwego wczytywania czcionek określonych dyrektywą @font-face jest chęć uniknięcia pobierania zbędnych wariantów i podzbiorów czcionek. Ponadto zwiększenie rozmiaru kodu CSS przez agresywne stosowanie zasady umieszczania czcionek w kodzie negatywnie wpływa na [krytyczną ścieżkę renderowania](/web/fundamentals/performance/critical-rendering-path/) &ndash; przeglądarka musi pobrać cały kod CSS przed utworzeniem modelu CSSOM, zbudowaniem drzewa renderowania i zrenderowaniem treści strony na ekranie.

### Optymalizacja renderowania czcionek przez zamieszczanie czcionek w kodzie

Zasoby czcionek są zazwyczaj zasobami statycznymi, niezbyt często aktualizowanymi. Dzięki temu zastosowanie długiego okresu upływu ważności max-age nie stanowi żadnego problemu. Pamiętaj, by określić zarówno [warunkowy nagłówek ETag](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), jak i [optymalną politykę Cache-Control](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) dla wszystkich zasobów czcionek.

#### Browser behaviors

Przechowywanie czcionek w magazynie lokalnym lub z użyciem innych mechanizmów jest niewskazane &ndash; każdy z nich powoduje różne specyficzne problemy z wydajnością. Pamięć podręczna HTTP przeglądarki w połączeniu z interfejsem API Font Loading lub biblioteką webfontloader to najlepszy i najbardziej wszechstronny sposób dostarczania zasobów czcionek do przeglądarki.

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

* Przeglądarka Safari wstrzymuje renderowanie tekstu, aż pobieranie czcionek się zakończy.
* Przeglądarki Chrome i Firefox wstrzymują renderowanie czcionek na maksymalnie 3 sekundy i po tym okresie stosują czcionki zastępcze, a po zakończeniu pobierania czcionek ponownie renderują tekst z użyciem pobranych czcionek.
* Przeglądarka IE niezwłocznie renderuje tekst z użyciem czcionek zastępczych, jeśli żądane czcionki nie są jeszcze dostępne, a następnie ponownie renderuje tekst z użyciem pobranych czcionek.

Wbrew powszechnemu mniemaniu użycie czcionek sieci web nie musi prowadzić do opóźnienia renderowania stron ani mieć negatywnego wpływu na wydajność. Zoptymalizowane użycie czcionek pozwala zwiększyć wygodę użytkowników: rozszerzyć rozpoznawalność marki, polepszyć czytelność, funkcjonalność i sprawność wyszukiwania, zapewniając przy tym możliwość zmiany skali i pracę przy wielu różnych rozdzielczościach i formatach ekranu. Nie obawiaj się używania czcionek sieci web!

#### The font display timeline

Pamiętaj jednak, że zbyt prosta implementacja może skutkować pobieraniem dużej ilości danych i niepotrzebnymi opóźnieniami. Właśnie w tej chwili należy przypomnieć sobie o naszym przewodniku optymalizacji i wspomóc przeglądarkę, optymalizując same zasoby czcionek i sposób ich pobierania na stronach.

1. **Audytuj i monitoruj użycie czcionek:** nie używaj zbyt wielu czcionek na stronach, a dla każdej czcionki ograniczaj liczbę obecnych wariantów. Pozwoli to skrócić czasy oczekiwania użytkowników na wyświetlenie treści.
2. **Używaj podzbiorów czcionek:** w przypadku wielu czcionek można wydzielić podzbiory, rozdzielić je na wiele zakresów Unicode i dostarczyć tylko glify konkretnie wymagane na danej stronie &ndash; pozwala to ograniczyć rozmiar pliku i zwiększyć prędkość pobierania zasobów. Przy określaniu podzbiorów pamiętaj o optymalizacji czcionek pod kątem ponownego użycia &ndash; np. niewskazane jest pobieranie różnych, ale zachodzących na siebie zbiorów znaków na różnych stronach. Dobrą praktyką jest wydzielanie podzbiorów w oparciu o transkrypcję &ndash; np. łacińską, cyrylicę i tak dalej.
3. **Przesyłaj czcionki w formatach dostosowanych do każdej z przeglądarek:** każda czcionka powinna być dostarczana w formatach WOFF2, WOFF, EOT i TTF. Upewnij się, że do formatów EOT i TTF stosowana będzie kompresja GZIP, ponieważ formaty te nie są domyślnie kompresowane.

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

### Optymalizacja ponownego użycia czcionek dzięki buforowaniu HTTP

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

* Arkusze stylów CSS z pasującymi zapytaniami o media są automatycznie pobierane przez przeglądarkę z wysokim priorytetem, ponieważ są potrzebne do utworzenia modelu CSSOM.
* Zamieszczanie danych czcionek w arkuszu stylów CSS wymusza na przeglądarce pobieranie czcionek z wysokim priorytetem i bez oczekiwania na drzewo renderowania &ndash; umożliwia to ręczne zastąpienie domyślnego zachowania polegającego na leniwym wczytywaniu.
* You can use the fallback font to unblock rendering and inject a new style that uses the desired font after the font is available.

Best of all, you can also mix and match the above strategies for different content on the page. For example, you can delay text rendering on some sections until the font is available, use a fallback font, and then re-render after the font download has finished, specify different timeouts, and so on.

Note: The Font Loading API is still [under development in some browsers](http://caniuse.com/#feat=font-loading). Consider using the [FontLoader polyfill](https://github.com/bramstein/fontloader) or the [webfontloader library](https://github.com/typekit/webfontloader) to deliver similar functionality, albeit with even more overhead from an additional JavaScript dependency.

### Proper caching is a must

Font resources are, typically, static resources that don't see frequent updates. As a result, they are ideally suited for a long max-age expiry - ensure that you specify both a [conditional ETag header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), and an [optimal Cache-Control policy](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) for all font resources.

If your web application uses a [service worker](/web/fundamentals/primers/service-workers/), serving font resources with a [cache-first strategy](/web/fundamentals/instant-and-offline/offline-cookbook/#cache-then-network) is appropriate for most use cases.

You should not store fonts using [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) or [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API); each of those has its own set of performance issues. The browser's HTTP cache provides the best and most robust mechanism to deliver font resources to the browser.

## Optymalizacja &ndash; lista czynności

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