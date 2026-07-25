# Focus & Life Tracker

To'liq funksional Flutter ilovasi: rejalashtiruvchi, landshaft Fokus rejimi,
ilova aktivligi tahlili va ikkita mavzu (An'anaviy / High-Tech Neon HUD).

## Ishga tushirish

```bash
flutter pub get
flutter run
```

Talab qilinadi: Flutter SDK 3.22+ (Dart 3.3+), Android Studio yoki Xcode
(iOS uchun).

Android'da: birinchi marta ishga tushirilganda **Sozlamalar > Tahlil**
bo'limida "Usage Access" ruxsatini qo'lda yoqish so'raladi (bu maxsus
ruxsat turi bo'lib, oddiy runtime dialog orqali so'ralmaydi — bu Android
tizimining o'zi belgilagan cheklov).

iOS'da ilova aktivligi tahlili uchun alohida native kengaytma kerak —
tafsilotlar: `ios/DeviceActivityExtension/README.md`.

## Arxitektura

```
lib/
  core/theme/          — ikkita mavzu (Classic, High-Tech Neon HUD)
  data/
    models/            — Task, FocusSession, AppUsage, UserSettings
    database/           — SQLite (sqflite) sxema: 4 jadval
    repositories/        — CRUD qatlami
  providers/            — Provider (ChangeNotifier) — state boshqaruvi
  services/             — Notifications, Usage Stats (native),
                           Audio (oq shovqin), Screen Wake
  screens/              — Planner, Focus Mode (landscape), Analytics,
                           Settings
  widgets/              — MiniCalendar, TimerDisplay, GamifiedProgress, ...
```

State-boshqaruv sifatida `provider` (ChangeNotifier) ishlatildi — bu
Flutter ekotizimida BLoC bilan bir xil "clean, scalable architecture"
maqsadiga xizmat qiladi, lekin ancha kam boilerplate bilan. Agar
loyihangiz uchun aynan BLoC kerak bo'lsa, `providers/` qatlamidagi har bir
klass to'g'ridan-to'g'ri mos Bloc/Cubit'ga ko'chiriladi — repository va UI
qatlamlari o'zgarishsiz qoladi.

## Ma'lumotlar bazasi

SQLite (`sqflite`) orqali 4 jadval: `users_settings`, `tasks`,
`focus_sessions`, `app_usage_logs` — aynan siz bergan sxema bo'yicha
(`lib/data/database/database_helper.dart`).

## Muhim eslatmalar

- **Screen Time / App Usage**: Android'da `UsageStatsManager` orqali
  to'liq ishlaydi (`usage_stats` paketi). iOS'da Apple'ning
  DeviceActivity/Family Controls entitlementi va alohida Swift
  kengaytmasi talab qilinadi — bu appstore review jarayonidan alohida
  o'tishi kerak bo'lgan native qism, shuning uchun Dart kodida
  scaffold + aniq TODO ko'rsatmalari bilan qoldirilgan.
- **White-noise audio**: `assets/sounds/` papkasiga real audio fayllar
  qo'shishingiz kerak (litsenziyaga ega, loop qilinadigan) — README shu
  papkada.
- **Ikonka va splash**: `flutter_launcher_icons` / `flutter_native_splash`
  paketlarini qo'shib, o'z brendingizga moslashtiring.
 
