project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: JavaScript biedt ons de mogelijkheid om vrijwel elk onderdeel van de pagina aan te passen: inhoud, vormgeving en de reactie op interactie van gebruikers. Maar JavaScript kan ook de DOM-opbouw blokkeren en vertragen wanneer een pagina wordt weergegeven. Zorg dat uw JavaScript asynchroon is en verwijder alle onnodige JavaScript uit het kritieke weergavepad om optimale prestaties te leveren.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2013-12-31 #}

# Interactiviteit toevoegen met JavaScript {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

JavaScript biedt ons de mogelijkheid om vrijwel elk onderdeel van de pagina aan te passen: inhoud, vormgeving en de reactie op interactie van gebruikers. Maar JavaScript kan ook de DOM-opbouw blokkeren en vertragen wanneer een pagina wordt weergegeven. Zorg dat uw JavaScript asynchroon is en verwijder alle onnodige JavaScript uit het kritieke weergavepad om optimale prestaties te leveren.

### TL;DR {: .hide-from-toc }

* JavaScript kan DOM en CSSOM opvragen en aanpassen.
* JavaScript-uitvoer wordt geblokkeerd bij CSSOM.
* JavaScript blokkeert DOM-opbouw tenzij expliciet aangegeven als asynchroon.

JavaScript is een dynamische taal die in de browser wordt uitgevoerd. Met JavaScript kan vrijwel elk onderdeel van het gedrag van de pagina worden aangepast: we kunnen inhoud op de pagina wijzigen door elementen uit de DOM-boomstructuur te verwijderen of eraan toe te voegen, we kunnen de CSSOM-eigenschappen van elk element aanpassen, we kunnen gebruikersinvoer verwerken en nog veel meer. Laten we ter verduidelijking ons eerdere voorbeeld `Hallo wereld` aanvullen met een eenvoudig inline script:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/script.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/script.html){: target="_blank" .external }

* Met JavaScript kunnen we in de DOM werken en de referentie naar de verborgen span-node eruit halen. Deze node is misschien niet zichtbaar in de weergaveboomstructuur, maar de node zit nog wel in het DOM. Zodra we deze referentie hebben, kunnen we de tekst ervan (via .textContent) wijzigen en zelfs de berekende weergavestijleigenschap overschrijven van `none` naar `inline`. En wanneer we klaar zijn, zal onze pagina het volgende weergeven: `**Hallo interactieve studenten!**`.

* Met JavaScript kunnen we ook nieuwe elementen in het DOM maken, opmaak geven en bijvoegen of verwijderen. Onze gehele pagina zou technisch gezien één groot JavaScript-bestand kunnen zijn waarmee de elementen één voor één worden gemaakt en opgemaakt. Dat zou natuurlijk kunnen, maar werken met HTML en CSS is in de praktijk veel eenvoudiger. In het tweede deel van de JavaScript-functie zullen we een nieuw div-element maken, de tekstinhoud instellen, het element opmaken en bijvoegen in de body.

<img src="images/device-js-small.png"  alt="page preview" />

Maar hieronder ligt wel een grote prestatieslurper verborgen. JavaScript biedt ons veel meer mogelijkheden, maar het creëert ook veel extra beperkingen op wanneer en hoe een pagina wordt weergegeven.

Zoals u kunt zien, staat ons inline script in het voorbeeld hierboven bijna helemaal onderaan de pagina. Waarom? Eigenlijk zou u het zelf moeten uitproberen, maar als we het script verplaatsen naar boven het element *span*, zult u merken dat het script mislukt en een melding geeft dat het geen enkele referentie naar de elementen *span* in het document kan vinden, dat wil zeggen *getElementsByTagName('span')* geeft *null* als antwoord. Dit demonstreert een belangrijke eigenschap: ons script wordt uitgevoerd op het precieze punt waar dit in het document wordt ingevoegd. Wanneer de HTML-parser een scripttag tegenkomt, wordt het proces van de DOM-opbouw gepauzeerd en krijgt de JavaScript-engine de controle. Zodra de JavaScript-engine klaar is, neemt de browser het weer over op het punt waar deze gebleven was en gaat verder met de DOM-opbouw.

