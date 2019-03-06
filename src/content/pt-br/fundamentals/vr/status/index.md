project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Fique por dentro das últimas informações sobre o status da WebVR, além dos pontos importantes da criação de experiências da WebVR.

{# wf_updated_on: 2016-12-12 #} {# wf_published_on: 2016-12-12 #}

# Status e considerações sobre a WebVR {: .page-title }

## Status de implementação da WebVR

### WebXR Device API {:#xrdevice}

* Chrome Beta (M56+) com um [Teste na origem](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md).
* Firefox Nightly.
* Navegador Samsung Internet do Gear VR (observação: compatível com uma versão mais antiga da especificação da WebVR).

Aviso: a WebVR ainda é experimental e, por isso, está sujeita a mudanças.

| Feature             | Chrome version                          | Details                                                                                                                                                                                                                   |
| ------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AR hit test support | Chrome Canary for the immediate future. | Enable the `#webxr` and `#webxr-hit-test` flags under chrome://flags. Note that VR magic windows do not work when the `#webxr-hit-test` flag is turned on. Please excuse our construction debri.                          |
| VR use cases        | Chrome 66 and later                     | Enable the `chrome://flags/#webxr` flag. (The URL must be entered manually.).                                                                                                                                             |
| VR use cases        | Chrome 67 origin trial                  | Enable the `chrome://flags/#webxr` flag *and* sign up for the origin trial ([explainer](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md), [sign-up form](http://bit.ly/OriginTrialSignup)). |

Hoje, a WebVR API está disponível no:

### Version 1.1 {:#version_1_1}

Veja mais informações sobre o status de implementação em navegadores em [chromestatus.com](https://www.chromestatus.com/features/4532810371039232?embed).

Veja alguns pontos importantes da criação de experiências da WebVR atualmente.

* Firefox Nightly.
* **Atualmente, o Chrome só é compatível com WebVR nativa no Android.** Você deve usar um fone de ouvido Daydream com um celular Pixel.
* **O [Polyfill da WebVR](https://github.com/googlevr/webvr-polyfill) nem sempre terá implementação exatamente igual às nativas da especificação.** Se você planejar usar o Polyfill, verifique-o em dispositivos compatíveis e não compatíveis com RV.

{# wf_devsite_translation #}

* Daydream View since M56
* Google Cardboard since M57

It's also available through the [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill). <iframe width="100%" height="320"
  src="https://www.chromestatus.com/feature/4532810371039232?embed"
  style="border: 1px solid #CCC" allowfullscreen mark="crwd-mark">
</iframe> 

Find more information on browser implementation status on [chromestatus.com](https://www.chromestatus.com/features/4532810371039232).

## Considerações

Here are things to remember when building WebVR experiences today.

* **You must serve your WebVR content over HTTPS.** If you don’t your users will get warnings from the browser. See [Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https) for more guidance.
* **The [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill) may not always be a 1:1 match with native implementations of the spec.** If you plan to use the Polyfill, be sure to check on both VR-capable and non-VR devices.
* **For some types of sessions, users must click a button before AR or VR are available to your code**. See the [Immersive Web Early Adopters Guide](https://immersive-web.github.io/webxr-reference/) for more information.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}