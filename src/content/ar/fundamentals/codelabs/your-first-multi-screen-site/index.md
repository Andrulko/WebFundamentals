project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: أصبح بإمكان عدد هائل من الأجهزة الوصول إلى شبكة الإنترنت بدءًا من الهواتف ذات الشاشة الصغيرة وانتهاءً بأجهزة التلفزيون ذات الشاشة الكبيرة. ويمكنك التعرف على كيفية تصميم موقع ويب يعمل جيدًا على جميع هذه الأجهزة.

{# wf_updated_on: 2014-01-05 #} {# wf_published_on: 2013-12-31 #}

# أول موقع ويب تنشئه لأجهزة متعددة {: .page-title }

Caution: This article has not been updated in a while and may not reflect reality. Instead, check out the free [Responsive Web Design](https://www.udacity.com/course/responsive-web-design-fundamentals--ud893) course on Udacity.

{% include "web/_shared/contributors/paulkinlan.html" %}

<img src="images/finaloutput-2x.jpg" alt="many devices showing the final project" class="attempt-right" />

Creating multi-device experiences is not as hard as it might seem. In this guide, we will build a product landing page for the [CS256 Mobile Web Development course](https://www.udacity.com/course/mobile-web-development--cs256) that works well across different device types.

قد يبدو تصميم موقع الويب للعمل على عدة أجهزة مختلفة الإمكانيات تتميز بأحجام شاشة وطرق تفاعل مختلفة أمرًا مزعجًا، وقد يبدو للبعض أمرًا مستحيلاً في البداية.

ولكن في الحقيقة ليس من الصعب تصميم مواقع سريعة الاستجابة، وحتى نبين لك ذلك، يمكنك الاستعانة بهذا الدليل في الحصول على خطوات يمكنك اتباعها عند البدء. وقد قسمنا الدليل إلى خطوتين بسيطتين، هما:

1. تحديد البنية المعلوماتية (تُعرف اختصارًا بـ IA) وهيكل الصفحة، 2. إضافة عناصر التصميم اللازمة ليصبح الموقع سريع الاستجابة وحتى يبدو جيدًا على جميع الأجهزة.
2. Adding design elements to make it responsive and look good across all devices.

## إنشاء المحتوى والهيكل

يعد المحتوى أهم عناصر أي موقع على الويب. لذا يجب الاهتمام بالتصميم لصالح المحتوى وعدم الامتثال لإملاءات التصميم على المحتوى. نتناول خلال هذا الدليل المحتوى المطلوب وكيفية إنشاء هيكل للصفحة يتناسب مع المحتوى، كما نعرض الصفحة بتنسيق خطي بسيط يتناسب مع إطارات العرض الواسعة والضيقة.

### إنشاء هيكل الصفحة

في ما يلي الأشياء التي نحتاج إليها:

1. مساحة تصف المنتج الذي نقدمه وهو دورة `CS256: تصميم الويب للجوّال` على مستوى عالٍ
2. نموذج لجمع المعلومات من المستخدمين المهتمين بالمنتج
3. وصف ومقطع فيديو تفصيلي
4. صور المنتج في الواقع
5. جدول بيانات يتضمن معلومات تؤيد ما نقوله

#### إنشاء العنوان والنموذج

* حدد المحتوى المطلوب أولاً.
* صمم البنية المعلوماتية (IA) لإطارات العرض الواسعة والضيقة.
* أنشئ عرضًا هيكليًا للصفحة مع المحتوى ولكن بدون تنسيق.

كما توصلنا إلى بنية معلوماتية تقريبية وتنسيق لإطارات العرض الواسعة والضيقة على حد سواء.

<div class="attempt-left">
  <figure>
    <img src="images/narrowviewport.png" alt="Narrow Viewport IA">
    <figcaption>
      Narrow Viewport IA
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img src="images/wideviewport.png" alt="Wide Viewport IA">
    <figcaption>
      Wide Viewport IA
     </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

This can be converted easily into the rough sections of a skeleton page that we will use for the rest of this project.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addstructure.html" region_tag="structure" adjust_indentation="auto" %}
</pre>

يمكن تحويل ذلك بسهولة إلى أقسام تقريبية لصفحة الهيكل التي سنستخدمها في الجزء المتبقي من المشروع.

### TL;DR {: .hide-from-toc }

اكتمل الهيكل الأساسي للموقع. ونحن نعرف الأقسام التي نحتاج إليها والمحتوى المطلوب عرضه في هذه الأقسام والمكان الذي يمكن وضعه فيه ضمن البنية العامة للمعلومات. والآن يمكننا بدء تصميم موقع الويب.

Note: سيأتي التصميم في وقت لاحق

### إضافة محتوى إلى الصفحة

يعد كل من العنوان ونموذج إشعار الطلب المكوِّن الأهم للصفحة. ويجب أن يتمكن المستخدم من الاطلاع عليهما في الحال.

في العنوان، يمكنك إضافة نموذج نص يصف الدورة:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addheadline.html" region_tag="headline" adjust_indentation="auto" %}
</pre>

كما نحتاج إلى ملء النموذج. سيكون نموذجًا بسيطًا يجمع أسماء المستخدمين وأرقام هواتفهم والوقت المناسب لإعادة الاتصال بهم.

يجب أن تتضمن جميع النماذج عناوين وعلامات موضعية تسهل على المستخدمين التركيز على العناصر واستيعاب ما يجب أن تتضمنه ولمساعدة أدوات سهولة الوصول أيضًا في استيعاب بنية النموذج. لا تتوقف أهمية سمة الاسم على إرسال قيمة النموذج إلى الخادم، بل يمكن استخدامها أيضًا في توفير تلميحات مهمة للمتصفح حول كيفية ملء النموذج للمستخدم تلقائيًا.

سنضيف أنواعًا دلالية لنسهل على المستخدمين الوصول إلى المحتوى على أجهزة الجوّال بسرعة. على سبيل المثال، عند إدخال رقم هاتف، يجب ألا يظهر للمستخدم سوى لوحة اتصال فقط.

سيتضمن قسم الفيديو والمعلومات في المحتوى مزيدًا من التعمق. سيتضمن قائمة نقطية بميزات المنتجات، كما يتضمن علامة موضعية للفيديو تعرض المنتج أثناء عمله للمستخدم.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addform.html" region_tag="form" adjust_indentation="auto" %}
</pre>

يتم كثيرًا الاستعانة بمقاطع الفيديو لوصف المحتوى بطريقة أكثر تفاعلاً، كما يتم استخدامها كثيرًا لتقديم عرض توضيحي للمنتج أو الفكرة.

#### إنشاء قسم الفيديو والمعلومات

ويضمن لك اتباع أفضل الممارسات سهولة دمج الفيديو في موقعك على الويب:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section1" adjust_indentation="auto" %}
</pre>

قد تبدو المواقع التي لا تتضمن صورًا مملة بعض الشيء. وهناك نوعان من الصور:

يتضمن قسم الصور في صفحتنا مجموعة من صور المحتوى.

وتكمن أهمية صور المحتوى في إمكانية استخدامها لتوضيح المقصود من الصفحة. ويمكنك النظر إلى هذه الصور باعتبارها مقالات صحفية. وتعد الصور التي نستخدمها صور دروس المدرسين المشاركين في المشروع: كريس ويلسون وبيتر لوبيرز وسيان بينيت.

* إضافة سمة `علامات التحكم` لتسهيل مشاهدة الفيديو على المستخدمين.
* إضافة صورة `الملصق` لتوفير معاينة المحتوى للمستخدمين.
* إضافة عدة عناصر `<source>` بناءً على تنسيقات الفيديو المتوفرة.
* إضافة نص تراجع يسمح للمستخدمين بتنزيل الفيديو إذا لم يكن بإمكانهم تشغيله في النافذة.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addvideo.html" region_tag="video" adjust_indentation="auto" %}
</pre>

وقد تم تعيين الصور بحيث تظهر بعرض 100% من الشاشة. ويتناسب هذا مع الأجهزة ذات إطار العرض الضيق، بينما يكون أقل جودة مع إطار العرض الواسع (مثل جهاز سطح المكتب). وسنتناول ذلك في قسم التصميم سريع الاستجابة.

#### إنشاء قسم الصور

لا تتوفر لدى العديد من المستخدمين إمكانية الاطلاع على الصور وغالبًا ما يستخدمون تكنولوجيا مساعدة مثل قارئ الشاشة لتحليل البيانات التي تظهر على الصفحة وينقلون ذلك إلى المستخدم شفهيًا. ولذلك يجب التأكد من أن جميع صور المحتوى تتضمن علامة `alt` الوصفية التي يمكن لقارئ الشاشة قراءتها للمستخدم.

* صور المحتوى &mdash; هي صور تتناسب مع المستند ويتم استخدامها في توفير مزيد من المعلومات حول المحتوى.
* الصور الأسلوبية &mdash; هي صور يتم استخدامها لإضفاء طابع أفضل على الصور، وغالبًا ما تكون صور خلفية وأنماطًا وتدرجات. سنتناول ذلك في [المقالة التالية](#).

وعند إضافة العلامات `alt`، يجب التأكد من أن جميع نصوص alt مختصرة قدر الإمكان لوصف الصورة وصفًا شاملاً. على سبيل المثال، نستخدم التنسيق البسيط `الاسم: الدور` مع السمة في العرض التوضيحي لنوفر للمستخدم معلومات كافية يعرف من خلالها أن هذا القسم يتناول المؤلفين ومهامهم.

يتناول القسم الأخير جدولاً بسيطًا يُستخدم في عرض إحصاءات معينة حول المنتج.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addimages.html" region_tag="images" adjust_indentation="auto" %}
</pre>

ويجب عدم استخدام الجداول إلا مع البيانات المجدولة، مثل قوالب المعلومات.

تحتاج معظم مواقع الويب إلى تذييل صفحة لعرض محتوى مثل `البنود والشروط` و`إخلاء المسؤولية` وغير ذلك من المحتوى الذي لا يكون مستهدفًا وضعه في قائمة التنقل الرئيسية أو في منطقة المحتوى الرئيسي للصفحة.

وفي موقعنا على الويب، سنكتفي بوضع رابط إلى البنود والشروط وصفحة الاتصال وملفاتنا الشخصية على الشبكات الاجتماعية.

لقد أنشأنا مخططًا لموقع الويب وحددنا جميع العناصر الرئيسية لهيكل الموقع. كما تأكدنا من أن جميع المحتوى وثيق الصلة قد أصبح جاهزًا لتلبية احتياجات نشاطنا التجاري.

#### إضافة قسم البيانات المجدولة

The final section is a simple table that is used to show specific product stats about the product.

ستلاحظ أن الصفحة لا تبدو مطمئنة في الوقت الحالي؛ ولكن هناك غرض من ذلك. يعد المحتوى أهم عنصر من عناصر موقع الويب، ويجب أن نتأكد من أن بنية المعلومات المقدمة والكثافة تتميز بالثبات. حصلنا من خلال هذا الدليل على أساس ممتاز يمكن الاعتماد عليه. وسنهتم في الدليل التالي بوضع تنسيق المحتوى.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section3" adjust_indentation="auto" %}
</pre>

أصبح بإمكان عدد هائل من الأجهزة الوصول إلى شبكة الإنترنت بدءًا من الهواتف ذات الشاشة الصغيرة وانتهاءً بأجهزة التلفزيون ذات الشاشة الكبيرة. ويتضمن كل جهاز ميزاته وعيوبه الخاصة. وبصفتك أحد مطوِّري الويب، يجب أن تهتم بتصميم موقع يتوافق مع جميع أشكال الأجهزة.

#### إضافة تذييل صفحة

نحن بصدد تصميم موقع ويب يمكن استعراضه على أحجام شاشات مختلفة وأنواع أجهزة متنوعة. وفي [المقالة السابقة](#)، تمكنا من إعداد البنية المعلوماتية للصفحة وأنشأنا هيكلاً أساسيًا. وفي هذا الدليل سنتناول الهيكل الأساسي مع المحتوى ونحوله إلى صفحة رائعة تكون سريعة الاستجابة على عدد كبير من أحجام الشاشات.

وفقًا لمبادئ `تصميم ويب الجوّال لأول مرة`، سنبدأ باستخدام إطار العرض الضيق &mdash; كما هو الحال في الهاتف الجوّال &mdash; والتصميم لهذه التجربة أولاً. بعد ذلك سنضبط الحجم بحيث يتناسب مع فئات الأجهزة الأكبر. ويمكننا تنفيذ ذلك من خلال جعل إطار العرض أوسع واتخاذ قرار بشأن مدى ظهور التصميم والتنسيق على نحو سليم من عدمه.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="footer" adjust_indentation="auto" %}
</pre>

سبق أن أنشأنا مجموعة مختلفة من تصميمات المستوى العالي لطريقة ظهور المحتوى. والآن يلزمنا ضبط الصفحة بحيث تتناسب مع هذه التنسيقات المختلفة. وسنتمكن من تنفيذ ذلك من خلال اتخاذ قرار بشأن مكان نقاط الفصل &mdash; نقاط تغير التنسيق والأنماط &mdash; بناءً على مدى تناسب المحتوى مع حجم الشاشة.

### ملخص

حتى مع الصفحة الأساسية، **يجب** دائمًا تضمين علامة إطار عرض وصفية. ويعد إطار العرض المحتوى الأهم الذي يلزمك لتصميم تجارب تتناسب مع عدة أجهزة. وبدون إطار العرض، لن يعمل موقعك على نحو جيد في جهاز الجوّال.

<div class="attempt-left">
  <figure>
    <img src="images/content.png" alt="Content">
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-without-styles.html">Content and structure</a>
    </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img  src="images/narrowsite.png" alt="Designed site" style="max-width: 100%;">
    <figcaption>
      <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-with-styles.html">Final site</a>
    </figcaption>
  </figure>
</div>

يشير إطار العرض إلى المتصفح الذي تحتاج إليه الصفحة ليتم ضبط حجمها بحيث تتناسب مع الشاشة. وهناك العديد من التهيئات المختلفة التي يمكنك استخدامها في إطار العرض لضبط طريقة ظهور الصفحة. إلا أننا نوصي باستخدام الإعدادات الافتراضية التالية:

## تصميم موقع ويب سريع الاستجابة

يظهر إطار العرض في رأس المستند ولن نحتاج إلى الإعلان عنه سوى مرة واحدة فقط.

تتبع شركتنا في المنتج إرشادات علامة تجارية وخط محددة وموضحة في دليل الأسلوب.

ويعد دليل الأسلوب أداة مفيدة تقدم شرحًا وافيًا لمظهر الصفحة، كما يساعدك في ضمان الحصول على أسلوب ثابت على مستوى التصميم.

Earlier we created a couple of different high-level designs for how our content should be displayed. Now we need to make our page adapt to those different layouts. We do this by making a decision on where to place our breakpoints &mdash; a point where the layout and styles change &mdash; based on how the content fits the screen-size.

### TL;DR {: .hide-from-toc }

* استخدم إطار عرض دائمًا.
* استخدم إطار عرض ضيقًا في البداية ثم اهتم بضبط الحجم بعد ذلك.
* ضع نقاط فصل عند الحاجة إلى تكييف المحتوى.
* ضع رؤية عالية المستوى للتنسيق على مستوى نقاط الفصل الأساسية.

### إضافة منفذ العرض

أضفنا في الدليل السابق صورًا تُعرف باسم `صور المحتوى`. وتتمثل أهمية هذه الصور في سردها لقصة المنتج. ولا تكمن أهمية الصور الأسلوبية في أنها تمثل جزءًا من المحتوى الأساسي بل لأنها توفر عنصرًا مرئيًا أو دليلاً لتوجيه انتباه المستخدم إلى محتوى بعينه.

وهناك مثال معبر عن ذلك وهو صورة العنوان لمحتوى `الجزء المرئي من الصفحة`. هذا المحتوى يُستخدم غالبًا لإثارة المستخدم وتشجيعه على الاطلاع على مزيد من المعلومات حول المنتج.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/viewport.html" region_tag="viewport" adjust_indentation="auto" %}
</pre>

يمكن تضمينه ببساطة شديدة. وفي حالتنا هذه، سنستخدمه خلفية للعنوان ونضعه من خلال محتوى CSS بسيط.

لقد اخترنا صورة خلفية بسيطة تم تعتيمها حتى لا تؤثر في المحتوى وتم تعيينها بحيث `تغطي` العنصر تمامًا؛ وبذلك سيتم تمديدها دائمًا مع الحفاظ على نسبة العرض إلى الارتفاع المناسبة.

### تطبيق التصميم البسيط

يبدأ التصميم في التأثر بشكل سلبي عند استخدام العرض 600 بكسل. وفي حالتنا هذه، يتجاوز طول السطر 10 كلمات (الطول المثالي للقراءة) وهذا هو المكان الذي نريد تغييره.

#### دليل الأسلوب

يبدو أن القيمة 600 بكسل جيدة لإنشاء أول نقطة فصل لأنها ستمنحنا هدفًا لعناصر تغيير الموضع للوصول إلى وضع يتناسب مع الشاشة بشكل أفضل. ويمكننا تنفيذ ذلك باستخدام تقنية تُعرف باسم [طلبات بحث الوسائط](/web/fundamentals/design-and-ux/responsive/#use-css-media-queries-for-responsiveness).

#### إضافة الصور الأسلوبية

<div class="styles" style="font-family: monospace;">
  <div style="background-color: #39b1a4">#39b1a4</div>
  <div style="background-color: white">#ffffff</div>
  <div style="background-color: #f5f5f5">#f5f5f5</div>

  <div style="background-color: #e9e9e9">#e9e9e9</div>
  <div style="background-color: #dc4d38">#dc4d38</div>
</div>

#### تعويم عنصر النموذج

<img  src="images/narrowsite.png" alt="Designed site"  class="attempt-right" />

Note: لن تضطر إلى نقل جميع العناصر مرة واحدة، ويمكنك إجراء تعديلات صغيرة عند اللزوم.

في سياق صفحة المنتج، سيبدو أننا سنحتاج إلى ما يلي:

لقد اخترنا أن يكون لدينا تنسيقان أساسيان هما: إطار العرض الضيق وإطار العرض الواسع، حتى نسهل عملية التصميم إلى حد كبير.

<div style="clear:both;"></div>

    #headline {
      padding: 0.8em;
      color: white;
      font-family: Roboto, sans-serif;
      background-image: url(backgroundimage.jpg);
      background-size: cover;
    }
    

كما قررنا إنشاء أقسام بهوامش كاملة في إطار العرض الضيق لتظل بهوامش كاملة في إطار العرض الواسع. وهذا يعني أنه يتعين علينا تقييد الحد الأقصى لعرض الشاشة حتى لا يتم توسيع النص والفقرات إلى سطر واحد طويل على الشاشات شديدة الاتساع. لقد اخترنا أن تكون هذه النقطة 800 بكسل تقريبًا.

### تحديد نقطة الأولى

ولتحقيق ذلك، يلزمنا تقييد عرض العناصر ومركزها. ويجب إنشاء حاوية حول كل قسم كبير وتطبيق `هامش:
تلقائي`. وسيتيح هذا نمو الشاشة، ولكن سيظل المحتوى متركزًا وبحد أقصى للحجم لا يتجاوز 800 بكسل.

<video controls poster="images/firstbreakpoint.png" style="width: 100%;">
  <source src="videos/firstbreakpoint.mov" type="video/mov"></source>
  <source src="videos/firstbreakpoint.webm" type="video/webm"></source>
  <p>عذرًا، فمتصفحك لا يتوافق مع الفيديو.
     <a href="videos/firstbreakpoint.mov">تنزيل الفيديو</a>.
  </p>
</video>

وستكون الحاوية عنصر `div` بسيطًا في النموذج التالي:

    @media (min-width: 600px) {
    
    }
    

في إطار العرض الضيق، لا تتوفر لدينا مساحة كبيرة لعرض المحتوى، ولذلك يكون حجم الطباعة ووزنها في الغالب قد تم خفضه بنسبة كبيرة ليناسب الشاشة.

أما في إطار العرض الكبير، فيجب أن نضع في الاعتبار احتمال استخدام المستخدم لشاشة كبيرة ولكن بنسبة أعلى. ولزيادة إمكانية قراءة المحتوى، يمكننا تكبير حجم الطباعة ووزنها، كما يمكننا تغيير المساحة المتروكة لتمييز مناطق محددة على نحو أكبر.

في صفحة المنتج، سنزيد المساحة المتروكة لعناصر القسم من خلال تعيينها لتبقى عند القيمة 5% من العرض. كما سنزيد حجم رؤوس الصفحة لكل قسم من الأقسام.

* تقييد الحد الأقصى لعرض التصميم.
* تغيير ترك المساحة للعناصر وتقليل حجم النص.
* نقل النموذج لتعويمه بحيث يحاذي محتوى العنوان.
* تعويم الفيديو حول المحتوى.
* تقليل حجم الصور وإظهارها في شبكة لطيفة.

### تقييد الحد الأقصى لعرض التصميم

كان إطار العرض الضيق الذي استخدمناه شاشة خطية مكدسة. وقد تم عرض كل قسم ومحتوى رئيسي داخله بترتيب من أعلى لأسفل.

ويوفر لنا إطار العرض الواسع مساحة أكبر يمكن استخدامها لعرض المحتوى بالطريقة المثلى بالنسبة إلى الشاشة. وبالنسبة إلى صفحة المنتج، يعني هذا أنه وفقًا للبنية المعلوماتية يمكننا تنفيذ ما يلي:

يشير إطار العرض الضيق إلى أن لدينا المساحة الأفقية المتوفرة لدينا أقل لوضع العناصر على الشاشة بطريقة مريحة.

وللوصول إلى طريقة استخدام أكثر كفاءة لمساحة الشاشة الأفقية، يلزمنا تقسيم التدفق الخطي لرأس الصفحة ونقل النموذج والقائمة بحيث يصير كل منهما بجانب الآخر.

    <div class="container">
    </div>
    

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="containerhtml" adjust_indentation="auto" %}
</pre>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="container" adjust_indentation="auto" %}
</pre>

تم تصميم الفيديو في إطار العرض الضيق ليصبح العرض الكامل للشاشة ولوضعه بعد قائمة الميزات الرئيسية. على إطار العرض الواسع، سيتم ضبط حجم الفيديو ليصبح كبيرًا جدًا وليظهر على نحو غير سليم عند وضعه بجوار قائمة الميزات.

### تغيير المساحة المتروكة وتقليل حجم النص

يجب نقل عنصر الفيديو خارج التدفق العمودي لإطار العرض الضيق ويجب عرضه جنبًا إلى جنب مع القائمة النقطية للمحتوى على إطار عرض واسع.

يتم تعيين الصور في واجهة إطار العرض الضيق (أجهزة الجوال غالبًا) لتظهر بالعرض الكامل للشاشة وبشكل عمودي مكدس. ولن يتم ضبط حجم ذلك جيدًا على إطار العرض الواسع.

وحتى تظهر الصور على نحو سليم في إطار العرض الواسع، يتم تعيين حجمها على 30% من عرض الحاوية ويتم وضعها أفقيًا (وليس عموديًا في إطار العرض الضيق). سنضيف كذلك نصف قطر للحد وظلاً مربعًا لجعل الصور تبدو أكثر جاذبية.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/alterpadding.html){: target="_blank" .external }

### ضبط العناصر على إطار العرض الواسع

عند استخدام الصور، يجب وضع حجم إطار العرض وكثافة العرض في الاعتبار.

وقد تم تصميم شبكة الويب ليتم عرضها على الشاشات التي يبلغ مستوى عدد النقاط في البوصة بها 96 نقطة في البوصة. وبعد ظهور أجهزة الجوّال، شاهدنا زيادة كبيرة في كثافة البكسل على الشاشات، بغض النظر عن الشاشات من الفئة Retina على أجهزة الكمبيوتر المحمول. ولذلك تظهر الصور التي يتم ترميزها باستخدام 96 نقطة في البوصة على نحو غير مريح في الأجهزة مرتفعة نسبة DPI.

* نقل النموذج حول معلومات رأس الصفحة.
* وضع الفيديو يمين النقاط الرئيسية.
* ترتيب الصور.
* توسيع الجدول.

#### تعويم عنصر الفيديو

وقد توصلنا إلى حل لم يتم تطبيقه على نطاق واسع بعد. بالنسبة إلى المتصفحات التي تتوافق مع ذلك، يمكنك عرض الصور ذات الكثافة العالية على شاشة ذات كثافة عالية.

من الصعوبة إلى حد كبير ظهور الجداول على نحو جيد على الأجهزة التي تتضمن إطار عرض ضيقًا ويجب الاهتمام بها اهتمامًا خاصًا.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="formfloat" adjust_indentation="auto" %}
</pre>

