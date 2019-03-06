project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: На мобильных устройствах заполнять формы непросто. Лучшими являются те формы, в которых как можно меньше полей

{# wf_updated_on: 2018-08-05 #} {# wf_published_on: 2014-04-30 #}

# Создание потрясающих форм {: .page-title }

На мобильных устройствах заполнять формы непросто. Лучшими являются те формы, в которых как можно меньше полей. Хорошие формы предусматривают наличие семантического ввода. Клавиши должны меняться в соответствии с тем, какие данные вводит пользователь, например, при выборе даты на календаре. Информируйте об этом своих пользователей. Средства проверки должны сообщать пользователям, что именно им нужно сделать до того, как форма будет отправлена

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="iYYHRwLqrKM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Общие сведения об этих указаниях по созданию потрясающих форм см. в приведенном далее видеоролике.

## Создание эффективных форм

Для создания эффективных форм следует избегать повторяющихся действий, запрашивать только необходимую информацию и направлять пользователей, показывая им, как далеко они уже зашли в заполнении форм, состоящих из нескольких частей

### TL;DR {: .hide-from-toc }

- Заранее заполняйте поля имеющимися данными и обязательно используйте автозаполнение.
- Используйте четко обозначенные индикаторы хода выполнения, чтобы пользователям было проще ориентироваться в формах, состоящих из нескольких частей.
- Предоставляйте наглядный календарь, чтобы пользователям не приходилось переходить с сайта в приложение ''Календарь'' на своем смартфоне.

### Сводите к минимуму повторяющиеся действия и одинаковые поля

<figure class="attempt-right">
  <img src="imgs/forms-multipart-good.png" srcset="imgs/forms-multipart-good.png 1x, imgs/forms-multipart-good-2x.png 2x" alt="Отображение хода выполнения в формах, состоящих из нескольких частей">
  <figcaption>
    На веб-сайте Progressive.com пользователей сначала просят ввести свой почтовый индекс, который затем автоматически переносится в следующую часть формы.
  </figcaption>
</figure>

В формах не должно быть повторяющихся действий, полей должно быть столько, сколько действительно нужно, а кроме того необходимо использовать [автозаполнение](/web/fundamentals/input/form/#use_metadata_to_enable_auto-complete), чтобы пользователям не составляло труда заполнять формы подставляемыми данными.

Ищите возможность заранее вставлять данные, которые вам уже известны или можно предвидеть, чтобы пользователям не пришлось указывать их еще раз. Например, в поля адреса для доставки автоматически вносите последний адрес доставки, указанный этим пользователем.

<div style="clear:both;"></div>

### Показывайте пользователям, как далеко они уже зашли

<figure class="attempt-right">
  <img src="imgs/forms-multipart-good.png" srcset="imgs/forms-multipart-good.png 1x, imgs/forms-multipart-good-2x.png 2x" alt="Отображение хода выполнения в формах, состоящих из нескольких частей">
  <figcaption>
    Используйте четко обозначенные индикаторы выполнения, чтобы пользователям было удобнее заполнять формы, состоящие из нескольких частей.
  </figcaption>
</figure>

Индикаторы хода выполнения и меню должны точно показывать общий ход заполнения форм и выполнения процессов, состоящих из нескольких частей.

Если в самом начале разместить слишком сложную форму, весьма вероятно, что пользователи уйдут с вашего сайта, не пройдя весь процесс.

<div style="clear:both;"></div>

### Предоставляйте наглядные календари при выборе дат

<figure class="attempt-right">
  <img src="imgs/forms-calendar-good.png" srcset="imgs/forms-calendar-good.png 1x, imgs/forms-calendar-good-2x.png 2x" alt="Веб-сайт отеля с удобным календарем">
  <figcaption>
    Веб-сайт отеля с удобным виджетом календаря для выбора дат.
  </figcaption>
</figure>

Зачастую пользователям нужно больше контекста при назначении встреч и дат поездок. Чтобы упростить им жизнь и не заставлять их уходить с вашего сайта, чтобы посмотреть в свой календарь, предоставьте им наглядный календарь для выбора дат начала и окончания.

<div style="clear:both;"></div>

## Выбор лучшего типа ввода

Оптимизируйте ввод информации путем выбора правильного типа ввода. Пользователям нравятся веб-сайты, которые автоматически выводят на экран цифровые клавиатуры для ввода номеров телефонов или автоматически выполняют переход к следующему полю после заполнения предыдущего. При разработке форм старайтесь, чтобы пользователю не нужно было лишний раз делать нажатия

### TL;DR {: .hide-from-toc }

- Выбирайте наиболее подходящий тип ввода для своих данных.
- По мере ввода данных выдавайте подсказки с помощью элемента `datalist`.

### TL;DR {: .hide-from-toc }

В HTML5 появилось несколько новых типов ввода. Эти новые типы ввода выдают подсказки браузеру о том, клавиатуру какого вида следует отобразить в качестве экранной клавиатуры. Пользователям проще вводить нужную информацию без необходимости переключать свою клавиатуру, когда они видят только те клавиши, которые нужны для ввода данного типа.

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Тип <code>ввода</code></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Input type">
        <code>url</code><br> Для ввода URL-адреса. Ввод должен начинаться с соответствующей схемы URI,
        например<code>http://</code>, <code>ftp://</code> или <code>mailto:</code>.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/url-ios.png" srcset="imgs/url-ios.png 1x, imgs/url-ios-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>tel</code><br>Для ввода номеров телефонов. Этот тип <b>не</b>
        принуждает использовать определенный синтаксис для проверки, поэтому, если требуется обеспечить
        конкретный формат, можно использовать шаблон.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/tel-android.png" srcset="imgs/tel-android.png 1x, imgs/tel-android-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>email</code><br>Для ввода адресов электронной почты. Выдает подсказки о том, что
        по умолчанию на клавиатуре должна быть клавиша с символом @. Можно добавить
        атрибут multiple, если должно быть указано несколько адресов электронной почты.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/email-android.png" srcset="imgs/email-android.png 1x, imgs/email-android-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>search</code><br>Поле для ввода текста в стиле,
        согласованном со стилем поля поиска, реализованного в данной платформе.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/plain-ios.png" srcset="imgs/plain-ios.png 1x, imgs/plain-ios-2x.png 2x" class="keybimg">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>number</code><br>Для ввода цифр. Это может быть любое рациональное
        целое число или число с плавающей запятой.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/number-android.png" srcset="imgs/number-android.png 1x, imgs/number-android-2x.png 2x" class="keybimg">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>range</code><br>Для ввода цифр, однако, в отличие от типа числового
        ввода, значение здесь менее важно. Отображается на экране в виде
        ползунка.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/range-ios.png">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>datetime-local</code><br>Для ввода значений даты и времени
        в местном часовом поясе.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/datetime-local-ios.png" srcset="imgs/datetime-local-ios.png 1x, imgs/datetime-local-ios-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>date</code><br>Для ввода даты (только), без часового пояса.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/date-android.png" srcset="imgs/date-android.png 1x, imgs/date-android-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>time</code><br>Для ввода времени (только), без часового пояса.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/time-ios.png" srcset="imgs/time-ios.png 1x, imgs/time-ios-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>week</code><br>Для ввода недели (только), без часового пояса.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/week-android.png" srcset="imgs/week-android.png 1x, imgs/week-android-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>month</code><br>Для ввода месяца (только), без часового пояса.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/month-ios.png" srcset="imgs/month-ios.png 1x, imgs/month-ios-2x.png 2x">
      </td>
    </tr>
    <tr>
      <td data-th="Input type">
        <code>color</code><br>Для выбора цвета.
      </td>
      <td data-th="Typical keyboard">
        <img src="imgs/color-android.png" srcset="imgs/color-android.png 1x, imgs/color-android-2x.png 2x">
      </td>
    </tr>
  </tbody>
</table>

Элемент `datalist` не является типом ввода. Это список предлагаемых значений ввода, который связывается с полем формы. Он позволяет браузеру предлагать варианты автоматического заполнения по мере того, как пользователь вводит значения. В отличие от элементов select, когда пользователям приходится просматривать длинные списки, чтобы найти требуемое значение, при этом они могут выбрать только значение из этого списка, элемент `datalist` выдает подсказки по мере того, как пользователь выполняет ввод.

### TL;DR {: .hide-from-toc }

Note: Значения `datalist` выдаются в качестве подсказки, пользователи при этом не ограничены предоставленными подсказками.

<pre class="prettyprint">
{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/input/forms/_code/order.html" region_tag="datalist" %}
</pre>

На мобильных устройствах заполнять формы непросто. Лучшими являются те формы, в которых как можно меньше полей. Хорошие формы предусматривают наличие семантического ввода. Клавиши должны меняться в соответствии с тем, какие данные вводит пользователь, например, при выборе даты на календаре. Информируйте об этом своих пользователей. Средства проверки должны сообщать пользователям, что именно им нужно сделать до того, как форма будет отправлена

Элемент `label` сообщает пользователю, какую информацию необходимо ввести в элемент формы. Каждый элемент `label` связывается с элементом поля ввода путем его размещения внутри элемента `label` либо с помощью атрибута "`for`" . Применение обозначений к элементам форм также позволяет улучшить целевой размер зоны касания: для перевода фокуса на элемент ввода пользователь может коснуться как обозначения, так и поля ввода.

## Обозначайте пола полЯ надлежащим образом

Обозначения и поля ввода должны быть достаточно большими, чтобы их было легко нажимать. При книжной ориентации обозначения полей следует размещать их над элементами ввода, а при альбомной – сбоку от них. И обозначения полей, и соответствующие поля ввода должны быть видны на экране одновременно. Будьте осторожны с нестандартными обработчиками прокрутки, которые могут прокрутить элемент ввода вверх страницы, скрыв обозначение, либо обозначения, расположенные ниже элементов ввода, могут оказаться под виртуальной клавиатурой.

### TL;DR {: .hide-from-toc }

- Всегда используйте элементы `label` для полей форм, причем эти обозначения должны быть видны, когда фокус переводится на данное поле.
- Используйте элементы `placeholder`, с помощью которых подсказывайте пользователям, что именно они должны ввести.
- Чтобы помочь браузеру в автоматическом заполнении форм, используйте стандартные элементы `name` для полей ввода и указывайте атрибут `autocomplete`.

### The importance of labels

Атрибут placeholder подсказывает пользователю, что именно следует указать в поле ввода, обычно отображая значение в виде текста светлого тона, пока пользователь не начнет вводить информацию в элемент.

<pre class="prettyprint">
{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/input/forms/_code/order.html" region_tag="labels" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/input/forms/order.html){: target="_blank" .external }

### Label sizing and placement

Note: Заполнители исчезают, как только пользователь начинает ввод информации в элемент, поэтому они не являются заменой обозначений. Их следует использовать как вспомогательное средство, чтобы помочь пользователю выбрать требуемый формат или контент.

### Use placeholders

Пользователям нравится, когда веб-сайты экономят их время, автоматически заполняя такие стандартные поля, как имена, адреса электронной почты и другие часто используемые поля. Кроме того, это позволяет снизить вероятность ошибки при вводе – особенно на виртуальных клавиатурах и небольших устройствах.

<input type="text" placeholder="MM-YYYY" />

    <input type="text" placeholder="MM-YYYY" ...>
    

Например, чтобы подсказать браузеру, что он может автоматически ввести в форму имя пользователя, его адрес электронной почты и номер телефона, следует использовать:

### Use metadata to enable auto-complete

Значения атрибута `autocomplete` являются частью текущего [стандарта WHATWG HTML](https://html.spec.whatwg.org/multipage/forms.html#autofill). Далее приведены наиболее часто используемые атрибуты `autocomplete`.

Атрибуты `autocomplete` можно сопровождать именем раздела, например **`shipping`**`given-name` или **`billing`**`street-address`. Браузер будет заполнять разные разделы по отдельности, а не как непрерывную форму.

Note: Используйте либо только `street-address`, либо и `address-line1`, и `address-line2`. `address-level1` и `address-level2` нужны, только если эти поля требуются для вашего формата адреса.

В некоторые формы, например, на главной странице Google, где единственное, что требуется от пользователя, – это заполнить одно поле, можно добавить атрибут `autofocus` . Когда этот атрибут задан, настольные браузеры сразу же переводят фокус на это поле ввода, после чего пользователи могут немедленно начать использовать форму. Мобильные браузеры игнорируют атрибут `autofocus`, чтобы не вызвать произвольного отображения клавиатуры .

<pre class="prettyprint">
{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/input/forms/_code/order.html" region_tag="autocomplete" %}
</pre>

Атрибут autofocus следует использовать с осторожностью, поскольку он убирает фокус с клавиатуры, что потенциально может не дать использовать для навигации клавишу возврата на один символ.

### Recommended input `name` and `autocomplete` attribute values

Проверка данных в реальном времени помогает не только обеспечить их точность, но также улучшить восприятие пользователей. Современные браузеры имеют несколько встроенных средств, позволяющих обеспечить проверку данных в реальном времени. Кроме того, они могут не позволить пользователю отправить форму с некорректными сведениями. Необходимо использовать визуальные подсказки, которые укажут, правильно ли заполнена форма

Атрибут `pattern` задает [регулярное выражение](http://en.wikipedia.org/wiki/Regular_expression), используемое для проверки поля ввода. Например, чтобы проверить почтовый индекс США (5 цифр, за которыми иногда следует дефис и еще четыре цифры), атрибут`pattern` необходимо задать следующим образом:

<table>
  <thead>
    <tr>
      <th data-th="Content type">Тип контента</th>
      <th data-th="name attribute"><code>name</code> attribute</th>
      <th data-th="autocomplete attribute"><code>autocomplete</code> attribute</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Content type">Имя</td>
      <td data-th="name attribute">
        <code>name</code>
        <code>fname</code>
        <code>mname</code>
        <code>lname</code>
      </td>
      <td data-th="autocomplete attribute">
        <ul>
          <li><code>name</code> (полное имя)</li>
          <li><code>given-name</code> (имя)</li>
          <li><code>additional-name</code> (отчество)</li>
          <li><code>family-name</code> (фамилия)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td data-th="Content type">Адрес эл. почты</td>
      <td data-th="name attribute"><code>email</code></td>
      <td data-th="autocomplete attribute"><code>email</code></td>
    </tr>
    <tr>
      <td data-th="Content type">Адрес</td>
      <td data-th="name attribute">
        <code>address</code>
        <code>city</code>
        <code>region</code>
        <code>province</code>
        <code>state</code>
        <code>zip</code>
        <code>zip2</code>
        <code>postal</code>
        <code>country</code>
      </td>
      <td data-th="autocomplete attribute">
        <ul>
          <li>Для ввода одного адреса:
            <ul>
              <li><code>street-address</code></li>
            </ul>
          </li>
          <li>Для ввода двух адресов:
            <ul>
              <li><code>address-line1</code></li>
              <li><code>address-line2</code></li>
            </ul>
          </li>
          <li><code>address-level1</code> (область или республика)</li>
          <li><code>address-level2</code> (город)</li>
          <li><code>postal-code</code> (почтовый индекс)</li>
          <li><code>country</code></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td data-th="Content type">Телефон</td>
      <td data-th="name attribute">
        <code>phone</code>
        <code>mobile</code>
        <code>country-code</code>
        <code>area-code</code>
        <code>exchange</code>
        <code>suffix</code>
        <code>ext</code>
      </td>
      <td data-th="autocomplete attribute"><code>tel</code></td>
    </tr>
    <tr>
      <td data-th="Content type">Кредитная карта</td>
      <td data-th="name attribute">
        <code>ccname</code>
        <code>cardnumber</code>
        <code>cvc</code>
        <code>ccmonth</code>
        <code>ccyear</code>
        <code>exp-date</code>
        <code>card-type</code>
      </td>
      <td data-th="autocomplete attribute">
        <ul>
          <li><code>cc-name</code></li>
          <li><code>cc-number</code></li>
          <li><code>cc-csc</code></li>
          <li><code>cc-exp-month</code></li>
          <li><code>cc-exp-year</code></li>
          <li><code>cc-exp</code></li>
          <li><code>cc-type</code></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td data-th="Content type">Usernames</td>
      <td data-th="name attribute">
        <code>username</code>
      </td>
      <td data-th="autocomplete attribute">
        <ul>
          <li><code>username</code></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td data-th="Content type">Passwords</td>
      <td data-th="name attribute">
        <code>password</code>
      </td>
      <td data-th="autocomplete attribute">
        <ul>
          <li><code>current-password</code> (for sign-in forms)</li>
          <li><code>new-password</code> (for sign-up and password-change forms)</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Если задан атрибут `required`, то, чтобы получить возможность отправить форму, в поле, снабженном этим атрибутом, должно быть указано значение. Например, чтобы сделать поле почтового индекса обязательным для заполнения, нужно просто добавить атрибут required:

### The `autofocus` attribute

Для ввода численной информации, такой как номер, диапазон, дата или время, можно указать минимальное и максимальное значения, а также шаг их изменения при регулировке с помощью ползунка или наборного счетчика. Например, для поля ввода размера обуви задается минимальный размер 1 и максимальный размер 13 с шагом изменения 0,5

С помощью атрибута `maxlength` можно задавать максимальную длину вводимого значения или текстового поля. Он полезен, когда требуется ограничить длину информации, которую может указать пользователь. Например, если длину имени файла требуется ограничить 12 символами, это можно сделать следующим образом.

    <input type="text" autofocus ...>
    

## Обеспечение проверки в реальном времени

С помощью атрибута `minlength` можно задавать минимальную длину вводимого значения или текстового поля. Он полезен, когда требуется указать минимальную длину информации, которую должен предоставить пользователь. Например, если требуется указать, что имя файла должно состоять минимум из 8 символов, это можно сделать следующим образом.

В некоторых случаях можно позволить пользователю отправить форму, даже если она содержит ошибки. Для этого добавьте в элемент формы или отдельные поля ввода атрибут `novalidate`. В этом случае все псевдоклассы и API-интерфейсы JavaScript все равно позволят вам проверять правильность заполнения формы.

### Field validation pitfalls

Note: Даже при наличии проверки ввода на стороне клиента всегда важно проверять данные на сервере для обеспечения их согласованности и безопасности.

Когда встроенной проверки и регулярных выражений недостаточно, можно использовать [API-интерфейс проверки ограничений](https://w3c.github.io/html/sec-forms.html#constraints) – мощное средство для выполнения нестандартной проверки. С помощью этого API-интерфейса можно, например, задать нетипичную ошибочную ситуацию, проверить корректность элемента и определить причину, по которой данный элемент является некорректным:

Если поле не проходит проверку, обозначьте его как некорректное с помощью `setCustomValidity()` и поясните, в чем заключается ошибка. Например, при заполнении регистрационной формы пользователю может быть предложено для подтверждения его адреса электронной почты ввести его дважды. Используйте событие blur после ввода данных во втором поле, чтобы проверить правильность указанных данных и выдать соответствующий ответ. Например:

### Use standard input fields

Поскольку некоторые браузеры позволяют пользователю отправлять формы, даже если в них есть некорректные данные, необходимо отслеживать событие отправки и с помощью `checkValidity()` в элементе формы определять ее корректность. Например:

### Don't use fake placeholders in input fields

Полезно снабдить каждое поле визуальной подсказкой, с тем чтобы пользователь еще перед отправкой формы видел, правильно ли он заполнил данное поле. В HTML5 также появилось несколько псевдоклассов, с помощью которых поля ввода можно оформлять в соответствии с их значениями или атрибутами.

### Don't copy the shipping address into the billing address section

Проверка выполняется мгновенно, а это означает, что, когда страница загружается, поля могут быть обозначены как некорректные, даже если пользователь еще не приступал к их заполнению. Это также означает, что, пока пользователь не закончил вводить информацию, для оформления поля может использоваться стиль, предназначенный для полей с ошибочными данными и указывающий на то, что введено неверное значение. Во избежание таких ситуаций можно с помощью CSS и JavaScript сделать так, чтобы стиль, указывающий на ошибку, использовался только после того, как пользователь перешел в другое поле.

### Ensure that autocomplete attributes are correct

Note: Пользователю сразу же следует показывать все имеющиеся в форме неполадки, а не по одной за раз.

For example, the correct attribute for the Credit Card CVC is "cc-csc". Many sites mistakenly use "cc-cvc", and because Autofill does not recognize this attribute, this field won't get autofilled.

The best practice for these attributes is to use this format: `autocomplete="<section> <fieldtype>"`, for example: `autocomplete="shipping address-line1"`. For a complete list of all the accepted values, please see the [WHATWG HTML Living Standard](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#autofill).

## Provide real-time validation

Keep your user informed. Validation tools should tell the user what they need to do before submitting the form.

Real-time data validation doesn't just help to keep your data clean, but it also helps improve the user experience. Modern browsers have several built-in tools to help provide real-time data validation and may prevent the user from submitting an invalid form. Visual cues should be used to indicate whether a form has been completed properly.

### TL;DR {: .hide-from-toc }

- Используйте встроенные в браузеры атрибуты проверки, например `pattern`, `required`, `min`, `max` и т. д.
- При необходимости более сложных вариантов проверки используйте JavaScript и API-интерфейс проверки ограничений.
- Отображайте ошибки, выявленные при проверке, в реальном времени, а если пользователь пытается отправить неправильно заполненную форму, показывайте все поля, которые ему необходимо исправить.

### Use these attributes to validate input

#### Типы ввода HTML5

The `pattern` attribute specifies a [regular expression](https://en.wikipedia.org/wiki/Regular_expression) used to validate an input field. For example, to validate a US Zip code (5 digits, sometimes followed by a dash and an additional 4 digits), we would set the `pattern` like this:

    <input type="text" pattern="^\d{5,6}(?:[-\s]\d{4})?$" ...>
    

##### Атрибут `pattern`

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Description">Почтовый адрес</td>
      <td data-th="Regular expression"><code>[a-zA-Z\d\s\-\,\#\.\+]+</code></td>
    </tr>
    <tr>
      <td data-th="Description">Почтовый индекс (США)</td>
      <td data-th="Regular expression"><code>^\d{5,6}(?:[-\s]\d{4})?$</code></td>
    </tr>
    <tr>
      <td data-th="Description">IP-адрес (IPv4)</td>
      <td data-th="Regular expression"><code>^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$</code></td>
    </tr>
    <tr>
      <td data-th="Description">IP-адрес (IPv6)</td>
      <td data-th="Regular expression"><code>^(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]).){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]).){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]))$</code></td>
    </tr>
    <tr>
      <td data-th="Description">IP-адрес (оба)</td>
      <td data-th="Regular expression"><code>^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)|(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]).){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]).){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]))$</code></td>
    </tr>
    <tr>
      <td data-th="Description">Номер кредитной карты</td>
      <td data-th="Regular expression"><code>^(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|3[47][0-9]{13}|3(?:0[0-5]|[68][0-9])[0-9]{11}|6(?:011|5[0-9]{2})[0-9]{12}|(?:2131|1800|35\d{3})\d{11})$</code></td>
    </tr>
    <tr>
      <td data-th="Description">Номер социального страхования</td>
      <td data-th="Regular expression"><code>^\d{3}-\d{2}-\d{4}$</code></td>
    </tr>
    <tr>
      <td data-th="Description">Номер телефона в Северной Америке</td>
      <td data-th="Regular expression"><code>^(?:(?:\+?1\s*(?:[.-]\s*)?)?(?:\(\s*([2-9]1[02-9]|[2-9][02-8]1|[2-9][02-8][02-9])\s*\)|([2-9]1[02-9]|[2-9][02-8]1|[2-9][02-8][02-9]))\s*(?:[.-]\s*)?)?([2-9]1[02-9]|[2-9][02-9]1|[2-9][02-9]{2})\s*(?:[.-]\s*)?([0-9]{4})(?:\s*(?:#|x\.?|ext\.?|extension)\s*(\d+))?$</code></td>
    </tr>
  </tbody>
</table>

#### Выдавайте подсказки во время ввода с помощью datalist

If the `required` attribute is present, then the field must contain a value before the form can be submitted. For example, to make the zip code required, we'd simply add the required attribute:

    <input type="text" required pattern="^\d{5,6}(?:[-\s]\d{4})?$" ...>
    

#### Важность обозначений

For numeric input types like number or range as well as date/time inputs, you can specify the minimum and maximum values, as well as how much they should each increment/decrement when adjusted by the slider or spinners. For example, a shoe size input would set a minimum size of 1 and a maximum size 13, with a step of 0.5

    <input type="number" min="1" max="13" step="0.5" ...>
    

#### Размер и местоположение обозначения

The `maxlength` attribute can be used to specify the maximum length of an input or textbox and is useful when you want to limit the length of information that the user can provide. For example, if you want to limit a filename to 12 characters, you can use the following.

    <input type="text" id="83filename" maxlength="12" ...>
    

#### Используйте заполнители

The `minlength` attribute can be used to specify the minimum length of an input or textbox and is useful when you want to specify a minimum length the user must provide. For example, if you want to specify that a file name requires at least 8 characters, you can use the following.

    <input type="text" id="83filename" minlength="8" ...>
    

#### Используйте метаданные для обеспечения автозаполнения

In some cases, you may want to allow the user to submit the form even if it contains invalid input. To do this, add the `novalidate` attribute to the form element, or individual input fields. In this case, all pseudo classes and JavaScript APIs will still allow you to check if the form validates.

    <form role="form" novalidate>
      <label for="inpEmail">Email address</label>
      <input type="email" ...>
    </form>
    

Success: Even with client-side input validation, it is always important to validate data on the server to ensure consistency and security in your data.

### Use JavaScript for more complex real-time validation

When the built-in validation plus regular expressions aren't enough, you can use the [Constraint Validation API](https://w3c.github.io/html/sec-forms.html#constraints), a powerful tool for handling custom validation. The API allows you to do things like set a custom error, check whether an element is valid, and determine the reason that an element is invalid:

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">API-интерфейс</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="API"><code>setCustomValidity()</code></td>
      <td data-th="Description">Задает нестандартное сообщение о результатах проверки, а свойству <code>customError</code> объекта <code>ValidityState</code> присваивает значение <code>true</code>.</td>
    </tr>
    <tr>
      <td data-th="API"><code>validationMessage</code></td>
      <td data-th="Description">Возвращает строку с указанием причины, по которой поле не прошло проверку.</td>
    </tr>
    <tr>
      <td data-th="API"><code>checkValidity()</code></td>
      <td data-th="Description">Возвращает значение <code>true</code>, если элемент прошел проверку по всем ограничениям. В противном случае возвращается значение <code>false</code>. Определение реакции страницы на ситуацию, когда проверка возвращает значение <code>false</code>, остается за разработчиком.</td>
    </tr>
    <tr>
      <td data-th="API"><code>reportValidity()</code></td>
      <td data-th="Description">Возвращает значение <code>true</code>, если элемент прошел проверку по всем ограничениям. В противном случае возвращается значение <code>false</code>. Когда страница возвращает значение <code>false</code>, о нарушении ограничения сообщается пользователю.</td>
    </tr>
    <tr>
      <td data-th="API"><code>validity</code></td>
      <td data-th="Description">Возвращает объект <code>ValidityState</code>, представляющий состояния корректности элемента.</td>
    </tr>
  </tbody>
</table>

### Set custom validation messages

If a field fails validation, use `setCustomValidity()` to mark the field invalid and explain why the field didn't validate. For example, a sign up form might ask the user to confirm their email address by entering it twice. Use the blur event on the second input to validate the two inputs and set the appropriate response. For example:

<pre class="prettyprint">
{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/input/forms/_code/order.html" region_tag="customvalidation" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/input/forms/order.html){: target="_blank" .external }

### Prevent form submission on invalid forms

Because not all browsers will prevent the user from submitting the form if there is invalid data, you should catch the submit event, and use the `checkValidity()` on the form element to determine if the form is valid. For example:

<pre class="prettyprint">
{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/input/forms/_code/order.html" region_tag="preventsubmission" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/input/forms/order.html){: target="_blank" .external }

### Show feedback in real-time

It's helpful to provide a visual indication on each field that indicates whether the user has completed the form properly before they've submitted the form. HTML5 also introduces a number of new pseudo-classes that can be used to style inputs based on their value or attributes.

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">Пвевдокласс</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Pseudo-class"><code>:valid</code></td>
      <td data-th="Use">В явном виде задает стиль, который будет использоваться для поля ввода, когда указанное в нем значение соответствует всем требованиям проверки.</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:invalid</code></td>
      <td data-th="Use">В явном виде задает стиль, который будет использоваться для поля ввода, когда указанное в нем значение не соответствует всем требованиям проверки.</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:required</code></td>
      <td data-th="Use">В явном виде задает стиль элемента ввода, для которого задан атрибут required.</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:optional</code></td>
      <td data-th="Use">В явном виде задает стиль элемента ввода, для которого не задан атрибут required.</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:in-range</code></td>
      <td data-th="Use">В явном виде задает стиль элемента ввода числа, значение которого находится в пределах диапазона.</td>
    </tr>
    <tr>
      <td data-th="Pseudo-class"><code>:out-of-range</code></td>
      <td data-th="Use">В явном виде задает стиль элемента ввода числа, значение которого находится вне диапазона.</td>
    </tr>
  </tbody>
</table>

Validation happens immediately which means that when the page is loaded, fields may be marked as invalid, even though the user hasn't had a chance to fill them in yet. It also means that as the user types, and it's possible they'll see the invalid style while typing. To prevent this, you can combine the CSS with JavaScript to only show invalid styling when the user has visited the field.

<pre class="prettyprint">
{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/input/forms/_code/order.html" region_tag="invalidstyle" %}
</pre>

<pre class="prettyprint">
{% includecode adjust_indentation="auto"  content_path="web/fundamentals/design-and-ux/input/forms/_code/order.html" region_tag="initinputs" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/input/forms/order.html){: target="_blank" .external }

Success: You should show the user all of the issues on the form at once, rather than showing them one at a time.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}