project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: La seguridad es un tema extenso; aprende sobre HTTPS, por qué es importante y cómo puedes implementarlo en tus servidores.

{# wf_updated_on: 2016-09-09 #} {# wf_published_on: 2015-09-08 #}

# Seguridad e identidad {: .page-title }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="pgBQn_z3zRE"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

La seguridad es un tema extenso; aquí tienes algo de información para comenzar.

<div class="clearfix"></div>

## Encriptación de datos en tránsito

<img src="/web/images/content-https-2x.jpg" class="attempt-right" />

Una de las características más importantes de la seguridad, que muchas API modernas y [progressive web apps](/web/progressive-web-apps/) requieren es [Secure HTTP, también llamado HTTPS](encrypt-in-transit/why-https). Un concepto equivocado que suele tenerse sobre HTTPS es que los únicos sitios web que lo necesitan son aquellos que manejan comunicaciones delicadas. Como si la privacidad y la seguridad no fueran razones suficientes para proteger a tus usuarios, muchas características nuevas de los navegadores, como los service worker o la Payment Request API requieren HTTPS.

Some people mistakenly believe that the only sites that need HTTPS are sites that handle some level of sensitive communication, like personal or financial data. But this isn't true. Every site should be using HTTPS, HTTPS helps to prevents people from listening into what's crossing the wire, and helps prevent it from being tampered with while in transit. Do you want your ISP or school to know every site you were looking at?

{# wf_devsite_translation #}

[Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https)

<div class="attempt-left">
  <h2>
    Política de seguridad de contenido
  </h2>
  
  <p>
    La Política de seguridad de contenido o CSP proporciona un amplio grupo de directivas que habilitan el control granular para configurar qué recursos se le permite cargar a una página y de dónde se cargan.<br /> <a href="csp/">Más información</a>
  </p>
</div>

<div class="attempt-right">
  <h2>
    Evita el contenido mixto
  </h2>
  
  <p>
    Una de las tareas que más tiempo lleva en cuanto a la implementación de HTTPS es encontrar y reparar contenido que mezcle HTTPS y HTTP. Afortunadamente, existen herramientas para ayudarte con esto.<br /> <a href="prevent-mixed-content/what-is-mixed-content">Primeros pasos</a>
  </p>
</div>

<div style="clear:both"></div>

## Recursos relacionados

* [Comprende los problemas de seguridad](https://www.youtube.com/watch?v=tgEIo7ZSkbQ)
* [Getting the Green Lock: HTTPS Stories from the Field](https://www.youtube.com/watch?v=GoXgl9r0Kjk)

### Chrome DevTools

* [Understand Security Issues](/web/tools/chrome-devtools/security)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}