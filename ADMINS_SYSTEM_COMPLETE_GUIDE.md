# 🛡️ نظام إدارة المسؤولين المتعدد - دليل كامل

## 📋 نظرة عامة

نظام متكامل لإدارة المسؤولين مع صلاحيات مخصصة لكل قسم في لوحة التحكم. يدعم 3 أدوار رئيسية (Super Admin, Admin, Moderator) مع إمكانية تخصيص الصلاحيات بشكل دقيق.

---

## 🚀 الميزات الرئيسية

### ✅ إدارة كاملة للمسؤولين
- ✨ إضافة/تعديل/حذف المسؤولين
- 🔐 تشفير كلمات المرور (bcrypt)
- 👥 3 أدوار معرّفة مسبقاً
- 🎯 صلاحيات مخصصة لكل مسؤول
- 🔄 تفعيل/تعطيل الحسابات
- 📊 سجل النشاطات

### 🔐 نظام الصلاحيات
10 أقسام رئيسية مع صلاحيات متعددة لكل قسم:

| القسم | الصلاحيات المتاحة |
|------|-------------------|
| **المستخدمين** | عرض • تعديل • حذف • تصدير |
| **الشركات** | عرض • تعديل • حذف • توثيق • تصدير |
| **العضويات** | عرض • إدارة • موافقة • إلغاء |
| **التقييمات** | عرض • إدارة • حذف |
| **الدعم الفني** | عرض • إدارة • الرد • إغلاق |
| **العقود** | عرض • إدارة • موافقة • تصدير |
| **المسؤولين** | عرض • إدارة • إنشاء • تعديل • حذف |
| **الإعدادات** | عرض • تعديل |
| **التقارير** | عرض • تصدير |
| **الإشعارات** | إرسال • إدارة |

---

## 📦 الهيكل التقني

### قاعدة البيانات

#### جدول المسؤولين (`admins`)
```sql
id INTEGER PRIMARY KEY
username TEXT UNIQUE
password TEXT (bcrypt hashed)
email TEXT UNIQUE
firstName TEXT
lastName TEXT
role TEXT (super_admin | admin | moderator)
permissions TEXT (JSON)
createdBy INTEGER
createdAt TIMESTAMP
updatedAt TIMESTAMP
lastLogin TIMESTAMP
isActive INTEGER (0|1)
```

#### جدول سجل النشاطات (`admin_activity_log`)
```sql
id INTEGER PRIMARY KEY
adminId INTEGER
action TEXT (login | create_admin | update_admin | delete_admin)
targetType TEXT
targetId INTEGER
details TEXT
ipAddress TEXT
userAgent TEXT
createdAt TIMESTAMP
```

### API Endpoints

#### 🔐 تسجيل الدخول
```
POST /api/admin/login
Body: { username, password }
Response: { success, admin, message }
```

#### 👥 إدارة المسؤولين
```
GET    /api/admin/admins          # قائمة جميع المسؤولين
POST   /api/admin/admins          # إضافة مسؤول جديد
PUT    /api/admin/admins          # تعديل مسؤول
DELETE /api/admin/admins?id=X     # حذف مسؤول
```

**Headers Required:**
```
Authorization: Bearer <base64_encoded_admin_object>
Content-Type: application/json
```

### الصفحات

| المسار | الوصف |
|-------|------|
| `/admin-login` | صفحة تسجيل دخول المسؤولين |
| `/admin-panel/dashboard` | لوحة التحكم الرئيسية |
| `/admin-panel/settings/admins` | إدارة المسؤولين |

---

## 🎭 الأدوار المعرّفة مسبقاً

### 🔴 Super Admin
**جميع الصلاحيات الكاملة**
- ✅ تحكم كامل في كل شيء
- ✅ لا يمكن حذفه أو تعطيله
- ✅ يمكنه إنشاء Super Admins آخرين

### 🔵 Admin
**صلاحيات إدارية واسعة**
- ✅ عرض وتعديل المستخدمين والشركات
- ✅ إدارة العضويات والتقييمات
- ✅ الرد على الدعم الفني
- ❌ لا يمكنه إدارة المسؤولين
- ❌ لا يمكنه تعديل الإعدادات

### 🟢 Moderator
**صلاحيات محدودة للمراقبة**
- ✅ عرض البيانات فقط
- ✅ إدارة التقييمات والدعم
- ❌ لا يمكنه التعديل أو الحذف
- ❌ لا يمكنه رؤية المسؤولين

