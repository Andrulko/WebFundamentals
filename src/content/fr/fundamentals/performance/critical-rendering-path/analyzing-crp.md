project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Identifier et résoudre les problèmes de chemin critique du rendu nécessite une bonne connaissance des pièges habituels. Nous allons étudier la question sous un angle pratique pour en tirer les modèles de performance courants qui vous permettront d'optimiser vos pages.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Analyser la performance du chemin critique du rendu {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Identifier et résoudre les problèmes de chemin critique du rendu nécessite une bonne connaissance des pièges habituels. Nous allons étudier la question sous un angle pratique pour en tirer les modèles de performance courants qui vous permettront d'optimiser vos pages.

L'objectif de l'optimisation du chemin critique du rendu est de permettre au navigateur d'afficher la page aussi vite que possible. Des pages plus rapides se traduisent par une hausse de l'engagement et du nombre de pages vues, et par une [amélioration des conversions](http://www.google.com/think/multiscreen/success.html){: .external }. Par conséquent, pour minimiser le temps passé par l'internaute à scruter un écran vide, nous allons optimiser le choix et l'ordre dans lequel charger les ressources.

Afin d'illustrer ce processus, nous allons commencer par le cas le plus simple possible, puis construire progressivement une page pour y inclure des ressources, des styles et une logique applicative supplémentaires. En même temps, nous verrons les cas qui peuvent poser problème et comment optimiser chacun d'entre eux.

Enfin, un dernier point avant de commencer... Jusqu'ici, nous nous sommes exclusivement intéressés à ce qui se produit dans le navigateur une fois que la ressource (fichier CSS, JavaScript ou HTML) est disponible et peut être traitée. Nous avons ignoré le laps de temps nécessaire pour récupérer celle-ci dans le cache ou sur le réseau. Nous allons étudier très en détail les méthodes d'optimisation des aspects liés au réseau de notre application dans la prochaine leçon. Mais en attendant, pour plus de réalisme, nous allons utiliser les valeurs suivantes :

* Une boucle réseau (latence de propagation) vers le serveur dure 100 ms.
* Le temps de réponse du serveur est de 100 ms pour un document HTML et de 10 ms pour tous les autres fichiers.

## L'expérience Hello World

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Nous allons commencer avec un balisage HTML basique et une seule image, sans CSS ni JavaScript, ce qui est d'une simplicité enfantine. Nous allons à présent ouvrir notre chronologie réseau dans Chrome DevTools, puis examiner le graphique en cascade des ressources qui en résulte :

We'll start with basic HTML markup and a single image; no CSS or JavaScript. Let's open up our Network timeline in Chrome DevTools and inspect the resulting resource waterfall:

<img src="images/waterfall-dom.png" alt="CRP" />

Une fois que le contenu HTML devient disponible, le navigateur doit analyser les octets, les convertir en jetons et construire l'arborescence DOM. Dans DevTools, vous remarquerez, ce qui est pratique, que la durée de l'événement DOMContentLoaded est indiquée sur la dernière ligne (216 ms) et qu'elle est représentée par la ligne verticale bleue. L'intervalle situé entre la fin du téléchargement du fichier HTML et la ligne verticale bleue (DOMContentLoaded) représente le temps nécessaire au navigateur pour construire l'arborescence DOM, à peine quelques millisecondes, dans le cas présent.

Enfin, il est intéressant de remarquer que la photo 'You're awesome' n'a pas bloqué l'événement DOMContentLoaded. Il s'avère que nous pouvons construire l'arborescence de rendu et même afficher la page sans attendre tous les éléments de celle-ci : ** toutes les ressources ne sont pas essentielles pour générer le premier affichage rapide**. Concrètement, comme nous le verrons, lorsqu'on évoque le chemin critique du rendu, il s'agit en général du balisage HTML, de CSS et de JavaScript. Les images ne bloquent pas l'affichage initial de la page. Cependant, nous devons bien entendu tenter de nous assurer que les images en question d'affichent également dès que possible.<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

Ceci étant, l'événement `load` (également appelé `onload` de manière courante) est bloqué sur l'image. DevTools affiche l'événement `onload` à 335 ms. Pour rappel, l'événement `onload` marque le moment où *toutes les ressources* nécessaires à la page ont été téléchargées et traitées. C'est à ce moment que l'indicateur de chargement peut cesser de tourner dans le navigateur. Il est représenté par une ligne verticale rouge dans le graphique en cascade.

Notre page de l'expérience 'Hello World' est simple en apparence, mais en coulisses, tout un processus est mis en œuvre pour qu'elle fonctionne. Ceci dit, en pratique, nous aurons besoin de bien plus que du HTML. Nous utiliserons probablement une feuille de styles CSS ainsi qu'un ou plusieurs scripts, afin de rendre la page plus interactive. Ajoutons à présent ces deux éléments dans la formule pour voir ce qui va se produire :

That said, the `load` event (also known as `onload`), is blocked on the image: DevTools reports the `onload` event at 335ms. Recall that the `onload` event marks the point at which **all resources** that the page requires have been downloaded and processed; at this point, the loading spinner can stop spinning in the browser (the red vertical line in the waterfall).

## Ajouter JavaScript et CSS à la formule

Our "Hello World experience" page seems simple but a lot goes on under the hood. In practice we'll need more than just the HTML: chances are, we'll have a CSS stylesheet and one or more scripts to add some interactivity to our page. Let's add both to the mix and see what happens:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_timing.html){: target="_blank" .external }

*Before adding JavaScript and CSS:*

<img src="images/waterfall-dom.png" alt="DOM CRP" />

*With JavaScript and CSS:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

Adding external CSS and JavaScript files adds two extra requests to our waterfall, all of which the browser dispatches at about the same time. However, **note that there is now a much smaller timing difference between the `domContentLoaded` and `onload` events.**

What happened?

* À la différence de l'exemple comportant simple fichier HTML, il faut désormais récupérer et analyser le fichier CSS pour construire le modèle objet CSS (CSSOM). Nous savons également que le DOM et le CSSOM sont nécessaires pour construire l'arborescence de rendu.
* Comme un analyseur bloque également le fichier JavaScript sur notre page, l'événement DOMContentLoaded est bloqué jusqu'au téléchargement et à l'analyse du fichier CSS. Le fichier JavaScript peut envoyer une requête au CSSOM, nous devons donc bloquer et attendre le fichier CSS avant de pouvoir exécuter le fichier JavaScript.

**What if we replace our external script with an inline script?** Even if the script is inlined directly into the page, the browser can't execute it until the CSSOM is constructed. In short, inlined JavaScript is also parser blocking.

Nous avons effectué une requête de moins, mais les temps des événements `DOMContentLoaded` et `onload` sont effectivement les mêmes. Pourquoi ? Nous savons à présent qu'il importe peu que le fichier JavaScript soit intégré ou externe, car dès que le navigateur charge la balise du script, il se bloque et attend que le CSSOM soit construit. De plus, dans notre premier exemple, le téléchargement des fichiers CSS et JavaScript s'effectue en parallèle dans le navigateur et se termine en même temps. Par conséquent, dans ce cas particulier, l'intégration du code JavaScript n'est pas d'une grande utilité. Cette solution ne convient pas. Alors que faire pour que notre page s'affiche plus vite ? En fait, il existe plusieurs stratégies différentes.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Inlined JavaScript:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, and inlined JS" />

We are making one less request, but both our `onload` and `domContentLoaded` times are effectively the same. Why? Well, we know that it doesn't matter if the JavaScript is inlined or external, because as soon as the browser hits the script tag it blocks and waits until the CSSOM is constructed. Further, in our first example, the browser downloads both CSS and JavaScript in parallel and they finish downloading at about the same time. In this instance, inlining the JavaScript code doesn't help us much. But there are several strategies that can make our page render faster.

C'est beaucoup mieux ! L'événement DOMContentLoaded survient peu de temps après l'analyse du HTML. Le navigateur détecte que le JavaScript ne doit pas être bloqué. Comme il n'y a pas d'autres scripts bloquants pour l'analyseur, la construction CSSOM peut également s'effectuer en parallèle.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

Much better! The `domContentLoaded` event fires shortly after the HTML is parsed; the browser knows not to block on JavaScript and since there are no other parser blocking scripts the CSSOM construction can also proceed in parallel.

**L'intervalle de temps compris entre T<sub>0</sub> et T<sub>1</sub> indique la durée de traitement du réseau et du serveur.** Dans le meilleur des cas, (si le fichier HTML est de petite taille), il suffit d'une seule boucle réseau pour récupérer l'intégralité du document. En raison du fonctionnement des protocoles de transport TCP, les fichiers de taille plus importante peuvent nécessiter davantage de boucles. Nous reviendrons sur ce sujet dans une leçon ultérieure. **Par conséquent, nous pouvons affirmer que la page ci-dessus, dans le meilleur des cas, a un chemin critique du rendu d'une boucle (au minimum).**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

À présent, observons la même page, avec un fichier CSS externe :

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="Chemin critique du rendu avec DOM et CSSOM" />

Là encore, la récupération du fichier réseau prend une boucle réseau. Le balisage récupéré indique ensuite que le fichier CSS est également requis. Cela signifie que le navigateur doit retourner sur le serveur, puis récupérer le CSS avant de pouvoir afficher la page à l'écran. **Par conséquent, un minimum de deux boucles réseau est nécessaire pour afficher cette page**. Encore une fois, le chargement du fichier CSS peut nécessiter plusieurs boucles, d'où l'accent sur le terme `minimum`.

Nous allons définir le vocabulaire que nous utiliserons pour décrire le chemin critique du rendu :

Comparons à présent ces données aux caractéristiques du chemin critique de l'exemple HTML + CSS ci-dessus :

## Modèles de performance

The simplest possible page consists of just the HTML markup; no CSS, no JavaScript, or other types of resources. To render this page the browser has to initiate the request, wait for the HTML document to arrive, parse it, build the DOM, and then finally render it on the screen:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Les fichiers HTML et CSS sont tous les deux nécessaires à la construction de l'arborescence de rendu, ce qui en fait deux ressources cruciales. Le fichier CSS n'est récupéré qu'une fois que le navigateur a chargé le document HTML. La longueur minimale du chemin critique est donc de deux boucles. Les deux ressources totalisent une somme de 9 Ko d'octets cruciaux.

<img src="images/analysis-dom.png" alt="Hello world CRP" />

Nous avons ajouté le fichier app.js, qui est un élément JavaScript externe sur cette page. Comme nous le savons à présent, il s'agit d'une ressource bloquant l'analyseur (c'est à dire, cruciale). Pire encore, nous allons devoir bloquer et attendre le CSSOM afin d'exécuter le fichier JavaScript. Pour rappel, le fichier JavaScript peut envoyer une requête au CSSOM et donc le navigateur se mettra en pause jusqu'au téléchargement du fichier `css.style` et à la construction du CSSOM.

