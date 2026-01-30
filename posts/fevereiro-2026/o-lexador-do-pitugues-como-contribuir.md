# O Lexador do Pituguês - Como Contribuir?

No nosso post anterior sobre como programar o Pituguês, aprendemos que ela é uma linguagem interpretada e que sua arquitetura de projeto é composta por três camadas: o Lexador, o Avaliador Sintático e o Interpretador. Cada uma delas possui sua responsabilidade para que possamos desenvolver uma linguagem de programação e executá-la e, agora, vamos começar a desbravar um pouquinho de cada uma delas, começando por este artigo em que falaremos do Lexador e mostrar um pouquinho como podemos contribuir com o seu código.

Mas, primeiramente...

## O que é um Lexador?

Nada mais é do que um programa que irá percorrer e escanear, da esquerda para a direita, os caracteres da nossa linguagem. Nesta etapa, serão identificados os lexemas (que também podemos chamar de símbolos ou tokens) e seu tipo, seu devido significado naquela instrução.

Existem diferentes categorias de símbolos...

### Palavras-reservadas

Também chamadas de _keywords_, são palavras que estão presentes na linguagem de programação e são utilizadas para se escrever instruções do que queremos que nosso programa faça. Por exemplo, palavras como "`while`", "`for`", "`const`", "`else`" e entre outras que já fazem parte da linguagem e não podem ser usadas para nomear variáveis ou funções.

Aliás, as funções nativas de uma linguagem também são categorizadas como palavras-reservadas e não podem ser usadas para criar alguma outra instrução. Mas, não se preocupe, caso você não saiba que o nome que você deu a uma variável ou função que já exista na linguagem, ela mesma irá te avisar!

