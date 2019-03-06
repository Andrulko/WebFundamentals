project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Een foto is evenveel waard als 1000 woorden. Afbeeldingen vormen dan ook een integraal onderdeel van elke pagina. Maar het downloaden ervan kost veel bytes. Met responsive webdesign kunnen niet alleen onze lay-outs veranderen op basis van apparaatkenmerken, maar ook afbeeldingen.

{# wf_updated_on: 2018-12-15 #} {# wf_published_on: 2000-01-01 #}

# Afbeeldingen {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Een foto is evenveel waard als 1000 woorden. Afbeeldingen vormen dan ook een integraal onderdeel van elke pagina. Maar het downloaden ervan kost veel bytes. Met responsive webdesign kunnen niet alleen onze lay-outs veranderen op basis van apparaatkenmerken, maar ook afbeeldingen.

## Afbeeldingen in opmaak

<img src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Other times the image may need to be changed more drastically: changing the proportions, cropping, and even replacing the entire image. In this case, changing the image is usually referred to as art direction. See [responsiveimages.org/demos/](https://responsiveimages.org/demos/) for more examples.

Soms moeten afbeeldingen misschien wat drastischer worden gewijzigd: de proporties wijzigen, bijsnijden of zelfs de afbeelding geheel vervangen. Het wijzigen van afbeeldingen wordt dan meestal `art direction` genoemd. Zie [responsiveimages.org/demos/](http://responsiveimages.org/demos/){: .external } voor meer voorbeelden.

## Afbeeldingen in CSS 

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

### Responsieve afbeeldingen

* Gebruik relatieve grootten voor afbeeldingen zodat deze niet per abuis tot buiten de randen van de container overlopen.
* Gebruik het element `picture` als u afhankelijk van de kenmerken van een apparaat andere afbeeldingen wilt opgeven (dit wordt ook wel art direction genoemd).
* Gebruik `srcset` en de descriptor `x` in het element `img` om de browser te helpen de beste afbeelding te kiezen wanneer er uit verschillende dichtheden moet worden gekozen.
* If your page only has one or two images and these are not used elsewhere on your site, consider using inline images to reduce file requests.

### Art direction

Het element `img` is een krachtig element. U kunt er inhoud mee downloaden, decoderen en weergeven. Door moderne browsers worden veel verschillende afbeeldingsindelingen ondersteund. Het toevoegen van afbeeldingen voor mobiele apparaten gebeurt in principe op dezelfde manier als voor desktopcomputers. Er zijn slechts een paar kleine aanpassingen nodig om een goede ervaring voor mobiele gebruikers te kunnen realiseren.

Vergeet niet om relatieve eenheden te gebruiken bij het opgeven van de breedte van afbeeldingen zodat deze niet per abuis tot buiten de randen van de viewport overlopen. Bijvoorbeeld `width: 50%;` zorgt ervoor dat de breedte van de afbeelding 50% van het omvattende element is (niet de viewport of de feitelijke pixelgrootte).

    img, embed, object, video {
      max-width: 100%;
    }
    

Aangezien CSS inhoud toestaat over te lopen tot buiten de begrenzing van de container, kan het nodig zijn max-width: 100% te gebruiken, zodat afbeeldingen en andere inhoud binnen de container blijven. Bijvoorbeeld:

### TL;DR {: .hide-from-toc }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="Pzc5Dly_jEM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Zorg voor zinvolle beschrijvingen via het kenmerk `alt` in `img`-elementen. Zo maakt u uw site beter toegankelijk door schermlezers en andere ondersteunende technologieën een context te bieden.

<div style="clear:both;">
</div>

    <img src="photo.png" srcset="photo@2x.png 2x" ...>
    

Het kenmerk `srcset` verbetert het gedrag van het element `img`, waardoor het eenvoudiger wordt om meerdere afbeeldingsbestanden te leveren voor verschillende apparaatkenmerken. Net zoals de bij CSS behorende `image-set` [CSS-functie](#use_image-set_to_provide_high_res_images), stelt `srcset` de browser in staat de beste afbeelding te kiezen afhankelijk van de kenmerken van het apparaat, bijvoorbeeld een 2x afbeelding gebruiken op een 2x-scherm en in de toekomst wellicht een 1x afbeelding op een 2x-apparaat in een netwerk met beperkte bandbreedte.

Op browsers die `srcset` niet ondersteunen, maakt de browser gewoon gebruik van het standaard-afbeeldingsbestand dat door het kenmerk `src` is opgegeven. Daarom is het belangrijk altijd een 1x afbeelding op te nemen die op elk willekeurig apparaat kan worden weergegeven, los van de capaciteiten. Wanneer `srcset` wordt ondersteund, wordt de lijst met door komma`s gescheiden waarden met afbeeldingen/voorwaarden geparseerd voordat er verzoeken worden gedaan. Vervolgens wordt alleen de meest geschikte afbeelding gedownload en weergegeven.

### Relatieve formaten gebruiken voor afbeeldingen

<img class="attempt-right" src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Het wijzigen van afbeeldingen op basis van apparaatkenmerken, ook wel `art direction` genoemd, kan worden gedaan met behulp van het element picture. Met het element `picture` wordt een declaratieve oplossing gedefinieerd voor het verkrijgen van meerdere versies van een afbeelding op basis van verschillende kenmerken, zoals het apparaatformaat, de apparaatresolutie, oriëntatie, enzovoort.

<div style="clear:both;">
</div>

Dogfood: The `picture` element is beginning to land in browsers. Although it's not available in every browser yet, we recommend its use because of the strong backward compatibility and potential use of the [Picturefill polyfill](https://scottjehl.github.io/picturefill/). See the [ResponsiveImages.org](http://responsiveimages.org/#implementation) site for further details.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="QINlm3vjnaY"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Note: Het element `picture` wordt langzaamaan steeds meer toegepast in browsers. Hoewel het nog niet in iedere browser beschikbaar is, bevelen we het gebruik hiervan aan vanwege de sterke terugwaartse compatibiliteit en het mogelijke gebruik van de [Picturefill polyfill](https://scottjehl.github.io/picturefill/). Zie voor meer informatie de site [ResponsiveImages.org](http://responsiveimages.org/#implementation).

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/media.html" region_tag="picture" %}
</pre>

Het element `picture` wordt gebruikt als een afbeeldingsbron in meerdere dichtheden voorkomt, of wanneer door een responsief design een iets afwijkende afbeelding op sommige typen scherm wordt afgedwongen. Net zoals bij het element `video` kunnen meerdere `source` -elementen worden opgenomen, waardoor het mogelijk is verschillende afbeeldingsbestanden op te geven, afhankelijk van de mediaquery`s of de afbeeldingsgrootte.

In het bovenstaande voorbeeld wordt, als de breedte van de browser ten minste 800px bedraagt, `head.jpg` of `head-2x.jpg` gebruikt, afhankelijk van de apparaatresolutie. Als de browserbreedte tussen 450px en 800px ligt, dan wordt `head-small.jpg` of `head-small-2x.jpg` gebruikt, maar ook hier is dat weer afhankelijk van de apparaatresolutie. Bij schermbreedten van minder dan 450px en compatibiliteit met eerdere versies, waarbij het element `picture` niet wordt ondersteund, geeft de browser in plaats daarvan het element `img` weer. Dit moet altijd worden gebruikt.

#### Afbeeldingen met relatieve grootte

Als de definitieve grootte van de afbeelding niet bekend is, kan het lastig zijn een dichtheidsdescriptor voor de afbeeldingsbronnen op te geven. Dit geldt vooral voor afbeeldingen die een proportionele breedte van de browser vullen en die vloeiend zijn, afhankelijk van de grootte van de browser.

In plaats van vaste afbeeldingsformaten en dichtheden te produceren, kan de grootte van elke geleverde afbeelding worden opgegeven door een descriptor voor de breedte toe te voegen, samen met de grootte van het afbeeldingselement, waardoor de browser automatisch de effectieve pixeldichtheid kan berekenen en de beste afbeelding kan kiezen om te downloaden.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/sizes.html" region_tag="picture" %}
</pre>

In het voorbeeld hierboven wordt een afbeelding weergegeven die de helft van de breedte van de viewport vult (`sizes="50vw"`), afhankelijk van de breedte van de browser en de pixelverhouding van het apparaat. Dit stelt de browser in staat de juiste afbeelding te kiezen, ongeacht hoe groot het browservenster is. In onderstaande tabel ziet u welke afbeelding de browser zou kiezen:

Vaak kan de grootte of afbeelding veranderen, afhankelijk van de lay-outbreekpunten van de site. Op een klein scherm, bijvoorbeeld, wilt u misschien dat de afbeelding de breedte van de viewport geheel vult, terwijl deze op een groter scherm slechts een klein gedeelte in beslag zou nemen.

<table class="">
  <tr>
    <th data-th="Browser width">
      Browserbreedte
    </th>
    
    <th data-th="Device pixel ratio">
      Pixelverhouding apparaat
    </th>
    
    <th data-th="Image used">
      Gebruikte afbeelding
    </th>
    
    <th data-th="Effective resolution">
      Effectieve resolutie
    </th>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400px
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
      400px
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
      2,5x
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
      640px
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
      1,27x
    </td>
  </tr>
</table>

#### Rekening houden met breekpunten in responsieve afbeeldingen

Het kenmerk `sizes` in het bovenstaande voorbeeld maakt gebruik van diverse mediaquery`s om de grootte van de afbeelding aan te geven. Als de browserbreedte groter is dan 600px, is de afbeelding 25% van de breedte van de viewport, bij een breedte van 500px tot 600px, is de afbeelding 50% van de breedte van de viewport en onder 500px heeft de afbeelding de volle breedte.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/breakpoints.html" region_tag="picture" %}
</pre>

Klanten willen zien wat ze kopen. Op sites van online winkels verwachten gebruikers dat ze close-ups in hoge resolutie van producten kunnen bekijken om de details beter te kunnen zien en [deelnemers aan het onderzoek](/web/fundamentals/getting-started/principles/) raakten geïrriteerd als ze dat niet konden doen.

Een goed voorbeeld van uitbreidbare afbeeldingen waarop gebruikers kunnen tikken is te zien op de site van J. Crew. Een verdwijnende overlay geeft aan dat op een afbeelding kan worden getikt, waarna een ingezoomde afbeelding met details zichtbaar wordt.

### Breid `img`-elementen uit met `srcset` voor high-DPI apparaten<figure class="attempt-right"> 

<img src="img/sw-make-images-expandable-good.png" srcset="img/sw-make-images-expandable-good.png 1x, img/sw-make-images-expandable-good-2x.png 2x" alt="Website van J. Crew met uitbreidbare productafbeelding" /> <figcaption>Website van J. Crew met uitbreidbare productafbeelding.</figcaption> </figure> 

De [techniek voor afbeeldingscompressie](http://www.html5rocks.com/en/mobile/high-dpi/#toc-tech-overview) zorgt voor sterk gecomprimeerde 2x afbeeldingen op alle apparaten, ongeacht de feitelijke mogelijkheden van het apparaat. Afhankelijk van het type afbeelding en het compressieniveau, lijkt de afbeeldingskwaliteit misschien niet te veranderen, maar de bestandsgrootte wordt wel veel kleiner.

A good example of tappable, expandable images is provided by the J. Crew site. A disappearing overlay indicates that an image is tappable, providing a zoomed in image with fine detail visible.

<div style="clear:both;">
</div>

### Art direction in responsieve afbeeldingen met `picture`

#### Gecomprimeerde afbeeldingen

Note: Ga verstandig om met de compressietechniek omdat deze hogere geheugen- en decoderingskosten met zich meebrengt. Het aanpassen van het formaat van grote afbeeldingen zodat ze op een kleiner scherm passen is duur en kan vooral op low-end apparaten lastig zijn omdat zowel het geheugen als de verwerkingsmogelijkheden hierop beperkt zijn.

Met JavaScript-afbeeldingsvervanging worden de mogelijkheden van het apparaat gecontroleerd en worden `de juiste afbeeldingen` gekozen. U kunt zelf de pixelverhouding van het apparaat bepalen via `window.devicePixelRatio`, de schermbreedte en -hoogte ophalen en mogelijk zelfs de netwerkverbinding controleren via `navigator.connection` of een vals verzoek uitgeven. Nadat u al deze informatie heeft verzameld, kunt u bepalen welke afbeelding u wilt laden.

Eén groot nadeel van deze benadering is dat u door het gebruik van JavaScript het laden van afbeeldingen uitstelt totdat op zijn minst de look-ahead parser gereed is. Dit betekent dat het downloaden van afbeeldingen pas begint nadat de gebeurtenis `pageload` is gestart. Daarnaast is de kans groot dat de browser zowel de 1x als de 2x afbeeldingen zal downloaden, waardoor de pagina zwaarder wordt.

#### JavaScript-afbeeldingsvervanging

De CSS-eigenschap `background` is een krachtige tool waarmee u complexe afbeeldingen aan elementen kunt toevoegen en gemakkelijk meerdere afbeeldingen toevoegt, ze kunt laten terugkomen, enzovoort. In combinatie met mediaquery's wordt de eigenschap `background` nog krachtiger, waardoor het voorwaardelijke laden van afbeeldingen op basis van onder andere schermresolutie en de grootte van de viewport mogelijk wordt.

Mediaquery`s beïnvloeden niet alleen de paginalay-out, maar kunnen ook worden gebruikt om afbeeldingen voorwaardelijk te laden of om art direction te verzorgen, afhankelijk van de breedte van de viewport.

#### Inlining images: raster and vector

In het onderstaande voorbeeld wordt op kleine schermen alleen `small.png` gedownload en toegepast op de inhoud-`div`, terwijl op grote schermen `background-image: url(body.png)` wordt toegepast op de hoofdtekst en `background-image: url(large.png)` op de inhoud-`div`.

Met de functie `image-set()` in CSS wordt de gedragseigenschap `background` verbeterd, waardoor het gemakkelijker wordt meerdere afbeeldingsbestanden voor verschillende apparaatkenmerken te maken. Zo kan de browser de beste afbeelding kiezen, afhankelijk van de kenmerken van het apparaat. Bijvoorbeeld een 2x afbeelding op een 2x scherm, of een 1x afbeelding op een 2x apparaat als het een netwerk met beperkte bandbreedte betreft.

De browser zal niet alleen de juiste afbeelding laden, maar deze ook correct schalen. Met andere woorden, de browser gaat er vanuit dat 2x afbeeldingen tweemaal zo groot zijn als 1x afbeeldingen en zal om die reden het 2x bestand omlaag schalen met een factor 2, waardoor de afbeelding op de papina dezelfde grootte lijkt te hebben.

##### SVG

Er is nog maar sinds kort ondersteuning voor `image-set()` en dit wordt alleen ondersteund in Chrome en Safari met het voorvoegsel `-webkit` van de leverancier. U moet er ook voor zorgen dat u een reserve-afbeelding toevoegt voor het geval `image-set()` niet wordt ondersteund, bijvoorbeeld:

De juiste asset wordt in browsers geladen die image-set ondersteunen, en kunnen bij gebrek aan ondersteuning terugvallen op de 1x asset. De logische restrictie is dat zolang `image-set()` browserondersteuning laag is, de meeste browsers de 1x asset zullen ontvangen.

<img class="side-by-side" src="img/html5.png" alt="HTML5 logo, PNG format" /> <img class="side-by-side" src="img/html5.svg" alt="HTML5 logo, SVG format" />

Chrome, Firefox en Opera ondersteunen alledrie de standaard `(min-resolution: 2dppx)`, terwijl voor zowel Safari als Android Browser de oudere syntax met voorvoegsel van de leverancier vereist is zonder de eenheid `dppx`. Houd er rekening mee dat deze stijlen alleen worden geladen als het apparaat overeenkomt met de mediaquery, en dat u stijlen moet opgeven voor de basiscasus. Dit biedt ook het voordeel dat er in ieder geval iets wordt weergegeven in het geval de browser geen resolutiespecifieke mediaquery`s ondersteunt.

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

Gebruik bij het toevoegen van pictogrammen aan uw pagina waar mogelijk SVG-pictogrammen of, in bepaalde gevallen, unicode-tekens.

<svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg><svg class="side-by-side" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" width="50%" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5"/><path fill="#E44D26" d="M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z"/><polygon fill="#F16529" points="480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2"/><polygon fill="#F16529" points="541,343 480,343 480,422.4 535.2,407.4"/><polygon fill="#EBEBEB" points="414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4"/><polygon fill="#EBEBEB" points="479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474"/><polygon points="343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5"/><polygon points="425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25"/><polygon points="508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5"/><polygon points="642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5"/><polygon fill="#FFFFFF" points="480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1"/><polygon fill="#FFFFFF" points="479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7"/></g></g></g></svg>

##### Data URI

Behalve de gebruikelijke tekenset omvat unicode soms ook symbolen voor getaltypen (&#8528;), pijlen (&#8592;), wiskundige bewerkingstekens (&#8730;), geometrische vormen (&#9733;), besturingsafbeeldingen (&#9654;), braillepatronen (&#10255;), muzieknotatie (&#9836;), Griekse letters (&#937;) en zelfs schaakstukken (&#9822;).

    <img src="data:image/svg+xml;base64,[data]">
    

Het toevoegen van een unicode-teken gebeurt op dezelfde manier als bij entiteiten met een naam: `&#XXXX`, waarbij `XXXX` staat voor het nummer van het unicode-teken. Bijvoorbeeld:

    background-image: image-set(
      url(icon1x.jpg) 1x,
      url(icon2x.jpg) 2x
    );
    

U bent een super &#9733;

Voor meer complexe pictogramvereisten zijn SVG-pictogrammen over het algemeen licht, gebruiksvriendelijk en geschikt voor styling met CSS. Vergeleken bij rasterafbeeldingen biedt SVG een aantal voordelen:

##### Inlining in CSS

Pictogramlettertypen zijn populair en kunnen soms handig zijn, maar vergeleken bij SVG-pictogrammen kennen ze ook enkele nadelen.

<span class="side-by-side" id="data_uri"></span> <span class="side-by-side" id="svg"></span>

##### Inlining pros & cons

Er bestaand honderden gratis en betaalde pictogramlettertypen, zoals [Font Awesome](http://fortawesome.github.io/Font-Awesome/){: .external }, [Pictos](http://pictos.cc/) en [Glyphicons](http://glyphicons.com/).

Weeg de zwaarte van het extra HTTP-verzoek en de bestandsgrootte af tegen de noodzaak van pictogrammen. Als u bijvoorbeeld maar een paar pictogrammen nodig heeft, kunt u beter gewoon een afbeelding of afbeeldingssprite gebruiken.

* Gebruik de beste afbeelding voor de kenmerken van de display, houd rekening met het formaat van het scherm, de resolutie van het apparaat en de paginalay-out.
* Wijzig de eigenschap `background-image` in CSS voor high-DPI-beeldschermen via mediaquery`s met `min-resolution` en `-webkit-min-device-pixel-ratio`.
* Gebruik srcset voor afbeeldingen met hoge resolutie naast de 1x afbeelding in opmaak.
* Houd rekening met de prestatiekosten wanneer u JavaScript-technieken gebruikt voor vervanging van afbeeldingen of wanneer u zwaar gecomprimeerde afbeeldingen met hoge resolutie op apparaten met een lagere resolutie plaatst.
* Data URIs cannot be cached, so must be downloaded for every page they're used on.
* They're not supported in IE 6 and 7, incomplete support in IE8.
* With HTTP/2, reducing the number of asset requests will become less of a priority.

Afbeeldingen zijn meestal verantwoordelijk voor de meeste gedownloade bytes en nemen ook een aanzienlijke hoeveelheid van de visuele ruimte in beslag op de pagina. Het optimaliseren van afbeeldingen kan dan ook een van de grootste besparingen op bytes en prestatieverbeteringen voor uw website opleveren: hoe kleiner het aantal bytes dat de browser moet downloaden, des te minder concurrentie voor de bandbreedte van de client er is en des te sneller de browser alle assets kan downloaden en weergeven.

## SVG gebruiken voor pictogrammen

Er zijn twee typen afbeeldingen waarmee u rekening moet houden: [vectorafbeeldingen](http://en.wikipedia.org/wiki/Vector_graphics){: .external } en [rasterafbeeldingen](http://en.wikipedia.org/wiki/Raster_graphics). Voor rasterafbeeldingen moet u bovendien de juiste compressie-indeling kiezen, bijvoorbeeld: `GIF`, `PNG`, `JPG`.

### Productafbeeldingen uitbreidbaar maken

* Gebruik SVG of unicode voor pictogrammen in plaats van rasterafbeeldingen.
* Change the `background-image` property in CSS for high DPI displays using media queries with `min-resolution` and `-webkit-min-device-pixel-ratio`.
* Use srcset to provide high resolution images in addition to the 1x image in markup.
* Consider the performance costs when using JavaScript image replacement techniques or when serving highly compressed high resolution images to lower resolution devices.

### Andere afbeeldingstechnieken

**Rasterafbeeldingen**, zoals foto`s en andere afbeeldingen die worden voorgesteld als een raster met afzonderlijke puntjes of pixels. Rasterafbeeldingen zijn meestal afkomstig van een camera of scanner, of kunnen in de browser worden gemaakt met het element`canvas`. Naarmate de afbeelding groter wordt, neemt ook het bestand in grootte toe. Als rasterafbeeldingen groter worden geschaald dan hun oorspronkelijke formaat, worden ze vaag omdat de browser moet gissen hoe de ontbrekende pixels moeten worden ingevuld.

**Vectorafbeeldingen**, zoals logo`s en tekeningen worden gedefinieerd door een reeks van curven, strepen, vormen en vulkleuren. Vectorafbeeldingen worden gemaakt met programma`s zoals Adobe Illustrator of Inkscape en opgeslagen in een vectorindeling zoals [`SVG`](http://css-tricks.com/using-svg/){: .external }. Omdat vectorafbeeldingen gebouwd zijn op eenvoudige primitieven, kunnen ze worden geschaald zonder enig kwaliteitsverlies en zonder een wijziging in bestandsgrootte.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/image-set.html" region_tag="imageset" %}
</pre>

Als u de juiste indeling kiest, is het belangrijk om rekening te houden met de oorsprong van de afbeelding (raster of vector), en de inhoud (kleuren, animatie, tekst, enzovoort). Geen enkele indeling is geschikt voor alle afdelingstypen en elke indeling biedt voor- en nadelen.

### TL;DR {: .hide-from-toc }

Start met de volgende richtlijnen om de juiste indeling te kiezen:

    @media (min-resolution: 2dppx),
    (-webkit-min-device-pixel-ratio: 2)
    {
      /* High dpi styles & resources here */
    }
    

De bestandsgrootte van afbeeldingen kan aanzienlijk worden teruggebracht door ze `na te bewerken` na het opslaan. Er bestaan diverse tools voor afbeeldingscompressie - lossy en lossless, online, GUI, opdrachtregel. Waar mogelijk raden we u aan afbeeldingsoptimalisatie te automatiseren zodat dit een grote rol speelt in uw workflow.

Er zijn verschillende tools beschikbaar die verdere, lossless compressie op `JPG`- en `PNG`-bestanden uitvoeren, zonder de afbeeldingskwaliteit aan te tasten. Probeer voor `JPG` [jpegtran](http://jpegclub.org/){: .external } of [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (alleen beschikbaar op Linux; uitvoeren met de optie --strip-all). Probeer voor `PNG` [OptiPNG](http://optipng.sourceforge.net/) of [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" %}
</pre>

CSS spriting is een techniek waarbij een aantal afbeeldingen wordt gecombineerd in een enkele `sprite sheet`-afbeelding. Afzonderlijke afbeeldingen kunnen vervolgens worden gebruikt door de achtergrondafbeelding op te geven voor een element (het spritesheet) plus een offset om het juiste onderdeel weer te geven.

The above loads the appropriate asset in browsers that support image-set; otherwise it falls back to the 1x asset. The obvious caveat is that while `image-set()` browser support is low, most browsers get the 1x asset.

### Gebruik mediaquery`s voor het voorwaardelijk laden van afbeeldingen of voor art direction

Spriting heeft het voordeel dat het aantal downloads wordt verminderd dat vereist is om meerdere afbeeldingen op te halen, terwijl het opslaan in cache mogelijk blijft.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

Met lazy loading kunt u het laadproces van lange pagina`s met veel afbeeldingen onder de vouw aanzienlijk versnellen door ze pas te laden wanneer ze nodig zijn of nadat de primaire inhoud geladen en weergegeven is. Behalve verbeterde prestaties kan het gebruik van lazy loading ook leiden tot oneindig scrollen.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" adjust_indentation="auto" %}
</pre>

Wees voorzichtig wanneer u pagina`s met oneindig scrollen maakt, want inhoud wordt geladen als deze zichtbaar is en het kan zijn dat zoekmachines die inhoud nooit te zien krijgen. Bovendien krijgen gebruikers de voettekst nooit te zien, omdat er voortdurend nieuwe inhoud wordt geladen, terwijl ze de gezochte informatie juist in de voettekst verwachten te vinden.

Soms is de beste afbeelding geen afbeelding. Gebruik waar mogelijk de systeemeigen mogelijkheden van de browser om dezelfde of gelijkwaardige functionaliteit te verkrijgen. Browsers genereren visuele elementen waarvoor vroeger afbeeldingen vereist waren. Dit betekent dat browsers geen afzonderlijke afbeeldingsbestanden meer hoeven te downloaden, waardoor onhandig geschaalde afbeeldingen worden voorkomen. Pictogrammen kunnen worden weergegeven met behulp van unicode of met lettertypen die speciaal voor pictogrammen zijn ontwikkeld.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

## Afbeeldingen optimaliseren ten behoeve van de prestaties

Tekst moet zoveel mogelijk tekst zijn en niet zijn ingesloten in afbeeldingen, bijvoorbeeld afbeeldingen gebruiken als koptekst of het plaatsen van contactgegevens zoals telefoonnummers en adressen direct in de afbeelding. Hierdoor kunnen mensen de informatie niet kopiëren en plakken, is de informatie niet toegankelijk voor schermlezers en is deze ook niet responsief. Plaats de tekst liever in uw markeringen en gebruik eventueel weblettertypen om de gewenste stijl te verkrijgen.

### Afbeeldingsset gebruiken voor het leveren van afbeeldingen met hoge resolutie

* Het zijn vectorafbeeldingen die oneindig kunnen worden geschaald.

### Mediaquery`s gebruiken om afbeeldingen met hoge resolutie of art direction te maken

Moderne browsers kunnen gebruikmaken van CSS-functies voor het maken van stijlen waarvoor vroeger afbeeldingen vereist waren. Zo kunnen complexe kleurovergangen worden gemaakt met de eigenschap `background`, schaduwen met `box-shadow` en afgeronde hoeken kunnen worden toegevoegd met de eigenschap `border-radius`.

Beyond the normal character set, unicode may include symbols for arrows (&#8592;), math operators (&#8730;), geometric shapes (&#9733;), control pictures (&#9654;), music notation (&#9836;), Greek letters (&#937;), even chess pieces (&#9822;).

Houd er rekening mee dat het weergeven van cycli vereist is voor het gebruik van deze technieken, en dat kan belastend zijn voor een mobiele telefoon. Als u dit te vaak doet, kunnen eventuele positieve effecten verdwijnen en de prestaties achteruitgaan.

    You're a super &#9733;
    

You're a super &#9733;

### TL;DR {: .hide-from-toc }

For more complex icon requirements, SVG icons are generally lightweight, easy to use, and can be styled with CSS. SVG have a number of advantages over raster images:

* Het zijn vectorafbeeldingen die oneindig kunnen worden geschaald, maar ze kunnen vloeiend zijn, wat pictogrammen kan opleveren die minder scherp zijn dan verwacht.
* Beperkte styling met CSS.
* Een perfecte positionering van pixels kan lastig zijn, afhankelijk van de regelhoogte, letterafstand, enzovoort.
* Ze zijn niet semantisch en kunnen onhandig zijn voor gebruik met schermlezers of andere ondersteunende technologie.
* Tenzij ze correct zijn gerelateerd aan een bereik, kunnen ze resulteren in een groot bestandsformaat terwijl maar een kleine subset van de beschikbare pictogrammen wordt gebruikt.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-svg.html){: target="_blank" .external }

### Eenvoudige pictogrammen vervangen door unicode<figure class="attempt-right"> 

<img src="img/icon-fonts.png" class="center" srcset="img/icon-fonts.png 1x, img/icon-fonts-2x.png 2x" alt="Example of a page that uses FontAwesome for its font icons." /> <figcaption> <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html" target="_blank" class="external"> Example of a page that uses FontAwesome for its font icons. </a> </figcaption> </figure> 

Icon fonts are popular, and can be easy to use, but have some drawbacks compared to SVG icons:

* Kies niet zomaar een afbeeldingsindeling, maar probeer inzicht te krijgen in de verschillende indelingen die beschikbaar zijn en gebruik de meest geschikte indeling.
* Gebruik ook tools voor afbeeldingsoptimalisatie en compressie in uw workflow om de grootte van bestanden te verkleinen.
* Verlaag het aantal http-verzoeken door veelgebruikte afbeeldingen in afbeeldingssprites te plaatsen.
* Denk eraan dat u afbeeldingen pas laadt nadat ze zichtbaar zijn in de weergave. Zo verbetert u de laadtijd van de eerste pagina en maakt u deze pagina minder zwaar.
* Unless properly scoped, they can result in a large file size for only using a small subset of the icons available.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/icon-font.html" region_tag="iconfont" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html){: target="_blank" .external }

There are hundreds of free and paid icon fonts available including [Font Awesome](https://fortawesome.github.io/Font-Awesome/), [Pictos](http://pictos.cc/){: .external }, and [Glyphicons](https://glyphicons.com/).

Be sure to balance the weight of the additional HTTP request and file size with the need for the icons. For example, if you only need a handful of icons, it may be better to use an image or an image sprite.

## Het gebruik van afbeeldingen geheel vermijden

Images often account for most of the downloaded bytes and also often occupy a significant amount of the visual space on the page. As a result, optimizing images can often yield some of the largest byte savings and performance improvements for your website: the fewer bytes the browser has to download, the less competition there is for client's bandwidth and the faster the browser can download and display all the assets.

### Complexe pictogrammen vervangen door SVG

* Gebruik `JPG` voor foto`s.
* Gebruik `SVG` voor vectorkunst en kleurafbeeldingen zoals logo`s en tekeningen. Probeer WebP of PNG als vectorkunst niet beschikbaar is.
* Liever `PNG` dan `GIF` omdat hiermee meer kleuren mogelijk zijn en het betere compressieverhoudingen biedt.
* Overweeg voor langere animaties het gebruik van `<video>` wat een betere beeldkwaliteit biedt en de gebruiker controle geeft over het afspelen.

### Let goed op bij het gebruik van pictogramlettertypen

There are two types of images to consider: [vector images](https://en.wikipedia.org/wiki/Vector_graphics) and [raster images](https://en.wikipedia.org/wiki/Raster_graphics). For raster images, you also need to choose the right compression format, for example: `GIF`, `PNG`, `JPG`.

**Raster images**, like photographs and other images, are represented as a grid of individual dots or pixels. Raster images typically come from a camera or scanner, or can be created in the browser with the `canvas` element. As the image size gets larger, so does the file size. When scaled larger than their original size, raster images become blurry because the browser needs to guess how to fill in the missing pixels.

**Vector images**, such as logos and line art, are defined by a set of curves, lines, shapes, and fill colors. Vector images are created with programs like Adobe Illustrator or Inkscape and saved to a vector format like [`SVG`](https://css-tricks.com/using-svg/). Because vector images are built on simple primitives, they can be scaled without any loss in quality or change in file size.

When choosing the appropriate format, it is important to consider both the origin of the image (raster or vector), and the content (colors, animation, text, etc). No one format fits all image types, and each has its own strengths and weaknesses.

Start with these guidelines when choosing the appropriate format:

* Vermijd waar mogelijk het gebruik van afbeeldingen. Benut liever de mogelijkheden die de browser biedt voor het maken van schaduwen, kleurovergangen, afgeronde hoeken, enzovoort.
* Use `SVG` for vector art and solid color graphics such as logos and line art. If vector art is unavailable, try `WebP` or `PNG`.
* Use `PNG` rather than `GIF` as it allows for more colors and offers better compression ratios.
* For longer animations consider using `<video>`, which provides better image quality and gives the user control over playback.

### TL;DR {: .hide-from-toc }

You can reduce image file size considerably by "post-processing" the images after saving. There are a number of tools for image compression&mdash;lossy and lossless, online, GUI, command line. Where possible, it's best to try automating image optimization so that it's a first-class citizen in your workflow.

Several tools are available that perform further, lossless compression on `JPG` and `PNG` files with no effect on image quality. For `JPG`, try [jpegtran](http://jpegclub.org/) or [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (available on Linux only; run with the --strip-all option). For `PNG`, try [OptiPNG](http://optipng.sourceforge.net/) or [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

### De juiste indeling kiezen

<img src="img/sprite-sheet.png" class="attempt-right" alt="Image sprite sheet used in example" />

CSS spriting is a technique whereby a number of images are combined into a single "sprite sheet" image. You can then use individual images by specifying the background image for an element (the sprite sheet) plus an offset to display the correct part.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/image-sprite.html){: target="_blank" .external }

Spriting has the advantage of reducing the number of downloads required to get multiple images, while still enabling caching.

### De bestandsgrootte verkleinen

Lazy loading can significantly speed up loading on long pages that include many images below the fold by loading them either as needed or when the primary content has finished loading and rendering. In addition to performance improvements, using lazy loading can create infinite scrolling experiences.

Be careful when creating infinite scrolling pages&mdash;because content is loaded as it becomes visible, search engines may never see that content. In addition, users who are looking for information they expect to see in the footer, never see the footer because new content is always loaded.

## Avoid images completely

Sometimes the best image isn't actually an image at all. Whenever possible, use the native capabilities of the browser to provide the same or similar functionality. Browsers generate visuals that would have previously required images. This means that browsers no longer need to download separate image files thus preventing awkwardly scaled images. You can use unicode or special icon fonts to render icons.

### Afbeeldingssprites gebruiken

Wherever possible, text should be text and not embedded into images. For example, using images for headlines or placing contact information&mdash;like phone numbers or addresses&mdash;directly into images prevents users from copying and pasting the information; it makes the information inaccessible for screen readers, and it isn't responsive. Instead, place the text in your markup and if necessary use webfonts to achieve the style you need.

### Lazy loading overwegen

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