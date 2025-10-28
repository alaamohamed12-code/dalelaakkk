# 🚀 دليل الاستخدام السريع - نظام التقييمات

## 📖 للمستخدمين

### كيفية تقييم شركة

1. **تسجيل الدخول**
   - يجب أن تكون مسجل دخول كمستخدم فرد (Individual)
   - الشركات لا يمكنها تقييم شركات أخرى

2. **زيارة صفحة الشركة**
   ```
   /company/[معرف-الشركة]
   ```

3. **الانتقال لتاب "التقييمات"**
   - ستجد التاب في قائمة الصفحة

4. **إضافة تقييمك**
   - اختر عدد النجوم (1-5)
   - اكتب تعليقك
   - اضغط "إضافة التقييم"

### ملاحظات مهمة
- ⚠️ يمكنك تقييم كل شركة **مرة واحدة فقط**
- ⚠️ التعليق **إلزامي**
- ⚠️ لا يمكن تعديل التقييم بعد الإضافة (يمكن الحذف فقط)

---

## 👨‍💼 للأدمن

### الوصول لإدارة التقييمات

1. **تسجيل الدخول للوحة التحكم**
   ```
   /admin-panel/login
   ```

2. **الذهاب لصفحة التقييمات**
   ```
   /admin-panel/reviews
   ```
   أو من لوحة التحكم → "إدارة التقييمات"

### الوظائف المتاحة

#### 1. عرض جميع التقييمات
- عرض كل التقييمات في الموقع
- معلومات الشركة والمستخدم
- تاريخ التقييم

#### 2. الفلترة
- **جميع التقييمات:** عرض الكل
- **تقييمات إيجابية:** 4-5 نجوم
- **تقييمات سلبية:** 1-2 نجوم

#### 3. حذف التقييمات
- إمكانية حذف أي تقييم غير لائق
- يتم تحديث متوسط الشركة تلقائياً

---

## 💻 للمطورين

### API Usage

#### جلب تقييمات شركة
```javascript
fetch('/api/reviews?companyId=5')
  .then(res => res.json())
  .then(data => {
    console.log(data.reviews); // التقييمات
    console.log(data.stats);   // الإحصائيات
  });
```

#### إضافة تقييم
```javascript
fetch('/api/reviews', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    companyId: 5,
    userId: 123,
    userFirstName: 'أحمد',
    userLastName: 'محمد',
    rating: 5,
    comment: 'خدمة ممتازة!'
  })
})
.then(res => res.json())
.then(data => console.log(data));
```

#### حذف تقييم
```javascript
fetch('/api/reviews?id=1&userId=123', {
  method: 'DELETE'
})
.then(res => res.json())
.then(data => console.log(data));
```

### Database Schema

```sql
-- جدول التقييمات
CREATE TABLE company_reviews (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  companyId INTEGER NOT NULL,
  userId INTEGER NOT NULL,
  userFirstName TEXT,
  userLastName TEXT,
  rating INTEGER NOT NULL CHECK(rating >= 1 AND rating <= 5),
  comment TEXT,
  createdAt TEXT DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(companyId, userId)
);

-- حقول إضافية في جدول الشركات
ALTER TABLE companies ADD COLUMN rating REAL DEFAULT 0;
ALTER TABLE companies ADD COLUMN reviewCount INTEGER DEFAULT 0;
```

### الملفات الرئيسية

```
app/
├── api/
│   ├── reviews/route.ts              # API للمستخدمين
│   └── admin/reviews/route.ts        # API للأدمن
├── company/[id]/page.tsx             # صفحة الشركة مع التقييمات
└── admin-panel/reviews/page.tsx      # إدارة التقييمات

scripts/
├── create-reviews-table.js           # إنشاء الجدول
├── check-reviews.js                  # فحص البيانات
└── test-reviews-api.js               # اختبار API
```

---

## 🔧 استكشاف الأخطاء

### المشكلة: لا تظهر التقييمات

**الحل:**
1. تأكد من وجود جدول `company_reviews`
   ```bash
   node scripts/check-reviews.js
   ```

2. تحقق من معرف الشركة
   ```javascript
   // تأكد من أن الشركة موجودة
   fetch('/api/company-profile?id=5')
   ```

### المشكلة: لا يمكن إضافة تقييم

**الحلول:**
1. تأكد من تسجيل الدخول كمستخدم فرد
2. تأكد من عدم تقييم الشركة مسبقاً
3. تأكد من كتابة تعليق

### المشكلة: خطأ في حساب المتوسط

**الحل:**
- يتم حساب المتوسط تلقائياً عند كل إضافة/حذف
- إذا كان هناك خطأ، يمكن إعادة حساب الكل:
   ```javascript
   // من قاعدة البيانات
   UPDATE companies SET 
     rating = (SELECT AVG(rating) FROM company_reviews WHERE companyId = companies.id),
     reviewCount = (SELECT COUNT(*) FROM company_reviews WHERE companyId = companies.id);
   ```

---

## 📞 الدعم

إذا واجهت أي مشكلة:
1. راجع تقرير المراجعة: `REVIEWS_TESTING_REPORT.md`
2. راجع وثائق النظام: `REVIEWS_SYSTEM_README.md`
3. تحقق من سجلات المتصفح (Console)
4. تحقق من سجلات الخادم (Terminal)

---

## ✅ قائمة التحقق قبل الإطلاق

- [ ] تشغيل `node scripts/check-reviews.js` للتأكد من قاعدة البيانات
- [ ] تشغيل `node scripts/test-reviews-api.js` لاختبار API
- [ ] اختبار إضافة تقييم من المتصفح
- [ ] اختبار صفحة الأدمن
- [ ] اختبار على موبايل
- [ ] مراجعة النصوص والترجمة
- [ ] التأكد من رسائل الأخطاء واضحة

---

**النظام جاهز! ابدأ باستخدامه الآن** 🎉
