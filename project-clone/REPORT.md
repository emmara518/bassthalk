# تقرير نسخ موقع bassthalk.com

## 1. ملخص تنفيذي

الموقع **ليس موقع HTML ثابت** — إنه تطبيق **React SPA (Create React App)** يتصاعد بالكامل في المتصفح عبر JavaScript، وكل المحتوى يُجلب من **API منفصل (Laravel)** على `api.bassthalk.com`. لذلك، الـ HTML الأصلي عبارة عن قشرة فارغة (`<div id="root">`) والمحتوى الحقيقي يتولد ديناميكيًا. تم نسخ كل الموارد الثابتة (CSS/JS/صور/خطوط) بجودتها الأصلية + التقاط الـ rendered HTML لكل صفحة من الصفحات الرئيسية.

## 2. بنية الموقع الأصلية

| المكوّن | التفاصيل |
|---|---|
| **الواجهة** | React SPA (Create React App) — `static/js/main.4195c775.js` (4.9MB) |
| **التنسيقات** | Tailwind CSS + CSS مخصص — `static/css/main.cd50eb63.css` (372KB) |
| **الـ Backend/API** | Laravel على `https://api.bassthalk.com` (+ نسخة `api2025.bassthalk.com`) |
| **الدفع** | Fawaterk (`app.fawaterk.com`) |
| **الفيديو** | Vdocipher + Vimeo + Inkrypt |
| **الأيقونات** | Iconify CDN (api.iconify.design) |
| **التحليلات** | GTM ×2، Google Analytics G-36SM3763DN، Meta Pixel (1201085508552613)، TikTok Pixel، Google Ads (16597094515) |
| **الأخطاء** | Sentry (DSN معروف في الباندل) |

## 3. عدد الصفحات المنسوخة

| الصفحة | الرابط الأصلي | الملف المنسوخ |
|---|---|---|
| الصفحة الرئيسية (قشرة SPA) | `/` | `index.html` |
| الصفحة الرئيسية (Rendered HTML كامل) | `/` | `index_rendered.html` |
| تسجيل الدخول | `/login` | `pages/login.html` |
| إنشاء حساب | `/register` | `pages/register.html` |
| انضم لفريقنا (توظيف) | `/join_us` | `pages/join-us.html` |
| صفحة مادة (نموذج) | `/subject/5` | `pages/subject-5.html` |
| صفحة مدرس (نموذج) | `/teacher/49` | `pages/teacher-49.html` |
| صفحة كورس (نموذج) | `/course/1733` | `pages/course-1733.html` |

**الموقع الكامل يحتوي على 69+ مسار:** 21 صفحة مواد (`/subject/5..88`)، 28 صفحة مدرس (`/teacher/3..75`)، و12+ صفحة كورس (`/course/1656..1733`) — كلها تولد من نفس القالب (نفس الكود) والبيانات من الـ API، فالنماذج الملتقطة أعلاه تمثلها كلها.

## 4. الملفات المُستخرجة (198 ملف — 38.8MB)

- **CSS (2 ملفات أصلية + 2 ملفات خطوط معاد كتابتها محليًا)**: `main.cd50eb63.css` (372KB) يشمل كل الـ media queries والـ breakpoints الأصلية (Tailwind + مخصص) — لم تُفقد أي أنيميشن أو transition.
- **JavaScript (1 ملف)**: `main.4195c775.js` (4.9MB) — الباندل الكامل بكل الـ Routes والمنطق.
- **الصور (109 ملف)**:
  - 67 أصلية من `/static/media/` (لوجوهات، خلفيات، أيقونات SVG، صور أقسام)
  - 42 صورة كورسات من `api.bassthalk.com/courses_images/` بجودتها الأصلية
  - favicon.ico + logo192 + logo512
- **الخطوط (74 ملف)**:
  - 13 خط محلي من `/static/media/`: Somar، Tajawal (7 أوزان)، FS Albert Arabic، VipHalaBold، Blabeloo، swiper-icons
  - 61 ملف woff2 من Google Fonts (Almarai، Aref Ruqaa، Cairo، Comfortaa، Comforter، IBM Plex Sans Arabic، Kufam، Rubik، Tajawal، Vibes، Lemonada) — أُعيدت كتابة ملفات CSS لترجع للملفات المحلية (`google-fonts-*-local.css`)

