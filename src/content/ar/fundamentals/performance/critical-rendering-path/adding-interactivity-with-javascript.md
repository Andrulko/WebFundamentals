project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: يتيح لنا جافا سكريبت تعديل كل شيء في الصفحة: المحتوى والتنسيق والسلوك مع تفاعلات المستخدمين. إلا أنه يمكن لجافا سكريبت كذلك حظر تركيبة DOM والتأخير عند عرض الصفحة. اهتم بأن يكون جافا سكريبت غير متزامن وتخلص من أية عناصر جافا سكريبت غير لازمة في مسار العرض الحرج للحصول على أفضل أداء.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2013-12-31 #}

# إضافة تفاعل مع جافا سكريبت {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

يتيح لنا جافا سكريبت تعديل كل شيء في الصفحة: المحتوى والتنسيق والسلوك مع تفاعلات المستخدمين. إلا أنه يمكن لجافا سكريبت كذلك حظر تركيبة DOM والتأخير عند عرض الصفحة. اهتم بأن يكون جافا سكريبت غير متزامن وتخلص من أية عناصر جافا سكريبت غير لازمة في مسار العرض الحرج للحصول على أفضل أداء.

### TL;DR {: .hide-from-toc }

* يمكن لجافا سكريبت الاستعلام عن كل من DOM وCSSOM وتعديلهما.
* يؤدي تنفيذ جافا سكريبت إلى الحظر على CSSOM.
* يحظر جافا سكريبت تركيبة DOM ما لم يتم الإفصاح عن عدم تزامنها.

تعد جافا سكريبت لغة ديناميكية تعمل في المتصفح وتتيح لنا تغيير كل شيء يتعلق بسلوك الصفحة: يمكننا تعديل المحتوى على الصفحة من خلال إضافة عناصر أو إزالتها من شجرة DOM، كما يمكننا تعديل خصائص CSSOM لكل عنصر، ويمكننا التعامل مع إدخال المستخدم، وغير ذلك الكثير. ولتوضيح ذلك عمليًا، يمكننا أن نزيد على مثال `Hello World` السابق نصًا برمجيًا مضمَّنًا وبسيطًا:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/script.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/script.html){: target="_blank" .external }

* يتيح لنا جافا سكريبت الوصول إلى DOM وسحب المرجع إلى عقدة الدورة المخفية، وقد لا تكون العقدة مرئية في شجرة العرض، ولكنها تظل موجودة في DOM! بعد ذلك وبمجرد أن يكون لدينا المرجع، يمكننا تغيير النص (عبر .textContent) وإلغاء خاصية نمط العرض المحسوبة من `none` إلى `inline`. وبعد أن يتم كل ذلك، ستظهر الصفحة `**Hello interactive students!**`.

* يتيح لنا جافا سكريبت أيضًا إمكانية إنشاء عناصر وتنسيقها وإضافتها إلى DOM وإزالتها منها. وفي الحقيقة، قد تكون الصفحة بأكملها من الناحية التقنية ملف جافا سكريبت كبيرًا ينشئ العناصر وينسقها واحدًا تلو الآخر - وقد يكون هذا عمليًا، ولكن يعد استخدام HTML وCSS أكثر سهولة عمليًا. في الجزء الثاني من وظيفة جافا سكريبت ننشئ عنصر div جديدًا، ونعين المحتوى النصي للعنصر، ثم نضيفه إلى النص الأساسي.

<img src="images/device-js-small.png"  alt="page preview" />

إلا أن هناك تنبيهًا هامًا يجب الإفصاح عنه بشأن الأداء. يوفر لنا جافا سكريبت إمكانيات كبيرة، إلا أنه ينشئ قيودًا إضافية كثيرًا على كيفية ووقت عرض الصفحة.

أولاً، لاحظ أنه في المثال الوارد أعلاه يقع النص البرمجي المضمَّن أسفل الصفحة. لماذا? يجب أن تجرب بنفسك، ولكن عند نقل النص البرمجي أعلى العنصر the *span*، ستلاحظ أن النص البرمجي يسقط ويتبين أنه لم يتمكن من العثور على مرجع إلى أية عناصر *span* في المستند - أي *getElementsByTagName('span')* will return *null*. يوضح ذلك خاصية مهمة وهي أنه تم تنفيذ النص البرمجي في النقطة المحددة حيث تم إدراجه في المستند. عندما يواجه محلل HTML علامة نص برمجي، فإنه يعطِّل العملية مؤقتًا حتى إنشاء DOM والحصول على التحكم في محرك جافا سكريبت، وبعد انتهاء تشغيل محرك جافا سكريبت، يبدأ المتصفح من جديد من حيث انتهى ويستكمل إنشاء DOM.

