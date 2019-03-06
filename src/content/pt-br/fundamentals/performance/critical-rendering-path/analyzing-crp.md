project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Saiba como identificar e resolver gargalos de desempenho do caminho crítico de renderização.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Análise do desempenho do caminho crítico de renderização {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Para identificar e resolver gargalos de desempenho no caminho crítico da renderização, é preciso ter bom nível de conhecimento sobre os obstáculos mais comuns. Vamos dar uma pincelada sobre o assunto e extrair padrões de desempenho comuns que ajudarão a otimizar suas páginas.

Ao otimizar o caminho crítico de renderização, o navegador pode colorir a página com a maior velocidade possível: páginas mais rápidas geram maior envolvimento, mais visualizações de páginas e [maior taxa de conversão](https://www.google.com/think/multiscreen/success.html). Para minimizar o tempo que um visitante perde olhando para uma tela em branco, precisamos definir os recursos a serem carregados e a ordem deles da maneira mais eficaz possível.

Para ajudar a ilustrar esse processo, vamos começar com o caso mais simples e ir montando a nossa página gradualmente para incluir outros recursos, estilos e lógica de aplicativo. No processo, otimizaremos todos os casos, e também vamos destacar os pontos em que podem acontecer erros.

Até aqui, trabalhamos exclusivamente com o que acontece no navegador depois que o recurso (arquivo CSS, JS ou HTML) fica disponível para processamento. Ignoramos o tempo necessário para buscar o recurso, esteja ele armazenado em cache ou na rede. Presumimos o seguinte:

* Uma ida e volta na rede (latência da propagação) até o servidor leva 100 ms.
* O tempo de resposta do servidor é 100 ms para documentos HTML e 10 ms para outros arquivos.

## A experiência do Hello World

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Experimente](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

Vamos começar com uma marcação HTML básica e uma única imagem, sem CSS e JavaScript. Vamos abrir a linha do tempo de "Network" no Chrome DevTools e dar uma olhada na cascata de recursos gerada:

<img src="images/waterfall-dom.png" alt="CRP" />

Observação: Embora esse documento use o DevTools para ilustrar conceitos do caminho crítico de renderização, hoje o DevTools não é a ferramenta mais indicada para análise de caminho crítico. Leia [O que o DevTools oferece?](measure-crp#devtools) para saber mais.

Como esperado, o download do arquivo HTML levou cerca de 200 ms. Note que a parte transparente da linha azul representa o tempo que o navegador espera na rede sem receber nenhum byte de resposta, onde a parte sólida mostra o tempo decorrido para a finalização do download depois do recebimento do primeiro byte de resposta. O download do HTML é bem pequeno (menos de 4 kb), então, só precisamos de uma única ida e volta para termos o arquivo inteiro. Como resultado, o documento HTML é obtido em cerca de 200 ms, com metade do tempo gasto ficando a cargo da espera da rede e a outra metade, à espera da resposta do servidor.<4K), so all we need is a single roundtrip to fetch the full file. As a result, the HTML document takes approximately 200ms to fetch, with half the time spent waiting on the network and the other half waiting on the server response.

Quando o conteúdo HTML é disponibilizado, o navegador analisa os bytes, converte-os em tokens e cria a árvore DOM. Observe que o DevTools informa o tempo do evento DOMContentLoaded na parte inferior (216 ms), que também corresponde à linha vertical azul. O intervalo entre a conclusão do download do HTML e a linha vertical azul (DOMContentLoaded) é o tempo de que o navegador precisa para criar a árvore DOM &mdash; nesse caso, apenas alguns milissegundos.

Observe que a nossa "awesome photo" não bloqueou o evento `domContentLoaded`. O fato é que podemos criar a árvore de renderização e até aplicar cor à página sem ter que esperar todos os ativos serem carregados: **nem todos os recursos são essenciais para se fornecer a primeira gravação rapidamente**. Na verdade, quando falamos do caminho crítico de renderização, normalmente estamos falando da marcação HTML, CSS e JavaScript. As imagens não bloqueiam a renderização inicial da página &mdash; apesar de podermos tentar colorir as imagens o quanto antes.

Nesse cenário, o evento `load` (também chamado de `onload`) é bloqueado na imagem: o DevTools relata o evento `onload` aos 335 ms. Não se esqueça de que o evento `onload` marca o ponto em que **todos os recursos** que a página requer foram baixados e processados. Nesse momento, o ícone de carregamento do navegador pode parar de girar (a linha vertical vermelha da cascata).

## Adicionar JavaScript e CSS à página

Nossa página "Hello World" parece simples, mas, por trás dos panos, não é bem assim. Na prática, precisamos de mais do que um simples HTML: provavelmente usaremos uma folha de estilo CSS e um ou mais scripts para acrescentar interatividade à página. Vamos adicionar ambos à nossa página e ver o que acontece:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_timing.html){: target="_blank" .external }

*Before adding JavaScript and CSS:*

<img src="images/waterfall-dom.png" alt="DOM CRP" />

*With JavaScript and CSS:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

O que aconteceu?

**E se substituirmos o script externo com um script em linha?** Mesmo que o script esteja embutido diretamente na página, o navegador não consegue executá-lo antes de o CSSOM ser criado. Resumindo, o JavaScript em linha também bloqueia o analisador.

* Ao contrário do exemplo com HTML simples, agora também foi preciso buscar e analisar o arquivo CSS para criar o CSSOM, e sabemos que precisamos do DOM e do CSSOM para criar a árvore de renderização.
* Como a página também contém um arquivo JavaScript que bloqueia o analisador, o evento `domContentLoaded` fica bloqueado até o arquivo CSS ser baixado e analisado: já que o JavaScript pode consultar o CSSOM, precisamos bloquear o arquivo CSS até que o download dele seja concluído antes de podermos executar o JavaScript.

Pensando nisso, apesar do bloqueio do CSS, será que embutir o script acelera a renderização da página? Vamos experimentar e ver o que acontece.

That said, despite blocking on CSS, does inlining the script make the page render faster? Let's try it and see what happens.

*External JavaScript:*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS" />

*Inlined JavaScript:*

<img src="images/waterfall-dom-css-js-inline.png" alt="DOM, CSSOM, and inlined JS" />

Primeiro, lembre-se de que todos os scripts em linha bloqueiam o analisador, mas podemos adicionar a palavra-chave "async" aos scripts externos para desbloqueá-lo. Vamos desfazer o JavaScript em linha e tentar o externo:

[Experimente](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_async.html){: target="_blank" .external }

*JavaScript assíncrono (externo):*

<img src="images/waterfall-dom-css-js.png" alt="DOM, CSSOM, JS assíncrono" />

*Async (external) JavaScript:*

<img src="images/waterfall-dom-css-js-async.png" alt="DOM, CSSOM, async JS" />

[Experimente](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/measure_crp_inlined.html){: target="_blank" .external }

Alternatively, we could have inlined both the CSS and JavaScript:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_timing.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Veja que o tempo de `domContentLoaded` é igual ao do exemplo anterior. Em vez de marcar o JavaScript como assíncrono, embutimos o CSS e o JS na própria página. Isso aumentou muito o tamanho da nossa página HTML, mas trouxe um ponto positivo: o navegador não precisa esperar para buscar recursos externos porque já está tudo na página.

<img src="images/waterfall-dom-css-inline-js-inline.png" alt="DOM, inline CSS, inline JS" />

Considerando tudo isso, vamos tentar voltar um pouco e identificar alguns padrões gerais de desempenho.

A página mais simples possível é composta apenas de marcação HTML: não tem CSS, JavaScript nem outro tipo de recurso. Para renderizar essa página, o navegador deve inicializar a solicitação, aguardar a chegada do documento HTML, analisá-lo, criar o DOM e finalmente renderizar o documento na tela:

[Experimente](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/basic_dom_nostyle.html){: target="_blank" .external }

## Padrões de desempenho

The simplest possible page consists of just the HTML markup; no CSS, no JavaScript, or other types of resources. To render this page the browser has to initiate the request, wait for the HTML document to arrive, parse it, build the DOM, and then finally render it on the screen:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_async.html" region_tag="full" adjust_indentation="auto" %}
</pre>

**O tempo entre T<sub>0</sub> e T<sub>1</sub> captura os tempos de processamento da rede e do servidor.** No melhor cenário (se o arquivo HTML for pequeno), basta uma ida e volta na rede para se obter o documento inteiro. Pela forma com que os protocolos de transporte TCP funcionam, é possível que arquivos maiores exijam mais idas e voltas. **Sendo assim, no melhor cenário, a página acima tem caminho crítico de renderização com uma ida e volta (no mínimo).**

<img src="images/analysis-dom.png" alt="Hello world CRP" />

[Experimente](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css.html){: target="_blank" .external }

Now, let's consider the same page but with an external CSS file:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/measure_crp_inlined.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Novamente, precisamos de uma ida e volta na rede para obter o documento HTML. A marcação obtida nos diz que precisaremos também do arquivo CSS. Isso significa que o navegador tem que voltar ao servidor e buscar o CSS antes de poder renderizar a página na tela. **Como consequência, essa página precisa de, pelo menos, duas idas e voltas antes de ser exibida.** Mais uma vez, o arquivo CSS pode exigir várias idas e voltas, por isso a ênfase em "no mínimo".

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

Agora, vamos comparar isso com as características de caminho crítico do exemplo HTML + CSS acima:

Let's define the vocabulary we use to describe the critical rendering path:

* **Recurso crítico:** recurso que pode parar a renderização inicial da página.
* **Tamanho do caminho crítico:** quantidade de idas e voltas ou o tempo total necessário para buscar todos os recursos críticos.
* **Bytes críticos:** total de bytes necessário para a primeira renderização da página, representados pela soma dos tamanhos dos arquivos de todos os recursos críticos transferidos. Nosso primeiro exemplo, em uma única página HTML, continha só um recurso crítico (o documento HTML). O tamanho do caminho crítico era igual a uma ida e volta na rede (considerando que o arquivo era pequeno) e o total de bytes críticos era simplesmente o tamanho do documento HTML transferido.

Precisamos tanto do HTML quanto do CSS para criar a árvore de renderização. Por isso, ambos são recursos críticos: o CSS é buscado somente depois que o navegador obtém o documento HTML. Sendo assim, o tamanho do caminho crítico é, no mínimo, duas idas e voltas. Ambos os recursos representam um total de 9 KB de bytes críticos.

<img src="images/analysis-dom-css.png" alt="DOM + CSSOM CRP" />

* **2** recursos críticos
* **2** idas e voltas ou mais como comprimento mínimo do caminho crítico
* **9** KB de bytes críticos

[Experimente](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

Adicionamos `app.js`, que é um ativo JavaScript externo da página e um recurso que bloqueia o analisador (ou seja, crítico) ao mesmo tempo. E, para piorar, para executar o arquivo JavaScript, temos que parar e esperar o CSSOM. Lembre-se de que o JavaScript pode consultar o CSSOM e, por isso, o navegador para até que `style.css` seja baixado e o CSSOM seja criado.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/basic_dom_nostyle.html" region_tag="full" adjust_indentation="auto" %}
</pre>

[Try it](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js.html){: target="_blank" .external }

Considerando tudo isso, na prática, se formos analisar a "cascata de rede" dessa página, veríamos que as solicitações CSS e JavaScript são iniciadas mais ou menos ao mesmo tempo, o navegador obtém o HTML, encontra os dois recursos e inicia as duas solicitações. Como resultado, a página acima tem as seguintes características de caminho crítico:

<img src="images/analysis-dom-css-js.png" alt="DOM, CSSOM, JavaScript CRP" />

Depois de conversar com os desenvolvedores do nosso site, percebemos que o JavaScript que incluímos na página não precisa bloquear o processo. Temos algumas análises e código que dispensam a necessidade de bloquear a renderização da página. Sabendo isso, podemos adicionar o atributo "async" à tag "script" para desbloquear o analisador:

* **3** recursos críticos
* **2** idas e voltas ou mais como comprimento mínimo do caminho crítico
* **11** KB de bytes críticos

[Experimente](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_js_async.html){: target="_blank" .external }

After chatting with our site developers, we realize that the JavaScript we included on our page doesn't need to be blocking; we have some analytics and other code in there that doesn't need to block the rendering of our page. With that knowledge, we can add the "async" attribute to the script tag to unblock the parser:

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Um script assíncrono tem diversas vantagens:

<img src="images/analysis-dom-css-js-async.png" alt="DOM, CSSOM, async JavaScript CRP" />

Por fim, se a folha de estilo CSS só for necessária para a impressão, como ficaria tudo isso?

* O script para de bloquear o analisador e não faz parte do caminho crítico de renderização.
* Como não há outros scripts críticos, o CSS não precisa parar o evento `domContentLoaded`.
* Quanto mais cedo o evento `domContentLoaded` for acionado, mais cedo será possível executar outra lógica do aplicativo.

[Experimente](https://googlesamples.github.io/web-fundamentals/fundamentals/performance/critical-rendering-path/analysis_with_css_nb_js_async.html){: target="_blank" .external }

Finally, if the CSS stylesheet were only needed for print, how would that look?

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/critical-rendering-path/_code/analysis_with_css_js.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Como o recurso style.css só é usado para impressão, o navegador não precisa parar nele para renderizar a página. Portanto, assim que a criação do DOM for concluída, o navegador terá as informações de que precisa para renderizar a página. Como resultado, essa página tem apenas um único recurso crítico (o documento HTML) e o comprimento mínimo do caminho crítico de renderização é uma ida e volta.

<img src="images/analysis-dom-css-nb-js-async.png" alt="DOM, non-blocking CSS, and async JavaScript CRP" />

{# wf_devsite_translation #}

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}