project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: A maioria dos navegadores pode obter acesso ao microfone do usuário.

{# wf_updated_on: 2016-08-22 #} {# wf_published_on: 2016-08-23 #}

# Como gravar áudio do usuário {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

Atualmente, muitos navegadores têm o recurso de acessar inserções de dados de áudio e vídeo do usuário. No entanto, dependendo do navegador, essa pode ser uma experiência dinâmica e integrada ou delegada a outro aplicativo do dispositivo do usuário.

## Comece com o simples e evolua aos poucos

A coisa mais fácil a se fazer é simplesmente solicitar ao usuário um arquivo pré-gravado. Faça isso criando um elemento "input" de arquivo simples e adicionando um filtro `accept` que indique que só podemos aceitar arquivos de áudio e que o ideal seria obtê-los diretamente do microfone.

    <input type="file" accept="audio/*" capture="microphone">
    

Esse método funciona em todas as plataformas. Em computadores, o usuário poderá carregar um arquivo do sistema de arquivos (ignorando `capture="microphone"`). No Safari para iOS, ele abrirá o aplicativo de microfone, permitindo a gravação do áudio e retornará o usuário à página da web. No Android, ele dará ao usuário a possibilidade e escolher um aplicativo para gravar o áudio antes de retorná-lo à página da web.

Quando o usuário terminar a gravação e for redirecionado ao site, você precisa obter os dados do arquivo de alguma forma. É possível obter acesso rápido anexando um evento `onchange` ao elemento "input" e lendo a propriedade `files` do objeto do evento.

<pre class="prettyprint">&lt;input type="file" accept="audio/*" capture id="recorder">
&lt;audio id="player" controls>&lt;/audio>
&lt;script>
  var recorder = document.getElementById('recorder');
  var player = document.getElementById('player');

  recorder.addEventListener('change', function(e) {
    var file = e.target.files[0];
    // Do something with the audio file.
    player.src =  URL.createObjectURL(file);
  });
&lt;/script>
</pre>

Depois que você tiver acesso ao arquivo, pode fazer o que quiser com ele. Por exemplo, é possível:

* Anexá-lo diretamente a um elemento `<audio>` para poder reproduzi-lo
* Baixar no dispositivo do usuário
* Carregá-lo em um servidor anexando-o a uma `XMLHttpRequest`
* Passá-lo pela Web Audio API e aplicar filtros a ele

Embora usar o método do elemento "input" para obter acesso aos dados de áudio seja algo universal, é a opção menos vantajosa. Queremos obter acesso de verdade ao microfone e oferecer uma experiência bacana diretamente na página.

## Acesse o microfone de forma interativa

Os navegadores modernos podem ter ligação direta com o microfone, permitindo criar experiências totalmente integradas dom a página web, sem o usuário precisar sair do navegador em nenhum momento.

### Obtenha acesso ao microfone

Podemos acessar o microfone diretamente usando uma API na especificação WebRTC chamada `getUserMedia()`. `getUserMedia()` pedirá ao usuário autorização para acessar os microfones e câmeras conectados.

Se receber a autorização, a API retornará um `Stream` que conterá os dados da câmera e/ou do microfone e poderemos anexá-los a um elemento `<audio>`, a um `AudioContext` da Web Audio ou salvá-los usando a `MediaRecorder` API.

Para obter dados do microfone, basta definir `audio: true` no objeto de restrições passado à API de `getUserMedia()`

<pre class="prettyprint">&lt;audio id="player" controls>&lt;/audio>
&lt;script>
  var player = document.getElementById('player');

  var handleSuccess = function(stream) {
    if (window.URL) {
      player.src = window.URL.createObjectURL(stream);
    } else {
      player.src = stream;
    }
  };

  navigator.mediaDevices.getUserMedia({ audio: true, video: false })
      .then(handleSuccess);
&lt;/script>
</pre>

Por si só, isso não é tão útil. Tudo que podemos fazer e obter os dados de áudio e reproduzi-lo.

    navigator.mediaDevices.enumerateDevices().then((devices) => {
      devices = devices.filter((d) => d.kind === 'audioinput');
    });
    

Para acessar os dados brutos do microfone, temos que pegar o fluxo criado por `getUserMedia()` e usar a Web Audio API para processar os dados. A Web Audio API é uma API simples que coleta as fontes de interação e as conecta a nós que podem processar os dados de áudio (ajustar ganho etc.) e, por fim, conecta-os a alto-falantes para que o usuário possa ouvir.

    navigator.mediaDevices.getUserMedia({
      audio: {
        deviceId: devices[0].deviceId
      }
    });
    

Um dos nós a que você pode conectar é um `ScriptProcessorNode`. Esse nó emitirá um evento `onaudioprocess` sempre que o buffer de áudio encher e você precisar processá-lo. Nesse momento, você pode gravar os dados no seu próprio buffer e salvá-lo para usar depois.

### Acesse os dados brutos do microfone

Os dados armazenados nos buffers são os dados brutos do microfone, e você tem diversas possibilidades do que fazer com os dados:

A forma mais fácil de salvar os dados do microfone é usando a API `MediaRecorder`.

<pre class="prettyprint">&lt;script>
  var handleSuccess = function(stream) {
    <strong>var context = new AudioContext();
    var source = context.createMediaStreamSource(stream);
    var processor = context.createScriptProcessor(1024, 1, 1);

    source.connect(processor);
    processor.connect(context.destination);

    processor.onaudioprocess = function(e) {
      // Do something with the data, i.e Convert this to WAV
      console.log(e.inputBuffer);
    };</strong>
  };

  navigator.mediaDevices.getUserMedia({ audio: true, video: false })
      .then(handleSuccess);
&lt;/script>
</pre>

A API `MediaRecorder` pegará o fluxo criado por `getUserMedia` e salvará os dados que estão no fluxo direcionado ao destino que você escolheu.

* Carregá-los direto no servidor.
* Armazenar localmente
* Converter para um formato de arquivo dedicado, como WAV e salvá-los nos servidores ou localmente

### Salve os dados do microfone

No nosso caso, estamos salvando os dados diretamente em uma matriz que poderemos transformar em um `Blob` depois que, em seguida, poderá ser usado para salvar no nosso servidor web ou diretamente no espaço de armazenamento do dispositivo do usuário.

Se o usuário ainda não tiver concedido acesso ao microfone para o seu site, no instante em que você chamar `getUserMedia`, o navegador pedirá que o usuário autorize o seu site a acessar o microfone.

<pre class="prettyprint">&lt;a id="download">Download&lt;/a>
&lt;button id="stop">Stop&lt;/button>
<script>
  let shouldStop = false;
  let stopped = false;
  const downloadLink = document.getElementById('download');
  const stopButton = document.getElementById('stop');

  stopButton.addEventListener('click', function() {
    shouldStop = true;
  });

  var handleSuccess = function(stream) {
    const options = {mimeType: 'audio/webm'};
    const recordedChunks = [];
    <strong>const mediaRecorder = new MediaRecorder(stream, options);

    mediaRecorder.addEventListener('dataavailable', function(e) {
      if (e.data.size > 0) {
        recordedChunks.push(e.data);
      }

      if(shouldStop === true && stopped === false) {
        mediaRecorder.stop();
        stopped = true;
      }
    });

    mediaRecorder.addEventListener('stop', function() {
      downloadLink.href = URL.createObjectURL(new Blob(recordedChunks));
      downloadLink.download = 'acetest.wav';
    });

    mediaRecorder.start();</strong>
  };

  navigator.mediaDevices.getUserMedia({ audio: true, video: false })
      .then(handleSuccess);

&lt;/script>
</pre>



<p>
  Os usuários odeiam receber solicitações de permissão de acesso a dispositivos importantes do seu aparelho e
  muitas vezes bloqueiam a solicitação ou a ignoram se não 
  entendem por que a solicitação foi criada. A abordagem mais indicada
  é só pedir acesso ao microfone na primeira vez em que ele for necessário. Quando o usuário
  concede acesso, não recebe mais solicitação de permissão de acesso, porém, se ele não autorizar, 
  você não poderá solicitar a permissão do usuário de novo.
</p>



<h2>
  Solicite autorização para usar o microfone com responsabilidade
</h2>



<p>
  Aviso: pedir acesso ao microfone durante o carregamento de uma página fará com que a maioria dos usuários não conceda permissão de acesso ao microfone.
</p>



<p>
  A API <code>getUserMedia</code> não fornece a informação sobre se você já tem
  acesso ao microfone. Isso é um problema: para oferecer uma IU bacana
  para fazer o usuário conceder acesso ao microfone a você, você tem que pedir
  acesso ao microfone.
</p>



<p>
  Esse problema pode ser resolvido em alguns navegadores por meio do uso da Permission API. A API
  <code>navigator.permission</code> permite consultar o estado da competência para
  acessar APIs específicas sem ter que pedir novamente.
</p>



<h3>
  Use a API de permissões para verificar se você já tem acesso
</h3>



<p>
  Para verificar se você tem acesso ao microfone do usuário, passe
  <code>{name: 'microphone'}</code> no método de consulta. Ele retornará:
</p>



<p>
  Agora você pode verificar rapidamente se precisa alterar sua interface
  do usuário para oferecer as ações que o usuário precisa tomar.
</p>



<p>
  {# wf_devsite_translation #}
</p>



<ul>
  <li>
    <code>granted</code> &mdash; o usuário já deu autorização de acesso ao microfone a você;
  </li>
  
  
  <li>
    <code>prompt</code> &mdash; o usuário não deu autorização de acesso a você e receberá uma solicitação quando 
    você chamar <code>getUserMedia</code>;
  </li>
  
  
  <li>
    <code>denied</code> &mdash; o sistema ou o usuário bloqueou explicitamente o acesso ao
    microfone e você não poderá pedir acesso a ele.
  </li>
  
</ul>



<p>
  And you can now check quickly check to see if you need to alter your user
  interface to accommodate the actions that the user needs to take.
</p>



<pre><code>navigator.permissions.query({name:'microphone'}).then(function(result) {
  if (result.state == 'granted') {

  } else if (result.state == 'prompt') {

  } else if (result.state == 'denied') {

  }
  result.onchange = function() {

  };
});
</code></pre>



<h2>
  Feedback {: #feedback }
</h2>



<p>
  {% include "web/_shared/helpful.html" %}
</p>