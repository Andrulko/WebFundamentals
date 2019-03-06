project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Per fornire il tempo più rapido possibile al primo rendering, dobbiamo ottimizzare tre variabili: ridurre il numero di risorse critiche, diminuire il numero di byte critici e ridurre la lunghezza del percorso critico.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Ottimizzazione del percorso di rendering critico {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Per fornire il tempo più rapido possibile al primo rendering, dobbiamo ottimizzare tre variabili:

- The number of critical resources.
- The critical path length.
- The number of critical bytes.

Una risorsa critica è una qualsiasi risorsa che potrebbe bloccare il rendering della pagina. Minore è il numero di queste risorse sulla pagina, minore sarà il lavoro che dovrà eseguire il browser per portare il contenuto sullo schermo e si verificherà anche minore conflitto per la CPU e per le altre risorse.

Similmente, minore è il numero di byte critici che il browser deve scaricare, più velocemente potrà arrivare a elaborare il contenuto e renderlo visibile sullo schermo. Per ridurre il numero di byte possiamo diminuire il numero di risorse (eliminarle o renderle non critiche) e anche assicurarci di ridurre le dimensioni di trasferimento attraverso la compressione e l'ottimizzazione di ciascuna risorsa.

Infine, la lunghezza del percorso critico varia in base a un grafico di dipendenza tra tutte le risorse critiche necessarie alla pagina e le relative dimensioni in byte: alcuni download delle risorse possono essere avviati solo una volta che è stata elaborata una risorsa precedente, e più grande è la risorsa maggiore sarà il numero di roundtrip necessari a scaricarla.

**The general sequence of steps to optimize the critical rendering path is:**

1. Analizzare e caratterizzare il tuo percorso critico: numero di risorse, byte e lunghezza.
2. Ridurre il numero di risorse critiche: eliminarle, rinviarne il download, segnarle come async, e così via.
3. Ottimizzare l'ordine in cui le risorse critiche rimanenti vengono caricate: vuoi scaricare il prima possibile tutti gli asset critici per ridurre la lunghezza del percorso critico.
4. Ottimizzare il numero di byte critici per ridurre il tempo di download (numero di roundtrip).

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}