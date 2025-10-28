# دليلك للأفضل 🏗️

منصة لربط المستخدمين بشركات الهندسة والمقاولات في دول الخليج

## 🚀 المميزات

- ✅ **نظام مصادقة كامل** - للمستخدمين والشركات
- 🏢 **لوحة تحكم الشركات** - إدارة الملف، الأعمال، الاشتراكات
- 💬 **نظام رسائل** - محادثات مباشرة ودعم فني
- 🔔 **إشعارات فورية** - تحديثات لحظية
- ⭐ **نظام تقييمات** - تقييم الشركات والخدمات
- 👨‍💼 **لوحة تحكم الأدمن** - إدارة متقدمة مع صلاحيات
- 🔍 **بحث متقدم** - تصفية حسب الخدمات والمناطق

## 🛠️ التقنيات المستخدمة

- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Animations**: Framer Motion
- **Database**: SQLite
- **State**: React Hooks

## 📦 التثبيت والتشغيل

```bash
# تثبيت المكتبات
npm install

# تشغيل المشروع في بيئة التطوير
npm run dev

# فتح المتصفح على
# http://localhost:3000
```

## 🏗️ هيكل المشروع

```
├── app/                    # Next.js App Router
│   ├── admin-panel/       # لوحة تحكم الأدمن
│   ├── company-dashboard/ # لوحة تحكم الشركات
│   ├── api/               # API Routes
│   └── ...
├── components/            # مكونات React
│   ├── layout/           # Header, Footer
│   ├── home/             # الصفحة الرئيسية
│   └── admin/            # مكونات الأدمن
├── lib/                  # Helper functions
├── types/                # TypeScript types
├── styles/               # CSS files
└── public/               # Static files
```

## 🔐 متغيرات البيئة

أنشئ ملف `.env.local`:

```env
NEXT_PUBLIC_API_URL=http://localhost:3000
```

## 📝 ملاحظات

- قاعدة البيانات: SQLite محلية (*.db files)
- الملفات المرفوعة في: `/public/uploads/`
- الصور في: `/public/company-works/`

## 🎨 التحسينات الأخيرة

- ✅ تحسين Header component (Performance + Code Quality)
- ✅ فصل Mobile Navigation
- ✅ إضافة TypeScript types
- ✅ تحسين API calls
- ✅ تحسين Error handling

## 📄 الترخيص

جميع الحقوق محفوظة © 2025

---

**صُنع بـ ❤️ في الخليج العربي**
