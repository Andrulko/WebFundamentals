project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: RAIL은 사용자 중심 성능 모델입니다. 모든 웹 앱은 수명 주기에 뚜렷한 네 가지 측면, 즉 응답, 애니메이션, 유휴, 로드가 있으며, 여기에 성능이 각기 다른 방식으로 적용됩니다.

{# wf_updated_on: 2015-06-07 #} {# wf_published_on: 2015-06-07 #}

# RAIL 모델로 성능 측정 {: .page-title }

{% include "web/_shared/contributors/megginkearney.html" %}

RAIL은 사용자 중심 성능 모델입니다. 모든 웹 앱은 수명 주기에 다음과 같은 뚜렷한 네 가지 측면이 있으며, 여기에 성능이 각기 다른 방식으로 적용됩니다.

Every web app has four distinct aspects to its life cycle, and performance fits into them in different ways:

<figure>
  <img src="images/rail.png"
    alt="The 4 parts of the RAIL performance model: Response, Animation, Idle, and Load."/>
  <figcaption>
    <b>Figure 1</b>. The 4 parts of the RAIL performance model
  </figcaption>
</figure>

## 사용자에게 주안점 두기

성능 개선 노력의 초점은 사용자에게 맞추어야 합니다. 사용자가 사이트에서 보내는 시간의 대부분은 페이지가 로드되기를 기다리는 시간이 아니라 사용 중 응답을 대기하는 시간입니다. 사용자가 성능 지연을 어떻게 받아들이는지 제대로 파악해야 합니다.

* 사용자에게 주안점을 두세요. 최종 목표는 사이트가 기기 유형을 불문하고 어디서나 빠른 성능을 발휘하는 것이 아닙니다. 궁극적으로 사용자를 만족시키는 것이 중요합니다.
* 사용자에게 즉각적으로 반응하세요. 사용자 입력은 100ms 내에 인지해야 합니다.

## 응답: 100ms 이내에 응답

사용자 입력에 100ms 이내에 응답하지 않으면 지연을 느낍니다. 이는 버튼 클릭, 양식 컨트롤 전환, 애니메이션 시작과 같은 대부분의 입력에 적용됩니다. 이것은 터치 드래그나 스크롤에는 적용되지 않습니다.

<table class="responsive">
  <th colspan="2">
    지연 및 사용자 반응
  </th>
  
  <tr>
    <td data-th="Delay">
      0 - 16ms
    </td>
    
    <td data-th="User Reaction">
      사람들은 움직임을 뒤쫓는 재주가 특히 탁월하며, 부드럽지 못한 애니메이션을 싫어합니다. 초당 60개의 새 프레임으로 렌더링되면 사용자는 애니메이션을 부드럽다고 인식합니다. 이것은 프레임당 16ms에 해당하고 여기에는 브라우저가 새 프레임을 화면에 그리는 시간이 포함되며, 약 10ms는 앱이 프레임을 생성하는 시간으로 남겨둡니다.
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      0 - 100ms
    </td>
    
    <td data-th="User Reaction">
      이 시간 범위 내에 사용자 동작에 응답하면 결과가 신속하다는 느낌을 사용자가 받게 됩니다. 이보다 더 오래 걸리면 동작과 반응 사이의 연결이 끊어집니다.
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      100 - 300ms
    </td>
    
    <td data-th="User Reaction">
      사용자가 약간의 지연을 인지할 수 있습니다.
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      300 - 1000ms
    </td>
    
    <td data-th="User Reaction">
      이 시간 범위 내의 지연은 작업이 자연스럽고 지속적으로 이어지는 과정의 일부분으로 느껴집니다. 웹 상의 대부분의 사용자에게 있어 페이지 로드와 뷰 변경이 하나의 작업으로 보입니다.
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      1000ms 이상
    </td>
    
    <td data-th="User Reaction">
      1초가 넘어가면 사용자가 자신이 수행 중인 작업에서 집중력을 잃게 됩니다.
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      10,000ms 이상
    </td>
    
    <td data-th="User Reaction">
      사용자가 짜증을 내고 작업을 중도 포기할 가능성이 커지며, 나중에 다시 돌아오지 않을 수도 있습니다.
    </td>
  </tr>
</table>

여러분이 응답하지 않으면 동작과 반응 사이의 연결이 끊어집니다. 사용자는 이런 현상을 잘 알아챕니다.

## 애니메이션: 10ms 이내에 프레임 생성

사용자의 작업에 즉시 응답하는 것이 명백한 정답으로 보일 수 있지만, 이것이 늘 옳은 방안은 아닙니다. 이 100ms의 시간 범위를 다른 고비용 작업에 사용해도 좋지만, 사용자를 가로막지 않도록 주의하세요. 가능하면 백그라운드에서 작업을 수행하세요.

완료하는 데 500ms 이상이 걸리는 작업의 경우, 항상 피드백을 제공하세요.

* Process user input events within 50ms to ensure a visible response within 100ms, otherwise the connection between action and reaction is broken. This applies to most inputs, such as clicking buttons, toggling form controls, or starting animations. This does not apply to touch drags or scrolls.
* Though it may sound counterintuitive, it's not always the right call to respond to user input immediately. You can use this 100ms window to do other expensive work. But be careful not to block the user. If possible, do work in the background.
* For actions that take longer than 50ms to complete, always provide feedback.

**50ms or 100ms?:**

애니메이션 프레임 속도가 다르면 사용자가 눈치챕니다. 목표는 초당 60프레임을 생성하는 것이고, 모든 프레임이 다음 단계를 거쳐야 합니다.

<figure>
  <img src="images/rail-response-details.png"
    alt="Diagram showing how input received during an idle task is queued,
         reducing available input processing time to 50ms."/>
  <figcaption>
    <b>Figure 2</b>. How idle tasks affect input response budget.
  </figcaption>
</figure>

## 유휴: 유휴 시간 극대화

**Goals**:

* Produce each frame in an animation in 10ms or less. Technically, the maximum budget for each frame is 16ms (1000ms / 60 frames per second ≈ 16ms), but browsers need about 6ms to render each frame, hence the guideline of 10ms per frame.
* Aim for visual smoothness. Users notice when frame rates vary.

순전히 수학적인 측면에서 보자면, 모든 프레임에는 약 16ms의 시간이 할당됩니다(1000ms / 초당 60프레임 = 프레임당 16.66ms). 그러나 브라우저가 새 프레임을 화면에 그리는 시간이 필요하므로 **코드는 10ms 이내에 실행이 끝나야 합니다**.

* In high pressure points like animations, the key is to do nothing where you can, and the absolute minimum where you can't. Whenever possible, make use of the 100ms response to pre-calculate expensive work so that you maximize your chances of hitting 60fps.
* See [Rendering Performance](/web/fundamentals/performance/rendering/) for various animation optimization strategies.
* Recognize all the types of animations. Animations aren't just fancy UI effects. Each of these interactions are considered animations: 
    * Visual animations, such as entrances and exits, [tweens](https://www.webopedia.com/TERM/T/tweening.html), and loading indicators.
    * Scrolling. This includes flinging, which is when the user starts scrolling, then lets go, and the page continues scrolling.
    * Dragging. Animations often follow user interactions, such as panning a map or pinching to zoom.

## 로드: 콘텐츠를 1000ms 이내에 전달

애니메이션과 같이 압박이 심한 시점에서 핵심은 가능하면 아무 작업도 하지 않는 것이며 아니면 최소한의 작업만 수행하는 것입니다. 가능하면 항상 100ms의 응답 시한을 활용하여 고비용 작업을 사전에 계산하는 것이 좋습니다. 그러면 60fps의 목표를 달성할 가능성이 극대화됩니다.

자세한 내용은 [렌더링 성능](/web/fundamentals/performance/rendering/)을 참조하세요.

* Use idle time to complete deferred work. For example, for the initial page load, load as little data as possible, then use idle time to load the rest.
* Perform work during idle time in 50ms or less. Any longer, and you risk interfering with the app's ability to respond to user input within 50ms.
* If a user interacts with a page during idle time work, the user interaction should always take the highest priority and interrupt the idle time work.

## 주요 RAIL 지표 요약

유휴 시간을 활용하여 지연된 작업을 완료합니다. 예를 들어, 미리 로드하는 데이터를 최소한으로 유지하면 앱이 빠르게 로드되며, 유휴 시간을 활용하여 남은 데이터를 로드하세요.

지연된 작업은 약 50ms 단위의 블록으로 그룹화해야 합니다. 사용자가 상호작용을 시작하면 우선순위가 가장 높은 것이 이에 응답합니다.

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

100ms 미만의 응답을 허용하려면 앱이 50ms 미만의 간격마다 메인 스레드에게 통제권을 양보해야 합니다. 그래야만 픽셀 파이프라인을 실행하고 사용자 입력에 반응하는 등의 여러 작업을 수행할 수 있습니다.

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

50ms 블록 단위로 나누면 작업을 완료하면서도 즉각적인 응답을 보장할 수 있습니다.

* [**Chrome DevTools**](#devtools). The developer tools built into Google Chrome. Provides in-depth analysis on everything that happens while your page loads or runs.
* [**Lighthouse**](#lighthouse). Available in Chrome DevTools, as a Chrome Extension, as a Node.js module, and within WebPageTest. You give it a URL, it simulates a mid-range device with a slow 3G connection, runs a series of audits on the page, and then gives you a report on load performance, as well as suggestions on how to improve. Also provides audits to improve accessibility, make the page easier to maintain, qualify as a Progressive Web App, and more.
* [**WebPageTest**](#webpagetest). Available at [webpagetest.org/easy](https://webpagetest.org/easy). You give it a URL, it loads the page on a real Moto G4 device with a slow 3G connection, and then gives you a detailed report on the page's load performance. You can also configure it to include a Lighthouse audit.

사이트를 1초 이내에 로드하세요. 그렇지 않으면 사용자의 주의가 흐트러지고 작업에 대한 집중도가 산만해집니다.

### TL;DR {: .hide-from-toc }

무엇보다 [중요 렌더링 경로 최적화](/web/fundamentals/performance/critical-rendering-path/)에 집중하여 렌더링 차단을 해제하세요.

완전히 로드되었다는 인상을 주기 위해 모든 것을 1초 이내에 로드할 필요는 없습니다. 프로그레시브 렌더링을 활성화하고 백그라운드에서 다른 작업을 수행합니다. 필수가 아닌 로드는 유휴 시간으로 미룹니다(자세한 내용은 이 [웹사이트 성능 최적화 Udacity 과정](https://www.udacity.com/course/website-performance-optimization--ud884)을 참조하세요).

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

RAIL 지표에 따라 사이트를 평가하려면 Chrome DevTools [타임라인 도구](/web/tools/chrome-devtools/profile/evaluate-performance/timeline-tool)를 사용하여 사용자 작업을 기록합니다. 그런 다음, 다음과 같은 주요 RAIL 지표에 따라 타임라인에서 기록 시간을 점검합니다.

<figure>
  <img src="images/lighthouse-performance.jpg"
    alt="An example Lighthouse report"/>
  <figcaption>
    <b>Figure 4</b>. An example Lighthouse report
  </figcaption>
</figure>

{# wf_devsite_translation #}

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

RAIL is a lens for looking at a website's user experience as a journey composed of distinct interactions. Understand how users perceive your site in order to set performance goals with the greatest impact on user experience.

* **Focus on the user**.
* **Respond to user input in under 100ms**.
* **Produce a frame in under 10ms when animating or scrolling**.
* **Maximize main thread idle time**.
* **Load interactive content in under 5000ms**.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}