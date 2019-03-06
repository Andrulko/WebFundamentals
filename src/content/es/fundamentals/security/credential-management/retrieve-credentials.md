project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-11-08 #}

# Recuperar credenciales {: .page-title }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/megginkearney.html" %}

Para hacer que el usuario acceda, recupera las credenciales del gestor de contraseñas del navegador y úsalas para iniciar su sesión.

## Obtener una credencial

Para recuperar la credencial de un usuario, usa `navigator.credentials.get()`, que muestra una promesa que se resuelve con un objeto de la credencial como un argumento. El objeto de la credencial obtenido puede ser [`PasswordCredential`](#authenticate_with_a_server) o [`FederatedCredential`](#authenticate_with_an_identity_provider). Si no existe información sobre la credencial, se muestra `null`.

Para iniciar sesión del usuario automáticamente, solicita un objeto de credencial con `unmediated: true`, tan pronto como accede a tu sitio web, por ejemplo:

1. Get credential information.
2. Authenticate the user.
3. Update the UI or proceed to the personalized page.

### Parámetros de `navigator.credentials.get` {: .hide-from-toc }

Esta solicitud se resuelve de inmediato con un objeto de la credencial y no mostrará un selector de cuentas. Cuando el navegador obtiene información de la credencial, aparece una notificación:

Si un usuario solicita mediación o tiene cuentas múltiples, usa el selector de cuentas para permitir que el usuario inicie sesión, omitiendo el formulario normal de inicio de sesión.

* El usuario no ha aceptado la función de inicio de sesión automática (una vez por instancia de navegador).
* El usuario no tiene credenciales o tiene más de dos objetos de la credencial almacenados en el origen.
* El usuario ha solicitado pedir una mediación del usuario con el origen.

Generalmente, se invoca al selector de cuentas cuando el usuario presiona el botón "Sign-In". El usuario puede seleccionar una cuenta para iniciar sesión, por ejemplo:

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
    

Para habilitar el selector de cuentas, fija la propiedad `unmediated` en `false`:

Una vez que el usuario haya seleccionado la cuenta que quiere usar, la promesa se resuelve con un `PasswordCredential` o un `FederatedCredential` según su selección. Luego, [determina el tipo de credencial](#determine-credential-type) y autentica al usuario con la credencial proporcionada.

### Obtén una credencial automáticamente

Si el usuario cancela el selector de cuentas o si no hay credenciales almacenadas, la promesa se resuelve con un valor `undefined`. En ese caso, recurre a la experiencia de formulario de acceso.

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
    

Cuando se resuelve el `navigator.credentials.get()`, se mostrará `undefined` o un objeto Credential. Para determinar si es un `PasswordCredential` o un `FederatedCredential`, simplemente mira a la propiedad del objeto `.type`, que será `password` o `federated`.

### Obtén una credencial a través del selector de cuentas

Si el `.type` es `federated`, la propiedad `.provider` es una string que representa al proveedor de identidad.

    }).then(profile => {
     if (profile) {
       updateUI(profile);
     }
    

### Autenticar con un nombre de usuario y contraseña

Por ejemplo:

<div>
  <figure>
    <img src="imgs/auto-sign-in.png" alt="Blue toast showing user is signing in.">
  </figure>
</div>

En el caso de un valor `undefined`, continua con el usuario en el estado de cierre de sesión.

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
    

### Autenticar con un proveedor de identidad

    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="id"
    
    chromedemojp@gmail.com
    ------WebKitFormBoundaryOkstjzGAv8zab97W
    Content-Disposition: form-data; name="password"
    
    testtest
    ------WebKitFormBoundaryOkstjzGAv8zab97W--
    

## Determinar el tipo de credencial {: #determine-credential-type }

Se pasa un valor `undefined` cuando:

<div>
  <figure>
    <img src="imgs/account-chooser.png" alt="Google account chooser showing multiple accounts.">
  </figure>
</div>

Para autenticar al usuario con tu servidor, PUBLICA el `PasswordCredential` proporcionado en el servidor usando `fetch()`.

1. Get credential information and show account chooser.
2. [Authenticate the user](#authenticate_user).
3. [Update UI or proceed to a personalized page](#update_ui).

### Get credential information and show account chooser

Una vez PUBLICADO, `fetch` automáticamente convierte el objeto `PasswordCredential` en un objeto `FormData` codificado como `multipart/form-data`:

Note: No puedes usar `XMLHttpRequest` para PUBLICAR el `PasswordCredential` en tu servidor.

Un objeto obtenido `PasswordCredential` incluye los siguientes parámetros:

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
    

En algunos casos, tal vez sea necesario agregar datos adicionales a la PUBLICACIÓN de autenticación.

### Don't forget to fallback to sign-in form

Cambia las claves de parámetros asignando una string a `.idName` o `.passwordName`.

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
    

## Autenticar al usuario

También puedes agregar parámetros adicionales como un token de falsificación de solicitudes entre sitios (CSRF) asignando `.additionalData` al `FormData` y puedes anexarle pares clave-valor.

Una vez que obtienes el objeto de la credencial:

1. Authenticate the user with a third-party identity.
2. Store the identity information.
3. [Update UI or proceed to a personalized page](#update_ui) (same as auto sign-in).

### Authenticate user with third-party identity

Puedes hacer algo similar asignando un objeto `URLSearchParams` en lugar de un `FormData` a `.additionalData`. En este caso, el objeto completo de la credencial se codifica usando `application/x-www-form-urlencoded`.

Para autenticar al usuario con un proveedor de identidad, simplemente usa el flujo de autenticación específico con el `FederatedCredential`.

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
    

Por ejemplo, si el proveedor es Google, usa la [biblioteca JavaScript de Google Sign-In](/identity/sign-in/web/):

Google Sign-In genera un token de ID como una prueba de autenticación que envías al servidor para crear una sesión.

* [Google Sign-In](/identity/sign-in/web/)
* [Facebook Login](https://developers.facebook.com/docs/facebook-login)
* [Twitter Sign-in](https://dev.twitter.com/web/sign-in/implementing)
* [GitHub OAuth](https://developer.github.com/v3/oauth/)

### Store identity information

Para proveedores de identidad adicionales, consulta la documentación respectiva:

Cuando un usuario cierra sesión en tu sitio web, es tu responsabilidad asegurarte de que el usuario no inicie sesión automáticamente en su próxima visita. Para desactivar el inicio de sesión automático, llama a [`navigator.credentials.requireUserMediation()`](https://developer.mozilla.org/en-US/docs/Web/API/CredentialsContainer/requireUserMediation):

Luego, si se llama al `navigator.credentials.get()` con `unmediated: true`, mostrará `undefined` y el usuario no iniciará sesión. Esto solo se recuerda para la instancia actual del navegador para este origen.

Para reanudar el inicio de sesión automático, un usuario puede elegir iniciar sesión de modo intencional, al elegir la cuenta con la que desea iniciar sesión, desde el selector de cuentas. Luego, se vuelve a iniciar la sesión del usuario, hasta que cierre sesión de modo explícito.

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
    

## Cerrar sesión {: #sign-out }

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