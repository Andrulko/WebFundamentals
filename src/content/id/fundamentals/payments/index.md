project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Payment Request API adalah untuk pembayaran yang cepat dan mudah di web.

{# wf_published_on: 2016-07-25 #} {# wf_updated_on: 2017-07-12 #}

# Payment Request API: Panduan Integrasi {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/zkoch.html" %}

<div class="video-wrapper-full-width">
  <iframe class="devsite-embedded-youtube-video" data-video-id="colCcgKoLUM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

## Memperkenalkan Payment Request API {: #introducing }

Dogfood: `PaymentRequest` masih dalam tahap development. Walaupun kami menganggapnya cukup stabil untuk diimplementasikan, mungkin saja terus berubah. Kami akan tetap memperbarui laman ini agar selalu merefleksikan status API saat ini ([perubahan M56](https://docs.google.com/document/d/1I8ha1ySrPWhx80EB4CVPmThkD4ILFM017AfOA5gEFg4/edit#)). Sementara itu, untuk melindungi Anda dari perubahan API yang mungkin tidak kompatibel ke belakang, kami menawarkan [sisipan](https://storage.googleapis.com/prshim/v1/payment-shim.js) yang bisa disematkan pada situs Anda. Sisipan ini akan menutupi perbedaan API untuk dua versi Chrome utama.

Membeli barang secara online memang praktis namun sering kali menjadi pengalaman yang mengecewakan, khususnya pada perangkat seluler. Walaupun lalu lintas seluler terus meningkat, akun konversi seluler hanya sekitar sepertiga dari semua pembelian yang diselesaikan. Dengan kata lain, pengguna meninggalkan pembelian lewat seluler dua kali lebih sering daripada pembelian lewat desktop. Mengapa?

**For consumers**, they simplify checkout flow, by making it a few taps instead of typing small characters many times on a virtual keyboard.

**For merchants**, they make it easier to implement with a variety of payment options already filtered for the customer.

Formulir pembelian online adalah intensif-pengguna, sulit digunakan, lambat dimuat dan disegarkan, serta perlu banyak langkah untuk menyelesaikannya. Ini karena kedua komponen utama pembayaran online&mdash;keamanan dan kenyamanan&mdash;sering kali berbenturan; mengutamakan yang satu berarti mengalahkan yang lain.

Kebanyakan masalah yang menyebabkan pengabaian bisa langsung dilacak ke formulir pembelian. Setiap aplikasi atau situs memiliki entri data dan proses validasinya sendiri, dan pengguna sering kali merasa mereka harus memasukkan informasi yang sama pada setiap titik pembelian di aplikasi. Juga, developer aplikasi berusaha membuat alur pembelian yang mendukung beberapa metode pembayaran berbeda sekaligus; bahkan perbedaan kecil dalam persyaratan metode pembayaran bisa memperumit penyelesaian formulir dan proses penyerahannya.

## Menggunakan Payment Request API {: #using }

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/standard-open.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Standard and Open</h3>
    Web Payments are an open payment standard for the web platform for the first time
    in history. They are available for any players to implement.</div>
</section>

<section style="display:flex;padding-bottom:32px;">
  <div style="min-width:50%">
    <h3>Easy and Consistent</h3>
    Web Payments make checkout easy for the user, by reusing stored 
payments and address information and removing the need for the user to fill in checkout forms. 
Since the UI is implemented by the browser natively, users see a familiar and consistent checkout 
experience on any website that makes use of the standard.</div>
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/easy-consistent.png" width="100%" alt="Standard and Open" title="">
  </div>
</section>

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/secure-flexible.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Secure and Flexible</h3>
    Web Payments provide industry-leading payment technology to the 
web, and can easily integrate a secure payment solution.</div>
</section>

## Mengumpulkan alamat pengiriman {: #shipping-address }

Sistem apa saja yang memperbaiki atau memecahkan satu atau beberapa masalah itu akan menjadi perubahan yang disambut baik. Kita sudah mulai memecahkan masalah dengan [Isiotomatis](/web/updates/2015/06/checkout-faster-with-autofill), namun kita ingin membicarakan tentang solusi yang lebih komprehensif.

Payment Request API adalah sistem yang dimaksudkan untuk *meniadakan formulir pemeriksaan*. Ini sangat memperbaiki alur kerja pengguna selama proses pembelian, sehingga memberikan pengalaman pengguna yang konsisten dan memungkinkan para penjual di web dengan mudah memanfaatkan metode pembayaran yang berbeda-beda. Payment Request API bukanlah metode pembayaran baru, tidak juga berintegrasi langsung dengan pemroses pembayaran; melainkan, layer proses yang bertujuan:

Payment Request API adalah standar terbuka dan lintas-browser yang menggantikan alur checkout tradisional yang memungkinkan penjual meminta dan menerima pembayaran dalam satu panggilan API. Payment Request API memungkinkan laman web bertukar informasi dengan agen-pengguna selagi pengguna memberikan masukan, sebelum menyetujui atau menolak permintaan pembayaran.

Yang terpenting, dengan browser berfungsi sebagai perantara, semua informasi yang diperlukan untuk checkout cepat bisa disimpan di browser, sehingga pengguna bisa tinggal mengonfirmasikan dan membayar, cukup dengan satu klik.

Menggunakan Payment Request API, proses transaksi berjalan semulus mungkin untuk pengguna dan penjual.

## Menambahkan opsi pengiriman {: #shipping-options}

{% include "web/_shared/helpful.html" %}