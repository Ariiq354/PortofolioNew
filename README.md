# Portofolio Ariiq

Website portofolio dengan Astro, Tailwind CSS, dan Starwind UI. Semua halaman
dibangun menjadi HTML static di folder `dist/` untuk di-host di Cloudflare Workers
melalui Static Assets. Konfigurasi deploy tersedia di `wrangler.jsonc`.

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

## Deploy ke Cloudflare Workers

1. Push repository ke GitHub atau GitLab.
2. Buka dashboard Cloudflare → **Workers & Pages** → **Create application**,
   lalu hubungkan repository Git.
3. Pilih repository dan branch produksi, lalu gunakan konfigurasi berikut:

   | Pengaturan | Nilai |
   | --- | --- |
   | Project name | `portofolionew` |
   | Build command | `bun install --frozen-lockfile && bun run build` |
   | Deploy command | `npx wrangler@4.148.0 deploy` |
   | Preview command | `npx wrangler@4.148.0 preview` |
   | Enable Preview builds | OFF untuk setup awal |
   | Protect with Cloudflare Access | OFF untuk portofolio publik |
   | Root directory | Default (root repository) |

4. Tambahkan **Build variables** di **Advanced settings**, atau di
   **Settings → Build → Build Variables and Secrets** setelah project dibuat:

   | Variable | Nilai |
   | --- | --- |
   | `NODE_VERSION` | `24` |
   | `BUN_VERSION` | `1.4.2` |
   | `SKIP_DEPENDENCY_INSTALL` | `true` |

   Instalasi dependency dijalankan oleh build command menggunakan `bun.lock`,
   sehingga versi dependency mengikuti lockfile repository.

5. Klik **Deploy**. Cloudflare akan memberikan URL `*.workers.dev` dan
   melakukan build ulang setiap ada push ke branch produksi.

Wrangler menggunakan `name`, `compatibility_date`, dan folder `assets.directory`
dari `wrangler.jsonc`. Jika mengganti nama project di dashboard, sesuaikan juga
`name` pada file tersebut. Push file konfigurasi ini sebelum menjalankan build
di Cloudflare.

Untuk deploy dari komputer lokal setelah login Cloudflare:

```sh
bun run build
npx wrangler@4.148.0 login
npx wrangler@4.148.0 deploy
```

Halaman yang dihasilkan: `/`, `/work`, `/resume`, dan `/contact`. Navigasi,
tabs, dan tooltip menggunakan JavaScript di browser. Tombol **Email Me** membuka
aplikasi email pengunjung melalui `mailto:`. File CV di `src/assets/CV.pdf`
ikut dibundel ke output build untuk diunduh dari halaman utama.

Dokumentasi: [Workers Static Assets](https://developers.cloudflare.com/workers/static-assets/)
dan [konfigurasi build environment](https://developers.cloudflare.com/workers/ci-cd/builds/build-image/).
