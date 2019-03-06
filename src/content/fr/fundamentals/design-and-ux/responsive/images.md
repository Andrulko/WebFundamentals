project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Les images font justement partie intégrante de chaque page. Cependant, elles constituent également la majorité des octets téléchargés. La conception de sites Web adaptatifs permet non seulement d'adapter les dispositions aux caractéristiques de l'appareil, mais aussi les images.

{# wf_updated_on: 2018-12-15 #} {# wf_published_on: 2014-04-29 #}

# Images {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Comme dit l'adage : 'Une image vaut mieux qu'un long discours'. Et les images font justement partie intégrante de chaque page. Cependant, elles constituent également la majorité des octets téléchargés. La conception de sites Web adaptatifs permet non seulement d'adapter les dispositions aux caractéristiques de l'appareil, mais aussi les images.

## Images dans le balisage

<img src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Other times the image may need to be changed more drastically: changing the proportions, cropping, and even replacing the entire image. In this case, changing the image is usually referred to as art direction. See [responsiveimages.org/demos/](https://responsiveimages.org/demos/) for more examples.

Dans d'autres cas, il se peut que l'image doive subir des modifications plus importantes : changement des proportions, recadrage, voire remplacement de toute l'image. On parle alors d''art direction'. Pour consulter d'autres exemples, rendez-vous sur [responsiveimages.org/demos/](http://responsiveimages.org/demos/){: .external }.

## Images dans la propriété CSS 

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

### Images adaptatives

* Utilisez des tailles relatives pour les images afin d'éviter tout dépassement accidentel de la capacité du conteneur.
* Utilisez l'élément `picture` pour spécifier des images différentes en fonction des caractéristiques de l'appareil (c'est ce que l'on désigne sous le nom d''art direction').
* Utilisez l'attribut `srcset` et le descripteur `x` dans l'élément `img` pour fournir au navigateur des indications quant à la meilleure image à utiliser parmi différentes densités.
* If your page only has one or two images and these are not used elsewhere on your site, consider using inline images to reduce file requests.

### Art direction

Particulièrement puissant, l'élément `img` télécharge, décode et affiche du contenu. De plus, les navigateurs modernes sont compatibles avec un large éventail de formats d'image. Inclure des images compatibles avec plusieurs appareils ne diffère pas de la méthode utilisée pour les ordinateurs de bureau. Il suffit de quelques réglages mineurs pour garantir une expérience utilisateur optimale.

Pensez à utiliser des unités relatives lors de l'indication de largeurs pour les images afin d'éviter tout dépassement accidentel de la capacité de la fenêtre d'affichage. Si vous définissez, par exemple, la valeur `width: 50%;`, la largeur de l'image équivaudra à 50 % de l'élément conteneur (et non à la fenêtre d'affichage, ni à la taille de pixel réelle).

    img, embed, object, video {
      max-width: 100%;
    }
    

Dans la mesure où la technologie CSS autorise le contenu à dépasser la capacité de son conteneur, il peut s'avérer nécessaire d'utiliser la valeur `max-width: 100%` pour éviter le débordement d'images et d'autre contenu. Par exemple :

### TL;DR {: .hide-from-toc }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="Pzc5Dly_jEM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Veillez à fournir des descriptions explicites au moyen de l'attribut `alt` sur les éléments `img`. Elles contribuent à rendre votre site plus accessible en offrant du contexte aux lecteurs d'écran et à d'autres technologies assistives.

<div style="clear:both;">
</div>

    <img src="photo.png" srcset="photo@2x.png 2x" ...>
    

L'attribut `srcset` améliore le comportement de l'élément `img`, facilitant ainsi la diffusion de plusieurs fichiers image pour différentes caractéristiques d'appareil. À l'instar de la `image-set` [fonction CSS](#use_image-set_to_provide_high_res_images) native de CSS, l'attribut `srcset` permet au navigateur de choisir l'image idéale en fonction des caractéristiques de l'appareil ; par exemple, une image 2x sur un écran 2x et, à l'avenir, une image 1x sur un appareil 2x sur un réseau à faible débit.

Sur les navigateurs non compatibles avec l'attribut `srcset`, le fichier image par défaut spécifié par l'attribut `src` est utilisé. C'est pourquoi il est important de toujours inclure une image 1x pouvant être affichée sur tout appareil, quelles qu'en soient les capacités. Lorsque l'attribut `srcset` est accepté, une liste d'images/de conditions séparées par des virgules est analysée avant de formuler des requêtes, et seule l'image appropriée est téléchargée et affichée.

### Appliquer des tailles relatives aux images

<img class="attempt-right" src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

La modification d'images sur la base des caractéristiques de l'appareil (opération connue sous le nom d''art direction') peut être réalisée à l'aide de l'élément `picture`. L'élément `picture` définit une solution déclarative pour fournir plusieurs versions d'une image sur la base de différentes caractéristiques, telles que la taille de l'appareil, la résolution, l'orientation, etc.

<div style="clear:both;">
</div>

Dogfood: The `picture` element is beginning to land in browsers. Although it's not available in every browser yet, we recommend its use because of the strong backward compatibility and potential use of the [Picturefill polyfill](https://scottjehl.github.io/picturefill/). See the [ResponsiveImages.org](http://responsiveimages.org/#implementation) site for further details.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="QINlm3vjnaY"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Note: L'élément `picture` commence à faire son apparition dans les navigateurs. Bien que cet élément ne soit pas encore disponible dans tous les navigateurs, il est conseillé de l'utiliser en raison de sa puissante rétrocompatibilité et de l'utilisation potentielle du [polyfill Picturefill](https://scottjehl.github.io/picturefill/). Pour plus d'informations, rendez-vous sur le site [ResponsiveImages.org](http://responsiveimages.org/#implementation).

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media.html" region_tag="picture"   adjust_indentation="auto" %}
</pre>

L'élément `picture` doit être utilisé lorsqu'une source d'image est présente dans plusieurs densités ou lorsque la conception de sites Web adaptatifs impose une image quelque peu différente sur certains types d'écrans. Comme c'est le cas pour l'élément `video`, plusieurs éléments `source` peuvent être inclus, ce qui permet de spécifier différents fichiers image en fonction des requêtes média ou du format d'image.

Dans l'exemple ci-dessus, si la largeur du navigateur est d'au moins 800 pixels, le fichier `head.jpg` ou `head-2x.jpg` est utilisé suivant la résolution de l'appareil. Si la largeur du navigateur est comprise entre 450 pixels et 800 pixels, le fichier `head-small.jpg` ou `head-small-2x.jp` est utilisé, une fois encore en fonction de la résolution de l'appareil. Pour les largeurs d'écran inférieures à 450 pixels et pour offrir une rétrocompatibilité lorsque l'élément `picture` n'est pas accepté, le navigateur affiche plutôt l'élément `img`, lequel doit toujours être inclus.

#### Images à dimensionnement relatif

Si la taille définitive de l'image est inconnue, il peut s'avérer difficile de spécifier un descripteur de densité pour les sources d'images. Cela vaut tout particulièrement pour les images qui couvrent une largeur proportionnelle du navigateur et qui sont fluides, en fonction de la taille du navigateur.

Au lieu de fournir des densités et des tailles d'image fixes, vous pouvez spécifier la taille de chaque image diffusée en ajoutant un descripteur de largeur avec la taille de l'élément d'image. Cela permet au navigateur de calculer automatiquement la densité de pixels effective et de choisir la meilleure image à télécharger.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/sizes.html" region_tag="picture"   adjust_indentation="auto" %}
</pre>

L'exemple ci-dessus affiche une image dont la largeur équivaut à la moitié de celle de la fenêtre d'affichage (sizes='50vw') et, en fonction de la largeur du navigateur et du rapport de pixel de l'appareil, permet au navigateur de choisir l'image appropriée, quelle que soit la taille de la fenêtre du navigateur. Le tableau ci-dessous illustre l'image qui sera choisie par le navigateur :

Dans la plupart des cas, il se peut que la taille ou l'image change en fonction des points de rupture de la mise en page du site. Sur un petit écran, par exemple, vous souhaitez que l'image occupe toute la largeur de la fenêtre d'affichage, alors qu'elle n'occupera qu'une petite partie sur un écran de plus grande dimension.

<table class="">
  <tr>
    <th data-th="Browser width">
      Largeur du navigateur
    </th>
    
    <th data-th="Device pixel ratio">
      Rapport de pixel de l'appareil
    </th>
    
    <th data-th="Image used">
      Image utilisée
    </th>
    
    <th data-th="Effective resolution">
      Résolution effective
    </th>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400 pixels
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
      400 pixels
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
      320 pixels
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
      600 pixels
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
      640 pixels
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
      1100 pixels
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

#### Tenir compte des points de rupture dans les images adaptatives

L'attribut `sizes` dans l'exemple ci-dessus utilise plusieurs requêtes média pour spécifier la taille de l'image. Si la largeur du navigateur est supérieure à 600 pixels, l'image équivaut à 25 % de la largeur de la fenêtre d'affichage. Si elle est comprise entre 500 pixels et 600 pixels, l'image équivaut à 25 % de la largeur de la fenêtre d'affichage. En-dessous de 500 pixels, elle occupe toute la largeur.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/breakpoints.html" region_tag="picture"   adjust_indentation="auto" %}
</pre>

Les clients veulent voir ce qu'ils achètent. Les personnes qui visitent les sites de vente au détail veulent être en mesure d'afficher des gros-plans en haute résolution des produits qu'ils consultent afin d'en visualiser les moindres détails, comme l'a démontré cette [étude menée par Google et AnswerLab](/web/fundamentals/getting-started/principles/).

Le site J. Crew constitue un excellent exemple de source d'images extensibles sur lesquelles l'utilisateur peut appuyer. Une superposition 'escamotable' indique une image sur laquelle il est possible d'appuyer pour afficher une image agrandie présentant les détails les plus infimes du produit.

### Améliorer les éléments `img` avec l'attribut `srcset` pour les écrans à haute densité de pixels<figure class="attempt-right"> 

<img src="img/sw-make-images-expandable-good.png" srcset="img/sw-make-images-expandable-good.png 1x, img/sw-make-images-expandable-good-2x.png 2x" alt="Site Web J. Crew avec une image de produit extensible" /> <figcaption>Site Web J. Crew avec une image de produit extensible</figcaption> </figure> 

La [technique d'image compressible](http://www.html5rocks.com/en/mobile/high-dpi/#toc-tech-overview){: .external} diffuse des images 2x fortement compressées vers tous les appareils, quelles qu'en soient les capacités réelles. En fonction du type d'image et du niveau de compression, la qualité d'image peut paraître inchangée, mais la taille du fichier diminue de manière significative.

A good example of tappable, expandable images is provided by the J. Crew site. A disappearing overlay indicates that an image is tappable, providing a zoomed in image with fine detail visible.

<div style="clear:both;">
</div>

### `Art direction` dans des images adaptatives avec l'élément `picture`

#### Images compressibles

Note: Soyez prudent lorsque vous utilisez la technique de compression, en raison des exigences supplémentaires sur le plan de la mémoire et du décodage. Le redimensionnement d'images sur des écrans de petite taille est une opération exigeante qui peut se révéler particulièrement laborieuse sur des appareils d'entrée de gamme disposant d'une mémoire et d'une puissance de traitement limitées.

La procédure de remplacement d'images JavaScript vérifie les capacités de l'appareil et 'agit en conséquence'. Vous pouvez déterminer le rapport de pixel de l'appareil à l'aide de `window.devicePixelRatio`, connaître la largeur et la hauteur de l'écran, et même procéder au reniflage de connexion réseau via `navigator.connection` ou émettre une fausse requête. Dès que vous avez collecté toutes ces informations, vous pouvez déterminer l'image à charger.

Le principal inconvénient de cette méthode est que l'utilisation du langage JavaScript diffère le chargement de l'image jusqu'à la fin de l'exécution de l'analyseur Look-Ahead, voire plus tard encore. En d'autres termes, le téléchargement des images ne commencera même pas avant le déclenchement de l'événement `pageload`. De plus, il est probable que le navigateur télécharge les images 1x et 2x entraînant, de ce fait, une augmentation du poids de la page.

#### Remplacement d'images JavaScript

La propriété CSS 'background' constitue un puissant outil pour ajouter des images complexes à des éléments, facilitant ainsi l'ajout de plusieurs images, leur répétition, etc. Associée à des requêtes média, la propriété 'background' s'avère encore plus puissante et permet notamment le chargement conditionnel d'images sur la base de la résolution d'écran, de la taille de la fenêtre d'affichage, etc.

Les requêtes média n'affectent pas seulement la mise en page. Vous pouvez également les utiliser pour le chargement conditionnel d'images ou le changement d'images en fonction de la largeur de la fenêtre d'affichage.

#### Inlining images: raster and vector

Dans l'exemple ci-dessous, seul le fichier `small.png` est téléchargé et appliqué à l'élément `div` de contenu sur les écrans de plus petite taille, tandis que, sur les écrans plus grands, `background-image: url(body.png)` est appliqué à `body` et `background-image: url(large.png)`, à l'élément `div` de contenu.

La fonction `image-set()` de CSS améliore le comportement de la propriété `background`, facilitant ainsi la diffusion de plusieurs fichiers image pour différentes caractéristiques d'appareil. Cela permet au navigateur de choisir l'image idéale en fonction des caractéristiques de l'appareil ; par exemple, une image 2x sur un écran 2x, ou bien une image 1x sur un appareil 2x sur un réseau à faible débit.

Outre le chargement de l'image appropriée, le navigateur la dimensionne comme il se doit. En d'autres termes, le navigateur suppose que les images 2x sont deux fois plus grandes que les images 1x et les réduit donc selon un facteur 2, de sorte qu'elles semblent avoir la même taille sur la page.

##### SVG

La fonction `image-set()` est relativement récente et seuls les navigateurs Chrome et Safari l'acceptent actuellement avec le préfixe fournisseur `-webkit`. Il convient également de veiller à inclure une image de substitution lorsque la fonction `image-set()` n'est pas acceptée. Par exemple :

Dans l'exemple ci-dessus, l'élément approprié est chargé dans les navigateurs compatibles avec la fonction `image-set`, tandis que l'élément 1x est utilisé dans les autres cas. Notons toutefois une restriction, et elle est de taille : dans la mesure où peu de navigateurs sont compatibles avec la fonction `image-set()`, la plupart d'entre eux recevront l'élément 1x.

<img class="side-by-side" src="img/html5.png" alt="HTML5 logo, PNG format" /> <img class="side-by-side" src="img/html5.svg" alt="HTML5 logo, SVG format" />

Les navigateurs Chrome, Firefox et Opera acceptent tous la syntaxe `(min-resolution: 2dppx)` standard. Pour Safari et Android, en revanche, l'ancienne syntaxe sans `dppx` est requise. Pour rappel, ces styles ne sont chargés que si l'appareil correspond à la requête média. En outre, vous devez spécifier des styles pour le scénario de base. Cette méthode offre également l'avantage d'afficher quelque chose si le navigateur n'accepte pas les requêtes média spécifiques à la résolution.

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

Lorsque vous ajoutez des icônes à une page, utilisez des icônes SVG dans la mesure du possible ou, dans certains cas, des caractères Unicode.

<svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg><svg class="side-by-side" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" width="50%" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5"/><path fill="#E44D26" d="M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z"/><polygon fill="#F16529" points="480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2"/><polygon fill="#F16529" points="541,343 480,343 480,422.4 535.2,407.4"/><polygon fill="#EBEBEB" points="414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4"/><polygon fill="#EBEBEB" points="479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474"/><polygon points="343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5"/><polygon points="425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25"/><polygon points="508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5"/><polygon points="642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5"/><polygon fill="#FFFFFF" points="480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1"/><polygon fill="#FFFFFF" points="479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7"/></g></g></g></svg>

##### Data URI

Outre le jeu de caractères normal, Unicode peut contenir des symboles pour certains formats de nombres (&#8528;), des flèches (&#8592;), des signes mathématiques (&#8730;), des formes géométriques (&#9733;), des images de commandes (&#9654;), des motifs en braille (&#10255;), des notes de musique (&#9836;), des lettres de l'alphabet grec (&#937;) et même des pièces de jeu d'échecs (&#9822;).

    <img src="data:image/svg+xml;base64,[data]">
    

Pour inclure un caractère unicode, vous devez utiliser le même format que pour les entités nommées : `&#XXXX`, `XXXX` représentant le numéro de caractère Unicode. Par exemple :

    background-image: image-set(
      url(icon1x.jpg) 1x,
      url(icon2x.jpg) 2x
    );
    

Vous êtes une super &#9733;

Pour les icônes dont les besoins sont plus complexes, les icônes SVG sont généralement d'un poids modéré, faciles à utiliser et peuvent être formatées avec CSS. Les icônes SVG présentent de nombreux avantages par rapports aux images matricielles :

##### Inlining in CSS

Data URIs and SVGs can also be inlined in CSS&mdash;and this is supported on both mobile and desktop. Here are two identical-looking images implemented as background images in CSS; one Data URI, one SVG:

<span class="side-by-side" id="data_uri"></span> <span class="side-by-side" id="svg"></span>

##### Inlining pros & cons

Inline code for images can be verbose&mdash;especially Data URIs&mdash;so why would you want to use it? To reduce HTTP requests! SVGs and Data URIs can enable an entire web page, including images, CSS and JavaScript, to be retrieved with one single request.

Des centaines de polices d'icônes gratuites et payantes existent, notamment [Font Awesome](http://fortawesome.github.io/Font-Awesome/){: .external }, [Pictos](http://pictos.cc/){: .external} et [Glyphicons](http://glyphicons.com/){: .external}.

* Utilisez l'image la mieux adaptée aux caractéristiques de l'écran en tenant compte de la taille de lécran, de la résolution de l''appareil et de la mise en page.
* Dans le cas des écrans à haute densité de pixels, modifiez la propriété `background-image` dans la feuille de style à l'aide de requêtes média avec `min-resolution` et `-webkit-min-device-pixel-ratio`.
* Utilisez l'attribut 'srcset' pour fournir des images en haute résolution en plus de l'image 1x dans le balisage.
* Tenez compte des critères de performances lors de l'utilisation de techniques de remplacement d'images JavaScript ou lors de la diffusion d'images haute résolution utilisant un taux de compression élevé sur des appareils de plus faible résolution.
* Data URIs cannot be cached, so must be downloaded for every page they're used on.
* They're not supported in IE 6 and 7, incomplete support in IE8.
* With HTTP/2, reducing the number of asset requests will become less of a priority.

Veillez à comparer le poids de la demande HTTP et de la taille de fichier supplémentaires avec le besoin en icônes. Par exemple, si vous n'avez besoin que de quelques icônes, il peut s'avérer plus judicieux d'utiliser une image ou un sprite d'images.

## Utiliser des requêtes média pour fournir des images haute résolution ou changer les images en fonction des caractéristiques de l'appareil ('art direction')

Les images comptent souvent pour la majorité des octets téléchargés. De plus, elles occupent généralement une grande partie de l'espace visuel sur la page. Par conséquent, leur optimisation peut vous permettre de réaliser des économies d'octets considérables et d'améliorer sensiblement les performances de votre site Web : moins le navigateur devra télécharger d'octets, moins il y aura de concurrence pour la bande passante du client, et plus vite le navigateur pourra télécharger et afficher tous les éléments.

### Rendre extensibles les images de produit

* Pour les icônes, utilisez des images SVG ou des symboles Unicode à la place des images matricielles.
* Change the `background-image` property in CSS for high DPI displays using media queries with `min-resolution` and `-webkit-min-device-pixel-ratio`.
* Use srcset to provide high resolution images in addition to the 1x image in markup.
* Consider the performance costs when using JavaScript image replacement techniques or when serving highly compressed high resolution images to lower resolution devices.

### Autres techniques relatives aux images

Vous devez prendre en compte deux types d'images : les [images vectorielles](http://fr.wikipedia.org/wiki/Image_vectorielle){: .external } et les [images matricielles](http://fr.wikipedia.org/wiki/Image_matricielle){: .external }. Dans le cas des images matricielles, vous devez également choisir le format de compression adéquat ; "GIF", "PNG" ou "JPG", par exemple.

Les **images matricielles**, telles que les photos et d'autres images, sont représentées sous la forme d'une grille de points individuels ou pixels. Elles proviennent généralement d'un appareil photo ou d'un scanner, ou elles peuvent être créées dans le navigateur à l'aide de l'élément `canvas`. Plus ces images sont grandes, plus la taille du fichier est importante. Lorsque ces images sont redimensionnées au-delà de leur taille initiale, elles deviennent floues, car le navigateur doit 'deviner' comment remplir les pixels manquants.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-set.html" region_tag="imageset"   adjust_indentation="auto" %}
</pre>

Les **images vectorielles**, telles que les logos et les illustrations au trait, sont définies par un ensemble de courbes, de lignes, de formes et de couleurs de remplissage. Elles sont créées à l'aide de programmes comme Adobe Illustrator et Inkscape, et enregistrées dans un format vectoriel tel que ['SVG'](http://css-tricks.com/using-svg/){: .external }. Les images vectorielles étant basées sur des primitives simples, leur dimensionnement n'entraîne aucune perte de qualité, ni modification de la taille du fichier.

### TL;DR {: .hide-from-toc }

Pour déterminer le format adéquat, il importe de tenir compte de l'origine de l'image (matricielle ou vectorielle), ainsi que du contenu (couleurs, animation, texte, etc.). Il n'existe pas de format idéal pour tous les types d'image, et chaque format présente des avantages et des inconvénients.

    @media (min-resolution: 2dppx),
    (-webkit-min-device-pixel-ratio: 2)
    {
      /* High dpi styles & resources here */
    }
    

Pour prendre la bonne décision, commencez par appliquer ces quelques directives :

Il est possible de réduire sensiblement la taille du fichier image en le soumettant à un post-traitement après l'avoir enregistré. Il existe tout un éventail d'outils destinés à la compression d'images : avec et sans perte, en ligne, avec interface graphique, par ligne de commande, etc. Lorsque cela s'avère possible, il est conseillé d'automatiser l'optimisation des images, de sorte qu'elle soit considérée comme un objet de première classe dans votre flux de travail.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx"   adjust_indentation="auto" %}
</pre>

Plusieurs outils permettent d'effectuer une compression sans perte plus poussée sur les fichiers `JPG` et `PNG`, sans nuire à la qualité d'image. Pour le format `JPG`, essayez [jpegtran](http://jpegclub.org/){: .external } ou [jpegoptim](http://freshmeat.net/projects/jpegoptim/){: .external} (disponible seulement sur Linux ; à exécuter avec l'option --strip-all). Pour le format `PNG`, essayez [OptiPNG](http://optipng.sourceforge.net/){: .external} ou [PNGOUT](http://www.advsys.net/ken/util/pngout.htm){: .external}.

La création de sprites CSS est une technique par laquelle plusieurs images sont regroupées dans un seul fichier appelé `sprite sheet`. Les images individuelles peuvent ensuite être utilisées en indiquant l'image d'arrière-plan d'un élément (la `sprite sheet`), plus un décalage pour afficher la partie appropriée.

### Utiliser des requêtes média pour le chargement conditionnel d'images ou le changement des images en fonction des caractéristiques de l'appareil ('art direction')

Media queries can create rules based on the [device pixel ratio](http://www.html5rocks.com/en/mobile/high-dpi/#toc-bg), making it possible to specify different images for 2x versus 1x displays.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

L'avantage de cette technique est de réduire le nombre de téléchargements requis pour obtenir plusieurs images, tout en permettant la mise en cache.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" adjust_indentation="auto" %}
</pre>

La technique de Lazy Loading permet d'accélérer sensiblement le chargement des longues pages qui comportent de nombreuses images sous la ligne de flottaison. Pour ce faire, les images sont chargées suivant les besoins ou une fois le chargement et le rendu du contenu principal terminés. Outre les améliorations qu'elle apporte sur le plan des performances, cette technique permet de créer des interfaces en défilement infini.

Soyez prudent lorsque vous créez des pages en défilement infini. En effet, puisque le contenu est chargé à mesure qu'il devient visible, il se peut que les moteurs de recherche ne le détectent jamais. De plus, les utilisateurs qui s'attendent à trouver des informations dans le pied de page ne les verront jamais, car du nouveau contenu est chargé en permanence.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

## Utiliser des images SVG pour les icônes

{# include shared/related_guides.liquid inline=true list=page.related-guides.optimize #}

### Utiliser la fonction image-set pour fournir des images haute résolution

* Ce sont des graphiques vectoriels dont on peut modifier la taille à l'infini.

### TL;DR {: .hide-from-toc }

Dans certains cas, l'image idéale n'est, en réalité, pas du tout une image. Dans la mesure du possible, utilisez les capacités natives du navigateur pour proposer des fonctions identiques ou similaires. Les navigateurs génèrent des éléments visuels qui, auparavant, auraient nécessité des images. Outre le fait que les navigateurs ne doivent plus télécharger de fichiers image distincts, cela permet d'éviter que les images ne soient dimensionnées de façon maladroite. Les icônes peuvent être affichées à l'aide de polices Unicode ou de polices d'icônes spéciales.

Dans la mesure du possible, évitez d'intégrer le texte dans des images. Évitez, par exemple, d'utiliser des images comme titres ou encore de placer directement des informations telles que des numéros de téléphone ou des adresses dans des images. Outre le fait que ces informations ne peuvent pas être copiées et collées par les utilisateurs, elles sont inexploitables par les lecteurs d'écran et n'offrent aucune souplesse d'adaptation. Placez plutôt le texte dans le balisage et, au besoin, utilisez des polices Web pour obtenir le style souhaité.

Les navigateurs modernes peuvent utiliser des fonctionnalités CSS pour créer des styles qui, auparavant, auraient nécessité des images. Quelques exemples : des dégradés complexes peuvent être créés à l'aide de la propriété `background`, des ombres peuvent être créées à l'aide de `box-shadow` et des coins arrondis peuvent être ajoutés à l'aide de la propriété `border-radius`.

    You're a super &#9733;
    

You're a super &#9733;

### Remplacer les icônes simples par des symboles Unicode

Veuillez noter que l'utilisation de ces techniques exige des cycles de rendu, qui peuvent être relativement importants sur des appareils mobiles. En cas d'utilisation excessive, vous risquez de perdre les éventuels avantages obtenus et une baisse des performances est possible.

* Ce sont des graphiques vectoriels dont on peut modifier la taille à l'infini. Mais elles font parfois l'objet d'un anti-crénelage, ce qui génère des icônes moins fines que prévu.
* Styles limités avec CSS.
* Il est parfois difficile de les positionner parfaitement au pixel près, selon la hauteur de ligne, l'espacement des lettres, etc.
* Elles ne sont pas sémantiques et peuvent être difficiles à utiliser avec des lecteurs d'écran ou d'autres technologies d'assistance.
* Si elles ne sont pas conçues correctement, elles peuvent générer un fichier de taille importante pour une utilisation limitée à un petit sous-groupe d'icônes, parmi celles qui sont disponibles.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-svg.html){: target="_blank" .external }

### Remplacer les icônes complexes par des icônes SVG<figure class="attempt-right"> 

<img src="img/icon-fonts.png" class="center" srcset="img/icon-fonts.png 1x, img/icon-fonts-2x.png 2x" alt="Example of a page that uses FontAwesome for its font icons." /> <figcaption> <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html" target="_blank" class="external"> Example of a page that uses FontAwesome for its font icons. </a> </figcaption> </figure> 

Icon fonts are popular, and can be easy to use, but have some drawbacks compared to SVG icons:

* Ne choisissez pas un format d''image au hasard, mais tâchez d''utiliser le format le mieux adapté en parfaite connaissance de cause.
* Intégrez des outils de compression et d'optimisation d'images dans votre flux de travail afin de réduire la taille des fichiers.
* Placez les images fréquemment utilisées dans des sprites d'image en vue de réduire le nombre de requêtes HTTP.
* Il est judicieux de ne charger les images qu'après leur défilement afin d'accélérer le chargement initial de la page et de réduire son poids initial.
* Unless properly scoped, they can result in a large file size for only using a small subset of the icons available.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/icon-font.html" region_tag="iconfont" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html){: target="_blank" .external }

There are hundreds of free and paid icon fonts available including [Font Awesome](https://fortawesome.github.io/Font-Awesome/), [Pictos](http://pictos.cc/){: .external }, and [Glyphicons](https://glyphicons.com/).

Be sure to balance the weight of the additional HTTP request and file size with the need for the icons. For example, if you only need a handful of icons, it may be better to use an image or an image sprite.

## Optimiser les images dans une optique de performances

Images often account for most of the downloaded bytes and also often occupy a significant amount of the visual space on the page. As a result, optimizing images can often yield some of the largest byte savings and performance improvements for your website: the fewer bytes the browser has to download, the less competition there is for client's bandwidth and the faster the browser can download and display all the assets.

### Utiliser les polices d'icônes avec précaution

* Utilisez le format `JPG` pour les images photographiques.
* Utilisez le format `SVG` pour les illustrations vectorielles et les graphiques unis tels que les logos et les illustrations au trait. Si les illustrations vectorielles ne sont pas disponibles, essayez donc le format WebP ou PNG.
* Utilisez le format `PNG` plutôt que `GIF`, car il autorise davantage de couleurs et offre de meilleurs taux de compression.
* Pour les animations plus longues, il est conseillé d'utiliser l'élément `<video>` qui offre une meilleure qualité d'image et permet à l'utilisateur de contrôler la lecture.

### TL;DR {: .hide-from-toc }

There are two types of images to consider: [vector images](https://en.wikipedia.org/wiki/Vector_graphics) and [raster images](https://en.wikipedia.org/wiki/Raster_graphics). For raster images, you also need to choose the right compression format, for example: `GIF`, `PNG`, `JPG`.

**Raster images**, like photographs and other images, are represented as a grid of individual dots or pixels. Raster images typically come from a camera or scanner, or can be created in the browser with the `canvas` element. As the image size gets larger, so does the file size. When scaled larger than their original size, raster images become blurry because the browser needs to guess how to fill in the missing pixels.

**Vector images**, such as logos and line art, are defined by a set of curves, lines, shapes, and fill colors. Vector images are created with programs like Adobe Illustrator or Inkscape and saved to a vector format like [`SVG`](https://css-tricks.com/using-svg/). Because vector images are built on simple primitives, they can be scaled without any loss in quality or change in file size.

When choosing the appropriate format, it is important to consider both the origin of the image (raster or vector), and the content (colors, animation, text, etc). No one format fits all image types, and each has its own strengths and weaknesses.

Start with these guidelines when choosing the appropriate format:

* Évitez d''utiliser des images lorsque cela s''avère possible. Utilisez plutôt les fonctionnalités offertes par le navigateur pour les ombres, les dégradés, les coins arrondis, etc.
* Use `SVG` for vector art and solid color graphics such as logos and line art. If vector art is unavailable, try `WebP` or `PNG`.
* Use `PNG` rather than `GIF` as it allows for more colors and offers better compression ratios.
* For longer animations consider using `<video>`, which provides better image quality and gives the user control over playback.

### Choisir le bon format

You can reduce image file size considerably by "post-processing" the images after saving. There are a number of tools for image compression&mdash;lossy and lossless, online, GUI, command line. Where possible, it's best to try automating image optimization so that it's a first-class citizen in your workflow.

Several tools are available that perform further, lossless compression on `JPG` and `PNG` files with no effect on image quality. For `JPG`, try [jpegtran](http://jpegclub.org/) or [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (available on Linux only; run with the --strip-all option). For `PNG`, try [OptiPNG](http://optipng.sourceforge.net/) or [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

### Réduire la taille de fichier

<img src="img/sprite-sheet.png" class="attempt-right" alt="Image sprite sheet used in example" />

CSS spriting is a technique whereby a number of images are combined into a single "sprite sheet" image. You can then use individual images by specifying the background image for an element (the sprite sheet) plus an offset to display the correct part.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/image-sprite.html){: target="_blank" .external }

Spriting has the advantage of reducing the number of downloads required to get multiple images, while still enabling caching.

### Utiliser des sprites d'image

Lazy loading can significantly speed up loading on long pages that include many images below the fold by loading them either as needed or when the primary content has finished loading and rendering. In addition to performance improvements, using lazy loading can create infinite scrolling experiences.

Be careful when creating infinite scrolling pages&mdash;because content is loaded as it becomes visible, search engines may never see that content. In addition, users who are looking for information they expect to see in the footer, never see the footer because new content is always loaded.

## Éviter complètement les images

Sometimes the best image isn't actually an image at all. Whenever possible, use the native capabilities of the browser to provide the same or similar functionality. Browsers generate visuals that would have previously required images. This means that browsers no longer need to download separate image files thus preventing awkwardly scaled images. You can use unicode or special icon fonts to render icons.

### Lazy Loading

Wherever possible, text should be text and not embedded into images. For example, using images for headlines or placing contact information&mdash;like phone numbers or addresses&mdash;directly into images prevents users from copying and pasting the information; it makes the information inaccessible for screen readers, and it isn't responsive. Instead, place the text in your markup and if necessary use webfonts to achieve the style you need.

### TL;DR {: .hide-from-toc }

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