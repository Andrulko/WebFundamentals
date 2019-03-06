project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Une fois que nous avons éliminé toutes les ressources inutiles, l'étape suivante consiste à réduire la taille totale des ressources restantes que le navigateur doit télécharger, c'est-à-dire les compresser en appliquant des algorithmes (GZip) de compression spécifiques au type de contenu et génériques.

{# wf_updated_on: 2014-09-11 #} {# wf_published_on: 2014-03-31 #}

# Optimiser l'encodage et la taille de transfert des éléments de texte {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Nos applications Web continuent de se développer en termes de portée, d'ambition et de fonctionnalité. Et c'est une bonne chose. Cependant, la course incessante vers un Web plus riche provoque une autre tendance : la quantité de données téléchargée par chaque application augmente sens cesse, rapidement. Pour offrir d'excellentes performances, nous devons optimiser la livraison de chaque octet de données !

## Compression de données 101

Une fois que nous avons éliminé toutes les ressources inutiles, l'étape suivante consiste à réduire la taille totale des ressources restantes que le navigateur doit télécharger, c'est-à-dire les compresser. En fonction du type de ressource (texte, images, polices, etc.) nous disposons de plusieurs techniques : outils génériques pouvant être activés sur le serveur, optimisations avant le traitement pour des types de contenu spécifiques et optimisations spécifiques à la ressource nécessitant une intervention du développeur.

Pour offrir les meilleures performances, il est nécessaire de combiner toutes ces techniques.

### TL;DR {: .hide-from-toc }

* La compression est le processus qui consiste à encoder des informations en utilisant un nombre réduit de bits.
* L'élimination des données inutiles offre toujours les meilleurs résultats.
* Il existe un grand nombre de techniques et d'algorithmes de compression différents.
* Vous aurez besoin de plusieurs techniques pour obtenir la meilleure compression possible.

Le processus de réduction de la taille des données est appelé 'compression des données', et c'est à lui seul un vaste domaine d'étude : de nombreuses personnes ont passé la totalité de leur carrière à travailler sur les algorithmes, techniques et optimisations afin d'améliorer les taux de compression, la vitesse et la mémoire requise par divers logiciels de compression. Il va sans dire qu'une discussion complète sur le sujet est hors de notre portée, mais il est cependant important de comprendre, à un niveau élevé, comment fonctionne la compression et quelles sont les techniques à notre disposition pour réduire la taille des différents éléments requis par nos pages.

Pour illustrer les principes fondamentaux de ces techniques en action, voyons comment nous pouvons optimiser un format de message texte simple que nous inventerons tout spécialement pour cet exemple :

    # Ci-dessous se trouve un message secret, composé d'un ensemble d'en-têtes au
    # format 'valeur clé', suivi d'une nouvelle ligne et du message crypté.
    format: secret-cipher
    date: 04/04/14
    AAAZZBBBBEEEMMM EEETTTAAA
    

1. Les messages peuvent contenir des annotations arbitraires, indiquées par le préfixe `#`. Les annotations n'affectent pas la signification, ni tout autre comportement du message.
2. Les messages peuvent contenir des 'en-têtes' qui sont des paires de valeurs clés (séparées par `:`) et qui doivent apparaître au début du message.
3. Les messages portent des données utiles au format texte.

Que pourrions-nous faire pour réduire la taille du message ci-dessus, qui est actuellement de 200 caractères ?

1. Ce commentaire est intéressant, mais nous savons qu'en réalité il n'affecte pas la signification du message. Nous l'éliminons donc au moment de la transmission du message.
2. Il existe probablement des techniques intelligentes que nous pourrions utiliser pour encoder les en-têtes de façon efficace. Par exemple, nous ne savons pas si tous les messages ont toujours un `format` et une `date`, mais si c'était le cas, nous pourrions convertir ces éléments en identifiants courts composés de nombres entiers, et simplement envoyer ces identifiants ! Cela dit, nous ne sommes pas certains que ce soit le cas, alors nous n'y toucherons pas pour l'instant.
3. The payload is text only, and while we don’t know what the contents of it really are (apparently, it’s using a "secret-message"), just looking at the text shows that there's a lot of redundancy in it. Perhaps instead of sending repeated letters, you can just count the number of repeated letters and encode them more efficiently. For example, "AAA" becomes "3A", which represents a sequence of three A’s.

En combinant nos techniques, nous obtenons le résultat suivant :

    format: secret-cipher
    date: 04/04/14
    3A2Z4B3E3M 3E3T3A
    

Le nouveau message comporte 56 caractères, ce qui signifie que nous avons réussi à compresser notre message d'origine de 72 %, ce qui est plutôt bien tout compte fait. En nous ne faisons que commencer !

Bien sûr, vous vous dites peut-être que c'est très bien, mais comment cela nous aide-t-il à optimiser nos pages Web ? Nous ne sommes pas en train d'essayer d'inventer nos propres algorithmes de compression, n'est-ce pas ? Bien sûr que non. Mais comme vous le verrez, nous allons utiliser exactement les mêmes techniques et façons de penser lors de l'optimisation de plusieurs ressources sur nos pages : prétraitement, optimisations spécifiques au contexte, et différents algorithmes pour différents contenus.

## Réduction de la taille : optimisations de prétraitement et spécifiques au contexte

### TL;DR {: .hide-from-toc }

* Les optimisations spécifiques au contenu peuvent réduire de façon importante la taille des ressources livrées.
* Les optimisations spécifiques au contenu sont d'autant plus efficaces lorsqu'elles sont appliquées dans le cadre de votre cycle de construction et de diffusion.

Le meilleur moyen de compresser les données redondantes ou inutiles est de les supprimer totalement. Bien sûr, nous ne pouvons pas simplement supprimer des données au hasard. Mais dans certains contextes permettant d'avoir des connaissances spécifiques au contexte sur le format des données et leurs propriétés, il est souvent possible de réduire de façon importante la taille des données utiles sans affecter leur signification réelle.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/optimizing-content-efficiency/_code/minify.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Considérez la page simple HTML ci-dessus et les trois différents types de contenu qui la composent : balisage HTML, styles CSS et JavaScript. Chaque type de contenu a des règles différentes pour définir ce qui constitue un balisage HTML valide, des règles CSS ou du contenu JavaScript, des règles différentes pour indiquer les commentaires, etc. Comment faire pour réduire la taille de cette page ?

Consider the simple HTML page above and the three different content types that it contains: HTML markup, CSS styles, and JavaScript. Each of these content types has different rules for what constitutes valid content, different rules for indicating comments, and so on. How can we reduce the size of this page?

* Les commentaires sur le code sont essentiels pour un développeur, mais le navigateur n'a pas besoin de les voir ! Le simple fait de supprimer les commentaires CSS ('/* ... */'), HTML ('<!-- ... -->') et JavaScript ('// ...') peut permettre de réduire de façon importante la taille totale de la page.

* Un logiciel de compression CSS 'intelligent' pourrait remarquer que notre façon de définir les règles pour `.awesome-container` est inefficace, et regrouper les deux déclarations en une seule sans affecter les autres styles. Cela permettrait d'économiser encore davantage d'octets.
* Les blancs (espaces et tabulations) sont une commodité pour le développeur dans le code HTML, CSS et JavaScript. Un logiciel de compression supplémentaire pourrait supprimer toutes les tabulations et tous les espaces.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/optimizing-content-efficiency/_code/minified.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Après avoir appliqué les étapes ci-dessus, notre page passe de 406 à 150 caractères, soit une compression de 63 % ! Soit, elle n'est pas très lisible. Mais elle n'a pas besoin de l'être : nous pouvons conserver la page originale comme 'version de développement', puis appliquer les étapes ci-dessus lorsque nous serons prêts à publier la page sur notre site Web.

Prenons un instant et remarquons que l'exemple ci-dessus illustre un point important : un logiciel de compression d'utilisation générale, disons conçu pour compresser du texte aléatoire, pourrait probablement compresser de façon efficace la page ci-dessus, mais ne saurait pas supprimer les commentaires, réduire les règles CSS, ou encore effectuer des dizaines d'autres optimisations spécifiques au contexte. C'est pour cela que l'optimisation de prétraitement, de réduction de la taille et en fonction du contexte peut être un outil très puissant.

Note: Dans ce cas, la version du développement non compressé de la bibliothèque JQuery approche maintenant les 300 Ko. La même bibliothèque, une fois réduite (commentaires supprimés, etc.) est environ 3x plus petite : ~100 Ko.

De même, les techniques ci-dessus peuvent être appliquées à d'autres éléments que le texte. Images, vidéos et autres types de contenu contiennent tous leurs propres formes de métadonnées et différentes données utiles. Par exemple, lorsque vous prenez une photo avec un appareil photo, la photo contient généralement un grand nombre d'informations supplémentaires : paramètres de l'appareil photo, localisation, etc. Selon votre application, ces données peuvent être essentielles (par exemple sur un site de partage de photos) ou complètement inutile. Vous devez donc vous demander s'il est possible de les supprimer. En pratique, ces métadonnées peuvent représenter jusqu'à des dizaines de kilo-octets pour chaque image !

En résumé, la première étape de l'optimisation de l'efficacité de vos éléments consiste à créer un inventaire des différents types de contenu et à définir quelles optimisations spécifiques au contexte vous pouvez appliquer afin de réduire leur taille. Vous pouvez ainsi réaliser des économies importantes ! Ensuite, une fois que vous les avez identifiées, automatisez ces optimisations en les ajoutant à vos processus de création et de diffusion. C'est le seul moyen de garantir que les optimisations resteront en place.

[GZIP](http://fr.wikipedia.org/wiki/Gzip){: .external } est un logiciel de compression générique qui peut être appliqué à n'importe quel flux d'octets : il se souvient du contenu déjà rencontré et tenter d'identifier et de remplacer les données en double de façon efficace. Pour les curieux, voici [une excellente explication de bas niveau de GZIP](https://www.youtube.com/watch?v=whGwm0Lky2s&feature=youtu.be&t=14m11s){: .external }. Cependant en pratique, GZIP est surtout efficace sur le contenu à base de texte, et obtient souvent des taux de compression de 70 à 90 % pour les fichiers volumineux, alors que son exécution sur des éléments déjà compressés par des algorithmes alternatifs (notamment la plupart des formats d'image) donne des résultats minimes voire nuls.

## Compression de texte avec GZIP

### TL;DR {: .hide-from-toc }

* GZIP offre les meilleures performances sur les éléments à base de texte : CSS, JavaScript, HTML.
* Tous les navigateurs modernes sont compatibles avec la compression GZIP et la demandent automatiquement.
* Votre serveur doit être configuré pour autoriser la compression GZIP.
* Certains CDN nécessitent une attention particulière pour s'assurer que GZIP est activé.

Tous les navigateurs modernes sont compatibles avec la compression GZIP et l'effectue de façon automatique pour toutes les requêtes HTTP : notre travail consiste à nous assurer que le serveur est correctement configuré pour diffuser la ressource compressée lorsque le client la demande.

Le tableau ci-dessus illustre les économies réalisées par la compression GZIP pour quelques-unes des librairies JavaScript et quelques-uns des logiciels intégrés CSS les plus populaires. Les économies réalisées vont de 60 à 88 %. Notez également que la combinaison de fichiers réduits (identifiés par `.min` dans leur nom) et de GZIP offre une économie encore plus importante.

<table>
  
<thead>
  <tr>
    <th>Bibliothèque</th>
    <th>Taille</th>
    <th>Taille compressée</th>
    <th>Taux de compression</th>
  </tr>
</thead>

<tr>
  <td data-th="library">jquery-1.11.0.js</td>
  <td data-th="size">276 Ko</td>
  <td data-th="compressed">82 Ko</td>
  <td data-th="savings">70 %</td>
</tr>
<tr>
  <td data-th="library">jquery-1.11.0.min.js</td>
  <td data-th="size">94 Ko</td>
  <td data-th="compressed">33 Ko</td>
  <td data-th="savings">65 %</td>
</tr>
<tr>
  <td data-th="library">angular-1.2.15.js</td>
  <td data-th="size">729 Ko</td>
  <td data-th="compressed">182 Ko</td>
  <td data-th="savings">75 %</td>
</tr>
<tr>
  <td data-th="library">angular-1.2.15.min.js</td>
  <td data-th="size">101 Ko</td>
  <td data-th="compressed">37 Ko</td>
  <td data-th="savings">63 %</td>
</tr>
<tr>
  <td data-th="library">bootstrap-3.1.1.css</td>
  <td data-th="size">118 Ko</td>
  <td data-th="compressed">18 Ko</td>
  <td data-th="savings">85 %</td>
</tr>
<tr>
  <td data-th="library">bootstrap-3.1.1.min.css</td>
  <td data-th="size">98 Ko</td>
  <td data-th="compressed">17 Ko</td>
  <td data-th="savings">83 %</td>
</tr>
<tr>
  <td data-th="library">foundation-5.css</td>
  <td data-th="size">186 Ko</td>
  <td data-th="compressed">22 Ko</td>
  <td data-th="savings">88 %</td>
</tr>
<tr>
  <td data-th="library">foundation-5.min.css</td>
  <td data-th="size">146 Ko</td>
  <td data-th="compressed">18 Ko</td>
  <td data-th="savings">88 %</td>
</tr>
</table>

Le meilleur dans tout cela, c'est que l'activation de GZIP est l'une des optimisations les plus simples et les plus rentables à mettre en œuvre. Malheureusement, beaucoup oublient encore de le faire. La plupart des serveurs Web compressent le contenu à votre place, et il vous suffit de vérifier que le serveur est correctement configuré pour compresser tous les types de contenu qui peuvent profiter de la compression GZIP.

1. **Apply content-specific optimizations first: CSS, JS, and HTML minifiers.**
2. **Apply GZIP to compress the minified output.**

Quelle est la meilleure configuration pour votre serveur ? Le projet HTML5 Boilerplate contient des [exemples de fichiers de configuration](https://github.com/h5bp/server-configs){: .external } pour tous les serveurs les plus populaires, ainsi que des commentaires détaillés pour chaque indicateur et paramètre de configuration : recherchez votre serveur de prédilection dans la liste, recherchez la section GZIP, et vérifiez que paramètres recommandés sont configurés pour votre serveur.

The HTML5 Boilerplate project contains [sample configuration files](https://github.com/h5bp/server-configs) for all the most popular servers with detailed comments for each configuration flag and setting. To determine the best configuration for your server, do the following:

* Find your favorite server in the list.
* Look for the GZIP section.
* Confirm that your server is configured with the recommended settings.

<img src="images/transfer-vs-actual-size.png"
  alt="DevTools demo of actual vs transfer size" />

Note: Croyez-le ou non, dans certains cas la compression GZIP peut augmenter la taille de l'élément. Cela se produit généralement lorsque l'élément est très petit et que le temps système du dictionnaire GZIP est supérieur à l'économie réalisée par la compression, ou lorsque la ressource est déjà bien compressée. Certains serveurs vous permettent de définir un 'seuil de taille de fichier minimal' afin d'éviter ce problème.

Pour finir, un mot d'avertissement : si la plupart des serveurs compressent automatiquement les éléments pour vous lorsque vous les mettez à la disposition des utilisateurs, certains CDN nécessitent une attention particulière et une action manuelle pour s'assurer que l'élément GZIP est diffusé. Auditez votre site et assurez-vous que vos éléments sont bien [compressés](http://www.whatsmyip.org/http-compression-test/){: .external } !

Finally, while most servers automatically compress the assets for you when serving them to the user, some CDNs require extra care and manual effort to ensure that the GZIP asset is served. Audit your site and ensure that your assets are, in fact, [being compressed](http://www.whatsmyip.org/http-compression-test/).

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}