# 🏦 الجمعية المالية الأسرية — دليل التثبيت والإعداد

## 📁 هيكل المشروع
```
family-finance-pwa/
├── index.html          ← التطبيق الرئيسي
├── manifest.json       ← إعدادات PWA
├── sw.js               ← Service Worker (العمل بدون إنترنت)
├── icons/              ← أيقونات التطبيق بجميع الأحجام
│   ├── icon.svg
│   ├── icon-72x72.png
│   ├── icon-96x96.png
│   ├── icon-128x128.png
│   ├── icon-144x144.png
│   ├── icon-152x152.png
│   ├── icon-192x192.png
│   ├── icon-384x384.png
│   └── icon-512x512.png
└── README.md
```

---

## 🔥 إعداد Firebase (خطوات سريعة)

### 1. إنشاء مشروع Firebase
1. اذهب إلى https://console.firebase.google.com
2. اضغط **إضافة مشروع** → اختر اسماً مثل `family-finance-app`
3. أكمل الإعداد (يمكن تعطيل Google Analytics)

### 2. إنشاء قاعدة بيانات Firestore
1. من القائمة الجانبية → **Firestore Database**
2. اضغط **إنشاء قاعدة بيانات**
3. اختر **وضع الاختبار** (Test Mode) مؤقتاً
4. اختر المنطقة الأقرب: `europe-west6` أو `asia-south1`

### 3. الحصول على إعدادات التطبيق
1. اضغط ⚙️ (إعدادات المشروع) → **تطبيقاتك**
2. اضغط أيقونة الويب `</>`
3. سجّل اسم التطبيق → اضغط **تسجيل**
4. انسخ كائن `firebaseConfig`

### 4. تحديث `index.html`
افتح `index.html` وابحث عن:
```javascript
const FIREBASE_CONFIG = {
  apiKey:            "YOUR_API_KEY",
  authDomain:        "YOUR_PROJECT.firebaseapp.com",
  ...
};
```
**استبدل القيم** بإعداداتك الحقيقية من Firebase.

### 5. قواعد Firestore (للأمان)
في Firestore → Rules، ضع هذه القواعد:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      // للاستخدام العائلي المحدود — غيّر هذا لاحقاً
      allow read, write: if true;
    }
  }
}
```

---

## 🌐 نشر التطبيق (Firebase Hosting)

```bash
# تثبيت Firebase CLI
npm install -g firebase-tools

# تسجيل الدخول
firebase login

# تهيئة المشروع
cd family-finance-pwa
firebase init hosting

# النشر
firebase deploy
```

بعد النشر ستحصل على رابط مثل:
`https://your-project.web.app`

---

## 📱 تثبيت التطبيق على الهاتف

### Android (Chrome)
1. افتح رابط التطبيق في Chrome
2. ستظهر رسالة **"إضافة إلى الشاشة الرئيسية"** تلقائياً
3. أو اضغط ⋮ → **تثبيت التطبيق**
4. اضغط **تثبيت** ← يظهر التطبيق كأيقونة مستقلة!

### iPhone (Safari)
1. افتح رابط التطبيق في Safari
2. اضغط زر المشاركة ↑ (Share)
3. اختر **إضافة إلى الشاشة الرئيسية**
4. اضغط **إضافة**

### الاستضافة المحلية للاختبار
```bash
# Python
python3 -m http.server 8080

# Node.js
npx serve .

# ثم افتح: http://localhost:8080
```

---

## ⚡ كيف يعمل وضع الأوف لاين
- عند فتح التطبيق لأول مرة مع إنترنت → يتم تحميل كل الملفات في الذاكرة
- عند قطع الإنترنت → يعمل التطبيق بالكامل محلياً
- البيانات تُحفظ في `localStorage` دائماً
- عند العودة للإنترنت → مزامنة تلقائية مع Firebase

---

## 🔒 ملاحظات أمنية
- لا تشارك `apiKey` علناً (GitHub عام مثلاً)
- فعّل **Firebase Authentication** لحماية البيانات
- حدّث قواعد Firestore بعد الإعداد الأولي