ونوصي عند استخدام إطار عرض ضيق أن يكون الجدول في شكل صفين، مع عرض العنوان والخلايا في صف للحصول على الشكل العمودي.

<video controls poster="images/floatingform.png" style="width: 100%;">
  <source src="videos/floatingform.mov" type="video/mov"></source>
  <source src="videos/floatingform.webm" type="video/webm"></source>
  <p>عذرًا، فمتصفحك لا يتوافق مع الفيديو.
     <a href="videos/floatingform.mov">تنزيل الفيديو</a>.
  </p>
</video>

#### ترتيب الصور

في موقعنا، كان يتعين علينا إنشاء نقطة فصل إضافية لمحتوى الجدول فقط. وعند التصميم لجهاز جوّال أولاً، يكون من الصعب التراجع عن الأنماط المستخدمة، ولذلك يجب تقسيم جدول إطار العرض الضيق CSS من إطار العرض الواسع css. وبذلك نحصل على فصل واضح وثابت.

**تهانينا.** إذا وصلت إلى هذه الأسطر، فهذا يعني أنك أنشأت أول صفحة مقصودة بسيطة للمنتج يمكن عرضها على عدد كبير من الأجهزة، وكذلك عناصر النموذج وأحجام الشاشات.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding" adjust_indentation="auto" %}
</pre>

