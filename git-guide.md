# 📘 Panduan Lengkap Git Branch untuk Proyek Next.js

Panduan ini cocok buat kamu yang sedang mengembangkan proyek seperti portofolio menggunakan **Next.js, Tailwind CSS, ShadCN UI, dsb** dengan kolaborasi via GitHub.

---

## 🔖 Daftar Prefix Branch (Konvensi Profesional)

Gunakan prefix ini agar struktur branch rapi dan mudah dipahami oleh tim.

| Prefix      | Fungsi                                       | Contoh                                 |
| ----------- | -------------------------------------------- | -------------------------------------- |
| `feat/`     | Penambahan fitur baru                        | `feat/about-page`, `feat/contact-form` |
| `fix/`      | Perbaikan bug                                | `fix/navbar-error`, `fix/image-load`   |
| `docs/`     | Perubahan dokumentasi                        | `docs/readme`, `docs/setup-guide`      |
| `style/`    | Perubahan tampilan (CSS, spacing, dsb)       | `style/hero`, `style/responsive`       |
| `refactor/` | Perubahan struktur kode (tanpa ubah fungsi)  | `refactor/components`                  |
| `chore/`    | Hal teknis: setup, config, update dependensi | `chore/husky`, `chore/update-eslint`   |
| `test/`     | Penambahan/perbaikan test                    | `test/form-validation`                 |

---

## 🆕 Membuat & Pindah Branch

### Buat branch baru dan langsung pindah:

```bash
git checkout -b nama-branch
```

Contoh:

```bash
git checkout -b feat/about-page
```

### Pindah ke branch lain:

```bash
git checkout nama-branch
```

---

## 🚀 Push Branch ke GitHub

### Push pertama kali:

```bash
git push -u origin nama-branch
```

Contoh:

```bash
git push -u origin feat/about-page
```

> `-u` menyimpan referensi ke remote, jadi berikutnya cukup `git push` aja

---

## 🔄 Merge Branch

### Merge branch ke `dev`:

```bash
git checkout dev
git pull origin dev

git merge nama-branch
```

Lalu push hasil merge:

```bash
git push origin dev
```

---

## 🧹 Menghapus Branch

### Hapus branch lokal:

```bash
git branch -d nama-branch
```

> Gunakan `-D` (huruf besar) jika ingin paksa hapus

### Hapus branch di remote (GitHub):

```bash
git push origin --delete nama-branch
```

---

## 🔍 Perintah Cek Branch

### Lihat semua branch lokal:

```bash
git branch
```

### Lihat semua branch di remote:

```bash
git branch -r
```

### Lihat semua (lokal + remote):

```bash
git branch -a
```

### Cek kamu sedang di branch apa:

```bash
git branch --show-current
```

---

## ✅ Contoh Commit dengan Format yang Benar

Gunakan commit format **Conventional Commits** biar pesan commit rapi dan konsisten:

### 📌 Format umum:

```
type(scope): pesan commit singkat
```

### 🧪 Contoh:

```bash
git commit -m "feat(about): tambah animasi fade-in di section about"
```

```bash
git commit -m "fix(navbar): perbaiki menu tidak muncul di mobile"
```

```bash
git commit -m "docs(readme): tambahkan instruksi setup awal"
```

> Gunakan prefix seperti `feat:`, `fix:`, `docs:`, `refactor:`, `chore:`, `style:`, dan `test:` sesuai perubahan yang kamu buat.

---

## 📝 Rangkuman Perintah Penting

| Fungsi              | Perintah                               |
| ------------------- | -------------------------------------- |
| Lihat semua branch  | `git branch -a`                        |
| Buat branch baru    | `git checkout -b nama-branch`          |
| Pindah branch       | `git checkout nama-branch`             |
| Push branch         | `git push -u origin nama-branch`       |
| Merge branch        | `git merge nama-branch`                |
| Hapus branch lokal  | `git branch -d nama-branch`            |
| Hapus branch remote | `git push origin --delete nama-branch` |
| Lihat branch aktif  | `git branch --show-current`            |
| Commit standar      | `git commit -m "feat(scope): pesan"`   |

---

> Simpan panduan ini di repo kamu (misalnya sebagai `git-guide.md`) biar semua kontributor paham alurnya. Mau lanjut bahas pull request & code review juga? 😉
