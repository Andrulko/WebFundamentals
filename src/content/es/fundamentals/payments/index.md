project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: La Payment Request API es para pagos rápidos y fáciles en la web.

{# wf_published_on: 2016-07-25 #} {# wf_updated_on: 2017-07-12 #}

# Payment Request API: una guía de integración {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/zkoch.html" %}

<div class="video-wrapper-full-width">
  <iframe class="devsite-embedded-youtube-video" data-video-id="colCcgKoLUM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

## Introducción de la Payment Request API {: #introducing }

Prueba interna: `PaymentRequest` está aún en desarrollo. Mientras creemos que es lo suficientemente estable para implementarse, puede seguir cambiando. Mantendremos esta página actualizada para que siempre muestre el estado actual de la API ([cambios M56](https://docs.google.com/document/d/1I8ha1ySrPWhx80EB4CVPmThkD4ILFM017AfOA5gEFg4/edit#)). Mientras tanto, te protegemos de los cambios de la API que puedan ser incompatibles con versiones anteriores, ofrecemos [una corrección de compatibilidad](https://storage.googleapis.com/prshim/v1/payment-shim.js) que se puede integrar a tu sitio. La corrección de compatibilidad oculta cualquier diferencia de la API de dos versiones principales de Chrome.

Comprar productos en línea es una experiencia conveniente, pero a menudo puede ser frustrante, en especial desde dispositivos móviles. Si bien el tráfico móvil continúa en aumento, las conversiones móviles representan solo un tercio de todas las compras realizadas. En otras palabras, los usuarios abandonan las compras desde dispositivos móviles con el doble de frecuencia en comparación con las compras desde equipos de escritorio. ¿Por qué?

**For consumers**, they simplify checkout flow, by making it a few taps instead of typing small characters many times on a virtual keyboard.

**For merchants**, they make it easier to implement with a variety of payment options already filtered for the customer.

Los formularios de compra en línea requieren una amplia intervención por parte del usuario, son difíciles de usar, se cargan y actualizan con lentitud, y requieren varios pasos para completar la compra. Esto se debe a que dos componentes principales de los pagos en línea, seguridad y conveniencia, a menudo trabajan con diferentes propósitos; por lo general, más de uno significa menos del otro.

La mayoría de los problemas que llevan al abandono se pueden asociar directamente con los formularios de compra. Cada app o sitio tiene su propio proceso de entrada de datos y validación, y los usuarios a menudo descubren que tienen que ingresar la misma información en cada punto de compra de la app. Asimismo, los desarrolladores de apps se esfuerzan por crear flujos de compra que admitan varios métodos de compra exclusivos; incluso las diferencias más pequeñas en los requisitos de los métodos de pago pueden complicar el proceso de llenado y envío de formularios.

## Uso de la PaymentRequest API {: #using }

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/standard-open.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Standard and Open</h3>
    Web Payments are an open payment standard for the web platform for the first time
    in history. They are available for any players to implement.</div>
</section>

<section style="display:flex;padding-bottom:32px;">
  <div style="min-width:50%">
    <h3>Easy and Consistent</h3>
    Web Payments make checkout easy for the user, by reusing stored 
payments and address information and removing the need for the user to fill in checkout forms. 
Since the UI is implemented by the browser natively, users see a familiar and consistent checkout 
experience on any website that makes use of the standard.</div>
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/easy-consistent.png" width="100%" alt="Standard and Open" title="">
  </div>
</section>

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/secure-flexible.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Secure and Flexible</h3>
    Web Payments provide industry-leading payment technology to the 
web, and can easily integrate a secure payment solution.</div>
</section>

## Recolectar una dirección de envío {: #shipping-address }

Cualquier sistema que mejore o resuelva uno o más de esos problemas es un cambio bienvenido. Ya hemos comenzado a solucionar el problema con [Autocompletar](/web/updates/2015/06/checkout-faster-with-autofill), pero ahora quisiéramos hablar sobre una solución más integral.

La Payment Request API es un sistema que tiene que *eliminar los formularios de salida*. Mejora notablemente el flujo de trabajo del usuario durante el proceso de compra, lo cual proporciona una experiencia del usuario más uniforme y permite a los comerciantes web usar con facilidad diferentes métodos de pago. La Payment Request API no es un nuevo método de pago ni se integra directamente con procesadores de pago; es una capa de proceso concebida con los siguientes objetivos:

La Payment Request API es un estándar abierto compatible con varios navegadores que reemplaza los flujos tradicionales de finalización de compra al permitir a los comerciantes solicitar y aceptar cualquier método de pago en una sola llamada a la API. La Payment Request API permite que la página web intercambie información con el usuario-agente mientras este último proporciona entradas, antes de aprobar o rechazar una solicitud de pago.

Lo mejor de todo, con el navegador como intermediario, toda la información necesaria para que la finalización de la compra sea rápida se puede guardar en el navegador, de modo que los usuarios solo deban confirmar y pagar con un solo clic.

Con la Payment Request API, el proceso de transacción es lo más fluido posible para usuarios y comerciantes.

## Adición de opciones de envío {: #shipping-options}

{% include "web/_shared/helpful.html" %}