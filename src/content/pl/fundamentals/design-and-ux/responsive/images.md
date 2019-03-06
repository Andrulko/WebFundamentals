project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Obraz jest wart tysiąc słów, a grafiki są nieodłączną częścią każdej strony. Jednak często stanowią większość pobieranych danych. Elastyczne projektowanie witryn pozwala na podstawie cech urządzenia zmieniać nie tylko układ strony, ale też obrazy.

{# wf_updated_on: 2018-12-15 #} {# wf_published_on: 2000-01-01 #}

# Obrazy {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Obraz jest wart tysiąc słów, a grafiki są nieodłączną częścią każdej strony. Jednak często stanowią większość pobieranych danych. Elastyczne projektowanie witryn pozwala na podstawie cech urządzenia zmieniać nie tylko układ strony, ale też obrazy.

## Obrazy w znacznikach

<img src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Other times the image may need to be changed more drastically: changing the proportions, cropping, and even replacing the entire image. In this case, changing the image is usually referred to as art direction. See [responsiveimages.org/demos/](https://responsiveimages.org/demos/) for more examples.

Czasami obraz trzeba zmienić w większym stopniu &ndash; dopasować proporcje, przyciąć, a nawet zastąpić innym. W takiej sytuacji zmianę obrazu określa się zwykle jako dostosowywanie grafiki. Więcej przykładów znajdziesz na [responsiveimages.org/demos/](http://responsiveimages.org/demos/){: .external }.

## Obrazy w CSS 

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

### Elastyczne obrazy

* Użyj względnych rozmiarów obrazów, by zapobiec przypadkowemu wyjściu poza kontener.
* Gdy chcesz określić różne obrazy wyświetlane w zależności od cech urządzenia (tzn. dostosować grafikę), użyj elementu `picture`.
* Użyj atrybutu `srcset` i deskryptora `x` w elemencie `img`, by przy wyborze obrazu podpowiedzieć przeglądarce, której rozdzielczości najlepiej użyć.
* If your page only has one or two images and these are not used elsewhere on your site, consider using inline images to reduce file requests.

### Dostosowywanie grafiki

Element `img` ma duże możliwości &ndash; pobiera, dekoduje i renderuje treści &ndash; a współczesne przeglądarki obsługują szeroką gamę formatów graficznych. Dodawanie obrazów, które wyświetlają się na różnych urządzeniach, nie różni się od dodawania tych przeznaczonych na komputery. Aby stworzyć atrakcyjny interfejs, wystarczy wprowadzić tylko kilka drobnych poprawek.

Przy określaniu szerokości obrazów używaj jednostek względnych, by zapobiec przypadkowemu wyjściu poza widoczny obszar. Na przykład `width: 50%` powoduje, że obraz ma 50% szerokości elementu, w którym się znajduje (nie widocznego obszaru ani konkretnego rozmiaru w pikselach).

    img, embed, object, video {
      max-width: 100%;
    }
    

CSS pozwala, by treści wychodziły poza swój kontener, więc czasami trzeba użyć parametru `max-width: 100%`, by uniemożliwić to obrazom i innym elementom. Na przykład:

### TL;DR {: .hide-from-toc }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="Pzc5Dly_jEM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Pamiętaj, by w atrybucie `alt` elementów `img` podać treściwe opisy. Zwiększają one dostępność strony, przekazując informacje czytnikom ekranu i innym funkcjom ułatwień dostępu.

<div style="clear:both;">
</div>

    <img src="photo.png" srcset="photo@2x.png 2x" ...>
    

Atrybut `srcset` rozszerza działanie elementu `img`, ułatwiając wyświetlanie różnych plików graficznych w zależności od cech urządzenia. Podobnie jak natywna [funkcja CSS](#use_image-set_to_provide_high_res_images) `image-set`, atrybut `srcset` pozwala przeglądarce wybrać obraz, który najlepiej pasuje do możliwości urządzenia. Na przykład pokazać obraz 2x na ekranie 2x, a w przyszłości być może obraz 1x na urządzeniu 2x przy ograniczonej przepustowości sieci.

Przeglądarki, które nie obsługują atrybutu `srcset`, wyświetlają domyślny plik graficzny podany w atrybucie `src`. Dlatego zawsze trzeba dołączać obraz 1x, który można pokazać na dowolnym urządzeniu, niezależnie od jego możliwości. Gdy atrybut `srcset` jest obsługiwany, przed wysłaniem jakichkolwiek żądań przeglądarka analizuje listę rozdzielonych przecinkami par obrazów i warunków, po czym pobiera i wyświetla najbardziej odpowiednią grafikę.

### Korzystanie ze względnych rozmiarów obrazów

<img class="attempt-right" src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Jeśli chcesz wyświetlać obrazy na podstawie cech urządzenia, czyli dostosowywać grafikę, użyj elementu `picture`. Element `picture` pozwala stworzyć deklarację z wieloma wersjami obrazu, które zależą od różnych cech urządzenia &ndash; takich jak rozmiar, rozdzielczość, orientacja itp.

<div style="clear:both;">
</div>

Dogfood: The `picture` element is beginning to land in browsers. Although it's not available in every browser yet, we recommend its use because of the strong backward compatibility and potential use of the [Picturefill polyfill](https://scottjehl.github.io/picturefill/). See the [ResponsiveImages.org](http://responsiveimages.org/#implementation) site for further details.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="QINlm3vjnaY"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Note: Element `picture` zaczyna pojawiać się w przeglądarkach. Mimo że jeszcze nie jest dostępny w każdej z nich, zalecamy jego stosowanie, bo ma dużą zgodność wsteczną i pozwala wykorzystać kod [polyfill Picturefill](https://scottjehl.github.io/picturefill/). Szczegółowe informacje znajdziesz na [ResponsiveImages.org](http://responsiveimages.org/#implementation).

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/media.html" region_tag="picture" %}
</pre>

Elementu `picture` należy używać wtedy, gdy mamy obraz źródłowy w różnych gęstościach lub gdy zasady projektowania elastycznego wymagają użycia nieco innego obrazu na niektórych typach ekranów. Podobnie jak w przypadku elementu `video`, możesz dodać wiele elementów `source`. To pozwala wskazać różne pliki graficzne w zależności od zapytań o media czy formatu obrazu.

Jeśli przeglądarka w przykładzie powyżej ma szerokość co najmniej 800&nbsp;pikseli, to w zależności od rozdzielczości urządzenia wyświetli się plik `head.jpg` lub `head-2x.jpg`. Jeśli szerokość widoku wynosi od 450 do 800&nbsp;pikseli, na tej samej zasadzie pojawi się plik `head-small.jpg` lub `head-small-2x.jpg`. W przypadku szerokości ekranu mniejszej niż 450&nbsp;pikseli i zgodności wstecznej, gdy element `picture` nie jest obsługiwany, przeglądarka renderuje element `img`, który zawsze należy dołączyć.

#### Względne rozmiary obrazów

Jeśli ostateczny rozmiar obrazu jest nieznany, trudno określić deskryptor gęstości przy obrazach źródłowych. Szczególnie wtedy, gdy obrazy rozciągają się proporcjonalnie do szerokości przeglądarki i zmieniają się płynnie w zależności od jej rozmiaru.

Zamiast podawać stałe rozmiary i gęstości obrazów, wielkość każdego z nich możesz określić, dodając deskryptor szerokości do rozmiaru w elemencie image. To pozwala przeglądarce automatycznie obliczać skuteczną gęstość pikseli i wybierać najlepszy obraz do pobrania.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/sizes.html" region_tag="picture" %}
</pre>

W przykładzie powyżej renderujemy obraz, który ma połowę szerokości widocznego obszaru (`sizes="50vw"`). Przeglądarka w zależności od swojej szerokości i współczynnika pikseli urządzenia wybiera właściwy obraz, bez względu na to, jak duże jest jej okno. Ta tabela pokazuje, który obraz wybierze przeglądarka:

W wielu przypadkach rozmiar lub obraz może się zmieniać w zależności od punktów granicznych w układzie strony. Na przykład na małym ekranie obraz może rozciągać się na całą szerokość widocznego obszaru, a na większym &ndash; zajmować tylko jego część.

<table class="">
  <tr>
    <th data-th="Browser width">
      Szerokość przeglądarki
    </th>
    
    <th data-th="Device pixel ratio">
      Współczynnik pikseli urządzenia
    </th>
    
    <th data-th="Image used">
      Wyświetlony obraz
    </th>
    
    <th data-th="Effective resolution">
      Skuteczna rozdzielczość
    </th>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400&nbsp;pikseli
    </td>
    
    <td data-th="Device pixel ratio">
      1
    </td>
    
    <td data-th="Image used">
      <code>200.png</code>
    </td>
    
    <td data-th="Effective resolution">
      1&nbsp;x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400&nbsp;pikseli
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2&nbsp;x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      320&nbsp;pikseli
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2,5&nbsp;x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      600&nbsp;pikseli
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>800.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2,67&nbsp;x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      640&nbsp;pikseli
    </td>
    
    <td data-th="Device pixel ratio">
      3
    </td>
    
    <td data-th="Image used">
      <code>1000.png</code>
    </td>
    
    <td data-th="Effective resolution">
      3,125&nbsp;x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      1100&nbsp;pikseli
    </td>
    
    <td data-th="Device pixel ratio">
      1
    </td>
    
    <td data-th="Image used">
      <code>1400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      1,27&nbsp;x
    </td>
  </tr>
</table>

#### Uwzględnianie punktów granicznych przy elastycznych obrazach

Atrybut `sizes` w przykładowym kodzie powyżej zawiera kilka zapytań o media, które określają rozmiar obrazu. Gdy szerokość przeglądarki przekracza 600&nbsp;pikseli, obraz ma 25% szerokości widocznego obszaru, od 500 do 600&nbsp;pikseli ma 50%, a poniżej 500&nbsp;pikseli &ndash; pełną szerokość.

<pre class="prettyprint">{% includecode adjust_indentation="auto" content_path="web/fundamentals/design-and-ux/responsive/_code/breakpoints.html" region_tag="picture" %}
</pre>

Klienci chcą zobaczyć, co kupują. Spodziewają się, że na stronie sklepu będą mogli wyświetlić zbliżenia produktu w wysokiej rozdzielczości, by lepiej przyjrzeć się szczegółom. [Uczestników naszego badania](/web/fundamentals/getting-started/principles/) irytował brak takiej możliwości.

Dobry przykład obrazów, które można kliknąć i rozwinąć, znajdziemy w witrynie J. Crew. Znikająca nakładka wskazuje, że obraz można kliknąć, by go powiększyć i zobaczyć drobne szczegóły.

### Rozszerzanie elementów `img` o atrybut `srcset` na potrzeby urządzeń z wysoką liczbą DPI<figure class="attempt-right"> 

<img src="img/sw-make-images-expandable-good.png" srcset="img/sw-make-images-expandable-good.png 1x, img/sw-make-images-expandable-good-2x.png 2x" alt="Witryna J. Crew z rozwijanym zdjęciem produktu" /> <figcaption>Witryna J. Crew z rozwijanym zdjęciem produktu</figcaption> </figure> 

[Technika obrazów kompresowanych](http://www.html5rocks.com/en/mobile/high-dpi/#toc-tech-overview) pozwala wyświetlać bardzo skompresowane obrazy 2x na wszystkich urządzeniach, bez względu na ich faktyczne możliwości. W zależności od typu obrazu i poziomu kompresji zmiana jakości może być niezauważalna, ale rozmiar pliku znacznie się zmniejsza.

A good example of tappable, expandable images is provided by the J. Crew site. A disappearing overlay indicates that an image is tappable, providing a zoomed in image with fine detail visible.

<div style="clear:both;">
</div>

### Używanie elementu `picture` przy dostosowywaniu grafiki w postaci elastycznych obrazów

#### Obrazy kompresowane

Note: Zachowaj ostrożność przy korzystaniu z technik kompresji, bo dekodowanie wymaga większej ilości pamięci i obciąża procesor. Zmiana rozmiaru dużych obrazów, by zmieściły się na mniejszym ekranie, wymaga znacznych zasobów i jest szczególnie uciążliwa na słabszych urządzeniach z niewielką pamięcią i mocą procesora.

Zastępowanie obrazów w JavaScripcie pozwala sprawdzić możliwości urządzenia i zastosować najlepsze rozwiązanie. Można sprawdzić współczynnik pikseli urządzenia (dzięki `window.devicePixelRatio`) oraz szerokość i wysokość ekranu, a nawet przeanalizować połączenie internetowe (korzystając z `navigator.connection` lub wysyłając fałszywe żądanie). Po zebraniu wszystkich tych informacji można wybrać obraz do wczytania.

Dużą wadą tej metody jest to, że użycie JavaScriptu opóźnia wczytanie obrazu przynajmniej do momentu zakończenia działania parsera podglądu. To oznacza, że pobieranie obrazów zaczyna się dopiero po wywołaniu zdarzenia `pageload`. Oprócz tego przeglądarka zwykle wczytuje zarówno obraz 1x, jak i 2x, pobierając więcej danych.

#### Zastępowanie obrazów w JavaScripcie

Właściwość CSS `background` to skuteczne narzędzie do umieszczania złożonych obrazów w elementach, które ułatwia dodawanie wielu obrazów, pozwala powtarzać je w elemencie itp. W połączeniu z zapytaniami o media staje się jeszcze bardziej przydatna, umożliwiając warunkowe wczytywanie obrazów na podstawie rozdzielczości ekranu, rozmiaru widocznego obszaru i innych parametrów.

Zapytania o media nie tylko wypływają na układ strony, ale też pozwalają warunkowo wczytywać obrazy i dostosowywać grafikę na podstawie szerokości widocznego obszaru.

#### Inlining images: raster and vector

W przykładzie poniżej na mniejszych ekranach tylko plik `small.png` jest pobierany i stosowany do elementu `div`. Z kolei na większych polecenie `background-image: url(body.png)` jest stosowane do treści, a `background-image: url(large.png)` &ndash; do elementu `div`.

Funkcja `image-set()` w CSS rozszerza działanie właściwości `background`, ułatwiając wyświetlanie różnych plików graficznych w zależności od cech urządzenia. Dzięki niej przeglądarka może wybrać obraz, który najlepiej pasuje do możliwości urządzenia. Na przykład pokazać obraz 2x na ekranie 2x lub obraz 1x na urządzeniu 2x przy ograniczonej przepustowości sieci.

Oprócz wczytania poprawnej grafiki przeglądarka także odpowiednio ją skaluje. Krótko mówiąc, przeglądarka zakłada, że obrazy 2x są dwa razy większe niż 1x, więc pomniejsza je dwukrotnie, by miały właściwy rozmiar na stronie.

##### SVG

Funkcja `image-set()` jest dość nowa i działa tylko w Chrome i Safari z przedrostkiem dostawcy `-webkit`. Pamiętaj też, by dołączyć obraz zastępczy, na wypadek gdyby funkcja `image-set()` nie była obsługiwana, na przykład:

Kod powyżej powoduje wczytanie odpowiedniego zasobu w przeglądarkach, w których działa funkcja `image-set`, a w pozostałych &ndash; wyświetlenie zasobu zastępczego 1x. Oczywiście dopóki niewiele przeglądarek obsługuje `image-set()`, najczęściej użytkownicy będą widzieć zasób 1x.

<img class="side-by-side" src="img/html5.png" alt="HTML5 logo, PNG format" /> <img class="side-by-side" src="img/html5.svg" alt="HTML5 logo, SVG format" />

Chrome, Firefox i Opera obsługują standardowe polecenie `(min-resolution: 2dppx)`, a Safari i przeglądarka w Androidzie wymagają starszej wersji składni z przedrostkiem dostawcy i bez jednostki `dppx`. Pamiętaj, że te style wczytują się tylko wtedy, gdy urządzenie pasuje do zapytania o media, więc musisz zdefiniować też style stosowane w podstawowym przypadku. Dzięki temu zyskasz pewność, że nawet gdy przeglądarka nie obsługuje rozdzielczości podanej w konkretnych zapytaniach o media, wyrenderuje poprawny widok.

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

Jeśli to możliwe, jako ikon na swojej stronie użyj obrazów SVG, a w niektórych przypadkach znaków Unicode.

<svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg><svg class="side-by-side" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" width="50%" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5"/><path fill="#E44D26" d="M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z"/><polygon fill="#F16529" points="480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2"/><polygon fill="#F16529" points="541,343 480,343 480,422.4 535.2,407.4"/><polygon fill="#EBEBEB" points="414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4"/><polygon fill="#EBEBEB" points="479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474"/><polygon points="343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5"/><polygon points="425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25"/><polygon points="508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5"/><polygon points="642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5"/><polygon fill="#FFFFFF" points="480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1"/><polygon fill="#FFFFFF" points="479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7"/></g></g></g></svg>

##### Data URI

Oprócz normalnego zestawu znaków czcionka Unicode może obejmować formy numeryczne (&#8528;), strzałki (&#8592;), operatory matematyczne (&#8730;), kształty geometryczne (&#9733;), symbole elementów sterujących (&#9654;), kody alfabetu Braille`a (&#10255;), nuty (&#9836;), litery greckie (&#937;), a nawet bierki szachowe (&#9822;).

    <img src="data:image/svg+xml;base64,[data]">
    

Znaki Unicode można dodawać do strony tak samo jak nazwane elementy &ndash; `&#XXXX`, gdzie `XXXX` to numer znaku Unicode. Na przykład:

    background-image: image-set(
      url(icon1x.jpg) 1x,
      url(icon2x.jpg) 2x
    );
    

Jesteś super&#9733;

Jeśli potrzebujesz bardziej złożonych ikon, skorzystaj z formatu SVG, który ma niewielkie wymagania, jest prosty w użyciu i działa ze stylami CSS. Obrazy SVG mają kilka zalet w porównaniu z obrazami rastrowymi:

##### Inlining in CSS

Data URIs and SVGs can also be inlined in CSS&mdash;and this is supported on both mobile and desktop. Here are two identical-looking images implemented as background images in CSS; one Data URI, one SVG:

<span class="side-by-side" id="data_uri"></span> <span class="side-by-side" id="svg"></span>

##### Inlining pros & cons

Inline code for images can be verbose&mdash;especially Data URIs&mdash;so why would you want to use it? To reduce HTTP requests! SVGs and Data URIs can enable an entire web page, including images, CSS and JavaScript, to be retrieved with one single request.

Istnieją setki darmowych i płatnych czcionek z ikonami, np. [Font Awesome](http://fortawesome.github.io/Font-Awesome/){: .external }, [Pictos](http://pictos.cc/) czy [Glyphicons](http://glyphicons.com/).

* Używaj obrazów, które najlepiej pasują do cech wyświetlacza. Weź pod uwagę rozmiar ekranu, rozdzielczość urządzenia i układ strony.
* Zmień właściwość `background-image` w CSS na potrzeby wyświetlaczy o wysokiej liczbie DPI, korzystając z zapytań o media z parametrami `min-resolution` i `-webkit-min-device-pixel-ratio`.
* Dodaj do znaczników atrybut srcset, by oprócz obrazów w skali 1x wyświetlać też wersje w wysokiej rozdzielczości.
* Rozważ spadek wydajności podczas stosowania technik zastępowania grafik w JavaScripcie lub wyświetlania mocno skompresowanych obrazów w wysokiej rozdzielczości na urządzeniach o niższej rozdzielczości.
* Data URIs cannot be cached, so must be downloaded for every page they're used on.
* They're not supported in IE 6 and 7, incomplete support in IE8.
* With HTTP/2, reducing the number of asset requests will become less of a priority.

Pamiętaj, by podczas wybierania ikon wziąć pod uwagę dodatkowe żądania HTTP i ilość pobieranych danych. Jeśli np. potrzebujesz tylko kilku ikon, lepiej użyć obrazu lub sprite`a graficznego.

## Używanie obrazów SVG jako ikon

Obrazy często stanowią większość pobranych danych i zajmują znaczną część powierzchni strony. W efekcie ich optymalizacja może przynieść zauważalny spadek ilości pobieranych danych i zwiększenie wydajności witryny. Im mniej bajtów musi pobrać przeglądarka, tym szybciej pobierze i wyświetli wszystkie zasoby oraz w mniejszym stopniu obciąży łącze klienta.

### Ustawianie obrazów produktów jako rozwijanych

* Zamiast obrazów rastrowych jako ikon użyj obrazów SVG lub znaków Unicode.
* Change the `background-image` property in CSS for high DPI displays using media queries with `min-resolution` and `-webkit-min-device-pixel-ratio`.
* Use srcset to provide high resolution images in addition to the 1x image in markup.
* Consider the performance costs when using JavaScript image replacement techniques or when serving highly compressed high resolution images to lower resolution devices.

### Inne techniki związane z obrazami

Należy wziąć pod uwagę dwa typy obrazów: [wektorowe](http://pl.wikipedia.org/wiki/Grafika_wektorowa){: .external } i [rastrowe](http://pl.wikipedia.org/wiki/Grafika_rastrowa). W przypadku obrazów rastrowych trzeba jeszcze wybrać właściwy format kompresji &ndash; np. GIF, PNG lub JPG.

**Obrazy rastrowe**, do których należą fotografie i inne grafiki, mają postać siatki osobnych kropek (pikseli). Zwykle pochodzą z aparatu lub skanera. Można też utworzyć je w przeglądarce, korzystając z elementu `canvas`. Wraz z powiększaniem się obrazu wzrasta wielkość pliku. Obraz przeskalowany do większego rozmiaru niż pierwotny staje się rozmazany, bo przeglądarka musi zgadywać, jak wypełnić brakujące piksele.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/image-set.html" region_tag="imageset" %}
</pre>

**Obrazy wektorowe**, do których należą logo i rysunki kreskowe, składają się z zestawu linii, krzywych, kształtów i kolorów wypełnienia. Powstają w takich programach jak Adobe Illustrator lub Inkscape i są zapisywane w formacie wektorowym, np. [SVG](http://css-tricks.com/using-svg/){: .external }. Obrazy wektorowe tworzy się z prostych elementów, dlatego można je skalować bez straty jakości i zmiany rozmiaru pliku.

### TL;DR {: .hide-from-toc }

Przy wyborze właściwego formatu musisz rozważyć zarówno rodzaj obrazu (rastrowy lub wektorowy), jak i jego treść (kolory, animacja, tekst itp.). Każdy format pasuje tylko do niektórych rodzajów obrazów oraz ma swoje zalety i wady.

    @media (min-resolution: 2dppx),
    (-webkit-min-device-pixel-ratio: 2)
    {
      /* High dpi styles & resources here */
    }
    

Podczas wybierania formatu postępuj zgodnie z tymi wskazówkami:

Plik można znacznie zmniejszyć, przetwarzając go po zapisaniu. Jest wiele narzędzi do kompresji obrazów &ndash; stratnej lub bezstratnej, online, z interfejsem graficznym, wierszem polecenia itp. W miarę możliwości najlepiej zautomatyzować optymalizację obrazów, by stała się uniwersalnym etapem procesu tworzenia.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" %}
</pre>

Niektóre dostępne narzędzia wykonują dodatkową, bezstratną kompresję plików JPG i PNG, bez wpływu na jakość obrazu. W przypadku JPG wypróbuj [jpegtran](http://jpegclub.org/){: .external } lub [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (dostępny tylko na Linuksa, uruchamiany z opcją --strip-all). W przypadku PNG wypróbuj [OptiPNG](http://optipng.sourceforge.net/) lub [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

Spriting CSS to technika, w której wiele obrazów łączy się w jeden obraz `arkusza sprite`ów`. Aby następnie wybrać konkretną grafikę, trzeba określić obraz tła elementu (arkusz sprite`ów) i przesunięcie wskazujące właściwą część.

### Używanie zapytań o media do warunkowego wczytywania obrazów lub dostosowywania grafiki

Media queries can create rules based on the [device pixel ratio](http://www.html5rocks.com/en/mobile/high-dpi/#toc-bg), making it possible to specify different images for 2x versus 1x displays.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

Zaletą spritingu jest obniżenie liczby plików pobieranych przy wyświetlaniu wielu obrazów, bez utrudniania zapisu w pamięci podręcznej.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" adjust_indentation="auto" %}
</pre>

Leniwe wczytywanie może znacznie przyspieszyć wyświetlanie długich stron, które w części widocznej po przewinięciu zawierają wiele obrazów. Obrazy te wczytują się w razie potrzeby lub po zakończeniu wczytywania i renderowania głównych treści. Oprócz podnoszenia wydajności leniwe wczytywanie pozwala tworzyć strony o nieograniczonej długości.

Podczas tworzenia takich stron zachowaj jednak ostrożność, bo wyszukiwarki mogą nie wykryć treści, które wczytują się dopiero po przewinięciu. Podobnie użytkownicy szukający informacji, które zwykle są w stopce, nigdy jej nie zobaczą, bo zawsze będą wczytywać się nowe treści.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

## Optymalizowanie obrazów pod kątem wydajności

Czasami najlepszym rozwiązaniem jest całkowita rezygnacja z dodawania obrazu. Gdy to możliwe, warto używać natywnych funkcji przeglądarki, które dają takie same lub podobne efekty. Przeglądarki mogą obecnie generować elementy wizualne, które dawniej wymagały stosowania obrazów. Dzięki temu przeglądarka nie musi już pobierać osobnych plików graficznych i nie wyświetla obrazów w dziwnej skali. Ikony można renderować, korzystając ze standardu Unicode lub specjalnych czcionek z ikonami.

### Używanie funkcji image-set do wyświetlania obrazów w wysokiej rozdzielczości

* To grafika wektorowa, którą można skalować bez ograniczeń.

### Używanie zapytań o media do wyświetlania obrazów w wysokiej rozdzielczości lub dostosowywania grafiki

Gdy to tylko możliwe, tekst powinien być tekstem, a nie elementem obrazu. Na przykład nie należy używać grafik jako nagłówków ani umieszczać w nich informacji kontaktowych takich jak numery telefonów czy adresy. To uniemożliwia użytkownikom skopiowanie i wklejenie tych danych oraz ukrywa je przed czytnikami ekranu. Strona jest wtedy nieelastyczna. Zamiast tego umieść tekst w znacznikach i w razie potrzeby użyj czcionek internetowych, by uzyskać odpowiedni styl.

Wiele przeglądarek pozwala korzystać z funkcji CSS, by tworzyć style, które dawniej wymagały stosowania obrazów. Na przykład właściwość `background` umożliwia tworzenie złożonych gradientów, `box-shadow` &ndash; cieni, a `border-radius` &ndash; zaokrąglonych narożników.

Including a unicode character is done in the same way named entities are: `&#XXXX`, where `XXXX` represents the unicode character number. For example:

    You're a super &#9733;
    

Pamiętaj, że te techniki wymagają cykli renderowania, co może mieć znaczenie na urządzeniach mobilnych. Jeśli będziesz ich nadużywać, możesz stracić uzyskane korzyści i obniżyć wydajność strony.

### TL;DR {: .hide-from-toc }

For more complex icon requirements, SVG icons are generally lightweight, easy to use, and can be styled with CSS. SVG have a number of advantages over raster images:

* To grafika wektorowa, którą nie tylko można skalować bez ograniczeń, ale też poddawać wygładzaniu krawędzi. W efekcie ikony czasami nie są odpowiednio ostre.
* W ograniczonym stopniu działają ze stylami CSS.
* Dokładne pozycjonowanie pikseli może być trudne ze względu na wysokość wiersza, odstępy między literami itp.
* Nie mają charakteru semantycznego i niezbyt dobrze współpracują z czytnikami ekranu oraz innymi funkcjami ułatwień dostępu.
* Przy braku prawidłowego zakresu mogą powodować pobieranie dużych plików, mimo że używasz tylko niewielkiego podzbioru dostępnych ikon.

<pre class="prettyprint">{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-svg.html){: target="_blank" .external }

### Zastępowanie prostych ikon znakami Unicode<figure class="attempt-right"> 

<img src="img/icon-fonts.png" class="center" srcset="img/icon-fonts.png 1x, img/icon-fonts-2x.png 2x" alt="Example of a page that uses FontAwesome for its font icons." /> <figcaption> <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html" target="_blank" class="external"> Example of a page that uses FontAwesome for its font icons. </a> </figcaption> </figure> 

Icon fonts are popular, and can be easy to use, but have some drawbacks compared to SVG icons:

* Nie wybieraj przypadkowo formatu obrazu. Zapoznaj się z dostępnymi formatami i wybierz ten najbardziej odpowiedni.
* W procesie tworzenia używaj narzędzi do optymalizacji i kompresji obrazów, by zmniejszyć rozmiary plików.
* Zmniejsz liczbę żądań HTTP, umieszczając często używane obrazy w sprite`ach graficznych.
* Rozważ opcję wczytywania obrazów dopiero wtedy, gdy po przewinięciu strony pojawią się w widoku, tak by skrócić czas początkowego wyświetlania strony i zmniejszyć ilość pobieranych danych.
* Unless properly scoped, they can result in a large file size for only using a small subset of the icons available.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/icon-font.html" region_tag="iconfont" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html){: target="_blank" .external }

There are hundreds of free and paid icon fonts available including [Font Awesome](https://fortawesome.github.io/Font-Awesome/), [Pictos](http://pictos.cc/){: .external }, and [Glyphicons](https://glyphicons.com/).

Be sure to balance the weight of the additional HTTP request and file size with the need for the icons. For example, if you only need a handful of icons, it may be better to use an image or an image sprite.

## Całkowite unikanie obrazów

Images often account for most of the downloaded bytes and also often occupy a significant amount of the visual space on the page. As a result, optimizing images can often yield some of the largest byte savings and performance improvements for your website: the fewer bytes the browser has to download, the less competition there is for client's bandwidth and the faster the browser can download and display all the assets.

### Zastępowanie złożonych ikon obrazami SVG

* Przy fotografiach użyj JPG.
* Przy grafikach wektorowych i zawierających jednolite kolory (np. logo i rysunki kreskowe) użyj SVG. Jeśli grafika wektorowa jest niedostępna, skorzystaj z WebP lub PNG.
* Zamiast formatu GIF użyj PNG, który pozwala zastosować więcej kolorów i ma lepsze współczynniki kompresji.
* Przy dłuższych animacjach użyj tagu `<video>`, który daje wyższą jakość obrazu i pozwala użytkownikowi sterować odtwarzaniem.

### Problemy przy stosowaniu czcionek z ikonami

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

### Wybór właściwego formatu

<img src="img/sprite-sheet.png" class="attempt-right" alt="Image sprite sheet used in example" />

CSS spriting is a technique whereby a number of images are combined into a single "sprite sheet" image. You can then use individual images by specifying the background image for an element (the sprite sheet) plus an offset to display the correct part.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/image-sprite.html){: target="_blank" .external }

Spriting has the advantage of reducing the number of downloads required to get multiple images, while still enabling caching.

### Zmniejszanie rozmiaru pliku

Lazy loading can significantly speed up loading on long pages that include many images below the fold by loading them either as needed or when the primary content has finished loading and rendering. In addition to performance improvements, using lazy loading can create infinite scrolling experiences.

Be careful when creating infinite scrolling pages&mdash;because content is loaded as it becomes visible, search engines may never see that content. In addition, users who are looking for information they expect to see in the footer, never see the footer because new content is always loaded.

## Avoid images completely

Sometimes the best image isn't actually an image at all. Whenever possible, use the native capabilities of the browser to provide the same or similar functionality. Browsers generate visuals that would have previously required images. This means that browsers no longer need to download separate image files thus preventing awkwardly scaled images. You can use unicode or special icon fonts to render icons.

### Korzystanie ze sprite`ów graficznych

Wherever possible, text should be text and not embedded into images. For example, using images for headlines or placing contact information&mdash;like phone numbers or addresses&mdash;directly into images prevents users from copying and pasting the information; it makes the information inaccessible for screen readers, and it isn't responsive. Instead, place the text in your markup and if necessary use webfonts to achieve the style you need.

### Stosowanie leniwego wczytywania

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