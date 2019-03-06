project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: La mayoría de los navegadores podrán tener acceso a la cámara del usuario.

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-08-23 #}

# Capturar una imagen desde el usuario {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

Ahora muchos navegadores tienen la habilidad de acceder a la entrada de audio y video desde el usuario. Sin embargo, según el navegador puede ser una experiencia en línea y totalmente dinámica o puede ser delegada a otra app en el dispositivo del usuario.

## Comenzar de modo simple y progresivo

Lo más sencillo es simplemente pedirle al usuario un archivo registrado anteriormente. Puedes hacerlo creando un elemento simple de entrada de archivo y agregando un filtro `accept` que indica que solo podemos aceptar archivos de imágenes e idealmente los obtendremos directamente desde la cámara.

### Capturar en un solo fotograma

Este método funciona en todas las plataformas. En el escritorio se le solicita al usuario cargar un archivo de imagen desde el sistema de archivos. En Safari para iOS, este método abre la app de la cámara, lo cual te permite capturar una imagen y luego enviarla devuelta a la página web; en Android este método le permite al usuario elegir qué app quiere usar para capturar la imagen antes de enviarla devuelta a la página web.

Los datos se adjuntan a un `<form>` o se manipulan con JavaScript detectando a un evento `onchange` en el elemento de entrada y luego leyendo la propiedad `files` del evento `target`.

### Adquirir acceso a la cámara

La obtención de acceso al archivo de imagen es sencilla.

    <input type="file" accept="image/*" capture>
    

Una vez que obtienes acceso al archivo, puedes hacer lo que quieras con él. Por ejemplo, puedes:

![](images/ios-chooser.png) ![](images/android-chooser.png)

Los navegadores modernos pueden obtener acceso directo a las cámaras, lo cual nos permite compilar experiencias que estén totalmente integradas con la página web, de modo que el usuario nunca tenga que abandonar el navegador.

<pre class="prettyprint">&lt;input type="file" accept="image/*" id="file-input">
&lt;script>
  const fileInput = document.getElementById('file-input');

  fileInput.addEventListener('change', (e) => doSomethingWithFiles(e.target.files));
&lt;/script>
</pre>

Warning: El acceso directo a la cámara es una poderosa función que solicita el consentimiento del usuario y tu sitio tiene que estar en un origen seguro (HTTPS).

Podemos acceder de modo directo a una cámara y a un micrófono usando una API en la especificación WebRTC llamada `getUserMedia()`. Con esto se le solicita al usuario acceder a sus micrófonos y cámaras conectados.

    <video id="player" controls autoplay></video>
    <script>  
      var player = document.getElementById('player');
    
      var handleSuccess = function(stream) {
        player.srcObject = stream;
      };
    
      navigator.mediaDevices.getUserMedia({video: true})
          .then(handleSuccess);
    </script>
    

Si tiene éxito, la API muestra un `MediaStream` que contiene datos de la cámara y luego podemos o bien adjuntarlo a un elemento `<video>` y ejecutarlo para mostrar una vista previa en tiempo real o podemos adjuntarlo a un `<canvas>` para obtener un resumen.

Para obtener datos desde la cámara, simplemente fijamos `video: true` en el objeto de restricciones que se pasa a la API `getUserMedia()`.

### Obtener un resumen desde la cámara

Por sí mismo, no es tan útil. Todo lo que podemos hacer es tomar los datos del video y reproducirlo.

Para acceder a los datos sin procesar desde la cámara, tenemos que tomar la transmisión creada por `getUserMedia()` y procesar los datos. A diferencia de `Web Audio`, no hay una API de procesamiento de transmisión exclusiva para videos en la web, de modo que tenemos que recurrir a un poco de piratería informática para capturar un resumen desde la cámara del usuario.

<pre class="prettyprint">&lt;div id="target">You can drag an image file here&lt;/div>
&lt;script>
  const target = document.getElementById('target');

  target.addEventListener('drop', (e) => {
    e.stopPropagation();
    e.preventDefault();

    doSomethingWithFiles(e.dataTransfer.files);
  });

  target.addEventListener('dragover', (e) => {
    e.stopPropagation();
    e.preventDefault();

    e.dataTransfer.dropEffect = 'copy';
  });
&lt;/script>
</pre>

El proceso es de la siguiente manera:

Listo.

