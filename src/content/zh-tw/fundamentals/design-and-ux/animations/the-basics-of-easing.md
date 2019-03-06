project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: 了解如何柔化並為您的動畫加權。

{# wf_updated_on: 2014-10-20 #} {# wf_published_on: 2014-08-08 #}

# 緩動基礎 {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}

自然界中沒有樣一東西是從一個點直線移動到另一個點。 在現實中，事物在移動時總會加速或減速。 我們的大腦設計會期待這類動作，所以在動畫處理時，應該加以利用它。 自然動作會讓您的使用者更適應您的應用程式，反過來會帶來更好的整體使用者體驗。

### TL;DR {: .hide-from-toc }

* 緩動會讓您的動畫感覺更自然。
* 為 UI 元素選擇緩出動畫。
* 除非您可以保持動畫持續時間短，否則請避免緩動或緩入緩出動畫；對最終使用者而言，它們往往令人感到遲鈍。

在傳統動畫中，慢慢開始並加快的運動之字眼是「慢入」，而快速開始並減速的動作被稱為「慢出」，但網頁上最常用的字眼分別是「緩入」和「緩出」。 有時兩者合併在一起，稱為「緩入緩出」。 因此，緩動事實上是指讓動畫降低劇烈或顯著感的過程。

## 緩動關鍵字

CSS 轉換和動畫均可讓您 [針對您的動畫選擇想要的緩動種類](choosing-the-right-easing)。 您可以使用影響動畫的緩動 (或有時候稱為計時) 之關鍵字。 您還可以 [完全自訂您的緩動](custom-easing)，給您更多自由以表達您應用程式的個性。

以下是您可以在 CSS 中使用的關鍵字：

* `linear`
* `ease-in`
* `ease-out`
* `ease-in-out`

資料來源： [CSS Transitions, W3C](http://www.w3.org/TR/css3-transitions/#transition-timing-function-property)

您也可以使用一個 `steps` 關鍵字，讓您建立具有分離步驟的轉換，但以上的關鍵字對於建立感覺自然的動畫才最實用，而這正是您想要達成的。

## 線性動畫

<div class="attempt-right">
  <figure>
    <img src="images/linear.png" alt="Linear ease animation curve." />
  </figure>
</div>

不帶任何緩動的動畫被稱為 **線性**。 線性轉換的圖形看似這樣：

As time moves along, the value increases in equal amounts. With linear motion, things tend to feel robotic and unnatural, and this is something that users find jarring. Generally speaking, you should avoid linear motion.

Whether you’re coding your animations using CSS or JavaScript, you’ll find that there is always an option for linear motion.

隨著時間的推移，值會以等量增加。 直線動作的東西往往感覺很機器化且不自然，而這是使用者會覺得格格不入的東西。 一般而言，您應該要避免直線動作。

<div style="clear:both;"></div>

無論您是以 CSS 或 JavaScript 來編寫動畫，您會發現總存在著直線動作的選項。 要以 CSS 達成以上效果，程式碼必須如下所示：

    transition: transform 500ms linear;
    

## 緩出動畫

<div class="attempt-right">
  <figure>
    <img src="images/ease-out.png" alt="Ease-out animation curve." />
  </figure>
</div>

緩出會使動畫比直線動畫更快速地開始，但結尾會減速。

Easing out is typically the best for user interface work, because the fast start gives your animations a feeling of responsiveness, while still allowing for a natural slowdown at the end.

有多種途徑可以達成緩出效果，但最簡單的是 CSS 中的 `ease-out` 關鍵字：

<div style="clear:both;"></div>

There are many ways to achieve an ease out effect, but the simplest is the `ease-out` keyword in CSS:

    transition: transform 500ms ease-out;
    

## 緩入動畫

<div class="attempt-right">
  <figure>
    <img src="images/ease-in.png" alt="Ease-in animation curve." />
  </figure>
</div>

緩出通常最適合使用者介面的工作，因為快速開始給您的動畫回應性的感覺，同時可在結束時允許些許自然減速。

緩入動畫是慢慢開始，快速結束；緩出則相反。

From an interaction point of view, however, ease-ins can feel a little unusual because of their abrupt end; things that move in the real world tend to decelerate rather than simply stopping suddenly. Ease-ins also have the detrimental effect of feeling sluggish when starting, which negatively impacts the perception of responsiveness in your site or app.

[See an ease-in animation](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/animations/box-move-ease-in.html){: target="_blank" .external }

<div style="clear:both;"></div>

這種動畫就像一塊沉重的石頭落下，它慢慢地開始，並砰地一聲快速落地。

    transition: transform 500ms ease-in;
    

## 緩入緩出動畫

<div class="attempt-right">
  <figure>
    <img src="images/ease-in-out.png" alt="Ease-in-out animation curve." />
  </figure>
</div>

類似緩出和線性動畫，要使用緩入動畫，您可以使用它的關鍵字：

然而從互動的角度來看，緩入因為其突兀的結束而感覺有點不尋常；現實世界中移動的東西傾向於減速，而非突然停止不動。 緩入也有一開始就感覺遲鈍的不利影響，而這會負面影響在您網站或應用程式給人的回應能力印象。

緩入緩出類似一輛汽車加速和減速，若是明智使用，可以提供比純緩出更戲劇性的效果。

<div style="clear:both;"></div>

To get an ease-in-out animation, you can use the `ease-in-out` CSS keyword:

    transition: transform 500ms ease-in-out;
    

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}