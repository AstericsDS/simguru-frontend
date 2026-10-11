# Standar Koding & Batasan (Coding Standards & Constraints)

## Konvensi (Conventions)
- **Penamaan (Naming):**
  - PascalCase untuk nama komponen Vue (`UserProfile.vue`), composable types, dan class.
  - camelCase untuk function, method, composable (`useAuth.ts`), dan variabel.
  - kebab-case untuk nama file halaman di `app/pages/` dan folder route.
- **Typing (TypeScript):** Wajib *strict typing*. Dilarang keras memakai `any` atau membiarkan parameter/return type tanpa definisi tipe.
- **Error Handling:** Tangani error secara eksplisit. Di sisi klien, tampilkan UI error/feedback yang jelas. Di sisi server (`server/api/`), lemparkan error menggunakan `createError()` bawaan Nuxt/H3 dengan status code yang seragam.
- **Komponen (Components):**
  - Pisahkan komponen besar menjadi bagian-bagian kecil (modular).
  - Untuk bagian yang butuh proses data berat, pisahkan menjadi komponen mandiri dan manfaatkan `<Suspense>` atau Nuxt lazy components (`<LazyComponentName />`) agar proses loading berjalan independen tanpa membekukan halaman utama.
- **Gaya Koding (Simplicity):** Utamakan kesederhanaan (KISS). Jangan *overengineer* atau membuat abstraksi berlebihan untuk fitur yang sederhana.

## Larangan Keras (Strict Prohibitions - Never Do)
- **Dilarang menambah library pihak ketiga:** Jangan menambah dependency/package baru di `package.json` tanpa instruksi eksplisit.
- **Dilarang menaruh logic berat di template:** Hindari ekspresi kompleks di dalam tag `<template>`. Pindahkan logic ke `computed` property atau fungsi di `<script setup>`.
- **Dilarang menulis query database mentah tanpa parameter binding:** Di dalam `server/api/`, jangan merangkai string SQL manual (string concatenation). Wajib gunakan query builder, ORM, atau prepared statement berparameter untuk mencegah SQL Injection.
- **Dilarang membocorkan kredensial ke frontend:** Jangan pernah mengimpor koneksi database MariaDB, secret key, atau query data langsung di dalam folder `app/`. Semua interaksi data sensitif wajib lewat `server/api/`.
- **Dilarang mengakses API browser langsung saat SSR:** Jangan panggil objek browser (`window`, `localStorage`, `document`) langsung di top-level `<script setup>`. Gunakan pengecekan `import.meta.client` atau jalankan di dalam lifecycle hook `onMounted()`.