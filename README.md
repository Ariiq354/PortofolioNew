# Portofolio Ariiq

Website portofolio dengan Astro, Tailwind CSS, dan Starwind UI. Semua halaman
dibangun menjadi HTML static di folder `dist/` untuk di-host di Cloudflare Pages.

## Pengembangan lokal

Gunakan Node.js 24 dan Bun 1.4.2.

```sh
bun install --frozen-lockfile
bun run dev
```

Server pengembangan tersedia di `http://localhost:4321`.

| Perintah | Fungsi |
| --- | --- |
| `bun run dev` | Menjalankan server pengembangan |
| `bun run build` | Menghasilkan website static di `dist/` |
| `bun run preview` | Memeriksa hasil build secara lokal |

## Deploy ke Cloudflare Pages

1. Push repository ke GitHub atau GitLab.
2. Buka dashboard Cloudflare → **Workers & Pages** → **Create application** →
   **Pages** → **Import an existing Git repository**.
3. Pilih repository dan branch produksi, lalu gunakan konfigurasi berikut:

   | Pengaturan | Nilai |
   | --- | --- |
   | Framework preset | Astro |
   | Build command | `bun install --frozen-lockfile && bun run build` |
   | Build output directory | `dist` |
   | Root directory | Kosongkan (root repository) |

4. Tambahkan environment variable untuk **Production** dan **Preview**:

   | Variable | Nilai |
   | --- | --- |
   | `NODE_VERSION` | `24` |
   | `BUN_VERSION` | `1.4.2` |
   | `SKIP_DEPENDENCY_INSTALL` | `true` |

   Instalasi dependency dijalankan oleh build command menggunakan `bun.lock`,
   sehingga versi dependency mengikuti lockfile repository.

5. Klik **Save and Deploy**. Cloudflare akan memberikan URL `*.pages.dev` dan
   melakukan build ulang setiap ada push ke branch produksi.

Halaman yang dihasilkan: `/`, `/work`, `/resume`, dan `/contact`. Navigasi,
tabs, dan tooltip menggunakan JavaScript di browser. Tombol **Email Me** membuka
aplikasi email pengunjung melalui `mailto:`. File CV di `src/assets/CV.pdf`
ikut dibundel ke output build untuk diunduh dari halaman utama.

Dokumentasi: [Astro di Cloudflare Pages](https://developers.cloudflare.com/pages/framework-guides/deploy-an-astro-site/)
dan [konfigurasi build environment](https://developers.cloudflare.com/pages/configuration/build-image/).
