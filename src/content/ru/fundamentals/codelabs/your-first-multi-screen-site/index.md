project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Пользоваться Интернетом можно с помощью разных устройств: от смартфонов с небольшим экраном до широкоэкранных стационарных компьютеров. Узнайте, как создавать сайты, которые одинаково хорошо отображаются на всех этих устройствах.

{# wf_updated_on: 2014-01-05 #} {# wf_published_on: 2013-12-31 #}

# Ваш первый сайт для различных устройств {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

Создавать сайты для различных устройств проще, чем вам кажется. В этом руководстве мы покажем, как создать целевую страницу для нашего [учебного курса по CS256 Mobile Web](https://www.udacity.com/course/mobile-web-development--cs256), которая будет отображаться на разных по ширине экранах.

<img src="images/finaloutput-2x.jpg" alt="несколько устройств, на экранах которых показывается готовый проект" class="attempt-right" />

На первый взгляд создание сайтов, которые будут одинаково хорошо открываться на устройствах с разной шириной и разрешением экрана, может показаться трудновыполнимой задачей.

На самом деле она гораздо проще, чем вы думаете. Убедитесь в этом сами, изучив материалы в этом руководстве. Мы разбили весь процесс создания сайта на два этапа:

Контент — это самый важный компонент любого интернет-ресурса. Именно контент определяет, каким должен быть дизайн сайта. В этом руководстве мы сперва определим, какие материалы будут представлены на сайте, затем опишем структуру страницы для этого контента и создадим страницу с простой линейной разметкой, которая одинаково хорошо выглядит как на узких, так и на широких экранах.

1. Определение информационной архитектуры и структуры страницы.
2. Добавление элементов отзывчивого веб-дизайна, чтобы с сайтом было одинаково удобно работать на любых устройствах.

## Работа с контентом и структурой сайта

Итак, нам необходимы:

### Создание структуры страницы

Мы уже разработали примерную информационную архитектуру сайта и разметку для устройств с разной шириной экрана.

1. Область, в которой в общих чертах будет рассказываться о нашем продукте `С8256: курс по разработке мобильных сайтов`.
2. Форма для сбора информации. С ее помощью мы сможем собирать данные о прользователях, заинтересовавшихся продуктом.
3. Подробное описание и видеоролик.
4. Изображения, показывающие продукт в работе.
5. Таблица со статистическими данными. В нашем случае она нужна для того, чтобы показать, каково процентное соотношение пользователей различных устройств.

#### Создание заголовка и формы

* Определите, с какими материалами вы будете работать.
* Продумайте информационную архитектуру сайта для областей просмотра разной ширины.
* Создайте эскиз страницы с материалами, но пока не добавляйте на нее дизайн.

We have also come up with a rough information architecture and layout for both the narrow and wide viewports.

<div class="attempt-left">
  <figure>
    <img src="images/narrowviewport.png" alt="Narrow Viewport IA">
    <figcaption>
      Narrow Viewport IA
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img src="images/wideviewport.png" alt="Wide Viewport IA">
    <figcaption>
      Wide Viewport IA
     </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

Оба варианта легко преобразуются в разделы страницы, с которыми мы будем работать на протяжении всего проекта.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addstructure.html" region_tag="structure" adjust_indentation="auto" %}
</pre>

Основа сайта готова. Мы знаем, какие разделы будут на сайте, что будет в них отображаться и как расположить их относительно друг друга. Теперь можно начать создание сайта.

### TL;DR {: .hide-from-toc }

Note: О стиле мы напишем позже

Заголовок и поле для подписки на уведомления — важные элементы страницы. Пользователь должен сразу их видеть.

### Добавление контента на страницу

В заголовок поместим простой текст, описывающий суть курса:

Также нам необходимо создать форму. В ней будут следующие поля: имя пользователя, номер телефона и время суток, в которое удобнее всего звонить.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addheadline.html" region_tag="headline" adjust_indentation="auto" %}
</pre>

Все формы должны содержать плейсхолдеры и описания. Таким образом, пользователь легко будет ориентироваться в форме и понимать, что необходимо вписать в то или иное поле. Корректно составленное описание формы позволит программам для людей с ограниченными возможностями (например, экранному диктору) считать информацию и воспроизвести ее пользователю. Атрибут name помогает браузеру автоматически заполнять поля.

Чтобы пользователю было удобно вводить информацию на мобильном устройстве, опишем семантические типы элемента ввода. Например, когда необходимо ввести номер телефона, клавиатура переключится в режим набора номера.

{# include shared/related_guides.liquid inline=true list=page.related-guides.create-amazing-forms #}

Этот раздел имеет более сложную структуру: в нем будет располагаться список функций наших продуктов и плейсхолдер для видеоролика о работе с ними.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addform.html" region_tag="form" adjust_indentation="auto" %}
</pre>

Видео часто используют, чтобы рассказать о продукте или идее в более интерактивном стиле или показать его в действии.

#### Создание раздела с видеоматериалами и информацией

Чтобы без труда встроить видеоролик на свой сайт, воспользуйтесь этими советами:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section1" adjust_indentation="auto" %}
</pre>

{# include shared/related_guides.liquid inline=true list=page.related-guides.video #}

Сайты с изображениями выглядят интереснее, чем сайты, содержащие только текст. Существует два типа изображений:

Раздел `Изображения` на нашем сайте будет содержать подборку контент-изображений.

* чтобы пользователю было удобно проигрывать видео, добавьте атрибут controls;
* чтобы сразу было понятно, о чем видео, включите в него изображение для заставки;
* Add multiple `<source>` elements based on supported video formats.
* для случаев, когда браузер не может проигрывать видео на странице, добавьте фолбэк-текст со ссылкой на скачивание видеоролика.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addvideo.html" region_tag="video"   adjust_indentation="auto" %}
</pre>

Такие картинки крайне важны для передачи информации. Например, невозможно представить себе газеты и журналы без фотографий и рисунков. В этом курсе мы используем фотографии преподавателей Криса Уилсона, Питера Лубберса и Шона Беннета.

#### Создание раздела `Изображения`

Эти изображения расположены так, чтобы заполнить экран по широкой стороне. Такую верстку удобнее всего использовать для устройств с узким экраном. На мониторах стационарных компьютеров она выглядит хуже. Подробнее о том, как сделать верстку удобной для разных экранов, мы поговорим в разделе `Отзвычивый дизайн`.

* Контент-изображения. Они являются частью контента на сайте и пополняют информативную функцию.
* Стилистические изображения. Их обычно помещают на фон страницы. Они делают внешний вид сайта более приятным. Подробнее об этом читайте в [следующей статье](#).

{# include shared/related_guides.liquid inline=true list=page.related-guides.images #}

У слабовидящих и незрячих людей нет возможности читать информацию на экране, поэтому они используют экранный диктор, который начитывает контент. Важно, чтобы все контент-изображения на странице имели содержательный тег alt. С его помощью экранный диктор сможет `прочесть` описание изображения.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addimages.html" region_tag="images"   adjust_indentation="auto" %}
</pre>

Убедитесь, что текст максимально емко и кратко описывает картинку. В нашем примере для атрибута используется формат `Имя: должность`. Таким образом, пользователь поймет, что на этой странице рассказывается про авторов курса и их профессии.

Последний раздел содержит таблицу со статистическими данными о продукте.

Используйте таблицы только для предоставления табличных данных (например, для информационных матриц).

Чтобы внизу страницы всегда отображались ссылки на такие разделы, как `Условия использования`, в большинство сайтов необходимо встраивать `подвал` (footer).

#### Добавление раздела с таблицей

На нашем сайте такими разделами будут `Контакты`, `Условия использования` и `Социальные сети`.

Мы успешно создали макет сайта и обозначили его основные элементы. Также мы убедились, что контент сайта располагается в нужных разделах.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section3" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/addtable.html){: target="_blank" .external }

#### Добавление `подвала страницы`

Наверное, вы уже заметили, что сейчас страница выглядит некрасиво. Мы намеренно оставили ее в таком виде. Контент — это неотъемлемая часть любого сайта. Поэтому сперва мы создаем условия для удобного отображения информации, а затем приступаем к оформлению внешнего вида сайта. Теперь у нашего сайта есть надежная структура. А о стиле мы поговорим в следующем разделе.

Пользователи заходят в Интернет с различных устройств, начиная от смартфонов и заканчивая стационарными компьютерами. У каждого устройства есть свои преимущества и недостатки. Ваша задача как веб-разработчика — сделать сайт удобным для любых устройств.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="footer" adjust_indentation="auto" %}
</pre>

Наша задача — сделать сайт, который будет открываться на любых устройствах. В [предыдущей главе]# мы разработали информационную архитектуру сайта и создали его базовую структуру. В этой главе мы продолжим работу со структурой и контентом. Мы также объясним, как улучшить внешний вид страницы и сделать его доступным для любых экранов.

### Заключение

Мы следуем принципу Mobile first и сначала разрабатываем версию для устройств с узким экраном. Затем переходим к созданию версии для более широких экранов: постепенно увеличиваем ширину окна конструирования и оцениваем, насколько удачно выглядит страница при новых параметрах.

<div class="attempt-left">
  <figure>
    <img src="images/content.png" alt="Content">
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-without-styles.html">Content and structure</a>
    </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img  src="images/narrowsite.png" alt="Designed site" style="max-width: 100%;">
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-with-styles.html">Final site</a>
    </figcaption>
  </figure>
</div>

Ранее мы создали несколько вариантов высокоуровневых проектов. В каждом из них контент располагался по-разному. Теперь необходимо адаптировать страницу к этим проектам. Для этого определим, где будут располагаться точки разрыва (определенные размеры экрана, при которых меняется стиль и верстка).

## Сделайте дизайн сайта отзывчивым

Даже создавая самые простые страницы, вы **всегда должны** включать в код метатег viewport. Он является самым важным компонентом в отзывчивом веб-дизайне. Без него ваш сайт будет плохо выглядеть на мобильных устройствах.

Тег viewport сообщает браузеру, что масштаб страницы необходимо изменить, чтобы она полностью помещалась на экране. Вы можете задать несколько параметров для этого тега. Мы рекомендуем следующие параметры по умолчанию:

Помещать тег viewport нужно только один раз в раздел head.

{# include shared/related_guides.liquid inline=true list=page.related-guides.responsive #}

### TL;DR {: .hide-from-toc }

* Всегда используйте метаэлемент viewport.
* Всегда начинайте с разработки версии для узких экранов.
* Внедряйте точки разрыва, если контент на странице начал выглядеть неаккуратно.
* Разработайте высокоуровневый дизайн вашей разметки для различных точек разрыва.

### Добавление тега viewport

В нашей компании уже разработаны инструкции по работе со шрифтами и оформлением; они описаны в руководстве по стилю.

В таком руководстве понятно и доступно объясняются правила и нюансы работы с внешним видом страницы.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/viewport.html" region_tag="viewport" adjust_indentation="auto" %}
</pre>

В предыдущей главе мы добавляли на страницу контент-изображения. Их основная функция — визуально дополнять контент на странице. В отличие от них, стилистические изображения — необязательная часть контента. Однако они оживляют страницу в целом, а также могут привлекать внимание пользователя к нужной ее части.

Хорошим примером использования таких изображений является картинка в верхней половине страницы. Ее назначение — побудить пользователя почитать о продукте подробнее.

### Применение простых стилей

Our product and company already has a very specific branding and font guide-lines supplied in a style guide.

#### Руководство по стилю

Встроить такие изображения на страницу очень просто. В нашем проекте мы поместим картинку на фон в верхней части экрана с помощью несложного кода CSS.

#### Добавление стилистических изображений

<div class="styles" style="font-family: monospace;">
  <div style="background-color: #39b1a4">#39b1a4</div>
  <div style="background-color: white">#ffffff</div>
  <div style="background-color: #f5f5f5">#f5f5f5</div>

  <div style="background-color: #e9e9e9">#e9e9e9</div>
  <div style="background-color: #dc4d38">#dc4d38</div>
</div>

#### Плавающая форма

<img  src="images/narrowsite.png" alt="Designed site"  class="attempt-right" />

На ширине 600 пикселей страница уже выглядит неаккуратно. Дело в том, что длина строки при таких настройках превышает удобные для восприятия 10 слов. Попробуем это исправить.

Чтобы элементы на странице автоматически перераспределялись, нам необходимо внедрить здесь точку разрыва. Сделать это можно с помощью технологии [Media Queries](/web/fundamentals/design-and-ux/responsive/#use-css-media-queries-for-responsiveness).

На широких экранах больше места, поэтому контент на странице можно разместить в разных комбинациях.

<div style="clear:both;"></div>

    #headline {
      padding: 0.8em;
      color: white;
      font-family: Roboto, sans-serif;
      background-image: url(backgroundimage.jpg);
      background-size: cover;
    }
    

Note: нет необходимости перемещать все элементы сразу: вносите изменения постепенно.

### Использование точек разрыва

В нашем случае потребуется сделать следующее:

<video controls poster="images/firstbreakpoint.png" style="width: 100%;">
  <source src="videos/firstbreakpoint.mov" type="video/mov"></source>
  <source src="videos/firstbreakpoint.webm" type="video/webm"></source>
  <p>Ваш браузер не поддерживает воспроизведение видеороликов.
     <a href="videos/firstbreakpoint.mov">Скачать видеоролик</a>.
  </p>
</video>

{# include shared/related_guides.liquid inline=true list=page.related-guides.first-break-point #}

    @media (min-width: 600px) {
    
    }
    

Выберем два основных вида верстки: для узких и широких экранов. Это сильно упростит нам задачу.

Для обоих видов удобнее будет располагать на странице текст без полей. Следовательно, нам необходимо задать максимальную ширину экрана, чтобы в очень широкой области просмотра текст не выстраивался в одну длинную строку. Допустим, мы выбрали максимальную ширину в 800 пикселей.

Зададим ее, не забыв расположить элементы по центру. Нам необходимо создать контейнер вокруг каждого большого раздела и применить стиль margin: auto. В результате на широких экранах контент будет располагаться по центру с максимальной шириной 800 пикселей.

* задать максимальную ширину страницы;
* изменить отступ и уменьшить размер текста;
* определить стиль заголовка как in-line и float и подвинуть форму;
* определить стиль видеоэлемента как float;
* уменьшить размер изображений и расположить их в более удобной сетке.

### Задание максимальной ширины

Контейнер будет выглядеть как простой тег div:

На узких экранах мобильных устройств места очень мало, поэтому необходимо сильно сокращать размер и вес типографики, чтобы гармонично уместить его в мобильной версии.

У устройств с большим экраном есть другая особенность: пользователь обычно находится дальше от экрана, и мелкий текст в этом случае воспринимается труднее. Чтобы это исправить, мы можем увеличить размер и вес типографики, а также изменить отступы, чтобы выделить те или иные области.

На нашей странице мы увеличим отступ до 5% от общей ширины. Также мы увеличим размер текста заголовка в каждом разделе.

    <div class="container">
    ...
    </div>
    

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="containerhtml"   adjust_indentation="auto" %}
</pre>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="container"   adjust_indentation="auto" %}
</pre>

Итак, на узких экранах содержимое сайта отображается линейно. Разделы располагаются в вертикальном порядке.

### Изменение отступа и уменьшение размера текста

На широком экране есть дополнительное место для оптимального размещения контента. Это значит, что, согласно информационной архитектуре, которую мы создали, мы можем:

В узкой области просмотра недостаточно места по горизонтали, чтобы в нем можно было удобно разместить все элементы.

Чтобы эффективно использовать пространство на широком экране, необходимо разбить на части контент в заголовке и поместить форму рядом со списком.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding"   adjust_indentation="auto" %}
</pre>

На узком экране видеоролики занимают всю ширину и располагаются после списка основных аспектов курса. Если использовать эти же настройки для широкого экрана, видео слишком сильно растянется и будет отображаться некорректно.

### Адаптирование элементов для устройств с широким экраном

Поэтому необходимо поместить его на одном уровне со списком, а не после него.

Как и в случае с видео, изображения на узких дисплеях также располагаются одно под другим и их ширина соответствует ширине экрана. Если открыть страницу с такими настройками на широком экране, картинки растянутся и будут смотреться плохо.

* перемещать форму для заполнения относительно заголовка;
* расположить видеоролик справа от списка основных аспектов курса;
* разместить изображения в разных областях экрана;
* растянуть таблицу.

#### Плавающий видеоролик

Мы предлагаем ограничить размер изображений 30-ю процентами от общей ширины контейнера и расположить картинки горизонтально. Также мы добавим тень и небольшие отступы по краям, чтобы сделать изображения более привлекательными.

To make more effective use of the horizontal screen space, we need to break out of the linear flow of the header and move the form and list to be next to each other.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="formfloat"   adjust_indentation="auto" %}
</pre>

При работе с изображениями помимо размера области просмотра необходимо учитывать плотность данных.

<video controls poster="images/floatingform.png" style="width: 100%;">
  <source src="videos/floatingform.mov" type="video/mov"></source>
  <source src="videos/floatingform.webm" type="video/webm"></source>
  <p>Ваш браузер не поддерживает проигрывание этого видеоролика.
     <a href="videos/floatingform.mov">Скачать видеоролик</a>
  </p>
</video>

#### Размещение изображений друг за другом

Изначально стандарты, применяемые в Интернете, подразумевали, что контент будет просматриваться на устройствах с разрешением в 96dpi. С появлением мобильных устройств сильно возросла плотность пикселей на экранах. Особенно заметно это на дисплеях класса Retina. Изображения, плотность пикселей у которых составляет 96dpi, очень плохо смотрятся на таких экранах.

У нас появилось решение этой проблемы, которое пока не слишком распространено. Некоторые браузеры позволяют добавить два варианта изображения: обычное и высококачественное. Последние будут показываться только в том случае, если экран имеет высокое разрешение.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding"   adjust_indentation="auto" %}
</pre>

{# include shared/related_guides.liquid inline=true list=page.related-guides.images #}

#### Адаптирование параметров изображения в соответствии с разрешением экрана

<img src="images/imageswide.png" class="attempt-right" />

Если ваша таблица состоит из трех столбцов и более, мы рекомендуем разбить в ней информацию так, чтобы получилось два столбца.

В код сайта мы вынуждены встроить еще одну точку разрыва специально для работы с таблицей. Поскольку мы начинаем создание сайта с версии для мобильных устройств, в CSS стилях отделите секцию для узких экранов от секции для широких экранов. Таким образом, получается отдельный раздел.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="floatvideo"   adjust_indentation="auto" %}
</pre>

**ПОЗДРАВЛЯЕМ!** Вы только что создали свою первую несложную страницу с описанием продукта. С этой страницей удобно работать на любых устройствах.

#### Таблицы

Следуйте рекомендациям ниже, и работа над отзывчивым дизайном сайта будет легкой и приятной:

The web was built for 96dpi screens. With the introduction of mobile devices, we have seen a huge increase in the pixel density of screens not to mention Retina class displays on laptops. As such, images that are encoded to 96dpi often look terrible on a hi-dpi device.

We have a solution that is not widely adopted yet. For browsers that support it, you can display a high density image on a high density display.

    <img src="photo.png" srcset="photo@2x.png 2x">
    

#### Tables

Tables are very hard to get right on devices that have a narrow viewport and need special consideration.

We recommend on a narrow viewport that you transform your table by changing each row into a block of key-value pairs (where the key is what was previously the column header, and the value is still the cell value). Fortunately, this isn't too difficult. First, annotate each `td` element with the corresponding heading as a data attribute. (This won't have any visible effect until we add some more CSS.)

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/updatingtablehtml.html" region_tag="table-tbody" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/updatingtablehtml.html){: target="_blank" .external }

Now we just need to add the CSS to hide the original `thead` and instead show the `data-th` labels using a `:before` pseudoelement. This will result in the multi-device experience seen in the following video.

<video controls poster="images/responsivetable.png" style="width: 100%;">
  <source src="videos/responsivetable.mov" type="video/mov"></source>
  <source src="videos/responsivetable.webm" type="video/webm"></source>
  <p>Ваш браузер не поддерживает воспроизведение видеоролика.
     <a href="videos/responsivetable.mov">Скачать видеоролик</a>.
  </p>
</video>

In our site, we had to create an extra breakpoint just for the table content. When you build for a mobile device first, it is harder to undo applied styles, so we must section off the narrow viewport table CSS from the wide viewport css. This gives us a clear and consistent break.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/content-with-styles.html" region_tag="table-css"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-with-styles.html){: target="_blank" .external }

## Wrapping up

Success: By the time you read this, you will have created your first simple product landing page that works across a large range of devices, form-factors, and screen sizes.

If you follow these guidelines, you will be off to a good start:

1. Создайте базовую информационную архитектуру и определите, какой контент будет на сайте перед тем, как начинать писать код.
2. Всегда определяйте ширину области просмотра.
3. Всегда начинайте разработку с версии для мобильных устройств.
4. После этого увеличивайте ширину окна до тех пор, пока контент не начнет выглядеть неаккуратно. В этом месте необходимо будет встроить точку разрыва.
5. Повторяйте последнее действие до тех пор, пока вы не достигнете максимально возможной ширины экрана.