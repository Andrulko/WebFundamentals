project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Er moeten drie variabelen worden geoptimaliseerd om de snelste initiële weergavetijd te leveren: minimaliseer het aantal kritieke bronnen, minimaliseer het aantal kritieke bytes en minimaliseer het kritieke weergavepad.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Het kritieke weergavepad optimaliseren {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Er moeten drie variabelen worden geoptimaliseerd om de snelste initiële weergavetijd te leveren:

- The number of critical resources.
- The critical path length.
- The number of critical bytes.

Een kritieke bron is elke bron die de initiële weergave van een pagina kan blokkeren. Hoe minder van dit soort bronnen een pagina heeft, hoe minder werk de browser moet verrichten om de inhoud op het scherm te krijgen en hoe minder concurrentie er is voor de CPU en andere bronnen.

Op vergelijkbare wijze betekent hoe minder kritieke bytes de browser moet downloaden, hoe sneller de browser kan beginnen met het verwerken van de inhoud en deze weer te geven. We kunnen het aantal bronnen verminderen (door deze te elimineren of niet-kritiek te maken) en zorgen dat de overdrachtsgrootte wordt geminimaliseerd door elke bron te comprimeren en te optimaliseren om het aantal bytes te verminderen.

Tot slot is de kritieke padlengte een functie van het afhankelijkheidsschema tussen alle kritieke bronnen (en de bytegrootte van deze bronnen) die de pagina nodig heeft: bepaalde brondownloads kunnen alleen worden geïnitieerd als de vorige bron is verwerkt en hoe groter de bron is, hoe meer roundtrips er nodig zijn om deze te downloaden.

**The general sequence of steps to optimize the critical rendering path is:**

1. Uw kritieke pad analyseren en karakteriseren: aantal bronnen, bytes, lengte.
2. Aantal kritieke bronnen minimaliseren: bronnen elimineren, brondownloads uitstellen, bronnen markeren als asynchroon, enzovoort.
3. De volgorde waarin de overgebleven kritieke bronnen worden geladen: u wilt alle kritieke items zo snel mogelijk downloaden om de kritieke padlengte te verkorten.
4. Het aantal kritieke bytes optimaliseren om de downloadtijd te verkorten (aantal roundtrips).

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}