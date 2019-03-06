project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Большинство интернет-ресурсов не оптимизировано для просмотра на разных типах устройств. Изучив основы, вы узнаете, как обеспечить одинаково хорошую работу сайта на телефонах, планшетах, домашних компьютерах... в общем, на любых устройствах, у которых есть экран.

{# wf_updated_on: 2014-04-29 #} {# wf_published_on: 2000-01-01 #}

# Основы отзывчивого веб-дизайна {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

Число пользователей, выходящих в Интернет с мобильных устройств, стремительно растет, но, к сожалению, большинство сетевых ресурсов не приспособлено для этого. Для каждого мобильного устройства необходим особый подход к расположению контента на экране из-за ограниченного размера дисплея.

{% include "web/_shared/udacity/ud893.html" %}

<video autoplay muted loop controls>
  <source src="videos/resize.webm" type="video/webm">
  <source src="videos/resize.mp4" type="video/mp4">
</video>

Существует множество различных устройств с экранами всевозможных размеров: телефоны, `фаблеты`, планшетные и домашние ПК, игровые консоли, телевизоры и даже электронные аксессуары, которые можно носить прямо на себе. Размеры экранов постоянно меняются, поэтому важно, чтобы сайт мог адаптироваться к любому из них - не только сейчас, но и в будущем.

Об отзывчивом веб-дизайне впервые написал [Итан Маркотт в статье журнала A List Apart](http://alistapart.com/article/responsive-web-design/). Это именно то, чего так не хватало устройствам и их пользователям. Макет подстраивается под устройство, исходя из его возможностей и размера. К примеру, определенный контент на телефоне располагается в одной колонке, а на планшете - в двух.

## Настройка области просмотра

Страницы, адаптированные для просмотра на разных устройствах, должны содержать в контейнере head метатег viewport. Он сообщает браузеру, каким образом нужно контролировать размеры и масштаб страницы.

### TL;DR {: .hide-from-toc }

- Чтобы контролировать масштабирование окна просмотра в браузере, используйте метатег viewport.
- Добавьте `width=device-width`, чтобы адаптировать ширину окна просмотра к экрану устройства.
- Вставьте `initial-scale=1`, чтобы обеспечить соотношение 1:1 между пикселями CSS и независимыми пикселями устройства.
- Чтобы страница была доступна, проверьте, не отключено ли пользовательское масштабирование.

Пытаясь улучшить работу с сайтами, мобильные браузеры отображают страницу·той же ширины, что и на экране ПК (обычно она составляет около 980 пикселей, но на разных устройствах может отличаться). После этого браузер увеличивает размер шрифтов и изменяет размер контента, чтобы уместить страницу на экране. В результате пользователи видят разные шрифты, и для работы с сайтом им приходится увеличивать его масштаб.

    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    

Чтобы страница подстроилась под ширину нужного экрана (в независимых пикселях), используйте метатег viewport `width=device-width`. С его помощью размер и положение контента изменятся, и сайт будет хорошо смотреться на любом устройстве.

<div class="attempt-left">
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/vp-no.html">
  <figure>
    <img src="imgs/no-vp.png" srcset="imgs/no-vp.png 1x, imgs/no-vp-2x.png 2x" alt="Page without a viewport set">
    <figcaption>
      Page without a viewport set
     </figcaption>
  </figure>
  </a>
</div>

<div class="attempt-right">
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/vp.html">
  <figure>
    <img src="imgs/vp.png" srcset="imgs/vp.png 1x, imgs/vp-2x.png 2x" alt="Page with a viewport set">
    <figcaption>
      Page with a viewport set
     </figcaption>
  </figure>
  </a>
</div>

Some browsers keep the page's width constant when rotating to landscape mode, and zoom rather than reflow to fill the screen. Adding the attribute `initial-scale=1` instructs browsers to establish a 1:1 relationship between CSS pixels and device-independent pixels regardless of device orientation, and allows the page to take advantage of the full landscape width.

Некоторые браузеры не редактируют ширину страницы и размер контента, а изменяют ориентацию сайта на альбомную или увеличивают масштаб. С помощью атрибута initial-scale=1 можно указать браузеру соотношение пикселей CSS и устройства, равное 1:1 независимо от ориентации дисплея. Благодаря этому страница будет лучше выглядеть в альбомной ориентации.

### Специальные возможности области просмотра

Note: Разделяйте атрибуты запятыми, чтобы устаревшие версии браузеров могли их правильно интерпретировать.

- `minimum-scale`
- `maximum-scale`
- `user-scalable`

Настроив initial-scale, вы также можете установить следующие атрибуты для области просмотра:

## Масштабирование контента для области просмотра

После их настройки устанавливается ограничение на изменение масштаба области просмотра, если из-за этого могут стать недоступны специальные возможности.

### TL;DR {: .hide-from-toc }

- Не используйте крупные элементы с фиксированной шириной.
- Для корректного отображения контента не ограничивайте его определенной шириной области просмотра.
- Используйте медиазапросы CSS, чтобы указать разные стили для больших и маленьких экранов.

Для просмотра сайтов на обычных компьютерах и мобильных устройствах используется вертикальная прокрутка. Если пользователю приходится прокручивать сайт по горизонтали или уменьшать масштаб, чтобы увидеть страницу полностью, он испытывает большие неудобства.

Используя метатег viewport при разработке сайта для мобильных устройств, можно по ошибке создать контент страницы, которые не полностью соответствует определенной области просмотра. Например, если ширина показываемого изображения больше области просмотра, придется использовать горизонтальную прокрутку. Чтобы избавить пользователей от этой необходимости, потребуется подогнать контент под шир ну области просмотра.

Диапазон размеров экрана и ширины в пикселях CSS очень велик (например, телефоны могут значительно отличаться от планшетов или даже от других телефонов), поэтому отображение контента не должно зависеть от конкретной ширины области просмотра.

<div class="attempt-left">
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/vp-fixed.html">
  <figure>
    <img src="imgs/vp-fixed-iph.png" srcset="imgs/vp-fixed-iph.png 1x, imgs/vp-fixed-iph-2x.png 2x" alt="Page with a 344px fixed width element on an iPhone.">
    <figcaption>
      Page with a 344px fixed width element on an iPhone
    </figcaption>
  </figure>
  </a>
</div>

<div class="attempt-right">
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/vp-fixed.html">
  <figure>
    <img src="imgs/vp-fixed-n5.png" srcset="imgs/vp-fixed-n5.png 1x, imgs/vp-fixed-n5-2x.png 2x" alt="Page with a 344px fixed width element on a Nexus 5.">
    <figcaption>
      Page with a 344px fixed width element on a Nexus 5
    </figcaption>
  </figure>
  </a>
</div>

<div class="clearfix"></div>

## Улучшение отзывчивости с помощью медиазапросов CSS

Установка больших абсолютных значений ширины CSS для компонентов страницы сделает div слишком большим для более узкой области просмотра (например, на устройствах шириной 320 пикселей CSS, таких как iPhone). Вместо этого можно использовать значения в относительных единицах, например width: 100%. Также старайтесь избегать больших абсолютных значений позиционирования. Из-за них на маленьком экране элемент может оказаться эа пределами области просмотра.

### TL;DR {: .hide-from-toc }

- Медиазапросы также позволяют выбрать стиль на основе характеристик устройства.
- Добавьте `min-width` вместо `min-device-width` для корректного отображения сайта на большинстве устройств.
- Чтобы не нарушать структуру макета, используйте элементы относительных размеров.

For example, you could place all styles necessary for printing inside a print media query:

    <link rel="stylesheet" href="print.css" media="print">
    

Медиазапросы - это простые фильтры, которые можно применять к стилям CSS. Они позволяют изменять стили на основании характеристик устройства, связанных с отображением контента, включая тип, ширину, высоту, ориентацию и даже разрешение экрана.

    @media print {
      /* print style sheets go here */
    }
    
    @import url(print.css) print;
    

Например, вы можете поместить в медиазапрос print все стили, необходимые для печати:

### Применение медиазапросов на основе размера области просмотра

Кроме использования атрибут media в ссылке на таблицу стилей существует ещё два способа применить медиазапросы @media и @import, которые можно встроить в файл CSS: Приоритет отдается первым двум методам, более эффективным, чем синтаксис @import (см. [Избегайте правила @import](/web/fundamentals/performance/critical-rendering-path/page-speed-rules-and-recommendations)).

    @media (query) {
      /* CSS Rules used when query matches */
    }
    

Логика медиазапросов не является взаимоисключающей, поэтому к блоку CSS применяются все фильтры, отвечающие его критериям. Порядок применения фильтров обусловлен стандартными правилами CSS.

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Атрибут</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="attribute"><code>min-width</code></td>
      <td data-th="Result">Правило применяется к браузеру, ширина которого больше значения, указанного в запросе.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>max-width</code></td>
      <td data-th="Result">Правило применяется к браузеру, ширина которого меньше значения, указанного в запросе.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>min-height</code></td>
      <td data-th="Result">Правило применяется к браузеру, высота которого больше значения, указанного в запросе.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>max-height</code></td>
      <td data-th="Result">Правило применяется к браузеру, высота которого меньше значения, указанного в запросе.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>orientation=portrait</code></td>
      <td data-th="Result">Правило применяется к браузеру, высота которого не меньше его ширины.</td>
    </tr>
    <tr>
      <td data-th="attribute"><code>orientation=landscape</code></td>
      <td data-th="Result">Правило применяется к браузеру, ширина которого больше высоты.</td>
    </tr>
  </tbody>
</table>

С помощью медиазапросов можно создать отзывчивую среду, в которой к каждому размеру экрана будут применяться подходящие стили. Синтаксис медиазапросов допускает создание правил, которые применяются на основании характеристик устройства.

<figure>
  <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/media-queries.html">
    <img src="imgs/mq.png" srcset="imgs/mq.png 1x, imgs/mq-2x.png 2x" alt="Preview of a page using media queries to change properties as it is resized.">
    <figcaption>
      Preview of a page using media queries to change properties as it is resized.
    </figcaption>
  </a>
</figure>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-queries.html" region_tag="mqueries" adjust_indentation="auto" %}
</pre>

В отзывчивом веб-дизайне наиболее часто используются функции min-width, max-width, min-height и max-height (хотя возможны и другие запросы).

- При ширине браузера от **0 пикс.** до **640 пикс.** применяется max-640px.css.
- При ширине браузера от **500 пикс.** до **600 пикс.** применяются стили из @media.
- При ширине браузера **от 640 пикс. и выше** применяется min-640px.css.
- Если в браузере **ширина больше высоты**, применяется landscape.css.
- Если в браузере **высота больше ширины**, применяется portrait.css.

### Примечание к min-device-width

Рассмотрим пример:

Также возможно создание запросов на основании *-device-width, хотя делать это **настоятельно не рекомендуется**.

Разница небольшая, но очень важная: min-width исходит из размера окна браузера, а min-device-width - из размера экрана устройства. К сожалению, некоторые браузеры (включая устаревшую версию браузера для Android) не всегда правильно определяют ширину области просмотра и вместо нее могут сообщить размер экрана в пикселях устройства.

### Использование относительных единиц

К тому же, использование *-device-width может помешать контенту подстроиться под экран обычного компьютера или другого устройства, на котором можно изменить размер окна. Причина в том, что этот запрос основан на размере конкретного устройства, а не окна браузера.

### TL;DR {: .hide-from-toc }

Основной принцип отзывчивого веб-дизайна (и главное отличие от макетов с фиксированной шириной) - подвижность и пропорциональность. Использование относительных размеров помогает упростить макет и предотвратить случайное создание компонентов, не вмещающихся в область просмотра.

Например, установка параметра width равным 100% для блока div верхнего уровня приведет к тому, что он будет заполнять всю ширину области просмотра и никогда не будет слишком мал или велик для нее. Блок div в любом случае будет соответствовать экрану, будь то iPhone (320 пикс.), Blackberry Z10 (342 пикс.) или Nexus 5 (360 пикс.).

Кроме того, использование относительных единиц позволяет браузерам отображать контент, исходя из пользовательского масштаба. Это значит, что горизонтальная панель прокрутки на странице не понадобится.

<span class="compare-worse">Not recommended</span> — fixed width

    div.fullWidth {
      width: 320px;
      margin-left: auto;
      margin-right: auto;
    }
    

<span class="compare-better">Recommended</span> — responsive width

    div.fullWidth {
      width: 100%;
    }
    

## Выбор контрольных точек

Будьте осторожны при определении контрольных точек на основе классов устройств. Если при установке этих точек вы будете ориентироваться на все доступные устройства, бренды или операционные системы, техническое обслуживание сайта станет практически невозможным. Лучше, если контент будет сам определять, как макет должен подстраиваться под контейнер.

### Выбор главных контрольных точек: от меньшего к большему

- Создавайте контрольные точки на основе контента, а не для отдельных устройств, продуктов или брендов.
- Сначала разработайте дизайн для самого маленького мобильного устройства, а затем переходите к версиям для больших экранов.
- Ограничьте длину строк 70-80 символами.

### Выбор второстепенных контрольных точек (если необходимо)

<figure class="attempt-right">
  <img src="imgs/weather-1.png" srcset="imgs/weather-1.png 1x, imgs/weather-1-2x.png 2x" alt="">
  <figcaption>
    <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/weather-1.html">
      Preview of the weather forecast displayed on a small screen.
    </a>
  </figcaption>
</figure>

Сначала разработайте дизайн контента для маленьких экранов, а затем увеличивайте его, пока не возникнет необходимость в контрольной точке. Так вы оптимизируете контрольные точки на основе контента и используете их минимальное количество.

В качестве примера рассмотрим уже знакомый вам [прогноз погоды](/web/fundamentals/design-and-ux/responsive/). Сначала сделаем так, чтобы прогноз хорошо смотрелся на небольшом экране.

<div style="clear:both;"></div>

<figure class="attempt-right">
  <img src="imgs/weather-2.png" class="center" srcset="imgs/weather-2.png 1x, imgs/weather-2-2x.png 2x" alt="Preview of the weather forecast as the page gets wider.">
  <figcaption>
    <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/weather-1.html">
      Preview of the weather forecast as the page gets wider.
    </a>
  </figcaption>
</figure>

Затем увеличим размер окна браузера, пока между элементами не появится слишком много пустого пространства. Теперь прогноз погоды выглядит не так красиво. Вы можете выбрать любое значение, но 600 пикселей - это слишком много.

<div style="clear:both;"></div>

Чтобы сделать 600 пикселей контрольной точкой, нужно создать две таблицы стилей: одну для меньших значений, другую для больших.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-2.html" region_tag="mqweather2" adjust_indentation="auto" %}
</pre>

В конце проведите рефакторинг CSS. В этом примере мы поместили общие стили (шрифты, значки, цвета и базовое позиционирование) в weather.css. Затем мы включили макет для маленьких экранов в weather-small.css, а для больших - в weather-large.css.

<figure class="attempt-right">
  <img src="imgs/weather-3.png"  srcset="imgs/weather-3.png 1x, imgs/weather-3-2x.png 2x" alt="Preview of the weather forecast designed for a wider screen.">
  <figcaption>
    <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/weather-2.html">
      Preview of the weather forecast designed for a wider screen.
    </a>
  </figcaption>
</figure>

Отметив главными контрольными точками крупные изменения макета, внесите второстепенные корректировки. Например, между главными контрольными точками можно установить отступы для элемента или увеличить размер шрифта, чтобы он смотрелся в макете более естественно.

<div style="clear:both"></div>

### Оптимизация текста для чтения

Начнем с оптимизации макета для небольших экранов. В данном случае мы увеличим шрифт для ширины области просмотра, превышающей 360 пикселей. Теперь, когда места достаточно, мы можем расположить минимальную температуру на одной линии с максимальной, а не под ней. Также немного увеличим значки погодных явлений.

Let's start by optimizing the small screen layout. In this case, let's boost the font when the viewport width is greater than 360px. Second, when there is enough space, we can separate the high and low temperatures so that they're on the same line instead of on top of each other. And let's also make the weather icons a bit larger.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-small.css" region_tag="mqsmallbpsm"   adjust_indentation="auto" %}
</pre>

<div class="attempt-left">
  <figure>
    <img src="imgs/weather-4-l.png" srcset="imgs/weather-4-l.png 1x, imgs/weather-4-l-2x.png 2x" alt="Before adding minor breakpoints.">
    <figcaption>
      Before adding minor breakpoints.
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img src="imgs/weather-4-r.png" srcset="imgs/weather-4-r.png 1x, imgs/weather-4-r-2x.png 2x" alt="After adding minor breakpoints.">
    <figcaption>
      After adding minor breakpoints.
     </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Этим же способом можно ограничить размер панели прогноза погоды для больших экранов, чтобы она не занимала всю доступную ширину.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/weather-large.css" region_tag="mqsmallbplg"   adjust_indentation="auto" %}
</pre>

### Никогда не скрывайте контент полностью

Согласно распространенной теории, самой удобной для чтения является колонка с длиной строки около 70-80 знаков (8-10 слов). Поэтому для всех текстовых блоков, ширина которых превышает 10 слов, следует установить контрольную точку.

<div class="attempt-left">
  <figure>
    <img src="imgs/reading-ph.png" srcset="imgs/reading-ph.png 1x, imgs/reading-ph-2x.png 2x" alt="Before adding minor breakpoints.">
    <figcaption>Before adding minor breakpoints.</figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img src="imgs/reading-de.png" srcset="imgs/reading-de.png 1x, imgs/reading-de-2x.png 2x" alt="After adding minor breakpoints.">
    <figcaption>After adding minor breakpoints.</figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Let's take a deeper look at the above blog post example. On smaller screens, the Roboto font at 1em works perfectly giving 10 words per line, but larger screens require a breakpoint. In this case, if the browser width is greater than 575px, the ideal content width is 550px.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/reading.html" region_tag="mqreading"   adjust_indentation="auto" %}
</pre>

Рассмотрим подробнее приведенный выше пример записи в блоге. На маленьком экране отлично помещаются строки длиной в 10 слов шрифтом Roboto с размером 1em, но для экранов большего размера необходимо установить контрольную точку. В этом случае для окна просмотра браузера размером больше 575 пикселей оптимальная ширина контента равна 550 пикселям.

### Never completely hide content

Внимательно выбирайте информацию, которая будет показана или скрыта в зависимости от размера экрана. Не убирайте контент только потому, что не можете разместить его на экране. Потребности пользователей не зависят от его размера. Например, если вы уберете из прогноза погоды информацию о содержании пыльцы в воздухе, это может стать серьезной проблемой для аллергиков, решающих, стоит ли сегодня выходить из дома.

## View media query breakpoints in Chrome DevTools {: #devtools }

Once you've got your media query breakpoints set up, you'll want to see how your site looks with them. You *could* resize your browser window to trigger the breakpoints, but there's a better way: Chrome DevTools. The two screenshots below demonstrate using DevTools to view how a page looks under different breakpoints.

![Example of DevTools' media queries feature](imgs/devtools-media-queries-example.png)

To view your page under different breakpoints:

[Open DevTools](/web/tools/chrome-devtools/#open) and then turn on [Device Mode](/web/tools/chrome-devtools/device-mode/#toggle).

Use the [viewport controls](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#viewport-controls) to select **Responsive**, which puts DevTools into responsive mode.

Last, open the Device Mode menu and select [**Show media queries**](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#media-queries) to display your breakpoints as colored bars above your page.

Click on one of the bars to view your page while that media query is active. Right-click on a bar to jump to the media query's definition. See [Media queries](/web/tools/chrome-devtools/device-mode/emulate-mobile-viewports#media-queries) for more help.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}