project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: يتطلب التعرف على نقاط التكدس في أداء مسار العرض الحرج وحلها معرفة جيدة بالمخاطر الشائعة. يمكننا الآن تنفيذ جولة عملية واستخلاص أنماط الأداء الشائعة التي تساعدك في تحسين صفحاتك.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# تحليل أداء مسار العرض الحرج {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

يتطلب التعرف على نقاط التكدس في أداء مسار العرض الحرج وحلها معرفة جيدة بالمخاطر الشائعة. يمكننا الآن تنفيذ جولة عملية واستخلاص أنماط الأداء الشائعة التي تساعدك في تحسين صفحاتك.

الهدف من تحسين مسار العرض الحرج هو السماح للمتصفح برسم الصفحة في أسرع وقت ممكن: ذلك أن الصفحات الأسرع تترجم إلى نسبة تفاعل أعلى وأرقام صفحات مشاهدة و[نسبة تحول محسنة](http://www.google.com/think/multiscreen/success.html). ونتيجة لذلك، نريد الحد من مقدار الوقت الذي يحتاج إليه الزائر للبدء في شاشة فارغة من خلال تحسين الموارد التي يتم تحميلها وترتيب ذلك.

وللمساعدة في توضيح هذه العملية، يمكننا البدء بأبسط حالة ممكنة والمتابعة بتصميم الصفحة لتضمين المزيد من الموارد والأنماط ومنطق التطبيق - وخلال العملية سنشاهد كذلك أشياء يمكن ألا تسير على ما يرام، وكيفية تحسين كل حالة من هذه الحالات.

وفي النهاية، يجب التنبيه على شيء قبل البدء... حتى الآن كان التركيز فقط على ما يحدث في المتصفح بعد أن يتوفر المورد (سواء أكان ملف CSS أو جافا سكريبت أو HTML) للمعالجة وتجاهلنا الوقت اللازم لجلبه سواء من ذاكرة التخزين المؤقت أو من الشبكة. سنتناول بالتفصيل كيفية تحسين عناصر الشبكات في التطبيق خلال الدرس التالي، ولكن في الوقت الحالي (وحتى تكون الأمور أكثر واقعية) سنفترض ما يلي:

* زمن انتقال الجولة (وقت استجابة النشر) على الخادم سيستغرق 100 ملي ثانية
* سيكون وقت استجابة الخادم 100 ملي ثانية لمستند HTML و10 ملي ثانية لجميع الملفات الأخرى

## تجربة Hello World

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

سنبدأ بترميز HTML الأساسي وصورة واحدة - بدون استخدام CSS أو جافا سكريبت - حتى يكون الأمر أكثر بساطة. والآن يمكننا فتح مخطط زمني للشبكة في Chrome DevTools وفحص تدفق الموارد الناتجة:

We'll start with basic HTML markup and a single image; no CSS or JavaScript. Let's open up our Network timeline in Chrome DevTools and inspect the resulting resource waterfall:

<img src="images/waterfall-dom.png" alt="CRP" />

وبعد توفر محتوى HTML، يكون على المتصفح تحليل وحدات بايت، وتحويلها إلى رموز مميزة، وتصميم شجرة DOM. لاحظ أن DevTools تعرض وقت حدث DOMContentLoaded على نحو سهل في الأسفل (216 ملي ثانية)، مع الاستجابة أيضًا للسطر العمودي الأزرق. تمثل الفجوة بين نهاية تنزيل HTML والسطر العمودي الأزرق (DOMContentLoaded) الوقت الذي يستغرقه المتصفح في تصميم شجرة DOM، في هذه الحالة مجرد عدد بسيط من الميلي ثانية.

وأخيرًا، هناك ملاحظة مثيرة: `الصورة الرائعة` الخاصة بنا لم تحظر حدث domContentLoaded. اتضح لنا في النهاية أنه يمكننا إنشاء شجرة العرض ورسم الصفحة بدون انتظار كل أصل من الأصول على الصفحة: **لا تعد جميع الموارد حرجة لعرض الرسم الأول والسريع**. في الحقيقة، وكما سنرى، عند التحدث عن مسار العرض الحرج، سنجد أننا نتحدث عادة عن ترميز HTML وCSS وجافا سكريبت. لا تحظر الصور العرض المبدئي للصفحة - ولكننا بالطبع يجب أن نحاول التأكد من الحصول على رسم للصور في أقرب وقت ممكن أيضًا.<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

ومع ذلك، يتم حظر الحدث `load` (يُعرف أيضًا باسم `onload`) على الصورة: تشير DevTools إلى الحدث أثناء التحميل عند 335 ملي ثانية. تذكر أن حدث onload يشير إلى النقطة عندما تم تنزيل ومعالجة **جميع الموارد** المطلوبة في الصفحة - وهذه النقطة تكون عند توقف أداة الانتقاء في التحميل عن الانتقاء في المتصفح وتتم الإشارة إلى هذه النقطة بسطر عمودي أحمر في التدفق.

قد تبدو صفحة `تجربة Hello World` بسيطة على السطح، ولكن هناك الكثير من التفاصيل التي تؤدي إلى ظهور النتيجة في النهاية. ولذلك سنحتاج عمليًا إلى أكثر من مجرد HTML: ربما نحتاج إلى ورقة أنماط CSS ونص برمجي واحد أو أكثر لإضافة بعض التفاعل على الصفحة. والآن يمكننا إضافتهما إلى المزيج لنرى ما سيحدث:

That said, the `load` event (also known as `onload`), is blocked on the image: DevTools reports the `onload` event at 335ms. Recall that the `onload` event marks the point at which **all resources** that the page requires have been downloaded and processed; at this point, the loading spinner can stop spinning in the browser (the red vertical line in the waterfall).

## إضافة جافا سكريبت وCSS إلى المزيج

Our "Hello World experience" page seems simple but a lot goes on under the hood. In practice we'll need more than just the HTML: chances are, we'll have a CSS stylesheet and one or more scripts to add some interactivity to our page. Let's add both to the mix and see what happens:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_timing.html){: target="_blank" .external }

*Before adding JavaScript and CSS:*

<img src="images/waterfall-dom.png" alt="DOM CRP" />

*With JavaScript and CSS:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

Adding external CSS and JavaScript files adds two extra requests to our waterfall, all of which the browser dispatches at about the same time. However, **note that there is now a much smaller timing difference between the `domContentLoaded` and `onload` events.**

What happened?

* على العكس من مثال HTML الصريح، نحتاج الآن أيضًا إلى جلب ملف CSS وتحليله لإنشاء CSSOM، ونحن نعرف أننا بحاجة إلى كل من DOM وCSSOM لتصميم شجرة العرض.
* ونظرًا لأن لدينا أيضًا ملف جافا سكريبت لحظر المحلل على الصفحة، فقد تم حظر حدث domContentLoaded حتى يتم تنزيل ملف CSS وتحليله: وقد يستعلم جافا سكريبت عن CSSOM، ولذلك يجب حظر CSS وانتظاره حتى نتمكن من تنفيذ جافا سكريبت.

**What if we replace our external script with an inline script?** Even if the script is inlined directly into the page, the browser can't execute it until the CSSOM is constructed. In short, inlined JavaScript is also parser blocking.

That said, despite blocking on CSS, does inlining the script make the page render faster? Let's try it and see what happens.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*جافا سكريبت (الخارجي) لحظر المحلل:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM وCSSOM وجافا سكريبت" />

We are making one less request, but both our `onload` and `domContentLoaded` times are effectively the same. Why? Well, we know that it doesn't matter if the JavaScript is inlined or external, because as soon as the browser hits the script tag it blocks and waits until the CSSOM is constructed. Further, in our first example, the browser downloads both CSS and JavaScript in parallel and they finish downloading at about the same time. In this instance, inlining the JavaScript code doesn't help us much. But there are several strategies that can make our page render faster.

First, recall that all inline scripts are parser blocking, but for external scripts we can add the "async" keyword to unblock the parser. Let's undo our inlining and give that a try:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

بدلاً من ذلك، فقد تمكنا من تجربة طريقة أخرى وضمَّنا كلاً من CSS وجافا سكريبت:

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

تتكون أبسط صفحة من ترميز HTML: بدون CSS أو جافا سكريبت أو أي نوع آخر من الموارد. ولعرض هذه الصفحة، يتعين على المتصفح بدء الطلب، وانتظار وصول مستند HTML، وتحليله، وتصميم DOM، وفي النهاية عرضه على الشاشة:

Alternatively, we could have inlined both the CSS and JavaScript:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**يحدد الزمن بين T<sub>0</sub> و T<sub>1</sub> captures the مرات معالجة الشبكة والخادم.** وفي أفضل الحالات (إذا كان ملف HTML صغيرًا)، لن نحتاج إلا إلى انتقال جولة شبكة واحد لجلب المستند بالكامل - نظرًا لكيفية عمل بروتوكولات نقل TCP، قد تتطلب الملفات الكبيرة مزيدًا من الجولات، وسنتناول هذا الموضوع في درس لاحق. **نتيجة لذلك، يمكننا القول بأن الصفحة الواردة أعلاه تتضمن مسار عرض حرج بجولة واحدة (حد أدنى).**

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" />

Notice that the `domContentLoaded` time is effectively the same as in the previous example; instead of marking our JavaScript as async, we've inlined both the CSS and JS into the page itself. This makes our HTML page much larger, but the upside is that the browser doesn't have to wait to fetch any external resources; everything is right there in the page.

مجددًا سنتحمل انتقال جولة شبكة لجلب مستند HTML ولنعرف بعد ذلك من الترميز الذي نسترده أننا سنحتاج كذلك إلى ملف CSS: وهذا يعني أنه يتعين على المتصفح الرجوع إلى الخادم والحصول على CSS حتى يتمكن من عرض الصفحة على الشاشة. **نتيجة لذلك، ستتحمل هذه الصفحة انتقال جولتين كحد أدنى حتى يتم عرض الصفحة** - ومجددًا، قد يستغرق ملف CSS عدة جولات، ومن ثم نركز على أنه `كحد أدنى`.

دعونا نعرِّف المفردات التي سنستخدمها لوصف مسار العرض الحرج:

## أنماط الأداء

الآن، يمكننا مقارنة ذلك بخصائص المسار الحرج في المثال HTML + CSS الوارد أعلاه:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

<img src="images/analysis-dom.png" alt="Hello world CRP" />

والآن يمكننا إضافة ملف جافا سكريبت إضافي في المزيج.

لقد أضفنا app.js، الذي يعد أصلاً خارجيًا من أصول جافا سكريبت على الصفحة، وكما نعرف من الآن، فهو مورد يحظر المحلل (أي حرج). والأسوأ من ذلك، حتى يمكننا تنفيذ ملف جافا سكريبت، يجب أيضًا أن نحظر CSSOM وننتظره - تذكر أنه يمكن لجافا سكريبت الاستعلام عن CSSOM ومن ثم سيتوقف المتصفح مؤقتًا حتى يتم تنزيل `style.css` ويتم إنشاء CSSOM.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

الآن أصبحت لدينا ثلاثة موارد حرجة تضيف حتى 11 كيلوبايت من وحدات بايت الحرجة، إلا أن طول المسار الحرج لا يزال جولتين نظرًا لأنه يمكننا نقل CSS وجافا سكريبت بالتوازي. **يعني التوصل إلى ميزات مسار العرض الحرج التمكن من تحديد الموارد الحرجة وكذلك استيعاب كيفية جدولة المتصفح لعمليات الجلب.** والآن يمكننا استكمال المثال…

بعد طرح الأمر على أحد مطوِّري الموقع اكتشفنا أن جافا سكريبت الذي ضمَّناه في الصفحة لا يحتاج إلى الحظر: ذلك أن لدينا بعض الإحصاءات وشفرة أخرى لا تحتاج إلى حظر عرض الصفحة. وبمعرفة ذلك، أصبح بإمكاننا إضافة السمة `async` إلى علامة النص البرمجي لإلغاء حظر المحلل:

* **المورد الحرج:** هو المورد الذي قد يحظر العرض الأولي للصفحة.
* **طول المسار الحرج:** عدد الجولات أو إجمالي الوقت المطلوب لجلب جميع الموارد الحرجة.
* **وحدات بايت الحرجة:** إجمالي عدد وحدات بايت المطلوبة للحصول على العرض الأول للصفحة، وهو مجموع أحجام ملفات النقل في الموارد الحرجة. كان المثال الأول المتعلق بصفحة HTML المفردة يتضمن موردًا حرجًا واحدًا (مستند HTML)، كما كان طول المسار الحرج أيضًا يساوي جولة شبكة واحدة (بافتراض أن حجم الملف صغير)، وكان إجمالي عدد وحدات بايت الحرج هو حجم نقل مستند HTML نفسه.

Now let's compare that to the critical path characteristics of the HTML + CSS example above:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** من الموارد الحرجة
* **2** أو أكثر من الجولات لطول المسار الحرج كحد أدنى
* **9** كيلوبايت من وحدات بايت الحرجة

نتيجة لذلك، ترجع الصفحة المحسنة إلى موردين حرجين (HTML وCSS)، بطول مسار حرج يبلغ جولتين كحد أدنى، وإجمالي 9 كيلوبايت من وحدات بايت الحرجة.

وفي الختام، هل يمكن القول بأن ورقة أنماط CSS لم تكن لازمة إلا للطباعة فقط؟ كيف كان سيبدو ذلك؟

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

نظرًا لأن المورد style.css لا يُستخدم إلا للطباعة، فلن يحتاج المتصفح إلى حظره لعرض الصفحة. ومن ثم، بمجرد اكتمال إنشاء DOM، تصبح لدى المتصفح معلومات كافية لعرض الصفحة. ونتيجة لذلك، تتضمن هذه الصفحة موردًا حرجًا واحدًا فقط (مستند HTML)، ويكون طول مسار العرض الحرج جولة واحدة كحد أدنى.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

That said, in practice if we look at this page's "network waterfall," you'll see that both the CSS and JavaScript requests are initiated at about the same time; the browser gets the HTML, discovers both resources, and initiates both requests. As a result, the above page has the following critical path characteristics:

* **3** موارد حرجة
* **2** أو أكثر من الجولات لطول المسار الحرج كحد أدنى
* **11** كيلوبايت من وحدات بايت الحرجة

We now have three critical resources that add up to 11KB of critical bytes, but our critical path length is still two roundtrips because we can transfer the CSS and JavaScript in parallel. **Figuring out the characteristics of your critical rendering path means being able to identify the critical resources and also understanding how the browser will schedule their fetches.** Let's continue with our example.

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

An asynchronous script has several advantages:

* لم يعد النص البرمجي يحظر المحلل وليس جزءًا من مسار العرض الحرج
* نظرًا لأنه ليست هناك أية نصوص برمجية حرجة، فلن يحتاج CSS أيضًا إلى حظر الحدث domContentLoaded
* كلما كان بدء حدث domContentLoaded أسرع، كان بدء تنفيذ منطق التطبيق أسرع

As a result, our optimized page is now back to two critical resources (HTML and CSS), with a minimum critical path length of two roundtrips, and a total of 9KB of critical bytes.

Finally, if the CSS stylesheet were only needed for print, how would that look?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

Because the style.css resource is only used for print, the browser doesn't need to block on it to render the page. Hence, as soon as DOM construction is complete, the browser has enough information to render the page. As a result, this page has only a single critical resource (the HTML document), and the minimum critical rendering path length is one roundtrip.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}