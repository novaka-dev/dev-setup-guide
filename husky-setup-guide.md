# 🦊 Setup Husky (Versi Terbaru) untuk Git Hook

Panduan ini akan membantumu setup Husky (v9+) untuk menjalankan `lint`, `prettier`, `commitlint`, `lint-staged`, dan `commitizen` secara otomatis sebelum commit. Cocok untuk project Next.js, React, atau lainnya.

---

## ✨ 1. Install Husky & Inisialisasi

```bash
npm install --save-dev husky
npx husky init
```

Ini akan:

- Menambahkan `prepare` script otomatis ke `package.json`
- Membuat folder `.husky/`
- Membuat file hook `.husky/pre-commit` default

> Kalau `npx husky init` belum menambahkan script `prepare`, tambahkan manual ke `package.json`:

```json
"scripts": {
  "prepare": "husky install"
}
```

---

## 🧹 2. Setup Pre-commit Hook

Pre-commit digunakan untuk menjalankan linting & formatting **sebelum commit terjadi**.

### ✅ Rekomendasi: Gunakan lint-staged agar hanya file yang berubah dicek

#### a. Install `lint-staged`

```bash
npm install --save-dev lint-staged
```

#### b. Tambahkan config di `package.json`

```json
"lint-staged": {
  "*.{js,jsx,ts,tsx}": [
    "eslint --fix",
    "prettier --write"
  ],
  "*.{json,md,css,html}": [
    "prettier --write"
  ]
}
```

#### c. Ubah isi file `.husky/pre-commit`

```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```

---

## 🔒 3. Setup Commitlint untuk Validasi Pesan Commit

```bash
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

### Buat file `commitlint.config.js` di root:

```js
module.exports = {
  extends: ["@commitlint/config-conventional"],
};
```

---

## 🧾 4. Setup Hook commit-msg

Hook ini akan memvalidasi pesan commit agar sesuai format `feat(scope): pesan`.

### Isi file: `.husky/commit-msg`

```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx commitlint --edit "$1"
```

Jangan lupa:

```bash
chmod +x .husky/commit-msg
```

---

## 🛠️ 5. Setup Commitizen (Interaktif Format Commit)

```bash
npm install --save-dev commitizen
npx commitizen init cz-conventional-changelog --save-dev --save-exact
```

### Tambahkan script ke `package.json`

```json
"scripts": {
  "commit": "cz"
}
```

### Cara pakai:

```bash
git add .
npm run commit
```

> Akan muncul pertanyaan interaktif: pilih tipe, scope, dan deskripsi — commit akan otomatis dibentuk rapi.

---

## ✅ Contoh Struktur Folder Husky

```
.husky/
├── _/                 # Folder helper dari husky
│   └── husky.sh
├── pre-commit         # Jalankan lint-staged
└── commit-msg         # Validasi pesan commit
```

---

## 🎉 Selesai!

Setiap kali kamu:

```bash
git add .
npm run commit
```

Husky akan:

1. Jalankan `lint-staged` untuk file yang berubah (`pre-commit`)
2. Validasi pesan commit dengan `commitlint` (`commit-msg`)
3. Format commit kamu dibantu dengan Commitizen (interaktif)

---

> Ini workflow modern untuk tim dev yang produktif dan konsisten. Bisa kamu simpan sebagai `HUSKY_SETUP.md` atau langsung di README project kamu 🚀
