project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: A Payment Request API oferece pagamentos rápidos e fáceis na web.

{# wf_published_on: 2018-08-30 #} {# wf_updated_on: 2018-08-30 #}

# Guia de integração da Payment Request API {: .page-title }

{% include "web/_shared/translation-out-of-date.html" %}

<div class="video-wrapper-full-width">
  <iframe class="devsite-embedded-youtube-video" data-video-id="colCcgKoLUM"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

## Introdução à Payment Request API {: #introducing }

{% include "web/_shared/contributors/agektmr.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/zkoch.html" %}

Dofgood: o `PaymentRequest` ainda está em desenvolvimento. Embora acreditemos que ele é estável o suficiente para implementação, ainda pode continuar mudando. Vamos manter esta página atualizada para sempre refletir a situação atual da API([alterações no M56](https://docs.google.com/document/d/1I8ha1ySrPWhx80EB4CVPmThkD4ILFM017AfOA5gEFg4/edit#)). Enquanto isso, para se proteger contra as alterações da API que podem não ser retrocompatíveis, estamos oferecendo [um paliativo](https://storage.googleapis.com/prshim/v1/payment-shim.js) que pode ser incorporado ao seu site. O paliativo resolverá temporariamente todas as diferenças de API entre as duas principais versões do Chrome.

A compra de mercadorias on-line é uma experiência conveniente, mas, muitas vezes, frustrante, particularmente em dispositivos móveis. Embora o tráfego de dispositivos móveis continue a crescer, as conversões móveis respondem apenas por cerca de um terço de todas as compras concluídas. Em outras palavras, os usuários abandonam as compras móveis duas vezes mais que nas compras em desktops. Por quê?

**For merchants**, they make it easier to implement with a variety of payment options already filtered for the customer.

**For payment handlers**, they allow bringing any type of payment methods to the web with relatively easy integration.

Os formulários de compras on-line exigem muita interação do usuário, são difíceis de usar, lentos para carregar e atualizar e exigem muitas etapas até a conclusão. Isso acontece porque dois componentes essenciais dos pagamentos on-line &mdash; segurança e conveniência &mdash; muitas vezes têm objetivos contrários. Normalmente, mais de um deles significa menos do outro.

## Usar a Payment Request API {: #using }

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/standard-open.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Standard and Open</h3>
    Web Payments are an open payment standard for the web platform for the first time
    in history. They are available for any players to implement.</div>
</section>

<section style="display:flex;padding-bottom:32px;">
  <div style="min-width:50%">
    <h3>Easy and Consistent</h3>
    Web Payments make checkout easy for the user, by reusing stored 
payments and address information and removing the need for the user to fill in checkout forms. 
Since the UI is implemented by the browser natively, users see a familiar and consistent checkout 
experience on any website that makes use of the standard.</div>
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/easy-consistent.png" width="100%" alt="Standard and Open" title="">
  </div>
</section>

<section style="display:flex;background-color:#f7f7f7;padding-bottom:32px;">
  <div style="min-width:50%;padding-top:32px;">
    <img src="images/overview/secure-flexible.png" width="100%" alt="Standard and Open" title="">
  </div>
  <div style="min-width:50%">
    <h3>Secure and Flexible</h3>
    Web Payments provide industry-leading payment technology to the 
web, and can easily integrate a secure payment solution.</div>
</section>

## Como obter um endereço de envio {: #shipping-address }

A maioria dos problemas que leva ao abandono podem ser atribuídos diretamente aos formulários de compra. Cada aplicativo ou site tem seu próprio processo de entrada e validação de dados. Muitas vezes, os usuários descobrem que têm de inserir as mesmas informações em todos os pontos de compra dos aplicativos. Além disso, os desenvolvedores de aplicativos têm dificuldade para criar fluxos de compra que permitam vários métodos de pagamento únicos. Até mesmo pequenas diferenças nos requisitos de métodos de pagamento podem complicar o processo de preenchimento e envio do formulário.

Qualquer sistema que aprimore ou resolva um ou mais desses problemas será um mudança bem-vinda. Já começamos a resolver o problema com o [preenchimento automático](/web/updates/2015/06/checkout-faster-with-autofill). Mas agora queremos falar sobre uma solução mais abrangente.

A Payment Request API é um sistema que pretende *eliminar os formulários das compras*. Esse sistema melhora consideravelmente o fluxo de trabalho do usuário durante o processo de compra, oferecendo uma experiência do usuário mais consistente e permitindo que os comerciantes da Web usem facilmente métodos de pagamento heterogêneos. A Payment Request API não é um novo método de pagamento nem se integra diretamente a processadores de pagamento. Em vez disso, é uma camada de processo cujos objetivos são:

A Payment Request API é um padrão aberto para todos os navegadores da Web que substitui fluxos de conclusão de compra tradicionais, permitindo que os comerciantes solicitem e aceitem qualquer pagamento em uma única chamada de API. A Payment Request API permite que a página da web troque informações com o user-agent enquanto o usuário fornece informações, ou seja, antes da aprovação ou recusa de uma solicitação de pagamento.

O melhor de tudo é que, com o navegador atuando como intermediário, todas as informações necessárias para concluir rapidamente a compra podem ser armazenadas no navegador, permitindo que os usuários confirmem e paguem com um único clique.

## Adicionar opções de envio {: #shipping-options}

Com a Payment Request API, o processo da transação é o mais transparente possível para usuários e comerciantes.