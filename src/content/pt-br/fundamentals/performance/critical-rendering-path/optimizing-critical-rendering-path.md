project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Conheça os principais fatores na otimização do caminho crítico de renderização.

{# wf_updated_on: 2015-10-05 #} {# wf_published_on: 2014-03-31 #}

# Otimização do caminho crítico de renderização {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Para obter a maior rapidez possível na primeira renderização, precisamos minimizar três variáveis:

- O número de recursos críticos.
- O tamanho do caminho crítico.
- O número de bytes críticos.

Um recurso crítico é aquele que pode bloquear a renderização inicial da página. Quanto menos desses recursos houver, menor será o trabalho do navegador, do CPU e de outros componentes.

De modo parecido, o tamanho do caminho crítico é uma função do gráfico de dependências entre os recursos críticos e seu tamanho em bytes: alguns downloads de recursos só podem ser iniciados depois que um recurso anterior já tiver sido processado, e quanto maior o recurso, mais idas e voltas são necessárias para baixá-lo.

Por fim, quanto menos bytes críticos o navegador precisar baixar, mais rápido poderá processar conteúdo e renderizá-lo na tela. Para reduzir o número de bytes, podemos diminuir o número de recursos (eliminá-los ou torná-los não críticos) e assegurar a redução do tamanho da transferência compactando e otimizando cada recurso.

**A sequência geral de etapas para otimizar o caminho crítico de renderização é:**

1. Analisar e caracterizar o caminho crítico: número de recursos, bytes e tamanho.
2. Minimizar o número de recursos críticos: eliminá-los, adiar o download, marcá-los como assíncronos etc.
3. Otimizar o número de bytes críticos para reduzir o tempo de download (número de idas e voltas).
4. Otimizar a ordem de carregamento dos recursos críticos restantes: baixar todos os ativos críticos o quanto antes para reduzir o tamanho do caminho crítico.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}