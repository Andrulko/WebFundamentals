project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: En este codelab, crearás una Progressive Web App, que se carga rápidamente, incluso con redes débiles, tiene un ícono en la pantalla principal y se carga como experiencia de pantalla completa y de primer nivel.

{# wf_updated_on: 2017-01-05 #} {# wf_published_on: 2016-01-01 #}

# Tu primera Progressive Web App {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

## Introducción

Las [Progressive Web Apps](/web/progressive-web-apps) son experiencias que combinan lo mejor de la Web y lo mejor de las apps. Están disponibles para los usuarios a partir de la primera visita en una pestaña del navegador y no requieren instalación. A medida que el usuario compila progresivamente una relación con la app con el paso del tiempo, se hace más y más poderosa. Se carga rápidamente, incluso con redes débiles, envía notificaciones push relevantes, tiene un ícono en la pantalla principal y se carga como experiencia de pantalla completa y de primer nivel.

### ¿Qué es una Progressive Web App?

Una Progressive Web App es:

* **Progresiva**: funciona para todos los usuarios, sin importar la elección de navegador, porque está construida con mejora progresiva como principio central.
* **Adaptable**: se adapta a cualquier factor de formulario, sea escritorio, móvil, tablet o lo que venga en el futuro.
* **Independiente de la conectividad**: mejorada con service workers para trabajar sin conexión o con redes de mala calidad.
* **Estilo app**: al usuario le parece una app con interacciones y navegación estilo app, porque está construida con modelo de shell de app.
* **Fresca**: siempre actualizada gracias al proceso de actualización de service worker.
* **Segura**: emitida vía HTTPS para evitar intromisiones y para garantizar que el contenido no se haya manipulado.
* **Descubrible**: se puede identificar como "app" gracias al manifiesto W3C y al alcance de registro de service worker, lo que permite que los motores de búsqueda la encuentren.
* **Posibilidad de volver a interactuar**: facilita la posibilidad de volver a interactuar a través de funciones como notificaciones push.
* **Instalable**: les permite a los usuarios "conservar" las apps que les resultan más útiles en su pantalla principal sin la molestia de una tienda de app.
* **Vinculable** : se puede compartir fácilmente vía URL, no requiere instalación compleja.

Este codelab te guiará para crear tu propia Progressive Web App, incluidas las consideraciones de diseño, como también la implementación de detalles para garantizar que tu app cumpla los principios claves de una Progressive Web App.<aside class="key-point">

<p>Looking for more? Check out the talks from the  <a href="https://www.youtube.com/playlist?list=PLNYkxOF6rcIAWWNR_Q6eLPhsyx6VvYjVb">2016 Progressive Web App Summit</a>.</p>

</aside> 

### ¿Qué crearemos?

<table>
  <p>
    <tr>
      <td colspan="1" rowspan="1">
        </p>

<p>In this codelab, you're going to build a Weather web app using Progressive Web App techniques. Your app will:</p>

        
        <ul>
          
<li>Utilize and demonstrate the above principles of Progressive Web Apps.</li>
<li>Use live weather data.</li>
          
          <li>
            
<p>Provide app-like interactions to allow the user to add cities.</p>
</td><td colspan="1" rowspan="1">
              </li> </ul>

<p><img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png"></p>

              
              <p>
                </td> </tr>
              </p></table> 
              
              <h3>
                Lo que aprenderás
              </h3>
              
              <ul>
                <li>
                  <strong>Progresiva</strong>: usaremos una mejora progresiva en todo el proceso.
                </li>
                <li>
                  <strong>Adaptable</strong>: nos aseguraremos de que se adapte a cualquier forma de formulario.
                </li>
                <li>
                  Independiente de la <strong>conectividad</strong>: almacenaremos en caché el shell de app con service workers.
                </li>
              </ul>
              
              <h3>
                Qué necesitarás
              </h3>
              
              <ul>
                <li>
                  Cómo diseñar y construir una app usando el método “shell de app”
                </li>
                <li>
                  Cómo hacer para que tu app funcione sin conexión
                </li>
                <li>
                  <a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip">The sample code</a>
                </li>
                <li>
                  A text editor
                </li>
                <li>
                  Basic knowledge of HTML, CSS, JavaScript, and <a href="https://developer.chrome.com/devtools">Chrome DevTools</a>
                </li>
              </ul>
              
              <p>
                En este codelab, crearás una app web de estado del tiempo usando técnicas de Progressive Web App. Analicemos las propiedades de una Progressive Web App:
              </p>
              
              <h2>
                Preparación
              </h2>
              
              <h3>
                Descarga el código
              </h3>
              
              <p>
                Este codelab se enfoca en Progressive Web Apps. Los conceptos que no son relevantes y los bloques de código se pasan por alto y se te brindan para que solo copies y pegues.
              </p>
              
              <p>
                <a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip">Download source code</a>
              </p>
              
              <p>
                Unpack the downloaded zip file. This will unpack a root folder (<code>your-first-pwapp-master</code>), which contains one folder for each step of this codelab, along with all of the resources you will need.
              </p>
              
              <p>
                Descomprime el archivo zip descargado. Esto descomprimirá una carpeta raíz (<code>your-first-pwapp-master</code>), que contiene una carpeta para cada paso de este codelab, junto con todos los recursos que necesitarás.
              </p>
              
              <h3>
                Instala y verifica el servidor web
              </h3>
              
              <p>
                Las carpetas <code>step-NN</code> contienen el estado final deseado de cada paso de este codelab. Están allí a modo de referencia. Haremos todo tu trabajo de codificación en un directorio llamado <code>work</code>.
              </p>
              
              <p>
                <a href="https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb">Install Web Server for Chrome</a>
              </p>
              
              <p>
                After installing the Web Server for Chrome app, click on the Apps shortcut on the bookmarks bar:
              </p>
              
              <p>
                <img src="img/9efdf0d1258b78e4.png" alt="9efdf0d1258b78e4.png" />
              </p><aside class="key-point">

<p>More help:  <a href="https://support.google.com/chrome_webstore/answer/3060053">Add and open Chrome apps</a></p>

</aside> 
              
              <p>
                In the ensuing window, click on the Web Server icon:
              </p>
              
              <p>
                <img src="img/dc07bbc9fcfe7c5b.png" alt="dc07bbc9fcfe7c5b.png" />
              </p>
              
              <p>
                You'll see this dialog next, which allows you to configure your local web server:
              </p>
              
              <p>
                <img src="img/433870360ad308d4.png" alt="433870360ad308d4.png" />
              </p>
              
              <p>
                Click the <strong>choose folder</strong> button, and select the <code>work</code> folder. This will enable you to serve your work in progress via the URL highlighted in the web server dialog (in the <strong>Web Server URL(s)</strong> section).
              </p>
              
              <p>
                Haz clic en el botón <strong>choose folder</strong> y selecciona la carpeta <code>work</code>. Esto te permitirá exhibir tu trabajo en progreso a través de la URL destacada en el diálogo del servidor web (en la sección <strong>Web Server URL(s)</strong>).
              </p>
              
              <p>
                <img src="img/39b4e0371e9703e6.png" alt="39b4e0371e9703e6.png" />
              </p>
              
              <p>
                Then stop and restart the server by sliding the toggle labeled "Web Server: STARTED" to the left and then back to the right.
              </p>
              
              <p>
                <img src="img/daefd30e8a290df5.png" alt="daefd30e8a290df5.png" />
              </p>
              
              <p>
                Now visit your work site in your web browser (by clicking on the highlighted Web Server URL) and you should see a page that looks like this:
              </p>
              
              <p>
                <img src="img/aa64e93e8151b642.png" alt="aa64e93e8151b642.png" />
              </p>
              
              <p>
                This app is not yet doing anything interesting - so far, it's just a minimal skeleton with a spinner we're using to verify your web server functionality. We'll add functionality and UI features in subsequent steps.
              </p><aside class="key-point">

<p>From this point forward, all testing/verification (e.g. the<strong> Test It Out</strong> sections in subsequent steps) should be performed using this web server setup.</p>

</aside> 
              
              <h2>
                Adapta la arquitectura del shell de tu app
              </h2>
              
              <h3>
                ¿Qué es el shell de la app?
              </h3>
              
              <p>
                Por supuesto, esta app aún no está haciendo nada interesante. Hasta ahora, solo es un esqueleto mínimo con un control de número que usaremos para verificar la funcionalidad de tu servidor web. Agregaremos funcionalidad y funciones de IU en los siguientes pasos.
              </p>
              
              <p>
                El shell de la app es el HTML, CSS y JavaScript mínimos necesarios para impulsar la interfaz de usuario de una app web progresiva y es uno de los componentes que garantiza un rendimiento bueno y confiable. Su primera carga debería ser muy rápida y almacenarse en caché inmediatamente. Que se "almacena en caché" significa que los archivos de shell se cargan una vez a través de la red y luego se guardan en el dispositivo local. Cada vez posterior en que el usuario abre la app, los archivos de shell se cargan en la caché local del dispositivo, lo que resulta en tiempos de inicio muy rápidos.
              </p>
              
              <p>
                La arquitectura de shell de app separa la infraestructura central de la aplicación y la IU de los datos. La IU y la infraestructura completas se almacenan localmente en la caché mediante un service worker, de modo que en las cargas posteriores la Progressive Web App solo deba recuperar los datos necesarios en lugar de cargar todo.
              </p>
              
              <p>
                <img src="img/156b5e3cc8373d55.png" alt="156b5e3cc8373d55.png" />
              </p>
              
              <p>
                Dicho de otra manera, el shell de app es similar al paquete de código que publicarías en una tienda de apps al crear una aplicación nativa. Contiene los componentes principales necesarios para poner en marcha tu app, pero probablemente no contenga los datos.
              </p>
              
              <h3>
                ¿Por qué usar la arquitectura de shell de app?
              </h3>
              
              <p>
                El uso de la arquitectura de shell de app te permite concentrarte en la velocidad y aporta a tu Progressive Web App propiedades similares a las que tienen las apps nativas (carga instantánea y actualizaciones periódicas), todo ello sin la necesidad de una tienda de apps.
              </p>
              
              <h3>
                Diseña el shell de app
              </h3>
              
              <p>
                El primer paso es dividir el diseño en los componentes centrales que lo integran.
              </p>
              
              <p>
                Pregúntate lo siguiente:
              </p>
              
              <ul>
                <li>
                  Chrome 52 o superior
                </li>
                <li>
                  <a href="https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb">Servidor web para Chrome</a> o el servidor web de tu elección
                </li>
                <li>
                  El ejemplo de código.
                </li>
              </ul>
              
              <p>
                Crearemos una app meteorológica como nuestra primera Progressive Web App. Los componentes claves serán los siguientes:
              </p>
              
              <table>
                <p>
                  <tr>
                    <td colspan="1" rowspan="1">
                      </p> 
                      
                      <ul>
                        
<li>Header with a title, and add/refresh buttons</li>
<li>Container for forecast cards</li>
<li>A forecast card template</li>
<li>A dialog box for adding new cities</li>
                        
                        <li>
                          
<p>A loading indicator</p>
</td><td colspan="1" rowspan="1">
                            </li> </ul>

<p><img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png"></p>

                            
                            <p>
                              </td> </tr>
                            </p></table> 
                            
                            <p>
                              Al diseñar una app más compleja, el contenido que no sea necesario para la carga inicial se puede solicitar más adelante y luego almacenarse en caché para su uso posterior. Por ejemplo, podríamos diferir la carga del diálogo New City hasta después de que representemos la experiencia de la primera ejecución y contemos con algunos ciclos inactivos disponibles.
                            </p>
                            
                            <h2>
                              Implementa el shell de tu app
                            </h2>
                            
                            <p>
                              Existen varias maneras de comenzar con cualquier proyecto, y generalmente recomendamos usar Web Starter Kit. Sin embargo, en este caso, para que tu proyecto sea lo más simple posible y para que puedas concentrarte en las Progressive Web Apps, te hemos ofrecido todos los recursos que necesitarás.
                            </p>
                            
                            <h3>
                              Crea la HTML para el shell de app
                            </h3>
                            
                            <p>
                              Ahora, agregaremos los componentes centrales que discutimos en <a href="/web/fundamentals/getting-started/your-first-progressive-web-app/step-01">Adapta la arquitectura del shell de la app</a>.
                            </p>
                            
                            <p>
                              Recuerda que los componentes claves consistirán en lo siguiente:
                            </p>
                            
                            <ul>
                              <li>
                                ¿Qué debe aparecer en pantalla de inmediato?
                              </li>
                              <li>
                                ¿Qué otros componentes de la IU son claves para tu app?
                              </li>
                              <li>
                                ¿Qué recursos de respaldo se requieren para el shell de la aplicación? Por ejemplo, imágenes, JavaScript, estilos, etc.
                              </li>
                              <li>
                                A dialog for adding new cities
                              </li>
                              <li>
                                A loading indicator
                              </li>
                            </ul>
                            
                            <p>
                              El archivo <code>index.html</code> que ya se encuentra en tu directorio <code>work</code> debería parecerse a este (este es un subset de los contenidos reales, no copies este código en tu archivo):
                            </p>
                            
                            <pre><code>&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;meta charset="utf-8"&gt;
  &lt;meta http-equiv="X-UA-Compatible" content="IE=edge"&gt;
  &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
  &lt;title&gt;Weather PWA&lt;/title&gt;
  &lt;link rel="stylesheet" type="text/css" href="styles/inline.css"&gt;
&lt;/head&gt;
&lt;body&gt;
  &lt;header class="header"&gt;
    &lt;h1 class="header__title"&gt;Weather PWA&lt;/h1&gt;
    &lt;button id="butRefresh" class="headerButton"&gt;&lt;/button&gt;
    &lt;button id="butAdd" class="headerButton"&gt;&lt;/button&gt;
  &lt;/header&gt;

  &lt;main class="main"&gt;
    &lt;div class="card cardTemplate weather-forecast" hidden&gt;
    . . .
    &lt;/div&gt;
  &lt;/main&gt;

  &lt;div class="dialog-container"&gt;
  . . .
  &lt;/div&gt;

  &lt;div class="loader"&gt;
    &lt;svg viewBox="0 0 32 32" width="32" height="32"&gt;
      &lt;circle id="spinner" cx="16" cy="16" r="14" fill="none"&gt;&lt;/circle&gt;
    &lt;/svg&gt;
  &lt;/div&gt;

  &lt;!-- Insert link to app.js here --&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>
                            
                            <p>
                              Observa que el cargador es visible de manera predeterminada. Esto garantiza que el usuario vea el cargador de inmediato cuando se carga la página, lo cual le proporciona una indicación clara de que el contenido se está cargando.
                            </p>
                            
                            <p>
                              Para ahorrar tiempo, también ya hemos creado la hoja de estilo para que uses.
                            </p><aside class="key-point">

<p>We've given you the markup and styles to save you some time and make sure you're starting on a solid foundation. In the next section, you'll have an opportunity to write your own code.</p>

</aside> 
                            
                            <h3>
                              Revisa el código clave de JavaScript de la app
                            </h3>
                            
                            <p>
                              Ahora que tenemos gran parte de la IU lista, es hora de comenzar a conectar el código para que todo funcione. Al igual que con el resto del shell de app, debes conocer el código que necesitas como parte de la experiencia clave y lo que podrás cargar posteriormente.
                            </p>
                            
                            <p>
                              El directorio de trabajo también ya incluye el código de la app (<code>scripts/app.js</code>) y en él encontrarás:
                            </p>
                            
                            <ul>
                              <li>
                                encabezado con título y botones de adición y actualización;
                              </li>
                              <li>
                                contenedor para tarjetas de pronóstico;
                              </li>
                              <li>
                                una plantilla para tarjetas de pronóstico;
                              </li>
                              <li>
                                un cuadro de diálogo para agregar nuevas ciudades;
                              </li>
                              <li>
                                un indicador de carga.
                              </li>
                              <li>
                                Some fake data (<code>initialWeatherForecast</code>) you can use to quickly test how things render.
                              </li>
                            </ul>
                            
                            <h3>
                              Probar
                            </h3>
                            
                            <p>
                              Ahora que tienes el HTML, los estilo y JavaScript centrales, es hora de probar la app.
                            </p>
                            
                            <p>
                              Para ver cómo se representan los datos de estado del tiempo falsos, elimina el comentario de la siguiente línea de la parte inferior de tu archivo <code>index.html</code>:
                            </p>
                            
                            <pre><code>&lt;!--&lt;script src="scripts/app.js" async&gt;&lt;/script&gt;--&gt;
</code></pre>
                            
                            <p>
                              A continuación, elimina el comentario de la siguiente línea de la parte inferior de tu archivo <code>app.js</code>:
                            </p>
                            
                            <pre><code>// app.updateForecastCard(initialWeatherForecast);
</code></pre>
                            
                            <p>
                              Vuelve a cargar tu app. El resultado debería ser una tarjeta de pronóstico climático de agradable formato (a pesar de ser falso, como verás por la fecha) con el control de número inhabilitado, así:
                            </p>
                            
                            <p>
                              <img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-04/">Vínculo</a>
                            </p>
                            
                            <p>
                              Una vez que la hayas probado y hayas verificado que funciona como esperabas, puedes quitar la llamada a <code>app.updateForecastCard</code> con los datos falsos nuevamente. Solo la necesitas para asegurarte de que todo funcione como esperabas.
                            </p>
                            
                            <h2>
                              Comienza con una primera carga rápida
                            </h2>
                            
                            <p>
                              Las Progressive Web Apps deben iniciarse rápidamente y deben poder usarse de inmediato. En su estado actual, nuestra app de estado del tiempo se inicia rápidamente, pero no puede usarse. No hay datos. Podríamos enviar una solicitud AJAX para obtener esos datos, pero eso generaría otra solicitud y demoraría la carga inicial. Como alternativa, proporciona datos reales en la primera carga.
                            </p>
                            
                            <h3>
                              Inyecta los datos del pronóstico del tiempo
                            </h3>
                            
                            <p>
                              Para este code lab, simularemos que el servidor introduce el pronóstico de estado del tiempo directamente en el JavaScript, pero, en una app de producción, el servidor introduciría los últimos datos de pronóstico de estado del tiempo según la geolocalización de la dirección IP del usuario.
                            </p>
                            
                            <p>
                              El código ya contiene los datos que introduciremos. Es el <code>initialWeatherForecast</code> que usamos en el paso anterior.
                            </p>
                            
                            <h3>
                              Diferenciación de la primera ejecución
                            </h3>
                            
                            <p>
                              ¿Cómo determinar el momento en que se debe mostrar esa información, que puede no ser relevante en cargas futuras, cuando se obtenga la app del estado del tiempo del caché? Cuando el usuario cargue la app en visitas posteriores, es posible que la ciudad cambie. Por ello, debemos cargar la información para la ciudad implicada, y no necesariamente para la primera ciudad que este buscó.
                            </p>
                            
                            <p>
                              Las preferencias del usuario, como la lista de ciudades a las que el usuario se ha suscrito, se deberían almacenar a nivel local usando IndexedDB u otro mecanismo de almacenamiento rápido. Para simplificar este code lab lo más posible, hemos usado <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage">localStorage</a>, que no es ideal para apps de producción porque es un mecanismo de bloqueo y almacenamiento sincrónico que puede ser muy lento en algunos dispositivos.
                            </p><aside class="key-point">

<p><strong>Extra Credit</strong>: Replace <code>localStorage</code> implementation with  <a href="https://www.npmjs.com/package/idb">idb</a>, check out  <a href="https://github.com/localForage/localForage">localForage</a> as a simple wrapper to idb.</p>

</aside> 
                            
                            <p>
                              En primer lugar, agreguemos el código necesario para guardar las preferencias de usuario. Encuentra el siguiente comentario TODO en tu código.
                            </p>
                            
                            <pre><code>  // TODO add saveSelectedCities function here
</code></pre>
                            
                            <p>
                              Y agrega el siguiente código debajo del comentario.
                            </p>
                            
                            <pre><code>  // Save list of cities to localStorage.
  app.saveSelectedCities = function() {
    var selectedCities = JSON.stringify(app.selectedCities);
    localStorage.selectedCities = selectedCities;
  };
</code></pre>
                            
                            <p>
                              A continuación, agreguemos el código de inicio para revisar si el usuario tiene ciudades guardadas y mostrarlas, o usar los datos introducidos. Encuentra el siguiente comentario:
                            </p>
                            
                            <pre><code>  // TODO add startup code here
</code></pre>
                            
                            <p>
                              Y agrega el siguiente código debajo de este comentario:
                            </p>
                            
                            <pre><code>/************************************************************************
   *

   * Código necesario para iniciar la app
   *
   * NOTA: To simplify this codelab, we've used localStorage.
   *   localStorage is a synchronous API and has serious performance
   *   implications. It should not be used in production applications!
   *   Instead, check out IDB (https://www.npmjs.com/package/idb) or
   *   SimpleDB (https://gist.github.com/inexorabletash/c8069c042b734519680c)
   ************************************************************************/

  app.selectedCities = localStorage.selectedCities;
  if (app.selectedCities) {
    app.selectedCities = JSON.parse(app.selectedCities);
    app.selectedCities.forEach(function(city) {
      app.getForecast(city.key, city.label);
    });
  } else {
    /* The user is using the app for the first time, or the user has not
     * saved any cities, so show the user some fake data. A real app in this
     * scenario could guess the user's location via IP lookup and then inject
     * that data into the page.
     */
    app.updateForecastCard(initialWeatherForecast);
    app.selectedCities = [
      {key: initialWeatherForecast.key, label: initialWeatherForecast.label}
    ];
    app.saveSelectedCities();
  }
</code></pre>
                            
                            <p>
                              El código de inicio revisa si hay ciudades guardadas en el almacenamiento local. Si la hay, analiza los datos de almacenamiento local y muestra una tarjeta de pronóstico climático de cada una de las ciudades guardadas. Si no, el código de inicio usa los datos falsos de pronóstico climático y los guarda como ciudad predeterminada.
                            </p>
                            
                            <h3>
                              Guarda las ciudades seleccionadas
                            </h3>
                            
                            <p>
                              Finalmente, tienes que modificar el controlador del botón "add city" para guardar la ciudad seleccionada en el almacenamiento local.
                            </p>
                            
                            <p>
                              Actualiza tu controlador de clic de <code>butAddCity</code> para que coincida con el siguiente código:
                            </p>
                            
                            <pre><code>document.getElementById('butAddCity').addEventListener('click', function() {
    // Add the newly selected city
    var select = document.getElementById('selectCityToAdd');
    var selected = select.options[select.selectedIndex];
    var key = selected.value;
    var label = selected.textContent;
    if (!app.selectedCities) {
      app.selectedCities = [];
    }
    app.getForecast(key, label);
    app.selectedCities.push({key: key, label: label});
    app.saveSelectedCities();
    app.toggleAddDialog(false);
  });
</code></pre>
                            
                            <p>
                              Los nuevos agregados son la inicialización de <code>app.selectedCities</code> si no existe y las llamadas a <code>app.selectedCities.push()</code> y <code>app.saveSelectedCities()</code>.
                            </p>
                            
                            <h3>
                              Probar
                            </h3>
                            
                            <ul>
                              <li>
                                encabezado con título y botones de adición y actualización;
                              </li>
                              <li>
                                contenedor para tarjetas de pronóstico;
                              </li>
                              <li>
                                una plantilla para tarjetas de pronóstico;
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-05/">Vínculo</a>
                            </p>
                            
                            <h2>
                              Usa service workers para almacenar en caché por adelantado el shell de la app
                            </h2>
                            
                            <p>
                              Las Progressive Web Apps tienen que ser rápidas y posibles de instalar, lo que significa que funcionan en línea, sin conexión y en conexiones lentas e intermitentes. Para lograr esto, tenemos que almacenar en caché el shell de nuestra app usando service worker, para que siempre esté disponible rápidamente y en forma confiable.
                            </p>
                            
                            <p>
                              Si no conoces los service workers, puedes adquirir conocimientos básicos leyendo <a href="/web/fundamentals/primers/service-worker/">Introducción a Service Workers</a> sobre qué hacen, cómo funciona su ciclo de vida y más. Una vez que hayas completado este code lab, asegúrate de revisar el <a href="https://goo.gl/jhXCBy">code lab Depuración de Service Workers</a> para conocer más cómo trabajar con service workers.
                            </p>
                            
                            <p>
                              Las funciones que se proporcionan mediante los procesos de trabajo deben considerarse como una mejora progresiva, y solo deben agregarse si son compatibles con el navegador. Por ejemplo, con los procesos de trabajo puedes almacenar en caché el shell de la app y datos para tu app, de modo que estén disponibles aun cuando no suceda lo mismo con la red. Cuando no se admitan procesos de trabajo, no se llamará al código sin conexión y el usuario obtendrá una experiencia básica. El uso de la detección de funciones para proporcionar una mejora progresiva tiene poca sobrecarga y no fallará en navegadores más antiguos que no admitan esa función.
                            </p><aside class="key-point">

<p><strong>Remember</strong>: Service worker functionality is only available on pages that are accessed via HTTPS (<a href="http://localhost">http://localhost</a> and equivalents will also work, to facilitate testing). To learn about the rationale behind this restriction check out  <a href="http://www.chromium.org/Home/chromium-security/prefer-secure-origins-for-powerful-new-features">Prefer Secure Origins For Powerful New Features</a> from the Chromium team.</p>

</aside> 
                            
                            <h3>
                              Registra el service worker si está disponible
                            </h3>
                            
                            <p>
                              El primer paso para lograr que la app funcione sin conexión es registrar un proceso de trabajo; una secuencia de comandos que permite el uso en segundo plano sin necesidad de abrir una página web o de que exista interacción por parte del usuario.
                            </p>
                            
                            <p>
                              Esto requiere dos pasos sencillos:
                            </p>
                            
                            <ol start="1">
                              <li>
                                Dile al navegador que registre el archivo de JavaScript como service worker.
                              </li>
                              
                              <li>
                                Crea un archivo de JavaScript que contenga el service worker.
                              </li>
                            </ol>
                            
                            <p>
                              Primero, tenemos que revisar si el navegador es compatible con service workers y, si lo es, registrar el service worker. Agrega el siguiente código a <code>app.js</code> (después del comentario <code>// TODO add service worker code here</code>):
                            </p>
                            
                            <pre><code>  if ('serviceWorker' in navigator) {
    navigator.serviceWorker
             .register('./service-worker.js')
             .then(function() { console.log('Service Worker Registered'); });
  }
</code></pre>
                            
                            <h3>
                              Almacena en caché los recursos del sitio
                            </h3>
                            
                            <p>
                              Cuando se registra el service worker y el evento de instalación se activa por primera vez, el usuario visita la página. En este controlador de eventos, almacenaremos en caché todos los recursos necesarios para la aplicación.
                            </p><aside class="warning">

<p>The code below must NOT be used in production, it covers only the most basic use cases and it's easy to get yourself into a state where your app shell will never update. Be sure to review the section below that discusses the pitfalls of this implementation and how to avoid them.</p>

</aside> 
                            
                            <p>
                              Cuando se activa el service worker, debe abrir los objetos <a href="https://developer.mozilla.org/en-US/docs/Web/API/Cache">cachés</a> y mostrarlos con los recursos necesarios para cargar el shell de la app. Crea un archivo llamado <code>service-worker.js</code> en tu carpeta de raíz de app (que debería ser el directorio <code>your-first-pwapp-master/work</code>). El archivo tiene que vivir en la raíz de la app porque el directorio en el que reside el archivo define el alcance de los service workers. Agrega este código en tu nuevo archivo <code>service-worker.js</code>:
                            </p>
                            
                            <pre><code>var cacheName = 'weatherPWA-step-6-1';
var filesToCache = [];

self.addEventListener('install', function(e) {
  console.log('[ServiceWorker] Install');
  e.waitUntil(
    caches.open(cacheName).then(function(cache) {
      console.log('[ServiceWorker] Caching app shell');
      return cache.addAll(filesToCache);
    })
  );
});
</code></pre>
                            
                            <p>
                              En primer lugar, debemos abrir la caché con <code>caches.open()</code> y proporcionar un nombre de caché. Proporcionar un nombre de caché nos permite versionar archivos o separar datos del shell de la app de modo que podamos actualizar uno con facilidad sin afectar al otro.
                            </p>
                            
                            <p>
                              Una vez que se abre la caché, podremos llamar a <code>cache.addAll()</code>, que toma una lista de URL, luego las obtiene del servidor y agrega la respuesta al caché. Lamentablemente, <code>cache.addAll()</code> es atómico; si alguno de los archivos falla, todo el paso de caché falla.
                            </p>
                            
                            <p>
                              Comencemos a conocer cómo usar DevTools para comprender y depurar los service workers. Antes de volver a cargar tu página, abre DevTools, dirígete al subpanel <strong>Service Worker</strong> del panel <strong>Application</strong>. Debería tener la siguiente apariencia:
                            </p>
                            
                            <p>
                              <img src="img/ed4633f91ec1389f.png" alt="ed4633f91ec1389f.png" />
                            </p>
                            
                            <p>
                              Cuando veas una página en blanco como esta, significa que la página que está abierta no tiene service workers registrados.
                            </p>
                            
                            <p>
                              Ahora, actualiza la página. El subpanel de Service Worker ahora debería lucir así.
                            </p>
                            
                            <p>
                              <img src="img/bf15c2f18d7f945c.png" alt="bf15c2f18d7f945c.png" />
                            </p>
                            
                            <p>
                              Cuando veas información como esta, significa que la página tiene un service worker en ejecución.
                            </p>
                            
                            <p>
                              Ahora vamos a tomar un desvío y mostraremos una trampa que puedes encontrar a la hora de desarrollar service workers. Para mostrarlo, agreguemos un receptor de eventos <code>activate</code> debajo del receptor de eventos <code>install</code> en tu archivo <code>service-worker.js</code>.
                            </p>
                            
                            <pre><code>self.addEventListener('activate', function(e) {
  console.log('[ServiceWorker] Activate');
});
</code></pre>
                            
                            <p>
                              El evento <code>activate</code> se activa cuando se inicia el service worker.
                            </p>
                            
                            <p>
                              Abre la Console de DevTools y vuelve a cargar la página, pasa al subpanel de Service Worker en el panel Application y haz clic para inspeccionar el service worker activado. Esperas ver el mensaje <code>[ServiceWorker] Activate</code> registrado en la consola, pero no sucedió. Observa el subpanel de Service Worker y verás que el nuevo service worker (que incluye el receptor de eventos activar) parece estar en estado de "espera".
                            </p>
                            
                            <p>
                              <img src="img/1f454b6807700695.png" alt="1f454b6807700695.png" />
                            </p>
                            
                            <p>
                              Básicamente, el antiguo service worker sigue controlando la página, siempre y cuando haya una pestaña abierta en la página. Así que, *podrías * cerrar y volver a abrir la página o presionar el botón <strong>skipWaiting</strong>, pero una solución a más largo plazo es habilitar la casilla de verificación <strong>Update on Reload</strong> en el subpanel de Service Worker de DevTools. Cuando esta casilla de verificación está marcada, el service worker se actualiza forzosamente cada vez que se vuelve a cargar la página.
                            </p>
                            
                            <p>
                              Marca la casilla de verificación <strong>update on reload</strong> ahora y vuelve a cargar la página para confirmar que se active el nuevo service worker.
                            </p>
                            
                            <p>
                              <strong>Nota:</strong> puedes ver un error en el subpanel de Service Worker del panel Application similar al siguiente, es <strong>seguro</strong> ignorar este error.
                            </p>
                            
                            <p>
                              <img src="img/b1728ef310c444f5.png" alt="b1728ef310c444f5.png" />
                            </p>
                            
                            <p>
                              Eso es todo por ahora en cuanto a inspección y depuración de service workers en DevTools. Más adelante te mostraremos más trucos. Volvamos a la compilación de tu app.
                            </p>
                            
                            <p>
                              Ampliemos la información sobre el receptor de eventos <code>activate</code> e incluyamos algo de lógica para actualizar la caché. Actualiza tu código para que coincida con el siguiente código.
                            </p>
                            
                            <pre><code>self.addEventListener('activate', function(e) {
  console.log('[ServiceWorker] Activate');
  e.waitUntil(
    caches.keys().then(function(keyList) {
      return Promise.all(keyList.map(function(key) {
        if (key !== cacheName) {
          console.log('[ServiceWorker] Removing old cache', key);
          return caches.delete(key);
        }
      }));
    })
  );
  return self.clients.claim();
});
</code></pre>
                            
                            <p>
                              Este código garantiza que tu service worker actualice su caché cada vez que cambie cualquiera de los archivos del shell de la app. Para que esto funcione, tendrías que incrementar la variable <code>cacheName</code> de la parte superior de tu archivo de service worker.
                            </p>
                            
                            <p>
                              La última instrucción corrige un caso de esquina sobre el que puedes leer en el siguiente cuadro de información (opcional).
                            </p><aside class="key-point">

<p>When the app is complete, <code>self.clients.claim()</code> fixes a corner case in which the app wasn't returning the latest data. You can reproduce the corner case by commenting out the line below and then doing the following steps: 1) load app for first time so that the initial New York City data is shown 2) press the refresh button on the app 3) go offline 4) reload the app. You expect to see the newer NYC data, but you actually see the initial data. This happens because the service worker is not yet activated. <code>self.clients.claim()</code> essentially lets you activate the service worker faster.</p>

</aside> 
                            
                            <p>
                              Por último, actualizaremos la lista de archivos necesarios para el shell de la app. En la matriz, debemos incluir todos los archivos que necesita nuestra app, como imágenes, JavaScript, hojas de estilo, etc. Cerca de la parte superior de tu archivo <code>service-worker.js</code>, reemplaza <code>var filesToCache = [];</code> con el siguiente código:
                            </p>
                            
                            <pre><code>var filesToCache = [
  '/',
  '/index.html',
  '/scripts/app.js',
  '/styles/inline.css',
  '/images/clear.png',
  '/images/cloudy-scattered-showers.png',
  '/images/cloudy.png',
  '/images/fog.png',
  '/images/ic_add_white_24px.svg',
  '/images/ic_refresh_white_24px.svg',
  '/images/partly-cloudy.png',
  '/images/rain.png',
  '/images/scattered-showers.png',
  '/images/sleet.png',
  '/images/snow.png',
  '/images/thunderstorm.png',
  '/images/wind.png'
];
</code></pre><aside class="key-point">

<p>Be sure to include all permutations of file names, for example our app is served from <code>index.html</code>, but it may also be requested as <code>/</code> since the server sends <code>index.html</code> when a root folder is requested. You could deal with this in the <code>fetch</code> method, but it would require special casing which may become complex.</p>

</aside> 
                            
                            <p>
                              Todavía nuestra app no funciona sin conexión. Hemos almacenado en caché los componentes del shell de la app, pero tenemos que cargarlos desde la caché local.
                            </p>
                            
                            <h3>
                              Obtén el shell de la app desde la caché
                            </h3>
                            
                            <p>
                              Los procesos de trabajo ofrecen la capacidad de interceptar solicitudes realizadas desde nuestra Progressive Web App y controlarlas desde el service worker. Esto significa que podemos determinar la manera en que deseamos controlar la solicitud y, posiblemente, ofrecer nuestra propia respuesta almacenada en caché.
                            </p>
                            
                            <p>
                              Por ejemplo:
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(event) {
  // Do something interesting with the fetch here
});
</code></pre>
                            
                            <p>
                              A continuación, obtendremos el shell de la app desde la caché. Agrega el siguiente código al final de tu archivo <code>service-worker.js</code>:
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(e) {
  console.log('[ServiceWorker] Fetch', e.request.url);
  e.respondWith(
    caches.match(e.request).then(function(response) {
      return response || fetch(e.request);
    })
  );
});
</code></pre>
                            
                            <p>
                              Desde adentro hacia afuera, <code>caches.match()</code> evalúa la solicitud web que activó el evento <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">extracción</a> y revisa si está disponible en la caché. Luego, este responde con la versión almacenada en caché o usa <code>fetch</code> para obtener una copia desde la red. La <code>response</code> se devuelve a la página web con <code>e.respondWith()</code>.
                            </p><aside class="warning">

<p>If you're not seeing the <code>[ServiceWorker]</code> logging in the console, be sure you've changed the <code>cacheName</code> variable and that you're inspecting the right service worker by opening the Service Worker pane in the Applications panel and clicking <strong>inspect</strong> on the running service worker. If that doesn't work, see the section on Tips for testing live service workers.</p>

</aside> 
                            
                            <h3>
                              Probar
                            </h3>
                            
                            <p>
                              ¡Ahora tu app puede funcionar sin conexión! Probémoslo.
                            </p>
                            
                            <p>
                              Vuelve a cargar tu página y dirígete al subpanel <strong>Cache Storage</strong> del panel <strong>Application</strong> de DevTools. Expande la sección y deberías ver el nombre de la caché del shell de tu app enumerado a la izquierda. Cuando haces clic en la caché del shell de tu app, puedes ver todos los recursos que actualmente ha almacenado en caché.
                            </p>
                            
                            <p>
                              <img src="img/ab9c361527825fac.png" alt="ab9c361527825fac.png" />
                            </p>
                            
                            <p>
                              Ahora, probemos el modo sin conexión. Vuelve al subpanel <strong>Service Worker</strong> de DevTools y marca la casilla de verificación <strong>Offline</strong>. Después de marcarla, deberías ver un pequeño ícono amarillo de advertencia al lado de la pestaña del panel <strong>Network</strong>. Esto indica que trabajas sin conexión.
                            </p>
                            
                            <p>
                              <img src="img/7656372ff6c6a0f7.png" alt="7656372ff6c6a0f7.png" />
                            </p>
                            
                            <p>
                              Vuelve a cargar tu página y... ¡funciona! O, al menos, así parece. Observa cómo carga los datos de estado del tiempo iniciales (falsos).
                            </p>
                            
                            <p>
                              <img src="img/8a959b48e233bc93.png" alt="8a959b48e233bc93.png" />
                            </p>
                            
                            <p>
                              Observa la oración <code>else</code> de <code>app.getForecast()</code> para comprender por qué la app puede cargar los datos falsos.
                            </p>
                            
                            <p>
                              El siguiente paso es la modificación de la lógica de la app y el service worker para poder almacenar en caché los datos de estado del tiempo, y mostrar los datos más recientes de la caché cuando la app trabaje sin conexión.
                            </p>
                            
                            <p>
                              <strong>Consejo:</strong> para comenzar desde cero y eliminar todos los datos guardados (localStoarge, datos de indexedDB, archivos almacenados en caché) y quita los service workers, usa el subpanel de almacenamiento Clear de la pestaña Application.
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-06/">Vínculo</a>
                            </p>
                            
                            <h3>
                              Ten cuidado con los casos extremos
                            </h3>
                            
                            <p>
                              Como ya se mencionó, este código <strong>no se debe usar en producción</strong> debido a todos los casos extremos sin manejar.
                            </p>
                            
                            <h4>
                              El almacenamiento en caché depende de la actualización de la clave del caché para cada cambio
                            </h4>
                            
                            <p>
                              Por ejemplo, este método de almacenamiento en caché exige que actualices la clave del caché cada vez que modifiques contenido; de lo contrario, la caché no se actualizará y se ofrecerá el contenido anterior. Asegúrate de cambiar la clave de caché con cada cambio mientras trabajas en tu proyecto.
                            </p>
                            
                            <h4>
                              Requiere que se vuelva a descargar todo para cada cambio
                            </h4>
                            
                            <p>
                              Otra desventaja es que se invalida todo la caché y se debe volver a descargar cada vez que cambia un archivo. Esto significa que si cambias un error ortográfico de un solo carácter, se invalidará la caché y se deberá descargar todo nuevamente. Esto no es precisamente eficaz.
                            </p>
                            
                            <h4>
                              La caché del navegador puede impedir la actualización del caché del service worker
                            </h4>
                            
                            <p>
                              Aquí encontramos otro inconveniente importante. Es fundamental que la solicitud HTTPS realizada durante el controlador de la instalación vaya directamente a la red y no muestre una respuesta del la caché del navegador. De lo contrario, el navegador puede mostrar la versión anterior almacenada en caché, lo cual hará que la caché del service worker nunca se actualice.
                            </p>
                            
                            <h4>
                              En la producción, ten en cuenta las estrategias en las que se prioriza la caché
                            </h4>
                            
                            <p>
                              En nuestra app se usa una estrategia en la que se prioriza la caché, lo cual genera una copia de todo el contenido almacenado en caché que se muestra sin enviar una consulta a la red. Si bien implementar una estrategia en la que se priorice la caché es sencillo, puede suponer desafíos en el futuro. Una vez que se almacena en caché el registro del proceso de trabajo y la página host, puede resultar muy difícil cambiar la configuración del proceso de trabajo (ya que esta depende del punto en el que se definió), y podrías encontrarte implementando sitios extremadamente difíciles de actualizar.
                            </p>
                            
                            <h4>
                              ¿Cómo evito estos casos extremos?
                            </h4>
                            
                            <p>
                              ¿Cómo evitamos estos casos extremos? Usa una biblioteca como <a href="https://github.com/GoogleChrome/sw-precache">sw-precache</a>, que brinda buen control sobre lo que vence, asegura que las solicitudes vayan directamente a la red y se encarga todo el trabajo duro por ti.
                            </p>
                            
                            <h3>
                              Sugerencias para probar service workers dinámicos
                            </h3>
                            
                            <p>
                              La depuración de service workers puede ser un desafío, y cuando incluye el almacenamiento en caché, todo se puede convertir en una pesadilla si la caché no se actualiza cuando tú lo esperas. Entre el ciclo de vida del proceso de trabajo típico y un error en tu código, puedes frustrarte bastante rápido. No lo hagas. Existen algunas herramientas que pueden hacer más simple tu trabajo.
                            </p>
                            
                            <h4>
                              Comienza desde cero
                            </h4>
                            
                            <p>
                              En algunos casos, puedes encontrarte cargando datos almacenados en caché o que las cosas no están actualizadas como esperas. Para eliminar todos los datos guardados (localStoarge, datos de indexedDB, archivos almacenados en caché) y quitar los service workers, usa el subpanel de almacenamiento Clear de la pestaña Application.
                            </p>
                            
                            <p>
                              Algunas otras sugerencias:
                            </p>
                            
                            <ul>
                              <li>
                                un objeto <code>app</code> que contiene parte de la información clave necesaria para la app;
                              </li>
                              <li>
                                los receptores de códigos para todos los botones del encabezado (<code>add/refresh</code>) y del diálogo para agregar la ciudad (<code>add/cancel</code>);
                              </li>
                              <li>
                                un método para agregar o actualizar tarjetas de pronóstico (<code>app.updateForecastCard</code>);
                              </li>
                              <li>
                                un método para obtener los últimos datos de pronóstico de estado del tiempo de la API de estado del tiempo pública de Firebase (<code>app.getForecast</code>);
                              </li>
                            </ul>
                            
                            <h2>
                              Usa service workers para almacenar en caché los datos de pronóstico climático
                            </h2>
                            
                            <p>
                              Escoger la <a href="https://jakearchibald.com/2014/offline-cookbook/">estrategia de almacenamiento en caché</a> adecuada para tus datos es vital y depende del tipo de datos que presenta tu app. Por ejemplo, los datos sensibles al tiempo, como el estado del tiempo o cotizaciones bursátiles, deberían ser lo más nuevos posibles, mientras que las imágenes de avatar y el contenido de artículos se puede actualizar menos a menudo.
                            </p>
                            
                            <p>
                              La estrategia de <a href="https://jakearchibald.com/2014/offline-cookbook/#cache-network-race">primero-caché-después-red</a> es ideal para nuestra app. Hace que se muestren datos en pantalla lo más rápido posible y luego los actualiza cuando obtiene de la red los datos más recientes. En comparación con la primero-red-luego-caché, el usuario no tiene que esperar hasta que la <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">extracción</a> finalice para obtener los datos almacenados en caché.
                            </p>
                            
                            <p>
                              Caché-primero-después-red significa que tenemos que emitir dos solicitudes asincrónicas: una a la caché y otra a la red. Nuestra solicitud de red con la app no debe cambiar demasiado, pero se debe modificar el service worker para almacenar en caché la respuesta antes de mostrarla.
                            </p>
                            
                            <p>
                              Bajo circunstancias normales, los datos almacenados en caché se mostrarán casi inmediatamente, brindándole a la app datos recientes que pueda usar. Posteriormente, cuando se muestre la respuesta de la red, se actualizará la app con los datos más recientes de la red.
                            </p>
                            
                            <h3>
                              Intercepta la solicitud de la red y almacena la respuesta en caché
                            </h3>
                            
                            <p>
                              Se debe modificar el proceso de trabajo para interceptar solicitudes enviadas a la weather API y almacenar sus respuestas en la caché, de modo que se pueda acceder fácilmente a ellas posteriormente. En la estrategia caché-después-red, esperamos que la respuesta de la red sea la "fuente de la verdad" y que siempre nos brinde la información más reciente. Si esta no puede hacerlo, podría producirse un error, pero no representará un problema porque se habrán recuperado los últimos datos almacenados en la caché de la app.
                            </p>
                            
                            <p>
                              En el proceso de trabajo, agregaremos un <code>dataCacheName</code> para poder separar los datos de nuestras aplicaciones del shell de la app. Cuando se actualice el shell de app y se depuren los cachés más antiguos, los datos permanecerán intactos y estarán listos para una carga rapidísima. Recuerda que si en el futuro cambias el formato de tus datos, deberás controlar esos cambios y asegurarte de que el shell y el contenido de la app permanezcan sincronizados.
                            </p>
                            
                            <p>
                              Agrega la siguiente línea en la parte superior de tu archivo <code>service-worker.js</code>:
                            </p>
                            
                            <pre><code>var dataCacheName = 'weatherData-v1';
</code></pre>
                            
                            <p>
                              Luego, actualiza el controlador de evento <code>activate</code> para que no borre la caché de datos cuando limpia la caché del shell de la app.
                            </p>
                            
                            <pre><code>if (key !== cacheName && key !== dataCacheName) {
</code></pre>
                            
                            <p>
                              Finalmente, actualiza el controlador de evento <code>fetch</code> para que controle solicitudes a la API de datos en forma separada de otras solicitudes.
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(e) {
  console.log('[Service Worker] Fetch', e.request.url);
  var dataUrl = 'https://query.yahooapis.com/v1/public/yql';
  if (e.request.url.indexOf(dataUrl) &gt; -1) {
    /*
     * When the request URL contains dataUrl, the app is asking for fresh
     * weather data. In this case, the service worker always goes to the
     * network and then caches the response. This is called the "Cache then
     * network" strategy:
     * https://jakearchibald.com/2014/offline-cookbook/#cache-then-network
     */
    e.respondWith(
      caches.open(dataCacheName).then(function(cache) {
        return fetch(e.request).then(function(response){
          cache.put(e.request.url, response.clone());
          return response;
        });
      })
    );
  } else {
    /*
     * The app is asking for app shell files. In this scenario the app uses the
     * "Cache, falling back to the network" offline strategy:
     * https://jakearchibald.com/2014/offline-cookbook/#cache-falling-back-to-network
     */
    e.respondWith(
      caches.match(e.request).then(function(response) {
        return response || fetch(e.request);
      })
    );
  }
});
</code></pre>
                            
                            <p>
                              El código intercepta la solicitud y comprueba si la URL comienza con la dirección de la weather API. Si lo hace, usa <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">extracción</a> para hacer la solicitud. Una vez que se muestra la respuesta, nuestro código abre la caché, clona la respuesta, la almacena en la caché y le muestra la respuesta al solicitante original.
                            </p>
                            
                            <p>
                              La app aún no funcionará sin conexión. Hemos implementado el almacenamiento en caché y la devolución para el shell de la app, pero, a pesar de estar almacenando los datos en caché, la app aún no revisa la caché para ver si tiene datos de estado del tiempo.
                            </p>
                            
                            <h3>
                              Cómo realizar las solicitudes
                            </h3>
                            
                            <p>
                              Como se mencionó anteriormente, la app debe emitir dos solicitudes asincrónicas: una al caché y otra a la red. La app usa el objeto <code>caches</code> disponible en <code>window</code> para acceder al caché y recuperar los datos más recientes. Este es un excelente ejemplo de mejora progresiva ya que el objeto <code>caches</code> puede no estar disponible en todos los navegadores, y, si no lo está, la solicitud de red debería funcionar.
                            </p>
                            
                            <p>
                              Para hacer esto, es necesario:
                            </p>
                            
                            <ol start="1">
                              <li>
                                Revisa si el objeto <code>caches</code> está disponible en el objeto global <code>window</code>.
                              </li>
                              
                              <li>
                                Solicita datos de la caché.
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                Cuando se ejecuta por primera vez, tu app debería mostrarle al usuario inmediatamente el pronóstico climático de <code>initialWeatherForecast</code>.
                              </li>
                            </ul>
                            
                            <ol start="3">
                              <li>
                                Solicita datos del servidor.
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                Una vez que se ha eliminado el registro de un service worker, puede permanecer enumerado hasta que se cierre la ventana del navegador que lo contiene.
                              </li>
                              <li>
                                Si hay varias ventanas de tu app abiertas, el nuevo service worker no tendrá efecto hasta que se hayan vuelto a cargar y se hayan actualizado al último service worker.
                              </li>
                            </ul>
                            
                            <h4>
                              Obtén datos de la caché
                            </h4>
                            
                            <p>
                              A continuación, se debe comprobar si existe el objeto <code>caches</code> y se le deben solicitar datos actualizados. Encuentra el comentario <code>TODO add cache logic here</code> en <code>app.getForecast()</code> y agrega el código debajo del comentario.
                            </p>
                            
                            <pre><code>    if ('caches' in window) {
      /*
       * Check if the service worker has already cached this city's weather
       * data. If the service worker has the data, then display the cached
       * data while the app fetches the latest data.
       */
      caches.match(url).then(function(response) {
        if (response) {
          response.json().then(function updateFromCache(json) {
            var results = json.query.results;
            results.key = key;
            results.label = label;
            results.created = json.query.created;
            app.updateForecastCard(results);
          });
        }
      });
    }
</code></pre>
                            
                            <p>
                              Nuestra app de estado del tiempo ahora hace dos solicitudes sincrónicas de datos, una desde <code>cache</code> y una vía un XHR. Si hay datos en la caché, se devolverán y se mostrarán muy rápidamente (decenas de milisegundos), y actualizarán la tarjeta solo si el XHR sigue estando destacado. Luego, cuando el XHR responda, la tarjeta se actualizará con los datos más nuevos directamente de la weather API.
                            </p>
                            
                            <p>
                              Observa cómo la solicitud de caché y la solicitud de XHR finalizan con una llamada a actualización de la tarjeta de pronóstico climático. ¿Cómo sabe la app si está mostrando los datos más nuevos? Esto se controla en el siguiente código de <code>app.updateForecastCard</code>:
                            </p>
                            
                            <pre><code>    var cardLastUpdatedElem = card.querySelector('.card-last-updated');
    var cardLastUpdated = cardLastUpdatedElem.textContent;
    if (cardLastUpdated) {
      cardLastUpdated = new Date(cardLastUpdated);
      // Bail if the card has more recent data then the data
      if (dataLastUpdated.getTime() &lt; cardLastUpdated.getTime()) {
        return;
      }
    }
</code></pre>
                            
                            <p>
                              Cada vez que se actualiza una tarjeta, la app almacena la marca de tiempo de los datos en un atributo oculto de la tarjeta. La app se retira si la marca de tiempo que ya existe en la tarjeta es más nueva que los datos que se pasaron a la función.
                            </p>
                            
                            <h3>
                              Probar
                            </h3>
                            
                            <p>
                              Ahora, la app debería funcionar por completo sin conexión. Guarda alguna ciudades y presiona el botón para actualizar en la app para obtener datos de estado del tiempo más nuevos, luego corta la conexión y vuelve a cargar la página.
                            </p>
                            
                            <p>
                              Luego ve al subpanel <strong>Cache Storage</strong> del panel <strong>Application</strong> de DevTools. Amplía la sección y deberías ver el nombre del shell de tu app y los datos de caché enumerados a la izquierda. Abrir la caché de datos debería mostrar los datos almacenados de cada ciudad.
                            </p>
                            
                            <p>
                              <img src="img/cf095c2153306fa7.png" alt="cf095c2153306fa7.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-07/">Vínculo</a>
                            </p>
                            
                            <h2>
                              Soporta la integración nativa
                            </h2>
                            
                            <p>
                              A nadie le agrada tener que escribir URLs largas en un teclado móvil si no tiene necesidad de hacerlo. Con la función de la pantalla principal Add To, tus usuarios pueden escoger agregar un vínculo de atajo a su dispositivo de la misma manera en que instalarían una app nativa de una tienda, pero con mucha menos fricción.
                            </p>
                            
                            <h3>
                              Banners de instalación de aplicaciones web y Add to Homescreen para Chrome en Android
                            </h3>
                            
                            <p>
                              Los banners de instalación de apps web te dan la posibilidad de permitir que tus usuarios agreguen de manera rápida y fluida tu app web a sus pantallas de inicio. Esto hace más simple abrir y regresar a tu app. Agregar banners de instalación de apps es sencillo y Chrome se encarga de la mayor parte del trabajo pesado. Solo tenemos que incluir un archivo de manifiesto de app web con detalles de la app.
                            </p>
                            
                            <p>
                              Chrome luego usa un conjunto de criterios que incluyen el uso de un service worker, estado de SSL y visita algoritmos heurísticos de frecuencia para saber cuándo mostrar el banner. Además, un usuario puede agregarlo en forma manual a través del botón del menú "Add to Home Screen" en Chrome.
                            </p>
                            
                            <h4>
                              Declara un manifiesto de las apps con un archivo <code>manifest.json</code>
                            </h4>
                            
                            <p>
                              El manifiesto de las apps web es un archivo JSON simple que te proporciona a ti, el programador, la capacidad de controlar cómo se le muestra tu app al usuario en las áreas en las que espera ver apps (por ejemplo, la pantalla de inicio para móvil), dirigir lo que el usuario puede ejecutar y, lo que es más importante, cómo puede hacerlo.
                            </p>
                            
                            <p>
                              Al usar el manifiesto para aplicaciones web, tu aplicación web puede:
                            </p>
                            
                            <ul>
                              <li>
                                Si la solicitud del servidor se sigue destacando, actualiza la app con los datos almacenados en caché.
                              </li>
                              <li>
                                Be launched in full-screen mode on Android with no URL bar
                              </li>
                              <li>
                                Control the screen orientation for optimal viewing
                              </li>
                              <li>
                                Define a "splash screen" launch experience and theme color for the site
                              </li>
                              <li>
                                Track whether you're launched from the home screen or URL bar
                              </li>
                            </ul>
                            
                            <p>
                              Crea un archivo llamado <code>manifest.json</code> en tu carpeta <code>work</code> y copia/pega el siguiente contenido:
                            </p>
                            
                            <pre><code>{
  "name": "Weather",
  "short_name": "Weather",
  "icons": [{
    "src": "images/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    }, {
      "src": "images/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    }],
  "start_url": "/index.html",
  "display": "standalone",
  "background_color": "#3E4EB8",
  "theme_color": "#2F3BA2"
}
</code></pre>
                            
                            <p>
                              El manifiesto es compatible con una variedad de íconos que sirven para distintos tamaños de pantallas. Cuando se escribió esto, Chrome y Opera Mobile, los únicos navegadores compatibles con manifiestos de la app web, no usaban nada de tamaño menor a 192 px.
                            </p>
                            
                            <p>
                              Una sencilla forma de rastrear cómo se lanza la app es agregar una cadena de consulta al parámetro <code>start_url</code> y usar una suite de análisis para rastrear la cadena de consulta. Si usas este método, recuerda actualizar la lista de archivos almacenados en caché por el shell de la app, para asegurarte de que el archivo que tiene la cadena de consulta se almacene en caché.
                            </p>
                            
                            <h4>
                              Notifica al navegador sobre tu archivo de manifiesto
                            </h4>
                            
                            <p>
                              Ahora agrega la siguiente línea al final del elemento <code>&lt;head&gt;</code> en tu archivo <code>index.html</code>:
                            </p>
                            
                            <pre><code>&lt;link rel="manifest" href="/manifest.json"&gt;
</code></pre>
                            
                            <h4>
                              Prácticas recomendadas
                            </h4>
                            
                            <ul>
                              <li>
                                Guarda los datos para tener un rápido acceso después.
                              </li>
                              <li>
                                Actualiza la app con los datos nuevos del servidor.
                              </li>
                              <li>
                                Define icon sets for different density screens. Chrome will attempt to use the icon closest to 48dp, for example, 96px on a 2x device or 144px for a 3x device.
                              </li>
                              <li>
                                Remember to include an icon with a size that is sensible for a splash screen and don't forget to set the <code>background_color</code>.
                              </li>
                            </ul>
                            
                            <p>
                              Lecturas adicionales:
                            </p>
                            
                            <p>
                              <a href="/web/fundamentals/engage-and-retain/simplified-app-installs/">Uso de banners de instalación de app</a>
                            </p>
                            
                            <h3>
                              Elementos de Add to Homescreen para Safari en iOS
                            </h3>
                            
                            <p>
                              En tu <code>index.html</code>, agrega lo siguiente al final del elemento <code>&lt;head&gt;</code>:
                            </p>
                            
                            <pre><code>  &lt;!-- Add to home screen for Safari on iOS --&gt;
  &lt;meta name="apple-mobile-web-app-capable" content="yes"&gt;
  &lt;meta name="apple-mobile-web-app-status-bar-style" content="black"&gt;
  &lt;meta name="apple-mobile-web-app-title" content="Weather PWA"&gt;
  &lt;link rel="apple-touch-icon" href="images/icons/icon-152x152.png"&gt;
</code></pre>
                            
                            <h3>
                              Ícono de mosaico para Windows
                            </h3>
                            
                            <p>
                              En tu <code>index.html</code>, agrega lo siguiente al final del elemento <code>&lt;head&gt;</code>:
                            </p>
                            
                            <pre><code>  &lt;meta name="msapplication-TileImage" content="images/icons/icon-144x144.png"&gt;
  &lt;meta name="msapplication-TileColor" content="#2F3BA2"&gt;
</code></pre>
                            
                            <h3>
                              Probar
                            </h3>
                            
                            <p>
                              En esta sección, te mostraremos alguna formas de probar el manifiesto de tu app web.
                            </p>
                            
                            <p>
                              La primera forma es con DevTools. Abre el subpanel <strong>Manifest__del panel __Application</strong>. Si has agregado correctamente la información del manifiesto, podrás verla analizada y presentada en un formato legible en este subpanel.
                            </p>
                            
                            <p>
                              También puedes probar la función para agregar a la pantalla principal desde este subpanel. Haz clic en el botón <strong>Add to homescreen</strong>. Deberías ver el mensaje "add this site to your shelf" debajo de tu barra de URL, como en la siguiente captura de pantalla.
                            </p>
                            
                            <p>
                              <img src="img/cbfdd0302b611ab0.png" alt="cbfdd0302b611ab0.png" />
                            </p>
                            
                            <p>
                              Este es el equivalente de escritorio de la función móvil de agregar a la pantalla principal. Puedes activar esta solicitud en el escritorio con éxito, luego puedes estar seguro de que los usuarios de dispositivos móviles puedan agregar tu app a sus dispositivos.
                            </p>
                            
                            <p>
                              La segunda forma de probarlo es vía Web Server for Chrome. Con este acercamiento, puedes exponer tu servidor de desarrollo local (en tu computadora de escritorio o laptop) a otras computadoras, y luego puedes acceder a tu progressive web app desde un dispositivo móvil real.
                            </p><aside class="warning">

<p>Opening up a port for remote access is handy for testing this step but may be blocked by your computer's firewall rules or network administrator. Opening ports for remote access is generally not a good thing to leave running on your computer. So, for security reasons, when you've completed testing this step, disable the <code>Accessible on local network</code> option and restart your web server.</p>

</aside> 
                            
                            <p>
                              En el diálogo de configuración de Web Server for Chrome, selecciona la opción <code>Accessible on local network</code>:
                            </p>
                            
                            <p>
                              <img src="img/81347b12f83e4291.png" alt="81347b12f83e4291.png" />
                            </p>
                            
                            <p>
                              Coloca el servidor web en <code>STOPPED</code> y de nuevo en <code>STARTED</code>. Verás una nueva URL que se puede usar para acceder a tu app en forma remota.
                            </p>
                            
                            <p>
                              Ahora, accede a tu sitio desde un dispositivo móvil, usando la nueva URL.
                            </p>
                            
                            <p>
                              Verás errores de service worker en la consola cuando hagas una prueba de esta forma porque el service worker no se emite a través de HTTPS.
                            </p>
                            
                            <p>
                              Usando Chrome desde un dispositivo Android, intenta agregar la app a la pantalla principal y verificar que la pantalla de inicio aparezca correctamente y se usen los íconos adecuados.
                            </p>
                            
                            <p>
                              En Safari e Internet Explorer, también puedes agregar la app en forma manual a tu pantalla principal.
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-08/">Vínculo</a>
                            </p>
                            
                            <h2>
                              Impleméntala en un host seguro y festeja
                            </h2>
                            
                            <p>
                              El último paso es implementar nuestra app de estado del tiempo en un servidor compatible con HTTPS. Si aún no tienes uno, el acercamiento más sencillo (y gratuito) es el uso del hosting de contenido estático de Firebase. Es muy fácil de usar, proporciona contenido a través de HTTPS y cuenta con el respaldo de una CDN global.
                            </p>
                            
                            <h3>
                              Crédito adicional: minifica e integra CSS
                            </h3>
                            
                            <p>
                              Hay algo más que deberías tener en cuenta: la minificación de los estilos claves y el alineamiento de los mismo directamente en <code>index.html</code>. <a href="/speed">Page Speed Insights</a> recomienda emitir el contenido de la mitad superior de la página en los primeros 15k bytes de la solicitud.
                            </p>
                            
                            <p>
                              Observa el nivel de reducción máxima que puedes lograr para la solicitud inicial con todo integrado.
                            </p>
                            
                            <p>
                              Lecturas adicionales: <a href="/speed/docs/insights/rules">Reglas de PageSpeed Insight</a>
                            </p><aside class="key-point">

<p>This step requires you to have  <a href="https://docs.npmjs.com/getting-started/installing-node">Node &#x26; NPM</a> installed on your system. If it's not, you can use any other hosting provider that supports HTTP<strong>S</strong>. We've used Firebase because it automatically redirects users from HTTP to HTTP<strong>S</strong>. If you use a different provider, be sure they're always redirects to HTTP<strong>S</strong>.</p>

</aside> 
                            
                            <h3>
                              Realiza implementaciones en Firebase
                            </h3>
                            
                            <p>
                              Si eres nuevo en Firebase, primero deberás crear tu cuenta e instalar algunas herramientas.
                            </p>
                            
                            <ol start="1">
                              <li>
                                Crea una cuenta de Firebase en <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a>
                              </li>
                              
                              <li>
                                Instala las herramientas de Firebase vía npm: <code>npm install -g firebase-tools</code>
                              </li>
                            </ol>
                            
                            <p>
                              Una vez que se haya creado tu cuenta y hayas iniciado sesión, estarás listo para la implementación.
                            </p>
                            
                            <ol start="1">
                              <li>
                                Crea una nueva app en <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a>
                              </li>
                              
                              <li>
                                Si no has iniciado sesión recientemente en las herramientas de Firebase, actualiza tus credenciales: <code>firebase login</code>
                              </li>
                              
                              <li>
                                Inicia tu app y proporciona el directorio (probablemente <code>work</code>) donde completaste la ubicación de la app: <code>firebase init</code>
                              </li>
                              
                              <li>
                                Finalmente, implementa la app en Firebase: <code>firebase deploy</code>
                              </li>
                              
                              <li>
                                Festeja. ¡Eso es todo! Tu app se implementará en el dominio: <code>https://YOUR-FIREBASE-APP.firebaseapp.com</code>
                              </li>
                            </ol>
                            
                            <p>
                              Lecturas adicionales: <a href="https://www.firebase.com/docs/hosting/guide/">Guía de Hosting de Firebase</a>
                            </p>
                            
                            <h3>
                              Probar
                            </h3>
                            
                            <ul>
                              <li>
                                tener una presencia destacada en la pantalla de inicio de Android del usuario;
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/final/">Vínculo</a>
                            </p>
                            
                            <h2>
                              ¿Encontraste un problema o tienes comentarios? {: .hide-from-toc }
                            </h2>
                            
                            <p>
                              Ayúdanos a que nuestros code labs sean mejores enviando un <a href="https://github.com/googlecodelabs/your-first-pwapp/issues">problema</a> hoy. ¡Gracias!
                            </p>