## 5. محتوى ديناميكي — يحتاج معالجة يدوية ⚠️

هذا الجزء **الأهم** في التقرير:

| # | العنصر | النوع | التفاصيل |
|---|---|---|---|
| 1 | **كل بيانات الموقع** (مدرسين، كورسات، مواد، أسعار، تواريخ) | API | تُجلب من `https://api.bassthalk.com` — أهم الـ endpoints: `/api/user/departments`, `/api/with_statistics/subjects`, `/api/courses/featured_courses`, `/api/platform_content_data` |
| 2 | **صور المدرسين** | API + Auth | تأتي من الـ API، وكثير من الـ endpoints محمي بـ authentication (اختبرت `/api/teachers/49` ورد `unauthenticated`) |
| 3 | **صفحات المواد/المدرسين/الكورسات** | React Router | كلها مسارات عميلة لا توجد كملفات HTML — تُبنى من البيانات المأخوذة من الـ API |
| 4 | **الأيقونات** | CDN | أيقونات Iconify تُحمّل لحظيًا من `api.iconify.design` — للنسخة دون اتصال يجب تحويلها لأيقونات SVG محلية (عددها قليل: search, chevron, whatsapp...) |
| 5 | **الفيديوهات** | طرف ثالث | مشغّل Vdocipher/Vimeo — المحتوى محمي DRM ومحتاج مفاتيح تجارية |
| 6 | **الدفع** | طرف ثالث | Fawaterk Checkout — تعاملات مالية حقيقية، تحتاج إعادة ربط بحساب جديد |
| 7 | **البحث** | React | بحث عميل في الـ JS bundle (SearchBar في الـ header) |
| 8 | **الوضع الليلي** | React | Toggle dark mode موجود — الثيم يتغير عبر الـ state ولا يوجد كملف منفصل |
| 9 | **التسجيل/الدخول** | API + Auth | Forms تعمل ضد الـ API (عناوين الـ endpoints مستخرجة من الباندل في ملف `api_endpoints.txt` إن أردت) |

## 6. عناصر لم تُستخرج بنجاح

1. **داتا المدرسين والكورسات الكاملة** — غير متاحة بدون تسجيل دخول (auth).
2. **فيديوهات الكورسات** — محمية بخصوصية الطرف الثالث (Vdocipher DRM).
3. **صور بعض المدرسين** — تأتي من الـ API المحمي.
4. **بيانات لوحة التحكم (Admin Panel)** — endpoints موجودة في الباندل (`/api/admin/*`) لكنها بالكامل خلف auth.
5. **Sentry DSN** — عمدًا لم أضع المفتاح في النسخة لتفادي إرسال أخطاء النسخة الجديدة لخوادمهم.

## 7. خطوات البناء لنسخة شغالة

1. الـ shell جاهز: افتح `index.html` محليًا (أو ارفعه على أي host ثابت) — الواجهة ستشتغل والمسارات React Router هتفعل.
2. أعد توجيه الـ API base URL في الباندل من `api.bassthalk.com` إلى الـ backend الجديد (عنوان الـ axios مركزي في `main.4195c775.js`).
3. أعد بناء الـ backend أو بدّل الـ endpoints بقيم ثابتة (البيانات الحالية ملتقطة في ملفات `pages/*.html` كمرجع).
4. أعد ربط Fawaterk وVdocipher بحساباتك.
5. حوّل أيقونات Iconify لملفات محلية إن احتجت العمل دون إنترنت.

## 8. ملاحظة حقوق ملكية

النسخة تحتوي كود الواجهة (React bundle) الذي هو ملك المطور السابق `@Om4rS4leh` (المكتوب في meta author). بما أنك مكلف رسميًا بالترحيل، استخدم هذا النسخ كمرجع بصري/وظيفي فقط، ويفضل **إعادة كتابة الكود من الصفر** بنفس التصميم لتجنب أي نزاع على حقوق الكود المصدر، خاصة مع وجود حقوق الطبع في `main.cd50eb63.css` وأكواد التحليلات الخاصة بهم.