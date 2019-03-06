project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: يعد أسلوب الطباعة أمرًا أساسيًا للحصول على تصميم رائع وكذلك علامة تجارية وسهولة قراءة ووصول. تمكِّن خطوط الويب جميع ما سبق وأكثر أيضًا: يكون النص قابلاً للتحديد والبحث والتكبير/التصغير ومناسبًا للعرض مع نسبة DPI مرتفعة، مما يوفر عرضًا متناسقًا وحادًا للنص بغض النظر عن حجم الشاشة والدقة. تمثل خطوط الويب أهمية كبيرة للحصول على تصميم جيد، وانطباع مستخدم وأداء رائع.

{# wf_updated_on: 2014-09-29 #} {# wf_published_on: 2014-09-19 #}

# تحسين خط الويب {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*This article contains contributions from [Monica Dinculescu](https://meowni.ca/posts/web-fonts/), [Rob Dodson](/web/updates/2016/02/font-display), and Jeff Posnick.*

يعد تحسين خط الويب عنصرًا ضروريًا من إستراتيجية الأداء بشكل عام. ويعد كل خط موردًا إضافيًا، وقد تحظر بعض الخطوط عرض النص، إلا أن استخدام الصفحة لخطوط ويب لا يعني أن العرض سيكون بطيئًا. على العكس، يمكن أن يساعد الخط المحسن عند جمعه بإستراتيجية حكيمة لطريقة التحميل والاستخدام على الصفحة، في خفض إجمالي حجم الصفحة وتحسين مرات عرض الصفحة.

يعد خط الويب مجموعة من صور النقش، وكل صورة منقوشة تمثل شكلاً متجهيًا يصف حرفًا أو رمزًا. ونتيجة لذلك، يتم تحديد حجم ملف الخط باستخدام متغيرين بسيطين: مدى تعقيد المسارات المتجهية لكل صورة نقش وعدد الصور المنقوشة في خط معين. على سبيل المثال، يعد Open Sans أحد أكثر خطوط الويب انتشارًا، وهو يتضمن 897 صورة منقوشة من بينها أحرف لاتينية ويونانية وسيريلية.

## تشريح خط الويب

### TL;DR {: .hide-from-toc }

* يمكن أن تتضمن خطوط الترميز الموحد آلاف رموز الصور المنقوشة
* 'يتوفر أربعة تنسيقات للخط: WOFF2 وWOFF وEOT وTTF'
* تتطلب بعض تنسيقات الخط استخدام ضغط GZIP

A *webfont* is a collection of glyphs, and each glyph is a vector shape that describes a letter or symbol. As a result, two simple variables determine the size of a particular font file: the complexity of the vector paths of each glyph and the number of glyphs in a particular font. For example, Open Sans, which is one of the most popular webfonts, contains 897 glyphs, which include Latin, Greek, and Cyrillic characters.

<img src="images/glyphs.png"  alt="Font glyph table" />

ويتطلب استخدام الخطوط على الويب بشكل واضح بعض الهندسة الواعية لضمان عدم اعتراض أسلوب الطباعة طريق الأداء. ولحسن الحظ توفر منصة الويب جميع البدائيات اللازمة وفي الجزء المتبقي من هذا الدليل سنلقي نظرة عملية على كيفية تحقيق ميزات الأمرين جميعًا.

تتوفر الآن تنسيقات حاوية لأربعة خطوط للاستخدام على الويب: وهي كما يلي: [EOT](http://en.wikipedia.org/wiki/Embedded_OpenType) و[TTF](http://en.wikipedia.org/wiki/TrueType) و[WOFF](http://en.wikipedia.org/wiki/Web_Open_Font_Format) و[WOFF2](http://www.w3.org/TR/WOFF2/). ولكن لسوء الحظ، تتنوع الاختيارات ولا يتوفر تنسيق عام واحد يعمل على جميع المتصفحات القديمة والحديثة: ذلك أن EOT يتوافق مع [IE فقط](http://caniuse.com/#feat=eot)، وTTF [يتوافق جزئيًا مع IE](http://caniuse.com/#search=ttf)، بينما يتمتع WOFF بإمكانية توافق واسعة ولكنه [ليس متاحًا مع بعض المتصفحات القديمة](http://caniuse.com/#feat=woff)، أما WOFF 2.0 فهو [بصدد التوافق مع العديد من المتصفحات](http://caniuse.com/#feat=woff2).

### تنسيقات خط الويب

فما الذي نستنتجه من ذلك كله؟ ليس هناك تنسيق واحد يعمل على جميع المتصفحات، أي أننا نحتاج إلى تقديم عدة تنسيقات لتوفير انطباع متناسق:

Note: على المستوى الفني، هناك أيضًا [حاوية خطوط SVG](http://caniuse.com/svg-fonts)، إلا أنه لم يسبق أن توفرت مع متصفح IE أو Firefox ويتم حاليًا التخلي عنها في Chrome. ولذلك، يتم استخدامها على نطاق محدود، كما أننا نعتزم إسقاطها من هذا الدليل.

* اعرض متغير WOFF 2.0 على المتصفحات التي تتوافق معه
* اعرض متغير WOFF على معظم المتصفحات
* اعرض متغير TTF على متصفحات Android القديمة (أدنى من 4.4)
* اعرض متغير EOT على متصفحات IE القديمة (أدنى من IE9)

يمثل الخط مجموعة من الصور المنقوشة، يعد كل منها مجموعة من المسارات التي تصف شكل الحرف. وبالطبع تختلف الصور المنقوشة منفردة، ولكنها تتضمن العديد من المعلومات المتماثلة التي يمكن ضغطها باستخدام GZIP أو أية أداة ضغط متوافقة:

### خفض حجم الخط باستخدام الضغط

ومن الجدير بالذكر في النهاية أن بعض تنسيقات الخطوط تتضمن بيانات وصفية إضافية، مثل معلومات [تلميحات الخط](http://en.wikipedia.org/wiki/Font_hinting) و[تقنين الأحرف](http://en.wikipedia.org/wiki/Kerning) التي قد لا تكون ضرورية على بعض أنظمة التشغيل، مما يتيح إجراء مزيد من التحسين لحجم الخط. يمكنك الاستعانة بأداة ضغط الخطوط لمعرفة خيارات التحسين المتاحة، وإذا اتخذت هذا المسلك، فتأكد من أن لديك البنية الأساسية المناسبة لاختبار الخطوط المحسنة هذه وعرضها على كل متصفح - على سبيل المثال، يحافظ Google Fonts على 30 أو أكثر من المتغيرات المحسنة لكل خط ويكتشف المتغير المثالي ويعرضه تلقائيًا لكل نظام تشغيل ومتصفح.

* لا يتم ضغط تنسيقات EOT وTTF افتراضيًا: تأكد من أن الخوادم مهيأة لتطبيق [ضغط GZIP](/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text-compression-with-gzip) عند عرض هذه التنسيقات.
* يتضمن WOFF ضغطًا مضمَّنًا - فتأكد من أن أداة ضغط WOFF تستخدم إعدادات الضغط المثالية.
* يستخدم WOFF2 المعالجة المسبقة المخصصة وخوارزميات الضغط لتوفير نسبة انخفاض في حجم الملف تصل إلى 30% تقريبًا مقارنة بالتنسيقات الأخرى - راجع [هذا التقرير](http://www.w3.org/TR/WOFF20ER/).

Note: جرب استخدام [ضغط Zopfli](http://en.wikipedia.org/wiki/Zopfli) مع تنسيقات EOT وTTF وWOFF. تعد Zopfli أداة ضغط متوافقة مع zlib توفر نسبة انخفاض في حجم الملف تصل إلى 5% تقريبًا زيادة عن gzip.

تتيح لنا قاعدة @font-face في CSS تحديد موضع مورد خط معين، وميزات نمطه والتمثيل البرمجي للترميز الموحد الذي يجب استخدامه معها. ويمكن الاستفادة من الجمع بين إعلانات @font-face هذه في إنشاء `مجموعة خطوط` يستخدمها المتصفح لتقييم موارد الخط التي يجب تنزيلها وتطبيقها على الصفحة الحالية. والآن يمكننا إلقاء نظرة عن قرب على آلية عمل ذلك بالتفصيل.

## تحديد مجموعة الخطوط باستخدام @font-face

### TL;DR {: .hide-from-toc }

* استخدم التلميح format() لتحديد عدة تنسيقات خط
* 'قلل حجم خطوط الترميز الموحد الكبيرة لتحسين الأداء: استخدم تقليل الحجم لنطاق الترميز الموحد وقدم رجوعًا يدويًا عن تقليل الحجم في المتصفحات القديمة'
* قلل عدد متغيرات الخط الأسلوبي لتحسين أداء عرض الصفحة والنص

يوفر كل إعلان @font-face اسم مجموعة الخطوط التي تمثل مجموعة منطقية لإعلانات متعددة، [خصائص الخط](http://www.w3.org/TR/css3-fonts/#font-prop-desc) مثل النمط والوزن والاتساع و[واصف src](http://www.w3.org/TR/css3-fonts/#src-desc) الذي يحدد قائمة المواضع ذات الأولوية لمورد الخط.

### تحديد التنسيق

أولاً، لاحظ أن الأمثلة الواردة أعلاه تحدد مجموعة *Awesome Font* واحدة بنمطين اثنين (normal and *italic*)، يشير كل واحد منهما إلى مجموعة مختلفة من موارد الخطوط. وفي المقابل، يتضمن كل واصف `src` قائمة مفصولة بفواصل ذات أولوية تضم متغيرات المورد:

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome.woff2') format('woff2'), 
           url('/fonts/awesome.woff') format('woff'),
           url('/fonts/awesome.ttf') format('ttf'),
           url('/fonts/awesome.eot') format('eot');
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: italic;
      font-weight: 400;
      src: local('Awesome Font Italic'),
           url('/fonts/awesome-i.woff2') format('woff2'), 
           url('/fonts/awesome-i.woff') format('woff'),
           url('/fonts/awesome-i.ttf') format('ttf'),
           url('/fonts/awesome-i.eot') format('eot');
    }
    

Note: ما لم تكن الإشارة إلى أحد خطوط النظام الافتراضية، يكون من النادر عمليًا أن تكون مثبتة محليًا لدى المستخدم، خاصة على الجوّال، حيث يستحيل فعليًا `تثبيت` خطوط إضافية. ونتيجة لذلك، يجب دائمًا توفير قائمة بمواقع الخطوط الخارجية.

* يتيح التوجيه `local()` لنا الإشارة إلى الخطوط المثبتة وتحميلها واستخدامها محليًا.
* يتيح لنا التوجيه `url()` تحميل الخطوط الخارجية، ويُسمح فيه بالاحتواء على تلميح `format()` اختياري يشير إلى تنسيق الخط المشار إليه في عنوان URL المقدم.

عندما يشير المتصفح إلى أن الخط لازم، فإنه يكرر النظر إلى قائمة الموارد المقدمة بالترتيب المحدد ويحاول تحميل المورد المناسب. على سبيل المثال، بالنسبة إلى المثال الوارد أعلاه:

يتيح لنا الجمع بين التوجيهات المحلية والخارجية مع تلميحات التنسيق المناسب تحديد جميع تنسيقات الخط المتاحة ويتيح للمتصفح التعامل مع الباقي: يكتشف المتصفح الموارد المطلوبة ويحدد التنسيق المثالي نيابة عنا.

1. ينفذ المتصفح تنسيق الصفحة ويحدد متغيرات الخط المطلوبة لعرض نص محدد على الصفحة.
2. مع كل خط مطلوب، يفحص المتصفح ما إذا كان الخط متاحًا محليًا أم لا.
3. إذا لم يكن الملف متاحًا محليًا، فيتم تكرار النظر في التعريفات الخارجية: 
    * إذا كان تلميح التنسيق متوفرًا، فسيفحص المتصفح ما إذا كان يتوافق معه قبل بدء التنزيل، وإلا فسيتم التقدم نحو التالي له.
    * إذا لم يكن هناك تلميح تنسيق، فسينزل المتصفح المورد.

Note: من الضروري النظر إلى ترتيب وضع متغيرات الخط. وسيحدد المتصفح التنسيق الأول الذي يتوافق معه. ولذلك، إذا كنت تريد من المتصفحات الجديدة استخدام WOFF2، فيجب وضع إعلان WOFF2 فوق WOFF، وهكذا.

بالإضافة إلى خصائص الخط، مثل النمط والوزن والاتساع، تتيح لنا قاعدة @font-face تحديد مجموعة من التمثيل البرمجي للترميز الموحد المتوافقة مع كل مورد. وهذا يمكننا من تقسيم خط الترميز الموحد الكبير إلى مجموعات فرعية أصغر (مثل المجموعات الفرعية اللاتينية واليونانية والسيريلية) ولا يتم تنزيل شيء خلاف الصور المنقوشة المطلوب لعرض النص على صفحة بعينها.

### تقليل الحجم لنطاق الترميز الموحد

يتيح لنا [واصف نطاق الترميز الموحد](http://www.w3.org/TR/css3-fonts/#descdef-unicode-range) تحديد قائمة مفصولة بفواصل من قيم النطاق، يمكن أن تكون كل قيمة منها بأحد أشكال ثلاثة مختلفة:

على سبيل المثال، يمكننا تقسيم مجموعة *Awesome Font* إلى مجموعات فرعية لاتينية ويابانية، كل منها يتم تنزيله بواسطة المتصفح عند اللزوم:

* التمثيل البرمجي المفرد (على سبيل المثال U+416)
* نطاق المهلة الزمنية (على سبيل المثال U+400-4ff): يشير إلى التمثيل البرمجي لبداية النطاق ونهايته
* نطاق حرف البدل (على سبيل المثال U+4??): تشير الرموز `?` إلى أي رقم ست عشري

Note: يمثل تقليل الحجم لنطاق الترميز الموحد أهمية خاصة مع اللغات الأسيوية، حيث يتجاوز عدد الصور المنقوشة العدد في اللغات الغربية، ويتم قياس الخط `الكامل` العادي غالبًا بالميغابايت، بدلاً من عشرات الكيلوبايت.

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-jp.woff2') format('woff2'), 
           url('/fonts/awesome-jp.woff') format('woff'),
           url('/fonts/awesome-jp.ttf') format('ttf'),
           url('/fonts/awesome-jp.eot') format('eot');
      unicode-range: U+3000-9FFF, U+ff??; /* Japanese glyphs */
    }
    

يتيح لنا استخدام المجموعات الفرعية لنطاق الترميز الموحد والملفات المنفصلة لكل متغير أسلوبي في الخط تحديد مجموعة خطوط مركبة تكون أسرع وأكثر كفاءة في التنزيل - ولن يحتاج الزائر سوى تنزيل المتغيرات والمجموعات الفرعية اللازمة، ولا يتم إجباره على تنزيل مجموعات فرعية قد لا تظهر له على الصفحة وقد لا يستخدمها.

إلا أن هناك مشكلة بسيطة في نطاق الترميز الموحد: [غير متوافق مع جميع المتصفحات](http://caniuse.com/#feat=font-unicode-range) حتى الآن. تتجاهل بعض المتصفحات بسهولة تلميح نطاق الترميز الموحد وتنزل جميع المتغيرات، وقد لا تعالج المتصفحات الأخرى إعلان @font-face على الإطلاق. ولحل هذه المشكلة، يجب الرجوع إلى "تقليل الحجم اليدوي" للتوافق مع المتصفحات القديمة.

ونظرًا لأن المتصفحات القديمة ليست ذكية بالقدر الكافي لتحديد المجموعات الفرعية اللازمة ولا يمكنها إنشاء خط مركب، فيجب الرجوع إلى توفير مورد خط واحد يتضمن جميع المجموعات الفرعية اللازمة ويخفي البقية من المتصفح. على سبيل المثال، إذا كانت الصفحة لا تستخدم سوى أحرف لاتينية، فيمكننا نزع الصور المنقوشة الأخرى وعرض المجموعة الفرعية المحددة كمورد منفرد بذاته.

تتألف كل مجموعة خطوط من عدة متغيرات أسلوبية (عادي وعريض ومائل)، وعدة أوزان لكل نمط، وكل بدوره قد يتضمن أشكالاً مختلفة تمامًا من أشكال الصور المنقوشة - مثل المسافات والأحجام المختلفة والأشكال المختلفة تمامًا.

1. **كيف يمكننا تحديد المجموعات الفرعية اللازمة؟** 
    * إذا كان تقليل حجم نطاق الترميز الموحد يتوافق مع المتصفح، فسيتم تلقائيًا تحديد المجموعة الفرعية المناسبة. ولن تحتاج الصفحة إلا إلى توفير ملفات المجموعة الفرعية وتحديد نطاقات الترميز الموحد المناسبة في قواعد @font-face.
    * إذا لم يكن نطاق الترميز الموحد متوافقًا، فستحتاج الصفحة إلى إخفاء جميع المجموعات الفرعية غير اللازمة - أي يجب على المطوِّر تحديد المجموعات الفرعية المطلوبة.
2. **كيف يمكننا إنشاء مجموعات فرعية من الخطوط؟** 
    * يمكنك استخدام [أداة pyftsubset](https://github.com/behdad/fonttools/blob/master/Lib/fontTools/subset.py#L16) مفتوحة المصدر لتقليل حجم الخطوط وتحسينها.
    * تتيح بعض خدمات الخطوط تقليل الحجم يدويًا عبر معلمات طلب مخصصة يمكنك استخدامها لتحديد المجموعة الفرعية المطلوبة يدويًا لصفحتك - ويمكنك الاطلاع على المستندات الخاصة بموفر الخطوط.

### تحديد الخطوط وتركيبها

Each font family is composed of multiple stylistic variants (regular, bold, italic) and multiple weights for each style, each of which, in turn, may contain very different glyph shapes&mdash;for example, different spacing, sizing, or a different shape altogether.

<img src="images/font-weights.png"  alt="Font weights" />

يسري منطق شبيه على متغيرات *italic*. يتحكم مصمم الخطوط في المتغيرات التي سينتجها، ونحن نتحكم في المتغيرات التي سنستخدمها على الصفحة - ونظرًا لأن كل متغير يكون بتنزيل منفصل، من المفيد الحفاظ على عدد صغير للمتغيرات. على سبيل المثال، يمكننا تحديد متغيرين عريضين لمجموعة *Awesome Font*:

> When a weight is specified for which no face exists, a face with a nearby weight is used. In general, bold weights map to faces with heavier weights and light weights map to faces with lighter weights.
> 
> > [CSS3 font matching algorithm](http://www.w3.org/TR/css3-fonts/#font-matching-algorithm)

يوضح المثال الوارد أعلاه مجموعة *Awesome Font* المؤلفة من موردين وتغطي مجموعة الصور المنقوشة اللاتينية نفسها (U+000-5FF) ولكنه يعرض `وزنين` مختلفين: عادي (400)، وغامق (700). ولكن، ماذا يحدث إذا حددت قاعدة من بين قواعد CSS وزن خط مختلفًا، أو عينت خاصية نمط الخط على مائل؟

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'), 
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('ttf'),
           url('/fonts/awesome-l.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 700;
      src: local('Awesome Font'),
           url('/fonts/awesome-l-700.woff2') format('woff2'), 
           url('/fonts/awesome-l-700.woff') format('woff'),
           url('/fonts/awesome-l-700.ttf') format('ttf'),
           url('/fonts/awesome-l-700.eot') format('eot');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    

The above example declares the *Awesome Font* family that is composed of two resources that cover the same set of Latin glyphs (`U+000-5FF`) but offer two different "weights": normal (400) and bold (700). However, what happens if one of your CSS rules specifies a different font weight, or sets the font-style property to italic?

* إذا لم يكن هناك تطابق تام للخط متوفر، فسيستبدله المتصفح بالتطابق الأقرب.
* إذا لم يتم العثور على تطابق أسلوبي (على سبيل المثال عدم الإعلان عن أية متغيرات مائلة في المثال أعلاه)، فسيركب المتصفح متغير الخط بنفسه.

<img src="images/font-synthesis.png"  alt="Font synthesis" />

Note: للحصول على أفضل نسبة ثبات ونتائج ظاهرة يجب عدم الاعتماد على تركيب الخطوط. بدلاً من ذلك، استخدم أقل عدد من متغيرات الخطوط وحدد مواقعها، وبذلك يتمكن المتصفح من تنزيلها عند استخدامها على الصفحة. وبذلك، في بعض الحالات، [قد يكون المتغير المركب خيارًا مقبولاً](https://www.igvita.com/2014/09/16/optimizing-webfont-selection-and-synthesis/) - ومن ثم توخ الحذر.

يمكن أن يتسبب خط الويب `الكامل` الذي يتضمن جميع المتغيرات الأسلوبية غير اللازمة بالإضافة إلى صور منقوشة لا فائدة منها في تنزيل عدد من وحدات ميغابايت. ولحل هذه المشكلة، تم تصميم قاعدة @font-face في CSS خصيصًا لتسمح لنا بتقسيم مجموعة الخطوط إلى مجموعة من الموارد: المجموعات الفرعية للترميز الموحد، ومتغيرات النمط المتميزة، وهكذا.

وبالنظر إلى هذه الإعلانات، يكتشف المتصفح المجموعة الفرعية المطلوبة والمتغيرات وينزل الحد الأدنى من المجموعة المطلوبة لعرض النص. ويعد هذا السلوك من السهولة بمكان، إلا أن عدم الحرص في إجرائه قد ينشئ عقبة في الأداء داخل مسار العرض الحرج وقد يتسبب في تأخر عرض النص - وهو شيء لا يرغب فيه أحد أبدًا.

## تحسين التحميل والعرض

### TL;DR {: .hide-from-toc }

* يتم تأخير طلبات الخط حتى يتم إنشاء شجرة العرض، وقد يؤدي هذا إلى تأخر في عرض النص
* تتيح واجهة برمجة التطبيقات Font Loading لنا تطبيق تحميل خطوط مخصص وإستراتيجيات عرض تحل محل تحميل الخط الكسول والافتراضي

يشير التحميل الكسول للخطوط إلى شيء مخفي وهام قد يؤخر عرض النص: يجب على المتصفح [إنشاء شجرة عرض](/web/fundamentals/performance/critical-rendering-path/render-tree-construction)، تعتمد على شجرتي DOM وCSSOM قبل تحديد موارد الخط اللازمة لعرض النص. ونتيجة لذلك، يتم تأخير طلبات الخط فترة طويلة بعد الموارد الحرجة الأخرى، وقد يتم حظر المتصفح من عرض النص حتى يتم جلب المورد.

Given these declarations, the browser figures out the required subsets and variants and downloads the minimal set required to render the text, which is very convenient. However, if you're not careful, it can also create a performance bottleneck in the critical rendering path and delay text rendering.

### خطوط الويب ومسار العرض الحرج

يؤدي `السباق` بين الرسم الأول لمحتوى الصفحة، الذي قد يتم بعد وقت قصير من تصميم شجرة العرض، وطلب مورد الخط إلى حدوث `مشكلة نص فارغ` حيث يمكن للمتصفح عرض تنسيق الصفحة ولكن يسقط أي نص. ويختلف السلوك الفعلي باختلاف المتصفح:

<img src="images/font-crp.png"  alt="Font critical rendering path" />

1. يطلب المتصفح مستند HTML
2. يبدأ المتصفح تحليل استجابة HTML وإنشاء DOM
3. يكتشف المتصفح CSS وجافا سكريبت وموارد أخرى ويرسل الطلبات
4. ينشئ المتصفح CSSOM بعد تلقي جميع محتوى CSS ويجمع بينه وبين شجرة DOM لإنشاء شجرة العرض 
    * يتم إرسال طلبات الخط بمجرد توضيح شجرة العرض لمتغيرات الخط اللازمة لعرض النص المحدد على الصفحة
5. ينفذ المتصفح التنسيق ويرسم المحتوى على الصفحة 
    * إذا لم يكن الخط متوفرًا بعد، فقد لا يعرض المتصفح أية وحدات بكسل للنص
    * بعد أن يتوفر الخط، يرسم المتصفح وحدات بكسل للنص

توفر [واجهة برمجة تطبيقات Font Loading](http://dev.w3.org/csswg/css-font-loading/) واجهة إعداد نصوص برمجية لتحديد وجوه خطوط CSS ومعالجتها، وتتبع مدى تقدم التنزيل، واستبدال سلوك التحميل الكسول الافتراضي. على سبيل المثال، إذا كنا على يقين من أن أحد متغيرات خط ما سيكون مطلوبًا، فيمكننا تحديده ومطالبة المتصفح ببدء جلب في الحال لمورد الخط:

علاوة على ذلك، ونظرًا لأنه يمكننا التحقق من طريقة فحص حالة الخط (عبر [check()](http://dev.w3.org/csswg/css-font-loading/#font-face-set-check)) وتتبع مدى تقدم التنزيل، يمكننا أيضًا تحديد إستراتيجية مخصصة لعرض النص على صفحاتنا:

### تحسين عرض الخط باستخدام واجهة برمجة تطبيقات Font Loading

والأهم من كل ذلك أنه يمكننا أيضًا المزج والربط بين الإستراتيجيات الواردة أعلاه للمحتوى المختلف على الصفحة - على سبيل المثال تعطيل عرض النص على بعض الأقسام حتى يتوفر الخط، واستخدام رجوع وإعادة العرض بعد انتهاء تنزيل الخط، وتحديد مهلات زمنية مختلفة، وهكذا.

Note: لا تزال واجهة برمجة تطبيقات Font Loading [تحت الإنشاء في بعض المتصفحات](http://caniuse.com/#feat=font-loading). جرب استخدام [FontLoader polyfill](https://github.com/bramstein/fontloader)، أو [webfontloader library](https://github.com/typekit/webfontloader)، لتوفير وظائف شبيهة، ولو مع تكلفة علاقة جافا سكريبت إضافية.

هناك إستراتيجية بديلة وبسيطة يمكن استخدام واجهة برمجة التطبيقات Font Loading فيها للحد من `مشكلة النص الفارغ` وهي تضمين محتويات الخط في ورقة أنماط CSS:

```html
var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
  style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
});

font.load(); // don't wait for render tree, initiate immediate fetch!

font.ready().then(function() {
  // apply the font (which may rerender text and cause a page reflow)
  // once the font has finished downloading
  document.fonts.add(font);
  document.body.style.fontFamily = "Awesome Font, serif";

  // OR... by default content is hidden, and rendered once font is available
  var content = document.getElementById("content");
  content.style.visibility = "visible";

  // OR... apply own render strategy here... 
});
```

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

ولا تعد إستراتيجية التضمين مرنة بالقدر الكافي ولا تسمح لنا بتحديد مهلات زمنية مخصصة أو إستراتيجيات عرض لمحتوى مختلف، ولكنها تعد حلاً بسيطًا ومفيدًا يمكن استخدامه على جميع المتصفحات. وللحصول على أفضل نتائج، يمكنك فصل الخطوط المضمنة إلى أوراق أنماط مستقلة بذاتها وعرضها بدون max-age طويل - وبهذه الطريقة،عند تحديث CSS، لن يتم إجبار الزائرين على إعادة تنزيل الخطوط.

Note: استخدام التضمين الانتقائي! تذكر أن سبب استخدام @font-face سلوك التحميل الكسول هو تجنب تنزيل المتغيرات والمجموعات الفرعية غير اللازمة للخط. كما أن زيادة حجم CSS عبر التضمين المتشدد من شأنه التأثير سلبيًا على [مسار العرض الحرج](/web/fundamentals/performance/critical-rendering-path/) - يجب على المتصفح تنزيل جميع محتويات CSS حتى يتمكن من إنشاء CSSOM وتصميم شجرة العرض وعرض محتويات الصفحة على الشاشة.

### تحسين عرض الخط باستخدام التضمين

تعد موارد الخط عادة موارد ثابتة ولا تتعرض لتحديثات بصفة مستمرة. ونتيجة لذلك، فهي تناسب تمامًا انتهاء صلاحية max-age الطويل - فتأكد من تحديد كل من [رأس ETag الشرطي](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags) و[سياسة Cache-Control الاختيارية](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) مع جميع موارد الخطوط.

#### Browser behaviors

وليست هناك حاجة إلى تخزين الخطوط في وحدة تخزين داخلية أو عبر آليات أخرى - لأن كلاً منها سيتسبب في مشكلة في الأداء. توفر ذاكرة تخزين HTTP المؤقت في المتصفح بالإضافة إلى واجهة برمجة تطبيقات Font Loading أو مكتبة webfontloader أفضل وأهم آلية لعرض موارد الخطوط على المتصفح.

<table>
  <thead>
    <tr>
      <th data-th="Browser">Browser</th>
      <th data-th="Timeout">Timeout</th>
      <th data-th="Fallback">Fallback</th>
      <th data-th="Swap">Swap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Browser">
        <strong>Chrome 35+</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Opera</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Firefox</strong>
      </td>
      <td data-th="Timeout">
        3 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Internet Explorer</strong>
      </td>
      <td data-th="Timeout">
        0 seconds
      </td>
      <td data-th="Fallback">
        Yes
      </td>
      <td data-th="Swap">
        Yes
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Safari</strong>
      </td>
      <td data-th="Timeout">
        No timeout
      </td>
      <td data-th="Fallback">
        N/A
      </td>
      <td data-th="Swap">
        N/A
      </td>
    </tr>
  </tbody>
</table>

* يعطل Safari عرض النص حتى يكتمل تنزيل الخط.
* يعطل كل من Chrome وFirefox عرض الخط لمدة تصل إلى 3 ثوانٍ، وبعد ذلك يستخدم خط رجوع، وبعد اكتمال تنزيل الخط، يعيد عرض النص مرة أخرى باستخدام الخط الذي تم تنزيله.
* يعرض IE في الحال خط الرجوع إذا لم يكن خط الطلب متاحًا بعد، ويعيد عرضه بمجرد اكتمال تنزيل الخط.

على النقيض من الاعتقاد الشائع، لا يحتاج استخدام خطوط الويب إلى تأخير عرض الصفحة ولا يكون له أثر سلبي على مقاييس الأداء الأخرى. ذلك أن الاستخدام المحسن جيدًا للخطوط يمكن أن يترك انطباع مستخدم أفضل إجمالاً من خلال ما يلي: ترويج جيد للعلامة التجارية، تحسين لإمكانية القراءة والاستخدام والبحث، بالإضافة إلى توفير حل يمكن ضبطه ليتناسب مع عدة مستويات دقة ويتكيف جيدًا مع جميع التنسيقات ومستويات دقة الشاشات. لا تقلق من استخدام خطوط الويب.

#### The font display timeline

إلا أن تطبيقها بدون خبرة قد يؤدي إلى تنزيلات كبيرة وحدوث تأخر بدون داعٍ. ولذلك يجب تطبيق مجموعة أدوات التحسين ومساعدة المتصفح من خلال تحسين أصول الخطوط نفسها وكيفية جلبها واستخدامها على الصفحات.

1. **راجع استخدام الخطوط وراقبه:** لا تستخدم عددًا كبيرًا من الخطوط على الصفحات، ومع كل خط، قلل عدد المتغيرات المستخدمة قدر الإمكان. وهذا سيساعد في ترك انطباع بالثبات والسرعة لدى المستخدم.
2. **قلل حجم موارد الخط:** يمكن تقليل حجم العديد من الخطوط أو تقسيمها إلى عدة نطاقات ترميز موحد لتقديم الصور المنقوشة المطلوبة فقط في صفحة ما - وهذا يقلل من حجم الملف ويحسن من سرعة تنزيل المورد. إلا أنه عند تحديد المجموعات الفرعية، يجب أن تتوخى الحذر من خلال التحسين لإعادة استخدام الخط - على سبيل المثال، عدم تنزيل مجموعة مختلفة ومتداخلة من الأحرف على كل صفحة. وهناك إجراء جيد وهو تقليل الحجم بناءً على النص البرمجي - على سبيل المثال اللاتيني والسيريلي وهكذا.
3. **وفر تنسيقات خطوط محسنة مع كل متصفح:** يجب تقديم كل خط بتنسيق WOFF2 وWOFF وEOT وTTF. تأكد من تطبيق ضغط GZIP على التنسيقين EOT وTTF، نظرًا لأنه لا يتم ضغطهما افتراضيًا.

Understanding these periods means you can use `font-display` to decide how your font should render depending on whether or when it was downloaded.

#### Using font-display

To work with the `font-display` property, add it your `@font-face` rules:

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  font-display: auto; /* or block, swap, fallback, optional */
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */ 
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

`font-display` currently supports the following range of values: `auto | block | swap | fallback | optional`.

* **`auto`** uses whatever font display strategy the user-agent uses. Most browsers currently have a default strategy similar to `block`.

* **`block`** gives the font face a short block period (3s is recommended in most cases) and an infinite swap period. In other words, the browser draws "invisible" text at first if the font is not loaded, but swaps the font face in as soon as it loads. To do this the browser creates an anonymous font face with metrics similar to the selected font but with all glyphs containing no "ink." This value should only be used if rendering text in a particular typeface is required for the page to be usable.

* **`swap`** gives the font face a zero second block period and an infinite swap period. This means the browser draws text immediately with a fallback if the font face isn’t loaded, but swaps the font face in as soon as it loads. Similar to `block`, this value should only be used when rendering text in a particular font is important for the page, but rendering in any font will still get a correct message across. Logo text is a good candidate for **swap** since displaying a company’s name using a reasonable fallback will get the message across but you’d eventually use the official typeface.

* **`fallback`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a short swap period (three seconds is recommended in most cases). In other words, the font face is rendered with a fallback at first if it’s not loaded, but the font is swapped as soon as it loads. However, if too much time passes, the fallback will be used for the rest of the page’s lifetime. `fallback` is a good candidate for things like body text where you’d like the user to start reading as soon as possible and don’t want to disturb their experience by shifting text around as a new font loads in.

* **`optional`** gives the font face an extremely small block period (100ms or less is recommended in most cases) and a zero second swap period. Similar to `fallback`, this is a good choice for when the downloading font is more of a "nice to have" but not critical to the experience. The `optional` value leaves it up to the browser to decide whether to initiate the font download, which it may choose not to do or it may do it as a low priority depending on what it thinks would be best for the user. This can be beneficial in situations where the user is on a weak connection and pulling down a font may not be the best use of resources.

`font-display` is [gaining adoption](https://caniuse.com/#feat=css-font-rendering-controls) in many modern browsers. You can look forward to consistency in browser behavior as it becomes widely implemented.

### تحسين إعادة استخدام الخط باستخدام ذاكرة التخزين المؤقت في HTTP

Used together, `<link rel="preload">` and the CSS `font-display` give developers a great deal of control over font loading and rendering, without adding in much overhead. But if you need additional customizations, and are willing to incur with the overhead introduced by running JavaScript, there is another option.

The [Font Loading API](https://www.w3.org/TR/css-font-loading/) provides a scripting interface to define and manipulate CSS font faces, track their download progress, and override their default lazyload behavior. For example, if you're sure that a particular font variant is required, you can define it and tell the browser to initiate an immediate fetch of the font resource:

    var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
      style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
    });
    
    // don't wait for the render tree, initiate an immediate fetch!
    font.load().then(function() {
      // apply the font (which may re-render text and cause a page reflow)
      // after the font has finished downloading
      document.fonts.add(font);
      document.body.style.fontFamily = "Awesome Font, serif";
    
      // OR... by default the content is hidden, 
      // and it's rendered after the font is available
      var content = document.getElementById("content");
      content.style.visibility = "visible";
    
      // OR... apply your own render strategy here... 
    });
    

Further, because you can check the font status (via the [check()](https://www.w3.org/TR/css-font-loading/#font-face-set-check)) method and track its download progress, you can also define a custom strategy for rendering text on your pages:

* ويتم تنزيل أوراق أنماط CSS التي تتضمن استعلامات وسائط متطابقة تلقائيًا باستخدام المتصفح مع الأولوية نظرًا لأنها تكون مطلوبة لإنشاء CSSOM.
* يجبر تضمين بيانات الخط في ورقة أنماط CSS المتصفح على تنزيل الخط بأولوية عالية وبدون انتظار شجرة العرض - أي يعتبر ذلك بمثابة استبدال يدوي لسلوك التحميل الكسول الافتراضي.
* You can use the fallback font to unblock rendering and inject a new style that uses the desired font after the font is available.

Best of all, you can also mix and match the above strategies for different content on the page. For example, you can delay text rendering on some sections until the font is available, use a fallback font, and then re-render after the font download has finished, specify different timeouts, and so on.

Note: The Font Loading API is still [under development in some browsers](http://caniuse.com/#feat=font-loading). Consider using the [FontLoader polyfill](https://github.com/bramstein/fontloader) or the [webfontloader library](https://github.com/typekit/webfontloader) to deliver similar functionality, albeit with even more overhead from an additional JavaScript dependency.

### Proper caching is a must

Font resources are, typically, static resources that don't see frequent updates. As a result, they are ideally suited for a long max-age expiry - ensure that you specify both a [conditional ETag header](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags), and an [optimal Cache-Control policy](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) for all font resources.

If your web application uses a [service worker](/web/fundamentals/primers/service-workers/), serving font resources with a [cache-first strategy](/web/fundamentals/instant-and-offline/offline-cookbook/#cache-then-network) is appropriate for most use cases.

You should not store fonts using [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) or [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API); each of those has its own set of performance issues. The browser's HTTP cache provides the best and most robust mechanism to deliver font resources to the browser.

## قائمة التحقق من التحسين

Contrary to popular belief, the use of webfonts doesn't need to delay page rendering or have a negative impact on other performance metrics. The well-optimized use of fonts can deliver a much better overall user experience: great branding, improved readability, usability, and searchability, all while delivering a scalable multi-resolution solution that adapts well to all screen formats and resolutions. Don't be afraid to use webfonts.

That said, a naive implementation may incur large downloads and unnecessary delays. You need to help the browser by optimizing the font assets themselves and how they are fetched and used on your pages.

* **Audit and monitor your font use:** don't use too many fonts on your pages, and, for each font, minimize the number of used variants. This helps produce a more consistent and a faster experience for your users.
* **Subset your font resources:** many fonts can be subset, or split into multiple unicode-ranges to deliver just the glyphs that a particular page requires. This reduces the file size and improves the download speed of the resource. However, when defining the subsets, be careful to optimize for font re-use. For example, don't download a different but overlapping set of characters on each page. A good practice is to subset based on script: for example, Latin, Cyrillic, and so on.
* **Deliver optimized font formats to each browser:** provide each font in WOFF2, WOFF, EOT, and TTF formats. Make sure to apply GZIP compression to the EOT and TTF formats, because they are not compressed by default.
* **Give precedence to `local()` in your `src` list:** listing `local('Font Name')` first in your `src` list ensures that HTTP requests aren't made for fonts that are already installed.
* **Customize font loading and rendering using `<link rel="preload">`, `font-display`, or the Font Loading API:** default lazyloading behavior may result in delayed text rendering. These web platform features allow you to override this behavior for particular fonts, and to specify custom rendering and timeout strategies for different content on the page.
* **Specify revalidation and optimal caching policies:** fonts are static resources that are infrequently updated. Make sure that your servers provide a long-lived max-age timestamp and a revalidation token to allow for efficient font reuse between different pages. If using a service worker, a cache-first strategy is appropriate.

## Automated testing for web font optimization with Lighthouse {: #lighthouse }

[Lighthouse](/web/tools/lighthouse) can help automate the process of making sure that you're following web font optimization best practices. Lighthouse is an auditing tool built by the Chrome DevTools team. You can run it as a Node module, from the command line, or from the Audits panel of Chrome DevTools. You tell Lighthouse what URL to audit, and then it runs a bunch of tests on the page, and gives you a report of what the page is doing well, and how it can improve.

The following audits can help you make sure that your pages are continuing to follow web font optimization best practices over time:

* [Enable text compression](/web/tools/lighthouse/audits/text-compression)
* [Preload key requests](/web/tools/lighthouse/audits/preload)
* [Uses inefficient cache policy on static assets](/web/tools/lighthouse/audits/cache-policy)
* [All text remains visible during webfont loads](/web/updates/2016/02/font-display)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}