project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: يتم جمع شجرتي CSSOM وDOM في شجرة عرض ليتم استخدامها بعد ذلك في حساب تنسيق كل عنصر مرئي وعرضه كإدخال لعملية الرسم التي تعرض وحدات بكسل على الشاشة. ويعد تحسين كل خطوة من هذه الخطوات أمرًا لا بد منه لتحقيق مستوى الأداء المثالي للعرض.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# إنشاء شجرة العرض، والتنسيق والطباعة {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

يتم جمع شجرتي CSSOM وDOM في شجرة عرض ليتم استخدامها بعد ذلك في حساب تنسيق كل عنصر مرئي وعرضه كإدخال لعملية الرسم التي تعرض وحدات بكسل على الشاشة. ويعد تحسين كل خطوة من هذه الخطوات أمرًا لا بد منه لتحقيق مستوى الأداء المثالي للعرض.

في القسم السابق الذي يتناول إنشاء نموذج الهدف، صممنا شجرتي DOM وCSSOM بناءً على إدخال HTML وCSS. إلا أن كلاً منهما يعد هدفًا مستقلاً يحدد أوجهًا مختلفة من المستند: أحدها يصف المحتوى والآخر يصف قواعد التنسيق التي يجب تطبيقها على المستند. كيف يمكننا دمج الاثنين ومطالبة المتصفح بعرض وحدات بكسل على الشاشة؟

### TL;DR {: .hide-from-toc }

* يتم الجمع بين شجرتي DOM و CSSOM لتكوين شجرة العرض.
* لا تتضمن شجرة العرض سوى العقد المطلوبة لعرض الصفحة.
* يحسب التنسيق الموضع المحدد والحجم المطلوب لكل هدف.
* يعد الرسم هو الخطوة الأخيرة التي توفر شجرة العرض الأخيرة وتعرض وحدات بكسل على الشاشة.

الخطوة الأولى أن يدمج المتصفح كلاً من DOM وCSSOM في `شجرة عرض` تحدد جميع محتوى DOM المرئي على الصفحة، بالإضافة غلى معلومات نمط CSSOM لكل عقدة.

<img src="images/render-tree-construction.png" alt="يتم الجمع بين DOM وCSSOM لإنشاء شجرة العرض" />

ولإنشاء شجرة العرض، ينفذ المتصفح ما يلي تقريبًا:

1. Starting at the root of the DOM tree, traverse each visible node.
    
    * بعض العقد لا تكون مرئية تمامًا (مثل علامات النص البرمجي والعلامات الوصفية وهكذا)، ويتم إسقاطها نظرًا لأنها لا تظهر في النتيجة المعروضة.
    * يتم إخفاء بعض العقد باستخدام CSS، كما يتم إسقاطها من شجرة العرض - على سبيل المثال، تعد العقدة الواردة في المثال أعلاه مفقودة من شجرة العرض نظرًا لأن لدينا قاعدة واضحة تعين الخاصية `display: none` عليها.

2. For each visible node, find the appropriate matching CSSOM rules and apply them.

3. أرسل العقد المرئية التي تتضمن المحتوى والأنماط المحسوبة.

Note: كملاحظة جانبية بسيطة، لاحظ أن 'visibility: hidden' يختلف عن 'display: none'. الأول يجعل العنصر غير مرئي، ولكن يظل العنصر يشغل مساحة في التنسيق (أي يتم عرضه كمربع فارغ)، بينما يزيل الأخير (display: none) العنصر بالكامل من شجرة العرض بحيث يكون العنصر غير مرئي وليس جزءًا من التنسيق.

يعد الناتج النهائي عرضًا يتضمن معلومات المحتوى والنمط لجميع المحتوى المرئي على الشاشة - وبذلك نكون أوشكنا على الوصول. **عندما تكون شجرة العرض في مكانها، سنتمكن من المتابعة حتى مرحلة `التنسيق`.**

بذلك نكون قد حسبنا العقد التي يجب أن تكون مرئية وأنماطها المحسوبة، ولكننا لم نحسب موضعها وحجمها الدقيق ضمن [إطار عرض](/web/fundamentals/design-and-ux/responsive/) الجهاز - وهذه هي مرحلة `التنسيق` والمعروفة أيضًا باسم `إعادة التدفق`.

