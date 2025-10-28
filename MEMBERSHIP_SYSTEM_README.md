# نظام العضوية للشركات - Company Membership System

## 📋 نظرة عامة

نظام شامل لإدارة العضويات والاشتراكات للشركات في المنصة. يتيح للشركات الاشتراك للظهور في نتائج البحث وتلقي طلبات من العملاء.

## ✨ الميزات الرئيسية

### 1. إدارة العضويات
- ✅ عضويات بفترات مختلفة (30، 90، 365 يوم)
- ✅ تجديد تلقائي أو يدوي
- ✅ حالات العضوية (نشطة / غير نشطة)
- ✅ تتبع تواريخ الانتهاء

### 2. نظام الإشعارات
- 🔔 تحذيرات قبل انتهاء العضوية (7، 3، 1 أيام)
- 🔔 إشعار عند انتهاء العضوية
- 🔔 تعطيل تلقائي للشركات المنتهية

### 3. لوحة تحكم الشركة
- 📊 عرض حالة العضوية الحالية
- ⏰ العد التنازلي للأيام المتبقية
- 🔄 تجديد سريع (شهر، 3 أشهر، سنة)
- 📈 Progress bar تفاعلي

### 4. لوحة تحكم الأدمن
- 👥 عرض جميع عضويات الشركات
- 🔧 تفعيل/تعطيل العضويات يدوياً
- ➕ تمديد العضويات
- 📊 إحصائيات شاملة
- 🔍 فلترة (الكل، نشطة، غير نشطة، تنتهي قريباً)

### 5. العرض العام
- 🎨 شارة "عضوية نشطة" في صفحات البروفايل
- 👁️ إخفاء الشركات غير النشطة من نتائج البحث
- ✨ تصميم UI جذاب ومتجاوب

## 🗂️ الملفات الأساسية

### قاعدة البيانات
```
scripts/create-membership-system.js  - إنشاء الجداول والحقول
scripts/check-membership-expiry.js   - فحص العضويات وإرسال الإشعارات
```

#### جدول `company_memberships`
```sql
- id (INTEGER PRIMARY KEY)
- companyId (INTEGER)
- status (TEXT) - active/inactive
- startDate (TEXT - ISO)
- endDate (TEXT - ISO)
- paymentDate (TEXT - ISO)
- paymentAmount (REAL)
- notificationSent (INTEGER) - 0,1,3,7
```

#### حقول جديدة في `companies`
```sql
- membershipStatus (TEXT) - active/inactive
- membershipExpiry (TEXT - ISO)
```

### صفحات الواجهة

#### لوحة تحكم الشركة
```
app/company-dashboard/membership/page.tsx
```
- عرض حالة العضوية
- الأيام المتبقية
- Progress bar
- خطط التجديد (30، 90، 365 يوم)

#### لوحة تحكم الأدمن
```
app/admin-panel/memberships/page.tsx
```
- جدول جميع الشركات
- فلترة متقدمة
- إحصائيات فورية
- إدارة كاملة للعضويات

#### Navigation
```
components/company-dashboard/CompanyDashboardNav.tsx
```
- قائمة تنقل لصفحات dashboard الشركة

### APIs

#### `/api/membership` (GET)
احصل على بيانات عضوية شركة معينة
```javascript
GET /api/membership?companyId=5
Response: {
  id, firstName, membershipStatus, membershipExpiry
}
```

#### `/api/membership/renew` (POST)
تجديد العضوية
```javascript
POST /api/membership/renew
Body: { companyId: 5, days: 30 }
Response: {
  success: true,
  newExpiry: "2025-12-21T...",
  message: "Membership renewed successfully"
}
```

#### `/api/admin/memberships` (GET)
احصل على جميع عضويات الشركات (Admin فقط)
```javascript
GET /api/admin/memberships
Response: {
  companies: [...]
}
```

#### `/api/admin/memberships/toggle` (POST)
تفعيل/تعطيل عضوية (Admin)
```javascript
POST /api/admin/memberships/toggle
Body: { companyId: 5, status: "active" }
```

#### `/api/admin/memberships/extend` (POST)
تمديد عضوية (Admin)
```javascript
POST /api/admin/memberships/extend
Body: { companyId: 5, days: 90 }
```

## 🚀 التثبيت والإعداد

### 1. إنشاء نظام العضوية
```bash
node scripts/create-membership-system.js
```
هذا السكريبت:
- ينشئ جدول `company_memberships`
- يضيف حقول `membershipStatus` و `membershipExpiry`
- يفعّل عضوية تجريبية 30 يوم لجميع الشركات الموجودة
- ينشئ indexes للأداء

### 2. جدولة فحص العضويات (Cron Job)
```bash
# تشغيل يومياً في منتصف الليل
0 0 * * * cd /path/to/project && node scripts/check-membership-expiry.js
```

أو على Windows Task Scheduler:
```powershell
# تشغيل يومياً الساعة 12 صباحاً
schtasks /create /tn "Check Memberships" /tr "node d:\mahmoud hammad\scripts\check-membership-expiry.js" /sc daily /st 00:00
```

