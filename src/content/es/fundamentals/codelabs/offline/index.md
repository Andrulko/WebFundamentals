project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Obtén más información sobre cómo integrar un service worker en una app existente para hacer que la app funcione sin conexión.

{# wf_updated_on: 2016-11-09 #} {# wf_published_on: 2016-01-01 #}

# Agregado de un Servicio Worker y trabajo sin conexión a tu app web {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

## Información general

![9246b0abd8d860da.png](img/3ec8737c20a475df.png)

En este codelab, aprenderás a integrar un service worker en una app existente para hacer que la app funcione sin conexión. La app se llama [Air Horner](https://airhorner.com). Haz clic en la bocina y esta hará un sonido.

#### Lo que aprenderás

* Cómo agregar un proceso de trabajo básico a un proyecto existente.
* Cómo simular el modo sin conexión e inspeccionar y depurar un service worker con Chrome DevTools.
* Una sencilla estrategia de almacenamiento en caché sin conexión.

#### Qué necesitarás

* Chrome 52 o superior.
* Un conocimiento básico de [Promesas](/web/fundamentals/getting-started/primers/promises), Git y Chrome DevTools.
* El ejemplo de código.
* Un editor de texto.
* Un servidor web local. Si deseas usar el servidor web descrito en este codelab, necesitas tener Python instalado en tu línea de comandos.

## Obtén el ejemplo de código

Clona el repositorio de GitHub desde la línea de comandos sobre SSH:

    $ git clone git@github.com:GoogleChrome/airhorn.git
    

O HTTPS:

    $ git clone https://github.com/GoogleChrome/airhorn.git
    

## Ejecuta el ejemplo de la app

En primer lugar, veamos cómo luce el ejemplo de app finalizado (pista: es increíble).

Asegúrate de estar en la rama correcta (final) revisando la rama `master`.

    $ git checkout master
    

Ejecuta el sitio desde un servidor web local. Puedes usar cualquier servidor web, pero para el resto de este codelab asumiremos que estás usando `SimpleHTTPServer` de Python en el puerto 3000, así que la app estará disponible desde `localhost:3000`.

    $ cd app
    $ python -m SimpleHTTPServer 3000
    <aside class="key-point">

<p>This repository has one main folder <strong>"app"</strong>. This folder contains the static assets (HTML, CSS, and JavaScript) that you will use for this project.</p>

</aside> 

Abre el sitio en Chrome. Deberías ver: ![9246b0abd8d860da.png](img/3ec8737c20a475df.png)

## Prueba la app

Haz clic en la bocina. Debería hacer un sonido.

Ahora, debes simular que pasas a trabajo sin conexión con Chrome DevTools.

Abre DevTools, dirígete al panel **Application** y habilita la casilla de verificación **Offline**. En la captura de pantalla a continuación, el mouse se desplaza sobre la casilla de verificación.

![479219dc5f6ea4eb.png](img/479219dc5f6ea4eb.png)

Después de hacer clic en la casilla de verificación, nota el ícono de advertencia (triángulo amarillo con signo de admiración) junto a la pestaña del panel __Network __. Esto indica que trabajas sin conexión.

Para probar que trabajas sin conexión, visita <https://google.com>. Deberías ver el mensaje de error "there is no Internet connection" de Chrome.

Ahora, vuelve a tu app. A pesar de que trabajas sin conexión, la página debería volver a cargarse por completo. Deberías poder seguir usando la bocina.

El motivo por el que esto funciona sin conexión es la base de este codelab: soporte sin conexión con service worker.

## Construye la app inicial

Ahora quitarás el soporte sin conexión de la app y aprenderás a usar un service worker para agregar el soporte sin conexión de vuelta en la app.

Revisa la versión "rota" de la app que no tiene implementado el service worker.

    $ git checkout code-lab
    

Vuelve al panel **Application** de DevTools e inhabilita la casilla de verificación **Offline**, para volver a estar en línea.

Ejecuta la página. La app debería funcionar como se espera.

Ahora, usa DevTools para simular nuevamente el modo sin conexión (habilitando la casilla de verificación **Offline** en el panel **Application**). **¡Atento!** Si no sabes mucho sobre los service workers, estás a punto de ver un comportamiento inesperado.

¿Qué esperas ver? Bien, ya que trabajas sin conexión y ya que la versión de la app no tiene service worker, esperarías ver el típico mensaje de error "there is no Internet connection" de Chrome.

Pero lo que ves es... ¡una app en total funcionamiento sin conexión!

![9246b0abd8d860da.png](img/3ec8737c20a475df.png)

¿Qué ocurrió? Bien, recuerda que cuando comenzaste este codelab probaste la versión completa de la app. Cuando ejecutaste esa versión, la app instaló un service worker. El service worker ahora funciona automáticamente cada vez que ejecutes la app. Una vez que un service worker se instala en un ámbito como `localhost:3000` (aprenderás más sobre el ámbito en la siguiente sección), ese service worker automáticamente se inicia cada vez que accedes al ámbito, a menos que lo borres en forma programada o manual.

Para solucionar esto, visita el panel **Application** de DevTools, haz clic en la pestaña **Service Workers** y haz clic en el botón **Unregister**. En la captura de pantalla a continuación, el mouse se desplaza sobre el botón.

![837b46360756810a.png](img/837b46360756810a.png)

Ahora, antes de volver a cargar el sitio, asegúrate de seguir usando DevTools para simular el modo sin conexión. Vuelve a cargar la página y debería mostrarte el mensaje de error "there is no Internet connection", como se espera.

![da11a350ed38ad2e.png](img/da11a350ed38ad2e.png)

## Registra un service worker en el sitio

Ahora es momento de agregar el soporte sin conexión de vuelta en la app. Esto consiste en dos pasos:

1. Crea un archivo de JavaScript que será el service worker.
2. Dile al navegador que registre el archivo de JavaScript como "service worker".

Primero, crea un archivo en blanco llamado `sw.js` y colócalo en la carpeta `/app`.<aside class="key-point">

<p><strong>The location of the service worker is important! </strong>For security reasons, a service worker can only control the pages that are in its same directory or its subdirectories. This means that if you place the service worker file in a scripts directory it will only be able to interact with pages in the scripts directory or below.</p>

</aside> 

Ahora abre `index.html` y agrega el siguiente código al final del `<body>`.

    <script>
    if('serviceWorker' in navigator) {
      navigator.serviceWorker
               .register('/sw.js')
               .then(function() { console.log("Service Worker Registered"); });
    }
    </script>
    

La secuencia de comandos revisa si el navegador admite service workers. Si lo hace, registra nuestro archivo actualmente vacío `sw.js` como service worker, y luego registra en Console.

Antes de ejecutar tu sitio de nuevo, vuelve a DevTools y observa la pestaña **Service Workers** del panel **Application**. Debería estar vacía, lo que indicaría que el sitio no tiene service workers instalados.

![37d374c4b51d273.png](img/37d374c4b51d273.png)

Asegúrate de que la casilla de verificación **Offline** de DevTools esté inhabilitada. Vuelve a cargar tu página nuevamente. Mientras la página se vuelve a cargar, puedes ver que se registra un service worker.

![b9af9805d4535bd3.png](img/b9af9805d4535bd3.png)

Junto a la etiqueta **Source** puedes ver un vínculo al código fuente del service worker registrado.

![3519a5068bc773ea.png](img/3519a5068bc773ea.png)

Si deseas inspeccionar el service worker actualmente instalado de una página, haz clic en el vínculo. Esto te mostrará el código fuente del service worker en el panel **Sources** de DevTools. Por ejemplo, haz clic en el vínculo ahora y deberías ver un archivo vacío.

![dbc14cbb8ca35312.png](img/dbc14cbb8ca35312.png)

## Instalación de los recursos del sitio

Con el service worker registrado, la primera vez que un usuario entra a la página, se dispara un evento `install`. Este evento es donde debes almacenar en caché los recursos de tu página.

Agrega el siguiente código a sw.js.

    importScripts('/cache-polyfill.js');
    
    
    self.addEventListener('install', function(e) {
     e.waitUntil(
       caches.open('airhorner').then(function(cache) {
         return cache.addAll([
           '/',
           '/index.html',
           '/index.html?homescreen=1',
           '/?homescreen=1',
           '/styles/main.css',
           '/scripts/main.min.js',
           '/sounds/airhorn.mp3'
         ]);
       })
     );
    });
    

La primera línea agrega el Polyfill de la caché. El Polyfill ya está incluido en el repositorio. Necesitamos usar Polyfill porque todavía no todos los navegadores son compatibles con la API de caché. A continuación, viene el receptor de eventos `install`. El receptor de eventos `install` abre el objeto `caches` y lo completa con una lista de recursos que almacenaremos en caché. Algo importante sobre la operación `addAll` es que es todo o nada. Si uno de los archivos no está presente o no se puede captar, toda la operación `addAll` falla. Una buena app controlará este panorama.

El siguiente paso es la programación de nuestro service worker para que muestre la intercepción de solicitudes a cualquiera de los recursos, y el uso del objeto `caches` para devolver la versión almacenada a nivel local de cada recurso.

<

El primer paso es adjuntar un controlador de evento al evento `fetch`. Este evento se dispara por cada solicitud que se hace.

<h4>Temas abarcados</h4>

<ul>
  
<li>Cómo agregar un proceso de trabajo básico a un proyecto existente.</li>
<li><a href="https://github.com/coonsta/cache-polyfill">https://github.com/coonsta/cache-polyfill</a> </li>
<li>Una sencilla estrategia de almacenamiento en caché sin conexión.</li>
<li>Currently Chrome and other browsers don't yet fully support the <code>addAll</code> method (<strong>note:</strong> Chrome 46 will be compliant).</li>
<li>Why do you have ?homescreen=1</li>
  
  <li>
    
<p>URLs with query string parameters are treated as individual URLs and need to be cached separately.</p>
</aside>
  </li>
</ul>

## Intercepta las solicitudes de página web

Agrega el siguiente código al final de tu `sw.js` para registrar las solicitudes hechos desde la página principal.

Probemos esto. **¡Atento!** Estás por ver un comportamiento de service worker inesperado.

Abre DevTools y dirígete al panel **Application**. La casilla de verificación **Offline** debería estar inhabilitada. Presiona la tecla `Esc` para abrir el panel lateral **Console** de la parte inferior de la ventana de DevTools. Tu ventana de DevTools debería lucir similar a las siguientes capturas de pantalla:

    self.addEventListener('fetch', function(event) {
     console.log(event.request.url);
    });
    

Let's test this out. **Heads up!** You're about to see some more unexpected service worker behavior.

Vuelve a cargar tu página ahora y vuelve a mirar la ventana de DevTools. Primero, esperamos ver muchas solicitudes registradas en Console, pero eso no sucede. Segundo, en el panel **Service Worker** podemos ver que **Status** ha cambiado:

![c7cfb6099e79d5aa.png](img/c96de824be6852d7.png)

En **Status** hay un nuevo service worker que espera activarse. Ese debe ser el nuevo service worker que incluye los cambios que acabamos de hacer. Entonces, por algún motivo, el service worker anterior que hemos instalado (que era solo un archivo vacío) sigue controlando la página. Si haces clic en el vínculo `sw.js` al lado de **Source** puedes verificar que el service worker anterior siga funcionando.

![c7cfb6099e79d5aa.png](img/c7cfb6099e79d5aa.png)

In the **Status** there's a new service worker that's waiting to activate. That must be the new service worker that includes the changes that we just made. So, for some reason, the old service worker that we installed (which was just a blank file) is still controlling the page. If you click on the `sw.js` link next to **Source** you can verify that the old service worker is still running.<aside class="key-point">

<p>This behavior is by design. Check out  <a href="/web/fundamentals/primers/service-worker/update-a-service-worker">Update a Service Worker</a> to learn more about the service worker lifecycle.</p>

</aside> 

Cuando esta casilla de verificación se habilita, DevTools siempre actualiza el service worker cada vez que se vuelve a cargar la página. Esto es muy útil cuando se desarrolla en forma activa un service worker.

![26f2ae9a805bc69b.png](img/26f2ae9a805bc69b.png)

When this checkbox is enabled, DevTools always updates the service worker on every page reload. This is very useful when actively developing a service worker.

Ahora tienes que decidir qué hacer con esas solicitudes. De manera predeterminada, si no haces nada, la solicitud se pasa a la red y la respuesta se devuelve a la página web.

![53c23650b131143a.png](img/53c23650b131143a.png)

Actualiza tu receptor de eventos de extracción para que coincidan con el siguiente código.

El método `event.respondWith()` le dice al navegador que evalúe el resultado del evento en el futuro. `caches.match(event.request)` toma la solicitud web actual que activó el evento de extracción y busca en la caché un recurso que coincida. La coincidencia se realiza observando la string de la URL. El método `match` muestra una promesa que se resuelve incluso si no se encuentra el archivo en la caché. Esto significa que tienes opción de qué hacer. En tu sencillo caso, cuando no se encuentra el archivo, solo debes `fetch` el archivo en la red y devolverlo al navegador.

Este es el caso más simple, existen muchos otros panoramas de almacenamiento en caché. Por ejemplo, podrías almacenar gradualmente en caché todas las respuestas a las solicitudes que anteriormente no se almacenaron en caché, de modo que en el futuro se muestren todas desde este.

    self.addEventListener('fetch', function(event) {
     console.log(event.request.url);
    
     event.respondWith(
       caches.match(event.request).then(function(response) {
         return response || fetch(event.request);
       })
     );
    });
    

Ahora tienes soporte sin conexión. Vuelve a cargar tu página mientras estés en línea para actualizar tu service worker a la última versión, y luego usa DevTools para pasar al modo sin conexión. Vuelve a cargar tu página de nuevo y deberías tener una bocina totalmente funcional sin conexión.

Ayúdanos a que nuestros code labs sean mejores enviando un [problema](https://github.com/googlesamples/io2015-codelabs/issues) hoy. ¡Gracias!

## ¡Felicitaciones!

{# wf_devsite_translation #}

#### Próximos pasos

* Obtén más información sobre cómo agregar fácilmente un sólido [soporte sin conexión con elementos Polymer sin conexión](https://codelabs.developers.google.com/codelabs/sw-precache/index.html?index=..%2F..%2Findex#0)
* Explora más [técnicas avanzadas de almacenamiento en caché](https://jakearchibald.com/2014/offline-cookbook/)
* A simple offline caching strategy.

#### Más información

* Learn how to easily add robust [offline support with Polymer offline elements](https://codelabs.developers.google.com/codelabs/sw-precache/index.html?index=..%2F..%2Findex#0)
* Explore more [advanced caching techniques](https://jakearchibald.com/2014/offline-cookbook/)

#### Learn More

* [Introduction to service worker](/web/fundamentals/primers/service-worker/)

## ¿Encontraste un problema o tienes comentarios? {: .hide-from-toc }

Help us make our code labs better by submitting an [issue](https://github.com/googlesamples/io2015-codelabs/issues) today. And thanks!