وعند اتباع الإرشادات التالية، سيكون بإمكانك وضع قدميك على بداية موفقة:

#### جعل الصور أكثر سرعة في الاستجابة لمستوى عدد النقاط في البوصة

<img src="images/imageswide.png" class="attempt-right" />

The images in the narrow viewport (mobile devices mostly) interface are set to be the full width of the screen and stacked vertically. This doesn't scale well on a wide viewport.

To make the images look correct on a wide viewport, they are scaled to 30% of the container width and laid out horizontally (rather than vertically in the narrow view). We will also add some border radius and box-shadow to make the images look more appealing.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="floatvideo" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/tiletheimages.html){: target="_blank" .external }

#### الجداول

When using images, take the size of the viewport and the density of the display into consideration.

The web was built for 96dpi screens. With the introduction of mobile devices, we have seen a huge increase in the pixel density of screens not to mention Retina class displays on laptops. As such, images that are encoded to 96dpi often look terrible on a hi-dpi device.

We have a solution that is not widely adopted yet. For browsers that support it, you can display a high density image on a high density display.

    <img src="photo.png" srcset="photo@2x.png 2x">
    

#### Tables

Tables are very hard to get right on devices that have a narrow viewport and need special consideration.

We recommend on a narrow viewport that you transform your table by changing each row into a block of key-value pairs (where the key is what was previously the column header, and the value is still the cell value). Fortunately, this isn't too difficult. First, annotate each `td` element with the corresponding heading as a data attribute. (This won't have any visible effect until we add some more CSS.)

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/updatingtablehtml.html" region_tag="table-tbody" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/updatingtablehtml.html){: target="_blank" .external }

