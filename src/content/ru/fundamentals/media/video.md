project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Узнайте, как легко и просто добавить видео на свой сайт, чтобы пользователи на любом устройстве получили удовольствие от его просмотра.

{# wf_updated_on: 2014-04-28 #} {# wf_published_on: 2000-01-01 #}

# Видео {: .page-title }

{% include "web/_shared/contributors/samdutton.html" %}

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="j5fYOYrsocs"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Пользователи любят видео, ведь они могут быть забавными и полезными. На мобильных устройствах видео может оказаться простейшим способом получить нужную информацию. Однако видеофайлы снижают пропускную способность. К тому же они по-разному работают на разных платформах. Пользователи не любят ждать, когда видео загрузится. Они также не любят, когда нажимают кнопку `Воспроизвести` - и ничего не происходит. Здесь вы найдете информацию о том, как легко и просто добавить видео на свой сайт, чтобы пользователи на любом устройстве получили удовольствие от его просмотра.

## Как добавить видео

### TL;DR {: .hide-from-toc }

* Используйте элемент video для загрузки, декодирования и воспроизведения видео на своем сайте.
* Запишите видео в нескольких форматах, адаптированных под различные мобильные платформы.
* Установите корректный размер видеофайлов; он не должен превышать максимальный размер контейнеров.
* Контент должен быть доступен пользователям с ограниченными возможностями. Добавьте track как дочерний элемент video.

### Добавьте элемент video

В этой статье вы найдете информацию о том, как легко и просто добавить видео на свой сайт, чтобы пользователи на любом устройстве получили удовольствие от его просмотра.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4" type="video/mp4">
  <p>Этот браузер не поддерживает элемент video.</p>
</video>

    <video src="chrome.webm" type="video/webm">
        <p>Ваш браузер не поддерживает элемент video.</p>
    </video>
    

### Укажите несколько форматов видеофайла

Добавьте элемент video для загрузки, декодирования и воспроизведения видео на вашем сайте:

For example:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/video-main.html" region_tag="sourcetypes" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/video-main.html){: target="_blank" .external }

Подобный подход имеет целый ряд преимуществ по сравнению с использованием скриптов HTML или исполнением скриптов на сервере, особенно в среде мобильных устройств:

Все эти факторы особенно важны в среде мобильных устройств, где пропускная способность и время реакции буквально на вес золота, а терпение пользователя не бесконечно. Отсутствие атрибута type повлияет на производительность в случае, если вы используете несколько источников неподдерживаемых типов.

С помощью инструментов разработчика вашего мобильного браузера сравните операции сетевого трафика [с использованием атрибута type](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html) и [без него](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/notype.html). Также проверьте заголовки ответов в инструментах разработчика вашего браузера, чтобы удостовериться, что [ваш сервер сообщает правильный тип MIME](//developer.mozilla.org/en/docs/Properly_Configuring_Server_MIME_Types). В ином случае проверка типа источника видео не сработает.

* Разработчики могут перечислить форматы в порядке их предпочтительности.
* Снижение времени реакции за счет переключения на стороне клиента. Для получения контента делается только один запрос.
* Проще, быстрее и надежнее дать самому браузеру выбрать формат, чем использовать поддерживаемую сервером базу данных с распознаванием на стороне пользовательского агента.
* Указание типа источника каждого файла повышает производительность сети. Браузер может выбрать источник видео, не прибегая к загрузке части файла для опознания формата.

Чтобы сохранить пропускную способность и сделать сайт более быстрым, используйте Media Fragments API для определения времени начала и окончания элемента video.

Для добавления медиафрагмента просто укажите `#t=[start_time][,end_time]` в URL медиафайла. Например, чтобы воспроизвести фрагмент видео с 5 по 10 секунду, укажите:

Вы также можете использовать Media Fragments API для просмотра различных фрагментов одного и того же видео (например, как на DVD) без кодирования и загрузки нескольких файлов.

### Укажите время начала и окончания

Note: - Media Fragments API поддерживается большинством платформ за исключением iOS.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm#t=5,10" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4#t=5,10" type="video/mp4">
  <p>Этот браузер не поддерживает элемент video.</p>
</video>

В заголовках ответов должно стоять "Accept Ranges: bytes" (вы можете проверить это с помощью инструментов разработчика вашего браузера):

    <source src="video/chrome.webm#t=5,10" type="video/webm">
    

You can also use the Media Fragments API to deliver multiple views on the same video&ndash;like cue points in a DVD&ndash;without having to encode and serve multiple files.

Добавьте в элемент video атрибут poster. Благодаря этому пользователи смогут сразу же получить общее представление о контенте. Им не надо будет загружать видео полностью или начинать просматривать его.

Постер также может пригодиться, если элемент src указан некорректно или браузер не поддерживает ни один из предложенных форматов видео. Единственным недостатком использования постеров является дополнительный запрос файла, который незначительно снижает пропускную способность и требует обработки. Дополнительную информацию вы можете найти в [этой статье по оптимизации изображений](../../performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html#image-optimization).

<img class="center" alt="Chrome DevTools screenshot: Accept-Ranges: bytes"
src="images/Accept-Ranges-Chrome-Dev-Tools.png" />

### Добавьте постер

Add a poster attribute to the `video` element so that your users have an idea of the content as soon as the element loads, without needing to download video or start playback.

    <video poster="poster.jpg" ...>
      ...
    </video>
    

Некоторые форматы видео не поддерживаются всеми платформами. Уточните, какие форматы поддерживаются основными платформами, и убедитесь, что ваше видео воспроизводится на каждой из них.

Чтобы уточнить, какие форматы видео поддерживаются, используйте canPlayType(). При этом способе берется строка аргументов, состоящая из mime-type и дополнительных кодеков, и возвращается одно из следующих значений:

<div class="attempt-left">
  <figure>
    <img alt="Android Chrome screenshot, portrait: no poster"
    src="images/Chrome-Android-video-no-poster.png">
    <figcaption>
      Android Chrome screenshot, portrait: no poster
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Android Chrome screenshot, portrait: with poster"
    src="images/Chrome-Android-video-poster.png">
    <figcaption>
      Android Chrome screenshot, portrait: with poster
     </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

## Как настроить резервные варианты для устаревших платформ

Ниже приведены примеры аргументов canPlayType() и возвращаемых значений при работе в Chrome:

### Проверьте, какие форматы поддерживаются

Существует целый ряд инструментов, с помощью которых можно сохранить видео в различных форматах:

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Возвращаемое значение</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Return value">(пустая строка)</td>
      <td data-th="Description">Контейнер и/или кодек не поддерживаются.</td>
    </tr>
    <tr>
      <td data-th="Return value"><code>maybe</code></td>
      <td data-th="Description">
        Контейнер и кодеки, возможно, поддерживаются, но браузеру
        необходимо загрузить какой-либо видеофайл для проверки.
      </td>
    </tr>
    <tr>
      <td data-th="Return value"><code>probably</code></td>
      <td data-th="Description">Видимо, формат поддерживается.
      </td>
    </tr>
  </tbody>
</table>

Хотите знать, какой формат видео был на самом деле выбран браузером?

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Тип</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Type"><code>video/xyz</code></td>
      <td data-th="Response">(пустая строка)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response">(пустая строка)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="nonsense, noise"</code></td>
      <td data-th="Response">(пустая строка)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/mp4; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response"><code>probably</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm</code></td>
      <td data-th="Response"><code>maybe</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm; codecs="vp8, vorbis"</code></td>
      <td data-th="Response"><code>probably</code></td>
    </tr>
  </tbody>
</table>

### Запишите видео в нескольких форматах

Чтобы получить данные о том, какой источник использовался, укажите в JavaScript свойство видео currentSrc.

* Убедитесь, что ваш сервер поддерживает запросы с диапазонами. Запросы с диапазонами по умолчанию включены на большинстве серверов, однако некоторые хостинги отключают их.
* GUI applications: [Miro](http://www.mirovideoconverter.com/), [HandBrake](//handbrake.fr/), [VLC](//www.videolan.org/)
* Online encoding/transcoding services: [Zencoder](//en.wikipedia.org/wiki/Zencoder), [Amazon Elastic Encoder](//aws.amazon.com/elastictranscoder)

### Проверьте, какие форматы использовались

Хотите посмотреть, как это работает? Вот [пример](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html). Chrome и Firefox используют chrome.webm (поскольку это первый пункт в списке поддерживаемых браузером форматов). В то же время Safari использует chrome.mp4.

Размер видеофайла имеет значение, поскольку влияет на восприятие контента.

## Корректно определите размер видеофайлов

Реальный размер видеокадра может отличаться от размера элемента video (так же, как размеры демонстрируемого изображения могут отличаться от изначальных).

### Проверьте размер видео

* Инструменты для ПК: [FFmpeg](//ffmpeg.org/)
* GUI-приложения: [Miro](//www.mirovideoconverter.com/), [HandBrake](//handbrake.fr/), [VLC](//www.videolan.org/)
* Онлайн-сервисы для кодирования и транскодирования: [Zencoder](//en.wikipedia.org/wiki/Zencoder), [Amazon Elastic Encoder](//aws.amazon.com/elastictranscoder)

### Убедитесь, что видеофайлы не превышают максимальные размеры контейнеров

Чтобы проверить кодированный размер видео, используйте такие свойства элемента video, как videoWidth и videoHeight. Свойства width и height сообщают размеры элемента video, которые были определены с помощью атрибутов высоты и ширины CSS или встроенных атрибутов.

Когда элементы video слишком велики для области просмотра, они могут переполнить контейнер. В таком случае пользователь не сможет посмотреть видео или использовать кнопки управления

### Изменение ориентации на различных устройствах

When video elements are too big for the viewport, they may overflow their container, making it impossible for the user to see the content or use the controls.

<div class="attempt-left">
  <figure>
    <img alt="Android Chrome screenshot, portrait: unstyled video element overflows
    viewport" src="images/Chrome-Android-portrait-video-unstyled.png">
    <figcaption>
      Android Chrome screenshot, portrait: unstyled video element overflows viewport
    </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Android Chrome screenshot, landscape: unstyled video element overflows
    viewport" src="images/Chrome-Android-landscape-video-unstyled.png">
    <figcaption>
      Android Chrome screenshot, landscape: unstyled video element overflows viewport
    </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Вы можете контролировать размеры видео с помощью JavaScript или CSS. Библиотеки и плагины JavaScript, такие как [FitVids](//fitvidsjs.com/), позволяют задать требуемый размер и соотношение сторон даже для Flash video с YouTube и других ресурсов.

Используйте [медиа-запросыCSS](../../layouts/rwd-fundamentals/#use-css-media-queries-for-responsiveness), чтобы определить размер элементов в зависимости от размера области просмотра. `max-width: 100%` - отличный вариант!

{# include shared/related_guides.liquid inline=true list=page.related-guides.media #}

Для медиаконтента в окнах iframe (в частности, видео на YouTube) попробуйте отзывчивый подход (например, [предложенный Джоном Сурдаковски](//avexdesigns.com/responsive-youtube-embed/)).

**CSS:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="styling"   adjust_indentation="auto" %}
</pre>

**CSS:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="markup"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/responsive_embed.html)

Сравните [образец видео, настроенного в соответствии с концепцией отзывчивого дизайна](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/responsive_embed.html), с [видео, не учитывающим этой концепции](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/unyt.html).

## Как настроить видеопроигрыватель

Различные платформы по-разному отображают видео. Мобильные решения должны учитывать расположение устройства. Для управления полноэкранным воспроизведением видео используйте Fullscreen API.

### Встроенное видео или полноэкранный режим

Различные платформы по-разному отображают видео. Мобильные решения должны учитывать ориентацию устройства. Для управления полноэкранным воспроизведением видео используйте Fullscreen API.

Ориентация не имеет никакого значения для настольных компьютеров и ноутбуков - ведь они всегда находятся в одном положении. Однако при разработке веб-страниц для смартфонов и планшетов учитывать ориентацию устройств чрезвычайно важно.

<div class="attempt-left">
  <figure>
    <img  alt="Screenshot of video playing in Safari on iPhone, portrait"
    src="images/iPhone-video-playing-portrait.png">
    <figcaption>Screenshot of video playing in Safari on iPhone, portrait</figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Screenshot of video playing in Safari on iPhone, landscape"
    src="images/iPhone-video-playing-landscape.png">
    <figcaption>Screenshot of video playing in Safari on iPhone, landscape</figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Safari для iPhone отлично справляется с задачей переключения между вертикальной и горизонтальной ориентацией:

<img alt="Скриншот видеопроигрывателя в Safati на iPhone, вертикальная ориентация"
src="images/iPad-Retina-landscape-video-playing.png" />

На iPad, а также в браузере Chrome для Android изменение ориентации устройства может оказаться более проблематичным. В частности, ненастроенный видеопроигрыватель на iPad в горизонтальном расположении выглядит так:

## Для пользователей с ограниченными возможностями

<img class="attempt-right" alt="Скриншот видеопроигрывателя в Safati на iPad Retina, горизонтальная ориентация"
src="images/iPhone-video-with-poster.png" />

Если вы установите настройки видео "width: 100%" или "max-width: 100%" с помощью CSS, это поможет решить многие проблемы, связанные с ориентацией устройства. Вы также можете использовать полноэкранный режим просмотра.

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Chrome on Android,
portrait" src="images/Chrome-Android-video-playing-portrait-3x5.png" />

On Android, users can request request fullscreen mode by clicking the fullscreen icon. But the default is to play video inline:

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Safari on iPad
Retina, landscape" src="images/iPad-Retina-landscape-video-playing.png" />

Safari on an iPad plays video inline:

<div style="clear:both;"></div>

### Управление полноэкранным отображением контента

Safari для iPad воспроизводит видео как встроенное:

To full screen an element, like a video:

    elem.requestFullScreen();
    

Платформы, которые не запускают автоматически полноэкранный режим, [в большинстве случаев поддерживают](//caniuse.com/fullscreen) Fullscreen API. Используйте этот API для управления полноэкранным отображением видео или страницы.

    document.body.requestFullScreen();
    

Полноэкранное отображение элемента, например video:

    video.addEventListener("fullscreenchange", handler);
    

Полноэкранное отображение всего документа:

    console.log("In full screen mode: ", video.displayingFullscreen);
    

Вы также можете установить прослушивание событий, связанных с изменением полноэкранного режима:

<video autoplay muted loop class="attempt-right">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.mp4" type="video/mp4">
  <p>Этот браузер не поддерживает элемент video.</p>
</video>

Или проверить, отображается ли элемент в полноэкранном режиме:

Кроме того, вы можете использовать псевдо-класс CSS `fullscreen` для изменения способа отображения элементов в полноэкранном режиме.

На устройствах, поддерживающих Fullscreen API, вы можете использовать значки для обозначения места под видео:

<div style="clear:both;"></div>

## Краткий справочник

Хотите посмотреть, как это работает? Тогда вам [сюда](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/fullscreen.html).

### Добавьте субтитры для пользователей с ограниченными возможностями

<img class="attempt-right" alt="Screenshot showing captions displayed using the
track element in Chrome on Android" src="images/Chrome-Android-track-landscape-5x3.jpg" />

Доступность контента для всех категорий пользователей очень важна. Люди с нарушениями слуха или зрения не смогут ознакомиться с вашим видео, если в нем не будет субтитров или голосового описания (тифлокомментариев). На их добавление у вас уйдет совсем немного времени. Зато все пользователи смогут ознакомиться с вашими материалами. Немного усилий - и контент доступен для всех!

<div style="clear:both;"></div>

### Добавьте элемент track

Чтобы с видео могли ознакомиться все пользователи мобильных устройств, добавьте субтитры или голосовое описание при помощи элемента track.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/track.html" region_tag="track"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/track.html)

При использовании элемента track субтитры будут выглядеть следующим образом:

### Маркируйте субтитры в файле трека

A track file consists of timed "cues" in WebVTT format:

    WEBVTT
    
    00:00.000 --> 00:04.000
    На ветке дерева сидит человек с ноутбуком.
    
    00:05.000 --> 00:08.000
    Ветка ломается, и он начинает падать.
    
    ...
    

## Quick Reference

### Атрибуты элемента video

Вставить субтитры легко - просто добавьте track как дочерний элемент video:

<table>
  <thead>
    <tr>
      <th>Attribute</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Attribute"><code>src</code></td>
      <td data-th="Description">Все браузеры</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>poster</code></td>
      <td data-th="Description">Все браузеры</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>preload</code></td>
      <td data-th="Description">Все мобильные браузеры игнорируют предварительную загрузку. </td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>autoplay</code></td>
      <td data-th="Description">Не поддерживается на iPhone и устройствах Android, поддерживается во всех браузерах для ПК, Firefox и Opera для Android, а также на iPad.</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>loop</code></td>
      <td data-th="Description">Все браузеры</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>controls</code></td>
      <td data-th="Description">Все браузеры</td>
    </tr>
  </tbody>
</table>

### JavaScript

Атрибут src элемента track определеяет местоположение файла с треком.

Файл трека состоит из разбитых по времени кусочков текста в формате WebVTT:

* Потребляемый трафик может довольно дорого стоить для пользователя.
* Автоматическая загрузка и воспроизведение видео может заметно снизить пропускную способность и сильно загрузить процессор. Это, в свою очередь, вызовет задержки в отрисовке страницы.
* Автовоспроизведение видео или аудио может оказаться слишком навязчивым для пользователя.

Краткий обзор свойств элемента video.

### Preload {: #preload }

Полный список атрибутов элемента video и их описание вы можете найти [здесь](//www.w3.org/TR/html5/embedded-content-0.html#the-video-element).

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Значение</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Value"><code>none</code></td>
      <td data-th="Description">Пользователь может даже не смотреть видео; предварительная загрузка не требуется</td>
    </tr>
    <tr>
      <td data-th="Value"><code>metadata</code></td>
      <td data-th="Description">Метаданные (длительность, размер, текстовые треки) должны быть предварительно загружены с минимальной загрузкой видео.</td>
    </tr>
    <tr>
      <td data-th="Value"><code>auto</code></td>
      <td data-th="Description">Желательна немедленная загрузка всего видео целиком.</td>
    </tr>
  </tbody>
</table>

На ПК атрибут autoplay означает, что браузер должен немедленно начать загрузку и воспроизведение видео. На iOS и в Chrome для Android атрибут autoplay не работает. Пользователь должен нажать на экран для воспроизведения видео.

### JavaScript

Даже в тех случаях, когда автовоспроизведение работает, вы должны тщательно оценить необходимость его подключения:

#### Автовоспроизведение

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Property &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Property"><code>currentTime</code></td>
      <td data-th="Description">Устанавливает время начала воспроизведения в секундах.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>volume</code></td>
      <td data-th="Description">Устанавливает уровень громкости воспроизведения видео.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>muted</code></td>
      <td data-th="Description">Отключает звук при воспроизведении видео.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>playbackRate</code></td>
      <td data-th="Description">Устанавливает скорость воспроизведения. 1 - нормальная скорость.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>buffered</code></td>
      <td data-th="Description">Информация о том, какой объем видео буферизован и готов к воспроизведению (посмотрите <a href="http://people.mozilla.org/~cpearce/buffered-demo.html" title="Пример, демонстрирующий буферизацию видео в элементе canvas">пример</a>).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>currentSrc</code></td>
      <td data-th="Description">Адрес, по которому находится воспроизводимое видео.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoWidth</code></td>
      <td data-th="Description">Ширина видео в пикселях (может отличаться от ширины элемента video).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoHeight</code></td>
      <td data-th="Description">Высота видео в пикселях (может отличаться от высоты элемента video).</td>
    </tr>
  </tbody>
</table>

#### Предварительная загрузка

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Method &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Method"><code>load()</code></td>
      <td data-th="Description">Загрузить или перезагрузить источник видео, не начиная воспроизведение, например когда атрибут src элемента video изменен с помощью JavaScript.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>play()</code></td>
      <td data-th="Description">Воспроизвести видео, начиная с текущей позиции.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>pause()</code></td>
      <td data-th="Description">Приостановить воспроизведение видео в текущей позиции.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>canPlayType('format')</code></td>
      <td data-th="Description">Определить, какие форматы поддерживаются (см. раздел `Уточните, какие форматы поддерживаются`).</td>
    </tr>
  </tbody>
</table>

Параметры автовоспроизведения можно настроить в Android WebView с помощью [WebSettings API](//developer.android.com/reference/android/webkit/WebSettings.html#setMediaPlaybackRequiresUserGesture(boolean)). По умолчанию ставится значение true, однако приложение WebView может отключить его.

#### Свойства

Атрибут preload сообщает браузеру, какой объем информации или контента должен быть предварительно загружен.

<table class="responsive">
  <thead>
  <tr>
    <th colspan="2">Event &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Event"><code>canplaythrough</code></td>
      <td data-th="Description">Инициируется, когда доступен достаточный объем данных, чтобы браузер мог воспроизвести видео целиком без остановок.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>ended</code></td>
      <td data-th="Description">Инициируется, когда воспроизведение видео закончено.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>error</code></td>
      <td data-th="Description">Инициируется, когда произошла ошибка.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>playing</code></td>
      <td data-th="Description">Инициируется, когда видео воспроизводится в первый раз, после паузы или после перезапуска.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>progress</code></td>
      <td data-th="Description">Периодически инициируется для обозначения прогресса процесса загрузки.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>waiting</code></td>
      <td data-th="Description">Инициируется, когда действие отложено в ожидании завершения иного действия.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>loadedmetadata</code></td>
      <td data-th="Description">Инициируется, когда браузер завершил загрузку метаданных видео: длительности, размеров и текстовых треков.</td>
    </tr>
  </tbody>
</table>

## Feedback {: #feedback }

Атрибут preload работает по-разному на различных платформах. Например, Chrome буферизует 25 секунд видео на ПК, однако на iOS или Android не буферизует ничего. Это означает, что при воспроизведении видео на мобильном устройстве может возникнуть некоторая задержка, чего не может случиться на ПК. Более подробную информацию вы можете найти на [тестовой странице Стива Судера](//stevesouders.com/tests/mediaevents.php).