project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description:RAIL 是一种以用户为中心的性能模型。每个网络应用均具有与其生命周期有关的四个不同方面，且这些方面以非常不同的方式影响着性能：响应 (Response)、动画 (Animation)、空闲 (Idle) 和加载 (Load)。

{# wf_updated_on:2015-06-07 #} {# wf_published_on:2015-06-07 #}

# 使用 RAIL 模型评估性能 {: .page-title }

{% include "web/_shared/contributors/megginkearney.html" %}

RAIL 是一种以用户为中心的性能模型。每个网络应用均具有与其生命周期有关的四个不同方面，且这些方面以不同的方式影响着性能：

Every web app has four distinct aspects to its life cycle, and performance fits into them in different ways:

<figure>
  <img src="images/rail.png"
    alt="The 4 parts of the RAIL performance model: Response, Animation, Idle, and Load."/>
  <figcaption>
    <b>Figure 1</b>. The 4 parts of the RAIL performance model
  </figcaption>
</figure>

## 以用户为中心

让用户成为您的性能工作的中心。用户花在网站上的大多数时间不是等待加载，而是在使用时等待响应。了解用户如何评价性能延迟：

* 以用户为中心；最终目标不是让您的网站在任何特定设备上都能运行很快，而是使用户满意。
* 立即响应用户；在 100 毫秒以内确认用户输入。

## 响应：在 100 毫秒以内响应

在用户注意到滞后之前您有 100 毫秒的时间可以响应用户输入。这适用于大多数输入，不管他们是在点击按钮、切换表单控件还是启动动画。但不适用于触摸拖动或滚动。

<table class="responsive">
  <th colspan="2">
    延迟与用户反应
  </th>
  
  <tr>
    <td data-th="Delay">
      0 - 16 毫秒
    </td>
    
    <td data-th="User Reaction">
      人们特别擅长跟踪运动，如果动画不流畅，他们就会对运动心生反感。 用户可以感知每秒渲染 60 帧的平滑动画转场。也就是每帧 16 毫秒（包括浏览器将新帧绘制到屏幕上所需的时间），留给应用大约 10 毫秒的时间来生成一帧。
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      0 - 100 毫秒
    </td>
    
    <td data-th="User Reaction">
      在此时间窗口内响应用户操作，他们会觉得可以立即获得结果。时间再长，操作与反应之间的连接就会中断。
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      100 - 300 毫秒
    </td>
    
    <td data-th="User Reaction">
      用户会遇到轻微可觉察的延迟。
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      300 - 1000 毫秒
    </td>
    
    <td data-th="User Reaction">
      在此窗口内，延迟感觉像是任务自然和持续发展的一部分。对于网络上的大多数用户，加载页面或更改视图代表着一个任务。
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      1000+ 毫秒
    </td>
    
    <td data-th="User Reaction">
      超过 1 秒，用户的注意力将离开他们正在执行的任务。
    </td>
  </tr>
  
  <tr>
    <td data-th="Delay">
      10,000+ 毫秒
    </td>
    
    <td data-th="User Reaction">
      用户感到失望，可能会放弃任务；之后他们或许不会再回来。
    </td>
  </tr>
</table>

如果您未响应，操作与反应之间的连接就会中断。用户会注意到。

## 动画：在 10 毫秒内生成一帧

尽管很明显应立即响应用户的操作，但这并不总是正确的做法。使用此 100 毫秒窗口执行其他开销大的工作，但需要谨慎，以免妨碍用户。如果可能，请在后台执行工作。

对于需要超过 500 毫秒才能完成的操作，请始终提供反馈。

* Process user input events within 50ms to ensure a visible response within 100ms, otherwise the connection between action and reaction is broken. This applies to most inputs, such as clicking buttons, toggling form controls, or starting animations. This does not apply to touch drags or scrolls.
* Though it may sound counterintuitive, it's not always the right call to respond to user input immediately. You can use this 100ms window to do other expensive work. But be careful not to block the user. If possible, do work in the background.
* For actions that take longer than 50ms to complete, always provide feedback.

**50ms or 100ms?:**

如果动画帧率发生变化，您的用户确实会注意到。您的目标就是每秒生成 60 帧，每一帧必须完成以下所有步骤：

<figure>
  <img src="images/rail-response-details.png"
    alt="Diagram showing how input received during an idle task is queued,
         reducing available input processing time to 50ms."/>
  <figcaption>
    <b>Figure 2</b>. How idle tasks affect input response budget.
  </figcaption>
</figure>

## 空闲：最大程度增加空闲时间

**Goals**:

* Produce each frame in an animation in 10ms or less. Technically, the maximum budget for each frame is 16ms (1000ms / 60 frames per second ≈ 16ms), but browsers need about 6ms to render each frame, hence the guideline of 10ms per frame.
* Aim for visual smoothness. Users notice when frame rates vary.

从纯粹的数学角度而言，每帧的预算约为 16 毫秒（1000 毫秒 / 60 帧 = 16.66 毫秒/帧）。 但因为浏览器需要花费时间将新帧绘制到屏幕上，**只有 10 毫秒来执行代码**。

* In high pressure points like animations, the key is to do nothing where you can, and the absolute minimum where you can't. Whenever possible, make use of the 100ms response to pre-calculate expensive work so that you maximize your chances of hitting 60fps.
* See [Rendering Performance](/web/fundamentals/performance/rendering/) for various animation optimization strategies.
* Recognize all the types of animations. Animations aren't just fancy UI effects. Each of these interactions are considered animations: 
    * Visual animations, such as entrances and exits, [tweens](https://www.webopedia.com/TERM/T/tweening.html), and loading indicators.
    * Scrolling. This includes flinging, which is when the user starts scrolling, then lets go, and the page continues scrolling.
    * Dragging. Animations often follow user interactions, such as panning a map or pinching to zoom.

## 加载：在 1000 毫秒以内呈现内容

在像动画一样的高压点中，关键是不论能不能做，什么都不要做，做最少的工作。 如果可能，请利用 100 毫秒响应预先计算开销大的工作，这样您就可以尽可能增加实现 60fps 的可能性。

如需了解详细信息，请参阅[渲染性能](/web/fundamentals/performance/rendering/)。

* Use idle time to complete deferred work. For example, for the initial page load, load as little data as possible, then use idle time to load the rest.
* Perform work during idle time in 50ms or less. Any longer, and you risk interfering with the app's ability to respond to user input within 50ms.
* If a user interacts with a page during idle time work, the user interaction should always take the highest priority and interrupt the idle time work.

## 关键 RAIL 指标汇总

利用空闲时间完成推迟的工作。例如，尽可能减少预加载数据，以便您的应用快速加载，并利用空闲时间加载剩余数据。

推迟的工作应分成每个耗时约 50 毫秒的多个块。如果用户开始交互，优先级最高的事项是响应用户。

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

要实现小于 100 毫秒的响应，应用必须在每 50 毫秒内将控制返回给主线程，这样应用就可以执行其像素管道、对用户输入作出反应，等等。

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

以 50 毫秒块工作既可以完成任务，又能确保即时的响应。

* [**Chrome DevTools**](#devtools). The developer tools built into Google Chrome. Provides in-depth analysis on everything that happens while your page loads or runs.
* [**Lighthouse**](#lighthouse). Available in Chrome DevTools, as a Chrome Extension, as a Node.js module, and within WebPageTest. You give it a URL, it simulates a mid-range device with a slow 3G connection, runs a series of audits on the page, and then gives you a report on load performance, as well as suggestions on how to improve. Also provides audits to improve accessibility, make the page easier to maintain, qualify as a Progressive Web App, and more.
* [**WebPageTest**](#webpagetest). Available at [webpagetest.org/easy](https://webpagetest.org/easy). You give it a URL, it loads the page on a real Moto G4 device with a slow 3G connection, and then gives you a detailed report on the page's load performance. You can also configure it to include a Lighthouse audit.

在 1 秒钟内加载您的网站。否则，用户的注意力会分散，他们处理任务的感觉会中断。

### TL;DR {: .hide-from-toc }

侧重于[优化关键渲染路径](/web/fundamentals/performance/critical-rendering-path/)以取消阻止渲染。

您无需在 1 秒内加载所有内容以产生完整加载的感觉。启用渐进式渲染和在后台执行一些工作。将非必需的加载推迟到空闲时间段（请参阅此[网站性能优化 Udacity 课程](https://www.udacity.com/course/website-performance-optimization--ud884)，了解更多信息）。

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

要根据 RAIL 指标评估您的网站，请使用 Chrome DevTools [Timeline 工具](/web/tools/chrome-devtools/profile/evaluate-performance/timeline-tool)记录用户操作。然后根据这些关键 RAIL 指标检查 Timeline 中的记录时间。

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