project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: События движения и изменения ориентации мобильного устройства открывают доступ ко встроенному акселерометру, гироскопу и компасу

{# wf_updated_on: 2014-10-20 #} {# wf_published_on: 2000-01-01 #}

# Ориентация устройства {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

События движения и изменения ориентации мобильного устройства открывают доступ ко встроенному акселерометру, гироскопу и компасу

Эти события могут использоваться во многих целях; например, в играх для контроля за передвижениями персонажа либо для определения высоты прыжка героя. Если использовать эти события вместе с функцией геолокации, можно создать более точную пошаговую систему навигации или предоставить информацию о местонахождении магазина.

Note: **Особенно** тщательно следует принимать решения об использовании событий, связанных с движением или изменением ориентации мобильных устройств. К сожалению, не все браузеры используют одну и ту же систему координат, и поэтому при одинаковых условиях они могут возвращать различные значения.

## В каком положении находится устройство?

* **X:** расположена в направлении запад-восток (направлена на восток).
* **Y:** расположена в направлении юг-север (направлена на север).

## Ориентация устройства

Чтобы использовать данные, получаемые от событий изменения ориентации и движения устройства, важно понимать суть предоставляемых значений.

### Система земных координат

Система земных координат описывается значениями осей `X`, `Y` и `Z`, ориентация которых определяется направлением магнитного меридиана и силы тяжести.

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

### Система координат устройства

<div class="attempt-right">
  <figure id="fig1">
    <img src="images/axes.png" alt="illustration of device coordinate frame">
    <b>альфа:</b> Угол поворота вокруг оси z = 0&deg;, когда верхняя часть
  устройства направлена точно на север.  При повороте устройства против часовой стрелки
    значение `alpha` увеличивается.
  </figure>
</div>

<!-- Special thanks to Sheppy (https://developer.mozilla.org/en-US/profiles/Sheppy)
  for his images which are in the public domain. -->

Система координат устройства описывается значениями по осям `x`, `y` и `z`, которые исходят из центра устройства.

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

### Данные о повороте

На телефоне или планшете ориентация устройства определяется по типичной ориентации экрана. На телефоне или планшете это положение в портретной ориентации. На персональных компьютерах или ноутбуках ориентация определяется по отношению к клавиатуре.

#### TL;DR {: .hide-from-toc }

<div class="attempt-right">
  <figure id="fig1">
    <img src="images/alpha.png" alt="illustration of device coordinate frame">
    <b>бета:</b> Угол поворота вокруг оси x = 0&deg;, когда верхняя и 
нижняя части устройства находятся на одинаковом расстоянии от земной поверхности. Значение
    увеличивается, когда верхнюю часть устройства наклоняют в направлении земной поверхности.
  </figure>
</div>

Данные о повороте поступают как величина [угла Эйлера](http://en.wikipedia.org/wiki/Euler_angles), представляющего собой разницу в градусах между координатами устройства и земными координатами.

<div style="clear:both;"></div>

#### TL;DR {: .hide-from-toc }

<div class="attempt-right">
  <figure id="fig1">
    <img src="images/beta.png" alt="illustration of device coordinate frame">
    <figcaption>
      Illustration of beta in the device coordinate frame
    </figcaption>
  </figure>
</div>

Событие 'изменение ориентации устройства' возвращает данные о повороте, такие как угол наклона устройства вперед-назад, из стороны в сторону и, если в телефоне или ноутбуке имеется компас, сведения о том, куда направлена лицевая сторона устройства.

<div style="clear:both;"></div>

#### Пример. Расчет максимального ускорения объекта

<div class="attempt-right">
  <figure id="fig1">
    <img src="images/gamma.png" alt="illustration of device coordinate frame">
    <figcaption>
      Illustration of gamma in the device coordinate frame
    </figcaption>
  </figure>
</div>

Существует несколько ситуаций, в которых можно использовать события "изменение ориентации устройства". Например:

<div style="clear:both;"></div>

## Движение устройства

Чтобы принимать события `DeviceOrientationEvent`, сначала проверьте, поддерживаются ли эти события в браузере. Затем подключите приемник событий к объекту `window`, принимающему события `deviceorientation`.

Событие "изменение ориентации устройства" запускается при перемещении или изменении ориентации устройства. Событие возвращает данные об изменении текущего положения устройства по отношению к [ системе земных координат](index.html#earth-coordinate-frame).

### Ситуации, в которых следует использовать события "изменение ориентации устройства"

Обычно событие возвращает три свойства:
<a href="index.html#rotation-data"><code>alpha</code></a>, 
<a href="index.html#rotation-data"><code>beta</code></a> и
<a href="index.html#rotation-data"><code>gamma</code></a>. В браузере для мобильных устройств Mobile Safari возвращается дополнительный параметр [`webkitCompassHeading`](https://developer.apple.com/library/safari/documentation/SafariDOMAdditions/Reference/DeviceOrientationEventClassRef/DeviceOrientationEvent/DeviceOrientationEvent.html) с заголовком "компас".

* **x:** эта ось расположена в плоскости экрана и направлена вправо.
* **y:** эта ось расположена в плоскости экрана и направлена к его верхней части.
* **z:** эта ось расположена перпендикулярно экрану или клавиатуре и направлена наружу.

### Проверка наличия поддержки и прием событий

Движение устройства дает информацию о силе ускорения, приложенной к устройству в данный момент, и о скорости вращения

    if (window.DeviceOrientationEvent) {
      window.addEventListener('deviceorientation', deviceOrientationHandler, false);
      document.getElementById("doeSupported").innerText = "Supported!";
    }
    

### Обработка событий "изменение ориентации устройства"

Существует несколько ситуаций, в которых можно использовать события "движение устройства". Например:

Чтобы принимать события `DeviceMotionEvent`, сначала проверьте, поддерживаются ли эти события в браузере. Затем подключите приемник событий к объекту `window`, принимающему события `devicemotion`.

## Device motion

Событие "движение устройства" запускается через регулярные интервалы и возвращает данные о вращении (в &deg; в секунду) и ускорении (в м в секунду<sup>2</sup>) , которые характеризуют перемещение устройства в данный момент времени. В некоторых устройствах нет компонентов, которые позволяют не учитывать воздействие силы тяжести.

Событие возвращает четыре свойства, 
<a href="index.html#device-frame-coordinate"><code>accelerationIncludingGravity</code></a>, 
<a href="index.html#device-frame-coordinate"><code>acceleration</code></a>, в котором не учитывается воздействие силы тяжести, 
<a href="index.html#rotation-data"><code>rotationRate</code></a> и `interval`.

### Ситуации, в которых следует использовать события "движение устройства"

Например, рассмотрим телефон, лежащий на горизонтальном столе экраном вверх.

* Не используйте это событие слишком часто.
* Проверьте наличие поддержки.
* Не обновляйте интерфейс при каждом событии. Вместо этого выполняйте синхронизацию с `requestAnimationFrame`.

### Проверка поддержки и приема событий

В отличие от этого, если экран телефона расположен перпендикулярно к земле и виден наблюдателю:

    if (window.DeviceMotionEvent) {
      window.addEventListener('devicemotion', deviceMotionHandler);
      setTimeout(stopJump, 3*1000);
    }
    

### Обработка событий "движение устройства"

Один из способов использовать события "движение устройства" — вычисление максимального ускорения объекта. Например, можно рассчитать максимальное ускорение человека в прыжке.

После касания кнопки Go! пользователю предлагается подпрыгнуть! В течение этого времени на странице сохраняются значения максимального (и минимального) ускорения, а после прыжка сообщается максимальное ускорение пользователя.

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

### Sample: Calculating the maximum acceleration of an object

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