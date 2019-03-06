project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: JavaScript позволяет модифицировать контент веб-страниц и их стиль, а также определять, что будет происходить при выполнении пользователем определенных действий. Однако JavaScript может препятствовать созданию модели DOM и задерживать визуализацию страницы. Избежать неполадок поможет асинхронизация JavaScript и устранение ненужного кода.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2013-12-31 #}

# Оптимизация JavaScript для быстрой визуализации страницы {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

JavaScript позволяет модифицировать контент веб-страниц и их стиль, а также определять, что будет происходить при выполнении пользователем определенных действий. Однако JavaScript может препятствовать созданию модели DOM и задерживать визуализацию страницы. Избежать неполадок поможет асинхронизация JavaScript и устранение ненужного кода.

### TL;DR {: .hide-from-toc }

* JavaScript может изменять DOM и CSSOM, а также получать данные из этих моделей.
* Выполнение JavaScript невозможно без модели CSSOM.
* JavaScript препятствует созданию DOM, если не использовать асинхронизацию.

JavaScript - это динамический язык, с помощью которого можно менять почти любые характеристики веб-страниц: модифицировать контент, добавляя элементы в модель DOM или удаляя их; менять CSSOM-свойства любых элементов; отвечать на действия пользователя и т. д. Рассмотрим эти возможности, дополнив прошлый пример Hello World встроенным кодом:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/script.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/script.html){: target="_blank" .external }

* JavaScript позволяет извлечь из DOM скрытый объект span, который может не отображаться в модели визуализации, а затем заменить текст внутри объекта (с помощью команды .textContext) и вычисляемое свойство стиля (с `none` на `inline`). Готово! На странице появится текст **Hello interactive students!**.

* Кроме того, JavaScript позволяет создавать новые объекты в модели DOM, менять их стиль, а также присоединять их к модели и удалять. То есть мы могли бы создать полноценную веб-страницу, прописав все элементы и их стили в файле JavaScript, однако это намного проще осуществить с помощью HTML и CSS. Обратите внимание, что во второй части примера мы создаем блок div, добавляем в него текст, задаем стиль и присоединяем блок к основному коду.

<img src="images/device-js-small.png"  alt="page preview" />

Однако при всех своих достинствах JavaScript может увеличить время загрузки страниц, создав разнообразные препятствия при их визуализации.

Например, обратите внимание, что в приведенном выше примере встроенный код добавлен в конец страницы. Почему? Все просто: если мы поместим скрипт над тегом *span*, его невозможно будет выполнить. Функция *getElementsByTagName('span')* не сможет обнаружить в документе элементов *span* и вернет результат *null*. Из этого можно сделать важный вывод: скрипты выполняются именно в той последовательности, в которой они представлены в документе. Когда синтаксический анализатор встречает тег скрипта, он приостанавливает создание модели DOM и запускает механизм выполнения JavaScript. Создание модели продолжится только после того, как механизм завершит работу.

Получается, что скрипт не может обнаружить расположенные после него элементы, потому что они ещё не обработаны. Кроме того, **при выполнении встроенного скрипта создание DOM прекращается, что замедляет визуализацию страницы**.

При использовании скриптов также следует помнить, что они могут изменять не только свойства DOM, но и CSSOM. Именно это и происходит в нашем примере при замене значения `none` на `inline` в свойствах стиля. В результате образуется состояние гонки.

Представьте, что к тому моменту, когда нужно запустить скрипт, браузер ещё не закончил создание CSSOM. Это приведет к тому, что **браузер будет задерживать выполнение скрипта до тех пор, пока не построит CSSOM**. В это время **создание DOM будет приостановлено**, из-за чего скорость визуализации существенно снизится.

Поэтому учтите, что из-за взаимосвязей между DOM, CSSOM и кодом JavaScript выполнение скриптов может вызвать задержку в обработке и визуализации страницы.

Чтобы ускорить визуализацию, нужно понять взаимосвязь между файлами HTML, CSS и JavaScript и оптимизировать ее. Это и есть оптимизация визуализации.

* The location of the script in the document is significant.
* When the browser encounters a script tag, DOM construction pauses until the script finishes executing.
* JavaScript can query and modify the DOM and the CSSOM.
* JavaScript execution pauses until the CSSOM is ready.

Как вы помните, встретив в документе JavaScript, браузер приостанавливает создание DOM и возобновляет его только после выполнения скрипта. Другими словами, JavaScript блокирует работу синтаксического анализатора, что видно на примере со встроенным кодом. Блокировку можно предотвратить с помощью дополнительного кода.

## Блокировка анализатора и асинхронный JavaScript

Но что если поместить скрипт не в HTML-код, а в отдельный файл, и сослаться на него с помощью script-тега? Например, вот так:

What about scripts included via a script tag? Let's take our previous example and extract the code into a separate file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/split_script.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**app.js**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/app.js" region_tag="full"   adjust_indentation="auto" %}
</pre>

Но паниковать не стоит. Мы можем избежать этой ситуации и предотвратить блокировку анализатора, подав браузеру сигнал о том, что скрипт JavaScript можно выполнить позже. Браузер сможет создать DOM и начнет выполнять скрипт только тогда, когда он будет полностью загружен из кеша или с удаленного сервера.

Для этого нужно просто добавить в тег скрипта ключевое слово *async*:

По нему браузер определит, что блокировать создание DOM на время загрузки скрипта не нужно. Результат - высокая скорость визуализации!

To achieve this, we mark our script as *async*:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/split_script_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/split_script_async.html){: target="_blank" .external }

Adding the async keyword to the script tag tells the browser not to block DOM construction while it waits for the script to become available, which can significantly improve performance.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}