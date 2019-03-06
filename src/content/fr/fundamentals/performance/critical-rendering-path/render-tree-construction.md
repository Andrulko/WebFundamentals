project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Les arborescences des modèles CSSOM et DOM sont combinées pour former une arborescence d'affichage, qui est ensuite utilisée pour calculer la mise en page de chaque élément visible et fait office de données d'entrée pour le processus de peinture qui affiche les pixels à l'écran. L'optimisation de chacune de ces étapes est essentielle pour obtenir des performances d'affichage optimales.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Construction, mise en page et peinture de l'arborescence d'affichage {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Les arborescences des modèles CSSOM et DOM sont combinées pour former une arborescence d'affichage, qui est ensuite utilisée pour calculer la mise en page de chaque élément visible et fait office de données d'entrée pour le processus de peinture qui affiche les pixels à l'écran. L'optimisation de chacune de ces étapes est essentielle pour obtenir des performances d'affichage optimales.

Dans la section précédente sur la construction du modèle d'objet, nous avons créé les arborescences des modèles DOM et CSSOM en fonction des données HTML et CSS. Cependant, il s'agit d'objets indépendants qui capturent différents aspects du document : l'un décrit le contenu et l'autre les règles de style qui doivent être appliquées au document. Comment pouvons-nous fusionner les deux pour que le navigateur affiche les pixels à l'écran ?

### TL;DR {: .hide-from-toc }

* Les arborescences des modèles DOM et CSSOM sont combinées pour former l'arborescence d'affichage.
* L'arborescence d'affichage ne contient que les nœuds nécessaires pour afficher la page.
* La mise en page calcule la position et la taille exactes de chaque objet.
* La peinture est la dernière étape, et affiche les pixels à l''écran à partir de l''arborescence d''affichage finale.

La première étape consiste à faire en sorte que le navigateur associe les modèles DOM et CSSOM pour former une 'arborescence d'affichage' qui capture tout le contenu DOM visible sur la page, ainsi que toutes les informations de style du modèle CSSOM pour chaque nœud.

<img src="images/render-tree-construction.png" alt="Les modèles DOM et CSSOM sont combinés pour créer l'arborescence d'affichage" />

Pour construire l'arborescence d'affichage, le navigateur procède à peu près de la façon suivante :

1. Starting at the root of the DOM tree, traverse each visible node.
    
    * Certains nœuds ne sont pas du tout visibles (les balises de script, les balises Meta, etc.) et sont omis, puisqu'ils ne sont pas représentés dans la sortie affichée.
    * Certains nœuds sont masqués par le code CSS et sont également omis dans l'arborescence d'affichage. Par exemple, le nœud `span` dans l'exemple ci-dessus est absent de l'arborescence d'affichage, car nous disposons d'une règle explicite qui lui applique la propriété `display: none`.

2. For each visible node, find the appropriate matching CSSOM rules and apply them.

3. Émettez les nœuds visibles avec leur contenu et leurs styles calculés.

Note: Notez que la propriété `visibility: hidden` est différente de la propriété `display: none`. La première rend l'élément invisible, mais celui-ci occupe toujours de l'espace dans la mise en page, c'est-à-dire qu'il est affiché sous la forme d'une case vide. La seconde `display: none` supprime totalement l'élément de l'arborescence d'affichage, afin que celui-ci soit invisible et ne fasse pas partie de la mise en page.

La sortie finale est un affichage qui contient à la fois les informations sur le contenu et sur le style de tout le contenu visible à l'écran. Nous avons presque terminé ! **Une fois l'arborescence d'affichage en place, nous pouvons passer à l'étape de 'mise en page'.**

