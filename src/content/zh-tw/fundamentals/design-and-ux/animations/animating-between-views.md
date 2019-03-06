project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: 學習如何在您的應用程式的兩個檢視之間執行動畫處理。

{# wf_updated_on: 2014-10-21 #} {# wf_published_on: 2014-08-08 #}

# 檢視之間的動畫處理 {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}

您可能常常會想要在應用程式中，讓使用者於檢視之間移動 -- 無論是前往詳細資料檢視的清單，或是顯示側邊欄導覽。 檢視之間的動畫處理能讓使用者不至於分心，並為專案增添更多的活力。

### TL;DR {: .hide-from-toc }

* 使用轉換，以在檢視之間移動；避免使用 `left`、`top` 或觸發版面配置的任何其他屬性。
* 確保您使用的任何動畫不但要潮，而且控制持續時間要短。
* 同時要考慮螢幕變大時的動畫和版面配置變化；較小螢幕上可行的內容，在桌面環境上可能看起來很突兀。

這些檢視轉換的外觀和行為重度取決於您正在處理的檢視類型，因此在檢視上層動畫處理強制回應重疊，較之於在清單和詳細資訊檢視之間轉換，應該是個不同的經驗。

Note: 您應該針對所有動畫，旨在維持至少 60 fps。 這樣您的使用者就不會遇到會間斷的動畫，而導致他們的體驗很出戲。 確保早在動畫開始之前，針對您計畫變更的一切，設定好動畫處理元素的 will-change。 針對檢視轉換，極有可能您會想要使用`will-change: transform`。

## 使用轉換以在檢視之間移動

<div class="attempt-left">
  <figure>
    <img src="images/view-translate.gif" alt="Translating between two views" />
  </figure>
</div>

要讓過程更容易些，讓我們假設有兩個檢視： 一個清單檢視和一個詳細資訊檢視。 當使用者在清單檢視內點選一個清單項目，詳細資訊檢視將滑入，而清單檢視將滑出。

<div style="clear:both;"></div>

<div class="attempt-right">
  <figure>
    <img src="images/container-two-views.svg" alt="View hierarchy." />
  </figure>
</div>

To achieve this effect, you need a container for both views that has `overflow: hidden` set on it. That way, the two views can both be inside the container side-by-side without showing any horizontal scrollbars, and each view can slide side-to-side inside the container as needed.

<div style="clear:both;"></div>

要達到這個效果，您需要針對兩種檢視使用容器，並將之設定 `overflow: hidden`。 以這種方式，兩個檢視都可以在容器裡並排，而不會顯示任何水平捲軸，而每一檢視也也可視需要在容器內並排滑動。

    .container {
      width: 100%;
      height: 100%;
      overflow: hidden;
      position: relative;
    }
    

The position of the container is set as `relative`. This means that each view inside it can be positioned absolutely to the top left corner and then moved around with transforms. This approach is better for performance than using the `left` property (because that triggers layout and paint), and is typically easier to rationalize.

    .view {
      width: 100%;
      height: 100%;
      position: absolute;
      left: 0;
      top: 0;
    
      /* let the browser know we plan to animate
         each view in and out */
      will-change: transform;
    }
    

該容器的 CSS 為：

    .view {
      /* Prefixes are needed for Safari and other WebKit-based browsers */
      transition: -webkit-transform 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946);
      transition: transform 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946);
    }
    

該容器的位置被設定為 `relative`。 這代表其中的每個檢視可以定位在最左上角，然後以變形來到處移動。 較之使用 `left` 屬性 (會觸發版面配置與繪製)，這種方法的效能更好，通常也更容易合理化。

    .details-view {
      -webkit-transform: translateX(100%);
      transform: translateX(100%);
    }
    

在 `transform` 屬性上新增 `transition`，會提供不錯的滑動效果。 為了給它一個不錯的感覺，它使用自訂 `cubic-bezier` 曲線，如我們在 [自訂緩動指南](custom-easing.html) 中所討論。

    var container = document.querySelector('.container');
    var backButton = document.querySelector('.back-button');
    var listItems = document.querySelectorAll('.list-item');
    
    /**
     * Toggles the class on the container so that
     * we choose the correct view.
     */
    function onViewChange(evt) {
      container.classList.toggle('view-change');
    }
    
    // When you click on a list item bring on the details view.
    for (var i = 0; i < listItems.length; i++) {
      listItems[i].addEventListener('click', onViewChange, false);
    }
    
    // And switch it back again when you click on the back button
    backButton.addEventListener('click', onViewChange);
    

畫面之外的檢視應該解譯為往右，因此在此例中，詳細資訊檢視應該要移動：

    .view-change .list-view {
      -webkit-transform: translateX(-100%);
      transform: translateX(-100%);
    }
    
    .view-change .details-view {
      -webkit-transform: translateX(0);
      transform: translateX(0);
    }
    

現在需要少量的 JavaScript 來處理類別。 這會切換檢視上的適當類別。

最後，我們為這些類別新增 CSS 宣告。

Caution: Making this kind of hierarchy in a cross-browser way can be challenging. For example, iOS requires an additional CSS property, `-webkit-overflow-scrolling: touch`, to "reenable" fling scrolling, but you don’t get to control which axis that’s for, as you can with the standard overflow property. Be sure to test your implementation across a range of devices!

您可以加以擴展，以涵蓋多個檢視，基本概念應該是一樣的；每個非可見檢視應該在畫面之外，並在必要時帶回，而目前的畫面上檢視應該要移開。

## 確保您的動畫可搭配較大螢幕運作

<div class="attempt-right">
  <figure>
    <img src="images/container-two-views-ls.svg" alt="View hierarchy on a large screen." />
  </figure>
</div>

Note: 要跨瀏覽器設計這種階層，可能頗具挑戰性。 例如，iOS 需要額外的 CSS 屬性 `-webkit-overflow-scrolling: touch`，以「重新啟用」快滑捲動，但不同於標準溢出屬性的是，您無法控制要針對哪個軸執行。 一定要在不同裝置測試您的建置！

## Feedback {: #feedback }

除了在檢視之間轉換，這項技巧也可套用於其他滑入元素中，例如側邊欄導覽元素。 唯一真正的區別是您應該不需要移動其他檢視。