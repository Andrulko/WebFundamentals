project_path: /web/fundamentals/_project.yaml book_path: /web/fundamentals/_book.yaml description: Precaricare video e audio per una riproduzione più veloce.

{# wf_published_on: 2017-08-17 #} {# wf_updated_on: 2018-03-20 #} {# wf_blink_components: Blink>Media #}

# Riproduzione veloce con il precaricamento dei video {: .page-title}

{% include "web/_shared/contributors/beaufortfrancois.html" %}

Un avvio più veloce della riproduzione significa che più persone possono guardare un video. È un fatto noto. In questo articolo esaminerò le tecniche che è possibile utilizzare per accelerare la riproduzione multimediale attivando il pre-caricamento delle risorse in relazione al caso d'uso.

Note: questo articolo si applica anche all'elemento audio, se non diversamente specificato.<figure> <video controls controlslist="nodownload" muted playsinline style="width: 100%"> <source src="https://storage.googleapis.com/webfundamentals-assets/videos/video-preload-hero.webm#t=1.1" type="video/webm"> <source src="https://storage.googleapis.com/webfundamentals-assets/videos/video-preload-hero.mp4#t=1.1" type="video/mp4"> </video> <figcaption> 

<p>
  Credits: copyright Blender Foundation | <a
href="http://www.blender.org">www.blender.org</a> .
</p></figcaption> </figure> 

### TL;DR {: .hide-from-toc }

<table>
  <tr>
    <th>
    </th>
    
    <th>
      È ottimo...
    </th>
    
    <th>
      Ma...
    </th>
  </tr>
  
  <tr>
    <td rowspan=3 style="white-space: nowrap"> <a href="#video_preload_attribute">Attributo video preload</a> </td> <td rowspan=3> Semplice da usare per un file unico ospitato su un server web. </td> 
    
    <td>
      I browser possono ignorare completamente l'attributo.
    </td>
    
    <tr>
      <td>
        Il recupero delle risorse inizia quando il documento HTML è stato completamente caricato e analizzato.
      </td>
    </tr>
    
    <tr>
      <td>
        MSE ignora l'attributo preload sugli elementi multimediali perché l'app è responsabile della fornitura di contenuti multimediali a MSE.
      </td>
    </tr>
    
    <tr>
      <td rowspan=2 style="white-space: nowrap"> <a href="#link_preload">Link preload</a> </td> 
      
      <td>
        Forza il browser ad effettuare una richiesta per una risorsa video senza bloccare l'evento <code>onload</code> del documento.
      </td>
      
      <td>
        Le richieste HTTP Range non sono compatibili.
      </td>
      
      <tr>
        <td>
          Compatibile con MSE e segmenti di file.
        </td>
        
        <td>
          Dovrebbe essere usato solo per file multimediali di piccole dimensioni (ll resources.
        </td>
      </tr>
      
      <tr>
        <td>
          <a href="#manual_buffering">Buffering manuale</a>
        </td>
        
        <td>
          Pieno controllo
        </td>
        
        <td>
          La gestione complessa degli errori è responsabilità del sito Web.
        </td>
      </tr></tbody> </table> 
      
      <h2>
        Attributo video preload
      </h2>
      
      <p>
        Se la sorgente video è un file univoco ospitato su un server web potresti voler utilizzare l'attributo video <code>preload</code> per fornire un suggerimento al browser sulla <a href="/web/fundamentals/media/video#preload">quantità di informazioni o contenuti da precaricare</a> . Ciò significa che <a href="/web/fundamentals/media/mse/basics">Media Source Extensions (MSE)</a> non è compatibile con <code>preload</code>.
      </p>
      
      <p>
        Il recupero delle risorse inizierà solo quando il documento HTML iniziale è stato completamente caricato ed analizzato (ad esempio l'evento <code>DOMContentLoaded</code> è stato attivato) a differenza dell'evento <code>window.onload</code> che sarà attivato quando la risorsa viene effettivamente recuperata.
      </p><figure> 
      
      <img src="/web/fundamentals/media/images/video-preload/video-preload.svg" /> </figure> 
      
      <p>
        L'impostazione dell'attributo <code>preload</code> sui <code>metadata</code> indica che non è previsto che l'utente abbia bisogno del video, ma si consiglia di recuperare i metadati (dimensioni, elenco di brani, durata e così via).
      </p>
      
      <pre><code>&lt;video id="video" preload="metadata" src="file.mp4" controls&gt;&lt;/video&gt;

&lt;script&gt;
  video.addEventListener('loadedmetadata', function() {
    if (video.buffered.length === 0) return;

    var bufferedSeconds = video.buffered.end(0) - video.buffered.start(0);
    console.log(bufferedSeconds + ' seconds of video are ready to play!');
  });
&lt;/script&gt;
</code></pre>
      
      <p>
        L'impostazione dell'attributo <code>preload</code> su <code>auto</code> indica che il browser può memorizzare nella cache un numero sufficiente di dati per completare la riproduzione senza richiedere arresti per ulteriore buffering.
      </p>
      
      <pre><code>&lt;video id="video" preload="auto" src="file.mp4" controls&gt;&lt;/video&gt;

&lt;script&gt;
  video.addEventListener('loadedmetadata', function() {
    if (video.buffered.length === 0) return;

    var bufferedSeconds = video.buffered.end(0) - video.buffered.start(0);
    console.log(bufferedSeconds + ' seconds of video are ready to play!');
  });
&lt;/script&gt;
</code></pre>
      
      <p>
        Bisogna fare attenzione però, perché questo è solo un suggerimento e il browser potrebbe ignorare completamente l'attributo <code>preload</code>. Al momento della stesura ecco alcune regole applicate da Chrome:
      </p>
      
      <ul>
        <li>
          Quando <a href="https://support.google.com/chrome/answer/2392284">Data Saver</a> è abilitato, Chrome impone il valore di <code>preload</code> su <code>none</code> .
        </li>
        <li>
          In Android 4.3 Chrome impone il valore di <code>preload</code> a <code>none</code> causa di un <a href="https://bugs.chromium.org/p/chromium/issues/detail?id=612909">bug di Android</a> .
        </li>
        <li>
          Su una connessione cellulare (2G, 3G e 4G) Chrome impone il valore <code>preload</code> a <code>metadata</code>.
        </li>
      </ul>
      
      <h3>
        Suggerimenti
      </h3>
      
      <p>
        Se il vostro sito contiene molte risorse video sullo stesso dominio vi consiglio di impostare il valore <code>preload</code> su <code>metadata</code> o definire gli attributi <code>poster</code> e impostare <code>preload</code> a <code>none</code>. Facendo ciò eviterai di raggiungere il numero massimo di connessioni HTTP per lo stesso dominio (6 in base alle specifiche di HTTP 1.1) che possono bloccare il caricamento di risorse. Tieni presente che ciò può anche migliorare la velocità della pagina se i video non fanno parte dell'esperienza utente principale.
      </p>
      
      <h2>
        Link preload
      </h2>
      
      <p>
        Come <a href="/web/updates/2016/03/link-rel-preload">abbiamo letto</a> in altri <a href="https://www.smashingmagazine.com/2016/02/preload-what-is-it-good-for/">articoli</a>, <a href="https://w3c.github.io/preload/">link preload</a> è un recupero dichiarativo che consente di forzare il browser ad effettuare la richiesta di una risorsa senza bloccare l'evento <code>window.onload</code>, mentre la pagina viene scaricata. Le risorse caricate tramite <code>&lt;link rel="preload"&gt;</code> sono memorizzate localmente nel browser e sono effettivamente inerti fino a quando non vengono esplicitamente referenziate da DOM, JavaScript o CSS.
      </p>
      
      <p>
        Preload è diverso da prefetch in quanto si concentra sulla navigazione corrente e recupera le risorse con priorità in base al loro tipo (script, stile, carattere, video, audio, ecc.). Dovrebbe essere usato per riempire la cache del browser per le sessioni correnti.
      </p><figure> 
      
      <img src="/web/fundamentals/media/images/video-preload/link-preload.svg" /> </figure> 
      
      <h3>
        Preload del video completo
      </h3>
      
      <p>
        Ecco come precaricare un video completo sul tuo sito Web in modo che quando il JavaScript chiede di recuperare il contenuto del video, questo viene letto dalla cache in quanto la risorsa potrebbe essere già stata memorizzata nella cache del browser. Se la richiesta di preload non è ancora terminata, si verificherà un recupero regolare della rete.
      </p>
      
      <pre><code>&lt;link rel="preload" as="video" href="https://cdn.com/small-file.mp4"&gt;

&lt;video id="video" controls&gt;&lt;/video&gt;

&lt;script&gt;
  // Later on, after some condition has been met, set video source to the
  // preloaded video URL.
  video.src = 'https://cdn.com/small-file.mp4';
  video.play().then(_ =&gt; {
    // If preloaded video URL was already cached, playback started immediately.
  });
&lt;/script&gt;
</code></pre>
      
      <p>
        Note: raccomanderei l'utilizzo solo per file multimediali di piccole dimensioni (
      </p>
      
      <p>
        Poiché la risorsa preload sta per essere consumata da un elemento video nell'esempio, il valore del preload link <code>as</code> è <code>video</code>. Se si trattasse di un elemento audio sarebbe stato <code>as="audio"</code> .
      </p>
      
      <h3>
        Preload del primo segmento
      </h3>
      
      <p>
        L'esempio seguente mostra come precaricare il primo segmento di un video con <code>&lt;link rel="preload"&gt;</code> e usarlo con Media Source Extensions. Se non hai familiarità con l'API Javascript di MSE, leggi le <a href="/web/fundamentals/media/mse/basics">nozioni di base di MSE</a> .
      </p>
      
      <p>
        Per semplicità supponiamo che l'intero video sia stato diviso in file più piccoli come "file_1.webm", "file_2.webm", "file_3.webm", ecc.
      </p>
      
      <pre><code>&lt;link rel="preload" as="fetch" href="https://cdn.com/file_1.webm"&gt;

&lt;video id="video" controls&gt;&lt;/video&gt;

&lt;script&gt;
  const mediaSource = new MediaSource();
  video.src = URL.createObjectURL(mediaSource);
  mediaSource.addEventListener('sourceopen', sourceOpen, { once: true });

  function sourceOpen() {
    URL.revokeObjectURL(video.src);
    const sourceBuffer = mediaSource.addSourceBuffer('video/webm; codecs="vp09.00.10.08"');

    // If video is preloaded already, fetch will return immediately a response
    // from the browser cache (memory cache). Otherwise, it will perform a
    // regular network fetch.
    fetch('https://cdn.com/file_1.webm')
    .then(response =&gt; response.arrayBuffer())
    .then(data =&gt; {
      // Append the data into the new sourceBuffer.
      sourceBuffer.appendBuffer(data);
      // TODO: Fetch file_2.webm when user starts playing video.
    })
    .catch(error =&gt; {
      // TODO: Show "Video is not available" message to user.
    });
  }
&lt;/script&gt;
</code></pre>
      
      <p>
        Warning: per le risorse di cross-origin, assicurati che le intestazioni CORS siano impostate correttamente. Poiché non possiamo creare un buffer di array da una response opaca recuperata con <code>fetch(videoFileUrl, { mode: 'no-cors' })</code>, non saremo in grado di alimentare alcun elemento video o audio.
      </p>
      
      <h3>
        Supporto
      </h3>
      
      <p>
        Link preload non è ancora supportato in tutti i browser. Accertati della sua disponibilità con i segmenti seguenti per adattare le tue metriche di performance.
      </p>
      
      <pre><code>function preloadFullVideoSupported() {
  const link = document.createElement('link');
  link.as = 'video';
  return (link.as === 'video');
}

function preloadFirstSegmentSupported() {
  const link = document.createElement('link');
  link.as = 'fetch';
  return (link.as === 'fetch');
}
</code></pre>
      
      <h2>
        Buffering manuale
      </h2>
      
      <p>
        Prima di approfondire l'argomento dell'<a href="https://developer.mozilla.org/en-US/docs/Web/API/Cache">API Cache</a> e dei service worker, vediamo come eseguire il buffering manuale di un video con MSE. L'esempio seguente presuppone che il tuo server web supporti richieste HTTP Range ma sarebbe abbastanza simile anche se si trattasse di segmenti di file. Nota che alcune librerie middleware come <a href="https://github.com/google/shaka-player">Shaka Player</a> , <a href="https://developer.jwplayer.com/">JW Player</a> e <a href="http://videojs.com/">Video.js di Google</a> sono progettate per gestire questo per te.
      </p>
      
      <pre><code>&lt;video id="video" controls&gt;&lt;/video&gt;

&lt;script&gt;
  const mediaSource = new MediaSource();
  video.src = URL.createObjectURL(mediaSource);
  mediaSource.addEventListener('sourceopen', sourceOpen, { once: true });

  function sourceOpen() {
    URL.revokeObjectURL(video.src);
    const sourceBuffer = mediaSource.addSourceBuffer('video/webm; codecs="vp09.00.10.08"');

    // Fetch beginning of the video by setting the Range HTTP request header.
    fetch('file.webm', { headers: { range: 'bytes=0-567139' } })
    .then(response =&gt; response.arrayBuffer())
    .then(data =&gt; {
      sourceBuffer.appendBuffer(data);
      sourceBuffer.addEventListener('updateend', updateEnd, { once: true });
    });
  }

  function updateEnd() {
    // Video is now ready to play!
    var bufferedSeconds = video.buffered.end(0) - video.buffered.start(0);
    console.log(bufferedSeconds + ' seconds of video are ready to play!');

    // Fetch the next segment of video when user starts playing the video.
    video.addEventListener('playing', fetchNextSegment, { once: true });
  }

  function fetchNextSegment() {
    fetch('file.webm', { headers: { range: 'bytes=567140-1196488' } })
    .then(response =&gt; response.arrayBuffer())
    .then(data =&gt; {
      const sourceBuffer = mediaSource.sourceBuffers[0];
      sourceBuffer.appendBuffer(data);
      // TODO: Fetch further segment and append it.
    });
  }
&lt;/script&gt;
</code></pre>
      
      <h3>
        Considerazioni
      </h3>
      
      <p>
        Dato che ora hai il controllo dell'intera esperienza di buffering multimediale, ti suggerisco di considerare il livello della batteria del dispositivo, la preferenza utente "Modalità risparmio dati" e le informazioni di rete quando pensi al pre-caricamento.
      </p>
      
      <h4>
        Consapevolezza della batteria
      </h4>
      
      <p>
        Ti consigliamo di considerare il livello di batteria dei dispositivi degli utenti prima di pensare al preload di un video. Ciò conserverà la durata della batteria quando il livello di potenza è basso.
      </p>
      
      <p>
        Disabilita il preload o almeno esegui preload di un video con risoluzione bassa quando il dispositivo sta esaurendo la batteria.
      </p>
      
      <pre><code>if ('getBattery' in navigator) {
  navigator.getBattery()
  .then(battery =&gt; {
    // If battery is charging or battery level is high enough
    if (battery.charging || battery.level &gt; 0.15) {
      // TODO: Preload the first segment of a video.
    }
  });
}
</code></pre>
      
      <h4>
        Rilevamento di "Data-Saver"
      </h4>
      
      <p>
        Utilizza l'intestazione della richiesta di suggerimento del client <code>Save-Data</code> per fornire applicazioni veloci e light agli utenti che hanno optato per la modalità "Data-saver" nel proprio browser. Identificando questa intestazione della richiesta, l'applicazione può personalizzare ed offrire un'esperienza ottimizzata agli utenti con limitazioni di costi e prestazioni.
      </p>
      
      <p>
        Scopri di più leggendo la nostra guida completa su come <a href="/web/updates/2016/02/save-data">Fornire applicazioni veloci e light con Save-Data</a>.
      </p>
      
      <h4>
        Caricamento intelligente basato sulle informazioni di rete
      </h4>
      
      <p>
        Ti consigliamo di controllare <code>navigator.connection.type</code> prima del precaricamento. Quando è impostato su <code>cellular</code>, è possibile impedire il pre-caricamento ed avvisare gli utenti che il proprio operatore di rete mobile potrebbe addebitare la larghezza di banda ed avviare la riproduzione automatica solo dei contenuti precedentemente memorizzati nella cache.
      </p>
      
      <pre><code>if ('connection' in navigator) {
  if (navigator.connection.type == 'cellular') {
    // TODO: Prompt user before preloading video
  } else {
    // TODO: Preload the first segment of a video.
  }
}
</code></pre>
      
      <p>
        Scopri l'<a href="https://googlechrome.github.io/samples/network-information/">Esempio Network Information</a> per sapere come reagire alle modifiche di rete.
      </p>
      
      <h3>
        Pre-cache primi segmenti multipli
      </h3>
      
      <p>
        Cosa succede se voglio precaricare alcuni contenuti multimediali senza sapere quale sceglierà l'utente alla fine? Se l'utente si trova su una pagina Web che contiene 10 video, probabilmente abbiamo abbastanza memoria per recuperare un file di segmento da ciascuno, ma dobbiamo evitare di creare 10 elementi video nascosti e 10 oggetti <code>MediaSource</code> e iniziare ad alimentare quei dati.
      </p>
      
      <p>
        L'esempio in due parti qui sotto mostra come pre-cache più segmenti video utilizzando l'API Cache, potente e facile da usare. Nota che qualcosa di simile può essere raggiunto anche con IndexedDB. Non stiamo ancora utilizzando i service worker in quanto l'API Cache è accessibile anche dall'oggetto Window.
      </p>
      
      <h4>
        Fetch e cache
      </h4>
      
      <pre><code>const videoFileUrls = [
  'bat_video_file_1.webm',
  'cow_video_file_1.webm',
  'dog_video_file_1.webm',
  'fox_video_file_1.webm',
];

// Let's create a video pre-cache and store all first segments of videos inside.
window.caches.open('video-pre-cache')
.then(cache =&gt; Promise.all(videoFileUrls.map(videoFileUrl =&gt; fetchAndCache(videoFileUrl, cache))));

function fetchAndCache(videoFileUrl, cache) {
  // Check first if video is in the cache.
  return cache.match(videoFileUrl)
  .then(cacheResponse =&gt; {
    // Let's return cached response if video is already in the cache.
    if (cacheResponse) {
      return cacheResponse;
    }
    // Otherwise, fetch the video from the network.
    return fetch(videoFileUrl)
    .then(networkResponse =&gt; {
      // Add the response to the cache and return network response in parallel.
      cache.put(videoFileUrl, networkResponse.clone());
      return networkResponse;
    });
  });
}
</code></pre>
      
      <p>
        Nota che se utilizzi le richieste HTTP Range, dovresti ricreare manualmente un oggetto <code>Response</code> poiché l'API Cache non supporta <a href="https://github.com/whatwg/fetch/issues/144">ancora</a> le risposte Range. Tieni presente che la chiamata <code>networkResponse.arrayBuffer()</code> recupera l'intero contenuto della risposta in una sola volta nella memoria del renderer, motivo per cui potresti voler utilizzare intervalli di piccole dimensioni.
      </p>
      
      <p>
        Per riferimento ho modificato parte dell'esempio precedente per salvare le richieste di intervallo HTTP sulla pre-cache del video.
      </p>
      
      <pre><code>    ...
    return fetch(videoFileUrl, { headers: { range: 'bytes=0-567139' } })
    .then(networkResponse =&gt; networkResponse.arrayBuffer())
    .then(data =&gt; {
      const response = new Response(data);
      // Add the response to the cache and return network response in parallel.
      cache.put(videoFileUrl, response.clone());
      return response;
    });
</code></pre>
      
      <h4>
        Riproduzione video
      </h4>
      
      <p>
        Quando un utente fa clic sul pulsante di riproduzione, recuperiamo il primo segmento video disponibile nell'API Cache in modo che la riproduzione inizi immediatamente se disponibile. Altrimenti, lo preleveremo semplicemente dalla rete. Tieni presente che i browser e gli utenti possono decidere di cancellare la <a href="/web/fundamentals/instant-and-offline/web-storage/offline-for-pwa">cache</a> .
      </p>
      
      <p>
        Come visto in precedenza utilizziamo MSE per alimentare quel primo segmento di video all'elemento video.
      </p>
      
      <pre><code>function onPlayButtonClick(videoFileUrl) {
  video.load(); // Used to be able to play video later.

  window.caches.open('video-pre-cache')
  .then(cache =&gt; fetchAndCache(videoFileUrl, cache)) // Defined above.
  .then(response =&gt; response.arrayBuffer())
  .then(data =&gt; {
    const mediaSource = new MediaSource();
    video.src = URL.createObjectURL(mediaSource);
    mediaSource.addEventListener('sourceopen', sourceOpen, { once: true });

    function sourceOpen() {
      URL.revokeObjectURL(video.src);

      const sourceBuffer = mediaSource.addSourceBuffer('video/webm; codecs="vp09.00.10.08"');
      sourceBuffer.appendBuffer(data);

      video.play().then(_ =&gt; {
        // TODO: Fetch the rest of the video when user starts playing video.
      });
    }
  });
}
</code></pre>
      
      <p>
        Warning: per le risorse cross-origin, assicurati che le intestazioni CORS siano impostate correttamente. Non possiamo creare un buffer di array da una risposta opaca recuperata con <code>fetch(videoFileUrl, { mode: 'no-cors' })</code>, poiché non saremo in grado di alimentare alcun elemento video o audio.
      </p>
      
      <h3>
        Creazione di risposte Range con service worker
      </h3>
      
      <p>
        Supponiamo di aver recuperato un intero file video e averlo salvato nell'API Cache. Quando il browser invia una richiesta di HTTP Range, certamente non vuoi includere l'intero video nella memoria del renderer poiché l'API Cache non supporta ancora le risposte Range.
      </p>
      
      <p>
        Ecco come intercettare queste richieste e restituire una risposta Range personalizzata da un service worker.
      </p>
      
      <pre><code>addEventListener('fetch', event =&gt; {
  event.respondWith(loadFromCacheOrFetch(event.request));
});

function loadFromCacheOrFetch(request) {
  // Search through all available caches for this request.
  return caches.match(request)
  .then(response =&gt; {

    // Fetch from network if it's not already in the cache.
    if (!response) {
      return fetch(request);
      // Note that we may want to add the response to the cache and return
      // network response in parallel as well.
    }

    // Browser sends a HTTP Range request. Let's provide one reconstructed
    // manually from the cache.
    if (request.headers.has('range')) {
      return response.blob()
      .then(data =&gt; {

        // Get start position from Range request header.
        const pos = Number(/^bytes\=(\d+)\-/g.exec(request.headers.get('range'))[1]);
        const options = {
          status: 206,
          statusText: 'Partial Content',
          headers: response.headers
        }
        const slicedResponse = new Response(data.slice(pos), options);
        slicedResponse.setHeaders('Content-Range': 'bytes ' + pos + '-' +
            (data.size - 1) + '/' + data.size);
        slicedResponse.setHeaders('X-From-Cache': 'true');

        return slicedResponse;
      });
    }

    return response;
  }
}
</code></pre>
      
      <p>
        È importante notare che ho usato <code>response.blob()</code> per ricreare questa risposta divisa in slice in quanto ciò mi fornisce semplicemente un handle per il file (<a href="https://github.com/whatwg/fetch/issues/569">in Chrome</a>), mentre <code>response.arrayBuffer()</code> porta l'intero file nella memoria del renderer.
      </p>
      
      <p>
        La mia intestazione HTTP personalizzata <code>X-From-Cache</code> può essere utilizzata per sapere se questa richiesta provenga dalla cache o dalla rete. Può essere usata da un servizio come <a href="https://github.com/google/shaka-player/blob/master/docs/tutorials/service-worker.md">ShakaPlayer</a> per ignorare il tempo di risposta come indicatore della velocità della rete.
      </p>
      
      <div class="video-wrapper">
        <iframe class="devsite-embedded-youtube-video" data-video-id="f8EGZa32Mts"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
      </div>
      
      <p>
        Dai un'occhiata all'app ufficiale <a href="https://github.com/GoogleChrome/sample-media-pwa">Sample Media</a> ed in particolare al file <a href="https://github.com/GoogleChrome/sample-media-pwa/blob/master/src/client/scripts/ranged-response.js">ranged-response.js</a> per una soluzione completa di gestione delle richieste Range.
      </p>
      
      <div class="clearfix">
      </div>
      
      <h2>
        Feedback {: #feedback }
      </h2>
      
      <p>
        Translated by {% include "web/_shared/contributors/lucaberton.html" %}
      </p>