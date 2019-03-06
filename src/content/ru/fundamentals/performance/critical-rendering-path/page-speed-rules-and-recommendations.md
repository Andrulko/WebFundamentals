project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Правила PageSpeed Insights: на что обращать внимание при оптимизации процесса визуализации и почему.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Правила и рекомендации PageSpeed {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Правила PageSpeed Insights: на что обращать внимание при оптимизации процесса визуализации и почему.

## Устраните файлы JavaScript и CSS, задерживающие загрузку

Чтобы браузер мог быстро вывести страницу на экран, минимизируйте число первоочередных ресурсов и байтов, а также сократите продолжительность обработки. Если возможно, удалите некоторые ресурсы.

## Оптимизируйте JavaScript

Вашему скрипту не присвоен параметр async? Вы не пользуетесь специальным снипетом JavaScript? Значит, ваш код замедляет загрузку страницы. Обнаружив скрипт, браузер начинает формировать модель CSSOM. Создание модели DOM приостанавливается, и возникает задержка.

### Prefer asynchronous JavaScript resources

Асинхронные скрипты не блокируют работу анализатора, поскольку не требуют от браузера создания модели CSSOM. Кроме того, если скрипт можно сделать асинхронным, значит, он не играет роли для первоочередной визуализации. Попробуйте загрузить его после визуализации.

### Avoid synchronous server calls

Если первоочередная визуализация возможна без скрипта, отложите его запуск. Так вы уменьшите нагрузку, с которой должен справиться браузер для вывода страницы на экран.

    <script>
      function() {
        window.addEventListener('pagehide', logData, false);
        function logData() {
          navigator.sendBeacon(
            'https://putsreq.herokuapp.com/Dt7t2QzUkG18aDTMMcop',
            'Sent by a beacon!');
        }
      }();
    </script>
    

Поскольку такой код задерживает создание DOM и CSSOM, а также визуализацию страницы, выполнение всех скриптов, не критичных для первоочередной визуализации, нужно отложить. Если долговыполняемый код все-таки необходим, разделите его на несколько этапов, чтобы между их обработкой браузер мог продолжить визуализацию.

    <script>
    fetch('./api/some.json')  
      .then(  
        function(response) {  
          if (response.status !== 200) {  
            console.log('Looks like there was a problem. Status Code: ' +  response.status);  
            return;  
          }
          // Examine the text in the response  
          response.json().then(function(data) {  
            console.log(data);  
          });  
        }  
      )  
      .catch(function(err) {  
        console.log('Fetch Error :-S', err);  
      });
    </script>
    

С помощью CSS браузер строит модель визуализации. Кроме того, модель CSSOM необходима для выполнения JavaScript, поэтому скрипт блокирует анализатор и ждет завершения обработки CSS. Чтобы не задерживать загрузку, присвойте второстепенным CSS-файлам (которые нужны для печати, вывода страницы на большой экран и т. п.) соответствующий параметр media. Минимизируйте количество первоочередных CSS-файлов и время их загрузки.

    <script>
    fetch(url, {
      method: 'post',
      headers: {  
        "Content-type": "application/x-www-form-urlencoded; charset=UTF-8"  
      },  
      body: 'foo=bar&lorem=ipsum'  
    }).then(function() { // Additional code });
    </script>
    

### Defer parsing JavaScript

Все CSS-файлы необходимо размещать как можно выше в HTML-документе, чтобы браузер мог быстрее обнаружить тег 

<link />
и скачать файлы.

### Avoid long running JavaScript

Избегайте правила @import, которое позволяет копировать стили из других CSS-файлов. Оно увеличивает число соединений с сервером, поскольку файл, на который вы ссылаетесь, тоже нужно скачать и проанализировать.

## Оптимизируйте обработку CSS

Иногда процесс визуализации можно оптимизировать, поместив код CSS в HTML-документ. Таким образом вы можете сократить число соединений с сервером и даже свести процесс визуализации к обработке одного файла - HTML-документа.

### Put CSS in the document head

Specify all CSS resources as early as possible within the HTML document so that the browser can discover the `<link>` tags and dispatch the request for the CSS as soon as possible.

### Avoid CSS imports

The CSS import (`@import`) directive enables one stylesheet to import rules from another stylesheet file. However, avoid these directives because they introduce additional roundtrips into the critical path: the imported CSS resources are discovered only after the CSS stylesheet with the `@import` rule itself is received and parsed.

### Inline render-blocking CSS

For best performance, you may want to consider inlining the critical CSS directly into the HTML document. This eliminates additional roundtrips in the critical path and if done correctly can deliver a "one roundtrip" critical path length where only the HTML is a blocking resource.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}