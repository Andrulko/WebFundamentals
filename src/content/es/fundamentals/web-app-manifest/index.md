project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: El manifiesto de las apps web es un archivo JSON que te permite controlar cómo tu sitio o app web se muestra al usuario en áreas donde normalmente ven apps nativas (por ejemplo, la pantalla de inicio de un dispositivo), además de dirigir lo que el usuario puede iniciar y definir su apariencia al iniciarse.

{# wf_updated_on: 2017-10-06 #} {# wf_published_on: 2016-02-11 #}

# El manifiesto de las apps web {: .page-title }

{% include "web/_shared/contributors/mattgaunt.html" %} {% include "web/_shared/contributors/paulkinlan.html" %}

El [manifiesto de las apps web](https://developer.mozilla.org/en-US/docs/Web/Manifest) es un archivo JSON simple que permite que tú, el desarrollador, puedas controlar cómo se muestra tu app al usuario en áreas donde normalmente ven apps nativas (por ejemplo, la pantalla de inicio de un dispositivo móvil), además de indicar lo que el usuario puede iniciar y definir su apariencia al iniciarse.

Los manifiestos de las apps web permiten guardar un marcador de sitio en la pantalla de inicio de un dispositivo. Cuando un sitio se inicia de esta manera:

## Crea el manifiesto

Hace todo esto a través del simple mecanismo de metadatos en un archivo de texto. Eso representa el manifiesto de aplicación web.

    {
      "short_name": "AirHorner",
      "name": "Kinlan's AirHorner of Infamy",
      "icons": [
        {
          "src": "launcher-icon-1x.png",
          "type": "image/png",
          "sizes": "48x48"
        },
        {
          "src": "launcher-icon-2x.png",
          "type": "image/png",
          "sizes": "96x96"
        },
        {
          "src": "launcher-icon-4x.png",
          "type": "image/png",
          "sizes": "192x192"
        }
      ],
      "start_url": "index.html?launcher=true"
    }
    

Note: Aunque los manifiestos de apps web pueden usarse en cualquier sitio, son necesarios para las [apps web progresivas](/web/progressive-web-apps/).

## Informa al navegador acerca del manifiesto

Antes de adentrarnos en detalles sobre el manifiesto de la app web, creemos un manifiesto básico y vinculemos una página web al mismo.

    <link rel="manifest" href="/manifest.json">
    

## Configura una URL de inicio

### TL;DR {: .hide-from-toc }

Puedes dar al manifiesto el nombre que desees. La mayoría de la gente usa `manifest.json`. A continuación, te mostramos un ejemplo:

    "start_url": "/?utm_source=homescreen"
    

### Configura una imagen y un título

Asegúrate de incluir lo siguiente:

Una vez que el manifiesto de tu app esté creado y se encuentre en tu sitio, agrega una etiqueta `link` a todas las páginas que compongan tu app web, como se muestra a continuación:

    "icons": [{
        "src": "images/touch/icon-128x128.png",
        "type": "image/png",
        "sizes": "128x128"
      }, {
        "src": "images/touch/apple-touch-icon.png",
        "type": "image/png",
        "sizes": "152x152"
      }, {
        "src": "images/touch/ms-touch-icon-144x144-precomposed.png",
        "type": "image/png",
        "sizes": "144x144"
      }, {
        "src": "images/touch/chrome-touch-icon-192x192.png",
        "type": "image/png",
        "sizes": "192x192"
      }],
    

Si no proporcionas una `start_url`, se usa la página actual, y es muy poco probable que esto sea lo que los usuarios quieran. Pero esa no es la razón para incluirla. Ya que ahora puedes definir la forma en la que se iniciará tu app, agrega un parámetro de cadena de consulta a la `start_url` para indicar cómo se inició.

### Configura un color de fondo

Puedes elegir lo que quieras; el valor que usamos tiene la ventaja de ser relevante para Google Analytics.

Puedes definir el conjunto de íconos que usará el navegador cuando un usuario agregue tu sitio a su pantalla de inicio. Puedes definirlos con un tipo y tamaño, de la siguiente manera:

    "background_color": "#2196F3",
    

Note: Cuando guardas un ícono en la pantalla de inicio, Chrome busca primero los íconos que concuerden con la densidad de la pantalla y que tengan el tamaño para una densidad de pantalla de 48 dp. Si no encuentra ninguno, busca el ícono que más cerca esté de concordar con las características del dispositivo. Si, por alguna razón, quieres que se encuentre específicamente un ícono en una densidad de píxeles en particular, puedes usar el miembro `density` opcional, que indica un número. Cuando no declaras la densidad, el valor predeterminado es 1.0. Esto significa: "usa este ícono para densidades de pantalla de 1.0 y superiores", que es lo que normalmente quieres.

### Configura un color de tema

Cuando inicias tu app web desde la pantalla de inicio suceden varias cosas bajo la superficie:

### Personaliza el tipo de pantalla

Mientras esto sucede, la pantalla se pone en blanco y parece detenida. Esto se nota sobre todo si cargas tu página web desde la red, ya que así las páginas tardan más de uno o dos segundos en visualizarse en la página principal.

    "display": "standalone"
    

<table id="display-params" class="responsive">
  <tbody>
    <tr>
      <th colspan=2>Parameters</th>
    </tr>
    <tr>
      <td><code>value</code></td><td><code>Description</code></td>
    </tr>
    <tr id="display-fullscreen">
      <td><code>fullscreen</code></td>
      <td>
        Opens the web application without any browser UI and takes
        up the entirety of the available display area.
      </td>
    </tr>
    <tr>
      <td><code>standalone</code></td>
      <td>
        Opens the web app to look and feel like a standalone native
        app. The app runs in its own window, separate from the browser, and
        hides standard browser UI elements like the URL bar, etc.</td>
    </tr>
    <tr>
      <td><code>minimal-ui</code></td>
      <td>
        <b>Not supported by Chrome</b><br>
        This mode is similar to <code>fullscreen</code>, but provides the
        user with some means to access a minimal set of UI elements for
        controlling navigation (i.e., back, forward, reload, etc).
      </td>
    </tr>
    <tr>
      <td><code>browser</code></td>
      <td>A standard browser experience.</td>
    </tr>
  </tbody>
</table>

Para ofrecer una experiencia de usuario mejor, puedes reemplazar la pantalla blanca con un título, color e imágenes.

### Especifica la orientación inicial de la página

Si sigues los pasos desde el principio, ya tienes una imagen y un título. Chrome infiere la imagen y el título a partir miembros específicos del manifiesto. Lo que importa aquí es conocer las características específicas.

    "display": "browser"
    

### `scope` {: #scope }

Se toma una imagen a partir de la matriz de `icons` para la pantalla de presentación. Chrome elige la imagen de densidad más cercana a 128 dp para el dispositivo. El título se obtiene simplemente a partir del miembro `name`.

    "orientation": "landscape"
    

Especifica el color de fondo usando la propiedad con el nombre apropiado `background_color` . Chrome usa este color desde el momento en que la app web se inicia, y el color permanece en la pantalla hasta la primera aparición de la app web.

Para configurar el color de fondo, indica lo siguiente en tu manifiesto:

* Tiene un ícono y un nombre únicos para que los usuarios puedan diferenciarlo de otros sitios.
* Muestra algo al usuario mientras se descargan los recursos o se restauran a partir del caché.
* Proporciona características predeterminadas de visualización al navegador para evitar una transición demasiado brusca cuando los recursos del sitio se encuentren disponibles.
* The `start_url` is relative to the path defined in the `scope` attribute.
* A `start_url` starting with `/` will always be the root of the origin.

### `theme_color` {: #theme-color }

Ahora no se muestra una pantalla blanca cuando se inicia tu sitio desde la pantalla de inicio.

    "theme_color": "#2196F3"
    

Un buen valor que se sugiere para esta propiedad es el color de fondo de la página de carga. Usar los mismos colores que la página de carga permite una transición más suave desde la pantalla de presentación a la página principal.

Especifica un color de tema por medio de la propiedad `theme_color`. Esta propiedad fija el color de la barra de herramientas. Para llevar esto a cabo sugerimos duplicar un color existente, específicamente el `theme-color` `<meta>`.

## Personaliza los íconos

<figure class="attempt-right">
  <img src="images/background-color.gif" alt="Ícono de adición a la pantalla principal">
  <figcaption>Ícono de adición a la pantalla principal</figcaption>
</figure>

Usa el manifiesto de aplicación web para controlar el tipo de pantalla y la orientación de página.

Puedes hacer que tu app web oculte la IU del navegador si configuras el tipo de `display` en `standalone`:

* `name`
* `background_color`
* `icons`

Si consideras que los usuarios preferirían ver tu página como un sitio normal en un navegador, puedes configurar el tipo de `display` en `browser`:

### Icons used for the splash screen {: #splash-screen-icons }

Puedes imponer una orientación específica, lo cual beneficia a las apps que solo funcionan en una orientación; por ejemplo, juegos Usa esto de manera selectiva. Los usuarios prefieren seleccionar la orientación.

Chrome introdujo el concepto de un color de tema para tu sitio en 2014. El color de tema es una sugerencia de tu página web que indica al navegador el color con que deben matizarse los [elementos de IU como la barra de direcciones](/web/fundamentals/design-and-ux/browser-customization/).

<div class="clearfix"></div>

## Agrega una pantalla de presentación

Sin un manifiesto, debes definir el color de tema en cada páginas, y si tu sitio es grande o heredado, no será posible realizar muchos cambios en él.

<div class="clearfix"></div>

## Configura el estilo para iniciar

<figure class="attempt-right">
  <img src="images/devtools-manifest.png" alt="color de fondo">
  <figcaption>Color de fondo para la pantalla de presentación</figcaption>
</figure>

Agrega un atributo `theme_color` a tu manifiesto. Cuando se inicie el sitio desde la pantalla de inicio, cada página del dominio recibirá el color de tema de modo automático.

Si quieres verificar de forma manual que el manifiesto de tu app web esté configurado correctamente, usa la pestaña **Manifest** en el panel **Application** de Chrome DevTools.

If you want an automated approach towards validating your web app manifest, check out [Lighthouse](/web/tools/lighthouse/). Lighthouse is a web app auditing tool. It's built into the Audits tab of Chrome DevTools, or can be run as an NPM module. You provide Lighthouse with a URL, it runs a suite of audits against that page, and then displays the results in a report.

## Proporciona un color de tema para todo el sitio

* Un `short_name` para usar como el texto de la pantalla de inicio de los usuarios.
* Un `name` para usar en el banner de instalación de apps web.
* If you want feature descriptions from the engineers who created web app manifests, you can read the [W3C Web App Manifest Spec](http://www.w3.org/TR/appmanifest/).

## Prueba tu manifiesto {: #test }

Esta pestaña proporciona una versión en lenguaje natural de muchas de las propiedades de tu manifiesto. Consulta [Manifiesto de app web](/web/tools/chrome-devtools/progressive-web-apps#manifest) en la documentación de Chrome DevTools para obtener más información sobre esta pestaña. También puedes simular los eventos Add to Homescreen (agregar a la pantalla de inicio) desde aquí. Consulta [Prueba del banner de instalación de apps](/web/fundamentals/app-install-banners#test) para obtener más información sobre este tema.