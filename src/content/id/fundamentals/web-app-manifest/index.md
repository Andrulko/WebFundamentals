project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Manifes aplikasi web adalah file JSON yang memberikan Anda kemampuan untuk mengontrol bagaimana aplikasi web atau situs terlihat oleh pengguna di daerah yang mereka harap akan melihat aplikasi asli (misalnya, layar beranda perangkat), mengarahkan apa yang bisa diluncurkan pengguna, dan menentukan tampilannya pada saat peluncuran.

{# wf_updated_on: 2017-10-06 #} {# wf_published_on: 2016-02-11 #}

# Manifes Aplikasi Web {: .page-title }

{% include "web/_shared/contributors/mattgaunt.html" %} {% include "web/_shared/contributors/paulkinlan.html" %}

[Manifes aplikasi web](https://developer.mozilla.org/en-US/docs/Web/Manifest) adalah file JSON sederhana yang memberikan Anda, developer, kemampuan untuk mengontrol bagaimana aplikasi terlihat oleh pengguna di daerah yang mereka harap akan melihat aplikasi (misalnya, layar beranda perangkat seluler), mengarahkan apa yang bisa diluncurkan pengguna, dan menentukan tampilannya pada saat peluncuran.

Manifes aplikasi web menyediakan kemampuan untuk menyimpan bookmark situs ke layar beranda perangkat. Ketika sebuah situs diluncurkan dengan cara ini:

## Buat manifes

Semua ini dilakukan melalui mekanisme sederhana dari metadata dalam file teks. Itulah manifes aplikasi web.

    {
      "short_name": "AirHorner",
      "name": "Kinlan's AirHorner of Infamy",
      "icons": [
        {
          "src": "launcher-icon-1x.png",
          "type": "image/png",
          "sizes": "48x48"
        },
        {
          "src": "launcher-icon-2x.png",
          "type": "image/png",
          "sizes": "96x96"
        },
        {
          "src": "launcher-icon-4x.png",
          "type": "image/png",
          "sizes": "192x192"
        }
      ],
      "start_url": "index.html?launcher=true"
    }
    

Note: Meskipun Anda bisa menggunakan manifes aplikasi web di situs mana pun, semua itu diperlukan untuk [aplikasi web progresif](/web/progressive-web-apps/).

## Beri tahu browser tentang manifes Anda

Sebelum mendalami detail manifes aplikasi web, mari kita buat manifes dasar dan menautkan laman web ke situ.

    <link rel="manifest" href="/manifest.json">
    

## Setel sebuah URL mulai

### TL;DR {: .hide-from-toc }

Anda bisa memanggil manifes kapan pun Anda membutuhkannya. Kebanyakan orang menggunakan `manifest.json`. Inilah contohnya:

    "start_url": "/?utm_source=homescreen"
    

### Setel gambar dan judul

Pastikan untuk menyertakan yang berikut ini:

Bila telah membuat manifes dan sudah ada di situs Anda, tambahkan tag `link` ke semua laman yang mencakup aplikasi web Anda, seperti berikut:

    "icons": [{
        "src": "images/touch/icon-128x128.png",
        "type": "image/png",
        "sizes": "128x128"
      }, {
        "src": "images/touch/apple-touch-icon.png",
        "type": "image/png",
        "sizes": "152x152"
      }, {
        "src": "images/touch/ms-touch-icon-144x144-precomposed.png",
        "type": "image/png",
        "sizes": "144x144"
      }, {
        "src": "images/touch/chrome-touch-icon-192x192.png",
        "type": "image/png",
        "sizes": "192x192"
      }],
    

Jika Anda tidak menyediakan `start_url`, maka laman saat ini akan digunakan, yang mungkin saja bukan yang diinginkan pengguna. Namun itu bukan satu-satunya alasan untuk menyertakannya. Karena Anda sekarang bisa mendefinisikan cara meluncurkan aplikasi, tambahkan parameter string kueri ke `start_url` yang menunjukkan cara meluncurkannya.

### Setel warna latar belakang

Ini bisa berupa apa saja yang Anda inginkan; nilai yang kami gunakan memiliki keuntungan karena bermakna bagi Google Analytics.

Ketika pengguna menambahkan situs ke layar beranda mereka, Anda bisa menetapkan set ikon untuk digunakanbrowser. Anda bisa mendefinisikannya bersama tipe dan ukuran, seperti berikut:

    "background_color": "#2196F3",
    

Note: Saat menyimpan sebuah ikon ke layar beranda, Chrome terlebih dahulu akan mencari ikon yang cocok dengan kepadatan tampilan dan mengubah ukurannya ke kepadatan layar 48 dp. Jika tidak ada yang ditemukan, Chrome akan menelusuri ikon yang paling cocok dengan karakteristik perangkat. Jika, karena alasan apa pun, Anda ingin spesifik dalam menargetkan ikon pada kepadatan piksel-tertentu, Anda bisa menggunakan anggota opsional `density`, yang mengambil sebuah angka. Bila Anda tidak mendeklarasikan kepadatan, nilai defaultnya adalah 1.0. Ini berarti "gunakan ikon ini untuk kepadatan layar 1.0 ke atas", itulah yang biasanya Anda inginkan.

### Setel warna tema

Ketika Anda menjalankan aplikasi web dari layar utama, sejumlah hal terjadi di belakang layar:

### Sesuaikan tipe tampilan

Saat ini terjadi, layar menjadi putih dan seperti terhenti. Hal ini sangat jelas jika Anda memuat laman web dari jaringan dan memerlukan waktu lebih dari satu atau dua detik untuk menampilkan materi di laman beranda.

    "display": "standalone"
    

<table id="display-params" class="responsive">
  <tbody>
    <tr>
      <th colspan=2>Parameters</th>
    </tr>
    <tr>
      <td><code>value</code></td><td><code>Description</code></td>
    </tr>
    <tr id="display-fullscreen">
      <td><code>fullscreen</code></td>
      <td>
        Opens the web application without any browser UI and takes
        up the entirety of the available display area.
      </td>
    </tr>
    <tr>
      <td><code>standalone</code></td>
      <td>
        Opens the web app to look and feel like a standalone native
        app. The app runs in its own window, separate from the browser, and
        hides standard browser UI elements like the URL bar, etc.</td>
    </tr>
    <tr>
      <td><code>minimal-ui</code></td>
      <td>
        <b>Not supported by Chrome</b><br>
        This mode is similar to <code>fullscreen</code>, but provides the
        user with some means to access a minimal set of UI elements for
        controlling navigation (i.e., back, forward, reload, etc).
      </td>
    </tr>
    <tr>
      <td><code>browser</code></td>
      <td>A standard browser experience.</td>
    </tr>
  </tbody>
</table>

Untuk menyediakan pengalaman pengguna yang lebih baik, Anda bisa mengganti layar putih dengan judul, warna, dan gambar.

### Tetapkan orientasi awal laman

Jika mengikuti dari awal, berarti Anda sudah menyetel gambar dan judul. Chrome menduga gambar dan judul dari anggota manifes tertentu. Yang penting di sini adalah mengetahui secara spesifik.

    "display": "browser"
    

### `scope` {: #scope }

Gambar layar pembuka gambar digambar dari larik `icons`. Chrome memilih gambar yang paling mendekati 128dp untuk perangkat. Judul diambil langsung dari anggota `name`.

    "orientation": "landscape"
    

Menetapkan warna latar belakang menggunakan properti `background_color` yang diberi nama dengan tepat. Chrome menggunakan warna ini begitu aplikasi web diluncurkan dan warnanya akan tetap di layar hingga render pertama aplikasi web.

Untuk menyetel warna latar belakang, setel yang berikut ini di manifes Anda:

* Situs akan memiliki ikon dan nama yang unik sehingga pengguna bisa membedakannya dari situs yang lain.
* Situs akan menampilkan sesuatu kepada pengguna selagi sumber daya diunduh atau dipulihkan dari cache.
* Situs akan menyediakan karakteristik tampilan default ke browser untuk menghindari transisi yang terlalu mendadak bila sumber daya situs tersedia.
* The `start_url` is relative to the path defined in the `scope` attribute.
* A `start_url` starting with `/` will always be the root of the origin.

### `theme_color` {: #theme-color }

Sekarang, tidak ada layar putih yang muncul saat situs Anda diluncurkan dari layar beranda.

    "theme_color": "#2196F3"
    

Nilai disarankan yang bagus untuk properti ini adalah warna latar belakang dari laman muat. Penggunaan warna yang sama seperti pemuatan laman memungkinkan transisi mulus dari layar pembuka ke laman beranda.

Tetapkan warna tema menggunakan properti `theme_color`. Properti ini menetapkan warna bilah alat. Untuk hal ini, kami juga menyarankan duplikasi warna yang ada, khususnya `theme-color` `<meta>`.

## Sesuaikan berbagai ikon

<figure class="attempt-right">
  <img src="images/background-color.gif" alt="Menambahkan ke Ikon Layar Utama">
  <figcaption>Menambahkan ke Ikon Layar Utama</figcaption>
</figure>

Menggunakan manifes aplikasi web untuk mengontrol tipe tampilan dan orientasi laman.

Anda bisa membuat aplikasi web menyembunyikan UI browser dengan menyetel tipe `display` ke `standalone`:

* `name`
* `background_color`
* `icons`

Jika menurut Anda pengguna akan lebih suka menampilkan laman sebagai situs normal dalam browser, Anda bisa menyetel tipe `display` ke `browser`:

### Icons used for the splash screen {: #splash-screen-icons }

Anda bisa memberlakukan orientasi khusus, yang menguntungkan aplikasi yang hanya berfungsi dalam satu orientasi, seperti game, misalnya. Gunakan ini secara selektif. Pengguna lebih suka memilih orientasi.

Chrome memperkenalkan konsep warna tema untuk situs Anda pada tahun 2014. Warna tema merupakan petunjuk dari laman web yang memberi tahu browser tentang warna yang dipakai untuk mewarnai [elemen UI seperti bilah alamat](/web/fundamentals/design-and-ux/browser-customization/).

<div class="clearfix"></div>

## Tambahkan layar pembuka

Tanpa manifes, Anda harus menentukan warna tema pada setiap laman, dan jika Anda memiliki situs yang besar atau situs lawas, membuat perubahan ke seluruh situs bukan pekerjaan yang mudah.

<div class="clearfix"></div>

## Setel gaya peluncuran

<figure class="attempt-right">
  <img src="images/devtools-manifest.png" alt="warna latar belakang">
  <figcaption>Warna latar belakang untuk layar peluncuran</figcaption>
</figure>

Tambahkan atribut `theme_color` ke manifes Anda, dan ketika situs tersebut diluncurkan dari layar beranda setiap laman di domain, itu akan secara otomatis mendapatkan warna tema.

Jika Anda ingin secara manual memverifikasi apakah manifes aplikasi web telah disiapkan dengan benar, gunakan tab **Manifest** di panel **Application** pada Chrome DevTools.

If you want an automated approach towards validating your web app manifest, check out [Lighthouse](/web/tools/lighthouse/). Lighthouse is a web app auditing tool. It's built into the Audits tab of Chrome DevTools, or can be run as an NPM module. You provide Lighthouse with a URL, it runs a suite of audits against that page, and then displays the results in a report.

## Sediakan warna tema tingkat-situs

* Sebuah `short_name` untuk digunakan sebagai teks di layar beranda pengguna.
* Sebuah `name` untuk digunakan di spanduk Pemasangan Aplikasi Web.
* If you want feature descriptions from the engineers who created web app manifests, you can read the [W3C Web App Manifest Spec](http://www.w3.org/TR/appmanifest/).

## Uji manifes Anda {: #test }

Tab ini menyediakan versi properti manifes yang mudah kita baca. Lihat [Manifes aplikasi web](/web/tools/chrome-devtools/progressive-web-apps#manifest) pada dokumen Chrome DevTools untuk informasi selengkapnya mengenai tab ini. Anda juga bisa menyimulasikan kejadian Add to Homescreen dari sini. Lihat [Menguji spanduk pemasangan aplikasi](/web/fundamentals/app-install-banners#test) untuk informasi selengkapnya mengenai topik ini.