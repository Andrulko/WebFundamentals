project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: 合成是指組合網頁上的繪製部分，以用於在螢幕上顯示。

{# wf_updated_on: 2015-03-19 #} {# wf_published_on: 2000-01-01 #}

# 堅守純合成器屬性和管理圖層數目 {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}

合成是指組合網頁上的繪製部分，以用於在螢幕上顯示。

此領域存在影響網頁效能的兩項關鍵因素：需要加以管理的合成器層數目，以及您針對動畫使用的屬性。

### TL;DR {: .hide-from-toc }

* 針對您的動畫，堅守變形和透明度變更。
* 以 will-change 或 translateZ 將移動元素升階。
* 避免過度使用升階規則；圖層需要記憶體和管理。

## 針對動畫使用變形和透明度變更

表現最佳的像素管道版本會避免版面配置和繪製，並只需要合成變更：

<img src="images/stick-to-compositor-only-properties-and-manage-layer-count/frame-no-layout-paint.jpg"  alt="無版面配置或繪製的像素管道。" />

為實現此目標，您需要堅守只需合成器即可處理的變更屬性。 時下只有兩個屬性才是真的：**變形** 和 **透明度**：

<img src="images/stick-to-compositor-only-properties-and-manage-layer-count/safe-properties.jpg"  alt="您可以不觸發版面配置或繪製而動畫處理的屬性。" />

使用變形和透明度需要注意的是，您變更這些屬性的所在元素應該要在 *它本身的合成器層上*。 要製作合成器層，您必須將元素升階，這部分我們將在稍後討論。

Note: 如果擔心可能無法限制動畫只使用這些屬性，請看看 [FLIP 原則](https://aerotwist.com/blog/flip-your-animations)，這可能幫助您將動畫從開銷更大的屬性重新映射為變形和透明度的更改。

## 將您計畫動畫處理的元素升階

正如我們在「[簡化繪製複雜性和減少繪製區域](simplify-paint-complexity-and-reduce-paint-areas)」一節中提到，您應升階您計畫動畫處理的元素 (要合理，不要過火！) 至它們本身的層：

    .moving-element {
      will-change: transform;
    }
    

或針對較舊的瀏覽器，或不支援 will-change 的瀏覽器：

    .moving-element {
      transform: translateZ(0);
    }
    

這會向瀏覽器提供即將送入變更的預警，並且視您計畫變更的對象而定，瀏覽器可以進行佈建預備 ，例如建立合成器層。

## 管理層，並避免層爆炸

知道層經常有助於效能之後，用以下設計將您網頁上所有元素升階的想法，或許相當誘人：

    * {
      will-change: transform;
      transform: translateZ(0);
    }
    

這是「您會想將網頁上每一元素都升階」的婉轉說法。 此處的問題在於您建立的每一層都需要記憶體與管理，這可不是免費的。 事實上，在記憶體有限的裝置上，其對效能的影響可能遠大於建立層的任何好處。 每一層的紋理必須上載至 GPU，所以 CPU 和 GPU 的頻寬及 GPU 紋理的可用記憶體，都存在著進一步的限制。

Warning: 簡而言之，**不要非必要地將元素升階**。

## 使用 Chrome DevTools，以瞭解您應用程式中的層

<div class="attempt-right">
  <figure>
    <img src="images/stick-to-compositor-only-properties-and-manage-layer-count/paint-profiler.jpg" alt="The toggle for the paint profiler in Chrome DevTools.">
  </figure>
</div>

要瞭解您應用程式中的層，以及元素具有一個層的原因，您必須在 Chrome DevTools 的 Timeline 中，啟用繪製分析工具：

<div style="clear:both;"></div>

With this switched on you should take a recording. When the recording has finished you will be able to click individual frames, which is found between the frames-per-second bars and the details:

<img src="images/stick-to-compositor-only-properties-and-manage-layer-count/frame-of-interest.jpg"  alt="A frame the developer is interested in profiling." />

Clicking on this will provide you with a new option in the details: a layer tab.

<img src="images/stick-to-compositor-only-properties-and-manage-layer-count/layer-tab.jpg"  alt="The layer tab button in Chrome DevTools." />

This option will bring up a new view that allows you to pan, scan and zoom in on all the layers during that frame, along with reasons that each layer was created.

<img src="images/stick-to-compositor-only-properties-and-manage-layer-count/layer-view.jpg"  alt="The layer view in Chrome DevTools." />

Using this view you can track the number of layers you have. If you’re spending a lot time in compositing during performance-critical actions like scrolling or transitions (you should aim for around **4-5ms**), you can use the information here to see how many layers you have, why they were created, and from there manage layer counts in your app.

## Feedback {: #feedback }

使用此檢視，您可以追蹤您擁有的層數目。 如果您在例如捲動或轉換等效能關鍵行為期間，花了很多時間在合成上 (以約 **4-5ms** 為目標)，您可以使用此處的資訊來查看您有多少層、其建立的原因，並從那裡管理您應用程式中的層數目。