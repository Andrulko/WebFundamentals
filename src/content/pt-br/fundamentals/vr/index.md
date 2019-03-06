project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: WebVR

{# wf_updated_on: 2016-12-12 #} {# wf_published_on: 2016-12-12 #}

# WebVR {: .page-title }

Aviso: a WebVR ainda é experimental e, por isso, está sujeita a mudanças.

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="jT2mR9WzJ7Y"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

A WebVR é uma JavaScript API que se beneficia de qualquer fone de ouvido de RV e de um dispositivo compatível com RV dos usuários — como o [fone Daydream](https://vr.google.com/daydream/) e o celular Pixel — para criar experiências 3D totalmente imersivas no navegador.

<div class="clearfix"></div>

## Suporte e disponibilidade

Today the WebXR Device API is [under development](https://www.chromestatus.com/features/5680169905815552), but you can try it out with:

* Chrome Beta (M56+) com um [Teste na origem](https://github.com/jpchase/OriginTrials/blob/gh-pages/developer-guide.md).
* Firefox Nightly.

Hoje, a WebVR API está disponível no:

Para navegadores que não oferecem suporte à WebVR ou que talvez tenham versões antigas das APIs, você pode voltar para o [Polyfill da WebVR](https://github.com/googlevr/webvr-polyfill). Porém, não se esqueça de que a RV está *fortemente ligada a desempenho* e os polyfills normalmente têm custo de desempenho relativamente alto, por isso, pode ser uma boa ideia avaliar se você realmente quer usar o polyfill para um usuário que não tem suporte nativo à WebVR.

[Get the latest status on WebVR.](./status/)

## Criar conteúdo para a WebVR

To make WebVR content, you will need to make use of some brand new APIs, as well as existing technologies like [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/Tutorial) and [Web Audio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API), as well as accounting for different input types and headsets.

<div class="attempt-left">
  <h3>Get Started with WebVR</h3>
  <a href="./getting-started-with-webvr/">
    <img src="img/getting-started-with-webvr.jpg" alt="Get started with WebVR" />
  </a>
  <p>
    Make a flying start with WebVR by taking a WebGL scene and adding VR APIs.<br>
    <a href="./getting-started-with-webvr/">Learn More</a>
  </p>
</div>

<div class="attempt-right">
  <h3>Add Input to a WebVR Scene</h3>
  <a href="./adding-input-to-a-webvr-scene/">
    <img src="img/adding-input-to-a-webvr-scene.jpg" alt="Add input to a WebVR scene" />
  </a>
  <p>
    Interaction is a crucial part of providing an engaging and immersive experience.<br>
    <a href="./adding-input-to-a-webvr-scene/">Get Started</a>
  </p>
</div>

<div class="clearfix"></div>

### Mais materiais

Para criar conteúdo para a WebVR, você precisa fazer uso de algumas APIs novas e de tecnologias já estabelecidas, como a [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/Tutorial) e a [Web Audio](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API), além de levar em consideração diversos tipos de interação e fones de ouvido.

* [Conheça as WebVR APIs](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API)
* [Veja os exemplos da WebVR](https://webvr.info/samples/)
* [Como projetar para o Google Cardboard](https://www.google.com/design/spec-vr/designing-for-google-cardboard/a-new-dimension.html)

## Controle o seu desempenho

<img src="img/oce.png" class="attempt-right" alt="WebVR Performance" />

In order to minimize discomfort for the people using WebVR experiences, they must maintain a consistent (and high) frame rate. Failing to do so can give users motion sickness!

Para minimizar o desconforto das pessoas que usufruem de experiências da WebVR, é preciso manter a taxa de quadros constante (e alta). Sem isso, os usuários podem ficar enjoados com os movimentos.

Em dispositivos móveis, a taxa de atualização é normalmente de 60 Hz, o que significa que a meta é 60 fps (ou 16 ms por quadro *incluindo* a sobrecarga do navegador por quadro). Nos computadores, a meta normalmente é 90 Hz (11 ms incluindo a sobrecarga).

## Receba o Progressive Enhancement de braços abertos

<img src="img/touch-input.png" class="attempt-right"
  alt="Use Progressive Enhancement to maximize reach" />

What are you to do if your users don’t have a Head Mounted Display (‘HMD’) or VR-capable device? The best answer is to use Progressive Enhancement.

1. Presuma que o usuário esteja usando dispositivo de entrada tradicional, como teclado, mouse ou tela tátil, e não tenha acesso a um fone de ouvido de RV.
2. Adapte a mudanças na interação e na disponibilidade de fones de ouvido em tempo de execução.

O que você faz se os usuários não tiverem um dispositivo de realidade virtual para a cabeça (HMD) ou dispositivo compatível com RV? A melhor resposta é: use o Progressive Enhancement.

Graças às [WebVR APIs](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API), podemos detectar mudanças no ambiente de RV para descobrir e adaptar-nos a mudanças na interação e nas opções de visualização no dispositivo do usuário.

Presumindo um ambiente sem RV primeiro, é possível maximizar o alcance das suas experiências e garantir que você ofereça a melhor experiência possível, seja qual for o equipamento usado pelos usuários.

## Feedback {: #feedback }

Para saber mais, leia nosso guia sobre [como adicionar interação a uma cena da WebVR](./adding-input-to-a-webvr-scene/).