project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Segurança é um assunto importante: ter conhecimento sobre HTTPS, por que é importante e como é possível implementá-lo nos servidores.

{# wf_updated_on: 2016-09-09 #} {# wf_published_on: 2015-09-08 #}

# Segurança e identidade {: .page-title }

<div class="video-wrapper">
  <iframe class="devsite-embedded-youtube-video" data-video-id="pgBQn_z3zRE"
          data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
  </iframe>
</div>

Segurança é um assunto importante. Por isso, veja algumas coisas para começar.

<div class="clearfix"></div>

## Criptografia de dados em trânsito

<img src="/web/images/content-https-2x.jpg" class="attempt-right" />

Um dos recursos de segurança mais importantes, exigido por muitas APIs modernas e [aplicativos web progressivos](/web/progressive-web-apps/) é o [HTTP seguro, também chamado de HTTPS](encrypt-in-transit/why-https). Um equívoco frequente sobre o HTTPS é a crença de que os sites que precisam dele são somente os que lidam com comunicação sigilosa. Se privacidade e segurança não fossem motivos suficientes para proteger os usuários, muitos novos recursos dos navegadores, como os service workers e a Payment Request API, exigem HTTPS.

Some people mistakenly believe that the only sites that need HTTPS are sites that handle some level of sensitive communication, like personal or financial data. But this isn't true. Every site should be using HTTPS, HTTPS helps to prevents people from listening into what's crossing the wire, and helps prevent it from being tampered with while in transit. Do you want your ISP or school to know every site you were looking at?

{# wf_devsite_translation #}

[Enabling HTTPS on Your Servers](/web/fundamentals/security/encrypt-in-transit/enable-https)

<div class="attempt-left">
  <h2>Política de segurança de conteúdo</h2>
  <p>
    A Política de segurança de conteúdo, ou CSP, fornecem um amplo conjunto de diretivas que
   permitem controle granular sobre os recursos que uma página tem permissão para carregar
    e sobre o local de onde são carregados.<br>
    <a href="csp/">Saiba mais</a>
  </p>
</div>

<div class="attempt-right">
  <h2>Evitar conteúdo misto</h2>
  <p>
    Uma das tarefas mais demoradas na implementação do HTTPS é encontrar e
    tratar de conteúdo que misture HTTPs e HTTP. Felizmente, há ferramentas
    que ajudam nisso.<br>
    <a href="prevent-mixed-content/what-is-mixed-content">Primeiros passos</a>
  </p>
</div>

<div style="clear:both"></div>

## Materiais relacionados

* [Entendendo problemas de segurança](https://www.youtube.com/watch?v=tgEIo7ZSkbQ)
* [Getting the Green Lock: HTTPS Stories from the Field](https://www.youtube.com/watch?v=GoXgl9r0Kjk)

### Chrome DevTools

* [Understand Security Issues](/web/tools/chrome-devtools/security)

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}