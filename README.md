# Ortu Sehat — Android App

Sedentary & Intermittent Fasting Tracker. Dibungkus dari `www/index.html` (single-file app) jadi APK Android via **Capacitor**.

## Struktur

```
ortu-sehat-apk/
├── www/index.html              # App utama (source of truth)
├── resources/                  # Asset mentah untuk generate icon & splash
│   ├── icon.png                # Legacy icon 1024x1024
│   ├── icon-foreground.png     # Adaptive icon foreground (logo, transparan)
│   ├── icon-background.png     # Adaptive icon background (solid #1F3D1F)
│   └── splash.png              # Splash screen
├── capacitor.config.json       # Konfigurasi Capacitor (appId, warna, dsb)
├── assets.config.json          # Config untuk @capacitor/assets generator
└── .github/workflows/build-apk.yml   # CI build otomatis
```

## App Identity

| Atribut | Nilai |
|---|---|
| App Name | Ortu Sehat |
| Package ID | `com.ghan.ortusehat` |
| Theme color | `#8A9A5B` (sage green) |
| Background/splash | `#1F3D1F` (dark green) |

> **Ganti Package ID** di `capacitor.config.json` dan nanti otomatis kepakai saat `cap add android` sebelum kamu build pertama kali kalau mau app ID lain. Kalau sudah pernah generate folder `android/`, harus ganti manual juga di `android/app/build.gradle` (`applicationId`) dan `android/app/src/main/java/.../MainActivity.java` path package-nya.

## Build via GitHub Actions (otomatis)

1. Push repo ini ke GitHub.
2. Setiap push ke branch `main` akan trigger build otomatis.
3. Hasil APK ada di tab **Actions** → pilih run terakhir → download artifact `ortu-sehat-debug-apk`.
4. Mau bikin release APK: push git tag, misal:
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```
   APK otomatis ter-attach ke GitHub Release.

## Build lokal (opsional, kalau mau testing manual)

```bash
npm install
npx cap add android          # sekali saja, generate folder android/
npx capacitor-assets generate --android
npx cap sync android
cd android
./gradlew assembleDebug
```

APK ada di `android/app/build/outputs/apk/debug/app-debug.apk`.

## Catatan penting

- **Icon foreground** sudah aku resize jadi 1024x1024 persegi dengan logo di-center pada *safe zone* 66% (spec resmi Android adaptive icon) — sebelumnya rasio-nya landscape (3665x2241) yang kalau dipaksa jadi adaptive icon bakal gepeng/terpotong.
- **Warna background** aku ambil sample langsung dari pixel `icon-background.png` (`#1F3D1F`), bukan tebakan, supaya splash screen & adaptive icon background match persis.
- File `www/index.html` masih pakai CDN (Tailwind, Chart.js, FontAwesome, Google Fonts) — pastikan device ada koneksi internet saat pertama buka, atau nanti kalau mau full-offline perlu di-bundle lokal.
- Kalau nanti nambah fitur export Excel/backup DB pakai `@capacitor/filesystem` + `@capacitor/share`, dua plugin itu udah include di `package.json`.
# ORTU-SEHAT-
