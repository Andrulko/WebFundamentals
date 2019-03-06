project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Un'immagine vale più di mille parole e ricopre un ruolo chiave per tutte le pagine. Tuttavia, le immagini richiedono il download di numerosi dati. Il Web design reactive consente di modificare disposizione e immagini in base alle caratteristiche del dispositivo.

{# wf_updated_on: 2018-12-15 #} {# wf_published_on: 2000-01-01 #}

# Immagini {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Un'immagine vale più di mille parole e ricopre un ruolo chiave in ogni pagina. Tuttavia spesso rappresenta anche la maggior parte dei byte scaricati. Con il Web design reactive, non solo il layout ma anche le immagini possono adattarsi alle caratteristiche del dispositivo.

## Immagini nel markup

<img src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Other times the image may need to be changed more drastically: changing the proportions, cropping, and even replacing the entire image. In this case, changing the image is usually referred to as art direction. See [responsiveimages.org/demos/](https://responsiveimages.org/demos/) for more examples.

In altre circostanze potrebbe essere necessario modificare drasticamente l'immagine, come ad esempio ridimensionarla, ritagliarla o persino sostituirla. In questi casi, le modifiche all'immagine vengono definite 'direzione rtistica'. Consulta [responsiveimages.org/demos/](http://responsiveimages.org/demos/){: .external } per ulteriori esempi.

## Immagini in CSS 

<style>
  .side-by-side {
    display: inline-block;
    margin: 0 20px 0 0;
    width: 45%;
  }

  span#data_uri {
    background: url(data:image/svg+xml,%3C%3Fxml%20version%3D%221.0%22%20encoding%3D%22utf-8%22%3F%3E%0D%0A%3C%21--%20Generator%3A%20Adobe%20Illustrator%2016.0.0%2C%20SVG%20Export%20Plug-In%20.%20SVG%20Version%3A%206.00%20Build%200%29%20%20--%3E%0D%0A%3C%21DOCTYPE%20svg%20PUBLIC%20%22-%2F%2FW3C%2F%2FDTD%20SVG%201.1%2F%2FEN%22%20%22http%3A%2F%2Fwww.w3.org%2FGraphics%2FSVG%2F1.1%2FDTD%2Fsvg11.dtd%22%3E%0D%0A%3Csvg%20version%3D%221.1%22%20id%3D%22Layer_1%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20xmlns%3Axlink%3D%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%22%20x%3D%220px%22%20y%3D%220px%22%0D%0A%09%20width%3D%22396.74px%22%20height%3D%22560px%22%20viewBox%3D%22281.63%200%20396.74%20560%22%20enable-background%3D%22new%20281.63%200%20396.74%20560%22%20xml%3Aspace%3D%22preserve%22%0D%0A%09%3E%0D%0A%3Cg%3E%0D%0A%09%3Cg%3E%0D%0A%09%09%3Cg%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23E44D26%22%20points%3D%22409.737%2C242.502%20414.276%2C293.362%20479.828%2C293.362%20480%2C293.362%20480%2C242.502%20479.828%2C242.502%20%09%09%09%0D%0A%09%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpath%20fill%3D%22%23E44D26%22%20d%3D%22M281.63%2C110.053l36.106%2C404.968L479.757%2C560l162.47-45.042l36.144-404.905H281.63z%20M611.283%2C489.176%0D%0A%09%09%09%09L480%2C525.572V474.03l-0.229%2C0.063L378.031%2C445.85l-6.958-77.985h22.98h26.879l3.536%2C39.612l55.315%2C14.937l0.046-0.013v-0.004%0D%0A%09%09%09%09L480%2C422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283%2C489.176z%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23F16529%22%20points%3D%22480%2C192.833%20604.247%2C192.833%20603.059%2C206.159%20600.796%2C231.338%20599.8%2C242.502%20599.64%2C242.502%20%0D%0A%09%09%09%09480%2C242.502%20480%2C293.362%20581.896%2C293.362%20595.28%2C293.362%20594.068%2C306.699%20582.396%2C437.458%20581.649%2C445.85%20480%2C474.021%20%0D%0A%09%09%09%09480%2C474.03%20480%2C525.572%20611.283%2C489.176%20642.17%2C143.166%20480%2C143.166%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23F16529%22%20points%3D%22540.988%2C343.029%20480%2C343.029%20480%2C422.35%20535.224%2C407.445%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23EBEBEB%22%20points%3D%22414.276%2C293.362%20409.737%2C242.502%20479.828%2C242.502%20479.828%2C242.38%20479.828%2C223.682%20%0D%0A%09%09%09%09479.828%2C192.833%20355.457%2C192.833%20356.646%2C206.159%20368.853%2C343.029%20479.828%2C343.029%20479.828%2C293.362%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23EBEBEB%22%20points%3D%22479.828%2C474.069%20479.828%2C422.4%20479.782%2C422.413%20424.467%2C407.477%20420.931%2C367.864%20%0D%0A%09%09%09%09394.052%2C367.864%20371.072%2C367.864%20378.031%2C445.85%20479.771%2C474.094%20480%2C474.03%20480%2C474.021%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22343.784%2C50.229%20366.874%2C50.229%20366.874%2C75.517%20392.114%2C75.517%20392.114%2C0%20366.873%2C0%20366.873%2C24.938%20%0D%0A%09%09%09%09343.783%2C24.938%20343.783%2C0%20318.544%2C0%20318.544%2C75.517%20343.784%2C75.517%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22425.307%2C25.042%20425.307%2C75.517%20450.549%2C75.517%20450.549%2C25.042%20472.779%2C25.042%20472.779%2C0%20403.085%2C0%20%0D%0A%09%09%09%09403.085%2C25.042%20425.306%2C25.042%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22508.537%2C38.086%20525.914%2C64.937%20526.349%2C64.937%20543.714%2C38.086%20543.714%2C75.517%20568.851%2C75.517%20568.851%2C0%20%0D%0A%09%09%09%09542.522%2C0%20526.349%2C26.534%20510.159%2C0%20483.84%2C0%20483.84%2C75.517%20508.537%2C75.517%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22642.156%2C50.555%20606.66%2C50.555%20606.66%2C0%20581.412%2C0%20581.412%2C75.517%20642.156%2C75.517%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23FFFFFF%22%20points%3D%22480%2C474.021%20581.649%2C445.85%20582.396%2C437.458%20594.068%2C306.699%20595.28%2C293.362%20581.896%2C293.362%20%0D%0A%09%09%09%09480%2C293.362%20479.828%2C293.362%20479.828%2C343.029%20480%2C343.029%20540.988%2C343.029%20535.224%2C407.445%20480%2C422.35%20479.828%2C422.396%20%0D%0A%09%09%09%09479.828%2C422.4%20479.828%2C474.069%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23FFFFFF%22%20points%3D%22479.828%2C242.38%20479.828%2C242.502%20480%2C242.502%20599.64%2C242.502%20599.8%2C242.502%20600.796%2C231.338%20%0D%0A%09%09%09%09603.059%2C206.159%20604.247%2C192.833%20480%2C192.833%20479.828%2C192.833%20479.828%2C223.682%20%09%09%09%22%2F%3E%0D%0A%09%09%3C%2Fg%3E%0D%0A%09%3C%2Fg%3E%0D%0A%3C%2Fg%3E%0D%0A%3C%2Fsvg%3E%0D%0A) no-repeat;
    background-size: cover;
    height: 484px;
  }

  span#svg {
    background: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' version='1.1' x='0px' y='0px' width='50%' height='560px' viewBox='281.63 0 396.74 560' enable-background='new 281.63 0 396.74 560' xml:space='preserve'><g><g><g><polygon fill='#E44D26' points='409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5'/><path fill='#E44D26' d='M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z'/><polygon fill='#F16529' points='480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2'/><polygon fill='#F16529' points='541,343 480,343 480,422.4 535.2,407.4'/><polygon fill='#EBEBEB' points='414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4'/><polygon fill='#EBEBEB' points='479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474'/><polygon points='343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5'/><polygon points='425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25'/><polygon points='508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5'/><polygon points='642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5'/><polygon fill='#FFFFFF' points='480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1'/><polygon fill='#FFFFFF' points='479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7'/></g></g></g></svg>") no-repeat;
    background-size: cover;
    height: 484px;
  }
</style>

 

{% include "web/_shared/udacity/ud882.html" %}

### Immagini reattive

* Utilizza le dimensioni relative delle immagini per evitare l'overflow involontario del contenitore.
* Utilizza l'elemento `picture` per specificare immagini diverse in base alle caratteristiche del dispositivo (operazione detta 'direzione artistica').
* Utilizza `srcset` e il descrittore `x` nell'elemento `img` per indicare al browser l'immagine da utilizzare in presenza di diverse densità.
* If your page only has one or two images and these are not used elsewhere on your site, consider using inline images to reduce file requests.

### Direzione artistica

Il potente elemento`img` consente di scaricare, decodificare e renderizzare i contenuti, mentre i browser odierni supportano numerosi formati di immagine. La procedura di inserimento delle immagini compatibili con diversi dispositivi è simile a quella usata con i computer desktop e richiede minime regolazioni per offrire un'esperienza ottimale.

L'utilizzo delle unità relative per la larghezza delle immagine previene l'overflow involontario del viewport. Ad esempio, con `width: 50%` si ottiene una larghezza dell'immagine pari al 50% dell'elemento contenitore e non della dimensione effettiva dei pixel o del viewport.

    img, embed, object, video {
      max-width: 100%;
    }
    

Poiché CSS consente l'overflow del contenitore dei contenuti, può essere necessario utilizzare `max-width: 100%` per evitare l'overflow di immagini e altri contenuti. Ad esempio:

### TL;DR {: .hide-from-toc }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="Pzc5Dly_jEM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Inserisci descrizioni dettagliate con l'attributo `alt` degli elementi `img` per ottimizzare l'accessibilità del sito e fornire contesto ai lettori di schermo o altre tecnologie assistive.

<div style="clear:both;">
</div>

    <img src="photo.png" srcset="photo@2x.png 2x" ...>
    

L'attributo `srcset` ottimizza l'azione dell'elemento `img` semplificando la fornitura di diversi file di immagine in base alle caratteristiche dei dispositivi. In maniera analoga alla funzione nativa CSS `image-set` <a href='#use_image-set_to_provide_high_res_images'> </a>, `srcset` indica al browser l'immagine ottimale in base alle caratteristiche del dispositivo, ad esempio un'immagine 2x su un display 2x e un'immagine 1x su dispositivi 2x con limiti di larghezza di banda.

I browser che non supportano `srcset` utilizzano il file di immagine predefinito indicato dall'attributo `src`. Per questo motivo fondamentale inserire sempre un'immagine 1x visualizzabile su qualsiasi dispositivo, indipendentemente dalle funzionalità. Se `srcset` è supportato, l'elenco di immagini/condizioni separato da virgole viene analizzato prima dell'esecuzione delle richieste, quindi viene scaricata e visualizzata solo l'immagine più appropriata.

### Utilizzo delle dimensioni relative per le immagini

<img class="attempt-right" src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

La modifica delle immagini in base alle caratteristiche del dispositivo, detta anche direzione artistica, avviene mediante l'elemento `picture`. L'elemento `picture` definisce una soluzione dichiarativa per la fornitura di diverse versioni di un'immagine in base a caratteristiche come dimensioni e risoluzione del dispositivo, orientamento e via dicendo.

<div style="clear:both;">
</div>

Dogfood: The `picture` element is beginning to land in browsers. Although it's not available in every browser yet, we recommend its use because of the strong backward compatibility and potential use of the [Picturefill polyfill](https://scottjehl.github.io/picturefill/). See the [ResponsiveImages.org](http://responsiveimages.org/#implementation) site for further details.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="QINlm3vjnaY"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Note: L'elemento `picture` inizia a essere supportato dai browser. Anche se non è disponibile in tutti i browser, è consigliabile utilizzarlo grazie alla retroattività e alla possibilità di utilizzare una [polilinea Picturefill](https://scottjehl.github.io/picturefill/). Consulta il sito [ResponsiveImages.org](http://responsiveimages.org/#implementation) per maggiori informazioni.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media.html" region_tag="picture"   adjust_indentation="auto" %}
</pre>

Utilizzare l'elemento `picture` quando esiste un'origine dell'immagine a densità multiple o quando una grafica reattiva impone l'utilizzo di un'immagine diversa su alcuni tipi di schermi. Così come avviene per l'elemento `video`, è possibile inserire diversi elementi `source` per specificare diversi file d'immagine in base alle media query o al formato dell'immagine.

Nell'esempio precedente, se la larghezza del browser è di almeno 800 pixel, in base alla risoluzione del dispositivo viene utilizzato `head.jpg` o `head-2x.jpg`. Se la larghezza del browser è compresa fra 450 e 800 pixel, anche in questo caso viene utilizzato `head.jpg` o `head-2x.jpg` a seconda della risoluzione del dispositivo. Con schermi di larghezze inferiori a 450 pixel e retrocompatibilità nei casi in cui l'elemento `picture` non sia supportato, il browser esegue il rendering dell'elemento `img` (che occorre includere sempre).

#### Immagini con dimensioni relative

Se non si conosce la dimensione finale dell'immagine può essere difficile indicare un descrittore di densità per le origini dell'immagine. In particolare, ciò avviene con le immagini fluide e distribuite sulla larghezza proporzionale del browser in base alle dimensioni dello stesso.

Invece di indicare densità e dimensioni fisse dell'immagine, è possibile specificare le dimensioni di ciascuna immagine fornita aggiungendo un descrittore di larghezza contenente la dimensione dell'elemento dell'immagine, consentendo al browser di calcolare la densità di pixel effettiva e scegliere l'immagine migliore da scaricare.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/sizes.html" region_tag="picture" adjust_indentation="auto" %}
</pre>

Nell'esempio precedente viene eseguito il rendering di un'immagine di dimensione pari alla metà della larghezza del viewport (sizes="50vw"), basata sulla larghezza del browser e sulle proporzioni in pixel del dispositivo, consentendo al browser di utilizzare l'immagine corretta indipendentemente dalla larghezza della finestra del browser. Ad esempio, la tabella sottostante indica l'immagine che verrà utilizzata dal browser:

In alcuni casi, le dimensioni o l'immagine potrebbero variare in base ai breakpoint della disposizione del sito. Ad esempio, con schermi di dimensioni ridotte l'immagine potrebbe coprire l'intera larghezza del viewport, mentre su schermi più grandi potrebbe occuparne solo una piccola parte.

<table class="">
  <tr>
    <th data-th="Browser width">
      Larghezza del browser
    </th>
    
    <th data-th="Device pixel ratio">
      Proporzioni in pixel del dispositivo
    </th>
    
    <th data-th="Image used">
      Immagine utilizzata
    </th>
    
    <th data-th="Effective resolution">
      Risoluzione effettiva
    </th>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400 pixel
    </td>
    
    <td data-th="Device pixel ratio">
      1
    </td>
    
    <td data-th="Image used">
      <code>200.png</code>
    </td>
    
    <td data-th="Effective resolution">
      1x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400 pixel
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      320px
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2.5x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      600px
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>800.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2,67x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      640 px
    </td>
    
    <td data-th="Device pixel ratio">
      3
    </td>
    
    <td data-th="Image used">
      <code>1000.png</code>
    </td>
    
    <td data-th="Effective resolution">
      3,125x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      1100px
    </td>
    
    <td data-th="Device pixel ratio">
      1
    </td>
    
    <td data-th="Image used">
      <code>1400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      1.27x
    </td>
  </tr>
</table>

#### Attenzione ai breakpoint nelle immagini reattive

L'attributo `size` dell'esempio precedente utilizza diverse media query per indicare le dimensioni dell'immagine. Se la larghezza del browser è superiore a 600 pixel, l'immagine occuperà il 25% della larghezza del viewport, mentre con una larghezza compresa fra 500 e 600 pixel l'immagine occuperà il 50% della larghezza del viewport. Infine, con una larghezza inferiore a 500 pixel l'immagine apparirà a larghezza piena.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/breakpoints.html" region_tag="picture" adjust_indentation="auto" %}
</pre>

I clienti desiderano visualizzare al meglio gli articoli da acquistare. Nei siti di vendita al dettaglio, gli utenti devono poter visualizzare ingrandimenti ad alta risoluzione dei prodotti per analizzarne i dettagli e i [partecipanti allo studio](/web/fundamentals/getting-started/principles/) non gradirebbero certo l'impossibilità di utilizzare questa funzione.

Il sito di J. Crews è un buon esempio di immagini interattive e dotate di funzione di zoom. L'overlay a scomparsa indica la possibilità di analizzare dell'immagine mediante una visualizzazione ingrandita e nitida.

### Ottimizzazione di `img` con `srcset` per dispositivi a DPI elevati<figure class="attempt-right"> 

<img src="img/sw-make-images-expandable-good.png" srcset="img/sw-make-images-expandable-good.png 1x, img/sw-make-images-expandable-good-2x.png 2x" alt="Sito web di J. Crews con
  immagini dei prodotti dotate di funzione di zoom" /> <figcaption>Sito web di J. Crews con immagini dei prodotti dotate di funzione di zoom</figcaption> </figure> 

Customers want to see what they're buying. On retail sites, users expect to be able to view high resolution closeups of products to get a better look at details, and [study participants](/web/fundamentals/getting-started/principles/#make-product-images-expandable) got frustrated if they weren't able to.

La [tecnica di compressione delle immagini](http://www.html5rocks.com/en/mobile/high-dpi/#toc-tech-overview) invia un'immagine 2x a elevata compressione a tutti i dispositivi, indipendentemente dalle funzionalità. In base al tipo d'immagine e al livello di compressione, la qualità dell'immagine potrebbe restare inalterata anche con una significativa riduzione delle dimensioni del file.

<div style="clear:both;">
</div>

### Direzione artistica nelle immagini reactive con `picture`

#### Compressione delle immagini

The [compressive image technique](http://www.html5rocks.com/en/mobile/high-dpi/#toc-tech-overview) serves a highly compressed 2x image to all devices, no matter the actual capabilities of the device. Depending on the type of image and level of compression, image quality may not appear to change, but the file size drops significantly.

Note: Utilizza con parsimonia le tecniche di compressione, poiché aumentano i costi in termini di decodifica e memoria. Il ridimensionamento delle immagini di grandi dimensioni per gli schermi di dimensioni ridotte è un'attività costosa che riduce le prestazioni dei dispositivi di fascia bassa con limiti di memoria e di capacità di calcolo.

La sostituzione dell'immagine JavaScript verifica le funzionalità del dispositivo ed esegue l'operazione in maniera ottimale. È possibile determinare la proporzione dei pixel del dispositivo con `window.devicePixelRatio`, ottenere altezza e larghezza dello schermo ed eseguire lo sniffing della connessione di rete con `navigator.connection` o una richiesta fasulla. Una volta raccolte queste informazioni è possibile scegliere l'immagine da caricare.

#### Sostituzione dell'immagine JavaScript

L'approccio è caratterizzato da un aspetto negativo: l'utilizzo di JavaScript rallenta il caricamento dell'immagine fino alla conclusione dell'attività del parser look-ahead e il download delle immagini non avrà luogo prima dell'attivazione dell'evento `pageload`. Inoltre, il browser scarica l'immagine 1x e 2x causando un aumento del peso della pagina.

La proprietà 'background' CSS è un potente strumento per l'aggiunta di immagini complesse agli elementi in modo da semplificare l'inserimento di immagini multiple, la ripetizione modulare delle stesse e molto altro. Se utilizzata con le media query, la proprietà background è ancor più utile grazie alla possibilità di eseguire il caricamento condizionale delle immagini in base a risoluzione dello schermo, dimensioni del viewport e così via.

#### Inlining images: raster and vector

Le media query influiscono sulla disposizione della pagina e consentono anche il caricamento condizionale delle immagini o la gestione della direzione artistica in base alla larghezza del viewport.

Nell'esempio sottostante, con schermi di dimensioni ridotte viene scaricato e applicato solo `small.png` al contenuto `div`, mentre con schermi di dimensioni elevate `background-image: url(body.png)` viene applicato al corpo e `background-image: url(large.png)` al `div` del contenuto.

La funzione `image-set()` dei CSS ottimizza la proprietà di comportamento `background` in modo da semplificare la fornitura di file di immagini multipli in base alle diverse caratteristiche dei dispositivi. In questo modo, il browser seleziona l'immagine ottimale in base alle caratteristiche del dispositivo, ad esempio un'immagine 2x su un display 2x o un'immagine 1x su un dispositivo 2x con larghezza di banda limitata.

##### SVG

Oltre al caricamento dell'immagine appropriata, il browser ne esegue un ridimensionamento adeguato. In altri termini, il browser suppone che le immagini 2x abbiano larghezza doppia rispetto a quelle 1x e ridimensiona l'immagine 2x riducendola di 2 volte, in modo che venga visualizzata nella pagina con la stesse dimensioni.

Il supporto di `image-set()` non è ancora diffuso ed è disponibile solo su Chrome e Safari usando il prefisso del fornitore `-webkit`. Attenzione a includere un'immagine alternativa in caso di assenza di supporto di `image-set()`. Ad esempio:

<img class="side-by-side" src="img/html5.png" alt="HTML5 logo, PNG format" /> <img class="side-by-side" src="img/html5.svg" alt="HTML5 logo, SVG format" />

Le media query possono creare regole basate sulle [proporzioni dei pixel del dispositivo](http://www.html5rocks.com/en/mobile/high-dpi/#toc-bg) per indicare diverse immagini per la visualizzazione 2x o 1x.

<img class="side-by-side" src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiB
      BZG9iZSBJbGx1c3RyYXRvciAxNi4wLjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW
      9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RUR
      CBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2
      ZzExLmR0ZCI+DQo8c3ZnIHZlcnNpb249IjEuMSIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8
      vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OT
      kveGxpbmsiIHg9IjBweCIgeT0iMHB4Ig0KCSB3aWR0aD0iMzk2Ljc0cHgiIGhlaWdodD0iNTYwc
      HgiIHZpZXdCb3g9IjI4MS42MyAwIDM5Ni43NCA1NjAiIGVuYWJsZS1iYWNrZ3JvdW5kPSJuZXcg
      MjgxLjYzIDAgMzk2Ljc0IDU2MCIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSINCgk+DQo8Zz4NCgk8Zz4
      NCgkJPGc+DQoJCQk8cG9seWdvbiBmaWxsPSIjRTQ0RDI2IiBwb2ludHM9IjQwOS43MzcsMjQyLj
      UwMiA0MTQuMjc2LDI5My4zNjIgNDc5LjgyOCwyOTMuMzYyIDQ4MCwyOTMuMzYyIDQ4MCwyNDIuN
      TAyIDQ3OS44MjgsMjQyLjUwMiAJCQkNCgkJCQkiLz4NCgkJCTxwYXRoIGZpbGw9IiNFNDREMjYi
      IGQ9Ik0yODEuNjMsMTEwLjA1M2wzNi4xMDYsNDA0Ljk2OEw0NzkuNzU3LDU2MGwxNjIuNDctNDU
    uMDQybDM2LjE0NC00MDQuOTA1SDI4MS42M3ogTTYxMS4yODMsNDg5LjE3Ng0KCQkJCUw0ODAsNT
    I1LjU3MlY0NzQuMDNsLTAuMjI5LDAuMDYzTDM3OC4wMzEsNDQ1Ljg1bC02Ljk1OC03Ny45ODVoM
    jIuOThoMjYuODc5bDMuNTM2LDM5LjYxMmw1NS4zMTUsMTQuOTM3bDAuMDQ2LTAuMDEzdi0wLjAw
    NA0KCQkJCUw0ODAsNDIyLjM1di03OS4zMmgtMC4xNzJIMzY4Ljg1M2wtMTIuMjA3LTEzNi44NzF
    sLTEuMTg5LTEzLjMyNWgxMjQuMzcxSDQ4MHYtNDkuNjY4aDE2Mi4xN0w2MTEuMjgzLDQ4OS4xNz
    Z6Ii8+DQoJCQk8cG9seWdvbiBmaWxsPSIjRjE2NTI5IiBwb2ludHM9IjQ4MCwxOTIuODMzIDYwN
    C4yNDcsMTkyLjgzMyA2MDMuMDU5LDIwNi4xNTkgNjAwLjc5NiwyMzEuMzM4IDU5OS44LDI0Mi41
    MDIgNTk5LjY0LDI0Mi41MDIgDQoJCQkJNDgwLDI0Mi41MDIgNDgwLDI5My4zNjIgNTgxLjg5Niw
    yOTMuMzYyIDU5NS4yOCwyOTMuMzYyIDU5NC4wNjgsMzA2LjY5OSA1ODIuMzk2LDQzNy40NTggNT
    gxLjY0OSw0NDUuODUgNDgwLDQ3NC4wMjEgDQoJCQkJNDgwLDQ3NC4wMyA0ODAsNTI1LjU3MiA2M
    TEuMjgzLDQ4OS4xNzYgNjQyLjE3LDE0My4xNjYgNDgwLDE0My4xNjYgCQkJIi8+DQoJCQk8cG9s
    eWdvbiBmaWxsPSIjRjE2NTI5IiBwb2ludHM9IjU0MC45ODgsMzQzLjAyOSA0ODAsMzQzLjAyOSA
    0ODAsNDIyLjM1IDUzNS4yMjQsNDA3LjQ0NSAJCQkiLz4NCgkJCTxwb2x5Z29uIGZpbGw9IiNFQk
    VCRUIiIHBvaW50cz0iNDE0LjI3NiwyOTMuMzYyIDQwOS43MzcsMjQyLjUwMiA0NzkuODI4LDI0M
    i41MDIgNDc5LjgyOCwyNDIuMzggNDc5LjgyOCwyMjMuNjgyIA0KCQkJCTQ3OS44MjgsMTkyLjgz
    MyAzNTUuNDU3LDE5Mi44MzMgMzU2LjY0NiwyMDYuMTU5IDM2OC44NTMsMzQzLjAyOSA0NzkuODI
    4LDM0My4wMjkgNDc5LjgyOCwyOTMuMzYyIAkJCSIvPg0KCQkJPHBvbHlnb24gZmlsbD0iI0VCRU
    JFQiIgcG9pbnRzPSI0NzkuODI4LDQ3NC4wNjkgNDc5LjgyOCw0MjIuNCA0NzkuNzgyLDQyMi40M
    TMgNDI0LjQ2Nyw0MDcuNDc3IDQyMC45MzEsMzY3Ljg2NCANCgkJCQkzOTQuMDUyLDM2Ny44NjQg
    MzcxLjA3MiwzNjcuODY0IDM3OC4wMzEsNDQ1Ljg1IDQ3OS43NzEsNDc0LjA5NCA0ODAsNDc0LjA
    zIDQ4MCw0NzQuMDIxIAkJCSIvPg0KCQkJPHBvbHlnb24gcG9pbnRzPSIzNDMuNzg0LDUwLjIyOS
    AzNjYuODc0LDUwLjIyOSAzNjYuODc0LDc1LjUxNyAzOTIuMTE0LDc1LjUxNyAzOTIuMTE0LDAgM
    zY2Ljg3MywwIDM2Ni44NzMsMjQuOTM4IA0KCQkJCTM0My43ODMsMjQuOTM4IDM0My43ODMsMCAz
    MTguNTQ0LDAgMzE4LjU0NCw3NS41MTcgMzQzLjc4NCw3NS41MTcgCQkJIi8+DQoJCQk8cG9seWd
    vbiBwb2ludHM9IjQyNS4zMDcsMjUuMDQyIDQyNS4zMDcsNzUuNTE3IDQ1MC41NDksNzUuNTE3ID
    Q1MC41NDksMjUuMDQyIDQ3Mi43NzksMjUuMDQyIDQ3Mi43NzksMCA0MDMuMDg1LDAgDQoJCQkJN
    DAzLjA4NSwyNS4wNDIgNDI1LjMwNiwyNS4wNDIgCQkJIi8+DQoJCQk8cG9seWdvbiBwb2ludHM9
    IjUwOC41MzcsMzguMDg2IDUyNS45MTQsNjQuOTM3IDUyNi4zNDksNjQuOTM3IDU0My43MTQsMzg
    uMDg2IDU0My43MTQsNzUuNTE3IDU2OC44NTEsNzUuNTE3IDU2OC44NTEsMCANCgkJCQk1NDIuNT
    IyLDAgNTI2LjM0OSwyNi41MzQgNTEwLjE1OSwwIDQ4My44NCwwIDQ4My44NCw3NS41MTcgNTA4L
    jUzNyw3NS41MTcgCQkJIi8+DQoJCQk8cG9seWdvbiBwb2ludHM9IjY0Mi4xNTYsNTAuNTU1IDYw
    Ni42Niw1MC41NTUgNjA2LjY2LDAgNTgxLjQxMiwwIDU4MS40MTIsNzUuNTE3IDY0Mi4xNTYsNzU
    uNTE3IAkJCSIvPg0KCQkJPHBvbHlnb24gZmlsbD0iI0ZGRkZGRiIgcG9pbnRzPSI0ODAsNDc0Lj
    AyMSA1ODEuNjQ5LDQ0NS44NSA1ODIuMzk2LDQzNy40NTggNTk0LjA2OCwzMDYuNjk5IDU5NS4yO
    CwyOTMuMzYyIDU4MS44OTYsMjkzLjM2MiANCgkJCQk0ODAsMjkzLjM2MiA0NzkuODI4LDI5My4z
    NjIgNDc5LjgyOCwzNDMuMDI5IDQ4MCwzNDMuMDI5IDU0MC45ODgsMzQzLjAyOSA1MzUuMjI0LDQ
    wNy40NDUgNDgwLDQyMi4zNSA0NzkuODI4LDQyMi4zOTYgDQoJCQkJNDc5LjgyOCw0MjIuNCA0Nz
    kuODI4LDQ3NC4wNjkgCQkJIi8+DQoJCQk8cG9seWdvbiBmaWxsPSIjRkZGRkZGIiBwb2ludHM9I
    jQ3OS44MjgsMjQyLjM4IDQ3OS44MjgsMjQyLjUwMiA0ODAsMjQyLjUwMiA1OTkuNjQsMjQyLjUw
    MiA1OTkuOCwyNDIuNTAyIDYwMC43OTYsMjMxLjMzOCANCgkJCQk2MDMuMDU5LDIwNi4xNTkgNjA
    0LjI0NywxOTIuODMzIDQ4MCwxOTIuODMzIDQ3OS44MjgsMTkyLjgzMyA0NzkuODI4LDIyMy42OD
    IgCQkJIi8+DQoJCTwvZz4NCgk8L2c+DQo8L2c+DQo8L3N2Zz4NCg==" /> <svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg>

È possibile utilizzare la sintassi `min-width` per la visualizzazione di immagini alternative basate sulla dimensione del viewport. Si tratta di una tecnica vantaggiosa che non richiede il download dell'immagine in caso di mancata corrispondenza con la media query. Ad esempio, `bg.png` viene scaricato e applicato a `body` solo se la larghezza del browser è di almeno 500 pixel:

<svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg><svg class="side-by-side" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" width="50%" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5"/><path fill="#E44D26" d="M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z"/><polygon fill="#F16529" points="480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2"/><polygon fill="#F16529" points="541,343 480,343 480,422.4 535.2,407.4"/><polygon fill="#EBEBEB" points="414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4"/><polygon fill="#EBEBEB" points="479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474"/><polygon points="343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5"/><polygon points="425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25"/><polygon points="508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5"/><polygon points="642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5"/><polygon fill="#FFFFFF" points="480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1"/><polygon fill="#FFFFFF" points="479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7"/></g></g></g></svg>

##### Data URI

Numerosi caratteri contengono glifi unicode che è possibile utilizzare al posto delle immagini. A differenza delle immagini, è possibile scalare i caratteri unicode ottenendo una qualità ottimale in qualsiasi dimensione.

    <img src="data:image/svg+xml;base64,[data]">
    

Oltre al normale insieme set di caratteri, l'unicode inlcude simboli per forme numeriche (&#8528;), frecce (&#8592;), operatori matematici (&#8730;), forme geometriche (&#9733;), immagini di controllo (&#9654;), schemi braille (&#10255;), note musicali (&#9836;), lettere greche (&#937;) e pezzi degli scacchi (&#9822;).

    background-image: image-set(
      url(icon1x.jpg) 1x,
      url(icon2x.jpg) 2x
    );
    

La procedura per l'inserimento dei caratteri unicode è identica a quella per le named entity, ovvero l'utilizzo del codice `&#XXXX`, in cui `XXXX` è il numero del carattere unicode. Ad esempio:

Sei un super&#9733;

##### Inlining in CSS

Per creare icone più complesse, SVG offre un risultato più leggero, intuitivo e personalizzabile con CSS. SVG offre diversi vantaggi rispetto alle immagini raster:

<span class="side-by-side" id="data_uri"></span> <span class="side-by-side" id="svg"></span>

##### Inlining pros & cons

Inline code for images can be verbose&mdash;especially Data URIs&mdash;so why would you want to use it? To reduce HTTP requests! SVGs and Data URIs can enable an entire web page, including images, CSS and JavaScript, to be retrieved with one single request.

Esistono diversi caratteri per icone gratuiti e a pagamento come [Font Awesome](http://fortawesome.github.io/Font-Awesome/){: .external }, [Pictos](http://pictos.cc/) e [Glyphicons](http://glyphicons.com/).

* Utilizza immagini adatte alle caratteristiche del display, prendendo in considerazione dimensioni dello schermo, risoluzione del dispositivo e disposizione della pagina.
* Modifica la proprietà `background-image` dei CSS per i display ad alta risoluzione utilizzando le media query con `min-resolution` e `-webkit-min-device-pixel-ratio`.
* Utilizza `scrset` per fornire immagini ad alta risoluzione oltre all'immagine 1x nel markup.
* Valuta i costi in termini di rendimento dovuti all'utilizzo di tecniche di sostituzione delle immagini via JavaScript o di immagini compresse ad alta risoluzione per i dispositivi a risoluzioni inferiori.
* Data URIs cannot be cached, so must be downloaded for every page they're used on.
* They're not supported in IE 6 and 7, incomplete support in IE8.
* With HTTP/2, reducing the number of asset requests will become less of a priority.

Equilibra il peso delle richieste HTTP aggiuntive e le dimensioni del file con le esigenze in termini di icone. Ad esempio, se occorrono poche icone è consigliabile l'utilizzo di un'immagine o di uno sprite di immagine.

## Utilizzo di SVG per le icone

Spesso le immagini richiedono il download di molti dati e occupano una parte rilevante dello spazio di visualizzazione della pagina. L'ottimizzazione delle immagini consente di risparmiare byte e migliorare il rendimento del sito web: il consumo della larghezza di banda del client e la velocità di download e visualizzazione delle risorse nel browser sono inversamente proporzionali al numero di byte da scaricare.

### Possibilità di ingrandire le immagini dei prodotti

* Utilizza SVG o unicode per le icone al posto delle immagini raster.
* Change the `background-image` property in CSS for high DPI displays using media queries with `min-resolution` and `-webkit-min-device-pixel-ratio`.
* Use srcset to provide high resolution images in addition to the 1x image in markup.
* Consider the performance costs when using JavaScript image replacement techniques or when serving highly compressed high resolution images to lower resolution devices.

### Ulteriori tecniche per le immagini

Esistono due tipi di immagini da prendere in considerazione: [immagini vettoriali](http://it.wikipedia.org/wiki/Grafica_vettoriale){: .external } e [immagini raster](http://it.wikipedia.org/wiki/Grafica_raster). Per le immagini raster occorre selezionare il formato di compressione appropriato, come ad esempio GIF, PNG e JPG.

Le **immagini raster** sono simili alle fotografie e alle altre immagini costituite da una griglia di pixel o punti singoli. Di solito, le immagini raster vengono prodotte da fotocamere o scanner ed è possibile crearle nel browser con l'elemento `canvas`. La dimensione del file è direttamente proporzionale alla dimensione dell'immagine. Aumentando le dimensioni delle immagini raster rispetto a quelle originali si ottiene un effetto sfocato poiché il browser deve riempire in qualche modo i pixel mancanti.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-set.html" region_tag="imageset"   adjust_indentation="auto" %}
</pre>

Le **immagini vettoriali**, come logo e line art, vengono definite da un insieme di curve, linee, forme e colori di riempimento, create con programmi come Adobe Illustrator o Inkscape e salvate in un formato vettoriale come ad esempio [SVG](http://css-tricks.com/using-svg/){: .external }. Le immagini vettoriali vengono realizzate con semplici primitive ed è possibile ridimensionarle senza perdite di qualità o modifiche della dimensione del file.

### TL;DR {: .hide-from-toc }

Per la scelta del formato corretto è importante prendere in considerazione l'origine dell'immagine (raster o vettoriale) e i contenuti (colori, animazioni, testo e così via). Non esiste un formato adatto a tutti i tipi di immagini: ciascun formato presenta vantaggi e svantaggi.

    @media (min-resolution: 2dppx),
    (-webkit-min-device-pixel-ratio: 2)
    {
      /* High dpi styles & resources here */
    }
    

Per scegliere il formato corretto attieniti alle linee guida seguenti:

È possibile ridurre le dimensioni del file utilizzando il 'post-processing' una volta concluso il salvataggio. Esistono diversi strumenti per la compressione delle immagini: con o senza perdita di informazioni, online, GUI e con riga di comando. È consigliabile eseguire un'ottimizzazione automatizzata dell'immagine come elemento principale del flusso di lavoro in uso.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx"   adjust_indentation="auto" %}
</pre>

Esistono diversi strumenti per eseguire un'ulteriore compressione senza perdita di informazioni dei file JPG e PNG senza compromettere la qualità dell'immagine. Per il formato JPG, prova [jpegtran](http://jpegclub.org/){: .external } o [jpegoptim](http://freshmeat.net/projects/jpegoptim/), disponibile solo su Linux e da eseguire con l'opzione `strip-all`. Per il formato PNG, prova [OptiPNG](http://optipng.sourceforge.net/) o [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

Lo spriting CSS è una tecnica che unisce diverse immagini in una singola immagine di 'foglio di sprite'. Quindi è possibile utilizzare singole immagini specificando l'immagine di sfondo di un elemento (foglio di sprite) e un offset per la visualizzazione della parte corretta.

### Utilizzo delle media query per il caricamento delle immagini adattabili o la direzione artistica

Media queries can create rules based on the [device pixel ratio](http://www.html5rocks.com/en/mobile/high-dpi/#toc-bg), making it possible to specify different images for 2x versus 1x displays.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

Lo spriting riduce il numero dei download necessari per ottenere immagini multiple senza disattivare il caching.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" adjust_indentation="auto" %}
</pre>

Il caricamento ritardato velocizza il caricamento delle pagine di grandi dimensioni con diverse immagini caricate secondo necessità o al termine del caricamento e del rendering del contenuto primario. Oltre ai miglioramenti in termini di rendimento, il caricamento ritardato consente di eseguire lo scorrimento infinito della pagina.

Attenzione nel creare pagine a scorrimento infinito, poiché i contenuti vengono caricati al momento della visualizzazione e i motori di ricerca potrebbero non indicizzarli. Inoltre, gli utenti in cerca delle informazioni visualizzate nei piè di pagina non riusciranno a visualizzare questa parte della pagina a causa del continuo caricamento dei nuovi contenuti.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

## Ottimizzazione delle immagini per un rendimento ottimale

{# include shared/related_guides.liquid inline=true list=page.related-guides.optimize #}

### Utilizzo di image-set per la fornitura di immagini ad alta risoluzione

* Si tratta di elementi grafici vettoriali scalabili all'infinito.

### Utilizzo delle media query per fornire immagini ad alta risoluzione o la direzione artistica

In alcuni casi, è consigliabile evitare l'utilizzo delle immagini. Se possibile, utilizza le funzionalità native del browser per offrire funzionalità analoghe. I browser possono creare elementi grafici che un tempo richiedevano l'utilizzo dei file d'immagine. In altre parole, il browser non deve più scaricare file di immagine separati in modo da evitare la visualizzazione di immagini ridimensionate in modo non ottimale. È possibile effettuare il rendering delle icone usando unicode o i caratteri speciali per icone.

Se possibile, usa il testo e non incorporarlo nelle immagini. Ad esempio, evita l'utilizzo delle immagini per le intestazioni o l'inserimento di informazioni di contatto come numeri telefonici o indirizzi, poiché in questo modo è impossibile copiare e incollare le informazioni, utilizzare gli screen reader e ottenere buone prestazioni. Al contrario, inserisci i testi in un markup e utilizza i webfont per ottenere lo stile desiderato.

I browser moderni utilizzano le funzionalità CSS per la creazione di stili che un tempo richiedevano l'utilizzo delle immagini. Ad esempio, è possibile creare gradienti complessi con la proprietà `background`, ombre con `box-shadow` e angoli smussati con `border-radius`.

    You're a super &#9733;
    

You're a super &#9733;

### TL;DR {: .hide-from-toc }

Inoltre, l'utilizzo di queste tecniche non richiede cicli di rendering, cosa molto importante per i dispositivi mobili. Attenzione a non esagerare, poiché potresti sacrificare i vantaggi ottenuti e ottenere un basso rendimento.

* Si tratta di elementi grafici vettoriali scalabili all'infinito, ma un eventuale anti-aliasing potrebbe ridurne la nitidezza.
* Personalizzazione con CSS limitata.
* L'esatto posizionamento dei pixel potrebbe essere difficile, essendo basato su altezza delle linee, spaziatura delle lettere e così via.
* Non essendo elementi semantici, sono inadatti a lettori dello schermo o altre tecnologie di assistenza.
* Se non utilizzati al meglio possono creare file di grandi dimensioni anche con l'utilizzo di un gruppo ridotto di icone.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-svg.html){: target="_blank" .external }

### Sostituzione delle icone semplici con caratteri unicode<figure class="attempt-right"> 

<img src="img/icon-fonts.png" class="center" srcset="img/icon-fonts.png 1x, img/icon-fonts-2x.png 2x" alt="Example of a page that uses FontAwesome for its font icons." /> <figcaption> <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html" target="_blank" class="external"> Example of a page that uses FontAwesome for its font icons. </a> </figcaption> </figure> 

Icon fonts are popular, and can be easy to use, but have some drawbacks compared to SVG icons:

* Non scegliere un formato a caso per le immagini, ma analizza quelli disponibili e utilizza il più adatto alle tue esigenze.
* Inserisci strumenti di ottimizzazione delle immagini e compressione al flusso di lavoro per la riduzione delle dimensioni dei file.
* Riduci il numero delle richieste HTTP inserendo immagini di utilizzo comune negli sprite immagine.
* Valuta se caricare le immagini solo al momento della visualizzazione, in modo da ottimizzare tempi di caricamento e peso iniziale della pagina.
* Unless properly scoped, they can result in a large file size for only using a small subset of the icons available.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/icon-font.html" region_tag="iconfont" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html){: target="_blank" .external }

There are hundreds of free and paid icon fonts available including [Font Awesome](https://fortawesome.github.io/Font-Awesome/), [Pictos](http://pictos.cc/){: .external }, and [Glyphicons](https://glyphicons.com/).

Be sure to balance the weight of the additional HTTP request and file size with the need for the icons. For example, if you only need a handful of icons, it may be better to use an image or an image sprite.

## Utilizzo degli sprite immagine

Images often account for most of the downloaded bytes and also often occupy a significant amount of the visual space on the page. As a result, optimizing images can often yield some of the largest byte savings and performance improvements for your website: the fewer bytes the browser has to download, the less competition there is for client's bandwidth and the faster the browser can download and display all the assets.

### Sostituzione delle icone complesse con SVG

* Utilizza il JPG per le fotografie.
* Utilizza l'SVG per gli elementi grafici vettoriali e a tinta unita come logo ed disegni al tratto. Se non sono disponibili elementi grafici vettoriali usa WebP o PNG.
* Utilizza il PNG e non il GIF poiché il primo formato offre colori e rapporto di compressione ottimali.
* Per le animazioni di maggiore durata utilizza i `<video>`, che offrono una qualità dell'immagine ottimale e un efficace controllo della riproduzione.

### Utilizza i caratteri per icone con attenzione

There are two types of images to consider: [vector images](https://en.wikipedia.org/wiki/Vector_graphics) and [raster images](https://en.wikipedia.org/wiki/Raster_graphics). For raster images, you also need to choose the right compression format, for example: `GIF`, `PNG`, `JPG`.

**Raster images**, like photographs and other images, are represented as a grid of individual dots or pixels. Raster images typically come from a camera or scanner, or can be created in the browser with the `canvas` element. As the image size gets larger, so does the file size. When scaled larger than their original size, raster images become blurry because the browser needs to guess how to fill in the missing pixels.

**Vector images**, such as logos and line art, are defined by a set of curves, lines, shapes, and fill colors. Vector images are created with programs like Adobe Illustrator or Inkscape and saved to a vector format like [`SVG`](https://css-tricks.com/using-svg/). Because vector images are built on simple primitives, they can be scaled without any loss in quality or change in file size.

When choosing the appropriate format, it is important to consider both the origin of the image (raster or vector), and the content (colors, animation, text, etc). No one format fits all image types, and each has its own strengths and weaknesses.

Start with these guidelines when choosing the appropriate format:

* Use `JPG` for photographic images.
* Use `SVG` for vector art and solid color graphics such as logos and line art. If vector art is unavailable, try `WebP` or `PNG`.
* Use `PNG` rather than `GIF` as it allows for more colors and offers better compression ratios.
* For longer animations consider using `<video>`, which provides better image quality and gives the user control over playback.

### TL;DR {: .hide-from-toc }

You can reduce image file size considerably by "post-processing" the images after saving. There are a number of tools for image compression&mdash;lossy and lossless, online, GUI, command line. Where possible, it's best to try automating image optimization so that it's a first-class citizen in your workflow.

Several tools are available that perform further, lossless compression on `JPG` and `PNG` files with no effect on image quality. For `JPG`, try [jpegtran](http://jpegclub.org/) or [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (available on Linux only; run with the --strip-all option). For `PNG`, try [OptiPNG](http://optipng.sourceforge.net/) or [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

### Scelta del formato corretto

<img src="img/sprite-sheet.png" class="attempt-right" alt="Image sprite sheet used in example" />

CSS spriting is a technique whereby a number of images are combined into a single "sprite sheet" image. You can then use individual images by specifying the background image for an element (the sprite sheet) plus an offset to display the correct part.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/image-sprite.html){: target="_blank" .external }

Spriting has the advantage of reducing the number of downloads required to get multiple images, while still enabling caching.

### Riduzione della dimensione dei file

Lazy loading can significantly speed up loading on long pages that include many images below the fold by loading them either as needed or when the primary content has finished loading and rendering. In addition to performance improvements, using lazy loading can create infinite scrolling experiences.

Be careful when creating infinite scrolling pages&mdash;because content is loaded as it becomes visible, search engines may never see that content. In addition, users who are looking for information they expect to see in the footer, never see the footer because new content is always loaded.

## Evitare le immagini

Sometimes the best image isn't actually an image at all. Whenever possible, use the native capabilities of the browser to provide the same or similar functionality. Browsers generate visuals that would have previously required images. This means that browsers no longer need to download separate image files thus preventing awkwardly scaled images. You can use unicode or special icon fonts to render icons.

### Valuta il caricamento ritardato

Wherever possible, text should be text and not embedded into images. For example, using images for headlines or placing contact information&mdash;like phone numbers or addresses&mdash;directly into images prevents users from copying and pasting the information; it makes the information inaccessible for screen readers, and it isn't responsive. Instead, place the text in your markup and if necessary use webfonts to achieve the style you need.

### Posiziona i testi in un markup senza incorporarli nelle immagini

Modern browsers can use CSS features to create styles that would previously have required images. For example: complex gradients can be created using the `background` property, shadows can be created using `box-shadow`, and rounded corners can be added with the `border-radius` property. 

<style>
  p#noImage {
    margin-top: 2em;
    padding: 1em;
    padding-bottom: 2em;
    color: white;
    border-radius: 5px;
    box-shadow: 5px 5px 4px 0 rgba(9,130,154,0.2);
    background: linear-gradient(rgba(9, 130, 154, 1), rgba(9, 130, 154, 0.5));
  }

  p#noImage code {
    color: rgb(64, 64, 64);
  }
</style>

 

<p id="noImage">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque sit amet augue eu magna scelerisque porta ut ut dolor. Nullam placerat egestas nisl sed sollicitudin. Fusce placerat, ipsum ac vestibulum porta, purus dolor mollis nunc, pharetra vehicula nulla nunc quis elit. Duis ornare fringilla dui non vehicula. In hac habitasse platea dictumst. Donec ipsum lectus, hendrerit malesuada sapien eget, venenatis tempus purus.
</p>

    <style>
      div#noImage {
        color: white;
        border-radius: 5px;
        box-shadow: 5px 5px 4px 0 rgba(9,130,154,0.2);
        background: linear-gradient(rgba(9, 130, 154, 1), rgba(9, 130, 154, 0.5));
      }
    </style>
    

Keep in mind that using these techniques does require rendering cycles, which can be significant on mobile. If over-used, you'll lose any benefit you may have gained and it may hinder performance.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}