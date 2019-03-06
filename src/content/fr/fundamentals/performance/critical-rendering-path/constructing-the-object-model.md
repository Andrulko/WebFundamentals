project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Avant que le navigateur puisse afficher le contenu sur l'écran, les arborescences DOM et CSSOM doivent être créées. Nous devons donc nous assurer que le code HTML et CSS est transmis au navigateur le plus rapidement possible.

{# wf_updated_on: 2014-09-11 #} {# wf_published_on: 2014-03-31 #}

# Construire le modèle d'objet {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %} Avant que le navigateur puisse afficher la page, les arborescences DOM et CSSOM doivent être créées. Nous devons donc nous assurer que le code HTML et CSS est transmis au navigateur le plus rapidement possible.

Commençons par un exemple le plus simple possible : une page en HTML brut avec du texte et une seule image. Que doit faire le navigateur pour traiter cette page simple ?

### TL;DR {: .hide-from-toc }

- Octets → caractères → jetons → nœuds → modèle d'objet.
- Le balisage HTML est transformé en DOM (modèle d'objet de document) et le balisage CSS est transformé en CSSOM (modèle d'objet CSS).
- Les modèles DOM et CSSOM sont des structures de données indépendantes.
- Chrome DevTools Timeline nous permet de capturer et de contrôler les coûts de construction et de traitement des modèles DOM et CSSOM.

## Modèle d'objet de document (DOM)

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom.html){: target="_blank" .external }

Let’s start with the simplest possible case: a plain HTML page with some text and a single image. How does the browser process this page?

<img src="images/full-process.png" alt="DOM construction process" />

