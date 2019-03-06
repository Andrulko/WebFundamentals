project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description:Shadow DOM 可讓網絡開發者爲網絡組件創建獨立的 DOM 和 CSS。

{# wf_updated_on: 2016-10-13 #} {# wf_published_on: 2016-08-01 #}

# Shadow DOM v1：獨立的網絡組件 {: .page-title }

{% include "web/_shared/contributors/ericbidelman.html" %}

### TL;DR {: #tldr .hide-from-toc}

Shadow DOM 解決了構建網絡應用的脆弱性問題。脆弱性是由 HTML、CSS 和 JS 的全局性引起的。 多年以來，我們發明了[多](http://getbem.com/introduction/)[個](https://github.com/css-modules/css-modules)[工具](https://www.smashingmagazine.com/2011/12/an-introduction-to-object-oriented-css-oocss/)來規避這些問題。例如，使用新的 HTML id/類時，無法瞭解是否與頁面所使用的現有名稱衝突。[微小錯誤](http://www.2ality.com/2012/08/ids-are-global.html)漸漸增多，CSS 特異性成爲一個大問題（`!important` 所有的事情！），樣式選擇器變得失控以及[性能可能受損](/web/updates/2016/06/css-containment)，不一而足。

**Shadow DOM 修復了 CSS 和 DOM**。它在網絡平臺中引入**作用域樣式**。 無需工具或命名約定，您即可使用原生 JavaScript **捆綁 CSS 和標記**、隱藏實現詳情以及**編寫獨立的組件**。

## 簡介{: #intro}

Note: **已經很熟悉 Shadow DOM？**本文章介紹新版 <a href="http://w3c.github.io/webcomponents/spec/shadow/" target="_blank">Shadow DOM v1 規範</a>。如果您有 Shadow DOM 的使用經驗，則應該瞭解 [Chrome 35 中隨附的 v0 版本](https://www.chromestatus.com/features/4507242028072960)以及 webcomponents.js polyfill。這些概念是相同的，只不過 v1 規範的 API 存在一些重要差異。此外，所有主要瀏覽器已確定將實現該版本，其中 Safari Tech Preview 和 Chrome Canary 已實現。請繼續閱讀，瞭解新的內容。或者參閱[歷史記錄和瀏覽器支持](#historysupport)，瞭解詳細信息。

Shadow DOM 是四大網絡組件標準之一：[HTML 模板](https://www.html5rocks.com/en/tutorials/webcomponents/template/)、[Shadow DOM](https://dom.spec.whatwg.org/#shadow-trees)、[自定義元素](/web/fundamentals/getting-started/primers/customelements)以及 [HTML 導入](https://www.html5rocks.com/en/tutorials/webcomponents/imports/)。

您無需編寫使用 shadow DOM 的網絡組件。但是如果您有編寫，可充分利用其各種優勢（CSS 作用域、DOM 封裝和組合），並構建可重複使用的[自定義元素](/web/fundamentals/getting-started/primers/customelements)，這些元素具有彈性、高度可配置且高度可重用。如果自定義元素是創建新 HTML（通過 JS API）的方式，shadow DOM 則是創建其 HTML 和 CSS 的方式。這兩種 API 組合使用，通過獨立的 HTML、CSS 和 JavaScript 來創建組件。

Shadow DOM 這款工具旨在構建基於組件的應用。因此，可爲網絡開發中的常見問題提供解決方案：

- **隔離 DOM**：組件的 DOM 是獨立的（例如，`document.querySelector()` 不會返回組件 shadow DOM 中的節點）。
- **作用域 CSS**：shadow DOM 內部定義的 CSS 在其作用域內。樣式規則不會泄漏，頁面樣式也不會滲入。
- **組合**：爲組件設計一個聲明性、基於標記的 API。
- **簡化 CSS** - 作用域 DOM 意味着您可以使用簡單的 CSS 選擇器，更通用的 id/類名稱，而無需擔心命名衝突。
- **效率** - 將應用看成是多個 DOM 塊，而不是一個大的（全局性）頁面。

Note: 儘管您可以在網絡組件之外利用 shadow DOM API 及其優勢，這裏我只列出一些基於自定義元素的示例。我將在所有示例中使用自定義元素 v1 API。

#### `fancy-tabs` 演示{: #demo}

在整篇文章中，我將引用演示組件 (`<fancy-tabs>`) 以及其中的代碼段。 如果您的瀏覽器支持 API，您可以看到下面的實時演示。 否則，請查看

<figure class="demoarea">
  <iframe
    style="height:360px;width:100%;border:none"
    src="https://rawgit.com/ebidel/2d2bb0cdec3f2a16cf519dbaa791ce1b/raw/fancy-tabs-demo.html">
  </iframe>
  <figcaption>
    <a href="https://gist.github.com/ebidel/2d2bb0cdec3f2a16cf519dbaa791ce1b" target="_blank">
      在 Github 上查看源代碼
    </a>
  </figcaption>
</figure>

## 什麼是 shadow DOM？{: #what}

#### DOM 相關背景{: #sdbackground}

<a href="https://gist.github.com/ebidel/2d2bb0cdec3f2a16cf519dbaa791ce1b" target="_blank">Github 上的完整源代碼</a>。

HTML 因其易於使用的特點驅動着網絡的發展。通過聲明幾個標記，即可在幾秒內編寫一個帶有圖文信息和結構的頁面。 但是，HTML 自身的功能並不強大。 對於我們人類而言，理解基於文本語言很容易，但是機器需要更多幫助才能理解。 因此，文檔對象模型 (DOM) 應運而生。

    const header = document.createElement('header');
    const h1 = document.createElement('h1');
    h1.textContent = 'Hello world!';
    header.appendChild(h1);
    document.body.appendChild(header);
    

瀏覽器加載網頁時會做一些很有趣的事情。其中之一就是它會將編寫的 HTML 轉變成活動文檔。爲理解頁面的結構，瀏覽器通常會將 HTML（靜態文本字符串）解析爲數據模型（對象/節點）。瀏覽器通過創建一個節點樹來保留 HTML 的層次結構：DOM。 DOM 很酷的一點在於它能夠生動地展示您的頁面。 與我們編寫的靜態 HTML 不同，瀏覽器生成的節點包含有屬性、方法，而且最棒的是可通過程序進行操作！這就是爲什麼我們直接使用 JavaScript 即可創建 DOM 元素的原因：

    <body>
      <header>
        <h1>Hello DOM</h1>
      </header>
    </body>
    

生成以下 HTML 標記：

#### 影子中的 DOM{: #sddom}

一切都還不錯。那麼，[究竟什麼是 *shadow DOM*](https://glazkov.com/2011/01/14/what-the-heck-is-shadow-dom/)？

## 創建 shadow DOM {: #create}

Shadow DOM 與普通 DOM 相同，但有兩點區別：1) 創建/使用的方式；2) 與頁面其他部分有關的行爲方式。 通常，您創建 DOM 節點並將其附加至其他元素作爲子項。 藉助於 shadow DOM，您可以創建作用域 DOM 樹，該 DOM 樹附加至該元素上，但與其自身真正的子項分離開來。這一作用域子樹稱爲**影子樹**。被附着的元素稱爲**影子宿主**。 您在影子中添加的任何項均將成爲宿主元素的本地項，包括 `<style>`。 這就是 shadow DOM 實現 CSS 樣式作用域的方式。

    const header = document.createElement('header');
    const shadowRoot = header.attachShadow({mode: 'open'});
    shadowRoot.innerHTML = '<h1>Hello Shadow DOM</h1>'; // Could also use appendChild().
    
    // header.shadowRoot === shadowRoot
    // shadowRoot.host === header
    

**影子根**是附加至“宿主”元素的文檔片段。元素通過附加影子根來獲取其 shadow DOM。 要爲元素創建 shadow DOM，請調用 `element.attachShadow()`：

我現在使用 `.innerHTML` 來填充影子根，不過您也可使用其他 DOM API 來實現。 這就是網絡。我們可自主選擇。

- The browser already hosts its own internal shadow DOM for the element (`<textarea>`, `<input>`).
- It doesn't make sense for the element to host a shadow DOM (`<img>`).

規範[定義了元素列表](http://w3c.github.io/webcomponents/spec/shadow/#h-methods)，這些元素無法託管影子樹， 元素之所以在所選之列，其原因如下：

    document.createElement('input').attachShadow({mode: 'open'});
    // Error. `<input>` cannot host shadow dom.
    

### 爲自定義元素創建 shadow DOM {: #elements}

例如，以下方法行不通：

創建[自定義元素](/web/fundamentals/getting-started/primers/customelements)時，Shadow DOM 尤其有用。使用 shadow DOM 來分隔元素的 HTML、CSS 和 JS，從而生成一個“網絡組件”。

    // Use custom elements API v1 to register a new HTML tag and define its JS behavior
    // using an ES6 class. Every instance of <fancy-tab> will have this same prototype.
    customElements.define('fancy-tabs', class extends HTMLElement {
      function Object() { [native code] }() {
        super(); // always call super() first in the constructor.
    
        // Attach a shadow root to <fancy-tabs>.
        const shadowRoot = this.attachShadow({mode: 'open'});
        shadowRoot.innerHTML = `
          <style>#tabs { ... }</style> <!-- styles are scoped to fancy-tabs! -->
          <div id="tabs">...</div>
          <div id="panels">...</div>
        `;
      }
      ...
    });
    

**例如** - 自定義元素**將 shadow DOM 附加至其自身**，對其 DOM/CSS 進行封裝：

這裏有幾個有趣的事情。首先，`<fancy-tabs>` 實例創建後，自定義元素**創建其自身的 shadow DOM**。這在 `function Object() { [native code] }()` 中完成。其次，因爲我們要創建一個影子根，因此 `<style>` 中的 CSS 規則將作用域僅限於 `<fancy-tabs>`。

## 組合和 slot{: #composition_slot}

Note: 嘗試運行該示例時，您可能會注意到沒有任何渲染。 用戶的標記似乎消失了！這是因爲**元素的 shadow DOM 代替其子項被渲染**。 如果想要顯示子項，您需要告訴瀏覽器在哪裏進行渲染，具體做法是在您的 shadow DOM 中添加 [`<slot>` 元素](#slots)。

[之後](#composition_slot)將會提供相關更多內容。

### 術語：light DOM 與 shadow DOM {: #lightdom}

組合是 shadow DOM 最難理解的功能之一，但可以說是最重要的功能。

**Light DOM**

Shadow DOM 組合引入了大量與網絡開發相關的新的基礎知識。 爲避免陷入迷茫，我們先標準化一些術語，這樣我們就能講同樣的行話。

    <button is="better-button">
      <!-- the image and span are better-button's light DOM -->
      <img src="gear.svg" slot="icon">
      <span>Settings</span>
    </button>
    

**Light DOM**

組件用戶編寫的標記。該 DOM 不在組件 shadow DOM 之內。 它是元素實際的子項。

    #shadow-root
      <style>...</style>
      <slot name="icon"></slot>
      <span id="wrapper">
        <slot>Button</slot>
      </span>
    

**Shadow DOM**

該 DOM 是由組件的作者編寫。Shadow DOM 對於組件而言是本地的，它定義內部結構、作用域 CSS 並封裝實現詳情。它還可定義如何渲染由組件使用者編寫的標記。

    <button is="better-button">
      #shadow-root
        <style>...</style>
        <slot name="icon">
          <img src="gear.svg" slot="icon">
        </slot>
        <slot>
          <span>Settings</span>
        </slot>
    </button>
    

### &lt;slot&gt; 元素{: #slots}

Shadow DOM composes different DOM trees together using the `<slot>` element. **Slots are placeholders inside your component that users *can* fill with their own markup**. By defining one or more slots, you invite outside markup to render in your component's shadow DOM. Essentially, you're saying *"Render the user's markup over here"*.

瀏覽器將用戶的 light DOM 分佈到您的 shadow DOM 的結果，對最終產品進行渲染。 扁平樹是指您在 DevTools 中最終看到的樹以及在頁面上渲染的對象。

Shadow DOM 使用 `<slot>` 元素將不同的 DOM 樹組合在一起。**Slot 是組件內部的佔位符，用戶*可以*使用自己的標記來填充**。

通過定義一個或多個 slot，您可將外部標記引入到組件的 shadow DOM 中進行渲染。 這相當於您在說“在此處渲染用戶的標記”。

    <!-- Default slot. If there's more than one default slot, the first is used. -->
    <slot></slot>
    
    <slot>Fancy button</slot> <!-- default slot with fallback content -->
    
    <slot> <!-- default slot entire DOM tree as fallback -->
      <h2>Title</h2>
      <summary>Description text</summary>
    </slot>
    

Note: Slot 是爲網絡組件創建“聲明性 API”的一種方法。它們混入到用戶的 DOM 中，幫助對整個組件進行渲染，從而**將不同的 DOM 樹組合在一起**。

如果 `<slot>` 引入了元素，則這些元素可“跨越” shadow DOM 的邊界。 這些元素稱爲**分佈式節點**。從概念上來看，分佈式節點似乎有點奇怪。 Slot 實際上並不移動 DOM；它們在 shadow DOM 內部的其他位置進行渲染。

    #shadow-root
      <div id="tabs">
        <slot id="tabsSlot" name="title"></slot>
      </div>
      <div id="panels">
        <slot id="panelsSlot"></slot>
      </div>
    

組件可在其 shadow DOM 中定義零個或多個 slot。Slot 可以爲空，或者提供回退內容。 如果用戶不提供 [light DOM](#lightdom) 內容，slot 將對其備用內容進行渲染。

    <fancy-tabs>
      <button slot="title">Title</button>
      <button slot="title" selected>Title 2</button>
      <button slot="title">Title 3</button>
      <section>content panel 1</section>
      <section>content panel 2</section>
      <section>content panel 3</section>
    </fancy-tabs>
    
    <!-- Using <h2>'s and changing the ordering would also work! -->
    <fancy-tabs>
      <h2 slot="title">Title</h2>
      <section>content panel 1</section>
      <h2 slot="title" selected>Title 2</h2>
      <section>content panel 2</section>
      <h2 slot="title">Title 3</h2>
      <section>content panel 3</section>
    </fancy-tabs>
    

您還可以創建**已命名 slot**。已命名 slot 是 shadow DOM 中用戶可通過名稱引用的特定槽。

    <fancy-tabs>
      #shadow-root
        <div id="tabs">
          <slot id="tabsSlot" name="title">
            <button slot="title">Title</button>
            <button slot="title" selected>Title 2</button>
            <button slot="title">Title 3</button>
          </slot>
        </div>
        <div id="panels">
          <slot id="panelsSlot">
            <section>content panel 1</section>
            <section>content panel 2</section>
            <section>content panel 3</section>
          </slot>
        </div>
    </fancy-tabs>
    

**例如** - `<fancy-tabs>` shadow DOM 中的已命名 slot：

## 設定樣式 {: #styling}

組件用戶對 `<fancy-tabs>` 的聲明類似於：

### 組件定義的樣式{: #host}

而且如果您很好奇，您會發現扁平樹看起來類似於：

- 外部頁面中的 CSS 選擇器不應用於組件內部。
- 內部定義的樣式也不會滲出。它們的作用域僅限於宿主元素。

注意，我們的組件可處理不同的配置，但是扁平的 DOM 樹保持不變。 我們還可以從 `<button>` 切換到 `<h2>`。 編寫此組件的目的在於處理不同類型的子項 - 如同 `<select>` 一樣。

有多種方式可設定網絡組件的樣式。使用 shadow DOM 的組件可通過主頁來設定樣式，定義其自己的樣式或提供鉤子（以 [CSS 自定義屬性](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)的形式）讓用戶替換默認值。

    #shadow-root
      <style>
        #panels {
          box-shadow: 0 2px 2px rgba(0, 0, 0, .3);
          background: white;
          ...
        }
        #tabs {
          display: inline-flex;
          ...
        }
      </style>
      <div id="tabs">
        ...
      </div>
      <div id="panels">
        ...
      </div>
    

請記住，shadow DOM 最有用的功能是**作用域 CSS**：

    #shadow-root
      <!-- Available in Chrome 54+ -->
      <!-- WebKit bug: https://bugs.webkit.org/show_bug.cgi?id=160683 -->
      <link rel="stylesheet" href="styles.css">
      <div id="tabs">
        ...
      </div>
      <div id="panels">
        ...
      </div>
    

**shadow DOM 內部使用的 CSS 選擇器在本地應用於組件。**。實踐中，這意味着我們可再次使用一般的 id/類名稱，而無需擔心在頁面其他位置有衝突。

<select multiple>
  <option>Do</option>
  <option selected>Re</option>
  <option>Mi</option>
  <option>Fa</option>
  <option>So</option>
</select>

**例如** - 在影子根中定義的樣式是本地的

樣式表的作用域也僅限於影子樹：

    <style>
    :host {
      display: block; /* by default, custom elements are display: inline */
      contain: content; /* CSS containment FTW. */
    }
    </style>
    

您可能想知道在您添加 `multiple` 屬性時，`<select>` 元素是如何渲染多選小部件（而不是下拉工具）的：

The functional form of `:host(<selector>)` allows you to target the host if it matches a `<selector>`. This is a great way for your component to encapsulate behaviors that react to user interaction or state or style internal nodes based on the host.

    <style>
    :host {
      opacity: 0.4;
      will-change: opacity;
      transition: opacity 300ms ease-in-out;
    }
    :host(:hover) {
      opacity: 1;
    }
    :host([disabled]) { /* style when host has disabled attribute. */
      background: grey;
      pointer-events: none;
      opacity: 0.4;
    }
    :host(.blue) {
      color: blue; /* color host when it has class="blue" */
    }
    :host(.pink) > #tabs {
      color: pink; /* color internal #tabs node when host has class="pink". */
    }
    </style>
    

### 基於情境設定樣式{: #contextstyling}

`<select>` 可基於您聲明的屬性爲*自身*設定不同的樣式。 網絡組件也可通過 `:host` 選擇器對自身進行樣式設定。

    <body class="darktheme">
      <fancy-tabs>
        ...
      </fancy-tabs>
    </body>
    

**例如** - 組件爲自身設定樣式

    :host-context(.darktheme) {
      color: white;
      background: black;
    }
    

使用 `:host` 的一個問題是，父頁面中的規則較之在元素中定義的 `:host` 規則具有更高的特異性。 也就是說，外部樣式優先。這可讓用戶從外部替換您的頂級樣式。 此外，`:host` 僅在影子根範圍內起作用，因此無法在 shadow DOM 之外使用。

### 爲分佈式節點設定樣式{: #stylinglightdom}

如果 `:host(<selector>)` 的函數形式與 `<selector>` 匹配，您可以指定宿主。 對於您的組件而言，這是一個很好的方法，它可讓您基於宿主將對用戶互動或狀態的反應行爲進行封裝，或對內部節點進行樣式設定。

如果 `:host-context(<selector>)` 或其任意父級與 `<selector>` 匹配，它將與組件匹配。 一個常見用途是根據組件的環境進行主題化。 例如，很多人都通過將類應用到 `<html>` 或 `<body>` 進行主題化：

    <name-badge>
      <h2>Eric Bidelman</h2>
      <span class="title">
        Digital Jedi, <span class="company">Google</span>
      </span>
    </name-badge>
    

如果 `:host-context(.darktheme)` 爲 `.darktheme` 的子級，它將對 `<fancy-tabs>` 進行樣式化：

    <style>
    ::slotted(h2) {
      margin: 0;
      font-weight: 300;
      color: red;
    }
    ::slotted(.title) {
       color: orange;
    }
    /* DOESN'T WORK (can only select top-level nodes).
    ::slotted(.company),
    ::slotted(.title .company) {
      text-transform: uppercase;
    }
    */
    </style>
    <slot></slot>
    

`:host-context()` 對於主題化很有用，但更好的方法是[使用 CSS 自定義屬性創建樣式鉤子](#stylehooks)。

`::slotted(<compound-selector>)` 與分佈到 `<slot>` 中的節點匹配。

    const shadowRoot = this.attachShadow({mode: 'open'});
    shadowRoot.innerHTML = `
      <style>
        #panels {
          box-shadow: 0 2px 2px rgba(0, 0, 0, .3);
          background: white;
          border-radius: 3px;
          padding: 16px;
          height: 250px;
          overflow: auto;
        }
        #tabs {
          display: inline-flex;
          -webkit-user-select: none;
          user-select: none;
        }
        #tabsSlot::slotted(*) {
          font: 400 16px/22px 'Roboto';
          padding: 16px 8px;
          ...
        }
        #tabsSlot::slotted([aria-selected="true"]) {
          font-weight: 600;
          background: white;
          box-shadow: none;
        }
        #panelsSlot::slotted([aria-hidden="true"]) {
          display: none;
        }
      </style>
      <div id="tabs">
        <slot id="tabsSlot" name="title"></slot>
      </div>
      <div id="panels">
        <slot id="panelsSlot"></slot>
      </div>
    `;
    

比如說我們已創建了一個 name badge 組件：

### 從外部爲組件設定樣式{: #stylefromoutside}

組件的 shadow DOM 可爲用戶的 `<h2>` 和 `.title` 設定樣式：

    fancy-tabs {
      width: 500px;
      color: red; /* Note: inheritable CSS properties pierce the shadow DOM boundary. */
    }
    fancy-tabs:hover {
      box-shadow: 0 3px 3px #ccc;
    }
    

如果您還記得前面的內容，就知道 `<slot>` 不會移動用戶的 light DOM。節點分佈於 `<slot>` 中後，`<slot>` 會對其 DOM 進行渲染，但節點實際上留在原處。**分佈之前已應用的樣式在分佈後仍繼續應用**。 但是，light DOM 分佈後，它*可以*採用其他樣式（通過 shadow DOM 定義的樣式）。

另一個來自 `<fancy-tabs>` 的更深入的例子：

#### 使用 CSS 自定義屬性創建樣式鉤子{: #stylehooks}

在該示例中，有兩個 slot：用於標籤標題的命名 slot，以及用於標籤內容的命名 slot。 用戶選擇一個標籤後，我們會對其選擇進行加粗並在面板上顯示。 這是通過選擇具有 `selected` 屬性的分佈式節點來實現的。 自定義元素的 JS（此處未顯示）會在合適的時間添加此屬性。

有幾種方法可從外部爲組件設定樣式：最簡單的方法是使用標記名稱作爲選擇器：

    <!-- main page -->
    <style>
      fancy-tabs {
        margin-bottom: 32px;
        --fancy-tabs-bg: black;
      }
    </style>
    <fancy-tabs background>...</fancy-tabs>
    

**外部樣式總是優先於在 shadow DOM 中定義的樣式**。例如，如果用戶編寫選擇器 `fancy-tabs { width: 500px; }`，它將優先於組件的規則：`:host { width: 650px;}`。

    :host([background]) {
      background: var(--fancy-tabs-bg, #9E9E9E);
      border-radius: 10px;
      padding: 10px;
    }
    

爲組件自身設定樣式只能到此爲止。但是如果您想要爲組件內容設定樣式，會發生什麼情況呢？ 對於這種情況，我們需要 CSS 自定義屬性。

如果組件的作者通過 [CSS 自定義屬性](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)提供樣式鉤子，則用戶可調整內部樣式。 從概念上看，這與 `<slot>` 類似。 您創建“樣式佔位符”以便用戶進行替換：

## 高級主題{: #advanced}

### 創建閉合影子根（應避免）{: #closed}

**例如** - `<fancy-tabs>` 可讓用戶替換背景顏色：

在其 shadow DOM 內部：

    const div = document.createElement('div');
    const shadowRoot = div.attachShadow({mode: 'closed'}); // close shadow tree
    // div.shadowRoot === null
    // shadowRoot.host === div
    

在本例中，該組件將使用 `black` 作爲背景值，因爲用戶指定了該值。 否則，背景顏色將採用默認值 `#9E9E9E`。

- `Element.assignedSlot` / `TextNode.assignedSlot` 返回 `null`
- `Event.composedPath()`，用於與 shadow DOM 內部元素關聯的事件，返回 []

Note: 作爲組件的作者，您負責讓開發者瞭解他們所能使用的 CSS 自定義屬性。 將其看成是組件公共接口的一部分。 確保將樣式鉤子記錄下來！

shadow DOM 的另一情況稱爲“閉合”模式。創建閉合影子樹後，在 JavaScript 外部無法訪問組件的內部 DOM。這與 `<video>` 等原生元素工作方式類似。JavaScript 無法訪問 `<video>` 的 shadow DOM，因爲瀏覽器使用閉合模式的影子根來實現。

1. 人爲的安全功能。沒有什麼能夠阻止攻擊者入侵 `Element.prototype.attachShadow`。

2. 閉合模式**阻止自定義元素代碼訪問其自己的 shadow DOM**。 這根本沒用。相反，如果您想要使用如 `querySelector()` 等元素，您必須存放影子根以備日後參考。 這就與閉合模式的最初目的完全背道而馳！
    
        customElements.define('x-element', class extends HTMLElement {
          function Object() { [native code] }() {
            super(); // always call super() first in the constructor.
            this._shadowRoot = this.attachShadow({mode: 'closed'});
            this._shadowRoot.innerHTML = '<div class="wrapper"></div>';
          }
          connectedCallback() {
            // When creating closed shadow trees, you'll need to stash the shadow root
            // for later if you want to use it again. Kinda pointless.
            const wrapper = this._shadowRoot.querySelector('.wrapper');
          }
          ...
        });
        

3. **閉合模式使組件對最終用戶的靈活性大爲降低**。在構建網絡組件時，您有時可能會忘記添加某項功能、某個配置選項以及用戶所需的用例。一個很常見的例子是忘記爲內部節點添加足夠的樣式鉤子。在閉合模式下，用戶無法替換默認值並調整樣式。 如果能訪問組件的內容，這將超級有用。最終，如果用戶得不到他們想要的，他們就會捨棄您的組件，尋找其他組件或創建自己的組件:(

### 在 JS 中使用 slot{: #workwithslots}

**例如** - 創建一個閉合的影子樹：

#### slotchange 事件{: #slotchange}

其他 API 也會受到閉合模式的影響：

    const slot = this.shadowRoot.querySelector('#slot');
    slot.addEventListener('slotchange', e => {
      console.log('light dom children changed!');
    });
    

Note: 閉合的影子樹不是非常有用。有些開發者將閉合模式視爲一項人工安全功能。 但是讓我們澄清一點，它並**不是**一項安全功能。 閉合模式只是簡單地阻止外部 JS 深入到元素的內部 DOM。

任何時候都不要使用 `{mode: 'closed'}` 來創建網絡組件，以下是我總結的幾點原因：

#### 哪些元素在 slot 中進行渲染？{: #slotnodes}

shadow DOM API 提供了使用 slot 和分佈式節點的實用程序。 這些實用程序在編寫自定義元素時遲早派得上用場。

當 slot 的分佈式節點發生變化時，`slotchange` 事件會觸發。例如，當用戶從 light DOM 中添加/移除子項時。

    <slot><b>fallback content</b></slot>
    

<table>
  <thead><th>用法</th><th>調用</th><th>結果</th></thead>
  <tr>
    <td>&lt;button is="better-button"&gt;My button&lt;/button&gt;</td>
    <td><code>slot.assignedNodes();</code></td>
    <td><code>[text]</code></td>
  </tr>
  <tr>
    <td>&lt;button is="better-button">&lt;/button&gt;</td>
    <td><code>slot.assignedNodes();</code></td>
    <td><code>[]</code></td>
  </tr>
  <tr>
    <td>&lt;button is="better-button"&gt;&lt;/button&gt;</td>
    <td><code>slot.assignedNodes({flatten: true});</code></td>
    <td><code>[&lt;b&gt;fallback content&lt;/b&gt;]</code></td>
  </tr>
</table>

#### 元素分配給哪個 Slot？{: #assignedslot}

Note: 當組件的實例首次初始化時，`slotchange` 不觸發。

### Shadow DOM 事件模型{: #events}

如要監控 light DOM 其他類型的變化，您可以在元素的構造函數中設置 [`MutationObserver`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)。

有時候，瞭解哪些元素與 slot 相關聯非常有用。調用 `slot.assignedNodes()` 可查看 slot 正在渲染哪些元素。 `{flatten: true}` 選項將返回 slot 的備用內容（前提是沒有分佈任何節點）。

- 聚焦事件：`blur`、`focus`、`focusin`、`focusout`
- 鼠標事件：`click`、`dblclick`、`mousedown`、`mouseenter`、`mousemove`，等等
- 滾輪事件：`wheel`
- 輸入事件：`beforeinput`、`input`
- 鍵盤事件：`keydown`、`keyup`
- 組合事件：`compositionstart`、`compositionupdate`、`compositionend`
- 拖放事件：`dragstart`、`drag`、`dragend`、`drop`，等等

**Tips**

這個反向問題也是可以回答的。`element.assignedSlot` 將告訴您元素分配給哪個組件 slot。

#### 使用自定義事件{: #customevents}

當事件從 shadow DOM 中觸發時，其目標將會調整爲維持 shadow DOM 提供的封裝。 也就是說，事件的目標重新進行了設定，因此這些事件看起來像是來自組件，而不是來自 shadow DOM 中的內部元素。

    // Inside <fancy-tab> custom element class definition:
    selectTab() {
      const tabs = this.shadowRoot.querySelector('#tabs');
      tabs.dispatchEvent(new Event('tab-select', {bubbles: true, composed: true}));
    }
    

有些事件甚至不會從 shadow DOM 中傳播出去。

    <fancy-tabs></fancy-tabs>
    <script>
      const tabs = document.querySelector('fancy-tabs');
      tabs.addEventListener('tab-select', e => {
        // won't fire if `tab-select` wasn't created with `composed: true`.
      });
    </script>
    

### 處理焦點{: #focus}

**確實**會跨過影子邊界的事件有：

    <x-focus>
      #shadow-root
        <input type="text" placeholder="Input inside shadow dom">
    

The `focus` event will look like it came from `<x-focus>`, not the `<input>`. Similarly, `document.activeElement` will be `<x-focus>`. If the shadow root was created with `mode:'open'` (see [closed mode](#closed)), you'll also be able access the internal node that gained focus:

    document.activeElement.shadowRoot.activeElement // only works with open mode.
    

如果影子樹處於打開狀態，調用 `event.composedPath()` 將返回事件經過的一組節點。

    function deepActiveElement() {
      let a = document.activeElement;
      while (a && a.shadowRoot && a.shadowRoot.activeElement) {
        a = a.shadowRoot.activeElement;
      }
      return a;
    }
    

通過影子樹中內部節點觸發的自定義 DOM 事件不會超出影子邊界，除非事件是使用 `composed: true` 標記創建的：

- 如果您點擊 shadow DOM 內的某個節點，且該節點不是一個可聚焦區域，那麼第一個可聚焦區域將成爲焦點。
- 當 shadow DOM 內的節點獲得焦點時，除了聚焦的元素外，`:focus` 還會應用到宿主。

如果是 `composed: false`（默認值），用戶無法偵聽到影子根之外的事件。

    <style>
      :focus {
        outline: 2px solid red;
      }
    </style>
    
    <x-focus></x-focus>
    
    <script>
    customElements.define('x-focus', class extends HTMLElement {
      function Object() { [native code] }() {
        super(); // always call super() first in the constructor.
    
        const root = this.attachShadow({mode: 'open', delegatesFocus: true});
        root.innerHTML = `
          <style>
            :host {
              display: flex;
              border: 1px dotted black;
              padding: 16px;
            }
            :focus {
              outline: 2px solid blue;
            }
          </style>
          <div>Clickable Shadow DOM text</div>
          <input type="text" placeholder="Input inside shadow dom">`;
    
        // Know the focused element inside shadow DOM:
        this.addEventListener('focus', function(e) {
          console.log('Active element (inside shadow dom):',
                      this.shadowRoot.activeElement);
        });
      }
    });
    </script>
    

**Result**

<img src="imgs/delegateFocusTrue.png" title="delegatesFocus: true behavior" />

如果存在多個級別的 shadow DOM（即自定義元素位於另一個自定義元素中），您需要以遞歸方式深入影子根以查找 `activeElement`：

焦點的另一個選項是 `delegatesFocus: true` 選項，它可以將元素的焦點行爲拓展到影子樹內：

<figure>
  <img src="imgs/delegateFocusFalse.png">
  <figcaption>
    <code>delegatesFocus: false</code> 和內部  <code>&lt;input></code> 獲得焦點。
  </figcaption>
</figure>

<figure>
  <img src="imgs/delegateFocusFalseFocus.png">
  <figcaption>
    <code>delegatesFocus: false</code> 和  <code>&lt;x-focus></code> 獲得焦點（例如， <code>tabindex="0"</code>）。
  </figcaption>
</figure>

<figure>
  <img src="imgs/delegateFocusNothing.png">
  <figcaption>
    <code>delegatesFocus: false</code> 並且點擊“Clickable Shadow DOM text”（或點擊元素 shadow DOM 內的其他空白區域）。
  </figcaption>
</figure>

## 提示與技巧{: #tricks}

**示例** - `delegatesFocus: true` 如何更改焦點行爲

### 使用 CSS 組件{: #containment}

Typically, a web component's layout/style/paint is fairly self-contained. Use [CSS containment](/web/updates/2016/06/css-containment) in `:host` for a perf win:

    <style>
    :host {
      display: block;
      contain: content; /* Boom. CSS containment FTW. */
    }
    </style>
    

### 重置可繼承樣式{: #reset}

Inheritable styles (`background`, `color`, `font`, `line-height`, etc.) continue to inherit in shadow DOM. That is, they pierce the shadow DOM boundary by default. If you want to start with a fresh slate, use `all: initial;` to reset inheritable styles to their initial value when they cross the shadow boundary.

    <style>
      div {
        padding: 10px;
        background: red;
        font-size: 25px;
        text-transform: uppercase;
        color: white;
      }
    </style>
    
    <div>
      <p>I'm outside the element (big/white)</p>
      <my-element>Light DOM content is also affected.</my-element>
      <p>I'm outside the element (big/white)</p>
    </div>
    
    <script>
    const el = document.querySelector('my-element');
    el.attachShadow({mode: 'open'}).innerHTML = `
      <style>
        :host {
          all: initial; /* 1st rule so subsequent properties are reset. */
          display: block;
          background: white;
        }
      </style>
      <p>my-element: all CSS properties are reset to their
         initial value using <code>all: initial</code>.</p>
      <slot></slot>
    `;
    </script>
    

上面是 `<x-focus>` 獲得焦點（用戶點擊、點按和 `focus()` 等）、點擊“Clickable Shadow DOM text”或內部 `<input>` 獲得焦點（包括 `autofocus`）時的結果。

<div class="demoarea">
  <style>
    #initialdemo {
      padding: 10px;
      background: red;
      font-size: 25px;
      text-transform: uppercase;
      color: white;
    }
  </style>

  <div id="initialdemo">
    <p>I'm outside the element (big/white)</p>
    <my-element>Light DOM content is also affected.</my-element>
    <p>I'm outside the element (big/white)</p>
  </div>
</div>

<script>
function supportsShadowDOM() {
  return !!HTMLElement.prototype.attachShadow;
}

if (supportsShadowDOM()) {
  const el = document.querySelector('#initialdemo my-element');
  el.attachShadow({mode: 'open'}).innerHTML = `
    <style>
      :host {
        all: initial; /* 1st rule so subsequent properties are reset. */
        display: block;
        background: white;
      }
    </style>
    <p>my-element: all CSS properties are reset to their
       initial value using <code>all: initial</code>.</p>
    <slot></slot>
  `;
} else {
  if (self.frameElement) {
    self.frameElement.style.display = 'none';
  }
}
 </script>

 

如果是設置 `delegatesFocus: false`，下面將是您看到的結果：

### 查找頁面所使用的所有自定義元素{: #findall}

這些年，我學到了一些關於編寫網絡組件的技巧。我覺得這些技巧對於編寫組件和調試 shadow DOM 會比較有用。

    const allCustomElements = [];
    
    function isCustomElement(el) {
      const isAttr = el.getAttribute('is');
      // Check for <super-button> and <button is="super-button">.
      return el.localName.includes('-') || isAttr && isAttr.includes('-');
    }
    
    function findAllCustomElements(nodes) {
      for (let i = 0, el; el = nodes[i]; ++i) {
        if (isCustomElement(el)) {
          allCustomElements.push(el);
        }
        // If the element has shadow DOM, dig deeper.
        if (el.shadowRoot) {
          findAllCustomElements(el.shadowRoot.querySelectorAll('*'));
        }
      }
    }
    
    findAllCustomElements(document.querySelectorAll('*'));
    

### 使用 &lt;template> 創建元素{: #fromtemplate}

通常，網絡組件的佈局/樣式/繪製相當獨立。在 `:host` 中使用 [CSS containment](/web/updates/2016/06/css-containment) 可獲得更好性能：

可繼承樣式（`background`、`color`、`font` 以及 `line-height` 等）可在 shadow DOM 中繼續繼承。 也就是說，默認情況下它們會突破 shadow DOM 邊界。 如果您想從頭開始，可在它們超出影子邊界時，使用 `all: initial;` 將可繼承樣式重置爲初始值。

## 歷史記錄和瀏覽器支持{: #historysupport}

{% framebox height="195px" %}

{% endframebox %}

### 瀏覽器支持{: #support}

有時，查找頁面所使用的自定義元素非常有用。爲此，您需要遞歸地遍歷頁面所使用的所有元素的 shadow DOM。

{% comment %} Some browsers also support using shadow DOM v0's `/deep/` combinator in `querySelectorAll()`:

    const allCustomElements = Array.from(document.querySelectorAll('html /deep/ *')).filter(el => {
      const isAttr = el.getAttribute('is');
      return el.localName.includes('-') || isAttr && isAttr.includes('-');
    });
    

#### Polyfill {: #polyfill}

For now, `/deep/` [continues to work in `querySelectorAll()` calls](https://bugs.chromium.org/p/chromium/issues/detail?id=633007). {% endcomment %}

我們不是使用 `.innerHTML` 來填充影子根，而是使用一個聲明性 `<template>`。 模板是用於聲明網絡組件結構的理想佔位符。

    const supportsShadowDOMV1 = !!HTMLElement.prototype.attachShadow;
    

具體請參見[“自定義元素：構建可重複使用的網絡組件”](/web/fundamentals/getting-started/primers/customelements)中的示例。

    bower install --save webcomponents/shadydom
    bower install --save webcomponents/shadycss
    

如果最近幾年您一直在關注網絡組件，您會發現有一段時間 Chrome 35+/Opera 隨附的是舊版本 shadow DOM。Blink 將繼續在一段時間內同時支持新舊兩種版本。 v0 規範提供了創建影子根的不同方法（`element.createShadowRoot`，而不是 v1 的 `element.attachShadow`）。 調用舊方法仍可通過 v0 語法來創建影子根，因此現有的 v0 代碼不會出錯。

## 結論

如果您想了解舊版 v0 規範，可查看 html5rocks 文章：[1](https://www.html5rocks.com/en/tutorials/webcomponents/shadowdom/)、[2](https://www.html5rocks.com/en/tutorials/webcomponents/shadowdom-201/)、[3](https://www.html5rocks.com/en/tutorials/webcomponents/shadowdom-301/)。[shadow DOM v0 與 v1 的差異](http://hayato.io/2016/shadowdomv1/)中也提供了大量的二者比較信息。

Chrome 53（[狀態](https://www.chromestatus.com/features/4667415417847808)）、Opera 40 和 Safari 10 隨附的是 shadow DOM v1。 Edge 在考慮中，但[優先級很高](https://developer.microsoft.com/en-us/microsoft-edge/platform/status/shadowdom/)。Mozilla 需要處理一個[未解決的錯誤](https://bugzilla.mozilla.org/show_bug.cgi?id=811542)。

#### 深入閱讀

- [Shadow DOM v1 與 v0 的差異](http://hayato.io/2016/shadowdomv1/)
- [“基於 Slot 的 Shadow DOM API 簡介”](https://webkit.org/blog/4096/introducing-shadow-dom-api/)（出自 WebKit 博客）。
- [網絡組件和模塊化 CSS 之未來](https://philipwalton.github.io/talks/2015-10-26/)（作者：[Philip Walton](https://twitter.com/@philwalton)）
- [“自定義元素：構建可重複使用的網絡組件”](/web/fundamentals/getting-started/primers/customelements)（出自：Google 的 WebFundamentals）。
- [Shadow DOM v1 規範](https://dom.spec.whatwg.org/#shadow-trees)
- [自定義元素 v1 規範](https://html.spec.whatwg.org/multipage/scripting.html#custom-elements)

## 常見問題解答

**Can I use Shadow DOM v1 today?**

在瀏覽器提供廣泛支持前，[shadydom](https://github.com/webcomponents/shadydom) 和 [shadycss](https://github.com/webcomponents/shadycss) polyfill 可以爲您提供 v1 功能。Shady DOM 可以模擬 Shadow DOM 的 DOM 作用域，而 shadycss polyfill 則可以模擬原生 API 提供的 CSS 自定義屬性和樣式作用域。

**What security features does shadow DOM provide?**

使用 polyfill：

**Does a web component have to use shadow DOM?**

有史以來第一次，我們擁有了實施適當 CSS 作用域、DOM 作用域的 API 原語，並且有真正意義上的組合。 與自定義元素等其他網絡組件 API 組合後，shadow DOM 提供了一種編寫真正封裝組件的方法，無需花多大的功夫或使用如 `<iframe>` 等陳舊的東西。

**What's the difference between open and closed shadow roots?**

See [Closed shadow roots](#closed).

## Feedback {: #feedback }

如果有 polyfill，那麼是的，您可以使用。請參見[瀏覽器支持](#support)。