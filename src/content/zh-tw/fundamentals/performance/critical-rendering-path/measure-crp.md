project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: 無法評估就談不上最佳化。幸運的是，Navigation Timing API 提供了所有必備工具來評估關鍵轉譯路徑的每個步驟！

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# 使用 Navigation Timing 評估關鍵轉譯路徑 {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

無法評估就談不上最佳化。幸運的是，Navigation Timing API 提供了所有必備工具來評估關鍵轉譯路徑的每個步驟！

* Navigation Timing 為評估關鍵轉譯路徑提供了高解析度的時間戳記。
* 瀏覽器發出一系列可採用的事件，藉此捕捉關鍵轉譯路徑的不同階段。

每個有效的效能策略背後絕對少不了準確的評估和檢測。這也就是 Navigation Timing API 所提供的。

## Auditing a page with Lighthouse {: #lighthouse }

Lighthouse is a web app auditing tool that runs a series of tests against a given page, and then displays the page's results in a consolidated report. You can run Lighthouse as a Chrome Extension or NPM module, which is useful for integrating Lighthouse with continuous integration systems.

在上圖中，每一個標籤都對應到瀏覽器針對載入的每個網頁追蹤的高解析度時間戳記。實際上，在這個具體的例子中，我們只顯示了各種不同時間戳記中的一小部分而已。我們現在暫時跳過所有與網路有關的時間戳記，但是在後續的課程中還會詳細介紹。

那麼，這些時間戳記到底有什麼含義呢？

![Lighthouse's CRP audits](images/lighthouse-crp.png)

See [Critical Request Chains](/web/tools/lighthouse/audits/critical-request-chains) for more information on this audit's results.

## Instrumenting your code with the Navigation Timing API {: #navigation-timing }

上述示例乍看之下可能會令人頭昏眼花，但實際上它確實很簡單。Navigation Timing API 會捕捉所有相關的時間戳記，而我們的程式碼只是等待`onload` 事件觸發，然後計算各個時間戳記之間的間隔。請記得，onLoad 事件會在 domInteractive、domContentLoaded 和 domComplete 之後觸發。

<img src="images/dom-navtiming.png"  alt="Navigation Timing" />

Each of the labels in the above diagram corresponds to a high resolution timestamp that the browser tracks for each and every page it loads. In fact, in this specific case we're only showing a fraction of all the different timestamps &mdash; for now we're skipping all network related timestamps, but we'll come back to them in a future lesson.

So, what do these timestamps mean?

* **domLoading：**這是整個過程開始的時間戳記，瀏覽器開始解析 HTML 文件第一批收到的位元組 。
* **domInteractive：**標記瀏覽器完成解析並且所有 HTML 和 DOM 都建構完畢的時間點。
* `domContentLoaded`標記 DOM 準備就緒並且沒有樣式表禁止 JavaScript 執行的時間點，表示我們現在 (大概) 可以開始建構轉譯樹狀結構了。 
    * 很多 JavaScript 框架等待這個事件發生後，才會開始執行自己的邏輯。因此，瀏覽器會透過捕捉 *EventStart* 和 *EventEnd* 時間戳記，方便我們追蹤執行邏輯所需的時間。
* **domComplete：** 顧名思義，所有的處理程序都已完成，網頁上所有資源 (圖片等) 也下載完成，表示載入旋轉圖示停止旋轉了。
* **loadEvent：**這是每個網頁載入的最後一步，瀏覽器會觸發「onLoad」事件，以便觸發額外的應用程式邏輯。

The HTML specification dictates specific conditions for each and every event: when it should be fired, which conditions should be met, and so on. For our purposes, we'll focus on a few key milestones related to the critical rendering path:

* **domInteractive** 標記 DOM 準備就緒。
* `domContentLoaded` 通常標記 [DOM 和 CSSOM 都準備就緒]的時間(http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/)。 
    * 如果沒有禁止剖析器的 JavaScript，*DOMContentLoaded* 會在 *domInteractive* 之後立即觸發。
* **domComplete** 標記網頁和所有附屬資源都已經準備就緒的時間。

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