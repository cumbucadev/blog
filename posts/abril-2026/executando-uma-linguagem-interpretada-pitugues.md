# Executando uma Linguagem Interpretada - Pituguês

Agora que já geramos a AST do Pituguês através do Avaliador Sintático, chegou o momento de entendermos o processo por que nosso código passa para ser executado. Por estarmos tratando de uma linguagem interpretada, nossas instruções estarão sendo traduzidas para código de máquina de forma simultânea, como já comentamos neste [artigo](https://cumbuca.dev/2026/01/16/como-e-programado-o-pitugues/) a respeito da diferença entre linguagens interpretadas e compiladas.&#x20;

### Impacto da Linguagem Interpretada

Quando falamos sobre a diferença entre a linguagem interpretada e a linguagem compilada, anteriormente, acabamos não deixando tão clara a aplicabilidade de cada uma delas.

Voltando um pouquinho no tempo, vamos pensar que, há alguns anos, era muito comum que fosse necessário instalar algum software na sua máquina para se ter acesso àquela funcionalidade/recurso. Antigamente, para conseguirmos nos comunicar por mensagens instantâneas em chats individuais, era comum instalarmos um software como o MSN Messenger, por exemplo, no computador.

Para um software assim rodar no seu computador, ele precisava ser construído para aquele sistema operacional e aquela máquina em específico, o que requer um desenvolvimento em uma linguagem compilada. Afinal, são estes tipos de linguagens em que, após sofrer o processo de build, conseguem traduzir um código de alto nível para a linguagem de máquina e que correspondem às exigências que aquele dispositivo requer. O que significaria que não haveria portabilidade entre dispositivos, uma vez que aquele programa foi desenvolvido para aquela máquina.

Além disso, depois que um código é compilado, aquele programa fica ali fechadinho em si e não conseguimos simplesmente fazer manutenção naquele código, mas precisamos retomar o código do programa e passar pelo processo de gerar um novo programa com as correções e deletar o antigo.&#x20;

Com o desenvolvimento e ampla aderência dos navegadores (browsers), começou a se dispensar a exigência de se ter um software instalado no seu computador para que você pudesse ter acesso àquele serviço. Tornou-se possível termos aplicações de troca de mensagens instantâneas como a versão web para o What'sApp em que podemos fazer o nosso login e ainda ter acesso às nossas conversas e lista de contatos sempre precisar da instalação de uma aplicação para aquela finalidade.  &#x20;

O impacto que os navegadores trouxeram foi justamente este de "encurtar o caminho" de execução das funcionalidades de um serviço, uma vez que eles têm a capacidade de interpretar instruções escritas em linguagens de programação interpretadas que são executadas sem passarem pela compilação, mas em tempo real. O que desobriga a instalação de aplicativos especializados para diferentes tipos de serviços.

Isto significa que poderemos criar uma série de sistemas que ficarão hospedados na web e, quando algum usuário quiser acessar aquele serviço específico, ele apenas precisará navegar até o endereço em que este serviço está disponível e o browser é que será responsável por fornecer o acesso do usuário àquele serviço. E como são aplicações que não passam pela compilação, são mais maleáveis em sua manutenibilidade.&#x20;

Ou seja, no ambiente web, as linguagens interpretadas possuem um poderio imensurável!

E justamente por terem esta característica que elas acabam se tornando agentes facilitadoras na portabilidade e flexibilidade de se construir e ofertar diversos tipos de serviços.

## Uma linguagem interpretada em português

A área de desenvolvimento de software vem ganhando destaque nos últimos anos, principalmente após a pandemia do COVID-19 em que muitas atividades precisaram se incluir no meio digital e o mercado de TI se viu precisando de mais profissionais. Diante deste cenário, uma expressiva parcela de falantes da língua portuguesa vem buscando formação neste universo devido ao mercado aquecido.&#x20;

No entanto, a gente já comentou [nesta outra postagem](https://cumbuca.dev/2025/08/22/hackeando-o-ingles-programando-na-lingua-em-que-voce-pensa/) sobre como as linguagens de programação que são usadas comercialmente podem ser desafiadoras para falantes lusófonos, especialmente para pessoas iniciantes em programação e por isso linguagens em português podem ser uma excelente porta de entrada para estas pessoas.

Por mais que existam outras linguagens de programação em português, elas ainda são voltadas apenas a fins didáticos e seu uso ocorre em "ambientes controlados", em editores de código exclusivos, ou até mesmo conseguem rodar no ambiente da web. E, embora contribuam imensamente como ferramenta de ensino de algoritmos e lógica de programação, não conseguimos criar e disseminar soluções em função desta limitação de não serem suportadas pelos interpretadores dos browsers.

E é verdade que a revolução conquistada pelos browsers em dispensar a necessidade de uma aplicação específica para diferentes sistemas torna o desenvolvimento de aplicações mais flexível e até mesmo mais ágil. Mas eles ainda utilizam linguagens de programação em língua inglesa e precisamos lembrar das dificuldades enfrentadas por falantes da língua portuguesa ao aprender a programação.

Com a ampla difusão dos navegadores e, como vimos antes, sua capacidade de executar serviços desenvolvidos com linguagens interpretadas, acreditamos que há espaço para se desenvolver uma linguagem interpretada em português que vá além da proposta educacional!&#x20;

Ao construirmos uma linguagem interpretada em português que consegue ser interpretada pelo Google Chrome, Opera, Firefox,Edge e entre outros, possibilitamos que falantes de língua portuguesa consigam usar essa mesma linguagem para criar!&#x20;

Com isto, temos a oportunidade de desenvolver e produzir serviços web em português! O que poderá contribuir para a geração de renda no mercado nacional!&#x20;

Então, por que não construir uma linguagem de programação interpretada em português que seja suportada pelos navegadores para desenvolvimento de serviços web e que contribua na geração de trabalho e renda, uma vez que teríamos a barreira linguística superada?

É isto que se busca oportunizar com o Pituguês: uma linguagem interpretada que possa ser executada em qualquer dispositivo que tenha acesso a um navegador!

## O Interpretador

Agora, chegou o momento de entendermos como funciona o código do Interpretador!&#x20;

Depois de passarmos pelo processo do Avaliador Sintático, onde é verificado se a nossa instrução em Pituguês está de acordo com a sintaxe esperada e também acontece a construção da AST (Abstract Syntax Tree, ou Árvore Sintática Abstrata) do programa, ainda nos resta desenvolver a etapa que irá, de fato, executar o nosso comando.&#x20;

Vamos retomar o exemplo de código que trabalhamos no Avaliador Sintático

```
letra = "b"
se letra == "a":
    escreva("Letra A")
senao:
    escreva("Não é letra A")
```

O Avaliador Sintático acabou gerando uma lista de declarações, que vem a ser a nossa AST, o que podemos chamar de "ramos da árvore", em que temos uma visão panorâmica quanto aos componentes que existem no nosso programa:

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

Aqui temos a ideia geral das declarações existentes no programa, mas não conseguimos ter acesso ao código em si e, como comentamos no artigo sobre o Avaliador Sintático, agora é responsabilidade do Interpretador percorrer esta lista de declarações, adentrar em cada uma delas e fazer com que o código seja executado.

### Executando a AST

Agora que geramos e temos acesso à AST do nosso código, o Interpretador precisa conseguir acessar cada ramificação do nosso código para executar as instruções ali escritas. Como abordamos no artigo anterior, estruturas de dados em árvores não podem ser acessadas da mesma forma que estruturas lineares, uma vez que elas possuem toda uma hierarquia e conexões entre seus nós.

Para este tipo de estrutura a literatura indica a utilização de um Design Pattern conhecido por Visitor Pattern - ou "Padrão Visitante" em tradução livre. Este padrão de projeto foi descrito no livro de mesmo nome, "Design Patterns", que apresenta uma série de problemas estruturais que podem ser enfrentados no código de um projeto e traz soluções através destes padrões de projeto.

Uma linguagem de programação deve conseguir implementar diversos comportamentos e ainda ser possível expandi-los, mas isto precisa ser feito de uma forma que não cause interferência em elementos que já existem e que ainda se consiga fazer a manutenção da linguagem quando necessário. Nesse sentido, o Visitor Pattern consegue cumprir estes requisitos por usar e abusar dos pilares de polimorfismo e herança que a Programação Orientada a Objetos oferece.&#x20;

Para conhecer um pouco mais sobre este padrão de projeto, recomendamos a leitura do [artigo sobre Visitor Patterns do Refactoring Guru](https://refactoring.guru/pt-br/design-patterns/visitor) e deste artigo no [Medium sobre aplicar o Visitor Pattern em um interpretador de linguagem](https://medium.com/engenharia-arquivei/visitor-em-interpretadores-um-uso-espec%C3%ADfico-para-um-pattern-incomum-bfc01d651477).&#x20;

### Visitor Pattern - ou Padrão Visitante

Nesta seção, iremos abordar o Padrão Visitante já aplicado no Pituguês, portanto, se seu entendimento ficar um pouco confuso, recomendamos a leitura prévia dos artigos referidos na seção anterior.&#x20;

Para desenvolvermos a nossa solução com este padrão de projeto, precisamos ter definido três componentes-chave:&#x20;

#### **Visitor Interface** (Interface dos Visitantes)

Como toda interface, aqui, vamos apenas definir as assinaturas das operações que queremos efetuar e você pode encontrar este componente de Pituguês [neste arquivo](https://github.com/DesignLiquido/delegua/blob/principal/fontes/interfaces/visitante-comum-interface.ts) da interface `VisitanteComumInterface`.  Ou seja, as funções apenas possuem sua assinatura desenvolvida, são abstratas, mas seus nomes e argumentos são descritos para que seja seguido um padrão de aplicação destes comportamentos que serão desenvolvidos em outras classes.

Note que encontramos as assinaturas das funções abstratas de `visitarDeclaracaoVariavel`, `visitarDeclaracaoSe` e `visitarDeclaracaoEscreva`, que são referentes ao comportamento das declarações que o Avaliador Sintático identificou no nosso código de exemplo.

#### Concrete Visitors (Visitantes Concretos)

São as classes que vão herdar e implementar as assinaturas das funções elaboradas na `VisitanteComumInterface` e que vão ser desenvolvidas em classes como a `InterpretadorBase`, disponível [neste arquivo](https://github.com/DesignLiquido/delegua/blob/principal/fontes/interpretador/interpretador-base.ts).

É importante estar atento em como usamos a herança para implementar as funcionalidades que queremos no projeto do Pituguês. Para chegarmos na classe em que estão desenvolvidas estas "funções visitantes", vamos passar pelo caminho:

* `VisitanteComumInterface`: assinaturas dos métodos de visitação.
* `InterpretadorInterface`: herda o `VisitanteComumInterface`  e suas assinaturas de métodos, além de declarar os atributos e novas assinaturas de métodos que irão cumprir o papel de executar as instruções que escreveremos em Pituguês.
* `InterpretadorBase`: implementa o `InterpretadorInterface` que traz consigo tanto seus métodos abstratos como os de `VisitanteComumInterface` que, agora, já possui implementação, já tem seus comportamentos desenvolvidos.
* `Interpretador`: irá herdar o `InterpretadorBase` com as funções já desenvolvidas, assim como irá sobrescrever algumas delas.
* `InterpretadorPitugues`: herda o Interpretador com seus métodos e também sobrescreve alguns deles.

Em suma, para desenvolvimento do Interpretador do Pituguês, este fluxo de heranças se faz necessário para que possamos usufruir ao máximo do reaproveitamento de código, uma vez que estas classes podem ser reutilizadas em outros dialetos que Delégua suporta, o que acabou resultando no caminho:

```
VisitanteComumInterface > InterpretadorInterface > InterpretadorBase > Interpretador > InterpretadorPitugues
```

E é dentro do código em `InterpretadorBase` que vamos ter desenvolvidos os comportamentos de:

* `visitarDeclaracaoVariavel`
* `visitarDeclaracaoSe`
* `visitarDeclaracaoEscreva`

#### Visitable Elements (Elementos Visitáveis)

Aqui vamos começar a ter os objetos correspondentes às funcionalidades e recursos que queremos incluir na nossa linguagem e, novamente, vamos usar o pilar para herança para que possamos adicionar elementos sem interferir uns nos outros.&#x20;

Primeiramente, devemos lembrar que o Interpretador recebe uma AST de declarações do nosso código, então, vamos ter uma [classe abstrata chamada `Declaracao`](https://github.com/DesignLiquido/delegua/blob/principal/fontes/declaracoes/declaracao.ts) que vai  conter atributos que mapeiam a declaração específica e a função aceitar que será da seguinte forma:

```
async aceitar(visitante: VisitanteComumInterface): Promise<any> {
    return Promise.reject(new Error('Este método não deveria ser chamado.'));
}
```

Repare que a função recebe como argumento um objeto chamado `visitante` do mesmo tipo da nossa primeira interface que declara a assinatura dos métodos de visitação: `VisitanteComumInterface`.

Agora, precisamos implementar a nossa classe abstrata e esta função em uma "classe concreta", uma classe que poderá inicializar um objeto e vamos tomar como exemplo o comportamento de criação de variável. Ou seja, teremos uma [classe para declaração de variável chamada `Var`](https://github.com/DesignLiquido/delegua/blob/principal/fontes/declaracoes/var.ts) que irá sobrescrever a função aceitar, declarada na classe `Declaracao`, implementando o comportamento necessário para sua execução, o que irá resultar em:

```
async aceitar(visitante: VisitanteComumInterface): Promise<any> {
    return await visitante.visitarDeclaracaoVar(this);
}
```

Nesta sobrescrita, estamos nos referindo a uma função cujo desenvolvimento se encontra em `IntrepretadorBase` e que, logo em seguida, iremos apresentar qual é o percurso que é feito para chegar até aqui e executar a instrução pelo Interpretador.

### Visitando as Declarações

**ATENÇÃO: Recomendamos que você acompanhe o código do Interpretador durante a leitura! Disponibilizaremos os links para o código referido ao longo do texto.**

Como já vimos antes, a AST é constituída pelas declarações geradas pelo Avaliador Sintático e precisamos usar o Padrão Visitante para percorrer esta árvore, adentrar seus ramos e executar o trecho de código que existe dentro de cada um deles.&#x20;

Vamos iniciar nosso passeio no arquivo de testes...

#### 1. Testes do Interpretador do Pituguês

**NOTA: Você pode acessar o arquivo por** [**este link**](https://github.com/DesignLiquido/delegua/blob/principal/testes/interpretador/dialetos/pitugues/interpretador-pitugues.test.ts)**.**

Vamos adicionar um teste unitário com o nosso código de exemplo para simular qual seria o trajeto que o interpretador faria para executá-lo caso estivéssemos usando uma IDE. Então, temos o teste:

```
it('Teste do exemplo de código presente no artigo sobre o Interpretador', async () => {
    const retornoLexador = lexador.mapear([
        'letra = "b"',
        'se letra == "a":',
        '    escreva("Letra A")',
        'senao:',
        '    escreva("Não é letra A")'
    ], -1);

    const retornoAvaliadorSintatico = await avaliadorSintatico.analisar(
        retornoLexador,
        -1
    );

    const retornoInterpretador = await interpretador.interpretar(
        retornoAvaliadorSintatico.declaracoes
    );

     expect(retornoInterpretador.erros).toHaveLength(0);
});
```

Neste teste, temos que repassar sequencialmente os retornos do Lexador para o Avaliador Sintático e do Avaliador Sintático para o Interpretador. O Interpretador irá chamar a função `interpretar`, que recebe as nossas declarações já identificadas pelo Avaliador Sintático e, agora, vamos conseguir entender um pouco mais o percurso que o Padrão Visitante faz ao entrar dentro da função `interpretar`.

#### 2. Função `interpretar`  da classe `Interpretador`

Inicialmente, vamos parar dentro do [arquivo da classe Interpretador](https://github.com/DesignLiquido/delegua/blob/principal/fontes/interpretador/interpretador.ts), cuja função `interpretar` consta como sobrescrita, ou seja, ela é herdada de outra classe, e logo em seguida temos o trecho de código:

```
const resultados = await super.interpretar(declaracoes, manterAmbiente);
```

Esta linha de código visa obter os resultados das declarações recebidas como argumentos a serem executadas, mas ela redireciona para outra função `interpretar` que pertence à classe mãe.

#### 3. Função `interpretar` da classe `InterpretadorBase`

O [InterpretadorBase está disponível neste arquivo](https://github.com/DesignLiquido/delegua/blob/principal/fontes/interpretador/interpretador-base.ts).

Aqui, nós vamos inicializar atributos referentes aos erros, escopo e pilha de execução e, mais abaixo, o código irá buscar se conseguirá receber um retorno das declarações ou algum erro e isto está declarado na linha:

```
const retornoOuErro = await this.executarUltimoEscopo(manterAmbiente);
```

#### 4. Função `executarUltimoEscoto` da classe `Interpretador`

Voltamos à [classe Interpretador](https://github.com/DesignLiquido/delegua/blob/principal/fontes/interpretador/interpretador.ts), em que será capturada a `declaracaoAtual` que está em espera na pilha de execução para ser executada até chegamos à instrução:

```
retornoExecucao = await this.executar(declaracaoAtual);
```

#### 5. Função `executar` da classe `InterpretadorBase`

E voltamos mais uma vez para o [`InterpretadorBase`](https://github.com/DesignLiquido/delegua/blob/principal/fontes/interpretador/interpretador-base.ts), em que está desenvolvido a função `executar`  que recebeu a `declaracaoAtual` e, logo em seguida, já temos o atributo `retorno`:

```
const resultado: any = await declaracao.aceitar(this);
```

Todo esse caminho percorrido, até então, ilustra como o Interpretador recebe a AST do código de exemplo e tira proveito do Padrão Visitante para reaproveitamento e padronização do código, mas ele ainda não foi executado, apenas estamos identificando de qual declaração se trata para que ela possa ser executada.&#x20;

De toda forma, agora é o momento em que vamos começar a direcionar a nossa declaração para o objeto dela e começar a ver o Padrão Visitante em ação e como o Interpretador descobre qual instrução ele precisará executar.

#### 6. Função `aceitar` nas classes `Var` e `Se`

Lembrando que a função `aceitar` foi declarada em uma [classe abstrata `Declaracao`](https://github.com/DesignLiquido/delegua/blob/principal/fontes/declaracoes/declaracao.ts) para ser herdada por outras declarações. Ela recebe como argumento um objeto do tipo `VisitanteComumInterface` que é onde estão as assinaturas dos métodos visitantes que foram desenvolvidos em `InterpretadorBase`.

Na etapa 5, a função aceitar recebeu tanto a declaração `Var` como a `Se` e o código de cada uma delas se encontra desenvolvido desta forma:

* **Declaração `Var`**

Primeiramente, o Interpretador irá reconhecer a primeira linha do nosso código:

```
letra = "b"
```

É uma instrução que já foi identificada pelo Avaliador Sintática como uma [declaração `Var` e, dentro do seu código](https://github.com/DesignLiquido/delegua/blob/principal/fontes/declaracoes/var.ts), ela herda a classe `Declaracao`, como já foi comentado, e aplica a função `aceitar`. Porém, quando estamos dentro da classe da declaração Var, aceitar é sobrescrita:

```
async aceitar(visitante: VisitanteComumInterface): Promise<any> {
    return await visitante.visitarDeclaracaoVar(this);
}
```

* **Declaração `Se`**

Em sequência, o Interpretador irá avançar para a próxima declaração, cujo código é:

```
se letra == "a":
    escreva("Letra A")
senao:
    escreva("Não é letra A")
```

A [declaração `Se`](https://github.com/DesignLiquido/delegua/blob/principal/fontes/declaracoes/se.ts) também irá herdar e aplicar a função `aceitar` da seguinte forma:

```
async aceitar(visitante: VisitanteComumInterface): Promise<any> {
    return await visitante.visitarDeclaracaoSe(this);
}
```

Repare que ambas recebendo um visitante de tipo `VisitanteComumInterface` em foram declaradas nela funções abstratas e foram desenvolvidas em `InterpretadorBase`, ou seja elas estão nos redirecionando para as funções de visitação de declaração correspondentes a elas.&#x20;

E é justamente esse o "pulo do gato", pois a função `aceitar` que foi chamada lá no `InterpretadorBase` faz com que sejamos redirecionados para os objetos das declarações identificadas no nosso código e, agora, podemos ir para a função que irá executar a declaração.

#### 7. Funções `visitarDeclaracaoVar` e `visitarDeclaracaoSe`&#x20;

Somos redirecionados, novamente, para o `InterpretadorBase` que é onde estão desenvolvidas as funções de visitação das declarações.

* Para a declaração `Var`, vamos ter:

```
async visitarDeclaracaoVar(declaracao: Var): Promise<any> {
    const valorFinal = await this.avaliacaoDeclaracaoVarOuConst(declaracao);
    let tipoResolvido = declaracao.tipo;
    if (tipoResolvido.startsWith('função<')) {
        tipoResolvido = tipoResolvido.replace('função<', '').replace('>', '');
    }

    this.pilhaEscoposExecucao.definirVariavel(
        declaracao.simbolo.lexema,
        valorFinal,
        tipoResolvido
    );
    
    return null;
}
```

Aqui, a nossa variável vai passar por uma verificação para garantir seu tipo e averiguar se já foi declarada antes ou não, até ser incluída na pilha de escopo para execução.

* Para a declaração `Se`:

```
async visitarDeclaracaoSe(declaracao: Se): Promise<any> {
        const avaliacaoCondicaoSe = await this.avaliar(declaracao.condicao);
        if (this.eVerdadeiro(avaliacaoCondicaoSe)) {
            return await this.executar(declaracao.caminhoEntao);
        }

        for (let i = 0; i < declaracao.caminhosSeSenao.length; i++) {
            // TODO: Qual o tipo de `atual`?
            const atual = declaracao.caminhosSeSenao[i] as any;

            if (this.eVerdadeiro(await this.avaliar(atual.condicao))) {
                return await this.executar(atual.caminho);
            }
        }

        if (declaracao.caminhoSenao !== null) {
            return await this.executar(declaracao.caminhoSenao);
        }

        return null;
    }
```

A condicional será avaliada aqui e o código irá mapear quais outras declarações e expressões existem dentro de `Se`. Isto fará com que o Interpretador percorra todo aquele caminho que foi criado com o Padrão Visitante para identificar e executar as declarações, blocos e expressões dentro do `Se` e que serão adicionadas à pilha de escopo para execução.

## Contribua com o Pituguês!

Agora que você sabe o que é o Padrão Visitante e como ele funciona aplicado no Interpretador do Pituguês, você já está apto a contribuir com ele!

Como comentamos, o código-fonte do Pituguês mora dentro de Delégua, então você irá encontrá-lo [neste repositório do GitHub](https://github.com/DesignLiquido/delegua).

Além disso, também preparamos [este documento](https://github.com/DesignLiquido/pitugues-docs/blob/principal/CONTRIBUTING.md) que **te guia em como contribuir** com a linguagem, em que damos uma visão geral da estrutura do código e os arquivos principais para você contribuir com o Pituguês.

Caso encontre alguma dificuldade, a Design Líquido fez um [vídeo demonstrando como programar e  contribuir com o Interpretador](https://www.youtube.com/watch?v=OUt0s4F3jMk) e você pode pegar inspirações de lá!

#### Mas com o que posso contribuir?

De diversas formas... você pode usar a linguagem e reportar bugs pra gente, pode divulgar a linguagem e até mesmo fazer contribuições diretas no código-fonte...

**Dica**

**Para contribuir com o Avaliador Sintático: investigue e analise os construtos que já existem na linguagem, pense o que mais você incluiria e/ou melhoria e vem contar pra gente!**

#### Um pequeno adendo sobre contribuições...

Aderir a boas práticas, registrando no GitHub seu processo de contribuição, é de suma importância! No guia de contribuição que trouxemos para vocês, é ensinado como buscar por issues para contribuir - ou abrir as próprias issues - e como fazer um PR (Pull Request). Parte da função dessas etapas é deixar documentado e acessível para que outros contribuidores possam acompanhar o desenvolvimento do projeto.

Aliás, não apenas para que outras pessoas consigam acompanhar o desenrolar do projeto, mas também é uma forma de mostrar para o mundo o seu trabalho! Aqui, na Cumbuca Dev, a gente promove e incentiva a contribuição em projetos de Código Aberto, por ser uma das formas de pessoas iniciantes aprenderem e se desenvolverem na programação, enquanto conquistam experiência real que será super válida para o mercado de trabalho!

Ou seja, enquanto você consegue participar ativamente, ajudando com o crescimento de um projeto e trocando ideia com outros contribuidores, também consegue desenvolver suas habilidades! E como tudo isso é feito com transparência, você pode facilmente comprovar experiência! <3

#### E agora?

Bom, agora que você já sabe como funciona e como se programa um Interpretador, te convidamos a aprender e testar o Pituguês! 😉​ Escrevemos [este tutorial](https://cumbuca.dev/2025/11/07/vem-com-a-gente-programar-em-portugues/) para te ensinar como programar com ele! Além disso, você sempre pode consultar a [documentação](https://github.com/DesignLiquido/pitugues-docs/wiki)!

[Neste repositório](https://github.com/cumbucadev/desafios-pitugues/issues) temos descrito alguns desafios para fazer em Pituguês! E se, por caso, tiver qualquer dificuldade com a linguagem, já pode reportar pra gente que te ajudamos!
