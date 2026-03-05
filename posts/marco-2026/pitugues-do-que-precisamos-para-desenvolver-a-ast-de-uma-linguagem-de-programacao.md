# Pituguês - Do que precisamos para desenvolver a AST de uma linguagem de programação?

Depois de tratarmos sobre o Lexador, vamos avançar para a próxima etapa do processo que torna possível transformar uma linguagem de alto nível em linguagem de máquina, permitindo o desenvolvimento de uma linguagem de programação: o Avaliador Sintático!&#x20;

Este termo foi a escolha feita pela Design Líquido como tradução de um `parser`, mas também podemos encontrar por aí outros nomes equivalentes como AST Walker, AST Evaluator ou até mesmo Analisador Sintático.&#x20;

### Retomando alguns conceitos...

Lembram que o Lexador gera uma lista de símbolos (tokens) a partir das instruções que escrevemos no código-fonte do nosso programa? Tomemos o mesmo exemplo do artigo anterior, ao declararmos a variável:

```
nome_da_linguagem = "Pituguês"
```

O Lexador irá mapear cada elemento que contém nesta linha de código, retornando um vetor (array) de objetos `Símbolo`, em que é identificado o seu `tipo` e  `lexema`. Estas informação são imprediscíveis para que o Avaliador Sintático possa dar continuidade da tradução da linguagem de alto nível para a de baixo nível e, assim, executar o programa que escrevemos.

```
simbolos: [
    Simbolo {
      tipo: 'IDENTIFICADOR',
      lexema: 'nome_da_linguagem',
      literal: null,
      linha: 1,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'IGUAL',
      lexema: '=',
      literal: null,
      linha: 1,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'TEXTO',
      lexema: 'Pituguês',
      literal: 'Pituguês',
      linha: 1,
      hashArquivo: undefined
    }
 ]
```

Mas escrever código e ter seus símbolos identificados por um Lexador não é o suficiente para que se possa tornar uma linguagem de programação executável, é a apenas a primeira parte do processo. Tomando como exemplo o Pituguês, imagine que o Lexador foi capaz de identificar a declaração de variável da seguinte linha de código:

```
var nome_da_linguagem = "Pituguês"
```

Nessa linha de código, o Lexador identificou que há dois símbolos que foram categorizados como `identificadores`, como nomes de variáveis, a palavra `var` e a palavra `nome_da_linguagem`. Até aí tudo bem, porque é a função dele apenas mapear os símbolos e seu tipos.

Mas quando tentamos executar este código...

<figure><img src="../../.gitbook/assets/Captura de tela 2026-01-22 145908.png" alt=""><figcaption></figcaption></figure>

A palavra `var` não é reconhecida pela linguagem por não fazer para de sua gramática e por isto temos retornado o erro acima que nos impede de executar o código. Afinal, assim como um idioma que segue uma estrutura sintática para gerar conexão entre palavras e transmitirmos uma mensagem, as linguagens de programação funcionam da mesma forma.

Os símbolos que compõem uma instrução de um código precisam o corresponder a uma ordem para serem reconhecidos dentro da regra gramatical da linguagem. Então, por exemplo, não podemos declarar uma variável assim:&#x20;

```
nome_da_linguagem "Pituguês" =
```

Mesmo que o Lexador consiga identificar cada um dos termos que são usados para escrever uma variável, eles estão desalinhados com o que se espera de uma declaração de variáveis e é o Avaliador Sintático que vai identificar isto e nos alertar quando esta incosistência ocorrer.

## Como o Avaliador Sintático avalia os tokens do Lexador?

Como foi comentado antes, o Lexador gera uma lista dos símbolos que existem no código escrito e ele vai enviar essa lista para o Avaliador Sintático. A partir do momento em que o Avaliador Sintático tem acesso a esta lista de símbolos com seus devidos tipos identificados, ele terá ciência sobre o significado daquele símbolo. Isto é de suma importância pois é como o Avaliador conseguirá determinar a função daquela comando no programa.&#x20;

Esta etapa é como se fosse uma análise sintática de uma oração mesmo, um momento em que iremos identificar a classe e função de cada palavra em uma frase, como:

