project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Прорисовка – это процесс заполнения пикселей, которые затем будут скомпонованы и выведены на экраны пользователей. Зачастую этот процесс является задачей, которая выполняется в конвейере дольше всего и выполнения которой следует избегать при любой возможности

{# wf_updated_on: 2015-03-19 #} {# wf_published_on: 2000-01-01 #}

# Упрощение и сокращение областей прорисовки {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}

Прорисовка – это процесс заполнения пикселей, которые затем будут скомпонованы и выведены на экраны пользователей. Зачастую этот процесс является задачей, которая выполняется в конвейере дольше всего и выполнения которой следует избегать при любой возможности

### TL;DR {: .hide-from-toc }

* Изменение любого свойства, кроме transform и opacity, вызывает прорисовку.
* Прорисовка зачастую является самой затратной частью конвейера. Избегайте ее при любой возможности.
* Сокращайте области прорисовки путем размещения элементов на отдельных слоях и оптимизации анимации.
* Оценивайте сложность прорисовки и затраты на нее с помощью средства профилирования Chrome DevTools. Снижайте сложность при любой возможности.

## Для быстрого определения проблем с прорисовкой используйте Chrome DevTools

Если запускается перерасчет макета, *всегда запускается и прорисовка*, поскольку изменение геометрии элемента означает, что его пиксели нужно привести в порядок!

<img src="images/simplify-paint-complexity-and-reduce-paint-areas/frame.jpg"  alt="Полный конвейер пикселей" />

Прорисовка также запускается при изменении свойств, не связанных с геометрией, таких как фон, цвет текста или тени. В этих случаях перерасчитывать макет не придется, а конвейер будет иметь следующий вид:

<img src="images/simplify-paint-complexity-and-reduce-paint-areas/frame-no-layout.jpg"  alt="Конвейер пикселей без перерасчета макета." />

## Перемещайте на отдельные слои элементы, которые двигаются или исчезают с экрана

<div class="attempt-right">
  <figure>
    <img src="images/simplify-paint-complexity-and-reduce-paint-areas/show-paint-rectangles.jpg" alt="The show paint rectangles option in DevTools.">
  </figure>
</div>

С помощью Chrome DevTools можно быстро определить области, на которые распространяется прорисовка. Перейдите в DevTools и нажмите на клавиатуре клавишу Escape. На открывшейся панели перейдите на вкладку прорисовки и установите флажок "Show paint rectangles" (Показать прямоугольники прорисовки) :

<div style="clear:both;"></div>

With this option switched on Chrome will flash the screen green whenever painting happens. If you’re seeing the whole screen flash green, or areas of the screen that you didn’t think should be painted, then you should dig in a little further.

<img src="images/simplify-paint-complexity-and-reduce-paint-areas/show-paint-rectangles-green.jpg"  alt="The page flashing green whenever painting occurs." />

<div class="attempt-right">
  <figure>
    <img src="images/simplify-paint-complexity-and-reduce-paint-areas/paint-profiler-toggle.jpg" alt="The toggle to enable paint profiling in Chrome DevTools.">
  </figure>
</div>

There’s an option in the Chrome DevTools timeline which will give you more information: a paint profiler. To enable it, go to the Timeline and check the “Paint” box at the top. It’s important to *only have this switched on when trying to profile paint issues*, as it carries an overhead and will skew your performance profiling. It’s best used when you want more insight into what exactly is being painted.

<div style="clear:both;"></div>

<div class="attempt-right">
  <figure>
    <img src="images/simplify-paint-complexity-and-reduce-paint-areas/paint-profiler-button.jpg" alt="The button to bring up the paint profiler." class="screenshot">
  </figure>
</div>

На шкале времени Chrome DevTools имеется средство, которое позволяет получить более подробные сведения, – профилировщик прорисовки. Чтобы включить его, перейдите на шкалу времени и установите флажок "Paint" вверху окна. Важно, чтобы *этот флажок находился в установленном состоянии только при профилировании проблем с прорисовкой*, поскольку, когда он установлен, потребляются дополнительные ресурсы и профилирование производительности даст искаженные результаты. Лучше всего использовать это средство, когда вам требуются сведения о том, какие объекты на странице были прорисованы.

<div style="clear:both;"></div>

Clicking on the paint profiler brings up a view where you can see what got painted, how long it took, and the individual paint calls that were required:

<img src="images/simplify-paint-complexity-and-reduce-paint-areas/paint-profiler.jpg"  alt="Chrome DevTools Paint Profiler." />

This profiler lets you know both the area and the complexity (which is really the time it takes to paint), and both of these are areas you can look to fix if avoiding paint is not an option.

## Сокращайте области прорисовки

При щелчке профилировщика прорисовки открывается представление, в котором указано, какие объекты подверглись прорисовке, сколько это заняло времени, а также отдельные вызовы прорисовки, которые потребовались:

<img src="images/simplify-paint-complexity-and-reduce-paint-areas/layers.jpg"  alt="Профилировщик прорисовки Chrome DevTools." />

Этот профилировщик позволяет определить и область, и сложность прорисовки (фактически это время, которое занимает прорисовка). А это именно те аспекты, которые необходимо исправлять, если избежать прорисовки совершенно невозможно.

Прорисовка не всегда формирует одно изображение в памяти. На самом деле, при необходимости браузер может сформировать несколько изображений или слоев для компоновки.

    .moving-element {
      will-change: transform;
    }
    

For browsers that don’t support `will-change`, but benefit from layer creation, such as Safari and Mobile Safari, you need to (mis)use a 3D transform to force a new layer:

    .moving-element {
      transform: translateZ(0);
    }
    

Преимущество этого подхода состоит в том, что элементы, которые регулярно заново прорисовываются или двигаются на экране с помощью свойств transform, можно обрабатывать, не затрагивая другие элементы. Это тоже самое, что и работа с графикой в Sketch, GIMP или Photoshop, где отдельные слои можно обрабатывать и располагать друг на друге для создания итогового изображения.

Лучший способ создания новыхе слоев – это использование свойства CSS `will-change`. Такой способ работает в Chrome, Opera и Firefox. Со значением `transform` это свойство создает новый слой:

## Снижайте сложность прорисовки

Для браузеров, которые не поддерживают свойство `will-change`, но будут работать быстрее, если создавать слои, таких как Safari и Mobile Safari, необходимо воспользоваться (немного неправильно) трехмерным преобразованием для принудительного формирования нового слоя:

Однако не следует создавать слишком много слоев, поскольку каждый из них занимает место в памяти и требует управления. Подробные сведения об этом приведены в разделе \[Используйте свойства, вызывающие только компоновку, и контролируйте количество слоев\] (stick-to-compositor-only-properties-and-manage-layer-count).

Если вы переместили элемент на новый слой, воспользуйтесь DevTools, чтобы убедиться в том, что это дало выигрыш по производительности. **Не перемещайте элементы на отдельные слои без профилирования.**

## Simplify paint complexity

<div class="attempt-right">
  <figure>
    <img src="images/simplify-paint-complexity-and-reduce-paint-areas/profiler-chart.jpg" alt="The time taken to paint part of the screen.">
  </figure>
</div>

Несмотря на возможность размещения элементов на отдельных слоях иногда все же необходимо выполнять прорисовку. Большая проблема с прорисовкой заключается в том, что браузеры объединяют вместе две области, которые требуется прорисовать, из-за чего заново прорисованным может быть весь экран. Так, например, если у вас есть фиксированный заголовок вверху страницы и область прорисовки в низу экрана, заново прорисованным может быть весь экран.

Note: На экранах с высоким разрешением элементы с фиксированным положением автоматически переносятся на собственный слой. Этого не происходит на экранах с низким разрешением, поскольку при переносе визуализация текста меняется с субпиксельной на монохромную, поэтому перемещение на слой должно выполняться вручную

Сокращение областей прорисовки зачастую заключается в том, чтобы анимация и переходы не слишком перекрывались, или в том, чтобы найти возможность избежать анимации определенных частей страницы.

## Feedback {: #feedback }

При выполнении прорисовки одни операции требуют больше ресурсов, чем другие. Например, все, что связано с событием blur (тень и т. п.), будет прорисовываться дольше чем, скажем, красный квадрат. Однако для CSS это не всегда очевидно: строки `background: red;` и `box-shadow: 0, 4px, 4px, rgba(0,0,0,0.5);` не выглядят так, что по производительности они значительно отличаются, но на самом деле это так и есть.