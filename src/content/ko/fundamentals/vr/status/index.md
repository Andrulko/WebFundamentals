project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: WebVR 경험을 빌드할 때 유의해야 할 사항뿐만 아니라 WebVR 상태에 대한 최신 정보를 제공합니다.

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-12-12 #}

# WebVR 상태 및 고려 사항 {: .page-title }

## WebVR 구현 상태

### WebXR Device API {:#xrdevice}

* [Origin Trial](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md)을 통한 Chrome Beta (M56+)
* Firefox Nightly
* Gear VR용 삼성 인터넷 브라우저 (참고: 현재 WebVR 사양의 이전 버전을 지원합니다.)

Warning: WebVR은 아직 실험 단계이며 변경될 수 있습니다.

| Feature             | Chrome version                          | Details                                                                                                                                                                                                                   |
| ------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AR hit test support | Chrome Canary for the immediate future. | Enable the `#webxr` and `#webxr-hit-test` flags under chrome://flags. Note that VR magic windows do not work when the `#webxr-hit-test` flag is turned on. Please excuse our construction debri.                          |
| VR use cases        | Chrome 66 and later                     | Enable the `chrome://flags/#webxr` flag. (The URL must be entered manually.).                                                                                                                                             |
| VR use cases        | Chrome 67 origin trial                  | Enable the `chrome://flags/#webxr` flag *and* sign up for the origin trial ([explainer](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md), [sign-up form](http://bit.ly/OriginTrialSignup)). |

현재 WebVR API는 다음 브라우저에서 사용할 수 있습니다.

### Version 1.1 {:#version_1_1}

브라우저 구현 상태에 대한 자세한 내용은 [chromestatus.com](https://www.chromestatus.com/features/4532810371039232?embed)을 참조하세요.

현재 WebVR 경험을 빌드할 때 다음 사항을 기억해야 합니다.

* Firefox Nightly.
* **Chrome은 현재 Android에서 기본 WebVR만 지원합니다.** Daydream 헤드셋을 픽셀폰과 함께 사용해야 합니다.
* **[WebVR 폴리필](https://github.com/googlevr/webvr-polyfill)은 사양의 기본 구현과 항상 일대일로 매칭되지 않을 수도 있습니다.** 폴리필을 사용하려면 VR 지원 기기와 비 VR 기기에서 모두 확인해야 합니다.

{# wf_devsite_translation #}

* Daydream View since M56
* Google Cardboard since M57

It's also available through the [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill). <iframe width="100%" height="320"
  src="https://www.chromestatus.com/feature/4532810371039232?embed"
  style="border: 1px solid #CCC" allowfullscreen mark="crwd-mark">
</iframe> 

Find more information on browser implementation status on [chromestatus.com](https://www.chromestatus.com/features/4532810371039232).

## 고려 사항

Here are things to remember when building WebVR experiences today.

* **You must serve your WebVR content over HTTPS.** If you don’t your users will get warnings from the browser. See [Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https) for more guidance.
* **The [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill) may not always be a 1:1 match with native implementations of the spec.** If you plan to use the Polyfill, be sure to check on both VR-capable and non-VR devices.
* **For some types of sessions, users must click a button before AR or VR are available to your code**. See the [Immersive Web Early Adopters Guide](https://immersive-web.github.io/webxr-reference/) for more information.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}