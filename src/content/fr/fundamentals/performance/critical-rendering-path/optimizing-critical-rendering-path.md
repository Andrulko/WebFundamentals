project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Afin de fournir un premier affichage le plus rapidement possible, nous devons optimiser trois variables : réduire le nombre de ressources critiques, réduire le nombre d'octets critiques et réduire la longueur du chemin critique.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Optimiser le chemin critique du rendu {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Afin de fournir un premier affichage le plus rapidement possible, nous devons optimiser trois variables :

- The number of critical resources.
- The critical path length.
- The number of critical bytes.

Une ressource critique est toute ressource susceptible d'empêcher l'affichage initial de la page. Plus le nombre de ces ressources sur la page est réduit, moins le navigateur doit fournir de travail pour afficher le contenu à l'écran, et moins il y a de contention pour le processeur et les autres ressources.

De même, plus le nombre d'octets critiques que le navigateur doit télécharger est réduit, plus vite il peut passer au traitement du contenu et l'afficher à l'écran. Pour réduire le nombre d'octets, nous pouvons réduire le nombre de ressources en les éliminant ou en les rendant non critiques. Nous pouvons également nous assurer de réduire au maximum la taille du transfert en compressant et en optimisant chaque ressource.

Enfin, la longueur du chemin critique dépend du graphique de dépendance entre toutes les ressources critiques requises par la page et leur taille en octets : le téléchargement de certaines ressources ne peut être lancé que lorsqu'une ressource la précédent a été traitée. Et plus une ressource est importante, plus le nombre d'allers-retours nécessaires pour la télécharger sera élevé.

**The general sequence of steps to optimize the critical rendering path is:**

1. Analysez et définissez votre chemin critique : nombre de ressources, octets, longueur.
2. Réduisez le nombre de ressources critiques : éliminez-les, différez leur téléchargement, marquez-les comme asynchrones, etc.
3. Optimisez l'ordre de chargement des ressources critiques restantes : téléchargez en priorité tous les éléments critiques pour réduire la longueur du chemin critique.
4. Optimisez le nombre d'octets critiques pour réduire la durée du téléchargement (le nombre d'allers-retours).

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}