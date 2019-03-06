project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: PageSpeed Insights-Regeln im Kontext: worauf bei der Optimierung des kritischen Rendering-Pfads zu achten ist und warum

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# PageSpeed-Regeln und Empfehlungen {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

PageSpeed Insights-Regeln im Kontext: worauf bei der Optimierung des kritischen Rendering-Pfads zu achten ist und warum

## JavaScript- und CSS-Code, die das Rendern blockieren, eliminieren

Im Sinne der kürzesten Zeit bis zum ersten Rendern sollte die Anzahl der kritischen Ressourcen auf der Seite minimiert und, soweit möglich, eliminiert, die Zahl der heruntergeladenen kritischen Bytes minimiert und die Länge des kritischen Pfads optimiert werden.

## JavaScript-Nutzung optimieren

JavaScript-Ressourcen blockieren standardmäßig den Parser, es sei denn, sie sind als as *async* gekennzeichnet oder werden über ein spezielles JavaScript-Snippet hinzugefügt. JavaScript, das den Parser blockiert, zwingt den Browser, auf das CSSOM zu warten und hält die Erstellung des DOM auf, was wiederum das erste Rendern erheblich verzögern kann.

### Prefer asynchronous JavaScript resources

Asynchrone Ressourcen heben die Blockierung des Dokumenten-Parsers auf und ermöglichen es dem Browser, die Blockierung im CSSOM vor der Ausführung des Skripts zu vermeiden. Wenn das Skript asynchron ausgeführt werden kann, bedeutet dies häufig, dass es für das erste Rendern nicht unbedingt benötigt wird. Erwägen Sie deshalb, asynchrone Skripts nach dem ersten Rendern zu laden.

### Avoid synchronous server calls

Alle Skripts, die nicht unbedingt für die Erstellung der sichtbaren Inhalte für das erste Rendern nötig sind, sollten zurückgestellt werden, um den Arbeitsaufwand des Browsers für die Darstellung der Seite zu minimieren.

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
    

Lang ausgeführter JavaScript-Code hindert den Browser an der Erstellung des DOM und des CSSOM und am Rendern der Seite. Deshalb sollte sämtliche Initialisierungslogik und Funktionalität, die nicht unbedingt für das erste Rendern benötigt wird, auf später verschoben werden. Wenn eine lange Initialisierungssequenz ausgeführt werden muss, erwägen Sie deren Aufteilung in mehrere Phasen, damit der Browser zwischenzeitlich andere Ereignisse verarbeiten kann.

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
    

CSS wird für die Erstellung der Rendering-Baumstruktur benötigt und JavaScript blockiert häufig die CSS-Nutzung während der ersten Erstellung der Seite. Sie sollten nicht absolut erforderlichen CSS-Code, z. B. Druck- und sonstige Medienanforderungen, unbedingt als nicht-kritisch kennzeichnen und sicherstellen, dass die Menge kritischer CSS-Elemente und die Zeit für deren Bereitstellung so gering wie möglich ausfällt.

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

Sämtliche CSS-Ressourcen sollten innerhalb des HTML-Dokuments so bald wie möglich spezifiziert werden, damit der Browser die `<link>`-Tags möglichst frühzeitig erkennen und die CSS-Anforderung ausgeben kann.

### Avoid long running JavaScript

Mithilfe der CSS-Import-Anweisung (@import) kann ein Stylesheet Regeln aus einer anderen Stylesheet-Datei importieren. Allerdings sollten diese Anweisungen vermieden werden, weil sie zusätzliche Paketumläufe im kritischen Pfad bedeuten: Die importierten CSS-Ressourcen werden erst erkannt, nachdem das CSS-Stylesheet mit der @import-Regel selbst empfangen und geparst wurde.

## CSS-Nutzung optimieren

Für optimale Leistung empfiehlt es sich, das kritische CSS direkt in das HTML-Dokument einzubetten. Damit vermeiden Sie zusätzliche Paketumläufe im kritischen Pfad und können bei korrekter Verwendung die Länge des kritischen Pfads auf einen Paketumlauf begrenzen, wobei nur der HTML-Code eine blockierende Ressource darstellt.

### Put CSS in the document head

Specify all CSS resources as early as possible within the HTML document so that the browser can discover the `<link>` tags and dispatch the request for the CSS as soon as possible.

### Avoid CSS imports

The CSS import (`@import`) directive enables one stylesheet to import rules from another stylesheet file. However, avoid these directives because they introduce additional roundtrips into the critical path: the imported CSS resources are discovered only after the CSS stylesheet with the `@import` rule itself is received and parsed.

### Inline render-blocking CSS

For best performance, you may want to consider inlining the critical CSS directly into the HTML document. This eliminates additional roundtrips in the critical path and if done correctly can deliver a "one roundtrip" critical path length where only the HTML is a blocking resource.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}