project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: CSSOM 樹狀結構和 DOM 樹狀結構會結合成轉譯樹狀結構，用於計算每個可視元素的版面配置，並作為繪製過程的輸入參數，在螢幕上轉譯各個像素。 如要達成最佳轉譯成效，關鍵就在於確實執行最佳化流程的每個步驟。

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# 轉譯樹狀結構的建構、版面配置和繪製 {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

CSSOM 樹狀結構和 DOM 樹狀結構會結合成轉譯樹狀結構，用於計算每個可視元素的版面配置，並作為繪製過程的輸入參數，在螢幕上轉譯各個像素。 如要達成最佳轉譯成效，關鍵就在於確實執行最佳化流程的每個步驟。

在上一節中，我們介紹了物件模型的建構方式，也就是根據輸入的 HTML 和 CSS 建構 DOM 樹狀結構和 CSSOM 樹狀結構。 不過，這是兩個互相獨立的物件，分別涵蓋文件的不同層面：一個描述內容，另一個則是描述套用於文件的樣式規則。 那麼瀏覽器是如何將兩者結合起來，並在螢幕上呈現各個像素呢？

### TL;DR {: .hide-from-toc }

* DOM 樹狀結構和 CSSOM 樹狀結構合併成轉譯樹狀結構。
* 轉譯樹狀結構只包含轉譯網頁所需的節點。
* 版面配置會計算每個物件的精確位置和大小。
* 繪製是最後一步，將會以最終的轉譯樹狀結構做為輸入參數，在螢幕上呈現各個像素。

對瀏覽器而言，第一步就是將 DOM 樹狀結構和 CSSOM 樹狀結構合併成「轉譯樹狀結構」，讓這個結構不僅包含網頁上所有可見的 DOM 內容，同時也包含各個節點的 CSSOM 樣式資訊。

<img src="images/render-tree-construction.png" alt="DOM 樹狀結構和 CSSOM 樹狀結構會合併成轉譯樹狀結構" />

為了建構轉譯樹狀結構，瀏覽器大致需要執行下列步驟：

1. Starting at the root of the DOM tree, traverse each visible node.
    
    * 某些節點完全無法察覺 (例如 script 標記、meta 標記等)。由於轉譯的輸出內容中不會反映這些節點，因此會被省略。
    * 某些節點透過 CSS 隱藏起來，在轉譯樹狀結構中也會被省略。以上述示例的 span 節點說明，由於該節點透過顯式規則設定了「display:none」屬性，因此不會出現在轉譯樹狀結構中。

2. For each visible node, find the appropriate matching CSSOM rules and apply them.

3. 發送可見的節點，包括內容和計算的的樣式。

Note: 順帶一提，請注意「visibility: hidden」和「display: none」不同。 前者會隱藏元素，但這個元素仍會佔據版面配置中的相應空間 (其實就是一個空白方塊)；而後者 (display: none) 會直接從轉譯樹狀結構中徹底移除元素，該元素不僅會消失，而且也不再屬於版面配置的一部分。

最終輸出的轉譯樹狀結構不僅包含螢幕上顯示的所有可見內容，同時也包含相應的樣式資訊。我們就快要大功告成了！ **有了轉譯樹狀結構，我們就能進入「版面配置」階段。**

到目前為止，我們已經計算了哪些節點應為可見節點及其計算的樣式，但是尚未計算節點在裝置的 [檢視區]/web/fundamentals/design-and-ux/responsive/#set-the-viewport中的準確位置和大小。這就是在「版面配置」階段 (有時也稱為「重新編排」) 所要做的工作。

為了計算出每個物件的準確大小和位置，瀏覽器從轉譯樹狀結構的根節點開始瀏覽，以計算網頁上每個物件的幾何形狀。 下面就讓我們看一個簡單的例子：

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/nested.html" region_tag="full" adjust_indentation="auto" %}
</pre>

以上網頁的內文包含兩個巢狀嵌套的 div：第一個 div (上層元素) 將節點的顯示大小設定為檢視區寬度的 50%，第二個 div (包含在上層元素中) 將寬度設定為上層元素的 50%，即檢視區寬度的 25%！

The body of the above page contains two nested div's: the first (parent) div sets the display size of the node to 50% of the viewport width, and the second div\---contained by the parent\---sets its width to be 50% of its parent; that is, 25% of the viewport width.

<img src="images/layout-viewport.png" alt="Calculating layout information" />

知道哪些節點可見、計算的樣式和幾何形狀之後，我們終於可以將這些資訊傳遞到最後一個階段，將轉譯樹狀結構中的每個節點轉換為螢幕上的實際像素。這個步驟通常稱為「繪製」或者「點陣化」。

到目前為止，您都理解了嗎？ 上述每個步驟都需要瀏覽器進行大量的工作，這也表示可能經常需要消耗相當長的時間。 幸好，Chrome DevTools 可以協助我們深入瞭解上述各個階段。 讓我們檢視原始「hello world」示例中的版面配置階段：

This can take some time because the browser has to do quite a bit of work. However, Chrome DevTools can provide some insight into all three of the stages described above. Let's examine the layout stage for our original "hello world" example:

<img src="images/layout-timeline.png" alt="Measuring layout in DevTools" />

* 在時間軸中透過「Layout」事件捕捉轉譯樹狀結構的建構、定位和大小的計算。
* 版面配置完成後，瀏覽器會觸發「Paint Setup」和「Paint」事件，將轉譯樹狀結構轉化為螢幕上的實際像素。

完成上述所有步驟後，我們的網頁終於呈現在檢視區了！

The page is finally visible in the viewport:

<img src="images/device-dom-small.png" alt="Rendered Hello World page" />

我們的示範網頁看起來也許很簡單，但是非常費工！ 如果修改了 DOM 或 CSSOM，您覺得將會發生什麼情況？ 如此一來，我們就必須重複上述步驟，以確定需要在螢幕上重新呈現的像素。

1. 處理 HTML 標記，產生 DOM 樹狀結構。
2. 處理 CSS 標記，產生 CSSOM 樹狀結構。
3. 將 DOM 樹狀結構和 CSSOM 樹狀結構合併為轉譯樹狀結構。
4. 對轉譯樹狀結構進行版面配置，計算每個節點的幾何形狀。
5. 在螢幕上繪製各個節點。

**所謂最佳化關鍵轉譯路徑，就是儘可能縮短上述第 1 步到第 5 步所花費的總時間。** 時間縮短後，我們就可以儘早在螢幕上呈現內容，還可以縮短初次轉譯後螢幕刷新的時間間隔，也就是提高互動式內容的更新速率。

***Optimizing the critical rendering path* is the process of minimizing the total amount of time spent performing steps 1 through 5 in the above sequence.** Doing so renders content to the screen as quickly as possible and also reduces the amount of time between screen updates after the initial render; that is, achieve higher refresh rates for interactive content.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}