<figure><img src="../../.gitbook/assets/Eu programo em Pituguês..png" alt=""><figcaption></figcaption></figure>

Perceba que cada palavra está ordenada de uma forma que faça sentindo para transmitir sua mensagem na língua portuguesa. Cada palavra pertence a uma classificação gramatical que possui uma função específica que determina seu propósito dentro da frase.&#x20;

Usando o exemplo da frase acima, é como se o Lexador identificasse que existe um pronome, um verbo, uma preposição e um substantivo, nessa respectiva ordem. O Avaliador Sintático vai, agora, identificar qual é a função desses elementos e verificar se eles estão ordenados seguindo o padrão de "SUJEITO + VERBO + OBJETO".&#x20;

A nossa linha de código vai sofrer um processo parecido de identificação quanto a função dela naquele programa. Mas, desta vez, irá analisar se há elementos como variáveis, condicionais, laços de repetição e etc. Tomemos como exemplo a declaração de variável que já usamos antes do Pituguês:

```
nome_da_linguagem = "Pituguês"
```

Quando o Avaliador recebe esta linha de código, terá sido mapeado que ali temos um identificador, um sinal de atribuição de valor e, por último, temos o valor, um dado de tipo textual. Após receber esta sequência de caracteres com seus significado, será analisado se aquela linha de código está na ordem correta para que seja considerada uma declaração variável. Isto será processado dentro desta função (você pode encontrá-la [aqui](https://github.com/DesignLiquido/delegua/blob/principal/fontes/avaliador-sintatico/dialetos/avaliador-sintatico-pitugues.ts)):

```
private async declaracaoImplicitaVariaveis(): Promise<Var> {
    const identificador = this.consumir(
        tiposDeSimbolos.IDENTIFICADOR,
        'Esperado nome de variável.'
    );

    this.consumir(tiposDeSimbolos.IGUAL, "Esperado '=' após identificador.");

    if (this.estaNoFinal()) {
        throw this.erro(
            this.simboloAnterior(),
            'Esperado valor após o símbolo de igual.'
        )
    }

    const valor = await this.expressao();
    const tipo = this.logicaComumInferenciaTiposVariaveisEConstantes(valor, 'qualquer');

    this.pilhaEscopos.definirInformacoesVariavel(
        identificador.lexema,
        new InformacaoElementoSintatico(identificador.lexema, tipo)
    );

    return new Var(identificador, valor, tipo);
}
```

Nesta função, em código TypeScript, sua assinatura espera que seja construído um objeto do tipo `Var`, para criarmos uma variável, após o avaliador receber aquela linha de código. Só que esta variável só será construída após passar pelas validações na função acima:

* `const identificador`: irá armazenar o identificador a nossa variável;
* É validado se existe o sinal de igual para atribuição de valor;
* Verifica se há algum valor após o sinal de atribuição;
* `const valor`: é quando o código vai verificar qual é o dado que queremos armazenar na variável;
* `const tipo`: a partir do valor, identifica o tipo do dado;
* Adiciona à pilha de escopo do programa a variável;
* Por fim, a função retorna um objeto `Var`, com as constantes e seus valores definidos na função: `identificador`, `valor` e `tipo`.

Ou seja, apenas após passar por todo esse processamento e validações é que o Pituguês conseguiu gerar uma variável para aquela linha de código.&#x20;

### Composição de um programa

Até aqui, conseguimos entender como o Avaliador Sintático é capaz de reconhecer uma variável, só que é preciso lembrar que um programa não consiste em apenas um elemento. Geralmente, acabamos usando uma série de construções como condicionais, funções, laços de repetição, criação de classes, enfim, temos uma ampla gama de comandos que podemos elaborar num programa.&#x20;

Todavia, além do processo de identificar as instruções do código, a função do Avaliador Sintático também é construir uma forma de representação hierárquica dele, conhecida por AST (Abstract Syntax Tree, ou Árvore de Sintaxe Abstrata). E é essa representação que, mais a frente, permitirá que nosso Interpretador percorra e execute o programa desenvolvido.&#x20;

Mas, antes, vamos entender o que é uma...

#### Estrutura de Dados em Árvore

Quando nos referimos ao tema de estrutura de dados, lidamos com formas de armazenamento de dados que são lineares por terem um início e um fim, como vetores, pilhas e filas. A árvore também é uma estrutura de dados, porém, ela não opera de forma linear como as outras mencionadas, mas de maneira hierárquica e permite um crescimento contínuo de dados.

Imagine que você elaborou uma lista de filmes que quer baixar para assistir mais tarde. Pensando em estrutura de dados, você pode guardar essa lista num vetor, como, por exemplo:

```
lista_de_filmes = [
 "Um conto chinês",
 "Nunca deixe de lembrar",
 "Nosferatu",
 "O enigma de Kaspar Hauser".
 "Cidade de Deus",
 "Minha mãe é uma peça",
 "Bacurau",
 "Parasita",
 "Invasão Zumbi"
]
```

Em um vetor, conseguimos ter essa visão mais panorâmica do que há na nossa lista e selecionar um item pela sua posição dentro do vetor, simplesmente por percorrer a estrutura com um laço de repetição, por exemplo. E até aqui tudo bem, porque a intenção era apenas de ter uma listagem dos filmes que se pretendia assistir.

Agora, você já baixou os filmes e criou uma pasta chamada `FILMES` no seu computador para deixar eles guardadinhos ali dentro, mas isso te deixou incomodado porque só colocar os arquivos ali não ficou exatamente organizado. Então, que tal organizar os filmes de acordo com seu país de lançamento?&#x20;

Podemos ter uma organização em que iremos aninhar os filmes de acordo com seu país e ela ficaria mais ou mesmo assim...

```
├── FILMES
│ ├── Argentina
│ │ ├── Um conto chinês
│ ├── Alemanha
│ │ ├── Nunca deixe de lembrar
│ │ ├── Nosferatu
│ │ ├── O enigma de Kaspar Hauser
│ ├── Brasil
│ │ ├── Cidade de Deus
│ │ ├── Minha mãe é uma peça
│ │ ├── Bacurau
│ ├── Coreia do Sul
│ │ ├── Parasita
│ │ ├── Invasão Zumbi
```

É só pensar num sistema de arquivos no seu computador de forma simplificada: depois de dividir os filmes em diretórios de acordo com seu país, você não vai mais conseguir acessar o filme da mesma forma que conseguia como no vetor de antes.&#x20;

Agora, se quiser assistir o filme "Cidade de Deus", vai precisar navegar pelo caminho de:

* Selecionar o diretório <kbd>FILMES</kbd>
* Depois o diretório `Brasil`
* E, por fim, o filme "`Cidade de Deus`"&#x20;

Ou seja, a partir do momento que eu quero ter acesso aos filmes que estão na pasta `Brasil`, eu já não tenho acesso visível aos filmes que estão aninhanados em outros países. Então, se eu estiver ainda dentro de Brasil, caso eu mude de ideia e queira assistir um filme da Argentina, eu vou precisar retroceder um diretório para poder entrar em outro, isto porque cada um do filmes está organizado na pasta do seu respectivo país.&#x20;

É mais ou menos assim que uma estrutura de dados em árvore se comporta. Você só consegue ter acesso àquele dado/informação se estiver "dentro do guarda-chuva dele". É justamente por isso que não haverá linearidade em uma árvore, pois os dados estarão organizados, de novo, hierarquicamente.

Se você quiser ler mais um pouquinho sobre as árvores, recomendamos este [artigo](https://www.freecodecamp.org/portuguese/news/tudo-o-que-voce-precisa-saber-sobre-estruturas-de-dados-em-arvore/).

#### &#x20;A AST do Avaliador Sintático

Para que nosso programa em Pituguês seja executado o Avaliador Sintático não irá apenas identificar a função de cada trecho do código, mais vai gerar uma hirarquia em árvore dele. Vamos usar como exemplo o seguinte código:

<pre><code>letra = "b"
<strong>se letra == "a":
</strong>    escreva("Letra A")
senao:
    "escreva("Não é letra A")
</code></pre>

Como comentamos, o Lexador irá mapear cada token presente em cada linha, retornando algo assim:

```
simbolos: [
    Simbolo {
      tipo: 'IDENTIFICADOR',
      lexema: 'letra',
      literal: null,
      linha: 1,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'IGUAL',
      lexema: '=',
      literal: null,
      linha: 1,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'TEXTO',
      lexema: 'b',
      literal: 'b',
      linha: 1,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'SE',
      lexema: 'se',
      literal: null,
      linha: 2,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'IDENTIFICADOR',
      lexema: 'letra',
      literal: null,
      linha: 2,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'IGUAL_IGUAL',
      lexema: '=',
      literal: null,
      linha: 2,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'TEXTO',
      lexema: 'a',
      literal: 'a',
      linha: 2,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'DOIS_PONTOS',
      lexema: '',
      literal: null,
      linha: 2,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'ESCREVA',
      lexema: 'escreva',
      literal: null,
      linha: 3,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'PARENTESE_ESQUERDO',
      lexema: '',
      literal: null,
      linha: 3,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'TEXTO',
      lexema: 'Letra A',
      literal: 'Letra A',
      linha: 3,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'PARENTESE_DIREITO',
      lexema: '',
      literal: null,
      linha: 3,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'SENAO',
      lexema: 'senao',
      literal: null,
      linha: 4,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'DOIS_PONTOS',
      lexema: '',
      literal: null,
      linha: 4,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'TEXTO',
      lexema: 'escreva(',
      literal: 'escreva(',
      linha: 5,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'IDENTIFICADOR',
      lexema: 'Não',
      literal: null,
      linha: 5,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'IDENTIFICADOR',
      lexema: 'é',
      literal: null,
      linha: 5,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'IDENTIFICADOR',
      lexema: 'letra',
      literal: null,
      linha: 5,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'IDENTIFICADOR',
      lexema: 'A',
      literal: null,
      linha: 5,
      hashArquivo: undefined
    },
    Simbolo {
      tipo: 'TEXTO',
      lexema: ')',
      literal: ')',
      linha: 5,
      hashArquivo: undefined
    }
  ]
```

Já o Avaliador Sintático, por sua vez, vai nos retornar algo mais enxuto:

```
declaracoes: [
    Var {
      linha: 1,
      hashArquivo: undefined,
      decoradores: [],
      assinaturaMetodo: '<principal>',
      simbolo: [Simbolo],
      inicializador: [Literal],
      tipo: 'texto',
      tipoExplicito: false,
      referencia: false,
      desestruturacao: false
    },
    Se {
      linha: 2,
      hashArquivo: 0,
      decoradores: [],
      assinaturaMetodo: '<principal>',
      condicao: [Binario],
      caminhoEntao: [Bloco],
      caminhosSeSenao: [],
      caminhoSenao: [Bloco]
    }
  ]
```

Percebe a diferença?&#x20;

Temos como resultado uma lista das declarações que foram feitas no nosso código: sabemos que existe uma declaração de variável e uma declaração da condicional `se`, mas não sabemos o que tem dentro de cada uma.&#x20;

É como se os ramos de uma árvore fossem vistos de longe, ou seja, podemos dizer que o retorno do Avaliador Sintático nos permite ter uma visão da ramificação do código em questão, conseguimos perceber como o código é organizado e hierarquizado.&#x20;

Neste exemplo, sabemos que a condicional não se limita ao o que é mostrado neste retorno, mas que ela se extende em mais linhas de código, só que, aqui, não temos acesso a esse detalhe. De toda forma, essa AST que o Avaliador Sintático gera vai ser usada pelo Interpretador. Mais adiante, é ele que irá adentrar o "ramo da condicional" e fazer com que o código ali de dentro seja executado (veremos mais detalhes sobre ele num próximo artigo).

## Gerando os ramos da AST

**ATENÇÃO: Recomendamos que você acompanhe o** [**código do Avaliador Sintático**](https://github.com/DesignLiquido/delegua/blob/principal/fontes/avaliador-sintatico/dialetos/avaliador-sintatico-pitugues.ts) **durante a leitura.**

Desenvolver uma linguagem de programação é uma atividade um tanto quanto complexa e temos que elaborar com bastante cuidado cada recurso e/ou funcionalidade que queremos empregar nela. Afinal, isso tudo é o que vai nos permitir construir com ela uma infinidade de soluções para sistemas e, para evitar que ocorram inconsistências na linguagem, precisamos ter todo este zelo no seu desenvolvimento.

Cada funcionalidade empregada na linguagem são chamadas de "construtos" que nada mais são do que os comportamentos que são utilizados para programar em Pituguês e você pode encontrar uma espécie de lista deles [neste arquivo](https://github.com/DesignLiquido/delegua/blob/principal/fontes/construtos/index.ts).&#x20;

Para dar um exemplo mais simples, vamos dar continuidade ao método `imprima` que adicionamos ao Lexador na postagem anterior sobre Pituguês...

Após receber do Lexador os tipos de símbolos no trecho de código (imprima,  texto e parenteses esquerdo e direito):

```
imprima("Agora, você está aprendendo sobre o Avaliador Sintático do Pituguês!")
```

O Avaliador Sintático recebe esta informação e vai seguir um fluxo de validações para gerar a nossa AST. Vamos entrar em cada uma das funções para gerar este "ramo da ávore" para o nosso método `imprima` que vai ser mais ou menos nesta ordem:

```
├── analisar 
│ ├── resolverDeclaracaoForaDeBloco 
│ │ ├── resolverDeclaracao 
│ │ │ ├── declaracaoEscreva 
│ │ │ │ ├── expressao 
│ │ │ │ │ ├── atribuir
│ │ │ │ │ │ ├── chamar
│ │ │ │ │ │ │ ├── primario
```

**OBS.:** Durante a explicação da lógica das funções, vamos ignorar alguns trechos de código para tornar a escrita mais concisa.

#### Função `analisar`

Neste primeiro momento, vamos receber a lista de símbolos do Lexador como argumento na função e vamos inicializar uma lista para abrigar as declarações que serão geradas e formarão a nossa AST:&#x20;

```
let declaracoes: Declaracao[] = [];
```

Logo abaixo, teremos um laço de repetição que permitirá que o Avaliador Sintático siga analisando os símbolos recebidos enquanto não chegar ao final das linhas de código do programa em Pituguês.

O objetivo principal desta função é que os símbolos trazidos pelo Lexador sejam traduzidos em declarações pelo Avaliador Sintático e é isto que será feito dentro deste laço de repetição, pois dentro dele teremos uma linha:

```
const retornoDeclaracao = await this.resolverDeclaracaoForaDeBloco();
```

É aqui que o código vai começar a análise de cada um dos símbolos recebidos para depois instanciarmos o construto referente a declaração que queremos gerar e adicioná-la à nossa lista de declarações.

#### Função `resolverDeclaracaoForaDeBloco`

Dentro desta função, o objetivo é verificar se a declaração é de uma função, classe, variável ou expressão. Como no caso da nossa linha de código se trata da instrução `imprima`, que não vai ser classificada como uma função, classe ou variável, vamos adentrar diretamente na função que irá analisar a instrução vigente:

```
return await this.resolverDeclaracao();
```

#### Função `resolverDeclaracao`

Vamos nos deparar com algumas verificações que dizem respeito a variáveis, mas, como não estamos analisando uma variável, a nossa linha de código vai entrar em um `switch case`, logo abaixo, que irá entrar neste trecho:

```
case tiposDeSimbolos.IMPRIMA:
case tiposDeSimbolos.ESCREVA:
    const simboloEscrevaOuImprima = this.avancarEDevolverAnterior();
    return this.declaracaoEscreva(simboloEscrevaOuImprima);
```

No nosso artigo sobre o Lexador, ensinamos que quando uma nova palavra-reservada for incluída, também devemos incluir o seu tipo dentro deste [arquivo](https://github.com/DesignLiquido/delegua/blob/principal/fontes/tipos-de-simbolos/pitugues.ts). É exatamente agora que ele vai ser de suma importância para que o Avaliador Sintático consiga buscar o método que irá verificar se a sintaxe do código foi escrita corretamente e gerar o construto  e declaração em questão para fazer parte da AST.

Antes de irmos adiante no `switch case`, talvez cause uma ligeira dúvida ao leitor de: "por que estamos sendo direcionados para a função `declaracaoEscreva` se estamos tratando da função `imprima`? Por que não uma `declaracaoImprima`?".&#x20;

A linguagem Pituguês, atualmente, é considerada um dialeto de programação e ela foi desenvolvida dentro do código-fonte de Delégua. Muitas características de Delégua foram herdadas pelo Pituguês, juntamente com os padrões de nomenclatura e assinatura dos métodos desenvolvidos para o Avaliador Sintático de Delégua.

Geralmente, quando incluímos um dialeto de progamação no ecossistema de Delégua, vamos fazer com que este dialeto herde a [classe Avaliador Sintatico Base](https://github.com/DesignLiquido/delegua/blob/principal/fontes/avaliador-sintatico/avaliador-sintatico-base.ts) que vai trazer para o nosso dialeto a assinatura de diversos métodos a serem implementados e por conta desta herança é que será empregada este padrão de nomes dos métodos. É apenas uma questão organizacional.

De qualquer forma, como o nosso `imprima` possui o mesmo comportamento do `escreva`, não precisamos criar um novo método "`declaracaoImprima`", por exemplo, para reproduzir esta funcionalidade, basta inserirmos o sinônimo "`imprima`" no `switch case` que será direcionado para a construção da declaração em questão.

#### Função `declaracaoEscreva`

Após ter reconhecido o símbolo `imprima`, entramos na função que irá construir a declaração do método para adicioná-la à AST.&#x20;

O que você pode notar é que a função `declaracaoEscreva` recebe como argumento o símbolo gerado pelo Lexador e que chegou do Avaliador Sintático e, agora, vamos entrar na verificação para garantir que a os itens estão na ordem esperada que se possa construir, finalmente, uma declaração para `imprima`:

1. Função `consumir`: apenas tem a finalidade de verificar se o símbolo atual corresponde ao parentese esquerdo que precede o texto que nossa função `imprima` deverá nos devolver;
2. `const argumentos [...]`: aqui, é iniciado uma constante que deverá receber um Array de algum  construto. Neste caso, será o nosso texto;
3. Vamos adentrar um laço de repetição que irá começar a verificação do conteúdo que há após a abertura do parenteses, a fim de validar se temos um dado textual;
4. Função `expressao`: ainda estamos dentro de `declaracaoEscreva`, mas fomos direcionados para a função `expressao` que apenas irá nos enviar para a seguinte;
5. Função `atribuir`: fará uma série de verificações buscando se o símbolo atual, o texto do nosso `imprima`, é algum tipo de operador ([lista de operadores](https://github.com/DesignLiquido/pitugues-docs/wiki/Operadores) do Pituguês), no entanto, como ele não irá entrar em nenhuma dessas condições, iremos para o próximo método;
6. Função `chamar`: esta função tem como objetivo identificar chamadas de funções, acesso de funções, acesso por índice e fatiamentos, porém, caso não se enquadre em nenhuma dessas opções, seremos enviados para a próxima função;
7. Função `primario`: finalmente, vamos passar por um `switch case` para inferir que os argumentos dentro de `imprima` são do tipo textual, além de retornar e instanciar um construto `Literal` que é onde estará definido o tipo do dado;
8. Vamos `consumir` novamente para verificar se está ocorrendo o fechamento da função com parenteses direito;
9. E, finalmente, o Avaliador Sintático conseguirá instanciar uma `declaracaoEscreva` que construirá a função `imprima` futuramente, o que nos trará o resultado:

```
declaracoes: [
  Escreva {
      linha: 1,
      hashArquivo: -1,
      decoradores: [],
      assinaturaMetodo: '<principal>',
      argumentos: [Array],
      simboloEscreva: [Simbolo]
  }
],
erros: []
```

Após todo esse percurso percorrido, voltaremos para dentro da função `analisar` que terá gerado a nossa declaração para `imprima` e irá inseri-lá na AST. A geração da AST pode ser percebida no trecho de código:

```
let declaracoes: Declaracao[] = [];
while (!this.estaNoFinal()) {
    const retornoDeclaracao = await this.resolverDeclaracaoForaDeBloco();

    if (retornoDeclaracao === null) {
        continue;
    }

    if (Array.isArray(retornoDeclaracao)) {
        declaracoes = declaracoes.concat(retornoDeclaracao);
    } else {
        declaracoes.push(retornoDeclaracao as Declaracao);
    }

 // Restante co código [...]
}
```

Para gerar a AST, depois de concluir todo aquele trajeto para conseguir transformar nosso símbolo numa declaração a partir da função `resolverDeclaracaoForaDeBloco` e, por fim, enviamos o que foi retornado na variável `retornoDeclaracao` por todo aquele processo de validação e enviamos para nosso array de `declaracoes` com um `push`.

## Contribua com o Pituguês!

Agora que você sabe o que é um Avaliador Sintático e como ele funciona no Pituguês, você já está apto a contribuir com ele!

Como comentamos, o código-fonte do Pituguês mora dentro de Delégua, então você irá encontrá-lo [neste repositório do GitHub](https://github.com/DesignLiquido/delegua).&#x20;

Além disso, também preparamos [este documento](https://github.com/DesignLiquido/pitugues-docs/blob/principal/CONTRIBUTING.md) que **te guia em como contribuir** com a linguagem, em que damos uma visão geral da estrutura do código e os arquivos principais para você contribuir com o Pituguês.

Caso encontre alguma dificuldade, a Design Líquido fez um [vídeo demonstrando como contribuir com um Avaliador Sintático](https://www.youtube.com/watch?v=lxqY48mDjqQ\&t) e você pode pegar inspirações de lá!&#x20;

E se você for uma pessoa um pouquinho mais ansiosa e quer entender todas as etapas de desenvolvimento de uma linguagem de programação, te indicamos esta [playlist no YouTube](https://www.youtube.com/watch?v=92RlDyXy5EE\&list=PL6y10dwsUMhpnsBj_f3Us-JkRDBwEnq8O) da Design Líquido!&#x20;

### Mas com o que posso contribuir?

De diversas formas... você pode usar a linguagem e reportar bugs pra gente, pode divulgar a linguagem e até mesmo fazer contribuições diretas no código-fonte...

#### Dica

**Para contribuir com o Avaliador Sintático: investique e analise os construtos que já existem na linguagem, pense o que mais você incluiria e/ou melhoria e vem contar pra gente!**&#x20;

### Um pequeno adendo sobre contribuições...

Aderir a boas práticas, registrando no GitHub seu processo de contribuição é de suma importância! No guia de contribuição que trouxemos para vocês, é ensinado a como buscar por issues para contribuir - ou abrir as próprias issues - e como fazer um PR (Pull Request). Parte da função dessas etapas é deixar documentado e acessível para que outros contribuidores possam acompanhar o desenvolvimento do projeto.

Aliás, não apenas para que outras pessoas consigam acompanhar o desenrolar do projeto, mas também é uma forma de mostrar para o mundo o seu trabalho! Aqui, na Cumbuca Dev, a gente promove e incentiva a contribuição em projetos de Código Aberto, por ser uma das formas de pessoas iniciantes aprenderem e se desenvolverem na programação, enquanto conquistam experiência real que será super válida para o mercado de trabalho!

Ou seja, enquanto você consegue participar ativamente, ajudando com o crescimento de um projeto e trocando ideia com outros contribuidores, também consegue desenvolver suas habilidades! E como tudo isso é feito com transparência, você pode facilmente comprovar experiência! <3

### E agora?

Bom, agora que você já sabe como funciona e como se programa um Lexador, te convidamos a aprender e testar o Pituguês! 😉​ Escrevemos [este tutorial](https://cumbuca.dev/2025/11/07/vem-com-a-gente-programar-em-portugues/) para te ensinar a como programar com ele! Além disso, você sempre pode consultar a [documentação](https://github.com/DesignLiquido/pitugues-docs/wiki)!

[Neste repositório](https://github.com/cumbucadev/desafios-pitugues/issues) temos descrito alguns desafios para fazer em Pituguês! E se, por caso, tiver qualquer dificuldade com a linguagem, já pode reportar pra gente que te ajudamos!<br>
