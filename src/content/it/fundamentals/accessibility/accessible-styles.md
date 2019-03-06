project_path: /web/fundamentals/_project.yaml book_path: /web/fundamentals/_book.yaml description: Utilizzare stili appropriati per migliorare l'accessibilità

{# wf_updated_on: 2018-05-23 #} {# wf_published_on: 2016-10-04 #}

# Stili accessibili {: .page-title}

{% include "web/_shared/contributors/megginkearney.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/robdodson.html" %}

Abbiamo esplorato due dei pilastri fondamentali dell'accessibilità: la concentrazione e la semantica. Ora affrontiamo il terzo: lo stile. È un argomento ampio che possiamo trattare in tre sezioni.

- Garantire che gli elementi siano disegnati per supportare i nostri sforzi di accessibilità aggiungendo stili per lo stato attivo e vari stati ARIA.
- Disegnare le nostre interfacce utente per la flessibilità in modo che possano essere ingrandite o ridimensionate per soddisfare gli utenti che potrebbero avere problemi con il testo di piccole dimensioni.
- Scegliere i colori e il contrasto giusti per evitare di trasmettere informazioni solo attraverso il colore.

## Stile per lo stato attivo

Generalmente, ogni volta che rendiamo attivo un elemento, utilizziamo l'anello di messa a fuoco integrato nel browser (la proprietà CSS `outline`) per modellare l'elemento. L'anello di messa a fuoco è utile perché, senza di esso, è impossibile per chi usa la tastiera indicare quale elemento sia attivato. La [checklist WebAIM](http://webaim.org/standards/wcag/checklist){: .external} puntualizza, richiedendo che sia "visivamente evidente, quale elemento della pagina viene attivato dalla tastiera (ossia mentre sfogliamo la pagina, è possibile vedere dove ci troviamo)".

![Modulo con anello di messa a fuoco](imgs/focus-ring.png)

Tuttavia, a volte l'anello di messa a fuoco può apparire distorto o potrebbe non adattarsi al design della pagina. Alcuni sviluppatori rimuovono completamente questo stile impostando l'`outline` dell'elemento su `0` o `none`. Ma senza un indicatore di messa a fuoco, come possono sapere gli utenti della tastiera con quale elemento interagiscono?

Warning: non impostare mai outline a 0 o none senza fornire una messa a fuoco alternativa!

Potresti avere familiarità con l'aggiunta di stati al passaggio del mouse ai tuoi controlli usando la *pseudo-classe* CSS `:hover`. Ad esempio, è possibile utilizzare `:hover` su un elemento di collegamento per cambiarne il colore o lo sfondo quando il mouse si trova su di esso. Simile a `:hover`, è possibile utilizzare la pseudo-classe `:focus` per indicare un elemento quando è attivo.

    /* At a minimum you can add a focus style that matches your hover style */
    :hover, :focus {
      background: #c0ffee;
    }
    

Una soluzione alternativa al problema della rimozione dell'anello di messa a fuoco consiste nel dare al tuo elemento gli stessi stili di passaggio del mouse e di attivazione, che risolvono il problema "dove è l'anello?" per gli utenti della tastiera. Come al solito, migliorare l'esperienza di accessibilità migliora l'esperienza di tutti.

### Modalità di input

![Pulsante HTML nativo con anello di messa a fuoco](imgs/sign-up.png){: .attempt-right }

Per gli elementi nativi come `button`, i browser possono rilevare se l'interazione dell'utente si è verificata tramite mouse o tastiera e in genere l'anello di messa a fuoco viene visualizzato solo per l'interazione con la tastiera. Ad esempio, facendo clic su un `button` nativo con il mouse non vi è alcun anello di messa a fuoco, ma accedendovi dalla tastiera, questo appare.

Questa logica si basa sul fatto che gli utenti del mouse hanno meno probabilità di aver bisogno dell'attivazione perché sanno su quale elemento hanno fatto clic. Sfortunatamente non esiste al momento una soluzione multi-browser per uniformare questo comportamento. Di conseguenza, applicando lo stile `:focus` a un elemento, verrà visualizzato per l'utente che fa clic sull'elemento o che lo attiva dalla tastiera. Prova a fare clic su questo pulsante finto e noterai che lo stile `:focus` viene sempre applicato.

    <style>
      fake-button {
        display: inline-block;
        padding: 10px;
        border: 1px solid black;
        cursor: pointer;
        user-select: none;
      }
    
      fake-button:focus {
        outline: none;
        background: pink;
      }
    </style>
    <fake-button tabindex="0">Click Me!</fake-button>
    

{% framebox height="80px" %}
<style>fake-button {display: inline-block;padding: 10px;border: 1px solid black;cursor: pointer;user-select: none;}fake-button:focus {outline: none;background: pink;}</style>
<fake-button tabindex="0">Click Me!</fake-button> {% endframebox %}

This can be a bit annoying, and often times developer will resort to using JavaScript with custom controls to help differentiate between mouse and keyboard focus.

Questo può essere un po' fastidioso e spesso lo sviluppatore ricorre all'uso di JavaScript con controlli personalizzati per poter distinguere tra mouse e tastiera.

In Firefox, la pseudo-classe CSS `:-moz-focusring` consente di scrivere uno stile di messa a fuoco che viene applicato solo se l'elemento è attivato tramite la tastiera, una caratteristica piuttosto utile. Nonostante questa pseudo-classe sia supportata attualmente solo in Firefox, [sono in atto diverse iniziative per farla divenire standard](https://github.com/wicg/modality){: .external}.

## Stili di stato con ARIA

C'è anche [questo fantastico articolo di Alice Boxhall e Brian Kardell](https://www.oreilly.com/ideas/proposing-css-input-modality){: .external} che esplora l'argomento della modalità e contiene il codice prototipo per differenziare l'input da mouse e da tastiera. Puoi usare subito la loro soluzione e includere la pseudo-classe dell'anello di messa a fuoco in un secondo momento, quando avrà un supporto più diffuso.

Quando si creano componenti, è prassi comune adattare il loro stato e, quindi, il loro aspetto, utilizzando le classi CSS controllate con JavaScript.

Ad esempio, considerando un pulsante di attivazione che si trasforma nello stato visivo "premuto" quando viene fatto clic e mantiene tale stato fino a quando non viene fatto nuovamente clic. Per definire lo stato, il tuo JavaScript potrebbe aggiungere una classe `pressed` al pulsante. E, dato che vuoi una buona semantica su tutti i tuoi controlli, devi anche impostare lo stato `aria-pressed` per il pulsante su `true`.

    .toggle.pressed { ... }
    

Una tecnica utile da impiegare qui è quella di rimuovere completamente la classe, ed usare gli attributi ARIA per applicare uno stile all'elemento. Ora puoi aggiornare il selettore CSS per lo stato premuto del pulsante da così

    .toggle[aria-pressed="true"] { ... }
    

a così:

## Responsive design multi-dispositivo

Ciò crea una relazione sia logica che semantica tra lo stato ARIA e l'aspetto dell'elemento e riduce anche il codice aggiuntivo.

Sappiamo che è una buona idea progettare in maniera responsive per offrire la migliore esperienza multi-dispositivo, ma il design responsive produce anche un miglioramento dell'accessibilità.

![Udacity.com at 100% magnification](imgs/udacity.jpg)

A low-vision user who has difficulty reading small print might zoom in the page, perhaps as much as 400%. Because the site is designed responsively, the UI will rearrange itself for the "smaller viewport" (actually for the larger page), which is great for desktop users who require screen magnification and for mobile screen reader users as well. It's a win-win. Here's the same page magnified to 400%:

![Udacity.com at 400% magnification](imgs/udacity-zoomed.jpg)

In fact, just by designing responsively, we're meeting [rule 1.4.4 of the WebAIM checklist](http://webaim.org/standards/wcag/checklist#sc1.4.4){: .external }, which states that a page "...should be readable and functional when the text size is doubled."

Infatti, solo progettando in modo reactive, stiamo rispettando la [regola 1.4.4 della checklist WebAIM](http://webaim.org/standards/wcag/checklist#sc1.4.4){: .external}, secondo la quale una pagina "[...]dovrebbe restare leggibile e funzionale quando la dimensione del testo è raddoppiata".

- Innanzitutto, assicurati di utilizzare sempre il metatag `viewport` appropriato.   
    `<meta name="viewport" content="width=device-width,
initial-scale=1.0">`   
    Impostando `width=device-width` ci sarà corrispondenza tra la larghezza dello schermo ed i pixel indipendenti dal dispositivo e l'impostazione `initial-scale=1` stabilisce una relazione 1: 1 tra pixel CSS e pixel indipendenti dal dispositivo. In questo modo il browser adatterà il tuo contenuto alle dimensioni dello schermo, in modo che gli utenti non vedano solo un mucchio di testo accartocciato.

![a phone display without and with the viewport meta tag](imgs/scrunched-up.jpg)

Warning: When using the viewport meta tag, make sure you don't set maximum-scale=1 or set user-scaleable=no. Let users zoom if they need to!

- Un'altra tecnica da tenere a mente è progettare con una griglia responsive. Come hai visto con il sito di Udacity, progettare con una griglia significa che il tuo contenuto si ridimensionerà quando la pagina cambia dimensione. Spesso questi layout sono prodotti utilizzando unità relative come percentuali, ems o rems invece di valori di pixel pre-codificati. Il vantaggio di farlo in questo modo è che il testo ed il contenuto possono ingrandirsi e forzare gli altri elementi in basso all'interno della pagina. Quindi l'ordine DOM e l'ordine di lettura rimangono gli stessi, anche se il layout cambia a causa dell'ingrandimento.

- Inoltre, considera l'utilizzo di unità relative come `em` o `rem` per cose come la dimensione del testo, anziché i valori dei pixel. Alcuni browser supportano il ridimensionamento del testo solo nelle preferenze dell'utente e, se stai utilizzando un valore in pixel per il testo, questa impostazione non influirà sulla tua copia. Se, tuttavia, hai utilizzato unità relative in tutto, la copia del sito verrà aggiornata per riflettere le preferenze dell'utente.

- Infine, quando il tuo progetto viene visualizzato su un dispositivo mobile, dovresti assicurarti che gli elementi interattivi, come pulsanti o collegamenti, siano abbastanza grandi e abbiano abbastanza spazio intorno a loro, per renderli facili da premere senza sovrapporsi accidentalmente ad altri elementi. Ciò avvantaggia tutti gli utenti, ma è particolarmente utile per chi ha una disabilità motoria.

Warning: utilizzando il meta tag viewport, assicurati di non impostare la maximum-scale=1 o di impostare user-scaleable=no. Consenti agli utenti di ingrandire se necessario!

![a diagram showing a couple of 48 pixel touch targets](imgs/touch-target.jpg)

Touch targets should also be spaced about 8 pixels apart, both horizontally and vertically, so that a user's finger pressing on one tap target does not inadvertently touch another tap target.

## Colore e contrasto

I target di tocco devono essere distanziati di circa 8 pixel tra loro, sia orizzontalmente che verticalmente, in modo che il dito dell'utente che li preme, non tocchi inavvertitamente un altro target.

Se hai una buona visione, è facile presumere che tutti percepiscano i colori, o la leggibilità del testo, nello stesso modo in cui lo fai tu, ma ovviamente non è così. Diamo un'occhiata a come possiamo usare efficacemente il colore e il contrasto per creare design piacevoli accessibili a tutti.

Come puoi immaginare, alcune combinazioni di colori che sono facili da leggere per alcune persone sono difficili o impossibili per altre. Questo di solito si riduce al *contrasto del colore*, la relazione tra la *luminosità* del colore di primo piano e dello sfondo. Quando i colori sono simili, il rapporto di contrasto è basso; quando sono diversi, il rapporto di contrasto è alto.

![comparison of various contrast ratios](imgs/contrast-ratios.jpg)

The contrast ratio of 4.5:1 was chosen for level AA because it compensates for the loss in contrast sensitivity usually experienced by users with vision loss equivalent to approximately 20/40 vision. 20/40 is commonly reported as typical visual acuity of people at about age 80. For users with low vision impairments or color deficiencies, we can increase the contrast up to 7:1 for body text.

Il rapporto di contrasto di 4,5:1 è stato scelto per il livello AA in quanto compensa la perdita di sensibilità al contrasto solitamente riscontrata da utenti con perdita della vista equivalente a circa 20/40 di vista. 20/40 è comunemente riportato come acuità visiva tipica delle persone di circa 80 anni. Per gli utenti con problemi di ipovisione o carenze di colore, possiamo aumentare il contrasto fino a 7:1 per il corpo del testo.

Puoi utilizzare l'[estensione Accessibility DevTools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb){: .external} per Chrome per identificare i rapporti di contrasto. Uno dei vantaggi dell'utilizzo di Chrome Devtools è che suggerisce alternative AA e AAA (migliorate) ai colori correnti, e puoi fare clic sui valori per visualizzarli in anteprima nella tua app.

1. Dopo aver installato l'estensione, fai clic su `Audits`
2. Deseleziona tutto tranne `Accessibility`
3. Fai clic su `Audit Present State`
4. Prendi nota di tutti gli avvisi relativi al contrasto

![the devtools contrast audit dialog](imgs/contrast-audit.png)

WebAIM itself provides a handy [color contrast checker](http://webaim.org/resources/contrastchecker/){: .external } you can use to examine the contrast of any color pair.

### Non trasmettere informazioni solo con il colore

WebAIM fornisce di per sé un pratico [controllo del contrasto del colore](http://webaim.org/resources/contrastchecker/){: .external} che puoi usare per esaminare il contrasto di qualsiasi coppia di colori.

Ci sono circa 320 milioni di utenti con deficit di visione dei colori. Circa 1 uomo su 12 e 1 donna su 200 hanno una qualche forma di "daltonismo"; ciò significa che circa 1/20 o il 5% dei tuoi utenti non vedrà il tuo sito nel modo desiderato. Quando ci affidiamo al colore per trasmettere informazioni, facciamo aumentare quella cifra fino a raggiungere livelli inaccettabili.

Note: il termine "daltonismo" è spesso usato per descrivere una condizione visiva in cui una persona ha difficoltà a distinguere i colori, ma in realtà pochissime persone sono davvero daltoniche. La maggior parte delle persone con deficit di colore può vedere alcuni o più colori, ma ha difficoltà a distinguere tra alcuni colori come rosso e verde (più comune), marrone e arancio, e blu e viola.

![an input form with an error underlined in red](imgs/input-form1.png)

The [WebAIM checklist states in section 1.4.1](http://webaim.org/standards/wcag/checklist#sc1.4.1){: .external } that "color should not be used as the sole method of conveying content or distinguishing visual elements." It also notes that "color alone should not be used to distinguish links from surrounding text" unless they meet certain contrast requirements. Instead, the checklist recommends adding an additional indicator such as an underscore (using the CSS `text-decoration` property) to indicate when the link is active.

La [checklist WebAIM afferma nella sezione 1.4.1](http://webaim.org/standards/wcag/checklist#sc1.4.1){: .external} che "il colore non deve essere utilizzato come unico metodo per trasmettere contenuto o distinguere elementi visivi." Rileva inoltre che "il colore da solo non deve essere utilizzato per distinguere i collegamenti dal testo circostante" a meno che non soddisfino determinati requisiti di contrasto. Invece la checklist raccomanda di aggiungere un altro indicatore, come il trattino basso, (usando la proprietà di `text-decoration` CSS) per indicare quando il collegamento è attivo.

![an input form with an added error message for clarity](imgs/input-form2.png)

When you're building an app, keep these sorts of things in mind and watch out for areas where you may be relying too heavily on color to convey important information.

Quando crei un'app, tieni a mente questo tipo di cose e fai attenzione alle aree in cui potresti affidarti troppo al colore per trasmettere informazioni importanti.

### Modalità ad alto contrasto

Se sei curioso di sapere come persone diverse percepiscono il tuo sito, o se fai affidamento sull'uso del colore nell'interfaccia utente, puoi utilizzare l'[estensione NoCoffee Chrome](https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl){: .external} per simulare varie forme di disabilità visive, tra cui diversi tipi di daltonismo.

La modalità ad alto contrasto consente all'utente di invertire i colori di primo piano e di sfondo, che spesso aiutano a migliorare il testo. Per le persone con problemi di ipovisione, la modalità ad alto contrasto può rendere molto più semplice la navigazione del contenuto sulla pagina. Ci sono alcuni modi per ottenere una configurazione ad alto contrasto sul tuo computer.

Sistemi operativi come Mac OSX e Windows offrono modalità ad alto contrasto che possono essere abilitate per tutto a livello di sistema. Oppure gli utenti possono installare un'estensione, come l'estensione [Chrome ad alto contrasto](https://chrome.google.com/webstore/detail/high-contrast/djcfdncoelnlbldjfhinnjlhdjlikmph){: .external} per abilitare il contrasto elevato solo in quella specifica app.

Un esercizio utile è quello di attivare le impostazioni ad alto contrasto e verificare che tutta l'interfaccia utente dell'applicazione sia ancora visibile e utilizzabile.

![a navigation bar in high contrast mode](imgs/tab-contrast.png)

Similarly, if you consider the example from the previous lesson, the red underline on the invalid phone number field might be displayed in a hard-to-distinguish blue-green color.

![a form with an error field in high contrast mode](imgs/high-contrast.jpg)

If you are meeting the contrast ratios covered in the previous lessons you should be fine when it comes to supporting high-contrast mode. But for added peace of mind, consider installing the Chrome High Contrast extension and giving your page a once-over just to check that everything works, and looks, as expected.

## Feedback {: #feedback }

Rispettando i rapporti di contrasto indicati nelle lezioni precedenti, non dovresti avere problemi a supportare la modalità ad alto contrasto. Tuttavia, per maggiore tranquillità, considera l'installazione dell'estensione Chrome High Contrast e analizza la pagina per verificare che tutto funzioni e si presenti come previsto.