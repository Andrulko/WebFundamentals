project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: لا يمكنك تحسين ما لا يمكنك قياسه. ولكن من حسن الطالع أن واجهة برمجة تطبيقات Navigation Timing تتيح لنا جميع الأدوات اللازمة لقياس كل خطوة في مسار العرض الحرج.

{# wf_updated_on: 2014-09-17 #} {# wf_published_on: 2014-03-31 #}

# قياس مسار العرض الحرج باستخدام توقيت التنقل {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

لا يمكنك تحسين ما لا يمكنك قياسه. ولكن من حسن الطالع أن واجهة برمجة تطبيقات Navigation Timing تتيح لنا جميع الأدوات اللازمة لقياس كل خطوة في مسار العرض الحرج.

* توفر Navigation Timing طوابع زمنية عالية الدقة لقياس CRP.
* يرسل المتصفح سلسلة من الأحداث القابلة للاستهلاك لتحديد عدة مراحل من CRP.

يعتمد أساس كل إستراتيجية أداء ثابتة على القياس الجيد والأدوات المفيدة. وقد اتضح لنا أن هذا ما توفره واجهة برمجة تطبيقات Navigation Timing تحديدًا.

## Auditing a page with Lighthouse {: #lighthouse }

Lighthouse is a web app auditing tool that runs a series of tests against a given page, and then displays the page's results in a consolidated report. You can run Lighthouse as a Chrome Extension or NPM module, which is useful for integrating Lighthouse with continuous integration systems.

تمثل كل علامة من العلامات الواردة في الرسم البياني أعلاه طابعًا زمنيًا عالي الدقة يتتبعه المتصفح مع كل صفحة يحملها. وفي الحقيقة، في هذه الحالة تحديدًا لا نعرض سوى جزء من جميع الطوابع الزمنية المختلفة - ذلك أننا في الوقت الحالي نتخطى جميع الطوابع الزمنية المتعلقة بالشبكة، ولكننا سنتناولها في درس لاحق.

ولكن ما المقصود بهذه الطوابع الزمنية؟

![Lighthouse's CRP audits](images/lighthouse-crp.png)

قد يبدو المثال الوارد أعلاه مفزعًا لأول وهلة، إلا أنه بسيط للغاية. تحدد واجهة برمجة تطبيقات Navigation Timing جميع الطوابع الزمنية ذات الصلة وتنتظر الشفرة الخاصة بنا بدء تشغيل الحدث `onload` event &mdash; تذكر أنه يتم تشغيل حدث onload بعد domInteractive وdomContentLoaded وdomComplete &mdash; كما تحسب الفرق بين الطوابع الزمنية المختلفة.

## Instrumenting your code with the Navigation Timing API {: #navigation-timing }

بعد أن أوضحنا كل شيء نظريًا وعمليًا، أصبحت لدينا الآن بعض المعالم التي يمكننا تتبعها ووظيفة بسيطة للحصول على هذه المقاييس. لاحظ أنه بدلاً من طباعة هذه المقاييس على الصفحة، يمكنك أيضًا تعديل الشفرة لإرسالها إلى خادم إحصائي ([Google Analytics يتولى تنفيذ ذلك تلقائيًا](https://support.google.com/analytics/answer/1205784))، وهي طريقة رائعة للحفاظ على أداء علامات التبويب في صفحاتك وتحديد الصفحات المرشحة التي يمكنها الاستفادة من بعض التحسين.

<img src="images/dom-navtiming.png"  alt="Navigation Timing" />

Each of the labels in the above diagram corresponds to a high resolution timestamp that the browser tracks for each and every page it loads. In fact, in this specific case we're only showing a fraction of all the different timestamps &mdash; for now we're skipping all network related timestamps, but we'll come back to them in a future lesson.

So, what do these timestamps mean?

* **domLoading:** هذا الطابع الزمني لبدء العملية بالكامل، حيث يكون المتصفح على وشك بدء تحليل وحدات بايت الأولى التي يتم الحصول عليها من HTML المستند.
* **domInteractive:** يحدد نقطة انتهاء المتصفح من تحليل كل بنية HTML وDOM
* `domContentLoaded`يحدد نقطة استعداد DOM ووجود أوراق أنماط تحظر تنفيذ جافا سكريبت - مما يعني أنه يمكننا الآن (بشكل محتمل) إنشاء شجرة العرض. 
    * تنتظر العديد من إطارات عمل جافا سكريبت هذا الحدث حتى تبدأ تنفيذ منطقها الخاص. لذلك يحدد المتصفح الطابعين الزمنيين *EventStart* و*EventEnd* للسماح لنا بتتبع المدة التي يستغرقها هذا التنفيذ.
* **domComplete:** كما يتضح من الاسم، تكتمل المعالجة ويكتمل تنزيل جميع الموارد على الصفحة (مثل الصور وما إلى ذلك) أي تتوقف أداة الانتقال في التحميل عن الانتقاء.
* **loadEvent:** كخطوة أخيرة عند تحميل كل صفحة، يشغِّل المتصفح حدث `onload` يمكنه تشغيل منطق تطبيق إضافي.

The HTML specification dictates specific conditions for each and every event: when it should be fired, which conditions should be met, and so on. For our purposes, we'll focus on a few key milestones related to the critical rendering path:

* **domInteractive** لتحديد وقت استعداد DOM.
* `domContentLoaded` يستخدم عادة لتحديد وقت [استعداد كل من DOM و CSSOM](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/). 
    * إذا لم يكن هناك جافا سكريبت يحظر المحلل، فسيتم تشغيل *DOMContentLoaded* في الحال بعد *domInteractive*.
* **domComplete** لتحديد وقت استعداد الصفحة وجميع مواردها الفرعية.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp.html){: target="_blank" .external }

The above example may seem a little daunting on first sight, but in reality it is actually pretty simple. The Navigation Timing API captures all the relevant timestamps and our code simply waits for the `onload` event to fire &mdash; recall that `onload` event fires after `domInteractive`, `domContentLoaded` and `domComplete` &mdash; and computes the difference between the various timestamps.

<img src="images/device-navtiming-small.png"  alt="NavTiming demo" />

All said and done, we now have some specific milestones to track and a simple function to output these measurements. Note that instead of printing these metrics on the page you can also modify the code to send these metrics to an analytics server ([Google Analytics does this automatically](https://support.google.com/analytics/answer/1205784)), which is a great way to keep tabs on performance of your pages and identify candidate pages that can benefit from some optimization work.

## What about DevTools? {: #devtools }

Although these docs sometimes use the Chrome DevTools Network panel to illustrate CRP concepts, DevTools is currently not well-suited for CRP measurements because it does not have a built-in mechanism for isolating critical resources. Run a [Lighthouse](#lighthouse) audit to help identify such resources.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}