Então, tomando como o exemplo o Pituguês, se quisermos desenvolver uma função para inverter a ordem de um vetor e queremos nomeá-la com `inverter`, não será possível, pois já [existe uma função que leva este nome](https://github.com/DesignLiquido/pitugues-docs/wiki/Estruturas-de-Dados-Elementares#inverter-1).

### Identificadores

É classificação que recebe tudo aquilo que podemos nomear enquanto desenvolvemos um programa! Ou seja, quando damos um nome a uma variável, seu nome é chamado de "identificador". O mesmo acontece quando nomeamos uma função.

Se criarmos a seguinte função...

```
função somar(valor1, valor2):
    retorna valor1 + valor2 
```

Vamos ter a palavra-reservada `função` para declaração de funções e, em seguida, o identificador `somar`. É assim que o Lexador irá reconher o nome da função e categoriza-lo para que seja reconhecido dentro da linguagem.

### Constantes

São valores que não sofrem alterações ao longo da execução do programa. Pense que se formos desenvolver um software matemático, o valor do número PI (3,14) jamais poderia ser alterado durante a execução. Linguagens como JavaScript e Delégua permitem que constantes sejam declaradas da seguinte forma:

`const valor_de_pi = 3.14;`

No entanto, assim como no Python, o Lexador de Pituguês não reconhece ou permite a declaração de constantes como no exemplo de Delégua e JavaScript. Mas isto é algo que pode ser contornado se aplicarmos a convenção do Python no Pituguês que é declarar uma variável em caixa alta:

`VALOR_DE_PI = 3.14`

Esta convenção fará com que outras pessoas que tiverem contato com o código entendam que aquele dado não deve ser alterado.

Mesmo que o Lexador do Pituguês não seja capaz de reconhecer constantes, achamos que é importante trazer para nossos leitores que elas ainda são uma forma de se identificar e categorizar dados e que, para que seu reconhecimento seja possível, deve ser incluído na programação de um Lexador (afinal, vai que um dia você mesmo queira desenvolver uma linguagem que use constantes...).

### Operadores

Outra categoria que um Lexador deve ser capaz de mapear são os operadores. Dentro das linguagens de progamação, teremos:

* Operadores Aritméticos: são os símbolos usados para realizar operações matemáticas - adição, subtração, multiplicação, divisão, módulo e exponenciação. Você pode encontrar os operadores aritméticos (ou matemáticos) que existem no Pituguês na [documentação](https://github.com/DesignLiquido/pitugues-docs/wiki/Operadores#operadores-matem%C3%A1ticos);
* Operadores Relacionais: são os operadores usados para comparação - igual a, diferente de, maior que, menor que, maior ou igual e etc... também temos eles descritos na [documentação do Pituguês](https://github.com/DesignLiquido/pitugues-docs/wiki/Operadores#operadores-de-compara%C3%A7%C3%A3o);
* Operadores Lógicos: podem ser símbolos ou palavras-reservadas em que podemos verificar condições, geralmente retornam os valores `verdadeiro` ou `falso`. Os símbolos mais utilizados são: AND, &&, OR, || e ! - mas existem outros mais! Na documentação do Pituguês, temos listados os [operadores lógicos](https://github.com/DesignLiquido/pitugues-docs/wiki/Operadores#operadores-l%C3%B3gicos) e os [operadores bit a bit](https://github.com/DesignLiquido/pitugues-docs/wiki/Operadores#operadores-bit-a-bit), que também podem se encaixar nesta categoria.

### Símbolos Especiais

Geralmente são os símbolos usados na estruturação do programa, por exemplo, quando uma linguagem requer que sua instrução seja terminada em ponto e vírgula, ou que a declaração de vetores exige que seus dados devem estar escrito entre colchetes. Nessas situações, o ponto e vírgula e os colchetes seriam os "símbolos especiais" que fazem parte da estrutura da linguagem para a escrita de comandos.

### Lexemas

Nada mais são dos que as strings que correspondem com um típo de símbolo. Por exemplo... quando declaramos uma variável em Pituguês:

`nome_da_linguagem = "Pituguês"`

Comentamos antes que o nome da variável é reconhecido como identificador, mas ele também vai ser mapeada como um lexema. Então, quando escrevemos esta linha de código em Pituguês e depuramos o Lexador, ele nos trará este retorno:

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

Note o Lexador nos retorna uma lista de símbolos, em formato json, e cada símbolo é mapeado de acordo com seu tipo e seu lexema. Aqui, o lexema é a declaração literal da escrita da instrução!&#x20;

Esta etapa é bastante importante para que se tenha a base inicial ao programar uma linguagem de programação, pois a partir da identificação desses lexemas e tipos é que poderemos criar as regras de execução de cada símbolo, baseados na sua função. E isto acontecerá na etapa do Avaliador Sintático.&#x20;

## E como eu posso contribuir com a linguagem Pituguês?

O repositório em que podemos encontrar o Pituguês está [disponível no GitHub](https://github.com/DesignLiquido/delegua), dentro do projeto Delégua, então, é dentro desse projeto que podemos fazer contribuições diretamente no Pituguês: é onde está seu código fonte.&#x20;

Além disso, temos outro repositório em que [disponibilizamos um documento](https://github.com/DesignLiquido/pitugues-docs/blob/principal/CONTRIBUTING.md) que ensina a como contribuir com a linguagem! Lá você vai encontrar um guia de como criar uma conta no GitHub, um passo a passo de como contribuir com a linguagem e como deve registrar sua contribuição na plataforma.

#### Um pequeno adendo...

Aderir a boas práticas, registrando no GitHub seu processo de contribuição é de suma importância! No guia de contribuição que trouxemos para vocês, é ensinado a como buscar por issues para contribuir - ou abrir as próprias issues - e como fazer um PR (Pull Request). Parte da função dessas etapas é deixar documentado e acessível para que outros contribuidores possam acompanhar o desenvolvimento da linguagem.

Aliás, não apenas para que outras pessoas consigam acompanhar o desenrolar do projeto, mas também é uma forma de mostrar para o mundo o seu trabalho! Aqui na Cumbuca Dev, a gente promove e incentiva a contribuição em projetos de Código Aberto, por ser uma das formas de pessoas iniciantes aprenderem e se desenvolverem na programação, enquanto conquistam experiência real que será super válida para o mercado de trabalho!&#x20;

Ou seja, enquanto você consegue participar ativamente, ajudando com o crescimento de um projeto e trocando ideia com outros contribuidores, também consegue desenvolver suas habilidades! E como tudo isso é feito com transparência, você pode facilmente comprovar experiência! <3&#x20;

### Contribuindo com o Lexador!&#x20;

No documento que contém o guia de como contribuir, temos esta [seção que trata da Estrutura do Projeto](https://github.com/DesignLiquido/pitugues-docs/blob/principal/CONTRIBUTING.md#9-estrutura-do-projeto), nela, temos sinalizados os principais arquivos que estão envolvidos na programação do Pituguês.

Mantendo o foco no Lexador, queremos trazer um exemplo de contribuição que foi implementado por quem vos escreve, a fim de ilustrar brevemente como adicionar uma funcionalidade neste elemento estrutural do dialeto.

#### No início...

Como Pituguês é um dialeto, muitas estruturas e características acabavam sendo herdadas de Delégua, parecia até uma cópia. O que podemos dizer que "não era considerado muito Pythônico", uma vez que a proposta do Pituguês é ser um equivalente a Python, enquanto Delégua tem como principais influências Ruby, C# e Kotlin.

Conforme fui estudando mais o dialeto e seu código, identifiquei que o Pituguês usava a mesma função que Delégua: `escreva`. Então, na época, como Pessoa Iniciante Contribuidora, isto me deixou um pouco inquieta, pois se queremos aproximar o Pituguês do Python, poderíamos buscar traduções equivalentes em casos como este.&#x20;

Dessa forma, veio a ideia de transformar o `escreva` em `imprima`, uma tradução um pouco mais direta do `print` da linguagem na qual Pituguês se inspira.

Como foi comentado, o Lexador visa identificar os símbolos e seus tipos para que depois sua lógica possa ser implementada no Avaliador Sintático. Precisamos lembrar que o comportamento para uma função chamada `imprima` já existia na função `escreva` da linguagem e apenas queríamos adicionar um sinônimo a ela. Portanto, o que precisamos fazer é simplesmente adicionar a palavra "imprima" como uma palavras-reservadas e fazer com que o Lexador a reconheça como parte do Pituguês.

#### Quais arquivos eu mexo?

Todo o código-fonte da linguagem está dentro do diretório `fontes` do projeto e como vamos mexer no Lexador do Pituguês, o caminho para chegar até ele é:&#x20;

```
fontes > lexador > dialetos > lexador-pitugues.ts
```

Dentro deste arquivo, você vai encontrar uma função chamada `mapear`. Imagine que você escreveu a seguinte linha de código:

```
imprima("Você está aprendendo sobre o Lexador do Pituguês!")
```

Quando você escreve uma instrução como esta em um editor de código como o VS Code, por exemplo, deve lembrar que escrevemos apenas texto no editor e que esse texto precisa ser traduzido para a linguagem de máquina, como comentamos [neste artigo](https://cumbuca.dev/2026/01/16/como-e-programado-o-pitugues/).&#x20;

Nesse contexto, o Lexador é quem fará a primeira etapa de reconhecimento do que há escrito no código, identificando e retornando os tokens ali presentes, com seus respectivos tipos. Basicamente, neste primeiro momento, precisamos apenas identificar os lexemas que compõe a nossa instrução.

Dentro do Lexador do Pituguês, temos a função `mapear` para percorrer todas linhas de código escritas no editor, identificando quando aquele comando é iniciado é encerrado. Assim como também verifica a partir de que momento está o início e o final do código por inteiro.&#x20;

Mas ainda não é aqui que vamos adicionar a palavra-reservada `imprima`!

Dentro do `mapear`, temos um trecho de código assim:

```
while (!this.eFinalDoCodigo()) {
     this.inicioSimbolo = this.atual;
     this.analisarToken();
}
```

Em resumo, esta instrução está dizendo que "enquanto não for o fim do código, cada token deverá ser analisado". E é aí aqui que vamos fazer com que o nosso comando `imprima` seja reconhecido! Através da função `analisarToken` no Lexador!

Esta função vai capturar caractere por caractere da nossa linha de código e vai buscar identificar cada um deles em um longo `switch case`. Pode reparar que vamos ter diversos `cases` para caracteres diferentes (tabulação, espaçamento, nova linha, ponto e vírgula etc). Caracteres correspondentes às categorias que vimos anteriormente no artigo (palavra-reservadas, identificadores, operadores e símbolos especiais). Ou seja, a partir de um momento que o nosso `analisarToken` consegue identificar um desses caracteres, ele vai verificar se há um tipo de símbolo correspondente  ao caractere.

Mas como vamos conseguir identificar a palavra `imprima` como palavra-reservada dentro deste `switch case`?

Ainda dentro do `analisarToken`, no final do `switch case`, temos:

```
default:
    if (this.eDigito(caractere)) this.analisarNumero();
    else if (this.eAlfabeto(caractere)) this.identificarPalavraChave();
    else {
         this.erros.push({
                   linha: this.linha + 1,
                   caractere: caractere,
                   mensagem: 'Caractere inesperado.',
          } as ErroLexador);
          this.avancar();
    }
```

Este trecho vai percorrer os caracteres também, mas seu objetivo é identificar o que é número e o que pode ser alguma letra do alfabeto. Inclusive, note que, na terceira linha, temos uma condição que diz que se haver um conjunto de caracteres, deve ser identificada a palavra chave através da função `identificaPalavraChave`!

Bom... vamos para esta função agora... nela, vai haver uma série de verificações quanto a composição da sequência de caracteres, mas a cereja do bolo está quando ela, finalmente, consegue determinar o lexema neste momento:

```
const tipo: string =
    textoPalavraChave in palavrasReservadas
         ? palavrasReservadas[textoPalavraChave]
         : tiposDeSimbolos.IDENTIFICADOR;
```

Aqui temos o que é chamado de "condição ternária" e está sendo verificado se a palavra que foi escrita no código existe ou não enquanto palavra-reservada.&#x20;

Na segunda linha do trecho de código anterior, temos a constante `palavraReservada`, em que temos dentro dela uma lista de palavras-reservadas. Esta constante é importada de um outro arquivo que está no caminho:

```
fontes > lexador > dialetos > palavras-reservadas > pitugues.ts
```

Dentro da constante, vamos adicionar o nosso "imprima", porém... temos que declará-lo neste formato:

```
imprima: tiposDeSimbolos.IMPRIMA
```

Você pode conferir a lista de palavras-reservadas [neste link do GitHub](https://github.com/DesignLiquido/delegua/blob/principal/fontes/lexador/dialetos/palavras-reservadas/pitugues.ts).&#x20;

Adicionar o imprima nesta lista de palavras-reservadas ainda não é o suficiente, temos que adicionar, também, na lista de objeto `tiposDeSimbolos` que está no arquivo:

```
fontes > tipos-de-simbolos > pitugues.ts
```

Você pode dar uma olhadinha [neste arquivo](https://github.com/DesignLiquido/delegua/blob/principal/fontes/tipos-de-simbolos/pitugues.ts) para conferir o formato que devemos adicionar o nosso tipo de símbolo que é:

```
IMPRIMA: 'IMPRIMA'
```

#### Testando a nossa adição

Seguindo todos esses passos, você já pode adicionar uma nova palavra-reservada no Lexador do Pituguês, mas a gente ainda precisa garantir que ela está sendo reconhecida pela linguagem e sendo executada corretamente.

Agora, vamos ir para nosso arquivo de teste do Lexador de Pituguês que você pode encontrar no seguinte caminho, já saindo do `fontes`:

```
testes > lexador > dialetos > pitugues > lexador.test.ts
```

Já [dentro do arquivo](https://github.com/DesignLiquido/delegua/blob/principal/testes/lexador/dialetos/pitugues/lexador.test.ts), queremos saber se o Lexador já está identificando `imprima` como nova palavra-reservada, então devemos elaborar um teste unitário e executá-lo:

```
            it('Sucesso - imprima', () => {
                const resultado = lexador.mapear(
                    ["imprima('Você está aprendendo sobre o Lexador do Pituguês!')"],
                    -1
                );

                expect(resultado).toBeTruthy();
                expect(resultado.simbolos).toHaveLength(4);
                expect(resultado.simbolos).toEqual(
                    expect.arrayContaining([
                        expect.objectContaining({ tipo: 'IMPRIMA' }),
                        expect.objectContaining({ tipo: 'PARENTESE_ESQUERDO' }),
                        expect.objectContaining({ tipo: 'TEXTO' }),
                        expect.objectContaining({ tipo: 'PARENTESE_DIREITO' }),
                    ])
                );
            });
```

No teste para o Lexador, note que estamos apenas verificando se contém os tipos de símbolos esperados naquela linha de código. É apenas isto que precisamos fazer com o Lexador: verificar se nossos símbolos estão sendo reconhecidos como previsto.

Após adicionar o teste, logo acima dele, no VS Code, você verá escrito `Run | Debug` e você pode clicar no `Run` para executar o teste e ver se ele é aprovado. Caso ele passar nos testes unitários, podemos enviar nossa contribuição para o repositório do projeto.

A Design Líquido já fez um vídeo ensinando a programar um Lexador [neste vídeo do YouTube](https://www.youtube.com/watch?v=92RlDyXy5EE)! Você pode dar uma conferida nele para ver como é todo o processo em ação.

Lembrando que também temos [este guia](https://github.com/DesignLiquido/pitugues-docs/blob/principal/CONTRIBUTING.md) de como você pode fazer sua contribuições no código-fonte da linguagem!

### E agora?

Bom, agora que você já sabe como funciona e como se programa um Lexador, te convidamos a aprender e testar o Pituguês! 😉​ Escrevemos [este tutorial](https://cumbuca.dev/2025/11/07/vem-com-a-gente-programar-em-portugues/) para te ensinar a como programar com ele! Além disso, você sempre pode consultar a [documentação](https://github.com/DesignLiquido/pitugues-docs/wiki)!

[Neste repositório](https://github.com/cumbucadev/desafios-pitugues/issues) temos descrito alguns desafios para fazer em Pituguês! E se, por caso, tiver qualquer dificuldade com a linguagem, já pode reportar pra gente que te ajudamos!
