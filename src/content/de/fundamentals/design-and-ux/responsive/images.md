project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Ein Bild sagt mehr als 1000 Worte, und Bilder spielen eine sehr wichtige Rolle auf jeder einzelnen Seite. Leider stellen sie aber ebenso einen Großteil des Volumens dar, das heruntergeladen wird. Mit einem responsiven Webdesign können sich nicht nur unsere Layouts Gerätecharakteristiken anpassen, sondern ebenso die Bilder.

{# wf_updated_on: 2018-12-15 #} {# wf_published_on: 2014-04-29 #}

# Bilder {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Ein Bild sagt mehr als 1000 Worte, und Bilder spielen eine sehr wichtige Rolle auf jeder einzelnen Seite. Leider stellen sie aber ebenso einen Großteil des Volumens dar, das heruntergeladen wird. Mit einem responsiven Webdesign können sich nicht nur unsere Layouts Gerätecharakteristiken anpassen, sondern ebenso die Bilder.

## Bilder im Markup

<img src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Other times the image may need to be changed more drastically: changing the proportions, cropping, and even replacing the entire image. In this case, changing the image is usually referred to as art direction. See [responsiveimages.org/demos/](https://responsiveimages.org/demos/) for more examples.

In anderen Fällen sind möglicherweise noch drastischere Änderungen erforderlich: Änderung der Proportionen, Zuschneiden und sogar der Austausch des gesamten Bilds. In solchen Fällen wir der Prozess zur Änderung des Bilds in der Regel als Art Direction bezeichnet. Weitere Beispiele finden Sie unter [responsiveimages.org/demos/](http://responsiveimages.org/demos/).

## Bilder in CSS 

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

### Responsive Bilder

* Nutzen Sie relative Größen für Bilder, um zu verhindern, dass diese sich versehentlich über die Container-Grenzen hinweg erstrecken.
* Verwenden Sie das `picture`-Element, wenn Sie verschiedene Bilder auf Grundlage von Gerätecharakteristiken festlegen, auch Art Direction genannt.
* Verwenden Sie `srcset` und den `x`-Deskriptor im `img`-Element, um den Browser darauf hinzuweisen, welches das am besten geeignete Bild bei der Auswahl aus verschiedenen Pixeldichten ist.
* If your page only has one or two images and these are not used elsewhere on your site, consider using inline images to reduce file requests.

### Art Direction

Das `img`-Element erfüllt viele Funktionen. Es lädt Inhalte herunter, decodiert sie und zeigt sie an. Darüber hinaus unterstützen moderne Browser eine große Anzahl an Bildformaten. Die Nutzung von Bildern, die auf allen Geräten funktionieren, unterscheidet sich im Vergleich zu Desktopcomputern nicht. Es sind lediglich ein paar kleinere Handgriffe nötig, um eine gute Erfahrung zu gewährleisten.

Denken Sie daran, relative Größen zu nutzen, wenn Sie Breiten für Bilder festlegen, um zu verhindern, dass sie sich über die Grenzen des Darstellungsbereichs hinweg erstrecken. So bleibt die Bildgröße bei `width: 50%` stets bei 50 % des Container-Elements - nicht des Darstellungsbereichs oder der tatsächlichen Pixelgröße.

    img, embed, object, video {
      max-width: 100%;
    }
    

Da es die CSS-Syntax zulässt, dass sich Inhalte über Container-Grenzen hinweg erstrecken, kann es erforderlich sein, die Angabe `max-width: 100%` einzufügen, damit Bilder und andere Inhalte innerhalb des Containers bleiben. Beispiel:

### TL;DR {: .hide-from-toc }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="Pzc5Dly_jEM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Achten Sie darauf, dem `alt`-Attribut des `img`-Elements aussagekräftige Beschreibungen hinzuzufügen. Hierdurch wird Ihre Website zugänglicher, denn die Beschreibungen bieten einer Bildschirmsprachausgabe und anderen Hilfstechnologien Kontext.

<div style="clear:both;">
</div>

    <img src="photo.png" srcset="photo@2x.png 2x" ...>
    

Das `srcset`-Attribut erweitert das `img`-Element, indem es dafür sorgt, dass auf einfache Weise mehrere Bilddateien für verschiedene Gerätecharakteristiken bereitgestellt werden können. Ähnlich wie die native [CSS-Funktion](#use_image-set_to_provide_high_res_images) `image-set` erlaubt `srcset` Browsern, abhängig von den Charakteristiken des jeweiligen Geräts das beste Bild auszuwählen, zum Beispiel ein 2x-Bild für ein 2x-Display, und in Zukunft möglicherweise auch ein 1x-Bild für ein 2x-Gerät, wenn nur eine geringe Bandbreite zur Verfügung steht.

Browser, die `srcset` nicht unterstützen, nutzen einfach die Standardbilddatei, die im `src`-Attribut festgelegt ist. Aus diesem Grund ist es wichtig, immer ein 1x-Bild zur Verfügung zu stellen, das auf allen Geräten unabhängig von jeglichen Funktionen angezeigt werden kann. Bei `srcset`-Unterstützung wird die kommagetrennte Liste mit Bild/Bedingungen vor dem Senden von Anfragen analysiert und nur das am besten geeignete Bild heruntergeladen und angezeigt.

### Relative Größen für Bilder verwenden

<img class="attempt-right" src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Das Ändern von Bildern auf Grundlage von Gerätecharakteristiken ist auch als Art Direction bekannt und kann mithilfe des `picture`-Elements bewerkstelligt werden. Das `picture`-Element definiert eine deklarative Lösung für die Bereitstellung verschiedener Versionen eines Bilds auf Grundlage verschiedener Charakteristiken wie Gerätegröße, Geräteauflösung, Ausrichtung usw.

<div style="clear:both;">
</div>

Dogfood: The `picture` element is beginning to land in browsers. Although it's not available in every browser yet, we recommend its use because of the strong backward compatibility and potential use of the [Picturefill polyfill](https://scottjehl.github.io/picturefill/). See the [ResponsiveImages.org](http://responsiveimages.org/#implementation) site for further details.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="QINlm3vjnaY"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Note: Das `picture`-Element wird von Browsern zunehmend unterstützt. Es ist zwar noch nicht in allen Browsern verfügbar, wir empfehlen aufgrund seiner guten Rückwärtskompatibilität und der möglichen Nutzung von [Picturefill/Polyfill](https://scottjehl.github.io/picturefill/) aber dennoch seinen Einsatz. Weitere Informationen erhalten Sie auf der Website [ResponsiveImages.org](http://responsiveimages.org/#implementation).

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media.html" region_tag="picture" adjust_indentation="auto" %}
</pre>

Das `picture`-Element sollte verwendet werden, wenn eine Bildquelle mit verschiedenen Pixeldichten verfügbar ist oder ein responsives Design nach leicht unterschiedlichen Bildern auf einigen Bildschirmarten verlangt. Ähnlich wie beim `video`-Element können mehrere `source`-Elemente verwendet werden, wodurch abhängig von Medienabfragen oder dem Bildformat die Angabe verschiedener Bilddateien möglich wird.

Im vorherigen Beispiel wird bei einer Browserbreite von mindestens 800 Pixeln entweder `head.jpg` oder `head-2x.jpg` verwendet, abhängig von der Auflösung des Geräts. Wenn der Browser zwischen 450 und 800 Pixeln breit ist, kommt entweder `head-small.jpg` oder `head-small-2x.jpg` zum Einsatz, wieder abhängig von der Auflösung des Geräts. Für Bildschirmbreiten mit weniger als 450 Pixeln und Rückwärtskompatibilität, bei denen das `picture`-Element nicht unterstützt wird, stellt der Browser stattdessen das `img`-Element dar, das immer verwendet werden sollte.

#### Bilder mit relativer Größe

Wenn die endgültige Größe des Bilds nicht bekannt ist, kann es schwierig sein, einen Deskriptor für die Pixeldichte der Bildquellen anzugeben. Dies gilt vor allem für Bilder, die sich über eine proportionale Breite des Browsers erstrecken und abhängig von der Größe des Browsers frei verschiebbar sind.

Statt eine feste Größe und Pixeldichte für Bilder festzulegen, kann die Größe aller bereitgestellten Bilder über das Hinzufügen eines Breitendeskriptors zusammen mit der Größe des Bildelements angegeben werden. So kann der Browser die effektive Pixeldichte automatisch berechnen und das beste Bild zum Herunterladen auswählen.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/sizes.html" region_tag="picture" adjust_indentation="auto" %}
</pre>

Im vorherigen Beispiel wird ein Bild dargestellt, das die Hälfte der Breite des Darstellungsbereichs (sizes=50vw) aufweist. Abhängig von der Breite des Browsers und seinem Gerätepixelverhältnis wird ihm ermöglicht, das richtige Bild unabhängig davon auszuwählen, wie groß das Browserfenster ist. In der folgenden Tabelle ist zu sehen, welches Bild er Browser auswählen würde:

In vielen Fällen ändert sich die Größe oder das Bild abhängig von den Layoutübergangspunkten der Website. So kann es bei kleinen Bildschirmen wünschenswert sein, die volle Breite des Darstellungsbereichs mit einem Bild abzudecken, während auf größeren Bildschirmen nur ein kleiner proportionaler Teil genutzt werden sollte.

<table class="">
  <tr>
    <th data-th="Browser width">
      Browserbreite
    </th>
    
    <th data-th="Device pixel ratio">
      Gerätepixelverhältnis
    </th>
    
    <th data-th="Image used">
      Verwendetes Bild
    </th>
    
    <th data-th="Effective resolution">
      Effektive Auflösung
    </th>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400 px
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
      400 px
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
      320 px
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
      600 px
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
      1100 px
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

#### Übergangspunkte in responsiven Bildern berücksichtigen

Das `sizes`-Attribut im vorherigen Beispiel nutzt mehrere Medienabfragen, um die Größe des Bilds festzulegen. Wenn die Breite des Browsers über 600 Pixeln liegt, wird das Bild mit 25 % der Breite des Darstellungsbereichs angezeigt. Bei einer Breite zwischen 500 und 600 Pixeln nimmt es 50 % der Breite des Darstellungsbereichs ein. Unter 500 Pixeln wird die volle Breite in Anspruch genommen.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/breakpoints.html" region_tag="picture" adjust_indentation="auto" %}
</pre>

Kunden möchten sehen, was sie kaufen. Auf Einzelhändler-Websites erwarten Nutzer, sich Nahaufnahmen von Produkten in hoher Auflösung ansehen zu können, um einen besseren Eindruck von Einzelheiten zu bekommen. [Studienteilnehmer](/web/fundamentals/getting-started/principles/) wurden frustriert, wenn sie dazu keine Gelegenheit erhielten.

Ein gutes Beispiel für ein maximierbares Bild, das angetippt werden kann, lässt sich auf der Website von J. Crew finden. Ein Overlay, das verschwindet, gibt einen Hinweis darauf, dass das Bild angetippt werden kann. Beim Antippen erhält der Nutzer anschließend ein gezoomtes Bild, auf dem Einzelheiten betrachtet werden können.

### Bilddarstellung mit `srcset` auf Geräten mit hohem DPI-Wert verbessern<figure class="attempt-right"> 

<img src="img/sw-make-images-expandable-good.png" srcset="img/sw-make-images-expandable-good.png 1x, img/sw-make-images-expandable-good-2x.png 2x" alt="Website von J. Crews mit maximierbarem Produktbild" /> <figcaption>Website von J. Crews mit maximierbarem Produktbild</figcaption> </figure> 

Die [Methode für Komprimierte Bilder](http://www.html5rocks.com/en/mobile/high-dpi/#toc-tech-overview) stellt ein stark komprimiertes 2x-Bild für alle Geräte bereit, unabhängig von den tatsächlichen Funktionen des Geräts. Abhängig vom Bildtyp und der Komprimierungsstufe ist möglicherweise keine Veränderung am Bild wahrnehmbar, die Dateigröße verringert sich jedoch beträchtlich.

A good example of tappable, expandable images is provided by the J. Crew site. A disappearing overlay indicates that an image is tappable, providing a zoomed in image with fine detail visible.

<div style="clear:both;">
</div>

### Art Direction in responsiven Bildern mit `picture`

#### Komprimierte Bilder

Note: Bei der Nutzung der Komprimierung ist aufgrund der erhöhten Speicherbelastung und des erhöhten Aufwands beim Codieren Vorsicht geboten. Die Änderung der Größe für kleinere Bildschirme ist rechenintensiv und kann besonders auf Low-End-Geräten mit wenig Speicher und geringer Rechenkapazität Probleme verursachen.

Die JavaScript-Bildersetzung prüft die Funktionen des Geräts und entscheidet sich dann für das passende Bild. Sie können das Gerätepixelverhältnis mithilfe von `window.devicePixelRatio` ermitteln, die Bildschirmhöhe und -breite abrufen und mit `navigator.connection` oder einer Fake-Anfrage möglicherweise sogar mehr über die Netzwerkverbindung herausfinden. Nachdem Sie diese Informationen gesammelt haben, können Sie entscheiden, welches Bild geladen werden soll.

Ein großer Nachteil dieses Ansatzes ist, dass das Bild bei der Verwendung von JavaScript erst dann geladen wird, wenn zumindest der `look-ahead`-Parser durchlaufen wurde. Das bedeutet, dass der Ladevorgang für das Bild nicht einmal gestartet wird, bis das `pageload`-Ereignis ausgelöst wurde. Zudem lädt der Browser mit hoher Wahrscheinlichkeit sowohl die 1x- als auch die 2x-Varianten des Bilds herunter, wodurch die Seite zusätzlich Speicher belegt.

#### JavaScript-Bildersetzung

Die CSS-Eigenschaft `background` ist ein leistungsstarkes Tool für das Hinzufügen von komplexen Bildern zu Elementen. Es können damit mehrere Bilder hinzugefügt und vervielfältigt werden und vieles mehr. Wenn die Eigenschaft zusammen mit Medienabfragen genutzt wird, ergeben sich sogar noch weitere Möglichkeiten. So können Bilder nur unter bestimmten Bedingungen auf Grundlage von Bildschirmauflösung, Größe des Darstellungsbereichs oder anderen Faktoren geladen werden.

Medienabfragen wirken sich nicht nur auf das Seitenlayout aus, sondern können auch dazu genutzt werden, Bilder unter bestimmten Bedingungen zu laden oder Art Direction auf Grundlage der Breite des Darstellungsbereichs bereitzustellen.

#### Inlining images: raster and vector

So wird im folgenden Beispiel für kleinere Bildschirme nur `small.png` heruntergeladen und auf die Inhalte des `div`-Containers angewendet, während für größere `background-image: url(body.png)` auf den Hauptteil und `background-image: url(large.png)` auf den Inhalt im `div`-Container angewendet wird.

Die CSS-Funktion `image-set()` erweitert die Funktionalität der `background`-Eigenschaft, sodass die Bereitstellung mehrerer Bilddateien für verschiedene Gerätecharakteristiken problemlos bewerkstelligt werden kann. Damit kann der Browser das für die Charakteristiken des jeweiligen Geräts am besten geeignete Bild auswählen, etwa ein 2x-Bild für ein 2x-Display oder ein 1x-Bild für ein 2x-Gerät, falls nur eine geringe Bandbreite zur Verfügung steht.

Der Browser lädt das richtige Bild nicht nur, sondern skaliert es auch entsprechend. Anders ausgedrückt: Der Browser geht davon aus, dass 2x-Bilder doppelt so groß wie 1x-Bilder sind und reduziert das 2x-Bild um den Faktor 2, sodass das Bild in der gleichen Größe auf der Seite erscheint.

##### SVG

Die Unterstützung für `image-set()` ist relativ neu und wird aktuell nur von Chrome und Safari mit dem Anbieterpräfix `-webkit` unterstützt. Darüber hinaus muss Sorge dafür getragen werden, dass ein Ersatzbild zur Verfügung steht, falls `image-set()` nicht unterstützt wird. Beispiel:

Im vorherigen Beispiel wird das passende Element in Browsern, die `image-set` unterstützen, und ansonsten das 1x-Element geladen. Das Problem hierbei ist natürlich, dass in den meisten Browsern das 1x-Element geladen wird, solange die `image-set()`-Unterstützung bei Browsern gering ist.

<img class="side-by-side" src="img/html5.png" alt="HTML5 logo, PNG format" /> <img class="side-by-side" src="img/html5.svg" alt="HTML5 logo, SVG format" />

Chrome, Firefox und Opera unterstützen alle standardmäßig `(min-resolution: 2dppx)`, während Safari und Android-Browser beide die ältere Syntax mit Anbieterpräfix ohne `dppx`-Einheit benötigen. Denken Sie daran, dass diese Stile nur dann geladen werden, wenn das Gerät der Medienabfrage entspricht, und Sie Stile für den Grundfall festlegen müssen. Dies hat den Vorteil, dass eine Darstellung auch in dem Fall gewährleistet ist, dass der Browser keine auflösungsspezifischen Medienabfragen unterstützt.

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

Wenn Sie Ihrer Seite Symbole hinzufügen, sollten Sie falls möglich SVG-Symbole oder gegebenenfalls Unicode-Zeichen verwenden.

<svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg><svg class="side-by-side" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" width="50%" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5"/><path fill="#E44D26" d="M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z"/><polygon fill="#F16529" points="480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2"/><polygon fill="#F16529" points="541,343 480,343 480,422.4 535.2,407.4"/><polygon fill="#EBEBEB" points="414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4"/><polygon fill="#EBEBEB" points="479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474"/><polygon points="343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5"/><polygon points="425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25"/><polygon points="508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5"/><polygon points="642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5"/><polygon fill="#FFFFFF" points="480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1"/><polygon fill="#FFFFFF" points="479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7"/></g></g></g></svg>

##### Data URI

Neben dem normalen Zeichensatz kann Unicode Symbole für Zahlzeichen (&#8528;), Pfeile (&#8592;), mathematische Operatoren (&#8730;), geometrische Formen (&#9733;), Steuerzeichen (&#9654;), Brailleschrift (&#10255;), Notenschrift (&#9836;), griechische Buchstaben (&#937;) und sogar Schachfiguren (&#9822;) enthalten.

    <img src="data:image/svg+xml;base64,[data]">
    

Unicode-Zeichen werden wie benannte Zeichen verwendet: `&#XXXX`, wobei `XXXX` der Zahl des Unicode-Zeichens entspricht. Beispiel:

    background-image: image-set(
      url(icon1x.jpg) 1x,
      url(icon2x.jpg) 2x
    );
    

Du bist ein echter &#9733;

Für komplexere Symbolanforderungen eignen sich SVG-Symbole. Sie sind in der Regel simpel, nutzerfreundlich und können mit CSS gestaltet werden. SVG bietet im Vergleich zu Rasterbildern folgende Vorteile:

##### Inlining in CSS

Data URIs and SVGs can also be inlined in CSS&mdash;and this is supported on both mobile and desktop. Here are two identical-looking images implemented as background images in CSS; one Data URI, one SVG:

<span class="side-by-side" id="data_uri"></span> <span class="side-by-side" id="svg"></span>

##### Inlining pros & cons

Inline code for images can be verbose&mdash;especially Data URIs&mdash;so why would you want to use it? To reduce HTTP requests! SVGs and Data URIs can enable an entire web page, including images, CSS and JavaScript, to be retrieved with one single request.

Es gibt Hunderte kostenloser und kostenpflichtiger Symbolschriften, darunter [Font Awesome](http://fortawesome.github.io/Font-Awesome/), [Pictos](http://pictos.cc/) und [Glyphicons](http://glyphicons.com/).

* Verwenden Sie das am besten zu den Charakteristiken der Anzeige passende Bild. Berücksichtigen Sie die Bildschirmgröße, die Geräteauflösung und das Seitenlayout.
* Ändern Sie für Anzeigen mit hohem DPI-Wert die `background-image`-Eigenschaft in CSS, indem Sie Medienabfragen mit `min-resolution` und `-webkit-min-device-pixel-ratio` verwenden.
* Verwenden Sie `srcset` zur Bereitstellung von Bildern mit hoher Auflösung zusätzlich zu den 1x-Bildern im Markup.
* Berücksichtigen Sie den Berechnungsaufwand beim Einsatz von JavaScript-Methoden zum Ersetzen von Bildern oder der Bereitstellung von stark komprimierten Bildern mit hoher Auflösung auf Geräten mit geringerer Auflösung.
* Data URIs cannot be cached, so must be downloaded for every page they're used on.
* They're not supported in IE 6 and 7, incomplete support in IE8.
* With HTTP/2, reducing the number of asset requests will become less of a priority.

Achten Sie darauf, dass die Last der zusätzlichen HTTP-Anfrage und Dateigröße mit dem Symbolbedarf im Verhältnis steht. Wenn Sie zum Beispiel nur eine Handvoll Symbole benötigen, ist ein Bild oder Sprite unter Umständen die bessere Wahl.

## SVG für Symbole verwenden

Bilder belegen in der Regel die meiste Bandbreite und darüber hinaus oft auch einen großen Teil des visuellen Bereichs einer Seite. Aus diesem Grund kann die Bandbreite oft durch die Optimierung von Bildern am besten entlastet und so die Leistung Ihrer Website erhöht werden: Je weniger der Browser herunterladen muss, desto weniger Bandbreitebelastung entsteht, wodurch der Browser sämtliche Inhalte schneller herunterladen und anzeigen kann.

### Produktbilder maximierbar machen

* Verwenden Sie anstelle von Rasterbildern SVG oder Unicode für Symbole.
* Change the `background-image` property in CSS for high DPI displays using media queries with `min-resolution` and `-webkit-min-device-pixel-ratio`.
* Use srcset to provide high resolution images in addition to the 1x image in markup.
* Consider the performance costs when using JavaScript image replacement techniques or when serving highly compressed high resolution images to lower resolution devices.

### Andere Bildmethoden

Es bestehen zwei relevante Bildtypen: [Vektorbilder](http://de.wikipedia.org/wiki/Vektorgrafik) and [Rasterbilder](http://de.wikipedia.org/wiki/Rastergrafik). Für Rasterbilder muss zudem das richtige Kompressionsformat gewählt werden, etwa GIF, PNG oder JPG.

Rasterbilder sind zum Beispiel Fotos und andere Bilder, die aus einem Raster einzelner Punkte oder Pixel bestehen. Sie stammen in der Regel von einer Kamera oder einem Scanner oder können in einem Browser mithilfe des `canvas`-Elements erstellt werden. Je größer das Bild, desto größer auch die Dateigröße. Wenn Rasterbilder hochskaliert werden, werden Sie verschwommen, da der Browser nicht weiß, womit die fehlenden Pixel aufgefüllt werden sollen.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-set.html" region_tag="imageset" adjust_indentation="auto" %}
</pre>

Vektorbilder wie Logos und Grafiken werden durch eine Anzahl an Kurven, Linien, Formen und Füllfarben definiert. Vektorbilder können mit Programmen wie Adobe Illustrator oder Inkscape erstellt und in einem Vektorformat wie [SVG](http://css-tricks.com/using-svg/) gespeichert werden. Da Vektorbilder auf einfachen Grundtypen basieren, können sie beliebig skaliert werden, ohne dass sich die Qualität oder Dateigröße ändert.

### TL;DR {: .hide-from-toc }

Beim Auswählen des Formats ist es wichtig, sowohl den Ursprung des Bilds - Raster oder Vektor - als auch die Inhalte - Farben, Animation, Text usw. - zu berücksichtigen. Ein Format stellt niemals die Ideallösung für sämtliche Bildtypen dar. Jedes hat seine Stärken und Schwächen.

    @media (min-resolution: 2dppx),
    (-webkit-min-device-pixel-ratio: 2)
    {
      /* High dpi styles & resources here */
    }
    

Orientieren Sie sich an den folgenden Richtlinien, wenn Sie das Format wählen:

Die Dateigröße von Bildern kann stark reduziert werden, indem sie nach dem Speichern zusätzlich komprimiert werden. Es besteht eine große Auswahl an Tools zur Bildkompression: verlustbehaftete und verlustfreie, online, GUI, Befehlszeile. Wann immer möglich sollte die Bildoptimierung automatisch erfolgen, damit sie problemlos in Ihren Prozess integriert werden kann.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" adjust_indentation="auto" %}
</pre>

Viele Tools bieten die Möglichkeit, eine weitere, verlustfreie Komprimierung für JPG- und PNG-Dateien vorzunehmen, was sich nicht weiter auf die Bildqualität auswirkt. Für JPG kann [jpegtran](http://jpegclub.org/) oder [jpegoptim](http://freshmeat.net/projects/jpegoptim/) verwendet werden. Diese Programme stehen nur unter Linux zur Verfügung und sollten mit der Option `--strip-all` ausgeführt werden. Für PNG kann [OptiPNG](http://optipng.sourceforge.net/) oder [PNGOUT](http://www.advsys.net/ken/util/pngout.htm) verwendet werden.

CSS-Spriting ist eine Methode, bei der eine Anzahl an Bildern zu einem einzelnen Sprite-Block zusammengefügt wird. Einzelne Bilder können anschließend verwendet werden, indem das Hintergrundbild - der Sprite-Block - für ein Element angegeben und um eine Positionsangabe ergänzt wird, damit der richtige Abschnitt erscheint.

### Medienabfragen für das bedingte Laden von Bildern oder Art Direction verwenden

Media queries can create rules based on the [device pixel ratio](http://www.html5rocks.com/en/mobile/high-dpi/#toc-bg), making it possible to specify different images for 2x versus 1x displays.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

Spriting hat den Vorteil, dass die Anzahl an einzelnen Downloads reduziert wird, die bei mehreren Bildern anfallen, während eine Zwischenspeicherung weiterhin möglich ist.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" adjust_indentation="auto" %}
</pre>

Lazy Loading kann die Ladezeiten für lange Seiten mit vielen Bildern, die nur beim Scrollen sichtbar werden, stark reduzieren, indem Bilder entweder nur bei Bedarf oder erst dann geladen werden, wenn die Hauptinhalte fertig heruntergeladen und dargestellt wurden. Zusätzlich zur Leistungsverbesserung kann Lazy Loading Erfahrungen bieten, bei denen unbegrenztes Scrollen möglich ist.

Achten Sie beim Erstellen von Seiten mit unbegrenztem Scrollen jedoch darauf, dass Inhalte erst dann geladen werden, wenn sie sichtbar werden. Suchmaschinen finden diese Inhalte möglicherweise nicht. Zudem bekommen Nutzer, die nach Informationen in der Fußzeile suchen, diese nie zu Gesicht, da ständig weitere Inhalte geladen werden.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

## Bilder auf Leistung optimieren

Manchmal ist es besser, Bilder gar nicht zu nutzen. Verwenden Sie wann immer möglich die nativen Funktionen des Browsers, um gleiche oder ähnliche Funktionalität bereitzustellen. Browser bieten heute optische Möglichkeiten, für die früher Bilder notwendig gewesen währen. So müssen Browser keine separaten Bilddateien mehr herunterladen und es besteht keine Gefahr, dass falsch skalierte Bilder erscheinen. Symbole können mithilfe von Unicode oder speziellen Symbolschriftarten dargestellt werden.

### `image-set` zur Bereitstellung von Bildern mit hoher Auflösung verwenden

* Die Vektorgrafiken lassen sich unendlich skalieren.

### Medienabfragen für die Bereitstellung von Bildern mit hoher Auflösung oder Art Direction verwenden

Wann immer möglich sollte Text in Text und nicht in Bilder eingebettet sein, wie etwa bei der Nutzung von Bildern für Titel oder Kontaktinformationen wie Telefonnummern und Adressen. Von Bildern können Nutzer die Informationen nicht kopieren und einfügen, außerdem ist der Text für eine Bildschirmsprachausgabe nicht verfügbar und auch nicht responsiv. Fügen Sie den Text stattdessen in Ihr Markup ein und verwenden Sie bei Bedarf Webschriftarten, um den gewünschten Stil umzusetzen.

Moderne Browser können auf CSS-Funktionen zurückgreifen, um Stile zu erschaffen, für die zuvor Bilder erforderlich waren. So können mit der `background`-Eigenschaft komplexe Farbverläufe erstellt und mit der `box-shadow`-Eigenschaft Schatten und der `border-radius`-Eigenschaft abgerundete Ecken hinzugefügt werden.

Including a unicode character is done in the same way named entities are: `&#XXXX`, where `XXXX` represents the unicode character number. For example:

    You're a super &#9733;
    

Beachten Sie, dass bei diesen Verfahren Renderingzyklen anfallen, deren Berechnung für Mobilgeräte ein Problem darstellen kann. Bei einer übermäßigen Inanspruchnahme gehen die Vorteile verloren und die Leistung wird möglicherweise zusätzlich beeinträchtigt.

### TL;DR {: .hide-from-toc }

For more complex icon requirements, SVG icons are generally lightweight, easy to use, and can be styled with CSS. SVG have a number of advantages over raster images:

* Es handelt sich um Vektorgrafiken, die unendlich skalierbar sind. Es kann jedoch zu einer Kantenglättung kommen, in deren Folge die Symbole nicht so scharf dargestellt werden wie erwartet.
* Die Gestaltungsmöglichkeiten mit CSS sind begrenzt.
* Die perfekte Positionierung der Pixel kann schwierig sein, je nach Zeilenhöhe, Buchstabenabstand usw.
* Sie sind nicht semantisch und die Verwendung mit Screenreadern oder anderen Bedienungshilfen kann sich als schwierig erweisen.
* Wenn der Bereich nicht ordnungsgemäß festgelegt ist, kann die Datei riesig werden, obwohl nur eine kleine Teilmenge der verfügbaren Symbole verwendet wird.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-svg.html){: target="_blank" .external }

### Einfache Symbole durch Unicode ersetzen<figure class="attempt-right"> 

<img src="img/icon-fonts.png" class="center" srcset="img/icon-fonts.png 1x, img/icon-fonts-2x.png 2x" alt="Example of a page that uses FontAwesome for its font icons." /> <figcaption> <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html" target="_blank" class="external"> Example of a page that uses FontAwesome for its font icons. </a> </figcaption> </figure> 

Icon fonts are popular, and can be easy to use, but have some drawbacks compared to SVG icons:

* Verwenden Sie nicht einfach irgendein Bildformat. Setzen Sie sich mit den verschiedenen verfügbaren Formaten auseinander und nutzen Sie das am besten geeignete.
* Nehmen Sie Schritte zur Bildoptimierung und -komprimierung in Ihren Prozess auf, um die Größe der Dateien zu reduzieren.
* Reduzieren Sie die Anzahl an HTTP-Anfragen, indem Sie häufig genutzte Bilder in Bild-Sprites platzieren.
* Ziehen Sie in Betracht, Bilder erst dann laden zu lassen, wenn sie sichtbar sind. So kann die Seite schneller geladen werden und belegt zu Anfang weniger Speicher.
* Unless properly scoped, they can result in a large file size for only using a small subset of the icons available.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/icon-font.html" region_tag="iconfont" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html){: target="_blank" .external }

There are hundreds of free and paid icon fonts available including [Font Awesome](https://fortawesome.github.io/Font-Awesome/), [Pictos](http://pictos.cc/){: .external }, and [Glyphicons](https://glyphicons.com/).

Be sure to balance the weight of the additional HTTP request and file size with the need for the icons. For example, if you only need a handful of icons, it may be better to use an image or an image sprite.

## Bilder nach Möglichkeit vermeiden

Images often account for most of the downloaded bytes and also often occupy a significant amount of the visual space on the page. As a result, optimizing images can often yield some of the largest byte savings and performance improvements for your website: the fewer bytes the browser has to download, the less competition there is for client's bandwidth and the faster the browser can download and display all the assets.

### Komplexe Symbole durch SVG ersetzen

* Verwenden Sie JPG für Fotos.
* Verwenden Sie SVG für Vektorgrafiken und Grafiken mit Volltonfarbe wie etwa Logos.
* Verwenden Sie WebP oder PNG, falls Vektorgrafiken nicht zur Verfügung stehen.
* Verwenden Sie PNG statt GIF, da das Format mehr Farben unterstützt und eine bessere Kompression bietet.

### Symbolschriften mit Vorsicht verwenden

There are two types of images to consider: [vector images](https://en.wikipedia.org/wiki/Vector_graphics) and [raster images](https://en.wikipedia.org/wiki/Raster_graphics). For raster images, you also need to choose the right compression format, for example: `GIF`, `PNG`, `JPG`.

**Raster images**, like photographs and other images, are represented as a grid of individual dots or pixels. Raster images typically come from a camera or scanner, or can be created in the browser with the `canvas` element. As the image size gets larger, so does the file size. When scaled larger than their original size, raster images become blurry because the browser needs to guess how to fill in the missing pixels.

**Vector images**, such as logos and line art, are defined by a set of curves, lines, shapes, and fill colors. Vector images are created with programs like Adobe Illustrator or Inkscape and saved to a vector format like [`SVG`](https://css-tricks.com/using-svg/). Because vector images are built on simple primitives, they can be scaled without any loss in quality or change in file size.

When choosing the appropriate format, it is important to consider both the origin of the image (raster or vector), and the content (colors, animation, text, etc). No one format fits all image types, and each has its own strengths and weaknesses.

Start with these guidelines when choosing the appropriate format:

* Verzichten Sie wann immer möglich auf Bilder und nutzen Sie stattdessen Browserfunktionen für Schatten, Farbverläufe, abgerundete Ecken usw.
* Use `SVG` for vector art and solid color graphics such as logos and line art. If vector art is unavailable, try `WebP` or `PNG`.
* Use `PNG` rather than `GIF` as it allows for more colors and offers better compression ratios.
* For longer animations consider using `<video>`, which provides better image quality and gives the user control over playback.

### TL;DR {: .hide-from-toc }

You can reduce image file size considerably by "post-processing" the images after saving. There are a number of tools for image compression&mdash;lossy and lossless, online, GUI, command line. Where possible, it's best to try automating image optimization so that it's a first-class citizen in your workflow.

Several tools are available that perform further, lossless compression on `JPG` and `PNG` files with no effect on image quality. For `JPG`, try [jpegtran](http://jpegclub.org/) or [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (available on Linux only; run with the --strip-all option). For `PNG`, try [OptiPNG](http://optipng.sourceforge.net/) or [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

### Das richtige Format wählen

<img src="img/sprite-sheet.png" class="attempt-right" alt="Image sprite sheet used in example" />

CSS spriting is a technique whereby a number of images are combined into a single "sprite sheet" image. You can then use individual images by specifying the background image for an element (the sprite sheet) plus an offset to display the correct part.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/image-sprite.html){: target="_blank" .external }

Spriting has the advantage of reducing the number of downloads required to get multiple images, while still enabling caching.

### Dateigröße reduzieren

Lazy loading can significantly speed up loading on long pages that include many images below the fold by loading them either as needed or when the primary content has finished loading and rendering. In addition to performance improvements, using lazy loading can create infinite scrolling experiences.

Be careful when creating infinite scrolling pages&mdash;because content is loaded as it becomes visible, search engines may never see that content. In addition, users who are looking for information they expect to see in the footer, never see the footer because new content is always loaded.

## Avoid images completely

Sometimes the best image isn't actually an image at all. Whenever possible, use the native capabilities of the browser to provide the same or similar functionality. Browsers generate visuals that would have previously required images. This means that browsers no longer need to download separate image files thus preventing awkwardly scaled images. You can use unicode or special icon fonts to render icons.

### Bild-Sprites verwenden

Wherever possible, text should be text and not embedded into images. For example, using images for headlines or placing contact information&mdash;like phone numbers or addresses&mdash;directly into images prevents users from copying and pasting the information; it makes the information inaccessible for screen readers, and it isn't responsive. Instead, place the text in your markup and if necessary use webfonts to achieve the style you need.

### Lazy Loading in Betracht ziehen

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