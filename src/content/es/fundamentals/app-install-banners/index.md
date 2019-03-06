project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Hay dos tipos de banners de instalación de apps: los de apps web y los de apps nativas. Te dan la posibilidad de permitir que los usuarios agreguen de manera rápida y fluida tu app nativa o web a sus pantallas de inicio sin salir del navegador.

{# wf_updated_on: 2017-09-27 #} {# wf_published_on: 2014-12-16 #}

# Banners de instalación de apps web {: .page-title }

{% include "web/_shared/contributors/mattgaunt.html" %} {% include "web/_shared/contributors/paulkinlan.html" %}

Hay dos tipos de banners de instalación de apps: los de apps **web** y los de apps [**nativas**](native-app-install). Permiten que los usuarios agreguen de manera rápida y fluida tu app nativa o web a sus pantallas de inicio sin salir del navegador.

Agregar banners de instalación de apps es sencillo, y Chrome se encarga de la mayoría del trabajo pesado. Deberás incluir un archivo de manifiesto de app web en tu sitio con información detallada sobre tu app.

* On mobile, Chrome will generate a [WebAPK](/web/fundamentals/integration/webapks), creating an even more integrated experience for your users.
* Contener un [service worker](/web/fundamentals/getting-started/primers/service-workers) registrado en tu sitio.

## Eventos de banner de instalación de apps

Chrome luego usará un conjunto de criterios y heurística de frecuencia de visitas para determinar el momento en que se mostrará el banner. Continúa leyendo para obtener más información.

Note: Add to Homescreen (a veces abreviado como A2HS) es otro nombre para los Banners de instalación de apps web. Los dos términos son equivalentes.

## Banners de instalación de apps web

<figure class="attempt-right">
  <img src="images/a2hs-dialog-g.png" alt="Add to Home Screen dialog on Android">
  <figcaption>Add to Home Screen dialog on Android</figcaption>
</figure>

Chrome mostrará de manera automática el banner cuando tu app cumpla con los siguientes criterios:

1. Abre Chrome DevTools.
2. Dirígete al panel **Application**.
3. Dirígete a la pestaña **Manifest**.

<div class="clearfix"></div>

Note: Los Banners de instalación de apps web son una tecnología emergente. Los criterios para mostrar los banners de instalación de app pueden cambiar en el futuro. Consulta [¿Qué exactamente convierte a algo en una Progressive Web App?](https://infrequently.org/2016/09/what-exactly-makes-something-a-progressive-web-app/) para ver una referencia canónica (que se actualizará con el tiempo) de los últimos criterios sobre banners de instalación de apps web.

### ¿Cuáles son los criterios?

Una vez que has establecido tu manifiesto de app web, deberás validar que está definido correctamente. Tienes dos enfoques a tu disposición. Uno es manual, el otro es automatizado.

Para ejecutar el banner de instalación de app manualmente:

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
    

### Prueba del banner de instalación de apps {: #test }

The best way to notify the user your app can be installed is by adding a button or other element to your user interface. **Don't show a full page interstitial or other elements that may be annoying or distracting.**

<pre class="prettyprint">window.addEventListener('beforeinstallprompt', (e) => {
  // Prevent Chrome 67 and earlier from automatically showing the prompt
  e.preventDefault();
  // Stash the event so it can be triggered later.
  deferredPrompt = e;
  <strong>// Update UI notify the user they can add to home screen
  btnAdd.style.display = 'block';</strong>
});
</pre>

Consulta [Simular eventos para agregar a la pantalla de inicio](/web/tools/chrome-devtools/progressive-web-apps#add-to-homescreen) para obtener más ayuda.

### ¿Un usuario instaló la app?

Para realizar una prueba automática de tu banner de instalación de app, usa Lighthouse. Lighthouse es una herramienta de auditoría de app web. Puedes ejecutarlo como una extensión de Chrome o como un módulo NPM. Para probar tu app, proporciona a Lighthouse una página específica para realizar la auditoría. Lighthouse ejecuta una serie de auditorías contra la página y luego suministra un informe con los resultados de la página.

Las dos series de auditorías de Lighthouse que aparecen en la captura de pantalla representan todas las pruebas que tu página necesita pasar para mostrarse en un banner de instalación de app.

    window.addEventListener('beforeinstallprompt', function(e) {
      console.log('beforeinstallprompt Event fired');
      e.preventDefault();
      return false;
    });
    

You can only call `prompt()` on the deferred event once. If the user dismisses it, you'll need to wait until the `beforeinstallprompt` event is fired on the next page navigation.

## The mini-info bar

<figure class="attempt-right">
  <img
      class="screenshot"
      src="/web/updates/images/2018/06/a2hs-infobar-cropped.png">
  <figcaption>
    The mini-infobar
  </figcaption>
</figure>

Consulta [Auditoría de apps web con Lighthouse](/web/tools/lighthouse/) para comenzar con Lighthouse.

Chrome proporciona un mecanismo sencillo para determinar la respuesta del usuario al banner de instalación de aplicaciones e incluso cancelarlo o diferirlo hasta un momento más conveniente.

El evento `beforeinstallprompt` muestra una promesa llamada `userChoice` que se resuelve cuando el usuario actúa ante el aviso. La promesa muestra un objeto con un valor `dismissed` en el atributo `outcome` o `accepted` si el usuario agregó la página web a la pantalla de inicio.

## Feedback {: .hide-from-toc }

Esta es una buena herramienta para comprender la interacción de tus usuarios con la solicitud de instalación de la aplicación.

<div class="clearfix"></div>

## Determine if the app was successfully installed {: #appinstalled }

Chrome administra el momento de activación de la solicitud. No obstante, es probable que para algunos sitios esto no sea ideal. Puedes diferir la solicitud para un momento posterior en el uso de la aplicación o incluso cancelarla.

    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

## Detecting if your app is launched from the home screen {: #detect-mode }

### Diferir o cancelar la solicitud

Cuando Chrome solicita al usuario que instale la aplicación, puedes evitar la acción predeterminada y almacenar el evento para después. Luego, cuando la interacción del usuario con tu sitio sea positiva, puedes reactivar la solicitud llamando a `prompt()` en el evento almacenado.

This causes Chrome to show the banner and all the Promise attributes such as `userChoice` will be available to bind to so that you can understand what action the user took.

    "prefer_related_applications": true,
    "related_applications": [
      {
      "platform": "play",
      "id": "com.google.samples.apps.iosched"
      }
    ]
    

Como alternativa, puedes cancelar la solicitud evitando el valor predeterminado.

    if (window.matchMedia('(display-mode: standalone)').matches) {
      console.log('display-mode is standalone');
    }
    

### Criterios para mostrar el banner

Los banners de instalación de apps nativas son similares a los [Banners de instalación de apps web](.), pero en lugar de agregarse a la pantalla de inicio, permitirán que el usuario instale tu app nativa sin abandonar tu sitio.

    if (window.navigator.standalone === true) {
      console.log('display-mode is standalone');
    }
    

## Updating your app's icon and name

### Requisitos del manifiesto

El criterio es similar al banner de instalación de aplicación web, excepto por la necesidad de un service worker. Tu sitio debe:

### Desktop

Para integrarse a cualquier manifiesto, agrega un conjunto de `related_applications` con las plataformas de `play` (para Google Play) y la Id. de la app.

## Test your add to home screen experience {: #test }

Si solo quieres ofrecer al usuario la capacidad de instalar tu app Android y no mostrar el banner de instalación de app web, agrega `"prefer_related_applications": true`. Por ejemplo:

{# wf_devsite_translation #}

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

Dogfood: To test the install flow for Desktop Progressive Web Apps on Mac, you'll need to enable the `#enable-desktop-pwas` flag.

### Will `beforeinstallprompt` be fired?

The easiest way to test if the `beforeinstallprompt` event will be fired, is to use [Lighthouse](/web/tools/lighthouse/) to audit your app, and check the results of the [User Can Be Prompted To Install The Web App](/web/tools/lighthouse/audits/install-prompt) test.