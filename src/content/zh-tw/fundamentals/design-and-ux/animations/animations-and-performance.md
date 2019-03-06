project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: 動畫效能必須良好，否則會負面影響使用者體驗。

{# wf_updated_on: 2014-10-20 #} {# wf_published_on: 2014-08-08 #}

# 動畫和效能 {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %} {% include "web/_shared/contributors/samthorogood.html" %}

製作動畫時，必須注意保持 60 fps，因為任何斷斷續續或停頓對使用者都很明顯，並負面影響他們的使用體驗。

### TL;DR {: .hide-from-toc }

* 注意不要讓您的動畫引起效能問題；確保您知道動畫處理特定 CSS 屬性的影響。
* 動畫處理會變更頁面幾何形狀 (版面配置) 或引起繪製的屬性，成本尤其高。
* 儘可能堅守可變的變形和透明度。
* 使用 `will-change` 以確保瀏覽器知道您計畫要動畫處理的目標。

動畫處理屬性並非免費，某些屬性的動畫處理成本比其他屬性來得低。 例如，動畫處理一項元素的 `width` 與 `height`，可能會造成其幾何形狀改變以及頁面上的其他元素移動或或變更大小。 此過程稱為版面配置 (或在例如 Firefox 等 Gecko 架構瀏覽器中稱為自動重排)，而且在頁面具有大量的元素時，成本高昂。 每當觸發版面配置時，頁面或其一部分通常會需要繪製，通常這甚至比版面配置操作本身更昂貴。

您應該儘可能避免動畫處理會觸發版面配置或繪製的屬性。 對於大多數最新瀏覽器，這代表要限制動畫處理為 `opacity` 或 `transform`，而這兩個都可以透過瀏覽器高度最佳化；動畫由 JavaScript 或 CSS 處理都不要緊。

至於可由個別 CSS 屬性觸發的行為之完整清單，可查閱 [CSS 觸發器](http://csstriggers.com)，而且在 \[HTML5 高效能動畫萬歲\] (High Performance Animations on HTML5 Rocks)(http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)上，您可以找到完整指南。

### 使用 will-change 屬性

{# include shared/related_guides.liquid inline=true list=page.related-guides.blocking-css #}

使用 [`will-change`](http://dev.w3.org/csswg/css-will-change/) 以確保瀏覽器知道您打算變更元素的屬性，這絕對值得您費時間為之。 在您變更之前，這可讓瀏覽器建置最適當的最佳化。 必須注意不要過度使用 `will-change`，因為這可能會導致瀏覽器浪費資源，反過來會導致更多效能問題。

    .box {
      will-change: transform, opacity;
    }
    

一般的經驗法則是，如果動畫可以在接下來的 200ms 觸發 -- 透過使用者互動或由於您應用程式的狀態 -- 那麼在動畫處理元素時納入 will-change 則是個好主意。 針對大多數情況，您打算動畫處理的應用程式之目前檢視內的任何元素，應該針對您計畫變更的任何屬性啟用 `will-change`。 就之前指南一直使用的方塊範例而言，為變形和透明度新增 `will-change`，看起來就像這樣：

## CSS vs JavaScript 效能

現在支援它的瀏覽器，如 Chrome、Firefox 和 Opera，會在檯面下執行適當的最佳化，以支援這些屬性的變更或動畫處理。

* CSS-based animations, and Web Animations where supported natively, are typically handled on a thread known as the "compositor thread". This is different from the browser's "main thread", where styling, layout, painting, and JavaScript are executed. This means that if the browser is running some expensive tasks on the main thread, these animations can keep going without being interrupted.

* Other changes to transforms and opacity can, in many cases, also be handled by the compositor thread.

* If any animation triggers paint, layout, or both, the "main thread" will be required to do work. This is true for both CSS- and JavaScript-based animations, and the overhead of layout or paint will likely dwarf any work associated with CSS or JavaScript execution, rendering the question moot.

網路上有許多網頁與評論討論串，都關於以效能角度而言討論 CSS 和 JavaScript 動畫的相對優點。 以下有幾點需要牢記：

## Feedback {: #feedback }

如果您想知道，動畫處理特定屬性會觸發哪些工作，請參閱 [CSS 觸發器](http://csstriggers.com)，以取得更多詳細資料。