بمعنى آخر، لا يمكن لحزمة النص البرمجي العثور على أية عناصر لاحقًا في الصفحة نظرًا لأنه لم تتم معالجتها بعد. وملخص ذلك في ما يلي: **يحظر تنفيذ النص البرمجي المضمَّن إنشاء DOM، وقد يؤدي كذلك إلى تأخر العرض المبدئي.**

وهناك خاصية بسيطة أخرى لوضع النصوص البرمجية في الصفحة وهي أنه لا يمكنها فقط قراءة وتعديل DOM، بل يمكنها أيضًا قراءة وتعديل خصائص CSSOM. وفي الحقيقة، هذا تحديدًا ما نفعله في المثال عند تغيير خاصية العرض لعنصر span من none إلى inline. وما النتيجة النهائية؟ في النهاية تصبح لدينا حالة تعارض.

ماذا لو لم ينته المتصفح من تنزيل وتصميم CSSOM عندما نرغب في تشغيل النص البرمجي؟ الإجابة بسيطة وليست جيدة للأداء: **سيؤخر المتصفح تنفيذ النص البرمجي حتى الانتهاء من تنزيل CSSOM وإنشائه، وأثناء الانتظار يتم كذلك حظر إنشاء DOM.**

باختصار، يوفر جافا سكريبت العديد من الملحقات الجديدة بين تنفيذ DOM وCSSOM وجافا سكريبت، ويمكن أن يؤدي إلى تأخر كبير في سرعة معالجة المتصفح وعرضه للصفحة على الشاشة:

عند الحديث عن `تحسين مسار العرض الحرج`، فإننا نتحدث إلى حد كبير عن استيعاب وتحسين الرسم البياني للعلاقة بين HTML وCSS وجافا سكريبت.

* The location of the script in the document is significant.
* When the browser encounters a script tag, DOM construction pauses until the script finishes executing.
* JavaScript can query and modify the DOM and the CSSOM.
* JavaScript execution pauses until the CSSOM is ready.

افتراضيًا، يكون تنفيذ جافا سكريبت `حظرًا للمحلل`: فعندما يواجه المتصفح نصًا برمجيًا في المستند، يتعين عليه إيقاف إنشاء DOM مؤقتًا، وتسليم إمكانية التحكم إلى تشغيل جافا سكريبت والسماح بتنفيذ النص البرمجي قبل متابعة إنشاء DOM. وقد رأينا هذا عمليًا بالفعل من خلال نص برمجي مضمَّن في مثال سابق. وفي الحقيقة، تكون النصوص البرمجية المضمنة دائمًا وسيلة حظر ما لم تهتم جيدًا بالأمر وتعد شفرة إضافية لتأجيل تنفيذها.

## الفرق بين حظر المحلل وجافا سكريبت غير المتزامن

ماذا عن النصوص البرمجية المضمنة عبر علامة نص برمجي؟ دعونا نتناول المثال السابق ونستخلص الشفرة في ملف منفصل:

What about scripts included via a script tag? Let's take our previous example and extract the code into a separate file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/split_script.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**app.js**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/app.js" region_tag="full" adjust_indentation="auto" %}
</pre>

إلا أن هناك مخرجًا. افتراضيًا تحظر جميع أشكال جافا سكريبت المحلل ولا يعرف المتصفح ما يسعى النص البرمجي إلى تنفيذه في الصفحة، ولذلك يضطر إلى افتراض السيناريو الأسوأ ويحظر المحلل. ولكن ماذا لو تمكنا من ترك إشارة للمتصفح تخبره بأن النص البرمجي لا يحتاج إلى التنفيذ في النقطة المشار إليها في المستند؟ ويؤدي إجراء ذلك إلى السماح للمتصفح بمتابعة إنشاء DOM ويتيح للنص البرمجي التنفيذ بعد أن يصبح جاهزًا، على سبيل المثال، بعد جلب الملف من ذاكرة التخزين المؤقت أو من خادم بعيد.

إذًا، كيف نحقق ذلك؟ الأمر بسيط للغاية، حيث يمكننا وضع علامة على النص البرمجي باعتباره *async*:

وتساعد إضافة الكلمة الرئيسية غير المتزامنة إلى علامة النص البرمجي في إخبار المتصفح أنه يتعين عليه عدم حظر إنشاء DOM أثناء انتظار توفر النص البرمجي - وهذا مكسب كبير بالنسبة إلى الأداء.

To achieve this, we mark our script as *async*:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/split_script_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/split_script_async.html){: target="_blank" .external }

Adding the async keyword to the script tag tells the browser not to block DOM construction while it waits for the script to become available, which can significantly improve performance.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}