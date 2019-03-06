project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: La typographie est essentielle pour la qualité du design, de la marque, de la lisibilité et de l'accessibilité. Les polices Web offrent tout ceci et plus encore : elles permettent de sélectionner le texte, de le rechercher, de l'agrandir, et de le rendre adapté aux appareils avec un ppp élevé, offrant un affichage du texte cohérent et net quelles que soient la taille de l'écran et la résolution.

{# wf_updated_on: 2014-09-29 #} {# wf_published_on: 2014-09-19 #}

# Optimisation des polices Web {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*This article contains contributions from [Monica Dinculescu](https://meowni.ca/posts/web-fonts/), [Rob Dodson](/web/updates/2016/02/font-display), and Jeff Posnick.*

L'optimisation de police Web est un élément essentiel de la stratégie de performance globale. Chaque police est une ressource supplémentaire, et certaines polices peuvent empêcher l'affichage du texte. Mais le fait que la page utilise des polices Web ne signifie pas que l'affichage doit être plus lent. Au contraire, une police optimisée associée à une stratégie judicieuse pour son chargement et son application sur la page peuvent contribuer à réduire la taille totale de la page et améliorer le délai d'affichage de la page.

Une police Web est une collection de glyphes, et chaque glyphe est une forme vectorielle qui représente une lettre ou un symbole. Par conséquent, la taille d'un fichier de police spécifique est déterminée par deux variables simples : la complexité des chemins vectoriels de chaque glyphe et le nombre de glyphes dans une police spécifique. Par exemple, Open Sans, qui est l'une des polices Web les plus populaires, contient 897 glyphes, comprenant des caractères latins, grecs et cyrilliques.

## Anatomie d'une police Web

### TL;DR {: .hide-from-toc }

* Les polices Unicode peuvent contenir des milliers de glyphes
* Il existe quatre formats de police : WOFF2, WOFF, EOT et TTF
* Certains formats de police nécessitent l'utilisation de la compression GZIP

A *webfont* is a collection of glyphs, and each glyph is a vector shape that describes a letter or symbol. As a result, two simple variables determine the size of a particular font file: the complexity of the vector paths of each glyph and the number of glyphs in a particular font. For example, Open Sans, which is one of the most popular webfonts, contains 897 glyphs, which include Latin, Greek, and Cyrillic characters.

<img src="images/glyphs.png"  alt="Font glyph table" />

De façon évidente, il convient de procéder avec soin lors de l'utilisation des polices sur le Web, afin de vous assurer que la typographie n'entrave pas les performances. Heureusement, la plateforme d'Internet fournit toutes les primitives nécessaires, et le reste de ce guide est consacré à une analyse concrète de la façon de profiter du meilleure des deux mondes.

Il existe aujourd'hui quatre formats de conteneur de police utilisés sur Internet : [EOT](http://en.wikipedia.org/wiki/Embedded_OpenType){: .external }, [TTF](http://fr.wikipedia.org/wiki/TrueType){: .external }, [WOFF](http://fr.wikipedia.org/wiki/Web_Open_Font_Format){: .external } et [WOFF2](http://www.w3.org/TR/WOFF2/){: .external }. Malheureusement, malgré ce vaste choix, il n'existe pas un seul format universel qui fonctionne sur tous les navigateurs, anciens et nouveaux : EOT fonctionne sur [IE uniquement](http://caniuse.com/#feat=eot){: .external }, TTF est [partiellement compatible avec IE](http://caniuse.com/#search=ttf){: .external }, WOFF est le plus compatible, mais il [n'est pas disponible sur certains anciens navigateurs](http://caniuse.com/#feat=woff){: .external } et la compatibilité de WOFF 2.0 est [en cours de développement pour de nombreux navigateurs](http://caniuse.com/#feat=woff2){: .external }.

### Formats de police Web

Alors que nous reste-t-il ? Il n'existe par un format unique qui fonctionne sur tous les navigateurs, ce qui signifie que nous devons fournir plusieurs formats pour offrir une expérience cohérente :

Note: Techniquement, il existe également le [conteneur de police SVG](http://caniuse.com/svg-fonts), mais il n'a jamais été compatible avec IE ni Firefox, et est maintenant obsolète dans Chrome. Son utilité est donc limitée, et nous l'omettons volontairement dans ce guide.

* Diffuser une variante WOFF 2.0 pour les navigateurs compatibles.
* Diffuser une variante WOFF pour la majorité des navigateurs.
* Diffuser une variante TTF pour les anciens navigateurs Android (avant 4.4).
* Diffuser une variante EOT pour les anciens navigateurs IE (avant IE9).

Une police est une collection de glyphes, et chaque glyphe est un ensemble de chemins décrivant la forme de la lettre. Chaque glyphe est différent, bien sûr. Mais ils contiennent cependant beaucoup d'informations similaires qui peuvent être compressées avec GZIP ou un logiciel de compression compatible :

### Réduire la taille de police avec la compression

Enfin il est important de noter que certains formats contiennent des métadonnées supplémentaires, telles que des informations d'[optimisation de rendu](http://fr.wikipedia.org/wiki/Font_hinting){: .external } et de [crénage](http://fr.wikipedia.org/wiki/Crénage){: .external } qui peuvent être inutiles sur certaines plateformes, ce qui permet d'optimiser encore davantage la taille du fichier. Consultez votre logiciel de compression des polices pour connaître les options d'optimisation disponibles, et si vous choisissez cette voie, assurez-vous que vous disposez d'une infrastructure adaptée pour tester et proposer ces polices optimisées à chaque navigateur. Par exemple, Google Fonts possède plus de 30 variantes optimisées pour chaque police, et détecte et fournit automatiquement la variante optimale pour chaque plateforme et navigateur.

* Les formats EOT et TTF ne sont pas compressés par défaut : assurez-vous que vos serveurs sont configurés pour appliquer la [compression GZIP](/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text-compression-with-gzip) lorsque vous utilisez ces formats.
* Le format WOFF dispose d'une compression intégrée. Assurez-vous que votre logiciel de compression WOFF utilise des paramètres de compression optimaux.
* Le format WOFF2 utilise un prétraitement et des algorithmes de compression personnalisés pour offrir une réduction de la taille de fichier supérieure de ~30 % par rapport aux autres formats. Consultez le [rapport](http://www.w3.org/TR/WOFF20ER/){: .external }.

Note: Pensez à utiliser la [compression Zopfli](http://en.wikipedia.org/wiki/Zopfli) pour les formats EOT, TTF et WOFF. Zopfli est un logiciel de compression compatible avec les données zlib, qui propose une réduction de la taille des fichiers supplémentaire d'environ 5 % par rapport à gzip.

La règle `at-rule` CSS @font-face nous permet de définir l'emplacement d'une ressource de police spécifique, ses caractéristiques stylistiques et les points de code Unicode pour lesquels elle doit être utilisée. Vous pouvez utiliser une combinaison de ces déclarations @font-face pour construire une 'famille de polices', que le navigateur utilise pour évaluer quelles ressources de police doivent être téléchargées et appliquées à la page actuelle. Regardons maintenant plus en détail comment cela fonctionne.

## Définir une famille de polices avec @font-face

### TL;DR {: .hide-from-toc }

* Utilisez l'algorithme d'optimisation format() pour spécifier plusieurs formats de police
* Définissez des sous-ensembles pour les polices Unicode volumineuses pour améliorer les performances: utilisez les sous-paramètres unicode-range et fournissez un sous-paramètre de rechange manuel pour les navigateurs plus anciens
* Réduisez le nombre de variantes de police stylistique afin d'améliorer les performances d'affichage de la page et du texte

Chaque déclaration @font-face fournit le nom de la famille de polices, qui joue le rôle de groupe logique de plusieurs déclarations, des [propriétés de police](http://www.w3.org/TR/css3-fonts/#font-prop-desc){: .external} telles que le style, le poids et l'étendue, et le [descripteur src](http://www.w3.org/TR/css3-fonts/#src-desc){: .external} qui spécifie une liste d'emplacements classés par priorité pour la ressource de la police.

### Sélection de format

Tout d'abord, notez que les exemples ci-dessus définissent une seule famille *Awesome Font* avec deux styles (normal et *italic*), chacun indiquant un ensemble de ressources de police différent. Ensuite, chaque descripteur `src` contient une liste de variantes de ressources, classées par priorité et séparées par une virgule :

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome.woff2') format('woff2'), 
           url('/fonts/awesome.woff') format('woff'),
           url('/fonts/awesome.ttf') format('ttf'),
           url('/fonts/awesome.eot') format('eot');
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: italic;
      font-weight: 400;
      src: local('Awesome Font Italic'),
           url('/fonts/awesome-i.woff2') format('woff2'), 
           url('/fonts/awesome-i.woff') format('woff'),
           url('/fonts/awesome-i.ttf') format('ttf'),
           url('/fonts/awesome-i.eot') format('eot');
    }
    

Note: À moins que vous référenciez l'une des polices système par défaut, en pratique il est rare que l'utilisateur l'ait installé localement, en particulier sur les appareils mobiles, où il est concrètement impossible 'd'installer' des polices supplémentaires. En conséquence, vous devez toujours fournir une liste d'emplacements pour les polices externes.

* La directive `local()` nous permet de référencer, charger et utiliser les polices installées localement.
* La directive `url()` nous permet de charger des polices externes, et peuvent contenir un algorithme d'optimisation `format()` facultatif indiquant le format de la police référencée par l'URL fournie.

Lorsque le navigateur détermine que la police est nécessaire, il consulte la liste de ressources fournir dans l'ordre indiqué et tente de charger la ressource adaptée. Considérons l'exemple ci-dessus :

La combinaison de directives locales et externes avec les algorithmes d'optimisation de format adaptés nous permet de spécifier tous les formats de police disponibles, et laisse le navigateur gérer le reste : le navigateur définit quelles ressources sont nécessaires et sélectionne pour nous le format le mieux adapté.

1. Le navigateur effectue la mise en page de la page et détermine quelles variantes de la police sont requises pour afficher le texte spécifié sur la page.
2. Pour chaque police requise, le navigateur vérifie si elle est disponible localement.
3. Si le fichier n'est pas disponible localement, il consulte le définitions externes l'une après l'autre : 
    * Si un algorithme d'optimisation de format est présent, le navigateur vérifie s'il est compatible avant de lancer le téléchargement, et s'il ne l'est pas, passe au suivant.
    * Si aucun algorithme d'optimisation n'est présent, le navigateur télécharge la ressource.

Note: L'ordre dans lequel les variantes d'une police sont spécifiées est important. Le navigateur choisit le premier format compatible. Par conséquent, si vous souhaitez que les navigateurs les plus récents utilisent WOFF2, vous devez placer la déclaration WOFF2 au-dessus de WOFF, et ainsi de suite.

Outre les propriétés de police telles que le style, le poids et la portée, la règle @font-face nous permet de définir un ensemble de points de code Unicode compatible avec chaque ressource. Cela nous permet de partager une police Unicode volumineuse en plusieurs sous-ensembles (par exemple latin, cyrillique, grec) et de ne télécharger que les glyphes nécessaires pour afficher le texte sur une page spécifique.

### Créer des sous ensembles unicode-range

Le [descripteur unicode-range](http://www.w3.org/TR/css3-fonts/#descdef-unicode-range){: .external} nous permet de spécifier une liste de valeurs de plage séparées par une virgule, chacune pouvant prendre l'une des trois formes différentes suivantes :

Par exemple, nous pouvons diviser notre famille *Awesome Font* en sous ensembles Latin et Japonais, chacun étant téléchargé par le navigateur en fonction des besoins :

* Point de code unique (par exemple, U+416)
* Plage d'intervalle (par exemple, U+400-4ff) : indique les points de code de début et de fin d'une plage
* Plage de caractères de remplacement (par exemple, U+4??) : les caractères `?` indiquent un chiffre hexadécimal

Note: Les sous-ensembles unicode-range sont particulièrement importants pour les langues asiatiques, dans lesquelles le nombre de glyphes est beaucoup plus important que dans les langues occidentales, et pour lesquelles une police 'complète' est souvent mesurée en mégaoctets et non pas en dizaines de kilo-octets !

    @font-face {
      font-family: `Awesome Font`;
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: `Awesome Font`;
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-jp.woff2') format('woff2'), 
           url('/fonts/awesome-jp.woff') format('woff'),
           url('/fonts/awesome-jp.ttf') format('ttf'),
           url('/fonts/awesome-jp.eot') format('eot');
      unicode-range: U+3000-9FFF, U+ff??; /* Japanese glyphs */
    }
    

L'utilisation de sous-ensembles unicode-range et de fichiers distincts pour chaque variante stylistique de la police nous permet de définir une famille de polices composite, à la fois plus rapide et plus efficace à télécharger : le visiteur ne télécharge que les variantes et sous-ensembles dont il a besoin, et n'est pas obligé de télécharger des sous-ensembles qu'il ne verra ou n'utilisera peut-être jamais sur la page.

Cela dit, il y a un petit inconvénient avec unicode-range : [il n'est pas compatible avec tous les navigateurs](http://caniuse.com/#feat=font-unicode-range){: .external }, du moins pas encore. Certains navigateurs ignorent simplement l'algorithme d'optimisation unicode-range et téléchargent toutes les variantes, alors que d'autres peuvent ne pas traiter la déclaration @font-face du tout. Pour résoudre ce problème, nous devons revenir aux "sous-ensembles manuels" pour les anciens navigateurs.

Parce que les anciens navigateurs ne sont pas assez intelligents pour ne sélectionner que les sous-ensembles nécessaires et ne peuvent pas construire une police composite, nous devons revenir à l'ancienne méthode : fournir une seule ressource de police qui contient tous les sous-ensembles nécessaires, et masquer le reste pour le navigateur. Par exemple, si la page n'utilise que des caractères latins, nous pouvons supprimer les autres glyphes et diffuser ce sous-ensemble particulier comme ressource autonome.

Chaque famille de police est composée de plusieurs variantes stylistiques (normal, gras, italique) et de plusieurs poids pour chaque style, chacun pouvant à son tour contenir des formes de glyphes très différentes, telles qu'un espacement ou une taille différents, ou tout simplement une forme différente.

1. **Comment pouvons-nous déterminer quels sous-ensembles sont nécessaires ?** 
    * Si le navigateur est compatible avec le sous-ensemble unicode-range, le navigateur sélectionne automatiquement le bon sous-ensemble. La page doit simplement fournir les fichiers du sous-ensemble et spécifier les unicode-ranges adaptés dans les règles @font-face.
    * Si le navigateur n'est pas compatible avec unicode-range, la page doit masquer tous les sous-ensembles inutiles. C'est-à-dire que le développeur doit spécifier les sous-ensembles requis.
2. **Comment pouvons-nous générer des sous-ensembles de police ?** 
    * Utilisez l'[outil Open Source pyftsubset](https://github.com/behdad/fonttools/blob/master/Lib/fontTools/subset.py#L16){: .external} pour créer des sous-ensembles et optimiser vos polices.
    * Certains services des polices permettent de créer des sous-ensembles manuellement via des paramètres de requête personnalisés, que vous pouvez utiliser pour spécifier manuellement le sous-ensemble requis pour votre page. Consultez la documentation de votre fournisseur de police.

### Sélection de police et synthèse

Each font family is composed of multiple stylistic variants (regular, bold, italic) and multiple weights for each style, each of which, in turn, may contain very different glyph shapes&mdash;for example, different spacing, sizing, or a different shape altogether.

<img src="images/font-weights.png"  alt="Font weights" />

Une logique similaire s'applique aux variantes *italic*. Le concepteur de polices contrôle les variantes qu'il produit, et nous contrôlons les variantes que nous utilisons sur la page. Puisque chaque variante est téléchargée séparément, il est conseillé d'en utiliser le moins possible ! Par exemple, nous pouvons définit deux variantes de gras pour notre famille *Awesome Font* :

> Lorsqu'un poids est spécifié pour lequel il n'existe pas de face, une face avec un poids approchant est utilisée. En général, les poids gras sont mappés à des faces avec des poids plus importants, et les poids légers sont mappés à des faces avec des poids plus légers.
> 
> > [Algorithme de correspondance de police CSS3](http://www.w3.org/TR/css3-fonts/#font-matching-algorithm)

L'exemple ci-dessus déclare que la famille *Awesome Font* est composée de deux ressources qui couvrent le même ensemble de glyphes latins (U+000-5FF), mais offrent des 'poids' différents : normal (400) et gras (700). Cependant, que se passe-t-il si l'une de nos règles CSS spécifie un poids de police différent, ou définit la propriété du style de police comme italique ?

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 700;
      src: local('Awesome Font'),
           url('/fonts/awesome-l-700.woff2') format('woff2'), 
           url('/fonts/awesome-l-700.woff') format('woff'),
           url('/fonts/awesome-l-700.ttf') format('ttf'),
           url('/fonts/awesome-l-700.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    

The above example declares the *Awesome Font* family that is composed of two resources that cover the same set of Latin glyphs (`U+000-5FF`) but offer two different "weights": normal (400) and bold (700). However, what happens if one of your CSS rules specifies a different font weight, or sets the font-style property to italic?

* Si aucune correspondance exacte n'est disponible pour la police, le navigateur la remplace par la correspondance la plus proche.
* Si aucune correspondance n'est trouvée pour le style (si nous n'avons pas déclaré de variantes en italique dans l'exemple ci-dessus, par exemple), le navigateur synthétise sa propre variante de police.

<img src="images/font-synthesis.png"  alt="Font synthesis" />

Note: Pour une meilleure cohérence et de meilleurs résultats visuels, ne vous fiez pas à la synthèse des polices. Réduisez le nombre de variantes de police utilisé et spécifiez leur emplacement, afin que le navigateur puisse les télécharger lorsqu'elles sont utilisées sur la page. Cela étant dit, dans certain cas une variante synthétisée [peut être une bonne option](https://www.igvita.com/2014/09/16/optimizing-webfont-selection-and-synthesis/). Mais utilisez-la avec précaution.

Une police Web 'complète' avec toutes les variantes stylistiques, dont nous pouvons ne pas avoir besoin, plus tous les glyphes, qui peuvent être inutiles, peut facilement générer un téléchargement de plusieurs mégaoctets. Pour résoudre ce problème, la règle CSS @font-face est conçue spécialement pour nous permettre de diviser la famille de polices en une collection de ressources : sous-ensembles Unicode, variantes stylistiques distinctes, etc.

Avec ces déclarations, le navigateur détermine les sous-ensembles et variantes nécessaires, et télécharge l'ensemble minimal requis pour afficher le texte. Ce comportement a de nombreux avantages, mais si nous ne faisons pas attention, il peut également créer un goulot d'étranglement en termes de performances dans le chemin critique du rendu et retarder l'affichage du texte. C'est un problème qu'il vaut mieux éviter !

## Optimiser le chargement et l'affichage

### TL;DR {: .hide-from-toc }

* Les demandes de police sont retardées jusqu'à ce que l'arborescence d'affichage soit construite, ce qui a pour conséquence un retard de l'affichage du texte'
* L'API Font Loading nous permet de mettre en œuvre des stratégies de chargement et d'affichage des polices personnalisées qui remplacent le chargement de police inactif par défaut

Le chargement inactif de polices comporte une importante implication cachée qui peut retarder l'affichage du texte : le navigateur doit [construire l'arborescence d'affichage](/web/fundamentals/performance/critical-rendering-path/render-tree-construction), qui dépend des arborescences des modèles DOM et CSSOM, avant de savoir quelles ressources de police sont nécessaires pour afficher le texte. Résultat, les requêtes de police sont retardées beaucoup plus longtemps que les autres ressources critiques, et le navigateur peut être empêché d'afficher le texte jusqu'à ce que la ressource soit récupérée.

Given these declarations, the browser figures out the required subsets and variants and downloads the minimal set required to render the text, which is very convenient. However, if you're not careful, it can also create a performance bottleneck in the critical rendering path and delay text rendering.

### Polices Web et le chemin critique du rendu

la 'course' entre la première peinture du contenu de la page, qui peut être effectuée peu de temps après la création de l'arborescence d'affichage, et la requête de la ressource de police est ce qui crée le 'problème de texte manquant', lorsque le navigateur affiche la page sans le texte. Le comportement réel dans cette situation varie en fonction du navigateur :

<img src="images/font-crp.png"  alt="Font critical rendering path" />

1. Le navigateur demande le document HTML.
2. Le navigateur commence à analyser la réponse HTML et à construire le modèle DOM.
3. Le navigateur découvre le code CSS, JS et autres ressources, et transmet les requêtes.
4. Le navigateur construit le modèle CSSOM une fois que tout le contenu du code CSS est reçu, et le combine avec l'arborescence du modèle DOM pour construire l'arborescence d'affichage. 
    * Les demandes de police sont envoyées une fois que l'arborescence d'affichage indique quelles variantes de la police sont nécessaires pour afficher le texte spécifié sur la page.
5. Le navigateur effectue la mise en page et peint le contenu sur l'écran. 
    * Si la police n'est pas encore disponible, le navigateur ne peut pas afficher les pixels de texte.
    * Une fois que la police est disponible, le navigateur peint les pixels de texte.

L'[API Font Loading](http://dev.w3.org/csswg/css-font-loading/){: .external } fournit une interface d'écriture de script qui permet de définir et de manipuler les faces des polices CSS, de suivre la progression de leur téléchargement et de contourner leur comportement de chargement inactif par défaut. Par exemple, si nous sommes certains qu'une variante de police spécifique sera nécessaire, nous pouvons la définir et demander au navigateur de lancer une récupération immédiate de la ressource de police :

De plus, comme nous pouvons contrôler la méthode du statut de la police (via la directive [check()](http://dev.w3.org/csswg/css-font-loading/#font-face-set-check){: .external }) et suivre la progression de son téléchargement, nous pouvons également définir une stratégie personnalisée pour afficher le texte sur nos pages :

### Optimiser l'affichage de la police avec l'API Font Loading

Encore mieux, nous pouvons aussi mélanger toutes les stratégies ci-dessus pour les différents contenus sur la page. Nous pouvons différer l'affichage du texte pour certaines sections jusqu'à ce que la police soit disponible, utiliser une police de rechange, puis afficher à nouveau le texte lorsque la police est téléchargée, définir des délais d'expiration différents, etc.

Note: L'API Font Loading est toujours [en cours de développement dans certains navigateurs](http://caniuse.com/#feat=font-loading). Pensez donc à utiliser l'[émulateur de navigateur Web FontLoader](https://github.com/bramstein/fontloader), ou la [bibliothèque webfontloader](https://github.com/typekit/webfontloader), afin de proposer des fonctionnalités similaires, que ce soit avec un temps système ou une dépendance JavaScript supplémentaires.

Une stratégie alternative simple à l'utilisation de l'API Font Loading pour éliminer le 'problème de texte manquant' consiste à intégrer le contenu des polices dans une feuille de style CSS :

```html
var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
  style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
});

font.load(); // don't wait for render tree, initiate immediate fetch!

font.ready().then(function() {
  // apply the font (which may rerender text and cause a page reflow)
  // once the font has finished downloading
  document.fonts.add(font);
  document.body.style.fontFamily = "Awesome Font, serif";

  // OR... by default content is hidden, and rendered once font is available
  var content = document.getElementById("content");
  content.style.visibility = "visible";

  // OR... apply own render strategy here... 
});
```

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

La stratégie d'intégration n'est pas aussi flexible, et ne nous permet pas de définir des délais d'expiration ou des stratégies d'affichage personnalisés pour les différents contenus, mais c'est une solution simple et fiable qui fonctionne sur tous les navigateurs. Pour obtenir les meilleurs résultats, séparez les polices intégrées en plusieurs feuilles de styles indépendantes et diffusez-les avec une directive `max-age` longue. Ainsi, lorsque vous mettez à jour votre code CSS, vous ne forcez pas vos visiteurs à télécharger à nouveau les polices.

Note: Utilisez l'intégration de façon sélective ! Souvenez-vous que @font-face utilise le comportement de chargement inactif pour éviter de télécharger des variantes et sous-ensembles de police inutiles. De plus, l'augmentation de la taille de votre CSS par une intégration agressive aura un impact négatif sur votre [chemin critique du rendu](/web/fundamentals/performance/critical-rendering-path/) : le navigateur doit télécharger tout le code CSS avant de pouvoir construire le modèle CSSOM, créer l'arborescence d'affichage et afficher le contenu de la page d'affichage à l'écran.

### Optimiser l'affichage des polices avec l'intégration

Les ressources de polices sont généralement des ressources statiques qui ne sont pas mises à jour fréquemment. Elles sont donc idéalement adaptées à une expiration `max-age` longue. Assurez-vous de spécifier à la fois un [en-tête ETag conditionnel](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags){: .external } et des [règles Cache-Control optimales](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control){: .external } pour toutes les ressources de polices.

#### Browser behaviors

Il est inutile de stocker les polices sur un dispositif de stockage local ou via d'autres mécanismes, puisque chacune a ses inconvénients en termes de performances. Le cache HTTP du navigateur, associée à l'API Font Loading ou à la bibliothèque webfontloader, offre le meilleur et le plus fiable mécanisme pour fournir les ressources de police au navigateur.

<table>
  <thead>
    <tr>
      <th data-th="Browser">Browser</th>
      <th data-th="Timeout">Timeout</th>
      <th data-th="Fallback">Fallback</th>
      <th data-th="Swap">Swap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Browser">
        <strong>Chrome 35+</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Opera</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Firefox</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Internet Explorer</strong>
      </td>
      <td data-th="Timeout">
        0 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Safari</strong>
      </td>
      <td data-th="Timeout">
        No timeout
      </td>
      <td data-th="Fallback">
        N/A
      </td>
      <td data-th="Swap">
        N/A
      </td>
    </tr>
  </tbody>
</table>

* Safari diffère l'affichage du texte jusqu'à la fin du téléchargement de la police.
* Chrome et Firefox diffèrent l'affichage de la police jusqu'à trois secondes, puis ils utilisent une police de rechange, et une fois que le téléchargement de la police est terminé, ils affichent à nouveau le texte avec la police téléchargée.
* IE affiche immédiatement le texte avec la police de rechange si la police demandée n'est pas encore disponible, et l'affiche à nouveau une fois le téléchargement de la police terminé.

Contrairement à une croyance répandue, l'utilisation de polices Web ne retarde pas nécessairement l'affichage de la page et n'a pas nécessairement un impact négatif sur les autres statistiques de performances. Une utilisation bien optimisée des polices peut offrir une expérience utilisateur largement améliorée : bonne présentation de la marque, lisibilité, facilité d'utilisation et recherche facilitées, tout en offrant une solution en plusieurs résolutions et à l'échelle qui s'adapte bien à tous les formats d'écran et toutes les résolutions. N'ayez pas peur d'utiliser les polices Web !

#### The font display timeline

Ceci dit, une mise en œuvre non informée peut provoquer des téléchargements importantes et des retards inutiles. C'est pour cela que nous devons ressortir notre kit d'optimisation et aider le navigateur en optimisant les éléments de police eux-mêmes, ainsi que la façon dont ils sont récupérés et utilisés sur nos pages.

1. **Auditez et surveillez votre utilisation des polices** : n'utilisez pas un trop grand nombre de polices sur vos pages, et pour chaque police, réduisez au minimum le nombre de variantes utilisées. Cela permettra d'offrir une expérience plus cohérente et plus rapide à vos utilisateurs.
2. **Organisez vos ressources de polices en sous-ensembles** : de nombreuses polices peuvent être organisées en sous-ensembles, ou divisées en plusieurs unicode-ranges, afin de ne fournir que les glyphes nécessaires à une page spécifique. Cela permet de réduire la taille des fichiers et d'améliorer la vitesse de téléchargement de la ressource. Cependant, lorsque vous définissez des sous-ensembles, pensez à les optimiser pour la réutilisation des polices, pour éviter par exemple de télécharger des ensembles de caractères différents, mais se chevauchant sur chaque page. Une méthode efficace consiste à créer des sous-ensembles en fonction du script : latin, cyrillique, etc.
3. **Fournissez des formats de police optimisés à chaque navigateur** : chaque police doit être fournie dans les formats WOFF2, WOFF, EOT et TTF. Assurez-vous d'appliquer la compression GZIP aux formats EOT et TTF, puisqu'ils ne sont pas compressés par défaut.

Understanding these periods means you can use `font-display` to decide how your font should render depending on whether or when it was downloaded.

#### Using font-display

To work with the `font-display` property, add it your `@font-face` rules:

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  font-display: auto; /* or block, swap, fallback, optional */
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

`font-display` currently supports the following range of values: `auto | block | swap | fallback | optional`.

* **`auto`** uses whatever font display strategy the user-agent uses. Most browsers currently have a default strategy similar to `block`.

* **`block`** gives the font face a short block period (3s is recommended in most cases) and an infinite swap period. In other words, the browser draws "invisible" text at first if the font is not loaded, but swaps the font face in as soon as it loads. To do this the browser creates an anonymous font face with metrics similar to the selected font but with all glyphs containing no "ink." This value should only be used if rendering text in a particular typeface is required for the page to be usable.

* **`swap`** gives the font face a zero second block period and an infinite swap period. This means the browser draws text immediately with a fallback if the font face isn’t loaded, but swaps the font face in as soon as it loads. Similar to `block`, this value should only be used when rendering text in a particular font is important for the page, but rendering in any font will still get a correct message across. Logo text is a good candidate for **swap** since displaying a company’s name using a reasonable fallback will get the message across but you’d eventually use the official typeface.

* **`fallback`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a short swap period (three seconds is recommended in most cases). In other words, the font face is rendered with a fallback at first if it’s not loaded, but the font is swapped as soon as it loads. However, if too much time passes, the fallback will be used for the rest of the page’s lifetime. `fallback` is a good candidate for things like body text where you’d like the user to start reading as soon as possible and don’t want to disturb their experience by shifting text around as a new font loads in.

* **`optional`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a zero second swap period. Similar to `fallback`, this is a good choice for when the downloading font is more of a "nice to have" but not critical to the experience. The `optional` value leaves it up to the browser to decide whether to initiate the font download, which it may choose not to do or it may do it as a low priority depending on what it thinks would be best for the user. This can be beneficial in situations where the user is on a weak connection and pulling down a font may not be the best use of resources.

`font-display` is [gaining adoption](https://caniuse.com/#feat=css-font-rendering-controls) in many modern browsers. You can look forward to consistency in browser behavior as it becomes widely implemented.

### Optimiser la réutilisation des polices avec la mise en cache HTTP

Used together, `<link rel="preload">` and the CSS `font-display` give developers a great deal of control over font loading and rendering, without adding in much overhead. But if you need additional customizations, and are willing to incur with the overhead introduced by running JavaScript, there is another option.

The [Font Loading API](https://www.w3.org/TR/css-font-loading/) provides a scripting interface to define and manipulate CSS font faces, track their download progress, and override their default lazyload behavior. For example, if you're sure that a particular font variant is required, you can define it and tell the browser to initiate an immediate fetch of the font resource:

    var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
      style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
    });
    
    // don't wait for the render tree, initiate an immediate fetch!
    font.load().then(function() {
      // apply the font (which may re-render text and cause a page reflow)
      // after the font has finished downloading
      document.fonts.add(font);
      document.body.style.fontFamily = "Awesome Font, serif";
    
      // OR... by default the content is hidden, 
      // and it's rendered after the font is available
      var content = document.getElementById("content");
      content.style.visibility = "visible";
    
      // OR... apply your own render strategy here... 
    });
    

Further, because you can check the font status (via the [check()](https://www.w3.org/TR/css-font-loading/#font-face-set-check)) method and track its download progress, you can also define a custom strategy for rendering text on your pages:

* Les feuilles de style CSS avec requêtes média correspondantes sont automatiquement téléchargées par le navigateur avec une priorité élevée, car elles ont nécessaires pour construire le modèle CSSOM.
* L'intégration des données des polices dans une feuille de style CSS force le navigateur à télécharger la police avec une priorité élevée et sans attendre l'arborescence d'affichage. Cela permet de contourner manuellement le comportement de chargement inactif par défaut.
* You can use the fallback font to unblock rendering and inject a new style that uses the desired font after the font is available.

Best of all, you can also mix and match the above strategies for different content on the page. For example, you can delay text rendering on some sections until the font is available, use a fallback font, and then re-render after the font download has finished, specify different timeouts, and so on.

Note: The Font Loading API is still [under development in some browsers](http://caniuse.com/#feat=font-loading). Consider using the [FontLoader polyfill](https://github.com/bramstein/fontloader) or the [webfontloader library](https://github.com/typekit/webfontloader) to deliver similar functionality, albeit with even more overhead from an additional JavaScript dependency.

### Proper caching is a must

Font resources are, typically, static resources that don't see frequent updates. As a result, they are ideally suited for a long max-age expiry - ensure that you specify both a [conditional ETag header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), and an [optimal Cache-Control policy](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) for all font resources.

If your web application uses a [service worker](/web/fundamentals/primers/service-workers/), serving font resources with a [cache-first strategy](/web/fundamentals/instant-and-offline/offline-cookbook/#cache-then-network) is appropriate for most use cases.

You should not store fonts using [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) or [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API); each of those has its own set of performance issues. The browser's HTTP cache provides the best and most robust mechanism to deliver font resources to the browser.

## Liste de contrôle de l'optimisation

Contrary to popular belief, the use of webfonts doesn't need to delay page rendering or have a negative impact on other performance metrics. The well-optimized use of fonts can deliver a much better overall user experience: great branding, improved readability, usability, and searchability, all while delivering a scalable multi-resolution solution that adapts well to all screen formats and resolutions. Don't be afraid to use webfonts.

That said, a naive implementation may incur large downloads and unnecessary delays. You need to help the browser by optimizing the font assets themselves and how they are fetched and used on your pages.

* **Audit and monitor your font use:** don't use too many fonts on your pages, and, for each font, minimize the number of used variants. This helps produce a more consistent and a faster experience for your users.
* **Subset your font resources:** many fonts can be subset, or split into multiple unicode-ranges to deliver just the glyphs that a particular page requires. This reduces the file size and improves the download speed of the resource. However, when defining the subsets, be careful to optimize for font re-use. For example, don't download a different but overlapping set of characters on each page. A good practice is to subset based on script: for example, Latin, Cyrillic, and so on.
* **Deliver optimized font formats to each browser:** provide each font in WOFF2, WOFF, EOT, and TTF formats. Make sure to apply GZIP compression to the EOT and TTF formats, because they are not compressed by default.
* **Give precedence to `local()` in your `src` list:** listing `local('Font Name')` first in your `src` list ensures that HTTP requests aren't made for fonts that are already installed.
* **Customize font loading and rendering using `<link rel="preload">`, `font-display`, or the Font Loading API:** default lazyloading behavior may result in delayed text rendering. These web platform features allow you to override this behavior for particular fonts, and to specify custom rendering and timeout strategies for different content on the page.
* **Specify revalidation and optimal caching policies:** fonts are static resources that are infrequently updated. Make sure that your servers provide a long-lived max-age timestamp and a revalidation token to allow for efficient font reuse between different pages. If using a service worker, a cache-first strategy is appropriate.

## Automated testing for web font optimization with Lighthouse {: #lighthouse }

[Lighthouse](/web/tools/lighthouse) can help automate the process of making sure that you're following web font optimization best practices. Lighthouse is an auditing tool built by the Chrome DevTools team. You can run it as a Node module, from the command line, or from the Audits panel of Chrome DevTools. You tell Lighthouse what URL to audit, and then it runs a bunch of tests on the page, and gives you a report of what the page is doing well, and how it can improve.

The following audits can help you make sure that your pages are continuing to follow web font optimization best practices over time:

* [Enable text compression](/web/tools/lighthouse/audits/text-compression)
* [Preload key requests](/web/tools/lighthouse/audits/preload)
* [Uses inefficient cache policy on static assets](/web/tools/lighthouse/audits/cache-policy)
* [All text remains visible during webfont loads](/web/updates/2016/02/font-display)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}