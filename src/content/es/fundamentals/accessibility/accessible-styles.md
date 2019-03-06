project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Uso de estilo adecuado para mejorar la accesibilidad

{# wf_updated_on: 2018-05-23 #} {# wf_published_on: 2016-10-04 #}

# Estilos accesibles {: .page-title }

{% include "web/_shared/contributors/megginkearney.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/robdodson.html" %}

Hemos explorado dos de los pilares cruciales de la accesibilidad: el foco y la semántica. Ahora, analicemos un tercero: el estilo. Es un tema amplio que podemos tratar en tres secciones.

- Asegurarnos que los elementos tienen el estilo adecuado para respaldar nuestros esfuerzos de accesibilidad, al agregar estilos para lograr foco y varios estados de ARIA.
- Crear un estilo para nuestras IU para que las mismas sean flexibles, y se les pueda hacer zoom o se puedan escalar para los usuarios con dificultad para leer texto con letra pequeña.
- Seleccionar el contraste y los colores adecuados para evitar transmitir información solo con color.

## Estilo del foco

Generalmente, siempre que nos enfocamos en un elemento, dependemos del anillo de enfoque incluido en el navegador (la propiedad CSS `outline`) para darle estilo al elemento. El anillo de enfoque es útil porque, sin él, es imposible que un usuario que usa el teclado distinga qué elemento tiene el foco. La [lista de comprobación de WebAIM](http://webaim.org/standards/wcag/checklist){: .external } destaca esto, solicitando que "Sea visualmente notable qué element de la página tiene el foco actual del teclado (es decir, mientras navegas por la página presionan tab, puedes ver dónde estás)".

![elementos de formulario con anillo de enfoque](imgs/focus-ring.png)

Sin embargo, a veces el anillo de enfoque puede lucir distorsionado o puede no adaptarse al diseño de tu página. Algunos programadores quitan este estilo por completo configurando el `outline` del elemento en `0` o `none`. Pero sin indicador de foco, ¿cómo puede un usuario de teclado saber con qué artículo está interactuando?

Warning: Nunca configures el contorno en 0 ni none sin brindar una alternativa de foco.

Puede ser que conozcas cómo agregar estados de desplazamiento a tus controles con la *pseudoclase* `:hover` del CSS. Por ejemplo, puedes usar `:hover` en un elemento vínculo para modificarle el color o el fondo cuando tiene el mouse encima. Como con `:hover`, puedes usar la pseudoclase `:focus` para seleccionar como objetivo un elemento cuando tiene foco.

    /* At a minimum you can add a focus style that matches your hover style */
    :hover, :focus {
      background: #c0ffee;
    }
    

Una solución alternativa al problema de quitar el anillo de enfoque es darle a tu elemento los mismos estilo de desplazamiento y foco, lo cual resuelve el problema de "¿Dónde está el foco?" de los usuarios de teclado. Como siempre, mejorar la experiencia de accesibilidad mejora la experiencia de todos.

### Modalidad de entrada

![un botón HTML nativo con anillo de enfoque](imgs/sign-up.png){: .attempt-right }

Para elementos nativos como `button`, los navegadores pueden detectar si la interacción de usuario ocurrió mediante mouse o teclado, y suele solo mostrar el anillo de enfoque para interacción de teclado. Por ejemplo, cuando haces clic en un `button` nativo con el mouse, no hay anillo de enfoque, pero, cuando llegas a él presionando tab con el teclado, aparece el anillo de enfoque.

La lógica es que es menos probable que los usuarios de mouse necesiten el anillo de enfoque porque saben en qué elemento hicieron clic. Lamentablemente, actualmente no existe un sola solución para varios navegadores que responda con el mismo comportamiento. Como resultado, si le das a cualquier elemento un estilo `:focus`, ese estilo aparecerá cuando el usuario haga clic en el elemento o haga foco en el mismo con el teclado. Intenta hacer clic en este botón falso y nota que el estilo `:focus` siempre está aplicado.

    <style>
      fake-button {
        display: inline-block;
        padding: 10px;
        border: 1px solid black;
        cursor: pointer;
        user-select: none;
      }
    
      fake-button:focus {
        outline: none;
        background: pink;
      }
    </style>
    <fake-button tabindex="0">¡Haz clic aquí!</fake-button>
    

{% framebox height="80px" %}
<style>fake-button {display: inline-block;padding: 10px;border: 1px solid black;cursor: pointer;user-select: none;}fake-button:focus {outline: none;background: pink;}</style>
<fake-button tabindex="0">Click Me!</fake-button> {% endframebox %}

This can be a bit annoying, and often times developer will resort to using JavaScript with custom controls to help differentiate between mouse and keyboard focus.

Esto puede resultar molesto y a menudo el programador recurre al uso de JavaScript con controles personalizados para ayudar a diferenciar entre foco de mouse y teclado.

En Firefox, la pseudoclase `:-moz-focusring` de CSS te permite escribir un estilo de foco que solo se aplique si ele elemento recibe foco mediante el teclado, una característica útil. A pesar de que la pseudoclase actualmente solo funciona con Firefox, [se está trabajando para convertirla en un estándar](https://github.com/wicg/modality){: .external }.

## Estilo de estados con ARIA

También existe [este excelente artículo de Alice Boxhall y Brian Kardell](https://www.oreilly.com/ideas/proposing-css-input-modality){: .external } que explora el tema de modalidad y contiene un prototipo de código para diferenciar entre entrada de mouse y de teclado. Puedes usar su solución hoy e incluir la pseudoclase de anillo de enfoque más adelante, cuando tenga un soporte más difundido.

Cuando creas componentes, es una práctica común reflejar su estado y también su apariencia, usando clases de CSS controladas por JavaScript.

Por ejemplo, piensa en un botón de alternancia que entra en estado visual "presionado" cuando se le hace clic y mantiene ese estado hasta que se le vuelve a hacer clic. Para darle estilo al estado, tu JavaScript puede agregarle una clase `pressed` al botón. Y, ya que quieres una buena semántica en todos tus controles, también configurarías el estado `aria-pressed` del botón como `true`.

    .toggle.pressed { ... }
    

Una técnica útil para emplear aquí es quitar la clase completa y usar los atributos de ARIA para darle estilo al elemento. Ahora puedes actualizar el selector de CSS para el estado presionado del botón de este

    .toggle[aria-pressed="true"] { ... }
    

a este.

## Diseño adaptable multidispositivo

Esto crea una relación lógica y semántica entre el estado ARIA y la apariencia del elemento, y también reduce el código extra.

Sabemos que es buena idea diseñar en forma receptiva para brindar la mejor experiencia multidispositivo, pero el diseño adaptable también proporciona un beneficio de accesibilidad.

![Udacity.com at 100% magnification](imgs/udacity.jpg)

A low-vision user who has difficulty reading small print might zoom in the page, perhaps as much as 400%. Because the site is designed responsively, the UI will rearrange itself for the "smaller viewport" (actually for the larger page), which is great for desktop users who require screen magnification and for mobile screen reader users as well. It's a win-win. Here's the same page magnified to 400%:

![Udacity.com at 400% magnification](imgs/udacity-zoomed.jpg)

In fact, just by designing responsively, we're meeting [rule 1.4.4 of the WebAIM checklist](http://webaim.org/standards/wcag/checklist#sc1.4.4){: .external }, which states that a page "...should be readable and functional when the text size is doubled."

De hecho, mediante una designación responsable, estamos cumpliendo la [regla 1.4.4 de la lista de comprobación de WebAIM](http://webaim.org/standards/wcag/checklist#sc1.4.4){: .external }, que indica que una página "...debería ser legible y funcional cuando se duplica el tamaño del texto".

- Primero, asegúrate de usar siempre la metaetiqueta `viewport` adecuada.  
    `<meta name="viewport" content="width=device-width, initial-scale=1.0">`   
    La configuración de `width=device-width` coincidirá con el ancho de la pantalla en píxeles independientes del dispositivo, y la configuración `initial-scale=1` establece una relación de 1:1 entre los píxeles de CSS y los píxeles independientes del dispositivo. Esto le indica al navegador que adapte tu contenido al tamaño de la pantalla, para que os usuarios no vean solamente un grupo de texto apretado.

![a phone display without and with the viewport meta tag](imgs/scrunched-up.jpg)

Warning: When using the viewport meta tag, make sure you don't set maximum-scale=1 or set user-scaleable=no. Let users zoom if they need to!

- Otra técnica para tener en cuenta es el diseño con una planilla receptiva. Como viste con el sitio de Udacity, el diseño con planilla significa que tu contenido se reprocesará cuando cambie el tamaño de la página. A menudo estos diseños se producen usando unidades relativas como porcentajes, em o rem, en lugar de valores de píxel codificados en forma rígida. La ventaja de hacerlo de esta forma es que el texto y el contenido pueden agrandar y forzar a otros artículos a las que bajen en la página. Entonces el orden del DOM y el orden de lectura permanecen iguales, incluyo si el diseño cambia debido a una ampliación.

- Además, piensa en usar unidades relativas como `em` o `rem` para cosas como tamaño de texto, en lugar de valores de píxel. Algunos navegadores soportan el cambio de tamaño del texto solo en preferencias de usuario, y si usas un valor de píxel para el texto, esta configuración no afectará a tu copia. Si, sin embargo, has usado unidades relativas en todo, la copia del sitio se actualizará para reflejar la preferencia del usuario.

- Finalmente, cuando tu diseño aparece en un dispositivo móvil, deberías asegurarte de que los elementos interactivos como botones o vínculos sean lo suficientemente grandes y tengan suficiente espacio alrededor, para que sea sencillo presionarlos sin superponerse accidentalmente sobre otros elementos. Esto beneficia a todos los usuarios, pero es especialmente útil para quien tenga una discapacidad motriz.

Warning: Cuando uses la metaetiqueta de ventana de visualización, asegúrate de no configurar la escala máxima=1 o establecer escalable por usuario=no. Permite que los usuarios amplíen si lo necesitan.

![a diagram showing a couple of 48 pixel touch targets](imgs/touch-target.jpg)

Touch targets should also be spaced about 8 pixels apart, both horizontally and vertically, so that a user's finger pressing on one tap target does not inadvertently touch another tap target.

## Color y contraste

Los objetivos táctiles también deberían tener un espacio de alrededor de 8 píxeles de separación, en orientación horizontal y vertical, de manera que el dedo del usuario, al presionar un objetivo de presión, no toque inintencionalmente otro objetivo de presión.

Si tienes buena visión, es fácil asumir que todas las personas perciben el color o la legibilidad del texto de la misma manera que tú &mdash; pero, por supuesto, eso no es real. Finalicemos viendo cómo podemos usar en forma efectiva el color y el contraste para crear diseños agradables accesibles para todos.

Como puedes imaginar, algunas combinaciones de colores que a algunas personas les resultan fáciles de leer les resultan difíciles o imposibles a otras. Esto se suele reducir a *color contraste*, la relación entre la *luminosidad* de los colores de primer plano y de segundo plano. Cuando los colores son similares, la relación de contraste es baja. Cuando son diferentes, la relación de contraste es alta.

![comparison of various contrast ratios](imgs/contrast-ratios.jpg)

The contrast ratio of 4.5:1 was chosen for level AA because it compensates for the loss in contrast sensitivity usually experienced by users with vision loss equivalent to approximately 20/40 vision. 20/40 is commonly reported as typical visual acuity of people at about age 80. For users with low vision impairments or color deficiencies, we can increase the contrast up to 7:1 for body text.

La relación de contraste de 4:5:1 se escogió para el nivel AA porque compensa la pérdida de sensibilidad de contraste que los usuarios con pérdida de visión equivalente a una visión de aproximadamente 20/40 suelen experimentar. Se suele informar 20/40 como agudeza visual típica de personas de alrededor de 80 años. Para usuarios con leves daños visuales o deficiencias de colores, podemos incrementar el contraste hasta 7:1 en el texto del cuerpo.

Puedes usar la [extensión de accesibilidad DevTools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb){: .external } para Chrome, para identificar las relaciones de contraste. Uno de los beneficios del uso de Chrome Devtools es que sugiere alternativas de AA y AAA (mejoradas) a tus colores actuales, y puede hacer clic en los valores para obtener una vista previa en tu app.

1. Después de la instalación de la extensión, haz clic en `Audits`
2. Desmarca todo excepto `Accessibility`
3. Haz clic en `Audit Present State`
4. Ten en cuenta todas las advertencias de contraste

![the devtools contrast audit dialog](imgs/contrast-audit.png)

WebAIM itself provides a handy [color contrast checker](http://webaim.org/resources/contrastchecker/){: .external } you can use to examine the contrast of any color pair.

### No transmitas información solo con color

WebAIM brinda un útil [revisor de contraste de color](http://webaim.org/resources/contrastchecker/){: .external } que puedes usar para evaluar el contraste de cualquier par de colores.

Existen alrededor de 320 millones de usuarios con deficiencia de visión en color. Alrededor de 1 de 12 hombres y 1 de 200 mujeres tienen alguna forma de "daltonismo", lo que significa que alrededor de 1 de cada 20, o el 5%, de tus usuarios no verán tu sitio en la forma que deseas. Cuando confiamos en el color para transmitir la información, movemos ese número hasta niveles inaceptables.

Note: El término "daltonismo" se usa para describir una afección visual debido a la que una persona tiene dificultades para distinguir los colores, pero muy pocas personas realmente no distinguen ningún color. La mayoría de las personas que sufren deficiencias de color ven algunos o la mayorías de los colores, pero tienen dificultades para diferenciar entre ciertos colores, tales como rojos y verdes (lo más común), marrones y naranjas, y azules y violetas.

![an input form with an error underlined in red](imgs/input-form1.png)

The [WebAIM checklist states in section 1.4.1](http://webaim.org/standards/wcag/checklist#sc1.4.1){: .external } that "color should not be used as the sole method of conveying content or distinguishing visual elements." It also notes that "color alone should not be used to distinguish links from surrounding text" unless they meet certain contrast requirements. Instead, the checklist recommends adding an additional indicator such as an underscore (using the CSS `text-decoration` property) to indicate when the link is active.

La [lista de comprobación de WebAIM indica en la sección 1.4.1](http://webaim.org/standards/wcag/checklist#sc1.4.1){: .external } que "el color no debería ser el único método para transmitir contenido ni para distinguir elementos visuales". También indica que "no se debería usar solo el color para diferenciar vínculos del texto que los rodea" a menos que cumplan ciertos requisitos de contraste. En cambio, la lista de comprobación recomienda agregar un indicador adicional como un guión bajo (incluida la propiedad `text-decoration` de CSS) para indicar cuando el vínculo está activo.

![an input form with an added error message for clarity](imgs/input-form2.png)

When you're building an app, keep these sorts of things in mind and watch out for areas where you may be relying too heavily on color to convey important information.

Cuando construyas una app, ten estas cosas en mente y está atento a áreas en las que puedas estar confiando demasiado en el color para transmitir información importante.

### Modo de contraste alto

Si te da curiosidad cómo luce tu sitio para distintas personas o si confías demasiado en el uso del color en tu IU, puedes usar la [extensión NoCoffee Chrome](https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl){: .external } para simular varias formas de daños visuales, incluidos distintos tipos de daltonismo.

El modo de contraste alto le permite al usuario invertir los colores del primer plano y el segundo plano, lo cual a menudo ayuda a que el texto se destaque mejor. Para alguien con un daño visual leve, el modo de contraste alto puede facilitar mucho la navegación por el contenido de la página. Existen varias formas de configurar un contraste alto en tu máquina.

Los sistemas operativos como Mac OSX y Windows ofrecen modos de contraste alto que se pueden habilitar para todo al nivel del sistema. O los usuarios pueden instalar una extensión, como la [extensión Chrome High Contrast](https://chrome.google.com/webstore/detail/high-contrast/djcfdncoelnlbldjfhinnjlhdjlikmph){: .external } para habilitar el contraste alto solo en esa app específica.

Un ejercicio útil es encender la configuración de contraste alto y verificar que todas las IU de tu app sean visibles y usables.

![a navigation bar in high contrast mode](imgs/tab-contrast.png)

Similarly, if you consider the example from the previous lesson, the red underline on the invalid phone number field might be displayed in a hard-to-distinguish blue-green color.

![a form with an error field in high contrast mode](imgs/high-contrast.jpg)

If you are meeting the contrast ratios covered in the previous lessons you should be fine when it comes to supporting high-contrast mode. But for added peace of mind, consider installing the Chrome High Contrast extension and giving your page a once-over just to check that everything works, and looks, as expected.

## Feedback {: #feedback }

Si alcanzas las relaciones de contraste cubiertas en las lecciones anteriores, no deberías tener ningún problema a la hora de soportar el modo de contraste alto. Pero, para más tranquilidad, piensa en instalar la extensión Chrome High Contrast y darle a tu página una revisada para comprobar que todo funcione y luzca como esperas.