project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml

{# wf_updated_on: 2016-11-08 #} {# wf_published_on: 2016-11-08 #}

# Obter credenciais {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/megginkearney.html" %}

Para fazer o usuário acessar, recupere as credenciais do gerenciador de senhas do navegador e use-as.

## Obter uma credencial

Para recuperar a credencial de um usuário use `navigator.credentials.get()`, que retorna uma promessa processada com um objeto "credential" como argumento. O objeto "credential" obtido pode ser [`PasswordCredential`](#authenticate_with_a_server) ou [`FederatedCredential`](#authenticate_with_an_identity_provider). Se não houver informações de credencial, `null` é retornado.

Para fazer o usuário efetuar login automaticamente, solicite um objeto "credential" com `unmediated: true` assim que ele chegar ao seu site. Por exemplo:

1. Get credential information.
2. Authenticate the user.
3. Update the UI or proceed to the personalized page.

### `navigator.credentials.get` Parâmetros {: .hide-from-toc }

Essa solicitação é processada imediatamente com um objeto "credential" e não exibirá um seletor de conta. Quando o navegador obtém as informações de credencial, uma notificação aparece:

Se um usuário solicitar mediação ou tiver diversas contas, use o seletor de contas para deixar o usuário fazer login, ignorando o formulário de login comum.

* O usuário não autoriza o recurso de login automático (um por instância do navegador).
* O usuário não tem credenciais ou mais de dois objetos "credential" armazenados na origem.
* O usuário solicita mediação à origem.

O seletor de conta normalmente é invocado quando o usuário toca no botão "Acessar" (ou "Fazer login"). O usuário pode selecionar uma conta para acessar, por exemplo:

    navigator.credentials.get({
      password: true,
      unmediated: false,
      federated: {
        providers: [
          'https://account.google.com',
          'https://www.facebook.com'
        ]
      }
    }).then(function(cred) {
      if (cred) {
        // Use provided credential to sign user in  
      }
    });
    

Para ativar o seletor de conta, defina a propriedade `unmediated` como `false`:

Quando o usuário selecionar a conta que quer usar, a promessa é processada com um `PasswordCredential` ou `FederatedCredential`, dependendo da seleção. Em seguida, [determine o tipo de credencial](#determine-credential-type) e autentique o usuário com a credencial fornecida.

### Obtenha uma credencial automaticamente

Se o usuário cancelar o seletor de conta ou não houver credenciais armazenadas, a promessa é processada com um valor `undefined`. Nesse caso, volte à experiência do formulário de acesso.

    }).then(c => {
     if (c) {
       switch (c.type) {
         case 'password':
           return sendRequest(c);
           break;
         case 'federated':
           return gSignIn(c);
           break;
       }
     } else {
       return Promise.resolve();
     }
    

Quando `navigator.credentials.get()` for processado, retornará `undefined` ou um objeto "Credential". Para determinar se ele é uma `PasswordCredential` ou uma `FederatedCredential`, basta dar uma olhada na propriedade `.type` do objeto, que será `password` ou `federated`.

### Obtenha uma credencial pelo seletor de conta

SE o `.type` for `federated`, a propriedade `.provider` será uma string que representa o provedor de identidade.

    }).then(profile => {
     if (profile) {
       updateUI(profile);
     }
    

### Autentique com um nome de usuário e senha

Por exemplo:

<div>
  <figure>
    <img src="imgs/auto-sign-in.png" alt="Blue toast showing user is signing in.">
  </figure>
</div>

No caso de um valor `undefined`, continue com o usuário em estado de logoff.

        if (cred) {
      switch (cred.type) {
        case 'password':
          // authenticate with a server
          break;
        case 'federated':
          switch (cred.provider) {
            case 'https://accounts.google.com':
              // run google identity authentication flow
              break;
            case 'https://www.facebook.com':
              // run facebook identity authentication flow
              break;
          }
          break;
      }
    } else {
      // auto sign-in not possible
    }
    

### Autentique com um provedor de identidade

    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="id"
    
    chromedemojp@gmail.com
    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="password"
    
    testtest
    ------WebKitFormBoundaryOkstjzGAv8zab97W--
    

## Determine o tipo de credencial {: #determine-credential-type }

Um valor `undefined` é passado quando:

<div>
  <figure>
    <img src="imgs/account-chooser.png" alt="Google account chooser showing multiple accounts.">
  </figure>
</div>

Para autenticar o usuário no seu servidor, use POST para publicar a `PasswordCredential` fornecida no servidor usando `fetch()`.

1. Get credential information and show account chooser.
2. [Authenticate the user](#authenticate_user).
3. [Update UI or proceed to a personalized page](#update_ui).

### Get credential information and show account chooser

Quando publicada com POST, `fetch` se converterá automaticamente no objeto `PasswordCredential` para um objeto `FormData` codificado como `multipart/form-data`:

Observação: Não é possível usar `XMLHttpRequest` para publicar a `PasswordCredential` no servidor usando POST.

Um objeto `PasswordCredential` obtido contém os seguintes parâmetros:

    if (cred) {
      if (cred.type == 'password') {
        // Use `email` instead of `id` for the id
        cred.idName = 'email';
    
        // Append CSRF Token
        var csrf_token = document.querySelector('#csrf_token').value;
        var form = new FormData();
        form.append('csrf_token', csrf_token);
    
        // Append additional credential data to `.additionalData`
        cred.additionalData = form;
    
        // `POST` the credential object.
        // id, password and the additional data will be encoded and
        // sent to the url as the HTTP body.
        fetch(url, {           // Make sure the URL is HTTPS
          method: 'POST',      // Use POST
          credentials: cred    // Add the password credential object
        }).then(function() {
          // continuation
        });
      }
    }
    

Em alguns casos, pode ser necessário adicionais mais dados ao POST de autenticação.

### Don't forget to fallback to sign-in form

Altere as chaves de parâmetro atribuindo uma string a `.idName` ou `.passwordName`.

* No credentials are stored.
* The user dismissed the account chooser without selecting an account.
* The API is not available.

<div class="clearfix"></div>

    // Instantiate an auth object
    var auth2 = gapi.auth2.getAuthInstance();
    
    // Is this user already signed in?
    if (auth2.isSignedIn.get()) {
      var googleUser = auth2.currentUser.get();
    
      // Same user as in the credential object?
      if (googleUser.getBasicProfile().getEmail() === id) {
        // Continue with the signed-in user.
        return Promise.resolve(googleUser);
      }
    }
    
    // Otherwise, run a new authentication flow.
    return auth2.signIn({
      login_hint: id || ''
    });
    

### Full code example

    // After a user signing out...
    navigator.credentials.requireUserMediation();
    

## Autenticar o usuário

Você também pode adicionar parâmetros extras, como um token contra falsificação de solicitação entre sites (CSRF, na sigla em inglês) atribuindo `.additionalData` a `FormData` e anexando valores-chave a ele.

Quando obtiver o objeto "credential":

1. Authenticate the user with a third-party identity.
2. Store the identity information.
3. [Update UI or proceed to a personalized page](#update_ui) (same as auto sign-in).

### Authenticate user with third-party identity

Você pode fazer algo similar atribuindo um objeto `URLSearchParams` em vez de um `FormData` a `.additionalData`. Nesse caso, todo o objeto "credential" será codificado usando `application/x-www-form-urlencoded`.

Para autenticar o usuário com um provedor de identidade, basta usar o fluxo de autenticação específico com a `FederatedCredential`.

    navigator.credentials.get({
      password: true,
      mediation: 'optional',
      federated: {
        providers: [
          'https://account.google.com'
        ]
      }
    }).then(function(cred) {
      if (cred) {
    
        // Instantiate an auth object
        var auth2 = gapi.auth2.getAuthInstance();
    
        // Is this user already signed in?
        if (auth2.isSignedIn.get()) {
          var googleUser = auth2.currentUser.get();
    
          // Same user as in the credential object?
          if (googleUser.getBasicProfile().getEmail() === cred.id) {
            // Continue with the signed-in user.
            return Promise.resolve(googleUser);
          }
        }
    
        // Otherwise, run a new authentication flow.
        return auth2.signIn({
          login_hint: id || ''
        });
    
      }
    });
    

Por exemplo, se o provedor for Google, use a [biblioteca JavaScript do Google Sign-In](/identity/sign-in/web/):

O Google Sign-In gera um id token como prova da autenticação, que você envia ao servidor para criar uma sessão.

* [Google Sign-In](/identity/sign-in/web/)
* [Facebook Login](https://developers.facebook.com/docs/facebook-login)
* [Twitter Sign-in](https://dev.twitter.com/web/sign-in/implementing)
* [GitHub OAuth](https://developer.github.com/v3/oauth/)

### Store identity information

Para ver outros provedores de identidade, consulte a documentação relacionada:

Quando um usuário faz logout no seu site, é sua responsabilidade garantir que o usuário não faça login automaticamente quando acessar o seu site novamente. Para desativar o login automático, chame [`navigator.credentials.requireUserMediation()`](https://developer.mozilla.org/en-US/docs/Web/API/CredentialsContainer/requireUserMediation):

Em seguida, se `navigator.credentials.get()` for chamado com `unmediated: true`, retornará `undefined` e o usuário não fará login. Isso só é lembrado pela instância atual do navegador desta origem.

Para reativar o login automático, o usuário pode escolher fazer login intencionalmente selecionando a conta que deseja acessar no seletor de conta. Depois disso, o usuário passa a sempre fazer login automaticamente, até fazer logout explicitamente.

    // Create credential object synchronously.
    var cred = new FederatedCredential({
      id:       id,                           // id in IdP
      provider: 'https://account.google.com', // A string representing IdP
      name:     name,                         // name in IdP
      iconURL:  iconUrl                       // Profile image url
    });
    

{# wf_devsite_translation #}

    // Create credential object asynchronously.
    var cred = await navigator.credentials.create({
      federated: {
        id:       id,
        provider: 'https://accounts.google.com',
        name:     name,
        iconURL:  iconUrl
      }
    });
    

Then store the credential object:

    // Store it
    navigator.credentials.store(cred)
    .then(function() {
      // continuation
    });
    

## Fazer logout {: #sign-out }

Sign out your users when the sign-out button is tapped. First terminate the session, then turn off auto sign-in for future visits. (How you terminate your sessions is totally up to you.)

### Turn off auto sign-in for future visits

Call `navigator.credentials.preventSilentAccess()`:

    signoutUser();
    if (navigator.credentials && navigator.credentials.preventSilentAccess) {
      navigator.credentials.preventSilentAccess();
    }
    

This will ensure the auto sign-in won’t happen until next time the user enables auto sign-in. To resume auto sign-in, a user can choose to intentionally sign-in by choosing the account they wish to sign in with, from the account chooser. Then the user is always signed back in until they explicitly sign out.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}