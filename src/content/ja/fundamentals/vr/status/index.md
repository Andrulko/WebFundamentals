project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: WebVR のステータスに関する最新情報と WebVR エクスペリエンスを構築するときの考慮事項について説明します。

{# wf_updated_on:2016-12-12 #} {# wf_published_on:2016-12-12 #}

# WebVR のステータスと考慮事項 {: .page-title }

## WebVR の実装ステータス

### WebXR Device API {:#xrdevice}

* Chrome ベータ版（M56+）（[オリジン トライアル](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md)によって）
* Firefox Nightly
* Samsung Internet Browser for Gear VR（注: 現在、このブラウザは古いバージョンの WebVR 仕様をサポートしています）

警告:WebVR はまだ試験運用版であり、仕様変更の可能性があります。

| Feature             | Chrome version                          | Details                                                                                                                                                                                                                   |
| ------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AR hit test support | Chrome Canary for the immediate future. | Enable the `#webxr` and `#webxr-hit-test` flags under chrome://flags. Note that VR magic windows do not work when the `#webxr-hit-test` flag is turned on. Please excuse our construction debri.                          |
| VR use cases        | Chrome 66 and later                     | Enable the `chrome://flags/#webxr` flag. (The URL must be entered manually.).                                                                                                                                             |
| VR use cases        | Chrome 67 origin trial                  | Enable the `chrome://flags/#webxr` flag *and* sign up for the origin trial ([explainer](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md), [sign-up form](http://bit.ly/OriginTrialSignup)). |

現在、WebVR API は次のブラウザで利用可能です。

### Version 1.1 {:#version_1_1}

ブラウザの実装ステータスの詳細については、[chromestatus.com](https://www.chromestatus.com/features/4532810371039232?embed) をご覧ください。

WebVR エクスペリエンスを構築するときの考慮事項を次に示します。

* Firefox Nightly.
* **現在、Chrome では Android 上のネイティブ WebVR のみをサポートしています**。Daydream ヘッドセットと Pixel スマートフォンを使用する必要があります。
* **[WebVR Polyfill](https://github.com/googlevr/webvr-polyfill) は、仕様のネイティブ実装と完全に対応してるわけではありません**。Polyfill を使用する予定がある場合は、VR 対応端末と非 VR 端末の両方でチェックする必要があります。

{# wf_devsite_translation #}

* Daydream View since M56
* Google Cardboard since M57

It's also available through the [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill). <iframe width="100%" height="320"
  src="https://www.chromestatus.com/feature/4532810371039232?embed"
  style="border: 1px solid #CCC" allowfullscreen mark="crwd-mark">
</iframe> 

Find more information on browser implementation status on [chromestatus.com](https://www.chromestatus.com/features/4532810371039232).

## 考慮事項

Here are things to remember when building WebVR experiences today.

* **You must serve your WebVR content over HTTPS.** If you don’t your users will get warnings from the browser. See [Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https) for more guidance.
* **The [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill) may not always be a 1:1 match with native implementations of the spec.** If you plan to use the Polyfill, be sure to check on both VR-capable and non-VR devices.
* **For some types of sessions, users must click a button before AR or VR are available to your code**. See the [Immersive Web Early Adopters Guide](https://immersive-web.github.io/webxr-reference/) for more information.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}