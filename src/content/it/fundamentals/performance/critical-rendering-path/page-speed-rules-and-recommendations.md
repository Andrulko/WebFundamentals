project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Regole di PageSpeed Insights nel contesto: a cosa prestare attenzione quando si ottimizza il percorso di rendering critico e perché.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Regole e consigli per PageSpeed {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Regole di PageSpeed Insights nel contesto: a cosa prestare attenzione quando si ottimizza il percorso di rendering critico e perché.

## Eliminazione del JavaScript con blocco del rendering e CSS

Per fornire il tempo più rapido per il primo rendering, dovrai ridurre e (ove possibile) eliminare il numero di risorse critiche sulla pagina, diminuire il numero di byte critici scaricati e ottimizzare la lunghezza del percorso critico.

## Ottimizzazione utilizzo JavaScript

Le risorse JavaScript bloccano il parser per impostazione predefinita, a meno che non siano indicate come *async* o aggiunte tramite speciale snippet JavaScript. Il JavaScript con blocco parser forza il browser ad attendere CSSOM e a sospendere la costruzione di DOM, che a sua volta può ridurre in modo significativo il tempo per il primo rendering.

### Prefer asynchronous JavaScript resources

Le risorse async sbloccano il parser del documento e consentono al browser di evitare il blocco di CSSOM prima dell'esecuzione dello script. Spesso, se lo script può essere reso async significa anche che non è essenziale per il primo rendering; considera dunque il caricamento degli script async dopo il rendering iniziale.

### Avoid synchronous server calls

Qualsiasi script non essenziale che non sia critico per la costruzione del contenuto visibile per il rendering iniziale deve essere rinviato per ridurre la quantità di lavoro che il browser deve effettuare per eseguire il rendering della pagina.

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
    

Il JavaScript a esecuzione prolungata impedisce al browser di costruire DOM, CSSOM e di eseguire il rendering della pagina. Di conseguenza, qualsiasi logica di inizializzazione e funzionalità che non è essenziale al primo rendering dovrà essere reinviata a un secondo momento. Se è necessario eseguire una lunga sequenza di inizializzazione, considera la suddivisione in numerose fasi così da consentire al browser di elaborare altri eventi intermedi.

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
    

CSS è necessario per la costruzione della struttura di rendering e spesso JavaScript bloccherà CSS durante la costruzione iniziale della pagina. Devi assicurarti che qualsiasi CSS non essenziale sia indicato come non critico (ad esempio query di stampa e di altri supporti) e che la quantità di CSS critico e il tempo per fornirlo siano il minimo possibile.

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

Tutte le risorse CSS devono essere specificate il prima possibile all'interno del documento HTML, così che il browser possa rilevare i tag `<link>` e inviare la richiesta per CSS il prima possibile.

### Avoid long running JavaScript

La direttiva dell'importazione CSS (@import) consente a un foglio di stile di importare le regole da un altro foglio di stile. Tuttavia, queste direttive devono essere evitate perché inseriscono altri roundtrip nel percorso critico: le risorse CSS importate sono individuate solo dopo che è stato ricevuto e analizzato il foglio di stile CSS con la regola @import.

## Ottimizzazione utilizzo CSS

Per prestazioni migliori, potresti voler considerare l'incorporamento del CSS critico direttamente nel documento HTML. Questo elimina roundtrip aggiuntivi nel percorso critico e, se eseguiti correttamente, possono essere utilizzati per fornire una lunghezza del percorso critico a 'unico roundtrip' dove solo l'HTML è una risorsa che blocca.

### Put CSS in the document head

Specify all CSS resources as early as possible within the HTML document so that the browser can discover the `<link>` tags and dispatch the request for the CSS as soon as possible.

### Avoid CSS imports

The CSS import (`@import`) directive enables one stylesheet to import rules from another stylesheet file. However, avoid these directives because they introduce additional roundtrips into the critical path: the imported CSS resources are discovered only after the CSS stylesheet with the `@import` rule itself is received and parsed.

### Inline render-blocking CSS

For best performance, you may want to consider inlining the critical CSS directly into the HTML document. This eliminates additional roundtrips in the critical path and if done correctly can deliver a "one roundtrip" critical path length where only the HTML is a blocking resource.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}