Una vez que tienes datos de la cámara almacenados en los lienzos puedes hacer muchas cosas con estos. Puedes:

### Detener la transmisión desde la cámara cuando no sea necesaria

Es una buena práctica detener el uso de la cámara cuando ya no la necesitas. Esto no solo ahorra batería y potencia de procesamiento, sino que también le brinda confianza en tu app a los usuarios.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;button id="capture">Capture&lt;/button>
&lt;canvas id="snapshot" width=320 height=240>&lt;/canvas>
&lt;script>
  var player = document.getElementById('player'); 
  var snapshotCanvas = document.getElementById('snapshot');
  var captureButton = document.getElementById('capture');
  <strong>var videoTracks;</strong>

  var handleSuccess = function(stream) {
    // Attach the video stream to the video element and autoplay.
    player.srcObject = stream;
    <strong>videoTracks = stream.getVideoTracks();</strong>
  };

  captureButton.addEventListener('click', function() {
    var context = snapshot.getContext('2d');
    context.drawImage(player, 0, 0, snapshotCanvas.width, snapshotCanvas.height);

    <strong>// Stop all video streams.
    videoTracks.forEach(function(track) {track.stop()});</strong>
  });

  navigator.mediaDevices.getUserMedia({video: true})
      .then(handleSuccess);
&lt;/script>
</pre>

Para detener el acceso a la cámara puedes simplemente llamar a `stop()` en cada pista de video para la transmisión que devuelve `getUserMedia()`.

Si el usuario no le ha otorgado a tu sitio acceso a la cámara anteriormente entonces en el momento que llames a `getUserMedia` el navegador le solicitará al usuario que le de permiso a tu sitio para acceder a la cámara.

A los usuarios no les gusta que se les solicite acceso a los dispositivos poderosos en su equipo y con frecuencia bloquean esta solicitud o la ignoran si no comprenden el contexto por el cual se ha creado la solicitud. Es mejor solo pedir acceso a la cámara la primera vez que se necesita. Una vez que el usuario ha otorgado el acceso, no le volverán a preguntar. Sin embargo, si el usuario rechaza el acceso, no puedes obtener acceso otra vez, a menos que cambien de modo manual las configuraciones de permiso de la cámara.

### Handling a FileList object

Warning: Pedir el acceso a la cámara cuando se carga la página genera que la mayoría de tus usuarios rechacen el acceso a la misma.

Más información acerca de la implementación de dispositivos móviles y navegadores de escritorio:

También recomendamos usar la corrección [adapter.js](https://github.com/webrtc/adapter) para proteger las apps de los cambios de especificación WebRTC y las diferencias de prefijos.

<pre class="prettyprint">&lt;img id="output">
&lt;script>
  const output = document.getElementById('output');

  function doSomethingWithFiles(fileList) {
    let file = null;

    for (let i = 0; i &lt; fileList.length; i++) {
      if (fileList[i].type.match(/^image\//)) {
        file = fileList[i];
        break;
      }
    }

    if (file !== null) {
      output.src = URL.createObjectURL(file);
    }
  }
&lt;/script>
</pre>

{# wf_devsite_translation #}

Once you have access to the file you can do anything you want with it. For example, you can:

- Adjuntarlo directamente a un elemento `<canvas>` para que puedas manipularlo
- Descárgalo al dispositivo del usuario
- Cargarlo a un servidor adjuntándolo a una `XMLHttpRequest`

## Acceder a la cámara de manera interactiva

Now that you've covered your bases, it's time to progressively enhance!

Modern browsers can get direct access to cameras, allowing you to build experiences that are fully integrated with the web page, so the user need never leave the browser.

### Acquire access to the camera

You can directly access a camera and microphone by using an API in the WebRTC specification called `getUserMedia()`. This will prompt the user for access to their connected microphones and cameras.

Support for `getUserMedia()` is pretty good, but it isn't yet everywhere. In particular, it is not available in Safari 10 or lower, which at the time of writing is still the latest stable version. However, [Apple have announced](https://webkit.org/blog/7726/announcing-webrtc-and-media-capture/) that it will be available in Safari 11.

It's very simple to detect support, however.

    const supported = 'mediaDevices' in navigator;
    

Warning: Direct access to the camera is a powerful feature. It requires consent from the user, and your site MUST be on a secure origin (HTTPS).

When you call `getUserMedia()`, you need to pass in an object that describes what kind of media you want. These choices are called constraints. There are a several possible constraints, covering things like whether you prefer a front- or rear-facing camera, whether you want audio, and your preferred resolution for the stream.

To get data from the camera, however, you need just one constraint, and that is `video: true`.

If successful the API will return a `MediaStream` that contains data from the camera, and you can then either attach it to a `<video>` element and play it to show a real time preview, or attach it to a `<canvas>` to get a snapshot.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;script>
  const player = document.getElementById('player');

  const constraints = {
    video: true,
  };

  navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
      player.srcObject = stream;
    });
&lt;/script>
</pre>

By itself, this isn't that useful. All you can do is take the video data and play it back. If you want to get an image, you have to do a little extra work.

### Grab a snapshot

Your best supported option for getting an image is to draw a frame from the video to a canvas.

Unlike the Web Audio API, there isn't a dedicated stream processing API for video on the web so you have to resort to a tiny bit of hackery to capture a snapshot from the user's camera.

The process is as follows:

1. Crea un objeto lienzo que contenga el marco desde la cámara
2. Obtén acceso a la transmisión de la cámara
3. Adjúntalo a un elemento de video
4. Cuando quieres capturar un marco preciso, agrega los datos desde el elemento de video hasta un objeto de lienzo usando `drawImage()`.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;button id="capture">Capture&lt;/button>
&lt;canvas id="canvas" width=320 height=240>&lt;/canvas>
&lt;script>
  const player = document.getElementById('player');
  const canvas = document.getElementById('canvas');
  const context = canvas.getContext('2d');
  const captureButton = document.getElementById('capture');

  const constraints = {
    video: true,
  };

  captureButton.addEventListener('click', () => {
    // Draw the video frame to the canvas.
    context.drawImage(player, 0, 0, canvas.width, canvas.height);
  });

  // Attach the video stream to the video element and autoplay.
  navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
      player.srcObject = stream;
    });
&lt;/script>
</pre>

Once you have data from the camera stored in the canvas you can do many things with it. You could:

- Cargarlos directamente en el servidor
- Almacenarlos localmente
- Aplicar efectos extravagantes a la imagen

## Pedir permiso para usar la cámara de modo responsable

### Stop streaming from the camera when not needed

It is good practice to stop using the camera when you no longer need it. Not only will this save battery and processing power, it will also give users confidence in your application.

To stop access to the camera you can simply call `stop()` on each video track for the stream returned by `getUserMedia()`.

<pre class="prettyprint">&lt;video id="player" controls autoplay>&lt;/video>
&lt;button id="capture">Capture&lt;/button>
&lt;canvas id="canvas" width=320 height=240>&lt;/canvas>
&lt;script>
  const player = document.getElementById('player');
  const canvas = document.getElementById('canvas');
  const context = canvas.getContext('2d');
  const captureButton = document.getElementById('capture');

  const constraints = {
    video: true,
  };

  captureButton.addEventListener('click', () => {
    context.drawImage(player, 0, 0, canvas.width, canvas.height);

    <strong>// Stop all video streams.
    player.srcObject.getVideoTracks().forEach(track => track.stop());</strong>
  });

  navigator.mediaDevices.getUserMedia(constraints)
    .then((stream) => {
      // Attach the video stream to the video element and autoplay.
      player.srcObject = stream;
    });
&lt;/script>
</pre>

### Ask permission to use camera responsibly

If the user has not previously granted your site access to the camera then the instant that you call `getUserMedia()` the browser will prompt the user to grant your site permission to the camera.

Users hate getting prompted for access to powerful devices on their machine and they will frequently block the request, or they will ignore it if they don't understand the context for which the prompt has been created. It is best practice to only ask to access the camera when first needed. Once the user has granted access they won't be asked again. However, if the user rejects access, you can't get access again, unless they manually change camera permission settings.

Warning: Asking for access to the camera on page load will result in most of your users rejecting access to it.

## Compatibilidad

More information about mobile and desktop browser implementation:

- [srcObject](https://www.chromestatus.com/feature/5989005896187904)
- [navigator.mediaDevices.getUserMedia()](https://www.chromestatus.com/features/5755699816562688)

We also recommend using the [adapter.js](https://github.com/webrtc/adapter) shim to protect apps from WebRTC spec changes and prefix differences.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}