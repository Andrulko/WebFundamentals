project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: RAIL es un modelo de rendimiento centrado en el usuario. Todas las aplicaciones web tienen estos cuatro aspectos distintivos en sus ciclos de vida, y el rendimiento se adapta a ellos de maneras muy diferentes: Respuesta, animación, inactividad, carga.

{# wf_updated_on: 2015-06-07 #} {# wf_published_on: 2015-06-07 #}

# Mide el rendimiento con el modelo RAIL {: .page-title }

{% include "web/_shared/contributors/megginkearney.html" %}

RAIL es un modelo de rendimiento centrado en el usuario. Todas las apps web tienen estos cuatro aspectos distintivos en sus ciclos de vida, y el rendimiento se adapta a ellos de maneras diferentes:

Every web app has four distinct aspects to its life cycle, and performance fits into them in different ways:

<figure>
  <img src="images/rail.png"
    alt="The 4 parts of the RAIL performance model: Response, Animation, Idle, and Load."/>
  <figcaption>
    <b>Figure 1</b>. The 4 parts of the RAIL performance model
  </figcaption>
</figure>

## Foco en el usuario

Haz de los usuarios el punto de foco de tu esfuerzo de rendimiento. La mayor parte del tiempo que los usuarios pasan en tu sitio no se destina a esperar que cargue, sino a esperar que responda mientras lo usan. Comprende cómo perciben los usuarios las demoras de rendimiento:

* Enfócate en el usuario, el objetivo final no es hacer que tu sitio funcione rápidamente en un dispositivo específico, sino que es hacer a los usuarios felices.
* Responde a los usuarios inmediatamente, reconoce la entrada del usuario en menos de 100 ms.

## Respuesta: responde en menos de 100 ms

Tienes 100 ms para responder a la entrada del usuario antes de que note una demora. Esto se aplica a la mayoría de las entradas, como hacer clic en botones, alternar controles de formularios o iniciar animaciones. Esto no se aplica a arrastrar o desplazar en pantalla táctil.

<table class="responsive">
  <thead>
      <th colspan="2">Demora y reacción del usuario</th>
  </thead>
  <tbody>
    <tr>
      <td data-th="Delay">0 a 16 ms</td>
      <td data-th="User Reaction">Las personas son excepcionalmente buenas para rastrear
      el movimiento, y les desagrada que las animaciones no sean fluidas. Los usuarios
      perciben las animaciones como fluidas, siempre y cuando se representen 60 nuevos fotogramas
      por segundo. Es decir, 16 ms por fotograma, incluido el tiempo que lleva que
      el navegador pinte el nuevo fotograma de la pantalla, dejándole a una app
      alrededor de 10 ms para producir un fotograma.</td>
    </tr>
    <tr>
      <td data-th="Delay">0 a 100 ms</td>
      <td data-th="User Reaction">Responde a una acción de usuario dentro de esta ventana de tiempo y los usuarios sentirán que el resultado es inmediato. Si demoras más tiempo, la conexión entre acción y reacción se romperá.</td>
    </tr>
    <tr>
      <td data-th="Delay">100 a 300 ms</td>
      <td data-th="User Reaction">Los usuarios experimentan una leve demora perceptible.</td>
    </tr>
    <tr>
      <td data-th="Delay">300 a 1000 ms</td>
      <td data-th="User Reaction">En esta ventana, todo parece parte de una progresión natural y continua de tareas. Para la mayoría de los usuarios de la Web, cargar páginas o cambiar las vistas constituye una tarea.</td>
    </tr>
    <tr>
      <td data-th="Delay">Más de 1000 ms</td>
      <td data-th="User Reaction">Después de 1 segundo, el usuario pierde el foco en la tarea que está realizando.</td>
    </tr>
    <tr>
      <td data-th="Delay">Más de 10 000 ms</td>
      <td data-th="User Reaction">El usuario está frustrado y probablemente abandone la tarea, puede ser que vuelva o no.</td>
    </tr>
  </tbody>
</table>

Si no respondes, la conexión entre acción y reacción se romperá. Los usuarios lo notarán.

## Animación: produce un fotograma en 10 ms

A pesar de que puede resultar obvio responder a las acciones del usuario inmediatamente, esa no siempre es la decisión adecuada. Usa esta ventana de 100 ms para hacer otros trabajos costosos, pero ten cuidado de no bloquear al usuario. Si es posible, trabaja en segundo plano.

Para acciones que llevan más que 500 ms en completarse, siempre brinda comentarios.

* Process user input events within 50ms to ensure a visible response within 100ms, otherwise the connection between action and reaction is broken. This applies to most inputs, such as clicking buttons, toggling form controls, or starting animations. This does not apply to touch drags or scrolls.
* Though it may sound counterintuitive, it's not always the right call to respond to user input immediately. You can use this 100ms window to do other expensive work. But be careful not to block the user. If possible, do work in the background.
* For actions that take longer than 50ms to complete, always provide feedback.

**50ms or 100ms?:**

Los usuarios notan cuando varía el índice de fotogramas de animación. Tu objetivo es producir 60 fotogramas por segundo y cada fotograma tiene que pasar por todos estos pasos:

<figure>
  <img src="images/rail-response-details.png"
    alt="Diagram showing how input received during an idle task is queued,
         reducing available input processing time to 50ms."/>
  <figcaption>
    <b>Figure 2</b>. How idle tasks affect input response budget.
  </figcaption>
</figure>

## Inhabilitado: maximiza el tiempo inhabilitado

**Goals**:

* Produce each frame in an animation in 10ms or less. Technically, the maximum budget for each frame is 16ms (1000ms / 60 frames per second ≈ 16ms), but browsers need about 6ms to render each frame, hence the guideline of 10ms per frame.
* Aim for visual smoothness. Users notice when frame rates vary.

Desde un punto de vista puramente matemático, todos los fotogramas tienen un plazo de alrededor de 16 ms (1000 ms/60 fotogramas por segundo = 16,66 ms por fotograma). Sin embargo, ya que los navegadores necesitan tiempo para pintar el nuevo fotograma en la pantalla, **tu código debería terminar de ejecutarse en menos de 10 ms**.

* In high pressure points like animations, the key is to do nothing where you can, and the absolute minimum where you can't. Whenever possible, make use of the 100ms response to pre-calculate expensive work so that you maximize your chances of hitting 60fps.
* See [Rendering Performance](/web/fundamentals/performance/rendering/) for various animation optimization strategies.
* Recognize all the types of animations. Animations aren't just fancy UI effects. Each of these interactions are considered animations: 
    * Visual animations, such as entrances and exits, [tweens](https://www.webopedia.com/TERM/T/tweening.html), and loading indicators.
    * Scrolling. This includes flinging, which is when the user starts scrolling, then lets go, and the page continues scrolling.
    * Dragging. Animations often follow user interactions, such as panning a map or pinching to zoom.

## Carga: brinda contenido en menos de 1000 ms

En puntos de presión altos como animaciones, la clave es no hacer nada donde puedes y lo mínimo donde no puedes. Cuando sea posible, usa la respuesta de 100 ms para calcular por adelantado el trabajo largo para maximizar tus posibilidades de llegar a 60 fps.

Para obtener más información, consulta [Rendimiento de la representación](/web/fundamentals/performance/rendering/).

* Use idle time to complete deferred work. For example, for the initial page load, load as little data as possible, then use idle time to load the rest.
* Perform work during idle time in 50ms or less. Any longer, and you risk interfering with the app's ability to respond to user input within 50ms.
* If a user interacts with a page during idle time work, the user interaction should always take the highest priority and interrupt the idle time work.

## Resumen de métricas clave de RAIL

Usa el tiempo inhabilitado para completar el trabajo pospuesto. Por ejemplo, mantén al mínimo los datos cargados previamente para que tu app se cargue rápido y usa el tiempo inhabilitado para cargar los datos restantes.

El trabajo pospuesto se debería agrupar en bloques de alrededor de 50 ms. Si un usuario comienza a interactuar, la principal prioridad es responder a eso.

* Optimize for fast loading performance relative to the device and network capabilities that your users use to access your site. Currently, a good target for first loads is to load the page and be [interactive](/web/tools/lighthouse/audits/time-to-interactive) in 5 seconds or less on mid-range mobile devices with slow 3G connections. See [Can You Afford It? Real-World Web Performance Budgets](https://infrequently.org/2017/10/can-you-afford-it-real-world-web-performance-budgets/). But be aware that these targets may change over time.
* For subsequent loads, a good target is to load the page in under 2 seconds. But this target may also change over time.

<figure>
  <img src="images/speed-metrics.png"
    alt="Each loading metric (First Paint, First Contentful Paint, First Meaningful Paint, Time
         To Interactive) represents a different phase of the user's perception of the loading
         experience"/>
  <figcaption>
    <b>Figure 3</b>. Each loading metric represents a different phase of the user's perception of
    the loading experience
  </figcaption>
</figure>

Para permitir una <respuesta de 100 ms, la app tiene que darle el control de vuelta a la cadena principal cada <50 ms, para que pueda ejecutar su canalización de píxeles, reaccionar a la entrada del

* Test your load performance on the mobile devices and network connections that are common among your users. If your business has information on what devices and network connections your users are on, then you can use that combination and set your own loading performance targets. Otherwise, [The Mobile Economy 2017](https://www.gsma.com/mobileeconomy/) suggests that a good global baseline is a mid-range Android phone, such as a Moto G4, and a slow 3G network, defined as 400ms RTT and 400kbps transfer speed. This combination is available on [WebPageTest](https://www.webpagetest.org/easy).
* Keep in mind that although your typical mobile user's device might claim that it's on a 2G, 3G, or 4G connection, in reality the *effective connection speed* is often significantly slower, due to packet loss and network variance.
* Focus on optimizing the [Critical Rendering Path](/web/fundamentals/performance/critical-rendering-path/) to unblock rendering.
* You don't have to load everything in under 5 seconds to produce the perception of a complete load. Enable progressive rendering and do some work in the background. Defer non-essential loads to periods of idle time. See [Website Performance Optimization](https://www.udacity.com/course/website-performance-optimization--ud884).
* Recognize the factors that affect page load performance: 
    * Network speed and latency
    * Hardware (slower CPUs, for example)
    * Cache eviction
    * Differences in L2/L3 caching
    * Parsing JavaScript

## Tools for measuring RAIL {: #tools }

Trabajar en bloques de 50 ms permite que se termine la tarea mientras garantiza una respuesta instantánea.

* [**Chrome DevTools**](#devtools). The developer tools built into Google Chrome. Provides in-depth analysis on everything that happens while your page loads or runs.
* [**Lighthouse**](#lighthouse). Available in Chrome DevTools, as a Chrome Extension, as a Node.js module, and within WebPageTest. You give it a URL, it simulates a mid-range device with a slow 3G connection, runs a series of audits on the page, and then gives you a report on load performance, as well as suggestions on how to improve. Also provides audits to improve accessibility, make the page easier to maintain, qualify as a Progressive Web App, and more.
* [**WebPageTest**](#webpagetest). Available at [webpagetest.org/easy](https://webpagetest.org/easy). You give it a URL, it loads the page on a real Moto G4 device with a slow 3G connection, and then gives you a detailed report on the page's load performance. You can also configure it to include a Lighthouse audit.

Carga tu sitio en menos de 1 segundo. Si no lo haces, la atención del usuario se dispersa y su percepción de lidiar con la tarea se quiebra.

### TL;DR {: .hide-from-toc }

Enfócate en [optimizar la ruta de acceso de representación](/web/fundamentals/performance/critical-rendering-path/) para desbloquear la representación.

No es necesario que cargues todo en menos de 1 segundo para producir la percepción de una carga completa. Habilita la representación progresiva y realiza parte del trabajo en segundo plano. Pospon las cargas que no son esenciales a periodos de tiempo inhabilitado (consulta este [curso de Udacity sobre Optimización de rendimiento de sitio web](https://www.udacity.com/course/website-performance-optimization--ud884) para obtener más información).

* [Throttle your CPU](/web/tools/chrome-devtools/evaluate-performance/reference#cpu-throttle) to simulate a less-powerful device.
* [Throttle the network](/web/tools/chrome-devtools/evaluate-performance/reference#network-throttle) to simulate slower connections.
* [View main thread activity](/web/tools/chrome-devtools/evaluate-performance/reference#main) to view every event that occurred on the main thread while you were recording.
* [View main thread activities in a table](/web/tools/chrome-devtools/evaluate-performance/reference#activities) to sort activities based on which ones took up the most time.
* [Analyze frames per second (FPS)](/web/tools/chrome-devtools/evaluate-performance/reference#fps) to measure whether your animations truly run smoothly.
* [Monitor CPU usage, JS heap size, DOM nodes, layouts per second, and more](/web/updates/2017/11/devtools-release-notes#perf-monitor) in real-time with the **Performance Monitor**.
* [Visualize network requests](/web/tools/chrome-devtools/evaluate-performance/reference#network) that occurred while you were recording with the **Network** section.
* [Capture screenshots while recording](/web/tools/chrome-devtools/evaluate-performance/reference#screenshots) to play back exactly how the page looked while the page loaded, or an animation fired, and so on.
* [View interactions](/web/tools/chrome-devtools/evaluate-performance/reference#interactions) to quickly identify what happened on a page after a user interacted with it.
* [Find scroll performance issues in real-time](/web/tools/chrome-devtools/evaluate-performance/reference#scrolling-performance-issues) by highlighting the page whenever a potentially problematic listener fires.
* [View paint events in real-time](/web/tools/chrome-devtools/evaluate-performance/reference#paint-flashing) to identify costly paint events that may be harming the performance of your animations.

### Lighthouse {: #lighthouse }

Para evaluar tu sitio en relación con las métricas de RAIL, usa la [herramienta Timeline](/web/tools/chrome-devtools/profile/evaluate-performance/timeline-tool) de DevTools de Chrome para registrar las acciones del usuario. Luego controla los tiempos de registro en Timeline con estas métricas claves de RAIL:

<figure>
  <img src="images/lighthouse-performance.jpg"
    alt="An example Lighthouse report"/>
  <figcaption>
    <b>Figure 4</b>. An example Lighthouse report
  </figcaption>
</figure>

The following audits are especially relevant:

* **Response** 
    * [Estimated Input Latency](/web/tools/lighthouse/audits/estimated-input-latency). Estimates how long your app will take to respond to user input, based on main thread idle time.
    * [Uses Passive Event Listeners To Improve Scrolling](/web/tools/lighthouse/audits/passive-event-listeners).
* **Load** 
    * [Registers A Service Worker](/web/tools/lighthouse/audits/registered-service-worker). A service worker can cache common resources on a user's device, reducing time spent fetching resources over the network.
    * [Page Load Is Fast Enough On 3G](/web/tools/lighthouse/audits/fast-3g).
    * [First Meaningful Paint](/web/tools/lighthouse/audits/first-meaningful-paint). Measures when the page appears meaningfully complete.
    * [First CPU Idle](/web/tools/lighthouse/audits/first-interactive). Marks the first time at which the page's main thread is quiet enough to handle input.
    * [Time To Interactive](/web/tools/lighthouse/audits/consistently-interactive). Measures when a user can consistently interact with all page elements.
    * [Perceptual Speed Index](/web/tools/lighthouse/audits/speed-index).
    * [Reduce Render-Blocking Resources](/web/tools/lighthouse/audits/blocking-resources).
    * [Offscreen Images](/web/tools/lighthouse/audits/offscreen-images). Defer the loading of offscreen images until they're needed.
    * [Properly Size Images](/web/tools/lighthouse/audits/oversized-images). Don't serve images that are significantly larger than the size that's rendered in the mobile viewport.
    * [Critical Request Chains](/web/tools/lighthouse/audits/critical-request-chains). Visualize your [Critical Rendering Path](/web/fundamentals/performance/critical-rendering-path/).
    * [Uses HTTP/2](/web/tools/lighthouse/audits/http2).
    * [Optimize Images](/web/tools/lighthouse/audits/optimize-images).
    * [Enable Text Compression](/web/tools/lighthouse/audits/text-compression).
    * [Avoid Enormous Network Payloads](/web/tools/lighthouse/audits/network-payloads).
    * [Uses An Excessive DOM Size](/web/tools/lighthouse/audits/dom-size). Reduce network bytes by only shipping DOM nodes that are needed for rendering the page.

### WebPageTest {: #webpagetest }

{# wf_devsite_translation #}

<figure>
  <img src="images/wpt-report.png"
    alt="An example WebPageTest report"/>
  <figcaption>
    <b>Figure 5</b>. An example WebPageTest report
  </figcaption>
</figure>

## Summary {: #summary }

RAIL is a lens for looking at a website's user experience as a journey composed of distinct interactions. Understand how users perceive your site in order to set performance goals with the greatest impact on user experience.

* **Focus on the user**.
* **Respond to user input in under 100ms**.
* **Produce a frame in under 10ms when animating or scrolling**.
* **Maximize main thread idle time**.
* **Load interactive content in under 5000ms**.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}