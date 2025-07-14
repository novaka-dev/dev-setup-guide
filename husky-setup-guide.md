# 🦊 Setup Husky (Versi Terbaru) untuk Git Hook

Panduan ini akan membantumu setup Husky (v9+) untuk menjalankan `lint`, `prettier`, `commitlint`, `lint-staged`, dan `commitizen` secara otomatis sebelum commit. Cocok untuk project Next.js, React, atau lainnya.

[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](https://commitizen.github.io/cz-cli/)
![Lint Passed](https://img.shields.io/badge/lint-passed-success?style=flat-square&color=brightgreen)

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

## ⚙️ Bonus: GitHub Actions untuk ESLint Otomatis

Buat file baru:

```
.github/workflows/lint.yml
```

### Isi file `lint.yml`

```yaml
name: ESLint Check

on:
  push:
    branches: [main, dev]
  pull_request:
    branches: [main, dev]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Run ESLint
        run: npx eslint . --ext .js,.jsx,.ts,.tsx
```

### Tambahkan badge ke README:

```md
![ESLint](https://github.com/[username]/[repo]/actions/workflows/lint.yml/badge.svg)
```

Contoh:

```md
![ESLint](https://github.com/novaka-dev/git-workflow/actions/workflows/lint.yml/badge.svg)
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
4. Otomatis lint dicek setiap push via GitHub Actions

---

> Ini workflow modern untuk tim dev yang produktif dan konsisten. Simpan sebagai `HUSKY_SETUP.md` atau langsung di README project kamu 🚀
