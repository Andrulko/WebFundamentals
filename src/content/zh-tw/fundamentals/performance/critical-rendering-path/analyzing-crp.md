project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: 您需要瞭解很多常見問題，才可確定並解決關鍵轉譯路徑效能方面的瓶頸。現在就讓我們開始實作之旅，找出常見的效能模式，以便您將網頁最佳化。

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# 分析關鍵轉譯路徑效能 {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

您需要瞭解很多常見問題，才可確定並解決關鍵轉譯路徑效能方面的瓶頸。現在就讓我們開始實作之旅，找出常見的效能模式，以便您將網頁最佳化。

最佳化關鍵轉譯路徑的目標是讓瀏覽器儘快繪製網頁：較快的頁面轉譯速度可以提高使用者的參與度、增加網頁瀏覽量並[提高轉換率](http://www.google.com/think/multiscreen/success.html)。因此，透過最佳化要載入的資源和載入順序，我們希望盡量減少訪客注視空白螢幕的時間。

為了更清楚介紹這項過程，我們先從最簡單的情況開始講解，再逐步建構我們的網頁，讓其中包含更多資源、樣式和套用邏輯；在這個過程中，我們還會探討出錯的環節，以及如何針對每種情況最佳化。

最後，在開始之前，我們還要處理一件事...到目前為止，我們一直專注於資源(CSS、JS 或 HTML 檔案) 可進行處理之後，瀏覽器當中所發生的情況，但忽略了從快取或網路中擷取資源的時間。在下一課中，我們將從應用程式的網路連線層面深入研究如何最佳化，但同時 (為了更貼近現實) 也將做出以下假設：

* 到伺服器的網路往返 (傳播延遲) 將花費 100 毫秒
* HTML 文件的伺服器回應時間為 100 毫秒，而其他所有檔案的回應時間都為 10 毫秒

## Hello World 體驗

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

我們將從基本的 HTML 標記和單一圖片開始，沒有 CSS 或 JavaScript，就是這麼簡單。現在，我們在 Chrome DevTools 中開啟網路時間軸，並檢查產生的資源瀑布：

We'll start with basic HTML markup and a single image; no CSS or JavaScript. Let's open up our Network timeline in Chrome DevTools and inspect the resulting resource waterfall:

<img src="images/waterfall-dom.png" alt="CRP" />

HTML 內容準備就緒後，瀏覽器必須剖析位元組、將其轉換為權杖，並建構 DOM 樹狀結構。為方便查看，DevTools 會在底部回報 DOMContentLoaded 事件的時間 (216 毫秒)，該時間也與藍色垂直線相對應。HTML 下載結束和藍色垂直線 (DOMContentLoaded) 之間的間隔是瀏覽器建構 DOM 樹狀結構花費的時間，在此示例中僅為幾毫秒。

最後，我們注意到一個有趣的現象：我們的「趣照」竟然沒有禁止 domContentLoaded 事件！ 由此可知，我們無需等待網頁上的每個資源，即可建構轉譯樹狀結構，甚至是繪製網頁：**快速初次繪製並不需要所有資源**。事實上，就像我們接下來將要說明的，談論關鍵轉譯路徑時，我們通常談論的是 HTML 標記、CSS 和 JavaScript。圖片不會阻止網頁的初次轉譯，儘管如此，我們也應努力確保系統儘快繪製圖片！<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

不過，系統會禁止圖片上的「load」事件 (也常稱為「onload」)：DevTools 在 335 毫秒時回報了 onload 事件。回想一下，onload 事件代表網頁所需的**所有資源**都已下載並經過處理的時間點，這是瀏覽器的載入旋轉圖示停止旋轉的時間，而資訊瀑布會以紅色垂直線標記這一點。

我們的「Hello World 體驗」頁面表面看起來好像非常簡單，但背後需要完成大量的工作才能呈現出這種成效！ 不過在實際使用時，我們還需要 HTML 以外的許多資源：我們可能需要 CSS 樣式表以及一個或多個新增網頁互動性的指令碼。我們將兩者搭配使用，看看會產生什麼結果：

That said, the `load` event (also known as `onload`), is blocked on the image: DevTools reports the `onload` event at 335ms. Recall that the `onload` event marks the point at which **all resources** that the page requires have been downloaded and processed; at this point, the loading spinner can stop spinning in the browser (the red vertical line in the waterfall).

## 搭配使用 JavaScript 和 CSS

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

* 與只有 HTML 的示例不同，我們現在還需要擷取並剖析 CSS 檔案以建構 CSSOM，而且我們必須使用 DOM 和 CSSOM 來建構轉譯樹狀結構。
* 我們的網頁上還有一個禁止剖析器的 JavaScript 檔案，因此在系統下載並剖析 CSS 檔案之前，domContentLoaded 事件將會遭到禁止：JavaScript 可能會查詢 CSSOM，因此在執行 JavaScript 之前，我們必須阻止並等待 CSS。

**What if we replace our external script with an inline script?** Even if the script is inlined directly into the page, the browser can't execute it until the CSSOM is constructed. In short, inlined JavaScript is also parser blocking.

我們減少了一個請求，但為什麼 onload 和 domContentLoaded 的時間仍然沒有變化呢？ 我們發現，無論 JavaScript 是內嵌或外部都無關痛癢，因為只要瀏覽器遇到指令碼標記，它就會禁止及等待，直到 CSSOM 建構完成。此外，在我們的第一個示例中，瀏覽器同時下載 CSS 和 JavaScript，而且下載程序幾乎是在同一時間完成。因此，在這個特定實例中，內嵌 JavaScript 程式並沒有太大意義！ 難道我們就陷入僵局，沒辦法加快網頁轉譯速度了嗎？ 實際上，我們還有多個不同的應對策略。

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Inlined JavaScript:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, and inlined JS" />

We are making one less request, but both our `onload` and `domContentLoaded` times are effectively the same. Why? Well, we know that it doesn't matter if the JavaScript is inlined or external, because as soon as the browser hits the script tag it blocks and waits until the CSSOM is constructed. Further, in our first example, the browser downloads both CSS and JavaScript in parallel and they finish downloading at about the same time. In this instance, inlining the JavaScript code doesn't help us much. But there are several strategies that can make our page render faster.

好多了！ 剖析 HTML 之後，不久即會觸發 domContentLoaded 事件：瀏覽器已得知不要禁止 JavaScript，而且因為沒有其他禁止剖析器的指令碼，CSSOM 建構也可以同步進行了。

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

Much better! The `domContentLoaded` event fires shortly after the HTML is parsed; the browser knows not to block on JavaScript and since there are no other parser blocking scripts the CSSOM construction can also proceed in parallel.

**T<sub>0</sub> 和 T<sub>1</sub> 之間的時間表示網路和伺服器的處理時間。** 在最理想的情況下 (HTML 檔案較小)，我們僅需一個網路往返過程即可擷取整份文件；由於 TCP 傳輸協定的工作方式，較大的檔案可能需要多個往返過程，我們將在以後的課程中深入探討這個主題。**因此，在最理想的情況下，上述網頁具有一個往返過程 (最少) 關鍵轉譯路徑。**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

現在，讓我們看看帶有外部 CSS 檔案的相同網頁：

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM + CSSOM CRP" />

再重複一下，我們需要一個網路往返過程來擷取 HTML 文件，然後檢索到的標記告知我們還需要 CSS 檔案：這代表瀏覽器必須返回伺服器並取得 CSS，然後才能在螢幕上呈現網頁。**因此，這個網頁最少需要兩個往返過程才能顯示**。請記得，CSS 檔案可能需要多個往返過程，因此重點應放在如何以「最少」時間達成目標。

我們來定義用於描述關鍵轉譯路徑的詞彙：

現在，我們將第一個示例與「HTML + CSS」示例的關鍵路徑特徵稍做比較：

## 效能模式

The simplest possible page consists of just the HTML markup; no CSS, no JavaScript, or other types of resources. To render this page the browser has to initiate the request, wait for the HTML document to arrive, parse it, build the DOM, and then finally render it on the screen:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

我們必須同時使用 HTML 和 CSS 來建構轉譯樹狀結構，因此 HTML 和 CSS 都是關鍵資源：瀏覽器僅會在取得 HTML 文件之後擷取 CSS，因此關鍵路徑長度最少為兩個往返過程；兩種資源加起來的關鍵位元組總量最多為 9 KB。

<img src="images/analysis-dom.png" alt="Hello world CRP" />

我們新增了 app.js (網頁上的外部 JavaScript 資源)，而且據我們目前所瞭解，這是一種剖析器禁止 (即關鍵) 資源。更糟的是，為了執行 JavaScript 檔案，我們還必須禁止並等待 CSSOM。請注意，在「style.css」下載和 CSSOM 建構完成之前，瀏覽器將會暫停。

Now, let's consider the same page but with an external CSS file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

不過，如果我們實際查看該網頁的「網路瀑布流」，就會發現 CSS 和 JavaScript 請求幾乎會在同一時間發出：瀏覽器獲得 HTML，發現這兩種資源，然後發出兩項請求。因此，上述網頁具有下列關鍵路徑特徵：

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

與網站開發人員交流之後，我們發現網頁中新增的 JavaScript 不必是禁止指令碼：我們的某些分析和其他程式碼不需要禁止網頁轉譯。瞭解這些要點後，我們就可以在指令碼標記中新增「async」屬性，取消對剖析器的禁止令：

Let's define the vocabulary we use to describe the critical rendering path:

* **關鍵資源**：可能禁止網頁初次轉譯的資源。
* **關鍵路徑長度**：即往返過程數量，或擷取所有關鍵資源所需的總時間。
* **關鍵位元組**：實現網頁初次轉譯所需的總位元組數，這是所有關鍵資源的傳輸檔案大小總和。 第一個示例的單一 HTML 網頁包含一項關鍵資源 (HTML 文件)，關鍵路徑長度也與 1 個網路往返過程 (假設檔案較小) 相等，而且總關鍵位元組數正好是 HTML 文件本身的傳輸大小。

對指令碼採用非同步模式具有以下幾項優勢：

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** 種關鍵資源
* **2** 個或更多往返過程的最短關鍵路徑長度
* **9** KB 的關鍵位元組

最後，假設只有在列印時才需要用到 CSS 樣式表， 網頁看起來又會如何呢？

Now let's add an extra JavaScript file into the mix.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

因為 style.css 資源僅用於列印，因此，瀏覽器不必禁止它即可轉譯網頁。因此，只要 DOM 建構完成，瀏覽器即具備轉譯網頁的足夠資訊！ 所以，這個網頁僅具有一種關鍵資源 (HTML 檔案)，最小關鍵轉譯路徑長度為一個往返過程。

We added `app.js`, which is both an external JavaScript asset on the page and a parser blocking (that is, critical) resource. Worse, in order to execute the JavaScript file we have to block and wait for CSSOM; recall that JavaScript can query the CSSOM and hence the browser pauses until `style.css` is downloaded and CSSOM is constructed.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

That said, in practice if we look at this page's "network waterfall," you'll see that both the CSS and JavaScript requests are initiated at about the same time; the browser gets the HTML, discovers both resources, and initiates both requests. As a result, the above page has the following critical path characteristics:

* **3** 種關鍵資源
* **2** 個或更多往返過程的最短關鍵路徑長度
* **11** KB 的關鍵位元組

We now have three critical resources that add up to 11KB of critical bytes, but our critical path length is still two roundtrips because we can transfer the CSS and JavaScript in parallel. **Figuring out the characteristics of your critical rendering path means being able to identify the critical resources and also understanding how the browser will schedule their fetches.** Let's continue with our example.

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

An asynchronous script has several advantages:

* 指令碼再也不會禁止剖析器，也不再是關鍵轉譯路徑的一部分。
* 因為沒有其他關鍵指令碼，CSS 也不需要阻止 domContentLoaded 事件
* domContentLoaded 事件越早觸發，其他應用程式邏輯就可越早開始執行

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