project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Узнайте, как добиться анимационного эффекта для модальных представлений в приложениях

{# wf_updated_on: 2014-10-20 #} {# wf_published_on: 2014-08-08 #}

# Создание анимации модальных представлений {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}

<div class="attempt-right">
  <figure>
    <img src="images/dont-press.gif" alt="Animating a modal view." />
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/animations/modal-view-animation.html" target="_blank" class="external">Try it</a>
    </figcaption>
  </figure>
</div>

Модальные представления предназначены для вывода столь важных сообщений, что для их отображения есть все основания заблокировать пользовательский интерфейс. При выборе таких представлений следует соблюдать осторожность, поскольку они прерывают работу приложений и в случае чрезмерного их применения могут испортить пользователям все впечатление от программы. Однако в определенных обстоятельствах именно такие представления и следует использовать. Добавление же к ним анимационных эффектов сделает их более привлекательными

### TL;DR {: .hide-from-toc }

* Модальные представления не следует использовать слишком часто; пользователям очень не понравится, если их работу прерывать без надобности.
* Добавление масштабирования к анимации позволяет получить зрелищный эффект 'выпадения'.
* Когда пользователь закрывает модальное представление, оно должно исчезать с экрана быстро. А вот выводить его на экран следует немного медленнее, чтобы это было не так неожиданно для пользователя.

<div class="clearfix"></div>

The modal overlay should be aligned to the viewport, so set its `position` to `fixed`:

    .modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    
      pointer-events: none;
      opacity: 0;
    
      will-change: transform, opacity;
    }
    

It has an initial `opacity` of 0, so it's hidden from view, but then it also needs `pointer-events` set to `none` so that clicks and touches pass through. Without that, it blocks all interactions, rendering the whole page unresponsive. Finally, because it animates its `opacity` and `transform`, those need to be marked as changing with `will-change` (see also [Using the will-change property](animations-and-performance#using-the-will-change-property)).

Модальное наложение должно быть выровнено по viewport, поэтому его свойству `position` следует задать значение `fixed`:

    .modal.visible {
      pointer-events: auto;
      opacity: 1;
    }
    

Начальное значение его свойства `opacity` равно 0, поэтому это представление не отображается, но кроме того, свойству `pointer-events` также необходимо задать значение `none`, с тем, чтобы представление не реагировало на нажатия. В противном случае будет заблокировано все взаимодействие и вся страница перестанет реагировать на действия пользователя. И наконец, поскольку эффекты анимации будут добавлены к свойствам `opacity` и `transform`, эти свойства необходимо обозначить как изменяющиеся с помощью атрибута `will-change` (см. также [Использование свойства will-change](/web/fundamentals/design-and-ux/animations/animations-and-performance#using-the-will-change-property)).

    modal.classList.add('visible');
    

Когда представление отображается на экране, оно должно реагировать на действия пользователя, а его свойство `opacity` должно иметь значение 1:

    .modal {
      -webkit-transform: scale(1.15);
      transform: scale(1.15);
    
      -webkit-transition:
        -webkit-transform 0.1s cubic-bezier(0.465, 0.183, 0.153, 0.946),
        opacity 0.1s cubic-bezier(0.465, 0.183, 0.153, 0.946);
    
      transition:
        transform 0.1s cubic-bezier(0.465, 0.183, 0.153, 0.946),
        opacity 0.1s cubic-bezier(0.465, 0.183, 0.153, 0.946);
    
    }
    

Теперь каждый раз, когда необходимо отобразить модальное представление, можно переключать класс "visible" с помощью JavaScript:

В этот момент модальное представление откроется вообще без анимации, которую теперь можно добавить (см. также [Изменение скорости при нестандартной анимации](/web/fundamentals/design-and-ux/animations/custom-easing)):

    .modal.visible {
    
      -webkit-transform: scale(1);
      transform: scale(1);
    
      -webkit-transition:
        -webkit-transform 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946),
        opacity 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946);
    
      transition:
        transform 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946),
        opacity 0.3s cubic-bezier(0.465, 0.183, 0.153, 0.946);
    
    }
    

При добавлении свойства `scale` к свойству transform представление будет как бы падать на экран ― довольно зрелищный эффект. Стандартный переход применяется и к свойству transform, и к свойству opacity с нестандартной кривой и продолжительностью в 0,1 секунды.

## Feedback {: #feedback }

Продолжительность весьма невелика, но она идеально подходит для ситуации, когда пользователь закрывает представление и возвращается в приложение. Недостатком является то, что появление модального представления на экране, возможно, выглядит слишком агрессивно. Чтобы исправить этот недостаток, необходимо переопределить значения перехода для класса `visible`: