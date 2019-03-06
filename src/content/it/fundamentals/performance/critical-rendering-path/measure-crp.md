project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Non puoi ottimizzare ciò che non puoi misurare. Fortunatamente, la Navigation Timing API ci offre tutti gli strumenti necessari per misurare ciascun passaggio del percorso di rendering critico.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Misurazione del percorso di rendering critico con Navigation Timing {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Non puoi ottimizzare ciò che non puoi misurare. Fortunatamente, Navigation Timing API ci offre tutti gli strumenti necessari per misurare ciascun passaggio del percorso di rendering critico.

* Navigation Timing offre informazioni cronologiche ad alta risoluzione per la misurazione di CRP.
* Il browser emette una serie di eventi non riproducibili che registrano varie fasi di CRP.

La base di ogni strategia prestazionale solida è una buona misurazione e strumentazione. A quanto pare è esattamente ciò che offre Navigation Timing API.

## Auditing a page with Lighthouse {: #lighthouse }

Lighthouse is a web app auditing tool that runs a series of tests against a given page, and then displays the page's results in a consolidated report. You can run Lighthouse as a Chrome Extension or NPM module, which is useful for integrating Lighthouse with continuous integration systems.

Ciascuna delle etichette del diagramma di cui sopra corrisponde a un'informazione cronologica ad alta risoluzione di cui il browser tiene traccia per ogni pagina che carica. In realtà, in questo caso specifico stiamo mostrando solo una parte di tutte le informazioni cronologiche &mdash; per adesso stiamo ignorando tutte le informazioni cronologiche relative alla rete, ma ci torneremo in una lezione futura.

Quindi cosa significano queste informazioni cronologiche?

![Lighthouse's CRP audits](images/lighthouse-crp.png)

See [Critical Request Chains](/web/tools/lighthouse/audits/critical-request-chains) for more information on this audit's results.

## Instrumenting your code with the Navigation Timing API {: #navigation-timing }

L'esempio di cui sopra potrebbe sembrare leggermente scoraggiante a prima vista, ma in realtà è davvero abbastanza semplice. Navigation Timing API acquisisce tutte le informazioni temporali pertinenti e il nostro codice attende semplicemente che l'evento `onload` sia attivato &mdash; ricorda che l'evento onload si attiva dopo domInteractive, domContentLoaded e domComplete &mdash e calcola la differenza tra le varie informazioni cronologiche.

<img src="images/dom-navtiming.png"  alt="Navigation Timing" />

Each of the labels in the above diagram corresponds to a high resolution timestamp that the browser tracks for each and every page it loads. In fact, in this specific case we're only showing a fraction of all the different timestamps &mdash; for now we're skipping all network related timestamps, but we'll come back to them in a future lesson.

So, what do these timestamps mean?

* **domLoading:** questa è l'informazione cronologica di inizio dell'intero processo, il browser sta per iniziare ad analizzare i primi byte ricevuti del documento HTML.
* **domInteractive:** segna il punto in cui il browser ha completato l'analisi di tutto l'HTML e la costruzione DOM è completa.
* `domContentLoaded`segna il punto in cui il DOM è pronto e non ci sono fogli di stile che bloccano l'esecuzione di JavaScript, quindi adesso possiamo (idealmente) costruire la struttura di rendering. 
    * Molti framework di JavaScript attendono questo evento prima di iniziare ad eseguire la propria logica. Per questo motivo, il browser acquisisce le informazioni temporali *EventStart* e *EventEnd* per consentirci di monitorare la durata dell'esecuzione.
* **domComplete:** come implica il nome, l'intera elaborazione è completa e tutte le risorse sulla pagina (immagini e così via) hanno terminato il download, ad esempio il rotante di caricamento ha smesso di girare.
* **loadEvent:** come passaggio finale in ogni caricamento della pagina, il browser lancia un evento di `onload` che può attivare un'ulteriore logica dell'applicazione.

The HTML specification dictates specific conditions for each and every event: when it should be fired, which conditions should be met, and so on. For our purposes, we'll focus on a few key milestones related to the critical rendering path:

* **domInteractive** segna quando DOM è pronto.
* `domContentLoaded` solitamente segna quando [sia DOM che CSSOM sono pronti](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/){: .external }. 
    * In assenza di JavaScript con blocco parser, *DOMContentLoaded* verrà attivato immediatamente dopo *domInteractive*.
* **domComplete** segna quando la pagina e tutte le relative sottorisorse sono pronte.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp.html" region_tag="full"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp.html){: target="_blank" .external }

The above example may seem a little daunting on first sight, but in reality it is actually pretty simple. The Navigation Timing API captures all the relevant timestamps and our code simply waits for the `onload` event to fire &mdash; recall that `onload` event fires after `domInteractive`, `domContentLoaded` and `domComplete` &mdash; and computes the difference between the various timestamps.

<img src="images/device-navtiming-small.png"  alt="NavTiming demo" />

All said and done, we now have some specific milestones to track and a simple function to output these measurements. Note that instead of printing these metrics on the page you can also modify the code to send these metrics to an analytics server ([Google Analytics does this automatically](https://support.google.com/analytics/answer/1205784)), which is a great way to keep tabs on performance of your pages and identify candidate pages that can benefit from some optimization work.

## What about DevTools? {: #devtools }

Although these docs sometimes use the Chrome DevTools Network panel to illustrate CRP concepts, DevTools is currently not well-suited for CRP measurements because it does not have a built-in mechanism for isolating critical resources. Run a [Lighthouse](#lighthouse) audit to help identify such resources.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}