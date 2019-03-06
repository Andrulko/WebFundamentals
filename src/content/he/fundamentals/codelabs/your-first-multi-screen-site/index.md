project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: האינטרנט נגיש במגוון עצום של מכשירים. מטלפונים עם מסך קטן ועד לטלביזיות ענק. למד כיצד לבנות אתר שעובד היטב על פני כל המכשירים הללו.

{# wf_updated_on: 2014-01-05 #} {# wf_published_on: 2013-12-31 #}

# האתר הראשון שלך שמותאם למגוון מכשירים {: .page-title }

Caution: This article has not been updated in a while and may not reflect reality. Instead, check out the free [Responsive Web Design](https://www.udacity.com/course/responsive-web-design-fundamentals--ud893){: .external } course on Udacity.

{% include "web/_shared/contributors/paulkinlan.html" %}

<img src="images/finaloutput-2x.jpg" alt="many devices showing the final project" class="attempt-right" />

Creating multi-device experiences is not as hard as it might seem. In this guide, we will build a product landing page for the [CS256 Mobile Web Development course](https://www.udacity.com/course/mobile-web-development--cs256) that works well across different device types.

בנייה עבור מספר מכשירים עם יכולות שונות, דרכי אינטראקציה שונות וכמובן, גדלי מסך שונים יכולה להיראות מרתיעה, אם לא בלתי אפשרית.

זה לא קשה לבנות אתרים מגיבים באופן מלא כמו שנראה לך. כדי לשכנע, מדריך זה לוקח אותך דרך השלבים צעד אחר צעד. אתה מוזמן להשתמש בו כדי להתחיל.  
חלקנו את הדרך לשתי צעדים פשוטים:

1. הגדר את (Information Architecture, IA) ובנה את השלד של העמוד.
2. הוסף את האלמנטים של העיצוב כדי להפוך את העמוד למותאם ודאג שהוא יראה טוב על מגוון מכשירים וגדלי מסך שונים.

## צור את התוכן והמבנה

תוכן הוא ההיבט החשוב ביותר של כל אתר. אז רצוי לעצב עבור התוכן ולא לתת לעיצוב להכתיב את התוכן. במדריך זה, אנו מזהים את התוכן שאנחנו צריכים בעדיפות ראשונה, ליצור מבנה דף המבוסס עליו ולאחר מכן, תציג את הדף בפריסה ליניארית שעובדת היטב על viewports הצר ורחב.

### צור את מבנה הדף

אנו זיהינו שאנחנו צריכים:

1. קטע שיתאר במבט על את המוצר שלנו: "CS256: Mobile web development"
2. טופס לאיסוף מידע ממשתמשים שמעוניינים במוצר שלנו
3. תיאור מעמיק של הוידאו
4. תמונות שמראות את המוצר בפעולה
5. טבלת הנתונים עם מידע כדי לגבות את הטענות

#### צור את הכותרת והטופס

* להוסיף `תכונת controls` כדי להקל על אנשים את ההפעלה של הווידאו.
* הוסף תמונת `poster` לתת לאנשי תצוגה מקדימה של התוכן.
* הוספה מספר `<source>` אלמנטים המבוססים על פורמטי וידאו נתמכים.

אנחנו גם צריכים לבוא עם ארכיטקטורת מידע ופריסה עבור שני viewports הצר ורחב.

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

זה ניתן להמיר בקלות לחלקים של דף שלד שאנו נשתמש לשאר הפרויקט הזה.

### הוסף תוכן לדף

המבנה הבסיסי של האתר הושלם. אנחנו יודעים את הסעיפים שאנחנו צריכים, תוכן שניתן להציג בסעיפים אלו, והיכן למקם אותו בכללי ארכיטקטורת מידע. כעת אנו יכולים להתחיל לבנות את האתר.

Note: הסטייל יגיע מאוחר יותר.

### סיכום

הכותרת וטופס הבקשה הם הרכיבים הקריטיים של בדף שלנו. אלה חייבים להיות מוצגים למשתמש באופן מיידי.

בכותרת, הוסף טקסט פשוט כדי לתאר את המהלך:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addheadline.html" region_tag="headline" adjust_indentation="auto" %}
</pre>

אנחנו צריכים גם למלא את הטופס. זה יהיה טופס פשוט שאוסף שמות של המשתמשים, מספרי הטלפון שלהם, וזמן נוח להם להתקשרות.

כל הטפסים צריכים תוויות ומצייני מיקום כדי להקל על משתמשים להתמקד באלמנטים, להבין מה הוא אמור ללכת בהם, וגם כדי לעזור לכלי נגישות להבין את המבנה של הטופס. שם התכונה (name attribute) לא רק שולח את ערך הטופס לשרת, הוא משמש גם כדי לתת לי רמז חשוב לדפדפן כיצד למלא באופן אוטומטי את הטופס עבור המשתמש.

אנחנו נוסיף סוגים סמנטיים כדי לעשות את זה מהיר ופשוט למשתמשים להיות מסוגלים להזין תוכן במכשיר נייד. לדוגמא, בעת הזנת טלפון מספר, המשתמש צריך רק לראות לוח מקשי חיוג.

{# include shared/related_guides.liquid inline=true list=page.related-guides.create-amazing-forms #}

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addform.html" region_tag="form" adjust_indentation="auto" %}
</pre>

החלק בדף של הוידאו והמידע יכיל יותר עומק. תהיה לה רשימה של התכונות של המוצרים שלנו והחלק הזה, גם יכיל מציין מיקום לוידאו שמראה את המוצר שלנו עובד עבור המשתמש.

#### צור את חלק הוידאו והמידע

סרטי וידאו משמשים לעתים קרובות כדי לתאר את התוכן באופן אינטראקטיבי, והם משמשים לעתים קרובות כדי להראות הדגמה של מוצר או רעיון.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section1" adjust_indentation="auto" %}
</pre>

על ידי ביצוע שיטות העבודה מומלצת הללו, אתה יכול בקלות לשלב וידאו לתוך האתר שלך:

{# include shared/related_guides.liquid inline=true list=page.related-guides.video #}

אתרים ללא תמונות יכולים להיות קצת משעממים. ישנם שני סוגים של תמונות:

* הפוך תמונות תוכן (Content Images) &mdash; תמונות שמוטמעות במסמך משמשות להעברת מידע נוסף על הוכן.
* תמונות לסטייל (Stylistic Image) &mdash; תמונות שמשמשות להעניק לאתר מראה משופר. בדרך כלל, אילו הם תמונות רקע או תמונות תבנית. אנו נעבור על זה בחלק הבא [מאמר הבא](#).
* Add multiple `<source>` elements based on supported video formats.
* Add fall-back text to let people download the video if they can't play it in the window.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addvideo.html" region_tag="video" adjust_indentation="auto" %}
</pre>

קטע התמונות בדף שלנו הוא אוסף של תמונות תוכן.

#### צור את חלק התמונות

תמונות תוכן הן קריטיות להעברת המשמעות של הדף. תחשוב עליהם כמו על התמונות אשר נמצאות בשימוש במאמרי עיתונות. התמונות שאנו משתמשים הן תמונות של המורים על הפרויקט: כריס ווילסון, פיטר לוברס ושון בנט.

* השתמש תמיד בviewport
* תמיד להתחיל עם viewport צר בהתחלה ורק אז לצאת לממשקים רחבים יותר

התמונות מוגדרות בקנה מידה לרוחב של המסך 100%. זה עובד גם על מכשירים עם viewport צר, אבל פחות טוב באלה עם viewport רחב (כמו שולחן עבודה). אנחנו ננהל את זה בסעיף responsive design.

{# include shared/related_guides.liquid inline=true list=page.related-guides.images #}

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addimages.html" region_tag="images" adjust_indentation="auto" %}
</pre>

ישנם אנשים רבים אשר לא יכולים לראות את התמונות ומשתמשים לעתים קרובות בעזר כגון קורא מסך כדי לנתח את הנתונים בדף וטכנולוגיה להעביר את זה למשתמש באופן מילולי. אתה צריך לוודא שכל התוכן של התמונות יכלול את התג התיאורי `alt` שהקורא המסך יכול לדבר אל המשתמש.

בעת הוספה 'תגי alt` יש לוודא שאתה שומר את טקסט alt תמציתי כמו אפשר לתאר את התמונה באופן מלא. לדוגמא בדמו שלנו אנחנו מאתחלים את התכונה להיות "שם: תפקיד", זה מציג מספיק מידע למשתמש להבין כי סעיף זה הוא על המחברים ועל מה העבודה שלהם היא.

החלק האחרון הוא טבלה פשוטה המשמשת כדי להראות מספר סטטיסטיקות מוצר ספציפיות.

יש להשתמש בטבלאות רק לנתונים טבלאיים, כלומר, מטריצות של מידע.

#### הוסף רשימה של מידע

רוב האתרים צריכים כותרת תחתונה כדי להציג תוכן כגון תנאים והגבלות, כתבי ויתור, ותכנים אחרים שאינם אמורים להיות בניווט הראשי או באזור התוכן העיקרי של העמוד.

באתר שלנו, אנו רק מקשרים לתנאי שימוש, דף יצירת קשר, ופרופילי המדיה החברתית שלנו.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="section3" adjust_indentation="auto" %}
</pre>

אנחנו יצרנו את קווי המתאר של האתר וזיהינו את כל האלמנטים מבניים הראשיים. בנוסף, גם דאגנו לכך שיש לנו את כל התוכן העיקרי מוכן ובמקום כדי לספק את הצרכים העסקיים שלנו.

#### הוסף את החלק התחתון (Footer)

Most sites need a footer to display content such as Terms and Conditions, disclaimers, and other content that isn't meant to be in the main navigation or in the main content area of the page.

תוכל להבחין כי העמוד נראה נורא עכשיו; זה הוא מכוון. תוכן הוא ההיבט החשוב ביותר של כל אתר ואנחנו צריכים לוודא שיש לנו את האדריכלות הטובה ביותר להצפת המידע. מדריך זה נתן לנו בסיס מצוין לבנות עליו. אנו נשפר את הסטייל של התוכן שלנו במדריך הבא.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/addcontent.html" region_tag="footer" adjust_indentation="auto" %}
</pre>

האינטרנט נגיש במגוון עצום של מכשירים, מטלפונים קטנים ועד מסכי טלביזיות ענקים. למד כיצד לבנות אתר שעובד היטב בכל המכשירים הללו. כל מכשיר מציג יתרונות ייחודיים משלו וגם אילוצים. כמפתח, אתה אמור לתמוך בכל שלל המכשירים הללו באופן מיטבי.

### TL;DR {: .hide-from-toc }

אנחנו בונים אתר שפועל על פני מסך בגדלים שונים ובמגוון מכשירים. [בארכיטקטורת](#) המידע של הדף ויצרה מבנה בסיסי. במדריך זה, אנחנו ניקח את המבנה הבסיסי שלנו עם תוכן ולהפוך אותו לדף יפה שמגיב למספר רב של גדלי מסך.

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

בעקבות העקרונות של פיתוח רשת עם מחשבה על ניידים בתחילה, אנחנו מתחילים עם viewport צר &mdash; בדומה לטלפון נייד &mdash; אנו בונים עבור חווית משתמש כזו בתחילה. לאחר מכן, אנו עולים למכשירים גדולים יותר. אנחנו יכולים לעשות את זה על ידי התאמת ה viewport שלנו למסך רחב יותר וביצוע שיפוטי בשאלה האם העיצוב והפריסה נראים טוב.

## הפוך את זה למגיב

מוקדם יותר יצרנו כמה עיצובים ברמת מקרו לאיך התוכן שלנו צריך להיות מוצג. עכשיו אנחנו צריכים להפוך את הדף שלנו לדינמי כך שהוא יוכל להסתגל לפריסות שונות אלה. אנו עושים זאת על ידי קבלת החלטה על היכן למקם את נקודות העצירה שלנו. &mdash; נקודה אשר בה העיצוב משתנה על הדף &mdash; בהתאם לתוכן כך שהוא יותאם לגודל המסך.

אפילו לדף בסיסי, אתה **חייב** תמיד להוסיף את תג meta viewport. Viewport הוא המרכיב הקריטי ביותר שאתה צריך לבניית חוויות רב מכשיר. בלי זה, האתר שלך לא יעבוד היטב במכשירים ניידים.

Viewport מציין לדפדפן שהדף צריך להיות בקנה מידה מסויים כדי להתאים המסך. ישנן תצורות רבות ושונות שניתן לציין ל viewport שלך כדי לשלוט בתצוגה של הדף. בתור ברירת מחדל, אנו ממליצים:

Viewport מתגורר בראש המסמך, ורק צריך להיות מוכלל פעם אחת.

### הוסף viewport

* הגבל את הרוחב המרבי של העיצוב.
* שנה את הריפוד של אלמנטים בכדי להקטין את גודל הטקסט.
* העבר את הטופס לצוף בקנה אחד עם תוכן הכותרת.
* הפוך את הווידאו לצף סביב התוכן.

### קבע סגנון פשוט

{# include shared/related_guides.liquid inline=true list=page.related-guides.responsive #}

לחברה ולמוצר שלנו כבר יש (ברוב המקרים) מדריך מיתוג וגופן ספציפיים. הם מסופקים במדריך הסגנון.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/viewport.html" region_tag="viewport" adjust_indentation="auto" %}
</pre>

מדריך סגנון הוא דרך יעילה לקבלת הבנה ברמת על של הייצוג החזותי של הדף וזה עוזר לך לוודא שאתה עקבי בכל העיצוב.

במדריך הקודם, הוספנו תמונות בשם "תמונות תוכן". אלה היו תמונות שהיו חשובות לנרטיב של המוצר שלנו. תמונות סגנונית הן תמונות שאינם נחוצות כחלק מתכני הליבה אבל מוסיפות לעיצוב ותורמות להנחות את תשומת לבו של המשתמש לפיסת התוכן ספציפי.

### קבע את נקודת השבירה הראשונה

דוגמא טובה לכך היא תמונת כותרת לתוכן "מעל לקפל" (אותו קו שהמשתמש רואה תמיד). הוא משמש לעתים קרובות כדי לפתות את המשתמשים לגלול ולקרוא עוד על המוצר..

#### מדריך הסגנון

A style guide is a useful way to get a high-level understanding of the visual representation of the page and it helps you ensure that you are consistent throughout the design.

#### הוספת תמונות בסטייל

<div class="styles" style="font-family: monospace;">
  <div style="background-color: #39b1a4">#39b1a4</div>
  <div style="background-color: white">#ffffff</div>
  <div style="background-color: #f5f5f5">#f5f5f5</div>

  <div style="background-color: #e9e9e9">#e9e9e9</div>
  <div style="background-color: #dc4d38">#dc4d38</div>
</div>

#### הצף את (Form)

<img  src="images/narrowsite.png" alt="Designed site"  class="attempt-right" />

בחרנו תמונת רקע פשוטה שמטושטשת, כך שהיא לא לוקחת מהתוכן ויש לנו להגדיר אותה כ 'cover` על כל האלמנט; באופן שתמיד מותח אוה תוך שמירה על יחס ממדים נכון.

העיצוב מתחיל להיראות רע ברוחב של 600px. במקרה שלנו, את אורכו של הקו הולך מעל 10 מילים (אורך הקריאה האופטימלי), ואותו אנו רוצים לשנות.

600px נראה מקום טוב כדי לקבוע נקודת העצירה הראשונה שלנו מפני שהוא ייתן לנו מרווח כדי לשנות את מיקום אלמנטים כדי להפוך אותם מותאמים טוב יותר למסך. אנחנו יכולים לעשות את זה באמצעות טכנולוגיה המכונות [שאילתות מדיה(Media Queries)](/web/fundamentals/design-and-ux/responsive/#use-css-media-queries-for-responsiveness)

<div style="clear:both;"></div>

    #headline {
      padding: 0.8em;
      color: white;
      font-family: Roboto, sans-serif;
      background-image: url(backgroundimage.jpg);
      background-size: cover;
    }
    

יש יותר מקום על מסך גדול יותר כך יש יותר גמישות עם איך ניתן להציג תוכן.

### הגבל את הרוחב המירבי של העיצוב

Note: אתה לא צריך להעביר את כל האלמנטים בבת אחת, אתה יכול לבצע התאמות קטנות יותר במידת צורך..

<video controls poster="images/firstbreakpoint.png" style="width: 100%;">
  <source src="videos/firstbreakpoint.mov" type="video/mov"></source>
  <source src="videos/firstbreakpoint.webm" type="video/webm"></source>
  <p>סליחה אבל הדפדפן שלך לא תומך בהצגת וידאו
     <a href="videos/firstbreakpoint.mov">הורד את הוידאו</a>.
  </p>
</video>

בהקשר של דף המוצר שלנו, נראה שאנחנו זקוקים למספר דברים:

    @media (min-width: 600px) {
    
    }
    

{# include shared/related_guides.liquid inline=true list=page.related-guides.first-break-point #}

אנחנו בחרנו רק שתי פריסות עיקריות: תצוגה צרה ותצוגה רחבה, אשר מאוד מפשטות את תהליך הבנייה שלנו.

החלטתנו גם ליצור מקטעים על viewport הצר שישאר מלא גם על ה viewport הרחב. זה אומר שאנחנו צריכים להגביל את רוחב מרבי של המסך, כך שהטקסט ופסקאות לא התארכו לשורה ארוכה על מסכים רחבים במיוחד. בחרנו נקודה זו להיות על 800px.

* הזז את הטופס סביב פרטי הכותרת.
* מקם את הווידאו בצד הימין של נקודות מפתח.
* קבע את התמונות בפסיפס שיותאם למסך
* הגדל את הטבלה
* Reduce the size of the images and have them appear in a nicer grid.

### התאמת הריפוד ולהקטין את גודל טקסט

כדי להשיג זאת, עלינו להגביל את הרוחב ולמרכז את האלמנטים. אנחנו צריך ליצור מיכל סביב כל סעיף עיקרי ולהחיל `margin: auto`. זה יאפשר המסך לגדול אבל התוכן יישאר מרוכז ובגודל מרבי של 800px.

המיכל יהיה 'div' פשוט בצורה הבאה:

בחלון התצוגה הצר, אין לנו הרבה מקום להצגת תוכן. בשל כך יש להקטין באופן משמעותי את גודל ומשקל הכתב כדי שיתאימו מסך באופן טוב יותר.

עם viewport גדול יותר, אנחנו צריכים לקחת בחשבון שהמשתמש עובד עם מסך גדול יותר אבל גם נמצא רחוק יותר. כדי להגדיל את מידת הקריאות של תוכן, אנו יכולים להגדיל את הגודל ומשקל של הטיפוגרפיה. כמו כן, רצוי לשנות את הריפוד כדי להפוך את האזורים השונים לבולטים יותר.

    <div class="container">
    ...
    </div>
    

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="containerhtml"   adjust_indentation="auto" %}
</pre>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="container"   adjust_indentation="auto" %}
</pre>

בדף המוצר שלנו, נוכל להגדיל את הריפוד של אלמנטי הsection על ידי הגדרה שישאר ב5% מהרוחב. אנו גם נגדיל את גודל הכותרות לכל אחד מהסעיפים.

### לשנות את הריפוד ולהקטין את גודל טקסט

Viewport הצר שלנו היה תצוגה ליניארי שנערם. כל קטע עיקרי והתוכן בתוכם הוצג לפי סדר מלמעלה למטה.

Viewport רחב נותן לנו שטח נוסף לשימוש כדי להציג את התוכן בצורה אופטימלית למסך. לדף המוצר שלנו, על פי IA שאנחנו יכולים:

המשמעות של Viewport צר היא כי יש לנו הרבה פחות שטח אופקי עבור האלמנטים על המסך.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding"   adjust_indentation="auto" %}
</pre>

כדי לעשות שימוש יעיל יותר של שטח המסך האופקי, אנחנו צריכים לפרוץ של הזרימה ליניארי של הכותרת ולהעביר את הטופס ורשימה להיות זה ליד זה.

### לסיכום

הווידאו בממשק viewport צר נועד להיות ברוחב מלא של המסך ואת מיקומו נקבע לאחר הרשימה של התכונות העיקריות. על viewport רחב, הוידאו יהיה בהיקף גדול מדי ונראה שגוי כאשר נמקם אותו ליד רשימת תכונות.

אלמנט הווידאו צריך להיות מועבר אל מחוץ לזרימה האנכית של ה viewport הצר ואמור להיות מוצג ליד הרשימה עם תבליטים של תוכן.

* Move the form around the header information.
* Position the video to the right of the key points.
* Tile the images.
* Expand the table.

#### הצף את אלמנט הוידאו

התמונות בחלון התצוגה הצרה (מכשירים ניידים בעיקר) מוגדרות להיות ברוחב המלא של המסך ולהערם בצורה אנכית. זה לא מתכוונן היטב לחלון תצוגה רחבה.

כדי להפוך את התמונות כדי שיראו טוב בחלון תצוגה רחבה, אנו נמתח אותן ל30% מרוחב המכל כאשר הוא אופקי (ולא אנכי במבט הצר על המכשיר). אנו גם להוסיף קצת רדיוס גבול ותיבת צל כדי להפוך את התמונות נראות מושכות יותר.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="formfloat"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/floattheform.html){: target="_blank" .external }

<video controls poster="images/floatingform.png" style="width: 100%;">
  <source src="videos/floatingform.mov" type="video/mov"></source>
  <source src="videos/floatingform.webm" type="video/webm"></source>
  <p>סליחה אבל הדפדפן שלך אינו תומך בוידאו
     <a href="videos/floatingform.mov">הורד את הוידאו</a>.
  </p>
</video>

#### סדר את התמונות

בעת שימוש בתמונות, יש לקחת את הגודל של נקודת מבט ואת הצפיפות של התצוגה בחשבון.

האינטרנט נבנה למסכי 96 dpi. עם כניסתם של מכשירים ניידים, ראינו גידול ניכר בצפיפות פיקסלים של המסכים הללו. יתרה מכך, במחשבים ניידים יש גם retina . משום כך, תמונות שמקודדות ל96 dpi לעתים קרובות נראה נורא על מכשיר היי dpi.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="padding"   adjust_indentation="auto" %}
</pre>

יש לנו פתרון שלא אומץ באופן נרחב עדיין. עבור דפדפנים שתומכים בה, אתה יכול להציג תמונה בצפיפות גבוהה על תצוגת צפיפות גבוהה.

#### ליצור תמונות מגיבות ל DPI

<img src="images/imageswide.png" class="attempt-right" />

טבלאות הן אלמנט קשה כדי להתאימן על מכשירים שיש להם חלון תצוגה צרה וצריכים התייחסות מיוחדת.

אנו ממליצים על viewport צר שאתה בונה את הטבלה שלך לשתי שורות, ולהחליף את הכותרת ותאים בשורה כדי להפוך אותם לעמודה.

<div style="clear:both;"></div>

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/fixingfirstbreakpoint.html" region_tag="floatvideo"   adjust_indentation="auto" %}
</pre>

באתר שלנו, היינו צריכים ליצור נקודת עצירה נוספת רק לתוכן הטבלה. כאשר אתה בונה למכשיר נייד בתחילה, קשה יותר לבטל סגנונות שימושיים. לכן אנחנו חייבים להחליף את ה CSS של ה viewport הצר מה viewport הרחב. זה נותן לנו הפסקה ברורה ועקבית.

#### אבלאות

** מזל טוב. ** אם אתה קורא את זה, אתה כבר בנית את העמוד מוצר הראשון שלך שעובד על פני מגוון גדול של מכשירים, גדלי טופס, וגדלי מסכים.

אם תפעל לפי ההנחיות הבאות, אתה תהיה על המסלול להתחלה טובה:

Translated By: {% include "web/_shared/contributors/greenido.html" %}

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
  <p>현재 브라우저에서 비디오를 지원하지 않습니다.
     <a href="videos/responsivetable.mov">비디오 다운로드하기</a>.
  </p>
</video>

In our site, we had to create an extra breakpoint just for the table content. When you build for a mobile device first, it is harder to undo applied styles, so we must section off the narrow viewport table CSS from the wide viewport css. This gives us a clear and consistent break.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/codelabs/your-first-multi-screen-site/_code/content-with-styles.html" region_tag="table-css"   adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/codelabs/your-first-multi-screen-site/content-with-styles.html){: target="_blank" .external }

## Wrapping up

Success: By the time you read this, you will have created your first simple product landing page that works across a large range of devices, form-factors, and screen sizes.

If you follow these guidelines, you will be off to a good start:

1. צור IA בסיסי ולמד את מבנה התוכן שלך לפני שאתה מקודד.
2. תמיד הגדר viewport.
3. צור את החוויה הבסיסית סביב הגישה של mobile-first
4. ברגע שיש לך החוויה הניידת, תוכל להגדיל את הרוחב של התצוגה עד שזה לא נראה טוב ולהגדיר נקודת העצירה שם.
5. המשך לנסות ולהתאים