# Avaliador Sintático

Depois de tratarmos sobre o Lexador, vamos avançar para a próxima etapa do processo que torna possível transformar uma linguagem de alto nível em linguagem de máquina, permitindo.  o desenvolvimento de uma linguagem de programação: o Avaliador Sintático!&#x20;

Este termo foi a escolha feita pela Design Líquido como tradução de um `parser`, mas também podemos encontrar por aí outros nomes equivalentes como AST Walker, AST Evaluator ou até mesmo Analisador Sintático.&#x20;

### Retomando alguns conceitos...

Lembram quem o Lexador gera uma lista de símbolos (tokens) a partir das instruções que escrevemos no código-fonte do nosso programa? Tomemos o mesmo exemplo do artigo anterior, ao declararmos a variável:

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

A palavra `var` não é reconhecida pela linguagem por não fazer para de sua gramática e por isto temos retornado o erro acima. Afinal, assim como um idioma que segue uma estrutura sintática para gerar conexão entre palavras e transmitirmos uma mensagem, as linguagens de programação funcionam da mesma forma.

Os símbolos que compõem uma instrução de um código precisam o corresponder a uma ordem para serem reconhecidos dentro da regra gramatical da linguagem. Então, por exemplo, não podemos declarar uma variável assim:&#x20;

```
nome_da_linguagem "Pituguês" =
```

Mesmo que o Lexador consiga identificar cada um dos termos que são usados para escrever uma variável, eles estão desalinhados com o que se espera de uma declaração de variáveis e é o Avaliador Sintático que vai identificar isto e nos alertar quanto a esta incosistência.

## Como o Avaliador Sintático avalia os tokens do Lexador?

Como foi comentado antes, o Lexador ele gera uma lista dos símbolos que existem no código escrito e ele vai enviar essa lista para o Avaliador Sintático. A partir do momento em que o Avaliador Sintático tem acesso à esta lista de símbolos com seus devidos tipos identificados, ele terá o conhecimento sobre o significado daquele símbolo. Isto é de suma importância pois é como o Avaliador conseguirá determinar a função daquela linha de código no programa.&#x20;

Esta etapa é como se fosse uma análise sintática de uma frase mesmo, um momento em que iremos identificar a classe e função de cada palavra em uma frase, como:

<figure><img src="../../.gitbook/assets/Eu programo em Pituguês..png" alt=""><figcaption></figcaption></figure>

Perceba que cada palavra está ordenada de uma forma que faça sentindo para transmitir sua mensagem na língua portuguesa. Cada palavra pertence a uma classificação gramatical que possui uma função específica que determina seu significado dentro da frase.&#x20;

Usando o exemplo da frase acima, é como se o Lexador identificasse que existe um pronome, um verbo, uma preposição e um substantivo, nessa respectiva ordem. O Avaliador Sintático vai, agora, identificar qual é a função desses elementos e verificar se eles estão ordenados seguindo o padrão de "SUJEITO + VERBO + OBJETO".&#x20;

A nossa linha de código funciona vai sofrer um processo parecido de idetentificação quanto a função dela naquele programa. Mas, desta vez, irá analisar se há elementos como variáveis, condicionais, laços de repetição e etc. Tomemos como exemplo a declaração de variável que já usamos antes do Pituguês:

```
nome_da_linguagem = "Pituguês"
```

Quando o Avaliador receber esta linha de código, terá sido mapeado que ali temos um identificador, um sinal de atribuição de valor e, por último, temos o valor, dado de tipo textual. Após receber esta sequência de caracteres com seus significado, será analisado se aquela linha de código está na ordem correta para que seja considerada uma declaração variável. Isto será processado dentro desta função (você pode encontrá-la [aqui](https://github.com/DesignLiquido/delegua/blob/principal/fontes/avaliador-sintatico/dialetos/avaliador-sintatico-pitugues.ts)):

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

Nesta função em código TypeScript, cuja assinatura espera que seja construído um objeto do tipo `Var`, após o avaliador receber aquela linha de código. Só que está variável só será construída após passar pelas validações na função acima:

* `const identificador`: irá armazenar o identificador a nossa variável;
* É validado se existe o sinal de igual para atribuição de valor;
* Verifica se há algum valor após o sinal de atribuição;
* `const valor`: é quando o código vai verificar qual é o dado que queremos armazenar na variável;
* `const tipo`: a partir do valor, identifica o tipo do dado;
* Adiciona à pilha de escopo do programa a variável;
* Por fim, a função retorna um objeto `Var`, com as constantes e seus valores definidos na função: `identificador`, `valor` e `tipo`.

Ou seja, apenas após passar por todo esse processamento e validações é que o Pituguês conseguiu gerar uma variável para aquela linha de código.&#x20;

### Composição de um programa

Até aqui, conseguimos entender como o Avaliador Sintático é capaz de reconhecer uma variável, só que é preciso lembrar que um programa não consiste em apenas um elemento. Geralmente, acabamos usando uma série de construções como condicionais, funções, laços de repetição, criação de classes, enfim, temos uma ampla gama de comandos para elaborar num programa.&#x20;

Também é função do Avaliador Sintático construir o "cerne" do código, digamos,&#x20;
