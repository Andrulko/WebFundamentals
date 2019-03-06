project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Sebagian besar browser bisa mengakses kamera pengguna.

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-08-23 #}

# Mengambil Gambar dari Pengguna {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

Banyak browser kini mampu mengakses masukan video dan audio dari pengguna. Akan tetapi, bergantung pada browser, hal ini bisa menjadi pengalaman inline dan dinamis penuh, atau bisa didelegasikan ke aplikasi lain pada perangkat pengguna.

## Mulailah dengan sederhana dan progresif

Hal termudah untuk dilakukan adalah cukup meminta file yang sudah direkam kepada pengguna. Lakukan hal ini dengan membuat elemen masukan file sederhana dan menambahkan filter `accept` yang menunjukkan bahwa kita hanya bisa menerima file gambar dan idealnya kita akan mendapatkannya langsung dari kamera.

### Menjepret satu bingkai

Metode ini bekerja pada semua platform. Di desktop, ini akan meminta pengguna untuk mengunggah file gambar dari sistem file. Di Safari pada iOS metode ini akan membuka aplikasi kamera, sehingga Anda dapat menjepret gambar, lalu mengirimnya kembali ke laman web. Di Android metode ini akan meminta pengguna memilih aplikasi untuk menjepret gambar sebelum mengirimnya kembali ke laman web.

Datanya kemudian dapat dilampirkan ke `<form>` atau dimanipulasi dengan JavaScript dengan memantau kejadian `onchange` pada elemen masukan, lalu membaca properti `files` pada kejadian `target`.

### Memperoleh akses ke kamera

Memperoleh akses ke file gambar itu mudah.

    <input type="file" accept="image/*" capture>
    

Setelah Anda memiliki akses ke file tersebut, Anda bisa melakukan apa saja yang diinginkan padanya. Misalnya, Anda bisa:

![](images/ios-chooser.png) ![](images/android-chooser.png)

Browser modern dapat mengakses langsung kamera, sehingga kita dapat menciptakan pengalaman yang terintegrasi penuh dengan laman web, agar pengguna tidak perlu meninggalkan browser.

<pre class="prettyprint">&lt;input type="file" accept="image/*" id="file-input">
&lt;script>
  const fileInput = document.getElementById('file-input');

  fileInput.addEventListener('change', (e) => doSomethingWithFiles(e.target.files));
&lt;/script>
</pre>

Caution: Akses langsung ke kamera adalah kemampuan yang dahsyat. Anda harus mendapat izin dari pengguna dan situs Anda harus menggunakan protokol yang aman (HTTPS).

Kita bisa mengakses langsung kamera dan mikrofon dengan menggunakan API di spesifikasi WebRTC yang disebut `getUserMedia()`. Ini akan meminta izin pengguna untuk mengakses mikrofon dan kamera yang terhubung.

    <video id="player" controls autoplay></video>
    <script>  
      var player = document.getElementById('player');
    
      var handleSuccess = function(stream) {
        player.srcObject = stream;
      };
    
      navigator.mediaDevices.getUserMedia({video: true})
          .then(handleSuccess);
    </script>
    

Jika berhasil, API akan mengembalikan `MediaStream` yang berisi data dari kamera, lalu kita bisa melampirkannya ke elemen `<video>` dan memutarnya untuk menampilkan pratinjau realtime, atau melampirkannya ke `<canvas>` untuk menjepret cuplikan.

Untuk mendapatkan data dari kamera, kita tinggal menyetel `video: true` di objek pembatas yang diteruskan ke API `getUserMedia()`.

### Mengambil cuplikan foto dari kamera

Dengan sendirinya, ini menjadi tidak berguna. Kita hanya bisa mengambil data video dan memutarnya.

Untuk mengakses data mentah dari kamera, kita harus mengambil aliran yang dibuat oleh `getUserMedia()` dan memproses datanya. Tidak seperti `Web Audio`, tidak ada API pemrosesan aliran khusus untuk video di web, jadi kita harus melakukan sedikit akal-akalan untuk menjepret cuplikan dari kamera pengguna.

<pre class="prettyprint">&lt;div id="target">You can drag an image file here&lt;/div>
&lt;script>
  const target = document.getElementById('target');

  target.addEventListener('drop', (e) => {
    e.stopPropagation();
    e.preventDefault();

    doSomethingWithFiles(e.dataTransfer.files);
  });

  target.addEventListener('dragover', (e) => {
    e.stopPropagation();
    e.preventDefault();

    e.dataTransfer.dropEffect = 'copy';
  });
