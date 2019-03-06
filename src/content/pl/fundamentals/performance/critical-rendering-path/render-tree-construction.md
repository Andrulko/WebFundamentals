project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Drzewo renderowania powstaje z połączenia drzew CSSOM i DOM. Wykorzystuje się je do wyznaczania rozmieszczenia każdego widocznego elementu na stronie, na jego podstawie procedura malowania renderuje piksele na ekranie. Aby osiągnąć najwyższą wydajność renderowania, ważne jest wykonanie optymalizacji każdego z powyższych etapów.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Tworzenie drzewa renderowania, układ strony i malowanie {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Drzewo renderowania powstaje z połączenia drzew CSSOM i DOM. Wykorzystuje się je do wyznaczania rozmieszczenia każdego widocznego elementu na stronie, na jego podstawie procedura malowania renderuje piksele na ekranie. Aby osiągnąć najwyższą wydajność renderowania, ważne jest wykonanie optymalizacji każdego z powyższych etapów.

W poprzedniej sekcji zawierającej opis tworzenia modelu obiektowego tworzyliśmy drzewa DOM i CSSOM w oparciu o dane wejściowe HTML i CSS. Jednak są one niezależnymi od siebie obiektami, charakteryzującymi różne aspekty dokumentu: jeden opisuje treść, drugi reguły stylu mające zastosowanie do dokumentu. W jaki sposób można je połączyć tak, by przeglądarka zrenderowała piksele na ekranie?

### TL;DR {: .hide-from-toc }

* Drzewo renderowania powstaje z połączenia drzew CSSOM i DOM.
* Drzewo renderowania zawiera tylko węzły wymagane do renderowania strony.
* W trakcie wyznaczania rozmieszczenia na stronie obliczane jest dokładne położenie i rozmiar każdego obiektu.
* Malowanie to ostatni krok, w którym na podstawie finalnego drzewa renderowania przeprowadza się renderowanie pikseli na ekranie.

Pierwszym krokiem jest połączenie drzew DOM i CSSOM w jedno `drzewo renderowania` zawierające opis całej zawartości DOM widocznej na stronie. Do każdego węzła dołączane są informacje o stylach z drzewa CSSOM.

<img src="images/render-tree-construction.png" alt="Drzewo renderowania powstaje z połączenia drzew CSSOM i DOM" />

Aby utworzyć drzewo renderowania, przeglądarka wykonuje z grubsza następujące czynności:

1. Starting at the root of the DOM tree, traverse each visible node.
    
    * Niektóre niewidoczne węzły (np. tagi skryptów, metatagi) są pomijane, ponieważ nie mają wpływu na renderowany obraz wyjściowy.
    * Niektóre węzły zostały ukryte przez kod CSS i są również wykluczane z drzewa renderowania &ndash; np. węzeł span w powyższym przykładzie jest nieobecny w drzewie renderowania, ponieważ jawnie określiliśmy jego właściwość `display: none`.

2. For each visible node, find the appropriate matching CSSOM rules and apply them.

3. Wygenerowanie widocznych węzłów z treścią, przy uwzględnieniu wyznaczonych dla nich stylów.

Note: Na marginesie: pamiętaj, że atrybut `visibility: hidden` różni się od atrybutu `display: none`. Pierwszy z nich powoduje, że element jest niewidoczny, ale nadal zajmuje miejsce w układzie strony (tzn. jest renderowany jako puste pole), a drugi `display: none` powoduje całkowite usunięcie elementu z drzewa renderowania, przez co element staje się niewidoczny i przestaje należeć do układu strony.

Końcowym efektem jest obraz zrenderowany na ekranie z odwzorowaniem zarówno całej widocznej treści, jak i jej stylów &ndash; zbliżamy się do celu. **Po przygotowaniu drzewa renderowania możemy przejść do etapu wyznaczania `układu strony`.**

Do tej chwili określaliśmy, które węzły powinny być widoczne, oraz ich style. Nie wyznaczaliśmy ich dokładnego położenia w [widocznym obszarze](/web/fundamentals/design-and-ux/responsive/#set-the-viewport) urządzenia &ndash; to etap znajdowania `układu strony`, określanego również pojęciem `rozmieszczenia elementów`.

Przeglądarka rozpoczyna wyznaczanie dokładnego rozmiaru i położenia każdego obiektu od korzenia drzewa renderowania i przeszukuje całe drzewo, obliczając geometrię każdego obiektu na stronie. Oto prosty przykład:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/nested.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Sekcja body powyższej strony zawiera dwa zagnieżdżone elementy div: pierwszy (nadrzędny) element div określa wartość rozmiaru wyświetlania jako 50% szerokości widocznego obszaru, drugi (zawarty w nadrzędnym elemencie div) określa szerokość jako 50% elementu nadrzędnego &ndash; tzn. 25% szerokości widocznego obszaru.

The body of the above page contains two nested div's: the first (parent) div sets the display size of the node to 50% of the viewport width, and the second div\---contained by the parent\---sets its width to be 50% of its parent; that is, 25% of the viewport width.

<img src="images/layout-viewport.png" alt="Calculating layout information" />

Po określeniu widoczności węzłów oraz wyznaczeniu ich stylów i geometrii dane te są przekazywane do ostatniego etapu: przekształcenia każdego węzła w drzewie renderowania na rzeczywiste piksele na ekranie &ndash; ten krok często określa się mianem `malowania` lub `rasteryzacji`.

Czy rozumiesz wszystkie etapy? Każdy z tych etapów wymaga od przeglądarki ogromnej liczby czynności, dlatego mogą one zająć sporo czasu. Na szczęście Narzędzia Chrome dla programistów pozwalają uzyskać wgląd we wszystkie trzy opisane wyżej etapy. Rozpatrzmy etap wyznaczania układu strony z pierwotnym przykładem `Witaj Świecie`:

This can take some time because the browser has to do quite a bit of work. However, Chrome DevTools can provide some insight into all three of the stages described above. Let's examine the layout stage for our original "hello world" example:

<img src="images/layout-timeline.png" alt="Measuring layout in DevTools" />

* Tworzenie drzewa renderowania oraz wyznaczanie położenia i rozmiaru są wykonywane w zdarzeniu `Layout` wyświetlanym na osi czasu.
* Po zakończeniu wyznaczania układu strony przeglądarka wywołuje zdarzenia `Paint Setup` i `Paint`, podczas których na podstawie drzewa renderowania wyznaczane są rzeczywiste piksele na ekranie.

Po wykonaniu tych wszystkich czynności nasza strona wyświetla się w końcu poprawnie w widocznym obszarze &ndash; hura!

The page is finally visible in the viewport:

<img src="images/device-dom-small.png" alt="Rendered Hello World page" />

Nasza demonstracyjna strona może wyglądać na prostą, ale wymaga wykonania wielu czynności. A co się stanie po wprowadzeniu modyfikacji do modelu DOM lub CSSOM? Cały proces trzeba będzie powtórzyć, by określić, które piksele powinny zostać zrenderowane ponownie na ekranie.

1. Przetworzenie znaczników HTML i utworzenie drzewa DOM.
2. Przetworzenie znaczników CSS i utworzenie drzewa CSSOM.
3. Połączenie drzew DOM i CSSOM w jedno drzewo renderowania.
4. Wyznaczenie układu strony na podstawie drzewa renderowania i określenie geometrii każdego węzła.
5. Odmalowanie poszczególnych węzłów na ekranie.

**Optymalizacja krytycznej ścieżki renderowania polega na minimalizacji łącznego czasu trwania kroków od 1 do 5 powyższej sekwencji.** Takie postępowanie ma na celu uzyskanie jak najszybszego renderowania treści na ekranie, jak również skrócenie odstępów między kolejnymi aktualizacjami ekranu po pierwszym renderowaniu, co oznacza uzyskanie wyższych częstotliwości odświeżania treści interaktywnej.

***Optimizing the critical rendering path* is the process of minimizing the total amount of time spent performing steps 1 through 5 in the above sequence.** Doing so renders content to the screen as quickly as possible and also reduces the amount of time between screen updates after the initial render; that is, achieve higher refresh rates for interactive content.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}