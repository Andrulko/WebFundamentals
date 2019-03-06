project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Uso do estilo adequado para aprimorar a acessibilidade

{# wf_updated_on: 2018-05-23 #} {# wf_published_on: 2016-10-04 #}

# Estilos acessíveis {: .page-title }

{% include "web/_shared/contributors/megginkearney.html" %} {% include "web/_shared/contributors/dgash.html" %} {% include "web/_shared/contributors/robdodson.html" %}

Exploramos dois dos pilares fundamentais da acessibilidade, foco e semântica. Agora vamos tratar do terceiro, estilo. É um tema amplo, que podemos cobrir em três seções.

- Garantir que os elementos sejam estilizados de modo a apoiar nossos esforços de acessibilidade adicionando estilos ao foco e vários estados ARIA.
- Estilizar nossas IUs para a flexibilidade de modo que possam ser ampliadas ou redimensionadas para acomodar usuários que podem ter problemas com texto pequeno.
- Escolher as cores e o contraste certos para evitar a transmissão de informações apenas pela cor.

## Estilização de foco

Geralmente, a qualquer momento que focamos um elemento, contamos com o anel integrado de foco do navegador anel (a propriedade CSS `outline`) para estilizar o elemento. O anel de foco é útil porque, sem ele, é impossível para um usuário de teclado dizer qual elemento tem o foco. A [lista de verificação WebAIM](http://webaim.org/standards/wcag/checklist){: .external } certifica isso, exigindo que "É visualmente aparente qual elemento da página tem o foco do teclado no momento (ou seja, à medida que percorre a página, você pode ver onde está)."

![elementos de formulário com um anel de foco](imgs/focus-ring.png)

No entanto, às vezes o anel de foco pode parecer distorcido ou simplesmente pode não caber no seu design da página. Alguns desenvolvedores removem este estilo completamente, definindo o `outline` do elemento como `0` ou `none`. Porém, sem um indicador de foco, como um usuário de teclado pode saber com qual item está interagindo?

Aviso: Nunca defina o contorno como 0 ou nenhum sem fornecer uma alternativa foco!

Você pode estar familiarizado com a adição de estados de passar cursor aos seus controles usando a *pseudoclasse* `:hover`. Por exemplo, você pode usar `:hover` em um elemento de link para alterar sua cor ou fundo quando o mouse está sobre ele. Similarmente a `:hover`, você pode usar a pseudoclasse `:focus` para direcionar um elemento quando ele tem foco.

    /* At a minimum you can add a focus style that matches your hover style */
    :hover, :focus {
      background: #c0ffee;
    }
    

Uma solução alternativa para o problema da remoção do anel de foco é dar ao seu elemento de os mesmos estilos em passar cursor e foco, o que resolve o problema "onde está o foco?" para usuários de teclado. Como sempre, melhorar a experiência de acessibilidade melhora a experiência de todos.

### Modalidade de interação

![um botão HTML nativo com um anel de foco](imgs/sign-up.png){: .attempt-right }

Para elementos nativos como `button`, os navegadores podem detectar se a interação do usuário ocorreu pelo mouse ou pressionando o teclado e, tipicamente exibe o anel de foco para interação por teclado. Por exemplo, quando você clica em um `button` nativo com o mouse, não há anel de foco, mas quando chega a pele percorrendo pelo teclado, o anel de foco aparece.

A lógica aqui é que usuários de mouse são menos propensos a precisar o anel de foco, porque sabem em que elemento clicaram. Infelizmente, atualmente não há uma única solução para todos os navegadores que produza esse mesmo comportamento. Como resultado, se você atribuir um estilo `:focus` a qualquer elemento, esse estilo será exibido quando o usuário clicar no elemento *ou* focá-lo com o teclado. Experimente clicar neste botão falso e observe que o estilo `:focus` é sempre aplicado.

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
    <fake-button tabindex="0">Click Me!</fake-button>
    

{% framebox height="80px" %}
<style>fake-button {display: inline-block;padding: 10px;border: 1px solid black;cursor: pointer;user-select: none;}fake-button:focus {outline: none;background: pink;}</style>
<fake-button tabindex="0">Click Me!</fake-button> {% endframebox %}

This can be a bit annoying, and often times developer will resort to using JavaScript with custom controls to help differentiate between mouse and keyboard focus.

Isso pode ser um pouco irritante, e muitas vezes o desenvolvedor recorre ao uso do JavaScript com controles personalizados para ajudar a diferenciar entre foco pelo mouse e pelo teclado.

No Firefox, a pseudoclasse CSS `:-moz-focusring` permite que você escreva um estilo de foco que só é aplicado quando o elemento é focado pelo teclado, um recurso bastante útil. Embora esta pseudoclasse seja suportada apenas no Firefox atualmente, [no momento há trabalho em andamento para transformá-la em um padrão](https://github.com/wicg/modality){: .external }.

## Estilização de estados com ARIA

Também há [este ótimo artigo de Alice Boxhall e Brian Kardell](https://www.oreilly.com/ideas/proposing-css-input-modality){: .external } que explora o tema da modalidade e contém protótipo de código para diferenciar entre interação por mouse e por teclado. Você pode usar a solução deles agora, e depois incluir a pseudoclasse do anel de foco mais tarde, quando ela tiver suporte mais generalizado.

Ao compilar componentes, é prática comum refletir seu estado e, assim, sua aparência, utilizando classes CSS controladas com JavaScript.

Por exemplo, considere um botão de alternância que vai para um estado visual "pressionado" quando clicado e mantém esse estado até ser clicado novamente. Para estilizar o estado, seu JavaScript pode adicionar uma classe `pressed` ao botão. E, como quer boa semântica em todos os seus controles, você também definiria o estado `aria-pressed` para o botão como `true`.

    .toggle.pressed { ... }
    

Uma técnica útil para empregar aqui é remover a classe totalmente e usar apenas os atributos ARIA para estilizar o elemento. Agora você pode atualizar o seletor CSS para o estado pressionado do botão deste

    .toggle[aria-pressed="true"] { ... }
    

para este.

## Design responsivo para vários dispositivos

Isso cria uma lógica e uma relação semântica entre o estado ARIA e a aparência do elemento, e também reduz o código extra.

Sabemos que é uma boa ideia fazer design de modo responsivo a fim de proporcionar a melhor experiência para vários dispositivos, mas o design responsivo também produz uma conquista em termos de acessibilidade.

![Udacity.com at 100% magnification](imgs/udacity.jpg)

A low-vision user who has difficulty reading small print might zoom in the page, perhaps as much as 400%. Because the site is designed responsively, the UI will rearrange itself for the "smaller viewport" (actually for the larger page), which is great for desktop users who require screen magnification and for mobile screen reader users as well. It's a win-win. Here's the same page magnified to 400%:

![Udacity.com at 400% magnification](imgs/udacity-zoomed.jpg)

In fact, just by designing responsively, we're meeting [rule 1.4.4 of the WebAIM checklist](http://webaim.org/standards/wcag/checklist#sc1.4.4){: .external }, which states that a page "...should be readable and functional when the text size is doubled."

Na verdade, apenas através do design responsivo, estamos cumprindo [regra 1.4.4 da lista de verificação do WebAIM](http://webaim.org/standards/wcag/checklist#sc1.4.4){: .external }, que afirma que uma página "... deve ser legível e funcional quando o tamanho do texto é dobrado".

- Primeiro, certifique-se de sempre usar a meta tag `viewport` adequada.  
    `<meta name="viewport" content="width=device-width, initial-scale=1.0">`   
    Configurar `width=device-width` se adaptará à largura da tela em pixels independentes de dispositivos, e configurar `initial-scale=1` estabelece uma relação 1:1 entre pixels CSS e pixels independentes de dispositivos. Fazê-lo instrui o navegador a ajustar seu conteúdo ao tamanho da tela, de modo que os usuários não vejam apenas um monte de texto sobreposto.

![a phone display without and with the viewport meta tag](imgs/scrunched-up.jpg)

Warning: When using the viewport meta tag, make sure you don't set maximum-scale=1 or set user-scaleable=no. Let users zoom if they need to!

- Outra técnica para ter em mente é fazer o design com uma grade responsiva. Como você viu no site Udacity, fazer design com uma grade significa que seu conteúdo sofrerá refluxo quando a página mudar de tamanho. Muitas vezes, estes layouts são produzidos usando unidades relativas como porcentagens, ems ou rems em vez de valores pixel rígidos. A vantagem de fazê-lo assim é que o texto e o conteúdo podem se ampliar e forçar outros itens para baixo na página. Assim, a ordem de DOM e a ordem de leitura permanecem iguais, mesmo se o layout mudar por causa da ampliação.

- Além disso, considere usar de unidades relativas como `em` ou `rem` para coisas como tamanho de texto, em vez de valores de pixel. Alguns navegadores suportam redimensionamento de texto somente nas preferências do usuário, e se você estiver usando um valor de pixel para o texto, esta configuração não afetará a sua cópia. Contudo, se tiver usado unidades relativas por toda parte, a cópia local será atualizada para refletir a preferência do usuário.

- Finalmente, quando seu design é exibido em um dispositivo móvel, você deve garantir que elementos interativos, como botões ou links são suficientemente grandes e têm espaço suficiente ao seu redor, para torná-los fáceis de pressionar sem sobreposição acidental com outros elementos. Isso beneficia todos os usuários, mas é especialmente útil para pessoas com deficiência motora.

Aviso: Ao usar a meta tag da janela de visualização, certifique-se de não definir maximum-scale=1 nem definir user-scaleable=no.

![a diagram showing a couple of 48 pixel touch targets](imgs/touch-target.jpg)

Touch targets should also be spaced about 8 pixels apart, both horizontally and vertically, so that a user's finger pressing on one tap target does not inadvertently touch another tap target.

## Cor e contraste

Alvos de toque também devem ter um espaço de cerca de 8 pixels entre si, tanto horizontal como verticalmente, de modo que o dedo de um usuário pressionando um alvo de toque não pressione outro alvo de toque sem querer.

Se você tem boa visão, é fácil supor que todos percebem as cores, ou a legibilidade do texto, da mesma forma que você &mdash; mas é claro que este não é o caso. Vamos concluir analisando como podemos efetivamente usar cor e contraste para criar designs agradáveis que sejam acessíveis a todos.

Como você pode imaginar, algumas combinações de cores que são de fácil leitura para algumas pessoas são difíceis ou impossíveis para outras. Isso geralmente se deve a *cor contraste *, a relação entre a *luminância* das cores em primeiro plano e do fundo. Quando as cores são semelhantes, a relação de contraste é baixa; quando elas são diferentes, a relação de contraste é alta.

![comparison of various contrast ratios](imgs/contrast-ratios.jpg)

The contrast ratio of 4.5:1 was chosen for level AA because it compensates for the loss in contrast sensitivity usually experienced by users with vision loss equivalent to approximately 20/40 vision. 20/40 is commonly reported as typical visual acuity of people at about age 80. For users with low vision impairments or color deficiencies, we can increase the contrast up to 7:1 for body text.

A relação de contraste de 4,5:1 foi escolhida como nível AA porque compensa a perda da sensibilidade ao contraste geralmente experimentada por usuários com perda de visão equivalente a cerca de 20/40 de visão. 20/40 geralmente é relatada como a acuidade visual típica de pessoas com aproximadamente 80 anos. Para usuários com dificuldades de visão subnormal ou deficiências de cor, podemos aumentar o contraste até 7:1 para o corpo de texto.

Você pode usar a [extensão Accessibility DevTools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb){: .external } para Chrome para identificar relações de contraste. Uma vantagem de usar Chrome DevTools é que elas sugerem alternativas AA e AAA (avançado) às suas cores atuais, e você pode clicar nos valores para visualizá-los no seu aplicativo.

1. Após instalar a extensão, clique em `Audits`
2. Desmarque tudo, exceto `Accessibility`
3. Clique em `Audit Present State`
4. Observe quaisquer avisos de contraste

![the devtools contrast audit dialog](imgs/contrast-audit.png)

WebAIM itself provides a handy [color contrast checker](http://webaim.org/resources/contrastchecker/){: .external } you can use to examine the contrast of any color pair.

### Não transmita informações somente pela cor

O próprio WebAIM fornece um [verificador de cor e contraste](http://webaim.org/resources/contrastchecker/){: .external } útil que você pode usar para examinar o contraste de qualquer par de cores.

Há cerca de 320 milhões de usuários com deficiência na visão de cores. Cerca de 1 em 12 homens e 1 em cada 200 mulheres têm alguma forma de "daltonismo"; isso significa que aproximadamente 1/20, ou 5%, de seus usuários não terão a experiência pretendida por seu site. Quando confiamos nas cores para transmitir informações, levamos esse número para níveis inaceitáveis.

Observação: O termo "daltonismo" muitas vezes é usado para descrever uma condição visual onde uma pessoa tem problema para distinguir cores; na verdade, muito poucas pessoas são totalmente cegas para cores. A maioria das pessoas com deficiências para cores podem ver algumas ou a maioria das cores, mas têm dificuldade em distinguir entre certas cores, como vermelhos e verdes (a mais comum), marrons e laranjas, e azuis e roxos.

![an input form with an error underlined in red](imgs/input-form1.png)

The [WebAIM checklist states in section 1.4.1](http://webaim.org/standards/wcag/checklist#sc1.4.1){: .external } that "color should not be used as the sole method of conveying content or distinguishing visual elements." It also notes that "color alone should not be used to distinguish links from surrounding text" unless they meet certain contrast requirements. Instead, the checklist recommends adding an additional indicator such as an underscore (using the CSS `text-decoration` property) to indicate when the link is active.

A [lista de verificação WebAIM afirma, na seção 1.4.1](http://webaim.org/standards/wcag/checklist#sc1.4.1){: .external } que "cor não deve ser utilizada como único método de transmissão de conteúdo de distinguir elementos visuais". Ela também observa que "a cor por si só não deve ser usada para distinguir links do texto ao redor", a menos que cumpra certas requisitos de contraste. Em vez disso, a lista de verificação recomenda adicionar um indicador adicional, como um sublinhado (usando a propriedade CSS `text-decoration`) para indicar quando o link está ativo.

![an input form with an added error message for clarity](imgs/input-form2.png)

When you're building an app, keep these sorts of things in mind and watch out for areas where you may be relying too heavily on color to convey important information.

Ao construir um aplicativo, tenha esse tipo de coisa em mente e atente para áreas onde possa estar dependendo demais da cor para transmitir informações importantes.

### Modo de alto contraste

Caso esteja curioso sobre como pessoas diferentes visualizam seu site, ou caso conte muito com o uso da cor em sua interface de usuário, você pode usar a [extensão NoCoffee do Chrome](https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl){: .external } para simular várias formas de deficiência visual, incluindo diferentes tipos de daltonismo.

O modo de alto contraste permite que um usuário inverta as cores do primeiro plano e do fundo, o que geralmente ajuda o texto a se destacar melhor. Para alguém com uma deficiência que causa baixa visão, o modo de alto contraste pode facilitar muito a navegação pelo conteúdo da página. Existem algumas maneiras de obter uma configuração de alto contraste em sua máquina.

Sistemas operacionais como o Mac OSX e Windows oferecem modos de alto contraste que podem ser ativados para tudo no nível do sistema. Ou os usuários podem instalar uma extensão, como a [extensão High Contrast do Chrome](https://chrome.google.com/webstore/detail/high-contrast/djcfdncoelnlbldjfhinnjlhdjlikmph){: .external } para ativar o alto contraste somente nesse aplicativo específico.

Um exercício útil é ativar as configurações de alto contraste e verificar se toda a IU no seu aplicativo ainda é visível e utilizável.

![a navigation bar in high contrast mode](imgs/tab-contrast.png)

Similarly, if you consider the example from the previous lesson, the red underline on the invalid phone number field might be displayed in a hard-to-distinguish blue-green color.

![a form with an error field in high contrast mode](imgs/high-contrast.jpg)

If you are meeting the contrast ratios covered in the previous lessons you should be fine when it comes to supporting high-contrast mode. But for added peace of mind, consider installing the Chrome High Contrast extension and giving your page a once-over just to check that everything works, and looks, as expected.

## Feedback {: #feedback }

Se está seguindo as relações de contraste citadas nas lições anteriores, você não precisa se preocupar quando se trata de suporte ao modo de alto contraste. Porém, para ficar mais tranquilo, considere instalar a extensão High Contrast do Chrome e dar uma olhada geral na sua página apenas para verificar se tudo funciona como esperado e tem a aparência desejada.