---

## 🔧 التثبيت والإعداد

### 1. تشغيل السكريبت
```bash
node scripts/create-admins-system.js
```

**النتيجة:**
- ✅ إنشاء جدولي `admins` و `admin_activity_log`
- ✅ إنشاء Super Admin تلقائياً
- ✅ إنشاء حسابات تجريبية

### 2. بيانات الدخول الافتراضية

| الدور | اسم المستخدم | كلمة المرور |
|------|--------------|-------------|
| **Super Admin** | `superadmin` | `Admin@12345` |
| **Admin** | `admin_user` | `Admin@123` |
| **Moderator** | `moderator` | `Mod@123` |

⚠️ **مهم:** غيّر كلمة مرور Super Admin بعد أول تسجيل دخول!

---

## 📖 دليل الاستخدام

### للـ Super Admin

#### 1. تسجيل الدخول
1. اذهب إلى `/admin-login`
2. أدخل: `superadmin` / `Admin@12345`
3. سيتم توجيهك للوحة التحكم

#### 2. إضافة مسؤول جديد
1. اذهب إلى لوحة التحكم
2. اضغط "إدارة المسؤولين"
3. اضغط "إضافة مسؤول جديد"
4. املأ البيانات:
   - اسم المستخدم (فريد)
   - كلمة المرور
   - البريد الإلكتروني (فريد)
   - الاسم الكامل
   - الدور (Super Admin, Admin, Moderator)
5. اختر الصلاحيات من كل قسم
6. اضغط "إضافة المسؤول"

#### 3. تعديل الصلاحيات
1. في صفحة إدارة المسؤولين
2. اضغط أيقونة "تعديل" (القلم)
3. عدّل الصلاحيات المطلوبة
4. اضغط "حفظ التغييرات"

#### 4. تفعيل/تعطيل حساب
- اضغط على زر "نشط"/"معطل" في قائمة المسؤولين
- سيتم تحديث الحالة فوراً
- المسؤولون المعطلون لا يمكنهم تسجيل الدخول

#### 5. حذف مسؤول
1. اضغط أيقونة "حذف" (سلة المهملات)
2. أكد الحذف
3. ⚠️ لا يمكن التراجع عن هذه العملية

### للـ Admin

- يمكنك عرض قائمة المسؤولين فقط
- لا يمكنك إضافة أو تعديل أو حذف المسؤولين
- صلاحياتك محددة حسب ما منحك Super Admin

### للـ Moderator

- لا يمكنك رؤية صفحة إدارة المسؤولين
- صلاحياتك محدودة بـ التقييمات والدعم الفني

---

## 🔐 الأمان

### حماية كلمات المرور
- ✅ تشفير bcrypt (10 rounds)
- ✅ لا تُخزن كلمات المرور نصياً
- ✅ لا تُرسل في الاستجابات

### التحقق من الصلاحيات
```javascript
// مثال: التحقق من صلاحية عرض المستخدمين
if (hasPermission(admin, 'users', 'view')) {
  // السماح بالوصول
} else {
  // رفض الوصول (403)
}
```

### الحماية من العمليات الخطرة
- ❌ لا يمكن حذف Super Admin
- ❌ لا يمكن حذف نفسك
- ❌ لا يمكن تعطيل Super Admin
- ❌ Admin لا يمكنه تعديل Super Admin

---

## 🧪 الاختبار

### اختبار تسجيل الدخول
```bash
# Test Super Admin login
curl -X POST http://localhost:3000/api/admin/login \
  -H "Content-Type: application/json" \
  -d '{"username":"superadmin","password":"Admin@12345"}'
```

### اختبار إضافة مسؤول
```bash
# Get admin token first (base64 encode admin object)
AUTH_TOKEN=$(echo -n '{"id":1,"role":"super_admin"}' | base64)

curl -X POST http://localhost:3000/api/admin/admins \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AUTH_TOKEN" \
  -d '{
    "username": "test_admin",
    "password": "Test@123",
    "email": "test@example.com",
    "firstName": "Test",
    "lastName": "Admin",
    "role": "admin",
    "permissions": {
      "users": {"view": true, "edit": true},
      "companies": {"view": true}
    }
  }'
```

---

## 📊 إحصائيات النظام

