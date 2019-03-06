project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Красивый и удобный шрифт - неотъемлемая составляющая хорошего сайта. Благодаря ему нам доступен привлекательный дизайн бренда, более простое чтение текста и удобный интерфейс. Но веб-шрифты дают нам дополнительные возможности! Они позволяют выбрать, найти и увеличить текст в любой момент. Кроме того, надписи не будут зависеть от разрешения экрана и останутся четкими на всех устройствах.

{# wf_updated_on: 2014-09-29 #} {# wf_published_on: 2014-09-19 #}

# Оптимизация шрифтов {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*This article contains contributions from [Monica Dinculescu](https://meowni.ca/posts/web-fonts/), [Rob Dodson](/web/updates/2016/02/font-display), and Jeff Posnick.*

Оптимизация шрифтов - это одно из важнейших действий для улучшения производительности страницы. Каждый шрифт - это отдельный ресурс, и некоторые из них могут блокировать отрисовку текста. Однако сам факт того, что на странице есть шрифты, не означает, что она будет обрабатываться медленнее. Наоборот, оптимизированный шрифт в сочетании с продуманной стратегией загрузки и применения поможет уменьшить общий размер страницы и ускорит ее обработку.

Шрифт состоит из глифов - векторных форм каждой буквы или символа. Поэтому размер файла шрифта зависит от двух переменных: количества глифов в шрифте и сложности их векторных контуров. Например, Open Sans, один из самых популярных веб-шрифтов, содержит 897 глифов, включая латинские, греческие и кириллические символы.

## Анатомия шрифта

### TL;DR {: .hide-from-toc }

* Шрифты Unicode могут содержать тысячи глифов.
* Существует четыре формата шрифтов: WOFF2, WOFF, EOT и TTF.
* Для некоторых форматов шрифтов необходимо GZIP-сжатие.

A *webfont* is a collection of glyphs, and each glyph is a vector shape that describes a letter or symbol. As a result, two simple variables determine the size of a particular font file: the complexity of the vector paths of each glyph and the number of glyphs in a particular font. For example, Open Sans, which is one of the most popular webfonts, contains 897 glyphs, which include Latin, Greek, and Cyrillic characters.

<img src="images/glyphs.png"  alt="Font glyph table" />

Конечно, придется поработать над установкой соответствующих настроек, чтобы шрифт не ухудшил работу ресурса. Однако на веб-платформах вы найдете все необходимые инструменты. Далее мы подробно рассмотрим, как совместить красивое оформление и высокую производительность.

Сегодня в Интернете используются четыре формата контейнеров шрифтов: [EOT](http://ru.wikipedia.org/wiki/Embedded_OpenType), [TTF](http://ru.wikipedia.org/wiki/TrueType), [WOFF](http://ru.wikipedia.org/wiki/Web_Open_Font_Format) и [WOFF2](http://www.w3.org/TR/WOFF2/){: .external }. К сожалению, несмотря на возможность выбора, не существует единого формата, который работает во всех браузерах. Например, EOT доступен [только в IE](http://caniuse.com/#feat=eot), TTF поддерживается в этом браузере только [частично](http://caniuse.com/#search=ttf). WOFF распространен шире всего, однако его нельзя использовать [в некоторых старых браузерах](http://caniuse.com/#feat=woff), а над поддержкой WOFF 2.0 [работают в настоящее время](http://caniuse.com/#feat=woff2).

### Форматы шрифтов

Итак, единого формата для всех браузеров не существует, поэтому мы должны использовать несколько форматов, чтобы страница выглядело одинаково у всех пользователей:

Note: Теоретически, существует ещё один формат контейнера шрифтов - [SVG](http://caniuse.com/svg-fonts). Однако он никогда не поддерживался в IE и Firefox, и сейчас перестает использоваться в Chrome. Из-за ограниченной сферы применения мы специально не рассказываем о нем в этом руководстве.

* Используйте WOFF 2.0 в тех браузерах, которые его поддерживают.
* Добавьте WOFF для большинства браузеров.
* Применяйте TTF в устаревших браузерах Android (для версий до 4.4).
* Добавьте EOT для поддержки устаревших версий IE (до IE9). ^

Шрифт - это набор глифов, каждый из которых состоит из контуров букв. Конечно, все глифы разные, но, тем не менее, они содержат много похожей информации, которую можно сжать с помощью GZIP или другого совместимого компрессора.

### Уменьшение размера шрифта с помощью сжатия

Стоит отметить, что некоторые форматы шрифтов содержат дополнительные метаданные, например информацию о [хинтинге](http://ru.wikipedia.org/wiki/Хинтинг) и [кернинге](http://ru.wikipedia.org/wiki/Кернинг). Они не нужны не для всех платформ, поэтому мы можем сжать файл ещё больше. Узнайте о соответствующих функциях вашего компрессора шрифтов и убедитесь, что у вас есть подходящие ПО для тестирования ресурсов и предоставления их браузерам. Например, Google Fonts поддерживает более 30 оптимизированных вариантов каждого шрифта и автоматически определяет и применяет оптимальный вариант для каждой платформы и браузера.

* Форматы EOT и TTF не сжимаются по умолчанию. Убедитесь, что на ваших серверах настроено [сжатие GZIP](/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text-compression-with-gzip) при передаче ресурсов в этих форматах.
* К формату WOFF применяется встроенное сжатие. Убедитесь, что компрессор использует оптимальный вариант соответствующих настроек.
* WOFF2 использует пользовательские алгоритмы предварительной обработки и сжатия для уменьшения размера файла на 30% по сравнению с другими форматами. Ознакомьтесь [с отчетом](http://www.w3.org/TR/WOFF20ER/){: .external }.

Note: Используйте [сжатие Zopfli](http://en.wikipedia.org/wiki/Zopfli) для форматов EOT, TTF и WOFF. Этот совместимый с zlib компрессор сжимает файлы примерно на 5% сильнее, чем gzip.

С помощью CSS-правила @font-face можно определить расположение определенного шрифта, его свойства стиля и кодовые точки Unicode, для которых он должен использоваться. Используйте сочетание таких объявлений @font-face для создания семейства шрифтов. Браузер будет использовать его, что понять, какие шрифты нужно скачать и применить для текущей страницы. Рассмотрим подробнее, как это работает.

## Создание семейства шрифтов с помощью @font-face

### TL;DR {: .hide-from-toc }

* Используйте подсказку format(), чтобы указать форматы шрифтов.
* Чтобы ускорить работу сайта, разделяйте крупные Unicode-шрифты на поднаборы. Используйте субнастройки Unicode-диапазона и вручную добавьте запасной вариант для более старых браузеров.
* Чтобы ускорить работу сайта и отрисовку страниц, сократите количество вариантов стиля для каждого шрифта.

В каждом объявлении @font-face указано название семейства шрифтов, которые действует как логическая группа из нескольких объявлений, [характеристик шрифта](http://www.w3.org/TR/css3-fonts/#font-prop-desc), например стиля, толщины и интервала, и [дескриптора src](http://www.w3.org/TR/css3-fonts/#src-desc), который определяет список расположений шрифта в порядке важности.

### Выбор формата

Обратите внимание, что примеры выше определяют два стиля (обычный и *курсив*) одного семейства шрифтов *Awesome Font*, каждый из которых указывает на разный набор ресурсов. В свою очередь, каждый дескриптор src содержит разделенный запятыми список вариантов ресурса в порядке важности:

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome.woff2') format('woff2'), 
           url('/fonts/awesome.woff') format('woff'),
           url('/fonts/awesome.ttf') format('ttf'),
           url('/fonts/awesome.eot') format('eot');
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: italic;
      font-weight: 400;
      src: local('Awesome Font Italic'),
           url('/fonts/awesome-i.woff2') format('woff2'), 
           url('/fonts/awesome-i.woff') format('woff'),
           url('/fonts/awesome-i.ttf') format('ttf'),
           url('/fonts/awesome-i.eot') format('eot');
    }
    

^ Note: На устройстве пользователя, особенно мобильном, редко установлены какие-либо шрифты, кроме системных. Поскольку добавить шрифт на телефон или планшет практически невозможно, вы всегда должны предоставлять список внешних расположений ресурса.

* Директива local() позволяет ссылаться на шрифт и загружать его, а также использовать шрифты, установленные на устройстве пользователя.
* С помощью директивы url() можно загружать внешние шрифты. Она может содержать подсказку format(), указывающую формат шрифта по ссылке.

Когда браузер определяет, что для отображении сайта нужен какой-либо шрифт, он читает предоставленных список ресурсов в указанном порядке и старается скачать нужный шрифт. Например, используя следующий пример:

С помощью сочетания локальных и внешних директив и подсказки мы можем указать все доступные форматы шрифта. После этого браузер сделает все остальное: определит нужные ресурсы и выберет подходящий формат.

1. Браузер читает разметку страницы и определяет, какие варианты шрифтов нужны для отрисовки текста.
2. Браузер проверяет, не установлены ли нужные шрифты на устройстве.
3. Если файла нет на устройстве, браузер читает список внешних расположений: 
    * Если формат указан, перед скачивание браузер проверяется, поддерживается ли он. В случае отрицательного ответа программа переходит к следующему варианту.
    * Если указание на формат отсутствует, браузер скачивает ресурс.

Note: Порядок, в котором указаны варианты шрифтов, имеет значение, потому что браузер выбирает первый поддерживаемый шрифт. Таким образом, если вы хотите, чтобы современные браузеры использовали WOFF2, укажите его до WOFF, и т. д.

Кроме таких характеристик шрифтов, как стиль, толщина и интервал, правило @font-face позволяет установить набор кодовых точек Unicode, поддерживаемых ресурсом. Благодаря этому мы можем разделить крупный Unicode-шрифт на несколько небольших поднаборов (например, латиницу, кириллицу и греческий алфавит) и скачивать только те глифы, которые нужны для отрисовки текста на конкретной странице.

### Субнастройки диапазона Unicode

С помощью [дескриптора диапазона Unicode](http://www.w3.org/TR/css3-fonts/#descdef-unicode-range) мы можем создать разделенный запятыми список значений диапазона. Каждое из них может быть указано в одной из трех форм:of three different forms:

Например, мы можем разделить семейство *Awesome Font* на латинский и японский поднаборы, которые браузер при необходимости может скачать:

* Одна кодовая точка (например, U+416)
* Диапазон интервала (например, U+400-4ff). Обозначает начанльную и конечную кодовые точки диапазона.
* Диапазон Wildcard (например, U+4??): '?' Символы обозначают любое шестандцатиричное число.

Note: Субнастройки диапазона Unicode особенно важны для азиатских языков, в которых намного больше глифов, чем в западных. Стандартный `полный` шрифт часто весит мегабайты, а не несколько десятков килобайт.

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-jp.woff2') format('woff2'), 
           url('/fonts/awesome-jp.woff') format('woff'),
           url('/fonts/awesome-jp.ttf') format('ttf'),
           url('/fonts/awesome-jp.eot') format('eot');
      unicode-range: U+3000-9FFF, U+ff??; /* Japanese glyphs */
    }
    

С помощью поднаборов диапазона Unicode и отдельных файлов для каждого варианта стиля мы можем указать сложное семейство шрифтов, которое быстро загружается и меньше весит. Пользователь будет скачивать только те варианты и поднаборы, которые он увидит на странице, и ничего больше.

Однако у этого метода есть маленький недостаток: [не все браузеры пока поддерживают](http://caniuse.com/#feat=font-unicode-range) диапазон Unicode. Одни просто игнорируют соответствующую подсказку и скачивают все варианты, а другие могут вообще не обрабатывать объявление @font-face. Чтобы решить эту проблему, мы должны вручную установить субнастройки в более старых браузерах.

Поскольку устаревшие браузеры не могут выбрать только нужный поднабор и создать сложный шрифт, нам нужно на всякий случай добавит один ресурс, который содержит необходимые поднаборы, и скрыть остальное от браузера. Например, если на странице используются только символы латиницы, мы можем удалить другие глифы и отправлять это поднабор как отдельный ресурс.

Семейство шрифтов состоит из нескольких вариантов стиля (обычный, полужирный, курсив) и вариантов жирности для каждого из них. Глифы в этих вариантах могут значительно отличаться, например интервалом, размером или даже формой.

1. **Как определить нужные поднаборы?** 
    * Если субнастройки диапазона Unicode поддерживаются браузером, он автоматически выберет необходимый поднабор. Вам нужно только добавить соответствующие файлы на страницу и указать требуемые диапазоны в правилах @font-face.
    * Если диапазон Unicode недоступен, следует скрыть все ненужные поднаборы, то есть указать подходящий.
2. **Как создать поднабор шрифта?** 
    * Для оптимизации шрифтов и создания поднаборов используйте [инструмент pyftsubset](https://github.com/behdad/fonttools/blob/master/Lib/fontTools/subset.py#L16) с открытым кодом.
    * Некоторые сервисы шрифтов позволяют вручную установить нужные субнастройки с помощью параметров запроса. Вы сами сможете выбрать требуемый поднабор для вашей страницы. Чтобы получить более подробную информацию, ознакомьтесь с документацией вашего поставщика шрифтов.

### Выбор и синтез шрифта

Each font family is composed of multiple stylistic variants (regular, bold, italic) and multiple weights for each style, each of which, in turn, may contain very different glyph shapes&mdash;for example, different spacing, sizing, or a different shape altogether.

<img src="images/font-weights.png"  alt="Font weights" />

Выбор варианта *курсива* происходит точно так же. Дизайнер шрифта создает разные версии, а мы выбираем, какие из них мы будем использовать. Не указывайте слишком много вариантов, поскольку каждый из них требует отдельного скачивания. Например, мы можем определить два полужирных способа начертания для семейства шрифтов *Awesome Font*:

> When a weight is specified for which no face exists, a face with a nearby weight is used. In general, bold weights map to faces with heavier weights and light weights map to faces with lighter weights.
> 
> > [CSS3 font matching algorithm](http://www.w3.org/TR/css3-fonts/#font-matching-algorithm)

В примере выше указано семейство шрифтов Awesome Font, состоящее из двух ресурсов. Они содержат одинаковый набор глифов для латиницы (U+000-5FF), но в разных вариантах жирности: стандартном (400) и полужирном (700). Что произойдет, если в CSS-правиле будет указано другое значение жирность или стиль `курсив`?

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 700;
      src: local('Awesome Font'),
           url('/fonts/awesome-l-700.woff2') format('woff2'), 
           url('/fonts/awesome-l-700.woff') format('woff'),
           url('/fonts/awesome-l-700.ttf') format('ttf'),
           url('/fonts/awesome-l-700.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    

The above example declares the *Awesome Font* family that is composed of two resources that cover the same set of Latin glyphs (`U+000-5FF`) but offer two different "weights": normal (400) and bold (700). However, what happens if one of your CSS rules specifies a different font weight, or sets the font-style property to italic?

* Если точное совпадение шрифта отсутствует, браузер выберет ближайший доступный вариант.
* Если на найден версия стиля (например, в примере выше мы не указали версию для варианта для курсива), браузер сам синтезирует начертание.

<img src="images/font-synthesis.png"  alt="Font synthesis" />

Note: Чтобы надписи на вашем сайте выглядели привлекательно, лучше не использовать синтез шрифтов. Вместо этого уменьшите количество вариантов шрифтов и укажите их расположение. Тогда браузер сможет при необходимости скачать их. Тем не менее, в некоторых случаях использование синтезированных версий [допустимо](https://www.igvita.com/2014/09/16/optimizing-webfont-selection-and-synthesis/).

`Полный` веб-шрифт со всеми глифами и вариантами стиля, которые могут нам не понадобиться, может весить несколько мегабайт. Чтобы избежать этой проблемы, CSS-правило @font-face позволяет разделить семейство шрифтов на набор ресурсов: поднаборы Unicode, отдельные варианты стиля и т. д.

С помощью объявлений браузер определяет необходимые поднаборы и варианты и скачивает только нужные данные. Это очень удобно, но если мы не будем достаточно внимательны, может произойти ошибка процесса отрисовки. Это замедлит отображение текста и ухудшит работу сайта, чего мы не можем допустить.

## Оптимизация загрузки и отрисовки

### TL;DR {: .hide-from-toc }

* Запросы шрифтов отправляются только после создания модели визуализации, поэтому отображение текста может быть задержано.
* Font Loading API позволяет изменить исходные настройки отложенной загрузки и использовать собственные стратегии загрузки и отображения шрифтов.

У отложенной загрузки шрифтов есть скрытое свойство, которое может замедлить отрисовку текста. Прежде чем определить, как шрифты нужны для показа страницы, браузер должен [создать модель визуализации](/web/fundamentals/performance/critical-rendering-path/render-tree-construction), которая зависит от моделей DOM и CSSOM. Таким образом, запросы для шрифтов будут отправлены позднее, чем для других первоочередных ресурсов. Возможно, браузер не покажет текст до того, как будут скачаны остальные ресурсы.

Given these declarations, the browser figures out the required subsets and variants and downloads the minimal set required to render the text, which is very convenient. However, if you're not careful, it can also create a performance bottleneck in the critical rendering path and delay text rendering.

### Веб-шрифты и процесс визуализации

Между началом показа контента на странице, которое происходит вскоре после создания модели визуализации, и запросом к шрифту идет "гонка". Из-за этого может возникнуть проблема отсутствия текста. У разных браузеров этот процесс может отличаться:

<img src="images/font-crp.png"  alt="Font critical rendering path" />

1. Браузер запрашивает HTML-документ.
2. Браузер анализирует HTML-ответ и создает модель DOM.
3. Браузер обнаруживает ресурсы CSS, JavaScript и т. д. и отправляет запросы.
4. После получения CSS-контента браузер создает модель CSSOM и совмещает ее с моделью DOM для получения модели визуализации. 
    * После того как модель визуализации определила необходимые варианты шрифтов, отправляются соответствующие запросы.
5. Браузер отображает макет страницы и ресурсы на нем. 
    * Если шрифт ещё не доступен, браузер может не показывать текст.
    * Как только шрифт становится доступен, браузер отображает текст.

С помощью интерфейса сценариев [Font Loading API](http://dev.w3.org/csswg/css-font-loading/) можно определить CSS-начертание веб-шрифта, управлять им, отслеживать процесс скачивания и менять исходные настройки отложенной загрузки. Например, если мы уверены, что потребуется какой-либо вариант шрифта, мы можем указать это и настроить в браузере немедленное скачивание этого ресурса.

Мы можем проверить статус шрифта с помощью директивы [check()](http://dev.w3.org/csswg/css-font-loading/#font-face-set-check)) и отследить процесс его скачивания. Это позволяет задать собственные правила отрисовки текста на странице:

### Оптимизации отрисовки шрифта с помощью Font Loading API

Кроме того, можно совмещать указанные выше стратегии для разного контента на странице: задерживать отображение текста на определенное время или пока шрифт не станет доступным, использовать запасных ресурсы а затем заново отрисовывать страницу и т. д.

Note: В некоторых браузерах Font Loading API пока находится [на стадии разработки](http://caniuse.com/#feat=font-loading). Чтобы вам были доступны его функции, используйте [полизаполнение FontLoader](https://github.com/bramstein/fontloader) или [библиотеку webfontloader](https://github.com/typekit/webfontloader). Тем не менее, вам придется использовать дополнительные ресурсы - средства поддержки JavaScript.

Чтобы решить проблему отсутствия текста, можно использовать другой метод - встроить шрифты в таблицу стилей CSS.

```html
var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
  style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
});

font.load(); // don't wait for render tree, initiate immediate fetch!

font.ready().then(function() {
  // apply the font (which may rerender text and cause a page reflow)
  // once the font has finished downloading
  document.fonts.add(font);
  document.body.style.fontFamily = "Awesome Font, serif";

  // OR... by default content is hidden, and rendered once font is available
  var content = document.getElementById("content");
  content.style.visibility = "visible";

  // OR... apply own render strategy here... 
});
```

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

Эта техника не очень гибка и не позволяет устанавливать время задержки или стратегии визуализации для разного контента. Однако это простое и надежное решение, которое работает во всех браузерах. Чтобы достичь лучшего результата, поместите все встроенные шрифты в отдельную таблицу стиля и установите для них максимальное значение max-age. Благодаря этому даже если вы обновите CSS, пользователям не придется заново скачивать шрифты.

Note: Не используйте встраивание слишком часто! Помните, что отложенная загрузка шрифтов используется для того, чтобы избежать скачивания ненужных поднаборов и вариантов. Кроме того, из-за большого количества встроенных ресурсов размер CSS увеличится. Это может помешать [процессу визуализации](/web/fundamentals/performance/critical-rendering-path/), потому что браузеру придется скачивать все эти данные. Только после этого он сможет создать модели CSSOM и модели визуализации, а также отобразить контент на странице.

### Оптимизация отрисовки шрифта с помощью встраивания

Обычно шрифты - это статические ресурсы, которые редко обновляются. Убедитесь, что вы указали [соответствующий заголовок ETag](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags) и [оптимальные правила Cache-Control](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) для всех шрифтов.

#### Browser behaviors

Не сохраняйте шрифты в локальном хранилище localStorage или с помощью других способов - это может привести к ухудшения производительности. Используйте HTTP-кеш браузера в сочетании с Font Loading API или библиотекой webfontloader. Благодаря им вы можете быть уверены в правильной и надежной доставке шрифтов.

<table>
  <thead>
    <tr>
      <th data-th="Browser">Browser</th>
      <th data-th="Timeout">Timeout</th>
      <th data-th="Fallback">Fallback</th>
      <th data-th="Swap">Swap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Browser">
        <strong>Chrome 35+</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Opera</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Firefox</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Internet Explorer</strong>
      </td>
      <td data-th="Timeout">
        0 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Safari</strong>
      </td>
      <td data-th="Timeout">
        No timeout
      </td>
      <td data-th="Fallback">
        N/A
      </td>
      <td data-th="Swap">
        N/A
      </td>
    </tr>
  </tbody>
</table>

* Safari отображает текст только после того, как шрифт скачан.
* Chrome и Firefox задерживают отрисовку шрифта до 3 секунд, после чего используют запасной вариант. После скачивания ресурса браузеры применяют его для повторной визуализации текста.
* IE сразу показывает текст с помощью запасного шрифта, а потом заново отображает страницу после скачивания ресурса.

Несмотря на распространенное мнение, веб-шрифты не замедляют отрисовку страницы и не ухудшают производительность. Использование оптимизированных шрифтов может значительно улучшить работу пользователей с приложением или сайтом. Благодаря ним вам доступно оформление бренда, более простое чтение текста, удобный интерфейс и поиск. Правильно настроенные шрифты хорошо смотрятся в любом разрешении на экране всех форматов. Не бойтесь использовать шрифты!

#### The font display timeline

Однако бездумное добавление шрифтов может привести к скачиванию большого количества ресурсов и задержкам в работе сайта. Именно поэтому нам необходимо использовать инструменты оптимизации и помочь браузеру уменьшить размер этих ресурсов, а также и правильно применить и разместетить их.

1. **Проверяйте использование шрифтов.** Не добавляйте слишком много шрифтов и для каждого из них сокращайте число доступных вариантов. Тогда дизайн будет выглядеть однородно, а сайт - быстро реагировать на запросы пользователя.
2. **Разделяйте шрифты на поднаборы.** Вы можете разделить ресурс на Unicode-диапазоны, чтобы отправлять только глифы, нужные на определенной страницу. Это уменьшает размер файла и ускоряет его скачивание. Однако при создании поднаборов не забудьте оптимизировать повторное использование шрифтов, чтобы не скачивать одни и те же символы для разных страниц. Мы советуем использовать поднаборы, основанные на видах письменности, например латинице, кириллице и т. д.
3. **Используйте оптимизированные форматы шрифтов для каждого браузера:** WOFF2, WOFF, EOT и TTF. Убедитесь, что к форматам EOT и TTF применено сжатие GZIP, так как они не сжимаются по умолчанию.

Understanding these periods means you can use `font-display` to decide how your font should render depending on whether or when it was downloaded.

#### Using font-display

To work with the `font-display` property, add it your `@font-face` rules:

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  font-display: auto; /* or block, swap, fallback, optional */
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

`font-display` currently supports the following range of values: `auto | block | swap | fallback | optional`.

* **`auto`** uses whatever font display strategy the user-agent uses. Most browsers currently have a default strategy similar to `block`.

* **`block`** gives the font face a short block period (3s is recommended in most cases) and an infinite swap period. In other words, the browser draws "invisible" text at first if the font is not loaded, but swaps the font face in as soon as it loads. To do this the browser creates an anonymous font face with metrics similar to the selected font but with all glyphs containing no "ink." This value should only be used if rendering text in a particular typeface is required for the page to be usable.

* **`swap`** gives the font face a zero second block period and an infinite swap period. This means the browser draws text immediately with a fallback if the font face isn’t loaded, but swaps the font face in as soon as it loads. Similar to `block`, this value should only be used when rendering text in a particular font is important for the page, but rendering in any font will still get a correct message across. Logo text is a good candidate for **swap** since displaying a company’s name using a reasonable fallback will get the message across but you’d eventually use the official typeface.

* **`fallback`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a short swap period (three seconds is recommended in most cases). In other words, the font face is rendered with a fallback at first if it’s not loaded, but the font is swapped as soon as it loads. However, if too much time passes, the fallback will be used for the rest of the page’s lifetime. `fallback` is a good candidate for things like body text where you’d like the user to start reading as soon as possible and don’t want to disturb their experience by shifting text around as a new font loads in.

* **`optional`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a zero second swap period. Similar to `fallback`, this is a good choice for when the downloading font is more of a "nice to have" but not critical to the experience. The `optional` value leaves it up to the browser to decide whether to initiate the font download, which it may choose not to do or it may do it as a low priority depending on what it thinks would be best for the user. This can be beneficial in situations where the user is on a weak connection and pulling down a font may not be the best use of resources.

`font-display` is [gaining adoption](https://caniuse.com/#feat=css-font-rendering-controls) in many modern browsers. You can look forward to consistency in browser behavior as it becomes widely implemented.

### Повторное использование шрифта с помощью HTTP-кеширования

Used together, `<link rel="preload">` and the CSS `font-display` give developers a great deal of control over font loading and rendering, without adding in much overhead. But if you need additional customizations, and are willing to incur with the overhead introduced by running JavaScript, there is another option.

The [Font Loading API](https://www.w3.org/TR/css-font-loading/) provides a scripting interface to define and manipulate CSS font faces, track their download progress, and override their default lazyload behavior. For example, if you're sure that a particular font variant is required, you can define it and tell the browser to initiate an immediate fetch of the font resource:

    var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
      style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
    });
    
    // don't wait for the render tree, initiate an immediate fetch!
    font.load().then(function() {
      // apply the font (which may re-render text and cause a page reflow)
      // after the font has finished downloading
      document.fonts.add(font);
      document.body.style.fontFamily = "Awesome Font, serif";
    
      // OR... by default the content is hidden, 
      // and it's rendered after the font is available
      var content = document.getElementById("content");
      content.style.visibility = "visible";
    
      // OR... apply your own render strategy here... 
    });
    

Further, because you can check the font status (via the [check()](https://www.w3.org/TR/css-font-loading/#font-face-set-check)) method and track its download progress, you can also define a custom strategy for rendering text on your pages:

* Браузер в первую очередь скачивает таблицы стилей CSS с совпадающими медиа-запросами, потому что эти ресурсы нужны для создания модели CSSOM.
* Если встроит данные шрифта в таблицу стилей CSS, браузер скачает его в первую очередь, не ожидая создания модели визуализации. Так мы можем вручную изменить исходные настройки отложенной загрузки.
* You can use the fallback font to unblock rendering and inject a new style that uses the desired font after the font is available.

Best of all, you can also mix and match the above strategies for different content on the page. For example, you can delay text rendering on some sections until the font is available, use a fallback font, and then re-render after the font download has finished, specify different timeouts, and so on.

Note: The Font Loading API is still [under development in some browsers](http://caniuse.com/#feat=font-loading). Consider using the [FontLoader polyfill](https://github.com/bramstein/fontloader) or the [webfontloader library](https://github.com/typekit/webfontloader) to deliver similar functionality, albeit with even more overhead from an additional JavaScript dependency.

### Proper caching is a must

Font resources are, typically, static resources that don't see frequent updates. As a result, they are ideally suited for a long max-age expiry - ensure that you specify both a [conditional ETag header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), and an [optimal Cache-Control policy](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) for all font resources.

If your web application uses a [service worker](/web/fundamentals/primers/service-workers/), serving font resources with a [cache-first strategy](/web/fundamentals/instant-and-offline/offline-cookbook/#cache-then-network) is appropriate for most use cases.

You should not store fonts using [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) or [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API); each of those has its own set of performance issues. The browser's HTTP cache provides the best and most robust mechanism to deliver font resources to the browser.

## Список методов оптимизации

Contrary to popular belief, the use of webfonts doesn't need to delay page rendering or have a negative impact on other performance metrics. The well-optimized use of fonts can deliver a much better overall user experience: great branding, improved readability, usability, and searchability, all while delivering a scalable multi-resolution solution that adapts well to all screen formats and resolutions. Don't be afraid to use webfonts.

That said, a naive implementation may incur large downloads and unnecessary delays. You need to help the browser by optimizing the font assets themselves and how they are fetched and used on your pages.

* **Audit and monitor your font use:** don't use too many fonts on your pages, and, for each font, minimize the number of used variants. This helps produce a more consistent and a faster experience for your users.
* **Subset your font resources:** many fonts can be subset, or split into multiple unicode-ranges to deliver just the glyphs that a particular page requires. This reduces the file size and improves the download speed of the resource. However, when defining the subsets, be careful to optimize for font re-use. For example, don't download a different but overlapping set of characters on each page. A good practice is to subset based on script: for example, Latin, Cyrillic, and so on.
* **Deliver optimized font formats to each browser:** provide each font in WOFF2, WOFF, EOT, and TTF formats. Make sure to apply GZIP compression to the EOT and TTF formats, because they are not compressed by default.
* **Give precedence to `local()` in your `src` list:** listing `local('Font Name')` first in your `src` list ensures that HTTP requests aren't made for fonts that are already installed.
* **Customize font loading and rendering using `<link rel="preload">`, `font-display`, or the Font Loading API:** default lazyloading behavior may result in delayed text rendering. These web platform features allow you to override this behavior for particular fonts, and to specify custom rendering and timeout strategies for different content on the page.
* **Specify revalidation and optimal caching policies:** fonts are static resources that are infrequently updated. Make sure that your servers provide a long-lived max-age timestamp and a revalidation token to allow for efficient font reuse between different pages. If using a service worker, a cache-first strategy is appropriate.

## Automated testing for web font optimization with Lighthouse {: #lighthouse }

[Lighthouse](/web/tools/lighthouse) can help automate the process of making sure that you're following web font optimization best practices. Lighthouse is an auditing tool built by the Chrome DevTools team. You can run it as a Node module, from the command line, or from the Audits panel of Chrome DevTools. You tell Lighthouse what URL to audit, and then it runs a bunch of tests on the page, and gives you a report of what the page is doing well, and how it can improve.

The following audits can help you make sure that your pages are continuing to follow web font optimization best practices over time:

* [Enable text compression](/web/tools/lighthouse/audits/text-compression)
* [Preload key requests](/web/tools/lighthouse/audits/preload)
* [Uses inefficient cache policy on static assets](/web/tools/lighthouse/audits/cache-policy)
* [All text remains visible during webfont loads](/web/updates/2016/02/font-display)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}