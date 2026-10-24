# ربط البورتفوليو بـ Firebase — خطوات سريعة

1. console.firebase.google.com ← Add project (أنشئ مشروعًا).
2. Project settings (إعدادات المشروع) ← Your apps ← أيقونة الويب `</>` ← سجّل التطبيق وانسخ كائن `firebaseConfig`.
3. افتح `index.html` وابحث عن `FIREBASE_CONFIG` والصق القيم (apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId).
4. Build ← Authentication ← Get started ← فعّل **Email/Password** ← تبويب Users ← Add user (بريدك + كلمة مرور قوية). انسخ **User UID**.
5. Build ← Firestore Database ← Create database ← تبويب Rules والصق:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /portfolio/{doc} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == 'ضع_الـUID_هنا';
    }
  }
}
```

6. Build ← Storage ← Get started (قد يتطلب خطة Blaze — الاستخدام الصغير غالبًا مجاني) ← تبويب Rules:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /portfolio/{allPaths=**} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == 'ضع_الـUID_هنا'
                   && request.resource.size < 50 * 1024 * 1024;
    }
  }
}
```

7. Authentication ← Settings ← Authorized domains ← أضف `citimarketn.github.io`.
8. ارفع `index.html` على GitHub، ثم افتح: `https://citimarketn.github.io/<repo>/#admin` وسجّل الدخول.

## ملفات الرفع على GitHub (كلها في نفس المجلد)
- `index.html`
- `manifest.webmanifest`
- مجلد `icons/` (icon-192.png, icon-512.png, apple-touch-icon.png, favicon.png)

بعد الرفع: على الموبايل افتح الموقع ← «إضافة إلى الشاشة الرئيسية» وسيظهر شعار «وادي المدينة» كأيقونة تطبيق.
