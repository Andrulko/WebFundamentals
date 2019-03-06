project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Узнайте, как смягчать и придавать вес анимации

{# wf_updated_on: 2014-10-20 #} {# wf_published_on: 2014-08-08 #}

# Основы изменения скорости {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}

В природе ничего не перемещается поступательно от одной точки к другой. В действительности любой перемещающийся предмет ускоряется или замедляется. Наш мозг ожидает именно такое перемещение, поэтому при анимации эту особенность следует использовать себе на пользу. Естественные движения придадут пользователям чувство большего комфорта при работе с вашими приложениями, что, в свою очередь, приведет к формированию более благоприятного ощущения в целом

### TL;DR {: .hide-from-toc }

* Изменение скорости позволяет сделать анимацию более естественной.
* Выбирайте анимацию с замедлением для элементов пользовательского интерфейса.
* Анимацию с ускорением или с ускорением в начале и замедлением в конце следует использовать, только если ее продолжительность мала. Пользователи зачастую воспринимают такую анимацию как медлительную.

В классической анимации движение, скорость которого в начале низкая, а затем увеличивается, называется "slow in" (смягчение в начале движения), а то, скорость которого в начале высокая, а затем уменьшается ― "slow out" (смягчение в конце движения). В Интернете же эти варианты движения называются "ease in" (ускорение) и "ease out" (замедление). Иногда используется сочетание двух этих вариантов, называемое "ease in out" (ускорение в начале – замедление в конце). Отсюда понятно, что изменение скорости ― это, по сути, процесс, направленный на то, чтобы сделать анимацию не столь резкой, смягчить ее.

## Ключевые слова изменения скорости

И переходы, и анимация средствами CSS позволяют вам [выбирать подходящий вариант изменения скорости](/web/fundamentals/design-and-ux/animations/choosing-the-right-easing). Можно использовать ключевые слова, которые влияют на изменение скорости (или времени) данной анимации. Также можно [сделать изменение скорости абсолютно нестандартным](/web/fundamentals/design-and-ux/animations/custom-easing), что дает намного больше свободы для выражения индивидуальности своего приложения.

Вот некоторые ключевые слова, которые можно использовать в CSS:

* `linear`
* `ease-in`
* `ease-out`
* `ease-in-out`

Источник: [CSS Transitions, W3C](http://www.w3.org/TR/css3-transitions/#transition-timing-function-property)

Также можно использовать ключевое слово `steps`, которое позволяет создавать переходы с дискретными шагами. Однако приведенные выше ключевые слова являются наиболее подходящими для создания естественной анимации, а это именно то, что вам желательно.

## Линейная анимация

<div class="attempt-right">
  <figure>
    <img src="images/linear.png" alt="Linear ease animation curve." />
  </figure>
</div>

Анимация без какого-либо изменения скорости называется **линейной**. График линейного перехода выглядит следующим образом:

As time moves along, the value increases in equal amounts. With linear motion, things tend to feel robotic and unnatural, and this is something that users find jarring. Generally speaking, you should avoid linear motion.

Whether you’re coding your animations using CSS or JavaScript, you’ll find that there is always an option for linear motion.

Со временем значение равномерно увеличивается. Линейное движение напоминает работу робота, оно неестественно, это вряд ли понравится пользователям. В целом, линейного движения следует избегать.

<div style="clear:both;"></div>

При написании анимации с использованием CSS или JavaScript всегда имеется возможность воссоздать линейное движение. Чтобы добиться показанного выше эффекта с помощью CSS, потребуется примерно такой код:

    transition: transform 500ms linear;
    

## Анимация с замедлением

<div class="attempt-right">
  <figure>
    <img src="images/ease-out.png" alt="Ease-out animation curve." />
  </figure>
</div>

При замедлении анимация начинается на высокой скорости, затем идет линейная часть движения, а в конце скорость падает.

Easing out is typically the best for user interface work, because the fast start gives your animations a feeling of responsiveness, while still allowing for a natural slowdown at the end.

Есть много способов достичь эффекта замедления, самым простым из которых является ключевое слово CSS `ease-out`:

<div style="clear:both;"></div>

There are many ways to achieve an ease out effect, but the simplest is the `ease-out` keyword in CSS:

    transition: transform 500ms ease-out;
    

## Анимация с ускорением

<div class="attempt-right">
  <figure>
    <img src="images/ease-in.png" alt="Ease-in animation curve." />
  </figure>
</div>

Замедление обычно лучше всего подходит для пользовательских интерфейсов, поскольку быстрое начало дает анимации ощущение динамичности, при этом в конце остается место естественному замедлению.

В отличие от анимации с замедлением, такая анимация начинается медленно, а заканчивается на высокой скорости.

From an interaction point of view, however, ease-ins can feel a little unusual because of their abrupt end; things that move in the real world tend to decelerate rather than simply stopping suddenly. Ease-ins also have the detrimental effect of feeling sluggish when starting, which negatively impacts the perception of responsiveness in your site or app.

[See an ease-in animation](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/animations/box-move-ease-in.html){: target="_blank" .external }

<div style="clear:both;"></div>

Такая анимация напоминает падающий камень, его движение начинается медленно, а до земли он долетает быстро с глухим ударом.

    transition: transform 500ms ease-in;
    

## Анимация с ускорением в начале и сзамедлением в конце

<div class="attempt-right">
  <figure>
    <img src="images/ease-in-out.png" alt="Ease-in-out animation curve." />
  </figure>
</div>

При написании анимации с ускорением, так же, как и анимации с замедлением и линейной анимации, нужно использовать ключевое слово:

Однако с точки зрения взаимодействия такая анимация ощущается немного необычно из-за резкого завершения. Движения в реальном мире имеют тенденцию к замедлению, а не просто резко останавливаются. Кроме того, анимация с ускорением также вызывает нежелательное ощущение медлительности, которое отрицательно влияет на восприятие вашего сайта или приложения.

Ускорение в начале и замедление в конце движения свойственно автомобилю, который ускоряется и тормозит. При правильном использовании такая анимация может сформировать более впечатляющий эффект, чем анимация только с замедлением.

<div style="clear:both;"></div>

To get an ease-in-out animation, you can use the `ease-in-out` CSS keyword:

    transition: transform 500ms ease-in-out;
    

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}