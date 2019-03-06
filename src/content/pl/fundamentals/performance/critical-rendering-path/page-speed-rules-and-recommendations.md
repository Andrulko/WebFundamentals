project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Reguły PageSpeed Insights w kontekście: na co zwracać uwagę podczas optymalizacji krytycznej ścieżki renderowania i dlaczego.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Reguły i zalecenia na temat PageSpeed Insights {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Reguły PageSpeed Insights w kontekście: na co zwracać uwagę podczas optymalizacji krytycznej ścieżki renderowania i dlaczego.

## Wyeliminuj kod JavaScript i CSS blokujący renderowanie

Aby maksymalnie skrócić czas wymagany do przeprowadzenia pierwszego renderowania strony, musimy zminimalizować liczbę zasobów krytycznych na stronie, liczbę pobieranych krytycznych bajtów i długość ścieżki krytycznej.

## Zoptymalizuj użycie kodu JavaScript

Zasoby JavaScript domyślnie blokują parser, chyba że zostaną opatrzone słowem kluczowym *async* lub dodane z wykorzystaniem specjalnego fragmentu kodu JavaScript. Kod JavaScript blokujący parser powoduje, że przeglądarka musi oczekiwać na utworzenie modelu CSSOM i wstrzymać tworzenie modelu DOM, co może się wiązać ze znacznym opóźnieniem pierwszego renderowania strony.

### Prefer asynchronous JavaScript resources

Zasoby asynchroniczne nie powodują blokowania parsera dokumentu i pozwalają uniknąć zablokowania przez model CSSOM przed wykonaniem skryptu. Zdarza się, że jeśli skrypt można zmienić na asynchroniczny, oznacza to również, że jest on zbędny przy pierwszym renderowaniu strony &ndash; rozważ wczytywanie skryptów asynchronicznych dopiero po pierwszym renderowaniu strony.

### Avoid synchronous server calls

Wykonywanie wszystkich skryptów niepotrzebnych do utworzenia treści widocznej przy pierwszym renderowaniu powinno zostać odłożone na później, by przeglądarka musiała wykonać jak najmniej czynności podczas renderowania strony.

    <script>
      function() {
        window.addEventListener('pagehide', logData, false);
        function logData() {
          navigator.sendBeacon(
            'https://putsreq.herokuapp.com/Dt7t2QzUkG18aDTMMcop',
            'Sent by a beacon!');
        }
      }();
    </script>
    

Długo wykonujące się skrypty JavaScript blokują przeglądarkę, uniemożliwiając jej tworzenie modelu DOM i CSSOM oraz renderowanie strony. Z tego względu wszelkie procedury inicjalizacyjne i funkcje zbędne przy pierwszym renderowaniu należy odłożyć na później. Jeśli wymagane jest uruchomienie długiej sekwencji poleceń inicjalizacyjnych, zastanów się, czy można ją podzielić na kilka etapów. Pozwoli to przeglądarce przetworzyć inne zdarzenia między tymi etapami.

    <script>
    fetch('./api/some.json')  
      .then(  
        function(response) {  
          if (response.status !== 200) {  
            console.log('Looks like there was a problem. Status Code: ' +  response.status);  
            return;  
          }
          // Examine the text in the response  
          response.json().then(function(data) {  
            console.log(data);  
          });  
        }  
      )  
      .catch(function(err) {  
        console.log('Fetch Error :-S', err);  
      });
    </script>
    

Do utworzenia drzewa renderowania potrzebny jest kod CSS, a skrypt JavaScript zazwyczaj blokuje kod CSS w trakcie pierwszego konstruowania strony. Upewnij się, że wszystkie nieistotne fragmenty kodu CSS (np. zapytania o wydruk i inne zapytania o media) są oznaczone jako niekrytyczne, a ilość krytycznego kodu CSS i czas wymagany do jego wykonania są możliwie najmniejsze.

    <script>
    fetch(url, {
      method: 'post',
      headers: {  
        "Content-type": "application/x-www-form-urlencoded; charset=UTF-8"  
      },  
      body: 'foo=bar&lorem=ipsum'  
    }).then(function() { // Additional code });
    </script>
    

### Defer parsing JavaScript

Wszystkie zasoby CSS należy zadeklarować w dokumencie HTML możliwie najwcześniej, by przeglądarka mogła jak najwcześniej wykryć tagi `<link>` i wysłać żądania odnośnie do zasobów CSS.

### Avoid long running JavaScript

Dyrektywa importu CSS (@import) umożliwia jednemu arkuszowi stylów import reguł z innego arkusza stylów. Jednak stosowania tych dyrektyw należy unikać, ponieważ wprowadzają one do ścieżki krytycznej dodatkowe cykle wymiany danych: importowane zasoby CSS są wyszukiwane dopiero po zakończeniu pobierania i parsowania arkusza stylów CSS zawierającego regułę @import.

## Zoptymalizuj użycie kodu CSS

Aby uzyskać najwyższą wydajność, zastanów się, czy nie umieścić krytycznej sekwencji znaczników CSS bezpośrednio w dokumencie HTML. Pozwala to wyeliminować ze ścieżki krytycznej dodatkowe cykle wymiany danych, a przy odpowiednim zaimplementowaniu może służyć do uzyskania ścieżki krytycznej o `jednym cyklu wymiany danych`, w którym zasobem blokującym są tylko znaczniki HTML.

### Put CSS in the document head

Specify all CSS resources as early as possible within the HTML document so that the browser can discover the `<link>` tags and dispatch the request for the CSS as soon as possible.

### Avoid CSS imports

The CSS import (`@import`) directive enables one stylesheet to import rules from another stylesheet file. However, avoid these directives because they introduce additional roundtrips into the critical path: the imported CSS resources are discovered only after the CSS stylesheet with the `@import` rule itself is received and parsed.

### Inline render-blocking CSS

For best performance, you may want to consider inlining the critical CSS directly into the HTML document. This eliminates additional roundtrips in the critical path and if done correctly can deliver a "one roundtrip" critical path length where only the HTML is a blocking resource.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}