---
"$title": أعلن في قصص AMP
"$order": '3'
description: قصص AMP هي تجربة قابلة للنقر بملء الشاشة تغمر القراء في المحتوى. يسمح الإعلان باستخدام إعلانات AMP Story بسلاسة وبدون انقطاع ...
formats:
- قصص
author: CrystalOnScript
---

قصص AMP هي تجربة قابلة للنقر بملء الشاشة تغمر القراء في المحتوى. يسمح الإعلان باستخدام إعلانات AMP Story بدمجًا سلسًا وخاليًا من الانقطاع في رحلة المستخدم ، مما يجعله منشغلًا بالمنصة ويسعده.

## موضع الإعلان

على عكس صفحات الويب بتنسيق AMP ، حيث يتم تحديد مقدار الإعلانات وموقعها من خلال موضع عدة مكونات [`amp-ad`](../../../documentation/components/reference/amp-ad.md) ، تعتمد قصص AMP على مكون [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) لإملاء كمية الإعلان وموضعه.

The [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) extension is a wrapper around the [`amp-ad`](../../../documentation/components/reference/amp-ad.md) component that dynamically inserts one or multiple ads while the user consumes the story content. To ensure the best user experience:

1. يتم عرض الإعلانات مسبقًا في وقت تشغيل AMP Stories ، ثم يتم إدراجها. يضمن هذا عدم عرض المستخدمين على الإطلاق لإعلان فارغ أو غير محمل.

2. تم تحسين كثافة الإعلان مع نسبة المحتوى لمنع التشبع الزائد. [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) وقت ومكان إدراج الإعلانات مع تقدم المستخدم.

يجعل وقت تشغيل AMP استدعاء الإعلان في أقرب وقت ممكن ويضع أول واحد في وقت ما بعد أول صفحتين ، وليس كصفحة أخيرة.

{٪ do doc.amp_dependencies.add ('amp-anim'، '0.1')٪}

<amp-anim width="360" height="640" src="/static/img/docs/stampads/stamp_gif_ad.gif">
  <amp-img placeholder width="360" height="640" src="/static/img/docs/stampads/stamp_gif_still.png">
  </amp-img>
</amp-anim>

[tip type="note"] **ملاحظة -** توفر قصة AMP الأطول المزيد من الفرص لوضع الإعلانات. سيستمر تحسين الموضع الدقيق لخوارزمية الإعلان بمرور الوقت. [/tip]

## تفاعل المستخدم

يمكن للمستخدمين التقدم في الإعلانات السابقة بنفس طريقة صفحات القصة العادية ؛ من خلال النقر على الثلثين الأيمن من الشاشة.

{{image ('/ static / img / docs / stampads / story_ad_ui.png'، 304، 512، layout = 'intrinsic'، alt = 'صورة تُظهر المنطقة التي يمكن للمستخدمين النقر عليها لتخطي إعلان'، caption = 'يمكن للمستخدمين تقدم الإعلانات الماضية عن طريق النقر على ثلثي الشاشة الأيمن. '، align =' ')}}