&lt;/script>
</pre>

Prosesnya seperti ini:

Selesai.

Setelah data dari kamera disimpan di kanvas, Anda bisa memanfaatkannya dengan berbagai cara. Anda bisa:

### Menghentikan streaming dari kamera saat tidak perlu

Biasakanlah untuk menghentikan penggunaan kamera saat tidak diperlukan lagi. Ini tidak hanya menghemat baterai dan daya pemrosesan, tetapi juga membuat pengguna lebih yakin pada aplikasi Anda.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;button id="capture">Capture&lt;/button>
&lt;canvas id="snapshot" width=320 height=240>&lt;/canvas>
&lt;script>
  var player = document.getElementById('player'); 
  var snapshotCanvas = document.getElementById('snapshot');
  var captureButton = document.getElementById('capture');
  <strong>var videoTracks;</strong>

  var handleSuccess = function(stream) {
    // Attach the video stream to the video element and autoplay.
    player.srcObject = stream;
    <strong>videoTracks = stream.getVideoTracks();</strong>
  };

  captureButton.addEventListener('click', function() {
    var context = snapshot.getContext('2d');
    context.drawImage(player, 0, 0, snapshotCanvas.width, snapshotCanvas.height);

    <strong>// Stop all video streams.
    videoTracks.forEach(function(track) {track.stop()});</strong>
  });

  navigator.mediaDevices.getUserMedia({video: true})
      .then(handleSuccess);
&lt;/script>
</pre>

Untuk menghentikan akses ke kamera, Anda cukup memanggil `stop()` pada setiap trek video untuk aliran yang dikembalikan oleh `getUserMedia()`.

Jika pengguna belum memberi situs Anda akses ke kamera, begitu Anda memanggil `getUserMedia`, browser akan meminta pengguna untuk mengizinkan situs Anda mengakses kamera.

Pengguna tidak suka dimintai akses ke alat pribadi di komputer atau perangkat mereka dan biasanya mereka memblokir permintaan itu, atau mengabaikannya jika tidak memahami konteks permintaan tersebut. Biasakanlah meminta akses ke kamera hanya saat pertama kali diperlukan. Setelah pengguna memberi akses, mereka tidak akan diminta lagi. Namun, jika pengguna menolak akses, Anda tidak bisa mendapat akses lagi, kecuali jika mereka mengubah setelan izin kamera secara manual.

### Handling a FileList object

Caution: Meminta akses ke kamera saat laman dimuat akan mengakibatkan sebagian besar pengguna menolak permintaan akses.

Informasi selengkapnya tentang implementasi browser seluler dan desktop:

