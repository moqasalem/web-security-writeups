# Access Control

## 🔹 ما هو Access Control؟
**التحكم في الوصول (Access Control)** هو الآلية التي تحدد:
> من يمكنه الوصول إلى ماذا، وبأي صلاحيات

📌 الهدف الأساسي هو ضمان أن كل مستخدم يصل فقط إلى الموارد المصرح له بها.

---

## 🔹 أنواع Access Control

### 1. Vertical Access Control
يتعلق بمستوى الصلاحيات (Roles)

📌 مثال:
- User عادي
- Admin

❌ ثغرة:
إذا تمكن المستخدم العادي من الوصول إلى وظائف Admin

---

### 2. Horizontal Access Control
يتعلق بالوصول بين المستخدمين بنفس المستوى

📌 مثال:
```http
GET /my-account?id=123
````

❌ ثغرة:
تغيير `id` إلى مستخدم آخر:

```http
GET /my-account?id=456
```

---

### 3. Context-Dependent Access Control

يعتمد على الحالة أو السياق

📌 مثال:

* المستخدم لا يجب أن يعدل الطلب بعد إرساله

❌ ثغرة:
إعادة إرسال الطلب وتعديله بعد التنفيذ

---

## 🔹 لماذا تحدث ثغرات Access Control؟

* الاعتماد على الواجهة الأمامية فقط
* عدم التحقق من الصلاحيات في السيرفر
* منطق تطبيق ضعيف (Business Logic flaws)

📌 قاعدة مهمة:

> لا تثق أبدًا بالـ Client

---

## 🔹 أمثلة على الثغرات

### 1. Unprotected Admin Functionality

```text
/admin
/admin/deleteUser
```

📌 إذا كانت هذه المسارات غير محمية → يمكن لأي شخص الوصول لها

---

### 2. Parameter-Based Access Control

```http
GET /admin?role=admin
```

❌ يمكن التلاعب بالقيمة:

```http
GET /admin?role=admin
```

📌 السيرفر يعتمد على Parameter يمكن تغييره بسهولة

---

### 3. IDOR (Insecure Direct Object Reference)

```http
GET /api/orders/1001
```

❌ تغيير الرقم:

```http
GET /api/orders/1002
```

📌 إذا تم عرض بيانات مستخدم آخر → ثغرة

---

### 4. التحكم عبر Headers

```http
X-User-Role: admin
```

❌ بعض التطبيقات تثق بالـ Headers

---

### 5. URL-Based Access Control

```text
/user/profile
/admin/profile
```

❌ تغيير المسار فقط قد يعطي صلاحيات أعلى

---

## 🔹 طرق اكتشاف الثغرات

### 🔍 1. تغيير القيم (Parameter Tampering)

جرب تعديل:

* IDs
* Roles
* Usernames

---

### 🔍 2. الوصول المباشر (Force Browsing)

جرب مسارات:

```text
/admin
/dashboard
/manage
```

---

### 🔍 3. اختبار المستخدمين

* سجل دخول كمستخدم عادي
* حاول تنفيذ عمليات Admin

---

### 🔍 4. حذف أو تعديل Headers

```http
Cookie: session=...
```

📌 جرب:

* إزالة الكوكي
* استخدام كوكي لمستخدم آخر

---

### 🔍 5. إعادة إرسال الطلبات

📌 باستخدام Burp Repeater:

* عدّل الطلب بعد التنفيذ
* غيّر القيم

---

## 🔹 أدوات مفيدة

* Burp Suite (Proxy / Repeater / Intruder)
* OWASP ZAP
* ffuf / dirsearch (لـ force browsing)

---

## 🔹 طرق الحماية (Mitigation)

### 1. التحقق في السيرفر دائمًا

* لا تعتمد على الواجهة الأمامية

---

### 2. استخدام Access Control قوي

مثل:

* RBAC (Role-Based Access Control)
* ABAC (Attribute-Based Access Control)

---

### 3. منع IDOR

* استخدم identifiers غير قابلة للتخمين (UUID)

---

### 4. التحقق من كل طلب

* تحقق من:

  * هوية المستخدم
  * صلاحياته
  * ملكية البيانات

---

### 5. عدم الثقة بالمدخلات

* لا تعتمد على:

  * Parameters
  * Headers
  * Hidden fields

---

### 6. تسجيل ومراقبة الأنشطة

* Log لكل العمليات الحساسة

---

## 🔹 أشهر Labs في PortSwigger

* Unprotected admin functionality
* User role controlled by request parameter
* IDOR vulnerabilities
* Multi-step process bypass

📌 هذه labs تحاكي سيناريوهات حقيقية

---

## 🔹 ملاحظات مهمة أثناء الاختبار

* فكّر كمهاجم:

  * ماذا لو غيرت هذا الـ ID؟
* لا تعتمد على الواجهة
* جرّب كل الاحتمالات:

  * URL
  * Parameters
  * Headers
  * Cookies

---

## 🔹 الخلاصة

ثغرات Access Control تحدث عندما:

> لا يتم التحقق من صلاحيات المستخدم بشكل صحيح في السيرفر

📌 وهي من أخطر الثغرات لأنها قد تؤدي إلى:

* تسريب بيانات
* التحكم الكامل في النظام

---

## 🔗 المصدر

[https://portswigger.net/web-security/access-control](https://portswigger.net/web-security/access-control)

```

