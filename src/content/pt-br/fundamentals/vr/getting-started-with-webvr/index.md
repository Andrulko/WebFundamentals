project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Aprenda a criar uma cena WebGL com o Three.js e adicionar recursos da WebVR.

{# wf_updated_on: 2016-12-12 #} {# wf_published_on: 2016-12-12 #}

# Primeiros passos com a WebVR {: .page-title }

{% include "web/_shared/contributors/paullewis.html" %} {% include "web/_shared/contributors/mscales.html" %}

Aviso: a WebVR ainda é experimental e, por isso, está sujeita a mudanças.

Nesta guia, falaremos sobre as WebVR APIs e como usá-las para melhorar uma cena WebGL simples criada com o [Three.js](https://threejs.org/). Porém, para trabalho de produção, pode ser uma boa ideia começar com soluções que já existem, como o [Boilerplate do WebVR](https://github.com/borismus/webvr-boilerplate). Se for sua primeira vez com o Three.js, use este [guia prático de início](https://aerotwist.com/tutorials/getting-started-with-three-js/). A comunidade também ajuda bastante, então se você parar em um obstáculo, use a ajuda deles.

![WebGL Scene running on Chrome desktop](./img/desktop.jpg)

### Uma pequena observação sobre suporte

WebVR is available in Chrome 56+ behind a runtime flag. Enabling the flag (head to `chrome://flags` and search for "WebVR") will allow you to build and test your VR work locally. If you want to support WebVR for your visitors, you can opt into an [Origin Trial](https://github.com/GoogleChrome/OriginTrials/blob/gh-pages/developer-guide.md), which will allow you to have WebVR enabled for your origin.

A WebVR está disponível no Chrome 56+ por trás de um sinalizador de tempo de execução. Ativar o sinalizador (siga para `chrome://flags` e busque "WebVR") permitirá compilar e testar seu trabalho de RV localmente. Se quiser oferecer suporte a WebVR para seus visitantes, inscreva-se em um [Teste na origem](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md) para ter a WebVR disponível para a sua origem.

Você ainda pode usar o [polyfill da WebVR](https://github.com/googlevr/webvr-polyfill), mas lembre-se de que há queda de desempenho considerável quando se usa polyfills. Você com certeza deve testar nos dispositivos com que almeja trabalhar e evitar enviar algo que não acompanhe a taxa de atualização do dispositivo. Uma taxa de quadros variável ou ruim pode gerar desconforto significativo para as pessoas que usam a sua experiência!

## Obter acesso a exibições de RV

Para saber mais, dê uma olhada na página do [status da WebVR](../status/).

    navigator.getVRDisplays().then(displays => {
      // Filter down to devices that can present.
      displays = displays.filter(display => display.capabilities.canPresent);
    
      // If there are no devices available, quit out.
      if (displays.length === 0) {
        console.warn('No devices available able to present.');
        return;
      }
    
      // Store the first display we find. A more production-ready version should
      // allow the user to choose from their available displays.
      this._vr.display = displays[0];
      this._vr.display.depthNear = DemoVR.CAMERA_SETTINGS.near;
      this._vr.display.depthFar = DemoVR.CAMERA_SETTINGS.far;
    });
    

Então, com uma cena WebGL, o que preciso fazer para fazê-la trabalhar com a WebVR? Bem, em primeiro lugar, precisamos consultas o navegador para descobrir se há alguma exibição de RV disponível, o que podemos fazer com navigator.getVRDisplays().

1. **Nem todo dispositivo pode "apresentar" para um dispositivo de realidade virtual.** Há dispositivos que permitem, digamos, uso de acelerômetro ou uma pseudoexperiência de RV, mas não fazer uso de um HMD. Para esses dispositivos, o booleano canPresent será falso, e é algo que deve ser verificado.

2. **É possível que não haja dispositivos de RV disponíveis.** Devemos buscar criar experiências que funcionem muito bem em aparelhos que não oferecem RV e tratar a disponibilidade de RV como Progressive Enhancement.

3. **É possível que haja muitos dispositivos de RV disponíveis. **Igualmente, é perfeitamente possível que alguém tenha diversos dispositivos de RV disponíveis e, por isso, devemos, na medida do possível, deixar o usuário escolher o dispositivo mais adequado para ele.

## Instale uma extensão de emulação da WebVR do Chrome DevTools

Há alguns pontos que merecem atenção nesse código.

![Emulating WebVR with Jaume Elias's Chrome Extension](./img/webvr-emulation.jpg)

While it’s always preferable to test on real devices (especially for performance testing!) having this extension to hand can help you quickly debug during your builds.

## Solicite apresentação pelo dispositivo

Embora sempre seja melhor testar em dispositivos reais (especialmente para testes de desempenho), ter essa extensão em mãos pode ajudar a depurar rapidamente durante as compilações.

    this._vr.display.requestPresent([{
      source: this._renderer.domElement
    }]);
    

Para começar a apresentar em "modo RV", temos que solicitá-lo pelo dispositivo:

## Delineie sua cena de RV

`requestPresent` obtém uma matriz do que as [especificações da WebVR](https://w3c.github.io/webvr/#vrlayer) chamam de "VRLayers", que, na verdade, é um encapsulador de um elemento "Canvas" fornecido ao dispositivo de RV. No fragmento de código acima, pegamos o elemento "Canvas" — `WebGLRenderer.domElement` — fornecido por Three.js e o passamos como a propriedade de origem de uma única VRLayer. Em troca, `requestPresent` dará a você uma [Promessa](/web/fundamentals/getting-started/primers/promises), que se cumprirá se a solicitação for bem-sucedida, se não, será rejeitada.

![The WebVR scene running on a Pixel](../img/getting-started-with-webvr.jpg)

Firstly let’s talk about what we need to do.

* Garantir que usemos o retorno de chamada de `requestAnimationFrame` do dispositivo.
* Solicitar o "pose", a orientação e as informações dos olhos atuais pelo dispositivo de RV.
* Dividir nosso contexto WebGL em duas metades: uma para cada olho, e delineá-las.

Primeiro, vamos falar sobre o que precisamos fazer.

    _render () {
      // Use the VR display's in-built rAF (which can be a diff refresh rate to
      // the default browser one).  _update will call _render at the end.
    
      this._vr.display.requestAnimationFrame(this._update);
      …
    }
    

Por que precisamos usar um `requestAnimationFrame` diferente do fornecido com o objeto "window"? Porque estamos trabalhando com uma exibição cuja taxa de atualização pode ser diferente da máquina que a hospeda! Se o fone de ouvido tiver uma taxa de atualização de 120 Hz, precisaremos gerar quadros de acordo com essa taxa, mesmo que a máquina host atualize a tela a 60 Hz. A WebVR API considera isso fornecendo uma `requestAnimationFrame` API diferente para chamar. No caso de um dispositivo móvel, normalmente só há uma exibição (e atualmente no Android, a taxa de atualização é de 60 Hz) mas, mesmo assim, devemos usar a API correta para deixar nosso código preparado para o futuro e com a compatibilidade mais ampla possível.

    // Get all the latest data from the VR headset and dump it into frameData.
    this._vr.display.getFrameData(this._vr.frameData);
    

Em seguida, precisamos solicitar as informações sobre a posição da cabeça da pessoa, sua rotação e todas as outras informações necessárias para podermos fazer a delineação corretamente, o que fazemos com `getFrameData()`.

    this._vr.frameData = new VRFrameData();
    

`getFrameData()` obterá um objeto em que pode colocar as informações de que precisamos. Precisamos de um objeto `VRFrameData`, que podemos criar com `new VRFrameData()`.

* **timestamp**. A marcação de tempo da atualização do dispositivo. Esse valor começa em 0 na primeira vez que getFrameData é invocado na exibição em RV.

* **leftProjectionMatrix** e **rightProjectionMatrix**. Essas são as matrizes da câmera que consideram a perspectiva dos olhos na cena. Falaremos mais sobre elas em breve.

* **leftViewMatrix** e **rightViewMatrix**. Essas são outras duas matrizes que fornecem uma estimativa da localização de cada olho na cena.

Há muita informação interessante nos dados de quadro, vamos dar olhada rápida neles.

* **Matrizes de projeção.** São usadas para criar a impressão de perspectiva dentro da cena. Normalmente fazem isso distorcendo a escala dos objetos da cena à medida que se afastam dos olhos.

* **Matrizes de vista-modelo.** Usadas para posicionar um objeto no espaço 3D. Pela forma com que elas funcionam, você pode criar imagens de cena e concentrar-se nas imagens, multiplicando a matriz de cada nódulo e chegando à matriz de vista-modelo final para o objeto em questão.

Se você está começando com 3D, as matrizes de projeção e as matrizes de vista-modelo podem parecer assustadoras. Embora tenha um pouco de matemática por trás do que elas fazem, tecnicamente não precisamos saber exatamente como elas funcionam, saber que funcionam já está de bom tamanho.

## Assuma o controla da renderização da cena

Há muitos guias bons na web que explicam as matrizes de projeção e de vista-modelo com muito mais detalhes, por isso, dê uma buscada no Google se quiser ter mais conhecimento.

    // Make sure not to clear the renderer automatically, because we will need
    // to render it ourselves twice, once for each eye.
    this._renderer.autoClear = false;
    
    // Clear the canvas manually.
    this._renderer.clear();
    

Já que já temos as matrizes de que precisamos, vamos delinear a vista para o olho esquerdo. Para começar, precisaremos instruir o Three.js a não apagar o contexto WebGL sempre que chamarmos a renderização, senão precisaríamos delinear duas vezes e não queremos perder a imagem do olho esquerdo quando delinearmos para o direito.

    this._renderer.setViewport(
        0, // x
        0, // y
        window.innerWidth * 0.5,
        window.innerHeight);
    

Em seguida, vamos configurar o renderizador para só delinear a metade esquerda:

    const lViewMatrix = this._vr.frameData.leftViewMatrix;
    const lProjectionMatrix = this._vr.frameData.leftProjectionMatrix;
    
    // Update the scene and camera matrices.
    this._camera.projectionMatrix.fromArray(lProjectionMatrix);
    this._scene.matrix.fromArray(lViewMatrix);
    
    // Tell the scene to update (otherwise it will ignore the change of matrix).
    this._scene.updateMatrixWorld(true);
    this._renderer.render(this._scene, this._camera);
    

Esse código assume que o contexto GL é tela cheia (`window.inner*`), o que é muito bom para RV. Agora, podemos conectar as duas matrizes para o olho esquerdo.

* **Movemos tudo, menos a câmera.** Pode parecer um pouco esquisito se você nunca se deparou com isso antes, mas é comum nos trabalhos gráficos deixar a câmera na origem (0, 0, 0) e mover todo o resto. Sem querer virar filósofo agora, mas se eu me mover 10 metros à frente, foi eu quem se moveu 10 metros à frente ou o mundo se moveu 10 metros para trás? É uma questão de ponto de vista, e não importa muito de uma perspectiva matemática qual das duas possibilidades realmente ocorre. Como a WebVR API retorna o "*inverso da matriz de modelo do olho", esperamos aplicá-lo ao mundo (`this._scene` no código), não à câmera em si.

* **Devemos atualizar a matriz manualmente depois que alterarmos seus valores.** O Three.js armazena valores muito pesados em cache (o que é ótimo para o desempenho), mas isso significa que você *precisa* informá-lo de que algo mudou para ver as mudanças aplicadas. É possível fazer isso com o método `updateMatrixWorld()`, que assume um booleano para garantir que os cálculos se propaguem até a imagem da cena.

Tem alguns detalhes da implementação que são importantes.

    // Ensure that left eye calcs aren't going to interfere with right eye ones.
    this._renderer.clearDepth();
    this._renderer.setViewport(
        window.innerWidth * 0.5, // x
        0, // y
        window.innerWidth * 0.5,
        window.innerHeight);
    

Estamos quase lá! A última etapa é repetir o processo para o olho direito. Aqui, apagaremos os cálculos de profundidade do renderizador após delinearmos a vista para o olho esquerdo, já que não queremos que eles afetem a renderização da vista do olho direito. Em seguida, atualizamos a janela de visualização para o lado direito e delineamos a cena novamente.

    const rViewMatrix = this._vr.frameData.rightViewMatrix;
    const rProjectionMatrix = this._vr.frameData.rightProjectionMatrix;
    
    // Update the scene and camera matrices.
    this._camera.projectionMatrix.fromArray(rProjectionMatrix);
    this._scene.matrix.fromArray(rViewMatrix);
    
    // Tell the scene to update (otherwise it will ignore the change of matrix).
    this._scene.updateMatrixWorld(true);
    this._renderer.render(this._scene, this._camera);
    

Agora, podemos conectar as duas matrizes para o olho direito.

## Instrua o dispositivo a atualizar

E acabou! Na verdade, ainda não...

    // Call submitFrame to ensure that the device renders the latest image from
    // the WebGL context.
    this._vr.display.submitFrame();
    

Se você parar por aqui, perceberá que a exibição nunca se atualiza. Isso acontece porque podemos fazer várias renderizações ao contexto WebGL, e o HMD não sabe quando realmente deve atualizar sua própria exibição. Não é uma boa ideia atualizar depois, digamos, que a imagem de cada um dos olhos for renderizada. Por isso, assumimos o controle disso nós mesmos chamando submitFrame.

## Conclusões e materiais de apoio

Com esse código, *agora sim* acabou. Se quiser ver a versão final, não se esqueça de conferir o [repositório de exemplos do Google Chrome](https://github.com/GoogleChrome/samples/tree/gh-pages/web-vr/hello-world).

* **Crie em Progressive Enhancement desde o início.** Como mencionamos diversas vezes nesse guia, é importante criar uma boa base de experiência, sobre a qual você pode aplicar a WebVR. Muitas experiências podem ser implementadas com controle de mouse/toque e ser atualizadas por meio dos controles de acelerômetro, até se transformarem em experiências de RV totalmente desenvolvidas. Maximizar o público-alvo sempre vale a pena.

* **Lembre-se de que você vai renderizar a cena duas vezes.** Pode ser necessário pensar sobre o Nível de detalhe (LOD, na sigla em inglês) e outras técnicas para garantir que, quando renderizar a cena duas vezes, ela reduza a carga de trabalho para o CPU e a GPU. Acima de tudo, você deve manter uma taxa de quadros sólida! Nenhum malabarismo ou efeito é importante para alguém se sentindo extremamente desconfortável e enjoado!

* **Teste em um dispositivo real.** Esse ponto tem relação com o anterior. Você deve tentar obter dispositivos reais para testar o que está criando, principalmente se está pensando em trabalhar com dispositivos móveis. Como dizem por aí: ["seu notebook é um verdadeiro enganador"](https://youtu.be/4bZvq3nodf4?list=PLNYkxOF6rcIBTs2KPy1E6tIYaWoFcG3uj&t=405).

A WebVR é uma ferramenta realmente incrível para adicionar imersão ao conteúdo, e usar bibliotecas como o Three.js facilita muito o desenvolvimento com a WebGL. No entanto, alguns pontos importantes que devem ser lembrados.

* **[VRView](https://github.com/googlevr/vrview)**. Essa biblioteca ajuda a incorporar fotos e vídeos panorâmicos de 360º.

* **[Boilerplate da WebVR](https://github.com/borismus/webvr-boilerplate)**. Para começar a usar a WebVR e o Three.js

* **[Polyfill da WebVR](https://github.com/googlevr/webvr-polyfill)**. Para preencher as APIs necessárias para a WebVR. Lembre-se de que há quedas de desempenho quando se usa polyfills, então, embora eles forneçam funcionalidade, talvez seus usuários fiquem mais felizes com a sua experiência sem RV.

* **[Ray-Input](https://github.com/borismus/ray-input)**. Uma biblioteca para ajudar a gerenciar os vários tipos de interação para dispositivos de RV e outros, como por mouse, toque e controladores de jogo de RV.

Enquanto estamos aqui, há uma vastidão de recursos por aí para dar a você um início bem animador quando se trata de criar conteúdo para WebVR:

## Feedback {: #feedback }

Agora é só partir para o abraço e criar RVs insanas!