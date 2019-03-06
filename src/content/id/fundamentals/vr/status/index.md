project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Dapatkan status informasi terbaru mengenai WebVR, serta hal yang perlu diingat saat membangun pengalaman WebVR.

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-12-12 #}

# Status dan Pertimbangan WebVR {: .page-title }

## Status Implementasi WebVR

### WebXR Device API {:#xrdevice}

* Chrome Beta (M56+), melalui [Origin Trial](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md).
* Firefox Nightly.
* Browser Samsung Internet untuk Gear VR. (Harap diingat: versi lama spesifikasi WebVR saat ini telah didukung.)

Caution: WebVR masih eksperimental dan dapat berubah.

| Feature             | Chrome version                          | Details                                                                                                                                                                                                                   |
| ------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AR hit test support | Chrome Canary for the immediate future. | Enable the `#webxr` and `#webxr-hit-test` flags under chrome://flags. Note that VR magic windows do not work when the `#webxr-hit-test` flag is turned on. Please excuse our construction debri.                          |
| VR use cases        | Chrome 66 and later                     | Enable the `chrome://flags/#webxr` flag. (The URL must be entered manually.).                                                                                                                                             |
| VR use cases        | Chrome 67 origin trial                  | Enable the `chrome://flags/#webxr` flag *and* sign up for the origin trial ([explainer](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md), [sign-up form](http://bit.ly/OriginTrialSignup)). |

Saat ini WebVR API tersedia di:

### Version 1.1 {:#version_1_1}

Informasi selengkapnya mengenai status implementasi browser bisa ditemukan di [chromestatus.com](https://www.chromestatus.com/features/4532810371039232?embed).

Inilah hal-hal yang harus diingat saat membangun pengalaman WebVR saat ini.

* Firefox Nightly.
* **Chrome hanya mendukung WebVR asli di Android saat ini.** Anda harus menggunakan headset Daydream bersama ponsel Pixel.
* **[WebVR Polyfill](https://github.com/googlevr/webvr-polyfill) mungkin tidak selalu cocok 1:1 dengan implementasi asli spesifikasi.** Jika Anda berencana menggunakan Polyfill, pastikan memeriksa di perangkat berkemampuan VR maupun non-VR.

{# wf_devsite_translation #}

* Daydream View since M56
* Google Cardboard since M57

It's also available through the [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill). <iframe width="100%" height="320"
  src="https://www.chromestatus.com/feature/4532810371039232?embed"
  style="border: 1px solid #CCC" allowfullscreen mark="crwd-mark">
</iframe> 

Find more information on browser implementation status on [chromestatus.com](https://www.chromestatus.com/features/4532810371039232).

## Pertimbangan

Here are things to remember when building WebVR experiences today.

* **You must serve your WebVR content over HTTPS.** If you don’t your users will get warnings from the browser. See [Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https) for more guidance.
* **The [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill) may not always be a 1:1 match with native implementations of the spec.** If you plan to use the Polyfill, be sure to check on both VR-capable and non-VR devices.
* **For some types of sessions, users must click a button before AR or VR are available to your code**. See the [Immersive Web Early Adopters Guide](https://immersive-web.github.io/webxr-reference/) for more information.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}