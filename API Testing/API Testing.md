# API Testing

## 🔹 ما هو API Testing؟
اختبار **واجهات برمجة التطبيقات (APIs - Application Programming Interfaces)** هو عملية تحليل واختبار الواجهات التي تسمح للتطبيقات بالتواصل مع بعضها.

📌 معظم تطبيقات الويب الحديثة تعتمد بشكل أساسي على APIs، لذلك:
- أي ثغرة في الـ API قد تؤثر مباشرة على:
  - **سرية البيانات (Confidentiality)**
  - **سلامتها (Integrity)**
  - **توفرها (Availability)**

---

## 🔹 لماذا يعتبر API Testing مهم؟
- APIs تمثل نقطة الدخول الحقيقية للتطبيق (**Backend**)
- قد تحتوي على **Endpoints مخفية** غير مستخدمة في الواجهة
- أحيانًا تكون أكثر عرضة للثغرات مقارنة بالواجهة الأمامية

📌 مثال:
حتى لو الواجهة تمنع تعديل `isAdmin`، قد يسمح الـ API بذلك.

---

## 🔹 Recon (جمع المعلومات)

أول خطوة في اختبار الـ API هي:
> جمع أكبر قدر ممكن من المعلومات عن الـ API

مثل:
- Endpoints
- Parameters
- HTTP Methods

---

### 🔍 اكتشاف Endpoints

مثال:
```http
GET /api/books HTTP/1.1
````

📌 هذا endpoint يعيد قائمة كتب

---

### 🔍 أماكن شائعة للبحث

```text
/api
/swagger/index.html
/openapi.json
```

📌 هذه المسارات قد تحتوي على توثيق كامل للـ API

---

### 🔍 تحليل المسارات

إذا وجدت:

```text
/api/v1/users/123
```

جرب:

```text
/api/v1
/api
/api/v1/users
```

---

## 🔹 طرق اختبار APIs

### 1. اختبار Endpoints غير المستخدمة

بعض الـ endpoints غير مرتبطة بالواجهة لكنها لا تزال تعمل

📌 مثال:

```http
DELETE /api/users/carlos
```

---

### 2. استغلال Mass Assignment

إذا كان السيرفر يربط المدخلات مباشرة بالكائن:

```json
{
  "username": "wiener",
  "isAdmin": true
}
```

📌 قد يؤدي ذلك إلى الحصول على صلاحيات Admin

---

### 3. Server-Side Parameter Pollution

إرسال نفس الباراميتر أكثر من مرة:

```text
userId=1&userId=2
```

📌 قد يسبب سلوك غير متوقع في السيرفر

---

### 4. التلاعب بالـ HTTP Methods

جرب تغيير نوع الطلب:

```text
GET → POST → PUT → PATCH → DELETE
```

📌 أحيانًا الحماية تكون مفعلة على method معين فقط

---

### 5. اختبار Content-Type

مثال:

```http
Content-Type: application/json
```

جرب تغييره إلى:

```text
application/xml
text/plain
```

📌 قد يؤدي إلى اختلاف في معالجة البيانات (Parsing)

---

## 🔹 أدوات مهمة

* Burp Suite (Proxy + Repeater + Intruder)
* Postman
* SoapUI

📌 يمكن استخدام أدوات لتحليل OpenAPI تلقائيًا

---

## 🔹 أمثلة على ثغرات API

* Broken Access Control
* Mass Assignment
* Information Disclosure
* Rate Limiting Bypass
* Injection (SQL / NoSQL)

---

## 🔹 طرق الحماية (Mitigation)

### 1. حماية التوثيق (Documentation)

* لا تترك Swagger مكشوف للعامة

### 2. استخدام Allowlist

* السماح فقط بالـ parameters المطلوبة

### 3. التحقق من HTTP Methods

* السماح فقط بالطرق اللازمة

### 4. التحقق من Content-Type

* رفض أي نوع غير متوقع

### 5. حماية جميع الإصدارات

* تأمين جميع النسخ:

```text
/v1/
/v2/
```

### 6. استخدام رسائل أخطاء عامة

* عدم كشف تفاصيل السيرفر

---

## 🔹 أشهر Labs في PortSwigger

* Exploiting API using documentation
* Finding unused endpoints
* Mass Assignment exploitation
* Server-side parameter pollution

📌 هذه labs تحاكي سيناريوهات واقعية

---

## 🔹 ملاحظات مهمة أثناء الاختبار

* راقب الـ traffic باستخدام Burp
* ابحث عن endpoints مخفية
* لا تعتمد فقط على الواجهة
* جرّب تغيير كل شيء:

  * Method
  * Headers
  * Body

---

## 🔹 الخلاصة

ثغرات API تحدث عندما:

> السيرفر يثق بمدخلات المستخدم أو لا يفرض قيود كافية على endpoints

📌 ومع انتشار APIs، أصبحت هذه الثغرات من أهم أهداف:

* Bug Bounty
* Pentesting

---

## 🔗 المصدر

[https://portswigger.net/web-security/api-testing](https://portswigger.net/web-security/api-testing)

```

