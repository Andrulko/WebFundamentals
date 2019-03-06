project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: يمكن للصورة أن تنوب عن 1000 كلمة، ولذلك تلعب الصور دورًا كبيرًا في أية صفحة. إلا أن وضع الصور يعني إضافة لعدد وحدات بايت التي يتم تنزيلها. في تصميم الويب سريع الاستجابة، لا يمكن للتنسيقات فقط التغير بناءً على سمات الجهاز، بل الصور أيضًا.

{# wf_updated_on: 2018-12-15 #} {# wf_published_on: 2014-04-29 #}

# الصور {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

يمكن للصورة أن تنوب عن 1000 كلمة، ولذلك تلعب الصور دورًا كبيرًا في أية صفحة. إلا أن وضع الصور يعني إضافة لعدد وحدات بايت التي يتم تنزيلها. في تصميم الويب سريع الاستجابة، لا يمكن للتنسيقات فقط التغير بناءً على سمات الجهاز، بل الصور أيضًا.

## الصور في الترميز

<img src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

Other times the image may need to be changed more drastically: changing the proportions, cropping, and even replacing the entire image. In this case, changing the image is usually referred to as art direction. See [responsiveimages.org/demos/](https://responsiveimages.org/demos/) for more examples.

المرات الأخرى التي قد تحتاج الصورة فيها إلى تغيير جذري: تغيير النسب والاقتصاص واستبدال الصورة بالكامل. وفي هذه الحالة، يُشار إلى تغيير الصورة عادة باسم الإخراج الفني. يمكنك الاطلاع على [responsiveimages.org/demos/](http://responsiveimages.org/demos/) للحصول على مزيد من الأمثلة.

## الصور في CSS 

<style>
  .side-by-side {
    display: inline-block;
    margin: 0 20px 0 0;
    width: 45%;
  }

  span#data_uri {
    background: url(data:image/svg+xml,%3C%3Fxml%20version%3D%221.0%22%20encoding%3D%22utf-8%22%3F%3E%0D%0A%3C%21--%20Generator%3A%20Adobe%20Illustrator%2016.0.0%2C%20SVG%20Export%20Plug-In%20.%20SVG%20Version%3A%206.00%20Build%200%29%20%20--%3E%0D%0A%3C%21DOCTYPE%20svg%20PUBLIC%20%22-%2F%2FW3C%2F%2FDTD%20SVG%201.1%2F%2FEN%22%20%22http%3A%2F%2Fwww.w3.org%2FGraphics%2FSVG%2F1.1%2FDTD%2Fsvg11.dtd%22%3E%0D%0A%3Csvg%20version%3D%221.1%22%20id%3D%22Layer_1%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20xmlns%3Axlink%3D%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%22%20x%3D%220px%22%20y%3D%220px%22%0D%0A%09%20width%3D%22396.74px%22%20height%3D%22560px%22%20viewBox%3D%22281.63%200%20396.74%20560%22%20enable-background%3D%22new%20281.63%200%20396.74%20560%22%20xml%3Aspace%3D%22preserve%22%0D%0A%09%3E%0D%0A%3Cg%3E%0D%0A%09%3Cg%3E%0D%0A%09%09%3Cg%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23E44D26%22%20points%3D%22409.737%2C242.502%20414.276%2C293.362%20479.828%2C293.362%20480%2C293.362%20480%2C242.502%20479.828%2C242.502%20%09%09%09%0D%0A%09%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpath%20fill%3D%22%23E44D26%22%20d%3D%22M281.63%2C110.053l36.106%2C404.968L479.757%2C560l162.47-45.042l36.144-404.905H281.63z%20M611.283%2C489.176%0D%0A%09%09%09%09L480%2C525.572V474.03l-0.229%2C0.063L378.031%2C445.85l-6.958-77.985h22.98h26.879l3.536%2C39.612l55.315%2C14.937l0.046-0.013v-0.004%0D%0A%09%09%09%09L480%2C422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283%2C489.176z%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23F16529%22%20points%3D%22480%2C192.833%20604.247%2C192.833%20603.059%2C206.159%20600.796%2C231.338%20599.8%2C242.502%20599.64%2C242.502%20%0D%0A%09%09%09%09480%2C242.502%20480%2C293.362%20581.896%2C293.362%20595.28%2C293.362%20594.068%2C306.699%20582.396%2C437.458%20581.649%2C445.85%20480%2C474.021%20%0D%0A%09%09%09%09480%2C474.03%20480%2C525.572%20611.283%2C489.176%20642.17%2C143.166%20480%2C143.166%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23F16529%22%20points%3D%22540.988%2C343.029%20480%2C343.029%20480%2C422.35%20535.224%2C407.445%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23EBEBEB%22%20points%3D%22414.276%2C293.362%20409.737%2C242.502%20479.828%2C242.502%20479.828%2C242.38%20479.828%2C223.682%20%0D%0A%09%09%09%09479.828%2C192.833%20355.457%2C192.833%20356.646%2C206.159%20368.853%2C343.029%20479.828%2C343.029%20479.828%2C293.362%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23EBEBEB%22%20points%3D%22479.828%2C474.069%20479.828%2C422.4%20479.782%2C422.413%20424.467%2C407.477%20420.931%2C367.864%20%0D%0A%09%09%09%09394.052%2C367.864%20371.072%2C367.864%20378.031%2C445.85%20479.771%2C474.094%20480%2C474.03%20480%2C474.021%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22343.784%2C50.229%20366.874%2C50.229%20366.874%2C75.517%20392.114%2C75.517%20392.114%2C0%20366.873%2C0%20366.873%2C24.938%20%0D%0A%09%09%09%09343.783%2C24.938%20343.783%2C0%20318.544%2C0%20318.544%2C75.517%20343.784%2C75.517%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22425.307%2C25.042%20425.307%2C75.517%20450.549%2C75.517%20450.549%2C25.042%20472.779%2C25.042%20472.779%2C0%20403.085%2C0%20%0D%0A%09%09%09%09403.085%2C25.042%20425.306%2C25.042%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22508.537%2C38.086%20525.914%2C64.937%20526.349%2C64.937%20543.714%2C38.086%20543.714%2C75.517%20568.851%2C75.517%20568.851%2C0%20%0D%0A%09%09%09%09542.522%2C0%20526.349%2C26.534%20510.159%2C0%20483.84%2C0%20483.84%2C75.517%20508.537%2C75.517%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20points%3D%22642.156%2C50.555%20606.66%2C50.555%20606.66%2C0%20581.412%2C0%20581.412%2C75.517%20642.156%2C75.517%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23FFFFFF%22%20points%3D%22480%2C474.021%20581.649%2C445.85%20582.396%2C437.458%20594.068%2C306.699%20595.28%2C293.362%20581.896%2C293.362%20%0D%0A%09%09%09%09480%2C293.362%20479.828%2C293.362%20479.828%2C343.029%20480%2C343.029%20540.988%2C343.029%20535.224%2C407.445%20480%2C422.35%20479.828%2C422.396%20%0D%0A%09%09%09%09479.828%2C422.4%20479.828%2C474.069%20%09%09%09%22%2F%3E%0D%0A%09%09%09%3Cpolygon%20fill%3D%22%23FFFFFF%22%20points%3D%22479.828%2C242.38%20479.828%2C242.502%20480%2C242.502%20599.64%2C242.502%20599.8%2C242.502%20600.796%2C231.338%20%0D%0A%09%09%09%09603.059%2C206.159%20604.247%2C192.833%20480%2C192.833%20479.828%2C192.833%20479.828%2C223.682%20%09%09%09%22%2F%3E%0D%0A%09%09%3C%2Fg%3E%0D%0A%09%3C%2Fg%3E%0D%0A%3C%2Fg%3E%0D%0A%3C%2Fsvg%3E%0D%0A) no-repeat;
    background-size: cover;
    height: 484px;
  }

  span#svg {
    background: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' version='1.1' x='0px' y='0px' width='50%' height='560px' viewBox='281.63 0 396.74 560' enable-background='new 281.63 0 396.74 560' xml:space='preserve'><g><g><g><polygon fill='#E44D26' points='409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5'/><path fill='#E44D26' d='M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z'/><polygon fill='#F16529' points='480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2'/><polygon fill='#F16529' points='541,343 480,343 480,422.4 535.2,407.4'/><polygon fill='#EBEBEB' points='414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4'/><polygon fill='#EBEBEB' points='479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474'/><polygon points='343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5'/><polygon points='425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25'/><polygon points='508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5'/><polygon points='642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5'/><polygon fill='#FFFFFF' points='480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1'/><polygon fill='#FFFFFF' points='479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7'/></g></g></g></svg>") no-repeat;
    background-size: cover;
    height: 484px;
  }
</style>

 

{% include "web/_shared/udacity/ud882.html" %}

### الصور سريعة الاستجابة

* استخدم الأحجام النسبية مع الصور لمنع تجاوز حدود الحاوية بدون قصد.
* استخدم العنصر `picture` إذا كنت تريد تحديد صور مختلفة بناءً على سمات الجهاز (يُعرف ذلك أيضًا باسم `الإخراج الفني`).
* استخدم الكلمة الوصفية `srcset` و `x` في العنصر `img` لتوفير تلميحات للمتصفح بشأن أفضل صورة يمكن استخدامها عند الاختيار من بين نسب كثافة مختلفة.
* If your page only has one or two images and these are not used elsewhere on your site, consider using inline images to reduce file requests.

### الإخراج الفني

يتميز العنصر `img` بأهميته الكبيرة؛ نظرًا لأنه يساعد على التنزيل وفك الترميز وعرض المحتوى، ولذلك تتوافق المتصفحات الحديثة مع عدد مختلف من تنسيقات الصور. لا يختلف تضمين الصور التي تعمل على مختلف الأجهزة عن المخصصة لأجهزة سطح المكتب، ولن يلزم سوى بعض التغييرات الطفيفة لتوفير انطباع جيد.

لا تنس استخدام وحدات نسبية عند تحديد قيم عرض محددة للصور لمنعها من تجاوز حدود إطار العرض بدون قصد. على سبيل المثال، سيؤدي استخدام `width: 50%;` إلى أن يصبح عرض الصورة 50% من عنصر الاحتواء (وليس إطار العرض أو حجم وحدة البكسل الفعلي).

    img, embed, object, video {
      max-width: 100%;
    }
    

نظرًا لأن CSS يتيح للمحتوى تجاوز حدود الحاوية، قد يلزم استخدام الحد الأقصى لقيمة العرض: 100% لمنع الصور والمحتوى الآخر من تجاوز الحدود. على سبيل المثال:

### TL;DR {: .hide-from-toc }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="Pzc5Dly_jEM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

لا تنس توفير كلمات وصفية معبرة عبر السمة `alt` على عناصر `img`؛ نظرًا لأن ذلك يساعد في جعل موقعك على الويب أكثر سهولة في الاستخدام من خلال توفير سياق لأجهزة قراءة الشاشة والتقنيات الحساسة الأخرى.

<div style="clear:both;">
</div>

    <img src="photo.png" srcset="photo@2x.png 2x" ...>
    

تحسِّن السمة `srcset` من سلوك العنصر `img`، مما يسهل توفير عدة ملفات صور لميزات أجهزة مختلفة. وكما هو الحال في `image-set` [وظيفية CSS](#use-image-set-to-provide-high-res-images)، الأصلية في CSS، تتيح `srcset` للمتصفح اختيار أفضل صورة بناءً على ميزات الجهاز، على سبيل المثال باستخدام صورة 2x على شاشة 2x، وربما في المستقبل، صورة 1x على جهاز 2x عند استخدام شبكة معدل نقل بيانات محدودة.

في المتصفحات التي لا تتوافق مع `srcset`، يستخدم المتصفح ملف الصورة الافتراضي المحدد في السمة `src`. ولذلك من الضروري دائمًا تضمين صورة 1x يمكن عرضها على أي جهاز، بغض النظر عن الإمكانيات. وعندما يكون هناك توافق مع `srcset`، يتم تحليل قائمة الصور/الشروط المفصولة بفواصل قبل تقديم أية طلبات، ولا يتم تنزيل سوى الصورة المناسبة وعرضها.

### استخدام أحجام نسبية للصور

<img class="attempt-right" src="img/art-direction.png" alt="Art direction example"
srcset="img/art-direction.png 1x, img/art-direction-2x.png 2x" />

يمكن إجراء تغيير للصور بناءً على سمات الجهاز، ويعرف ذلك أيضًا باسم الإخراج الفني ويمكن تنفيذه باستخدام العنصر picture. يحدد عنصر `picture` حلاً تفسيريًا لتوفير عدة نسخ من الصورة بناءً على سمات مختلفة، مثل حجم الجهاز ودقته واتجاهه وغير ذلك الكثير.

<div style="clear:both;">
</div>

Dogfood: The `picture` element is beginning to land in browsers. Although it's not available in every browser yet, we recommend its use because of the strong backward compatibility and potential use of the [Picturefill polyfill](https://scottjehl.github.io/picturefill/). See the [ResponsiveImages.org](http://responsiveimages.org/#implementation) site for further details.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="QINlm3vjnaY"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Note: يبدأ العنصر `picture` الوصول إلى المتصفحات. وعلى الرغم من أنه ليس متوفرًا في جميع المتصفحات إلى الآن، نوصي باستخدامه نظرًا لإمكانية التوافق مع إصدارات أقدم وإمكانية الاستفادة من [Picturefill polyfill](https://scottjehl.github.io/picturefill/). راجع موقع [ResponsiveImages.org](http://responsiveimages.org/#implementation) للحصول على مزيد من التفاصيل.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media.html" region_tag="picture" adjust_indentation="auto" %}
</pre>

يجب استخدام العنصر `picture` عندما يكون مصدر الصورة متوفرًا بمستوى كثافة متعدد، أو عندما يكتشف التصميم سريع الاستجابة صورة مختلفة بعض الشيء عن بعض أنواع الشاشات. وكما هو الحال في العنصر `video`، يمكن تضمين عدة عناصر `source`، مما يتيح تحديد ملفات صور مختلفة بناءً على استعلامات الوسائط أو تنسيق الصور.

في المثال الوارد أعلاه، إذا كانت قيمة عرض المتصفح 800 بكسل على الأقل، فسيتم استخدام `head.jpg` أو `head-2x.jpg`، بناءً على دقة الجهاز. وإذا كانت قيمة المتصفح بين 450 بكسل و800 بكسل، فسيتم استخدام `head-small.jpg` أو `head-small-2x.jpg` بناءً على دقة الجهاز أيضًا. وبالنسبة إلى قيم عرض الشاشة الأقل من 450 بكسل ومدى التوافق مع الإصدارات الأقدم الذي لا يوفر العنصر `picture`، سيعرض المتصفح العنصر `img` بدلاً من ذلك، ويجب تضمينه دائمًا.

#### صور الحجم النسبي

عندما لا يكون الحجم النهائي للصورة معروفًا، قد يصعب تحديد كلمة وصفية للكثافة في مصادر الصور. وينطبق هذا خاصة على الصور التي تتسع لعرض تناسبي في المتصفح وتعد صورًا مائعة بناءً على حجم المتصفح.

وبدلاً من توفير أحجام صور وقيم كثافة ثابتة، يمكن تحديد حجم كل صورة يتم تقديمها من خلال إضافة كلمة وصفية إلى حجم عنصر الصورة، مما يتيح للمتصفح تلقائيًا حساب كثافة وحدات بكسل اللازمة واختيار الصورة الأفضل التي يمكن تنزيلها.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/sizes.html" region_tag="picture" adjust_indentation="auto" %}
</pre>

يعرض المثال الموضح أعلاه صورة تحتل نصف قيمة عرض الإطار (`sizes=`50vw``)، وبناءً على قيمة عرض المتصفح ونسبة وحدات بكسل في الجهاز، يتم السماح للمتصفح باختيار الصورة المناسبة بغض النظر عن حجم نافذة المتصفح إذا كانت كبيرة. على سبيل المثال، يعرض الجدول الوارد أدناه الصورة التي يمكن للمتصفح اختيارها:

في العديد من الحالات، قد يتغير الحجم أو تتغير الصورة بناءً على نقاط فصل تنسيق الموقع. على سبيل المثال، في الشاشة الصغيرة، قد تحتاج إلى أن تمتد الصورة على عرض الإطار بالكامل، بينما يجب ألا تأخذ الصورة في الشاشات الكبيرة سوى مساحة صغيرة.

<table class="">
  <tr>
    <th data-th="Browser width">
      عرض المتصفح
    </th>
    
    <th data-th="Device pixel ratio">
      معدل وحدات بكسل في الجهاز
    </th>
    
    <th data-th="Image used">
      الصورة المستخدمة
    </th>
    
    <th data-th="Effective resolution">
      الدقة الفعالة
    </th>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400 بكسل
    </td>
    
    <td data-th="Device pixel ratio">
      1
    </td>
    
    <td data-th="Image used">
      <code>200.png</code>
    </td>
    
    <td data-th="Effective resolution">
      1x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      400 بكسل
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      320بكسل
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2.5x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      600 بكسل
    </td>
    
    <td data-th="Device pixel ratio">
      2
    </td>
    
    <td data-th="Image used">
      <code>800.png</code>
    </td>
    
    <td data-th="Effective resolution">
      2.67x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      640 بكسل
    </td>
    
    <td data-th="Device pixel ratio">
      3
    </td>
    
    <td data-th="Image used">
      <code>1000.png</code>
    </td>
    
    <td data-th="Effective resolution">
      3.125x
    </td>
  </tr>
  
  <tr>
    <td data-th="Browser width">
      1100 بكسل
    </td>
    
    <td data-th="Device pixel ratio">
      1
    </td>
    
    <td data-th="Image used">
      <code>1400.png</code>
    </td>
    
    <td data-th="Effective resolution">
      1.27x
    </td>
  </tr>
</table>

#### توضيح نقاط الفصل في الصور سريعة الاستجابة

تستخدم السمة `sizes` في المثال السابق عدة استعلامات وسائط لتحديد حجم الصورة. عندما تكون قيمة عرض المتصفح أكبر من 600 بكسل، ستظهر الصورة بنسبة 25% من عرض الإطار، وعندما تتراوح قيمتها بين 500 بكسل و600 بكسل، فستظهر بنسبة 50% من عرض الإطار، أما إذا كانت أقل من 500 بكسل، فستظهر بملء العرض.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/breakpoints.html" region_tag="picture" adjust_indentation="auto" %}
</pre>

يريد العملاء دائمًا الاطلاع على الأشياء التي يرغبون في شرائها. وفي مواقع البيع بالتجزئة على الويب، يتوقع المستخدمون تمكينهم من الاطلاع على صور مقربة بدقة عالية للمنتجات لمعرفة تفاصيل أكثر عن المنتج، ويتضح من [المشاركين في هذه الدراسة](/web/fundamentals/principles/research-study) أنه يحدث انزعاج عند عدم تمكن العملاء من ذلك.

يقدم موقع J. Crew على الويب مثالاً معبرًا عن الصور القابلة للنقر والتوسيع. ويشير التراكب القابل للاختفاء إلى أن الصورة قابلة للنقر، مما يوفر صورة قابلة للتكبير مع مظهر تفصيلي جيد.

### تحسين`img` باستخدام `srcset` للعمل على الأجهزة مرتفعة نسبة DPI<figure class="attempt-right"> 

<img src="img/sw-make-images-expandable-good.png" srcset="img/sw-make-images-expandable-good.png 1x, img/sw-make-images-expandable-good-2x.png 2x" alt="موقع J. Crews على الويب مع صورة منتج يمكن توسيعها" /> <figcaption>موقع J. Crews على الويب مع صورة منتج يمكن توسيعها</figcaption> </figure> 

يوفر [أسلوب الصور المضغوطة](http://www.html5rocks.com/en/mobile/high-dpi/#toc-tech-overview) صورة 2x مضغوطة بنسبة كبيرة على جميع الأجهزة، بغض النظر عن إمكانيات الجهاز الفعلية. وبناءً على نوع الصورة ومستوى الضغط، قد لا يظهر تغير على جودة الصورة، إلا أن حجم الملف يقل بشكل ملحوظ.

A good example of tappable, expandable images is provided by the J. Crew site. A disappearing overlay indicates that an image is tappable, providing a zoomed in image with fine detail visible.

<div style="clear:both;">
</div>

### الإخراج الفني في الصور سريعة الاستجابة باستخدام `picture`

#### الصور المضغوطة

Note: توخ الحذر بشأن الأسلوب المضغوط نظرًا للتكاليف الزائدة التي يتسبب فيها بسبب الذاكرة وإلغاء الترميز. يعد تغيير حجم الصور الكبيرة لتناسب الشاشات الصغيرة أمرًا مكلفًا وقد يتسبب في إزعاج خاصة على الأجهزة محدودة التكلفة حيث يكون كل من الذاكرة والمعالج محدودين.

يساعد استبدال صور جافا سكريبت على فحص إمكانيات الجهاز و`إنجاز المطلوب`. ويمكنك تحديد معدل وحدات بكسل في الجهاز من خلال `window.devicePixelRatio`، ومعرفة عرض الشاشة وطولها، بل وتنفيذ بعض فحوصات الاتصال على الشبكة من خلال `navigator.connection` أو إصدار طلب وهمي. بعد جمع كل هذه المعلومات، يمكنك تحديد الصورة المطلوب تحميلها.

إحدى العقبات الكبيرة التي تواجه هذا الأسلوب أن استخدام جافا سكريبت يعني أنك ستؤجل تحميل الصورة حتى انتهاء المحلل المقدم على الأقل. وهذا يعني أن الصور لن تبدأ في التنزيل حتى بعد بدء حدث `pageload`. علاوة على ذلك، سيتولى المتصفح في الغالب تنزيل كل من صور 1x و2x، مما يؤدي إلى زيادة في وزن الصفحة.

#### استبدال صور جافا سكريبت

يعد العنصر `background` في CSS `أداة مفيدة لإضافة صور معقدة إلى العناصر، مما يسهل إضافة عدة صور ويساعد على تكرارها وغير ذلك الكثير. عند اقتران عنصر الخلفية باستعلامات الوسائط، يصبح أكثر فائدة، ويمكن الصور الشرطية من التحميل بناءً على دقة الشاشة وحجم إطار العرض وغير ذلك الكثير.

لا تؤثر استعلامات الوسائط فقط في تنسيق الصفحة، بل يمكن استخدامها أيضًا لتحميل الصور شرطيًا أو لتوفير إخراج فني بناءً على قيمة عرض إطار العرض.

#### Inlining images: raster and vector

على سبيل المثال، يظهر في المثال الوارد أدناه على الشاشات الصغيرة يتم تنزيل `small.png` فقط ويتم تطبيقها على المحتوى `div`، أما الشاشات الكبيرة، فيتم فيها تطبيق `background-image: url(body.png)` على النص الأساسي و`background-image: url(large.png)` على المحتوى `div`.

تحسِّن الوظيفة `image-set()`في CSS من سلوك الخاصية `background`، مما يجعل من السهل توفير ملفات صور متعددة لميزات أجهزة مختلفة. وبذلك يتمكن المتصفح من اختيار الصورة الأفضل بناءً على سمات الجهاز، كاستخدام صورة 2x على شاشة 2x أو صورة 1x على جهاز 2x عند استخدام شبكة معدل نقل بيانات محدودة.

وبالإضافة إلى تحميل الصورة السليمة، سيكون بإمكان المتصفح ضبطها وفقًا لذلك. وبمعنى آخر، يفترض المتصفح أن الصور 2x أكبر بنسبة الضعف من الصور 1x، ولذلك سيضبط الصورة 2x إلى قيمة أقل بمضاعف 2، حتى تظهر الصورة بالحجم نفسه على الصفحة.

##### SVG

لا يزال التوافق مع `image-set()` أمرًا مستحدثًا ولا يتوفر إلا في Chrome وSafari مع بادئة مقدم الخدمة `-webkit`. ويجب الاهتمام بتضمين صورة ترجيع عندما لا يكون `image-set()` متوافقًا، على سبيل المثال:

سيؤدي ما تمت الإشارة إليه أعلاه إلى تحميل الأصل المناسب في المتصفحات التي تتوافق مع image-set، والترجيع إلى الأصل 1x خلافًا لذلك. الأمر الخطير الذي يجب الانتباه إليه أنه على الرغم من أن توافق متصفح `image-set()` يعد منخفضًا، ستحصل معظم المتصفحات على الأصل 1x.

<img class="side-by-side" src="img/html5.png" alt="HTML5 logo, PNG format" /> <img class="side-by-side" src="img/html5.svg" alt="HTML5 logo, SVG format" />

يتوافق كل من Chrome وFirefox وOpera مع المعيار `(min-resolution: 2dppx)`، بينما يتطلب كل من Safari ومتصفح Android أن تكون بنية مقدم الخدمة المثبتة مسبقًا والأقدم بدون وحدة `dppx`. لا تنس أن هذه الأنماط يتم تحميلها إذا كان الجهاز يتوافق مع استعلام الوسائط، ويجب تحديد أنماط للحالة الأساسية. كما يوفر هذا ميزة التأكد من أنه سيتم عرض شيء ما إذا لم يتوافق المتصفح مع استعلامات وسائط بدقة محددة.

<img class="side-by-side" src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiB
      BZG9iZSBJbGx1c3RyYXRvciAxNi4wLjAsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW
      9uOiA2LjAwIEJ1aWxkIDApICAtLT4NCjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RUR
      CBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2
      ZzExLmR0ZCI+DQo8c3ZnIHZlcnNpb249IjEuMSIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8
      vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OT
      kveGxpbmsiIHg9IjBweCIgeT0iMHB4Ig0KCSB3aWR0aD0iMzk2Ljc0cHgiIGhlaWdodD0iNTYwc
      HgiIHZpZXdCb3g9IjI4MS42MyAwIDM5Ni43NCA1NjAiIGVuYWJsZS1iYWNrZ3JvdW5kPSJuZXcg
      MjgxLjYzIDAgMzk2Ljc0IDU2MCIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSINCgk+DQo8Zz4NCgk8Zz4
      NCgkJPGc+DQoJCQk8cG9seWdvbiBmaWxsPSIjRTQ0RDI2IiBwb2ludHM9IjQwOS43MzcsMjQyLj
      UwMiA0MTQuMjc2LDI5My4zNjIgNDc5LjgyOCwyOTMuMzYyIDQ4MCwyOTMuMzYyIDQ4MCwyNDIuN
      TAyIDQ3OS44MjgsMjQyLjUwMiAJCQkNCgkJCQkiLz4NCgkJCTxwYXRoIGZpbGw9IiNFNDREMjYi
      IGQ9Ik0yODEuNjMsMTEwLjA1M2wzNi4xMDYsNDA0Ljk2OEw0NzkuNzU3LDU2MGwxNjIuNDctNDU
    uMDQybDM2LjE0NC00MDQuOTA1SDI4MS42M3ogTTYxMS4yODMsNDg5LjE3Ng0KCQkJCUw0ODAsNT
    I1LjU3MlY0NzQuMDNsLTAuMjI5LDAuMDYzTDM3OC4wMzEsNDQ1Ljg1bC02Ljk1OC03Ny45ODVoM
    jIuOThoMjYuODc5bDMuNTM2LDM5LjYxMmw1NS4zMTUsMTQuOTM3bDAuMDQ2LTAuMDEzdi0wLjAw
    NA0KCQkJCUw0ODAsNDIyLjM1di03OS4zMmgtMC4xNzJIMzY4Ljg1M2wtMTIuMjA3LTEzNi44NzF
    sLTEuMTg5LTEzLjMyNWgxMjQuMzcxSDQ4MHYtNDkuNjY4aDE2Mi4xN0w2MTEuMjgzLDQ4OS4xNz
    Z6Ii8+DQoJCQk8cG9seWdvbiBmaWxsPSIjRjE2NTI5IiBwb2ludHM9IjQ4MCwxOTIuODMzIDYwN
    C4yNDcsMTkyLjgzMyA2MDMuMDU5LDIwNi4xNTkgNjAwLjc5NiwyMzEuMzM4IDU5OS44LDI0Mi41
    MDIgNTk5LjY0LDI0Mi41MDIgDQoJCQkJNDgwLDI0Mi41MDIgNDgwLDI5My4zNjIgNTgxLjg5Niw
    yOTMuMzYyIDU5NS4yOCwyOTMuMzYyIDU5NC4wNjgsMzA2LjY5OSA1ODIuMzk2LDQzNy40NTggNT
    gxLjY0OSw0NDUuODUgNDgwLDQ3NC4wMjEgDQoJCQkJNDgwLDQ3NC4wMyA0ODAsNTI1LjU3MiA2M
    TEuMjgzLDQ4OS4xNzYgNjQyLjE3LDE0My4xNjYgNDgwLDE0My4xNjYgCQkJIi8+DQoJCQk8cG9s
    eWdvbiBmaWxsPSIjRjE2NTI5IiBwb2ludHM9IjU0MC45ODgsMzQzLjAyOSA0ODAsMzQzLjAyOSA
    0ODAsNDIyLjM1IDUzNS4yMjQsNDA3LjQ0NSAJCQkiLz4NCgkJCTxwb2x5Z29uIGZpbGw9IiNFQk
    VCRUIiIHBvaW50cz0iNDE0LjI3NiwyOTMuMzYyIDQwOS43MzcsMjQyLjUwMiA0NzkuODI4LDI0M
    i41MDIgNDc5LjgyOCwyNDIuMzggNDc5LjgyOCwyMjMuNjgyIA0KCQkJCTQ3OS44MjgsMTkyLjgz
    MyAzNTUuNDU3LDE5Mi44MzMgMzU2LjY0NiwyMDYuMTU5IDM2OC44NTMsMzQzLjAyOSA0NzkuODI
    4LDM0My4wMjkgNDc5LjgyOCwyOTMuMzYyIAkJCSIvPg0KCQkJPHBvbHlnb24gZmlsbD0iI0VCRU
    JFQiIgcG9pbnRzPSI0NzkuODI4LDQ3NC4wNjkgNDc5LjgyOCw0MjIuNCA0NzkuNzgyLDQyMi40M
    TMgNDI0LjQ2Nyw0MDcuNDc3IDQyMC45MzEsMzY3Ljg2NCANCgkJCQkzOTQuMDUyLDM2Ny44NjQg
    MzcxLjA3MiwzNjcuODY0IDM3OC4wMzEsNDQ1Ljg1IDQ3OS43NzEsNDc0LjA5NCA0ODAsNDc0LjA
    zIDQ4MCw0NzQuMDIxIAkJCSIvPg0KCQkJPHBvbHlnb24gcG9pbnRzPSIzNDMuNzg0LDUwLjIyOS
    AzNjYuODc0LDUwLjIyOSAzNjYuODc0LDc1LjUxNyAzOTIuMTE0LDc1LjUxNyAzOTIuMTE0LDAgM
    zY2Ljg3MywwIDM2Ni44NzMsMjQuOTM4IA0KCQkJCTM0My43ODMsMjQuOTM4IDM0My43ODMsMCAz
    MTguNTQ0LDAgMzE4LjU0NCw3NS41MTcgMzQzLjc4NCw3NS41MTcgCQkJIi8+DQoJCQk8cG9seWd
    vbiBwb2ludHM9IjQyNS4zMDcsMjUuMDQyIDQyNS4zMDcsNzUuNTE3IDQ1MC41NDksNzUuNTE3ID
    Q1MC41NDksMjUuMDQyIDQ3Mi43NzksMjUuMDQyIDQ3Mi43NzksMCA0MDMuMDg1LDAgDQoJCQkJN
    DAzLjA4NSwyNS4wNDIgNDI1LjMwNiwyNS4wNDIgCQkJIi8+DQoJCQk8cG9seWdvbiBwb2ludHM9
    IjUwOC41MzcsMzguMDg2IDUyNS45MTQsNjQuOTM3IDUyNi4zNDksNjQuOTM3IDU0My43MTQsMzg
    uMDg2IDU0My43MTQsNzUuNTE3IDU2OC44NTEsNzUuNTE3IDU2OC44NTEsMCANCgkJCQk1NDIuNT
    IyLDAgNTI2LjM0OSwyNi41MzQgNTEwLjE1OSwwIDQ4My44NCwwIDQ4My44NCw3NS41MTcgNTA4L
    jUzNyw3NS41MTcgCQkJIi8+DQoJCQk8cG9seWdvbiBwb2ludHM9IjY0Mi4xNTYsNTAuNTU1IDYw
    Ni42Niw1MC41NTUgNjA2LjY2LDAgNTgxLjQxMiwwIDU4MS40MTIsNzUuNTE3IDY0Mi4xNTYsNzU
    uNTE3IAkJCSIvPg0KCQkJPHBvbHlnb24gZmlsbD0iI0ZGRkZGRiIgcG9pbnRzPSI0ODAsNDc0Lj
    AyMSA1ODEuNjQ5LDQ0NS44NSA1ODIuMzk2LDQzNy40NTggNTk0LjA2OCwzMDYuNjk5IDU5NS4yO
    CwyOTMuMzYyIDU4MS44OTYsMjkzLjM2MiANCgkJCQk0ODAsMjkzLjM2MiA0NzkuODI4LDI5My4z
    NjIgNDc5LjgyOCwzNDMuMDI5IDQ4MCwzNDMuMDI5IDU0MC45ODgsMzQzLjAyOSA1MzUuMjI0LDQ
    wNy40NDUgNDgwLDQyMi4zNSA0NzkuODI4LDQyMi4zOTYgDQoJCQkJNDc5LjgyOCw0MjIuNCA0Nz
    kuODI4LDQ3NC4wNjkgCQkJIi8+DQoJCQk8cG9seWdvbiBmaWxsPSIjRkZGRkZGIiBwb2ludHM9I
    jQ3OS44MjgsMjQyLjM4IDQ3OS44MjgsMjQyLjUwMiA0ODAsMjQyLjUwMiA1OTkuNjQsMjQyLjUw
    MiA1OTkuOCwyNDIuNTAyIDYwMC43OTYsMjMxLjMzOCANCgkJCQk2MDMuMDU5LDIwNi4xNTkgNjA
    0LjI0NywxOTIuODMzIDQ4MCwxOTIuODMzIDQ3OS44MjgsMTkyLjgzMyA0NzkuODI4LDIyMy42OD
    IgCQkJIi8+DQoJCTwvZz4NCgk8L2c+DQo8L2c+DQo8L3N2Zz4NCg==" /> <svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg>

عند إضافة رموز إلى الصفحة، يمكنك استخدام رموز SVG قدر الإمكان أو رموز الترميز الموحد في بعض الحالات.

<svg class="side-by-side" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="396.74px" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.737,242.502 414.276,293.362 479.828,293.362 480,293.362 480,242.502 479.828,242.502"/><path fill="#E44D26" d="M281.63,110.053l36.106,404.968L479.757,560l162.47-45.042l36.144-404.905H281.63z M611.283,489.176 L480,525.572V474.03l-0.229,0.063L378.031,445.85l-6.958-77.985h22.98h26.879l3.536,39.612l55.315,14.937l0.046-0.013v-0.004 L480,422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283,489.176z"/><polygon fill="#F16529" points="480,192.833 604.247,192.833 603.059,206.159 600.796,231.338 599.8,242.502 599.64,242.502 480,242.502 480,293.362 581.896,293.362 595.28,293.362 594.068,306.699 582.396,437.458 581.649,445.85 480,474.021 480,474.03 480,525.572 611.283,489.176 642.17,143.166 480,143.166 "/><polygon fill="#F16529" points="540.988,343.029 480,343.029 480,422.35 535.224,407.445 "/><polygon fill="#EBEBEB" points="414.276,293.362 409.737,242.502 479.828,242.502 479.828,242.38 479.828,223.682 479.828,192.833 355.457,192.833 356.646,206.159 368.853,343.029 479.828,343.029 479.828,293.362 "/><polygon fill="#EBEBEB" points="479.828,474.069 479.828,422.4 479.782,422.413 424.467,407.477 420.931,367.864 394.052,367.864 371.072,367.864 378.031,445.85 479.771,474.094 480,474.03 480,474.021 "/><polygon points="343.784,50.229 366.874,50.229 366.874,75.517 392.114,75.517 392.114,0 366.873,0 366.873,24.938 343.783,24.938 343.783,0 318.544,0 318.544,75.517 343.784,75.517 "/><polygon points="425.307,25.042 425.307,75.517 450.549,75.517 450.549,25.042 472.779,25.042 472.779,0 403.085,0 403.085,25.042 425.306,25.042 "/><polygon points="508.537,38.086 525.914,64.937 526.349,64.937 543.714,38.086 543.714,75.517 568.851,75.517 568.851,0 542.522,0 526.349,26.534 510.159,0 483.84,0 483.84,75.517 508.537,75.517 "/><polygon points="642.156,50.555 606.66,50.555 606.66,0 581.412,0 581.412,75.517 642.156,75.517 "/><polygon fill="#FFFFFF" points="480,474.021 581.649,445.85 582.396,437.458 594.068,306.699 595.28,293.362 581.896,293.362 480,293.362 479.828,293.362 479.828,343.029 480,343.029 540.988,343.029 535.224,407.445 480,422.35 479.828,422.396 479.828,422.4 479.828,474.069 "/><polygon fill="#FFFFFF" points="479.828,242.38 479.828,242.502 480,242.502 599.64,242.502 599.8,242.502 600.796,231.338 603.059,206.159 604.247,192.833 480,192.833 479.828,192.833 479.828,223.682 "/></g></g></g></svg><svg class="side-by-side" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" width="50%" height="560px" viewbox="281.63 0 396.74 560" enable-background="new 281.63 0 396.74 560" xml:space="preserve"><g><g><g><polygon fill="#E44D26" points="409.7,242.5 414.3,293.4 479.8,293.4 480,293.4 480,242.5 479.8,242.5"/><path fill="#E44D26" d="M281.63 110.053l36.106 404.968L479.757 560l162.47-45.042l36.144-404.905H281.63z M611.283 489.2 L480 525.572V474.03l-0.229 0.063L378.031 445.85l-6.958-77.985h22.98h26.879l3.536 39.612l55.315 14.937l0.046-0.013v-0.004 L480 422.35v-79.32h-0.172H368.853l-12.207-136.871l-1.189-13.325h124.371H480v-49.668h162.17L611.283 489.176z"/><polygon fill="#F16529" points="480,192.8 604.2,192.8 603.1,206.2 600.8,231.3 599.8,242.5 599.6,242.5 480,242.5 480,293.4 581.9,293.4 595.3,293.4 594.1,306.7 582.4,437.5 581.6,445.9 480,474 480,474 480,525.6 611.3,489.2 642.2,143.2 480,143.2"/><polygon fill="#F16529" points="541,343 480,343 480,422.4 535.2,407.4"/><polygon fill="#EBEBEB" points="414.3,293.4 409.7,242.5 479.8,242.5 479.8,242.4 479.8,223.7 479.8,192.8 355.5,192.8 356.6,206.2 368.9,343 479.8,343 479.8,293.4"/><polygon fill="#EBEBEB" points="479.8,474.1 479.8,422.4 479.8,422.4 424.5,407.5 420.9,367.9 394.1,367.9 371.1,367.9 378,445.9 479.8,474.1 480,474 480,474"/><polygon points="343.8,50.2 366.9,50.2 366.9,75.5 392.1,75.5 392.1,0 366.9,0 366.9,24.9 343.8,24.9 343.8,0 318.5,0 318.5,75.5 343.8,75.5"/><polygon points="425.3,25 425.3,75.5 450.5,75.5 450.5,25 472.8,25 472.8,0 403.1,0 403.1,25 425.3,25"/><polygon points="508.5,38.1 525.9,64.9 526.3,64.9 543.7,38.1 543.7,75.5 568.9,75.5 568.9,0 542.5,0 526.3,26.5 510.2,0 483.8,0 483.8,75.5 508.5,75.5"/><polygon points="642.2,50.6 606.7,50.6 606.7,0 581.4,0 581.4,75.5 642.2,75.5"/><polygon fill="#FFFFFF" points="480,474 581.6,445.9 582.4,437.5 594.1,306.7 595.3,293.4 581.9,293.4 480,293.4 479.8,293.4 479.8,343 480,343 541,343 535.2,407.4 480,422.4 479.8,422.4 479.8,422.4 479.8,474.1"/><polygon fill="#FFFFFF" points="479.8,242.4 479.8,242.5 480,242.5 599.6,242.5 599.8,242.5 600.8,231.3 603.1,206.2 604.2,192.8 480,192.8 479.8,192.8 479.8,223.7"/></g></g></g></svg>

##### Data URI

بخلاف دليل الرموز العادي، قد يتضمن الترميز الموحد رموزًا لنماذج أعداد (&#8528;)، أو صفوف (&#8592;)، أو مشغلات رياضية (&#8730;)، أو أشكالاً هندسية (&#9733;)، أو صور تحكم (&#9654;)، أو أنماط برايل (&#10255;)، أو علامات موسيقية (&#9836;)، أو أحرفًا يونانية (&#937;)، أو قطع شطرنج (&#9822;).

    <img src="data:image/svg+xml;base64,[data]">
    

يتم تضمين رمز الترميز الموحد بالطريقة نفسها المستخدمة في الكيانات التي تحمل اسمًا: `&#XXXX`، حيث يمثل `XXXX` رقم الترميز الموحد. على سبيل المثال:

    background-image: image-set(
      url(icon1x.jpg) 1x,
      url(icon2x.jpg) 2x
    );
    

You're a super &#9733;

بالنسبة إلى متطلبات الرموز المعقدة، تعد رموز SVG خفيفة الوزن بشكل عام، وسهلة الاستخدام، ويمكن تحديد أنماط لها باستخدام CSS. تتضمن SVG عددًا من الميزات التي تميزها عن الصور النقطية:

##### Inlining in CSS

تتميز خطوط الرموز بأنها شائعة، ويمكن استخدامها بسهولة، إلا أنها تحتوي على عيوب مقارنة برموز SVG.

<span class="side-by-side" id="data_uri"></span> <span class="side-by-side" id="svg"></span>

##### Inlining pros & cons

هناك المئات من خطوط الرموز المجانية ومدفوعة المقابل المتاحة، من بينها [Font Awesome](http://fortawesome.github.io/Font-Awesome/) و[Pictos](http://pictos.cc/) و [Glyphicons](http://glyphicons.com/).

تأكد من الموازنة بين وزن طلب HTTP الإضافي وحجم الملف الذي يحتاج إلى الرموز. على سبيل المثال، إذا كنت بحاجة إلى قدر محدود من الرموز فقط، فمن الأفضل استخدام صورة أو نقش متحرك.

* استخدم أفضل صورة مع ميزات العرض، وفكر في حجم الشاشة ودقة الجهاز وتنسيق الصفحة.
* غير العنصر `background-image` في CSS للحصول على عرض مرتفع نسبة DPI باستخدام استعلامات الوسائط مع `min-resolution` و`-webkit-min-device-pixel-ratio`.
* استخدم srcset لتوفير صور عالية الدقة بالإضافة إلى صورة 1x في الترميز.
* فكر جيدًا في تكاليف الأداء عند استخدام تقنيات لاستبدال صورة جافا سكريبت أو عند عرض صور عالية الدقة مضغوطة إلى حد كبير على الأجهزة ذات الدقة الأقل.
* Data URIs cannot be cached, so must be downloaded for every page they're used on.
* They're not supported in IE 6 and 7, incomplete support in IE8.
* With HTTP/2, reducing the number of asset requests will become less of a priority.

غالبًا ما تكون الصور مسؤولة عن معظم وحدات بايت التي يتم تنزيلها، كما أنها غالبًا ما تستهلك قدرًا كبيرًا من المساحة الظاهرة على الصفحة. ونتيجة لذلك، يمكن أن يؤدي تحسين الصور في الغالب إلى توفير كبير في وحدات بايت وتحسين في الأداء على موقع الويب: ذلك أنه كلما قلت وحدات بايت التي ينزلها المتصفح، قلت المنافسة على معدل نقل البيانات وتمكن المتصفح من تنزيل جميع الأصول وعرضها.

## استخدام SVG مع الرموز

هناك نوعان من الصور يجب وضعهما في الاعتبار: [الصور المتجهية](http://en.wikipedia.org/wiki/Vector_graphics) و[الصور النقطية](http://en.wikipedia.org/wiki/Raster_graphics). بالنسبة إلى الصور النقطية، يلزمك أيضًا اختيار تنسيق الضغط المناسب، على سبيل المثال: `GIF` و`PNG` و`JPG`.

### تعيين صور المنتج على إمكانية التوسيع

* استخدم SVG أو الترميز الموحد مع الرموز بدلاً من الصور النقطية.
* Change the `background-image` property in CSS for high DPI displays using media queries with `min-resolution` and `-webkit-min-device-pixel-ratio`.
* Use srcset to provide high resolution images in addition to the 1x image in markup.
* Consider the performance costs when using JavaScript image replacement techniques or when serving highly compressed high resolution images to lower resolution devices.

### أساليب عرض الصور الأخرى

**الصور النقطية**، مثل الصور الفوتوغرافية والصور الأخرى التي يتم عرضها في شكل شبكة من النقاط الفردية أو وحدات بكسل. عادة ما يكون مصدر الصور النقطية كاميرا أو ماسحة ضوئية ويمكن إنشاؤها في المتصفح باستخدام العنصر `canvas`. وكلما زاد حجم الصور، زاد حجم الملف. وعند ضبط حجم الصور النقطية على قيمة أكبر من قيمة الحجم الأصلية، تصبح هذه الصور ضبابية نظرًا لأن المتصفح يحتاج إلى تخمين كيفية ملء وحدات بكسل المفقودة.

**الصور المتجهية**، مثل الشعارات والرسم الخطي، ويتم تحديدها من خلال مجموعة من الانحناءات والخطوط والأشكال وألوان التعبئة. يتم إنشاء الصور المتجهية باستخدام برامج مثل Adobe Illustrator أو Inkscape ويتم حفظها بتنسيق متجهي مثل [`SVG`](http://css-tricks.com/using-svg/). نظرًا لأنه يتم تصميم الصور المتجهية على عناصر بدائية بسيطة، يمكن ضبطها بدون أية خسائر في الدقة وبدون تغيير في حجم الملف.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-set.html" region_tag="imageset" adjust_indentation="auto" %}
</pre>

وعند اختيار التنسيق المناسب، من الضروري الاهتمام بأصل الصورة (سواء أكانت متجهية أم نقطية)، والمحتوى (سواء أكان ألوانًا أم رسومًا متحركة أم نصوصًا أم غير ذلك). لا يتوفر تنسيق واحد يناسب جميع أنواع الصور، بل لكل ميزاته وعيوبه.

### TL;DR {: .hide-from-toc }

ويمكنك البدء بهذه الإرشادات عند اختيار التنسيق المناسب:

    @media (min-resolution: 2dppx),
    (-webkit-min-device-pixel-ratio: 2)
    {
      /* High dpi styles & resources here */
    }
    

يمكن خفض حجم الملف بنسبة كبيرة من خلال `المعالجة التابعة` بعد الحفظ. وهناك عدد من الأدوات المتوفرة لضغط الصور، من خلال ضغط البيانات المنقوص وغير المنقوص وأدوات عبر الإنترنت وواجهة المستخدم التصويرية وسطر الأوامر. من الأفضل قدر الإمكان محاولة تحسين الصور تلقائيًا بحيث يكون ذلك أهم إجراء في خطوات العمل.

وهناك العديد من الأدوات التي يمكن استخدامها، منها ضغط البيانات غير المنقوص على ملفات `JPG` و`PNG`، بدون تأثر جودة الصورة. بالنسبة إلى `JPG`، جرب [jpegtran](http://jpegclub.org/) أو [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (المتاح على نظام التشغيل Linux فقط؛ مع تشغيل الخيار `تجريد الكل`). بالنسبة إلى `PNG` جرب [OptiPNG](http://optipng.sourceforge.net/) أو [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" adjust_indentation="auto" %}
</pre>

يمكن من خلال أسلوب النقوش المتحركة في CSS الجمع بين عدة صور في صورة تُعرف باسم `ورقة نقوش متحركة`. وبعد ذلك يمكن استخدام الصور الفردية من خلال تحديد صورة الخلفية للعنصر (ورقة النقوش المتحركة) بالإضافة إلى تعويض لعرض الجزء المناسب.

The above loads the appropriate asset in browsers that support image-set; otherwise it falls back to the 1x asset. The obvious caveat is that while `image-set()` browser support is low, most browsers get the 1x asset.

### استخدام استعلامات الوسائط لتحميل الصور الشرطية أو الإخراج الفني

تساعد النقوش المتحركة في خفض عدد مرات التنزيل المطلوبة للحصول على عدة صور، بدون تعطيل ذاكرة التخزين المؤقت.

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

يمكن أن يؤدي التحميل الكسول إلى تسريع التحميل على الصفحات الطويلة التي تتضمن العديد من الصور في الجزء غير المرئي من الصفحة من خلال تحميلها عند اللزوم أو بمجرد انتهاء المحتوى الأساسي من التحميل والعرض. وبالإضافة إلى تحسينات الأداء، يمكن أن يؤدي استخدام التحميل الكسول إلى إنشاء تجارب تمرير لا نهائية.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/media-query-dppx.html" region_tag="mqdppx" adjust_indentation="auto" %}
</pre>

توخ الحذر عند إنشاء صفحات تمرير لا نهائية، نظرًا لأنه يتم تحميل المحتوى عندما يكون مرئيًا، ولن يظهر هذا المحتوى أبدًا لمحركات البحث. بالإضافة إلى ذلك، لن يظهر تذييل الصفحة أبدًا للمستخدمين الذين يبحثون عن معلومات يتوقعون ظهورها في تذييل الصفحة نظرًا لأنه يتم تحميل محتوى جديد باستمرار.

{# include shared/related_guides.liquid inline=true list=page.related-guides.optimize #}

    @media (min-width: 500px) {
      body {
        background-image: url(bg.png);
      }
    }
    

## تحسين أداء الصور

أحيانًا تكون أفضل صورة ليست صورة إطلاقًا. ومتى أمكن، استخدم الميزات الأصلية للمتصفح لتوفير الوظائف نفسها أو أخرى شبيهة. تنشئ المتصفحات مواد مصورة كان يلزمها في السابق توفير صور. وهذا يعني أن المتصفحات لم تعد بحاجة إلى تنزيل ملفات صور منفصلة وأنها تحجب الصور المعدلة من حيث الحجم تعديلاً غير ملائم. يمكن عرض الرموز باستخدام ترميز موحد أو خطوط رموز خاصة.

### استخدام مجموعة صور لتوفير صور عالية الدقة

* تعد رسومات متجهية يمكن ضبطها بشكل لا نهائي.

### استخدام طلبات بحث الوسائط لتوفير صور عالية الدقة أو إخراج فني

متى أمكن، يجب أن يكون النص نصًا وليس مضمَّنًا داخل صور، كاستخدام الصور في العناوين، أو وضع معلومات الاتصال مثل أرقام الهاتف أو العناوين في الصور مباشرة. وبذلك لن يتمكن المستخدمون من نسخ المعلومات أو لصقها، وتصبح المعلومات غير متاحة لأجهزة قراءة الشاشة ولن تكون سريعة الاستجابة. بدلاً من ذلك، ضع النصوص في الترميز واستخدم خطوط الويب عند اللزوم لأرشفة النمط اللازم.

يمكن للمتصفحات الحديثة استخدام ميزات CSS لإنشاء أنماط كانت من قبل تتطلب صورًا. على سبيل المثال، يمكن إنشاء التدرجات المعقدة باستخدام العنصر `background`، كما يمكن إنشاء الظلال باستخدام `box-shadow`، ويمكن إضافة الجوانب المستديرة باستخدام العنصر`border-radius`.

Including a unicode character is done in the same way named entities are: `&#XXXX`, where `XXXX` represents the unicode character number. For example:

    You're a super &#9733;
    

ضع في اعتبارك أن استخدام هذه التقنيات لا يتطلب عرض الدوائر، الأمر الذي يظهر جليًا في الجوّال. وعند المبالغة في استخدام هذه التقنيات، ستفقد أية ميزة قد تكون حصلت عليها وقد يؤدي ذلك إلى إعاقة الأداء.

### TL;DR {: .hide-from-toc }

For more complex icon requirements, SVG icons are generally lightweight, easy to use, and can be styled with CSS. SVG have a number of advantages over raster images:

* هي عبارة عن رسومات متجهية يمكن ضبطها بشكل لا نهائي، ولكنها قد تكون ذات ظلال وينتج عنها رموز ليست بالحدة المتوقعة.
* أنماط محدودة باستخدام CSS.
* يمكن أن يكون تحديد الموضع المثالي لوحدة بكسل صعبًا، بناءً على طول السطر ومسافة الحرف وما إلى ذلك.
* ليست دلالية، وقد يصعب استخدامها مع أجهزة قراءة الشاشة أو التقنيات المساعدة الأخرى.
* ما لم يتم تحديد الهدف منها على نحو سليم، قد تؤدي هذه الصور إلى زيادة حجم الملف بسبب استخدام مجموعة فرعية صغيرة فقط من الرموز المتاحة.

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-svg.html){: target="_blank" .external }

### استبدال الرموز البسيطة بالترميز الموحد<figure class="attempt-right"> 

<img src="img/icon-fonts.png" class="center" srcset="img/icon-fonts.png 1x, img/icon-fonts-2x.png 2x" alt="Example of a page that uses FontAwesome for its font icons." /> <figcaption> <a href="https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html" target="_blank" class="external"> Example of a page that uses FontAwesome for its font icons. </a> </figcaption> </figure> 

Icon fonts are popular, and can be easy to use, but have some drawbacks compared to SVG icons:

* احذر الاختيار العشوائي لتنسيق الصورة، واستوعب جيدًا التنسيقات المختلفة المتاحة واستخدم أفضل تنسيق مناسب.
* ضمِّن أدوات تحسين الصور وضغطها في خطوات العمل لتقليل حجم الملفات.
* قلل عدد طلبات http من خلال وضع الصور شائعة الاستخدام في نقوش متحركة.
* جرب تحميل الصور بعد تمريرها في العرض فقط لتحسين وقت التحميل الأول للصور وخفض الوزن الأول للصفحة.
* Unless properly scoped, they can result in a large file size for only using a small subset of the icons available.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/icon-font.html" region_tag="iconfont" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/icon-font.html){: target="_blank" .external }

There are hundreds of free and paid icon fonts available including [Font Awesome](https://fortawesome.github.io/Font-Awesome/), [Pictos](http://pictos.cc/){: .external }, and [Glyphicons](https://glyphicons.com/).

Be sure to balance the weight of the additional HTTP request and file size with the need for the icons. For example, if you only need a handful of icons, it may be better to use an image or an image sprite.

## تجنب الصور تمامًا

Images often account for most of the downloaded bytes and also often occupy a significant amount of the visual space on the page. As a result, optimizing images can often yield some of the largest byte savings and performance improvements for your website: the fewer bytes the browser has to download, the less competition there is for client's bandwidth and the faster the browser can download and display all the assets.

### استبدال الرموز المعقدة بـ SVG

* استخدم `JPG` مع الصور الفوتوغرافية.
* استخدم `SVG` مع الصور المتجهية والرسومات الثابتة الملونة مثل الشعارات والرسم الخطي. وإذا لم يكن الرسم المتجهي متاحًا، فجرب استخدام WebP أو PNG.
* استخدم `PNG` وليس `GIF` نظرًا لأن ذلك يوفر مزيدًا من الألوان ونسب ضغط أفضل.
* بالنسبة إلى الرسوم المتحركة الأطول، جرب استخدام `<video>` الذي يوفر جودة صور أفضل ويوفر للمستخدم إمكانية التحكم في التشغيل.

### الحذر عند استخدام خطوط الرموز

There are two types of images to consider: [vector images](https://en.wikipedia.org/wiki/Vector_graphics) and [raster images](https://en.wikipedia.org/wiki/Raster_graphics). For raster images, you also need to choose the right compression format, for example: `GIF`, `PNG`, `JPG`.

**Raster images**, like photographs and other images, are represented as a grid of individual dots or pixels. Raster images typically come from a camera or scanner, or can be created in the browser with the `canvas` element. As the image size gets larger, so does the file size. When scaled larger than their original size, raster images become blurry because the browser needs to guess how to fill in the missing pixels.

**Vector images**, such as logos and line art, are defined by a set of curves, lines, shapes, and fill colors. Vector images are created with programs like Adobe Illustrator or Inkscape and saved to a vector format like [`SVG`](https://css-tricks.com/using-svg/). Because vector images are built on simple primitives, they can be scaled without any loss in quality or change in file size.

When choosing the appropriate format, it is important to consider both the origin of the image (raster or vector), and the content (colors, animation, text, etc). No one format fits all image types, and each has its own strengths and weaknesses.

Start with these guidelines when choosing the appropriate format:

* Use `JPG` for photographic images.
* Use `SVG` for vector art and solid color graphics such as logos and line art. If vector art is unavailable, try `WebP` or `PNG`.
* Use `PNG` rather than `GIF` as it allows for more colors and offers better compression ratios.
* For longer animations consider using `<video>`, which provides better image quality and gives the user control over playback.

### TL;DR {: .hide-from-toc }

You can reduce image file size considerably by "post-processing" the images after saving. There are a number of tools for image compression&mdash;lossy and lossless, online, GUI, command line. Where possible, it's best to try automating image optimization so that it's a first-class citizen in your workflow.

Several tools are available that perform further, lossless compression on `JPG` and `PNG` files with no effect on image quality. For `JPG`, try [jpegtran](http://jpegclub.org/) or [jpegoptim](http://freshmeat.net/projects/jpegoptim/) (available on Linux only; run with the --strip-all option). For `PNG`, try [OptiPNG](http://optipng.sourceforge.net/) or [PNGOUT](http://www.advsys.net/ken/util/pngout.htm).

### اختيار التنسيق المناسب

<img src="img/sprite-sheet.png" class="attempt-right" alt="Image sprite sheet used in example" />

CSS spriting is a technique whereby a number of images are combined into a single "sprite sheet" image. You can then use individual images by specifying the background image for an element (the sprite sheet) plus an offset to display the correct part.

<div style="clear:both;">
</div>

<pre class="prettyprint">{% includecode content_path="web/fundamentals/design-and-ux/responsive/_code/image-sprite.html" region_tag="sprite" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/design-and-ux/responsive/image-sprite.html){: target="_blank" .external }

Spriting has the advantage of reducing the number of downloads required to get multiple images, while still enabling caching.

### خفض حجم الملف

Lazy loading can significantly speed up loading on long pages that include many images below the fold by loading them either as needed or when the primary content has finished loading and rendering. In addition to performance improvements, using lazy loading can create infinite scrolling experiences.

Be careful when creating infinite scrolling pages&mdash;because content is loaded as it becomes visible, search engines may never see that content. In addition, users who are looking for information they expect to see in the footer, never see the footer because new content is always loaded.

## Avoid images completely

Sometimes the best image isn't actually an image at all. Whenever possible, use the native capabilities of the browser to provide the same or similar functionality. Browsers generate visuals that would have previously required images. This means that browsers no longer need to download separate image files thus preventing awkwardly scaled images. You can use unicode or special icon fonts to render icons.

### استخدام النقوش المتحركة

Wherever possible, text should be text and not embedded into images. For example, using images for headlines or placing contact information&mdash;like phone numbers or addresses&mdash;directly into images prevents users from copying and pasting the information; it makes the information inaccessible for screen readers, and it isn't responsive. Instead, place the text in your markup and if necessary use webfonts to achieve the style you need.

### تجربة التحميل الكسول

Modern browsers can use CSS features to create styles that would previously have required images. For example: complex gradients can be created using the `background` property, shadows can be created using `box-shadow`, and rounded corners can be added with the `border-radius` property. 

<style>
  p#noImage {
    margin-top: 2em;
    padding: 1em;
    padding-bottom: 2em;
    color: white;
    border-radius: 5px;
    box-shadow: 5px 5px 4px 0 rgba(9,130,154,0.2);
    background: linear-gradient(rgba(9, 130, 154, 1), rgba(9, 130, 154, 0.5));
  }

  p#noImage code {
    color: rgb(64, 64, 64);
  }
</style>

 

<p id="noImage">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque sit amet augue eu magna scelerisque porta ut ut dolor. Nullam placerat egestas nisl sed sollicitudin. Fusce placerat, ipsum ac vestibulum porta, purus dolor mollis nunc, pharetra vehicula nulla nunc quis elit. Duis ornare fringilla dui non vehicula. In hac habitasse platea dictumst. Donec ipsum lectus, hendrerit malesuada sapien eget, venenatis tempus purus.
</p>

    <style>
      div#noImage {
        color: white;
        border-radius: 5px;
        box-shadow: 5px 5px 4px 0 rgba(9,130,154,0.2);
        background: linear-gradient(rgba(9, 130, 154, 1), rgba(9, 130, 154, 0.5));
      }
    </style>
    

Keep in mind that using these techniques does require rendering cycles, which can be significant on mobile. If over-used, you'll lose any benefit you may have gained and it may hinder performance.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}