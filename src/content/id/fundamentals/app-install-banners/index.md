project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Ada dua tipe spanduk pemasangan aplikasi: spanduk pemasangan aplikasi web dan spanduk pemasangan aplikasi asli. Keduanya memberi Anda kemampuan untuk memungkinkan pengguna dengan cepat dan mulus menambahkan aplikasi asli atau web Anda ke layar beranda tanpa meninggalkan browser.

{# wf_updated_on: 2017-09-29 #} {# wf_published_on: 2014-12-16 #}

# Spanduk Pemasangan Aplikasi Web {: .page-title }

{% include "web/_shared/contributors/mattgaunt.html" %} {% include "web/_shared/contributors/paulkinlan.html" %}

Ada dua tipe spanduk pemasangan aplikasi: spanduk pemasangan aplikasi **web** dan spanduk pemasangan aplikasi [**asli**](#native-app-install). Keduanya memungkinkan pengguna dengan cepat dan mulus menambahkan aplikasi asli atau web Anda ke layar beranda tanpa meninggalkan browser.

Mudah saja menambahkan spanduk pemasangan aplikasi; sebagian besar tugas berat tersebut ditangani Chrome. Anda perlu menyertakan file manifes aplikasi web di situs bersama detail tentang aplikasi.

* On mobile, Chrome will generate a [WebAPK](/web/fundamentals/integration/webapks), creating an even more integrated experience for your users.
* Memiliki [service worker](/web/fundamentals/getting-started/primers/service-workers) yang terdaftar di situs Anda.

## Kejadian spanduk pemasangan aplikasi

Chrome kemudian menggunakan serangkaian kriteria dan heuristik frekuensi kunjungan untuk menentukan waktu yang tepat menampilkan spanduk. Baca terus untuk detail selengkapnya.

Note: Add to Homescreen (terkadang disingkat menjadi A2HS) adalah nama lain untuk Spanduk Pemasangan Aplikasi Web. Dua istilah itu setara.

## Native app install banners

<figure class="attempt-right">
  <img src="images/a2hs-dialog-g.png" alt="Add to Home Screen dialog on Android">
  <figcaption>Add to Home Screen dialog on Android</figcaption>
</figure>

Chrome secara otomatis menampilkan spanduk bila aplikasi Anda memenuhi kriteria berikut:

1. Buka Chrome DevTools.
2. Masuk ke panel **Application**.
3. Masuk ke tab **Manifest**.

<div class="clearfix"></div>

Note: Spanduk Pemasangan Aplikasi Web adalah teknologi yang sedang berkembang. Kriteria untuk menampilkan spanduk pemasangan aplikasi dapat berubah di masa mendatang. Lihat [Apa, Sebenarnya, yang Menjadikan Sesuatu sebagai Progressive Web App?](https://infrequently.org/2016/09/what-exactly-makes-something-a-progressive-web-app/) untuk referensi kanonis (yang akan diperbarui dari waktu ke waktu) dan kriteria terbaru spanduk pemasangan aplikasi web.

### Apa kriterianya?

Setelah menyiapkan manifes aplikasi web, Anda perlu memvalidasi apakah manifes telah didefinisikan dengan benar. Anda telah memperoleh dua pendekatan yang diinginkan. Yang satu adalah manual, dan satu lagi adalah otomatis.

Untuk memicu spanduk pemasangan aplikasi secara manual:

    window.addEventListener('beforeinstallprompt', function(e) {
      // beforeinstallprompt Event fired
    
      // e.userChoice will return a Promise. 
      // For more details read: https://developers.google.com/web/fundamentals/getting-started/primers/promises
      e.userChoice.then(function(choiceResult) {
    
        console.log(choiceResult.outcome);
    
        if(choiceResult.outcome == 'dismissed') {
          console.log('User cancelled home screen install');
        }
        else {
          console.log('User added to home screen');
        }
      });
    });
    

### Menguji spanduk pemasangan aplikasi {: #test }

The best way to notify the user your app can be installed is by adding a button or other element to your user interface. **Don't show a full page interstitial or other elements that may be annoying or distracting.**

<pre class="prettyprint">window.addEventListener('beforeinstallprompt', (e) => {
  // Prevent Chrome 67 and earlier from automatically showing the prompt
  e.preventDefault();
  // Stash the event so it can be triggered later.
  deferredPrompt = e;
  <strong>// Update UI notify the user they can add to home screen
  btnAdd.style.display = 'block';</strong>
});
</pre>

Lihat [Simulasikan kejadian Add to Homescreen](/web/tools/chrome-devtools/progressive-web-apps#add-to-homescreen) untuk bantuan selengkapnya.

### Apakah pengguna memasang aplikasi?

Untuk pengujian otomatis spanduk pemasangan aplikasi Anda, gunakan Lighthouse. Lighthouse adalah alat (bantu) pengauditan aplikasi web. Anda bisa menjalankannya sebagai Ekstensi Chrome atau sebagai modul NPM. Untuk menguji aplikasi, Anda menyediakan laman khusus untuk audit pada Lighthouse. Lighthouse menjalankan paket audit terhadap laman, kemudian menyediakan hasil laman dalam laporan.

Dua paket audit Lighthouse di tangkapan layar di bawah ini menyatakan semua pengujian yang dibutuhkan laman Anda untuk diteruskan guna menampilkan spanduk pemasangan aplikasi.

    window.addEventListener('beforeinstallprompt', function(e) {
      console.log('beforeinstallprompt Event fired');
      e.preventDefault();
      return false;
    });
    

You can only call `prompt()` on the deferred event once. If the user dismisses it, you'll need to wait until the `beforeinstallprompt` event is fired on the next page navigation.

## The mini-info bar

<figure class="attempt-right">
  <img
      class="screenshot"
      src="/web/updates/images/2018/06/a2hs-infobar-cropped.png">
  <figcaption>
    The mini-infobar
  </figcaption>
</figure>

Lihat [Audit Aplikasi Web dengan Lighthouse](/web/tools/lighthouse/) untuk memulai Lighthouse.

Chrome menyediakan mekanisme yang mudah untuk menentukan bagaimana pengguna menanggapi spanduk pemasangan aplikasi dan bahkan membatalkan atau menundanya sampai waktu yang lebih tepat.

Kejadian `beforeinstallprompt` mengembalikan promise yang disebut `userChoice` yang terselesaikan ketika pengguna beraksi pada prompt. Promise mengembalikan sebuah objek dengan nilai `dismissed` pada atribut `outcome` atau `accepted` jika pengguna menambahkan laman web ke layar beranda.

## Feedback {: .hide-from-toc }

Ini adalah alat yang baik untuk mengetahui bagaimana pengguna berinteraksi dengan peringatan pemasangan aplikasi.

<div class="clearfix"></div>

## Determine if the app was successfully installed {: #appinstalled }

Chrome mengatur kapan memicu prompt, tapi untuk beberapa situs, ini mungkin tidak ideal. Anda bisa menunda prompt ke lain waktu ketika aplikasi digunakan atau bahkan membatalkannya.

    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

## Detecting if your app is launched from the home screen {: #detect-mode }

### Menangguhkan atau membatalkan prompt

Ketika Chrome memutuskan untuk meminta pengguna memasang aplikasi, Anda bisa mencegah tindakan default tersebut dan menyimpan kejadian untuk digunakan di lain waktu. Kemudian ketika pengguna memiliki interaksi positif dengan situs, Anda bisa kembali memicu prompt dengan memanggil `prompt()` pada kejadian yang disimpan.

This causes Chrome to show the banner and all the Promise attributes such as `userChoice` will be available to bind to so that you can understand what action the user took.

    "prefer_related_applications": true,
    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

Atau, Anda bisa membatalkan prompt dengan mencegah tindakan default.

    if (window.matchMedia('(display-mode: standalone)').matches) {
      console.log('display-mode is standalone');
    }
    

### Kriteria untuk menampilkan spanduk

Spanduk pemasangan aplikasi asli serupa dengan [Spanduk pemasangan aplikasi web](.), namun sebagai ganti menambahkan ke layar beranda, spanduk ini memungkinkan pengguna memasang aplikasi asli tanpa meninggalkan situs Anda.

    if (window.navigator.standalone === true) {
      console.log('display-mode is standalone');
    }
    

## Updating your app's icon and name

### Persyaratan manifes

Kriterianya serupa dengan spanduk pemasangan aplikasi web hanya saja membutuhkan service worker. Situs Anda harus:

### Desktop

Untuk mengintegrasikan ke dalam manifes, tambahkan larik `related_applications` bersama platform `play` (untuk Google Play) dan App Id.

## Test your add to home screen experience {: #test }

Jika Anda hanya ingin menawarkan pengguna kemampuan untuk memasang aplikasi Android, dan tidak menampilkan spanduk pemasangan aplikasi web, maka tambahkan `"prefer_related_applications": true`. Misalnya:

{# wf_devsite_translation #}

### Chrome for Android

1. Open a [remote debugging](/web/tools/chrome-devtools/remote-debugging/) session to your phone or tablet.
2. Go to the **Application** panel.
3. Go to the **Manifest** tab.
4. Click **Add to home screen**

### Chrome OS, Linux, or Windows

1. Open Chrome DevTools
2. Go to the **Application** panel.
3. Go to the **Manifest** tab.
4. Click **Add to home screen**

Dogfood: To test the install flow for Desktop Progressive Web Apps on Mac, you'll need to enable the `#enable-desktop-pwas` flag.

### Will `beforeinstallprompt` be fired?

The easiest way to test if the `beforeinstallprompt` event will be fired, is to use [Lighthouse](/web/tools/lighthouse/) to audit your app, and check the results of the [User Can Be Prompted To Install The Web App](/web/tools/lighthouse/audits/install-prompt) test.