Jusqu'à maintenant, nous avons calculé quels nœuds doivent être visibles, ainsi que leurs styles calculés. Toutefois, nous n'avons pas calculé leur position et leur taille exactes dans la [fenêtre](/web/fundamentals/design-and-ux/responsive/#set-the-viewport) de l'appareil. C'est l'étape de 'mise en page', parfois appelée 'ajustement de la mise en page'.

Pour définir la taille et la position exactes de chaque objet, le navigateur commence à la racine de l'arborescence d'affichage et la traverse pour calculer la géométrie de chaque objet sur la page. Prenons un exemple simple et concret :

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/nested.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Le corps de la page ci-dessus contient deux éléments DIV imbriqués : le premier élément DIV (parent) définit la taille d'affichage du nœud à 50 % de la largeur de la fenêtre, et le second élément DIV contenu par le parent définit sa largeur à 50 % de cette du parent, soit 25 % de la largeur de la fenêtre !

The body of the above page contains two nested div's: the first (parent) div sets the display size of the node to 50% of the viewport width, and the second div\---contained by the parent\---sets its width to be 50% of its parent; that is, 25% of the viewport width.

<img src="images/layout-viewport.png" alt="Calculating layout information" />

Enfin, maintenant que nous savons quels nœuds sont visibles, et que nous connaissons leurs styles calculés et leur géométrie, nous pouvons enfin transmettre ces informations à la dernière étape, qui convertira chaque nœud de l'arborescence d'affichage en pixels réels à l'écran. Cette étape est souvent appelée 'peinture' ou 'pixellisation des données'.

Avez-vous bien compris ? Chacune de ces étapes demande au navigateur une quantité de travail non négligeable, ce qui signifie également qu'elle peut prendre un certain temps. Heureusement, Chrome DevTools peut nous aider à mieux comprendre les trois étapes décrites ci-dessus. Étudions de plus près l'étape de mise en page pour notre exemple 'bonjour le monde' original :

This can take some time because the browser has to do quite a bit of work. However, Chrome DevTools can provide some insight into all three of the stages described above. Let's examine the layout stage for our original "hello world" example:

<img src="images/layout-timeline.png" alt="Measuring layout in DevTools" />

* La construction de l'arborescence d'affichage et le calcul de la position et de la taille sont capturés avec l'événement 'Mise en page' dans la chronologie.
* Une fois la mise en page terminée, le navigateur produit des événements 'Configuration de la peinture' et 'Peinture' qui convertissent l'arborescence d'affichage en pixels réels à l'écran.

Une fois ces étapes effectuées, notre page est enfin visible dans la fenêtre, youpi !

The page is finally visible in the viewport:

<img src="images/device-dom-small.png" alt="Rendered Hello World page" />

Notre page de démo peut sembler très simple, mais elle nécessite beaucoup de travail ! Pouvez-vous deviner ce qui ce passerait si le modèle DOM ou CSSOM était modifié ? Nous devrions répéter la totalité du processus pour déterminer quels pixels doivent être affichés à nouveau à l'écran.

1. Traiter le balisage HTML et créer l'arborescence du modèle DOM.
2. Traiter le balisage CSS et créer l'arborescence du modèle CSSOM.
3. Combiner les modèles DOM et CSSOM pour créer une arborescence d'affichage.
4. Exécuter la mise en page sur l'arborescence d'affichage pour calculer la géométrie de chaque nœud.
5. Peindre chaque nœud sur l'écran.

**L'optimisation du chemin critique du rendu est le processus qui consiste à réduire la durée totale des étapes 1 à 5 dans la séquence ci-dessus.** Cela nous permet d'afficher le contenu à l'écran le plus rapidement possible, mais également de réduire le temps écoulé entre les mises à jour de l'écran après l'affichage initial. C'est-à-dire que cela permet d'obtenir un taux d'actualisation plus élevé pour le contenu interactif.

***Optimizing the critical rendering path* is the process of minimizing the total amount of time spent performing steps 1 through 5 in the above sequence.** Doing so renders content to the screen as quickly as possible and also reduces the amount of time between screen updates after the initial render; that is, achieve higher refresh rates for interactive content.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}