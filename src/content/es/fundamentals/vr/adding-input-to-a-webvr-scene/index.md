project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Aprende a usar la biblioteca Ray Input para agregar entrada a tu escena de WebVR.

{# wf_updated_on: 2017-07-12 #} {# wf_published_on: 2016-12-12 #}

# Agregar entrada a una escena de WebVR {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %}

Warning: WebVR todavía es experimental y se encuentra sujeta a modificaciones.

![A ray beam showing input in a WebVR Scene](./img/ray-input.jpg)

With WebVR (and 3D in general) there can be a variety of inputs, and ideally speaking we want to not only account for all of them, but switch between them as the user’s context changes.

En WebVR (y 3D en general) puede haber varios tipos de entradas, y lo ideal sería no solo representarlas a todas, sino también poder cambiar entre ellas según cambie el contexto del usuario.

<img class="attempt-right" src="../img/touch-input.png" alt="Touch input icon" />

* **Mouse.**
* **Entrada táctil.**
* **Acelerómetro y giroscopio.**
* **Controladores sin grado de libertad** (como Cardboard). Estos son controladores que se encuentran completamente vinculados con la ventana de visualización, y con los cuales se supone que la interacción normalmente tiene origen en el centro de la ventana de visualización.
* **Controladores con 3 grados de libertad** (como el controlador Daydream). Un controlador con 3 grados ofrece información de orientación, pero no de ubicación. Normalmente, se supone que la persona sostiene este tipo de controladores en la mano izquierda o derecha, y su posición en espacios 3D es estimada.
* **Controladores con 6 grados de libertad** (como Oculus Rift o Vive). Todos los controladores con 6 grados de libertad ofrecerán tanto información de orientación como de ubicación. Normalmente, estos son los que tienen las mejores capacidades y precisión.

In the future, as WebVR matures, we may even see new input types, which means our code needs to be as future-proof as possible. Writing code to handle all input permutations, however, can get complicated and unwieldy. The [Ray Input](https://github.com/borismus/ray-input) library by Boris Smus already provides a flying start, supporting the majority of input types available today, so we will start there.

En el futuro, con la maduración de WebVR, incluso podremos ver nuevos tipos de entrada, lo que significa que nuestro código necesita estar tan preparado para el futuro como sea posible. Sin embargo, escribir el código para poder adaptarse a todas las nuevas versiones de entradas puede resultar complicado y difícil de manejar. La biblioteca [Ray Input](https://github.com/borismus/ray-input) de Boris Smus ya ofrece un buen punto de partida. Admite la mayoría de tipos de entradas que se encuentran disponibles hoy, así que comenzaremos por ahí.

## Agrega la biblioteca Ray Input a la página

Comencemos desde nuestra escena anterior y [agreguemos controladores de entrada con Ray Input](https://googlechrome.github.io/samples/web-vr/basic-input/). Si quieres ver el código final, deberías consultar el [repositorio de ejemplos de Google Chrome](https://github.com/GoogleChrome/samples/tree/gh-pages/web-vr/basic-input/).

    <!-- Must go after Three.js as it relies on its primitives -->
    <script src="third_party/ray.min.js"></script>
    

Para mayor simplicidad, podemos agregar Ray Input directamente con una etiqueta de secuencia de comandos:

## Obtén acceso a las entradas

Si usas Ray Input como parte de un sistema de compilación mayor, también puedes importarla de esa forma. El [archivo README de Ray Input contiene más información](https://github.com/borismus/ray-input/blob/master/README.md), así que deberías consultarlo.

    this._getDisplays().then(_ => {
      // Get any available inputs.
      this._getInput();
      this._addInputEventListeners();
    
      // Default the box to 'deselected'.
      this._onDeselected(this._box);
    });
    

Luego de obtener acceso a cualquier pantalla de RV, podemos solicitar acceso a todas las entradas disponibles. A partir de allí, podemos agregar receptores de eventos, y actualizaremos la escena para dejar al estado predeterminado de nuestra caja como "deselected".

    _getInput () {
      this._rayInput = new RayInput.default(
          this._camera, this._renderer.domElement);
    
      this._rayInput.setSize(this._renderer.getSize());
    }
    

Let’s take a look inside both the `_getInput` and `_addInputEventListeners` functions.

La creación de una biblioteca Ray Input supone pasarle la cámara de Three.js desde la escena y un elemento al cual pueda unir mouse, entrada táctil y cualquier otro receptor de eventos que necesite. Si no pasas un elemento como el segundo parámetro, de manera predeterminada se unirá a `window`, lo que puede impedir que partes de tu interfaz de usuario (IU) reciban eventos de entrada.

## Habilita la interactividad para entidades de escena

Otra acción necesaria es decirle qué tan grande debe ser el área con la que debe trabajar, lo que en la mayoría de los casos es el área del elemento de lienzo de WebGL.

    _addInputEventListeners () {
      // Track the box for ray inputs.
      this._rayInput.add(this._box);
    
      // Set up a bunch of event listeners.
      this._rayInput.on('rayover', this._onSelected);
      this._rayInput.on('rayout', this._onDeselected);
      this._rayInput.on('raydown', this._onSelected);
      this._rayInput.on('rayup', this._onDeselected);
    }
    

A continuación, debemos decirle a Ray Input qué rastrear y qué eventos nos interesa recibir.

    _onSelected (optMesh) {
      if (!optMesh) {
        return;
      }
    
      optMesh.material.opacity = 1;
    }
    
    _onDeselected (optMesh) {
      if (!optMesh) {
        return;
      }
    
      optMesh.material.opacity = 0.5;
    }
    

As you interact with the scene, whether by mouse, touch, or other controllers, these events will fire. In the scene we can make our box’s opacity change based on whether the user is pointing at it.

    this._box.material.transparent = true;
    

Para que esto funcione, debemos asegurarnos de decirle a Three.js que el material de la caja debería admitir la transparencia.

## Habilita las extensiones de Gamepad API

Eso debería cubrir las interacciones con mouse y táctiles. Veamos qué implica agregar un controlador con 3 grados de libertad, como el controlador Daydream.

* En Chrome 56 necesitarás habilitar el indicador Gamepad Extensions en `chrome://flags`. Si tienes un [Origin Trial](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md), Gamepad Extensions ya estará habilitado junto con las WebVR API. **Necesitarás el que el indicador esté habilitado para el desarrollo local**.

* La información de la pose para el controlador para juegos (que es como obtienes acceso a esos 3 grados de libertad) **solo se habilita luego de que el usuario presione un botón en su controlador de RV**.

Existen dos consideraciones importantes para entender cómo usar la Gamepad API en WebVR hoy:

    this._vr.display.requestPresent([{
      source: this._renderer.domElement
    }])
    .then(_ => {
      **this._showPressButtonModal();**
    })
    .catch(e => {
      console.error(`Unable to init VR: ${e}`);
    });
    

Ya que el usuario necesita interactuar antes de que podamos mostrarle un puntero en la escena, será necesario pedirle que presione un botón de su controlador. El mejor momento para hacer eso es luego de que comencemos a enviar la presentación a las gafas de realidad virtual (HMD).

![Showing a "Press Button" message to users](./img/press-a-button.jpg)

The code to do that looks something like this.

    _showPressButtonModal () {
      // Get the message texture, but disable mipmapping so it doesn't look blurry.
      const map = new THREE.TextureLoader().load('./images/press-button.jpg');
      map.generateMipmaps = false;
      map.minFilter = THREE.LinearFilter;
      map.magFilter = THREE.LinearFilter;
    
      // Create the sprite and place it into the scene.
      const material = new THREE.SpriteMaterial({
        map, color: 0xFFFFFF
      });
    
      this._modal = new THREE.Sprite(material);
      this._modal.position.z = -4;
      this._modal.scale.x = 2;
      this._modal.scale.y = 2;
      this._scene.add(this._modal);
    
      // Finally set a flag so we can pick this up in the _render function.
      this._isShowingPressButtonModal = true;
    }
    

El código para hacer eso se ve similar al siguiente.

    _render () {
      if (this._rayInput) {
        if (this._isShowingPressButtonModal &&
            this._rayInput.controller.wasGamepadPressed) {
          this._hidePressButtonModal();
        }
    
        this._rayInput.update();
    
      }
      …
    }
    

## Agrega la malla del puntero a la escena

Finalmente, en la función `_render` podemos ver interacciones y usar eso para esconder el modal. También necesitamos decirle a Ray Input cuándo actualizar, del mismo modo en que llamamos a `submitFrame()` en las HMD para que vacíe el lienzo.

    this._scene.add(this._rayInput.getMesh());
    

Además de permitir interacciones, es muy probable que queramos mostrar algo al usuario que indique hacia dónde apunta. Ray Input proporciona una malla que puedes agregar a tu escena para lograr justamente eso.

![A ray beam showing input in a WebVR Scene](./img/ray-input.jpg)

## Consideraciones finales

There are some things to keep in mind as you add input to your experiences.

* **Deberías adoptar las mejoras progresivas.** Ya que una persona podría pretender usar lo que creaste con cualquiera de las nuevas versiones de entradas de la lista, deberías esforzarte para planear tu IU de forma que pueda adaptarse correctamente entre distintos tipos. Donde sea posible, prueba varios dispositivos y entradas para maximizar lo que puedes abarcar.

* **Las entradas pueden no ser perfectamente precisas.** El controlador Daydream, en particular, tiene 3 niveles de libertad, pero opera en un espacio que admite 6. Eso significa que aunque su orientación es correcta, su posición en un espacio 3D debe suponerse. Para compensar esto, tal vez sea conveniente que agrandes los objetivos de la entrada y te asegures de tener los espacios adecuados para evitar confusiones.

Debes tener en cuenta algunos aspectos cuando agregas entradas a tus experiencias.

Es vital agregar una entrada a tu escena para lograr una experiencia envolvente, y con [Ray Input](https://github.com/borismus/ray-input) es mucho más fácil comenzar.

## Feedback {: #feedback }

¡Cuéntanos cómo lo haces!