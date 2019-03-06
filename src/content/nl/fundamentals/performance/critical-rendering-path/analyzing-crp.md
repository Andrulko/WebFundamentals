project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Het identificeren en oplossen van knelpunten in de prestatie van het kritieke weergavepad vereist een goede kennis van de gebruikelijke struikelblokken. Laten we een praktijkgerichte rondleiding nemen en de gebruikelijke prestatiepatronen eruit lichten waarmee u uw pagina's kunt optimaliseren.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Het kritieke weergavepad analyseren {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Het identificeren en oplossen van knelpunten in de prestatie van het kritieke weergavepad vereist een goede kennis van de gebruikelijke struikelblokken. Laten we een praktijkgerichte rondleiding nemen en de gebruikelijke prestatiepatronen eruit lichten waarmee u uw pagina's kunt optimaliseren.

Het doel van het kritieke weergavepad optimaliseren is om de browser de mogelijkheid te bieden de pagina zo snel mogelijk te kleuren: snellere pagina's zorgen voor grotere betrokkenheid, meer bekeken pagina's en \[verbeterde conversie\] (http://www.google.com/think/multiscreen/success.html){: .external }. Daarom willen we de tijd die een bezoeker naar een blanco scherm moet staren minimaliseren door een optimalisatie van welke bronnen worden geladen en in welke volgorde.

Ter illustratie van dit proces zullen we beginnen met het meest eenvoudige voorbeeld en zullen we onze pagina`s incrementeel opbouwen en extra bronnen, stijlen en app-structuur toevoegen. We zullen in dit proces ook zien waar het verkeerd kan gaan en hoe we elk van deze gevallen kunnen optimaliseren.

Tot slot nog één ding voordat we van start gaan... Tot nu toe hebben we ons enkel gericht op wat er in de browser gebeurt wanneer de bron (CSS-, JS- of HTML-bestand) beschikbaar is voor verwerking. We hebben de tijd die nodig is om deze bestanden op te halen van het cache of netwerk genegeerd. In de volgende les zullen we gedetailleerd ingaan op hoe we het netwerkaspect van onze app kunnen optimaliseren, maar ondertussen (om alles realistischer te maken) zullen we van het volgende uitgaan:

* Een netwerkroundtrip (propogatievertraging) naar de server kost 100 ms
* De serverresponstijd is 100 ms voor het HTML-document en 10 ms voor alle andere bestanden

## De Hallo wereld-ervaring

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

We beginnen met basis HTML-opmaak en een enkele afbeelding, geen CSS of JavaScript. Dit is het meest eenvoudige voorbeeld. Laten we nu de `Network timeline` (Netwerktijdlijn) openen in Chrome DevTools en de volgende bronwaterval bekijken:

We'll start with basic HTML markup and a single image; no CSS or JavaScript. Let's open up our Network timeline in Chrome DevTools and inspect the resulting resource waterfall:

<img src="images/waterfall-dom.png" alt="CRP" />

Zodra de HTML-inhoud beschikbaar wordt, moet de browser de bytes parseren, omzetten in tokens en de DOM-boomstructuur opbouwen. Merk hierbij op dat Chrome DevTools heel handig de tijd weergeeft die de gebeurtenis DOMContentLoaded onderaan nodig heeft (216 ms). Dit komt ook overeen met de blauwe verticale lijn. Het gat tussen het einde van de HTML-download en de blauwe verticale lijn (DOMContentLoaded) staat voor de tijd die de browser nodig had om de DOM-boomstructuur te bouwen, in dit geval slecht een paar milliseconden.

Let tot slot nog op iets interessants: onze `awesome-foto` heeft de gebeurtenis DOMContentloaded niet geblokkeerd. Het blijkt dus dat we de weergaveboomstructuur kunnen opbouwen en zelfs de pagina kunnen kleuren zonder dat er hoeft te worden gewacht op elk item van de pagina: **niet alle bronnen zijn essentieel om de snelle eerste weergave te bieden**. Wanneer we praten over het kritieke weergavepad, hebben we het normaal gesproken over de HTML-opmaak, CSS en JavaScript. Afbeeldingen blokkeren de initiële weergave van de pagina niet, hoewel we natuurlijk wel moeten zorgen dat de afbeeldingen ook zo snel mogelijk worden weergegeven.<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

De gebeurtenis `load` (ook bekend als `onload`) wordt geblokkeerd bij de afbeelding: DevTools geeft de `onload`-gebeurtenis aan op 335 ms. Misschien herinnert u zich nog de `onload`-gebeurtenis het punt markeert waarop **alle bronnen** die de pagina nodig heeft, zijn gedownload en verwerkt. Dit is het punt waarop de laadspinner kan stoppen met draaien en dit punt is gemarkeerd met de rode verticale lijn in de waterval.

Onze pagina `Hallo Wereld-ervaring` ziet er misschien eenvoudig uit aan de oppervlakte, maar voordat de pagina wordt weergeven, worden er een heleboel stappen uitgevoerd die u niet kunt zien. In de praktijk hebben we ook meer nodig dan alleen de HTML: het is waarschijnlijk dat we een CCS-stijldocument en één of meer scripts hebben om enige interactie aan onze pagina toe te voegen. Laten beide aan de mix toevoegen en kijken wat er gebeurt:

That said, the `load` event (also known as `onload`), is blocked on the image: DevTools reports the `onload` event at 335ms. Recall that the `onload` event marks the point at which **all resources** that the page requires have been downloaded and processed; at this point, the loading spinner can stop spinning in the browser (the red vertical line in the waterfall).

## JavaScript en CSS toevoegen aan de mix

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

* Anders dan bij ons eenvoudige HTML-voorbeeld moet nu ook het CSS-bestand worden opgehaald en geparseerd om het CSSOM op te bouwen, en we weten dat we zowel het DOM als het CSSOM nodig hebben om de weergaveboomstructuur op te bouwen.
* Omdat we ook een parserblokkerend JavaScript-bestand in onze pagina hebben staan, wordt de gebeurtenis DOMContentLoaded geblokkeerd totdat het CSS-bestand is gedownload en geparseerd: het JavaScript kan het CSSOM opvragen, dus moet de opbouw worden geblokkeerd en wordt er gewacht op het CSS-bestand totdat we het JavaScript kunnen uitvoeren.

**What if we replace our external script with an inline script?** Even if the script is inlined directly into the page, the browser can't execute it until the CSSOM is constructed. In short, inlined JavaScript is also parser blocking.

That said, despite blocking on CSS, does inlining the script make the page render faster? Let's try it and see what happens.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Parserblokkerend (extern) JavaScript:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, JS" />

We are making one less request, but both our `onload` and `domContentLoaded` times are effectively the same. Why? Well, we know that it doesn't matter if the JavaScript is inlined or external, because as soon as the browser hits the script tag it blocks and waits until the CSSOM is constructed. Further, in our first example, the browser downloads both CSS and JavaScript in parallel and they finish downloading at about the same time. In this instance, inlining the JavaScript code doesn't help us much. But there are several strategies that can make our page render faster.

First, recall that all inline scripts are parser blocking, but for external scripts we can add the "async" keyword to unblock the parser. Let's undo our inlining and give that a try:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Er is ook nog een andere aanpak die we kunnen proberen: zowel CCS als JavaScript inline toevoegen:

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

De meest eenvoudige pagina bestaat enkel uit HTML-opmaak: geen CSS, geen JavaScript of andere soorten bronnen. Om deze pagina weer te geven moet de browser de aanvraag starten, wachten tot het HTML-bestand binnenkomt, dit parseren, het DOM opbouwen en tot slot de pagina weergeven op het scherm:

Alternatively, we could have inlined both the CSS and JavaScript:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**De tijd tussen T<sub>0</sub> en T<sub>1</sub> legt de netwerk- en serververwerkingstijden vast.** In het beste geval (als het HTML-bestand klein is) hebben we slechts één netwerkroundtrip nodig om het volledige document op te halen. Vanwege de manier waarop TCP-transportprotocols werken, kunnen voor grotere bestanden meerdere roundtrips nodig zijn. Dit is een onderwerp waar we in een latere les op terug zullen komen. **Daarom kunnen we zeggen dat de pagina hierboven, in het beste geval één roundtrip (minimum) kritiek weergavepad heeft.**

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" />

Notice that the `domContentLoaded` time is effectively the same as in the previous example; instead of marking our JavaScript as async, we've inlined both the CSS and JS into the page itself. This makes our HTML page much larger, but the upside is that the browser doesn't have to wait to fetch any external resources; everything is right there in the page.

Op deze manier hebben we weer een netwerkroundtrip nodig om het HTML-document op te halen en vervolgens vertelt de opgehaalde opmaak ons dat we ook het CSS-bestand nodig hebben: dit betekent dat de browsers terug naar de server moet gaan en het CSS moet ophalen voordat de pagina op het scherm kan worden weergegeven. **Het gevolg is dat de pagina minimaal twee roundtrips nodig heeft voordat deze kan worden weergegeven.** Hier geldt weer dat het CSS-bestand meerdere roundtrips nodig kan hebben, vandaar met nadruk op `minimaal`.

Laten we de termen definiëren die we gebruiken om het kritieke weergavepad te beschrijven:

## Prestatiepatronen

Laten we dit vergelijken met de kritieke padkenmerken van het HTML- en CSS-voorbeeld hierboven:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

<img src="images/analysis-dom.png" alt="Hello world CRP" />

Laten we nu een extra JavaScript-bestand aan de mix toevoegen.

We hebben app.js toegevoegd. Dit is een extern JavaScript-item op de pagina en zoals we weten, is dit een parserblokkerende (dat wil zeggen kritieke) bron. Het is zelfs nog erger: om het JavaScript-bestand uit te voeren, wordt het proces weer geblokkeerd terwijl we wachten op het CSSOM. Onthoud dat JavaScript het CSSOM kan aanvragen en dat de browser daarom pauzeert totdat `style.css` is gedownload en het CSSOM is opgebouwd.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

We hebben nu drie kritieke bronnen die in het totaal 11 KB aan kritieke bytes opleveren, maar onze kritieke padlengte is nog altijd twee roundtrips omdat het CSS en het JavaScript parallel kunnen worden overdragen. **Het uitpluizen van de kenmerken van uw kritieke weergavepad betekent dat u kunt identificeren wat kritieke bronnen zijn en ook begrijpt hoe de browser het ophalen hiervan inplant.** Laten we verder gaan met ons voorbeeld...

Na het gesprek met de site-ontwikkelaars begrepen we dat het JavaScript dat we aan onze pagina hebben toegevoegd, niet blokkerend hoeft te zijn: we hebben een aantal analyses en andere code tot onze beschikking die de weergave van onze pagina helemaal niet hoeven te blokkeren. Nu we dit weten, kunnen we het kenmerk 'async' aan de scripttag toevoegen om de parser te deblokkeren:

* **Kritieke bron:** bron die de initiële weergave van de pagina kan blokkeren.
* **Kritieke padlengte:** aantal roundtrips of de totale tijd die vereist is om alle kritieke bronnen op te halen.
* **Kritieke bytes:** totaal aantal bytes dat vereist is om een eerste weergave van de pagina te krijgen, dit betekent de som van de grootte van alle opgehaalde bestanden voor alle kritieke bronnen. Ons eerste voorbeeld met een enkele HTML-pagina bevat een enkele kritieke bron (het HTML-bestand), de kritieke padlengte was ook gelijk aan één netwerkroundtrip (waarbij we aannemen dat het bestand klein is) en het totale aantal kritieke bytes was alleen de overdrachtsgrootte van het HTML-document zelf.

Now let's compare that to the critical path characteristics of the HTML + CSS example above:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** kritieke bronnen
* **2** of meer roundtrips voor de minimale lengte voor het kritieke pad
* **9** KB aan kritieke bytes

Hierdoor heeft onze geoptimaliseerde pagina opnieuw maar twee kritieke bronnen (HTLM en CSS) met een kritieke padlengte van minimaal twee roundtrips en totaal 9 KB aan kritieke bytes.

Laten we tot slot zeggen dat het CSS-stijlblad alleen nodig was voor het afdrukken. Wat zou dat betekenen?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

Omdat de bron style.css alleen gebruikt wordt voor afdrukken, hoeft de browser hierbij niet te blokkeren om de pagina weer te geven. Daarom heeft de browser zodra de DOM-opbouw voltooid is, voldoende informatie om de pagina weer te geven. De pagina heeft hierdoor maar één kritieke bron (het HTML-document) en de minimale kritieke padlengte is één roundtrip.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

That said, in practice if we look at this page's "network waterfall," you'll see that both the CSS and JavaScript requests are initiated at about the same time; the browser gets the HTML, discovers both resources, and initiates both requests. As a result, the above page has the following critical path characteristics:

* **3** kritieke bronnen
* **2** of meer roundtrips voor de minimale lengte voor het kritieke pad
* **11** KB aan kritieke bytes

We now have three critical resources that add up to 11KB of critical bytes, but our critical path length is still two roundtrips because we can transfer the CSS and JavaScript in parallel. **Figuring out the characteristics of your critical rendering path means being able to identify the critical resources and also understanding how the browser will schedule their fetches.** Let's continue with our example.

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

An asynchronous script has several advantages:

* Het script is niet langer parserblokkerend en is ook geen onderdeel van het kritieke weergavepad
* Omdat er geen andere kritieke scripts zijn, hoeft het CSS de gebeurtenis `domContentLoaded` niet te blokkeren
* Hoe sneller de gebeurtenis domContentLoaded wordt gestart, hoe eerder de overige app-logistiek kan worden uitgevoerd

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