Now, let's consider the same page but with an external CSS file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Ceci étant, concrètement, en observant le 'graphique en cascade du réseau' pour cette page, vous remarquerez que les requêtes CSS et JavaScript seront initiées à peu près au même moment. Le navigateur reçoit le fichier HTML, découvre les deux ressources, puis lance es deux requêtes. Ainsi, les caractéristiques de chemin critique de cette page sont les suivantes :

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

Après en avoir discuté avec les développeurs de notre site, nous avons réalisé que le fichier JavaScript inclus dans notre page ne doit pas forcément être bloquant. Elle contient des statistiques et d'autres codes qui ne bloquent pas obligatoirement son affichage. Sachant cela, nous pouvons ajouter l'attribut `async` à la balise de script pour débloquer l'analyseur :

Let's define the vocabulary we use to describe the critical rendering path:

* **Ressource cruciale** : ressource susceptible de bloquer l'affichage initial de la page.
* **Longueur du chemin critique** : nombre de boucles ou temps total nécessaire à la récupération de toutes les ressources cruciales.
* **Octets cruciaux** : nombre total d'octets nécessaires pour obtenir le premier affichage de la page, composé de la somme des tailles de transfert des fichiers de toutes les ressources cruciales Notre premier exemple de page avec un seul fichier HTML contenait une ressource cruciale unique (le document HTML), la longueur du chemin critique équivalait à une boucle réseau (dans l'hypothèse d'un fichier de petite taille) et le nombre d'octets cruciaux n'était que la taille de transfert du document HTML lui-même.

