API Testing
🔹 ما هو API Testing؟

اختبار APIs (Application Programming Interfaces) هو عملية تحليل واختبار الواجهات التي تسمح للتطبيقات بالتواصل مع بعضها.

📌 معظم تطبيقات الويب الحديثة تعتمد بشكل أساسي على APIs، لذلك:

أي ثغرة في الـ API قد تؤثر مباشرة على سرية البيانات (Confidentiality) وسلامتها (Integrity) وتوفرها (Availability)

🔹 لماذا يعتبر API Testing مهم؟
APIs هي نقطة الدخول الحقيقية للتطبيق (backend)
أحيانًا لا يتم استخدامها بالكامل في الواجهة (Hidden endpoints)
قد تحتوي على ثغرات أخطر من الواجهة الأمامية

📌 مثال:
حتى لو الواجهة تمنعك من تعديل isAdmin، قد يسمح الـ API بذلك!

🔹 Recon (جمع المعلومات)

أول خطوة في اختبار الـ API هي:

معرفة أكبر قدر ممكن من المعلومات عن الـ API (Endpoints, Parameters, Methods)

🔍 اكتشاف Endpoints

مثال:

GET /api/books HTTP/1.1

📌 هذا endpoint يعيد قائمة كتب

🔍 أماكن شائعة للبحث:
/api
/swagger/index.html
/openapi.json

📌 هذه المسارات قد تحتوي على توثيق كامل للـ API

🔍 تحليل المسارات

إذا وجدت:

/api/v1/users/123

جرب:

/api/v1
/api
/api/v1/users
🔹 طرق اختبار APIs
1. اختبار الـ Endpoints غير المستخدمة
بعض endpoints غير مرتبطة بالواجهة
لكنها ما زالت تعمل

📌 مثال:

DELETE /api/users/carlos
2. استغلال Mass Assignment

إذا كان السيرفر يربط المدخلات مباشرة بالكائن:

{
  "username": "wiener",
  "isAdmin": true
}

📌 قد تحصل على صلاحيات Admin!

3. Server-Side Parameter Pollution

إرسال parameters متعددة لنفس القيمة:

userId=1&userId=2

📌 قد يسبب سلوك غير متوقع في السيرفر

4. التلاعب بالـ HTTP Methods

جرب تغيير:

GET → POST → PUT → PATCH → DELETE

📌 أحيانًا الحماية تكون فقط على method معين

5. اختبار Content-Type

مثلاً:

Content-Type: application/json

جرب تغييره إلى:

application/xml
text/plain

📌 قد يكشف parsing مختلف أو ثغرات

🔹 أدوات مهمة
Burp Suite (Proxy + Repeater + Intruder)
Postman
SoapUI

📌 يمكن استخدام أدوات لتحليل OpenAPI تلقائيًا

🔹 أمثلة على ثغرات API
Broken Access Control
Mass Assignment
Information Disclosure
Rate Limiting bypass
Injection (SQL / NoSQL)
🔹 طرق الحماية (Mitigation)
1. حماية التوثيق (Documentation)
لا تترك Swagger مكشوف للعامة
2. استخدام Allowlist
تحديد parameters المسموحة فقط
3. التحقق من HTTP Methods
السماح فقط بالـ methods المطلوبة
4. التحقق من Content-Type
رفض أي نوع غير متوقع
5. حماية جميع الإصدارات
/v1/, /v2/ يجب تأمينها كلها
6. رسائل أخطاء عامة
لا تكشف تفاصيل السيرفر
🔹 أشهر Labs في PortSwigger
Exploiting API using documentation
Finding unused endpoints
Mass Assignment exploitation
Server-side parameter pollution

📌 هذه labs تغطي سيناريوهات واقعية جدًا

🔹 ملاحظات مهمة أثناء الاختبار
راقب الـ traffic باستخدام Burp
ابحث عن endpoints مخفية
لا تعتمد فقط على الواجهة
جرّب تغيير كل شيء (method / headers / body)
🔹 الخلاصة

ثغرات API تحدث عندما:

السيرفر يثق بمدخلات المستخدم أو لا يفرض قيود كافية على endpoints

📌 ومع انتشار الـ APIs، أصبحت هذه الثغرات من أهم أهداف الـ Bug Bounty والـ Pentesting

🔗 المصدر
https://portswigger.net/web-security/api-testing
