project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: O RAIL é um modelo de desempenho centrado no usuário. Todo app da Web tem estes quatro aspectos distintos no seu ciclo de vida, e o desempenho se relaciona com eles de maneiras muito diferentes: Resposta, animação, ociosidade e carregamento.

{# wf_updated_on: 2015-06-07 #} {# wf_published_on: 2015-06-07 #}

# Medição do desempenho com o modelo RAIL {: .page-title }

{% include "web/_shared/translation-out-of-date.html" %}

{% include "web/_shared/contributors/megginkearney.html" %}

O RAIL é um modelo de desempenho centrado no usuário. Todo aplicativo web tem estes quatro aspectos distintos no seu ciclo de vida, e o desempenho se relaciona com eles de maneiras muito diferentes:

<figure>
  <img src="images/rail.png"
    alt="The 4 parts of the RAIL performance model: Response, Animation, Idle, and Load."/>
  <figcaption>
    <b>Figure 1</b>. The 4 parts of the RAIL performance model
  </figcaption>
</figure>

## Foco no usuário

In the context of RAIL, the terms **goals** and **guidelines** have specific meanings:

* Foco no usuário, o objetivo final não é fazer o site ser rápido em dispositivos específicos, é deixar os usuários satisfeitos.
* Responder aos usuários imediatamente. Reconhecer o comando do usuário em menos de 100 ms.

## Resposta: responda em menos de 100 ms

Torne os usuários o ponto central das suas iniciativas de desempenho. A maior parte do tempo que os usuários gastam no seu site não é esperando-o carregar, mas esperando a resposta enquanto o usam. Saiba como os usuários percebem a lentidão:

<table class="responsive">
  <thead>
      <th colspan="2">Lentidão &amp; Reação do usuário</th>
  </thead>
  <tbody>
    <tr>
      <td data-th="Delay">0–16 ms</td>
      <td data-th="User Reaction">As pessoas são muito boas em acompanhar
      movimentos e não gostam quando as animações não são fluidas. Os usuários
      consideram animações fluidas desde que 60 novos quadros sejam renderizados
      por segundo. Isso significa 16 ms por quadro, incluindo o tempo necessário para
      o navegador exibir o novo quadro na tela, deixando o aplicativo
      com cerca de 10 ms para produzi-lo.</td>
    </tr>
    <tr>
      <td data-th="Delay">0–100 ms</td>
      <td data-th="User Reaction">Responda a uma ação do usuário dentro dessa janela de tempo e os usuários sentirão que a resposta foi imediata. Qualquer tempo a mais faz com que a conexão entre ação e reação seja rompida.</td>
    </tr>
    <tr>
      <td data-th="Delay">100–300 ms</td>
      <td data-th="User Reaction">Os usuários notam um atraso leve.</td>
    </tr>
    <tr>
      <td data-th="Delay">300–1000 ms</td>
      <td data-th="User Reaction">Nesta janela, as coisas são percebidas como parte de uma progressão natural e contínua de tarefas. Para a maioria dos usuários da web, carregar páginas ou trocar vistas representa uma tarefa.</td>
    </tr>
    <tr>
      <td data-th="Delay">Mais de 1000 ms</td>
      <td data-th="User Reaction">Após 1 segundo, o usuário perde o foco na tarefa que está realizando.</td>
    </tr>
    <tr>
      <td data-th="Delay">Mais de 10.000 ms</td>
      <td data-th="User Reaction">O usuário fica frustrado e provavelmente abandona a tarefa. Pode ou não retornar no futuro.</td>
    </tr>
  </tbody>
</table>

Você tem 100 ms para responder à interação do usuário antes de ele perceber uma lentidão. Isso se aplica à maioria das interações, como clicar em botões, ativar ou desativar controles de formulário ou iniciar animações. Essa regra não se aplica a rolagem de toque ou convencional.

## Animação: produza um quadro em 10ms

Se você não responder, a conexão entre ação e reação será rompida. Os usuários percebem.

Embora pareça óbvio que responder às ações do usuário de forma imediata é o ideal, nem sempre é a decisão mais certa. Use essa janela de 100 ms para fazer outros trabalhos pesados, mas tome cuidado para não parar o usuário. Se possível, faça o trabalho em segundo plano.

* Process user input events within 50ms to ensure a visible response within 100ms, otherwise the connection between action and reaction is broken. This applies to most inputs, such as clicking buttons, toggling form controls, or starting animations. This does not apply to touch drags or scrolls.
* Though it may sound counterintuitive, it's not always the right call to respond to user input immediately. You can use this 100ms window to do other expensive work. But be careful not to block the user. If possible, do work in the background.
* For actions that take longer than 50ms to complete, always provide feedback.

**50ms or 100ms?:**

As animações não são simplesmente efeitos de IU sofisticados. Por exemplo, rolagem e rolagem de toque são tipos de animação.

<figure>
  <img src="images/rail-response-details.png"
    alt="Diagram showing how input received during an idle task is queued,
         reducing available input processing time to 50ms."/>
  <figcaption>
    <b>Figure 2</b>. How idle tasks affect input response budget.
  </figcaption>
</figure>

## Ociosidade: maximize o tempo de ociosidade

Os usuários percebem quando a taxa de quadros da animação varia. Seu objetivo é produzir 60 quadros por segundo, e todo quadro tem que passar pelas seguintes etapas:

* Produce each frame in an animation in 10ms or less. Technically, the maximum budget for each frame is 16ms (1000ms / 60 frames per second ≈ 16ms), but browsers need about 6ms to render each frame, hence the guideline of 10ms per frame.
* Aim for visual smoothness. Users notice when frame rates vary.

**Guidelines**:

* In high pressure points like animations, the key is to do nothing where you can, and the absolute minimum where you can't. Whenever possible, make use of the 100ms response to pre-calculate expensive work so that you maximize your chances of hitting 60fps.
* See [Rendering Performance](/web/fundamentals/performance/rendering/) for various animation optimization strategies.
* Recognize all the types of animations. Animations aren't just fancy UI effects. Each of these interactions are considered animations: 
    * Visual animations, such as entrances and exits, [tweens](https://www.webopedia.com/TERM/T/tweening.html), and loading indicators.
    * Scrolling. This includes flinging, which is when the user starts scrolling, then lets go, and the page continues scrolling.
    * Dragging. Animations often follow user interactions, such as panning a map or pinching to zoom.

## Carregamento: entregue conteúdo em menos de 1000 ms

De um ponto de vista puramente matemático, todo quadro tem um "orçamento" de cerca de 16 ms (1000 ms/60 quadros por segundo = 16,66 ms por quadro). Porém, como os navegadores precisam de algum tempo para exibir o novo quadro na tela, **o seu código deve finalizar a execução em até 10 ms**.

Em pontos de alta pressão, como as animações, a chave é não fazer nada onde for possível e absolutamente o mínimo onde não for possível não fazer nada. Sempre que der, use a resposta de 100 ms para pré-calcular trabalhos demorados para maximizar suas chances de atingir 60 fps.

* Use idle time to complete deferred work. For example, for the initial page load, load as little data as possible, then use idle time to load the rest.
* Perform work during idle time in 50ms or less. Any longer, and you risk interfering with the app's ability to respond to user input within 50ms.
* If a user interacts with a page during idle time work, the user interaction should always take the highest priority and interrupt the idle time work.

## Resumo das principais métricas do RAIL

Para saber mais, acesse [Desempenho de renderização](/web/fundamentals/performance/rendering/).

Use o tempo de ociosidade para concluir trabalhos adiados. Por exemplo, mantenha um mínimo de dados pré-carregados para que o aplicativo carregue rapidamente, e use o tempo de ociosidade para carregar os demais dados.

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

Os trabalhos adiados devem ser agrupados em blocos de cerca de 50 ms. Caso um usuário comece a interagir, a prioridade é responder a isso.

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

Para tornar possível uma <resposta de 100 ms, o aplicativo deve devolver o controle ao encadeamento principal a cada <50 ms, de forma que ele possa executar o funil de pixels, reagir à interação do u

* [**Chrome DevTools**](#devtools). The developer tools built into Google Chrome. Provides in-depth analysis on everything that happens while your page loads or runs.
* [**Lighthouse**](#lighthouse). Available in Chrome DevTools, as a Chrome Extension, as a Node.js module, and within WebPageTest. You give it a URL, it simulates a mid-range device with a slow 3G connection, runs a series of audits on the page, and then gives you a report on load performance, as well as suggestions on how to improve. Also provides audits to improve accessibility, make the page easier to maintain, qualify as a Progressive Web App, and more.
* [**WebPageTest**](#webpagetest). Available at [webpagetest.org/easy](https://webpagetest.org/easy). You give it a URL, it loads the page on a real Moto G4 device with a slow 3G connection, and then gives you a detailed report on the page's load performance. You can also configure it to include a Lighthouse audit.

Trabalhar com blocos de 50 ms permite tanto que a tarefa seja finalizada quanto resposta instantânea.

### TL;DR {: .hide-from-toc }

Carregue o site em menos de 1 segundo. Caso contrário, o usuário se dispersa e a percepção dele de lidar com a tarefa é quebrada.

Concentre-se em [otimizar o caminho crítico de renderização](/web/fundamentals/performance/critical-rendering-path/) para desobstruir a renderização.

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

Você não precisa carregar tudo em menos de 1 segundo para produzir a percepção de um carregamento completo. Use a renderização progressiva e faça algumas coisas em segundo plano. Adie carregamentos não essenciais a períodos de tempo ocioso (veja mais sobre isso no [curso de otimização do desempenho de sites do Udacity](https://www.udacity.com/course/website-performance-optimization--ud884)).

<figure>
  <img src="images/lighthouse-performance.jpg"
    alt="An example Lighthouse report"/>
  <figcaption>
    <b>Figure 4</b>. An example Lighthouse report
  </figcaption>
</figure>

Para avaliar o seu site com base nas métricas do RAIL, use a [ferramenta "Timeline"](/web/tools/chrome-devtools/profile/evaluate-performance/timeline-tool) do Chrome DevTools para registrar as ações dos usuários. Em seguida, compare os tempos de registro da "Timeline" com as principais métricas do RAIL:

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

Enter a URL at [webpagetest.org/easy](https://webpagetest.org/easy) to get a report on how that page loads on a real mid-range Android device with a slow 3G connection.

<figure>
  <img src="images/wpt-report.png"
    alt="An example WebPageTest report"/>
  <figcaption>
    <b>Figure 5</b>. An example WebPageTest report
  </figcaption>
</figure>

## Summary {: #summary }

{# wf_devsite_translation #}

* **Focus on the user**.
* **Respond to user input in under 100ms**.
* **Produce a frame in under 10ms when animating or scrolling**.
* **Maximize main thread idle time**.
* **Load interactive content in under 5000ms**.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}