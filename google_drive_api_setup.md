
# إعداد Google Drive API وحساب الخدمة في GitHub Actions

## ✅ الخطوة 1: إعداد Google Drive API وحساب الخدمة (Service Account)

### 1.1 الدخول إلى Google Cloud Console
- افتح [Google Cloud Console](https://console.cloud.google.com/) وسجّل الدخول بحسابك في Google.

### 1.2 إنشاء مشروع جديد أو اختيار مشروع موجود
- اضغط على شعار المشروع في أعلى الصفحة، ثم اختر "New Project".

### 1.3 تفعيل Google Drive API
- من القائمة الجانبية: **APIs & Services > Library**  
- ابحث عن **Google Drive API** واضغط عليها، ثم اضغط **Enable**.

### 1.4 إنشاء Service Account (حساب خدمة)
- من القائمة الجانبية: **APIs & Services > Credentials**  
- اضغط على **Create Credentials > Service Account**.  
- أدخل اسم الحساب ثم اضغط **Create and Continue**.  
- لا حاجة لإعطاء صلاحيات إضافية، اضغط **Done**.

### 1.5 تحميل مفتاح JSON
- بعد إنشاء الحساب، ستجده في قائمة "Service Accounts".  
- اضغط على البريد الإلكتروني الخاص بالحساب.  
- انتقل إلى تبويب **Keys**.  
- اضغط على **Add Key > Create new key**.  
- اختر **JSON** ثم اضغط **Create**.  
- سيتم تحميل ملف JSON تلقائيًا. هذا هو الملف الذي ستحتاجه.

---

## ✅ الخطوة 2: مشاركة مجلد Google Drive مع حساب الخدمة

### 2.1 مشاركة المجلد مع حساب الخدمة
- افتح مجلد Google Drive الذي ترغب في رفع الـ APK إليه.  
- اضغط على **Share** (مشاركة).  
- الصق **البريد الإلكتروني لحساب الخدمة** في حقل "Add People". (مثلًا: `your-service-account@your-project.iam.gserviceaccount.com`).  
- امنح الحساب صلاحية **Editor** ثم اضغط **Send**.

### 2.2 نسخ الـ Folder ID
- افتح المجلد في Google Drive.  
- عنوان URL الخاص بالمجلد سيكون مثل:  
  ```
  https://drive.google.com/drive/u/0/folders/1ABCDEF123456789xyz
  ```  
- الجزء الذي بعد `/folders/` هو الـ **folder ID**:  
  ```
  1ABCDEF123456789xyz
  ```

---

## ✅ الخطوة 3: إضافة المتغيرات في GitHub

### 3.1 الدخول إلى إعدادات الريبو في GitHub
- اذهب إلى GitHub > الريبو الخاص بك > **Settings**.

### 3.2 إضافة المتغيرات داخل "Secrets and Variables"
- من القائمة الجانبية اختر **Secrets and Variables > Actions**.  
- اضغط على **New Repository Secret** لإضافة المتغيرات.

### 3.3 إضافة المتغيرات
- **المتغير الأول: `GOOGLE_DRIVE_CREDENTIALS`**  
  - **Name:** `GOOGLE_DRIVE_CREDENTIALS`  
  - **Value:** افتح ملف الـ JSON الذي حمّلته وانسخه بالكامل (المحتوى كاملًا).
  
- **المتغير الثاني: `GOOGLE_DRIVE_FOLDER_ID`**  
  - **Name:** `GOOGLE_DRIVE_FOLDER_ID`  
  - **Value:** الصق الـ folder ID الذي نسخته في الخطوة 2.2.

---

## ✅ الخطوة 4: استخدام المتغيرات في GitHub Actions

الآن، يمكن لـ GitHub Actions استخدام هذه القيم تلقائيًا داخل ملف الـ workflow الخاص بك:

```yaml
with:
  credentials: ${{ secrets.GOOGLE_DRIVE_CREDENTIALS }}
  folderId: ${{ secrets.GOOGLE_DRIVE_FOLDER_ID }}
```

---

إذا واجهت أي مشكلة أو كنت بحاجة لمساعدة إضافية في أي من الخطوات، لا تتردد في إخباري. سأكون سعيدًا بمساعدتك أكثر!
