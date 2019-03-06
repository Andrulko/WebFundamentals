project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Existem dois tipos de banners de instalação de aplicativo: os web e os nativos. Eles dão a você a possibilidade de permitir que os usuários adicionem seu aplicativo web ou nativo às telas iniciais de forma rápida e fácil, sem sair do navegador.

{# wf_updated_on: 2017-09-27 #} {# wf_published_on: 2014-12-16 #}

# Banners de instalação de aplicativo web {: .page-title }

{% include "web/_shared/translation-out-of-date.html" %}

{% include "web/_shared/contributors/mattgaunt.html" %} {% include "web/_shared/contributors/paulkinlan.html" %}

Há dois tipos de banner de instalação de aplicativo: os **web** e os [**nativos**](native-app-install). Eles permitem que os usuários adicionem seu aplicativo web ou nativo às telas iniciais de forma rápida e fácil sem sair do navegador.

* On mobile, Chrome will generate a [WebAPK](/web/fundamentals/integration/webapks), creating an even more integrated experience for your users.
* Ter um [service worker](/web/fundamentals/getting-started/primers/service-workers) registrado no seu site.

## Eventos do banner de instalação de aplicativo

É muito fácil adicionar banners de instalação de aplicativo: o Chrome faz todo o trabalho duro para você. Você precisa incluir o arquivo de manifesto do aplicativo web no site com detalhes sobre o aplicativo.

Em seguida, o Chrome usa um conjunto de critérios e dados heurísticos de frequência de acesso para determinar quando exibir o banner. Continue lendo para saber mais.

## Banner de instalação de aplicativo nativo

<figure class="attempt-right">
  <img src="images/a2hs-dialog-g.png" alt="Add to Home Screen dialog on Android">
  <figcaption>Add to Home Screen dialog on Android</figcaption>
</figure>

Observação: "Adicionar à tela inicial" (às vezes abreviado como A2HS) é outro nome dos Banners de instalação de aplicativo web. Os dois termos são equivalentes.

1. Abra o Chrome DevTools.
2. Acesse o painel **Application**.
3. Siga para a guia **Manifest**.

<div class="clearfix"></div>

O Chrome exibe o banner automaticamente quando o aplicativo atende aos seguintes critérios:

### Quais são os critérios?

Observação: os banners de instalação de aplicativo web são uma tecnologia em ascensão. Os critérios para exibi-los podem mudar no futuro. Acesse [O que exatamente torna algo um Progressive Web App?](https://infrequently.org/2016/09/what-exactly-makes-something-a-progressive-web-app/) para ver uma referência canônica (que será atualizada com o tempo) sobre os últimos critérios para os banners de instalação de aplicativo web.

Depois de configurar o manifesto do aplicativo web, o ideal é confirmar que ele foi definido corretamente. Há duas abordagens à sua disposição. Uma é manual, e a outra é automatizada.

    window.addEventListener('beforeinstallprompt', function(e) {
      // beforeinstallprompt Event fired
    
      // e.userChoice will return a Promise.
      // For more details read: https://developers.google.com/web/fundamentals/getting-started/primers/promises
      e.userChoice.then(function(choiceResult) {
    
        console.log(choiceResult.outcome);
    
        if(choiceResult.outcome == 'dismissed') {
          console.log('User cancelled home screen install');
        }
        else {
          console.log('User added to home screen');
        }
      });
    });
    

### Testando o banner de instalação de aplicativo {: #test }

Para acionar manualmente o banner de instalação de aplicativo:

<pre class="prettyprint">window.addEventListener('beforeinstallprompt', (e) => {
  // Prevent Chrome 67 and earlier from automatically showing the prompt
  e.preventDefault();
  // Stash the event so it can be triggered later.
  deferredPrompt = e;
  <strong>// Update UI notify the user they can add to home screen
  btnAdd.style.display = 'block';</strong>
});
</pre>

Success: you may want to wait before showing the prompt to the user, so you don't distract them from what they're doing. For example, if the user is in a check-out flow, or creating their account, let them complete that before interrupting them with the prompt.

### Um usuário instalou o aplicativo?

Acesse [Simular eventos de adicionar à tela inicial](/web/tools/chrome-devtools/progressive-web-apps#add-to-homescreen) para saber mais.

Para fazer testes automatizados com o banner de instalação de aplicativo, use o Lighthouse. O Lighthouse é uma ferramenta de auditoria de aplicativos web. Você pode executá-lo como uma extensão do Chrome ou um módulo do NPM. Para testar o aplicativo, você deve fornece o Lighthouse a uma página específica para auditar. O Lighthouse realiza um conjunto de auditorias na página e insere os resultados em um relatório.

    window.addEventListener('beforeinstallprompt', function(e) {
      console.log('beforeinstallprompt Event fired');
      e.preventDefault();
      return false;
    });
    

Os dois conjuntos de auditorias do Lighthouse na captura de tela abaixo representam todos os testes de que sua página precisa para passar a exibir um banner de instalação de aplicativo.

## The mini-info bar

<figure class="attempt-right">
  <img
      class="screenshot"
      src="/web/updates/images/2018/06/a2hs-infobar-cropped.png">
  <figcaption>
    The mini-infobar
  </figcaption>
</figure>

The mini-infobar is an interim experience for Chrome on Android as we work towards creating a consistent experience across all platforms that includes an install button into the omnibox.

Acesse [Auditar apps da web com o Lighthouse](/web/tools/lighthouse/) para começar a usá-lo.

O Chrome oferece um mecanismo fácil para determinar como o usuário respondeu ao banner de instalação de aplicativo e até mesmo cancelá-lo ou deferi-lo para um momento mais conveniente.

## Feedback {: .hide-from-toc }

O evento `beforeinstallprompt` retorna uma promessa chamada `userChoice` que resolve quando o usuário executa uma ação em relação à solicitação. A promessa retorna um objeto com um valor de `dismissed` no atributo `outcome` ou `accepted` se o usuário adicionou a página à tela inicial.

<div class="clearfix"></div>

## Determine if the app was successfully installed {: #appinstalled }

Essa é uma boa ferramenta para entender como seus usuários interagem com a solicitação de instalação de aplicativo.

    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

## Detecting if your app is launched from the home screen {: #detect-mode }

### Retardando ou cancelando a solicitação

O Chrome gerencia quando acionar a solicitação, mas, para alguns sites, isso pode não ser o ideal. Você pode deferir a solicitação para um momento posterior no uso do aplicativo ou mesmo cancelá-la.

Quando o Chrome decide solicitar o usuário a instalar o aplicativo, você pode evitar a ação padrão e armazenar o evento para um momento posterior. Então, quando o usuário tiver uma interação positiva com seu site, você poderá acionar novamente a solicitação chamando `prompt()` no evento armazenado.

    "prefer_related_applications": true,
    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

This causes Chrome to show the banner and all the Promise attributes such as `userChoice` will be available to bind to so that you can understand what action the user took.

    if (window.matchMedia('(display-mode: standalone)').matches) {
      console.log('display-mode is standalone');
    }
    

### Critérios para exibir o banner

Como alternativa, você pode cancelar a solicitação ao impedir a ação padrão.

    if (window.navigator.standalone === true) {
      console.log('display-mode is standalone');
    }
    

## Updating your app's icon and name

### Requisitos do manifesto

Os banners de instalação de aplicativo nativo são parecidos com os [banners de instalação de aplicativo web](.) mas, em vez de adicionarem à tela inicial, eles permitem que o usuário instalem seu aplicativo nativo sem sair do site.

### Desktop

Os critérios também são parecidos com os do banner de instalação de aplicativo web, exceto pela necessidade de ter um service worker. O seu site deve:

## Test your add to home screen experience {: #test }

Para integrar a qualquer manifesto, adicione uma matriz `related_applications` com as plataformas de `play` (do Google Play) e o ID do aplicativo.

Se só quiser oferecer ao usuário a possibilidade de instalar o seu aplicativo Android e não exibir o banner de instalação de aplicativo web, adicione `"prefer_related_applications": true`. Por exemplo:

### Chrome for Android

1. Open a [remote debugging](/web/tools/chrome-devtools/remote-debugging/) session to your phone or tablet.
2. Go to the **Application** panel.
3. Go to the **Manifest** tab.
4. Click **Add to home screen**

### Chrome OS, Linux, or Windows

1. Open Chrome DevTools
2. Go to the **Application** panel.
3. Go to the **Manifest** tab.
4. Click **Add to home screen**

{# wf_devsite_translation #}

### Will `beforeinstallprompt` be fired?

The easiest way to test if the `beforeinstallprompt` event will be fired, is to use [Lighthouse](/web/tools/lighthouse/) to audit your app, and check the results of the [User Can Be Prompted To Install The Web App](/web/tools/lighthouse/audits/install-prompt) test.