project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Узнайте, как обеспечить плавный переход с использованием анимации между двумя представлениями в приложениях

{# wf_updated_on: 2014-10-21 #} {# wf_published_on: 2014-08-08 #}

# Анимационные переходы между представлениями {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}

В приложении нередко бывает необходимо, чтобы пользователь выполнил переход от одного представления к другому. Это может быть переход из списка в представление с подробными данными или вывод на экран боковой панели навигации. Добавление анимационных эффектов к переходам между этими представлениями отлично подходит, чтобы удержать внимание пользователя и повысить визуальную привлекательность ваших проектов

### TL;DR {: .hide-from-toc }

* Используйте свойство transition для перемещения между представлениями; не следует применять `left`, `top` или любое другое свойство, которое вызывает перерасчет макета.
* Любая анимация должна быть мгновенной, ее продолжительность не следует делать большой.
* Продумайте, как будут меняться анимационные эффекты и макеты при увеличении размера экрана; то, что подойдет для небольшого экрана может выглядеть неуклюже на экране большого размера.

То, как эти переходы между представлениями будут выглядеть и выполняться, в значительной степени зависит от типа представлений, с которыми вы имеете дело. Поэтому, например, анимация модального наложения поверх какого-либо представления будет отличаться от перехода от списка к представлению с подробными сведениями или наоборот.

Note: Нужно стремиться к тому, чтобы частота кадров любой анимации составляла 60 кадров в секунду. Это позволит избежать запинок при воспроизведении анимации, которые вряд ли понравятся вашим пользователям. Любому анимируемому элементу следует задать свойство will-change для всех компонентов, которые планируется изменять, задолго до того, как будет реализована сама анимация. Весьма вероятно, что для переходов между представлениями будет использоваться свойство `will-change: transform`

## Использование атрибутов translation для перемещения между представлениями

<div class="attempt-left">
  <figure>
    <img src="images/view-translate.gif" alt="Translating between two views" />
  </figure>
</div>

Чтобы не усложнять, давайте предположим, что есть два представления: представление в виде списка и представление с подробными сведениями. После того как пользователь нажмет элемента списка в представлении списка, представление с подробными сведениями появится на экране, а представление списка исчезнет с него.

<div style="clear:both;"></div>

<div class="attempt-right">
  <figure>
    <img src="images/container-two-views.svg" alt="View hierarchy." />
  </figure>
</div>

To achieve this effect, you need a container for both views that has `overflow: hidden` set on it. That way, the two views can both be inside the container side-by-side without showing any horizontal scrollbars, and each view can slide side-to-side inside the container as needed.

<div style="clear:both;"></div>

Чтобы достичь такого эффекта, вам потребуется контейнер для обоих представлений, которому будет задано свойство `overflow: hidden`. Таким образом, оба представления смогут находиться в контейнере одновременно без отображения горизонтальных полос прокрутки, при этом каждое представление будет при необходимости скользить из стороны в сторону внутри контейнера.

    .container {
      width: 100%;
      height: 100%;
      overflow: hidden;
      position: relative;
    }
    

The position of the container is set as `relative`. This means that each view inside it can be positioned absolutely to the top left corner and then moved around with transforms. This approach is better for performance than using the `left` property (because that triggers layout and paint), and is typically easier to rationalize.

    .view {
      width: 100%;
      height: 100%;
      position: absolute;
      left: 0;
      top: 0;
    
      /* let the browser know we plan to animate
         each view in and out */
      will-change: transform;
    }
    

Код CSS для контейнера:

    .view {
      /* Prefixes are needed for Safari and other WebKit-based browsers */
      transition: -webkit-transform 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946);
      transition: transform 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946);
    }
    

Положение контейнера задается как `relative`. Это означает, что каждое представление, находящееся внутри контейнера, можно разместить точно в левом верхнем углу, а затем перемещать с помощью свойства transform. Такой способ дает выигрыш с точки зрения производительности по сравнению с использованием свойства `left` (поскольку оно вызывает перерасчет макета и фактическую перерисовку страницы), кроме того, как правило, его проще рационализировать.

    .details-view {
      -webkit-transform: translateX(100%);
      transform: translateX(100%);
    }
    

Добавление `transition` к свойству `transform` обеспечивает привлекательный эффект скольжения. Чтобы скольжение вызывало приятные ощущения, необходимо использовать нестандартную кривую `cubic-bezier`, которая рассматривалась в документе \[Руководство по изменению скорости при нестандартной анимации\] (custom-easing.html).

    var container = document.querySelector('.container');
    var backButton = document.querySelector('.back-button');
    var listItems = document.querySelectorAll('.list-item');
    
    /**
     * Toggles the class on the container so that
     * we choose the correct view.
     */
    function onViewChange(evt) {
      container.classList.toggle('view-change');
    }
    
    // When you click on a list item bring on the details view.
    for (var i = 0; i < listItems.length; i++) {
      listItems[i].addEventListener('click', onViewChange, false);
    }
    
    // And switch it back again when you click on the back button
    backButton.addEventListener('click', onViewChange);
    

Представление, которое исчезает с экрана, следует перемещать вправо, поэтому в данном случае представление с подобными сведениями необходимо двигать:

    .view-change .list-view {
      -webkit-transform: translateX(-100%);
      transform: translateX(-100%);
    }
    
    .view-change .details-view {
      -webkit-transform: translateX(0);
      transform: translateX(0);
    }
    

Далее необходимо использовать небольшой фрагмент кода JavaScript для работы с классами. Это позволит включить соответствующие классы для представлений.

Наконец, добавляем декларации CSS для этих классов.

Caution: Making this kind of hierarchy in a cross-browser way can be challenging. For example, iOS requires an additional CSS property, `-webkit-overflow-scrolling: touch`, to "reenable" fling scrolling, but you don’t get to control which axis that’s for, as you can with the standard overflow property. Be sure to test your implementation across a range of devices!

Этот код можно изменить, включив в него несколько представлений, но основная идея при этом останется неизменной. Каждое невидимое представление должно находиться за пределами экрана и появляться на нем только при необходимости, при этом представление, которое в данный момент находится на экране, должно с него исчезать.

## Обеспечение работы анимации на больших экранах

<div class="attempt-right">
  <figure>
    <img src="images/container-two-views-ls.svg" alt="View hierarchy on a large screen." />
  </figure>
</div>

Note: Создание такой иерархии, которая будет работать в разных браузерах, может быть очень сложной задачей. Например, в iOS для повторного включения fling-прокрутки требуется дополнительное свойство CSS, `-webkit-overflow-scrolling: touch`, однако при этом невозможно контролировать, для какой оси эта прокрутка будет работать (тогда как стандартное свойство overflow позволяет это делать). Обязательно тестируйте работу своего кода на различных устройствах!

## Feedback {: #feedback }

Помимо перехода между представлениями, этот метод также можно применять для других элементов, выводимых на экран, таких как боковые элементы навигации. Разница заключается лишь в том, что другие представления при этом перемещать не нужно.