project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: DOM и CSSOM образуют модель визуализации. С ее помощью браузер формирует из всех видимых объектов макет, а затем выводит пиксели на экран. Чтобы ускорить загрузку страницы, нужно оптимизировать каждый из этих этапов.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# Создание модели визуализации и макета, вывод страницы на экран {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

DOM и CSSOM образуют модель визуализации. С ее помощью браузер формирует из всех видимых объектов макет, а затем выводит пиксели на экран. Чтобы ускорить загрузку страницы, нужно оптимизировать каждый из этих этапов.

В предыдущей главе мы узнали о том, как на основании HTML- и CSS-файлов строятся модели DOM и CSSOM. Эти независимые друг от друга модели отвечают за разные аспекты страницы: DOM описывает контент, а CSSOM - стили, которые будут к нему применены. Рассмотрим, как браузер объединяет эти модели и выводит страницу на экран.

### TL;DR {: .hide-from-toc }

* Браузер объединяет DOM и CSSOM, формируя модель визуализации.
* Модель визуализации содержит только те объекты, которые нужны для вывода страницы на экран.
* Далее формируется макет, отражающий расположение и размер каждого объекта.
* Наконец, объекты выводятся на экран.

Для начала браузер формирует модель визуализации, в которой всем видимым объектам из модели DOM присваиваются наборы стилей из модели CSSOM.

<img src="images/render-tree-construction.png" alt="Объединение DOM и CSSOM в модель визуализации" />

Для формирования модели визуализации браузер выполняет следующие действия:

1. Starting at the root of the DOM tree, traverse each visible node.
    
    * Этот этап не затрагивает элементы, которые не будут видны на странице, например теги скриптов, метатеги и т. п.
    * Он также не затрагивает объекты, помеченные как невидимые с помощью CSS. Взгляните на приведенную выше схему: объект span отсутствует в модели визуализации, потому что ему присвоен параметр `display: none`.

2. For each visible node, find the appropriate matching CSSOM rules and apply them.

3. Формирует модель из видимых объектов, их содержания и стилей.

Note: Обратите внимание, что параметры `visibility: hidden` и `display: none` различаются. При использовании первого параметра объект занимает место в макете, но скрывается на странице (в результате вы можете увидеть, например, пустой квадрат). В то же время `display: none` указывает, что объект будет удален из макета и вообще не появится на странице.

Создав модель визуализации, браузер на шаг приблизился к выводу страницы на экран! **Теперь можно приступить к формированию макета.**

Браузер уже определил, какие объекты будут видны на странице и какие стили нужно им присвоить. Пришло время создать макет, т. е. выяснить, какого размера будут объекты и как их нужно расположить в [области просмотра](/web/fundamentals/design-and-ux/responsive/#set-the-viewport).

Для этого браузер вычисляет геометрическую форму объектов, анализируя модель визуализации с самого начала. Рассмотрим простой пример:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/nested.html" region_tag="full" adjust_indentation="auto" %}
</pre>

В теле этой страницы есть два блока div. Ширина родительского блока - 50% от области просмотра, а вложенного - 50% от родительского, т. е. 25% экрана.

The body of the above page contains two nested div's: the first (parent) div sets the display size of the node to 50% of the viewport width, and the second div\---contained by the parent\---sets its width to be 50% of its parent; that is, 25% of the viewport width.

<img src="images/layout-viewport.png" alt="Calculating layout information" />

Наконец, когда браузеру известно, какие объекты будут отображаться на странице, где их разместить и какие стили им нужно присвоить, можно приступать к следующему этапу - выводу страницы на экран. Этот этап также называется визуализацией или растеризацией.

Как вы наверняка заметили, браузеру нужно выполнить множество действий, на которые зачастую уходит немало времени. Чтобы оптимизировать время загрузки, продолжительность каждого этапа можно отслеживать с помощью инструментов разработчика в Chrome. Рассмотрим этап формирования макета на нашем первом примере Hello World:

This can take some time because the browser has to do quite a bit of work. However, Chrome DevTools can provide some insight into all three of the stages described above. Let's examine the layout stage for our original "hello world" example:

<img src="images/layout-timeline.png" alt="Measuring layout in DevTools" />

* Событие Layout на вкладке Timeline отражает время, затраченное на формирование макета, а также на определение положения и размера объектов.
* Сформировав макет, браузер переходит к этапам Paint Setup и Paint для преобразования модели визуализации в пиксели.

Тем временем, визуализация завершилась, и наша страница появилась в области просмотра!

The page is finally visible in the viewport:

<img src="images/device-dom-small.png" alt="Rendered Hello World page" />

Вот что нужно проделать для визуализации даже такой простой страницы, как наша. А если мы внесем изменения в DOM или CSSOM, браузеру придется повторить все действия, чтобы определить, как вывести пиксели на экран.

1. Обработка HTML-разметки и создание модели DOM.
2. Обработка CSS-файла и создание модели CSSOM.
3. Создание модели визуализации из DOM и CSSOM.
4. Определение формы и расположения объектов, создание макета.
5. Вывод объектов на экран.

**Чтобы оптимизировать процесс визуализации, нужно уменьшить время, затрачиваемое на выполнение пяти действий из списка выше.** В результате контент будет выводиться на экран быстрее, время обновления сократится, а вместе с этим возрастет частота обновления интерактивного контента.

***Optimizing the critical rendering path* is the process of minimizing the total amount of time spent performing steps 1 through 5 in the above sequence.** Doing so renders content to the screen as quickly as possible and also reduces the amount of time between screen updates after the initial render; that is, achieve higher refresh rates for interactive content.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}