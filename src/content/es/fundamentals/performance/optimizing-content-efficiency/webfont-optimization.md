project_path: /web/fundamentals/_project.yaml book_path: /web/fundamentals/_book.yaml description: La tipografía es fundamental para lograr un buen nivel de diseño, desarrollo de la marca, legibilidad y accesibilidad. Las fuentes web permiten todo lo anterior y más: el texto admite selección, búsqueda, ampliación y reducción, y admite valores elevados de ppp, lo que permite la representación de texto consistente y bien definido, independientemente del tamaño de la pantalla y la resolución.

{# wf_updated_on: 2019-02-06 #} {# wf_published_on: 2014-09-19 #} {# wf_blink_components: Blink>CSS #}

# Optimización de fuentes web {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

*This article contains contributions from [Monica Dinculescu](https://meowni.ca/posts/web-fonts/), [Rob Dodson](/web/updates/2016/02/font-display), and Jeff Posnick.*

La optimización de fuentes web es una pieza crítica de la estrategia de rendimiento general. Cada fuente es un recurso adicional, y algunas fuentes pueden bloquear la representación del texto; sin embargo, el hecho de que en la página se usen fuentes para sitios web no significa que su representación sea lenta. Por el contrario, las fuentes optimizadas, combinadas con una estrategia criteriosa sobre cómo se deben cargar y aplicar en la página, pueden ayudar a reducir el tamaño total de la página y mejorar los tiempos de representación de la página.

Una *fuente para sitios web* es un conjunto de glifos y cada glifo es una forma vectorial que describe una letra o un símbolo. En consecuencia, el tamaño de un archivo de fuente específico está determinado por dos variables simples: la complejidad de las rutas de acceso vectoriales de cada glifo y la cantidad de glifos en una fuente determinada. Por ejemplo, Open Sans, que es una de las fuentes para sitios web más populares, contiene 897 glifos, entre los que se incluyen caracteres latinos, griegos y cirílicos.

## Anatomía de una fuente para sitios web

### TL;DR {: .hide-from-toc }

* Las fuentes Unicode pueden contener miles de glifos.
* Existen cuatro formatos de fuente: WOFF2, WOFF, EOT y TTF.
* Algunos formatos de fuente requieren compresión.

A *webfont* is a collection of glyphs, and each glyph is a vector shape that describes a letter or symbol. As a result, two simple variables determine the size of a particular font file: the complexity of the vector paths of each glyph and the number of glyphs in a particular font. For example, Open Sans, which is one of the most popular webfonts, contains 897 glyphs, which include Latin, Greek, and Cyrillic characters.

<img src="images/glyphs.png"  alt="Font glyph table" />

Claramente, el uso de fuentes en la Web requiere una ingeniería cuidadosa para garantizar que la tipografía no obstaculice el rendimiento. Afortunadamente, la plataforma web proporciona todas las primitivas necesarias, y en el resto de esta guía, abordaremos una observación práctica relacionada con cómo sacar el mayor provecho de ambos aspectos.

En la actualidad, en la Web se usan cuatro formatos de contenedores: [EOT](https://en.wikipedia.org/wiki/Embedded_OpenType), [TTF](https://en.wikipedia.org/wiki/TrueType), [WOFF](https://en.wikipedia.org/wiki/Web_Open_Font_Format) y [WOFF2](https://www.w3.org/TR/WOFF2/){: .external }. Lamentablemente, a pesar de la gran variedad de opciones, no hay un formato universal que funcione tanto en los navegadores nuevos como en los más antiguos: EOT [solo es compatible con IE](http://caniuse.com/#feat=eot), TTF [es parcialmente compatible con IE](http://caniuse.com/#search=ttf), WOFF es el que ofrece la compatibilidad más amplia, aunque [no está disponible en algunos navegadores anteriores](http://caniuse.com/#feat=woff) y la compatibilidad en WOFF 2.0 [se encuentra en desarrollo para muchos navegadores](http://caniuse.com/#feat=woff2).

### Formatos de fuentes para sitios web

Entonces, ¿a dónde nos lleva todo esto? No hay un solo formato que funcione en todos los navegadores. Esto significa que debemos ofrecer varios formatos para proporcionar una experiencia uniforme:

Note: Técnicamente existe otro formato de contenedor, el [contenedor de fuente SVG](http://caniuse.com/svg-fonts), pero IE y Firefox nunca lo admitieron, y ahora quedó en desuso en Chrome. Por este motivo, es de uso limitado y se omite intencionalmente en esta guía.

* Proporciona la variante WOFF 2.0 para los navegadores que la admitan.
* Proporciona la variante WOFF para la mayoría de los navegadores.
* Proporciona la variante TTF para navegadores Android previos (anteriores a la versión 4.4).
* Proporciona la variante EOT para navegadores IE anteriores (previos a IE9).

Una fuente es un conjunto de glifos, y cada uno de estos es un conjunto de rutas de acceso que describen la forma de la letra. Los glifos individuales son diferentes, pero contienen mucha información similar que se puede comprimir con GZIP o un compresor compatible:

### Reducción del tamaño de fuente con compresión

Por último, cabe mencionar que algunos formatos de fuente contienen metadatos adicionales, como información de [sugerencia de fuentes](https://en.wikipedia.org/wiki/Font_hinting) e [interletraje](https://en.wikipedia.org/wiki/Kerning) que puede no ser necesaria en algunas plataformas, lo cual permite optimizar aún más el tamaño de archivo. Consulta el compresor de fuentes para averiguar las opciones de optimización disponibles y, si sigues este camino, asegúrate de contar con la infraestructura adecuada para probar y proporcionar estas fuentes optimizadas a cada navegador específico. Por ejemplo, [Google Fonts](https://fonts.google.com/) ofrece más de 30 variantes optimizadas para cada fuente, y detecta y proporciona automáticamente la variante óptima para cada plataforma y navegador.

* Los formatos EOT y TTF no se comprimen de forma predeterminada. Asegúrate de que los servidores estén configurados para aplicar [compresión GZIP](/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer#text-compression-with-gzip) cuando proporciones estos formatos.
* WOFF cuenta con compresión integrada. Asegúrate de que el compresor WOFF esté configurado con los ajustes de compresión óptimos.
* WOFF2 usa algoritmos personalizados de procesamiento previo y compresión para permitir una reducción del tamaño de archivo de aproximadamente el 30 % en comparación con otros formatos. Para obtener más información, consulta [el informe de evaluación de WOFF 2.0](http://www.w3.org/TR/WOFF20ER/){: .external }.

Note: Considera usar [compresión Zopfli](http://en.wikipedia.org/wiki/Zopfli) para los formatos EOT, TTF y WOFF. Zopfli es un compresor compatible con zlib y permite una reducción del tamaño de archivo que supera en aproximadamente un 5 % a la de gzip.

La regla-at `@font-face` de CSS te permite definir la ubicación de un recurso de fuente determinado, sus características de estilo y los puntos de código Unicode para los que se debe usar. Se puede usar una combinación de estas declaraciones`@font-face a fin de construir una "familia de fuentes" que el navegador usará para evaluar los recursos de fuente que debe descargar y aplicar a la página actual.

## Definición de la familia de fuentes con @font-face

### TL;DR {: .hide-from-toc }

* Usa la indicación `format()` para especificar varios formatos de fuente.
* Dispone en subconjuntos las fuentes Unicode grandes para mejorar el rendimiento. Usa subconjuntos de Unicode y proporciona una reserva de subconjunto manual para navegadores anteriores.
* Reduce la cantidad de variantes de fuente estilísticas para mejorar el rendimiento de representación de la página y del texto.

Cada declaración `@font-face` proporciona el nombre de la familia de fuentes, que actúa como un grupo lógico compuesto por varias declaraciones, [propiedades de fuentes](http://www.w3.org/TR/css3-fonts/#font-prop-desc) como el estilo, el espesor y la expansión, y el [descriptor src](http://www.w3.org/TR/css3-fonts/#src-desc), que especifica una lista priorizada de ubicaciones para el recurso de fuente.

### Selección de formato

Primero, observa que los ejemplos anteriores definen una familia *Awesome Font* individual con dos estilos (normal e *italic*), y cada uno apunta a un conjunto diferente de recursos de fuente. A la vez, cada descriptor `src` contiene una lista priorizada de variantes de recursos separadas por comas:

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome.woff2') format('woff2'),
           url('/fonts/awesome.woff') format('woff'),
           url('/fonts/awesome.ttf') format('truetype'),
           url('/fonts/awesome.eot') format('embedded-opentype');
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: italic;
      font-weight: 400;
      src: local('Awesome Font Italic'),
           url('/fonts/awesome-i.woff2') format('woff2'),
           url('/fonts/awesome-i.woff') format('woff'),
           url('/fonts/awesome-i.ttf') format('truetype'),
           url('/fonts/awesome-i.eot') format('embedded-opentype');
    }
    

Note: A menos que hagas referencia a las fuentes predeterminadas del sistema, en la práctica es poco común que el usuario lo tenga instalado localmente, en especial en dispositivos móviles, en los cuales resulta imposible “instalar” fuentes adicionales. Debes empezar con una entrada `local()` "por si acaso" y, luego, proporciona una lista de entradas `url()`.

* La directiva `local()` te permite hacer referencia a fuentes instaladas localmente, cargarlas y usarlas.
* La directiva `url()` te permite cargar fuentes externas y pueden contener una sugerencia de `format()` opcional que indique el formato de la fuente a la que se hace referencia en la URL proporcionada.

Cuando el navegador determina que se necesita la fuente, recorre la lista de recursos proporcionada en el orden especificado e intenta cargar el recurso correcto. Por caso, si se sigue el ejemplo anterior:

La combinación de directivas locales y externas con sugerencias de formato adecuadas te permite especificar todos los formatos de fuente disponibles y dejar que el navegador haga el resto. El navegador detecta qué recursos son necesarios y selecciona el formato óptimo.

1. El navegador realiza un diseño de la página y determina las variantes de fuente necesarias para representar en la página el texto especificado.
2. Para cada fuente necesaria, el navegador comprueba si la fuente está disponible localmente.
3. Si la fuente no está disponible localmente, el navegador recorre definiciones externas: 
    * Si hay una sugerencia de formato presente, el navegador comprueba si la admite antes de iniciar la descarga; de lo contrario, avanza hacia la siguiente.
    * Si no hay sugerencias de formato, el navegador descarga el recurso.

Note: El orden en el que se especifican las variantes de las fuentes es importante. El navegador selecciona el primer formato compatible. Por lo tanto, si deseas que los navegadores más nuevos usen WOFF2, debes disponer la declaración de WOFF2 por encima de la de WOFF, etc.

Además de las propiedades de la fuente, como estilo, espesor y estiramiento, la regla `@font-face` nos permite definir un conjunto de puntos de código Unicode admitidos para cada recurso. Esto nos permite dividir una fuente Unicode grande en subconjuntos más pequeños (por ejemplo, los subconjuntos latino, cirílico y griego) y solo descargar los glifos requeridos para representar el texto en una página determinada.

### Subdivisión de Unicode

El [descriptor del intervalo Unicode](http://www.w3.org/TR/css3-fonts/#descdef-unicode-range) te permite especificar una lista de valores de intervalo delimitada por comas, en la que cada valor puede presentar una de tres formas diferentes:

Por ejemplo, puedes dividir tu familia *Awesome Font* en subconjuntos latino y japonés que el navegador descarga cuando sea necesario:

* Punto de código individual (por ejemplo, `U+416`)
* Amplitud del intervalo (por ejemplo, `U+400-4ff`): indica los puntos de código inicial y final de un intervalo
* Intervalo comodín (por ejemplo, `U+4??`): los caracteres `?` indican dígitos hexadecimales

Note: La disposición en subconjuntos de Unicode tiene particular importancia para los idiomas asiáticos, en los cuales la cantidad de glifos es mucho mayor que en los idiomas occidentales y la medición de una fuente "completa" normalmente se realiza en megabytes en lugar de decenas de kilobytes.

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'),
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('truetype'),
           url('/fonts/awesome-l.eot') format('embedded-opentype');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-jp.woff2') format('woff2'),
           url('/fonts/awesome-jp.woff') format('woff'),
           url('/fonts/awesome-jp.ttf') format('truetype'),
           url('/fonts/awesome-jp.eot') format('embedded-opentype');
      unicode-range: U+3000-9FFF, U+ff??; /* Japanese glyphs */
    }
    

El uso de subconjuntos del intervalo Unicode y archivos independientes para cada variante estilística nos permite definir una familia de fuentes combinada cuya descarga es más rápida y más eficiente. El visitante solo descargará las variantes y los subconjuntos que necesita, y no está obligado a descargar subconjuntos que quizá nunca vea ni use en la página.

Dicho esto, solo hay un pequeño problema con el intervalo Unicode: [no todos los navegadores lo admiten](http://caniuse.com/#feat=font-unicode-range) todavía. Algunos navegadores simplemente ignoran la sugerencia del intervalo Unicode y descargan todas las variantes, mientras otros podrían no procesar la declaración `@font-face` en absoluto. Para abordar esto, debemos recurrir a la "subdivisión manual" para navegadores anteriores.

Dado que los navegadores anteriores no son lo suficientemente inteligentes para seleccionar solo los subconjuntos necesarios y no pueden construir una fuente compuesta, debes recurrir a proporcionar un solo recurso de fuente que contenga todos los subconjuntos necesarios y oculte el resto del navegador. Por ejemplo, si la página solo usa caracteres latinos, puedes eliminar otros glifos y proporcionar ese subconjunto específico como recurso independiente.

Cada familia de fuentes está compuesta por múltiples variantes estilísticas (regular, negrita, cursiva) y múltiples espesores para cada estilo; y cada estilo, a la vez, puede contener diferentes formas de glifos, como espaciado diferente, ajuste de tamaño diferente o una forma diferente.

1. **¿Cómo se determinan los subconjuntos necesarios?** 
    * Si el navegador admite la subdivisión del intervalo Unicode, seleccionará automáticamente el subconjunto adecuado. La página solo deberá proporcionar los archivos del subconjunto y especificar intervalos Unicode apropiados en las reglas `@font-face`.
    * Si el navegador no admite la subdivisión del intervalo Unicode, la página debe ocultar todos los subconjuntos innecesarios; es decir, el programador debe especificar los subconjuntos necesarios.
2. **¿Cómo generas subconjuntos de fuentes?** 
    * Usa la [herramienta pyftsubset](https://github.com/behdad/fonttools/){: .external } de código abierto para disponer en subconjuntos y optimizar tus fuentes.
    * Algunos servicios de fuente permiten la subdivisión manual mediante parámetros de consulta personalizados, que puedes usar para especificar manualmente el subconjunto necesario para tu página. Consulta la documentación de tu proveedor de fuentes.

### Selección y síntesis de fuentes

Each font family is composed of multiple stylistic variants (regular, bold, italic) and multiple weights for each style, each of which, in turn, may contain very different glyph shapes&mdash;for example, different spacing, sizing, or a different shape altogether.

<img src="images/font-weights.png"  alt="Font weights" />

Se aplica la misma lógica a las variantes de *cursiva*. El diseñador de fuentes controla las variantes que producirá y tú controlas las que usarás en la página. Dado que cada variante supone una descarga individual, se recomienda que el número de variantes sea reducido. Por ejemplo, puedes definir dos variantes en negrita para la familia *Awesome Font*:

> Cuando se especifica un espesor para el que no existe una fuente, se usa una fuente con un espesor aproximado. En general, los espesores en negrita se asignan a fuentes con espesores más gruesos, y los espesores finos se asignan a fuentes con espesores más delgados.
> 
> > [Algoritmo de coincidencia de fuentes CSS3](http://www.w3.org/TR/css3-fonts/#font-matching-algorithm)

En el ejemplo anterior, se declara que la familia *Awesome Font* está compuesta por dos recursos que abarcan el mismo conjunto de glifos latinos (`U+000-5FF`), pero estos ofrecen dos "espesores" diferentes: normal (400) y negrita (700). Sin embargo, ¿qué sucede si en una de nuestras reglas CSS se especifica un espesor de fuente diferente o se fija la propiedad de estilo de la fuente en cursiva?

    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 400;
      src: local('Awesome Font'),
           url('/fonts/awesome-l.woff2') format('woff2'),
           url('/fonts/awesome-l.woff') format('woff'),
           url('/fonts/awesome-l.ttf') format('truetype'),
           url('/fonts/awesome-l.eot') format('embedded-opentype');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    
    @font-face {
      font-family: 'Awesome Font';
      font-style: normal;
      font-weight: 700;
      src: local('Awesome Font'),
           url('/fonts/awesome-l-700.woff2') format('woff2'),
           url('/fonts/awesome-l-700.woff') format('woff'),
           url('/fonts/awesome-l-700.ttf') format('truetype'),
           url('/fonts/awesome-l-700.eot') format('embedded-opentype');
      unicode-range: U+000-5FF; /* Latin glyphs */
    }
    

The above example declares the *Awesome Font* family that is composed of two resources that cover the same set of Latin glyphs (`U+000-5FF`) but offer two different "weights": normal (400) and bold (700). However, what happens if one of your CSS rules specifies a different font weight, or sets the font-style property to italic?

* Si no hay una coincidencia de fuente exacta disponible, el navegador usa la coincidencia más cercana.
* Si no se encuentra una coincidencia estilística (por ejemplo, en el caso anterior no se declararon variantes de cursiva), el navegador sintetiza su propia variante de fuente.

<img src="images/font-synthesis.png"  alt="Font synthesis" />

En el ejemplo anterior, se ilustra la diferencia entre los resultados de la fuente real frente a la fuente sintetizada para Open Sans. Todas las variantes sintetizadas se generan a partir de una fuente individual con un espesor de 400. Como puedes ver, existe una diferencia notoria en los resultados. No se especifican los detalles sobre cómo generar las variantes negrita y oblicua. Por lo tanto, los resultados varían de un navegador a otro y también dependerán en gran medida de la fuente.

Note: Para obtener la mejor uniformidad y los mejores resultados visuales, no debes basarte en la síntesis de fuentes. En lugar de ello, minimiza la cantidad de variantes de las fuentes que usas y especifica sus ubicaciones para que el navegador pueda descargarlas cuando se usen en la página. Dicho esto, en algunos casos una variante sintetizada [podría ser una opción viable](https://www.igvita.com/2014/09/16/optimizing-webfont-selection-and-synthesis/), pero debes actuar con precaución al usar variantes sitentizadas.

Una fuente para sitios web "completa" que incluya todas las variantes estilísticas, que quizá no necesites, y todos los glifos, que quizá no se usen, fácilmente puede implicar una descarga de muchos megabytes. Para abordar esto, la regla CSS `@font-face` está específicamente diseñada con el propósito de permitirte dividir la familia de fuentes en una colección de recursos: subconjuntos Unicode, diferentes variantes estilísticas, etc.

## Optimización de la carga y la representación

### TL;DR {: .hide-from-toc }

* Las solicitudes de fuente se difieren hasta que se crea el árbol de representación, lo cual puede generar demoras en la representación del texto.
* `<link rel="preload">`, la propiedad de CSS `font-display` y la API Font Loading proporcionan los enlaces necesarios para implementar estrategias personalizadas de carga y representación de fuentes que anulan el comportamiento predeterminado.

Dadas estas declaraciones, el navegador descifrará las variantes y los subconjuntos necesarios, y descargará el conjunto mínimo requerido para representar el texto, lo cual es muy conveniente. Sin embargo, si no eres cuidadoso, también puede crear un cuello de botella de rendimiento en la ruta de acceso de representación crítica y retardar la representación del texto.

La carga lenta de fuentes conlleva una consecuencia oculta importante que puede retardar la representación del texto: el navegador debe [construir el árbol de representación](/web/fundamentals/performance/critical-rendering-path/render-tree-construction), que depende de los árboles del DOM y CSSOM, a fin de determinar los recursos que necesita para representar el texto. Como resultado, las solicitudes de fuentes se demoran hasta mucho después de otros recursos críticos y la representación del texto en el navegador puede quedar bloqueada hasta que se obtenga el recurso.

### El comportamiento predeterminado

Lazy loading of fonts carries an important hidden implication that may delay text rendering: the browser must [construct the render tree](/web/fundamentals/performance/critical-rendering-path/render-tree-construction), which is dependent on the DOM and CSSOM trees, before it knows which font resources it needs in order to render the text. As a result, font requests are delayed well after other critical resources, and the browser may be blocked from rendering text until the resource is fetched.

<img src="images/font-crp.png"  alt="Font critical rendering path" />

1. El navegador solicita el archivo HTML.
2. El navegador comienza a analizar la respuesta HTML y construir el DOM.
3. El navegador detecta CSS, JS y otros recursos, y envía solicitudes.
4. El navegador construye el CSSOM una vez que recibe todo el contenido CSS, y lo combina con el árbol del DOM para construir el árbol de representación. 
    * Las solicitudes se envían un vez que el árbol de representación indica las variantes de fuente que se necesitan para representar el texto especificado en la página.
5. El navegador realiza un diseño y pinta contenido en la pantalla. 
    * Si la fuente aún no está disponible, es posible que en el navegador no se representen los píxeles de texto.
    * Cuando la fuente está disponible, el navegador pinta los píxeles de texto.

En la siguiente sección, se describe una serie de opciones para personalizar este comportamiento predeterminado.

Si existe una probabilidad alta de que la página necesite una fuente para sitios web específica alojada en una URL conocida de antemano, puedes aprovechar la característica de una plataforma nueva: [`<link rel="preload">`](/web/fundamentals/performance/resource-prioritization).

### Precargar los recursos de la fuente para sitios web

Permite incluir un elemento en el código HTML, en general como parte del `<head>`, lo cual activa una solicitud de la fuente para sitios web al comienzo de la ruta de acceso de representación crítica, sin tener que esperar la creación del CSSOM.

`<link rel="preload">` le "sugiere" al navegador que un recurso dado va a necesitarse pronto, pero no le indica al navegador *cómo* usarlo. Debes usar la precarga junto con una definición adecuada de CSS `@font-face` para poder indicarle al navegador qué hacer con determinada URL de la fuente para sitios web.

No todos los navegadores [admiten `<link rel="preload">`](https://caniuse.com/#feat=link-rel-preload), y en estos navegadores, `<link rel="preload">` simplemente se pasa por alto. No obstante, cada navegador que admite la precarga también admite WOFF2, por lo que este es siempre el formato que debe precargarse.

```html
<head>
  <!-- Other tags... -->
  <link rel="preload" href="/fonts/awesome-l.woff2" as="font">
</head>
```

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

Warning: El uso de `<link rel="preload">` activa una solicitud incondicional, de alta prioridad para la URL de la fuente para sitios web, independientemente de que termine o no siendo necesaria en la página. Si hay una posibilidad razonable de que no se necesite una copia remota de la fuente para sitios web, por ejemplo, porque la definición de `@font-face` incluye una entrada `local()` para una fuente común como Roboto, entonces el uso de `<link rel="preload">` dará como resultado una solicitud desperdiciada. Algunos navegadores muestran una advertencia en la consola de Developer Tools para desarrolladores cuando un recurso se precarga , pero en realidad no se usa.

Si bien a partir de la precarga es más probable que una fuente para sitios web esté disponible al representar el contenido de una página, no hay ninguna garantía de que esto ocurra. Deberás tener en cuenta de qué modo se comportan los navegadores cuando representan texto que usa `font-family`, lo cual todavía no está disponible.

### Personalizar la demora de representación de texto

La "carrera" entre la primera pintura del contenido de la página, que puede ocurrir poco después de la construcción del árbol de representación, y la solicitud del recurso de fuente son los elementos que generan el "problema de texto en blanco"; cuando este se produce, el navegador puede representar el diseño de la página, pero omite el texto. La mayoría de los navegadores implementan un tiempo de espera máximo para la descarga de una fuente para sitios web; transcurrido ese tiempo, se utiliza una fuente de reserva. Lamentablemente, los navegadores difieren en su implementación:

#### Comportamientos de los navegadores

Para garantizar la coherencia de ahí en adelante, el grupo de trabajo de CSS propuso un nuevo descriptor `@font-face`, [`font-display`](https://drafts.csswg.org/css-fonts-4/#font-display-desc) y su correspondiente propiedad para controlar el modo en que se comporta una fuente descargable antes de cargarse.

<table>
  <thead>
    <tr>
      <th data-th="Browser">Navegador</th>
      <th data-th="Timeout">Tiempo de espera</th>
      <th data-th="Fallback">Reserva</th>
      <th data-th="Swap">Intercambio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td data-th="Browser">
        <strong>Chrome 35+</strong>
      </td>
      <td data-th="Timeout">
        3 segundos
      </td>
      <td data-th="Fallback">
        Sí
      </td>
      <td data-th="Swap">
        Sí
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Opera</strong>
      </td>
      <td data-th="Timeout">
        3 segundos
      </td>
      <td data-th="Fallback">
        Sí
      </td>
      <td data-th="Swap">
        Sí
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Firefox</strong>
      </td>
      <td data-th="Timeout">
        3 segundos
      </td>
      <td data-th="Fallback">
        Sí
      </td>
      <td data-th="Swap">
        Sí
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Internet Explorer</strong>
      </td>
      <td data-th="Timeout">
        0 segundos
      </td>
      <td data-th="Fallback">
        Sí
      </td>
      <td data-th="Swap">
        Sí
      </td>
    </tr>
    <tr>
      <td data-th="Browser">
        <strong>Safari</strong>
      </td>
      <td data-th="Timeout">
        Sin tiempo de espera
      </td>
      <td data-th="Fallback">
        N/C
      </td>
      <td data-th="Swap">
        N/C
      </td>
    </tr>
  </tbody>
</table>

* Chrome y Firefox tienen un tiempo de espera de tres segundos; transcurrido ese tiempo, el texto se muestra con la fuente de reserva. Si la fuente logra descargarse, finalmente se produce un intercambio y el texto se vuelve a representar con la fuente esperada.
* Internet Explorer tiene un tiempo de espera de cero segundos, que se traduce en la representación inmediata del texto. Si la fuente solicitada aún no está disponible, se usa una de reserva y el texto se vuelve a representar una vez que la fuente solicitada está disponible.
* Safari no cuenta con un comportamiento de tiempo de espera (o al menos no otro que el tiempo de espera inicial de la red).

De manera similar al comportamiento de tiempo de espera de las fuentes existentes que hoy en día implementan algunos navegadores, `font-display` segmenta la vida útil de una descarga de fuente en tres períodos principales:

#### Línea de tiempo de visualización de la fuente

Al comprender estos períodos, se podrá usar `font-display` para decidir de qué modo debe representarse la fuente si se descarga y según cuándo se descargue.

1. El primer período es el **período de bloqueo de fuente**. En él, si el tipo de fuente no se carga, cualquier elemento que intente usarla debe representarse con un tipo de fuente de reserva invisible. Si el tipo de fuente se carga correctamente durante el período de bloqueo, entonces se utiliza de manera normal.
2. El **período de intercambio de fuente** ocurre inmediatamente después del período de bloqueo. En él, si el tipo de fuente no se carga, cualquier elemento que intente usarla debe representarse con un tipo de fuente de reserva. Si el tipo de fuente se carga correctamente durante el período de intercambio, entonces se utiliza de manera normal.
3. El **período de error de fuente** ocurre inmediatamente después del período de intercambio. Si el tipo de fuente aún no se cargó cuando comienza este período, se lo marca como una carga fallida y se utiliza la fuente de reserva. De lo contrario, el tipo de fuente se utiliza con normalidad.

Para trabajar con la propiedad `font-display`, debes agregarla a las reglas `@font-face`:

#### Uso de font-display

`font-display` actualmente admite el siguiente rango de valores: `auto | block | swap | fallback | optional`.

```css
@font-face {
  font-family: 'Awesome Font';
  font-style: normal;
  font-weight: 400;
  font-display: auto; /* or block, swap, fallback, optional */
  src: local('Awesome Font'),
       url('/fonts/awesome-l.woff2') format('woff2'), /* will be preloaded */
       url('/fonts/awesome-l.woff') format('woff'),
       url('/fonts/awesome-l.ttf') format('truetype'),
       url('/fonts/awesome-l.eot') format('embedded-opentype');
  unicode-range: U+000-5FF; /* Latin glyphs */
}
```

`font-display` está [implementándose cada vez más](https://caniuse.com/#feat=css-font-rendering-controls) en muchos navegadores modernos. A medida que se implemente a gran escala, puede esperarse un comportamiento coherente del navegador.

* **`auto`** usa la estrategia de visualización de fuente que usa el agente. La mayoría de los navegadores actualmente cuentan con una estrategia predeterminada similar a `block`.

* **`block`** le asigna al tipo de fuente un breve período de bloqueo (se recomiendan 3 segundos en la mayoría de los casos) y un período de intercambio infinito. Es decir, el navegador extrae texto "invisible" en primer lugar si no se carga la fuente, pero intercambia el tipo de fuente en cuanto se carga. Para ello, el navegador crea un tipo de fuente anónima con métricas similares a la fuente seleccionada, pero todos los glifos carecen de "tinta". Este valor solo debe usarse si se requiere la representación de texto en un tipo de letra específico para que la página pueda usarse.

* **`swap`** asigna al tipo de fuente un período de bloqueo de cero segundos y un período de intercambio infinito. Esto significa que el navegador extrae texto de inmediato con una reserva si el tipo de fuente no se carga, pero intercambia el tipo de fuente en cuanto se carga. De manera similar a `block`, este valor debe usarse solo si la representación de texto en una fuente en particular es importante para la página, aunque representar en cualquier fuente de todos modos serviría para transmitir el mensaje. El texto de logotipos es un buen candidato para el **intercambio**, ya que mostrar el nombre de una empresa con una reserva razonable servirá para transmitir el mensaje, pero finalmente se usará el tipo de letra oficial.

* **`fallback`** asigna al tipo de fuente un período de bloqueo extremadamente breve (se recomiendan 100 ms o menos en la mayoría de los casos) y un breve período de intercambio (se recomiendan 3 segundos en casi todos los casos). En otras palabras: el tipo de fuente se representa con una reserva en primer lugar si no está cargado, pero la fuente se intercambia en cuanto se carga. Sin embargo, si transcurre demasiado tiempo, se usa la reserva para el resto de la vida útil de la página. `fallback` es un buen candidato, por ejemplo, para texto de cuerpo si queremos que el usuario empiece a leer lo antes posible y no queremos perturbar su experiencia cambiando el texto a medida que se carga una fuente nueva.

* **`optional`** asigna al tipo de fuente un período de bloqueo extremadamente breve (se recomiendan 100 ms o menos en la mayoría de los casos) y un período de intercambio de cero segundos. De manera similar a `fallback`, esta es una buena opción cuando la fuente que se está descargando es "atractiva", pero no es fundamental para la experiencia. El valor `optional` deja que el navegador decida si iniciará la descarga de la fuente: puede elegir no hacerlo o puede hacerlo con una prioridad baja, según lo que considere mejor para el usuario. Esto puede resultar de utilidad en los casos donde el usuario tiene una conexión débil y obtener una fuente quizás no constituya el mejor uso de los recursos.

Si se usan en conjunto, `<link rel="preload">` y CSS `font-display` ofrecen a los desarrolladores bastante control sobre la representación y la carga de fuentes, pero no suman demasiada sobrecarga. Ahora bien, si necesitas un nivel mayor de personalización y estás dispuesto a soportar la sobrecarga que presenta la ejecución de JavaScript, hay otra opción.

### Font Loading API

La [API de Font Loading](https://www.w3.org/TR/css-font-loading/) ofrece una interfaz de escritura para definir y manipular los tipos de fuente CSS, realizar un seguimiento de la descarga y anular su comportamiento predeterminado de carga lenta. Por ejemplo, si estás seguro de que se requerirá una variante de fuente determinada, puedes definirla e indicar al navegador que inicie una obtención inmediata del recurso de fuente:

Además, debido a que puedes comprobar el estado de la fuente (a través del método [check()](https://www.w3.org/TR/css-font-loading/#font-face-set-check)) y realizar un seguimiento del progreso de la descarga, también puedes definir una estrategia personalizada para representar texto en tus páginas:

    var font = new FontFace("Awesome Font", "url(/fonts/awesome.woff2)", {
      style: 'normal', unicodeRange: 'U+000-5FF', weight: '400'
    });
    
    // don't wait for the render tree, initiate an immediate fetch!
    font.load().then(function() {
      // apply the font (which may re-render text and cause a page reflow)
      // after the font has finished downloading
      document.fonts.add(font);
      document.body.style.fontFamily = "Awesome Font, serif";
    
      // OR... by default the content is hidden,
      // and it's rendered after the font is available
      var content = document.getElementById("content");
      content.style.visibility = "visible";
    
      // OR... apply your own render strategy here...
    });
    

Lo mejor de todo es que también puedes combinar estas estrategias para contenido diferente en la página. Por ejemplo, puedes demorar la representación de texto en algunas secciones hasta que la fuente esté disponible, usar una fuente de reserva y luego volver realizar la representación cuando la descarga de la fuente haya finalizado, especificar diferentes tiempos de espera, etc.

* Puedes demorar la representación de todo el texto hasta que la fuente esté disponible.
* Puedes implementar un tiempo de espera personalizado para cada fuente.
* Puedes usar la fuente de reserva para desbloquear la representación e inyectar un nuevo estilo que use la fuente deseada cuando esté disponible.

Note: La API de Font Loading aún se encuentra [en desarrollo para algunos navegadores](http://caniuse.com/#feat=font-loading). Considera usar el polyfill [FontLoader](https://github.com/bramstein/fontloader) o la biblioteca [webfontloader](https://github.com/typekit/webfontloader) para proporcionar una funcionalidad similar, aunque con la sobrecarga que implica una dependencia adicional de JavaScript.

Generalmente, los recursos de fuentes son recursos estáticos que no presentan actualizaciones frecuentes. En consecuencia, son ideales para una vida útil prolongada; asegúrate de especificar un [encabezado ETag condicional](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#validating-cached-responses-with-etags) y una [óptima política de Cache-Control](/web/fundamentals/performance/optimizing-content-efficiency/http-caching#cache-control) para todos los recursos de fuente.

### Un correcto almacenamiento en caché es obligatorio

Si la aplicación web utiliza un [service worker](/web/fundamentals/primers/service-workers/), proporcionar recursos de fuente con una [estrategia en la que se prioriza la caché](/web/fundamentals/instant-and-offline/offline-cookbook/#cache-then-network) es adecuado en la mayoría de los casos de uso.

No debes almacenar las fuentes mediante [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) o [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API); cada una tiene su propia serie de problemas de rendimiento. La caché HTTP del navegador ofrece el mecanismo más adecuado y sólido para proporcionar recursos de fuente al navegador.

En contra de lo que se suele creer, el uso de fuentes para sitios web no debería demorar la representación de la página ni afectar negativamente otras métricas de rendimiento. El uso bien optimizado de fuentes puede ofrecer una experiencia general de usuario mucho más positiva: excelente desarrollo de marca; legibilidad, usabilidad y capacidad de búsqueda mejoradas; al mismo tiempo, se proporciona una solución escalable en varias resoluciones que se adapta bien a los formatos y las resoluciones de pantalla. No dudes en usar fuentes para sitios web.

## Lista de comprobación de optimización

Dicho esto, una implementación nueva puede implicar descargas pesadas y demoras innecesarias. Tú debes ayudar al navegador optimizando los recursos de fuente y la manera en que se obtienen y usan en tus páginas.

That said, a naive implementation may incur large downloads and unnecessary delays. You need to help the browser by optimizing the font assets themselves and how they are fetched and used on your pages.

* **Audita y controla el uso de fuentes:** no uses demasiadas fuentes en las páginas y, para cada fuente, minimiza la cantidad de variantes empleadas. Esto ayuda a proporcionar una experiencia más uniforme y rápida para los usuarios.
* **Subdivide los recursos de fuentes:** muchas fuentes pueden subdividirse en diferentes intervalos Unicode a fin de proporcionar solo los glifos necesarios para una página determinada; esto reduce el tamaño de archivo y mejora la velocidad de descarga del recurso. No obstante, al definir subconjuntos, procura aplicar optimizaciones para la reutilización de las fuentes; por ejemplo, no te convendrá descargar un conjunto de caracteres diferentes y superpuestos en cada página. Una buena práctica es la disposición en subconjuntos según el tipo de fuente; por ejemplo, latino, cirílico, etc.
* **Proporciona formatos de fuente optimizados para cada navegador:** las fuentes deben proporcionarse en los formatos WOFF2, WOFF, EOT y TTF. Asegúrate de aplicar compresión GZIP a los formatos EOT y TTF, ya que no se comprimen de forma predeterminada.
* **Prioriza la opción `local()` en la lista de `src`:** al asignar a `local('Font Name')` el primer lugar en la lista de `src`, se garantiza que no se realicen solicitudes HTTP en las fuentes que ya están instaladas.
* **Personaliza la representación y la carga de fuentes mediante `<link rel="preload">`, `font-display` o la API Font Loading:** un comportamiento de carga lenta de forma predeterminada puede demorar la representación de texto. Estas funciones de la plataforma web permiten anular este comportamiento para determinadas fuentes, y también especificar estrategias de representación y tiempo de espera personalizadas para diferentes contenidos de la página.
* **Especifica políticas de revalidación y almacenamiento en caché óptimo:** las fuentes son recursos estáticos que se actualizan con poca frecuencia. Asegúrate de que los servidores proporcionen una marca de tiempo de max-age prolongada y un token de revalidación para permitir la reutilización eficaz de fuentes en diferentes páginas. Si se utiliza service worker, corresponde emplear una estrategia en la que se priorice la caché.

## Comentarios {: #feedback }

{% include "web/_shared/helpful.html" %}

The following audits can help you make sure that your pages are continuing to follow web font optimization best practices over time:

* [Enable text compression](/web/tools/lighthouse/audits/text-compression)
* [Preload key requests](/web/tools/lighthouse/audits/preload)
* [Uses inefficient cache policy on static assets](/web/tools/lighthouse/audits/cache-policy)
* [All text remains visible during webfont loads](/web/updates/2016/02/font-display)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}