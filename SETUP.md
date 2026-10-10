# دليل الإعداد المفصل

دليل شامل لإعداد بيئة التطوير لمشروع ELPOP Studio على جهازك المحلي.

## المتطلبات الأساسية

### 1. Java Development Kit (JDK)

**للتحقق من التثبيت:**
```bash
java -version
javac -version
```

**التثبيت:**

**Windows:**
- تحميل من [Oracle](https://www.oracle.com/java/technologies/downloads/)
- أو استخدم Chocolatey:
```bash
choco install openjdk11
```

**macOS:**
```bash
brew install openjdk@11
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt-get update
sudo apt-get install openjdk-11-jdk
```

### 2. Android Studio

**التحميل من:** [https://developer.android.com/studio](https://developer.android.com/studio)

**بعد التثبيت:**
1. افتح Android Studio
2. اذهب إلى Preferences/Settings → SDK Manager
3. ثبت أحدث Android SDK (Android 13+)
4. ثبت Build Tools (الإصدار 33+)
5. ثبت Android Emulator (اختياري للاختبار)

### 3. Git

**التحقق:**
```bash
git --version
```

**التثبيت:**

**Windows:**
```bash
choco install git
```

**macOS:**
```bash
brew install git
```

**Linux:**
```bash
sudo apt-get install git
```

---

## خطوات الإعداد

### الخطوة 1: استنساخ المشروع

```bash
# استنساخ المشروع
git clone https://github.com/amelpop1122-web/improved-eureka.git

# الدخول إلى مجلد المشروع
cd improved-eureka

# إضافة المستودع الأصلي (للتحديثات)
git remote add upstream https://github.com/amelpop1122-web/improved-eureka.git
```

### الخطوة 2: إعداد متغيرات البيئة

**Linux/macOS:**

أنشئ ملف `~/.bashrc` أو `~/.zshrc`:

```bash
export JAVA_HOME=$(/usr/libexec/java_home)
export ANDROID_SDK_ROOT=~/Library/Android/sdk
export PATH=$PATH:$ANDROID_SDK_ROOT/tools:$ANDROID_SDK_ROOT/platform-tools
```

ثم أعد تحميل:
```bash
source ~/.bashrc  # أو ~/.zshrc
```

**Windows:**

1. فتح Settings → System → Environment Variables
2. أضف متغيرات جديدة:
   - `JAVA_HOME`: مسار JDK (مثل `C:\Program Files\Java\jdk-11`)
   - `ANDROID_SDK_ROOT`: مسار Android SDK (مثل `C:\Users\YourName\AppData\Local\Android\Sdk`)

3. أضف إلى `Path`:
   - `%ANDROID_SDK_ROOT%\tools`
   - `%ANDROID_SDK_ROOT%\platform-tools`

### الخطوة 3: التحقق من الإعداد

```bash
# تحقق من Java
java -version

# تحقق من Android SDK
$ANDROID_SDK_ROOT/tools/bin/sdkmanager --version

# تحقق من adb
adb --version
```

### الخطوة 4: فتح المشروع في Android Studio

```bash
# في مجلد المشروع
# Windows
start android-studio .

# macOS
open -a "Android Studio" .

# Linux
android-studio .
```

أو:
1. افتح Android Studio
2. اختر "Open"
3. اختر مجلد `improved-eureka`

### الخطوة 5: بناء المشروع الأول

```bash
# تنزيل المتطلبات
./gradlew build

# أو من Android Studio
# Build → Make Project
```

---

## اختبار التطبيق

### على محاكي Android

```bash
# قائمة المحاكاة المتاحة
emulator -list-avds

# بدء محاكي
emulator -avd Pixel_6_API_33

# تثبيت وتشغيل التطبيق
./gradlew installDebug
./gradlew run
```

### على جهاز حقيقي

**الخطوة 1: تفعيل Developer Options**
1. اذهب إلى Settings → About phone
2. اضغط على "Build Number" 7 مرات
3. عودة إلى Settings → Developer Options
4. فعّل "USB Debugging"

**الخطوة 2: الاتصال عبر USB**
```bash
# تحقق من الجهاز المتصل
adb devices

# تثبيت التطبيق
./gradlew installDebug
```

---

## استكشاف الأخطاء الشائعة

### المشكلة: "ANDROID_SDK_ROOT not set"

**الحل:**
```bash
# تحديد المسار
export ANDROID_SDK_ROOT=~/Library/Android/sdk

# أو في gradle.properties
echo "sdk.dir=$ANDROID_SDK_ROOT" >> local.properties
```

### المشكلة: "Gradle sync failed"

**الحل:**
```bash
# امسح cache
./gradlew clean

# أعد المحاولة
./gradlew build
```

### المشكلة: "adb: command not found"

**الحل:**
```bash
# أضف إلى PATH
export PATH=$PATH:~/Library/Android/sdk/platform-tools
```

### المشكلة: "Unable to get local issuer certificate"

**الحل:**
```bash
# قد تحتاج إلى شهادة SSL
./gradlew build --no-https
```

---

## أوامر مفيدة في التطوير

### البناء والتشغيل

```bash
# بناء debug
./gradlew assembleDebug

# بناء release
./gradlew assembleRelease

# تشغيل على جهاز/محاكي
./gradlew installDebug
./gradlew run

# شاشة logcat
adb logcat
```

### الاختبارات

```bash
# جميع الاختبارات
./gradlew test

# اختبارات معينة
./gradlew test --tests com.elpop.studio.*

# اختبارات الواجهة
./gradlew connectedAndroidTest
```

### التنظيف

```bash
# مسح build
./gradlew clean

# مسح cache
./gradlew cleanBuildCache

# مسح كل شيء
rm -rf build .gradle
```

---

## الإعدادات المتقدمة

### تسريع البناء

**في `gradle.properties`:**

```properties
# تفعيل الهندسة المعمارية الأصلية
android.useAndroidX=true

# تسريع البناء
org.gradle.parallel=true
org.gradle.workers.max=8
org.gradle.configureondemand=true

# استخدام daemon
org.gradle.daemon=true
org.gradle.daemon.idletimeout=3600000
```

### التصحيح (Debugging)

```bash
# تشغيل مع debugger
./gradlew installDebug
./gradlew connectedCheck

# في Android Studio:
# Run → Debug (أو Shift+F9)
```

---

## المجلدات والملفات المهمة

```
improved-eureka/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/elpop/studio/
│   │   │   ├── res/
│   │   │   ├── AndroidManifest.xml
│   │   │   └── ...
│   │   ├── test/        # Unit tests
│   │   └── androidTest/ # Integration tests
│   ├── build.gradle     # تكوين التطبيق
│   └── proguard-rules.pro
├── build.gradle         # تكوين المشروع
├── settings.gradle      # إعدادات Gradle
├── gradle.properties    # خصائص Gradle
└── local.properties     # (تم إنشاؤه محلياً فقط)
```

---

## التعديات وأفضل الممارسات

### Git Workflow

```bash
# الفرع الرئيسي
git checkout main

# إنشاء فرع جديد
git checkout -b feature/my-feature

# بعد الانتهاء
git add .
git commit -m "feat: add new feature"
git push origin feature/my-feature

# ثم افتح Pull Request على GitHub
```

### Commit Guidelines

استخدم البادئات:
- `feat:` - ميزة جديدة
- `fix:` - إصلاح خطأ
- `docs:` - توثيق
- `style:` - تنسيق الكود
- `refactor:` - إعادة هيكلة
- `test:` - إضافة اختبارات

**مثال:**
```bash
git commit -m "feat(auth): add login validation"
git commit -m "fix(ui): correct button alignment"
```

---

## الموارد الإضافية

- [Android Developer Docs](https://developer.android.com/docs)
- [Gradle Build System](https://gradle.org/)
- [Java Documentation](https://docs.oracle.com/javase/11/)
- [GitHub Guides](https://guides.github.com/)

---

**هل تواجه مشكلة؟** افتح issue على GitHub أو اتصل بفريق التطوير.

آخر تحديث: October 2026