Now we just need to add the CSS to hide the original `thead` and instead show the `data-th` labels using a `:before` pseudoelement. This will result in the multi-device experience seen in the following video.

<video controls poster="images/responsivetable.png" style="width: 100%;">
  <source src="videos/responsivetable.mov" type="video/mov"></source>
  <source src="videos/responsivetable.webm" type="video/webm"></source>
  <p>عذرًا، فمتصفحك لا يتوافق مع الفيديو.
     <a href="videos/responsivetable.mov">تنزيل الفيديو</a>.
  </p>
</video>

In our site, we had to create an extra breakpoint just for the table content. When you build for a mobile device first, it is harder to undo applied styles, so we must section off the narrow viewport table CSS from the wide viewport css. This gives us a clear and consistent break.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/content-with-styles.html" region_tag="table-css" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-with-styles.html){: target="_blank" .external }

## Wrapping up

Success: By the time you read this, you will have created your first simple product landing page that works across a large range of devices, form-factors, and screen sizes.

If you follow these guidelines, you will be off to a good start:

1. إنشاء بنية معلوماتية أساسية واستيعاب المحتوى قبل الترميز.
2. تعيين إطار عرض دائمًا.
3. إنشاء التجربة الأساسية حول مبدأ `الجوّال أولاً`.
4. بعد أن تكون لديك تجربة الجوّال، يمكنك زيادة عرض الشاشة حتى لا تظهر على نحو سليم، ومن ثم يمكنك تعيين نقطة فصل.
5. واصل التجربة والتعديل.