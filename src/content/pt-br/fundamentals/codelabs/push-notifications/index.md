project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Neste codelab você aprenderá como adicionar notificações push ao seu app da Web.

{# wf_updated_on: 2017-10-06 #} {# wf_published_on: 2016-01-01 #}

# Adicionar Notificações Push a um app da Web {: .page-title }

{% include "web/_shared/contributors/mattgaunt.html" %}

## Obter Configuração

Visão geral do ##

### O que você aprenderá

* Como se inscrever e cancelar a inscrição de um usuário para mensagens push
* Como lidar com a entrada de mensagens push
* Como exibir uma notificação
* Como responder a cliques de notificação

### O que será necessário

* Chrome 52 ou superior
* [Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb), seu servidor de Web preferido
* Um editor de texto
* Conhecimento básico de HTML, CSS, JavaScript e Chrome DevTools
* O exemplo de código, consulte Obter Configuração

## Registrar um Service Worker

### Faça download do exemplo de código

Mensagens push fornecem uma maneira simples e eficaz para voltar a interagir com seus usuários e neste codelab você vai aprender como adicionar notificações push ao seu app da Web.

[Download source code](https://github.com/googlechrome/push-notifications/archive/master.zip)

or by cloning this git repo:

    git clone https://github.com/GoogleChrome/push-notifications.git
    

ou clonando este repositório git:

### Instale e verifique o servidor de Web

Se você baixou a fonte como um zip, descompactá-lo deve fornecer uma pasta raiz `push-notifications-master`.

[Install Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb)

After installing the Web Server for Chrome app, click on the Apps shortcut on the bookmarks bar:

![a80b29d5e878df22.png](img/a80b29d5e878df22.png)

In the ensuing window, click on the Web Server icon:

![dc07bbc9fcfe7c5b.png](img/dc07bbc9fcfe7c5b.png)

You'll see this dialog next, which allows you to configure your local web server:

![433870360ad308d4.png](img/433870360ad308d4.png)

Click the **choose folder** button, and select the app folder. This will enable you to serve your work in progress via the URL highlighted in the web server dialog (in the **Web Server URL(s)** section).

Clique no botão **choose folder**, e selecione a pasta do app. Isto permitirá servir o seu trabalho em andamento pelo URL em destaque na caixa de diálogo do servidor de Web (na seção **Web Server URL(s)**).

![39b4e0371e9703e6.png](img/39b4e0371e9703e6.png)

Then stop and restart the server by sliding the toggle labeled "Web Server: STARTED" to the left and then back to the right.

![daefd30e8a290df5.png](img/daefd30e8a290df5.png)

Now visit your site in your web browser (by clicking on the highlighted Web Server URL) and you should see a page that looks like this:

![4525ec369fc2ae47.png](img/4525ec369fc2ae47.png)

### Atualize sempre o service worker

During development it's helpful to ensure your service worker is always up to date and has the latest changes.

Durante o desenvolvimento é útil garantir que seu service worker esteja sempre atualizado e tenha as últimas alterações.

![6b698d7c7bbf1bc0.png](img/6b698d7c7bbf1bc0.png)

## Inicializar Estado

In your `app` directory, notice that you have an empty file named `sw.js`. This file will be your service worker, for now it can stay empty and we'll be adding code to it later.

No seu diretório `app`, perceba que você tem um arquivo vazio chamado `sw.js`. Este arquivo será o seu service worker. Por enquanto ele pode ficar vazio, adicionaremos código a ele mais tarde.

Primeiro, precisamos registrar este arquivo como nosso Service Worker.

Nossa página `app/index.html` carrega `scripts/main.js`, e é neste arquivo de JavaScript que registraremos nosso service worker.

    if ('serviceWorker' in navigator && 'PushManager' in window) {
      console.log('Service Worker and Push is supported');
    
      navigator.serviceWorker.register('sw.js')
      .then(function(swReg) {
        console.log('Service Worker is registered', swReg);
    
        swRegistration = swReg;
      })
      .catch(function(error) {
        console.error('Service Worker Error', error);
      });
    } else {
      console.warn('Push messaging is not supported');
      pushButton.textContent = 'Push Not Supported';
    }
    

Adicione o código a seguir ao `scripts/main.js`:

#### Experimente

Esse código verifica se service workers e mensagens push são suportados pelo navegador atual e, caso sejam, ele registra nosso arquivo `sw.js`.

Verifique suas alterações, abrindo o URL **127.0.0.1:8887** no navegador.

![de3ceca91043d278.png](img/de3ceca91043d278.png)

### Obtenha as Chaves do Servidor do Aplicativo

To work with this code lab you need to generate some application server keys which we can do with this companion site: <https://web-push-codelab.glitch.me/>

Para trabalhar com este codelab, você precisa gerar algumas chaves de servidor de aplicativo, o que podemos fazer com este site associado: <https://web-push-codelab.glitch.me/>

![a1304b99e7b981dd.png](img/a1304b99e7b981dd.png)

Copy your public key into `scripts/main.js` replacing the `<Your Public Key>` value:

    const applicationServerPublicKey = '<Your Public Key>';
    

Copie a chave pública para `scripts/main.js` substituindo o valor `<Your Public Key>`:

## Inscreva o usuário

Observação: Nunca se deve colocar sua chave privada no aplicativo da Web!

No momento, o botão do aplicativo da Web está desativado e não pode ser clicado. Isso ocorre porque é uma boa prática desativar o botão de push por padrão e ativá-lo quando se sabe que push é suportado e pode-se saber se o usuário está inscrito ou não.

Vamos criar duas funções em `scripts/main.js`, uma chamada `initialiseUI`, que vai verificar se o usuário está inscrito, e uma chamada `updateBtn`, que habilitará nosso botão e mudará o texto dependendo se do status de inscrição do usuário.

    function initialiseUI() {
      // Set the initial subscription value
      swRegistration.pushManager.getSubscription()
      .then(function(subscription) {
        isSubscribed = !(subscription === null);
    
        if (isSubscribed) {
          console.log('User IS subscribed.');
        } else {
          console.log('User is NOT subscribed.');
        }
    
        updateBtn();
      });
    }
    

Nossa função `initialiseUI` deve ter a seguinte aparência:

Nosso novo método usa o `swRegistration` da etapa anterior e chama `getSubscription()` em seu `pushManager`. `getSubscription()` é um método que retorna uma promessa que se resolve com a inscrição atual, se houver; caso contrário, ele retorna `null`. Com isso, podemos verificar se o usuário já está inscrito ou não, definir um estado e depois chamar `updateBtn()`, para que o botão possa ser ativado com algum texto útil.

    function updateBtn() {
      if (isSubscribed) {
        pushButton.textContent = 'Disable Push Messaging';
      } else {
        pushButton.textContent = 'Enable Push Messaging';
      }
    
      pushButton.disabled = false;
    }
    

Adicione o código a seguir para implementar a função `updateBtn()`.

Esta função simplesmente muda o texto, dependendo do status de inscrição do usuário e, em seguida, ativa o botão.

    navigator.serviceWorker.register('sw.js')
    .then(function(swReg) {
      console.log('Service Worker is registered', swReg);
    
      swRegistration = swReg;
      initialiseUI();
    })
    

#### Experimente

A última coisa a fazer é chamar `initialiseUI()` quando nosso service worker está registrado.

![15f6375617c11974.png](img/15f6375617c11974.png)

When we progress through the rest of the code lab you should see the button text change when the user subscribed / un-subscribed.

## Permissão de Gerenciamento Negada

Com o progresso ao longo do codelab, você deve ver o texto do botão mudar quando o usuário está inscrito/não inscrito.

No momento, nosso botão 'Enable Push Messaging' não faz quase nada, então vamos consertar isso.

    function initialiseUI() {
      pushButton.addEventListener('click', function() {
        pushButton.disabled = true;
        if (isSubscribed) {
          // TODO: Unsubscribe user
        } else {
          subscribeUser();
        }
      });
    
      // Set the initial subscription value
      swRegistration.pushManager.getSubscription()
      .then(function(subscription) {
        isSubscribed = !(subscription === null);
    
        updateSubscriptionOnServer(subscription);
    
        if (isSubscribed) {
          console.log('User IS subscribed.');
        } else {
          console.log('User is NOT subscribed.');
        }
    
        updateBtn();
      });
    }
    

Adicione um ouvinte de clique ao nosso botão na função `initialiseUI()`, assim:

Quando o usuário clica no botão de push, primeiro desativamos o botão, apenas para certificar que o usuário não possa clicar nele uma segunda vez enquanto o estamos inscrevendo para push, pois isso pode levar algum tempo.

    function subscribeUser() {
      const applicationServerKey = urlB64ToUint8Array(applicationServerPublicKey);
      swRegistration.pushManager.subscribe({
        userVisibleOnly: true,
        applicationServerKey: applicationServerKey
      })
      .then(function(subscription) {
        console.log('User is subscribed:', subscription);
    
        updateSubscriptionOnServer(subscription);
    
        isSubscribed = true;
    
        updateBtn();
      })
      .catch(function(err) {
        console.log('Failed to subscribe the user: ', err);
        updateBtn();
      });
    }
    

Em seguida, chamamos `subscribeUser()` quando sabemos que o usuário não está inscrito atualmente, portanto, copie e cole o código a seguir no `scripts/main.js`.

Vamos analisar o que este código está fazendo e como está inscrevendo o usuário para mensagens push.

Primeiro, tomamos a chave pública do servidor do aplicativo, que é codificada com base em URL 64 seguro, e a convertemos em um `UInt8Array`, pois esta é a interação esperada da chamada de inscrição. Já lhe demos a função `urlB64ToUint8Array` no topo de `scripts/main.js`.

    const applicationServerKey = urlB64ToUint8Array(applicationServerPublicKey);
    swRegistration.pushManager.subscribe({
      userVisibleOnly: true,
      applicationServerKey: applicationServerKey
    })
    

Depois de converter o valor, chamamos o método `subscribe()` no `pushManager` do nosso service worker, passando a chave pública do nosso servidor de aplicativo e o valor `userVisibleOnly: true`.

O parâmetro `userVisibleOnly` é, basicamente, uma admissão de que você vai mostrar uma notificação cada vez que um push for enviado. No momento desta redação, este valor é obrigatório e deve ser verdadeiro.

1. O usuário concedeu permissão para exibir notificações.
2. O navegador enviou uma solicitação de rede a um serviço de push para obter os detalhes para gerar um PushSubscription.

Chamar `subscribe()` retorna uma promessa que se resolverá após as seguintes etapas:

    swRegistration.pushManager.subscribe({
      userVisibleOnly: true,
      applicationServerKey: applicationServerKey
    })
    .then(function(subscription) {
      console.log('User is subscribed:', subscription);
    
      updateSubscriptionOnServer(subscription);
    
      isSubscribed = true;
    
      updateBtn();
    
    })
    .catch(function(err) {
      console.log('Failed to subscribe the user: ', err);
      updateBtn();
    });
    

A promessa `subscribe()` se resolve com um `PushSubscription` se essas etapas tiverem sido bem sucedidas. Se o usuário não conceder permissão, ou se houver qualquer problema para inscrever o usuário, a promessa será rejeitada com um erro. Isso nos dá a seguinte cadeia de promessa em nosso codelab:

Com isso, obtemos uma assinatura e tratamos o usuário como inscrito, ou pegamos o erro e o imprimimos para o console. Em ambos os cenários, chamamos `updateBtn()` para garantir que o botão seja reativado e tenha o texto apropriado.

    function updateSubscriptionOnServer(subscription) {
      // TODO: Send subscription to application server
    
      const subscriptionJson = document.querySelector('.js-subscription-json');
      const subscriptionDetails =
        document.querySelector('.js-subscription-details');
    
      if (subscription) {
        subscriptionJson.textContent = JSON.stringify(subscription);
        subscriptionDetails.classList.remove('is-invisible');
      } else {
        subscriptionDetails.classList.add('is-invisible');
      }
    }
    

#### Experimente

O método `updateSubscriptionOnServer` é um método onde, em um aplicativo real enviaríamos nossa inscrição para um back-end; porém, para o nosso codelab, vamos imprimir a inscrição em nossa IU, que nos será útil mais tarde. Adicione este método a `scripts/main.js`:

![227cea0abe03a5b4.png](img/227cea0abe03a5b4.png)

If you grant the permission you should see the console print "User is subscribed.", the button's text will change to ‘Disable Push Messaging' and you'll be able to view the subscription as JSON at the bottom of the page.

![8fe2b1b110f87b34.png](img/8fe2b1b110f87b34.png)

## Gerenciar um Evento Push

One thing that we haven't handled yet is what happens if the user blocks the permission request. This needs some unique consideration because if the user blocks the permission, our web app will not be able to re-show the permission prompt and will not be able to subscribe the user, so we need to at least disable the push button so the user knows it can't be used.

Uma coisa que ainda não tratamos é o que acontece se o usuário bloquear a solicitação de permissão. Isso precisa de uma consideração exclusiva, porque se o usuário bloquear a permissão, nosso app da Web não será capaz de voltar a mostrar a solicitação de permissão e não poderá inscrever o usuário, portanto, precisamos desativar pelo menos um botão push para que o usuário sabe que ele não pode ser usado.

    function updateBtn() {
      if (Notification.permission === 'denied') {
        pushButton.textContent = 'Push Messaging Blocked.';
        pushButton.disabled = true;
        updateSubscriptionOnServer(null);
        return;
      }
    
      if (isSubscribed) {
        pushButton.textContent = 'Disable Push Messaging';
      } else {
        pushButton.textContent = 'Enable Push Messaging';
      }
    
      pushButton.disabled = false;
    }
    

O lugar óbvio para gerenciarmos este cenário é na função `updateBtn()`. Tudo que precisamos fazer é verificar o valor `Notification.permission`, assim:

#### Experimente

Sabemos que, se a permissão for `denied`, o usuário não pode ser inscrito e não há nada mais que possamos fazer, de modo que desabilitar o botão definitivamente é a melhor abordagem.

![8775071d7fd66432.png](img/8775071d7fd66432.png)

After you've changed this setting, refresh the page and click the *Enable Push Messaging* button and this time select *Block* on the permission dialog. The button text will now be *Push Messaging Blocked* and be disabled.

![2b5314607196f4e1.png](img/2b5314607196f4e1.png)

With this change we can now subscribe the user and we're taking care of the possible permission scenarios.

## Clique em notificação

Com esta alteração, agora podemos inscrever o usuário e estamos cuidando dos possíveis cenários de permissão.

Antes de falar sobre como enviar uma mensagem push do seu back-end, precisamos considerar o que realmente acontecerá quando um usuário inscrito receber uma mensagem push.

Quando disparamos uma mensagem push, o navegador recebe a mensagem push, descobre para qual service worker o push se destina antes de ativar esse service worker e despachar um evento push. Precisamos ouvir esse evento e mostrar uma notificação, como resultado.

    self.addEventListener('push', function(event) {
      console.log('[Service Worker] Push Received.');
      console.log(`[Service Worker] Push had this data: "${event.data.text()}"`);
    
      const title = 'Push Codelab';
      const options = {
        body: 'Yay it works.',
        icon: 'images/icon.png',
        badge: 'images/badge.png'
      };
    
      event.waitUntil(self.registration.showNotification(title, options));
    });
    

Adicione o seguinte código ao seu arquivo `sw.js`:

    self.addEventListener('push', ...... );
    

Vamos passar por este código. Estamos ouvindo por eventos push em nosso service worker, adicionando um ouvinte de eventos ao nosso service worker, que é este pedaço de código:

A menos que você já tenha usado Web Workers antes, `self` é provavelmente novo. `self` está fazendo referência ao service worker em si, por isso estamos adicionando um ouvinte de eventos ao nosso service worker.

    const title = 'Push Codelab';
    const options = {
      body: 'Yay it works.',
      icon: 'images/icon.png',
      badge: 'images/badge.png'
    };
    self.registration.showNotification(title, options);
    

Quando uma mensagem push é recebida, nosso ouvinte de eventos será acionado, e criamos uma notificação chamando `showNotification()` em nosso registro. `showNotification()` espera um `title` e podemos receber um objeto `options`. Aqui, definiremos um corpo, ícone e indicador para a mensagem nas opções (o indicador é usado somente em Android no momento desta redação).

A última coisa a tratar no nosso evento push é `event.waitUntil()`. Este método requer uma promessa e o navegador manterá seu service worker funcionando até que a promessa passado tenha se resolvido.

    const notificationPromise = self.registration.showNotification(title, options);
    event.waitUntil(notificationPromise);
    

Para tornar o código acima de um pouco mais fácil de entender, podemos reescrevê-lo assim:

#### Experimente

Agora que já analisamos o evento push, vamos testar um evento push.

Com nosso evento push no service worker, podemos testar o que acontece quando uma mensagem é recebida desencadeando um evento push falso com o uso de DevTools.

![2b089bdf10a8a945.png](img/2b089bdf10a8a945.png)

Once you've clicked it you should see a notification like this:

![eee7f9133a97c1c4.png](img/eee7f9133a97c1c4.png)

Note: If this step doesn't work, try unregistering your service work, via the *Unregister* link in the DevTools Application panel, wait for the service worker to be stopped, and then reload the page.

## Envio de mensagens push

Observação: Se este passo não funcionar, tente cancelar o registro do seu service worker, através do link *Unregister* no painel DevTools Application, aguarde o service worker ser parado e, em seguida, atualize a página.

Se clicar em uma dessas notificações, você perceberá que nada acontece. Podemos gerenciar cliques em notificação ouvindo por eventos `notificationclick` no seu service worker.

    self.addEventListener('notificationclick', function(event) {
      console.log('[Service Worker] Notification click Received.');
    
      event.notification.close();
    
      event.waitUntil(
        clients.openWindow('https://developers.google.com/web/')
      );
    });
    

Comece adicionando um ouvinte `notificationclick` em `sw.js` assim:

Quando o usuário clica na notificação, o ouvinte de evento `notificationclick` será chamado.

    event.notification.close();
    

Neste codelab, primeiro fechamos a notificação que foi clicada com:

    clients.openWindow('https://developers.google.com/web/')
    

Em seguida, abrimos uma nova janela/guia carregando o url [developers.google.com/web/](/web/), fique à vontade para alterá-lo :)

#### Experimente

Estamos chamando `event.waitUntil()` novamente para garantir que o navegador não encerre nosso service worker antes que nossa nova janela tenha sido exibida.

## Cancelar a inscrição do usuário

Tente acionar uma mensagem automática no DevTools e clicar novamente na notificação. Agora você verá a notificação fechar e abrir uma nova guia.

Vimos que nosso app da Web é capaz de mostrar uma notificação usando DevTools e aprendemos como fechar a notificação de um clique, o próximo passo é enviar uma mensagem push real.

Normalmente, o processo para isso seria o envio de uma inscrição a partir de uma página da Web para um back-end e o back-end acionaria uma mensagem push, fazendo uma chamada de API para o ponto final na inscrição.

![cf0e71f76cb79cc4.png](img/cf0e71f76cb79cc4.png)

Then paste this into the companion site in the *Subscription to Send To* text area:

![a12fbfdc08233592.png](img/a12fbfdc08233592.png)

Then under *Text to Send* you can add any string you want to send with the push message and finally click the *Send Push Message* button.

![2973c2b818ca9324.png](img/2973c2b818ca9324.png)

You should then receive a push message and the text you included will be printed to the console.

![75b1fedbfb7e0b99.png](img/75b1fedbfb7e0b99.png)

This should give you a chance to test out sending and receiving data and manipulate notifications as a result.

Isso deve te dar uma chance para testar o envio e recebimento de dados e a manipulação de notificações resultante.

O aplicativo associado é, na verdade, apenas um servidor de nó que está usando a [biblioteca web-push](https://github.com/web-push-libs/web-push) para enviar mensagens. Vale a pena conferir as [bibl-web-push org no GitHub](https://github.com/web-push-libs/) para ver quais bibliotecas estão disponíveis para enviar mensagens push para você (isso gerencia diversos pequenos pormenores para acionar mensagens push).

## Concluído

A única coisa que está faltando é a capacidade de cancelar a inscrição para push do usuário. Para fazer isso, precisamos chamar `unsubscribe()` em uma `PushSubscription`.

De volta ao nosso arquivo `scripts/main.js`, altere o ouvinte de clique `pushButton` em `initialiseUI()` para o seguinte:

    pushButton.addEventListener('click', function() {
      pushButton.disabled = true;
      if (isSubscribed) {
        unsubscribeUser();
      } else {
        subscribeUser();
      }
    });
    

Observe que agora vamos chamar uma nova função `unsubscribeUser()`. Neste método, obteremos a inscrição atual e chamamos cancelar inscrição nela. Adicione o código a seguir ao `scripts/main.js`:

    function unsubscribeUser() {
      swRegistration.pushManager.getSubscription()
      .then(function(subscription) {
        if (subscription) {
          return subscription.unsubscribe();
        }
      })
      .catch(function(error) {
        console.log('Error unsubscribing', error);
      })
      .then(function() {
        updateSubscriptionOnServer(null);
    
        console.log('User is unsubscribed.');
        isSubscribed = false;
    
        updateBtn();
      });
    }
    

Vamos analisar esta função.

Primeiro obtemos a inscrição atual, chamando `getSubscription()`:

    swRegistration.pushManager.getSubscription()
    

Isso retorna uma promessa que resolve com uma `PushSubscription`, se existir, caso contrário, retorna `null`. Se houver uma inscrição, chamamos `unsubscribe()` o que invalida a `PushSubscription`.

    swRegistration.pushManager.getSubscription()
    .then(function(subscription) {
      if (subscription) {
        // TODO: Tell application server to delete subscription
        return subscription.unsubscribe();
      }
    })
    .catch(function(error) {
      console.log('Error unsubscribing', error);
    })
    

Chamar `unsubscribe()` retorna uma promessa, pois isso pode levar algum tempo para ser concluído, então retornamos a promessa para que o próximo `then()` na cadeia aguarde a finalização de `unsubscribe()`. Também adicionamos um gerenciador de captura para o caso da chamada `unsubscribe()` resultar em um erro. Depois disso, podemos atualizar nossa IU.

    .then(function() {
      updateSubscriptionOnServer(null);
    
      console.log('User is unsubscribed.');
      isSubscribed = false;
    
      updateBtn();
    })
    

#### Experimente

Você deve conseguir pressionar o botão *Enable Push Messaging* / *Disable Push Messaging* em seu app da Web e os registros mostrarão o usuário sendo inscrito e tendo sua inscrição cancelada.

![33dd89c437c17c97.png](img/33dd89c437c17c97.png)

## Encontrou um problema ou tem feedback? {: .hide-from-toc }

Parabéns pela conclusão deste codelab!

Este codelab mostrou como começar a usar a adição push ao seu app da Web. Se deseja saber mais sobre o que notificações de web podem fazer, [confira estes docs](/web/fundamentals/push-notifications).

Se procura implantar push em seu site, você pode estar interessado em adicionar suporte para navegadores compatíveis mais antigos/não-padrão que utilizam GCM, [saiba mais aqui](https://web-push-book.gauntface.com/chapter-06/01-non-standards-browsers/).

### Leitura adicional

* [Notificação push na Web](/web/fundamentals/push-notifications) documentação sobre **Fundamentos** da Web
* [Bibliotecas push na Web](https://github.com/web-push-libs/) - Bibliotecas Push na Web, incluindo Node.js, PHP, Java e Python.

#### Postagens do blog relevantes

* [Criptografia de carga útil de push na Web](/web/updates/2016/03/web-push-encryption)
* [Chaves de Servidor de Aplicativo e push na Web](/web/updates/2016/07/web-push-interop-wins)
* [Ações de notificação](/web/updates/2016/01/notification-actions)
* [Ícones, eventos fechados, preferências de renotificação e timestamps](/web/updates/2016/03/notifications)

## Found an issue, or have feedback? {: .hide-from-toc }

Ajude-nos a melhorar nossos codelabs reportando um [problema](https://github.com/googlechrome/push-notifications/issues) hoje. E obrigado!