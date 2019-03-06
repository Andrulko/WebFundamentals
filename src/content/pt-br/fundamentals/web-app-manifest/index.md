project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: O manifesto de um aplicativo web é um arquivo JSON que permite controlar como o aplicativo web ou site é exibido para o usuário em áreas que normalmente se espera ver aplicativos nativos (por exemplo, a tela inicial de um dispositivo), como definir o que o usuário pode inicializar e o visual durante a inicialização.

{# wf_updated_on: 2017-10-06 #} {# wf_published_on: 2016-02-11 #}

# O manifesto do aplicativo web {: .page-title }

{% include "web/_shared/translation-out-of-date.html" %}

{% include "web/_shared/contributors/mattgaunt.html" %} {% include "web/_shared/contributors/paulkinlan.html" %}

O [manifesto dos aplicativos web](https://developer.mozilla.org/en-US/docs/Web/Manifest) é um arquivo JSON que permite controlar como o aplicativo web ou site é exibido para o usuário em áreas que normalmente se espera ver aplicativos nativos (por exemplo, a tela inicial de um dispositivo), como definir o que o usuário pode inicializar e o visual durante a inicialização.

## Crie o manifesto

Manifestos de app da Web fornecem a capacidade de salvar um site marcado como favorito na tela inicial de um dispositivo. Quando um site é iniciado dessa maneira:

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
    

Ele faz tudo isso com um mecanismo simples de metadados em um arquivo de texto. Esse é o manifesto de app da Web.

## Informe o navegador da presença do seu manifesto

Observação: apesar de ser possível usar um manifesto de app da Web em qualquer site, ele é obrigatório para [Progressive Web Apps](/web/progressive-web-apps/).

    <link rel="manifest" href="/manifest.json">
    

## Defina um URL inicial

### TL;DR {: .hide-from-toc }

Antes de abordarmos os detalhes do manifesto de um aplicativo web, vamos criar um manifesto básico e vincular uma página da web a ele.

    "start_url": "/?utm_source=homescreen"
    

### Defina uma imagem e um título

Você pode chamar o manifesto como quiser. A maioria das pessoas usa o nome `manifest.json`. Veja um exemplo:

Não deixe de incluir o seguinte:

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
    

Quando criar o manifesto e ele estiver no seu site, adicione uma tag `link` a todas as páginas que envolve o seu aplicativo web da seguinte forma:

### Defina a cor do fundo

Se você não fornecer um `start_url`, a página atual será usada, o que provavelmente não é o que seus usuários querem. Mas esse não é o único motivo para incluir esse elemento. Como agora você pode definir como o seu aplicativo é inicializado, adicione um parâmetro de string de consulta ao `start_url` que indique como ele foi inicializado.

Isso pode funcionar como você quiser. O valor que estamos usando tem a vantagem de ser relevante para o Google Analytics.

    "background_color": "#2196F3",
    

Quando um usuário adiciona seu site à tela inicial dele, você pode definir um conjunto de ícones para o navegador usar. Você pode definir um tipo e um tamanho a eles da seguinte forma:

### Defina uma cor de tema

Observação: ao salvar um ícone na tela inicial, o Chrome primeiro procura ícones que correspondam à densidade da tela e que sejam dimensionados de acordo com a densidade de 48 dp. Se não encontrar nenhum, ele procura o ícone que mais se aproxima das características do dispositivo. Se, por qualquer motivo, você quiser escolher um ícone específico para uma determinada densidade de pixels, pode usar o membro `density` opcional, que assume um número. Se você não declara a densidade, assume-se o padrão 1,0. Isso significa: "use esse ícone para densidades de tela de 1,0 ou mais", o que, em geral, é o que se deseja.

### Personalize o tipo de exibição

Quando seu app da Web é iniciado pela tela inicial, acontecem diversas coisas nos bastidores:

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

Enquanto isso acontece, a tela fica branca e parece estar paralisada. Isso fica muito aparente quando se carrega a página da web pela rede, em que se leva mais de um ou dois segundos para que as páginas fiquem visíveis na página inicial.

### Especifique a orientação inicial da página

Para oferecer uma experiência melhor ao usuário, substitua a tela branca por um título, cores e imagens.

    "display": "browser"
    

### `scope` {: #scope }

Se você acompanhou tudo desde o início, já deve ter definido uma imagem e um título. O Chrome infere a imagem e o título de membros específicos do manifesto. O que importa é saber os detalhes específicos.

    "orientation": "landscape"
    

A imagem de uma tela de apresentação é delineada a partir da matriz `icons`. O Chrome escolhe a imagem mais próxima de 128 dp para o dispositivo. O título é simplesmente retirado do membro `name`.

Especifique a cor do fundo usando a propriedade `background_color` , cujo nome é bastante apropriado. O Chrome usa essa cor no exato momento em que o aplicativo web é inicializado, e a cor permanece na tela até a primeira renderização do aplicativo.

* Ele tem um ícone e um nome exclusivos para que os usuários possam distingui-los de outros sites.
* Mostra algo ao usuário enquanto os recursos são baixados ou restaurados do cache.
* Fornece características de exibição padrão ao navegador para evitar transição muito brusca quando os recursos do site ficam disponíveis.
* The `start_url` is relative to the path defined in the `scope` attribute.
* A `start_url` starting with `/` will always be the root of the origin.

### `theme_color` {: #theme-color }

Para definir a cor do fundo, aplique o seguinte ao seu manifesto:

    "theme_color": "#2196F3"
    

Agora, não aparecerá uma tela branca enquanto seu aplicativo é inicializado a partir da tela inicial.

Uma boa sugestão de valor para essa propriedade é a cor de fundo da página de carregamento. Usar as mesmas cores da página de carregamento proporciona uma transição suave da tela de apresentação para a página inicial.

## Personalize os ícones

<figure class="attempt-right">
  <img src="images/background-color.gif" alt="Ícone de adicionar à tela inicial">
  <figcaption>Ícone de adicionar à tela inicial</figcaption>
</figure>

Especifique uma cor de tema usando a prioridade `theme_color`. Essa propriedade define a cor da barra de ferramentas. Para esse elemento, também sugerimos duplicar uma cor existente, especificamente, a `theme-color` `<meta>`.

Use o manifesto de app da Web para controlar o tipo de exibição e a orientação da página.

* `name`
* `background_color`
* `icons`

Você pode fazer seu aplicativo web esconder a interface do navegador definindo o tipo `display` como `standalone`.

### Icons used for the splash screen {: #splash-screen-icons }

Se acha que os usuários prefeririam ver a página como um site normal no navegador, basta definir o tipo `display` como `browser`:

Você pode impor uma orientação específica, o que é vantajoso para aplicativos que funcionam em apenas uma orientação, como jogos, por exemplo. Use essa opção de forma seletiva. Os usuários preferem selecionar a orientação

<div class="clearfix"></div>

## Adicione uma tela de apresentação

O Chrome introduziu o conceito de uma cor de tema para seu site em 2014. A cor de tema é uma dica da sua página da Web que informa o navegador qual cor usar para os [elementos da IU, como a barra de endereços](/web/fundamentals/design-and-ux/browser-customization/).

<div class="clearfix"></div>

## Defina o estilo da inicialização

<figure class="attempt-right">
  <img src="images/devtools-manifest.png" alt="cor do fundo">
  <figcaption>Cor de fundo para a tela de inicialização</figcaption>
</figure>

Sem um manifesto, você precisará definir a cor de tema em cada página e, se tiver um site grande ou legado, não é viável fazer muitas alterações que englobem todo o site.

Adicione um atributo `theme_color` ao seu manifesto e, quando o site for iniciado pela tela inicial, todas as páginas do domínio terão a mesma cor de tema automaticamente.

Se quiser verificar manualmente se o manifesto do seu aplicativo web está configurado corretamente, acesse a guia **Manifest** no painel **Application** do Chrome DevTools.

## Forneça uma cor de tema para todo o site

* Um `short_name` para usar como o texto na tela inicial para os usuários.
* Um `name` para usar no banner de instalação de aplicativo web.
* If you want feature descriptions from the engineers who created web app manifests, you can read the [W3C Web App Manifest Spec](http://www.w3.org/TR/appmanifest/).

## Teste o manifesto {: #test }

{% include "web/_shared/helpful.html" %}