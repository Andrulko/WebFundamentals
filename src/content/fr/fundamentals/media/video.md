project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Découvrez les méthodes les plus simples pour ajouter des vidéos à votre site et pour garantir aux internautes la meilleure expérience possible sur tous les appareils.

{# wf_updated_on: 2014-04-28 #} {# wf_published_on: 2000-01-01 #}

# Vidéo {: .page-title }

{% include "web/_shared/contributors/samdutton.html" %}

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="j5fYOYrsocs"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Les internautes apprécient les vidéos. Elles peuvent être amusantes et instructives. Sur les appareils mobiles, les vidéos permettent d'accéder plus facilement à des informations. En revanche, elles prennent de la bande passante et ne fonctionnent pas toujours de manière identique sur chaque plate-forme. Les internautes n'aiment pas attendre qu'une vidéo charge, ni que rien ne se produise lorsqu'ils appuient sur le bouton de lecture. Consultez les articles suivants pour découvrir les méthodes les plus simples pour ajouter des vidéos à votre site et pour garantir aux internautes la meilleure expérience possible sur tous les appareils.

## Ajouter une vidéo

### TL;DR {: .hide-from-toc }

* Utilisez l'élément vidéo pour importer, décoder et lire des contenus vidéos sur votre site.
* Produisez des vidéos de plusieurs formats pour couvrir une gamme de plates-formes mobiles.
* Définissez correctement la taille des vidéos. Veillez à ce qu'elles ne débordent pas de leurs contenants.
* L'accessibilité est importante. Ajoutez l'élément de suivi en tant qu'élément enfant de l'élément vidéo.

### Ajouter l'élément vidéo

Découvrez les méthodes les plus simples pour ajouter des vidéos à votre site et pour garantir aux internautes la meilleure expérience possible sur tous les appareils.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4" type="video/mp4">
  <p>Ce navigateur n'est pas compatible avec l'élément vidéo.</p>
</video>

    - Ajoutez l'élément vidéo pour importer, décoder et lire des contenus vidéos sur votre site :
    

### Spécifier plusieurs formats de fichiers

Not all browsers support the same video formats. The `<source>` element lets you specify multiple formats as a fallback in case the user's browser doesn't support one of them.

Lorsque le navigateur analyse les balises `<source>`, l'attribut facultatif `type` lui permet de sélectionner le fichier à télécharger et à lire. Si le navigateur est compatible avec WebM, le format de fichier chrome.webm est lu. Dans le cas contraire, une vérification est lancée, pour savoir si la lecture de vidéos au format MPEG-4 est possible. Consultez la page [A Digital Media Primer for Geeks (Une première approche des médias numériques pour les geeks)](//www.xiph.org/video/vid1.shtml "Highly entertaining and informative video guide to digital video") pour découvrir le fonctionnement des contenus vidéo et audio sur le Web.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/video-main.html" region_tag="sourcetypes" adjust_indentation="auto" %}
</pre>

Cette approche comporte plusieurs avantages par rapport aux différentes méthodes d'établissement de scripts via HTML ou côté serveur, en particulier sur les plates-formes mobiles :

Chacun de ces points est particulièrement important pour les plates-formes mobiles. En effet, la bande passante et le temps de réponse sont cruciaux, car la patience de l'utilisateur risque d'être limitée. La performance peut également être affectée si un attribut type n'est pas inclus, lorsque plusieurs sources comportent des types qui ne sont pas compatibles.

À l'aide des outils de développeur de votre navigateur mobile, comparez l'activité du réseau [avec attributs type](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html) et [sans attributs type](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/notype.html). Vérifiez également les en-têtes de réponse dans les outils pour les développeurs de votre navigateur pour [vous assurer que le serveur transmet le bon type MIME](//developer.mozilla.org/en/docs/Properly_Configuring_Server_MIME_Types), sinon les vérifications de type de source vidéo ne fonctionneront pas.

Pour économiser la bande passante et pour que votre site soit plus adaptatif, utilisez l'API Media Fragments afin d'ajouter un intervalle de lecture à l'élément vidéo.

* Le développeur peut répertorier les formats par ordre de préférence.
* La commutation via le côté client natif réduit le temps de réponse. Une seule requête est envoyée pour obtenir du contenu.
* Il est plus simple, plus rapide et potentiellement plus fiable de laisser le navigateur déterminer un format que d'utiliser une base de données de support côté serveur avec détection d'agent utilisateur.
* La performance du réseau est améliorée lorsque le type de source de chaque fichier est indiquée. Le navigateur sélectionne ainsi une source vidéo sans avoir à télécharger cette dernière pour détecter le format.

Pour ajouter un fragment média, il vous suffit d'ajouter #t=\[start_time\]\[,end_time\] à l'URL du fichier multimédia. Par exemple, pour générer une lecture de la vidéo dans l'intervalle compris entre la seconde 5 et la seconde 10, saisissez le code suivant :

Vous pouvez également utiliser l'API Media Fragments pour fournir plusieurs vues de la même vidéo, comme des marqueurs temporels sur un DVD, sans avoir besoin d'encoder ni de traiter plusieurs fichiers.

Note: - L'API Media Fragments est compatible avec la plupart des plates-formes, à l'exception d'iOS. Assurez-vous que les demandes de type `Range` sont compatibles avec votre serveur. Elles sont activées par défaut sur la plupart des serveurs. Cependant, il arrive qu'elles soient désactivées sur certains services d'hébergement.'

### Vérifier les formats compatibles

À l'aide des outils pour les développeurs de votre navigateur, vérifiez la présence de la `chaîne Accept-Ranges: bytes` dans les en-têtes de réponse:

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm#t=5,10" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4#t=5,10" type="video/mp4">
  <p>Ce navigateur n'est pas compatible avec l'élément vidéo.</p>
</video>

To add a media fragment, you simply add `#t=[start_time][,end_time]` to the media URL. For example, to play the video between seconds 5 through 10, specify:

    <source src="video/chrome.webm#t=5,10" type="video/webm">
    

Ajoutez un attribut affiche à l'élément vidéo de sorte que l'internaute puisse se faire une idée du contenu dès le chargement de l'élément, sans devoir télécharger la vidéo ni démarrer la lecture.

Une affiche constitue également une solution de repli, si l'attribut `src` de la vidéo est rompu ou si aucun des formats vidéo disponibles n'est compatible. Le seul inconvénient des affiches est qu'elles nécessitent une requête de fichier supplémentaire, ce qui consomme de la bande passante et entraîne un traitement de rendu. Pour en savoir plus, consultez la page \[Optimisation des images\] (../../performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html#image-optimization).

Voici la comparaison de deux vidéos, avec et sans affiche. L'affiche est représentée en échelle de gris, pour montrer qu'il ne s'agit pas de la vidéo :

<img class="center" alt="Capture d'écran Android Chrome, format portrait : sans affiche"
src="images/Accept-Ranges-Chrome-Dev-Tools.png" />

### Créer une vidéo dans plusieurs formats

Certains formats vidéos ne sont pas compatibles avec toutes les plates-formes. Vérifiez quels sont les formats compatibles avec les plates-formes principales, puis assurez-vous que votre vidéo fonctionne dans chacun d'entre eux.

    <source src="video/chrome.webm#t=5,10" type="video/webm">
    

Utilisez l'attribut `canPlayType()` pour savoir quels formats vidéos sont compatibles. La méthode nécessite un argument de chaîne composé d'un `type MIME` et de codecs facultatifs. Elle renvoie ensuite l'une des valeurs suivantes :

Voici quelques exemples d'arguments `canPlayType()` et de valeurs renvoyées lors de l'exécution dans Chrome :

<div class="attempt-left">
  <figure>
    <img alt="Android Chrome screenshot, portrait: no poster"
    src="images/Chrome-Android-video-no-poster.png">
    <figcaption>
      Android Chrome screenshot, portrait: no poster
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Android Chrome screenshot, portrait: with poster"
    src="images/Chrome-Android-video-poster.png">
    <figcaption>
      Android Chrome screenshot, portrait: with poster
     </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

## Définir un intervalle de lecture

Les nombreux outils suivants permettent d'enregistrer la même vidéo dans différents formats :

### Vérifier la taille de la vidéo

Vous voulez savoir quel format vidéo a été sélectionné par le navigateur ?

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Valeur renvoyée</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Return value">(chaîne vide)</td>
      <td data-th="Description">Le contenant et/ou le codec n'est pas compatible.</td>
    </tr>
    <tr>
      <td data-th="Return value"><code>maybe</code></td>
      <td data-th="Description">
        Le contenant et le(s) codec(s) sont peut-être compatibles mais le navigateur
        doit télécharger une partie de la vidéo pour vérifier.
      </td>
    </tr>
    <tr>
      <td data-th="Return value"><code>probably</code></td>
      <td data-th="Description">Le format semble être compatible.
      </td>
    </tr>
  </tbody>
</table>

Dans JavaScript, utilisez la propriété `currentSrc` de la vidéo pour renvoyer la source utilisée.

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Type"><code>video/xyz</code></td>
      <td data-th="Response">(chaîne vide)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response">(chaîne vide)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="nonsense, noise"</code></td>
      <td data-th="Response">(chaîne vide)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/mp4; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response"><code>probably</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm</code></td>
      <td data-th="Response"><code>maybe</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm; codecs="vp8, vorbis"</code></td>
      <td data-th="Response"><code>probably</code></td>
    </tr>
  </tbody>
</table>

### Éviter les débordements

Pour regarder une démonstration, [cliquez ici](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html) : Chrome et Firefox ont sélectionné `chrome.webm` (car c'est le premier format répertorié dans la liste des sources potentielles compatibles avec ces navigateurs). Le navigateur Safari a sélectionné `chrome.mp4`.

* Outils pour les ordinateurs de bureau : [FFmpeg](//ffmpeg.org/)
* Applications d'interface graphique utilisateur : [Miro](//www.mirovideoconverter.com/), [HandBrake](//handbrake.fr/), [VLC](//www.videolan.org/)
* Services d'encodage/transcodage en ligne : [Zencoder](//en.wikipedia.org/wiki/Zencoder), [Amazon Elastic Encoder](//aws.amazon.com/elastictranscoder)

### Fonctionnement de l'orientation selon les appareils

La taille des vidéos est importante lorsqu'il s'agit de satisfaire les internautes.

La taille réelle de la trame vidéo telle qu'elle est encodée peut différer des dimensions de l'élément vidéo, tout comme une image peut ne pas s'afficher à sa taille réelle.

## Inclure une affiche

Pour vérifier la taille encodée d'une vidéo, utilisez les propriétés d'élément vidéo `videoWidth` et `videoHeight`. Les propriétés `width` et `height` renvoient les dimensions de l'élément vidéo, dont la taille a pu être définie via CSS ou des attributs de largeur et de hauteur intégrés à la page.

### Contrôler l'affichage en mode plein écran du contenu

* La consommation de données peut s'avérer coûteuse.
* Lorsque le téléchargement et la lecture du contenu multimédia démarrent sans demander l'autorisation de l'internaute, la bande passante et le CPU risquent d'être monopolisés de façon inattendue et d'entraîner un retard dans l'affichage de la page.
* L'internaute peut se trouver dans un environnement dans lequel la lecture d'un contenu vidéo ou audio peut le déranger.

### Ajouter des sous-titres pour améliorer l'accessibilité

Lorsque l'élément vidéo est trop grand pour la fenêtre d'affichage, il risque de déborder et d'empêcher l'internaute de voir le contenu ou d'utiliser les commandes.

To check the encoded size of a video, use the video element `videoWidth` and `videoHeight` properties. `width` and `height` return the dimensions of the video element, which may have been sized using CSS or inline width and height attributes.

### Attributs d'élément vidéo

Vous pouvez contrôler les dimensions de la vidéo dans JavaScript ou CSS. Les bibliothèques et les plug-ins JavaScript, comme [FitVids](//fitvidsjs.com/), par exemple, permettent de conserver la taille et le format d'image appropriés, même pour les vidéos Flash de YouTube et d'autres sources.

<div class="attempt-left">
  <figure>
    <img alt="Android Chrome screenshot, portrait: unstyled video element overflows
    viewport" src="images/Chrome-Android-portrait-video-unstyled.png">
    <figcaption>
      Android Chrome screenshot, portrait: unstyled video element overflows viewport
    </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Android Chrome screenshot, landscape: unstyled video element overflows
    viewport" src="images/Chrome-Android-landscape-video-unstyled.png">
    <figcaption>
      Android Chrome screenshot, landscape: unstyled video element overflows viewport
    </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Utilisez les [requêtes média CSS](../../layouts/rwd-fundamentals/#use-css-media-queries-for-responsiveness) pour spécifier la taille d'éléments en fonction des dimensions de la fenêtre d'affichage. Dans ce cas, nous vous conseillons d'utiliser `max-width: 100%`.

Pour le contenu multimédia intégré dans des cadres iFrame, comme les vidéos YouTube, tentez d'avoir une approche de type adaptatif. Consultez celle qui est [proposée par John Surdakowski](//avexdesigns.com/responsive-youtube-embed/).

Note: Ne forcez pas la taille d'un élément générant un format d'image différent de celui de la vidéo d'origine. Les vidéos écrasées ou étirées donnent une mauvaise image du site.

Caution: Don't force element sizing that results in an aspect ratio different from the original video. Squashed or stretched looks bad.

**HTML:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="styling" adjust_indentation="auto" %}
</pre>

**HTML:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="styling"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/responsive_embed.html)

L'affichage des vidéos varie d'une plate-forme à l'autre. Les solutions pour mobile doivent donc tenir compte de l'orientation de l'appareil. Utilisez l'API Fullscreen afin de contrôler l'affichage en plein écran du contenu vidéo.

## Offrir des solutions pour les anciennes plates-formes

L'orientation de l'appareil ne pose pas de problème en ce qui concerne les ordinateurs de bureau et les portables. En revanche, elle revêt une importance capitale lorsqu'il s'agit de concevoir des pages Web pour les téléphones mobiles et les tablettes.

### JavaScript

Safari, sur l'iPhone, passe parfaitement bien du mode portrait au mode paysage :

Safari on iPhone does a good job of switching between portrait and landscape orientation:

<div class="attempt-left">
  <figure>
    <img  alt="Screenshot of video playing in Safari on iPhone, portrait"
    src="images/iPhone-video-playing-portrait.png">
    <figcaption>Screenshot of video playing in Safari on iPhone, portrait</figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Screenshot of video playing in Safari on iPhone, landscape"
    src="images/iPhone-video-playing-landscape.png">
    <figcaption>Screenshot of video playing in Safari on iPhone, landscape</figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

L'orientation de l'appareil peut néanmoins être problématique sur un iPad et sur Chrome pour Android. Par exemple, en l'absence de personnalisation, une vidéo lue sur un iPad en mode paysage a l'apparence suivante :

<img alt="Capture d'écran d'une vidéo lue dans Safari sur un iPad Retina, en mode paysage"
src="images/iPad-Retina-landscape-video-playing.png" />

L'utilisation des paramètres de largeur "width: 100%" ou "max-width: 100%" avec CSS permet de résoudre de nombreux problèmes de présentation liés à l'orientation de l'appareil. Vous pouvez également recourir aux solutions d'affichage en mode plein écran.

## Vérifier le format utilisé

<img class="attempt-right" alt="Screenshot of video element on iPhone, portrait"
src="images/iPhone-video-with-poster.png" />

Different platforms display video differently. Safari on an iPhone displays a video element inline on a web page, but plays video back in fullscreen mode:

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Chrome on Android,
portrait" src="images/Chrome-Android-video-playing-portrait-3x5.png" />

On Android, users can request request fullscreen mode by clicking the fullscreen icon. But the default is to play video inline:

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Safari on iPad
Retina, landscape" src="images/iPad-Retina-landscape-video-playing.png" />

Safari on an iPad plays video inline:

<div style="clear:both;"></div>

### Control fullscreening of content

Pour les plates-formes qui ne forcent pas la lecture des vidéos en mode plein écran, l'API Fullscreen est \[largement compatible\] (//caniuse.com/fullscreen). Utilisez cette API pour contrôler l'affichage en mode plein écran du contenu ou de la page.

Pour afficher un élément en plein écran, par exemple une video:

    elem.requestFullScreen();
    

Pour afficher la totalité du document en plein écran :

    elem.requestFullScreen();
    

Vous pouvez également surveiller les modifications d'état du mode plein écran :

    document.body.requestFullScreen();
    

Ou encore, vous pouvez vérifier si l'élément est actuellement en mode plein écran :

    video.addEventListener("fullscreenchange", handler);
    

Vous pouvez enfin utiliser la pseudo-classe CSS ":fullscreen" pour modifier l'affichage des éléments en mode plein écran.

<video autoplay muted loop class="attempt-right">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.mp4" type="video/mp4">
  <p>Ce navigateur n'est pas compatible avec l'élément vidéo.</p>
</video>

Sur les appareils compatibles avec l'API Fullscreen, il peut être judicieux d'utiliser une vignette en tant qu'espace réservé pour le contenu vidéo :

Pour voir comment cette vidéo s'affiche en plein écran, regardez la [démonstration](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/fullscreen.html).

NOTE: `requestFullScreen()` is currently vendor prefixed and may require extra code for full cross browser compatibility.

<div style="clear:both;"></div>

## Définir correctement la taille des vidéos

L'accessibilité n'est pas une fonctionnalité. Les utilisateurs malentendants ou malvoyants ne peuvent pas du tout bénéficier d'une vidéo si celle-ci ne contient pas de sous-titres ni de descriptions. Le temps que vous allez passer à ajouter ces éléments à la vidéo vous coûtera bien moins que de faire subir une expérience désagréable aux internautes. Faites en sorte de fournir au minimum une expérience de base accessible à tous.

### Include captions to improve accessibility

<img class="attempt-right" alt="Screenshot showing captions displayed using the
track element in Chrome on Android" src="images/Chrome-Android-track-landscape-5x3.jpg" />

Note: L'élément de suivi est compatible avec Chrome pour Android, Safari pour iOS, ainsi que tous les navigateurs actuels pour ordinateur de bureau, sauf Firefox (voir [caniuse.com/track](http://caniuse.com/track "État de compatibilité d&apos;un élément de suivi")). Plusieurs polyfills sont également disponibles. Nous vous recommandons d'utiliser l'[Playr](//www.delphiki.com/html5/playr/ "élément de suivi polyfill Playr") ou le[Captionator](//captionatorjs.com/ "suivi Captionator").

<div style="clear:both;"></div>

### Add track element

Lorsque vous utilisez l'élément de suivi, les sous-titres ont l'apparence suivante :

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/track.html" region_tag="track" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/track.html)

Il est très facile d'ajouter des sous-titres à une vidéo. Il vous suffit d'ajouter un élément de suivi en tant qu'élément enfant de l'élément vidéo :

### Define captions in track file

L'attribut d'élément de suivi `src` indique l'emplacement du fichier de suivi.

    WEBVTT
    
    00:00.000 --> 00:04.000
    Man sitting on a tree branch, using a laptop.
    
    00:05.000 --> 00:08.000
    The branch breaks, and he starts to fall.
    
    ...
    

## Personnaliser le lecteur de vidéos

### Video element attributes

Un fichier de suivi est composé de `signaux` chronométrés au format WebVTT :

<table>
  <thead>
    <tr>
      <th>Attribute</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Attribute"><code>src</code></td>
      <td data-th="Description">Tous les navigateurs</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>poster</code></td>
      <td data-th="Description">Tous les navigateurs</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>preload</code></td>
      <td data-th="Description">Tous les navigateurs mobiles ignorent le préchargement. </td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>autoplay</code></td>
      <td data-th="Description">Non compatible avec iPhone ou Android. Compatible avec tous les navigateurs pour ordinateurs, iPad, Firefox et Opera pour Android.</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>loop</code></td>
      <td data-th="Description">Tous les navigateurs</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>controls</code></td>
      <td data-th="Description">Tous les navigateurs</td>
    </tr>
  </tbody>
</table>

### Autoplay {: #autoplay }

Brève présentation des propriétés de l'élément vidéo

Pour obtenir la liste complète des attributs d'élément vidéo et de leurs définitions, consultez la page relative à la [norme de l'élément vidéo](//www.w3.org/TR/html5/embedded-content-0.html#the-video-element).

* Data usage can be expensive.
* Causing media to download and playback to begin without asking first, can unexpectedly hog bandwidth and CPU, and thereby delay page rendering.
* Users may be in a context where playing video or audio is intrusive.

Sur un ordinateur de bureau, l'attribut `autoplay` indique au navigateur de démarrer immédiatement le téléchargement et la lecture de la vidéo, dès que cela est possible. Sur iOS et Chrome pour Android, l'attribut `autoplay` ne fonctionne pas. L'internaute doit appuyer sur l'écran pour lire la vidéo.

### Preload {: #preload }

Même sur les plates-formes sur lesquelles il est possible d'intégrer l'attribut `autoplay`, vous devez tenir compte des facteurs suivants avant de l'activer :

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Valeur</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Value"><code>none</code></td>
      <td data-th="Description">L'internaute peut ne pas regarder la vidéo du tout. Aucun préchargement n'est lancé</td>
    </tr>
    <tr>
      <td data-th="Value"><code>metadata</code></td>
      <td data-th="Description">Les métadonnées (durée, dimensions, pistes de texte) doivent être préchargées, mais avec un contenu vidéo minimal.</td>
    </tr>
    <tr>
      <td data-th="Value"><code>auto</code></td>
      <td data-th="Description">Le téléchargement immédiat de l'intégralité de la vidéo est le comportement souhaité.</td>
    </tr>
  </tbody>
</table>

Le comportement de la lecture automatique peut être configuré dans la section AndroidWebView de l'[API WebSettings](//developer.android.com/reference/android/webkit/WebSettings.html#setMediaPlaybackRequiresUserGesture(boolean)). Par défaut, ce comportement est défini sur la valeur `true`, mais il peut être désactivé par une application WebView.

### JavaScript

L'attribut `preload` indique au navigateur la quantité d'informations ou de contenu à précharger.

#### Lecture automatique

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Property &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Property"><code>currentTime</code></td>
      <td data-th="Description">Fournit ou définit la position de lecture en secondes.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>volume</code></td>
      <td data-th="Description">Fournit ou définit le niveau de volume actuel de lecture de la vidéo.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>muted</code></td>
      <td data-th="Description">Fournit ou définit la désactivation du son.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>playbackRate</code></td>
      <td data-th="Description">Fournit ou définit la vitesse de lecture. La valeur 1 correspond à une lecture à vitesse normale.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>buffered</code></td>
      <td data-th="Description">Informations sur la quantité du contenu vidéo mis en mémoire tampon et prêt pour la lecture (regardez la <a href="http://people.mozilla.org/~cpearce/buffered-demo.html" title="Demo displaying amount of buffered video in a canvas element">démonstration</a>).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>currentSrc</code></td>
      <td data-th="Description">L'adresse de la vidéo en cours de lecture.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoWidth</code></td>
      <td data-th="Description">Largeur de la vidéo en pixels (qui peut être différente de la largeur de l'élément vidéo).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoHeight</code></td>
      <td data-th="Description">Hauteur de la vidéo en pixels (qui peut être différente de la hauteur de l'élément vidéo).</td>
    </tr>
  </tbody>
</table>

#### Préchargement

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Method &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Method"><code>load()</code></td>
      <td data-th="Description">Charge ou recharge une source vidéo sans lancer la lecture : par exemple, lorsque que l'attribut `src` est modifié à l'aide de JavaScript.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>play()</code></td>
      <td data-th="Description">Lit la vidéo à partir de son emplacement actuel.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>pause()</code></td>
      <td data-th="Description">Met en pause la vidéo à son emplacement actuel.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>canPlayType('format')</code></td>
      <td data-th="Description">Détecte les formats compatibles (consultez l'article 'Vérifier les formats compatibles').</td>
    </tr>
  </tbody>
</table>

L'attribut "preload" entraîne des effets différents selon les plates-formes. Par exemple, le navigateur Chrome met en mémoire tampon 25 secondes de contenu vidéo sur un ordinateur de bureau, et rien sur iOS ou Android. Cela peut entraîner des retards au démarrage de la lecture sur les appareils mobiles, qui n'existent pas sur les ordinateurs de bureau. Consultez la \[page de test de Steve Souder\] (//stevesouders.com/tests/mediaevents.php) pour obtenir davantage d'informations.

#### Propriétés

L'article relatif à la [vidéo du site HTML5 Rocks](//www.html5rocks.com/en/tutorials/video/basics/#toc-javascript) résume parfaitement les propriétés, les méthodes et les événements JavaScript qui peuvent être utilisés pour contrôler la lecture de contenu vidéo. Nous avons intégré le contenu de cet article dans cette section et avons mis à jour les contenus spécifiques aux appareils mobiles, le cas échéant.

<table class="responsive">
  <thead>
  <tr>
    <th colspan="2">Event &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Event"><code>canplaythrough</code></td>
      <td data-th="Description">Est envoyé lorsque suffisamment de données sont disponibles pour que le navigateur détecte que la vidéo peut être lue en entier, sans interruption.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>ended</code></td>
      <td data-th="Description">Est envoyé lorsque la lecture de la vidéo est terminée.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>error</code></td>
      <td data-th="Description">Est envoyé lorsqu'une erreur se produit.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>playing</code></td>
      <td data-th="Description">Est envoyé lorsque la lecture de la vidéo démarre pour la première fois, après avoir été mise en pause ou lors d'un redémarrage.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>progress</code></td>
      <td data-th="Description">Est envoyé périodiquement pour indiquer la progression du téléchargement.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>waiting</code></td>
      <td data-th="Description">Est envoyé lorsqu'une action est retardée dans l'attente de la fin d'une autre action.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>loadedmetadata</code></td>
      <td data-th="Description">Est envoyé lorsque le navigateur a terminé de charger les métadonnées de la vidéo : durée, dimensions et pistes de texte.</td>
    </tr>
  </tbody>
</table>

## Affichage intégré ou en mode plein écran

Les propriétés "playbackRate" ([voir la démonstration](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/scripted.html)) et `volume` ne sont pas compatibles avec les plates-formes mobiles.