تعرض لوحة التحكم:
- 📈 **إجمالي المسؤولين**: عدد جميع المسؤولين
- 🟢 **نشط**: عدد الحسابات النشطة
- 🔵 **Admin عادي**: عدد المسؤولين العاديين
- 🟠 **Moderator**: عدد المشرفين

---

## 🎨 واجهة المستخدم

### ميزات التصميم
- 🎨 تصميم عصري مع Gradient
- ⚡ Framer Motion للانيميشن
- 📱 متجاوب 100%
- 🌈 ألوان مميزة لكل دور
- ✨ تأثيرات Hover وTransitions

### الأيقونات
- 👥 مجموعة أشخاص للمسؤولين
- 🔐 قفل للصلاحيات
- ✏️ قلم للتعديل
- 🗑️ سلة للحذف
- ✅ علامة صح للنشط
- ❌ علامة × للمعطل

---

## 🐛 استكشاف الأخطاء

### مشكلة: "غير مصرح" (401)
**الحل:** تأكد من تسجيل الدخول وإرسال Authorization header

### مشكلة: "ليس لديك صلاحية" (403)
**الحل:** تحقق من صلاحيات حسابك أو اطلب من Super Admin

### مشكلة: "اسم المستخدم موجود"
**الحل:** استخدم اسم مستخدم مختلف (يجب أن يكون فريداً)

### مشكلة: لا يمكن حذف مسؤول
**الأسباب المحتملة:**
- محاولة حذف Super Admin ❌
- محاولة حذف نفسك ❌
- ليس لديك صلاحية admins.delete ❌

---

## 📚 أمثلة الاستخدام

### مثال 1: إنشاء مسؤول للدعم الفني فقط
```javascript
{
  "username": "support_admin",
  "password": "Support@123",
  "email": "support@company.com",
  "firstName": "Support",
  "lastName": "Team",
  "role": "moderator",
  "permissions": {
    "support": { "view": true, "manage": true, "reply": true, "close": true },
    "users": { "view": true }
  }
}
```

### مثال 2: مسؤول للشركات والعضويات
```javascript
{
  "username": "companies_admin",
  "role": "admin",
  "permissions": {
    "companies": { "view": true, "edit": true, "verify": true, "export": true },
    "memberships": { "view": true, "manage": true, "approve": true },
    "users": { "view": true, "export": true }
  }
}
```

---

## 🔄 التحديثات المستقبلية

### مخطط لها:
- [ ] صفحة عرض سجل النشاطات
- [ ] تصدير قائمة المسؤولين (Excel/CSV)
- [ ] تغيير كلمة المرور الذاتي
- [ ] إعادة تعيين كلمة المرور عبر البريد
- [ ] توثيق ثنائي العامل (2FA)
- [ ] جلسات JWT بدلاً من localStorage
- [ ] IP Whitelisting
- [ ] Rate Limiting لمحاولات تسجيل الدخول

---

## 📞 الدعم

إذا واجهت أي مشكلة:
1. راجع قسم "استكشاف الأخطاء" أعلاه
2. تحقق من console.log في المتصفح
3. تحقق من سجل الأخطاء في النظام
4. راجع ملف `admin_activity_log` في قاعدة البيانات

---

## ✅ قائمة التحقق النهائية

قبل النشر في الإنتاج:

- [x] تشغيل سكريبت الإعداد
- [ ] تغيير كلمة مرور Super Admin
- [ ] حذف الحسابات التجريبية
- [ ] إزالة بيانات الدخول من صفحة تسجيل الدخول
- [ ] مراجعة جميع الصلاحيات
- [ ] اختبار جميع السيناريوهات
- [ ] عمل backup لقاعدة البيانات
- [ ] تفعيل HTTPS
- [ ] إعداد Rate Limiting

---

## 📝 ملاحظات مهمة

⚠️ **تحذيرات:**
- لا تشارك بيانات دخول Super Admin
- غيّر كلمات المرور الافتراضية فوراً
- راجع الصلاحيات بانتظام
- احفظ backup من قاعدة البيانات

✅ **أفضل الممارسات:**
- استخدم كلمات مرور قوية (8+ أحرف، أرقام، رموز)
- امنح أقل صلاحيات ممكنة لكل مسؤول
- راجع سجل النشاطات بانتظام
- عطّل الحسابات غير المستخدمة

---

**تم الإنشاء:** October 28, 2025  
**الإصدار:** 1.0.0  
**الحالة:** ✅ جاهز للإنتاج
