project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: L'identificazione e la risoluzione dei colli di bottiglia della performance del percorso di rendering critico richiede una buona conoscenza delle insidie comuni. Facciamo un tour pratico ed estraiamo i pattern di performance comuni che faciliteranno l'ottimizzazione delle tue pagine.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Analisi della performance del percorso di rendering critico {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

L'identificazione e la risoluzione dei colli di bottiglia della performance del percorso di rendering critico richiede una buona conoscenza delle insidie comuni. Facciamo un tour pratico ed estraiamo i pattern di performance comuni che faciliteranno l'ottimizzazione delle tue pagine.

L'obiettivo di ottimizzare il percorso di rendering critico è quello di consentire al browser di disegnare la pagina il più rapidamente possibile: pagine più veloci offrono un impegno superiore, un maggior numero di pagine visualizzate e [conversione migliore](http://www.google.com/think/multiscreen/success.html){: .external }. Di conseguenza, vogliamo ridurre il tempo che il visitatore deve trascorrere fissando una pagina vuota attraverso l'ottimizzazione delle risorse che sono caricate e nel relativo ordine.

Per facilitare l'illustrazione di questo processo, iniziamo con il caso più semplice possibile e costruiamo in modo incrementale la nostra pagina affinché includa risorse aggiuntive, stili e logica di applicazione. Durante questo processo, vedremo in che modo le cose possono andare storte e come poter ottimizzare ciascuno di questi casi.

Infine, un'ultima cosa prima di iniziare... finora ci siamo concentrati esclusivamente su ciò che accade nel browser una volta che la risorsa (file CSS, JS, o HTML) è disponibile per l'elaborazione e abbiamo ignorato il tempo necessario al recupero dalla cache o dalla rete. Nella prossima lezione approfondiremo come ottimizzare gli aspetti di networking della nostra applicazione con maggior dettaglio, ma nel frattempo (per rendere le cose più realistiche) daremo per scontato quanto segue:

* Il roundtrip della rete (latenza di propagazione) al server costerà 100 ms
* Il tempo di risposta del server sarà 100 ms per il documento HTML e 10 ms per tutti gli altri file

## L'esperienza Ciao mondo

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Inizieremo con un markup HTML di base e un'immagine singola, senza CSS o JavaScript, quindi il massimo della semplicità. Adesso dai Chrome DevTools apriamo la barra temporale dell'attività di rete e ispezioniamo la sequenza delle risorse:

We'll start with basic HTML markup and a single image; no CSS or JavaScript. Let's open up our Network timeline in Chrome DevTools and inspect the resulting resource waterfall:

<img src="images/waterfall-dom.png" alt="CRP" />

Una volta che il contenuto HTML è disponibile, il browser deve analizzare i byte, convertirli in token e costruire la struttura DOM. DevTools segnala in modo pratico l'orario dell'evento DOMContentLoaded nella parte inferiore (216 ms), che corrisponde anche alla linea blu verticale. La distanza tra la fine del download HTML e la linea blu verticale (DOMContentLoaded) corrisponde al tempo che il browser ha impiegato per la costruzione della struttura DOM, in questo caso, solo pochi millisecondi.

Infine, noterai qualcosa d'interessante: la nostra 'incredibile foto' non ha bloccato l'evento domContentLoaded. Ne emerge che possiamo costruire la struttura di rendering e addirittura disegnare la pagina senza dover attendere ogni asset sulla pagina: **non tutte le risorse sono cruciali alla fornitura della fast first paint**. In realtà, come vedremo, quando parliamo di percorso di rendering critico solitamente parliamo di markup HTML, CSS e JavaScript. Le immagini non bloccano il rendering iniziale della pagina, sebbene, ovviamente, dovremmo cercare di assicurarci di ottenere le immagini disegnate anche il prima possibile.<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

Detto questo, l'evento di `load` (noto comunemente anche come `onload`) viene bloccato sull'immagine: DevTools segnala l'evento di onload a 335 ms. Ricorda che l'evento di onload segna il punto in cui **tutte le risorse** necessarie alla pagina sono state scaricate ed elaborate, questo è il punto in cui il rotante di caricamento può interrompere la rotazione nel browser e viene contrassegnato dalla linea verticale rossa nella sequenza.

La nostra pagina 'Esperienza ciao mondo' potrebbe sembrare semplice in apparenza, ma ci sono molte cose in ballo in sottofondo per metterla in atto. Detto questo, in pratica ci servirà anche molto più di HTML: è possibile che avremo un foglio di stile CSS e uno o più script per aggiungere interattività alla nostra pagina. Aggiungiamo entrambe le cose all'insieme e vediamo che succede:

That said, the `load` event (also known as `onload`), is blocked on the image: DevTools reports the `onload` event at 335ms. Recall that the `onload` event marks the point at which **all resources** that the page requires have been downloaded and processed; at this point, the loading spinner can stop spinning in the browser (the red vertical line in the waterfall).

## Aggiunta di JavaScript e CSS all'insieme

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

* A differenza del nostro esempio di HTML semplice, adesso dobbiamo anche recuperare e analizzare il file CSS per costruire CSSOM e sappiamo che ci serve sia DOM che CSSOM per costruire la struttura di rendering.
* Dato che sulla nostra pagina abbiamo anche un parser che blocca il file JavaScript sulla nostra pagina, l'evento domContentLoaded viene bloccato finché il file CSS non è stato scaricato e analizzato: JavaScript potrebbe eseguire una query a CSSOM, per questo motivo dobbiamo bloccare e aspettare CSS prima di poter eseguire JavaScript.

**What if we replace our external script with an inline script?** Even if the script is inlined directly into the page, the browser can't execute it until the CSSOM is constructed. In short, inlined JavaScript is also parser blocking.

That said, despite blocking on CSS, does inlining the script make the page render faster? Let's try it and see what happens.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*JavaScript (esterno) con blocco parser:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, JS" />

We are making one less request, but both our `onload` and `domContentLoaded` times are effectively the same. Why? Well, we know that it doesn't matter if the JavaScript is inlined or external, because as soon as the browser hits the script tag it blocks and waits until the CSSOM is constructed. Further, in our first example, the browser downloads both CSS and JavaScript in parallel and they finish downloading at about the same time. In this instance, inlining the JavaScript code doesn't help us much. But there are several strategies that can make our page render faster.

First, recall that all inline scripts are parser blocking, but for external scripts we can add the "async" keyword to unblock the parser. Let's undo our inlining and give that a try:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

In alternativa, avremmo potuto provare un approccio differente e rendere inline sia CSS che JavaScript:

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

La pagina più semplice possibile è costituita solo da markup HTML: niente CSS, JavaScript o altri tipi di risorse. Per eseguire il rendering di questa pagina, il browser deve avviare la richiesta, attendere l'arrivo del documento HTML, analizzarlo, costruire il DOM e infine eseguirne il rendering sullo schermo:

Alternatively, we could have inlined both the CSS and JavaScript:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**Il tempo tra T<sub>0</sub> e T<sub>1</sub> acquisisce i tempi di elaborazione della rete e del server.** Nel caso migliore (se il file HTML è piccolo), tutto quello che ci servirà è un roundtrip di rete per recuperare l'intero documento: a causa delle modalità di funzionamento dei protocolli TCP, i file di maggiori dimensioni potrebbero richiedere più roundtrip, questo è un argomento su cui torneremo in una lezione futura. **Di conseguenza, possiamo dire che la pagina di cui sopra, nel caso migliore, ha un percorso di rendering critico del roundtrip (minimo).**

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" />

Notice that the `domContentLoaded` time is effectively the same as in the previous example; instead of marking our JavaScript as async, we've inlined both the CSS and JS into the page itself. This makes our HTML page much larger, but the upside is that the browser doesn't have to wait to fetch any external resources; everything is right there in the page.

Ancora una volta, affrontiamo un roundtrip di rete per recuperare il documento HTML, quindi il markup recuperato ci informa che ci servirà anche il file CSS: questo significa che il browser deve tornare al server e ottenere il CSS prima che possa eseguire il rendering della pagina sullo schermo. **Di conseguenza, questa pagina affronterà un minimo di due roundtrip prima di poter visualizzare la pagina**: ancora una volta, il file CSS potrebbe eseguire multipli roundtrip, da qui l'enfasi su 'minimo'.

Definiamo il vocabolario che utilizzeremo per descrivere il percorso di rendering critico:

## Pattern di performance

Adesso confrontiamolo alle caratteristiche del percorso critico degli esempi HTML e CSS di cui sopra:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

<img src="images/analysis-dom.png" alt="Hello world CRP" />

Ok, adesso aggiungiamo un altro file JavaScript all'insieme.

Abbiamo aggiunto app.js, che è un asset JavaScript esterno della pagina e, come ormai sappiamo, è una risorsa per il blocco del parser (dunque critica). Ancora peggio, per poter eseguire il file JavaScript dovremo anche bloccare e aspettare CSSOM, ricorda che JavaScript può eseguire una query a CSSOM e quindi il browser si fermerà finché non sarà stato scaricato `style.css` e CSSOM non sarà stato costruito.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

Adesso abbiamo tre risorse critiche che arrivano a 11 KB di byte critici, ma la nostra lunghezza del percorso critico è ancora due roundtrip perché possiamo trasferire CSS e JavaScript in parallelo. **Individuare le caratteristiche del tuo percorso di rendering critico significa essere in grado di identificare le risorse critiche e anche comprendere le modalità in cui il browser ne pianificherà il recupero.** Procediamo con il nostro esempio...

Dopo aver parlato con i nostri sviluppatori del sito, ci siamo resi conto che il JavaScript che abbiamo incluso sulla nostra pagina non deve bloccare: è presente analisi e altro codice che non deve bloccare il rendering della pagina. Sapendo questo, possiamo aggiungere l'attributo `async` al tag script per sbloccare il parser:

* **Risorsa critica:** risorsa che potrebbe bloccare il rendering iniziale della pagina.
* **Lunghezza percorso critico:** numero di roundtrip, o il tempo totale necessario a recuperare tutte le risorse critiche.
* **Byte critici:** quantità totale di byte necessari a ottenere il primo rendering della pagina, che è la somma delle dimensioni file di trasferimento di tutte le risorse critiche. Il nostro primo esempio con una singola pagina HTML conteneva un'unica risorsa critica (il documento HTML), la lunghezza del percorso critico era inoltre uguale a un roundtrip di rete (presumendo che il file sia piccolo) e i byte totali critici erano poco più delle dimensioni di trasferimento del documento HTML stesso.

Now let's compare that to the critical path characteristics of the HTML + CSS example above:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** risorse critiche
* **2** o più roundtrip per la lunghezza del percorso critico minima
* **9** KB di byte critici

Di conseguenza, la nostra pagina ottimizzata è tornata a due risorse critiche (HTML e CSS), con una lunghezza del percorso critico di due roundtrip e un totale di 9 KB di byte critici.

Infine, poniamo che il foglio di stile CSS fosse necessario solo per la stampa. Che aspetto avrebbe?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

Dato che la risorsa style.css viene utilizzata solo per la stampa, il browser non la deve bloccarsi su di esso per eseguire il rendering della pagina. Quindi, non appena la costruzione DOM è completa, il browser dispone di informazioni sufficienti per eseguire il rendering della pagina. Di conseguenza, questa pagina presenta solamente una singola risorsa critica (il documento HTML) e la lunghezza minima del percorso di rendering critico è un roundtrip.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

That said, in practice if we look at this page's "network waterfall," you'll see that both the CSS and JavaScript requests are initiated at about the same time; the browser gets the HTML, discovers both resources, and initiates both requests. As a result, the above page has the following critical path characteristics:

* **3** risorse critiche
* **2** o più roundtrip per la lunghezza del percorso critico minima
* **11** KB di byte critici

We now have three critical resources that add up to 11KB of critical bytes, but our critical path length is still two roundtrips because we can transfer the CSS and JavaScript in parallel. **Figuring out the characteristics of your critical rendering path means being able to identify the critical resources and also understanding how the browser will schedule their fetches.** Let's continue with our example.

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

An asynchronous script has several advantages:

* Lo script non blocca più il parser e non fa parte del percorso di rendering critico
* Dato che non ci sono altri script critici, nemmeno CSS deve bloccare l'evento domContentLoaded
* Prima viene avviato l'evento domContentLoaded, prima l'altra logica delle applicazioni potrà iniziare l'esecuzione

As a result, our optimized page is now back to two critical resources (HTML and CSS), with a minimum critical path length of two roundtrips, and a total of 9KB of critical bytes.

Finally, if the CSS stylesheet were only needed for print, how would that look?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

Because the style.css resource is only used for print, the browser doesn't need to block on it to render the page. Hence, as soon as DOM construction is complete, the browser has enough information to render the page. As a result, this page has only a single critical resource (the HTML document), and the minimum critical rendering path length is one roundtrip.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}