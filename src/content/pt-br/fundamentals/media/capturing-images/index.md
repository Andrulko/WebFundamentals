project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: A maioria dos navegadores pode obter acesso à câmera do usuário.

{# wf_updated_on: 2016-08-22 #} {# wf_published_on: 2016-08-23 #}

# Como capturar uma imagem do usuário {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

Atualmente, muitos navegadores têm o recurso de acessar inserções de dados de áudio e vídeo do usuário. No entanto, dependendo do navegador, essa pode ser uma experiência dinâmica e integrada ou delegada a outro aplicativo do dispositivo do usuário.

## Comece com o simples e evolua aos poucos

A coisa mais fácil a se fazer é simplesmente solicitar ao usuário um arquivo pré-gravado. Faça isso criando um elemento "input" de arquivo simples e adicionando um filtro `accept` que indique que só podemos aceitar arquivos de imagem e que o ideal seria obtê-los diretamente da câmera.

### Capturar um único quadro

Esse método funciona em todas as plataformas. Em computadores, o usuário poderá carregar um arquivo de imagem do sistema de arquivos. No Safari para iOS, esse método abrirá o aplicativo da câmera, permitindo capturar uma imagem e enviá-la para a página da web. No Android, esse método dará ao usuário a opção de definir que aplicativo será usado para capturar a imagem antes de enviá-la para a página da web.

Em seguida, os dados podem ser ligados a um `<form>` ou manipulados com JavaScript pela detecção de um evento `onchange` no elemento "input" e, depois, lendo a propriedade `files` do `target` do evento.

### Obtenha acesso à câmera

Obter acesso ao arquivo de imagem é bem simples.

    <input type="file" accept="image/*" capture>
    

Depois que você tiver acesso ao arquivo, pode fazer o que quiser com ele. Por exemplo, é possível:

![](images/ios-chooser.png) ![](images/android-chooser.png)

Os navegadores modernos podem obter acesso direto à câmera, o que permite criar experiências totalmente integradas com a página web para que o usuário não precise sair do navegador em nenhum momento.

<pre class="prettyprint">&lt;input type="file" accept="image/*" id="file-input">
&lt;script>
  const fileInput = document.getElementById('file-input');

  fileInput.addEventListener('change', (e) => doSomethingWithFiles(e.target.files));
&lt;/script>
</pre>

Aviso: acesso direto à câmera é um recurso poderoso e, por isso, requer autorização do usuário e o seu site precisa estar em uma origem segura (HTTPS).

Podemos acessar a câmera e o microfone diretamente usando uma API na especificação WebRTC chamada `getUserMedia()`. Assim, enviamos uma solicitação de acesso às câmeras e microfones ao usuário.

    <video id="player" controls autoplay></video>
    <script>  
      var player = document.getElementById('player');
    
      var handleSuccess = function(stream) {
        player.srcObject = stream;
      };
    
      navigator.mediaDevices.getUserMedia({video: true})
          .then(handleSuccess);
    </script>
    

Se a resposta for positiva, a API retornará um `MediaStream` que contém dados da câmera e poderemos anexá-lo a um elemento `<video>` e reproduzi-lo para ver uma prévia em tempo real ou anexá-lo a um `<canvas>` para obter uma imagem instantânea.

Para obter dados da câmera, basta definir `video: true` no objeto de restrições passado à API de `getUserMedia()`

### Tire uma foto com a câmera

Por si só, isso não é tão útil. Tudo que podemos fazer e obter os dados de vídeo e reproduzi-lo.

Para acessar os dados brutos da câmera, temos que pegar o fluxo criado por `getUserMedia()` e processar os dados. Ao contrário da `Web Audio`, não há uma API de processamento de fluxo dedicada para vídeos na web, por isso temos que recorrer a um pouquinho do jeitinho brasileiro para capturar uma foto da câmera do usuário.

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

Esse processo acontece assim:

Pronto!

Quando tiver os dados da câmera armazenados no "canvas", você vai poder fazer muitas coisas com eles. Como:

### Pare de transmitir da câmera quando não for necessário

É uma boa prática parar de usar a câmera quando ela não é necessária. Isso não só economiza bateria e capacidade de processamento, mas também faz com que os usuários confiem no seu aplicativo.

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

Para parar de acessar a câmera, basta chamar `stop()` em cada trilha de vídeo do fluxo retornado por `getUserMedia()`.

Se o usuário ainda não tiver concedido acesso à câmera para o seu site, no instante em que você chamar `getUserMedia`, o navegador pedirá que o usuário autorize o seu site a acessá-la.

Os usuários odeiam receber solicitações de permissão de acesso a dispositivos importantes do seu aparelho e muitas vezes bloqueiam a solicitação ou a ignoram se não entendem por que a solicitação foi criada. A abordagem mais indicada é só pedir acesso à câmera na primeira vez em que ela for necessária. Depois que o usuário conceder acesso, ele não receberá mais solicitações de permissão de acesso. Porém, se o usuário não der a permissão, você não poderá acessar novamente, a menos que ele mude manualmente as configurações de permissão da câmera.

### Handling a FileList object

Aviso: pedir acesso à câmera durante o carregamento de uma página fará com que a maioria dos usuários não conceda permissão de acesso a ela.

Saiba mais sobre a implementação em navegadores para dispositivo móvel e computador:

Recomendamos também usar o paliativo [adapter.js](https://github.com/webrtc/adapter) para proteger os aplicativos de mudanças na especificação WebRTC e diferenças de prefixo.

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

- Anexá-lo diretamente a um elemento `<canvas>` para poder manipulá-lo
- Baixar no dispositivo do usuário
- Carregá-lo em um servidor anexando-o a uma `XMLHttpRequest`

## Acesse a câmera de forma interativa

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

1. Crie um objeto "canvas" que armazenará o quadro da câmera
2. Obtenha acesso ao fluxo da câmera
3. Anexe-o a um elemento "video"
4. Quando quiser capturar um quadro preciso, adicione os dados do elemento "video" a um objeto "canvas" usando `drawImage()`.

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

- Carregá-los direto no servidor.
- Armazenar localmente
- Aplicar efeitos modernos à imagem

## Peça autorização para usar a câmera com responsabilidade

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

## Compatibilidade

More information about mobile and desktop browser implementation:

- [srcObject](https://www.chromestatus.com/feature/5989005896187904)
- [navigator.mediaDevices.getUserMedia()](https://www.chromestatus.com/features/5755699816562688)

We also recommend using the [adapter.js](https://github.com/webrtc/adapter) shim to protect apps from WebRTC spec changes and prefix differences.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}