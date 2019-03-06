project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description:获取有关 WebVR 状态的最新信息，以及在构建 WebVR 体验时需要注意的事项。

{# wf_updated_on:2016-12-12 #} {# wf_published_on:2016-12-12 #}

# WebVR 状态和注意事项 {: .page-title }

## WebVR 实现状态

### WebXR Device API {:#xrdevice}

* Chrome Beta (M56+)，通过一个[来源试用版](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md)实现。
* Firefox Nightly。
* Samsung Internet Browser for Gear VR。（请Note: 此浏览器目前支持一个较早版本的 WebVR 规范）。

Warning: WebVR 仍处于实验阶段，并且随时可能更改。

| Feature             | Chrome version                          | Details                                                                                                                                                                                                                   |
| ------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AR hit test support | Chrome Canary for the immediate future. | Enable the `#webxr` and `#webxr-hit-test` flags under chrome://flags. Note that VR magic windows do not work when the `#webxr-hit-test` flag is turned on. Please excuse our construction debri.                          |
| VR use cases        | Chrome 66 and later                     | Enable the `chrome://flags/#webxr` flag. (The URL must be entered manually.).                                                                                                                                             |
| VR use cases        | Chrome 67 origin trial                  | Enable the `chrome://flags/#webxr` flag *and* sign up for the origin trial ([explainer](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md), [sign-up form](http://bit.ly/OriginTrialSignup)). |

目前，WebVR API 可用于以下浏览器：

### Version 1.1 {:#version_1_1}

如需了解有关浏览器实现状态的更多信息，请访问 [chromestatus.com](https://www.chromestatus.com/features/4532810371039232?embed)。

以下是目前构建 WebVR 体验时需要注意的事项。

* Firefox Nightly.
* **Chrome 目前仅在 Android 上支持原生 WebVR。** 您必须使用一个 Daydream 耳机和一部 Pixel 手机。
* **[WebVR Polyfill](https://github.com/googlevr/webvr-polyfill) 可能不会始终与规范的原生实现一一对应。** 如果您计划使用 Polyfill，请务必在 VR 设备和非 VR 设备上都进行检查。

{# wf_devsite_translation #}

* Daydream View since M56
* Google Cardboard since M57

It's also available through the [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill). <iframe width="100%" height="320"
  src="https://www.chromestatus.com/feature/4532810371039232?embed"
  style="border: 1px solid #CCC" allowfullscreen mark="crwd-mark">
</iframe> 

Find more information on browser implementation status on [chromestatus.com](https://www.chromestatus.com/features/4532810371039232).

## 注意事项

Here are things to remember when building WebVR experiences today.

* **You must serve your WebVR content over HTTPS.** If you don’t your users will get warnings from the browser. See [Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https) for more guidance.
* **The [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill) may not always be a 1:1 match with native implementations of the spec.** If you plan to use the Polyfill, be sure to check on both VR-capable and non-VR devices.
* **For some types of sessions, users must click a button before AR or VR are available to your code**. See the [Immersive Web Early Adopters Guide](https://immersive-web.github.io/webxr-reference/) for more information.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}