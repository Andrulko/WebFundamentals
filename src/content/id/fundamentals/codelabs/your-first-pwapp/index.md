project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Di codelab ini, Anda membangun Progressive Web App, yang dimuat dengan cepat, bahkan pada jaringan yang tidak stabil, memiliki ikon di layar beranda, dan dimuat dengan pengalaman tingkat atas selayar penuh.

{# wf_updated_on: 2017-01-05 #} {# wf_published_on: 2016-01-01 #}

# Progressive Web App Anda yang Pertama {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

## Pengantar

[Progressive Web App](/web/progressive-web-apps) adalah pengalaman yang menggabungkan yang terbaik dari web dan yang terbaik dari aplikasi. Pengalaman ini bermanfaat untuk pengguna dari kunjungan pertamanya di tab browser, tanpa harus melakukan pemasangan. Pengguna secara progresif akan membangun hubungan dengan aplikasi, yang semakin lama semakin kuat. Aplikasi dimuat cepat, bahkan pada jaringan yang tidak stabil, mengirimkan pemberitahuan push yang relevan, memiliki ikon pada layar beranda, dan dimuat dengan pengalaman tingkat atas selayar penuh.

### Apa itu Progressive Web App?

Progressive Web App adalah:

* **Progresif** - Bekerja untuk setiap pengguna, apa pun pilihan browser mereka karena dibangun dengan peningkatan progresif sebagai konsep intinya.
* **Responsif** - Cocok dengan setiap faktor bentuk: perangkat desktop, seluler, tablet, atau apa saja yang muncul berikutnya.
* **Konektivitas independen** - Disempurnakan dengan service worker agar bisa bekerja offline atau pada jaringan berkualitas-rendah.
* **Seperti-Aplikasi** - Terasa seperti sebuah aplikasi untuk pengguna dengan interaksi dan navigasi bergaya-aplikasi karena mereka dibangun di atas model shell aplikasi.
* **Segar** - Selalu terkini berkat proses pembaruan service worker.
* **Aman** - Disediakan melalui HTTPS untuk mencegah snooping dan memastikan materi belum dirusak.
* **Dapat ditemukan** - Dapat diidentifikasi sebagai "aplikasi" berkat manifes W3C dan cakupan registrasi service worker, yang memungkinkan mesin telusur untuk menemukannya.
* **Bisa dilibatkan-kembali** - Kemudahan untuk dilibatkan-kembali dengan fitur seperti pemberitahuan push.
* **Dapat dipasang** - Memungkinkan pengguna untuk "menyimpan" aplikasi yang mereka anggap paling berguna di layar beranda tanpa kerumitan toko aplikasi.
* **Bisa ditautkan** - Dapat dengan mudah dibagikan melalui URL, tidak memerlukan pemasangan yang rumit.

Codelab ini akan memandu Anda untuk membuat Progressive Web App, termasuk pertimbangan desain, serta detail implementasi untuk memastikan bahwa aplikasi Anda memenuhi prinsip kunci dari Progressive Web App.<aside class="key-point">

<p>Looking for more? Check out the talks from the  <a href="https://www.youtube.com/playlist?list=PLNYkxOF6rcIAWWNR_Q6eLPhsyx6VvYjVb">2016 Progressive Web App Summit</a>.</p>

</aside> 

### Apa yang akan kita bangun?

<table>
  <p>
    <tr>
      <td colspan="1" rowspan="1">
        </p>

<p>In this codelab, you're going to build a Weather web app using Progressive Web App techniques. Your app will:</p>

        
        <ul>
          
<li>Utilize and demonstrate the above principles of Progressive Web Apps.</li>
<li>Use live weather data.</li>
          
          <li>
            
<p>Provide app-like interactions to allow the user to add cities.</p>
</td><td colspan="1" rowspan="1">
              </li> </ul>

<p><img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png"></p>

              
              <p>
                </td> </tr>
              </p></table> 
              
              <h3>
                Apa yang akan Anda pelajari
              </h3>
              
              <ul>
                <li>
                  <strong>Progresif</strong> - kita akan menggunakan peningkatan progresif ke seluruh proses.
                </li>
                <li>
                  <strong>Responsif</strong> - kita akan memastikan itu cocok dengan setiap faktor bentuk.
                </li>
                <li>
                  <strong>Konektivitas</strong> independen - kita akan meng-cache shell aplikasi dengan service worker.
                </li>
              </ul>
              
              <h3>
                Apa yang Anda butuhkan
              </h3>
              
              <ul>
                <li>
                  Bagaimana merancang dan membangun sebuah aplikasi menggunakan metode "shell aplikasi"
                </li>
                <li>
                  Cara membuat aplikasi Anda bekerja offline
                </li>
                <li>
                  <a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip">The sample code</a>
                </li>
                <li>
                  A text editor
                </li>
                <li>
                  Basic knowledge of HTML, CSS, JavaScript, and <a href="https://developer.chrome.com/devtools">Chrome DevTools</a>
                </li>
              </ul>
              
              <p>
                Dalam codelab ini, Anda akan membangun aplikasi web Weather menggunakan teknik Progressive Web App. Mari kita mengingat sifat dari Progressive Web App:
              </p>
              
              <h2>
                Persiapan
              </h2>
              
              <h3>
                Mengunduh Kode
              </h3>
              
              <p>
                Codelab ini berfokus pada Progressive Web App. Konsep dan blok kode yang tidak-relevan akan dipoles dan disediakan sehingga Anda cukup salin dan tempel.
              </p>
              
              <p>
                <a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip">Download source code</a>
              </p>
              
              <p>
                Unpack the downloaded zip file. This will unpack a root folder (<code>your-first-pwapp-master</code>), which contains one folder for each step of this codelab, along with all of the resources you will need.
              </p>
              
              <p>
                Mengekstrak file zip yang diunduh. Ini akan mengekstrak folder root (<code>your-first-pwapp-master</code>), yang berisi satu folder untuk setiap langkah codelab, bersama dengan semua sumber daya yang Anda butuhkan.
              </p>
              
              <h3>
                Memasang dan memverifikasi server web
              </h3>
              
              <p>
                Folder <code>step-NN</code> berisi status akhir yang diinginkan dari setiap langkah codelab ini. Folder tersebut digunakan sebagai referensi. Kita akan melakukan semua pekerjaan pengkodean di direktori yang disebut <code>work</code>.
              </p>
              
              <p>
                <a href="https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb">Install Web Server for Chrome</a>
              </p>
              
              <p>
                After installing the Web Server for Chrome app, click on the Apps shortcut on the bookmarks bar:
              </p>
              
              <p>
                <img src="img/9efdf0d1258b78e4.png" alt="9efdf0d1258b78e4.png" />
              </p><aside class="key-point">

<p>More help:  <a href="https://support.google.com/chrome_webstore/answer/3060053">Add and open Chrome apps</a></p>

</aside> 
              
              <p>
                In the ensuing window, click on the Web Server icon:
              </p>
              
              <p>
                <img src="img/dc07bbc9fcfe7c5b.png" alt="dc07bbc9fcfe7c5b.png" />
              </p>
              
              <p>
                You'll see this dialog next, which allows you to configure your local web server:
              </p>
              
              <p>
                <img src="img/433870360ad308d4.png" alt="433870360ad308d4.png" />
              </p>
              
              <p>
                Click the <strong>choose folder</strong> button, and select the <code>work</code> folder. This will enable you to serve your work in progress via the URL highlighted in the web server dialog (in the <strong>Web Server URL(s)</strong> section).
              </p>
              
              <p>
                Klik tombol <strong>choose folder</strong>, dan pilih folder <code>work</code>. Ini memungkinkan Anda untuk menyajikan pekerjaan yang sedang berlangsung melalui URL yang disorot dalam dialog server web (di bagian <strong>Web Server URL(s)</strong>).
              </p>
              
              <p>
                <img src="img/39b4e0371e9703e6.png" alt="39b4e0371e9703e6.png" />
              </p>
              
              <p>
                Then stop and restart the server by sliding the toggle labeled "Web Server: STARTED" to the left and then back to the right.
              </p>
              
              <p>
                <img src="img/daefd30e8a290df5.png" alt="daefd30e8a290df5.png" />
              </p>
              
              <p>
                Now visit your work site in your web browser (by clicking on the highlighted Web Server URL) and you should see a page that looks like this:
              </p>
              
              <p>
                <img src="img/aa64e93e8151b642.png" alt="aa64e93e8151b642.png" />
              </p>
              
              <p>
                This app is not yet doing anything interesting - so far, it's just a minimal skeleton with a spinner we're using to verify your web server functionality. We'll add functionality and UI features in subsequent steps.
              </p><aside class="key-point">

<p>From this point forward, all testing/verification (e.g. the<strong> Test It Out</strong> sections in subsequent steps) should be performed using this web server setup.</p>

</aside> 
              
              <h2>
                Arsitektur Shell Aplikasi Anda
              </h2>
              
              <h3>
                Apa yang dimaksud dengan shell aplikasi?
              </h3>
              
              <p>
                Jelas, aplikasi ini belum melakukan sesuatu yang menarik - sejauh ini, hanya kerangka minimal dengan spinner yang kami gunakan untuk memverifikasi fungsionalitas server web Anda. Kami akan menambahkan fungsionalitas dan fitur UI dalam langkah-langkah berikutnya.
              </p>
              
              <p>
                Shell aplikasi adalah HTML, CSS, dan JavaScript minimum yang diperlukan untuk menenagai antarmuka pengguna progressive web app dan merupakan salah satu komponen yang memastikan kinerja yang baik dan bisa diandalkan. Pemuatan pertama harus sangat cepat dan langsung di-cache. "Di-cache" berarti bahwa file shell dimuat setelah melalui jaringan dan kemudian disimpan ke perangkat lokal. Setiap kali pengguna membuka aplikasi, file shell dimuat dari cache perangkat lokal, yang menghasilkan waktu startup super cepat.
              </p>
              
              <p>
                Arsitektur shell aplikasi memisahkan infrastruktur aplikasi inti dan UI dari data. Semua UI dan infrastruktur di-cache secara lokal menggunakan service worker sehingga pada pemuatan berikutnya, Progressive Web App hanya perlu mengambil data yang dibutuhkan, alih-alih memuat semuanya.
              </p>
              
              <p>
                <img src="img/156b5e3cc8373d55.png" alt="156b5e3cc8373d55.png" />
              </p>
              
              <p>
                Dengan kata lain, shell aplikasi serupa dengan bundel kode yang akan Anda publikasikan ke toko aplikasi ketika membuat aplikasi asli. Ini adalah komponen inti yang diperlukan untuk membangun aplikasi Anda dari dasar, namun kemungkinan tidak berisi data.
              </p>
              
              <h3>
                Mengapa menggunakan arsitektur Shell Aplikasi?
              </h3>
              
              <p>
                Menggunakan arsitektur shell aplikasi memungkinkan Anda untuk fokus pada kecepatan, memberikan Progressive Web App properti yang mirip dengan aplikasi asli: pemuatan langsung dan pembaruan rutin, semua tanpa membutuhkan sebuah toko aplikasi.
              </p>
              
              <h3>
                Mendesain Shell Aplikasi
              </h3>
              
              <p>
                Langkah pertama adalah memecahkan desain hingga ke dalam komponen inti.
              </p>
              
              <p>
                Tanyakan pada diri Anda:
              </p>
              
              <ul>
                <li>
                  Chrome 52 atau di atasnya
                </li>
                <li>
                  <a href="https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb">Web Server for Chrome</a>, atau server web pilihan Anda
                </li>
                <li>
                  What supporting resources are needed for the app shell? For example images, JavaScript, styles, etc.
                </li>
              </ul>
              
              <p>
                Kita akan membuat aplikasi Weather sebagai Progressive Web App yang pertama. Komponen utamanya terdiri dari:
              </p>
              
              <table>
                <p>
                  <tr>
                    <td colspan="1" rowspan="1">
                      </p> 
                      
                      <ul>
                        
<li>Header with a title, and add/refresh buttons</li>
<li>Container for forecast cards</li>
<li>A forecast card template</li>
<li>A dialog box for adding new cities</li>
                        
                        <li>
                          
<p>A loading indicator</p>
</td><td colspan="1" rowspan="1">
                            </li> </ul>

<p><img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png"></p>

                            
                            <p>
                              </td> </tr>
                            </p></table> 
                            
                            <p>
                              Ketika mendesain aplikasi yang lebih kompleks, materi yang tidak diperlukan untuk pemuatan awal dapat diminta belakangan dan kemudian di-cache untuk penggunaan mendatang. Misalnya, kita bisa menunda pemuatan kotak dialog New City sampai kita selesai merender pengalaman pertama menjalankan dan tersedia beberapa siklus yang diam.
                            </p>
                            
                            <h2>
                              Mengimplementasikan Shell Aplikasi Anda
                            </h2>
                            
                            <p>
                              Ada beberapa cara untuk memulai proyek, dan kami biasanya merekomendasikan untuk menggunakan Web Starter Kit. Namun, dalam kasus ini, agar proyek kita tetap sesederhana mungkin dan berkonsentrasi pada Progressive Web App, kami telah menyediakan semua sumber daya yang Anda butuhkan.
                            </p>
                            
                            <h3>
                              Membuat HTML untuk Shell Aplikasi
                            </h3>
                            
                            <p>
                              Sekarang kita akan menambahkan komponen inti yang kita bahas dalam <a href="/web/fundamentals/getting-started/your-first-progressive-web-app/step-01">Arsitektur Shell Aplikasi</a>.
                            </p>
                            
                            <p>
                              Ingat, komponen utama terdiri dari:
                            </p>
                            
                            <ul>
                              <li>
                                Apa yang harus segera ditampilkan di layar?
                              </li>
                              <li>
                                Apa komponen UI lain yang merupakan kunci untuk aplikasi?
                              </li>
                              <li>
                                Apa sumber daya pendukung yang dibutuhkan oleh shell aplikasi? Misalnya gambar, JavaScript, gaya, dll.
                              </li>
                              <li>
                                A dialog for adding new cities
                              </li>
                              <li>
                                A loading indicator
                              </li>
                            </ul>
                            
                            <p>
                              File <code>index.html</code> yang sudah ada dalam direktori <code>work</code> harus terlihat seperti ini (ini adalah bagian dari materi sebenarnya, jangan menyalin kode ini ke dalam file Anda):
                            </p>
                            
                            <pre><code>&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;meta charset="utf-8"&gt;
  &lt;meta http-equiv="X-UA-Compatible" content="IE=edge"&gt;
  &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
  &lt;title&gt;Weather PWA&lt;/title&gt;
  &lt;link rel="stylesheet" type="text/css" href="styles/inline.css"&gt;
&lt;/head&gt;
&lt;body&gt;
  &lt;header class="header"&gt;
    &lt;h1 class="header__title"&gt;Weather PWA&lt;/h1&gt;
    &lt;button id="butRefresh" class="headerButton"&gt;&lt;/button&gt;
    &lt;button id="butAdd" class="headerButton"&gt;&lt;/button&gt;
  &lt;/header&gt;

  &lt;main class="main"&gt;
    &lt;div class="card cardTemplate weather-forecast" hidden&gt;
    . . .
    &lt;/div&gt;
  &lt;/main&gt;

  &lt;div class="dialog-container"&gt;
  . . .
  &lt;/div&gt;

  &lt;div class="loader"&gt;
    &lt;svg viewBox="0 0 32 32" width="32" height="32"&gt;
      &lt;circle id="spinner" cx="16" cy="16" r="14" fill="none"&gt;&lt;/circle&gt;
    &lt;/svg&gt;
  &lt;/div&gt;

  &lt;!-- Insert link to app.js here --&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>
                            
                            <p>
                              Perhatikan bahwa loader terlihat secara default. Ini memastikan bahwa pengguna melihat pemuat begitu laman dimuat, memberi mereka tanda yang jelas bahwa materi sedang memuat.
                            </p>
                            
                            <p>
                              Untuk menghemat waktu, kami juga sudah membuat stylesheet yang bisa Anda gunakan.
                            </p><aside class="key-point">

<p>We've given you the markup and styles to save you some time and make sure you're starting on a solid foundation. In the next section, you'll have an opportunity to write your own code.</p>

</aside> 
                            
                            <h3>
                              Memeriksa kunci kode aplikasi JavaScript
                            </h3>
                            
                            <p>
                              Sekarang kita memiliki sebagian besar UI yang siap, tiba saatnya menghubungkan kode untuk membuatnya berfungsi. Seperti seluruh shell aplikasi, kenali kodeapa yang diperlukan sebagai bagian dari pengalaman kunci dan apa yang bisa dimuat belakangan.
                            </p>
                            
                            <p>
                              Direktori pekerjaan Anda juga sudah memuat kode aplikasi (<code>scripts/app.js</code>), di dalamnya Anda akan menemukan:
                            </p>
                            
                            <ul>
                              <li>
                                Header dengan judul, dan tombol add/refresh
                              </li>
                              <li>
                                Kontainer untuk kartu prakiraan cuaca
                              </li>
                              <li>
                                Template kartu prakiraan
                              </li>
                              <li>
                                Kotak dialog untuk menambahkan kota baru
                              </li>
                              <li>
                                Indikator pemuatan
                              </li>
                              <li>
                                Some fake data (<code>initialWeatherForecast</code>) you can use to quickly test how things render.
                              </li>
                            </ul>
                            
                            <h3>
                              Lakukan pengujian
                            </h3>
                            
                            <p>
                              Karena sekarang Anda telah mendapat HTML, gaya dan JavaScript inti, saatnya untuk menguji aplikasi.
                            </p>
                            
                            <p>
                              Untuk melihat bagaimana data cuaca palsu dirender, hilangkan tanda komentar pada baris berikut di bagian bawah file <code>index.html</code> Anda:
                            </p>
                            
                            <pre><code>&lt;!--&lt;script src="scripts/app.js" async&gt;&lt;/script&gt;--&gt;
</code></pre>
                            
                            <p>
                              Berikutnya, hilangkan tanda komentar pada baris berikut di bagian bawah file <code>app.js</code> Anda:
                            </p>
                            
                            <pre><code>// app.updateForecastCard(initialWeatherForecast);
</code></pre>
                            
                            <p>
                              Muat ulang aplikasi Anda. Hasilnya akan menjadi kartu prakiraan yang terformat dengan baik (meskipun palsu, Anda bisa tahu dari tanggalnya) dengan spinner dinonaktifkan, seperti ini:
                            </p>
                            
                            <p>
                              <img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-04/">Tautan</a>
                            </p>
                            
                            <p>
                              Setelah Anda mencobanya dan memverifikasi bahwa itu bekerja sesuai harapan, Anda bisa membuang panggilan ke <code>app.updateForecastCard</code> dengan data palsu lagi. Kita hanya memerlukannya untuk memastikan bahwa semuanya bekerja sesuai harapan.
                            </p>
                            
                            <h2>
                              Memulai dengan pemuatan pertama yang cepat
                            </h2>
                            
                            <p>
                              Progressive Web App harus mulai dengan cepat dan bisa langsung dipakai. Dengan kondisi saat ini, Aplikasi Weather kita dimulai dengan cepat, tapi tidak dapat digunakan. Tidak ada data. Kita bisa membuat permintaan AJAX untuk mendapatkan data tersebut, tapi itu akan mengakibatkan permintaan tambahan dan membuat muat awal lebih lama. Sebaiknya, berikan data real di pemuatan pertama.
                            </p>
                            
                            <h3>
                              Memasukkan data prakiraan cuaca
                            </h3>
                            
                            <p>
                              Untuk code lab ini, kami menyimulasikan server memasukkan prakiraan cuaca langsung ke dalam JavaScript, namun dalam aplikasi produksi, data prakiraan cuaca terbaru akan dimasukkan oleh server berdasarkan geo-lokasi alamat IP pengguna.
                            </p>
                            
                            <p>
                              Kode sudah berisi data yang akan kita masukkan. Ini adalah <code>initialWeatherForecast</code> yang kita gunakan pada langkah sebelumnya.
                            </p>
                            
                            <h3>
                              Membedakan pertama kali dijalankan
                            </h3>
                            
                            <p>
                              Namun, bagaimana kita tahu kapan menampilkan informasi ini, yang mungkin tidak relevan pada pemuatan mendatang ketika aplikasi cuaca ditarik dari cache? Ketika pengguna memuat aplikasi pada kunjungan berikutnya, mereka mungkin telah berpindah kota, jadi kita harus memuat informasi untuk kota tersebut, tidak selalu kota pertama yang pernah mereka singgahi.
                            </p>
                            
                            <p>
                              Preferensi pengguna, seperti daftar kota langganan pengguna, harus disimpan secara lokal menggunakan IndexedDB atau mekanisme penyimpanan cepat lainnya. Untuk menyederhanakan code lab ini semaksimal mungkin, kami menggunakan <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage">localStorage</a>, yang tidak ideal untuk aplikasi produksi karena ini adalah mekanisme penyimpanan sinkron yang memblokir sehingga berpotensi sangat lambat pada beberapa perangkat.
                            </p><aside class="key-point">

<p><strong>Extra Credit</strong>: Replace <code>localStorage</code> implementation with  <a href="https://www.npmjs.com/package/idb">idb</a>, check out  <a href="https://github.com/localForage/localForage">localForage</a> as a simple wrapper to idb.</p>

</aside> 
                            
                            <p>
                              Pertama, mari kita tambahkan kode yang diperlukan untuk menyimpan preferensi pengguna. Temukan komentar TODO berikut dalam kode Anda.
                            </p>
                            
                            <pre><code>  // TODO add saveSelectedCities function here
</code></pre>
                            
                            <p>
                              Dan tambahkan kode berikut di bawah komentar.
                            </p>
                            
                            <pre><code>  // Save list of cities to localStorage.
  app.saveSelectedCities = function() {
    var selectedCities = JSON.stringify(app.selectedCities);
    localStorage.selectedCities = selectedCities;
  };
</code></pre>
                            
                            <p>
                              Berikutnya, mari kita tambahkan kode startup untuk memeriksa apakah pengguna memiliki kota yang disimpan dan merender kota tersebut, atau menggunakan data yang dimasukkan. Temukan komentar berikut:
                            </p>
                            
                            <pre><code>  // TODO add startup code here
</code></pre>
                            
                            <p>
                              Dan tambahkan kode berikut di bawah komentar ini.
                            </p>
                            
                            <pre><code>/************************************************************************
   *

   * Code required to start the app
   *
   * NOTE: To simplify this codelab, we've used localStorage.
   *   localStorage is a synchronous API and has serious performance
   *   implications. It should not be used in production applications!
   *   Instead, check out IDB (https://www.npmjs.com/package/idb) or
   *   SimpleDB (https://gist.github.com/inexorabletash/c8069c042b734519680c)
   ************************************************************************/

  app.selectedCities = localStorage.selectedCities;
  if (app.selectedCities) {
    app.selectedCities = JSON.parse(app.selectedCities);
    app.selectedCities.forEach(function(city) {
      app.getForecast(city.key, city.label);
    });
  } else {
    /* The user is using the app for the first time, or the user has not
     * saved any cities, so show the user some fake data. A real app in this
     * scenario could guess the user's location via IP lookup and then inject
     * that data into the page.
     */
    app.updateForecastCard(initialWeatherForecast);
    app.selectedCities = [
      {key: initialWeatherForecast.key, label: initialWeatherForecast.label}
    ];
    app.saveSelectedCities();
  }
</code></pre>
                            
                            <p>
                              Kode startup memeriksa bila ada kota yang disimpan dalam penyimpanan lokal. Jika ada, itu akan mem-parse data penyimpanan lokal dan kemudian menampilkan kartu prakiraan untuk masing-masing kota yang disimpan. Jika tidak, kode startup akan menggunakan data prakiraan palsu dan menyimpannya sebagai kota default.
                            </p>
                            
                            <h3>
                              Menyimpan kota yang dipilih
                            </h3>
                            
                            <p>
                              Yang terakhir, Anda harus mengubah penangan tombol "add city" untuk menyimpan kota yang dipilih ke penyimpanan lokal.
                            </p>
                            
                            <p>
                              Perbarui penangan klik <code>butAddCity</code> Anda sehingga sesuai dengan kode berikut:
                            </p>
                            
                            <pre><code>document.getElementById('butAddCity').addEventListener('click', function() {
    // Add the newly selected city
    var select = document.getElementById('selectCityToAdd');
    var selected = select.options[select.selectedIndex];
    var key = selected.value;
    var label = selected.textContent;
    if (!app.selectedCities) {
      app.selectedCities = [];
    }
    app.getForecast(key, label);
    app.selectedCities.push({key: key, label: label});
    app.saveSelectedCities();
    app.toggleAddDialog(false);
  });
</code></pre>
                            
                            <p>
                              Tambahan yang baru adalah inisialisasi <code>app.selectedCities</code> jika tidak terdapat hal tersebut, serta panggilan ke <code>app.selectedCities.push()</code> dan <code>app.saveSelectedCities()</code>.
                            </p>
                            
                            <h3>
                              Lakukan pengujian
                            </h3>
                            
                            <ul>
                              <li>
                                Header dengan judul, dan tombol add/refresh
                              </li>
                              <li>
                                Kontainer untuk kartu prakiraan cuaca
                              </li>
                              <li>
                                Template kartu prakiraan
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-05/">Tautan</a>
                            </p>
                            
                            <h2>
                              Menggunakan service worker untuk melakukan precache Shell Aplikasi
                            </h2>
                            
                            <p>
                              Progressive Web App harus cepat, dan dapat dipasang, yang berarti bahwa mereka tetap berfungsi saat online, offline, dan pada koneksi yang lambat serta tidak stabil. Untuk mencapai ini, kita harus meng-cache shell aplikasi menggunakan service worker, sehingga selalu tersedia dengan cepat dan bisa diandalkan.
                            </p>
                            
                            <p>
                              Jika Anda belum familier dengan service worker, Anda bisa mendapatkan pemahaman dasar dengan membaca <a href="/web/fundamentals/primers/service-worker/">Pengantar Service Workers</a> tentang apa yang bisa mereka lakukan, bagaimana daur hidupnya dan lainnya. Setelah menyelesaikan code lab ini, pastikan untuk memeriksa <a href="https://goo.gl/jhXCBy">Men-debug Service Worker code lab</a> untuk menilik secara lebih dalam tentang cara bekerja dengan service worker.
                            </p>
                            
                            <p>
                              Fitur yang disediakan melalui service worker harus dianggap sebagai peningkatan progresif, dan hanya ditambahkan jika didukung oleh browser. Misalnya, dengan service worker Anda bisa meng-cache shell aplikasi dan data untuk aplikasi, sehingga mereka tersedia bahkan ketika tidak ada jaringan. Ketika service worker tidak didukung, kode offline tidak dipanggil, dan pengguna mendapatkan pengalaman dasar. Menggunakan deteksi fitur untuk memberikan peningkatan progresif membutuhkan overhead dan tidak akan masuk dalam browser lama yang tidak mendukung fitur tersebut.
                            </p><aside class="key-point">

<p><strong>Remember</strong>: Service worker functionality is only available on pages that are accessed via HTTPS (<a href="http://localhost">http://localhost</a> and equivalents will also work, to facilitate testing). To learn about the rationale behind this restriction check out  <a href="http://www.chromium.org/Home/chromium-security/prefer-secure-origins-for-powerful-new-features">Prefer Secure Origins For Powerful New Features</a> from the Chromium team.</p>

</aside> 
                            
                            <h3>
                              Mendaftarkan service worker jika tersedia
                            </h3>
                            
                            <p>
                              Langkah pertama agar aplikasi bisa bekerja secara offline adalah dengan mendaftarkan service worker, skrip yang memungkinkan fungsionalitas latar belakang tanpa membutuhkan laman web terbuka atau interaksi pengguna.
                            </p>
                            
                            <p>
                              Ini membutuhkan dua langkah sederhana:
                            </p>
                            
                            <ol start="1">
                              <li>
                                Perintahkan browser untuk mendaftarkan file JavaScript sebagai service worker.
                              </li>
                              
                              <li>
                                Buat file JavaScript yang memuat service worker.
                              </li>
                            </ol>
                            
                            <p>
                              Pertama, kita harus memeriksa apakah browser mendukung service worker, dan jika mendukung, daftarkan service worker. Tambahkan kode berikut ke <code>app.js</code> (setelah komentar <code>// TODO add service worker code here</code>):
                            </p>
                            
                            <pre><code>  if ('serviceWorker' in navigator) {
    navigator.serviceWorker
             .register('./service-worker.js')
             .then(function() { console.log('Service Worker Registered'); });
  }
</code></pre>
                            
                            <h3>
                              Meng-cache aset situs
                            </h3>
                            
                            <p>
                              Bila service worker telah terdaftar, kejadian pasang dipicu saat pengguna pertama kali mengunjungi laman. Dalam penangan kejadian ini, kita akan meng-cache semua aset yang dibutuhkan untuk aplikasi.
                            </p><aside class="warning">

<p>The code below must NOT be used in production, it covers only the most basic use cases and it's easy to get yourself into a state where your app shell will never update. Be sure to review the section below that discusses the pitfalls of this implementation and how to avoid them.</p>

</aside> 
                            
                            <p>
                              Ketika diaktifkan, service worker akan membuka objek <a href="https://developer.mozilla.org/en-US/docs/Web/API/Cache">cache</a> dan mengisinya dengan aset yang diperlukan untuk memuat Shell Aplikasi. Buatlah file bernama <code>service-worker.js</code> di folder root aplikasi Anda (yang seharusnya direktori <code>your-first-pwapp-master/work</code>). File harus tinggal di root aplikasi karena cakupan untuk service worker didefinisikan oleh direktori tempat file berada. Tambahkan kode berikut ke file <code>service-worker.js</code> yang baru:
                            </p>
                            
                            <pre><code>var cacheName = 'weatherPWA-step-6-1';
var filesToCache = [];

self.addEventListener('install', function(e) {
  console.log('[ServiceWorker] Install');
  e.waitUntil(
    caches.open(cacheName).then(function(cache) {
      console.log('[ServiceWorker] Caching app shell');
      return cache.addAll(filesToCache);
    })
  );
});
</code></pre>
                            
                            <p>
                              Pertama, kita harus membuka cache dengan <code>caches.open()</code> dan memberikan nama cache. Memberikan nama cache memungkinkan kita untuk memberikan file nama versi, atau data terpisah dari shell aplikasi sehingga kita bisa dengan mudah memperbarui suatu item tanpa memengaruhi yang lainnya.
                            </p>
                            
                            <p>
                              Setelah cache terbuka, kita kemudian bisa memanggil <code>cache.addAll()</code>, yang membawa daftar URL, kemudian mengambilnya dari server dan menambahkan respons ke cache. Sayangnya, <code>cache.addAll()</code> bersifat atomis, jika salah satu file gagal, seluruh langkah cache akan gagal!
                            </p>
                            
                            <p>
                              Baiklah, mari kita mulai mengakrabkan diri dengan bagaimana Anda bisa menggunakan DevTools untuk memahami dan men-debug service worker. Sebelum memuat ulang laman Anda, buka DevTools, masuk ke panel __Service Worker __pada panel __Application __. Terlihat seperti ini.
                            </p>
                            
                            <p>
                              <img src="img/ed4633f91ec1389f.png" alt="ed4633f91ec1389f.png" />
                            </p>
                            
                            <p>
                              Ketika Anda melihat laman kosong seperti ini, berarti laman yang sedang terbuka tidak memiliki service worker yang terdaftar.
                            </p>
                            
                            <p>
                              Sekarang, muat ulang laman Anda. Panel Service Worker sekarang terlihat seperti ini.
                            </p>
                            
                            <p>
                              <img src="img/bf15c2f18d7f945c.png" alt="bf15c2f18d7f945c.png" />
                            </p>
                            
                            <p>
                              Ketika Anda melihat informasi seperti ini, berarti laman memiliki service worker aktif.
                            </p>
                            
                            <p>
                              Oke, sekarang kita akan melakukan penjelajahan singkat dan menunjukkan kejutan yang mungkin Anda hadapi ketika mengembangkan service worker. Untuk menunjukkannya, mari kita tambahkan event listener <code>activate</code> di bawah event listener <code>install</code> dalam file <code>service-worker.js</code> Anda.
                            </p>
                            
                            <pre><code>self.addEventListener('activate', function(e) {
  console.log('[ServiceWorker] Activate');
});
</code></pre>
                            
                            <p>
                              Kejadian <code>activate</code> diaktifkan saat service worker dijalankan.
                            </p>
                            
                            <p>
                              Buka DevTools Console dan muat ulang laman, pindah ke panel Service Worker di panel Application dan klik inspect pada service worker yang diaktifkan. Anda mengira pesan <code>[ServiceWorker] Activate</code> dicatat ke konsol, namun itu tidak terjadi. Periksa panel Service Worker dan Anda bisa melihat bahwa service worker yang baru (termasuk event listener aktif) tampaknya berada dalam status "menunggu".
                            </p>
                            
                            <p>
                              <img src="img/1f454b6807700695.png" alt="1f454b6807700695.png" />
                            </p>
                            
                            <p>
                              Pada dasarnya, service worker lama terus mengontrol laman selama ada tab yang terbuka pada laman. Jadi, Anda <em>bisa</em> menutup dan membuka kembali laman atau menekan tombol __skipWaiting __, namun solusi jangka panjangnya adalah dengan mengaktifkan kotak centang __Update on Reload __pada panel Service Worker DevTools. Ketika kotak centang ini diaktifkan, service worker dengan paksa diperbarui setiap kali laman dimuat ulang.
                            </p>
                            
                            <p>
                              Aktifkan kotak centang __update on reload __ sekarang dan muat ulang laman tersebut untuk memastikan bahwa service worker baru telah diaktifkan.
                            </p>
                            
                            <p>
                              <strong>Catatan:</strong> Anda mungkin melihat pesan kesalahan dalam panel Service Worker dari panel Application mirip dengan yang terlihat di bawah, tetap <strong>aman</strong> mengabaikan pesan kesalahan ini.
                            </p>
                            
                            <p>
                              <img src="img/b1728ef310c444f5.png" alt="b1728ef310c444f5.png" />
                            </p>
                            
                            <p>
                              Itu semua untuk saat ini mengenai memeriksa dan men-debug service worker di DevTools. Kami akan menunjukkan kepada Anda beberapa trik lagi nanti. Mari kita kembali membangun aplikasi Anda.
                            </p>
                            
                            <p>
                              Mari kita meluaskan event listener <code>activate</code> agar menyertakan beberapa logika untuk memperbarui cache. Perbarui kode Anda agar cocok dengan kode di bawah ini.
                            </p>
                            
                            <pre><code>self.addEventListener('activate', function(e) {
  console.log('[ServiceWorker] Activate');
  e.waitUntil(
    caches.keys().then(function(keyList) {
      return Promise.all(keyList.map(function(key) {
        if (key !== cacheName) {
          console.log('[ServiceWorker] Removing old cache', key);
          return caches.delete(key);
        }
      }));
    })
  );
  return self.clients.claim();
});
</code></pre>
                            
                            <p>
                              Kode ini memastikan bahwa service worker Anda memperbarui cache-nya setiap kali file shell aplikasi berubah. Agar bisa berfungsi, Anda harus menaikkan variabel <code>cacheName</code> di atas file service worker Anda.
                            </p>
                            
                            <p>
                              Pernyataan terakhir memperbaiki kasus abnormal yang bisa Anda baca di informasi (opsional) kotak di bawah ini.
                            </p><aside class="key-point">

<p>When the app is complete, <code>self.clients.claim()</code> fixes a corner case in which the app wasn't returning the latest data. You can reproduce the corner case by commenting out the line below and then doing the following steps: 1) load app for first time so that the initial New York City data is shown 2) press the refresh button on the app 3) go offline 4) reload the app. You expect to see the newer NYC data, but you actually see the initial data. This happens because the service worker is not yet activated. <code>self.clients.claim()</code> essentially lets you activate the service worker faster.</p>

</aside> 
                            
                            <p>
                              Yang terakhir, perbarui daftar file yang dibutuhkan untuk shell aplikasi. Dalam array, kita harus menyertakan semua file yang dibutuhkan aplikasi kita, termasuk gambar, JavaScript, stylesheet, dll. Dekat bagian atas file <code>service-worker.js</code> Anda, ganti <code>var filesToCache = [];</code> dengan kode di bawah ini:
                            </p>
                            
                            <pre><code>var filesToCache = [
  '/',
  '/index.html',
  '/scripts/app.js',
  '/styles/inline.css',
  '/images/clear.png',
  '/images/cloudy-scattered-showers.png',
  '/images/cloudy.png',
  '/images/fog.png',
  '/images/ic_add_white_24px.svg',
  '/images/ic_refresh_white_24px.svg',
  '/images/partly-cloudy.png',
  '/images/rain.png',
  '/images/scattered-showers.png',
  '/images/sleet.png',
  '/images/snow.png',
  '/images/thunderstorm.png',
  '/images/wind.png'
];
</code></pre><aside class="key-point">

<p>Be sure to include all permutations of file names, for example our app is served from <code>index.html</code>, but it may also be requested as <code>/</code> since the server sends <code>index.html</code> when a root folder is requested. You could deal with this in the <code>fetch</code> method, but it would require special casing which may become complex.</p>

</aside> 
                            
                            <p>
                              Aplikasi kita belum sepenuhnya bekerja offline. Kita sudah meng-cache komponen shell aplikasi, namun kita masih harus memuatnya dari cache lokal.
                            </p>
                            
                            <h3>
                              Menyediakan shell aplikasi dari cache
                            </h3>
                            
                            <p>
                              Service worker memberikan kemampuan untuk mencegat permintaan yang dilakukan dari Progressive Web App dan menanganinya dalam service worker. Ini berarti kita bisa menentukan bagaimana kita menangani suatu permintaan dan mungkin sekali menyediakan respons cache kita sendiri.
                            </p>
                            
                            <p>
                              Misalnya:
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(event) {
  // Do something interesting with the fetch here
});
</code></pre>
                            
                            <p>
                              Sekarang mari kita sediakan shell aplikasi dari cache. Tambahkan kode berikut ke bagian bawah file <code>service-worker.js</code> Anda:
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(e) {
  console.log('[ServiceWorker] Fetch', e.request.url);
  e.respondWith(
    caches.match(e.request).then(function(response) {
      return response || fetch(e.request);
    })
  );
});
</code></pre>
                            
                            <p>
                              Bergerak dari dalam ke luar, <code>caches.match()</code> mengevaluasi permintaan web yang memicu kejadian <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">fetch</a>, dan memeriksa apakah itu tersedia dalam cache. Ini kemudian merespons dengan versi cache, atau menggunakan <code>fetch</code> untuk mendapatkan salinan dari jaringan. <code>response</code> dikembalikan ke laman web dengan <code>e.respondWith()</code>.
                            </p><aside class="warning">

<p>If you're not seeing the <code>[ServiceWorker]</code> logging in the console, be sure you've changed the <code>cacheName</code> variable and that you're inspecting the right service worker by opening the Service Worker pane in the Applications panel and clicking <strong>inspect</strong> on the running service worker. If that doesn't work, see the section on Tips for testing live service workers.</p>

</aside> 
                            
                            <h3>
                              Lakukan pengujian
                            </h3>
                            
                            <p>
                              Aplikasi Anda sekarang bisa-offline! Mari kita mencobanya.
                            </p>
                            
                            <p>
                              Muat ulang laman Anda lalu buka panel <strong>Cache Storage</strong> pada panel <strong>Application</strong> dari DevTools. Luaskan bagian itu dan Anda akan melihat nama cache shell aplikasi Anda tercantum di sisi sebelah kiri. Bila Anda mengeklik pada cache shell aplikasi, Anda bisa melihat semua sumber daya yang saat ini telah di-cache.
                            </p>
                            
                            <p>
                              <img src="img/ab9c361527825fac.png" alt="ab9c361527825fac.png" />
                            </p>
                            
                            <p>
                              Sekarang, mari kita menguji mode offline. Kembali ke panel <strong>Service Worker</strong> dari DevTools dan aktifkan kotak centang <strong>Offline</strong>. Setelah mengaktifkannya, Anda akan melihat ikon peringatan kecil berwarna kuning di sebelah tab panel <strong>Network</strong>. Ini menunjukkan bahwa Anda offline.
                            </p>
                            
                            <p>
                              <img src="img/7656372ff6c6a0f7.png" alt="7656372ff6c6a0f7.png" />
                            </p>
                            
                            <p>
                              Muat ulang laman Anda dan... itu bekerja! Paling tidak, sedikit bekerja. Perhatikan bagaimana itu memuat data cuaca (palsu) awal.
                            </p>
                            
                            <p>
                              <img src="img/8a959b48e233bc93.png" alt="8a959b48e233bc93.png" />
                            </p>
                            
                            <p>
                              Periksa klausul <code>else</code> di <code>app.getForecast()</code> untuk memahami mengapa aplikasi ini mampu memuat data palsu.
                            </p>
                            
                            <p>
                              Langkah berikutnya adalah memodifikasi logika aplikasi dan service worker agar bisa meng-cache data cuaca, dan mengembalikan data terbaru dari cache ketika aplikasi offline.
                            </p>
                            
                            <p>
                              <strong>Tip:</strong> Untuk memulai baru dan menghapus semua data yang tersimpan (localStoarge, data indexedDB, file cache) dan membuang service worker, gunakan panel Clear storage di tab Application.
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-06/">Tautan</a>
                            </p>
                            
                            <h3>
                              Waspadalah terhadap kasus ekstrem
                            </h3>
                            
                            <p>
                              Seperti yang disebutkan sebelumnya, kode ini <strong>tidak boleh digunakan dalam produksi</strong> karena banyaknya kasus ekstrem yang tidak tertangani.
                            </p>
                            
                            <h4>
                              Cache mengandalkan pembaruan kunci cache untuk setiap perubahan
                            </h4>
                            
                            <p>
                              Misalnya metode caching ini mengharuskan Anda untuk memperbarui kunci cache setiap kali materi berubah, jika tidak, cache tidak akan diperbarui, dan materi lama yang akan ditampilkan. Jadi pastikan untuk mengubah kunci cache bersama setiap perubahan saat Anda bekerja pada proyek Anda!
                            </p>
                            
                            <h4>
                              Mengharuskan semuanya diunduh ulang setiap terjadi perubahan
                            </h4>
                            
                            <p>
                              Kekurangan lainnya adalah bahwa seluruh cache dibuat tidak valid dan harus diunduh ulang setiap kali terjadi perubahan file. Ini berarti memperbaiki kesalahan ejaan karakter tunggal sederhana akan membuat cache tidak valid dan mengharuskan semuanya diunduh ulang. Tidak efisien.
                            </p>
                            
                            <h4>
                              Cache browser bisa menghalangi cache service worker melakukan pembaruan
                            </h4>
                            
                            <p>
                              Ada peringatan penting lain di sini. Sangat penting bahwa permintaan HTTPS yang dibuat saat penangan pemasangan langsung menuju jaringan dan tidak mengembalikan respons dari cache browser. Jika tidak, browser bisa mengembalikan versi cache yang lama, sehingga cache service worker tidak pernah diperbarui!
                            </p>
                            
                            <h4>
                              Waspadalah terhadap strategi cache-terlebih-dahulu dalam produksi
                            </h4>
                            
                            <p>
                              Aplikasi kita menggunakan strategi cache-terlebih-dahulu, yang mengakibatkan salinan dari setiap materi yang di-cache dikembalikan tanpa memperhatikan jaringan. Meskipun strategi cache-terlebih-dahulu mudah diimplementasikan, ini dapat menyebabkan masalah di masa mendatang. Setelah salinan dari laman host dan pendaftaran service worker di-cache, mengubah konfigurasi service worker bisa sangat sulit dilakukan (karena konfigurasi tergantung tempat itu didefinisikan), dan Anda bisa mendapati diri Anda menerapkan situs yang sangat sulit untuk diperbarui!
                            </p>
                            
                            <h4>
                              Bagaimana cara menghindari kasus ekstrem ini?
                            </h4>
                            
                            <p>
                              Jadi bagaimana kita menghindari kasus ekstrem ini? Gunakan pustaka seperti <a href="https://github.com/GoogleChrome/sw-precache">sw-precache</a>, yang memberikan kontrol presisi mengenai apa yang akan berakhir, memastikan permintaan langsung menuju jaringan dan melakukan semua kerja keras untuk Anda.
                            </p>
                            
                            <h3>
                              Tip untuk menguji service worker aktif
                            </h3>
                            
                            <p>
                              Melakukan debug service worker bisa menjadi sebuah tantangan, dan ketika melibatkan caching, sesuatu bisa menjadi lebih buruk lagi jika cache tidak diperbarui saat Anda mengharapkannya. Anda bisa cepat frustrasi saat mengurusi daur hidup service worker khusus dan bug dalam kode Anda. Tapi jangan. Ada beberapa alat yang bisa Anda gunakan untuk memudahkan Anda.
                            </p>
                            
                            <h4>
                              Mulai Baru
                            </h4>
                            
                            <p>
                              Dalam beberapa kasus, Anda mungkin mengalami kejadian memuat data cache atau hal tersebut tidak diperbarui seperti yang Anda harapkan. Untuk menghapus semua data yang tersimpan (localStoarge, data indexedDB, file cache) dan membuang service worker, gunakan panel Clear storage di tab Application.
                            </p>
                            
                            <p>
                              Beberapa tip lain:
                            </p>
                            
                            <ul>
                              <li>
                                Objek <code>app</code> yang berisi beberapa informasi kunci yang diperlukan untuk aplikasi.
                              </li>
                              <li>
                                Event listener untuk semua tombol di header (<code>add/refresh</code>) dan pada dialog tambahkan kota (<code>add/cancel</code>).
                              </li>
                              <li>
                                Metode untuk menambah atau memperbarui kartu prakiraan (<code>app.updateForecastCard</code>).
                              </li>
                              <li>
                                Metode untuk mendapatkan data prakiraan cuaca terbaru dari Firebase Public Weather API (<code>app.getForecast</code>).
                              </li>
                            </ul>
                            
                            <h2>
                              Menggunakan service worker untuk meng-cache data prakiraan
                            </h2>
                            
                            <p>
                              Memilih <a href="https://jakearchibald.com/2014/offline-cookbook/">strategi caching</a> yang tepat untuk data adalah hal yang sangat penting dan bergantung pada tipe data yang diberikan aplikasi Anda. Misalnya, data sensitif-waktu seperti cuaca atau harga saham harus selalu terbaru, sedangkan gambar avatar untuk materi artikel bisa diperbarui lebih jarang.
                            </p>
                            
                            <p>
                              Strategi <a href="https://jakearchibald.com/2014/offline-cookbook/#cache-network-race">cache-dulu-lalu-jaringan</a> sangat ideal untuk aplikasi kita. Ini menampilkan data di layar secepat mungkin, kemudian memperbaruinya setelah jaringan mengembalikan data terbaru. Dibandingkan strategi jaringan-dulu-lalu-cache, pengguna tidak harus menunggu sampai <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">fetch</a> habis waktunya untuk mendapatkan data cache.
                            </p>
                            
                            <p>
                              Cache-dulu-lalu-jaringan berarti kita harus memulai dua permintaan asinkron, satu ke cache dan satu ke jaringan. Permintaan jaringan dengan aplikasi tidak perlu banyak berubah, namun kita harus memodifikasi service worker untuk meng-cache respons sebelum mengembalikannya.
                            </p>
                            
                            <p>
                              Dalam keadaan normal, data cache akan dikembalikan secara langsung dan menyediakan aplikasi dengan data terbaru yang bisa digunakan. Kemudian, ketika permintaan jaringan kembali, aplikasi akan diperbarui menggunakan data terbaru dari jaringan.
                            </p>
                            
                            <h3>
                              Mencegat permintaan jaringan dan meng-cache respons
                            </h3>
                            
                            <p>
                              Kita harus memodifikasi service worker untuk mencegat permintaan ke weather API dan menyimpan responsnya dalam cache, sehingga kita bisa dengan mudah mengaksesnya di lain waktu. Dalam strategi cache-lalu-jaringan, kita mengharapkan respons jaringan sebagai 'sumber kebenaran', selalu menyediakan kita dengan informasi terbaru. Jika tidak bisa, tidak apa-apa karena kita sudah mengambil data cache terbaru dalam aplikasi kita.
                            </p>
                            
                            <p>
                              Dalam service worker, tambahkan <code>dataCacheName</code> sehingga kita bisa memisahkan data aplikasi dari shell aplikasi. Ketika shell aplikasi diperbarui dan cache lama dibersihkan, data kita akan tetap tak tersentuh, sehingga siap untuk pemuatan super cepat. Perlu diingat, jika format data Anda berubah nantinya, Anda perlu cara untuk menanganinya dan memastikan shell aplikasi dan materi tetap tersinkronisasi.
                            </p>
                            
                            <p>
                              Tambahkan baris berikut ke bagian atas file <code>service-worker.js</code> Anda:
                            </p>
                            
                            <pre><code>var dataCacheName = 'weatherData-v1';
</code></pre>
                            
                            <p>
                              Berikutnya, perbarui penangan kejadian <code>activate</code> sehingga tidak menghapus cache data ketika membersihkan cache shell aplikasi.
                            </p>
                            
                            <pre><code>if (key !== cacheName && key !== dataCacheName) {
</code></pre>
                            
                            <p>
                              Yang terakhir, perbarui penangan kejadian <code>fetch</code> untuk menangani permintaan ke data API secara terpisah dari permintaan lainnya.
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(e) {
  console.log('[Service Worker] Fetch', e.request.url);
  var dataUrl = 'https://query.yahooapis.com/v1/public/yql';
  if (e.request.url.indexOf(dataUrl) &gt; -1) {
    /*
     * When the request URL contains dataUrl, the app is asking for fresh
     * weather data. In this case, the service worker always goes to the
     * network and then caches the response. This is called the "Cache then
     * network" strategy:
     * https://jakearchibald.com/2014/offline-cookbook/#cache-then-network
     */
    e.respondWith(
      caches.open(dataCacheName).then(function(cache) {
        return fetch(e.request).then(function(response){
          cache.put(e.request.url, response.clone());
          return response;
        });
      })
    );
  } else {
    /*
     * The app is asking for app shell files. In this scenario the app uses the
     * "Cache, falling back to the network" offline strategy:
     * https://jakearchibald.com/2014/offline-cookbook/#cache-falling-back-to-network
     */
    e.respondWith(
      caches.match(e.request).then(function(response) {
        return response || fetch(e.request);
      })
    );
  }
});
</code></pre>
                            
                            <p>
                              Kode ini mencegat permintaan dan memeriksa apakah URL dimulai dengan alamat weather API. Jika benar, kita akan menggunakan <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">fetch</a> untuk membuat permintaan. Setelah respons dikembalikan, kode kita akan membuka cache, membuat duplikat respons, menyimpannya dalam cache, dan akhirnya mengembalikan respons ke pemohon asli.
                            </p>
                            
                            <p>
                              Aplikasi kita belum dapat bekerja secara offline. Kita telah mengimplementasikan caching dan pengambilan untuk shell aplikasi, namun meskipun kami melakukan cache data, aplikasi belum memeriksa cache untuk melihat apakah ia memiliki data cuaca.
                            </p>
                            
                            <h3>
                              Membuat permintaan
                            </h3>
                            
                            <p>
                              Seperti telah disebutkan sebelumnya, aplikasi harus memulai dua permintaan asinkron, satu untuk cache dan satu untuk jaringan. Aplikasi menggunakan objek <code>caches</code> yang tersedia di <code>window</code> untuk mengakses cache dan mengambil data terbaru. Ini adalah contoh yang sangat baik dari penyempurnaan progresif karena objek <code>caches</code> mungkin tidak tersedia di semua browser, dan jika memang begitu, permintaan jaringan seharusnya tetap bekerja.
                            </p>
                            
                            <p>
                              Untuk melakukan ini, kita harus:
                            </p>
                            
                            <ol start="1">
                              <li>
                                Memeriksa apakah objek <code>caches</code> tersedia di objek <code>window</code> global.
                              </li>
                              
                              <li>
                                Meminta data dari cache.
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                Ketika pertama kali dijalankan, aplikasi Anda harus langsung menunjukkan pengguna prakiraan dari <code>initialWeatherForecast</code>.
                              </li>
                            </ul>
                            
                            <ol start="3">
                              <li>
                                Meminta data dari server.
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                Setelah tidak terdaftar, service worker akan tetap tercantum sampai jendela browser yang memuatnya ditutup.
                              </li>
                              <li>
                                Jika beberapa jendela dalam aplikasi Anda terbuka, service worker baru tidak akan berpengaruh hingga mereka semua dimuat ulang dan diperbarui ke service worker terbaru.
                              </li>
                            </ul>
                            
                            <h4>
                              Mendapatkan data dari cache
                            </h4>
                            
                            <p>
                              Berikutnya, kita harus memeriksa apakah objek <code>caches</code> ada dan meminta data terbaru dari situ. Temukan komentar <code>TODO add cache logic here</code> di <code>app.getForecast()</code>, dan kemudian tambahkan kode berikut di bawah komentar.
                            </p>
                            
                            <pre><code>    if ('caches' in window) {
      /*
       * Check if the service worker has already cached this city's weather
       * data. If the service worker has the data, then display the cached
       * data while the app fetches the latest data.
       */
      caches.match(url).then(function(response) {
        if (response) {
          response.json().then(function updateFromCache(json) {
            var results = json.query.results;
            results.key = key;
            results.label = label;
            results.created = json.query.created;
            app.updateForecastCard(results);
          });
        }
      });
    }
</code></pre>
                            
                            <p>
                              Aplikasi cuaca kita sekarang membuat dua permintaan data asinkron, satu dari <code>cache</code> dan satu melalui XHR. Jika terdapat data dalam cache, itu akan dikembalikan dan dirender dengan sangat cepat (puluhan milidetik) dan memperbarui kartu hanya jika XHR belum diselesaikan. Kemudian, ketika XHR merespons, kartu akan diperbarui dengan data terbaru langsung dari weather API.
                            </p>
                            
                            <p>
                              Perhatikan bagaimana permintaan cache dan permintaan XHR berakhir dengan panggilan untuk memperbarui kartu prakiraan. Bagaimana aplikasi tahu bahwa itu menampilkan data terbaru? Ini ditangani dalam kode berikut dari <code>app.updateForecastCard</code>:
                            </p>
                            
                            <pre><code>    var cardLastUpdatedElem = card.querySelector('.card-last-updated');
    var cardLastUpdated = cardLastUpdatedElem.textContent;
    if (cardLastUpdated) {
      cardLastUpdated = new Date(cardLastUpdated);
      // Bail if the card has more recent data then the data
      if (dataLastUpdated.getTime() &lt; cardLastUpdated.getTime()) {
        return;
      }
    }
</code></pre>
                            
                            <p>
                              Setiap kali kartu diperbarui, aplikasi menyimpan stempel waktu dari data pada atribut tersembunyi dalam kartu. Aplikasi ini hanya terlepas jika stempel waktu yang ada pada kartu lebih baru daripada data yang diteruskan ke fungsi.
                            </p>
                            
                            <h3>
                              Lakukan pengujian
                            </h3>
                            
                            <p>
                              Aplikasi seharusnya sudah berfungsi offline sepenuhnya sekarang. Simpan beberapa kota dan tekan tombol refresh pada aplikasi untuk mendapatkan data cuaca baru, kemudian masuk ke mode offline dan muat ulang halaman tersebut.
                            </p>
                            
                            <p>
                              Kemudian buka panel <strong>Cache Storage</strong> pada panel <strong>Application</strong> dari DevTools. Luaskan bagian itu dan Anda akan melihat nama shell aplikasi dan cache data tercantum di sisi sebelah kiri. Membuka data cache akan membuat data tersimpan untuk setiap kota.
                            </p>
                            
                            <p>
                              <img src="img/cf095c2153306fa7.png" alt="cf095c2153306fa7.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-07/">Tautan</a>
                            </p>
                            
                            <h2>
                              Dukungan integrasi asli
                            </h2>
                            
                            <p>
                              Tak ada yang suka mengetikkan URL yang panjang di keyboard seluler jika memang tidak terpaksa. Dengan fitur Add to Home Screen, pengguna bisa memilih untuk menambahkan tautan pintasan ke perangkat seperti ketika mereka memasang aplikasi asli dari toko, namun dengan lebih mulus.
                            </p>
                            
                            <h3>
                              Spanduk Pemasangan Aplikasi Web dan Add to Home Screen untuk Chrome pada Android
                            </h3>
                            
                            <p>
                              Spanduk pemasangan aplikasi web memberikan Anda kemampuan agar pengguna bisa dengan cepat dan mulus menambahkan aplikasi web ke layar beranda mereka, sehingga mudah dijalankan dan kembali ke aplikasi Anda. Sangat mudah menambahkan spanduk pemasangan aplikasi, karena sebagian besar tugas tersebut sudah ditangani Chrome. Kita hanya perlu memasukkan file manifes aplikasi web dengan detail tentang aplikasi tersebut.
                            </p>
                            
                            <p>
                              Chrome kemudian menggunakan sejumlah kriteria seperti penggunaan service worker, status SSL dan heuristik frekuensi kunjungan untuk menentukan kapan spanduk akan ditampilkan. Selain itu, pengguna secara manual bisa menambahkannya melalui tombol menu "Add to Home Screen" di Chrome.
                            </p>
                            
                            <h4>
                              Mendeklarasikan manifes aplikasi dengan file <code>manifest.json</code>
                            </h4>
                            
                            <p>
                              Manifes aplikasi web adalah file JSON sederhana yang memberikan Anda, developer, kemampuan untuk mengontrol bagaimana aplikasi terlihat oleh pengguna di daerah yang mereka harap akan melihat aplikasi (misalnya, layar beranda seluler), mengarahkan apa yang bisa diluncurkan pengguna, dan yang lebih penting lagi adalah bagaimana mereka bisa meluncurkannya.
                            </p>
                            
                            <p>
                              Dengan menggunakan manifes aplikasi web, aplikasi web Anda bisa:
                            </p>
                            
                            <ul>
                              <li>
                                Jika permintaan server masih belum selesai, perbarui aplikasi dengan data cache.
                              </li>
                              <li>
                                Be launched in full-screen mode on Android with no URL bar
                              </li>
                              <li>
                                Control the screen orientation for optimal viewing
                              </li>
                              <li>
                                Define a "splash screen" launch experience and theme color for the site
                              </li>
                              <li>
                                Track whether you're launched from the home screen or URL bar
                              </li>
                            </ul>
                            
                            <p>
                              Membuat file bernama <code>manifest.json</code> di folder <code>work</code> Anda dan salin/tempel materi berikut:
                            </p>
                            
                            <pre><code>{
  "name": "Weather",
  "short_name": "Weather",
  "icons": [{
    "src": "images/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    }],
  "start_url": "/index.html",
  "display": "standalone",
  "background_color": "#3E4EB8",
  "theme_color": "#2F3BA2"
}
</code></pre>
                            
                            <p>
                              Manifes mendukung berbagai ikon, ditujukan untuk ukuran layar yang berbeda. Pada saat penulisan ini, Chrome dan Opera Mobile, adalah satu-satunya browser yang mendukung manifes aplikasi web, tidak akan menggunakan apa pun yang lebih kecil dari 192 px.
                            </p>
                            
                            <p>
                              Cara termudah untuk melacak bagaimana aplikasi diluncurkan adalah menambahkan string kueri ke parameter <code>start_url</code> dan kemudian menggunakan suite analytics untuk melacak string kueri. Jika Anda menggunakan metode ini, ingat untuk memperbarui daftar file yang di-cache dengan Shell Aplikasi untuk memastikan bahwa file dengan string kueri telah di-cache.
                            </p>
                            
                            <h4>
                              Memberi tahu browser tentang file manifes Anda
                            </h4>
                            
                            <p>
                              Sekarang tambahkan baris berikut ke bagian bawah elemen <code>&lt;head&gt;</code> dalam file <code>index.html</code> Anda:
                            </p>
                            
                            <pre><code>&lt;link rel="manifest" href="/manifest.json"&gt;
</code></pre>
                            
                            <h4>
                              Praktik Terbaik
                            </h4>
                            
                            <ul>
                              <li>
                                Menyimpan data untuk akses cepat nantinya.
                              </li>
                              <li>
                                Memperbarui aplikasi dengan data baru dari server.
                              </li>
                              <li>
                                Define icon sets for different density screens. Chrome will attempt to use the icon closest to 48dp, for example, 96px on a 2x device or 144px for a 3x device.
                              </li>
                              <li>
                                Remember to include an icon with a size that is sensible for a splash screen and don't forget to set the <code>background_color</code>.
                              </li>
                            </ul>
                            
                            <p>
                              Bacaan Lebih Lanjut:
                            </p>
                            
                            <p>
                              <a href="/web/fundamentals/engage-and-retain/simplified-app-installs/">Menggunakan spanduk pemasangan aplikasi</a>
                            </p>
                            
                            <h3>
                              Elemen Add to Homescreen untuk Safari pada iOS
                            </h3>
                            
                            <p>
                              Dalam <code>index.html</code> Anda, tambahkan baris berikut ke bagian bawah elemen <code>&lt;head&gt;</code>:
                            </p>
                            
                            <pre><code>  &lt;!-- Add to home screen for Safari on iOS --&gt;
  &lt;meta name="apple-mobile-web-app-capable" content="yes"&gt;
  &lt;meta name="apple-mobile-web-app-status-bar-style" content="black"&gt;
  &lt;meta name="apple-mobile-web-app-title" content="Weather PWA"&gt;
  &lt;link rel="apple-touch-icon" href="images/icons/icon-152x152.png"&gt;
</code></pre>
                            
                            <h3>
                              Ikon Petak untuk Windows
                            </h3>
                            
                            <p>
                              Dalam <code>index.html</code> Anda, tambahkan baris berikut ke bagian bawah elemen <code>&lt;head&gt;</code>:
                            </p>
                            
                            <pre><code>  &lt;meta name="msapplication-TileImage" content="images/icons/icon-144x144.png"&gt;
  &lt;meta name="msapplication-TileColor" content="#2F3BA2"&gt;
</code></pre>
                            
                            <h3>
                              Lakukan pengujian
                            </h3>
                            
                            <p>
                              Pada bagian ini kami akan menunjukkan beberapa cara untuk menguji manifes aplikasi web Anda.
                            </p>
                            
                            <p>
                              Cara pertama adalah dengan DevTools. Buka panel __Manifest __pada panel __Application __. Jika Anda menambahkan informasi manifes dengan benar, Anda akan melihatnya di-parse dan ditampilkan dalam format yang mudah dipahami di panel ini.
                            </p>
                            
                            <p>
                              Anda juga bisa menguji fitur add to homescreen dari panel ini. Klik tombol __Add to homescreen __. Anda akan melihat pesan "add this site to your shelf" di bawah bilah URL, seperti pada tangkapan layar di bawah ini.
                            </p>
                            
                            <p>
                              <img src="img/cbfdd0302b611ab0.png" alt="cbfdd0302b611ab0.png" />
                            </p>
                            
                            <p>
                              Ini adalah fitur desktop yang serupa dengan add to homescreen pada seluler. Jika berhasil memicu peringatan ini pada desktop, maka Anda bisa memastikan bahwa pengguna seluler menambahkan aplikasi ke perangkat mereka.
                            </p>
                            
                            <p>
                              Cara kedua adalah dengan mengujinya melalui Web Server for Chrome. Dengan pendekatan ini, Anda mengekspos server development lokal (pada desktop atau laptop) ke komputer lain, dan kemudian hanya mengakses progressive web app dari perangkat seluler yang sesungguhnya.
                            </p><aside class="warning">

<p>Opening up a port for remote access is handy for testing this step but may be blocked by your computer's firewall rules or network administrator. Opening ports for remote access is generally not a good thing to leave running on your computer. So, for security reasons, when you've completed testing this step, disable the <code>Accessible on local network</code> option and restart your web server.</p>

</aside> 
                            
                            <p>
                              Pada dialog konfigurasi Web Server for Chrome, pilih opsi <code>Accessible on local network</code>:
                            </p>
                            
                            <p>
                              <img src="img/81347b12f83e4291.png" alt="81347b12f83e4291.png" />
                            </p>
                            
                            <p>
                              Alihkan Web Server ke <code>STOPPED</code> dan kembali ke <code>STARTED</code>. Anda akan melihat URL baru yang bisa digunakan untuk mengakses aplikasi dari jarak jauh.
                            </p>
                            
                            <p>
                              Sekarang, akses situs Anda dari perangkat seluler, menggunakan URL yang baru.
                            </p>
                            
                            <p>
                              Anda akan melihat kesalahan service worker di konsol saat menguji dengan cara ini karena service worker tidak disajikan melalui HTTPS.
                            </p>
                            
                            <p>
                              Menggunakan Chrome dari perangkat Android, coba tambahkan aplikasi ke layar beranda dan verifikasi bahwa layar peluncuran muncul dengan benar dan menggunakan ikon yang tepat.
                            </p>
                            
                            <p>
                              Pada Safari dan Internet Explorer, Anda juga bisa secara manual menambahkan aplikasi ke layar beranda.
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-08/">Tautan</a>
                            </p>
                            
                            <h2>
                              Menerapkan dalam host yang aman dan rayakan
                            </h2>
                            
                            <p>
                              Langkah terakhir adalah menerapkan aplikasi cuaca dalam server yang mendukung HTTPS. Jika Anda belum memilikinya, pendekatan yang sangat mudah (dan gratis) adalah menggunakan hosting materi statis dari Firebase. Sangat mudah digunakan, menyajikanmateri melalui HTTPS dan didukung oleh CDN global.
                            </p>
                            
                            <h3>
                              Kredit tambahan: mengecilkan dan menyisipkan CSS
                            </h3>
                            
                            <p>
                              Ada satu hal lagi yang harus Anda pertimbangkan, mengecilkan penataan gaya kunci dan menyisipkannya langsung ke dalam <code>index.html</code>. <a href="/speed">Page Speed Insights</a> merekomendasikan penyajian materi paro atas di permintaan 15.000 byte yang pertama.
                            </p>
                            
                            <p>
                              Lihat seberapa kecil Anda bisa mendapatkan permintaan awal dengan segala sesuatu disisipkan.
                            </p>
                            
                            <p>
                              Bacaan Lebih Lanjut: <a href="/speed/docs/insights/rules">PageSpeed Insight Rules</a>
                            </p><aside class="key-point">

<p>This step requires you to have  <a href="https://docs.npmjs.com/getting-started/installing-node">Node &#x26; NPM</a> installed on your system. If it's not, you can use any other hosting provider that supports HTTP<strong>S</strong>. We've used Firebase because it automatically redirects users from HTTP to HTTP<strong>S</strong>. If you use a different provider, be sure they're always redirects to HTTP<strong>S</strong>.</p>

</aside> 
                            
                            <h3>
                              Menerapkan ke Firebase
                            </h3>
                            
                            <p>
                              Jika Anda baru dalam dunia Firebase, Anda harus membuat akun dan memasang beberapa alat terlebih dahulu.
                            </p>
                            
                            <ol start="1">
                              <li>
                                Buat akun Firebase di <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a>
                              </li>
                              
                              <li>
                                Pasang alat Firebase melalui npm: <code>npm install -g firebase-tools</code>
                              </li>
                            </ol>
                            
                            <p>
                              Setelah akun dibuat dan Anda telah masuk, Anda siap untuk menerapkan!
                            </p>
                            
                            <ol start="1">
                              <li>
                                Buat aplikasi baru di <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a>
                              </li>
                              
                              <li>
                                Jika Anda belum masuk ke alat Firebase, perbarui kredensial Anda: <code>firebase login</code>
                              </li>
                              
                              <li>
                                Inisialisasi aplikasi Anda, serta berikan direktori (kemungkinan besar <code>work</code>) tempat aplikasi berada setelah selesai: <code>firebase init</code>
                              </li>
                              
                              <li>
                                Yang terakhir, terapkan aplikasi ke Firebase: <code>firebase deploy</code>
                              </li>
                              
                              <li>
                                Rayakan. Selesai! Aplikasi Anda akan diterapkan ke domain: <code>https://YOUR-FIREBASE-APP.firebaseapp.com</code>
                              </li>
                            </ol>
                            
                            <p>
                              Bacaan lebih lanjut: <a href="https://www.firebase.com/docs/hosting/guide/">Panduan Hosting Firebase</a>
                            </p>
                            
                            <h3>
                              Lakukan pengujian
                            </h3>
                            
                            <ul>
                              <li>
                                Memiliki kehadiran yang kaya di layar beranda Android pengguna
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/final/">Tautan</a>
                            </p>
                            
                            <h2>
                              Menemukan masalah, atau memiliki masukan? {: .hide-from-toc }
                            </h2>
                            
                            <p>
                              Bantu kami menjadikan code lab lebih baik dengan mengirimkan <a href="https://github.com/googlecodelabs/your-first-pwapp/issues">masalah</a> hari ini. Dan terima kasih!
                            </p>