# 📋 PANDUAN INSTALASI & DEPLOYMENT — Form Data Pribadi Karyawan MANO AGRI NUSANTARA

## A. Backend + Aplikasi (Google Apps Script)

1. Buka https://script.google.com → **Proyek Baru**. Beri nama `MANO Form Data Pribadi`.
2. Ganti isi `Code.gs` dengan isi **Kode.gs**, lalu rename file menjadi `Kode`.
3. Tambah file **Script**: `Notifikasi` (isi Notifikasi.gs) dan `Migrasi` (isi Migrasi.gs).
4. Tambah file **HTML** (File → New → HTML), nama tanpa `.html`: `Index`, `Stylesheet`, `JavaScript`. Paste isi masing-masing.
5. Jalankan fungsi **`setupAppEnvironment`** (pilih di dropdown → ▶ Run) dan beri izin (Spreadsheet, Drive, Gmail/Mail, UrlFetch, Trigger).
6. Buka **Execution log**. Catat:
   - URL Spreadsheet dan folder Drive yang dibuat,
   - **Sandi HRD awal** (hanya tampil sekali). Lupa? Jalankan `resetSandiHRD`.
7. **Deploy → New deployment → Web app**: *Execute as* **Me**, *Who has access* **Anyone**. Salin URL `/exec`.
8. Buka URL tersebut → formulir pelamar tampil. Klik **Masuk HRD** di bawah halaman, login dengan sandi dari langkah 6.
9. Menu **Pengaturan** (HRD): isi email HRD, aktifkan Email/WhatsApp, tempel **token Fonnte** (disimpan di Script Properties, tidak pernah dikirim ke browser), simpan.

⚠️ Setiap mengubah kode: **Deploy → Manage deployments → Edit → New version → Deploy**.

## B. Embed di iframe (Blogger / situs lain)

```html
<div style="position:fixed;top:0;left:0;width:100%;height:100%;z-index:9999">
  <iframe src="URL_EXEC_WEB_APP" style="width:100%;height:100%;border:none" allow="camera; clipboard-write"></iframe>
</div>
```
Aplikasi hanya memakai satu URL `/exec` tanpa parameter, sehingga aman di iframe.

## C. Pasang sebagai aplikasi di HP & komputer (PWA via GitHub Pages)

1. Buat repository GitHub (mis. `mano-form`), unggah seluruh isi folder `mano-agri-pwa/` ke root.
2. Edit **config.js**: ganti `GAS_URL` dengan URL `/exec` dari langkah A.7.
3. **Settings → Pages → Deploy from a branch → main / (root)** → Save. Tunggu 1–2 menit.
4. Buka `https://USERNAME.github.io/mano-form/`:
   - **Android/Chrome & Desktop**: tombol **⬇ Pasang Aplikasi** muncul (atau menu ⋮ → Install).
   - **iPhone/Safari**: Bagikan → **Tambah ke Layar Utama** (petunjuk tampil otomatis).

## D. Migrasi dari spreadsheet lama (opsional)

Menu **Pengaturan → Migrasi**: tempel ID/URL spreadsheet lama (akun Google yang sama) → **Pindai** (simulasi) → **Impor**. Aman diulang (delta-sync). Status & catatan HRD yang sudah ada tidak ditimpa.

## E. Uji cepat setelah deploy

| Uji | Hasil yang diharapkan |
|---|---|
| Isi formulir 3 halaman, unggah foto/KTP/ijazah, kirim | Halaman konfirmasi muncul instan, chip berubah “Tersimpan di server”, baris baru di lembar `Pelamar`, folder pelamar di Drive |
| Matikan internet saat kirim | Muncul “Belum terkirim” + tombol Kirim Ulang, draf teks tetap ada |
| Login HRD salah 5× | Terkunci 15 menit |
| Ubah status + centang notifikasi | Lembar `Log` dan `AntrianNotif` terisi; trigger 1 menit mengirim |
| Filter di Daftar Pelamar | Dashboard & grafik ikut berubah |
| Unduh Excel | File `.xlsx` berisi semua / hasil filter |
