project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Obtén la última información sobre el estado de WebVR y sobre qué tener en cuenta a la hora de crear experiencias con WebVR.

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-12-12 #}

# Estado y consideraciones de WebVR {: .page-title }

## Estado de implementación de WebVR

### WebXR Device API {:#xrdevice}

* Chrome Beta (M56+), a través de un [Origin Trial](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md).
* Firefox Nightly.
* Samsung Internet Browser for Gear VR. (Ten en cuenta que actualmente es compatible con una versión antigua de la especificación de WebVR).

Warning: WebVR todavía es experimental y se encuentra sujeta a modificaciones.

| Feature             | Chrome version                          | Details                                                                                                                                                                                                                   |
| ------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AR hit test support | Chrome Canary for the immediate future. | Enable the `#webxr` and `#webxr-hit-test` flags under chrome://flags. Note that VR magic windows do not work when the `#webxr-hit-test` flag is turned on. Please excuse our construction debri.                          |
| VR use cases        | Chrome 66 and later                     | Enable the `chrome://flags/#webxr` flag. (The URL must be entered manually.).                                                                                                                                             |
| VR use cases        | Chrome 67 origin trial                  | Enable the `chrome://flags/#webxr` flag *and* sign up for the origin trial ([explainer](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md), [sign-up form](http://bit.ly/OriginTrialSignup)). |

Actualmente, la WebVR API se encuentra disponible en:

### Version 1.1 {:#version_1_1}

Puedes encontrar más información sobre el estado de implementación en navegadores en [chromestatus.com](https://www.chromestatus.com/features/4532810371039232?embed).

A continuación, encontrarás algunos aspectos que debes recordar si creas experiencias con WebVR hoy.

* Firefox Nightly.
* **Actualmente, Chrome solo admite WebVR nativa en Android.** Debes usar un dispositivo Daydream con un teléfono Pixel.
* **Puede ser que [WebVR Polyfill](https://github.com/googlevr/webvr-polyfill) no siempre coincida exactamente con las implementaciones nativas de las especificaciones.** Si planeas usar Polyfill, asegúrate de probar tanto en dispositivos con capacidad para RV como en otros.

{# wf_devsite_translation #}

* Daydream View since M56
* Google Cardboard since M57

It's also available through the [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill). <iframe width="100%" height="320"
  src="https://www.chromestatus.com/feature/4532810371039232?embed"
  style="border: 1px solid #CCC" allowfullscreen mark="crwd-mark">
</iframe> 

Find more information on browser implementation status on [chromestatus.com](https://www.chromestatus.com/features/4532810371039232).

## Consideraciones

Here are things to remember when building WebVR experiences today.

* **You must serve your WebVR content over HTTPS.** If you don’t your users will get warnings from the browser. See [Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https) for more guidance.
* **The [WebXR Polyfill](https://github.com/immersive-web/webxr-polyfill) may not always be a 1:1 match with native implementations of the spec.** If you plan to use the Polyfill, be sure to check on both VR-capable and non-VR devices.
* **For some types of sessions, users must click a button before AR or VR are available to your code**. See the [Immersive Web Early Adopters Guide](https://immersive-web.github.io/webxr-reference/) for more information.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}