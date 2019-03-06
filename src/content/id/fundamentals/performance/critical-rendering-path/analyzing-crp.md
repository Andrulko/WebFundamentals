project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Pelajari cara mengidentifikasi dan mengatasi bottleneck kinerja jalur rendering penting.

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2014-03-31 #}

# Menganalisis Kinerja Jalur Rendering Penting {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Untuk mengidentifikasi dan mengatasi bottleneck kinerja jalur rendering penting diperlukan pengetahuan yang baik mengenai kesalahan umum. Mari kita ikuti tur praktik langsung dan memahami pola kinerja umum yang akan membantu mengoptimalkan laman Anda.

Pengoptimalan jalur rendering penting memungkinkan browser menggambar laman secepat mungkin: semakin cepat mengubah laman menjadi interaksi yang lebih tinggi, semakin banyak laman yang ditampilkan, dan [semakin baik konversi](https://www.google.com/think/multiscreen/success.html). Untuk meminimalkan lama pengunjung menampilkan layar kosong, kita perlu mengoptimalkan sumber daya yang akan dimuat dan urutannya.

Untuk membantu mengilutrasikan proses ini, mari kita mulai dengan kasus yang sesederhana mungkin dan secara bertahap membangun laman untuk menyertakan sumber daya tambahan, gaya, dan logika aplikasi. Dalam prosesnya, kita akan mengoptimalkan setiap kasus; kita juga akan melihat di mana saja kesalahan bisa terjadi.

Sejauh ini kita telah memfokuskan secara eksklusif pada apa yang terjadi di browser setelah sumber daya (file CSS, JS, atau HTML) tersedia untuk diproses. Kita telah mengabaikan waktu yang diperlukan untuk mengambil sumber daya, baik dari cache maupun dari jaringan. Kita akan menggunakan anggapan berikut ini:

* Perjalanan bolak-balik jaringan (latensi propagasi) ke server menghabiskan waktu 100 md.
* Waktu respons server adalah 100 md bagi dokumen HTML dan 10 md bagi semua file lainnya.

## Pengalaman hello world

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Cobalah](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

Kita akan mulai dengan markup HTML dasar dan gambar tunggal; tanpa CSS atau JavaScript. Mari kita buka timeline Network di Chrome DevTools dan periksa jenjang sumber daya yang dihasilkan:

<img src="images/waterfall-dom.png" alt="CRP" />

Note: Walaupun dokumen ini menggunakan DevTools untuk mengilustrasikan konsep CRP, DevTools saat ini tidak cocok untuk analisis CRP. Lihat [Bagaimana dengan DevTools?](measure-crp#devtools) untuk informasi selengkapnya.

Sebagaimana yang diharapkan, file HTML memerlukan waktu sekitar 200 md untuk diunduh. Perhatikan, bagian transparan pada garis biru menyatakan waktu tunggu browser di jaringan tanpa menerima byte respons sementara bagian yang padat menampilkan waktu untuk menyelesaikan pengunduhan setelah byte respons pertama diterima. Ukuran unduhan HTML kecil (<4 K), jadi kita hanya membutuhkan satu kali bolak-balik untuk mengambil file lengkap. Hasilnya, dokumen HTML memerlukan waktu sekitar 200 md untuk diambil, dengan setengah dari waktunya dihabiskan untuk menunggu di jaringan, dan separuh lagi untuk menunggu respo<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

Bila materi HTML tersedia, browser akan mem-parse byte, mengonversinya menjadi token, dan membangun pohon DOM. Perhatikan, DevTools dengan mudah melaporkan waktu untuk kejadian DOMContentLoaded di bawah (216 md), yang juga dinyatakan dengan garis vertikal biru. Selisih antara akhir pengunduhan HTML dan garis vertikal biru (DOMContentLoaded) adalah waktu yang dihabiskan browser untuk membangun pohon DOM&mdash;dalam hal ini, hanya beberapa milidetik.

Perhatikan, "awesome photo" kita tidak memblokir kejadian `domContentLoaded`. Ternyata, kita bisa membangun pohon render dan bahkan menggambar laman tanpa menunggu setiap aset pada laman: **tidak semua sumber daya wajib menghasilkan penggambaran pertama yang cepat**. Sebenarnya, bila membicarakan tentang jalur rendering penting, biasanya kita membicarakan markup HTML, CSS, dan JavaScript. Gambar tidak memblokir render awal laman&mdash;meskipun kita juga harus mencoba menggambar gambar sesegera mungkin.

Dengan demikian, kejadian `load` (disebut juga dengan `onload`), diblokir pada gambar: DevTools melaporkan kejadian `onload` pada 335 md. Ingatlah bahwa kejadian `onload` menandai titik ketika **semua sumber daya** yang diperlukan oleh laman telah diunduh dan diproses; ini adalah titik ketika spinner pemuatan bisa berhenti berputar dalam browser (garis vertikal merah dalam jenjang).

## Menambahkan JavaScript dan CSS ke dalam campuran

Laman "pengalaman Hello World" kita tampaknya sederhana namun banyak yang terjadi di balik itu semua. Pada praktiknya kita juga akan memerlukan lebih dari sekadar HTML: kemungkinannya adalah, kita akan memiliki stylesheet CSS dan satu atau beberapa skrip untuk menambahkan beberapa interaktivitas ke laman. Marilah kita tambahkan keduanya pada campuran dan melihat apa yang terjadi:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Cobalah](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_timing.html){: target="_blank" .external }

*Sebelum menambahkan JavaScript dan CSS:*

<img src="images/waterfall-dom.png" alt="DOM CRP" />

*Dengan JavaScript dan CSS:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

Penambahan file JavaScript dan CSS eksternal akan menambahkan dua permintaan ekstra ke jenjang kita, yang semuanya yang dikirim pada waktu yang hampir bersamaan oleh browser. Akan tetapi, **perhatikan sekarang ada banyak selisih waktu lebih kecil antara kejadian `domContentLoaded` dan `onload`.**

Apa yang terjadi?

* Tidak seperti contoh HTML biasa, kita juga perlu mengambil dan mem-parse file CSS untuk membangun CSSOM, dan kita membutuhkan DOM maupun CSSOM untuk membangun pohon render.
* Karena laman juga berisi file JavaScript pemblokir parser di laman, kejadian `domContentLoaded` akan diblokir hingga file CSS diunduh dan di-parse: karena JavaScript mungkin akan membuat kueri CSSOM, maka kita harus memblokir file CSS hingga selesai diunduh agar kita bisa mengeksekusi JavaScript.

**Bagaimana jika kita menggantikan skrip eksternal dengan skrip inline?** Meskipun skrip dibuat inline secara langsung ke dalam laman, browser tidak bisa mengeksekusinya sebelum CSSOM dibangun. Singkatnya, JavaScript yang disisipkan juga merupakan pemblokir parser.

Dengan demikian, meski memblokir CSS, apakah penyisipan skrip secara inline akan membuat render laman menjadi lebih cepat? Mari kita coba dan lihat apa yang terjadi.

*JavaScript Eksternal:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*JavaScript Disisipkan:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, dan JS yang disisipkan inline" />

Kita menghilangkan satu permintaan, namun waktu `onload` dan `domContentLoaded` secara efektif sama. Mengapa? Seperti yang kita ketahui, tidak masalah apakah JavaScript inline atau eksternal, karena begitu browser mencapai tag skrip, browser akan memblokir dan menunggu CSSOM dibangun. Lebih jauh, dalam contoh pertama, browser mengunduh CSS maupun JavaScript secara bersamaan dan selesai mengunduhnya pada waktu yang hampir bersamaan. Dalam hal ini, menyisipkan kode JavaScript secara inline tidak terlalu membantu. Namun ada sejumlah strategi yang bisa membuat laman kita dirender lebih cepat.

Pertama-tama, ingatlah bahwa semua skrip inline adalah pemblokir parser, namun untuk skrip eksternal kita dapat menambahkan kata kunci "async" untuk membuka kunci parser. Mari kita urungkan penyisipan dan mencoba yang itu:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

Alternatifnya, kita bisa membuat CSS dan JavaScript secara inline:

[Cobalah](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_inlined.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_inlined.html){: target="_blank" .external }

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" />

Seperti yang Anda dapat lihat, meski dengan satu laman sederhana, mengoptimalkan jalur rendering penting merupakan latihan yang tidak remeh: kita harus memahami ketergantungan grafik antar berbagai sumber daya berbeda, kita perlu mengidentifikasikan sumber daya mana yang "penting", dan kita harus memilih di antara berbagai strategi berbeda mengenai cara menyertakan sumber daya tersebut pada laman. Tidak ada satu solusi untuk masalah ini; setiap laman berbeda. Anda perlu mengikuti proses serupa untuk mencari tahu sendiri strategi yang optimal.

Dengan demikian, mari kita lihat apakah kita bisa mundur selangkah dan mengidentifikasi beberapa pola kinerja umum.

Laman paling sederhana mungkin hanya terdiri dari markup HTML: tanpa CSS, tanpa JavaScript, atau tipe sumber daya lainnya. Untuk merender laman ini, browser harus memulai permintaan, tunggu dokumen HTML tiba, lakukan parse, bangun DOM dan terakhir render pada layar.

## Pola kinerja

[Cobalah](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

<img src="images/analysis-dom.png" alt="Hello world CRP" />

Sekarang, marilah kita pertimbangkan laman yang sama namun dengan file CSS eksternal:

[Cobalah](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

Mari kita definisikan kosa kata yang akan digunakan untuk menjelaskan jalur rendering penting:

Sekarang marilah kita bandingkan dengan karakteristik jalur penting dari contoh HTML + CSS di atas:

* **Sumber Daya Penting:** Sumber daya yang bisa memblokir rendering awal laman.
* **Panjang Jalur Penting:** Jumlah bolak-balik, atau total waktu yang diperlukan untuk mengambil semua sumber daya penting.
* **Byte Penting:** Total jumlah byte yang diperlukan untuk mendapatkan render pertama laman, yaitu jumlah ukuran file yang ditransfer dari semua sumber daya penting. Contoh pertama kita dengan satu laman HTML yang berisi satu sumber daya penting (dokumen HTML); panjang jalur pentingnya juga sama dengan satu kali bolak-balik jaringan (dengan anggapan ukuran file kecil), dan total byte penting hanya seukuran transfer dokumen HTML itu sendiri.

Now let's compare that to the critical path characteristics of the HTML + CSS example above:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** sumber daya penting
* **2** atau lebih perjalanan bolak balik untuk panjang jalur penting minimum.
* **9** KB byte penting

Sekarang mari kita tambahkan file JavaScript ekstra ke dalam campuran.

[Cobalah](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Kita telah menambahkan `app.js`, yang keduanya merupakan aset JavaScript eksternal pada laman dan sumber daya pemblokiran parser (yang penting). Yang lebih buruk, untuk mengeksekusi file JavaScript kita harus memblokir dan menunggu CSSOM - ingatlah bahwa JavaScript bisa membuat kueri CSSOM dan karena itu browser akan dihentikan sementara hingga `style.css` diunduh dan CSSOM dibangun.

We added `app.js`, which is both an external JavaScript asset on the page and a parser blocking (that is, critical) resource. Worse, in order to execute the JavaScript file we have to block and wait for CSSOM; recall that JavaScript can query the CSSOM and hence the browser pauses until `style.css` is downloaded and CSSOM is constructed.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

Sekarang kita memiliki tiga sumber daya penting yang jumlahnya hingga 11 KB byte penting, namun panjang jalur penting kita masih dua kali bolak-balik karena kita bisa mentransfer CSS dan JavaScript secara bersamaan. **Dengan mengetahui karakteristik jalur rendering penting berarti kita bisa mengidentifikasi sumber daya penting, dan memahami cara browser menjadwalkan pengambilannya.** Mari kita lanjutkan dengan contoh.

* **3** sumber daya penting
* **2** atau lebih perjalanan bolak balik untuk panjang jalur penting minimum.
* **11** KB byte penting

Setelah chatting dengan para developer situs, kita menyadari bahwa JavaScript yang disertakan pada laman tidak perlu diblokir; kita memiliki beberapa analitik dan kode lain di dalamnya yang tidak perlu memblokir rendering laman kita. Dengan mengetahui hal itu, kita bisa menambahkan atribut "async" ke tag skrip untuk membuka blokir parser:

[Cobalah](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

Hasilnya, laman kita yang telah dioptimalkan kini kembali dengan dua sumber daya penting (HTML dan CSS), dengan panjang jalur penting minimum dua perjalanan bolak balik, dan total 9 KB byte penting.

* Skrip tidak lagi merupakan pemblokiran parser dan bukan bagian dari jalur rendering penting.
* Karena tidak ada skrip penting lainnya, CSS juga tidak perlu memblokir kejadian `domContentLoaded`.
* Semakin cepat kejadian `domContentLoaded` dipicu, semakin cepat pula logika aplikasi bisa mulai dieksekusi.

Terakhir, jika stylesheet CSS hanya perlu dicetak, seperti apa penampilannya?

[Cobalah](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

Because the style.css resource is only used for print, the browser doesn't need to block on it to render the page. Hence, as soon as DOM construction is complete, the browser has enough information to render the page. As a result, this page has only a single critical resource (the HTML document), and the minimum critical rendering path length is one roundtrip.

## Feedback {: #feedback }

{# wf_devsite_translation #}