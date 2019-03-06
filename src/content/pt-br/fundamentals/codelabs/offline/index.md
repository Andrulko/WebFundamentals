project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Saiba como integrar um service worker a um aplicativo existente para que o aplicativo funcione off-line.

{# wf_auto_generated #} {# wf_updated_on: 2016-11-09 #} {# wf_published_on: 2016-01-01 #}

# Adicionar um Service Worker e Off-line ao seu App da Web {: .page-title }

{% include "web/_shared/contributors/paulkinlan.html" %}

## Obtenha o exemplo de código

![3ec8737c20a475df.png](img/3ec8737c20a475df.png)

In this codelab, you learn how to integrate a service worker into an existing application to make the application work offline. The application is called [Air Horner](https://airhorner.com). Click the horn and it makes a sound.

#### O que você aprenderá

* Como adicionar um service worker básico a um projeto existente
* Como simular o modo off-line e inspecionar e depurar um service worker com Chrome DevTools.
* Uma estratégia simples para o armazenamento em cache off-line.

#### O que será necessário

* Chrome 52 ou superior.
* Conhecimentos básicos sobre [Promises](/web/fundamentals/getting-started/primers/promises), Git e Chrome DevTools.
* O exemplo de código.
* Um editor de texto.
* Um servidor da Web local. Se deseja usar o servidor de Web descrito neste codelab, você precisa do Python instalado em sua linha de comando.

## Execute o exemplo de aplicativo

Neste codelab, você aprenderá a integrar um service worker a um aplicativo existente para que o aplicativo funcione off-line. O aplicativo se chama [Air Horner](https://airhorner.com). Clique na buzina e ela faz um som.

    $ git clone git@github.com:GoogleChrome/airhorn.git
    

Clone o repositório do GitHub pela linha de comando sobre SSH:

    $ git clone https://github.com/GoogleChrome/airhorn.git
    

## Teste o aplicativo

Ou HTTPS:

Primeiro, vejamos qual a aparência do aplicativo de exemplo acabado (dica: é incrível).

    $ git checkout master
    

Certifique-se de que você esteja na ramificação correta (final), verificando a ramificação `master`.

    $ cd app
    $ python -m SimpleHTTPServer 3000
    <aside class="key-point">

<p>This repository has one main folder <strong>"app"</strong>. This folder contains the static assets (HTML, CSS, and JavaScript) that you will use for this project.</p>

</aside> 

Execute o site a partir de um servidor de Web local. Você pode usar qualquer servidor de Web, mas pelo resto deste codelab vamos supor que esteja usando `SimpleHTTPServer` do Python na porta 3000, portanto o aplicativo estará disponível em `localhost:3000`.![3ec8737c20a475df.png](img/3ec8737c20a475df.png)

## Compile o aplicativo inicial

Abra o site no Chrome. Você deve ver: 

Clique na buzina. Ela deve fazer um som.

Agora, você vai simular ficar off-line usando Chrome DevTools.

![479219dc5f6ea4eb.png](img/479219dc5f6ea4eb.png)

After clicking the checkbox note the warning icon (yellow triangle with exclamation mark) next to the **Network** panel tab. This indicates that you're offline.

Após clicar na caixa de seleção, observe o ícone de aviso (triângulo amarelo com ponto de exclamação) ao lado da guia **Network** do painel. Isto indica que você está off-line.

Para provar que você está off-line, vá para <https://google.com>. Você deve ver a mensagem de erro "não há conexão com a Internet" do Chrome.

Agora, volte para o seu aplicativo. Embora esteja off-line, a página ainda deve atualizar totalmente. Você ainda deve conseguir usar a buzina.

## Registre um service worker no site

O motivo para ele funcionar off-line é a base deste codelab: suporte off-line com o service worker.

Agora você vai remover todo o suporte off-line do aplicativo e vai aprender como usar um service worker para adicionar o suporte off-line de volta ao aplicativo

    $ git checkout code-lab
    

Confira a versão "quebrada" do aplicativo que não tem o service worker implementado.

Volte ao painel **Application** do DevTools e desative a caixa de seleção **Offline**, para voltar a ficar online.

Execute a página. O aplicativo deve funcionar da forma esperada.

Agora, use DevTools para simular o modo off-line novamente (ativando a caixa de seleção **Offline** no painel **Application**). **Atenção!** Se não sabe muito sobre service workers, você está prestes a ver um comportamento inesperado.

O que você espera ver? Bem, como está off-line e esta versão do aplicativo não tem service worker, seria de esperar ver a mensagem de erro "não há conexão com a Internet" típica do Chrome.

![3ec8737c20a475df.png](img/3ec8737c20a475df.png)

What happened? Well, recall that when you began this codelab, you tried out the completed version of the app. When you ran that version, the app actually installed a service worker. That service worker is now automatically running every time that you run the app. Once a service worker is installed to a scope such as `localhost:3000` (you'll learn more about scope in the next section), that service worker automatically starts up every time that you access the scope, unless you programmatically or manually delete it.

O que aconteceu? Bem, lembre-se de que ao começar este codelab, você experimentou a versão concluída do aplicativo. Quando aquela versão foi executada, o aplicativo realmente instalou um service worker. Esse service worker agora está sendo executado automaticamente toda vez que você executa o aplicativo. Uma vez que um service worker é instalado para um escopo, como `localhost:3000` (você vai aprender mais sobre escopo na próxima seção), esse service worker é iniciado automaticamente toda vez que se acessa o escopo, a menos que você o exclua programática ou manualmente.

![837b46360756810a.png](img/837b46360756810a.png)

Now, before you reload the site, make sure that you're still using DevTools to simulate offline mode. Reload the page, and it should show you the "there is no Internet connection" error message as expected.

![da11a350ed38ad2e.png](img/da11a350ed38ad2e.png)

## Instale os ativos do site

Now it's time to add offline support back into the app. This consists of two steps:

1. Crie um arquivo JavaScript que será o service worker.
2. Instrua o navegador a registrar o arquivo JavaScript como o "service worker".

Agora é hora de adicionar suporte off-line de volta ao aplicativo. Isso consiste em dois passos:<aside class="key-point">

<p><strong>The location of the service worker is important! </strong>For security reasons, a service worker can only control the pages that are in its same directory or its subdirectories. This means that if you place the service worker file in a scripts directory it will only be able to interact with pages in the scripts directory or below.</p>

</aside> 

Primeiro, crie um arquivo vazio chamado `sw.js` e coloque-o na pasta `/app`.

    <script>
    if('serviceWorker' in navigator) {
      navigator.serviceWorker
               .register('/sw.js')
               .then(function() { console.log("Service Worker Registered"); });
    }
    </script>
    

Agora, abra `index.html` e adicione o código a seguir na parte inferior de `<body>`.

O script verifica se o navegador suporta service workers. Se suportar, ele registra o nosso arquivo `sw.js` atualmente em branco como o service worker, e, em seguida, registra para o console.

![37d374c4b51d273.png](img/37d374c4b51d273.png)

Make sure that the **Offline** checkbox in DevTools is disabled. Reload your page again. As the page loads, you can see that a service worker is registered.

![b9af9805d4535bd3.png](img/b9af9805d4535bd3.png)

Next to the **Source** label you can see a link to the source code of the registered service worker.

![3519a5068bc773ea.png](img/3519a5068bc773ea.png)

If you ever want to inspect the currently-installed service worker for a page, click on the link. This will show you the source code of the service worker in the **Sources** panel of DevTools. For example, click on the link now, and you should see an empty file.

![dbc14cbb8ca35312.png](img/dbc14cbb8ca35312.png)

## Intercepte as solicitações da página da Web

With the service worker registered, the first time a user hits the page an `install` event is triggered. This event is where you want to cache your page assets.

Com o service worker registrado, na primeira vez que um usuário acessar a página, um evento `install` é acionado. Este evento é o local onde você deseja armazenar em cache os ativos da sua página.

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
    

Adicione o código a seguir ao sw.js.

A primeira linha adiciona o Cache polyfill. Este polyfill já está incluído no repositório. Precisamos usar o polyfill porque a API Cache ainda não é totalmente suportada em todos os navegadores. Em seguida, vem o ouvinte de evento `install`. O ouvinte de evento `install` abre o objeto `caches` e, em seguida, o preenche com a lista de recursos que queremos armazenar em cache. Uma coisa importante sobre a operação `addAll` é que ela é tudo ou nada. Se um dos arquivos não estiver presente ou não for buscado, toda a operação `addAll` falha. Um bom aplicativo cuidará desse cenário.

<

Um recurso poderoso dos service workers é que, uma vez que um service worker controla uma página, ele pode interceptar todas as solicitações que a página faz e decidir o que fazer com a solicitação. Nesta seção você vai programar o service worker para interceptar solicitações e retornar as versões em cache de ativos, em vez de ir para a rede para recuperá-los.

<h4>Tópicos abordados</h4>

<ul>
  
<li>Como adicionar um service worker básico a um projeto existente</li>
<li><a href="https://github.com/coonsta/cache-polyfill">https://github.com/coonsta/cache-polyfill</a> </li>
<li>Uma estratégia simples para o armazenamento em cache off-line.</li>
<li>Currently Chrome and other browsers don't yet fully support the <code>addAll</code> method (<strong>note:</strong> Chrome 46 will be compliant).</li>
<li>Why do you have ?homescreen=1</li>
  
  <li>
    
<p>URLs with query string parameters are treated as individual URLs and need to be cached separately.</p>
</aside>
  </li>
</ul>

## Parabéns!

A primeira etapa é anexar um gerenciador de eventos ao evento `fetch`. Esse evento é acionado para todas as solicitações realizadas.

Adicione o código a seguir ao à parte inferior do seu `sw.js` para registrar as solicitações feitas a partir da página principal.

Vamos testar isso. **Atenção!** Você está prestes a ver mais algum comportamento inesperado do service worker.

    self.addEventListener('fetch', function(event) {
     console.log(event.request.url);
    });
    

Abra DevTools e vá para o painel **Application**. A caixa de seleção **Offline** deve estar desativada. Pressione a tecla `Esc` para abrir a gaveta **Console** na parte inferior da sua janela DevTools. Sua janela DevTools deve ser semelhante à imagem a seguir:

Open DevTools and go to the **Application** panel. The **Offline** checkbox should be disabled. Press the `Esc` key to open the **Console** drawer at the bottom of your DevTools window. Your DevTools window should look similar to the following screenshot:

![c96de824be6852d7.png](img/c96de824be6852d7.png)

Reload your page now and look at the DevTools window again. For one, we're expecting to see a bunch of requests logged to the Console, but that's not happening. For two, in the **Service Worker** pane we can see that the **Status** has changed:

![c7cfb6099e79d5aa.png](img/c7cfb6099e79d5aa.png)

Para corrigir esse inconveniente, ative a caixa de seleção **Update on reload**.<aside class="key-point">

<p>This behavior is by design. Check out  <a href="/web/fundamentals/primers/service-worker/update-a-service-worker">Update a Service Worker</a> to learn more about the service worker lifecycle.</p>

</aside> 

To fix this inconvenience, enable the **Update on reload** checkbox.

![26f2ae9a805bc69b.png](img/26f2ae9a805bc69b.png)

Atualize a página agora e você pode ver que um novo service worker está instalado e que os URLs de solicitação estão sendo registrados no Console, como esperado.

Reload the page now and you can see that a new service worker is installed and that the request URLs are being logged to the Console, as expected.

![53c23650b131143a.png](img/53c23650b131143a.png)

Para que seu aplicativo funcione off-line, você precisa coletar a solicitação do cache se ela estiver disponível.

Atualize seu ouvinte de evento de busca para coincidir com o código abaixo.

O método `event.respondWith()` informa ao navegador para avaliar o resultado do evento no futuro. `caches.match(event.request)` leva a atual solicitação da web que ativou o evento de busca e procura no cache por um recurso que corresponda. A correspondência é realizada verificando a string do URL. O método `match` retorna uma promessa que resolve, mesmo se o arquivo não for encontrado no cache. Isso significa que você tem uma escolha sobre o que fazer. Em nosso caso simples, quando o arquivo não é encontrado, você deve `fetch` o mesmo da rede e retorná-lo ao navegador.

    self.addEventListener('fetch', function(event) {
     console.log(event.request.url);
    
     event.respondWith(
       caches.match(event.request).then(function(response) {
         return response || fetch(event.request);
       })
     );
    });
    

Este é o caso mais simples; existem muitos outros cenários de armazenamento em cache. Por exemplo, é possível armazenar em cache de forma incremental todas as respostas de solicitações que anteriormente não estavam armazenadas para que, no futuro, todas sejam retornadas do cache.

Agora você tem suporte off-line. Atualize a página enquanto ainda está online para atualizar o service worker para a última versão, e depois use DevTools para entrar em modo off-line. Atualizar a página novamente, e você deve ter uma buzina de ar off-line totalmente funcional!

## Encontrou um problema ou tem feedback? {: .hide-from-toc }

Ajude-nos a melhorar nossos codelabs reportando um [problema](https://github.com/googlesamples/io2015-codelabs/issues) hoje. E obrigado!

#### Próximas etapas

* Saiba como adicionar facilmente [suporte off-line com elementos Polymer off-line](https://codelabs.developers.google.com/codelabs/sw-precache/index.html?index=..%2F..%2Findex#0) robusto
* Explore mais [técnicas avançadas de armazenamento em cache](https://jakearchibald.com/2014/offline-cookbook/)
* A simple offline caching strategy.

#### Saiba mais

* Learn how to easily add robust [offline support with Polymer offline elements](https://codelabs.developers.google.com/codelabs/sw-precache/index.html?index=..%2F..%2Findex#0)
* Explore more [advanced caching techniques](https://jakearchibald.com/2014/offline-cookbook/)

#### Learn More

* [Introduction to service worker](/web/fundamentals/primers/service-worker/)

## Found an issue, or have feedback? {: .hide-from-toc }

{# wf_devsite_translation #}