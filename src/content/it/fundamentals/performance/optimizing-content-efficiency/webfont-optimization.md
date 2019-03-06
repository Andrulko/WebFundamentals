project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: La tipografia è fondamentale per un buon design, branding, leggibilità ed accessibilità. I font web consentono di ottenere tutto ciò, e non solo: il testo è selezionabile, ricercabile, ingrandibile e compatibile con high-DPI, offrendo un rendering coerente e conciso, indipendentemente dalle dimensioni e dalla risoluzione del monitor. I font web sono fondamentali per ottenere design, UX e prestazioni ottimali.

{# wf_updated_on: 2017-11-10 #} {# wf_published_on: 2014-09-19 #}

# Ottimizzazione dei font web {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*This article contains contributions from [Monica Dinculescu](https://meowni.ca/posts/web-fonts/), [Rob Dodson](/web/updates/2016/02/font-display), and Jeff Posnick.*

L'ottimizzazione dei font web rappresenta una fase critica nell'intera strategia prestazionale. Ogni font costituisce una risorsa aggiuntiva, e alcuni font possono bloccare il rendering del testo, ma il solo fatto che la pagina utilizzi i font web non significa che il rendering debba essere più lento. Al contrario, un font ottimizzato, assieme a una strategia ponderata in merito al loro caricamento e alla loro applicazione nella pagina, può ridurre le dimensioni totali di quest'ultima e migliorare i tempi di rendering.

Un *font web* è una raccolta di glifi, ciascuno dei quali rappresenta una forma vettoriale che descrive una lettera o un simbolo. Di conseguenza, le dimensioni di un particolare file font sono determinate da due semplici variabili: la complessità dei percorsi vettoriali di ogni glifo e il numero di glifi di un determinato font. Ad esempio, OpenSans, uno dei font web più popolari, contiene 897 glifi, che includono caratteri latini, greci e cirillici.

## Anatomia di un font web

### TL;DR {: .hide-from-toc }

* I font Unicode possono contenere migliaia di glifi.
* I formati dei font sono quattro: WOFF2, WOFF, EOT, TTF.
* Per alcuni formati font è necessario utilizzare una compressione GZIP.

A *webfont* is a collection of glyphs, and each glyph is a vector shape that describes a letter or symbol. As a result, two simple variables determine the size of a particular font file: the complexity of the vector paths of each glyph and the number of glyphs in a particular font. For example, Open Sans, which is one of the most popular webfonts, contains 897 glyphs, which include Latin, Greek, and Cyrillic characters.

<img src="images/glyphs.png"  alt="Font glyph table" />

Naturalmente, l'utilizzo di font web richiede una progettazione attenta per garantire che la tipografia non intralci le prestazioni. Per fortuna, la piattaforma web offre tutte le primitive necessarie, mentre nella presente guida daremo un'occhiata pratica a come ottenere il meglio da entrambi i mondi.

Oggigiorno, sul web sono disponibili quattro famiglie di font: [EOT](http://en.wikipedia.org/wiki/Embedded_OpenType), [TTF](http://en.wikipedia.org/wiki/TrueType), [WOFF](http://en.wikipedia.org/wiki/Web_Open_Font_Format), e [WOFF2](http://www.w3.org/TR/WOFF2/). Sfortunatamente, nonostante l'ampia scelta, non esiste un unico formato universale che funzioni con tutti i browser, più o meno recenti: EOT funziona [solo su IE](http://caniuse.com/#feat=eot), TTF è [parzialmente supportato in IE](http://caniuse.com/#search=ttf), WOFF è maggiormente supportato [non disponibile in alcuni browser precedenti](http://caniuse.com/#feat=woff), mentre il supporto di WOFF 2.0 è ancora [in progress in molti browser](http://caniuse.com/#feat=woff2).

### Formati dei font web

Dunque, come dobbiamo muoverci? Non esiste un unico formato che funzioni in tutti i browser, il che significa che dobbiamo utilizzare più formati per offrire un'esperienza coerente:

Note: Tecnicamente esiste anche l'[ SVG font container](http://caniuse.com/svg-fonts), ma non è mai stato supportato né in IE, né in Firefox e non è approvato in Chrome. Di conseguenza, ha un utilizzo limitato e lo ometteremo intenzionalmente nella presente guida.

* Offrire la variante WOFF 2.0 ai browser che la supportano
* Offrire la variante WOFF alla maggior parte dei browser
* Offrire la variante TTF ai vecchi browser Android (precedenti al 4.4)
* Offrire la variante EOT ai vecchi browser IE (precedenti a IE9)

Un font è una raccolta di glifi, ciascuno dei quali è composto da un insieme di percorsi de definiscono la forma del carattere. I singoli glifi sono naturalmente diversi, ma contengono comunque molte informazioni simili che possono essere compresse con GZIP o altro compressore compatibile:

### Riduzione delle dimensioni del font tramite compressione

Infine, vale la pena notare che alcuni formati font contengono metadati aggiuntivi, quali informazioni di [font hinting](http://en.wikipedia.org/wiki/Font_hinting) e [kerning](http://en.wikipedia.org/wiki/Kerning) che possono non essere necessarie su alcune piattaforme, ma che consentono un'ulteriore ottimizzazione delle dimensioni file. Consulta le opzioni di ottimizzazione messe a disposizione dal tuo compressore font e, se scegli questo percorso, assicurati di disporre delle infrastrutture adatte per testare e utilizzare tali font ottimizzati in ogni browser. Ad es., Google Fonts offre più di 30 varianti ottimizzate per ogni font, individuando e fornendo automaticamente la variabile ottimale per ogni piattaforma e browser.

* I formati EOT e TTF non sono compressi per default: assicurati che i server siano configurati per applicare la [compressione GZIP](/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text-compression-with-gzip) quando utilizzi tali formati.
* WOFF ha una compressione integrata. Assicurati che il tuo compressore WOFF utilizzi impostazioni ottimali.
* WOFF2 utilizza algoritmi di pre-elaborazione e compressione personalizzati per garantire una riduzione di ~30% delle dimensioni file rispetto ad altri formati. Per maggiori informazioni si veda il [report](http://www.w3.org/TR/WOFF20ER/).

Note: Valuta l'utilizzo della 
<a href='http://en.wikipedia.org/wiki/Zopfli'>compressione Zopfli</a> per i formati EOT, TTF e WOFF. Zopfli è un compressore compatibile con zlib che offre una riduzione delle dimensioni file ~5% superiore a gzip.

L'at-rule del CSS @font-face ci consente di definire il percorso di un determinato font, nonché le sue caratteristiche stilistiche e i codepoint Unicode per cui deve essere utilizzato. È possibile utilizzare una combinazione di tali dichiarazioni @font-face per costruire una "famiglia di font" che il browser userà per stabilire quali font debbano essere scaricati e applicati alla pagina corrente.

## Definire una famiglia di font con @font-face

### TL;DR {: .hide-from-toc }

* Utilizza il `format()` hint per specificare più formati font
* Raggruppa i font unicode più ampi in subset per migliorare le prestazioni: utilizza subset unicode-range e consenti in alternativa un subsetting manuale per versioni precedenti dei browser
* Riduci il numero di varianti dei font di stile per migliorare le prestazioni della pagina e il rendering del testo

Ogni dichiarazione @font-face fornisce il nome della famiglia di font, utilizzato come gruppo logico di dichiarazioni multiple, le [proprietà del font](http://www.w3.org/TR/css3-fonts/#font-prop-desc) quali stile, spessore ed estensione, e il [descrittore src](http://www.w3.org/TR/css3-fonts/#src-desc), che specifica un elenco prioritario di percorsi per il font.

### Selezione formato

Nota innanzitutto che gli esempi precedenti definiscono un'unica famiglia *Awesome Font* con due stili (normale e *italic*), ciascuno dei quali si riferisce a un insieme diverso di font. Ogni descrittore `src` contiene un elenco prioritario, separato da virgole, di varianti:

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome.woff2') format('woff2'), 
           url('/fonts/awesome.woff') format('woff'),
           url('/fonts/awesome.ttf') format('truetype'),
           url('/fonts/awesome.eot') format('embedded-opentype');
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: italic;
      font-weight: 400;
      src: local('Awesome Font Italic'),
           url('/fonts/awesome-i.woff2') format('woff2'), 
           url('/fonts/awesome-i.woff') format('woff'),
           url('/fonts/awesome-i.ttf') format('truetype'),
           url('/fonts/awesome-i.eot') format('embedded-opentype');
    }
    

Note: A meno che non faccia riferimento a uno dei font di sistema predefiniti, nella realtà è raro che l'utente l'abbia installato localmente, soprattutto su dispositivi mobili, dove è di fatto impossibile 'installare' font aggiuntivi. Di conseguenza, dovrai sempre fornire un elenco di percorsi font esterni.

* La direttiva `local()` ci consente di riferirci, caricare e utilizzare i font installati localmente.
* La direttiva `url()` dci consente di caricare font esterni, che possono contenere un suggerimento `format()` aggiuntivo che indichi il formato del font cui fa riferimento l'URL fornito.

Quando il browser stabilisce che il font è necessario, scorre l'elenco di risorse fornito nell'ordine specificato e prova a caricare la risorsa corretta. Ad esempio, seguendo l'esempio precedente:

La combinazione di direttive interne ed esterne con format hint idonei ci consente di specificare tutti i formati font disponibili e lasciare al browser la scelta: il browser definisce le risorse necessarie e seleziona il formato ottimale per nostro conto.

1. Il browser esegue il layout di pagina e stabilisce quali varianti del font sono necessarie per il rendering del testo della pagina.
2. Per ogni font richiesto, il browser verifica se è disponibile localmente.
3. In caso contrario, il browser fa riferimento a definizioni esterne: 
    * Se è presente un format hint, il browser verifica se è supportato prima di avviare il download, altrimenti passa al successivo.
    * Se non è presente alcun format hint, il browser scarica la risorsa.

Note: L'ordine di specifica delle varianti del font ha una data importanza. Il browser sceglierà il primo formato supportato. Di conseguenza, se desideri che i browser più recenti utilizzino WOFF2, dovrai posizionare il riferimento WOFF2 sopra WOFF, e così via.

Oltre alle proprietà del font, quali stile, spessore ed estensione, la regola @font-face ci consente di definire un insieme di codepoin Unicode supportati da ogni risorsa. Questo ci consente di suddividere un font Unicode vasto in subset più piccoli (ad es. latino, cirillico, greco) e scaricare soltanto i glifi necessari per il rendering del testo di una data pagina.

### Subset Unicode-range

L'[unicode-range descriptor](http://www.w3.org/TR/css3-fonts/#descdef-unicode-range) ci consente di specificare un elenco delimitato da virgole di valori di range, ciascuno dei quali può collocarsi in uno dei tre gruppi seguenti:

Ad esempio, possiamo suddividere la nostra famiglia *Awesome Font* nei subset Latino e Giapponese, ciascuno dei quali verrà scaricato dal browser in base alla necessità:

* Codepoint singolo (ad es. U+416)
* Range di intervallo (ad es. U+400-4ff): indica il codepoint iniziale e quello finale di una serie
* Range wildcard (ad es. U+4??): `?` caratteri indica una cifra esadecimale

Note: I subset unicode-range sono particolarmente importanti per le lingue asiatiche, il cui numero di glifi è molto più elevato rispetto alle lingue occidentali e un tipico font "intero" è spesso misurato in megabyte, invece che in decine di kilobyte!

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('truetype'),
           url('/fonts/awesome-l.eot') format('embedded-opentype');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-jp.woff2') format('woff2'), 
           url('/fonts/awesome-jp.woff') format('woff'),
           url('/fonts/awesome-jp.ttf') format('truetype'),
           url('/fonts/awesome-jp.eot') format('embedded-opentype');
      unicode-range: U+3000-9FFF, U+ff??; /* Japanese glyphs */
    }
    

L'utilizzo di subset unicode-range e di file distinti per ogni variante stilistica del font ci consente di definire una famiglia di font composita più rapida ed efficiente da scaricare; il visitatore dovrà infatti scaricare soltanto le varianti e i subset necessari, senza essere obbligato a scaricare subset che potrebbe non utilizzare mai né visualizzare sulla pagina.

Detto ciò, unicode-range ha un piccolo difetto: [non tutti i browser lo supportano](http://caniuse.com/#feat=font-unicode-range), o almeno, non ancora. Alcuni browser ignorano semplicemente l'hint unicode-range e scaricano tutte le varianti, mentre altri non elaborano proprio la dichiarazione @font-face. Per risolvere tutto ciò, dobbiamo eseguire un "subsetting manuale" nelle vecchie versioni dei browser.

Poiché i vecchi browser non sono abbastanza intelligenti per selezionare automaticamente i subset necessari, né di costruire un font composito, dobbiamo fornire un unico font che contenga tutti i subset necessari, nascondendo il resto al browser. Ad esempio, se la pagina utilizza soltanto caratteri latini, possiamo eliminare gli altri glifi, fornendo tale subset come unica risorsa.

Ogni famiglia di font è composta da diverse varianti stilistiche (normale, grassetto, corsivo) e diversi spessori per ogni stile, ciascuno dei quali può contenere glifi di forme molto diverse, ad esempio con spaziature, dimensioni o forme diverse.

1. **Come possiamo stabilire quali sono i subset necessari?** 
    * Se il browser supporta unicode-range, allora selezionerà automaticamente il subset giusto. La pagina dovrà soltanto fornire i file di subset e specificare gli unicode-range applicabili nelle proprietà @font-face.
    * Se unicode-range non è supportato, allora la pagina dovrà nascondere tutti i subset non necessari, ovvero lo sviluppatore dovrà specificare i subset necessari.
2. **Come facciamo a generare un subset di font?** 
    * Utilizza lo [strumento open-source pyftsubset](https://github.com/behdad/fonttools/) per suddividere in subset ed ottimizzare i tuoi font.
    * Per alcuni font è consentito il subsetting manuale tramite parametri query personalizzati, utilizzabili per specificare manualmente il subset necessario per la pagina. Consulta la documentazione del tuo font provider.

### Selezione dei font e sintesi

Each font family is composed of multiple stylistic variants (regular, bold, italic) and multiple weights for each style, each of which, in turn, may contain very different glyph shapes&mdash;for example, different spacing, sizing, or a different shape altogether.

<img src="images/font-weights.png"  alt="Font weights" />

Un'operazione logica simile si applica alle varianti *italic*. Il font designer controlla quali varianti produrrà, mentre noi verifichiamo le varianti che useremo sulla pagina. Visto che ogni variante prevede un download separato, è buona norma scaricarne un numero esiguo! Ad esempio, possiamo definire due varianti di grassetto per la nostra famiglia *Awesome Font*:

> Quando è specificato uno spessore per cui non esiste font-face, ne verrà utilizzata una con spessore simile. In generale, i caratteri in grassetto vengono associati a font-face con spessore maggiore e quelli normali a font face con spessore minore.
> 
> > [Algoritmo di corrispondenza dei font CSS3](http://www.w3.org/TR/css3-fonts/#font-matching-algorithm)

Nell'esempio precedente, la famiglia *Awesome Font* risulta composta da due risorse che coprono il medesimo insieme di glifi latini (U+000-5FF), ma offre due "spessori" diversi: normale (400) e grassetto (700). Tuttavia, che cosa accade se una delle nostre regole CSS specifica uno spessore diverso o definisce la proprietà font-style come corsivo?

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('truetype'),
           url('/fonts/awesome-l.eot') format('embedded-opentype');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 700;
      src: local('Awesome Font'),
           url('/fonts/awesome-l-700.woff2') format('woff2'), 
           url('/fonts/awesome-l-700.woff') format('woff'),
           url('/fonts/awesome-l-700.ttf') format('truetype'),
           url('/fonts/awesome-l-700.eot') format('embedded-opentype');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    

The above example declares the *Awesome Font* family that is composed of two resources that cover the same set of Latin glyphs (`U+000-5FF`) but offer two different "weights": normal (400) and bold (700). However, what happens if one of your CSS rules specifies a different font weight, or sets the font-style property to italic?

* Se non è disponibile un font corrispondente, il browser lo sostituisce con quello più simile.
* Se non viene trovata alcuna corrispondenza stilistica (nell'esempio precedente, non dichiariamo alcuna variante corsiva), il browser sintetizzerà la propria variante.

<img src="images/font-synthesis.png"  alt="Font synthesis" />

Note: Per una maggiore coerenza nei risultati visivi, non affidarti alla font-synthesis. Minimizza invece il numero di varianti del font utilizzate e specificane il percorso, in modo che il browser possa scaricarle quando sono utilizzate nella pagina. Detto ciò, in alcuni casi una variante synthesized [può rappresentare un'opzione valida](https://www.igvita.com/2014/09/16/optimizing-webfont-selection-and-synthesis/) se utilizzala con cautela.

Un font web "completo" che includa tutte le varianti stilistiche, che potrebbero non servirci, nonché tutti i glifi, che potremmo non utilizzare mai, può comportare un download di diversi megabyte. Per risolvere tale problema, la regola CSS @font-face è finalizzata specificatamente a consentire la suddivisione della famiglia di font in una raccolta di risorse: subset unicode, varianti di stile distinte, e così via.

Stabilito ciò il browser definisce i subset e varianti necessarie e scarica l'insieme minimo richiesto per il rendering del testo, che  
risulta particolarmente conveniente. Tuttavia, se non facciamo attenzione, può anche creare un collo di bottiglia nel percorso di rendering critico ritardando il rendering del testo.

## Ottimizzazione di caricamento e rendering

### TL;DR {: .hide-from-toc }

* Le richieste di font vengono ritardate sino alla costruzione dell'albero di rendering; ciò può comportare un ritardo nel rendering del testo
* Il Font Loading API ci consente di implementare una strategia di caricamento e rendering dei font personalizzata al posto della predefinita di caricamento lazy

Il caricamento lazy di un font implica un aspetto importante che può ritardare il rendering del testo: il browser deve [costruire l'albero di rendering ](/web/fundamentals/performance/critical-rendering-path/render-tree-construction), che dipende a sua volta dagli alberi DOM e CSSOM, per poter capire quali risorse saranno necessarie per il rendering. Di conseguenza, le richieste di font sono ritardate dopo altre risorse critiche, e il browser può bloccare il rendering del testo fino al recupero della risorsa.

Given these declarations, the browser figures out the required subsets and variants and downloads the minimal set required to render the text, which is very convenient. However, if you're not careful, it can also create a performance bottleneck in the critical rendering path and delay text rendering.

### Font web e percorso di rendering critico

La "competizione" tra la prima rappresentazione dei contenuti della pagina, che può essere realizzata subito dopo la creazione dell'albero di rendering, e la richiesta di risorse font, è quella che crea il "problema del testo blank" in cui il browser deve renderizzare il layout della pagina ma omette qualsiasi testo. Il risultato differisce tra i diversi browser:

<img src="images/font-crp.png"  alt="Font critical rendering path" />

1. Il browser richiede il documento HTML
2. Il browser inizia a scansionare la risposta HTML e a costruire il DOM
3. Il browser scopre risorse CSS, JS e di altro tipo e indirizza le richieste
4. Il browser crea il CSSOM una volta ricevuto tutto il contenuto CSS e lo combina con l'albero DOM per costruire l'albero di rendering 
    * Le richieste di font vengono inviate dopo che l'albero di rendering ha indicato quali varianti dei font sono necessarie per effettuare il rendere di un testo specificato nella pagina.
5. Il browser definisce il layout e disegna i contenuti sullo schermo 
    * Se il font non è ancora disponibile, il browser potrebbe non renderizzare alcun pixel di testo
    * Non appena il font è disponibile, il browser inserisce i pixel di testo

[Font Loading API](http://dev.w3.org/csswg/css-font-loading/) offre un'interfaccia di scripting per definire e manipolare le font face CSS, tracciarne il download e sostituire il loro comportamento di caricamento lazy. Ad esempio, se siamo sicuri che sarà necessaria una specifica variante del font, possiamo definirla e comunicare al browser di iniziare immediatamente a recuperare la risorsa:

Inoltre, potendo verificare lo stato del font (tramite [check()](http://dev.w3.org/csswg/css-font-loading/#font-face-set-check)) e tracciarne il download, possiamo anche definire una strategia personalizzata per il rendering del testo sulle nostre pagine:

### Ottimizzazione del rendering del font con Font Loading API

La cosa migliore è mixare e combinare le strategie precedenti per i diversi contenuti della pagina; ad esempio, bloccare il rendering di alcune sezioni fino a quando il font non è disponibile, utilizzare un font alternativo e poi renderizzarlo una volta terminato il download del font, specificare timeout diversi, e così via.

Note: Font Loading API è ancora <a href='http://caniuse.com/#feat=font-loading'>in fase di sviluppo in alcuni browser</a>. Valuta l'utilizzo del 
<a href='https://github.com/bramstein/fontloader'>FontLoader polyfill</a> o della 
<a href='https://github.com/typekit/webfontloader'>webfontloader library</a> per ottenere funzionalità simili, sebbene con un'ulteriore dipendenza da JavaScript.

Una semplice strategia alternativa per utilizzare Font Loading API consiste nell'eliminare il "problema del testo blank" con priorità alta, poiché sono necessari per costruire il CSSOM.

```html
var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
  style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
});

// don't wait for the render tree, initiate an immediate fetch!
font.load().then(function() {
  // apply the font (which may re-render text and cause a page reflow)
  // after the font has finished downloading
  document.fonts.add(font);
  document.body.style.fontFamily = "Awesome Font, serif";

  // OR... by default the content is hidden, 
  // and it's rendered after the font is available
  var content = document.getElementById("content");
  content.style.visibility = "visible";

  // OR... apply your own render strategy here... 
});
```

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

La strategia di inlining non è flessibile e non ci consente di definire alcun timeout personalizzato o strategia di rendering per contenuti diversi, ma rappresenta comunque una soluzione semplice ed efficace in tutti i browser. Per ottenere migliori risultati, separa i font sottoposti ad inlining in stylesheet separati e imposta una proprietà max-age lunga; in tal modo, all'aggiornamento del CSS, non obbligherai i visitatori a riscaricare i font.

Note: Utilizza l'inlining con criterio selettivo! Ricorda che @font-face utilizza il caricamento lazy per evitare di scaricare varianti e subset di font non necessari. Inoltre, l'aumento delle dimensioni del tuo CSS con un inlining aggressivo avrà un impatto negativo sul <a href='/web/fundamentals/performance/critical-rendering-path/'>percorso di rendering critico</a> - il browser deve scaricare l'intero CSS prima di costruire il CSSOM e l'albero di rendering e visualizzare i contenuti della pagina sullo schermo.

### Ottimizzazione del rendering con inlining

Le risorse di tipo font sono, tipicamente, statiche e non hanno aggiornamenti frequenti. Di conseguenza, sono ideali per una scadenza max-age lunga; assicurati di specificare sia una [intestazione ETag condizionale](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), sia una [policy di Cache-Control ottimale](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) per tutti i font.

#### Browser behaviors

Non è necessario immagazzinare i font nel localStorage o attraverso altri meccanismi; ciascuno di essi dispone del proprio insieme di problemi di prestazioni. La cache HTTP del browser, in combinazione con Font Loading API o con la libreria webfontloader, offre il meccanismo migliore e più efficace per inviare dei font al browser.

<table>
  <thead>
    <tr>
      <th data-th="Browser">Browser</th>
      <th data-th="Timeout">Timeout</th>
      <th data-th="Fallback">Fallback</th>
      <th data-th="Swap">Swap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Browser">
        <strong>Chrome 35+</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Opera</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Firefox</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Internet Explorer</strong>
      </td>
      <td data-th="Timeout">
        0 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Safari</strong>
      </td>
      <td data-th="Timeout">
        No timeout
      </td>
      <td data-th="Fallback">
        N/A
      </td>
      <td data-th="Swap">
        N/A
      </td>
    </tr>
  </tbody>
</table>

* Safari effettua il rendering del testo soltanto una volta completato il download del font.
* Chrome e Firefox bloccano il rendering per 3 secondi, dopo i quali utilizzano un font alternativo e, una volta scaricato il font, renderizzano nuovamente il testo in base ad esso.
* IE esegue immediatamente il rendering con il font alternativo se quello richiesto non è ancora disponibile, per poi eseguire nuovamente il rendering una volta completato il download.

Contrariamente a quanto si crede, l'utilizzo di font web non ritarda necessariamente il rendering della pagina né ha un impatto negativo su altre prestazioni. Un utilizzo dei font ottimizzato può garantire all'utente un'esperienza migliore sotto ogni punto di vista: branding perfetto, migliore leggibilità, fruibilità e ricercabilità, offrendo al contempo una risoluzione multipla scalabile che ben si adatta a qualsiasi formato e risoluzione del monitor. Non temere di utilizzare i font web.

#### The font display timeline

Certo, un'applicazione non ponderata può causare ritardi evitabili e download eccessivamente pesanti. È per questo che dobbiamo svecchiare i nostri strumenti di ottimizzazione e aiutare il browser ottimizzando i font e il modo in cui vengono recuperati ed utilizzati all'interno delle nostre pagine.

1. The first period is the **font block period**. During this period, if the font face is not loaded, any element attempting to use it must instead render with an invisible fallback font face. If the font face successfully loads during the block period, the font face is then used normally.
2. The **font swap period** occurs immediately after the font block period. During this period, if the font face is not loaded, any element attempting to use it must instead render with a fallback font face. If the font face successfully loads during the swap period, the font face is then used normally.
3. The **font failure period** occurs immediately after the font swap period. If the font face is not yet loaded when this period starts, it’s marked as a failed load, causing normal font fallback. Otherwise, the font face is used normally.

Understanding these periods means you can use `font-display` to decide how your font should render depending on whether or when it was downloaded.

#### Using font-display

To work with the `font-display` property, add it your `@font-face` rules:

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  font-display: auto; /* or block, swap, fallback, optional */
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

`font-display` currently supports the following range of values: `auto | block | swap | fallback | optional`.

* **`auto`** uses whatever font display strategy the user-agent uses. Most browsers currently have a default strategy similar to `block`.

* **`block`** gives the font face a short block period (3s is recommended in most cases) and an infinite swap period. In other words, the browser draws "invisible" text at first if the font is not loaded, but swaps the font face in as soon as it loads. To do this the browser creates an anonymous font face with metrics similar to the selected font but with all glyphs containing no "ink." This value should only be used if rendering text in a particular typeface is required for the page to be usable.

* **`swap`** gives the font face a zero second block period and an infinite swap period. This means the browser draws text immediately with a fallback if the font face isn’t loaded, but swaps the font face in as soon as it loads. Similar to `block`, this value should only be used when rendering text in a particular font is important for the page, but rendering in any font will still get a correct message across. Logo text is a good candidate for **swap** since displaying a company’s name using a reasonable fallback will get the message across but you’d eventually use the official typeface.

* **`fallback`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a short swap period (three seconds is recommended in most cases). In other words, the font face is rendered with a fallback at first if it’s not loaded, but the font is swapped as soon as it loads. However, if too much time passes, the fallback will be used for the rest of the page’s lifetime. `fallback` is a good candidate for things like body text where you’d like the user to start reading as soon as possible and don’t want to disturb their experience by shifting text around as a new font loads in.

* **`optional`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a zero second swap period. Similar to `fallback`, this is a good choice for when the downloading font is more of a "nice to have" but not critical to the experience. The `optional` value leaves it up to the browser to decide whether to initiate the font download, which it may choose not to do or it may do it as a low priority depending on what it thinks would be best for the user. This can be beneficial in situations where the user is on a weak connection and pulling down a font may not be the best use of resources.

`font-display` is [gaining adoption](https://caniuse.com/#feat=css-font-rendering-controls) in many modern browsers. You can look forward to consistency in browser behavior as it becomes widely implemented.

### Ottimizzazione per il riutilizzo con HTTP Caching

Used together, `<link rel="preload">` and the CSS `font-display` give developers a great deal of control over font loading and rendering, without adding in much overhead. But if you need additional customizations, and are willing to incur with the overhead introduced by running JavaScript, there is another option.

The [Font Loading API](https://www.w3.org/TR/css-font-loading/) provides a scripting interface to define and manipulate CSS font faces, track their download progress, and override their default lazyload behavior. For example, if you're sure that a particular font variant is required, you can define it and tell the browser to initiate an immediate fetch of the font resource:

    var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
      style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
    });
    
    // don't wait for the render tree, initiate an immediate fetch!
    font.load().then(function() {
      // apply the font (which may re-render text and cause a page reflow)
      // after the font has finished downloading
      document.fonts.add(font);
      document.body.style.fontFamily = "Awesome Font, serif";
    
      // OR... by default the content is hidden, 
      // and it's rendered after the font is available
      var content = document.getElementById("content");
      content.style.visibility = "visible";
    
      // OR... apply your own render strategy here... 
    });
    

Further, because you can check the font status (via the [check()](https://www.w3.org/TR/css-font-loading/#font-face-set-check)) method and track its download progress, you can also define a custom strategy for rendering text on your pages:

* Gli stylesheet CSS come media query corrispondenti sono scaricati automaticamente dal browser con priorità alta, essendo necessari per costruire il CSSOM.
* L'inlining dei dati del font nello stylesheet CSS obbliga il browser a scaricare il font senza priorità alta e senza attendere l'albero di rendering, agendo quindi come override manuale sul comportamento di caricamento lazy predefinito.
* You can use the fallback font to unblock rendering and inject a new style that uses the desired font after the font is available.

Best of all, you can also mix and match the above strategies for different content on the page. For example, you can delay text rendering on some sections until the font is available, use a fallback font, and then re-render after the font download has finished, specify different timeouts, and so on.

Note: The Font Loading API is still [under development in some browsers](http://caniuse.com/#feat=font-loading). Consider using the [FontLoader polyfill](https://github.com/bramstein/fontloader) or the [webfontloader library](https://github.com/typekit/webfontloader) to deliver similar functionality, albeit with even more overhead from an additional JavaScript dependency.

### Proper caching is a must

Font resources are, typically, static resources that don't see frequent updates. As a result, they are ideally suited for a long max-age expiry - ensure that you specify both a [conditional ETag header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), and an [optimal Cache-Control policy](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) for all font resources.

If your web application uses a [service worker](/web/fundamentals/primers/service-workers/), serving font resources with a [cache-first strategy](/web/fundamentals/instant-and-offline/offline-cookbook/#cache-then-network) is appropriate for most use cases.

You should not store fonts using [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) or [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API); each of those has its own set of performance issues. The browser's HTTP cache provides the best and most robust mechanism to deliver font resources to the browser.

## Checklist di ottimizzazione

Contrary to popular belief, the use of webfonts doesn't need to delay page rendering or have a negative impact on other performance metrics. The well-optimized use of fonts can deliver a much better overall user experience: great branding, improved readability, usability, and searchability, all while delivering a scalable multi-resolution solution that adapts well to all screen formats and resolutions. Don't be afraid to use webfonts.

That said, a naive implementation may incur large downloads and unnecessary delays. You need to help the browser by optimizing the font assets themselves and how they are fetched and used on your pages.

* **Verifica e controlla il tuo utilizzo dei font:** non utilizzare troppi font sulle tue pagine e, per ogni font, minimizza il numero di varianti utilizzate. Ciò ti consentirà di offrire ai tuoi utenti un'esperienza più rapida e uniforme.
* **Suddividi i font in subset:** molti font possono essere suddivisi in subset o in più unicode-range, così da utilizzare soltanto i glifi richiesti da una pagina specifica - così facendo, le dimensioni file si riducono, mentre la velocità di download della risorsa aumenta. Tuttavia, nella definizione dei subset, fa attenzione ad ottimizzarli per il riutilizzo dei font; ad es., per ogni pagina non scaricare un set di caratteri diverso ma sovrapposto. È buona norma creare subset in base allo script, ad es. latino, cirillico, e così via.
* **Ottimizza il formato dei font per ogni browser:** ogni font dovrebbe essere fornito in formato WOFF2, WOFF, EOT e TTF. Assicurati di comprimere con GZIP i formati EOT e TTF, non compressi normalmente.
* **Specifica le politiche ottimali di validazione e caching:** i font sono risorse statiche che vengono aggiornate molto raramente. Assicurati che i tuoi server prevedano un timestamp max-age di lunga durata e un token di riconvalida per consentire un riutilizzo efficace tra pagine diverse.
* **Utilizza FontLoading API per ottimizzare il percorso di rendering critico:** il caricamento lazy predefinito può comportare un ritardo nel rendering del testo. Font Loading API ci consente di evitarlo per determinati font, specificando una strategia di rendering e timeout personalizzata in base ai diversi contenuti della pagina. Per le versioni più vecchie dei browser che non supportano l'API, puoi utilizzare la libreria webfontloader JavaScript o la strategia di inlining del CSS.
* **Specify revalidation and optimal caching policies:** fonts are static resources that are infrequently updated. Make sure that your servers provide a long-lived max-age timestamp and a revalidation token to allow for efficient font reuse between different pages. If using a service worker, a cache-first strategy is appropriate.

## Automated testing for web font optimization with Lighthouse {: #lighthouse }

[Lighthouse](/web/tools/lighthouse) can help automate the process of making sure that you're following web font optimization best practices. Lighthouse is an auditing tool built by the Chrome DevTools team. You can run it as a Node module, from the command line, or from the Audits panel of Chrome DevTools. You tell Lighthouse what URL to audit, and then it runs a bunch of tests on the page, and gives you a report of what the page is doing well, and how it can improve.

The following audits can help you make sure that your pages are continuing to follow web font optimization best practices over time:

* [Enable text compression](/web/tools/lighthouse/audits/text-compression)
* [Preload key requests](/web/tools/lighthouse/audits/preload)
* [Uses inefficient cache policy on static assets](/web/tools/lighthouse/audits/cache-policy)
* [All text remains visible during webfont loads](/web/updates/2016/02/font-display)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}