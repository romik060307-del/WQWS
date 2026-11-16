# AI Floating Assistant — AssistiveTouch iPhone Style (v0.3.0)

Aplikasi Android native dengan tombol melayang transparan bergaya iPhone AssistiveTouch, panel AI di atas aplikasi lain, chat Gemini/DeepSeek, dan analisis screenshot dengan persetujuan Android.

## Yang sudah disiapkan

- Tombol kaca abu-abu transparan dengan ring putih, ikon AI, dan bayangan.
- Tombol dapat digeser dan otomatis menempel ke tepi kiri/kanan layar.
- Ketuk tombol untuk membuka panel di atas aplikasi aktif.
- Pilihan Gemini dan DeepSeek langsung dari panel.
- Chat teks dengan konteks beberapa pesan sebelumnya.
- Analisis screenshot melalui izin MediaProjection Android.
- Tidak ada halaman gateway, Cloudflare, pengaturan URL, atau input API key saat aplikasi dipakai.
- API dipanggil langsung dari aplikasi Android.

## PENTING: konsekuensi API key tertanam

Atas permintaan pemilik proyek, kredensial API provider dikemas di dalam source dan APK supaya tidak perlu setup gateway atau memasukkan key manual. Ini **bukan penyimpanan rahasia yang aman**: orang yang menerima APK dapat membongkar aplikasi dan mengekstrak kredensial. Kunci yang dikirim di percakapan juga sudah terekspos. Nilai di source disamarkan dengan Base64 hanya agar tidak terlihat sebagai teks biasa; Base64 **bukan enkripsi** dan tetap mudah dipulihkan. Jangan bagikan APK atau repository ini secara publik; segera rotasi/revoke kedua kunci pada dashboard provider setelah uji coba, lalu buat kunci baru dengan pembatasan kuota/akses.

Permintaan API akan memakai kuota dan ketentuan akun Gemini/DeepSeek pemilik key. Aplikasi tidak membuat kuota gratis atau tak terbatas. Aku tidak menjalankan permintaan uji dengan kunci tersebut, jadi validitas, saldo, izin model, dan keberhasilan respons aktual belum dapat dipastikan sampai diuji di HP.

## Build APK melalui GitHub Actions

1. Unduh dan ekstrak ZIP source.
2. Upload seluruh isi folder proyek ke root repository GitHub, termasuk folder tersembunyi `.github`.
3. Buka tab **Actions**.
4. Pilih **Build AI Floating Assistant APK** lalu tekan **Run workflow** (atau push commit ke branch `main`/`master`).
5. Setelah workflow berstatus hijau, unduh artifact `AI-Floating-Assistant-APK-v0.3.0`.
6. Ekstrak artifact dan pasang `app-debug.apk` di Android.

Workflow ini tidak membutuhkan Cloudflare, GitHub Secrets, API gateway URL, atau langkah deploy server. Workflow hanya membangun APK.

## Izin Android

- **Tampil di atas aplikasi lain** diperlukan untuk tombol AssistiveTouch dan panel.
- **Notifikasi** digunakan agar service melayang berjalan sesuai aturan Android.
- **Screenshot layar** hanya diambil setelah pengguna meminta analisis dan menyetujui dialog izin Android. Gambar dikirim langsung ke provider AI yang dipilih.
- Beberapa merek HP mungkin meminta izin autostart atau pengecualian optimasi baterai.

## Status pengujian dan batasan

- Source dan workflow sudah diperbarui untuk koneksi API langsung.
- File APK belum dibuild/diuji pada perangkat fisik dalam sesi ini.
- Kunci API belum diuji terhadap provider; jika key sudah dicabut, salah, kuota habis, atau model tidak diizinkan, aplikasi akan menampilkan pesan error dari provider.
- Artifact GitHub adalah debug APK, bukan rilis bertanda tangan untuk distribusi publik.
- Android tetap mewajibkan persetujuan pengguna sebelum screenshot layar diambil.

## Versi teknis

- Package: `com.ohli.floatingai`
- Minimum Android: 6.0 (API 23)
- Compile SDK: 36
- Target SDK: 35
- Android Gradle Plugin: 8.13.2
- Gradle: 8.13
- JDK: 17
- Gemini: `gemini-3.8-flash` melalui Gemini `generateContent` REST API
- DeepSeek: `deepseek-flash` melalui Chat Completions REST API, termasuk input gambar