Kami juga menyarankan menggunakan shim [adapter.js](https://github.com/webrtc/adapter) untuk melindungi aplikasi dari perubahan spesifikasi WebRTC dan perbedaan awalan.

<pre class="prettyprint">&lt;img id="output">
&lt;script>
  const output = document.getElementById('output');

  function doSomethingWithFiles(fileList) {
    let file = null;

    for (let i = 0; i &lt; fileList.length; i++) {
      if (fileList[i].type.match(/^image\//)) {
        file = fileList[i];
        break;
      }
    }

    if (file !== null) {
      output.src = URL.createObjectURL(file);
    }
  }
&lt;/script>
</pre>

{# wf_devsite_translation #}

Once you have access to the file you can do anything you want with it. For example, you can:

- Melampirkannya langsung ke elemen `<canvas>` supaya Anda dapat memanipulasinya
- Mengunduhnya ke perangkat pengguna
- Mengunggahnya ke server dengan melampirkan ke `XMLHttpRequest`

## Mengakses kamera secara interaktif

Now that you've covered your bases, it's time to progressively enhance!

Modern browsers can get direct access to cameras, allowing you to build experiences that are fully integrated with the web page, so the user need never leave the browser.

### Acquire access to the camera

You can directly access a camera and microphone by using an API in the WebRTC specification called `getUserMedia()`. This will prompt the user for access to their connected microphones and cameras.

Support for `getUserMedia()` is pretty good, but it isn't yet everywhere. In particular, it is not available in Safari 10 or lower, which at the time of writing is still the latest stable version. However, [Apple have announced](https://webkit.org/blog/7726/announcing-webrtc-and-media-capture/) that it will be available in Safari 11.

It's very simple to detect support, however.

    const supported = 'mediaDevices' in navigator;
    

Warning: Direct access to the camera is a powerful feature. It requires consent from the user, and your site MUST be on a secure origin (HTTPS).

When you call `getUserMedia()`, you need to pass in an object that describes what kind of media you want. These choices are called constraints. There are a several possible constraints, covering things like whether you prefer a front- or rear-facing camera, whether you want audio, and your preferred resolution for the stream.

To get data from the camera, however, you need just one constraint, and that is `video: true`.

If successful the API will return a `MediaStream` that contains data from the camera, and you can then either attach it to a `<video>` element and play it to show a real time preview, or attach it to a `<canvas>` to get a snapshot.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;script>
  const player = document.getElementById('player');

  const constraints = {
    video: true,
  };

  navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
      player.srcObject = stream;
    });
&lt;/script>
</pre>

By itself, this isn't that useful. All you can do is take the video data and play it back. If you want to get an image, you have to do a little extra work.

### Grab a snapshot

Your best supported option for getting an image is to draw a frame from the video to a canvas.

Unlike the Web Audio API, there isn't a dedicated stream processing API for video on the web so you have to resort to a tiny bit of hackery to capture a snapshot from the user's camera.

The process is as follows:

1. Buat objek kanvas yang akan menyimpan bingkai dari kamera
2. Dapatkan akses ke aliran kamera
3. Lampirkan ke elemen video
4. Saat Anda ingin menjepret bingkai tertentu, tambahkan data itu dari elemen video ke objek kanvas menggunakan `drawImage()`.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;button id="capture">Capture&lt;/button>
&lt;canvas id="canvas" width=320 height=240>&lt;/canvas>
&lt;script>
  const player = document.getElementById('player');
  const canvas = document.getElementById('canvas');
  const context = canvas.getContext('2d');
  const captureButton = document.getElementById('capture');

  const constraints = {
    video: true,
  };

  captureButton.addEventListener('click', () => {
    // Draw the video frame to the canvas.
    context.drawImage(player, 0, 0, canvas.width, canvas.height);
  });

  // Attach the video stream to the video element and autoplay.
  navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
      player.srcObject = stream;
    });
&lt;/script>
</pre>

Once you have data from the camera stored in the canvas you can do many things with it. You could:

- Menggunggahnya langsung ke server
- Menyimpannya secara lokal
- Menerapkan efek keren pada gambar

## Meminta izin menggunakan kamera secara bertanggung jawab

### Stop streaming from the camera when not needed

It is good practice to stop using the camera when you no longer need it. Not only will this save battery and processing power, it will also give users confidence in your application.

To stop access to the camera you can simply call `stop()` on each video track for the stream returned by `getUserMedia()`.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;button id="capture">Capture&lt;/button>
&lt;canvas id="canvas" width=320 height=240>&lt;/canvas>
&lt;script>
  const player = document.getElementById('player');
  const canvas = document.getElementById('canvas');
  const context = canvas.getContext('2d');
  const captureButton = document.getElementById('capture');

  const constraints = {
    video: true,
  };

  captureButton.addEventListener('click', () => {
    context.drawImage(player, 0, 0, canvas.width, canvas.height);

    <strong>// Stop all video streams.
    player.srcObject.getVideoTracks().forEach(track => track.stop());</strong>
  });

  navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
      // Attach the video stream to the video element and autoplay.
      player.srcObject = stream;
    });
&lt;/script>
</pre>

### Ask permission to use camera responsibly

If the user has not previously granted your site access to the camera then the instant that you call `getUserMedia()` the browser will prompt the user to grant your site permission to the camera.

Users hate getting prompted for access to powerful devices on their machine and they will frequently block the request, or they will ignore it if they don't understand the context for which the prompt has been created. It is best practice to only ask to access the camera when first needed. Once the user has granted access they won't be asked again. However, if the user rejects access, you can't get access again, unless they manually change camera permission settings.

Warning: Asking for access to the camera on page load will result in most of your users rejecting access to it.

## Kompatibilitas

More information about mobile and desktop browser implementation:

- [srcObject](https://www.chromestatus.com/feature/5989005896187904)
- [navigator.mediaDevices.getUserMedia()](https://www.chromestatus.com/features/5755699816562688)

We also recommend using the [adapter.js](https://github.com/webrtc/adapter) shim to protect apps from WebRTC spec changes and prefix differences.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}