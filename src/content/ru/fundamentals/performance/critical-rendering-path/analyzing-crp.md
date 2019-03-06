project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: В этой статье мы рассмотрим несколько примеров из практики, которые помогут вам оптимизировать процесс визуализации, т. е. находить и устранять замедляющие ее помехи.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Анализ процесса визуализации {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

В этой статье мы рассмотрим несколько примеров из практики, которые помогут вам оптимизировать процесс визуализации, т. е. находить и устранять замедляющие ее помехи.

Цель оптимизации - минимизировать время визуализации страниц. Этого можно добиться, меняя порядок загрузки ресурсов. Помните, что чем меньше времени пользователь проводит перед пустым экраном, тем больше степень его вовлеченности. Кроме того, оптимизация позволяет увеличить число просмотров и [улучшить конверсию](http://www.google.com/think/multiscreen/success.html).

Для наглядности мы приведем несколько примеров: начнем с самого простого и будем постепенно дополнять его ресурсами, стилями и скриптами. В результате вы поймете, где могут возникнуть ошибки, и научитесь оптимизировать разные типы страниц.

Перед тем как начать этот урок, обговорим один технический момент. В прошлых главах мы не принимали в расчет время, необходимое для загрузки HTML-, CSS- и JavaScript-файлов из сети или кеша. О том, как оптимизировать это время, мы расскажем позже. а пока в основу наших примеров лягут следующие значения:

* соединение (вызов сервера и получение ответа) - 100 мс;
* ответ сервера - 100 мс для HTML-документов и 10 мс для других файлов.

## Страница Hello World

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Начнем с самой простой страницы без CSS и JavaScript, состоящей из HTML-разметки и картинки. Запустим инструменты разработчика в Chrome, откроем вкладку Network (Сеть) и проанализируем динамический список:

We'll start with basic HTML markup and a single image; no CSS or JavaScript. Let's open up our Network timeline in Chrome DevTools and inspect the resulting resource waterfall:

<img src="images/waterfall-dom.png" alt="CRP" />

Скачав HTML-документ, браузер анализирует байты, преобразует их в объекты и строит модель визуализации. Обратите внимание, что в инструментах разработчика также указана продолжительность события DOMContentLoaded (216 мс), отмеченного на графике синей вертикальной чертой. Пробел между этой чертой и темно-голубой полосой - это время, которое понадобилось браузеру для создания модели визуализации (всего пара миллисекунд в нашем случае).

На графике есть ещё одна любопытная деталь: картинка `awesome photo` не помешала событию DOMContentLoaded. Значит, браузер может создавать модель визуализации и выводить страницу на экран, не дожидаясь загрузки всех элементов. Отсюда вывод: **не все ресурсы необходимы для первоочередной визуализации**. Первоочередной процесс визуализации затрагивает только HTML-, CSS и JavaScript-файлы. Изображения не препятствуют выводу страницы, однако мы должны позаботиться о том, чтобы они также загружались достаточно быстро.<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

До тех пор пока картинки не отобразятся на экране, невозможна полная загрузка страницы и использование события onload. Помните, что страница загружается полностью только после скачивания и обработки **всех ресурсов**. На скриншоте выше загрузка (load) завершилась через 335 мс - это событие отмечено красной вертикальной чертой.

Несмотря на простоту нашего примера, браузеру пришлось потрудиться. Однако обычно страницы содержат не только только HTML-разметку, но и таблицы стилей (CSS), и интерактивные функции (скрипты). Попробуем добавить их в код Hello World:

That said, the `load` event (also known as `onload`), is blocked on the image: DevTools reports the `onload` event at 335ms. Recall that the `onload` event marks the point at which **all resources** that the page requires have been downloaded and processed; at this point, the loading spinner can stop spinning in the browser (the red vertical line in the waterfall).

## Страница с CSS и JavaScript

Our "Hello World experience" page seems simple but a lot goes on under the hood. In practice we'll need more than just the HTML: chances are, we'll have a CSS stylesheet and one or more scripts to add some interactivity to our page. Let's add both to the mix and see what happens:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_timing.html){: target="_blank" .external }

*Before adding JavaScript and CSS:*

<img src="images/waterfall-dom.png" alt="DOM CRP" />

*With JavaScript and CSS:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

Adding external CSS and JavaScript files adds two extra requests to our waterfall, all of which the browser dispatches at about the same time. However, **note that there is now a much smaller timing difference between the `domContentLoaded` and `onload` events.**

What happened?

* В отличие от первого примера, браузеру нужно скачать и проанализировать CSS-файл, а также построить модель CSSOM. Без нее создать модель визуализации невозможно.
* Поскольку для выполнения JavaScript может понадобиться CSSOM, браузер блокирует событие DOMContentLoaded (см. синюю черту) до тех пор, пока не скачает и не проанализирует CSS-файл.

**What if we replace our external script with an inline script?** Even if the script is inlined directly into the page, the browser can't execute it until the CSSOM is constructed. In short, inlined JavaScript is also parser blocking.

Итак, количество запросов уменьшилось, но время завершения загрузки (load) не изменилось, как и временная метка события DOMContentLoaded. Почему? Во-первых, как мы уже знаем, встроенный скрипт требует создания CSSOM, что невозможно без приостановки синтаксического анализа. Во-вторых, в прошлый раз браузер скачивал файл JavaScript одновременно с CSS, поэтому устранение одного из запросов не играет роли. В результате встроенный код не решил проблему на нашей странице. Что же делать? Есть ли другой способ оптимизации? Есть, и даже не один.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Inlined JavaScript:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, and inlined JS" />

We are making one less request, but both our `onload` and `domContentLoaded` times are effectively the same. Why? Well, we know that it doesn't matter if the JavaScript is inlined or external, because as soon as the browser hits the script tag it blocks and waits until the CSSOM is constructed. Further, in our first example, the browser downloads both CSS and JavaScript in parallel and they finish downloading at about the same time. In this instance, inlining the JavaScript code doesn't help us much. But there are several strategies that can make our page render faster.

Уже лучше! Теперь браузер инициирует событие DOMContentLoaded почти сразу после завершения анализа HTML. Поскольку JavaScript не блокирует анализатор, CSSOM создается одновременно с обработкой скрипта.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

Much better! The `domContentLoaded` event fires shortly after the HTML is parsed; the browser knows not to block on JavaScript and since there are no other parser blocking scripts the CSSOM construction can also proceed in parallel.

**Промежуток между T<sub>0</sub> и T<sub>1</sub> - это время сетевой и серверной обработки.** Если HTML-документ небольшой, то для его загрузки достаточно одного соединения. Большие файлы требуют нескольких соединений из-за особенностей протокола TCP. Эту тему мы рассмотрим в одном из следующих уроков. **Итак, для визуализации нашей страницы требуется как минимум одно соединение.**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Теперь усложним задачу и добавим внешний CSS-файл:

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="Процесс визуализации: DOM и CSSOM" />

Для передачи HTML-документа требуется одно соединение, но теперь в разметке содержится ссылка на CSS-файл, который также необходим для вывода страницы на экран. Браузер снова соединяется с сервером и скачивает CSS-файл. **В результате для визуализации страницы нужно как минимум два соединения** (и больше, если мы имеем дело со сложным CSS- или HTML-файлом).

Остановимся на терминах, с помощью которых можно описать процесс визуализации:

Теперь рассмотрим характеристики второй страницы, состоящей из HTML и CSS:

## Алгоритмы визуализации

The simplest possible page consists of just the HTML markup; no CSS, no JavaScript, or other types of resources. To render this page the browser has to initiate the request, wait for the HTML document to arrive, parse it, build the DOM, and then finally render it on the screen:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Поскольку для создания модели визуализации нужны DOM и CSSOM, первоочередными ресурсами являются два файла - HTML и CSS. Сначала браузер скачивает HTML-документ, и только потом - CSS, поэтому для обработки файлов нужно как минимум два соединения. Общий размер ресурсов составляет 9 КБ.

<img src="images/analysis-dom.png" alt="Hello world CRP" />

Мы привели в HTML-документе ссылку на внешний файл app.js. Как нам уже известно, это первоочередной ресурс, который задерживает визуализацию. К тому же скрипт может ссылаться на CSSOM, поэтому для его выполнения браузеру нужно скачать файл style.css и сформировать модель. На это время визуализация также будет приостановлена.

Now, let's consider the same page but with an external CSS file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Однако на вкладке Network (Сеть) в инструментах разработчика можно увидеть следующее: когда браузер обнаруживает файлы CSS и JavaScript, он инициирует оба запроса почти одновременно. Поэтому характеристики страницы выглядят так:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

Как вы помните, мы можем избежать приостановки визуализации, сообщив браузеру, что JavaScript нужно обработать позже. Для этого мы присвоим скрипту параметр async:

Let's define the vocabulary we use to describe the critical rendering path:

* **Первоочередные ресурсы** - файлы, которые могут заблокировать процесс визуализации.
* **Продолжительность обработки** - количество соединений или общее время, необходимое для загрузки всех первоочередных ресурсов.
* **Число байтов** - количество байтов, которое нужно получить для первоочередной визуализации страницы (общий размер всех первоочередных ресурсов). Вернемся к первому алгоритму: страница Hello World содержит один первоочередной ресурс (HTML-документ), продолжительность обработки составляет 1 соединение (т. к. файл имеет небольшой размер), а число байтов совпадает с размером HTML-документа.

У асинхронного кода есть ряд преимуществ:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* первоочередные ресурсы - **2** файла;
* продолжительность обработки - **2** соединения минимум;
* число байтов - **9** КБ.

Но ещё лучшего результата можно добиться, если CSS необходим только для печати страницы:

Now let's add an extra JavaScript file into the mix.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Поскольку файл style.css используется только для печати, браузер пропустит его обработку, чтобы не задерживать визуализацию. Значит, страницу можно вывести на экран уже после того, как браузер построит модель DOM! Таким образом мы сократили число первоочередных ресурсов до одного HTML-документа, для обработки которого понадобится как минимум одно соединение.

We added `app.js`, which is both an external JavaScript asset on the page and a parser blocking (that is, critical) resource. Worse, in order to execute the JavaScript file we have to block and wait for CSSOM; recall that JavaScript can query the CSSOM and hence the browser pauses until `style.css` is downloaded and CSSOM is constructed.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

That said, in practice if we look at this page's "network waterfall," you'll see that both the CSS and JavaScript requests are initiated at about the same time; the browser gets the HTML, discovers both resources, and initiates both requests. As a result, the above page has the following critical path characteristics:

* первоочередные ресурсы - **3** файла;
* продолжительность обработки - **2** соединения минимум;
* число байтов - **11** КБ.

We now have three critical resources that add up to 11KB of critical bytes, but our critical path length is still two roundtrips because we can transfer the CSS and JavaScript in parallel. **Figuring out the characteristics of your critical rendering path means being able to identify the critical resources and also understanding how the browser will schedule their fetches.** Let's continue with our example.

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

An asynchronous script has several advantages:

* Он предотвращает приостановку визуализации, поэтому скрипт больше не считается первоочередным ресурсом.
* Поскольку браузеру не нужно сразу выполнять скрипт, он может отложить создание модели CSSOM и не блокировать событие DOMContentLoaded.
* Чем скорее браузер инициирует событие DOMContentLoaded, тем раньше начнется выполнение скрипта.

As a result, our optimized page is now back to two critical resources (HTML and CSS), with a minimum critical path length of two roundtrips, and a total of 9KB of critical bytes.

Finally, if the CSS stylesheet were only needed for print, how would that look?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

Because the style.css resource is only used for print, the browser doesn't need to block on it to render the page. Hence, as soon as DOM construction is complete, the browser has enough information to render the page. As a result, this page has only a single critical resource (the HTML document), and the minimum critical rendering path length is one roundtrip.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}