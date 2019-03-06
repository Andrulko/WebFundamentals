project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: WebVR

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-12-12 #}

# WebVR {: .page-title }

Warning: WebVR todavía es experimental y se encuentra sujeta a modificaciones.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="jT2mR9WzJ7Y"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

WebVR es una JavaScript API que utiliza las gafas de RV y un dispositivo con capacidad para RV que tengan tus usuarios, como unas [gafas Daydream](https://vr.google.com/daydream/) y un teléfono Pixel, para crear experiencias 3D completamente envolventes en el navegador.

<div class="clearfix"></div>

## Compatibilidad y disponibilidad

Today the WebXR Device API is [under development](https://www.chromestatus.com/features/5680169905815552), but you can try it out with:

* Chrome Beta (M56+), a través de un [Origin Trial](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md).
* Firefox Nightly.

Actualmente, la WebVR API se encuentra disponible en:

Para los navegadores que no admiten WebVR, o que tal vez tengan versiones anteriores de las API, puedes recurrir a [WebVR Polyfill](https://github.com/googlevr/webvr-polyfill). Sin embargo, ten en cuenta que la RV es *extremadamente sensible al rendimiento* y los polyfills normalmente suponen un costo de rendimiento relativamente elevado. Por lo tanto, tal vez debas considerar si realmente quieres usar el polyfill para un usuario que no tiene compatibilidad nativa para WebVR.

[Get the latest status on WebVR.](./status/)

## Creación de contenido de WebVR

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

### Más recursos

Para crear contenido de WebVR, necesitarás usar algunas API nuevas y tecnologías existentes, como [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/Tutorial) y [WebAudio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API), además de abarcar distintos tipos de entradas y gafas.

* [Conoce más sobre las WebVR API](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API)
* [Consulta los ejemplos de WebVR](https://webvr.info/samples/)
* [Diseñar para Google Cardboard](https://www.google.com/design/spec-vr/designing-for-google-cardboard/a-new-dimension.html)

## Lleva registro de tu rendimiento

<img src="img/oce.png" class="attempt-right" alt="WebVR Performance" />

In order to minimize discomfort for the people using WebVR experiences, they must maintain a consistent (and high) frame rate. Failing to do so can give users motion sickness!

Para minimizar el malestar para la gente que usa las experiencias de WebVR, deben mantener un índice de fotogramas constante (y alto). Si se falla en esto, ¡los usuarios pueden sufrir mareos por movimiento!

En dispositivos móviles, la frecuencia de actualización es normalmente de 60 Hz, lo que significa que el objetivo es 60 fps (o 16 ms por fotograma *incluida* la sobrecarga por fotograma del navegador). En escritorio, el objetivo es normalmente 90 Hz (11 ms incluida la sobrecarga).

## Adopta la mejora progresiva

<img src="img/touch-input.png" class="attempt-right"
  alt="Use Progressive Enhancement to maximize reach" />

What are you to do if your users don’t have a Head Mounted Display (‘HMD’) or VR-capable device? The best answer is to use Progressive Enhancement.

1. Debes suponer que el usuario usa entradas tradicionales, como teclado, mouse o pantalla táctil sin acceso a gafas de RV.
2. Debes poder adaptarte a los cambios en la disponibilidad de entradas y gafas durante el tiempo de ejecución.

¿Qué puedes hacer si tus usuarios no cuentan con gafas de realidad ("HMD") u otro dispositivo con capacidad para RV? La mejor respuesta es usar mejora progresiva.

Afortunadamente las [WebVR API](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API) hacen posible detectar cambios en el entorno de RV para que podamos descubrir y adaptarnos a los cambios en las opciones de entrada y visualización del dispositivo del usuario.

Al considerar un entorno que no sea de RV, en primer lugar puedes maximizar el alcance de tus experiencias y asegurarte de que ofreces la mejor experiencia posible sin importar con qué equipo cuenten tus usuarios.

## Feedback {: #feedback }

Para obtener más información, lee nuestra guía sobre [agregar entrada a una escena de WebVR](./adding-input-to-a-webvr-scene/).