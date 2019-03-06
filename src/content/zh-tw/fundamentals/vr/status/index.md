project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description:獲取有關 WebVR 狀態的最新信息，以及在構建 WebVR 體驗時需要注意的事項。

{# wf_updated_on:2016-12-12 #} {# wf_published_on:2016-12-12 #}

# WebVR 狀態和注意事項 {: .page-title }

## WebVR 實現狀態

### WebXR Device API {:#xrdevice}

* Chrome Beta (M56+)，通過一個[來源試用版](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md)實現。
* Firefox Nightly。
* Samsung Internet Browser for Gear VR。（請Note: 此瀏覽器目前支持一個較早版本的 WebVR 規範）。

Warning: WebVR 仍處於實驗階段，並且隨時可能更改。

| Feature             | Chrome version                          | Details                                                                                                                                                                                                                   |
| ------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AR hit test support | Chrome Canary for the immediate future. | Enable the `#webxr` and `#webxr-hit-test` flags under chrome://flags. Note that VR magic windows do not work when the `#webxr-hit-test` flag is turned on. Please excuse our construction debri.                          |
| VR use cases        | Chrome 66 and later                     | Enable the `chrome://flags/#webxr` flag. (The URL must be entered manually.).                                                                                                                                             |
| VR use cases        | Chrome 67 origin trial                  | Enable the `chrome://flags/#webxr` flag *and* sign up for the origin trial ([explainer](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md), [sign-up form](http://bit.ly/OriginTrialSignup)). |

目前，WebVR API 可用於以下瀏覽器：

### Version 1.1 {:#version_1_1}

如需瞭解有關瀏覽器實現狀態的更多信息，請訪問 [chromestatus.com](https://www.chromestatus.com/features/4532810371039232?embed)。

以下是目前構建 WebVR 體驗時需要注意的事項。

* Firefox Nightly.
* **Chrome 目前僅在 Android 上支持原生 WebVR。** 您必須使用一個 Daydream 耳機和一部 Pixel 手機。
* **[WebVR Polyfill](https://github.com/googlevr/webvr-polyfill) 可能不會始終與規範的原生實現一一對應。** 如果您計劃使用 Polyfill，請務必在 VR 設備和非 VR 設備上都進行檢查。

{# wf_devsite_translation #}

* Daydream View since M56
* Google Cardboard since M57

It's also available through the [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill). <iframe width="100%" height="320"
  src="https://www.chromestatus.com/feature/4532810371039232?embed"
  style="border: 1px solid #CCC" allowfullscreen mark="crwd-mark">
</iframe> 

Find more information on browser implementation status on [chromestatus.com](https://www.chromestatus.com/features/4532810371039232).

## 注意事項

Here are things to remember when building WebVR experiences today.

* **You must serve your WebVR content over HTTPS.** If you don’t your users will get warnings from the browser. See [Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https) for more guidance.
* **The [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill) may not always be a 1:1 match with native implementations of the spec.** If you plan to use the Polyfill, be sure to check on both VR-capable and non-VR devices.
* **For some types of sessions, users must click a button before AR or VR are available to your code**. See the [Immersive Web Early Adopters Guide](https://immersive-web.github.io/webxr-reference/) for more information.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}