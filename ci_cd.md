## شرح ملف GitHub Actions بالتفصيل
---

### `name: Build and Upload Release APK`
هذا هو اسم الـ workflow وسيظهر في تبويب Actions داخل GitHub.

---

### `on:`
تحدد متى يتم تشغيل الـ workflow.

```yaml
on:
  push:
    branches:
      - "**"
```
معناه: يتم تشغيل هذا الـ workflow عند أي `push` لأي فرع (branch) في المشروع.

---

### `jobs:`
تحتوي على جميع المهام (Jobs) التي سيتم تنفيذها.

```yaml
jobs:
  build_release:
```
اسم الـ Job هو `build_release`.

```yaml
    if: contains(github.event.head_commit.message, 'build release')
```
يعني: سيتم تنفيذ الـ job فقط إذا كان في رسالة الـ commit عبارة `build release`.

```yaml
    runs-on: ubuntu-latest
```
يشير إلى أن هذه المهمة ستعمل على سيرفر Ubuntu.

---

### `steps:`
هي الخطوات التي سيتم تنفيذها داخل هذا الـ job.

#### 1. Checkout code
```yaml
- name: Checkout code
  uses: actions/checkout@v3
```
تقوم هذه الخطوة بجلب الكود الموجود في المستودع.

#### 2. Set up Java
```yaml
- name: Set up Java
  uses: actions/setup-java@v3
  with:
    distribution: "temurin"
    java-version: "17"
    cache: "gradle"
```
تقوم بإعداد Java 17 باستخدام Temurin وتفعيل الكاش لأدوات Gradle.

#### 3. Set up Flutter
```yaml
- name: Set up Flutter
  uses: subosito/flutter-action@v2
  with:
    flutter-version: "3.27.0"
    channel: "stable"
    cache: true
```
تقوم بتنصيب Flutter بإصدار محدد (3.27.0) على القناة المستقرة `stable`، وتفعيل الكاش.

#### 4. Install dependencies
```yaml
- name: Install dependencies
  run: flutter pub get
```
تنزيل كل الحزم المطلوبة من `pubspec.yaml`.

#### 5. Run Analyzer
```yaml
- name: Run Analyzer
  run: flutter analyze
  continue-on-error: true
```
تحليل الكود والتأكد من خلوه من الأخطاء البرمجية، بدون إيقاف الـ workflow إذا حدثت أخطاء.

---

#### 6. Cache Flutter dependencies
```yaml
- name: Cache Flutter dependencies
  uses: actions/cache@v3
  with:
    path: |
      ~/.pub-cache
      .dart_tool
      build/
      android/.gradle/
    key: ${{ runner.os }}-flutter-${{ hashFiles('**/*.gradle*','pubspec.lock') }}
```
هذه الخطوة تخزن ملفات Flutter وGradle و `.dart_tool` و `.pub-cache` في كاش لتسريع البناء في المستقبل. المفتاح (key) يتم إنشاؤه تلقائيًا بناءً على نظام التشغيل ومحتوى ملفات البناء.

#### 7. Build release APK
```yaml
- name: Build release APK
  run: flutter build apk --release
```
يقوم ببناء نسخة APK من التطبيق بصيغة "release"، وهي النسخة الجاهزة للنشر.

#### 8. Extract Folder ID from commit message
```yaml
- name: Extract Folder ID from commit message
  id: extract_folder
  run: |
    FOLDER_ID=$(echo "${{ github.event.head_commit.message }}" | grep -oP 'FOLDER_ID=\K\S+')
    echo "folder_id=$FOLDER_ID" >> $GITHUB_OUTPUT
```
يستخرج معرف مجلد Google Drive من رسالة الـ commit (يجب أن تحتوي الرسالة على `FOLDER_ID=xxxxxxxx`) ثم يحفظها كمتغير يمكن استخدامه في الخطوات القادمة.

#### 9. Write Google credentials to file
```yaml
- name: Write Google credentials to file
  run: echo '${{ secrets.GOOGLE_DRIVE_CREDENTIALS }}' > credentials.json
```
يكتب بيانات اعتماد Google Drive (المحفوظة في GitHub Secrets) إلى ملف `credentials.json` ليتم استخدامه لاحقًا بواسطة سكربت Python.

#### 10. Upload APK to Google Drive & Send Email
```yaml
- name: Upload APK to Google Drive & Send Email
  env:
    FOLDER_ID: ${{ steps.extract_folder.outputs.folder_id }}
    EMAIL_USER: ${{ secrets.EMAIL_USER }}
    EMAIL_PASS: ${{ secrets.EMAIL_PASS }}
    EMAIL_RECEIVER: ${{ secrets.EMAIL_RECEIVER }}
    GITHUB_REF_NAME: ${{ github.ref_name }}
  run: |
    pip install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib

    cat > upload_and_email.py <<EOF
    import os
    import smtplib
    from email.mime.text import MIMEText
    from google.oauth2 import service_account
    from googleapiclient.discovery import build
    from googleapiclient.http import MediaFileUpload

    # === Google Drive Upload ===
    FOLDER_ID = os.environ['FOLDER_ID']
    SCOPES = ['https://www.googleapis.com/auth/drive.file']
    SERVICE_ACCOUNT_FILE = 'credentials.json'

    credentials = service_account.Credentials.from_service_account_file(
        SERVICE_ACCOUNT_FILE, scopes=SCOPES)

    service = build('drive', 'v3', credentials=credentials)

    ref_name = os.environ.get('GITHUB_REF_NAME', 'unknown-branch')

    file_name = f"app-release-{ref_name}.apk"

    file_metadata = {
        'name': file_name,
        'parents': [FOLDER_ID]
    }
    media = MediaFileUpload(
        'build/app/outputs/flutter-apk/app-release.apk',
        mimetype='application/vnd.android.package-archive'
    )
    file = service.files().create(
        body=file_metadata,
        media_body=media,
        fields='id, webViewLink'
    ).execute()

    file_link = file.get("webViewLink")
    print("✅ File uploaded to Google Drive!")
    print("📎 Link:", file_link)

    # === Send Email ===
    to_emails = [email.strip() for email in os.environ['EMAIL_RECEIVER'].split(',')]
    msg = MIMEText(f"✅ APK build is complete!\n\n🔗 Download link:\n{file_link}")
    msg['Subject'] = f"✅ New APK Build Available for {ref_name}"
    msg['From'] = os.environ['EMAIL_USER']
    msg['To'] = ', '.join(to_emails)

    try:
        with smtplib.SMTP_SSL('smtp.gmail.com', 465) as server:
            server.login(os.environ['EMAIL_USER'], os.environ['EMAIL_PASS'])
            server.send_message(msg)
            print("📬 Email sent successfully to:", ', '.join(to_emails))
    except Exception as e:
        print("❌ Failed to send email:", e)
    EOF
    python3 upload_and_email.py
```

هذا السكربت يقوم بتنفيذ الخطوتين:
1. رفع ملف APK إلى Google Drive باستخدام API.
2. إرسال بريد إلكتروني يحتوي على رابط الملف إلى الإيميلات المحددة.