1. **Conversion** : le navigateur lit les octets bruts du HTML sur le disque ou le réseau, et les traduit en caractères individuels en fonction de l'encodage spécifique du fichier (UTF-8, par exemple).
2. **Création de jetons** : le navigateur convertit les chaînes de caractères en différents jetons spécifiés par la [norme HTML5 du W3C](http://www.w3.org/TR/html5/){: .external }, telles que `<html>`, `<body>` et d'autres chaînes entre chevrons. Chaque jeton possède une signification particulière et un ensemble de règles.
3. **Analyse lexicale** : les jetons émis sont convertis en 'objets' qui définissent leurs propriétés et leurs règles.
4. **Construction du DOM** : enfin, puisque le balisage HTML définit les relations entre les différentes balises (certaines balises sont contenues dans d'autres), les objets créés sont associés selon une arborescence, qui capture également la relation parent-enfant définie dans le balisage d'origine : l'objet *HTML* est un parent de l'objet *body*, *body* est un parent de l'objet *paragraph*, etc.

<img src="images/dom-tree.png"  alt="DOM tree" />

**The final output of this entire process is the Document Object Model (DOM) of our simple page, which the browser uses for all further processing of the page.**

Note: Nous supposons que vous connaissez les principes de base de Chrome DevTools, c'est-à-dire que vous savez comment capturer une suite de réseaux ou enregistrer une chronologie. Si vous avez besoin de vous rafraîchir la mémoire, consultez la [documentation Chrome DevTools](https://developer.chrome.com/devtools), ou si vous découvrez DevTools pour la première fois, nous vous conseillons de suivre le cours Codeschool [Découvrir DevTools](http://discover-devtools.codeschool.com/).

<img src="images/dom-timeline.png"  alt="Tracing DOM construction in DevTools" />

Une fois l'arborescence du DOM prête, disposons-nous de suffisamment d'informations pour afficher la page à l'écran ? Pas encore ! L'arborescence du DOM capture les propriétés et les relations du balisage du document, mais elle ne nous donne aucune information sur l'aspect que doit avoir l'élément lorsqu'il est affiché. C'est à cela que sert le CSSOM, dont nous allons parler maintenant !

Pendant la construction du DOM de notre page simple, le navigateur a rencontré une balise de lien dans la section `head` du document, faisant référence à une feuille de style CSS externe : style.css. Anticipant le fait que cette ressource sera nécessaire pour afficher la page, le navigateur envoie immédiatement une demande pour cette ressource, qui est renvoyée avec le contenu suivant :

Évidemment, nous aurions pu déclarer nos styles directement au sein du balisage HTML (intégré). Toutefois, le fait de séparer le CSS du code HTML nous permet de traiter le contenu et le graphisme comme deux choses distinctes : les graphistes peuvent travailler sur le CSS, les développeurs peuvent se concentrer sur le code HTML, et ainsi de suite.

## Modèle d'objet CSS (CSSOM)

Comme pour le code HTML, nous devons convertir les règles CSS reçues en un format que le navigateur peut comprendre et utiliser. C'est pourquoi nous répétons un processus très semblable à celui utilisé avec le code HTML :

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/style.css" region_tag="full"   adjust_indentation="auto" %}
</pre>

We could have declared our styles directly within the HTML markup (inline), but keeping our CSS independent of HTML allows us to treat content and design as separate concerns: designers can work on CSS, developers can focus on HTML, and so on.

Les octets CSS sont convertis en caractères, puis en jetons et en nœuds, et sont enfin associés selon une structure arborescente appelée 'Modèle d'objet CSS' ou CSSOM :

<img src="images/cssom-construction.png"  alt="Arborescence du CSSOM" />

Pourquoi le CSSOM possède-t-il une structure arborescente ? Lors du calcul de l'ensemble de styles final pour tout objet sur la page, le navigateur commence par la règle la plus générale applicable à ce nœud (par exemple, s'il s'agit d'un élément enfant du corps, tous les styles du corps s'appliquent). Il affine ensuite de façon récursive les styles calculés en appliquant des règles plus spécifiques, c'est-à-dire que les règles sont 'appliquées en cascade'.

<img src="images/cssom-tree.png"  alt="CSSOM tree" />

Notez également que l'arborescence ci-dessus n'est pas l'arborescence CSSOM complète et qu'elle ne montre que les styles que nous avons décidé de remplacer dans notre feuille de style. Chaque navigateur fournit un ensemble de styles par défaut également appelés 'styles user-agent'. C'est ce que nous voyons lorsque nous ne fournissons pas nos propres styles, et nos styles remplacent simplement ces styles par défaut (les [styles IE par défaut], par exemple(http://www.iecss.com/)). Vous savez maintenant d'où viennent tous les 'styles calculés' dans Chrome DevTools !

Vous vous demandez combien de temps a duré le traitement du CSS ? Enregistrez une chronologie dans DevTools et recherchez l'événement 'Recalculer le style' : contrairement à l'analyse du DOM, la chronologie n'affiche pas d'entrée 'Analyser le CSS' distincte, mais capture l'analyse et la construction de l'arborescence du CSSOM, ainsi que le calcul récursif des styles calculés sous cet événement précis.

Also, note that the above tree is not the complete CSSOM tree and only shows the styles we decided to override in our stylesheet. Every browser provides a default set of styles also known as "user agent styles"&mdash;that’s what we see when we don’t provide any of our own&mdash;and our styles simply override these defaults (for example, [default IE styles](http://www.iecss.com/){: .external }).

Le traitement de notre feuille de style simple prend environ 0,6 ms, et cette feuille de style affecte huit éléments sur la page. C'est peu, mais une fois encore, ce n'est pas gratuit. Cependant, d'où viennent ces huit éléments ? Les modèles CSSOM et DOM sont des structures de données indépendantes ! En fait, le navigateur masque une étape importante. Parlons ensuite de l'arborescence d'affichage qui lie les modèles DOM et CSSOM entre eux.

<img src="images/cssom-timeline.png"  alt="Tracing CSSOM construction in DevTools" />

Our trivial stylesheet takes ~0.6ms to process and affects eight elements on the page&mdash;not much, but once again, not free. However, where did the eight elements come from? The CSSOM and DOM are independent data structures! Turns out, the browser is hiding an important step. Next, lets talk about the [render tree](/web/fundamentals/performance/critical-rendering-path/render-tree-construction) that links the DOM and CSSOM together.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}