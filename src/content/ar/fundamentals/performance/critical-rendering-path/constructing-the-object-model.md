project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: حتى يتمكن المتصفح من عرض المحتوى على الشاشة يلزم إنشاء شجرتي DOM وCSSOM. ونتيجة لذلك، يلزم التأكد من توفير كل من HTML وCSS للمتصفح في أقرب وقت ممكن.

{# wf_updated_on: 2014-09-11 #} {# wf_published_on: 2014-03-31 #}

# إنشاء نموذج الهدف {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

حتى يتمكن المتصفح من عرض الصفحة يلزم إنشاء شجرتي DOM وCSSOM. ونتيجة لذلك، يلزم التأكد من توفير كل من HTML وCSS للمتصفح في أقرب وقت ممكن.

### TL;DR {: .hide-from-toc }

- وحدات بايت → الأحرف → الرموز المميزة → العقد → نموذج الهدف.
- يتم تحويل ترميز HTML إلى نموذج هدف مستند (DOM)، ويتم تحويل ترميز CSS إلى نموذج هدف CSS المعروف اختصارًا باسم CSSOM.
- يعد كل من DOM وCSSOM تركيبة بيانات مستقلة.
- يتيح لنا المخطط الزمني لـ Chrome DevTools تحديد وفحص تكاليف الإنشاء والمعالجة في DOM وCSSOM.

## نموذج هدف المستند (DOM)

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom.html" region_tag="full" adjust_indentation="auto" %}
</pre>

دعونا نبدأ بأبسط حالة ممكنة: صفحة HTML بسيطة تتضمن نصوصًا وصورة واحدة. ما الذي يحتاج إليه المتصفح لمعالجة هذه الصفحة البسيطة؟

Let’s start with the simplest possible case: a plain HTML page with some text and a single image. How does the browser process this page?

<img src="images/full-process.png" alt="شجرة DOM" />

1. **التحويل:** يقرأ المتصفح وحدات بايت الأساسية في HTML على المكتب أو الشبكة ويترجمها إلى أحرف مفردة بناءً على الترميز المحدد للملف (مثل UTF-8).
2. **إعداد الرموز المميزة:** يحول المتصفح سلاسل الأحرف إلى رموز مميزة يتم تحديدها بواسطة [المعيار W3C HTML5](http://www.w3.org/TR/html5/) - على سبيل المثال `<html>` و`<body>` وسلاسل أخرى داخل `أقواس معقوقة`. يتضمن كل رمز مميز معنى خاصًا ومجموعة من القواعد.
3. **تحليل البنية:** يتم تحويل الرموز الناتجة إلى `أهداف` تحدد الخصائص والقواعد.
4. **إنشاء DOM:** في النهاية، ونظرًا لأن ترميز HTML يحدد العلاقات بين العلامات المختلفة (يتم الجمع بين بعض العلامات داخل علامات)، يتم ربط الأهداف الناتجة في بنية شجرة بيانات تحدد أيضًا علاقات الأساس والفرع المحددة في الترميز الأصلي: يعد الهدف *HTML* أساسًا في *body* object، ويعد the *body* أساسًا في *paragraph* object، وهكذا.

<img src="images/dom-tree.png"  alt="DOM tree" />

**The final output of this entire process is the Document Object Model (DOM) of our simple page, which the browser uses for all further processing of the page.**

Every time the browser processes HTML markup, it goes through all of the steps above: convert bytes to characters, identify tokens, convert tokens to nodes, and build the DOM tree. This entire process can take some time, especially if we have a large amount of HTML to process.

<img src="images/dom-timeline.png"  alt="Tracing DOM construction in DevTools" />

إذا فتحت Chrome DevTools وسجلت مخططًا زمنيًا أثناء تحميل الصفحة، فيمكنك الاطلاع على الوقت الفعلي المستخدم في تنفيذ هذه الخطوة &mdash; وقد احتجنا في المثال أعلاه إلى 5 ملي ثانية تقريبًا لتحويل عدد من وحدات بايت في HTML إلى شجرة DOM. وإذا كانت الصفحة أكبر، كما هو الحال في معظم الصفحات، فقد تستغرق هذه العملية وقتًا أطول بشكل ملحوظ. ومن خلال الأقسام اللاحقة التي تتناول إنشاء رسوم متحركة بسيطة أن هذا قد يصبح بسهولة عقبة إذا كان يتعين على المتصفح معالجة قدر كبير من HTML.

بعد أن تصبح شجرة DOM جاهزة، هل تكون لدينا معلومات كافية لعرض الصفحة على الشاشة؟ ليس بعد. تحدد شجرة DOM الخصائص والعلاقات في ترميز المستند، إلا أنها لا تخبرنا بشيء عن الطريقة التي سيظهر بها العنصر عند عرضه. وهذه هي مسؤولية CSSOM، التي نتركها لما يلي.

على الرغم من أن المتصفح كان ينشئ DOM للصفحة البسيطة، فقد واجه علامة رابط في القسم الأساسي للمستند يشير إلى ورقة أنماط CSS خارجية: style.css. ومن خلال توقع أن هذا المورد سيحتاج إلى عرض الصفحة، فقد أرسل في الحال طلبًا إلى هذا المورد، جاءت نتيجته بالمحتوى التالي:

## نموذج هدف CSS المعروف اختصارًا باسم (CSSOM)

بالطبع كان يمكننا الإعلان عن الأنماط مباشرة داخل ترميز HTML (المضمَّن)، ولكن يتيح لنا الفصل بين CSS وHTML معاملة المحتوى والتصميم على نحو منفصل: يمكن للمصممين العمل على CSS، كما يمكن للمطوِّرين التركيز على HTML وهكذا.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/style.css" region_tag="full" adjust_indentation="auto" %}
</pre>

وكما هو الحال في HTML، يجب تحويل قواعد CSS المستلمة إلى شيء يستوعبه المتصفح ويعمل معه. ومن ثم فإننا مجددًا نكرر عملية شبيهة للغاية لما نفذناه في HTML:

As with HTML, we need to convert the received CSS rules into something that the browser can understand and work with. Hence, we repeat the HTML process, but for CSS instead of HTML:

<img src="images/cssom-construction.png"  alt="CSSOM construction steps" />

The CSS bytes are converted into characters, then tokens, then nodes, and finally they are linked into a tree structure known as the "CSS Object Model" (CSSOM):

<img src="images/cssom-tree.png"  alt="CSSOM tree" />

وحتى يصبح الأمر أكثر وضوحًا، يمكنك الاطلاع على شجرة CSSOM الواردة أعلاه. سيتضمن أي نص داخل العلامة *span* الموضوعة في عنصر النص الأساسي حجم خط يصل إلى 16 بكسل وبلون أحمر، ويتم تتالي توجيه حجم الخط لأسفل من النص الأساسي إلى امتداد. ولكن إذا كانت علامة الامتداد فرعًا عن فقرة علامة (p)، فلن يتم عرض المحتويات.

لاحظ كذلك أن الشجرة الواردة أعلاه ليست شجرة CSSOM الكاملة ولن تعرض سوى الأنماط التي قررنا استبدالها في ورقة الأنماط. يقدم كل متصفح مجموعة افتراضية من الأنماط تُعرف أيضًا باسم `أنماط وكيل المستخدم` -- وهي التي تظهر عندما لا نقدم أيًا من أنماطنا الخاصة -- بينما تحل أنماطنا محل هذه الإعدادات الافتراضية (على سبيل المثال [أنماط IE الافتراضية](http://www.iecss.com/)). إذا سبق لك فحص `الأنماط المحسوبة` في Chrome DevTools وتساءلت من أين تأتي جميع الأنماط، فالآن أصبح الأمر واضحًا.

إذا كنت مهتمًا بمعرفة المدة التي تستغرقها معالجة CSS، فيمكنك تسجيل مخطط زمني في DevTools والبحث عن حدث `إعادة حساب النمط`: على خلاف تحليل DOM، لا يعرض المخطط الزمني إدخال `Parse CSS` منفصلاً، وبدلاً من ذلك يحدد بنية شجرة التحليل وCSSOM، بالإضافة إلى الحساب المتكرر للأنماط المحسوبة ضمن هذا الحدث وحده.

To find out how long the CSS processing takes you can record a timeline in DevTools and look for "Recalculate Style" event: unlike DOM parsing, the timeline doesn’t show a separate "Parse CSS" entry, and instead captures parsing and CSSOM tree construction, plus the recursive calculation of computed styles under this one event.

<img src="images/cssom-timeline.png"  alt="Tracing CSSOM construction in DevTools" />

Our trivial stylesheet takes ~0.6ms to process and affects eight elements on the page&mdash;not much, but once again, not free. However, where did the eight elements come from? The CSSOM and DOM are independent data structures! Turns out, the browser is hiding an important step. Next, lets talk about the [render tree](/web/fundamentals/performance/critical-rendering-path/render-tree-construction) that links the DOM and CSSOM together.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}