In andere woorden: onze scriptblokkering kan geen elementen verderop in de pagina vinden, omdat deze nog niet verwerkt zijn. Of nog anders gezegd: **het uitvoeren van ons inline script blokkeert de DOM-opbouw, waardoor de initiële weergave van de pagina ook wordt vertraagd.**

Een andere subtiele eigenschap van het introduceren van scripts in onze pagina is dat deze niet alleen de DOM-eigenschappen kunnen lezen en aanpassen, maar ook de CSSOM-eigenschappen. Dat is precies wat we doen in ons voorbeeld op het moment dat we de eigenschap `display` van het span-element van `none` naar `inline` hebben gewijzigd. Het eindresultaat? We zitten nu in een vreemde situatie.

Wat als de browser nog niet klaar is met het downloaden en opbouwen van het CSSOM wanneer wij ons script willen uitvoeren? Het antwoord is simpel en niet goed voor de prestaties: **de browser vertraagt de scriptuitvoer totdat deze klaar is met het downloaden en opbouwen van het CSSOM en terwijl we wachten, is de DOM-opbouw ook geblokkeerd.**

Kortom, JavaScript introduceert veel nieuwe afhankelijkheden tussen het DOM, CSSOM en de JavaScript-uitvoer en kan leiden tot aanzienlijke vertraging in hoe snel de browser onze pagina kan verwerken en weergeven:

Wanneer we het hebben over `het optimaliseren van het kritieke weergavepad`, hebben we het vooral over het begrijpen en optimaliseren van het afhankelijkheidsschema tussen HTML, CSS en JavaScript.

* The location of the script in the document is significant.
* When the browser encounters a script tag, DOM construction pauses until the script finishes executing.
* JavaScript can query and modify the DOM and the CSSOM.
* JavaScript execution pauses until the CSSOM is ready.

De JavaScript-uitvoer is standaard `parserblokkerend`: wanneer de browser een script in het document tegenkomt, moet deze de DOM-opbouw pauzeren, de controle aan de JavaScript-runtime geven en het script laten uitvoeren, voordat de browser verder kan met de DOM-opbouw. We hebben dit al in actie gezien bij het inline script uit ons eerdere voorbeeld. Inline scripts zijn feitelijk altijd parserblokkerend, tenzij u speciaal extra code schrijft om de uitvoer uit te stellen.

## Parserblokkering versus asynchroon JavaScript

Hoe zit het met scripts die via een scripttag zijn toegevoegd? Laten we ons eerdere voorbeeld nemen en onze code in een apart bestand weergeven:

What about scripts included via a script tag? Let's take our previous example and extract the code into a separate file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/split_script.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**app.js**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/app.js" region_tag="full" adjust_indentation="auto" %}
</pre>

Gelukkig is er ook goed nieuws: er is een noodoplossing. JavaScript is standaard parserblokkerend en de browser weet niet wat het script zal gaan doen op de pagina, daarom zal de browser uitgaan van het ergste scenario en wordt de parser geblokkeerd. Maar wat zou er gebeuren als we de browser een seintje konden geven en vertellen dat het script niet op het precieze punt hoeft te worden uitgevoerd waarop ernaar gerefereerd wordt in het document? Hierdoor kan de browser doorgaan met het opbouwen van het DOM en het script uitvoeren wanneer de opbouw klaar is, dat wil zeggen wanneer het bestand is opgehaald uit het cache of van een externe server.

Dus de vraag is, hoe krijgen we dit voor elkaar? Het is eigenlijk vrij simpel, we kunnen ons script markeren als _async:

Het toegevoegde sleutelwoord `async` aan de scripttag vertelt de browser dat deze de DOM-opbouw niet moet blokkeren terwijl er wordt gewacht tot het script beschikbaar is. En dat zorgt voor een enorm verschil in de prestatie.

To achieve this, we mark our script as *async*:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/split_script_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/split_script_async.html){: target="_blank" .external }

Adding the async keyword to the script tag tells the browser not to block DOM construction while it waits for the script to become available, which can significantly improve performance.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}