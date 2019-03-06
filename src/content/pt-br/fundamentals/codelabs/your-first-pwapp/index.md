project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Neste codelab, você vai criar um Progressive Web App, que é carregado com rapidez, mesmo em redes instáveis, tem um ícone na tela inicial e é carregado como uma experiência de tela inteira de alto nível.

{# wf_updated_on: 2017-01-05 #} {# wf_published_on: 2016-01-01 #}

# Seu primeiro Progressive Web App {: .page-title }

{% include "web/_shared/contributors/petelepage.html" %}

## Introdução

[Progressive Web Apps](/web/progressive-web-apps) são experiências que combinam o melhor da Web e o melhor dos aplicativos. Eles são úteis para os usuários desde a primeira visita em uma guia de navegador sem exigir instalações. Conforme o usuário desenvolve uma relação com o aplicativo ao longo do tempo, ele se torna cada vez mais eficaz. Ele é carregado com rapidez, mesmo em redes instáveis, envia notificações push relevantes, tem um ícone na tela inicial e é carregado como uma experiência de tela inteira de alto nível.

### O que é um Progressive Web App?

Um Progressive Web App é:

* **Progressivo** - Funciona para qualquer usuário, independentemente do navegador escolhido, pois é criado com aprimoramento progressivo como princípio fundamental.
* **Responsivo** - Se adequa a qualquer formato: desktop, celular, tablet ou o que for inventado a seguir.
* **Independente de conectividade** - Aprimorado com service workers para trabalhar off-line ou em redes de baixa qualidade.
* **Semelhante a aplicativos** - Parece com aplicativos para os usuários, com interações e navegação de estilo de aplicativos, pois é compilado no modelo de shell de aplicativo.
* **Atual** - Sempre atualizado graças ao processo de atualização do service worker.
* **Seguro** - Fornecido via HTTPS para evitar invasões e garantir que o conteúdo não seja adulterado.
* **Descobrível** - Pode ser identificado como "aplicativo" graças aos manifestos W3C e ao escopo de registro do service worker, que permitem que os mecanismos de pesquisa os encontrem.
* **Reenvolvente** - Facilita o reengajamento com recursos como notificações push.
* **Instalável** - Permite que os usuários "guardem" os aplicativos mais úteis em suas telas iniciais sem precisar acessar uma loja de aplicativos.
* **Linkável** - Compartilhe facilmente por URL, não requer instalação complexa.

Este codelab orientará você a criar seu próprio Progressive Web App, incluindo considerações de design, além de detalhes de implementação para garantir que seu aplicativo atenda aos principais princípios de um Progressive Web App.<aside class="key-point">

<p>Looking for more? Check out the talks from the  <a href="https://www.youtube.com/playlist?list=PLNYkxOF6rcIAWWNR_Q6eLPhsyx6VvYjVb">2016 Progressive Web App Summit</a>.</p>

</aside> 

### O que vamos criar?

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
                O que você aprenderá
              </h3>
              
              <ul>
                <li>
                  <strong>Progressivo</strong> - usaremos o aprimoramento progressivo em todo o aplicativo.
                </li>
                <li>
                  <strong>Responsivo</strong> - garantiremos que ele se encaixe em qualquer formato.
                </li>
                <li>
                  <strong>Independente de conectividade</strong> - armazenaremos o shell do aplicativo em cache com service workers.
                </li>
              </ul>
              
              <h3>
                O que será necessário
              </h3>
              
              <ul>
                <li>
                  Como projetar e criar um aplicativo usando o método de "shell de aplicativo"
                </li>
                <li>
                  Como fazer com que seu aplicativo funcione off-line
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
                Neste codelab, você criará um app da Web de previsão do tempo usando técnicas de Progressive Web App. Vamos considerar as propriedades de um Progressive Web App:
              </p>
              
              <h2>
                Configuração
              </h2>
              
              <h3>
                Faça o download do código
              </h3>
              
              <p>
                Este codelab é focado em Progressive Web Apps. Conceitos não-relevantes e blocos de código são apenas pincelados e são fornecidos para que você simplesmente os copie e cole.
              </p>
              
              <p>
                <a href="https://github.com/googlecodelabs/your-first-pwapp/archive/master.zip">Download source code</a>
              </p>
              
              <p>
                Unpack the downloaded zip file. This will unpack a root folder (<code>your-first-pwapp-master</code>), which contains one folder for each step of this codelab, along with all of the resources you will need.
              </p>
              
              <p>
                Descompacte o arquivo zip baixado. Isso irá descompactar uma pasta raiz (<code>your-first-pwapp-master</code>), que contém uma pasta para cada etapa deste codelab, juntamente com todos os recursos que você vai precisar.
              </p>
              
              <h3>
                Instale e verifique o servidor de Web
              </h3>
              
              <p>
                As pastas <code>step-NN</code> contêm o estado final desejado de cada etapa deste codelab. Elas servem como referência. Faremos todo nosso trabalho de codificação em um diretório chamado <code>work</code>.
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
                Clique no botão <strong>choose folder</strong> e selecione a pasta <code>work</code>. Isto permitirá servir o seu trabalho em andamento pelo URL em destaque na caixa de diálogo do servidor de Web (na seção <strong>Web Server URL(s)</strong>).
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
                Faça sua arquitetura de shell do aplicativo
              </h2>
              
              <h3>
                O que é o shell do aplicativo?
              </h3>
              
              <p>
                Obviamente, este aplicativo ainda não está fazendo nada interessante - até agora, é apenas um esqueleto mínimo com um controle giratório que estamos usando para verificar a funcionalidade do seu servidor de Web. Adicionaremos funcionalidade e recursos de IU em etapas posteriores.
              </p>
              
              <p>
                O shell do aplicativo é o HTML, CSS e JavaScript mínimos necessários para capacitar a interface do usuário de um Progressive Web App e é um dos componentes que garante um desempenho confiavelmente bom. Seu primeiro carregamento deve ser extremamente rápido e armazenado em cache imediatamente. "Armazenado em cm cache" significa que os arquivos do shell são carregados uma vez pela rede e salvos no dispositivo local. Toda vez que o usuário abre o aplicativo posteriormente, os arquivos do shell são carregados a partir do cache do dispositivo local, o que resulta em tempos de inicialização extremamente rápidos.
              </p>
              
              <p>
                A arquitetura de shell do aplicativo separa a infraestrutura principal do aplicativo e a IU dos dados. Toda a IU e a infraestrutura são armazenadas em cache localmente usando um service worker para que, em carregamentos subsequentes, o Progressive Web App só precise recuperar os dados necessários em vez de precisar carregar tudo.
              </p>
              
              <p>
                <img src="img/156b5e3cc8373d55.png" alt="156b5e3cc8373d55.png" />
              </p>
              
              <p>
                Em outras palavras, o shell do aplicativo é semelhante a um pacote de código que você publicaria em uma loja de aplicativos ao compilar um aplicativo nativo. Ele consiste nos elementos principais necessários para criar seu aplicativo, mas é improvável que ele contenha os dados.
              </p>
              
              <h3>
                Por que usar a arquitetura de shell de aplicativo?
              </h3>
              
              <p>
                O uso da arquitetura de shell de aplicativo permite que você se concentre na velocidade, fornecendo ao seu Progressive Web App propriedades semelhantes às dos aplicativos nativos: carregamento instantâneo e atualizações regulares sem a necessidade de uma loja de aplicativos.
              </p>
              
              <h3>
                Projete o shell do aplicativo
              </h3>
              
              <p>
                A primeira etapa é dividir o design em seus componentes principais.
              </p>
              
              <p>
                Considere:
              </p>
              
              <ul>
                <li>
                  Chrome 52 ou superior
                </li>
                <li>
                  <a href="https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb">Web Server for Chrome</a>, seu servidor de Web preferido
                </li>
                <li>
                  O exemplo de código
                </li>
              </ul>
              
              <p>
                Vamos criar um aplicativo de previsão do tempo como nosso primeiro Progressive Web App. Os componentes principais consistirão em:
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
                              Ao projetar um aplicativo mais completo, o conteúdo que não é necessário para o carregamento inicial pode ser solicitado posteriormente e armazenado em cache para uso futuro. Por exemplo, podemos adiar o carregamento da caixa de diálogo New City até após a renderização da primeira experiência de execução e a disponibilização de alguns ciclos de ociosidade.
                            </p>
                            
                            <h2>
                              Implemente seu shell do aplicativo
                            </h2>
                            
                            <p>
                              Existem diversas maneiras de começar qualquer projeto e, geralmente, nós recomendamos o uso do Web Starter Kit. Mas, neste caso, para que nosso projeto seja o mais simples possível e para que nos concentremos nos Progressive Web Apps, nós fornecemos todos os recursos de que você precisará.
                            </p>
                            
                            <h3>
                              Crie o HTML do shell do aplicativo
                            </h3>
                            
                            <p>
                              Agora adicionaremos os componentes essenciais discutidos em <a href="/web/fundamentals/getting-started/your-first-progressive-web-app/step-01">Arquitete o shell do aplicativo</a>.
                            </p>
                            
                            <p>
                              Lembre-se de que os componentes principais consistirão em:
                            </p>
                            
                            <ul>
                              <li>
                                O que precisa estar na tela imediatamente?
                              </li>
                              <li>
                                Que outros componentes da IU são essenciais para seu aplicativo?
                              </li>
                              <li>
                                Que recursos de suporte são necessários para o shell do aplicativo? Por exemplo, imagens, JavaScript, estilos etc.
                              </li>
                              <li>
                                A dialog for adding new cities
                              </li>
                              <li>
                                A loading indicator
                              </li>
                            </ul>
                            
                            <p>
                              O arquivo <code>index.html</code> que já está em seu diretório <code>work</code> deve ser algo parecido com isso (este é um subconjunto do conteúdo efetivo, não copie este código em seu arquivo):
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
                              Perceba que o carregador é visível por padrão. Isso garante que o usuário veja o carregador imediatamente conforme a página é carregada, o que oferece uma indicação clara de que o conteúdo está sendo carregado.
                            </p>
                            
                            <p>
                              Para poupar tempo, já criamos a folha de estilo para seu uso.
                            </p><aside class="key-point">

<p>We've given you the markup and styles to save you some time and make sure you're starting on a solid foundation. In the next section, you'll have an opportunity to write your own code.</p>

</aside> 
                            
                            <h3>
                              Verifique o código chave do aplicativo JavaScript
                            </h3>
                            
                            <p>
                              Agora que grande parte da IU está pronta, é hora de começar a conectar o código para fazer tudo funcionar. Como o restante do shell do aplicativo, tenha consciência dos códigos que são necessários como parte da experiência principal e o que pode ser carregado posteriormente.
                            </p>
                            
                            <p>
                              Seu diretório de trabalho também já inclui o código do aplicativo (<code>scripts/app.js</code>). Nele, você encontrará:
                            </p>
                            
                            <ul>
                              <li>
                                Cabeçalho com um título e botões de adição/atualização
                              </li>
                              <li>
                                Contêiner para os cartões de previsão
                              </li>
                              <li>
                                Um modelo de cartão de previsão
                              </li>
                              <li>
                                Uma caixa de diálogo para adicionar novas cidades
                              </li>
                              <li>
                                Um indicador de carregamento
                              </li>
                              <li>
                                Some fake data (<code>initialWeatherForecast</code>) you can use to quickly test how things render.
                              </li>
                            </ul>
                            
                            <h3>
                              Faça testes
                            </h3>
                            
                            <p>
                              Agora que você tem o HTML, os estilos e o JavaScript principais, chegou a hora de testar o aplicativo.
                            </p>
                            
                            <p>
                              Para ver como os dados falsos de meteorologia são processados, retire o comentário da seguinte linha na parte inferior do seu arquivo <code>index.html</code>:
                            </p>
                            
                            <pre><code>&lt;!--&lt;script src="scripts/app.js" async&gt;&lt;/script&gt;--&gt;
</code></pre>
                            
                            <p>
                              Em seguida, remova o comentário da seguinte linha na parte inferior do seu arquivo <code>app.js</code>:
                            </p>
                            
                            <pre><code>// app.updateForecastCard(initialWeatherForecast);
</code></pre>
                            
                            <p>
                              Atualize seu aplicativo. O resultado deve ser um cartão de previsão bem formatado (embora falso, como pode ser percebido pela data) cartão de previsão com o controle giratório desativado, como este:
                            </p>
                            
                            <p>
                              <img src="img/166c3b4982e4a0ad.png" alt="166c3b4982e4a0ad.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-04/">Link</a>
                            </p>
                            
                            <p>
                              Depois de experimentar e verificar que ele funciona como esperado, você pode remover a chamada para <code>app.updateForecastCard</code> com os dados falsos. Ela foi necessária apenas para garantir que tudo funcionava como esperado.
                            </p>
                            
                            <h2>
                              Comece com um primeiro carregamento rápido
                            </h2>
                            
                            <p>
                              Progressive Web Apps devem ser iniciados rapidamente e disponibilizados para uso imediatamente. No seu estado atual, nosso aplicativo de previsão do tempo é iniciado rapidamente, mas não pode ser usado. Não há dados. Nós poderíamos fazer uma solicitação AJAX para obter os dados, mas isso resultaria em uma solicitação adicional e faria o carregamento inicial demorar mais. Em vez disso, forneça dados reais no primeiro carregamento.
                            </p>
                            
                            <h3>
                              Injete os dados de previsão do tempo
                            </h3>
                            
                            <p>
                              Neste codelab, simularemos o servidor injetando a previsão do tempo diretamente no JavaScript, mas em um aplicativo de produção, os dados de previsão do tempo mais recentes seriam injetados pelo servidor com base na geolocalização do endereço IP do usuário.
                            </p>
                            
                            <p>
                              O código já contém os dados que injetaremos. É a <code>initialWeatherForecast</code> que usamos no passo anterior.
                            </p>
                            
                            <h3>
                              Diferenciando a primeira execução
                            </h3>
                            
                            <p>
                              Então, como podemos saber quando exibir essas informações, que podem não ser relevantes em carregamentos futuros, quando o aplicativo de previsão for coletado do cache? Quando o usuário carregar o aplicativo em visitas subsequentes, ele poderá estar em uma cidade diferente, então será preciso carregar as informações dessa cidade, não necessariamente da primeira cidade que foi procurada.
                            </p>
                            
                            <p>
                              As preferências do usuário, como a lista de cidades nas quais um usuário está inscrito, devem ser armazenadas localmente usando o IndexedDB ou outro mecanismo de armazenamento rápido. Para simplificar este codelab o máximo possível, usamos <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage">localStorage</a>, que não é ideal para aplicativos de produção por ser um mecanismo de armazenamento sincrônico com bloqueio que pode ser muito lento em alguns dispositivos.
                            </p><aside class="key-point">

<p><strong>Extra Credit</strong>: Replace <code>localStorage</code> implementation with  <a href="https://www.npmjs.com/package/idb">idb</a>, check out  <a href="https://github.com/localForage/localForage">localForage</a> as a simple wrapper to idb.</p>

</aside> 
                            
                            <p>
                              Primeiro, vamos adicionar o código necessário para salvar as preferências do usuário. Localize o seguinte comentário TODO em seu código.
                            </p>
                            
                            <pre><code>  // TODO add saveSelectedCities function here
</code></pre>
                            
                            <p>
                              E adicione o código a seguir abaixo do comentário.
                            </p>
                            
                            <pre><code>  // Save list of cities to localStorage.
  app.saveSelectedCities = function() {
    var selectedCities = JSON.stringify(app.selectedCities);
    localStorage.selectedCities = selectedCities;
  };
</code></pre>
                            
                            <p>
                              Em seguida, adicionamos o código de inicialização para verificar se o usuário salvou alguma cidade e renderizar esses dados, ou usar os dados injetados. Localize o seguinte comentário:
                            </p>
                            
                            <pre><code>  // TODO add startup code here
</code></pre>
                            
                            <p>
                              E adicione o código a seguir abaixo desse comentário:
                            </p>
                            
                            <pre><code>/************************************************************************
   *

   * Code required to start the app
   *
   * OBSERVAÇÃO: To simplify this codelab, we've used localStorage.
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
                              O código de inicialização verifica se existem cidades salvas no armazenamento local. Se houver, ele analisa os dados de armazenamento local e exibe um cartão de previsão para cada uma das cidades salvas. Caso contrário, o código de inicialização usa apenas os dados falsos de previsão e salva isso como a cidade padrão.
                            </p>
                            
                            <h3>
                              Salvar as cidades selecionadas
                            </h3>
                            
                            <p>
                              Finalmente, você precisa modificar o gerenciador do botão "add city" para salvar a cidade escolhida no armazenamento local.
                            </p>
                            
                            <p>
                              Atualize seu gerenciador de clique <code>butAddCity</code> para que ele corresponda ao seguinte código:
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
                              As novas adições são a inicialização de <code>app.selectedCities</code> se ele não existir, e as chamadas para <code>app.selectedCities.push()</code> e <code>app.saveSelectedCities()</code>.
                            </p>
                            
                            <h3>
                              Faça testes
                            </h3>
                            
                            <ul>
                              <li>
                                Cabeçalho com um título e botões de adição/atualização
                              </li>
                              <li>
                                Contêiner para os cartões de previsão
                              </li>
                              <li>
                                Um modelo de cartão de previsão
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-05/">Link</a>
                            </p>
                            
                            <h2>
                              Use service workers para pré-armazenar em cache no shell do aplicativo
                            </h2>
                            
                            <p>
                              Progressive Web Apps precisam ser rápidos e instaláveis, o que significa que eles devem funcionar on-line, off-line ou em condições intermitentes e lentas. Para conseguir isso, precisamos armazenar em cache nosso shell de aplicativo usando service worker para que ele seja sempre disponibilizado de forma rápida e confiável.
                            </p>
                            
                            <p>
                              Se não tiver experiência com service workers, você pode obter uma noção básica lendo <a href="/web/fundamentals/primers/service-worker/">Introdução aos service workers</a> sobre o que eles podem fazer, como seu ciclo de vida funciona e muito mais. Após concluir este codelab, certifique-se de verificar o <a href="https://goo.gl/jhXCBy">codelab Depurar Service Workers</a> para ter uma visão mais detalhada de como trabalhar com service workers.
                            </p>
                            
                            <p>
                              Recursos fornecidos por service workers devem ser considerados aprimoramentos progressivos e adicionados apenas se o navegador for compatível. Por exemplo, com service workers, você pode armazenar em cache o shell de aplicativo e os dados do seu aplicativo para que eles estejam disponíveis mesmo quando a rede não estiver. Quando service workers não forem compatíveis, o código off-line não será chamado e o usuário terá uma experiência básica. O uso da detecção de recursos para fornecer aprimoramentos progressivos incorre em poucos custos adicionais e não falhará em navegadores mais antigos incompatíveis com o recurso.
                            </p><aside class="key-point">

<p><strong>Remember</strong>: Service worker functionality is only available on pages that are accessed via HTTPS (<a href="http://localhost">http://localhost</a> and equivalents will also work, to facilitate testing). To learn about the rationale behind this restriction check out  <a href="http://www.chromium.org/Home/chromium-security/prefer-secure-origins-for-powerful-new-features">Prefer Secure Origins For Powerful New Features</a> from the Chromium team.</p>

</aside> 
                            
                            <h3>
                              Registre o service worker se ele estiver disponível
                            </h3>
                            
                            <p>
                              A primeira etapa necessária para fazer com que o aplicativo funcione off-line é registrar um service worker, um script que oferece a funcionalidade off-line sem precisar de uma página da Web aberta ou de interação do usuário.
                            </p>
                            
                            <p>
                              Bastam duas etapas simples:
                            </p>
                            
                            <ol start="1">
                              <li>
                                Instrua o navegador a registrar o arquivo JavaScript como o service worker.
                              </li>
                              
                              <li>
                                Crie um arquivo JavaScript que contendo o service worker.
                              </li>
                            </ol>
                            
                            <p>
                              Primeiro, precisamos verificar se o navegador oferece suporte a service workers e, em caso positivo, registrar o service worker. Adicionar o seguinte código ao <code>app.js</code> (após o comentário <code>// TODO add service worker code here</code>):
                            </p>
                            
                            <pre><code>  if ('serviceWorker' in navigator) {
    navigator.serviceWorker
             .register('./service-worker.js')
             .then(function() { console.log('Service Worker Registered'); });
  }
</code></pre>
                            
                            <h3>
                              Armazene em cache os ativos do site
                            </h3>
                            
                            <p>
                              Quando o service worker é registrado, um evento de instalação é acionado na primeira vez que o usuário visitar a página. Nesse gerenciador de eventos, nós armazenaremos em cache todos os ativos dos quais o aplicativo precisa.
                            </p><aside class="warning">

<p>The code below must NOT be used in production, it covers only the most basic use cases and it's easy to get yourself into a state where your app shell will never update. Be sure to review the section below that discusses the pitfalls of this implementation and how to avoid them.</p>

</aside> 
                            
                            <p>
                              Quando o service worker é acionado, ele deve abrir o objeto <a href="https://developer.mozilla.org/en-US/docs/Web/API/Cache">caches</a> e preenchê-lo com os ativos necessários para carregar o shell do aplicativo. Crie um arquivo chamado <code>service-worker.js</code> na pasta raiz do seu aplicativo (que deve ser o diretório <code>your-first-pwapp-master/work</code>). Esse arquivo deve estar ativo na raiz do aplicativo, poiso escopo dos service workers é definido pelo diretório onde o arquivo se encontra. Adicione este código ao seu novo arquivo <code>service-worker.js</code>:
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
                              Primeiro, precisamos abrir o cache com <code>caches.open()</code> e fornecer um nome de cache. Fornecer um nome de cache nos permite distinguir as versões dos arquivos, ou separar os dados do shell do aplicativo para atualizarmos um sem afetar o outro com facilidade.
                            </p>
                            
                            <p>
                              Quando cache estiver aberto, podemos chamar <code>cache.addAll()</code>, que aceita uma lista de URLs e os recupera do servidor e os adiciona à resposta ao cache. Infelizmente, <code>cache.addAll()</code> é atômico e, se qualquer arquivo falhar, toda a etapa do cache também falha.
                            </p>
                            
                            <p>
                              Muito bem, vamos começar nos familiarizar com a forma como você pode usar DevTools para entender e debug service workers. Antes de recarregar sua página, abra DevTools, vá ao painel <strong>Service Worker</strong> no painel <strong>Application</strong>. Ele deve ter a aparência a seguir.
                            </p>
                            
                            <p>
                              <img src="img/ed4633f91ec1389f.png" alt="ed4633f91ec1389f.png" />
                            </p>
                            
                            <p>
                              Quando se vê uma página em branco como esta, isso significa que a página atualmente aberta não possui service workers registrados.
                            </p>
                            
                            <p>
                              Agora, atualize sua página. O painel Service Worker deve ter a aparência a seguir.
                            </p>
                            
                            <p>
                              <img src="img/bf15c2f18d7f945c.png" alt="bf15c2f18d7f945c.png" />
                            </p>
                            
                            <p>
                              Quando você vê informações como estas, isso significa que a página tem um service worker em execução.
                            </p>
                            
                            <p>
                              OK, agora faremos um breve desvio para demonstrar um problema que você pode encontrar ao desenvolver service workers. Para demonstrar, vamos adicionar um ouvinte de evento <code>activate</code> abaixo do ouvinte de evento <code>install</code> em seu arquivo <code>service-worker.js</code>.
                            </p>
                            
                            <pre><code>self.addEventListener('activate', function(e) {
  console.log('[ServiceWorker] Activate');
});
</code></pre>
                            
                            <p>
                              O evento <code>activate</code> é acionado quando o service worker inicia.
                            </p>
                            
                            <p>
                              Abra o Console do DevTools e recarregue a página, alterne para o painel Service Worker no painel Application e clique em inspecionar no service worker ativado. Você espera ver a mensagem <code>[ServiceWorker] Activate</code> registrada para o console, mas isso não aconteceu. Confira seu painel Service Worker e você pode ver que o novo service worker (que inclui ativar o ouvinte de evento) parece estar em um estado de "espera".
                            </p>
                            
                            <p>
                              <img src="img/1f454b6807700695.png" alt="1f454b6807700695.png" />
                            </p>
                            
                            <p>
                              Basicamente, o antigo service worker continua a controlar a página enquanto houver uma guia aberta para página. Então, você <em>poderia</em> fechar e reabrir a página ou pressionar o botão <strong>skipWaiting</strong>, mas uma solução de longo prazo é simplesmente ativar a caixa de seleção <strong>Update on Reload</strong> no painel Service Worker do DevTools. Quando esta caixa de seleção está ativada, o service worker é forçosamente atualizado toda vez que a página recarrega.
                            </p>
                            
                            <p>
                              Ative a caixa de seleção <strong>atualizar ao recarregar</strong> e recarregue a página para confirmar que o novo service worker é ativado.
                            </p>
                            
                            <p>
                              <strong>Observação:</strong> Você pode ver um erro no painel Service Worker do painel Application semelhante ao mostrado abaixo, é <strong>seguro</strong> ignorar este erro.
                            </p>
                            
                            <p>
                              <img src="img/b1728ef310c444f5.png" alt="b1728ef310c444f5.png" />
                            </p>
                            
                            <p>
                              Por enquanto é só sobre a inspeção e depuração de service workers no DevTools. Mostraremos mais alguns truques depois. Vamos voltar para a construção do seu aplicativo.
                            </p>
                            
                            <p>
                              Vamos expandir sobre o ouvinte de evento <code>activate</code> para incluir alguma lógica para atualizar o cache. Atualize seu código para coincidir com o código abaixo.
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
                              Este código garante que o service worker atualiza seu cache sempre que qualquer um dos arquivos do shell do aplicativo mudar. Para que isso funcione, você precisa incrementar a variável <code>cacheName</code> na parte superior do seu arquivo do service worker.
                            </p>
                            
                            <p>
                              A última declaração corrige um corner case sobre o qual você pode ler na caixa de informações (opcional) abaixo.
                            </p><aside class="key-point">

<p>When the app is complete, <code>self.clients.claim()</code> fixes a corner case in which the app wasn't returning the latest data. You can reproduce the corner case by commenting out the line below and then doing the following steps: 1) load app for first time so that the initial New York City data is shown 2) press the refresh button on the app 3) go offline 4) reload the app. You expect to see the newer NYC data, but you actually see the initial data. This happens because the service worker is not yet activated. <code>self.clients.claim()</code> essentially lets you activate the service worker faster.</p>

</aside> 
                            
                            <p>
                              Por fim, vamos atualizar a lista de arquivos necessários para o shell do aplicativo. Na matriz, precisamos incluir todos os arquivos dos quais o aplicativo precisa, incluindo imagens, JavaScript, folhas de estilo etc. Perto do topo do seu arquivo <code>service-worker.js</code>, substitua <code>var filesToCache = [];</code> pelo o código abaixo:
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
                              Nosso aplicativo ainda não funciona off-line. Nós armazenamos em cache os componentes do shell do aplicativo, mas ainda precisamos carregá-los do cache local.
                            </p>
                            
                            <h3>
                              Forneça a estrutura do aplicativo do cache
                            </h3>
                            
                            <p>
                              Service workers fornecem a capacidade de interceptar solicitações feitas do nosso Progressive Web App e gerenciá-las no service worker. Isso significa que podemos determinar como queremos gerenciar a solicitação e potencialmente fornecer nossa própria resposta de cache.
                            </p>
                            
                            <p>
                              Por exemplo:
                            </p>
                            
                            <pre><code>self.addEventListener('fetch', function(event) {
  // Do something interesting with the fetch here
});
</code></pre>
                            
                            <p>
                              Agora vamos fornecer a estrutura do aplicativo do cache. Adicione o seguinte código na parte inferior do seu arquivo <code>service-worker.js</code>:
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
                              De dentro para fora, o <code>caches.match()</code> avalia a solicitação da Web que acionou o evento de <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">busca</a> e verifica se ele está disponível no cache. Em seguida, ele responde com a versão do cache ou usa <code>fetch</code> para obter uma cópia da rede. A <code>response</code> é passada à página da Web com <code>e.respondWith()</code>.
                            </p><aside class="warning">

<p>If you're not seeing the <code>[ServiceWorker]</code> logging in the console, be sure you've changed the <code>cacheName</code> variable and that you're inspecting the right service worker by opening the Service Worker pane in the Applications panel and clicking <strong>inspect</strong> on the running service worker. If that doesn't work, see the section on Tips for testing live service workers.</p>

</aside> 
                            
                            <h3>
                              Faça testes
                            </h3>
                            
                            <p>
                              Agora seu aplicativo tem funcionalidade off-line. Vamos experimentar.
                            </p>
                            
                            <p>
                              Atualize sua página e, em seguida, vá para o painel <strong>Cache Storage</strong> no painel <strong>Application</strong> do DevTools. Expanda a seção e você deve ver o nome do cache do seu shell do aplicativo listado do lado esquerdo. Ao clicar no cache do seu shell do aplicativo, você pode ver todos os recursos que estão armazenados em cache atualmente.
                            </p>
                            
                            <p>
                              <img src="img/ab9c361527825fac.png" alt="ab9c361527825fac.png" />
                            </p>
                            
                            <p>
                              Agora, vamos testar o modo off-line. Volte para o painel <strong>Service Worker</strong> do DevTools e ative a caixa de seleção <strong>Offline</strong>. Após ativá-la, você deve ver um ícone de aviso amarelo pequeno ao lado da guia do painel <strong>Network</strong>. Isto indica que você está off-line.
                            </p>
                            
                            <p>
                              <img src="img/7656372ff6c6a0f7.png" alt="7656372ff6c6a0f7.png" />
                            </p>
                            
                            <p>
                              Atualize sua página e... ela funciona! Quer dizer, mais ou menos. Observe como ela carrega os dados meteorológicos iniciais (falsos).
                            </p>
                            
                            <p>
                              <img src="img/8a959b48e233bc93.png" alt="8a959b48e233bc93.png" />
                            </p>
                            
                            <p>
                              Confira a cláusula <code>else</code> em <code>app.getForecast()</code> para entender por que o aplicativo consegue carregar os dados falsos.
                            </p>
                            
                            <p>
                              O próximo passo é modificar a lógica do aplicativo e do service worker para poder armazenar dados meteorológicos em cache e retornar os dados mais recentes do cache quando o aplicativo estiver off-line.
                            </p>
                            
                            <p>
                              <strong>Dica:</strong> Para começar do zero, limpar todos os dados salvos (localStoarge, dados de indexedDB, arquivos armazenados em cache) e remover quaisquer service workers, use o painel Clear storage na guia Application.
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-06/">Link</a>
                            </p>
                            
                            <h3>
                              Tenha cuidado com os casos de borda
                            </h3>
                            
                            <p>
                              Como já mencionamos, esse código <strong>não deve ser usado em produção</strong> devido aos muitos casos de borda não gerenciados.
                            </p>
                            
                            <h4>
                              O cache depende da atualização de cada chave de cache para cada alteração
                            </h4>
                            
                            <p>
                              Por exemplo, esse método de armazenamento em cache exige que você atualize a chave de cache sempre que o conteúdo for alterado, caso contrário, o cache não será atualizado e o conteúdo antigo será fornecido. Portanto, não deixe de alterar a chave de cache após cada alteração enquanto trabalha no seu projeto.
                            </p>
                            
                            <h4>
                              Exige que tudo seja baixado novamente para cada alteração
                            </h4>
                            
                            <p>
                              Outra desvantagem é que todo o cache é invalidado e precisa ser baixado novamente sempre que um arquivo é alterado. Isso significa que a correção de um simples erro de ortografia invalidará o cache e exigirá que tudo seja baixado novamente. Isso não é muito eficiente.
                            </p>
                            
                            <h4>
                              O cache do navegador pode impedir que o service worker seja atualizado
                            </h4>
                            
                            <p>
                              Existe outra ressalva. É essencial que a solicitação HTTPS realizada durante o gerenciador de instalação vá diretamente para a rede e não retorne uma resposta do cache do navegador. Caso contrário, o navegador poderá retornar a versão de cache antiga, resultando em um service worker que nunca é atualizado realmente.
                            </p>
                            
                            <h4>
                              Tenha cuidado com estratégias que priorizam o cache em produção
                            </h4>
                            
                            <p>
                              Nosso aplicativo usa uma estratégia que prioriza o cache, o que resulta em uma cópia de qualquer conteúdo no cache sendo retornada sem consultar a rede. Embora esse tipo de estratégia seja fácil de implementar, ela pode causar problemas no futuro. Depois que a cópia da página do host e do registro do service worker é armazenada em cache, pode ser extremamente difícil alterar a configuração do service worker (pois a configuração depende de onde ele foi definido) e você pode acabar implantando sites muito difíceis de atualizar.
                            </p>
                            
                            <h4>
                              Como posso evitar esses casos de borda?
                            </h4>
                            
                            <p>
                              Então, como evitar esses casos de borda? Use uma biblioteca como <a href="https://github.com/GoogleChrome/sw-precache">sw-precache</a>, que oferece um controle preciso do que é expirado, garante que as solicitações vão diretamente para a rede e faz todo o trabalho pesado para você.
                            </p>
                            
                            <h3>
                              Dicas para testar service workers ao vivo
                            </h3>
                            
                            <p>
                              A depuração de service workers pode ser um desafio e, quando ela envolve o cache, o problema pode ser ainda maior se o cache não for atualizado quando você espera. Entre o ciclo de vida de um service worker e os erros típicos no seu código, você pode se frustrar rapidamente. Mas não desanime. Existem algumas ferramentas que podem facilitar sua vida.
                            </p>
                            
                            <h4>
                              Começar do zero
                            </h4>
                            
                            <p>
                              Em alguns casos, você pode perceber que está carregando dados armazenados em cache ou que as coisas não são atualizadas conforme o esperado. Para limpar todos os dados salvos (localStoarge, dados de indexedDB, arquivos armazenados em cache) e remover quaisquer service workers, use o painel Clear storage na guia Application.
                            </p>
                            
                            <p>
                              Algumas outras dicas:
                            </p>
                            
                            <ul>
                              <li>
                                Um objeto <code>app</code> que contém algumas informações essenciais necessárias para o aplicativo.
                              </li>
                              <li>
                                Os ouvintes de eventos para todos os botões no cabeçalho (<code>add/refresh</code>) e na caixa de diálogo de adição de cidade (<code>add/cancel</code>).
                              </li>
                              <li>
                                Um método para adicionar ou atualizar os cartões de previsão (<code>app.updateForecastCard</code>).
                              </li>
                              <li>
                                Um método para obter a previsão do tempo mais recente da Firebase Public Weather API (<code>app.getForecast</code>).
                              </li>
                            </ul>
                            
                            <h2>
                              Use service workers para armazenar em cache os dados de previsão
                            </h2>
                            
                            <p>
                              Escolher a estratégia de <a href="https://jakearchibald.com/2014/offline-cookbook/">armazenamento em cache</a> certa é essencial e depende do tipo de dados apresentado por seu aplicativo. Por exemplo, dados que dependem do momento, como dados meteorológicos ou a cotação da bolsa, devem ser o mais atualizados possível, enquanto imagens de avatar ou o conteúdo de artigos podem ser atualizados com menos frequência.
                            </p>
                            
                            <p>
                              A estratégia <a href="https://jakearchibald.com/2014/offline-cookbook/#cache-network-race">cache-primeiro-depois-rede</a> é ideal para o nosso aplicativo. Ele apresenta dados na tela com a máxima rapidez possível e atualiza esses dados quando a rede retornar as informações mais recentes. Em comparação com a estratégia que prioriza a rede e depois o cache, o usuário não precisa aguardar até que o evento <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">fetch</a> atinja o tempo limite para obter os dados do cache.
                            </p>
                            
                            <p>
                              Priorizar o cache em vez da rede significa que precisamos acionar duas solicitações assíncronas, uma para o cache e outra para a rede. Nossa solicitação de rede com o aplicativo não precisa mudar muito, mas devemos modificar o service worker para armazenar a resposta em cache antes de retorná-la.
                            </p>
                            
                            <p>
                              Em circunstâncias normais, dos dados do cache serão retornados quase imediatamente, fornecendo ao aplicativo dados recentes que podem ser usados. Em seguida, quando a solicitação de rede retornar, o aplicativo será atualizado usando os dados mais recentes da rede.
                            </p>
                            
                            <h3>
                              Intercepte a solicitação de rede e armazene a resposta em cache
                            </h3>
                            
                            <p>
                              Nós precisamos modificar o service worker para interceptar solicitações para a Weather API e armazenar suas respostas no cache para que possamos acessá-las com facilidade posteriormente. Na estratégia que prioriza o cache em vez da rede, esperamos que a resposta da rede seja a "fonte da verdade", sempre nos fornecendo as informações mais recentes. Se isso não for possível, não há problema, pois já recuperamos os dados de cache mais recentes no nosso aplicativo.
                            </p>
                            
                            <p>
                              No service worker, vamos adicionar um <code>dataCacheName</code> para que possamos separar os dados do aplicativo do shell do aplicativo. Quando o shell do aplicativo for atualizado e os caches mais antigos forem limpos, nossos dados estarão intocados, prontos para um carregamento rápido. Lembre-se de que, se o formato dos seus dados for alterado no futuro, você precisará de uma maneira para gerenciar isso e garantir que o shell do aplicativo e o conteúdo permaneçam sincronizados.
                            </p>
                            
                            <p>
                              Adicione a seguinte linha à parte superior do seu arquivo <code>service-worker.js</code>:
                            </p>
                            
                            <pre><code>var dataCacheName = 'weatherData-v1';
</code></pre>
                            
                            <p>
                              Em seguida, atualize o gerenciador de eventos <code>activate</code> para não excluir o cache de dados ao limpar o cache do shell do aplicativo.
                            </p>
                            
                            <pre><code>if (key !== cacheName && key !== dataCacheName) {
</code></pre>
                            
                            <p>
                              Finalmente, atualize o gerenciador de eventos <code>fetch</code> para gerenciar solicitações para a API de dados separadamente de outras solicitações.
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
                              O código intercepta a solicitação e verifica se o URL é iniciado pelo endereço da Weather API. Em caso positivo, usaremos <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API">fetch</a> para realizar a solicitação. Quando a resposta for retornada, nosso código abrirá o cache, clonará a resposta, a armazenará no cache e, por fim, a retornará para o solicitador original.
                            </p>
                            
                            <p>
                              Nosso aplicativo ainda não funciona off-line. Já implementamos o armazenamento em cache e a recuperação para o shell do aplicativo, mas mesmo armazenando dados em cache, o aplicativo ainda não verifica o cache para ver se há algum dado meteorológico.
                            </p>
                            
                            <h3>
                              Realizando as solicitações
                            </h3>
                            
                            <p>
                              Como já mencionamos, o aplicativo precisa acionar duas solicitações assíncronas, uma para o cache e outra para a rede. O aplicativo usa o objeto <code>caches</code> disponível em <code>window</code> para acessar o cache e recuperar os dados mais recentes. Esse é um exemplo excelente de aprimoramento progressivo, pois o objeto <code>caches</code> pode não estar disponível em todos os navegadores e, se não houver uma solicitação de rede, ele ainda funcionará.
                            </p>
                            
                            <p>
                              Para isso, é preciso:
                            </p>
                            
                            <ol start="1">
                              <li>
                                Verificar se o objeto <code>caches</code> está disponível no objeto global <code>window</code>.
                              </li>
                              
                              <li>
                                Solicitar dados do cache.
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                Na primeira execução, seu aplicativo deve imediatamente mostrar ao usuário a previsão de <code>initialWeatherForecast</code>.
                              </li>
                            </ul>
                            
                            <ol start="3">
                              <li>
                                Solicitar dados do servidor.
                              </li>
                            </ol>
                            
                            <ul>
                              <li>
                                Depois que o registro de um service worker é cancelado, ele pode permanecer listado até que a janela de navegador que o contém seja fechada.
                              </li>
                              <li>
                                Se várias janelas do seu aplicativo estiverem abertas, o novo service worker não entrará em vigor até que todas tenham sido recarregadas e atualizadas para o service worker mais recente.
                              </li>
                            </ul>
                            
                            <h4>
                              Obter dados do cache
                            </h4>
                            
                            <p>
                              Em seguida, precisamos verificar se o objeto <code>caches</code> existe e solicitar os dados mais recentes dele. Localize o comentário <code>TODO add cache logic here</code> em <code>app.getForecast()</code>, e depois adicione o código abaixo do comentário.
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
                              Nosso aplicativo de previsão do tempo agora realiza duas solicitações de dados assíncronas, uma para o <code>cache</code>e a outra via XHR. Se houver dados no cache, eles serão retornados e renderizados com extrema rapidez (dezenas de milissegundos) e atualizarão o cartão somente se o XHR ainda estiver pendente. Em seguida, quando o XHR responder, o cartão será atualizado com os dados mais recentes diretamente da weather API.
                            </p>
                            
                            <p>
                              Repare como a solicitação de cache e a solicitação XHR terminam com uma chamada para atualizar o cartão de previsão. Como o app sabe se ele está exibindo os dados mais recentes? Isso é gerenciado no seguinte código de <code>app.updateForecastCard</code>:
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
                              Toda vez que um cartão é atualizado, o aplicativo armazena o timestamp dos dados em um atributo oculto no cartão. O aplicativo só resgata se o timestamp que já existe no cartão for mais recente que os dados que foram passados para a função.
                            </p>
                            
                            <h3>
                              Faça testes
                            </h3>
                            
                            <p>
                              Agora aplicativo deve ser completamente funcional off-line. Salve algumas cidades e pressione o botão de atualização no aplicativo para obter dados meteorológicos atuais, e depois fique off-line e recarregue a página.
                            </p>
                            
                            <p>
                              Em seguida, vá para o painel <strong>Cache Storage</strong> no painel <strong>Application</strong> do DevTools. Expanda a seção e você deve ver o nome do cache do seu shell do aplicativo e cache de dados listado do lado esquerdo. Abrir o cache de dados deve mostrar os dados armazenados para cada cidade.
                            </p>
                            
                            <p>
                              <img src="img/cf095c2153306fa7.png" alt="cf095c2153306fa7.png" />
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-07/">Link</a>
                            </p>
                            
                            <h2>
                              Ofereça suporte à integração nativa
                            </h2>
                            
                            <p>
                              Ninguém gosta de digitar URLs longos em um dispositivo móvel se não for absolutamente necessário. Com o recurso de adicionar à tela inicial, seus usuários podem optar por adicionar um link de atalho ao seu dispositivo nativo da mesma forma que instalariam um aplicativo nativo de uma loja de app, mas com menos atrito.
                            </p>
                            
                            <h3>
                              Banners de instalação de aplicativos da Web e recurso de adicionar à tela inicial para o Chrome no Android
                            </h3>
                            
                            <p>
                              Os banners de instalação de aplicativos web permitem que seus usuários adicionem seu aplicativo web forma rápida e tranquila à tela inicial do seu dispositivo, o que facilita a inicialização e o retorno ao aplicativo. É muito fácil adicionar banners de instalação de aplicativo e o Chrome realiza a maior parte do trabalho para você. Basta incluir um arquivo de manifesto de app da Web com detalhes sobre o aplicativo.
                            </p>
                            
                            <p>
                              Em seguida, o Chrome usa um conjunto de critérios, incluindo o uso de um service worker, status de SSL e dados heurísticos de frequência de visitas para determinar quando mostrar o banner. Além disso, um usuário pode adicionar o aplicativo manualmente pelo botão de menu "Add to Home Screen" no Chrome.
                            </p>
                            
                            <h4>
                              Declare um manifesto de aplicativo com um arquivo <code>manifest.json</code>
                            </h4>
                            
                            <p>
                              O manifesto do app da Web é um arquivo JSON simples que proporciona a você, desenvolvedor, a capacidade de controlar a aparência do seu aplicativo para o usuário nas áreas onde ele pode ver aplicativos (por exemplo, na tela inicial do celular), direcionar o que o usuário pode acessar e, o mais importante, como pode acessar.
                            </p>
                            
                            <p>
                              Usando o manifesto do app da Web, seu aplicativo pode:
                            </p>
                            
                            <ul>
                              <li>
                                Se a solicitação do servidor ainda estiver pendente, atualizar o aplicativo com os dados em cache.
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
                              Crie um arquivo com o nome <code>manifest.json</code> em sua pasta <code>work</code> e copie/cole os conteúdos a seguir:
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
                              O manifesto uma variedade de ícones, destinados a diferentes tamanhos de tela. No momento da redação deste artigo, Chrome e Opera Mobile, os únicos navegadores que suportam manifestos de apps da Web, não usam nada menor que 192px.
                            </p>
                            
                            <p>
                              Uma maneira fácil de controlar como o aplicativo é inicializado é adicionar uma string de consulta ao parâmetro <code>start_url</code> e depois usar um pacote de análise para rastrear a string de consulta Se usar esse método, lembre-se de atualizar a lista de arquivos em cache pelo App Shell para garantir que o arquivo com a string de consulta está armazenado no cache.
                            </p>
                            
                            <h4>
                              Envie informações sobre seu arquivo de manifesto ao navegador
                            </h4>
                            
                            <p>
                              Agora, adicione a linha a seguir na parte inferior do elemento <code>&lt;head&gt;</code> no seu arquivo <code>index.html</code>:
                            </p>
                            
                            <pre><code>&lt;link rel="manifest" href="/manifest.json"&gt;
</code></pre>
                            
                            <h4>
                              Práticas recomendadas
                            </h4>
                            
                            <ul>
                              <li>
                                Salvar os dados para acesso rápido posteriormente.
                              </li>
                              <li>
                                Atualizar o aplicativo com os dados mais recentes do servidor.
                              </li>
                              <li>
                                Define icon sets for different density screens. Chrome will attempt to use the icon closest to 48dp, for example, 96px on a 2x device or 144px for a 3x device.
                              </li>
                              <li>
                                Remember to include an icon with a size that is sensible for a splash screen and don't forget to set the <code>background_color</code>.
                              </li>
                            </ul>
                            
                            <p>
                              Leitura adicional:
                            </p>
                            
                            <p>
                              <a href="/web/fundamentals/engage-and-retain/simplified-app-installs/">Como usar banners de instalação de aplicativo</a>
                            </p>
                            
                            <h3>
                              Elementos de adição à tela inicial para Safari no iOS
                            </h3>
                            
                            <p>
                              No seu <code>index.html</code>, adicione o seguinte à parte inferior do elemento <code>&lt;head&gt;</code>:
                            </p>
                            
                            <pre><code>  &lt;!-- Add to home screen for Safari on iOS --&gt;
  &lt;meta name="apple-mobile-web-app-capable" content="yes"&gt;
  &lt;meta name="apple-mobile-web-app-status-bar-style" content="black"&gt;
  &lt;meta name="apple-mobile-web-app-title" content="Weather PWA"&gt;
  &lt;link rel="apple-touch-icon" href="images/icons/icon-152x152.png"&gt;
</code></pre>
                            
                            <h3>
                              Ícone de bloco para janelas
                            </h3>
                            
                            <p>
                              No seu <code>index.html</code>, adicione o seguinte à parte inferior do elemento <code>&lt;head&gt;</code>:
                            </p>
                            
                            <pre><code>  &lt;meta name="msapplication-TileImage" content="images/icons/icon-144x144.png"&gt;
  &lt;meta name="msapplication-TileColor" content="#2F3BA2"&gt;
</code></pre>
                            
                            <h3>
                              Faça testes
                            </h3>
                            
                            <p>
                              Nesta seção, mostraremos algumas maneiras de testar o manifesto do seu app da Web.
                            </p>
                            
                            <p>
                              A primeira maneira é DevTools. Abra o painel <strong>Manifest</strong> no painel <strong>Application</strong>. Se adicionou as informações do manifesto corretamente, você poderá vê-las analisadas e exibidas em um formato de fácil leitura para seres humanos neste painel.
                            </p>
                            
                            <p>
                              Também é possível testar o recurso de adicionar à tela principal característica a partir deste painel. Clique no botão <strong>Add to homescreen</strong>. Você deve ver uma mensagem "adicionar este site à sua estante" abaixo da sua barra de URL, como na imagem abaixo.
                            </p>
                            
                            <p>
                              <img src="img/cbfdd0302b611ab0.png" alt="cbfdd0302b611ab0.png" />
                            </p>
                            
                            <p>
                              Este é o equivalente para desktop do recurso adicionar à tela principal de dispositivos móveis. Se conseguir acionar esta solicitação com sucesso em desktop, você pode ter certeza de que usuários de dispositivos móveis conseguem adicionar seu aplicativo a seus aparelhos.
                            </p>
                            
                            <p>
                              A segunda maneira de testar é via Web Server for Chrome. Com esta abordagem, você expõe seu servidor de desenvolvimento local (no seu desktop ou laptop) a outros computadores, e depois basta acessar seu Progressive Web App de um dispositivo móvel real.
                            </p><aside class="warning">

<p>Opening up a port for remote access is handy for testing this step but may be blocked by your computer's firewall rules or network administrator. Opening ports for remote access is generally not a good thing to leave running on your computer. So, for security reasons, when you've completed testing this step, disable the <code>Accessible on local network</code> option and restart your web server.</p>

</aside> 
                            
                            <p>
                              Na caixa de diálogo do Web Server for Chrome, selecione a opção <code>Accessible on local network</code>:
                            </p>
                            
                            <p>
                              <img src="img/81347b12f83e4291.png" alt="81347b12f83e4291.png" />
                            </p>
                            
                            <p>
                              Alterne o Web Server para <code>STOPPED</code> e de volta para <code>STARTED</code>. Você verá um novo URL que pode ser usado para acessar o aplicativo remotamente.
                            </p>
                            
                            <p>
                              Agora, acesse seu site a partir de um dispositivo móvel, usando o novo URL.
                            </p>
                            
                            <p>
                              Você verá erros do service worker no console ao testar desta forma, porque o Service Worker não está sendo servido por HTTPS.
                            </p>
                            
                            <p>
                              Usando o Chrome a partir de um dispositivo Android, tente adicionar o aplicativo à tela inicial e verificar que a tela de inicialização aparece corretamente e os ícones corretos são utilizados.
                            </p>
                            
                            <p>
                              No Safari e no Internet Explorer, você também pode adicionar o aplicativo à sua tela inicial manualmente.
                            </p>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/step-08/">Link</a>
                            </p>
                            
                            <h2>
                              Implante em um host seguro e comemore!
                            </h2>
                            
                            <p>
                              A etapa final é implantar nosso aplicativo de previsão do tempo em um servidor que ofereça suporte a HTTPS. Se ainda não tiver um, a abordagem mais fácil (e gratuita) é usar o conteúdo estático hospedado no Firebase. Ele é muito fácil de usar, fornece conteúdo por HTTPS e tem o apoio de uma CDN global.
                            </p>
                            
                            <h3>
                              Crédito extra: CSS minificado e em linha
                            </h3>
                            
                            <p>
                              Mais de uma consideração deve feita ao minificar os estilos principais e adicioná-los em linha diretamente no <code>index.html</code>. O <a href="/speed">Page Speed Insights</a> recomenda fornecer o conteúdo acima da dobra dos primeiros 15 mil bytes da solicitação.
                            </p>
                            
                            <p>
                              Veja até onde você pode reduzir a solicitação inicial com todos os elementos em linha.
                            </p>
                            
                            <p>
                              Leitura adicional: <a href="/speed/docs/insights/rules">Regras do Page Speed Insight</a>
                            </p><aside class="key-point">

<p>This step requires you to have  <a href="https://docs.npmjs.com/getting-started/installing-node">Node &#x26; NPM</a> installed on your system. If it's not, you can use any other hosting provider that supports HTTP<strong>S</strong>. We've used Firebase because it automatically redirects users from HTTP to HTTP<strong>S</strong>. If you use a different provider, be sure they're always redirects to HTTP<strong>S</strong>.</p>

</aside> 
                            
                            <h3>
                              Implemente no Firebase
                            </h3>
                            
                            <p>
                              Se nunca tiver usado o Firebase, você deverá criar uma conta e instalar algumas ferramentas primeiro.
                            </p>
                            
                            <ol start="1">
                              <li>
                                Crie uma conta do Firebase em <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a>
                              </li>
                              
                              <li>
                                Instale as ferramentas do Firebase via npm: <code>npm install -g firebase-tools</code>
                              </li>
                            </ol>
                            
                            <p>
                              Após criar a conta e fazer login, você estará pronto para implantar!
                            </p>
                            
                            <ol start="1">
                              <li>
                                Crie um novo aplicativo em <a href="https://firebase.google.com/console/">https://firebase.google.com/console/</a>
                              </li>
                              
                              <li>
                                Se não tiver feito login nas ferramentas do Firebase recentemente, atualize suas credenciais: <code>firebase login</code>
                              </li>
                              
                              <li>
                                Inicialize seu aplicativo e forneça o diretório (provavelmente <code>work</code>) onde se encontra o aplicativo concluído: <code>firebase init</code>
                              </li>
                              
                              <li>
                                Por fim, implemente seu aplicativo no Firebase: <code>firebase deploy</code>
                              </li>
                              
                              <li>
                                Comemore. Pronto! Seu aplicativo será implantado no domínio: <code>https://YOUR-FIREBASE-APP.firebaseapp.com</code>
                              </li>
                            </ol>
                            
                            <p>
                              Leitura adicional: <a href="https://www.firebase.com/docs/hosting/guide/">Guia de hospedagem do Firebase</a>
                            </p>
                            
                            <h3>
                              Faça testes
                            </h3>
                            
                            <ul>
                              <li>
                                Ter uma presença avançada na tela inicial do Android do usuário
                              </li>
                            </ul>
                            
                            <p>
                              <a href="https://weather-pwa-sample.firebaseapp.com/final/">Link</a>
                            </p>
                            
                            <h2>
                              Encontrou um problema ou tem feedback? {: .hide-from-toc }
                            </h2>
                            
                            <p>
                              Ajude-nos a melhorar nossos codelabs reportando um <a href="https://github.com/googlecodelabs/your-first-pwapp/issues">problema</a> hoje. E obrigado!
                            </p>