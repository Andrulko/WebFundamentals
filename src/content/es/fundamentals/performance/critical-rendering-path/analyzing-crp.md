project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Aprende a identificar y resolver cuellos de botella en el rendimiento de la ruta de acceso de representación crítica.

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2014-03-31 #}

# Analizar el rendimiento de la ruta de acceso de representación crítica {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Para la identificación y resolución de cuellos de botella en el rendimiento de la ruta de acceso de representación crítica se requiere un amplio conocimiento de los inconvenientes comunes. Realicemos un recorrido práctico y extraigamos patrones comunes de rendimiento que te ayudarán a optimizar tus páginas.

Optimizar la ruta de acceso de representación crítica permite que el navegador pinte la página lo más pronto posible: las páginas más rápidas proporcionan una mayor atracción, aumentan el número de páginas vistas y [mejoran la conversión](https://www.google.com/think/multiscreen/success.html). Para minimizar la cantidad de tiempo que un visitante pierde en ver una pantalla en blanco, necesitamos optimizar qué recursos se cargan y en qué orden.

Para ayudar a ilustrar este proceso, comencemos con el caso más simple posible y avancemos gradualmente en nuestra página para incluir recursos, estilos y lógica de app adicionales. En el proceso, también veremos cuándo las cosas pueden salir mal y cómo optimizar cada uno de esos casos.

Hasta ahora, nos hemos enfocado exclusivamente en lo que sucede en el navegador luego de que el recurso (CSS, JS o archivo HTML) está disponible para ser procesado. No hemos tenido en cuenta el tiempo que lleva obtener el recurso del caché o de la red. Supongamos lo siguiente:

* Para un recorrido completo desde la red (latencia de propagación) hasta el servidor se necesitan 100 ms.
* El tiempo de respuesta del servidor es de 100 ms para el documento HTML y 10 ms para todos los demás archivos.

## La experiencia Hello World

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Pruébalo](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

Comenzaremos con lenguaje de marcado HTML básico y una sola imagen (sin CSS ni JavaScript). Abriremos la línea de tiempo de nuestra red en Chrome DevTools e inspeccionaremos la cascada de recursos resultante:

<img src="images/waterfall-dom.png" alt="Ruta de acceso de representación crítica" />

Note: Aunque este documento use DevTools para ilustrar conceptos de CRP, DevTools actualmente no es adecuado para un análisis de CRP. Para más información, consulta [¿Qué hay de DevTools?](measure-crp#devtools).

Tal como se suponía, el archivo HTML tardó aproximadamente 200ms en descargarse. Ten en cuenta que la parte transparente de la línea azul indica el tiempo de espera del navegador en la red (es decir, aún no se recibió ningún byte de la respuesta), mientras que la parte sólida muestra el tiempo restante hasta que finalice la descarga después de haber recibido los primeros bytes de la respuesta. La descarga HTML es muy pequeña (<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

Una vez que esté disponible el contenido HTML, el navegador debe analizar los bytes, convertirlos en tokens y compilar el árbol del DOM. Ten en cuenta que DevTools informa de manera práctica el tiempo para el evento DOMContentLoaded en la parte inferior (216 ms), que también corresponde a la línea vertical azul. La brecha entre el final de la descarga HTML y la línea vertical azul (DOMContentLoaded) es el tiempo que tardó el navegador en crear el árbol del DOM; en este caso, solo unos milisegundos.

Observa que nuestra "foto increíble" no bloqueó el evento `domContentLoaded` . Según parece, podemos construir el árbol de representación e incluso pintar la página sin esperar a cada uno de los recursos de la página: **no todos los recursos son esenciales para proporcionar la primera pintura rápida**. De hecho, al hablar sobre la ruta de acceso de representación crítica generalmente hacemos referencia al lenguaje de marcado HTML, CSS y JavaScript. Las imágenes no bloquean la representación inicial de la página&mdash;no obstante, también debemos asegurarnos de pintar las imágenes lo antes posible.

Dicho esto, el evento `load` (también conocido como `onload`) está bloqueado en la imagen: DevTools informa el evento `onload` a los 335ms. Recuerda que el evento `onload` marca el momento en el cual **todos los recursos** necesarios para la página se han descargado y procesado; es el momento en que el indicador de carga puede dejar de girar en el navegador (la línea vertical roja en la cascada).

## Cómo agregar JavaScript y CSS a la mezcla

Nuestra página "experiencia Hello World" parece simple pero muchas cosas se producen en el interior. En la práctica necesitaremos más que solo HTML: es probable que haya una hoja de estilo CSS y una o más secuencias de comandos para agregar un interactividad a nuestra página. Agreguemos ambas a la mezcla y veamos lo que ocurre:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Pruébalo](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_timing.html){: target="_blank" .external }

*Antes de agregar JavaScript y CSS:*

<img src="images/waterfall-dom.png" alt="Ruta de acceso de representación crítica de DOM" />

*Con JavaScript y CSS:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

La adición de archivos CSS y JavaScript externos suma a nuestra cascada dos solicitudes adicionales que el navegador envía aproximadamente al mismo tiempo. No obstante, **ten en cuenta que ahora la diferencia de tiempo entre los eventos `domContentLoaded` y `onload` es mucho menor.**

¿Qué ocurrió?

* A diferencia de nuestro ejemplo sobre HTML, ahora también debemos obtener y analizar el archivo CSS para construir el CSSOM, y sabemos que necesitamos el DOM y el CSSOM para generar el árbol de representación.
* Dado que la página también contiene un archivo JavaScript que bloquea el analizador, el evento `domContentLoaded` se bloquea hasta que se descargue y analice el archivo CSS: JavaScript puede consultar el CSSOM, por lo que debemos bloquearlo y esperar que se descargue para poder ejecutar JavaScript.

**¿Qué ocurre si reemplazamos nuestra secuencia de comandos externa por una secuencia de comandos integrada?** Incluso cuando la secuencia de comandos está directamente integrada a la página, el navegador no puede ejecutarla hasta construir el CSSOM. En resumen, si usamos código JavaScript integrado, también bloqueará el analizador.

Dicho esto, a pesar del bloqueo del CSS, ¿la integración de la secuencia de comandos hará que la página se represente más rápido? Intentémoslo y veamos lo que sucede...

*JavaScript externo:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*JavaScript integrado:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM y JS integrado" />

Haremos una solicitud menos, pero los tiempos de nuestros eventos `onload` y `domContentLoaded` son los mismos. ¿Por qué? Bien, sabemos que no importa si el código JavaScript está integrado o es externo, ya que no bien el navegador alcance la etiqueta de secuencia de comandos se bloqueará y esperará hasta que se construya el CSSOM. Además, en nuestro primer ejemplo el navegador descarga CSS y JavaScript en paralelo, y estos procesos de descarga finalizan aproximadamente al mismo tiempo. Como consecuencia, en esta instancia específica, integrar el código JavaScript no sirve mucho. Sin embargo, existen varias estrategias que pueden lograr que nuestra página se represente más rápido.

Primero, recuerda que todas las secuencias de comandos integradas bloquean el analizador, pero en las secuencias de comandos externas podemos agregar la palabra clave “async” para desbloquear el analizador. Deshagamos nuestra integración y veamos qué sucede:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

*Parser-blocking (external) JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

Como alternativa, podríamos haber integrado tanto el CSS como JavaScript:

[Pruébalo](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_inlined.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_inlined.html){: target="_blank" .external }

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" />

Como puedes ver, incluso con una página muy sencilla, la optimización de la ruta de acceso de representación crítica es un ejercicio beneficioso: debemos comprender el gráfico de dependencia entre diferentes recursos, identificar los recursos “críticos” y escoger diferentes estrategias para decidir la manera de incluir esos recursos en la página. No hay una única solución para este problema: cada página es diferente. Y deberás seguir un proceso similar por ti mismo para determinar cuál es la mejor estrategia.

Dicho esto, veamos si podemos regresar e identificar algunos patrones de rendimiento generales.

La página más sencilla posible consiste simplemente en el lenguaje de marcado HTML: sin CSS, JavaScript ni otros tipos de recursos. Para representar esta página, el navegador debe iniciar la solicitud, esperar que llegue el documento HTML, analizarlo, compilar el DOM y luego representarlo en la pantalla:

## Patrones de rendimiento

[Pruébalo](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

<img src="images/analysis-dom.png" alt="Hello world CRP" />

Ahora consideremos la misma página, pero en este caso con un archivo CSS externo:

[Pruébalo](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

Definamos el vocabulario que usaremos para describir la ruta de acceso de representación crítica:

Ahora comparemos esto con las características de la ruta crítica del ejemplo de HTML + CSS anterior:

* **Recurso crítico:** recurso que podría bloquear la representación inicial de la página.
* **Extensión de la ruta de acceso crítica:** cantidad de recorridos o el tiempo total necesario para obtener todos los recursos críticos.
* **Bytes críticos:** cantidad total de bytes necesarios para lograr la primera representación de la página, que es la suma de los tamaños de los archivos que se transfieren de todos los recursos críticos. Nuestro primer ejemplo con una sola página HTML contenía un solo recurso crítico (el documento HTML), la longitud de la ruta de acceso crítica también equivalía a 1 recorrido de la red (asumiendo que el archivo es pequeño) y la cantidad total de bytes críticos era el tamaño de la transferencia del documento HTML.

Now let's compare that to the critical path characteristics of the HTML + CSS example above:

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** recursos críticos;
* **2** o más recorridos para la longitud mínima de la ruta de acceso crítica;
* **9** KB de bytes críticos.

Ahora agreguemos un archivo JavaScript adicional a la mezcla.

[Pruébalo](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Agregamos `app.js`, que es un recurso JavaScript externo en la página y, como ya sabemos, bloquea el analizador (es decir, crítico). Lo que es peor, para poder ejecutar el archivo JavaScript también debemos bloquear y esperar el CSSOM. Recuerda que JavaScript puede consultar el CSSOM y, por lo tanto, el navegador se pausará hasta que se descargue `style.css` y se construya el CSSOM.

We added `app.js`, which is both an external JavaScript asset on the page and a parser blocking (that is, critical) resource. Worse, in order to execute the JavaScript file we have to block and wait for CSSOM; recall that JavaScript can query the CSSOM and hence the browser pauses until `style.css` is downloaded and CSSOM is constructed.

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

Ahora tenemos tres recursos críticos que suman 11 KB de bytes críticos, pero la longitud de nuestra ruta de acceso crítica aún es de dos recorridos, ya que podemos transferir el CSS y el JavaScript en paralelo. **Determinar las características de tu ruta de acceso de representación crítica implica que podrás identificar los recursos críticos y comprender la manera en que el navegador programará la obtención de estos.** Continuemos con nuestro ejemplo...

* **3** recursos críticos;
* **2** o más recorridos para la longitud mínima de la ruta de acceso crítica;
* **11** KB de bytes críticos

Después de conversar con nuestros programadores de sitios, nos dimos cuenta de que el JavaScript que incluimos en nuestra página no debe ser de bloqueo: contamos con algunas herramientas de análisis y otros elementos de código que no necesitan bloquear la representación de nuestra página. Sabiendo esto, podemos agregar el atributo “async” a la etiqueta de secuencia de comandos para desbloquear el analizador:

[Pruébalo](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

Como consecuencia, nuestra página optimizada volvió a tener dos recursos críticos (HTML y CSS) con una extensión mínima de ruta de acceso de representación crítica, de dos recorridos, y un total de 9 KB de bytes críticos.

* La secuencia de comandos ya no bloquea el analizador y no forma parte de la ruta de acceso de representación crítica.
* Debido a que no hay otras secuencias de comandos críticas, el CSS tampoco necesita bloquear el evento `domContentLoaded`.
* Cuanto antes se active el evento `domContentLoaded` , antes podrá comenzar a ejecutarse otra lógica de la app.

Por último, si la hoja de estilo CSS solo fuese necesaria para imprimir, ¿cómo se vería?

[Pruébalo](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

Because the style.css resource is only used for print, the browser doesn't need to block on it to render the page. Hence, as soon as DOM construction is complete, the browser has enough information to render the page. As a result, this page has only a single critical resource (the HTML document), and the minimum critical rendering path length is one roundtrip.

## Feedback {: #feedback }

{# wf_devsite_translation #}