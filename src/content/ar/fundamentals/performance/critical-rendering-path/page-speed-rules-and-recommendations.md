project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: PageSpeed Insights في السياق: ما يجب الانتباه له عند تحسين مسار العرض الحرج وسبب ذلك.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# قواعد وتوصيات PageSpeed {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

PageSpeed Insights في السياق: ما يجب الانتباه له عند تحسين مسار العرض الحرج وسبب ذلك.

## الحد من جافا سكريبت وCSS الذي يحظر العرض

حتى نتمكن من الحصول على أسرع عرض أول ممكن، يلزمنا الوصول إلى أقل عدد (ممكن) من الموارد الحرجة على الصفحة، والوصول إلى أقل عدد من وحدات بايت الحرجة التي يتم تنزيلها، والوصول إلى أقل وقت للمسار الحرج.

## تحسين استخدام جافا سكريبت

تعد موارد جافا سكريبت وسيلة لحظر المحلل افتراضيًا ما لم يتم وضع علامة *async* أو الإضافة عبر مقتطف جافا سكريبت خاص. تفرض جافا سكريبت التي تحظر المحلل على المتصفح انتظار CSSOM وتعمل على إيقاف إنشاء DOM مؤقتًا، وهذا بدوره يؤدي إلى تأخر في العرض لأول مرة.

### Prefer asynchronous JavaScript resources

تلغي الموارد غير المتزامنة حظر محلل المستند وتتيح للمتصفح تجنب الحظر على CSSOM قبل تنفيذ النص البرمجي. في الغالب، إذا كان من الممكن جعل النص البرمجي غير متزامن، فهذا يعني أنه ليس ضروريًا للعرض أول مرة - فيمكنك تحميل نصوص برمجية غير متزامنة بعد العرض الأول.

### Avoid synchronous server calls

يجب تأجيل أية نصوص برمجية غير أساسية وليست حرجة لإنشاء المحتوى المرئي في العرض الأول، وذلك لتقليل نسبة العمل المطلوب من المتصفح لعرض الصفحة.

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
    

يؤدي جافا سكريبت الذي يستغرق وقتًا طويلاً إلى حظر المتصفح من إنشاء DOM وCSSOM ومن عرض الصفحة. ونتيجة لذلك، يجب تأجيل أي منطق للبدء أو وظيفة غير أساسية للعرض لأول مرة حتى وقت لاحق. وإذا كانت هناك حاجة إلى تشغيل تسلسل بدء طويل، فجرب تقسيمه إلى عدة مراحل للسماح للمتصفح بمعالجة أحداث أخرى في الوسط.

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
    

يلزم توفر CSS لإنشاء شجرة العرض وسيحظر جافا سكريبت في الغالب على CSS أثناء الإنشاء الأول للصفحة. يجب التأكد من أن أي CSS غير أساسي قد تم وصفه باعتباره غير حرج (مثل الطباعة واستعلامات الوسائط الأخرى)، وأن مقدار CSS الحرج ووقت عرضه صغير قدر الإمكان.

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

يجب تحديد جميع موارد CSS في أقرب وقت ممكن ضمن مستند HTML، حتى يتمكن المتصفح من اكتشاف علامات `<link>` وإرسال الطلب لصالح CSS في أسرع وقت ممكن.

### Avoid long running JavaScript

يؤدي توجيه استيراد CSS وهو (`@import`) إلى تمكين ورقة أنماط واحدة لاستيراد القواعد من ملف ورقة أنماط أخرى. إلا أنه يجب تجنب هذه التوجيهات نظرًا لأنها تضع جولات إضافية في المسار الحرج: يتم اكتشاف موارد CSS بعد تلقي ورقة أنماط CSS التي تتضمن قاعدة @import نفسها وتحليلها.

## تحسين استخدام CSS

للحصول على أفضل أداء، يمكنك تضمين CSS الحرج مباشرة في مستند HTML. ويؤدي هذا إلى تقليص عدد الجولات الإضافية في المسار الحرج وإذا تم تقديمه على نحو سليم، فيمكن استخدامه لتقديم طول مسار حرج مكون من `جولة واحدة` حيث يكون HTML هو عنصر الحظر الوحيد.

### Put CSS in the document head

Specify all CSS resources as early as possible within the HTML document so that the browser can discover the `<link>` tags and dispatch the request for the CSS as soon as possible.

### Avoid CSS imports

The CSS import (`@import`) directive enables one stylesheet to import rules from another stylesheet file. However, avoid these directives because they introduce additional roundtrips into the critical path: the imported CSS resources are discovered only after the CSS stylesheet with the `@import` rule itself is received and parsed.

### Inline render-blocking CSS

For best performance, you may want to consider inlining the critical CSS directly into the HTML document. This eliminates additional roundtrips in the critical path and if done correctly can deliver a "one roundtrip" critical path length where only the HTML is a blocking resource.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}