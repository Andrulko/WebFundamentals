project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: المورد الأسرع والأفضل هو المورد الذي لم يتم إرساله. هل سبقت لك مراجعة مواردك مؤخرًا؟ يجب إجراء ذلك من آن لآخر لضمان تمكن كل مورد من ترك أفضل انطباع لدى المستخدم.

{# wf_updated_on: 2014-04-28 #} {# wf_published_on: 2014-03-31 #}

# التخلص من التنزيلات غير اللازمة {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

### TL;DR {: .hide-from-toc }

* أصول المستودع المملوكة بالكامل والتابعة لجهات خارجية على الصفحات
* 'قياس مستوى أداء كل أصل من الأصول: قيمته ومستوى أدائه الفني'
* Determine if the resources are providing sufficient value.

المورد الأسرع والأفضل هو المورد الذي لم يتم إرساله. هل سبقت لك مراجعة مواردك مؤخرًا؟ يجب إجراء ذلك من آن لآخر لضمان تمكن كل مورد من ترك أفضل انطباع لدى المستخدم.

* You've always included resource X on your pages, but does the cost of downloading and displaying it offset the value it delivers to the user? Can you measure and prove its value?
* Does the resource (especially if it's a third-party resource) deliver consistent performance? Is this resource in the critical path, or need to be? If the resource is in the critical path, could it be a single point of failure for the site? That is, if the resource is unavailable, does it affect performance and the user experience of your pages?
* Does this resource need or have an SLA? Does this resource follow performance best practices: compression, caching, and so on?

كثيرًا ما تتضمن الصفحات موارد غير لازمة، والأسوأ أن يتم تعطيل أداء الصفحة بدون إضافة قيمة لزائر موقع الويب الذي يستضيف هذه الصفحات. ويسري هذا أيضًا على موارد وأدوات الجهات المعنية والجهات الخارجية:

* Site A has decided to display a photo carousel on its homepage to allow the visitor to preview multiple photos with a quick click. All of the photos are loaded when the page is loaded, and the user advances through the photos. 
    * **Question:** Have you measured how many users view multiple photos in the carousel? You might be incurring high overhead by downloading resources that most visitors never view.
* Site B has decided to install a third-party widget to display related content, improve social engagement, or provide some other service. 
    * **Question:** Have you tracked how many visitors use the widget or click-through on the content that the widget provides? Is the engagement that this widget generates enough to justify its overhead?

كما ترى، على الرغم من أن عبارة التخلص من التنزيلات غير اللازمة تبدو عبارة رنانة، في الواقع قد يكون الأمر مختلفًا كثيرًا لأنه يتطلب الكثير من التفكير الحذر والقياس كذلك. وللحصول على أفضل النتائج يجب في الحقيقة إجراء جرد وإعادة النظر في هذه الأسئلة من آن لآخر بالنسبة إلى كل أصل يظهر على الصفحات.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}