project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: WebVR

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-12-12 #}

# WebVR {: .page-title }

Caution: WebVR masih eksperimental dan dapat berubah.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="jT2mR9WzJ7Y"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

WebVR adalah JavaScript API yang memanfaatkan headset VR dan perangkat berkemampuan VR yang dimiliki pengguna — seperti [headset Daydream](https://vr.google.com/daydream/) dan ponsel Pixel — untuk menciptakan pengalaman 3D yang lebih mendalam di browser Anda.

<div class="clearfix"></div>

## Dukungan dan Ketersediaan

Today the WebXR Device API is [under development](https://www.chromestatus.com/features/5680169905815552), but you can try it out with:

* Chrome Beta (M56+), melalui [Origin Trial](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md).
* Firefox Nightly.

Saat ini WebVR API tersedia di:

Untuk browser yang tidak mendukung WebVR, atau mungkin memiliki versi API yang lebih lama, Anda bisa mundur ke [WebVR Polyfill](https://github.com/googlevr/webvr-polyfill). Akan tetapi, ingatlah bahwa VR *sangat peka pada kinerja* dan polyfill umumnya memiliki biaya kinerja yang relatif besar, jadi Anda mungkin perlu mempertimbangkan penggunaan polyfill bagi pengguna yang tidak memiliki dukungan asli untuk WebVR.

[Get the latest status on WebVR.](./status/)

## Membuat Materi WebVR

To make WebVR content, you will need to make use of some brand new APIs, as well as existing technologies like [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/Tutorial) and [Web Audio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API), as well as accounting for different input types and headsets.

<div class="attempt-left">
  <h3>Get Started with WebVR</h3>
  <a href="./getting-started-with-webvr/">
    <img src="img/getting-started-with-webvr.jpg" alt="Get started with WebVR" />
  </a>
  <p>
    Make a flying start with WebVR by taking a WebGL scene and adding VR APIs.<br>
    <a href="./getting-started-with-webvr/">Learn More</a>
  </p>
</div>

<div class="attempt-right">
  <h3>Add Input to a WebVR Scene</h3>
  <a href="./adding-input-to-a-webvr-scene/">
    <img src="img/adding-input-to-a-webvr-scene.jpg" alt="Add input to a WebVR scene" />
  </a>
  <p>
    Interaction is a crucial part of providing an engaging and immersive experience.<br>
    <a href="./adding-input-to-a-webvr-scene/">Get Started</a>
  </p>
</div>

<div class="clearfix"></div>

### Sumber daya selengkapnya

Untuk membuat materi WebVR Anda perlu memanfaatkan beberapa API baru, serta teknologi yang ada seperti [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/Tutorial) dan [Web Audio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API), serta mempertimbangkan beragam tipe masukan dan headset.

* [Pelajari tentang WebVR API](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API)
* [Lihat Contoh WebVR](https://webvr.info/samples/)
* [Mendesain Google Cardboard](https://www.google.com/design/spec-vr/designing-for-google-cardboard/a-new-dimension.html)

## Lacak kinerja Anda

<img src="img/oce.png" class="attempt-right" alt="WebVR Performance" />

In order to minimize discomfort for the people using WebVR experiences, they must maintain a consistent (and high) frame rate. Failing to do so can give users motion sickness!

Untuk meminimalkan ketidaknyamanan bagi orang yang menggunakan pengalaman WebVR, mereka harus mempertahankan laju bingkai yang konsisten (dan tinggi). Bila tidak dilakukan, itu bisa membuat pengguna mabuk darat!

Para perangkat seluler, laju penyegaran umumnya 60 Hz, ini berarti targetnya adalah 60 fps (atau 16 md per bingkai *termasuk* overhead browser per bingkai). Di desktop, target umumnya adalah 90 Hz (11 md termasuk overhead).

## Terapkan Penyempurnaan Progresif

<img src="img/touch-input.png" class="attempt-right"
  alt="Use Progressive Enhancement to maximize reach" />

What are you to do if your users don’t have a Head Mounted Display (‘HMD’) or VR-capable device? The best answer is to use Progressive Enhancement.

1. Anggaplah pengguna menggunakan masukan tradisional, misalnya keyboard, mouse, atau layar sentuh tanpa akses ke headset VR.
2. Adaptasikan pada perubahan dalam masukan dan ketersediaan headset pada waktu proses.

Apa yang akan Anda lakukan jika pengguna tidak memiliki Head Mounted Display (‘HMD’) atau perangkat berkemampuan VR? Jawaban terbaik adalah menggunakan Penyempurnaan Progresif.

Untungnya [WebVR API](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API) memungkinkan mendeteksi perubahan pada lingkungan VR agar kita dapat menemukan dan mengadaptasikan dengan perubahan dalam masukan dan menampilkan opsi di perangkat pengguna.

Dengan menganggap lingkungan non-VR, pertama Anda bisa memaksimalkan jangkauan pengalaman, dan memastikan Anda menyediakan pengalaman terbaik, terlepas dari persiapan pengguna Anda.

## Feedback {: #feedback }

Untuk informasi selengkapnya, baca panduan kami tentang [menambahkan masukan ke adegan WebVR](./adding-input-to-a-webvr-scene/).