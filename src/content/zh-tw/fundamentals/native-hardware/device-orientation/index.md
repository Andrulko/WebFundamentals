project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: 裝置動作和定位事件能存取行動裝置中內建的加速計、陀螺儀和羅盤。

{# wf_updated_on: 2014-10-20 #} {# wf_published_on: 2000-01-01 #}

# 裝置定向 {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

裝置動作和定位事件能存取行動裝置中內建的加速計、陀螺儀和羅盤。

這些事件可用於多種用途； 例如在遊戲中要控制角色的方向，或判斷角色應該跳多高。 當用於 GeoLocation 時，它可以建立更精確的逐向式導航系統， 或提供有關商店所在的資訊。

Note: 決定要使裝置動作或裝置定向事件時，請**極為**謹慎。 遺憾的是，並非所有瀏覽器都使用相同的座標系統，並可能會在類似情況下回報不同的值。

## 哪一端朝上？

* **X：**代表東西方向 (其中東是正值)。
* **Y：**代表南北方向 (北是正值)。

## 裝置定向

要使用裝置定向和動作事件所傳回的資料， 理解所提供的值很重要。

### 地球座標視框。

以 `X`、`Y` 與 `Z` 值所描述的地球座標視框， 是根據重力和標準的磁性定向對準。

<table class="responsive">
  
<tr><th colspan="2">Coordinate system</th></tr>
<tr>
  <td><code>X</code></td>
  <td>Represents the east-west direction (where east is positive).</td>
</tr>
<tr>
  <td><code>Y</code></td>
  <td>Represents the north-south direction (where north is positive).</td>
</tr>
<tr>
  <td><code>Z</code></td>
  <td>Represents the up-down direction, perpendicular to the ground
      (where up is positive).
  </td>
</tr>
</table>

### 裝置座標框架的圖例

<div class="attempt-right">
  <figure id="fig1">
    <img src="images/axes.png" alt="illustration of device coordinate frame">
    <figcaption>
      Illustration of device coordinate frame
    </figcaption>
  </figure>
</div>

<!-- Special thanks to Sheppy (https://developer.mozilla.org/en-US/profiles/Sheppy)
  for his images which are in the public domain. -->

以 `x`、`y` 與 `z` 值所描述的裝置座標框架， 是根據裝置中心對準。

<table class="responsive">
  
<tr><th colspan="2">Coordinate system</th></tr>
<tr>
  <td><code>X</code></td>
  <td>In the plane of the screen, positive to the right.</td>
</tr>
<tr>
  <td><code>Y</code></td>
  <td>In the plane of the screen, positive towards the top.</td>
</tr>
<tr>
  <td><code>Z</code></td>
  <td>Perpendicular to the screen or keyboard, positive extending
    away.
  </td>
</tr>
</table>

On a phone or tablet, the orientation of the device is based on the typical orientation of the screen. For phones and tablets, it is based on the device being in portrait mode. For desktop or laptop computers, the orientation is considered in relation to the keyboard.

### 旋轉資料

在手機或平板電腦上， 裝置定向是基於螢幕的典型定向。螢幕方向螢幕方向 對於手機和平板電腦而言， 它則是基於垂直模式下的裝置。 對於桌上型電腦或筆記型電腦， 定向是考慮與鍵盤的相對位置。

#### 範例：計算物件的最大加速度

<div class="attempt-right">
  <figure id="fig1">
    <img src="images/alpha.png" alt="illustration of device coordinate frame">
    <figcaption>
      Illustration of alpha in the device coordinate frame
    </figcaption>
  </figure>
</div>

旋轉資料會被傳回為 \[尤拉角\] (http://en.wikipedia.org/wiki/Euler_angles){: .external}， 代表裝置的座標框架與地球座標框架之間差異的角度數字。

<div style="clear:both;"></div>

#### Beta

<div class="attempt-right">
  <figure id="fig1">
    <img src="images/beta.png" alt="illustration of device coordinate frame">
    <figcaption>
      Illustration of beta in the device coordinate frame
    </figcaption>
  </figure>
</div>

裝置定向事件會傳回旋轉資料，此資料包含裝置的前後與左右傾斜程度，以及電話或筆記型電腦是否具有羅盤、裝置面對的方向。

<div style="clear:both;"></div>

#### Gamma

<div class="attempt-right">
  <figure id="fig1">
    <img src="images/gamma.png" alt="illustration of device coordinate frame">
    <figcaption>
      Illustration of gamma in the device coordinate frame
    </figcaption>
  </figure>
</div>

裝置定向事件有幾個用途。 例如：

<div style="clear:both;"></div>

## 裝置動作

若要接聽 `DeviceOrientationEvent`， 首先請查看瀏覽器是否支援這些事件。 然後，將事件接聽器附加到 `window` 物件，接聽 `deviceorientation` 物件。

當裝置移動或變更方向時， 裝置定位事件會觸發。 它會傳回裝置在目前位置相對於 [ 之間差異的資料 地球座標視框](index.html#earth-coordinate-frame)。

### TL;DR {: .hide-from-toc }

此事件通常會傳回三個屬性，
<a href="index.html#rotation-data"><code>alpha</code></a>、
<a href="index.html#rotation-data"><code>beta</code></a>和
<a href="index.html#rotation-data"><code>gamma</code></a>。 在 Mobile Safari 上， 額外參數[`webkitCompassHeading`](https://developer.apple.com/library/safari/documentation/SafariDOMAdditions/Reference/DeviceOrientationEventClassRef/DeviceOrientationEvent/DeviceOrientationEvent.html)會與羅盤標題一起傳回 。

* **x：**在螢幕平面上，往右為正值。
* **y：**在螢幕平面上，往上為正值。
* **z：**垂直於螢幕或鍵盤， 遠離為正值。

### 使用裝置定位事件的時機

裝置動作提供在特定時刻套用在裝置身上的加速度，以及旋轉速度的資訊。

    if (window.DeviceOrientationEvent) {
      window.addEventListener('deviceorientation', deviceOrientationHandler, false);
      document.getElementById("doeSupported").innerText = "Supported!";
    }
    

### 查看支援並接聽事件

裝置動作事件有有幾個用途。 例如：

要接聽 `DeviceMotionEvent`， 首先查看瀏覽器是否支援這些事件。 然後，將事件接聽器附加到 `window` 物件，接聽 `devicemotion` 物件。

## Device motion

裝置動作事件會以規律周期觸發， 並及時在當下傳回關於裝置的旋轉 (每秒 &deg; 次) 和加速度 (單位為 m/sec<sup>2</sup>) 的資料。 某些裝置沒有必要硬體， 可以排除重力的作用。

此事件會返回四個屬性、
<a href="index.html#device-frame-coordinate"><code>accelerationIncludingGravity</code></a>、
<a href="index.html#device-frame-coordinate"><code>acceleration</code></a>
 (排除重力的影響)、
<a href="index.html#rotation-data"><code>rotationRate</code></a>和`interval`。

### 處理裝置定位事件

例如，讓我們看看手機放在平坦桌面上， 螢幕朝上。

* 請保守使用。
* 支援測試。
* 不要每次定向事件就更新 UI；反之請同步至 requestAnimationFrame。

### TL;DR {: .hide-from-toc }

反之，如果手持電話時，螢幕垂直於地面， 並直接面對檢視者：

    if (window.DeviceMotionEvent) {
      window.addEventListener('devicemotion', deviceMotionHandler);
      setTimeout(stopJump, 3*1000);
    }
    

### 使用裝置動作事件的時機

使用裝置動作事件的一種方法是計算物件的最大加速度。 例如，一個人跳躍的最大加速度為何。

在點選「執行！」按鈕後，使用者被告知要跳躍！在這段時間中， 頁面會儲存最大 (和最小) 的加速度值，並在跳躍後， 告知使用者他們的最大加速度。

For example, let's take a look at a phone, lying on a flat table, with its screen facing up.

<table>
  <thead>
    <tr>
      <th data-th="State">State</th>
      <th data-th="Rotation">Rotation</th>
      <th data-th="Acceleration (m/s<sup>2</sup>)">Acceleration (m/s<sup>2</sup>)</th>
      <th data-th="Acceleration with gravity (m/s<sup>2</sup>)">Acceleration with gravity (m/s<sup>2</sup>)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="State">Not moving</td>
      <td data-th="Rotation">[0, 0, 0]</td>
      <td data-th="Acceleration">[0, 0, 0]</td>
      <td data-th="Acceleration with gravity">[0, 0, 9.8]</td>
    </tr>
    <tr>
      <td data-th="State">Moving up towards the sky</td>
      <td data-th="Rotation">[0, 0, 0]</td>
      <td data-th="Acceleration">[0, 0, 5]</td>
      <td data-th="Acceleration with gravity">[0, 0, 14.81]</td>
    </tr>
    <tr>
      <td data-th="State">Moving only to the right</td>
      <td data-th="Rotation">[0, 0, 0]</td>
      <td data-th="Acceleration">[3, 0, 0]</td>
      <td data-th="Acceleration with gravity">[3, 0, 9.81]</td>
    </tr>
    <tr>
      <td data-th="State">Moving up and to the right</td>
      <td data-th="Rotation">[0, 0, 0]</td>
      <td data-th="Acceleration">[5, 0, 5]</td>
      <td data-th="Acceleration with gravity">[5, 0, 14.81]</td>
    </tr>
  </tbody>
</table>

Conversely, if the phone were held so the screen was perpendicular to the ground, and was directly visible to the viewer:

<table>
  <thead>
    <tr>
      <th data-th="State">State</th>
      <th data-th="Rotation">Rotation</th>
      <th data-th="Acceleration (m/s<sup>2</sup>)">Acceleration (m/s<sup>2</sup>)</th>
      <th data-th="Acceleration with gravity (m/s<sup>2</sup>)">Acceleration with gravity (m/s<sup>2</sup>)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="State">Not moving</td>
      <td data-th="Rotation">[0, 0, 0]</td>
      <td data-th="Acceleration">[0, 0, 0]</td>
      <td data-th="Acceleration with gravity">[0, 9.81, 0]</td>
    </tr>
    <tr>
      <td data-th="State">Moving up towards the sky</td>
      <td data-th="Rotation">[0, 0, 0]</td>
      <td data-th="Acceleration">[0, 5, 0]</td>
      <td data-th="Acceleration with gravity">[0, 14.81, 0]</td>
    </tr>
    <tr>
      <td data-th="State">Moving only to the right</td>
      <td data-th="Rotation">[0, 0, 0]</td>
      <td data-th="Acceleration">[3, 0, 0]</td>
      <td data-th="Acceleration with gravity">[3, 9.81, 0]</td>
    </tr>
    <tr>
      <td data-th="State">Moving up and to the right</td>
      <td data-th="Rotation">[0, 0, 0]</td>
      <td data-th="Acceleration">[5, 5, 0]</td>
      <td data-th="Acceleration with gravity">[5, 14.81, 0]</td>
    </tr>
  </tbody>
</table>

### 查看支援並接聽事件

One way to use device motion events is to calculate the maximum acceleration of an object. For example, what's the maximum acceleration of a person jumping?

    if (evt.acceleration.x > jumpMax.x) {
      jumpMax.x = evt.acceleration.x;
    }
    if (evt.acceleration.y > jumpMax.y) {
      jumpMax.y = evt.acceleration.y;
    }
    if (evt.acceleration.z > jumpMax.z) {
      jumpMax.z = evt.acceleration.z;
    }
    

After tapping the Go! button, the user is told to jump. During that time, the page stores the maximum (and minimum) acceleration values, and after the jump, tells the user their maximum acceleration.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}