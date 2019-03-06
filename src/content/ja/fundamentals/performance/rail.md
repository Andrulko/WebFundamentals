project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: RAIL はユーザーを中心に考えるパフォーマンス モデルです。すべてのウェブアプリのライフサイクルには 4 つの異なる側面があり、これらの側面に適したパフォーマンスはそれぞれ大きく異なります。その側面とは、レスポンス（Response）、アニメーション（Animation）、アイドル（Idle）、読み込み（Load）の 4 つです。

{# wf_updated_on:2015-06-07 #} {# wf_published_on:2015-06-07 #}

# RAIL モデルでパフォーマンスを計測する {: .page-title }

{% include "web/_shared/contributors/megginkearney.html" %}

RAIL はユーザーを中心に考えるパフォーマンス モデルです。すべてのウェブアプリのライフサイクルには 4 つの異なる側面があり、これらの側面に適したパフォーマンスはそれぞれ異なります。

Every web app has four distinct aspects to its life cycle, and performance fits into them in different ways:

<figure>
  <img src="images/rail.png"
    alt="The 4 parts of the RAIL performance model: Response, Animation, Idle, and Load."/>
  <figcaption>
    <b>Figure 1</b>. The 4 parts of the RAIL performance model
  </figcaption>
</figure>

## ユーザー第一

パフォーマンス改善に取り組む際はユーザーを第一に考えます。ユーザーがサイトに費やす時間の大半が、サイトの読み込みを待つ時間ではなく、サイトを使用しながらレスポンスを待つ時間になるようにします。パフォーマンスが低下すると、ユーザーがどのように感じるかを理解します。

* ユーザーを第一に考えます。最終目標は、特定の端末でのサイトの処理速度を上げることではありません。ユーザーが満足感を得ることが最終目標です。
* ユーザーに対して即座に応答します。ユーザー入力は、100 ミリ秒以内に認識します。

## レスポンス: 100 ミリ秒以内にレスポンスを返す

ユーザーの入力に 100 ミリ秒以内にレスポンスを返せば、ユーザーは遅れを感じません。この時間は、ユーザーによるボタンのクリック、フォーム上のコントロールの切り替え、アニメーションの開始など、すべての入力に当てはまります。 ただし、タッチ操作やスクロールにはあてはまりません。

<table class="responsive">
  <thead>
      <th colspan="2">遅延 &amp; ユーザーの反応</th>
  </thead>
  <tbody>
    <tr>
      <td data-th="Delay">0～16 ミリ秒</td>
      <td data-th="User Reaction">人は非常に動作の追跡が得意なので、アニメーションがスムーズでなければ不満を持ちます。
      画面が 1 秒間あたり 60 回更新されると、スムーズなアニメーションであると感じます。これは 1 フレームあたり 16 ミリ秒となり、ブラウザが新しいフレームを画面に描画する時間が含まれるため、アプリがフレームを生成できる時間は約 10 ミリ秒です。</td>
    </tr>
    <tr>
      <td data-th="Delay">0～100 ミリ秒</td>
      <td data-th="User Reaction">この時間内にユーザーのアクションに応答すると、ユーザーはすぐに結果が得られたと感じます。これより時間がかかると、操作と反応にずれが生じます。</td>
    </tr>
    <tr>
      <td data-th="Delay">100～300 ミリ秒</td>
      <td data-th="User Reaction">ユーザーはやや遅いと感じます。</td>
    </tr>
    <tr>
      <td data-th="Delay">300～1000 ミリ秒</td>
      <td data-th="User Reaction">この時間内に収まれば、途切れることなく自然にタスクが進んでいると感じられます。ウェブを利用する大部分のユーザーにとってのタスクとは、ページの読み込みやビューの切り替えを指します。</td>
    </tr>
    <tr>
      <td data-th="Delay">1,000 ミリ秒以上</td>
      <td data-th="User Reaction">1 秒を超えると、ユーザーは実行したタスクへの関心を失います。</td>
    </tr>
    <tr>
      <td data-th="Delay">10,000 ミリ秒以上</td>
      <td data-th="User Reaction">ユーザーは不満を感じてタスクを中断し、そのまま戻ってこない恐れがあります。</td>
    </tr>
  </tbody>
</table>

この時間内にレスポンスが返らないと、操作と反応にずれが生じるため、ユーザーは遅れを感じます。

## アニメーション: 10 ミリ秒間隔でフレームをレンダリングする

ユーザーの操作に対してすぐにレスポンスを返すのは当然だと思うかもしれませんが、タイミングよく返せないこともあります。その場合はこの 100 ミリ秒を利用して、他の負荷の高い処理を実行します。ただし、ユーザーの妨げにならないよう注意して、可能であればバックグラウンドで処理を実行します。

操作に 500 ミリ秒以上かかる場合は、必ずフィードバックを提供します。

* Process user input events within 50ms to ensure a visible response within 100ms, otherwise the connection between action and reaction is broken. This applies to most inputs, such as clicking buttons, toggling form controls, or starting animations. This does not apply to touch drags or scrolls.
* Though it may sound counterintuitive, it's not always the right call to respond to user input immediately. You can use this 100ms window to do other expensive work. But be careful not to block the user. If possible, do work in the background.
* For actions that take longer than 50ms to complete, always provide feedback.

**50ms or 100ms?:**

アニメーションのフレームレートが一定でなければ、ユーザーはすぐに気付きます。目標は、1 秒あたり 60 フレームとし、各フレームでは以下の手順を実行します。

<figure>
  <img src="images/rail-response-details.png"
    alt="Diagram showing how input received during an idle task is queued,
         reducing available input processing time to 50ms."/>
  <figcaption>
    <b>Figure 2</b>. How idle tasks affect input response budget.
  </figcaption>
</figure>

## アイドル: アイドル時間を最大限に活用する

**Goals**:

* Produce each frame in an animation in 10ms or less. Technically, the maximum budget for each frame is 16ms (1000ms / 60 frames per second ≈ 16ms), but browsers need about 6ms to render each frame, hence the guideline of 10ms per frame.
* Aim for visual smoothness. Users notice when frame rates vary.

単純計算では、各フレームには約 16 ミリ秒の割り当てがあります（1 秒を 60 フレームで除算 = 16.66 ms / フレーム）。 しかし、ブラウザが新しいフレームを画面に描画する時間が必要になるため、アニメーション中に**コードで実際に使用できる時間は 10 ミリ秒程度です**。

* In high pressure points like animations, the key is to do nothing where you can, and the absolute minimum where you can't. Whenever possible, make use of the 100ms response to pre-calculate expensive work so that you maximize your chances of hitting 60fps.
* See [Rendering Performance](/web/fundamentals/performance/rendering/) for various animation optimization strategies.
* Recognize all the types of animations. Animations aren't just fancy UI effects. Each of these interactions are considered animations: 
    * Visual animations, such as entrances and exits, [tweens](https://www.webopedia.com/TERM/T/tweening.html), and loading indicators.
    * Scrolling. This includes flinging, which is when the user starts scrolling, then lets go, and the page continues scrolling.
    * Dragging. Animations often follow user interactions, such as panning a map or pinching to zoom.

## 読み込み: 1,000 ミリ秒以内にコンテンツを提供する

アニメーションのように負荷の高い処理では、できる限り何も行わず、必要最小限の処理にとどめることが重要です。 可能であれば、100 ミリ秒のレスポンス時間を利用して負荷の高い処理を事前に実行し、60 fps を実現できる可能性を最大限に高めます。

詳細については、[レンダリング パフォーマンス](/web/fundamentals/performance/rendering/)を参照してください。

* Use idle time to complete deferred work. For example, for the initial page load, load as little data as possible, then use idle time to load the rest.
* Perform work during idle time in 50ms or less. Any longer, and you risk interfering with the app's ability to respond to user input within 50ms.
* If a user interacts with a page during idle time work, the user interaction should always take the highest priority and interrupt the idle time work.

## RAIL の重要なメトリクスの概要

アイドル時間を利用して遅延している作業を完了します。たとえば、データのプリロードを最小限に抑えてアプリの読み込みを高速にし、残っているデータはアイドル時間に読み込みます。

遅延している作業は 50 ミリ秒程度のブロックにグループ化します。ユーザーが操作を始めた場合は、その操作にレスポンスを返すことが最優先です。

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

100 ミリ秒以内にレスポンスを返すためには、アプリで毎回 50 ミリ秒以内にメインスレッドに制御を返す必要があります。そうすることで、ピクセル パイプラインの実行や、ユーザー入力への反応などが可能になります。

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

50 ミリ秒単位のブロックにすることによって、即座にレスポンスを返しながら、タスクを完了できます。

* [**Chrome DevTools**](#devtools). The developer tools built into Google Chrome. Provides in-depth analysis on everything that happens while your page loads or runs.
* [**Lighthouse**](#lighthouse). Available in Chrome DevTools, as a Chrome Extension, as a Node.js module, and within WebPageTest. You give it a URL, it simulates a mid-range device with a slow 3G connection, runs a series of audits on the page, and then gives you a report on load performance, as well as suggestions on how to improve. Also provides audits to improve accessibility, make the page easier to maintain, qualify as a Progressive Web App, and more.
* [**WebPageTest**](#webpagetest). Available at [webpagetest.org/easy](https://webpagetest.org/easy). You give it a URL, it loads the page on a real Moto G4 device with a slow 3G connection, and then gives you a detailed report on the page's load performance. You can also configure it to include a Lighthouse audit.

サイトは 1 秒以内に読み込みます。それよりも時間がかかると、ユーザーの集中力が切れ、タスクの操作に失敗したと感じます。

### TL;DR {: .hide-from-toc }

[クリティカル レンダリング パスの最適化](/web/fundamentals/performance/critical-rendering-path/)に重点を置き、スムーズなレンダリングを行います。

実際にすべてを 1 秒以内に読み込む必要はなく、読み込みが終わったと感じられるようにします。プログレッシブ レンダリングを有効にして、一部の処理をバックグラウンドで実行します。重要度の低い読み込みは、アイドル状態になるまで延期します（詳細については、[ウェブサイト パフォーマンスの最適化に関する Udacity コース](https://www.udacity.com/course/website-performance-optimization--ud884)をご覧ください）。

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

RAIL のメトリクスに照らしてサイトを評価するには、Chrome DevTools の [Timeline ツール](/web/tools/chrome-devtools/profile/evaluate-performance/timeline-tool)を使用してユーザーの操作を記録します。その後、RAIL の重要なメトリクスに照らして Timeline の記録時間をチェックします。

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