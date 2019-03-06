project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Regels van PageSpeed Insights in context: waar moet u op letten bij het optimaliseren van het kritieke weergavepad en waarom.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Regels en aanbevelingen voor PageSpeed {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Regels van PageSpeed Insights in context: waar moet u op letten bij het optimaliseren van het kritieke weergavepad en waarom.

## Weergaveblokkerend JavaScript en CSS elimineren

Voor de snelste eerste weergave moet u het aantal kritieke bronnen op een pagina minimaliseren en (indien mogelijk) elimineren, het aantal gedownloade bytes minimaliseren en de kritieke padlengte optimaliseren.

## Het gebruik van JavaScript optimaliseren

JavaScript-bronnen zijn standaard parserblokkerend, tenzij deze zijn gemarkeerd als *async* of zijn toegevoegd via een speciale JavaScript-snippet. Parserblokkerend JavaScript dwingt de browser te wachten op het CSSOM en pauzeert de DOM-opbouw. Dit kan zorgen voor een aanzienlijke vertraging in de tijd voor de eerste weergave.

### Prefer asynchronous JavaScript resources

Asynchrone bronnen deblokkeren de documentparser en zorgen dat de browser niet blokkeert bij CSSOM voordat het script wordt uitgevoerd. Als een script asynchroon kan worden gemaakt, betekent dit vaak ook dat het niet essentieel is voor de eerste weergave. Overweeg daarom om asynchrone scripts te laden na de initiële weergave.

### Avoid synchronous server calls

Alle niet-essentiële scripts, die niet kritiek zijn voor het opbouwen van zichtbare inhoud voor de initiële weergave moeten worden uitgesteld om de hoeveelheid werk te minimaliseren die de browser moet uitvoeren om de pagina weer te geven.

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
    

Lange JavaScripts blokkeren de browser bij het opbouwen van het DOM en CSSOM, en bij de weergave van de pagina. Daarom moet alle initialisatielogistiek en -functionaliteit die niet essentieel is voor de eerste weergave worden uitgesteld naar een later moment. Als een lange initialisatiereeks moet worden uitgevoerd, kunt u overwegen deze in verschillende fasen op te splitsen, zodat de browser de mogelijkheid krijgt om andere gebeurtenissen tussendoor te verwerken.

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
    

Het CSS is nodig om de weergaveboomstructuur op te bouwen en JavaScript blokkeert vaak op CSS tijdens de initiële opbouw van de pagina. U moet ervoor zorgen dat alle niet-essentiële CSS is gemarkeerd als niet-essentieel (bijvoorbeeld als afdrukken of andere mediaquery`s) en dat de hoeveelheid kritieke CSS en de tijd die nodig is om deze te leveren, zo klein mogelijk is.

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

Alle CSS-bonnen moeten zo snel mogelijk in het HTML-document worden aangegeven, zodat de browser de tags `<link>` zo snel mogelijk kan ontdekken en gelijk de aanvraag voor de CSS kan uitsturen.

### Avoid long running JavaScript

De opdracht voor CSS-import (@import) zorgt ervoor dat een stijlblad regels van een ander stijlbladbestand kan importeren. Maar dit soort opdrachten moet worden vermeden omdat hierdoor extra roundtrips aan het kritieke pad worden toegevoegd: de geïmporteerde CSS-bronnen worden pas ontdekt nadat het CSS-stijlblad met de @import-regel zelf is ontvangen en geparseerd.

## Het CSS-gebruik optimaliseren

Voor de beste prestatie kunt u overwegen om kritieke CSS direct inline in het HTML-document te plaatsen. Hierdoor worden extra roundtrips in het kritieke pad geëlimineerd. Als dit op een goede manier wordt gedaan, kan dit worden gebruikt om een kritieke padlengte van één roundtrip te leveren waarbij alleen de HTML een blokkerende bron is.

### Put CSS in the document head

Specify all CSS resources as early as possible within the HTML document so that the browser can discover the `<link>` tags and dispatch the request for the CSS as soon as possible.

### Avoid CSS imports

The CSS import (`@import`) directive enables one stylesheet to import rules from another stylesheet file. However, avoid these directives because they introduce additional roundtrips into the critical path: the imported CSS resources are discovered only after the CSS stylesheet with the `@import` rule itself is received and parsed.

### Inline render-blocking CSS

For best performance, you may want to consider inlining the critical CSS directly into the HTML document. This eliminates additional roundtrips in the critical path and if done correctly can deliver a "one roundtrip" critical path length where only the HTML is a blocking resource.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}