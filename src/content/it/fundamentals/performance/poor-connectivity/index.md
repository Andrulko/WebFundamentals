project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: È importante capire come si comporta l'applicazione o il sito come quando la connettività è scarsa o inaffidabile, e costruirlo di conseguenza. Una gamma di strumenti può aiutarti.

{# wf_updated_on: 2017-11-10 #} {# wf_published_on: 2016-05-09 #}

# Comprendere la bassa larghezza di banda e la latenza elevata {: .page-title }

{% include "web/_shared/contributors/samdutton.html" %}

È importante capire come si comporta l'applicazione o il sito in caso di connettività scarsa o inaffidabile, e costruire di conseguenza. Una gamma di strumenti può aiutarti.

## Test con bassa larghezza di banda e latenza elevata {: #testing }

Una [quantità crescente](http://adwords.blogspot.co.uk/2015/05/building-for-next-moment.html) di persone utilizza la rete sui dispositivi mobili. Anche a casa, [molte persone stanno abbandonando la banda larga fissa per il mobile](https://www.washingtonpost.com/news/the-switch/wp/2016/04/18/new-data-americans-are-abandoning-wired-home-internet/).

In questo contesto, è importante capire come si comporta l'applicazione o il sito quando la connettività è scarsa o inaffidabile. Una gamma di strumenti software può aiutarti a [emulare e simulare](https://stackoverflow.com/questions/1584617/simulator-or-emulator-what-is-the-differenza) la bassa larghezza di banda e [la latenza elevata](https: //www.igvita.com/2012/07/19/latency-the-new-web-performance-bottleneck/).

### Emulare un rallentamento di rete

Durante la creazione o l'aggiornamento di un sito, è necessario assicurare prestazioni adeguate in una varietà di condizioni di connettività. Diversi strumenti possono aiutarti.

#### Strumenti del browser

[Chrome DevTools](/web/tools/chrome-devtools) ti consente di testare il tuo sito con varie velocità di upload/download e [tempi di round-trip](https://www.igvita.com/2012/07/19/latency-the-new-web-performance-bottleneck/), utilizzando impostazioni preimpostate o personalizzate dal pannello di rete. Vedere [Iniziare con l'Analisi delle Performance di Rete](/web/tools/chrome-devtools/network-performance/) per imparare le basi.

![Chrome DevTools throttling](images/chrome-devtools-throttling.png)

#### Strumenti di sistema

Network Link Conditioner è un pannello di preferenze disponibile su Mac installando [Hardware IO Tools](https://developer.apple.com/downloads/?q=Hardware%20IO%20Tools) per Xcode:

![impostazioni Mac Network Link Conditioner](images/network-link-conditioner-control-panel.png)

![impostazioni personalizzate Mac Network Link Conditioner](images/network-link-conditioner-settings.png)

![Mac Network Link Conditioner custom settings](images/network-link-conditioner-custom.png)

#### Emulazione del dispositivo

[Android Emulator](http://developer.android.com/tools/devices/emulator.html#netspeed) allows you to simulate various network conditions while running apps (including web browsers and hybrid web apps) on Android:

![Impostazioni dell'Emulatore Android](images/android-emulator.png)

![Android Emulator settings](images/android-emulator-settings.png)

Le prestazioni della connettività dipendono dalla posizione del server e dal tipo di rete.

### Test da diversi luoghi e reti

[WebPagetest](https://webpagetest.org) è un servizio online che consente di eseguire un set di test di prestazioni per il tuo sito utilizzando una vasta gamma di reti e località di hosting. Ad esempio, puoi provare il tuo sito da un server in India su una rete 2G o su un cavo da una città negli Stati Uniti.

[WebPagetest](https://webpagetest.org) is an online service that enables a set of performance tests to be run for your site using a variety of networks and host locations. For example, you can try out your site from a server in India on a 2G network, or over cable from a city in the US.

![WebPagetest settings](images/webpagetest.png)

[Fiddler](http://www.telerik.com/fiddler) supporta il proxy Globale tramite [GeoEdge](http://www.geoedge.com/faq) e le sue regole personalizzate possono essere utilizzate per simulare le velocità del modem:

[Fiddler](http://www.telerik.com/fiddler) supports Global proxying via [GeoEdge](http://www.geoedge.com/faq), and its custom rules can be used to simulate modem speeds:

![Fiddler proxy](images/fiddler.png)

### Test su una rete altalenante

Il sistema Facebook di [Augmented Traffic Control](http://facebook.githubith.io/augmented-traffic-control/) (ATC) è un insieme di applicazioni con licenza BSD che possono essere utilizzate per modificare il traffico ed emulare condizioni di rete scarse.

Facebook's [Augmented Traffic Control](http://facebook.github.io/augmented-traffic-control/) (ATC) is a BSD-licensed set of applications that can be used to shape traffic and emulate impaired network conditions:

![Facebook's Augmented Traffic Control](images/augmented-traffic-control.png)

> Facebook ha anche istituito i [Martedì 2G](https://code.facebook.com/posts/1556407321275493/building-for-emerging-markets-the-story-behind-2g-tuesdays/) per capire come le persone su 2G utilizzano il loro prodotto. Il martedì i dipendenti visualizzano un pop-up che offre loro la possibilità di simulare una connessione 2G.

The [Charles](https://www.charlesproxy.com/){: .external } HTTP/HTTPS proxy can be used to [adjust bandwidth and latency](http://www.charlesproxy.com/documentation/proxying/throttling/). Charles is commercial software, but a free trial is available.

![Charles proxy bandwidth and latency settings](images/charles.png)

Il termine [lie-fi](http://www.urbandictionary.com/define.php?term=lie-fi") risale almeno al 2008 (quando i telefoni erano fatti così [Immagini di telefoni dal 2008](https://www.mobilegazette.com/2008-phones-wallchart.htm)), e si riferisce alla connettività che non è ciò che sembra. Il tuo browser si comporta come se avesse connettività quando, per qualunque motivo, non è così.

## Gestire connessioni non affidabili e "lie-fi" {: #lie-fi }

### Che cos'è il lie-fi?

La connettività errata può provocare una scarsa esperienza d'uso poiché il browser (o JavaScript) persiste nel tentativo di recuperare risorse anziché abbandonare e scegliere un fallback ragionevole. Il lie-fi può in realtà essere peggiore dell'offline; almeno se un dispositivo è decisamente offline, il tuo JavaScript può adottare un'opportuna azione evasiva.

È probabile che il lie-fi diventi un problema sempre maggiore in quanto più persone si muovono in mobilità e lontano dalla banda larga fissa. Recenti [dati del censimento degli Stati Uniti](https://www.ntia.doc.gov/blog/2016/evolving-technologies-change-nature-internet-use) mostrano un [allontanamento dalla banda larga fissa](https://www.washingtonpost.com/news/the-switch/wp/2016/04/18/new-data-americans-are-abandoning-wired-home-internet/). Il seguente grafico mostra l'utilizzo di internet mobile a casa nel 2015 rispetto al 2013:

Lie-fi is likely to become a bigger problem as more people move to mobile and away from fixed broadband. Recent [US Census data](https://www.ntia.doc.gov/blog/2016/evolving-technologies-change-nature-internet-use) shows a [move away from fixed broadband](https://www.washingtonpost.com/news/the-switch/wp/2016/04/18/new-data-americans-are-abandoning-wired-home-internet/). The following chart shows the use of mobile internet at home in 2015 compared with 2013:

<img src="images/home-broadband.png" class="center" alt="Chart from US census data
showing the move to mobile away from fixed broadband, particularly in lower-income households" />

### Utilizzare timeout per gestire la connettività intermittente

È inoltre prevista una [opzione di timeout](https://github.com/whatwg/fetch/issues/20) per l'[API Fetch](https://developer.mozilla.org/en-US/docs/Web/API/GlobalFetch/fetch) e l'[API Streams](https://www.w3.org/TR/streams-api/) che dovrebbero aiutare ad ottimizzare la distribuzione dei contenuti ed evitare richieste monolitiche. Jake Archibald fornisce maggiori dettagli su come affrontare il lie-fi nel [Supercharging page load](https://youtu.be/d5_6yHixpsQ?t=6m42s).

    toolbox.router.get(
      '/path/to/image',
      toolbox.networkFirst,
      {networkTimeoutSeconds: 3}
    );
    

Translated by {% include "web/_shared/contributors/lucaberton.html" %}

[Timeout functionality](/web/updates/2017/09/abortable-fetch) is also being developed for the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/GlobalFetch/fetch), and the [Streams API](https://www.w3.org/TR/streams-api/) should help by optimizing content delivery and avoiding monolithic requests. Jake Archibald gives more details about tackling lie-fi in [Supercharging page load](https://youtu.be/d5_6yHixpsQ?t=6m42s).

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}