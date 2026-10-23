.github/workflows/build.yml# TwoPluse — منصة الإنتاجية وتطوير الذات المتقدمة (تطبيق أندرويد أصلي بالعربية)

**TwoPluse** هو تطبيق أندرويد أصلي (Native Android) متكامل، مصمم باللغة العربية أولاً وبدعم كامل لاتجاه العرض من اليمين إلى اليسار (**RTL**)، وبخط **الكوفي المربع (Square Kufi)** المدمج محلياً، مع التزام صارم بنظام الألوان البنفسجي والسيان المستقبلي المعتمد.

## ١. الهوية البصرية والتقنية
- **اسم التطبيق**: `TwoPluse`
- **معرّف الحزمة (Package Name)**: `com.twopluse.app`
- **الإصدار**: `1.0.0` (`versionCode = 1`)
- **الحد الأدنى لنظام أندرويد (minSdk)**: `Android 8.0 (API 26)`
- **النظام المستهدف (targetSdk / compileSdk)**: `Android 15 (API 35)`
- **المعمارية**: `MVVM + Repository Pattern + Clean Domain Engines`
- **التقنيات المستخدمة**:
  - `Kotlin 2.0.21` + `Jetpack Compose (Material 3)`
  - `Room Database 2.6.1` مع `TypeConverters` واستعلامات تفاعلية `Flow` وترحيل قواعد البيانات (`MIGRATION_1_2`)
  - `DataStore Preferences` لحفظ الإعدادات واستعادة حالة مؤقت الدراسة
  - `WorkManager` + `NotificationCompat` + `BroadcastReceiver` للتذكيرات المحلية وإعادة الجدولة بعد إعادة تشغيل الجهاز
  - `Navigation Compose` للتنقل بين الوجهات الخمس الرئيسية والشاشات الفرعية
  - `Kotlinx Serialization JSON` للنسخ الاحتياطي المحلي والاستعادة وتصدير `CSV`

## ٢. لوحة الألوان المعتمدة حصرياً (Strict Color Palette)
يلتزم التطبيق حرفياً بلوحة الألوان المحددة دون أي ألوان خارجية:
- الخلفيات العميقة: `#120232`، `#1F0345`
- البنفسجي الكهربائي: `#5305A1`، `#6D2AC9`
- البنفسجي المضيء: `#8351F5`
- الأزرق الكهربائي: `#587DF1`
- السيان المضيء: `#66D1FC`
- اللافندر: `#A087F5`
- الوردي-البنفسجي الناعم: `#C58AEF`
- النص الأساسي: `#FFFFFF`

## ٣. الخط العربي (الكوفي المربع — Square Kufi)
- تم تضمين ملفات الخط الكوفي محلياً داخل المشروع في المسار:
  - `app/src/main/res/font/kufi_square_regular.ttf`
  - `app/src/main/res/font/kufi_square_bold.ttf`
- لاستبدال ملف الخط بملف خط كوفي مربع مخصص آخر مستقبلاً، يكفي استبدال هذين الملفين بنفس الاسمين داخل `app/src/main/res/font/` دون تعديل أي كود برمجي.

## ٤. خطوات بناء ملف APK (Debug & Signed Release)

### أ. عبر Android Studio
1. افتح برنامج **Android Studio (Ladybug 2024.2.1 أو أحدث)**.
2. اختر **Open** وحدد مجلد المشروع `TwoPluse-Android`.
3. انتظر اكتمال مزامنة **Gradle Sync**.
4. لبناء نسخة التقييم (**Debug APK**):
   - من القائمة العلوية اختر: **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
   - ستجد ملف الـ APK الناتج في المسار:
     `app/build/outputs/apk/debug/app-debug.apk`

### ب. عبر سطر الأوامر (Command Line)
```bash
# لبناء نسخة Debug APK القابلة للتثبيت المباشر:
./gradlew assembleDebug

# لتشغيل اختبارات الوحدة (Unit Tests):
./gradlew testDebugUnitTest

# لبناء نسخة Release موقعة (Signed Release APK):
export TWOPLUSE_KEYSTORE_PATH="/path/to/release-keystore.jks"
export TWOPLUSE_KEYSTORE_PASSWORD="your_keystore_password"
export TWOPLUSE_KEY_ALIAS="twopluse_key"
export TWOPLUSE_KEY_PASSWORD="your_key_password"
./gradlew assembleRelease
```

### ج. إنشاء مفتاح توقيع جديد (Keystore) لإصدارات الإنتاج
```bash
keytool -genkey -v -keystore twopluse-release.jks -keyalg RSA -keysize 2048 -validity 10000 -alias twopluse_key
```