## 📝 سير العمل (Workflow)

### 1. تسجيل شركة جديدة
```
التسجيل → الموافقة → عضوية تجريبية 30 يوم تلقائياً
```

### 2. إدارة العضوية من الشركة
```
Dashboard → العضوية → عرض الحالة → تجديد (30/90/365 يوم)
```

### 3. إدارة من الأدمن
```
Admin Panel → إدارة العضويات → فلترة → تفعيل/تعطيل/تمديد
```

### 4. انتهاء العضوية
```
قبل 7 أيام → إشعار
قبل 3 أيام → إشعار
قبل يوم واحد → إشعار
اليوم 0 → تعطيل العضوية → إخفاء من نتائج البحث
```

## 🔍 الفلترة في APIs

### API الشركات العامة (`/api/companies`)
```sql
WHERE status = 'approved' 
AND membershipStatus = 'active' 
AND (membershipExpiry IS NULL OR membershipExpiry > NOW)
```

### API البروفايل (`/api/company-profile`)
```javascript
if (isOwner) {
  // المالك يرى ملفه دائماً
  query = "SELECT * WHERE id = ?"
} else {
  // الزوار يرون النشطة فقط
  query += "AND membershipStatus = 'active' AND ..."
}
```

## 🎨 UI Components

### Badge العضوية النشطة
```jsx
<div className="bg-gradient-to-r from-purple-500 to-pink-500 ...">
  <Shield icon />
  <span>عضوية نشطة</span>
</div>
```

### Progress Bar
```jsx
<div className="progress-bar">
  <div style={{ width: `${(daysLeft / 30) * 100}%` }} />
</div>
```

### خطط التجديد
```jsx
- شهر واحد (30 يوم) - مجاني حالياً
- 3 أشهر (90 يوم) - Best Value
- سنة كاملة (365 يوم)
```

## 📊 الإحصائيات

### Dashboard الأدمن
- إجمالي الشركات
- عضويات نشطة
- عضويات غير نشطة
- تنتهي قريباً (خلال 7 أيام)

### معلومات الشركة
- الحالة الحالية (active/inactive)
- تاريخ الانتهاء
- الأيام المتبقية
- نسبة الوقت المتبقي (progress)

## 🔐 الأمان

- ✅ فحص نوع المستخدم (company فقط)
- ✅ فحص صلاحيات Admin
- ✅ التحقق من ملكية الشركة
- ✅ منع التلاعب في التواريخ

## 🌐 متعدد اللغات

النظام يدعم العربية والإنجليزية:
```javascript
{lang === 'ar' ? 'عضوية نشطة' : 'Active Member'}
{lang === 'ar' ? 'الأيام المتبقية' : 'Days Remaining'}
```

## ⚙️ الإعدادات

### فترات الإشعارات
```javascript
const notificationPeriods = [7, 3, 1]; // أيام قبل الانتهاء
```

### خطط العضوية
```javascript
const plans = [
  { days: 30, price: 0 },   // شهر
  { days: 90, price: 0 },   // 3 أشهر
  { days: 365, price: 0 }   // سنة
];
```

## 🐛 استكشاف الأخطاء

### المشكلة: الشركة لا تظهر في نتائج البحث
```sql
-- تحقق من حالة العضوية
SELECT membershipStatus, membershipExpiry FROM companies WHERE id = ?

-- تفعيل يدوياً
UPDATE companies SET membershipStatus = 'active', 
membershipExpiry = datetime('now', '+30 days') WHERE id = ?
```

### المشكلة: الإشعارات لا ترسل
```bash
# تشغيل السكريبت يدوياً للتحقق
node scripts/check-membership-expiry.js

# تحقق من جدول الإشعارات
SELECT * FROM notifications WHERE type = 'membership_warning'
```

### المشكلة: العضوية انتهت لكن الشركة تظهر
```sql
-- تحديث فوري للحالة
UPDATE companies SET membershipStatus = 'inactive' 
WHERE membershipExpiry < datetime('now')
```

## 📅 المهام المستقبلية (Optional)

- [ ] تكامل بوابة دفع حقيقية (Stripe/Fawry)
- [ ] خطط أسعار مختلفة
- [ ] خصومات وعروض ترويجية
- [ ] تقارير مالية
- [ ] فواتير PDF
- [ ] إحصائيات متقدمة
- [ ] Email notifications
- [ ] Auto-renewal option

## 🎯 الخلاصة

نظام العضوية الآن:
✅ **جاهز للإنتاج**
✅ **متكامل بالكامل**
✅ **قابل للتوسع**
✅ **سهل الإدارة**

للشركات الجديدة: تحصل على 30 يوم مجاناً تلقائياً
للشركات الحالية: تم تفعيل 30 يوم تجريبي
للأدمن: تحكم كامل في جميع العضويات

---

**تم التطوير:** أكتوبر 2025  
**الإصدار:** 1.0.0  
**الحالة:** ✅ Production Ready
