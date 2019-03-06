project_path: /web/fundamentals/_project.yaml book_path: /web/fundamentals/_book.yaml description: Payment Request API per pagamenti veloci e facili sul web.

{# wf_published_on: 2016-07-25 #} {# wf_updated_on: 2018-03-29 #} {# wf_blink_components: Blink>Payments #}

# Introduzione all'API Payment Request {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/zkoch.html" %}

<div class="video-wrapper-full-width">
  <iframe class="devsite-embedded-youtube-video" data-video-id="colCcgKoLUM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

## Introduzione all'API Payment Request {: #introducing }

Fare acquisti online è comodo ma a volte può essere frustrante, in particolare sui dispositivi mobili. Sebbene il traffico sui dispositivi mobili continui ad aumentare, le conversioni dai dispositivi mobili rappresentano solo circa un terzo di tutti gli acquisti completati. In altre parole, gli utenti abbandonano gli acquisti sul cellulare il doppio delle volte degli acquisti effettuati dal desktop. Perché?

Benefits of Web Payments:

**For consumers**, they simplify checkout flow, by making it a few taps instead of typing small characters many times on a virtual keyboard.

I moduli di acquisto online possono essere impegnativi per gli utenti, difficili da utilizzare, lenti da caricare e aggiornare e richiedono più passaggi da completare. Questo perché due componenti principali dei pagamenti online - sicurezza e convenienza - spesso funzionano inversamente: più di uno in genere significa meno dell'altro.

La maggior parte dei problemi che portano all'abbandono possono essere direttamente ricondotti ai moduli di acquisto. Ogni app o sito ha il proprio processo di immissione e convalida dei dati e gli utenti spesso si trovano a dover inserire le stesse informazioni nel punto di acquisto di ogni app. Inoltre, gli sviluppatori di applicazioni faticano a creare flussi di acquisto che supportano più metodi di pagamento unici; anche piccole differenze nei requisiti del metodo di pagamento possono complicare il completamento del modulo e il processo di invio.

Qualsiasi sistema che migliori o risolva uno o più di questi problemi è un cambiamento positivo. Abbiamo già iniziato a risolvere il problema con [Autofill](/web/updates/2015/06/checkout-faster-with-autofill), ma ora vorremmo parlare di una soluzione più completa.

## 3 principles

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/standard-open.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Standard and Open</h3>
    Web Payments are an open payment standard for the web platform for the first time
    in history. They are available for any players to implement.</div>
</section>

<section style="display:flex;padding-bottom:32px;">
  <div style="min-width:50%">
    <h3>Easy and Consistent</h3>
    Web Payments make checkout easy for the user, by reusing stored 
payments and address information and removing the need for the user to fill in checkout forms. 
Since the UI is implemented by the browser natively, users see a familiar and consistent checkout 
experience on any website that makes use of the standard.</div>
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/easy-consistent.png" width="100%" alt="Standard and Open" title="">
  </div>
</section>

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/secure-flexible.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Secure and Flexible</h3>
    Web Payments provide industry-leading payment technology to the 
web, and can easily integrate a secure payment solution.</div>
</section>

## Next Up

L'API Payment Request è una possibile soluzione [standard W3C](https://www.w3.org/TR/payment-request/) che ha lo scopo di *eliminare i moduli di checkout* . Migliora notevolmente il flusso di lavoro degli utenti durante il processo di acquisto, offrendo un'esperienza utente più coerente e consentendo ai commercianti di sfruttare facilmente diversi metodi di pagamento.

L'API Payment Request è progettata per essere indipendente dal fornitore, ossia non richiede l'uso di un particolare sistema di pagamento. Non si tratta di un nuovo metodo di pagamento né si integra direttamente con i processori di pagamento; piuttosto, è un canale che collega le informazioni di pagamento e spedizione dell'utente ai commercianti, con i seguenti obiettivi:

L'API Payment Request è uno standard aperto con compatibilità browser che sostituisce i flussi di pagamento tradizionali consentendo ai commercianti di richiedere e accettare qualsiasi pagamento in una singola chiamata API. L'API consente alla pagina Web di scambiare informazioni con l'agente utente mentre l'utente fornisce l'input, prima di approvare o rifiutare una richiesta di pagamento.

Meglio ancora, con il browser che funge da intermediario, tutte le informazioni necessarie per un checkout veloce possono essere memorizzate nel browser, in modo che gli utenti possano semplicemente confermare e pagare, tutto con un solo clic.

Utilizzando l'API Payment Request, il processo della transazione è reso il più semplice possibile sia per gli utenti che per i commercianti.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}