ولحساب الحجم والموضع الدقيق لكل هدف يبدأ المتصفح من جذر شجرة العرض ويجتازها لحساب هندسة كل هدف على الصفحة. وفي ما يلي مثال عملي على ذلك:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/nested.html" region_tag="full" adjust_indentation="auto" %}
</pre>

يتضمن النص الأساسي للصفحة الواردة أعلاه عنصري div مضمَّنين: الأول (الأساسي) يعين حجم الشاشة للعقدة على 50% من عرض الإطار، بينما الثاني الذي يتضمنه الأساسي يعين العرض على 50% من الأساسي - أي 25% من عرض الإطار.

The body of the above page contains two nested div's: the first (parent) div sets the display size of the node to 50% of the viewport width, and the second div\---contained by the parent\---sets its width to be 50% of its parent; that is, 25% of the viewport width.

<img src="images/layout-viewport.png" alt="Calculating layout information" />

أخيرًا وبعد أن تعرفنا على العقد المرئية وأنماطها المحسوبة وهندستها، يمكننا تجاوز هذه المعلومات والوصول إلى المرحلة الأخيرة وهي تحويل كل عقدة في شجرة العرض إلى وحدات بكسل فعلية على الشاشة - وتُعرف هذه الخطوة غالبًا باسم `الرسم` أو `التحويل`.

هل تابعت كل ذلك؟ تتطلب كل واحدة من هذه الخطوات قدرًا لا يُستهان به من العمل بواسطة المتصفح، وهو ما يعني أنه قد يستغرق في الغالب بعض الوقت. ولحسن الحظ، يمكن الاستفادة من Chrome DevTools في الحصول على فكرة عن جميع المراحل الثلاث الموضحة أعلاه. وفي ما يلي نفحص مرحلة التنسيق للمثال الأصلي `hello world`:

This can take some time because the browser has to do quite a bit of work. However, Chrome DevTools can provide some insight into all three of the stages described above. Let's examine the layout stage for our original "hello world" example:

<img src="images/layout-timeline.png" alt="Measuring layout in DevTools" />

* يتم تحديد إنشاء شجرة العرض وموضعها وحساب حجمها باستخدام حدث `التنسيق` في المخطط الزمني.
* وبعد اكتمال التنسيق، يصدر المتصفح حدث `إعداد الرسم` و`الرسم` الذي يحول شجرة العرض إلى وحدات بكسل فعلية على الشاشة.

وبعد أن يتم كل ذلك، تظهر الصفحة في النهاية ضمن إطار العرض. إنجاز كبير، أليس كذلك؟!

The page is finally visible in the viewport:

<img src="images/device-dom-small.png" alt="Rendered Hello World page" />

قد تظهر صفحة العرض التجريبي بسيطة جدًا، ولكنها تتطلب مجهودًا كبيرًا. هل تهتم بتخمين ما سيحدث إذا تم تعديل DOM أو CSSOM؟ سنحاول تكرار العملية نفسها مجددًا لحساب وحدات بكسل اللازمة لإعادة العرض على الشاشة.

1. معالجة ترميز HTML وتصميم شجرة DOM.
2. معالجة ترميز CSS وتصميم شجرة CSSOM.
3. الجمع بين DOM وCSSOM في شجرة عرض.
4. تنفيذ التنسيق على شجرة العرض لحساب هندسة كل عقدة.
5. رسم العقد الفردية على الشاشة.

**يعد تحسين مسار العرض الحرج عملية يتم خلالها الحد من إجمالي الوقت المطلوب لتنفيذ الخطوات من 1 إلى 5 بالترتيب الوارد أعلاه.** ويمكننا ذلك من عرض المحتوى على الشاشة في أسرع وقت ممكن، كما يقلل من الوقت اللازم بين تحديثات الشاشة بعد العرض الأول - أي الوصول إلى أعلى معدل تحديث للمحتوى التفاعلي.

***Optimizing the critical rendering path* is the process of minimizing the total amount of time spent performing steps 1 through 5 in the above sequence.** Doing so renders content to the screen as quickly as possible and also reduces the amount of time between screen updates after the initial render; that is, achieve higher refresh rates for interactive content.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}