Rendre le script asynchrone comporte plusieurs avantages :

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** ressources cruciales
* **2** ou plusieurs boucles réseau pour la longueur minimum du chemin critique
* **9** Ko d'octets cruciaux

Pour finir, supposons que la feuille de style CSS ne soit utile que pour l'impression. Quel serait le résultat ?

Now let's add an extra JavaScript file into the mix.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Comme la ressource css.style ne sert qu'à l'impression, le navigateur n'a pas besoin de la bloquer pour afficher la page. Donc, dès que la construction du DOM est terminée, le navigateur dispose de suffisamment d'informations pour afficher la page. Par conséquent, cette page ne comporte qu'une seule ressource cruciale (le document HTML) et la longueur minimale du chemin critique du rendu est d'une boucle.

We added `app.js`, which is both an external JavaScript asset on the page and a parser blocking (that is, critical) resource. Worse, in order to execute the JavaScript file we have to block and wait for CSSOM; recall that JavaScript can query the CSSOM and hence the browser pauses until `style.css` is downloaded and CSSOM is constructed.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

That said, in practice if we look at this page's "network waterfall," you'll see that both the CSS and JavaScript requests are initiated at about the same time; the browser gets the HTML, discovers both resources, and initiates both requests. As a result, the above page has the following critical path characteristics:

* **3** ressources cruciales
* **2** ou plusieurs boucles réseau pour la longueur minimum du chemin critique
* **11** Ko d'octets cruciaux

We now have three critical resources that add up to 11KB of critical bytes, but our critical path length is still two roundtrips because we can transfer the CSS and JavaScript in parallel. **Figuring out the characteristics of your critical rendering path means being able to identify the critical resources and also understanding how the browser will schedule their fetches.** Let's continue with our example.

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

An asynchronous script has several advantages:

* Le script n'est plus bloquant pour l'analyseur et ne fait pas partie du chemin critique du rendu.
* Comme il n'y a pas d'autres scripts cruciaux, le fichier CSS ne doit pas bloquer l'événement DOMContentLoaded.
* Plus tôt l'événement DOMContentLoaded est déclenché, plus tôt d'autres logiques applicatives peuvent commencer à s'exécuter.

As a result, our optimized page is now back to two critical resources (HTML and CSS), with a minimum critical path length of two roundtrips, and a total of 9KB of critical bytes.

Finally, if the CSS stylesheet were only needed for print, how would that look?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

Because the style.css resource is only used for print, the browser doesn't need to block on it to render the page. Hence, as soon as DOM construction is complete, the browser has enough information to render the page. As a result, this page has only a single critical resource (the HTML document), and the minimum critical rendering path length is one roundtrip.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}