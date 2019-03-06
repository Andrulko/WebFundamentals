project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Vous ne pouvez pas optimiser ce que vous ne pouvez pas mesurer. Heureusement, l'API Navigation Timing offre tous les outils nécessaires pour mesurer chaque étape du chemin critique du rendu.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Mesurer le chemin critique du rendu avec Navigation Timing {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Vous ne pouvez pas optimiser ce que vous ne pouvez pas mesurer. Heureusement, l'API Navigation Timing offre tous les outils nécessaires pour mesurer chaque étape du chemin critique du rendu.

* Navigation Timing fournit des horodatages haute résolution pour la mesure du chemin critique du rendu.
* Le navigateur émet une série d'événements consommables qui capturent les différentes étapes du chemin critique du rendu.

Des mesures et des instruments de qualité sont la base de toute stratégie de performances efficace. Et c'est exactement ce que l'API Navigation Timing propose.

## Auditing a page with Lighthouse {: #lighthouse }

Lighthouse is a web app auditing tool that runs a series of tests against a given page, and then displays the page's results in a consolidated report. You can run Lighthouse as a Chrome Extension or NPM module, which is useful for integrating Lighthouse with continuous integration systems.

Chaque libellé du schéma ci-dessus correspond à un horodatage haute résolution que le navigateur suit pour chaque page chargée. En fait, dans ce cas précis nous ne montrons qu'une fraction de l'ensemble des horodatages. Pour le moment, nous ignorerons tous les horodatages associés au réseau, mais nous y reviendrons à l'occasion d'une autre leçon.

Mais alors, que signifient ces horodatages ?

![Lighthouse's CRP audits](images/lighthouse-crp.png)

L'exemple ci-dessus peut semble un peu intimidant de prime abord, mais il est en réalité assez simple. L'API Navigation Timing capture tous les horodatages concernés, et notre code attend simplement le déclenchement de l'événement `onload`. N'oubliez pas que l'événement `onload` est déclenché après domInteractive, domContentLoaded et domComplete. L'API calcule alors la différence entre les différents horodatages.

## Instrumenting your code with the Navigation Timing API {: #navigation-timing }

Cela étant dit, nous disposons maintenant d'étapes spécifiques pour effectuer le suivi et d'une fonction simple pour produire ces mesures. Notez qu'au lieu d'imprimer ces statistiques sur la page, vous pouvez également modifier le code pour les envoyer à un serveur d'analyse ([Google Analytics le fait automatiquement](https://support.google.com/analytics/answer/1205784){: .external }). C'est un excellent moyen de garder un œil sur les performances de vos pages et d'identifier les pages candidates qui pourraient bénéficier d'un travail d'optimisation.

<img src="images/dom-navtiming.png"  alt="Navigation Timing" />

Each of the labels in the above diagram corresponds to a high resolution timestamp that the browser tracks for each and every page it loads. In fact, in this specific case we're only showing a fraction of all the different timestamps &mdash; for now we're skipping all network related timestamps, but we'll come back to them in a future lesson.

So, what do these timestamps mean?

* **domLoading** : c'est l'horodatage de démarrage de la totalité du processus. Le navigateur est sur le point de commencer à analyser les premiers octets reçus pour le document HTML.
* **domInteractive** : indique le moment où le navigateur a terminé d'analyser l'ensemble du code HTML et où la construction du DOM est terminée.
* `domContentLoaded`: indique le moment où le modèle DOM est prêt et où aucune feuille de style n'empêche l'exécution de JavaScript. Cela signifie qu'il est désormais possible (potentiellement) de construire l'arborescence d'affichage. 
    * De nombreux logiciels JavaScript attendent cet événement avant de commencer à exécuter leur propre logique. Pour cette raison, le navigateur capture les horodatages *EventStart* et *EventEnd* pour nous permettre de savoir combien de temps a duré l'exécution.
* **domComplete** : comme son nom l'implique, la totalité du traitement est terminée et le téléchargement de toutes les ressources sur la page (images, etc.) est terminé. C'est-à-dire que le bouton fléché en cours de chargement a cessé de tourner.
* **loadEvent** : c'est la dernière étape du chargement de chaque page. Le navigateur lance un événement `onload` qui peut déclencher une logique d'application supplémentaire.

The HTML specification dictates specific conditions for each and every event: when it should be fired, which conditions should be met, and so on. For our purposes, we'll focus on a few key milestones related to the critical rendering path:

* **domInteractive** indique le moment où le modèle DOM est prêt.
* `domContentLoaded` indique généralement quand [les modèles DOM et CSSOM sont prêts tous les deux](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/){: .external }. 
    * Si aucun analyseur ne bloque le JavaScript, alors *DOMContentLoaded* est déclenché immédiatement après *domInteractive*.
* **domComplete** indique quand la page et toutes ses sous-ressources sont prêtes.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp.html" region_tag="full"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp.html){: target="_blank" .external }

The above example may seem a little daunting on first sight, but in reality it is actually pretty simple. The Navigation Timing API captures all the relevant timestamps and our code simply waits for the `onload` event to fire &mdash; recall that `onload` event fires after `domInteractive`, `domContentLoaded` and `domComplete` &mdash; and computes the difference between the various timestamps.

<img src="images/device-navtiming-small.png"  alt="NavTiming demo" />

All said and done, we now have some specific milestones to track and a simple function to output these measurements. Note that instead of printing these metrics on the page you can also modify the code to send these metrics to an analytics server ([Google Analytics does this automatically](https://support.google.com/analytics/answer/1205784)), which is a great way to keep tabs on performance of your pages and identify candidate pages that can benefit from some optimization work.

## What about DevTools? {: #devtools }

Although these docs sometimes use the Chrome DevTools Network panel to illustrate CRP concepts, DevTools is currently not well-suited for CRP measurements because it does not have a built-in mechanism for isolating critical resources. Run a [Lighthouse](#lighthouse) audit to help identify such resources.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}