يمكن للمستخدمين التفاعل مباشرة مع الإعلان عن طريق النقر على زر الحث [على اتخاذ إجراء](story_ads_best_practices.md#call-to-action-button-text-enum) الذي يعرضه النظام والذي يظهر في الثلث السفلي من جميع إعلانات AMP Story. يمكن أن يؤدي النقر على الزر إلى إرسال المستخدم إلى أحد المواقع التالية التي تم تكوينها بواسطة منشئ الإعلان:

- صفحة ويب AMP
- صفحة ويب ليست بتنسيق AMP
- متجر التطبيقات أو متجر Google Play
- [قصة دعائية](story_ads_best_practices.md#sponsored-story)

{{image ('/ static / img / docs / stampads / sponsored_story.png'، 1600، 597، layout = 'intrinsic'، alt = 'صورة توضح أنه تمت إعادة توجيه المستخدمين إلى وجهة الإعلان المقصودة ، ولكن يمكنهم العودة إلى القصة. '، caption =' تتم إعادة توجيه المستخدمين إلى وجهة مقصودة للإعلان ، لكن يمكنهم العودة إلى القصة. '، align =' ')}}

## تهيئة

لا يمكن لقصص AMP دعم [`amp-ad`](../../../documentation/components/reference/amp-ad.md) مباشرة على الصفحة. بدلاً من ذلك ، يتم جلب جميع الإعلانات وعرضها بواسطة إضافة [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) . يجب وضع المكوِّن [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) كعنصر فرعي مباشر في [`amp-story`](../../../documentation/components/reference/amp-story.md) .

[sourcecode:html]
<amp-story>
  <amp-story-auto-ads>
    <script type="application/json">
      {
        "ad-attributes": {
          // ad server configuration
        }
      }
    </script>
  </amp-story-auto-ads>
  <amp-story-page>
  ...
</amp-story>
[/sourcecode]

على عكس [`amp-ad`](../../../documentation/components/reference/amp-ad.md) العادي ، لا يلزم استخدام `<fallback>` أو `<placeholder>` ، حيث لن يتم عرض إعلانات AMP Story إلا بعد عرضها بالكامل.

## دمج دعم خادم الإعلانات

تتمثل أسهل طريقة لتضمين الإعلانات في قصة AMP الخاصة بك في عرض الإعلانات من خادم إعلانات مدعوم.

خوادم الإعلانات التي تدعم حاليًا إعلانات AMP Story:

- [Google Ad Manager (DoubleClick سابقًا)](advertise_amp_stories.md#google-ad-manager)

إذا كنت خادم إعلانات مهتمًا بخدمة إعلانات القصة ، فيرجى الاتصال بنا عن طريق تقديم مشكلة في [GitHub](https://github.com/ampproject/amphtml/issues/new) . سيسعد فريق AMP بالتواصل معك!

يمكن للناشرين أيضًا وضع إعلانات مخصصة إذا قاموا بإعداد خادم الإعلانات الخاص بهم. [العملية مفصلة هنا](https://github.com/ampproject/amphtml/blob/master/extensions/amp-story/amp-story-ads.md#publisher-placed-ads) .

## مدير إعلانات جوجل<a name="google-ad-manager"></a>

يتم تعيين معلومات خادم الإعلانات ضمن [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) في بداية قصة AMP.

يجب عليك تحديد كائن تهيئة JSON داخل [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) الذي يحدد كيفية جلب الإعلانات وعرضها. الحقول التالية مطلوبة للعرض والإعلان باستخدام Google Ad Manager:

- يجب تحديد `"type"` على أنها `"doubleclick"`
- يجب إقران `"data-slot"` بوحدتك الإعلانية

[sourcecode:html]
<amp-story>
  <amp-story-auto-ads>
    <script type="application/json">
      {
        "ad-attributes": {
          "type": "doubleclick",
          "data-slot": "/30497360/a4a/amp_story_dfp_example"
        }
      }
    </script>
  </amp-story-auto-ads>
  <amp-story-page>
  ...
</amp-story>
[/sourcecode]

يتم نسخ أزواج القيم الأساسية هذه إلى عنصر [`amp-ad`](../../../documentation/components/reference/amp-ad.md) إنشاؤه للقصة. يمكن إضافة المعلومات الإضافية المطلوبة للعنصر بدلاً من البيانات `additional_data` ، مثل `targeting` .

[sourcecode:html]
<amp-story>
  <amp-story-auto-ads>
    <script type="application/json">
     {
       "ad-attributes": {
         "type": "doubleclick",
         "data-slot": "/30497360/a4a/amp_story_dfp_example",
         "additional_data": "additional_data_information"
       }
     }
    </script>
  </amp-story-auto-ads>
  <amp-story-page>
  ...
</amp-story>
[/sourcecode]

[tip type="note"] اقرأ [تصميمات الزيارات المخصصة في قصص AMP](https://support.google.com/admanager/answer/9038178) للحصول على معلومات حول تحميل الإعلانات إلى Google Ad Manager واطلع على دليلنا حول [أفضل الممارسات لإنشاء إعلان قصة AMP](story_ads_best_practices.md) . [/tip]
