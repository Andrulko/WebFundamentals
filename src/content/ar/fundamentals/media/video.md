project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: تعرف على أسهل طرق إضافة الفيديو إلى موقعك وتأكد من ترك أفضل انطباع لدى المستخدمين على أي جهاز.

{# wf_updated_on: 2014-04-28 #} {# wf_published_on: 2000-01-01 #}

# الفيديو {: .page-title }

{% include "web/_shared/contributors/samdutton.html" %}

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="j5fYOYrsocs"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

يفضل المستخدمون مشاهدة مقاطع الفيديو نظرًا لأنها تمثل عنصر تسلية وإفادة. على أجهزة الجوّال، يمكن أن تكون مقاطع الجوّال الوسيلة الأسهل للاستفادة من المعلومات. إلا أن مقاطع الفيديو تستهلك معدل نقل بيانات، كما أنها لا تعمل بطريقة واحدة على جميع أنظمة التشغيل. لا يفضل المستخدمون الانتظار حتى تحميل مقاطع الفيديو، أو عند الضغط على التشغيل وعدم تلقي أي شيء. يمكنك الاطلاع على مزيد من المعلومات لمعرفة أسهل طرق إضافة فيديو إلى موقعك على الويب والتأكد من ترك أفضل انطباع لدى المستخدمين على أي جهاز.

## إضافة مقطع فيديو

### TL;DR {: .hide-from-toc }

* استخدم عنصر الفيديو لتحميل الفيديو على موقعك وفك ترميزه وتشغيله.
* اهتم بإنتاج الفيديو بعدة تنسيقات لتغطية نطاق واسع من أنظمة تشغيل الجوّال.
* عيِّن حجمًا مناسبًا للفيديو، وتأكد من عدم تجاوزه للحاويات.
* نظرًا لأهمية إمكانية الوصول، أضف عنصر المقطع الصوتي كفرع لعنصر الفيديو.

### إضافة عنصر الفيديو

تعرف على أسهل طرق إضافة الفيديو إلى موقعك وتأكد من ترك أفضل انطباع لدى المستخدمين على أي جهاز.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4" type="video/mp4">
  <p>هذا المتصفح ليس متوافقًا مع عنصر الفيديو.</p>
</video>

    <video src="chrome.webm" type="video/webm">
        <p>متصفحك ليس متوافقًا مع عنصر الفيديو.</p>
    </video>
    

### تحديد عدة تنسيقات ملفات

أضف عنصر الفيديو لتحميل الفيديو على موقعك وفك ترميزه وتشغيله:

لا تعد جميع المتصفحات متوافقة مع تنسيقات الفيديو نفسها. ويتيح العنصر `<source>` لك تحديد عدة تنسيقات في شكل نقاط رجوع في حالة عدم توافق متصفح المستخدم مع أحدها. على سبيل المثال:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/video-main.html" region_tag="sourcetypes" adjust_indentation="auto" %}
</pre>

عندما يحلل المتصفح علامات `<source>`، فإنه يستخدم السمة الاختيارية `type` للمساعدة في تحديد الملف المطلوب تنزيله وتشغيله. إذا كان المتصفح يتوافق مع WebM، فسيتم تشغيل chrome.webm، وإلا، فسيتم التحقق مما إذا كان يمكن تشغيل مقاطع فيديو MPEG-4. يمكنك الاطلاع على [A Digital Media Primer for Geeks](//www.xiph.org/video/vid1.shtml "Highly entertaining and informative video guide to digital video") لاستكشاف المزيد حول آلية عمل الفيديو والصوت على الويب.

يتميز هذا الأسلوب بالعديد من الميزات عن عرض نصوص برمجية مختلفة على HTML أو من جهة الخادم، خاصة على الجوّال:

تعد جميع هذه النقاط ذات أهمية خاصة في سياقات الجوّال؛ حيث يعد معدل نقل البيانات ووقت الاستجابة العنصر الأهم، وغالبًا ما تكون قدرة المستخدم على الانتظار محدودة. ويمكن أن يؤدي عدم تضمين سمة النوع إلى التأثير في مستوى الأداء عندما تكون هناك عدة مصادر بأنواع غير متوافقة.

باستخدام أدوات مطوِّري متصفح الجوّال، يمكنك مقارنة نشاط الشبكة [مع سمات النوع](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html) و[without type attributes](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/notype.html). راجع كذلك رؤوس الاستجابة في أدوات مطوِّري المتصفح [للتأكد من أن الخادم يعرض نوع MIME المناسب](//developer.mozilla.org/en/docs/Properly_Configuring_Server_MIME_Types)، وإلا فلن تعمل فحوصات نوع مصدر الفيديو.

* يمكن لمطوِّري البرامج إدراج التنسيقات بترتيب الأفضلية.
* يؤدي التبديل من جهة العميل الأصلي إلى خفض وقت الاستجابة؛ بحيث لا يتم إلا تقديم طلب واحد للحصول على المحتوى.
* يعد السماح للمتصفح باختيار تنسيق أمرًا أسهل وأسرع ويمكن الاعتماد عليه أكثر من استخدام قاعدة بيانات الدعم من جهة الخادم مع اكتشاف وكيل المستخدم.
* يساعد تحديد نوع مصدر كل ملف في تحسين أداء الشبكة؛ ويمكن للمتصفح اختيار مصدر الفيديو بدون الاضطرار إلى تنزيل جزء من الفيديو لإجراء `فحص` للتنسيق.

اهتم بتوفير معدل نقل البيانات وجعل الموقع يبدو أكثر سرعة في الاستجابة من خلال استخدام واجهة برمجة تطبيقات Media Fragments لإضافة وقت بدء ووقت انتهاء إلى عنصر الفيديو.

لإضافة قطاع وسائط، يمكنك إضافة `#t=[start_time][,end_time]` إلى عنوان URL للوسائط. على سبيل المثال، لتشغيل الفيديو من الثانية 5 إلى الثانية 10، يمكنك تحديد:

يمكنك أيضًا استخدام واجهة برمجة تطبيقات Media Fragments لتقديم عدة عروض على مقطع الفيديو نفسه - مثل نقاط التلميح في DVD - بدون الاضطرار إلى ترميز عدة ملفات وعرضها.

### تحديد وقت بدء ووقت انتهاء

Note: - تتوافق واجهة برمجة تطبيقات Media Fragments مع معظم أنظمة التشغيل، وليس iOS.

<video controls>
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm#t=5,10" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/chrome.mp4#t=5,10" type="video/mp4">
  <p>هذا المتصفح ليس متوافقًا مع عنصر الفيديو.</p>
</video>

باستخدام أدوات مطوِّري برامج المتصفح، راجع `Accept-Ranges: bytes` في رؤوس الاستجابة:

    <source src="video/chrome.webm#t=5,10" type="video/webm">
    

You can also use the Media Fragments API to deliver multiple views on the same video&ndash;like cue points in a DVD&ndash;without having to encode and serve multiple files.

أضف سمة ملصق إلى عنصر الفيديو حتى تكون لدى المستخدمين فكرة عن المحتوى بمجرد تحميل العنصر، بدون الحاجة إلى تنزيل فيديو أو بدء التشغيل.

كما يمكن أن يكون الملصق نقطة رجوع إذا تم خرق `src` في الفيديو أو لم يكن أي من تنسيقات الفيديو المقدمة متوافقة. التأثير السلبي الوحيد لصور الملصق هو وجود طلب ملف إضافي يستهلك جزءًا من معدل نقل البيانات ويتطلب العرض. للاطلاع على مزيد من المعلومات، راجع [Image optimization](../../performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html#image-optimization).

<img class="center" alt="Chrome DevTools screenshot: Accept-Ranges: bytes"
src="images/Accept-Ranges-Chrome-Dev-Tools.png" />

### تضمين صورة ملصق

Add a poster attribute to the `video` element so that your users have an idea of the content as soon as the element loads, without needing to download video or start playback.

    <video poster="poster.jpg" ...>
      ...
    </video>
    

لا تتوافق جميع تنسيقات الفيديو على جميع أنظمة التشغيل. ويمكنك الاطلاع على التنسقات المتوافقة على أنظمة التشغيل الأساسية والتأكد من توافق الفيديو مع كل منها.

يمكنك استخدام `canPlayType()` للبحث عن تنسيقات الفيديو المتوافقة. وتحتاج هذه الطريقة إلى وسيط سطر ثابت من `mime-type` وبرامج ترميز اختيارية للخروج بإحدى القيم التالية:

<div class="attempt-left">
  <figure>
    <img alt="Android Chrome screenshot, portrait: no poster"
    src="images/Chrome-Android-video-no-poster.png">
    <figcaption>
      Android Chrome screenshot, portrait: no poster
     </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Android Chrome screenshot, portrait: with poster"
    src="images/Chrome-Android-video-poster.png">
    <figcaption>
      Android Chrome screenshot, portrait: with poster
     </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

## توفير خيارات بديلة لأنظمة التشغيل القديمة

في ما يلي بعض الأمثلة على وسيطات `canPlayType()` والقيم الناتجة عند التشغيل في Chrome:

### الاطلاع على التنسيقات المتوافقة

هناك الكثير من الأدوات التي تساعد في حفظ فيديو واحد بعدة تنسيقات مختلفة:

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">القيمة المعروضة</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Return value">(سطر فارغ)</td>
      <td data-th="Description">الحاوية و/أو برنامج الترميز غير متوافق.</td>
    </tr>
    <tr>
      <td data-th="Return value"><code>maybe</code></td>
      <td data-th="Description">
        قد تكون الحاوية أو برامج الترميز متوافقة، ولكن يحتاج المتصفح
        إلى تنزيل فيديو ما للفحص.
      </td>
    </tr>
    <tr>
      <td data-th="Return value"><code>probably</code></td>
      <td data-th="Description">يبدو أن التنسيق متوافق.
      </td>
    </tr>
  </tbody>
</table>

هل تريد معرفة التنسيق الذي اختاره المتصفح بالفعل؟

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">النوع</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Type"><code>video/xyz</code></td>
      <td data-th="Response">(سطر فارغ)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response">(سطر فارغ)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/xyz; codecs="nonsense, noise"</code></td>
      <td data-th="Response">(سطر فارغ)</td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/mp4; codecs="avc1.42E01E, mp4a.40.2"</code></td>
      <td data-th="Response"><code>probably</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm</code></td>
      <td data-th="Response"><code>maybe</code></td>
    </tr>
    <tr>
      <td data-th="Type"><code>video/webm; codecs="vp8, vorbis"</code></td>
      <td data-th="Response"><code>probably</code></td>
    </tr>
  </tbody>
</table>

### إنتاج الفيديو بعدة تنسيقات

في جافا سكريبت، يمكنك استخدام الخاصية `currentSrc` في الفيديو لعرض المصدر المستخدم.

* تأكد من أن طلبات النطاق متوافقة مع خادمك. يتم تمكين طلبات النطاق افتراضيًا على معظم الخوادم، إلا أن بعض خدمات الاستضافة قد تعطلها.
* GUI applications: [Miro](http://www.mirovideoconverter.com/), [HandBrake](//handbrake.fr/), [VLC](//www.videolan.org/)
* Online encoding/transcoding services: [Zencoder](//en.wikipedia.org/wiki/Zencoder), [Amazon Elastic Encoder](//aws.amazon.com/elastictranscoder)

### التحقق من التنسيق المستخدم

للاطلاع على ذلك عمليًا، يمكنك الرجوع إلى [هذا العرض التجريبي](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/video-main.html): يختار كل من Chrome وFirefox ما يلي `chrome.webm` (نظرًا لأنه الخيار الأول في قائمة المصادر الممكنة التي يتوافق معها هذان المتصفحان) بينما يختار Safari ما يلي `chrome.mp4`.

إذا كان الأمر يتعلق بترك انطباع جيد لدى المستخدمين، فيجب الاهتمام بحجم الفيديو.

## استخدام حجم الفيديو المناسب

قد يختلف الحجم الفعلي لإطار الفيديو عن أبعاد عنصر الفيديو (كما هو الحال عندما لا تظهر الصورة باستخدام أبعادها الحقيقية).

### TL;DR {: .hide-from-toc }

* أدوات سطح المكتب: [FFmpeg](//ffmpeg.org/)
* تطبيقات واجهة المستخدم التصويرية: [Miro](//www.mirovideoconverter.com/) و[HandBrake](//handbrake.fr/) و[VLC](//www.videolan.org/)
* خدمات الترميز/تحويل الترميز على الإنترنت: [Zencoder](//en.wikipedia.org/wiki/Zencoder) و[Amazon Elastic Encoder](//aws.amazon.com/elastictranscoder)

### التحقق من حجم الفيديو

وللتحقق من حجم ترميز الفيديو، يمكنك استخدام عنصر الفيديو `videoWidth` وخصائص `videoHeight`. يعرض كل من `width` و`height` أبعاد عنصر الفيديو التي ربما يكون تم تحديد حجمها باستخدام CSS أو سمات العرض والطول المضمَّنة.

عندما تكون عناصر الفيديو كبيرة جدًا مقارنة بإطار العرض، فقد يتم تجاوز حدود الحاوية، مما يجعل من المستحيل على المستخدم مشاهدة المحتوى أو استخدام عناصر التحكم.

### التأكد من عدم تجاوز مقاطع الفيديو لحدود الحاويات

When video elements are too big for the viewport, they may overflow their container, making it impossible for the user to see the content or use the controls.

<div class="attempt-left">
  <figure>
    <img alt="Android Chrome screenshot, portrait: unstyled video element overflows
    viewport" src="images/Chrome-Android-portrait-video-unstyled.png">
    <figcaption>
      Android Chrome screenshot, portrait: unstyled video element overflows viewport
    </figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Android Chrome screenshot, landscape: unstyled video element overflows
    viewport" src="images/Chrome-Android-landscape-video-unstyled.png">
    <figcaption>
      Android Chrome screenshot, landscape: unstyled video element overflows viewport
    </figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

يمكنك التحكم في أبعاد الفيديو باستخدام جافا سكريبت أو CSS. تتيح مكتبات جافا سكريبت والمكوِّنات الإضافية أيضًا مثل [FitVids](//fitvidsjs.com/) إمكانية الحفاظ على الحجم ونسبة العرض إلى الارتفاع المناسبة، حتى بالنسبة إلى مقاطع فيديو Flash من YouTube والصادر الأخرى.

يمكنك استخدام [استعلامات وسائط CSS](../../layouts/rwd-fundamentals/#use-css-media-queries-for-responsiveness) لتحديد حجم العناصر بناءً على أبعاد إطار العرض؛ ويعتبر `max-width: 100%` الخيار الصديق.

{# include shared/related_guides.liquid inline=true list=page.related-guides.media #}

بالنسبة إلى محتوى الوسائط في إطارات iframes (مثل مقاطع فيديو YouTube)، جرب طريقة الاستجابة السريعة (على غرار ما يقترحه جون سورداكوسكي](//avexdesigns.com/responsive-youtube-embed/)).

**CSS:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="styling" adjust_indentation="auto" %}
</pre>

**CSS:**

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/responsive_embed.html" region_tag="markup" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/responsive_embed.html)

قارن [النموذج سريع الاستجابة](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/responsive_embed.html) بـ [النسخة بطيئة الاستجابة](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/unyt.html).

## تخصيص مشغِّل الفيديو

تعرض أنظمة التشغيل المختلفة مقطع الفيديو على نحو مختلف. ويجب أن تهتم حلول الجوّال باتجاه الجهاز. استخدم واجهة برمجة تطبيقات Fullscreen للتحكم في عرض ملء الشاشة لمحتوى الفيديو.

### كيفية عمل اتجاه الجهاز على الأجهزة المختلفة

تعرض أنظمة التشغيل المختلفة مقطع الفيديو على نحو مختلف. ويجب أن تهتم حلول الجوّال باتجاه الجهاز. استخدم واجهة برمجة تطبيقات Fullscreen للتحكم في عرض ملء الشاشة لمحتوى الفيديو.

لا يمثل اتجاه الجهاز مشكلة لشاشات سطح المكتب أو أجهزة الكمبيوتر المحمول، إلا أنه يمثل أهمية كبيرة عند التفكير في تصميم لصفحة ويب على الجوّال والأجهزة اللوحية.

<div class="attempt-left">
  <figure>
    <img  alt="Screenshot of video playing in Safari on iPhone, portrait"
    src="images/iPhone-video-playing-portrait.png">
    <figcaption>Screenshot of video playing in Safari on iPhone, portrait</figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img alt="Screenshot of video playing in Safari on iPhone, landscape"
    src="images/iPhone-video-playing-landscape.png">
    <figcaption>Screenshot of video playing in Safari on iPhone, landscape</figcaption>
  </figure>
</div>

<div style="clear:both;"></div>

يؤدي متصفح Safari على iPhone مهمة رائعة من خلال التبديل بين الاتجاه العمودي والأفقي:

<img alt="لقطة شاشة لفيديو يعمل على متصفح Safari لجهاز iPhone في الاتجاه العمودي"
src="images/iPad-Retina-landscape-video-playing.png" />

قد يتسبب اتجاه الجهاز على جهاز iPad وعلى Chrome لجهاز Android في حدوث مشكلات. على سبيل المثال، بدون أي تخصيص، قد يظهر الفيديو الذي يتم تشغيله على جهاز iPad في الاتجاه الأفقي على النحو التالي:

## المسائل المتعلقة بإمكانية الوصول

<img class="attempt-right" alt="لقطة شاشة لفيديو يعمل على متصفح Safari لجهاز iPad Retina في الاتجاه الأفقي"
src="images/iPhone-video-with-poster.png" />

يمكن أن يؤدي إعداد الفيديو `width: 100%` أو `max-width: 100%` مع CSS إلى حل العديد من مشكلات تنسيق اتجاه الجهاز. قد تحتاج أيضًا إلى التفكير في بدائل ملء الشاشة.

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Chrome on Android,
portrait" src="images/Chrome-Android-video-playing-portrait-3x5.png" />

On Android, users can request request fullscreen mode by clicking the fullscreen icon. But the default is to play video inline:

<div style="clear:both;"></div>

<img class="attempt-right" alt="Screenshot of video playing in Safari on iPad
Retina, landscape" src="images/iPad-Retina-landscape-video-playing.png" />

Safari on an iPad plays video inline:

<div style="clear:both;"></div>

### العرض المضمَّن أو بملء الشاشة

يشغِّل متصفح Safari على iPad الفيديو مضمَّنًا:

To full screen an element, like a video:

    elem.requestFullScreen();
    

بالنسبة إلى أنظمة التشغيل التي لا تفرض تشغيل الفيديو بملء الشاشة، تكون واجهة برمجة تطبيقات Fullscreen [متوافقة على نطاق واسع](//caniuse.com/fullscreen). يمكنك استخدام واجهة برمجة التطبيقات هذه للتحكم في ملء الشاشة بالمحتوى، أو الصفحة.

    document.body.requestFullScreen();
    

لملء الشاشة بعنصر ما، مثل video:

    video.addEventListener("fullscreenchange", handler);
    

لملء الشاشة بالمستند بالكامل:

    console.log("In full screen mode: ", video.displayingFullscreen);
    

يمكنك أيضًا الاستماع إلى تغييرات حالة ملء الشاشة:

<video autoplay muted loop class="attempt-right">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.webm" type="video/webm">
  <source src="https://storage.googleapis.com/webfundamentals-assets/videos/fullscreen.mp4" type="video/mp4">
  <p>هذا المتصفح ليس متوافقًا مع عنصر الفيديو.</p>
</video>

كما يمكنك الاطلاع لمعرفة ما إذا كان العنصر في وضع ملء الشاشة حاليًا أم لا:

يمكنك أيضًا استخدام الفئة الصورية `:fullscreen` في CSS لتغيير طريقة عرض العناصر في وضع ملء الشاشة.

على الأجهزة التي تتوافق مع واجهة برمجة تطبيقات Fullscreen، فكر في استخدام الصور المصغَّرة بمثابة عناصر نائبة للفيديو:

<div style="clear:both;"></div>

## مرجع سريع

للاطلاع على ذلك عمليًا، راجع [demo](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/media/fullscreen.html).

### التحكم في ملء الشاشة بالمحتوى

<img class="attempt-right" alt="Screenshot showing captions displayed using the
track element in Chrome on Android" src="images/Chrome-Android-track-landscape-5x3.jpg" />

لا تعد إمكانية الوصول ميزة. ذلك أن المستخدمين الذين لا يمكنهم السمع أو الرؤية لن يكون بإمكانهم تجربة الفيديو إطلاقًا بدون تسميات توضيحية أو أوصاف. ولا تستغرق إضافة هذه الميزات إلى الفيديو وقتًا أطول من الانطباع السيئ الذي سيشعر به المستخدمون. ولذلك يجب ترك انطباع أساسي على الأقل لدى جميع المستخدمين.

<div style="clear:both;"></div>

### تضمين التسميات التوضيحية لتحسين إمكانية الوصول

حتى تصبح الوسائط أسهل في الوصول على الجوَّال، يمكنك تضمين التسميات التوضيحية أو الأوصاف باستخدام عنصر المسار الصوتي.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/media/_code/track.html" region_tag="track" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/media/track.html)

باستخدام عنصر المسار الصوتي، تظهر التسميات التوضيحية على النحو التالي:

### إضافة عنصر مسار صوتي

A track file consists of timed "cues" in WebVTT format:

    WEBVTT
    
    00:00.000 --> 00:04.000
    رجل يجلس على فرع شجرة ويستخدم جهاز كمبيوتر محمولاً.
    
    00:05.000 --> 00:08.000
    انكسر الفرع وبدأ الرجل في السقوط.
    
    ...
    

## Quick Reference

### تحديد التسميات التوضيحية في ملف المسار الصوتي

من السهل جدًا إضافة تسميات توضيحية إلى الفيديو؛ وذلك من خلال إضافة عنصر مسار صوتي كفرع لعنصر الفيديو:

<table>
  <thead>
    <tr>
      <th>Attribute</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Attribute"><code>src</code></td>
      <td data-th="Description">جميع المتصفحات</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>poster</code></td>
      <td data-th="Description">جميع المتصفحات</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>preload</code></td>
      <td data-th="Description">جميع متصفحات الجوّال تتجاهل التحميل المسبق (preload). </td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>autoplay</code></td>
      <td data-th="Description">غير متوافقة على iPhone وAndroid وتتوافق مع جميع متصفحات أجهزة سطح المكتب وiPad وFirefox وOpera لنظام التشغيل Android.</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>loop</code></td>
      <td data-th="Description">جميع المتصفحات</td>
    </tr>
    <tr>
      <td data-th="Attribute"><code>controls</code></td>
      <td data-th="Description">جميع المتصفحات</td>
    </tr>
  </tbody>
</table>

### سمات عنصر الفيديو

توفر سمة عنصر المسار الصوتي `src` موقع ملف المسار الصوتي.

يتضمن ملف المسار الصوتي `تلميحات` بالوقت بتنسيق WebVTT:

* لا تعرض مقاطع فيديو حجم إطارها كبير أو أعلى جودة من الجودة التي يمكن لنظام التشغيل التعامل معها.
* احرص على ألا يكون مقطع الفيديو أطول من اللازم أبدًا.
* مقاطع الفيديو الطويلة قد تتسبب في حدوث حالات فواق للتنزيل والبحث؛ وقد يتعين على بعض المتصفحات الانتظار حتى يتم تنزيل الفيديو قبل بدء التشغيل.

نظرة عامة سريعة حول الخواص على عنصر الفيديو.

### جافا سكريبت

للحصول على القائمة الكاملة بسمات عنصر الفيديو وتعريفاتها، يمكنك الاطلاع على [مواصفات عنصر الفيديو](//www.w3.org/TR/html5/embedded-content-0.html#the-video-element).

<table class="responsive">
  <thead>
    <tr>
      <th colspan="2">القيمة</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Value"><code>none</code></td>
      <td data-th="Description">قد لا يشاهد المستخدم مقطع الفيديو &ndash; عدم تحميل أي شيء مسبقًا</td>
    </tr>
    <tr>
      <td data-th="Value"><code>metadata</code></td>
      <td data-th="Description">يجب تحميل البيانات الوصفية (المدة والأبعاد ومسارات النص) مسبقًا، ولكن بأصغر فيديو.</td>
    </tr>
    <tr>
      <td data-th="Value"><code>auto</code></td>
      <td data-th="Description">يُعتبر تنزيل الفيديو الكامل مباشرة أمرًا مفضلاً.</td>
    </tr>
  </tbody>
</table>

في أجهزة سطح المكتب، يطلب `autoplay` من المتصفح بدء تنزيل الفيديو وتشغيله في الحال وفي أقرب وقت ممكن. في أجهزة iOS وChrome لنظام Android، لا يعمل `autoplay`؛ ويتعين على المستخدمين النقر على الشاشة لتشغيل الفيديو.

### JavaScript

حتى على أنظمة التشغيل التي يتم تمكين التشغيل التلقائي فيها، يجب التفكير جيدًا في جدوى تمكينه:

#### التشغيل التلقائي

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Property &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Property"><code>currentTime</code></td>
      <td data-th="Description">الحصول على موضع التشغيل بالثواني أو تعيينه.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>volume</code></td>
      <td data-th="Description">الحصول على مستوى الصوت الحالي للفيديو أو تعيينه.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>muted</code></td>
      <td data-th="Description">الحصول على كتم الصوت أو تعيينه.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>playbackRate</code></td>
      <td data-th="Description">الحصول على معدل التشغيل، السرعة العادية هي 1 للأمام.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>buffered</code></td>
      <td data-th="Description">معلومات حول مقدار الفيديو الذي يتم تخزينه وتجهيزه للتشغيل (راجع <a href="http://people.mozilla.org/~cpearce/buffered-demo.html" title="Demo displaying amount of buffered video in a canvas element">العرض التجريبي</a>).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>currentSrc</code></td>
      <td data-th="Description">عنوان الفيديو المطلوب تشغيله.</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoWidth</code></td>
      <td data-th="Description">عرض الفيديو بالبكسل (قد يكون مختلفًا عن عرض عنصر الفيديو).</td>
    </tr>
    <tr>
      <td data-th="Property"><code>videoHeight</code></td>
      <td data-th="Description">طول الفيديو بالبكسل (قد يكون مختلفًا عن طول عنصر الفيديو).</td>
    </tr>
  </tbody>
</table>

#### التحميل المسبق

<table class="responsive">
  <thead>
    <tr>
    <th colspan="2">Method &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Method"><code>load()</code></td>
      <td data-th="Description">حمِل مصدر الفيديو أو أعد تحميله بدون بدء التشغيل: على سبيل المثال، عندما يتم تغيير السمة src للفيديو باستخدام جافا سكريبت.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>play()</code></td>
      <td data-th="Description">تشغيل الفيديو من موقعه الحالي.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>pause()</code></td>
      <td data-th="Description">وقف الفيديو مؤقتًا في موقعه الحالي.</td>
    </tr>
    <tr>
      <td data-th="Method"><code>canPlayType('format')</code></td>
      <td data-th="Description">التعرف على التنسيقات المتوافقة (راجع `التعرف على التنسيقات المتوافقة`).</td>
    </tr>
  </tbody>
</table>

يمكن ضبط سلوك التشغيل التلقائي في Android WebView عبر [واجهة برمجة تطبيقات WebSettings](//developer.android.com/reference/android/webkit/WebSettings.html#setMediaPlaybackRequiresUserGesture(boolean)). يتم اختيار الإعداد الافتراضي على صواب ولكن يمكن لتطبيق WebView اختيار تعطيله.

#### الخصائص

توفر السمة `preload` تلميحًا يعرف من خلاله المتصفح مقدار البيانات أو المحتوى الذي يجب تحميله مسبقًا.

<table class="responsive">
  <thead>
  <tr>
    <th colspan="2">Event &amp; Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Event"><code>canplaythrough</code></td>
      <td data-th="Description">يتم التشغيل عندما تتوفر بيانات كافية تفيد بأن المتصفح يعتقد أنه يمكن تشغيل الفيديو بالكامل بدون أعطال.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>ended</code></td>
      <td data-th="Description">يتم التشغيل عندما ينتهي تشغيل الفيديو.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>error</code></td>
      <td data-th="Description">يتم التشغيل عندما يظهر خطأ.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>playing</code></td>
      <td data-th="Description">يتم التشغيل عندما يبدأ تشغيل الفيديو لأول مرة بعد الإيقاف المؤقت أو عند إعادة التشغيل.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>progress</code></td>
      <td data-th="Description">يتم التشغيل من آن لآخر للإشارة إلى أن التنزيل قيد التقدم.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>waiting</code></td>
      <td data-th="Description">يتم التشغيل عندما يتأخر الإجراء في انتظار اكتمال إجراء آخر.</td>
    </tr>
    <tr>
      <td data-th="Event"><code>loadedmetadata</code></td>
      <td data-th="Description">يتم التشغيل عندما ينتهي المتصفح من تحميل البيانات الوصفية للفيديو: المدة والأبعاد والمسارات النصية.</td>
    </tr>
  </tbody>
</table>

## Feedback {: #feedback }

تتضمن السمة `preload` تأثيرات مختلفة على أنظمة التشغيل المختلفة. على سبيل المثال، يخزن Chrome مقدار 25 ثانية من الفيديو على سطح المكتب، بينما لا يتم تخزين أي شيء على iOS وAndroid. وهذا يعني أنه على الجوّال، قد يتأخر البدء في التشغيل على العكس من أجهزة سطح المكتب. يمكنك الاطلاع على [صفحة اختبار Steve Souders](//stevesouders.com/tests/mediaevents.php) للحصول على التفاصيل الكاملة.