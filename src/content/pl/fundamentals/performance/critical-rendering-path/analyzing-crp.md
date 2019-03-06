project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Wykrywanie i usuwanie wąskich gardeł ograniczających wydajność krytycznej ścieżki renderowania wymaga dobrej znajomości typowych problemów. Ten praktyczny przewodnik pomaga określić typowe schematy wydajności i zoptymalizować strony.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Analiza wydajności krytycznej ścieżki renderowania {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Wykrywanie i usuwanie wąskich gardeł ograniczających wydajność krytycznej ścieżki renderowania wymaga dobrej znajomości typowych problemów. Ten praktyczny przewodnik pomaga określić typowe schematy wydajności i zoptymalizować strony.

Celem optymalizacji krytycznej ścieżki renderowania jest umożliwienie przeglądarce jak najszybszego wyświetlenia strony &ndash; sprawne działanie oznacza większą liczbę zaangażowanych użytkowników, odwiedzonych stron i [uzyskanych konwersji](http://www.google.com/think/multiscreen/success.html){: .external }. Dlatego chcemy zoptymalizować zakres i kolejność wczytywania zasobów, by użytkownik jak najkrótszy czas spędzał na wpatrywaniu się w pusty ekran.

Aby zilustrować ten proces, zaczniemy od najprostszego możliwego przypadku i będziemy stopniowo rozbudowywać stronę, dodając kolejne zasoby, style i procedury aplikacji. W ten sposób określimy miejsca, w których coś może pójść nie tak, i każde z nich zoptymalizujemy.

Jeszcze jedna rzecz, zanim rozpoczniemy. Dotychczas koncentrowaliśmy się tylko na tym, co dzieje się w przeglądarce, gdy zasób (plik CSS, JS lub HTML) jest dostępny do przetworzenia. Ignorowaliśmy czas potrzebny na pobranie go z pamięci podręcznej lub sieci. Optymalizacją sieciowych aspektów aplikacji zajmiemy się szczegółowo w następnym artykule, na razie jednak (by wszystko było bardziej realistyczne) przyjmiemy te założenia:

* Sieciowy cykl wymiany danych z serwerem zajmuje 100&nbsp;ms (czas przesyłania).
* Czas odpowiedzi serwera to 100&nbsp;ms w przypadku dokumentu HTML i 10&nbsp;ms przy wszystkich pozostałych plikach.

## Strona `Witaj Świecie`

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Zaczynamy od podstawowych znaczników HTML i jednego obrazu, bez CSS czy JavaScriptu. To najprostsza wersja. Otwieramy oś czasu sieci w Narzędziach Chrome dla programistów i sprawdzamy uzyskany wykres zasobów:

We'll start with basic HTML markup and a single image; no CSS or JavaScript. Let's open up our Network timeline in Chrome DevTools and inspect the resulting resource waterfall:

<img src="images/waterfall-dom.png" alt="CRP" />

Po udostępnieniu treści HTML przeglądarka musi przeanalizować dane, przekonwertować je na tokeny i utworzyć drzewo DOM. Narzędzia dla programistów podają u dołu czas zdarzenia DOMContentLoaded (216&nbsp;ms), na wykresie oznaczony niebieską pionową linią. Odstęp między zakończeniem pobierania kodu HTML a niebieską pionową linią (DOMContentLoaded) to czas tworzenia drzewa DOM przez przeglądarkę &ndash; w tym przypadku tylko kilka milisekund.

Na koniec zwróć uwagę na coś ciekawego: plik `awesome photo` nie zablokował zdarzenia domContentLoaded. Okazuje się, że możemy utworzyć drzewo renderowania, a nawet wyświetlić stronę bez czekania na każdy umieszczony na niej zasób. **Nie wszystkie zasoby są wymagane do szybkiego wstępnego pokazania strony**. W rzeczywistości, gdy mówimy o krytycznej ścieżce renderowania, mamy zwykle na myśli kod HTML, CSS i JavaScript. Obrazy nie blokują początkowego renderowania strony. Oczywiście musimy się postarać, by one także wyświetlały się jak najszybciej.<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

Zdarzenie `load` (nazywane też `onload`) zostaje zablokowane w przypadku obrazu &ndash; Narzędzia dla programistów podają, że następuje po 335&nbsp;ms. Wskazuje ono moment, w którym **wszystkie zasoby** wymagane przez stronę zostały już pobrane i przetworzone, a ikona wczytywania przestaje się obracać w przeglądarce. Na wykresie jest oznaczone czerwoną pionową linią.

Nasza strona `Witaj Świecie` z zewnątrz może wydawać się prosta, ale w środku sporo się dzieje, by mogła działać. W praktyce potrzebujemy czegoś więcej niż tylko kodu HTML &ndash; zwykle przydaje się arkusz stylów CSS i co najmniej jeden skrypt, który zwiększa interaktywność strony. Dodajemy oba te elementy i oceniamy wyniki:

_Przed dodaniem JavaScriptem i CSS: _

## Dodawanie JavaScriptu i CSS do strony

Our "Hello World experience" page seems simple but a lot goes on under the hood. In practice we'll need more than just the HTML: chances are, we'll have a CSS stylesheet and one or more scripts to add some interactivity to our page. Let's add both to the mix and see what happens:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_timing.html){: target="_blank" .external }

*Before adding JavaScript and CSS:*

<img src="images/waterfall-dom.png" alt="DOM CRP" />

*With JavaScript and CSS:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

Adding external CSS and JavaScript files adds two extra requests to our waterfall, all of which the browser dispatches at about the same time. However, **note that there is now a much smaller timing difference between the `domContentLoaded` and `onload` events.**

What happened?

* Inaczej niż w przykładzie z samym kodem HTML, obecnie musimy także pobrać i przeanalizować plik CSS, by utworzyć model CSSOM. Zarówno model DOM, jak i CSSOM są potrzebne do utworzenia drzewa renderowania.
* Strona dodatkowo zawiera teraz plik JavaScript, który wstrzymuje parser, więc zdarzenie domContentLoaded jest blokowane aż do pobrania i przeanalizowania pliku CSS. Kod JavaScript może odczytywać model CSSOM, dlatego musimy wstrzymać jego wykonanie i poczekać na CSS.

**What if we replace our external script with an inline script?** Even if the script is inlined directly into the page, the browser can't execute it until the CSSOM is constructed. In short, inlined JavaScript is also parser blocking.

Wysyłamy jedno żądanie mniej, ale czasy zdarzeń onload i domContentLoaded są praktycznie takie same. Dlaczego? Wiemy, że niezależnie od tego, czy JavaScript jest wbudowany czy zewnętrzny, gdy tylko przeglądarka napotka tag script, wstrzymuje działanie i czeka na utworzenie modelu CSSOM. Oprócz tego w pierwszym przykładzie przeglądarka wczytywała CSS oraz JavaScript równolegle i kończyła mniej więcej w tym samym czasie. W efekcie wbudowanie kodu JavaScript w tej konkretnej sytuacji wiele nam nie daje. Czy to koniec i nic więcej nie możemy zrobić, by przyspieszyć renderowanie strony? Mamy jeszcze kilka różnych strategii.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Inlined JavaScript:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, and inlined JS" />

We are making one less request, but both our `onload` and `domContentLoaded` times are effectively the same. Why? Well, we know that it doesn't matter if the JavaScript is inlined or external, because as soon as the browser hits the script tag it blocks and waits until the CSSOM is constructed. Further, in our first example, the browser downloads both CSS and JavaScript in parallel and they finish downloading at about the same time. In this instance, inlining the JavaScript code doesn't help us much. But there are several strategies that can make our page render faster.

Znacznie lepiej. Zdarzenie domContentLoaded następuje krótko po przeanalizowaniu znaczników HTML &ndash; przeglądarka wie, że nie musi przerywać działania w oczekiwaniu na wykonanie kodu JavaScript. Nie ma żadnych innych skryptów blokujących parser, więc równolegle można też tworzyć model CSSOM.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

Much better! The `domContentLoaded` event fires shortly after the HTML is parsed; the browser knows not to block on JavaScript and since there are no other parser blocking scripts the CSSOM construction can also proceed in parallel.

**Czas między T<sub>0</sub> i T<sub>1</sub> obejmuje działanie sieci i serwera.** W najlepszym przypadku (gdy plik HTML jest mały), potrzebujemy tylko jednego cyklu wymiany danych przez sieć, by pobrać cały dokument. Ze względu na sposób działania protokołów transportowych TCP większe pliki mogą wymagać wielu cykli wymian danych. Wrócimy do tego tematu w jednym z kolejnych artykułów. **Możemy więc stwierdzić, że strona ma krytyczną ścieżkę renderowania z minimum jednym cyklem wymiany danych.**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Teraz przyjrzymy się tej samej stronie, ale z zewnętrznym plikiem CSS:

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="Krytyczna ścieżka renderowania: DOM + CSSOM" />

Ponownie wykonujemy jeden cykl wymiany danych przez sieć, by pobrać dokument HTML. Następnie pobrany kod informuje nas, że potrzebujemy też pliku CSS. To oznacza, że przed wyrenderowaniem strony na ekranie przeglądarka musi jeszcze raz skontaktować się z serwerem i pobrać arkusz CSS. **W wyniku tego strona przed wyświetleniem przeprowadza minimum dwa cykle wymiany danych.** Także w tym przypadku plik CSS może wymagać wielu takich cykli, dlatego wspominamy o `minimum`.

Zdefiniujemy pojęcia, które pozwolą nam opisać krytyczną ścieżkę renderowania:

Porównamy to z charakterystyką ścieżki krytycznej przykładowej strony z HTML i CSS:

## Schematy wydajności

The simplest possible page consists of just the HTML markup; no CSS, no JavaScript, or other types of resources. To render this page the browser has to initiate the request, wait for the HTML document to arrive, parse it, build the DOM, and then finally render it on the screen:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Do utworzenia drzewa renderowania potrzebujemy zarówno pliku HTML, jak i CSS, więc oba to zasoby krytyczne. Arkusz CSS jest pobierany dopiero po tym, gdy przeglądarka odczyta dokument HTML, więc długość ścieżki krytycznej to minimum dwa cykle wymiany danych. Zasoby dają łącznie 9&nbsp;KB danych krytycznych.

<img src="images/analysis-dom.png" alt="Hello world CRP" />

Dodaliśmy plik app.js, który jest zewnętrznym zasobem JavaScript na stronie. Wiemy już, że blokuje on parser (czyli to zasób krytyczny). Co gorsza, przed wykonaniem kodu JavaScript przeglądarka musi wstrzymać działanie i poczekać na model CSSOM. JavaScript może go odczytywać, więc przeglądarka najpierw pobiera plik `style.css` i tworzy CSSOM.

Now, let's consider the same page but with an external CSS file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Na "wykresie sieciowym" strony możemy zauważyć, że żądania udostępnienia plików CSS i JavaScript są wysyłane mniej więcej w tym samym czasie. Przeglądarka pobiera plik HTML, wykrywa oba zasoby i wysyła żądania. W efekcie otrzymujemy taką charakterystykę ścieżki krytycznej strony:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

Po rozmowie z programistami witryny stwierdzamy, że plik JavaScript dodany do strony nie wymaga wstrzymywania pracy przeglądarki. Zawarty w nim kod do analityki itp. nie musi blokować renderowania strony. Dzięki temu możemy dodać atrybut `async` do tagu script, by odblokować parser:

Let's define the vocabulary we use to describe the critical rendering path:

* **Zasób krytyczny:** zasób, który może zablokować początkowe renderowanie strony.
* **Długość ścieżki krytycznej:** liczba cykli wymiany danych lub łączny czas potrzebny do tego, by pobrać wszystkie zasoby krytyczne.
* **Dane krytyczne:** łączna liczba bajtów wymaganych do pierwszego wyrenderowania strony. To suma rozmiarów wszystkich przesyłanych plików zasobów krytycznych. W pierwszym przykładzie z pojedynczym plikiem HTML strona zawiera jeden zasób krytyczny (dokument HTML), długość ścieżki krytycznej to jeden cykl wymiany danych (jeśli plik jest mały), a całkowita ilość danych krytycznych to rozmiar przesyłanego dokumentu HTML.

Oznaczenie skryptu jako asynchronicznego ma kilka zalet:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** zasoby krytyczne
* **2** lub więcej cykli wymiany danych przy minimalnej długości ścieżki krytycznej
* **9**&nbsp;KB danych krytycznych

Na koniec przypuśćmy, że arkusz stylów CSS jest potrzebny tylko do drukowania. Jak zmieni się ścieżka?

Now let's add an extra JavaScript file into the mix.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Zasób style.css jest używany tylko do drukowania, więc przeglądarka nie musi z jego powodu blokować renderowania strony. Dzięki temu od razu po utworzeniu modelu DOM ma dość informacji, by wyświetlić stronę. W efekcie strona zawiera tylko jeden zasób krytyczny (dokument HTML), a minimalna długość krytycznej ścieżki renderowania to jeden cykl wymiany danych.

We added `app.js`, which is both an external JavaScript asset on the page and a parser blocking (that is, critical) resource. Worse, in order to execute the JavaScript file we have to block and wait for CSSOM; recall that JavaScript can query the CSSOM and hence the browser pauses until `style.css` is downloaded and CSSOM is constructed.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

That said, in practice if we look at this page's "network waterfall," you'll see that both the CSS and JavaScript requests are initiated at about the same time; the browser gets the HTML, discovers both resources, and initiates both requests. As a result, the above page has the following critical path characteristics:

* **3** zasoby krytyczne
* **2** lub więcej cykli wymiany danych przy minimalnej długości ścieżki krytycznej
* **11**&nbsp;KB danych krytycznych

We now have three critical resources that add up to 11KB of critical bytes, but our critical path length is still two roundtrips because we can transfer the CSS and JavaScript in parallel. **Figuring out the characteristics of your critical rendering path means being able to identify the critical resources and also understanding how the browser will schedule their fetches.** Let's continue with our example.

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

An asynchronous script has several advantages:

* Skrypt nie blokuje już parsera i nie należy do krytycznej ścieżki renderowania.
* Nie ma innych skryptów krytycznych, więc CSS także nie musi blokować wywołania zdarzenia domContentLoaded.
* Im szybciej nastąpi zdarzenie domContentLoaded, tym wcześniej zaczną działać inne procedury aplikacji.

As a result, our optimized page is now back to two critical resources (HTML and CSS), with a minimum critical path length of two roundtrips, and a total of 9KB of critical bytes.

Finally, if the CSS stylesheet were only needed for print, how would that look?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

Because the style.css resource is only used for print, the browser doesn't need to block on it to render the page. Hence, as soon as DOM construction is complete, the browser has enough information to render the page. As a result, this page has only a single critical resource (the HTML document), and the minimum critical rendering path length is one roundtrip.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}