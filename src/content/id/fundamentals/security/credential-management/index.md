project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml

{# wf_updated_on: 2016-11-08 #} {# wf_published_on: 2016-11-08 #}

# Credential Management API {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/megginkearney.html" %}

[Credential Management API](https://www.w3.org/TR/credential-management/) adalah API browser berbasis standar yang menyediakan antarmuka programatik antara situs dan browser untuk masuk tanpa kendala, ke berbagai perangkat, dan menghilangkan gesekan dari aliran masuk.

Credential Management API:

* **Menyederhanakan aliran masuk** - Pengguna bisa secara otomatis masuk kembali ke situs, bahkan jika sesinya telah berakhir.
* **Memungkinkan masuk dengan satu ketukan menggunakan pemilih akun** - Sebuah pemilih akun asli akan ditampilkan yang meniadakan formulir masuk.
* **Menyimpan kredensial** - Bisa menyimpan kombinasi nama pengguna & sandi atau bahkan detail akun gabungan.

Ingin melihat aksinya? Cobalah [demo Credential Management API](https://credential-management-sample.appspot.com) dan lihat [kodenya](https://github.com/GoogleChrome/credential-management-sample).

Walaupun ada banyak cara agar berhasil mengintegrasikan Credential Management API, spesifik integrasi ini bergantung pada struktur dan pengalaman pengguna situs, situs yang menggunakan aliran tersebut memiliki keunggulan pengalaman pengguna ini:

<div class="clearfix"></div>

### Ambil kredensial dan masuk pengguna

Poin Utama: Penggunaan Credential Management API mengharuskan laman disajikan dari asal yang aman.

    if (window.PasswordCredential || window.FederatedCredential) {
      // Call navigator.credentials.get() to retrieve stored
      // PasswordCredentials or FederatedCredentials.
    }
    

Untuk memasukkan pengguna, ambil kredensial dari pengelola sandi di browser dan gunakan untuk memproses masuknya pengguna.

### Simpan atau perbarui kredensial pengguna

Misalnya:

Ketahui selengkapnya di [Ambil Kredensial](/web/fundamentals/security/credential-management/retrieve-credentials).

1. Bila pengguna mendarat di situs Anda dan mereka tidak masuk, panggil `navigator.credential.get()`
2. Gunakan kredensial yang diambil untuk memasukkan pengguna.
3. Perbarui UI untuk menunjukkan bahwa pengguna yang telah masuk.

Jika pengguna telah masuk dengan nama pengguna dan sandi:

### Keluar

Jika pengguna masuk dengan penyedia identitas gabungan seperti Google Sign-In, Facebook, GitHub, dll:

1. Setelah pengguna berhasil masuk, membuat akun atau mengubah sandi, buat `PasswordCredential` dengan ID pengguna dan sandi.
2. Simpan objek kredensial menggunakan `navigator.credentials.store()`.

Ketahui selengkapnya di [Simpan Kredensial](/web/fundamentals/security/credential-management/store-credentials).

Bila pengguna keluar, panggil `navigator.credentials.requireUserMediation()` untuk mencegah pengguna dimasukkan kembali secara otomatis.

1. Setelah pengguna berhasil masuk, membuat akun atau mengubah sandi, buat `FederatedCredential` dengan alamat email pengguna sebagai ID dan tetapkan penyedia identitas dengan `.provider`
2. Simpan objek kredensial menggunakan `navigator.credentials.store()`.

Dengan menonaktifkan masuk-otomatis juga memungkinkan pengguna beralih akun dengan mudah, misalnya, antara akun pribadi dan kantor, atau antara akun di perangkat bersama, tanpa harus memasukkan kembali informasi masuk mereka.

### Sign out

Ketahui selengkapnya di [Keluar](/web/fundamentals/security/credential-management/retrieve-credentials#sign-out).

Disabling auto-sign-in also enables users to switch between accounts easily, for example, between work and personal accounts, or between accounts on shared devices, without having to re-enter their sign-in information.

{# wf_devsite_translation #}

## Langkah-langkah untuk mengimplementasikan Pengelolaan Kredensial

{% include "web/_shared/helpful.html" %}