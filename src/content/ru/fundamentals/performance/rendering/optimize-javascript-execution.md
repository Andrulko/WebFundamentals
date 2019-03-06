project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: JavaScript often triggers visual changes. Sometimes that's directly through style manipulations, and sometimes it's calculations that result in visual changes, like searching or sorting data. Badly-timed or long-running JavaScript is a common cause of performance issues. You should look to minimize its impact where you can.

{# wf_updated_on: 2015-03-19 #} {# wf_published_on: 2000-01-01 #}

# Оптимизация выполнения JavaScript {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}

JavaScript часто используется для внесения визуальных изменений. Иногда это делается непосредственно путем переработки стилей, в других же случаях визуальные изменения являются результатом определенных вычислений, например поиска или сортировки тех или иных данных. Неправильно выбранные параметры времени и продолжительности выполнения JavaScript часто являются причиной проблем с производительностью, поэтому при любой возможности следует стараться свести влияние этого кода к минимуму

Профилирование производительности JavaScript иногда является своего рода искусством, поскольку код JavaScript, который вы пишете, не имеет ничего общего с кодом, который фактически выполняется. Современные браузеры используют компиляторы JIT и всевозможные варианты оптимизации с целью добиться наиболее быстрого выполнения, а это коренным образом меняет динамику кода.

Note: Если вы хотите посмотреть JIT в работе, рекомендуем инструмент [IRHydra<sup>2</sup> разработки Вячеслава Егорова](http://mrale.ph/irhydra/2/). Этот образец демонстрирует промежуточное состояние кода JavaScript, когда его оптимизирует движок JavaScript браузера Chrome (V8).

Однако, даже с учетом всего вышесказанного, несомненно можно кое-что сделать, чтобы помочь приложениям хорошо выполнять код JavaScript.

### TL;DR {: .hide-from-toc }

* Не используйте функции setTimeout или setInterval для внесения визуальных изменений; вместо этого всегда пользуйтесь функцией `requestAnimationFrame`.
* Перемещайте скрипты JavaScript, которые выполняются долго, за пределы основного потока в рабочие веб-процессы Web Worker.
* Для внесения изменений в DOM-элементы за несколько кадров используйте микрозадачи.
* Для оценки влияния JavaScript используйте шкалу времени и средство профилирования JavaScript из Chrome DevTools.

## Используйте функцию requestAnimationFrame для внесения визуальных изменений

Когда на экране происходят визуальные изменения, свою работу нужно выполнять в подходящее время для браузера, а именно – в самом начале кадра. Единственным способом гарантировать выполнение кода JavaScript в начале кадра является использование функции `requestAnimationFrame`.

    /**
     * If run as a requestAnimationFrame callback, this
     * will be run at the start of the frame.
     */
    function updateScreen(time) {
      // Make visual updates here.
    }
    
    requestAnimationFrame(updateScreen);
    

Платформы или образцы могут использовать функции `setTimeout` или `setInterval` для реализации таких визуальных изменений, как анимация, однако проблема заключается в том, что обратный вызов будет выполняться *где-то* в течение кадра, возможно даже в самом его конце, а это может вызвать пропуск кадра, результатом чего будет подвисание.

<img src="images/optimize-javascript-execution/settimeout.jpg" alt="Функция setTimeout, из-за которой браузер пропускает кадр." />

Скажу больше, на сегодня для `animate` jQuery по умолчанию использует `setTimeout`! Можно [установить исправление, чтобы использовалась функция `requestAnimationFrame`](https://github.com/gnarf/jquery-requestAnimationFrame), что настоятельно рекомендуется сделать.

## Снижайте сложность или используйте рабочие веб-процессы Web Worker

JavaScript выполняется в основном потоке браузера вместе с вычислением стилей, макета и, во многих случаях, прорисовкой. Если ваш код JavaScript выполняется в течение длительного времени, он заблокирует все эти задачи, что может привести к пропуску кадров.

Следует тактически грамотно выбирать время и продолжительность выполнения JavaScript. Например, если выполняется такая анимация, как прокрутка, идеальным будет выполнение JavaScript в течение первых **3–4 мс**. Чуть дольше – и вы рискуете занять слишком много времени. Если же в данный момент никаких действий не выполняется, то за временем работы можно позволить себе следить не так строго.

Во многих случаях чисто вычислительную работу можно переместить в [рабочие веб-процессы (Web Worker)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/basic_usage), если, например, для нее не требуется доступ к DOM. Обработка данных или такие промежуточные состояния, как сортировка или поиск, нередко хорошо подходят для этой модели, как и загрузка или формирование моделей.

    var dataSortWorker = new Worker("sort-worker.js");
    dataSortWorker.postMesssage(dataToSort);
    
    // The main thread is now free to continue working on other things...
    
    dataSortWorker.addEventListener('message', function(evt) {
       var sortedData = e.data;
       // Update data on screen...
    });
    

Не вся работа годится для этой модели: у рабочих веб-процессов Web Worker нет доступа к DOM. Когда работу необходимо выполнять в основном потоке, подумайте об использовании пакетов, когда крупные задачи разбиваются на несколько микрозадач, каждая из которых занимает лишь несколько миллисекунд и выполняется внутри обработчиков `requestAnimationFrame` в каждом кадре.

    var taskList = breakBigTaskIntoMicroTasks(monsterTaskList);
    requestAnimationFrame(processTaskList);
    
    function processTaskList(taskStartTime) {
      var taskFinishTime;
    
      do {
        // Assume the next task is pushed onto a stack.
        var nextTask = taskList.pop();
    
        // Process nextTask.
        processTask(nextTask);
    
        // Go again if there’s enough time to do the next task.
        taskFinishTime = window.performance.now();
      } while (taskFinishTime - taskStartTime < 3);
    
      if (taskList.length > 0)
        requestAnimationFrame(processTaskList);
    
    }
    

Такой подход несет с собой последствия для восприятия пользователей и пользовательского интерфейса, поэтому с помощью [индикатора хода выполнения или действия](http://www.google.com/design/spec/components/progress-activity.html) пользователя необходимо будет проинформировать о том, что в данный момент выполняется некая задача. В любом случае такой подход позволяет освободить основной поток вашего приложения, что дает ему возможность лучше реагировать на действия пользователей.

## Знайте, как ваш код JavaScript влияет на кадры

При оценке платформы, библиотеки или собственного кода важно определить, во что обойдется выполнение кода JavaScript в каждом кадре. Это особенно важно при создании анимации, которая обязательно должна работать без подвисаний, например, переходов или прокрутки.

Лучше всего определять затраты на выполнение кода JavaScript и его профиль производительности с помощью Chrome DevTools. Обычно программа выдает не очень подробные записи следующего вида:

<img src="images/optimize-javascript-execution/low-js-detail.png"
     alt="Шкала времени Chrome DevTools с малоинформативными сведениями о выполнении JS." />

Если оказалось, что код JavaScript выполняется долго, в верхней части пользовательского интерфейса DevTools можно будет включить средство профилирования JavaScript:

Armed with this information you can assess the performance impact of the JavaScript on your application, and begin to find and fix any hotspots where functions are taking too long to execute. As mentioned earlier you should seek to either remove long-running JavaScript, or, if that’s not possible, move it to a Web Worker freeing up the main thread to continue on with other tasks.

Для определения профиля работы кода JavaScript этим способом требуется больше ресурсов, поэтому его следует включать, только когда требуются дополнительные сведения о характеристиках времени выполнения JavaScript. Установив этот флажок, можно выполнить те же действия и получить намного больше информации о том, какие функции вызывались в JavaScript:

## Избегайте микрооптимизации кода JavaScript

It may be cool to know that the browser can execute one version of a thing 100 times faster than another thing, like that requesting an element’s `offsetTop` is faster than computing `getBoundingClientRect()`, but it’s almost always true that you’ll only be calling functions like these a small number of times per frame, so it’s normally wasted effort to focus on this aspect of JavaScript’s performance. You'll typically only save fractions of milliseconds.

Вооружившись этими сведениями, можно оценить воздействие, которое JavaScript окажет на производительность приложения, и начать выявлять и исправлять те места, в которых функции выполняются слишком долго. Как уже говорилось ранее, код JavaScript, который выполняется долго, необходимо либо убрать совсем, либо, если это невозможно, переместить его в рабочий веб-процесс (Web Worker), высвободив основной поток для продолжения обработки других задач.

Возможно, это круто ― знать, что браузер может выполнить одну версию кода в 100 раз быстрее, чем другую, например, что запросы или `offsetTop` элемента быстрее, чем вычисление `getBoundingClientRect()`. Однако почти всегда верно, что такие функции вызываются лишь несколько раз за кадр, поэтому уделять этой стороне работы JavaScript основное внимание – это просто пустая трата времени. Сэкономить удастся лишь доли миллисекунды.

## Feedback {: #feedback }

Если вы пишете игру или приложение с большим объемом вычислений, то можно сделать исключение из этого правила, поскольку, скорее всего, нужно будет умещать в отдельные кадры множество вычислений, а в этом случае нужно искать любые возможные варианты.