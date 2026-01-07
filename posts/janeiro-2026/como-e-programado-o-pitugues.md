# Como é programado o Pituguês?

Há poucos meses, apresentamos a vocês a linguagem Pituguês e como usar ele nesta [postagem do nosso blog](https://cumbuca.dev/2025/11/07/vem-com-a-gente-programar-em-portugues/). Mas lembram que também contamos que seu código-fonte é aberto? Isto significa que qualquer pessoa pode participar do desenvolvimento da linguagem, é o que chamamos de "Comunidade de Código Aberto (ou Open Source)".

### E como faço para participar da Comunidade de Código Aberto?

Basta se voluntariar para ajudar com algum projeto que você tenha interesse e buscar formas de contato com as pessoas que já estão envolvidas com o projeto, seja por fóruns, redes sociais, e-mails e etc.&#x20;

Nesse sentido o GitHub também cumpre um papel importante para projetos de código aberto, pois podemos fazer discussões por lá, escrever documentação dos projetos, fazer nossas contribuições, deixar organizado e registrado o que e como a comunidade pode ajudar e por aí vai...

As participações vão desde comentários, trazer suas dúvidas, sugestões, reportar imprevistos ao usar a linguagem, escrever e divulgar sobre ela ou, caso a pessoa se sinta a vontade, pode ajudar fazendo alterações no próprio código fonte da linguagem. Dentro do Código Aberto, cada pessoa pode colaborar com o pouquinho que lhe cabe e toda a contribuição é importante, válida e ajuda o projeto a crescer!

## Legal! E, agora, como contribuir com o Pituguês?

### Primeiramente, precisamos entender onde está o Pituguês!&#x20;

O Pituguês é uma das linguagens de programação que existem dentro de um projeto maior que é a linguagem Delégua. Como o código-fonte de Pituguês está em Delégua, podemos dizer que ele é, até o momento, mais um "dialeto de programação", pois ele acaba por ser uma variante de uma linguagem maior.&#x20;

Dialetos nada mais são do que variações de uma mesma língua e na programação essa lógica também se aplica. Pense em um país continental como o Brasil, dividido em 5 regiões e com 27 estados (incluindo o Distrito Federal). Embora existam discussões no campo da linguística se as variações do português no Brasil sejam dialetos ou apenas sotaques, é inegável que podemos notar diferenças bem acentuadas de expressões, palavras e entonações entre regiões diferentes ao ponto de um nortista ter dificuldade de entender a fala de um sulista e vice-versa. Esta pequena diferença linguística entre pessoas oriundas de diferentes regiões de um mesmo país, que geram um contratempo comunicacional, pode ser classificada como um dialeto.

E, talvez você não saiba, mas já tenha tido contato com dialetos de outras linguagens de programação! Podemos dizer que TypeScript seria um dialeto de JavaScript, por exemplo, uma vez que usa toda a sintaxe do JavaScript e adiciona a tipagem à linguagem.&#x20;

Outro exemplo interessante são os dialetos PL/SQL e PL/pgSQL que unem a lógica de programação procedural com linguagem de consulta. Se você for comparar, eles são bastante similares, mas ainda possuem diferenças em sua sintaxe que são bem características e que demarcam sua distinção.

Como comentado, o Pituguês, até o momento, ele é um dialeto de Delégua e podemos ver suas similaridades e diferenças na escrita do código:

#### Delégua

```
var idade = 15

se idade < 18 {
    escreva('Menoridade')
} senao {
    escreva('Maioridade')
}
```

#### Pituguês

```
idade = 15

se idade < 18:
    imprima('Menoridade')
senao:
    imprima('Maioridade')
```

Você pode notar que, sim, são linguagens bastante semelhantes, mas a principal diferença é que Pituguês vai utilizar a indentação para determinar os blocos de código, enquanto Delégua usa as chaves. Ou que Delégua exige a palavra-reservada "var" para declarar uma variável, ao passo que Pituguês a dispensa. Nesse exemplo, também podemos notar que a palavra-reservada para retornar um valor textual é diferente: Pituguês vai usar "imprima" e Delégua, "escreva".

### Onde fica o código-fonte de Pituguês e como ele é organizado?

Comentamos repetidas vezes que ele é um dialeto de Delégua e que vive dentro do código-fonte de Delégua. Você pode encontrar o [repositório do núcleo de Delégua disponível no Github](https://github.com/DesignLiquido/delegua) e, além disso, também está disponibilizado este [tutorial de como contribuir com o Pituguês](https://github.com/DesignLiquido/pitugues-docs/blob/principal/CONTRIBUTING.md).

Mas, antes, o que a gente precisa entender é como a linguagem tem seu código arquitetado e organizado! Lembrando que estamos programando uma linguagem de programação e que existem dois tipos principais de linguagens de programação: a compilada e a interpretada.

#### Linguagem Compilada

Geralmente, quando construímos um software, a pessoa desenvolvedora irá escrever o código do programa com o que chamamos de "linguagem de alto nível". Isto significa que se trata de uma linguagem de programação mais próxima à linguagem humana, de mais fácil leitura e entendimento para quem está lidando com aquele código.&#x20;

Por essa lógica, podemos presumir que "quanto mais baixo o nível, mais distante da linguagem humana"... O que é bem verdade!

Só que quando programamos algo, estamos criando programas para serem executados em máquinas e elas não falam a mesma língua que a gente! Elas só entendem linguagens de baixo nível, que vem a ser o que chamamos de "linguagem de máquina" ou "linguagem binária".&#x20;

É a famosa linguagem composta de zeros e uns, em que eles representam o estado da eletricidade - desligado ou ligado. Convenhamos que ela realmente não é muito acessível para se escrever um programa...&#x20;

O ponto é que, nas linguagens compiladas, a gente consegue escrever programas de uma forma que é acessível para pessoas, mas ainda não é para a máquina. Então, após o programa ser desenvolvido por inteiro, é aí que entra o papel do compilador que vai fazer no nosso código passar por um processo que, normalmente, é chamado de "build" (construir) e que irá traduzir o nosso código da linguagem humana para a linguagem de máquina. Só assim vamos conseguir executar o programa na máquina.&#x20;

E isso pode acontecer sucessivamente, pois cada vez que identificamos algum bug ou queremos atualizar ou editar algo no código, vamos ter que fazer todo esse processo de reconstruir o código para criar um arquivo executável dele que a máquina consiga rodar.&#x20;

Um detalhe bastante interessante é que as linguagens compiladas elas te ajudam a identificar erros de sintaxe na escrita do código, pois, se houver qualquer erro, elas vão impedir o processo de compilação - a construção do programa -, já que ele não conseguirá ser executado de jeito nenhum.

#### Linguagem Interpretada&#x20;

Já aqui, nas linguagens interpretadas, ainda que o programa precise ser traduzido para código de máquina, as instruções serão executadas ao mesmo tempo em que a linha de código é lida. É como se fosse uma tradução simultânea.&#x20;

As linguagens compiladas nos avisam quando encontram um erro que as impede de construir o programa. Já as linguagens interpretadas, quando o interpretador esbarra numa linha de código que possui um bug, ou alguma inconsistência, simplesmente quebram e interrompem o a execução.&#x20;

Por um lado, ela nos dá uma certa vantagem de que não precisamos compilar nosso código, reconstruindo o programa, toda vez que for necessário fazer alguma alteração nele. Ela acaba nos dando mais flexibilidade e agilidade na edição do algoritmo, embora sua execução possa ficar um pouco comprometida.

Outra diferença importante é que o interpretador pode tanto traduzir a linguagem de alto nível direto para a linguagem de máquina, como pode haver uma linguagem intermediária que esta irá traduzir para o código de máquina.&#x20;

#### Afinal, como é programado o Pituguês?

Bom, acabamos de ver, de forma resumida, quais são os dois principais de tipos de linguagens de programação. E isso é bastante importante para sabermos como programar a nossa linguagem!

Mas... qual delas é o Pituguês?   &#x20;

No [próprio código-fonte](https://github.com/DesignLiquido/delegua/tree/principal/fontes#o-que-esperar-de-cada-diret%C3%B3rio), é descrito como é organizada a arquitetura da linguagem, com os componentes: lexador, avaliador sintático e, finalmente, interpretador. Dessa forma, podemos presumir que ela é uma linguagem interpretada, uma vez que sua arquitetura possui um interpretador.

Cada um dos componentes mencionados é responsável por uma etapa do processo que faz com que as instruções escritas em Pituguês sejam traduzidas para o código de máquina, permitindo que um programa desenvolvido nesta linguagem possa ser efetivamente executado. Vamos entender um pouquinho deles...?

#### Lexador

A Design Líquido escolheu a palavra "Lexador" para traduzir o que normalmente é chamado de "Lexer" e/ou "Tokenizer", mas você ainda pode encontrar outras traduções que irão usar o termo "Analisador Léxico" e/ou "Tokenizador".

E qual é a função dele aqui?

Bom, pense que as linguagens de programação possuem símbolos e termos específicos que usamos para conseguirmos elaborar instruções e construir um programa para a finalidade que queremos. Todo esse arcabouço de recursos precisa ser mapeado e reconhecido como um item que faz parte daquela linguagem e é o Lexador que cumprirá este papel.

&#x20;Por exemplo, quando queremos declarar uma variável no Pituguês:

```
nome_da_linguagem = "Pituguês"
```

O Lexador vai percorrer cada caractere existente nesta linha de código e mapear cada elemento e sua devida função no código, então, teremos:

* "nome\_da\_linguagem": será reconhecido como o "identificador", o nome da variável;
* "=":  a maioria das pessoas nomeia este símbolo como "sinal de igual", mas no contexto da declaração de variável, o Lexador deverá identifica-lo como um "sinal de atribuição", pois após ele deverá ser escrito o valor que aquela variável conterá;
* "Pituguês": finalmente temos o valor que será guardado pela variável e, neste caso, como o Pituguês é capaz de identificar o tipo de dado por si mesmo, a própria linguagem deverá presumir que se trata de um tipo textual.

Ou seja, para o Pituguês aceitar a declaração de uma variável, o Lexador deve conseguir mapear cada um destes elementos, caso contrário, por ser uma linguagem interpretada, o programa irá interromper sua execução abruptamente, enquanto está em processo de execução. Esta mesma lógica é empregada para todas as outras palavras reservadas e símbolos que compõem a linguagem.&#x20;

#### Avaliador Sintático

O Avaliador Sintático é a parte do processo que irá fazer a "análise sintática", identificando se os símbolos estão dispostos na ordem adequada para serem reconhecidos enquanto sentenças que fazem parte da linguagem em questão. Ele vai receber do Lexador os símbolos mapeados e verificar se eles estão estruturados de uma maneira que faça sentido para que se possa executar as instruções do programa.

E por que essa etapa é tão importante?

Pense que o único propósito do Lexador é mapear e identificar os tipos de símbolos. Vamos supor que tentamos declarar uma variável escrita na seguinte maneira:

```
var nome_da_linguagem = "Pituguês"
```

Note que adicionamos a palavra 'var' antes do identificador da variável, 'nome\_da\_linguagem', o Lexador ainda consegue reconhecer e identificar os símbolos que existem nesta sequência textual, porém, o Avaliador Sintático vai impedir que esta linha de código seja executada, pois ela não será reconhecida como uma sentença válida. &#x20;

E o que faz o Avaliador Sintático reconhecer uma instrução possível ou não? A sua programação! É dentro deste componente estrutural da linguagem de programação que elaboramos e verificamos quais instruções serão válidas: nós programamos o Avaliador Sintático para que não reconheça "var" enquanto uma palavra reservada para declaração de variáveis.

Esse tipo de comportamento é equivalente ao que encontramos num idioma, por exemplo, ao ler a sentença "_Eu batata programo em Pituguês_". Note que temos uma palavra inserida que não faz sentido para a mensagem da frase. Podemos dizer que ela não está seguindo as regras gramaticais do português, uma vez que tempos uma palavra extra deslocada na frase.&#x20;

Agora, se escrevermos "_eu programo em Pituguês_", os termos estarão conectados e relacionados, tornando a mensagem compreensível! Da mesma forma acontece com linguagens de programação: **seguir suas regras gramaticais é essencial para que se possa construir um programa!**

#### Interpretador

Esta é a etapa da nossa arquitetura que será responsável pela execução do código! Como mencionamos antes, um interpretador é como se fosse uma tradução simultânea em que, conforme nosso interpretador vai lendo linha a linha, suas instruções vão sendo verificadas pelo Avaliador Sintáitco e, se estiverem na estrutura esperada da linguagem, serão executadas.&#x20;

Para que a execução seja efetuada, partimos do princípio em que nosso código passou pela aprovação do Lexador e do Avaliador Sintático e, agora, o Interpretador, também, precisará contribuir com a sua aprovação.&#x20;

Mas como isso é feito?

Ele irá receber as declarações que foram identificadas no Avaliador Sintático e que estão seguindo as regras gramaticais esperadas. Caso a gramática não esteja de acordo, é o momento em que a nossa "tradução simultânea" será interrompida abruptamente e a linguagem retornará um erro para nós.

Devemos lembrar que o Avaliador Sintático fez a verificação gramatical das instruções em Pituguês e, se ela corresponder às normas esperadas, irá gerar uma espécie de Árvore de Análise Sintática, também conhecida como Árvore de Derivação. É papel do Interpretador percorrer essa "árvore" e verificar qual instrução está sendo chamada para execução e, para desempenhar esta função, o Interpretador vai usar um "Design Pattern" conhecido como "Visitor Pattern", ou "padrão Visitante".

É com o padrão Visitante é que o interpretador poderá buscar e identificar a instrução escrita em Pituguês a partir de Árvore de Análise Sintática, no entanto, estes conceitos são um pouco mais complexos e sua aplicabilidade para o Pituguês será tratada mais futuramente.  Mas... caso você tenha interesse em já ir conhecendo um pouquinho mais deles, recomendamos a leitura do [artigo Visitor do refactoring.guru](https://refactoring.guru/pt-br/design-patterns/visitor) e o trecho de [Análise Sintática do ebook Compiladores para Humanos](https://johnidm.gitbooks.io/compiladores-para-humanos/content/part1/syntax-analysis.html).

Recomendamos fortemente que eassista [este vídeo](https://www.youtube.com/watch?v=PgKMZbUGFy8), produzido pela Design Líquido, demonstrando como cada etapa funciona e se complementa - Lexador, Avaliador Sintático e Interpretador. No vídeo, a demonstração é com a linguagem Delégua, mas a mesma estrutura é utilizada na programação do Pituguês e pode ficar mais claro para você ao ver cada etapa em ação!&#x20;

E, só pra reforçar, como vimos antes, nas linguagens interpretadas, elas podem traduzir nosso código diretamente para o código de máquina, ou podem ter um código intermediário que a traduz para o código de máquina.&#x20;

No caso do Pituguês, ela é uma linguagem de programação desenvolvida em TypeScript que passará direto para o JavaScript, que é uma linguagem de característica híbrida, o que chamamos de[ Just In Time Compilation (JIT](https://www.freecodecamp.org/portuguese/news/compilacao-just-in-time-explicada/)), para traduzir/transpilar o Pituguês para a linguagem de máquina e ser executado. O processo de transpilação do JavaScript se dá por uma Máquina Virtual, chamada de V8, para criar o código de máquina e rodar o programa.&#x20;

Ou seja, o Pituguês precisa de um intermediário para traduzir seu código em código de máquina e tornar possível a execução de um programa, percorrendo um caminho mais ou menos assim:

<figure><img src="../../.gitbook/assets/Pituguês - Linguagem Interpretada (2).png" alt=""><figcaption></figcaption></figure>

## Falando sobre o futuro...

Até a escrita deste artigo, como comentamos repetidamente, Pituguês é considerado um dialeto, uma vez que ele vive dentro do código-fonte de Delégua. Por ele se encontrar dependende do núcleo de outra linguagem, acaba por ter certas limitações no seu desenvolvimento, principalmente por ambas compartilharem do mesmo Interpretado&#x72;**.**&#x20;

**Acreditamos que para maior expansão do Pituguês, ele precise "caminhar com suas próprias pernas" e ser oficialmente uma linguagem!**

Gostaríamos de promover a independência do Pituguês para que, assim como em Python, a comunidade possa ter mais liberdade de contribuir e incrementar tudo aquilo que for benéfico para seus usuários! E, principalmente, sem a preocupação do quanto as contribuições possam afetar outro projeto.&#x20;

Inicialmente, queremos propor as discussões:

* Quais os primeiros passos para tornar o Pituguês independente?
* Como podemos projetar esta linguagem?
* Aliás, em qual linguagem de programação devemos programar o Pituguês? Continuaríamos com TypeScript ou seria mais interessante escolher outra e por quê?
* Materíamos a arquitetura de: Lexador, Avaliador Sintático e Interpretador?&#x20;

