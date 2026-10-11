# Arsitektur Sistem

## Tech Stack
- **Framework:** Nuxt 4 (struktur direktori `app/`)
- **Bahasa:** TypeScript (strict mode)
- **Database:** MariaDB
- **Styling:** Tailwind CSS + Nuxt Icon

## Struktur Direktori
- `app/pages/`: Routing berbasis file; merender UI dan menangani data fetching tingkat halaman via `useFetch` / `useAsyncData`.
- `app/layouts/`: Kerangka layout utama (navigasi, sidebar, header); membungkus halaman melalui `<slot />`.
- `app/components/`: Komponen UI modular yang dapat digunakan kembali (auto-imported).
- `app/composables/`: Business logic sisi klien dan utility reaktif (auto-imported).
- `app/types/`: Khusus tipe UI/klien (misal: state komponen, view model, prop komponen).
- `server/api/`: Endpoint Nitro API untuk menangani request HTTP, validasi server-side, dan query database.
- `server/utils/`: Database client, handler autentikasi, dan helper server-side (auto-imported khusus di lingkungan server).
- `shared/types/`: Tipe yang dipakai bersama antara frontend dan backend (misal: DTO request/response API, interface entity/model).

## Alur Data & State Utama
1. **Request dari Klien:** Halaman/komponen memanggil data menggunakan `useFetch('/api/...')` dengan dukungan hidrasi SSR.
2. **Pemrosesan Nitro:** Endpoint di `server/api/*.ts` memvalidasi input, memeriksa cookie session/token auth, lalu menjalankan query ke MariaDB.
3. **Database & Serialisasi:** MariaDB mengembalikan data; Nitro men-serialize payload tersebut menjadi JSON kembali ke klien.
4. **Sinkronisasi State Klien:** Data respons mengisi store Pinia atau ref reaktif lokal tanpa mengekspos koneksi database ke sisi browser.