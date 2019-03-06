project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: RAIL adalah model kinerja yang berfokus pada pengguna. Setiap aplikasi web memiliki empat aspek berbeda pada siklus hidupnya dan kinerja yang pas pada aspek tersebut dalam cara yang sangat berbeda: Respons, Animasi, Diam, Muat.

{# wf_updated_on: 2015-06-07 #} {# wf_published_on: 2015-06-07 #}

# Ukur Kinerja dengan Model RAIL {: .page-title }

{% include "web/_shared/contributors/megginkearney.html" %}

RAIL adalah model kinerja yang berfokus pada pengguna. Setiap aplikasi web memiliki keempat aspek berbeda ini pada siklus hidupnya dan kinerjanya bergantung pada keempat aspek tersebut dalam beragam cara:

Every web app has four distinct aspects to its life cycle, and performance fits into them in different ways:

<figure>
  <img src="images/rail.png"
    alt="The 4 parts of the RAIL performance model: Response, Animation, Idle, and Load."/>
  <figcaption>
    <b>Figure 1</b>. The 4 parts of the RAIL performance model
  </figcaption>
</figure>

## Fokus pada pengguna

Jadikan pengguna titik fokus upaya kinerja Anda. Sebagian besar waktu yang dihabiskan pengguna di situs Anda bukanlah untuk menunggu situs dimuat, melainkan menunggunya merespons saat digunakan. Pahami cara pengguna mempersepsi keterlambatan:

* Fokus pada pengguna; sasaran akhir Anda bukanlah membuat situs bekerja cepat di suatu perangkat, melainkan menyenangkan pengguna.
* Respons pengguna dengan segera; akui masukan pengguna dalam waktu kurang dari 100 md.

## Respons: respons dalam waktu kurang dari 100 md

Anda memiliki waktu 100 md untuk merespons masukan pengguna sebelum pengguna merasakan jeda waktu. Ini berlaku untuk hampir semua masukan, mulai dari mengeklik tombol, beralih kontrol pada formulir, atau memulai animasi. Ini tidak berlaku pada menyeret atau menggulir melalui sentuhan.

<table class="responsive">
  <thead>
      <th colspan="2">Tunda &amp; Reaksi Pengguna</th>
  </thead>
  <tbody>
    <tr>
      <td data-th="Delay">0 - 16 md</td>
      <td data-th="User Reaction">Orang sangat bagus dalam melacak
      gerakan dan tidak suka bila animasi tidak berjalan mulus. Pengguna
      mempersepsi animasi berjalan mulus asalkan 60 bingkai baru dirender
      setiap detiknya. Itu adalah 16 md per bingkai, termasuk waktu yang diperlukan
      browser untuk menggambar bingkai baru ke layar, yang memberi waktu pada aplikasi
      sekitar 10 md untuk menghasilkan sebuah bingkai.</td>
    </tr>
    <tr>
      <td data-th="Delay">0 - 100 md</td>
      <td data-th="User Reaction">Respons aksi pengguna dalam rentang waktu ini, maka pengguna akan merasa bahwa hasilnya seketika. Jika lebih lama akan merusak hubungan antara aksi dan reaksi.</td>
    </tr>
    <tr>
      <td data-th="Delay">100 - 300 md</td>
      <td data-th="User Reaction">Pengguna akan mengalami sedikit penundaan.</td>
    </tr>
    <tr>
      <td data-th="Delay">300 - 1000 md</td>
      <td data-th="User Reaction">Dalam rentang ini, berbagai hal akan terasa sebagai bagian dari kemajuan tugas yang alami dan kontinu. Bagi sebagian besar pengguna di web, memuat laman atau mengubah tampilan menyatakan sebuah tugas.</td>
    </tr>
    <tr>
      <td data-th="Delay">1000+ md</td>
      <td data-th="User Reaction">Di atas 1 detik, pengguna akan kehilangan fokus pada tugas yang sedang dikerjakan.</td>
    </tr>
    <tr>
      <td data-th="Delay">10.000+ md</td>
      <td data-th="User Reaction">Pengguna akan frustrasi dan mungkin akan meninggalkan tugas; mereka mungkin saja tidak kembali lagi nanti.</td>
    </tr>
  </tbody>
</table>

Jika Anda tidak merespons, hubungan antara aksi dan reaksi akan rusak. Pengguna akan merasakannya.

## Animasi: hasilkan bingkai dalam waktu 10 md

Meskipun sepertinya tindakan pengguna direspons dengan segera, itu tidak selalu benar. Gunakan rentang 100 md ini untuk melakukan pekerjaan mahal lainnya, namun berhati-hatilah agar tidak memblokir pengguna. Jika memungkinkan, lakukan pekerjaan di latar belakang.

Untuk tindakan yang membutuhkan waktu lebih dari 500 md, selalu sediakan masukan.

* Process user input events within 50ms to ensure a visible response within 100ms, otherwise the connection between action and reaction is broken. This applies to most inputs, such as clicking buttons, toggling form controls, or starting animations. This does not apply to touch drags or scrolls.
* Though it may sound counterintuitive, it's not always the right call to respond to user input immediately. You can use this 100ms window to do other expensive work. But be careful not to block the user. If possible, do work in the background.
* For actions that take longer than 50ms to complete, always provide feedback.

**50ms or 100ms?:**

Pengguna akan merasa bila laju bingkai animasi bervariasi. Sasaran Anda adalah menghasilkan 60 bingkai per detik, dan setiap bingkai harus melalui semua langkah ini:

<figure>
  <img src="images/rail-response-details.png"
    alt="Diagram showing how input received during an idle task is queued,
         reducing available input processing time to 50ms."/>
  <figcaption>
    <b>Figure 2</b>. How idle tasks affect input response budget.
  </figcaption>
</figure>

## Diam: maksimalkan waktu diam

**Goals**:

* Produce each frame in an animation in 10ms or less. Technically, the maximum budget for each frame is 16ms (1000ms / 60 frames per second ≈ 16ms), but browsers need about 6ms to render each frame, hence the guideline of 10ms per frame.
* Aim for visual smoothness. Users notice when frame rates vary.

Dari sudut pandang matematika murni, setiap bingkai dianggarkan sekitar 16 md (1000 md / 60 bingkai per detik = 16,66 md per bingkai). Akan tetapi, karena browser membutuhkan waktu untuk menggambar bingkai baru ke layar, **kode Anda harus selesai dieksekusi dalam waktu kurang dari 10 md**.

* In high pressure points like animations, the key is to do nothing where you can, and the absolute minimum where you can't. Whenever possible, make use of the 100ms response to pre-calculate expensive work so that you maximize your chances of hitting 60fps.
* See [Rendering Performance](/web/fundamentals/performance/rendering/) for various animation optimization strategies.
* Recognize all the types of animations. Animations aren't just fancy UI effects. Each of these interactions are considered animations: 
    * Visual animations, such as entrances and exits, [tweens](https://www.webopedia.com/TERM/T/tweening.html), and loading indicators.
    * Scrolling. This includes flinging, which is when the user starts scrolling, then lets go, and the page continues scrolling.
    * Dragging. Animations often follow user interactions, such as panning a map or pinching to zoom.

## Muat: sajikan materi dalam waktu kurang dari 1000 md

Di titik tekanan tinggi seperti animasi, kuncinya adalah tidak melakukan apa pun sebisa mungkin, dan jika tidak bisa, maka harus minimum. Bila memungkinkan, manfaatkan respons 100 md untuk menghitung pekerjaan mahal lebih awal sehingga Anda bisa memaksimalkan peluang mencapai 60 fps.

Untuk informasi selengkapnya, lihat [Kinerja Rendering](/web/fundamentals/performance/rendering/).

* Use idle time to complete deferred work. For example, for the initial page load, load as little data as possible, then use idle time to load the rest.
* Perform work during idle time in 50ms or less. Any longer, and you risk interfering with the app's ability to respond to user input within 50ms.
* If a user interacts with a page during idle time work, the user interaction should always take the highest priority and interrupt the idle time work.

## Rangkuman metrik RAIL utama

Gunakan waktu diam untuk menyelesaikan tugas yang ditangguhkan. Misalnya, minimumkan pramuat data sehingga aplikasi Anda dimuat dengan cepat, dan gunakan waktu diam untuk memuat sisa data.

Pekerjaan yang ditangguhkan harus dikelompokkan ke dalam beberapa blok sekitar 50 md. Jika pengguna mulai berinteraksi, prioritas tertinggi adalah meresponsnya.

* Optimize for fast loading performance relative to the device and network capabilities that your users use to access your site. Currently, a good target for first loads is to load the page and be [interactive](/web/tools/lighthouse/audits/time-to-interactive) in 5 seconds or less on mid-range mobile devices with slow 3G connections. See [Can You Afford It? Real-World Web Performance Budgets](https://infrequently.org/2017/10/can-you-afford-it-real-world-web-performance-budgets/). But be aware that these targets may change over time.
* For subsequent loads, a good target is to load the page in under 2 seconds. But this target may also change over time.

<figure>
  <img src="images/speed-metrics.png"
    alt="Each loading metric (First Paint, First Contentful Paint, First Meaningful Paint, Time
         To Interactive) represents a different phase of the user's perception of the loading
         experience"/>
  <figcaption>
    <b>Figure 3</b>. Each loading metric represents a different phase of the user's perception of
    the loading experience
  </figcaption>
</figure>

Untuk mencapai respons <100 md, aplikasi harus menyerahkan kontrol kembali ke thread utama setiap <50 md, sehingga bisa mengeksekusi saluran pikselnya, menanggapi masukan pengguna,

* Test your load performance on the mobile devices and network connections that are common among your users. If your business has information on what devices and network connections your users are on, then you can use that combination and set your own loading performance targets. Otherwise, [The Mobile Economy 2017](https://www.gsma.com/mobileeconomy/) suggests that a good global baseline is a mid-range Android phone, such as a Moto G4, and a slow 3G network, defined as 400ms RTT and 400kbps transfer speed. This combination is available on [WebPageTest](https://www.webpagetest.org/easy).
* Keep in mind that although your typical mobile user's device might claim that it's on a 2G, 3G, or 4G connection, in reality the *effective connection speed* is often significantly slower, due to packet loss and network variance.
* Focus on optimizing the [Critical Rendering Path](/web/fundamentals/performance/critical-rendering-path/) to unblock rendering.
* You don't have to load everything in under 5 seconds to produce the perception of a complete load. Enable progressive rendering and do some work in the background. Defer non-essential loads to periods of idle time. See [Website Performance Optimization](https://www.udacity.com/course/website-performance-optimization--ud884).
* Recognize the factors that affect page load performance: 
    * Network speed and latency
    * Hardware (slower CPUs, for example)
    * Cache eviction
    * Differences in L2/L3 caching
    * Parsing JavaScript

## Tools for measuring RAIL {: #tools }

Dengan mengerjakan blok 50 md akan memungkinkan tugas diselesaikan sekaligus memastikan respons seketika.

* [**Chrome DevTools**](#devtools). The developer tools built into Google Chrome. Provides in-depth analysis on everything that happens while your page loads or runs.
* [**Lighthouse**](#lighthouse). Available in Chrome DevTools, as a Chrome Extension, as a Node.js module, and within WebPageTest. You give it a URL, it simulates a mid-range device with a slow 3G connection, runs a series of audits on the page, and then gives you a report on load performance, as well as suggestions on how to improve. Also provides audits to improve accessibility, make the page easier to maintain, qualify as a Progressive Web App, and more.
* [**WebPageTest**](#webpagetest). Available at [webpagetest.org/easy](https://webpagetest.org/easy). You give it a URL, it loads the page on a real Moto G4 device with a slow 3G connection, and then gives you a detailed report on the page's load performance. You can also configure it to include a Lighthouse audit.

Muat situs Anda dalam waktu kurang dari 1 detik. Jika tidak, perhatian pengguna akan teralih, dan persepsi mereka terhadap tugas akan terganggu.

### TL;DR {: .hide-from-toc }

Fokus pada [mengoptimalkan jalur rendering penting](/web/fundamentals/performance/critical-rendering-path/) untuk membuka blokir rendering.

Anda tidak harus memuat semuanya dalam waktu kurang dari 1 detik untuk menghasilkan persepsi pemuatan lengkap. Aktifkan rendering progresif dan selesaikan sebagian tugas di latar belakang. Tangguhkan pemuatan yang tidak penting ke jangka waktu diam (lihat [kursus Udacity, Optimalisasi Kinerja Situs Web](https://www.udacity.com/course/website-performance-optimization--ud884) untuk informasi selengkapnya).

* [Throttle your CPU](/web/tools/chrome-devtools/evaluate-performance/reference#cpu-throttle) to simulate a less-powerful device.
* [Throttle the network](/web/tools/chrome-devtools/evaluate-performance/reference#network-throttle) to simulate slower connections.
* [View main thread activity](/web/tools/chrome-devtools/evaluate-performance/reference#main) to view every event that occurred on the main thread while you were recording.
* [View main thread activities in a table](/web/tools/chrome-devtools/evaluate-performance/reference#activities) to sort activities based on which ones took up the most time.
* [Analyze frames per second (FPS)](/web/tools/chrome-devtools/evaluate-performance/reference#fps) to measure whether your animations truly run smoothly.
* [Monitor CPU usage, JS heap size, DOM nodes, layouts per second, and more](/web/updates/2017/11/devtools-release-notes#perf-monitor) in real-time with the **Performance Monitor**.
* [Visualize network requests](/web/tools/chrome-devtools/evaluate-performance/reference#network) that occurred while you were recording with the **Network** section.
* [Capture screenshots while recording](/web/tools/chrome-devtools/evaluate-performance/reference#screenshots) to play back exactly how the page looked while the page loaded, or an animation fired, and so on.
* [View interactions](/web/tools/chrome-devtools/evaluate-performance/reference#interactions) to quickly identify what happened on a page after a user interacted with it.
* [Find scroll performance issues in real-time](/web/tools/chrome-devtools/evaluate-performance/reference#scrolling-performance-issues) by highlighting the page whenever a potentially problematic listener fires.
* [View paint events in real-time](/web/tools/chrome-devtools/evaluate-performance/reference#paint-flashing) to identify costly paint events that may be harming the performance of your animations.

### Lighthouse {: #lighthouse }

Untuk mengevaluasi situs Anda dengan metrik RAIL, gunakan [alat (bantu) Timeline](/web/tools/chrome-devtools/profile/evaluate-performance/timeline-tool) Chrome DevTools untuk merekam tindakan pengguna. Kemudian periksa waktu rekaman di Timeline dengan metrik RAIL utama ini:

<figure>
  <img src="images/lighthouse-performance.jpg"
    alt="An example Lighthouse report"/>
  <figcaption>
    <b>Figure 4</b>. An example Lighthouse report
  </figcaption>
</figure>

The following audits are especially relevant:

* **Response** 
    * [Estimated Input Latency](/web/tools/lighthouse/audits/estimated-input-latency). Estimates how long your app will take to respond to user input, based on main thread idle time.
    * [Uses Passive Event Listeners To Improve Scrolling](/web/tools/lighthouse/audits/passive-event-listeners).
* **Load** 
    * [Registers A Service Worker](/web/tools/lighthouse/audits/registered-service-worker). A service worker can cache common resources on a user's device, reducing time spent fetching resources over the network.
    * [Page Load Is Fast Enough On 3G](/web/tools/lighthouse/audits/fast-3g).
    * [First Meaningful Paint](/web/tools/lighthouse/audits/first-meaningful-paint). Measures when the page appears meaningfully complete.
    * [First CPU Idle](/web/tools/lighthouse/audits/first-interactive). Marks the first time at which the page's main thread is quiet enough to handle input.
    * [Time To Interactive](/web/tools/lighthouse/audits/consistently-interactive). Measures when a user can consistently interact with all page elements.
    * [Perceptual Speed Index](/web/tools/lighthouse/audits/speed-index).
    * [Reduce Render-Blocking Resources](/web/tools/lighthouse/audits/blocking-resources).
    * [Offscreen Images](/web/tools/lighthouse/audits/offscreen-images). Defer the loading of offscreen images until they're needed.
    * [Properly Size Images](/web/tools/lighthouse/audits/oversized-images). Don't serve images that are significantly larger than the size that's rendered in the mobile viewport.
    * [Critical Request Chains](/web/tools/lighthouse/audits/critical-request-chains). Visualize your [Critical Rendering Path](/web/fundamentals/performance/critical-rendering-path/).
    * [Uses HTTP/2](/web/tools/lighthouse/audits/http2).
    * [Optimize Images](/web/tools/lighthouse/audits/optimize-images).
    * [Enable Text Compression](/web/tools/lighthouse/audits/text-compression).
    * [Avoid Enormous Network Payloads](/web/tools/lighthouse/audits/network-payloads).
    * [Uses An Excessive DOM Size](/web/tools/lighthouse/audits/dom-size). Reduce network bytes by only shipping DOM nodes that are needed for rendering the page.

### WebPageTest {: #webpagetest }

{# wf_devsite_translation #}

<figure>
  <img src="images/wpt-report.png"
    alt="An example WebPageTest report"/>
  <figcaption>
    <b>Figure 5</b>. An example WebPageTest report
  </figcaption>
</figure>

## Summary {: #summary }

RAIL is a lens for looking at a website's user experience as a journey composed of distinct interactions. Understand how users perceive your site in order to set performance goals with the greatest impact on user experience.

* **Focus on the user**.
* **Respond to user input in under 100ms**.
* **Produce a frame in under 10ms when animating or scrolling**.
* **Maximize main thread idle time**.
* **Load interactive content in under 5000ms**.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}