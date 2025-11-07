# Como é programado o Pituguês?

Numa das nossas últimas postagens, apresentamos a vocês a linguagem Pituguês e como usar ele. Mas lembram que também contamos que seu código é aberto? Isto significa que qualquer pessoa pode participar do desenvolvimento da linguagem, é o que chamamos de "Comunidade de Código Aberto (ou Open Source)".

### E como faço para participar da Comunidade de Código Aberto?

Basta se voluntariar para ajudar com algum projeto que você tenha interesse e buscar formas de contato com as pessoas que já estão envolvidas com o projeto, seja por fóruns, redes sociais, e-mails e etc.&#x20;

Nesse sentido o GitHub também cumpre um papel importante para projetos de código aberto, pois podemos fazer discussões por lá, escrever documentação dos projetos, fazer nossas contribuições, deixar organizado e registrado o que e como a comunidade pode ajudar e por aí vai...

As participações vão desde comentários, trazer suas dúvidas, sugestões, reportar imprevistos ao usar a linguagem, escrever e divulgar sobre ela ou, caso a pessoa se sinta a vontade, pode ajudar fazendo alterações no próprio código fonte da linguagem. Dentro do código aberto, cada pessoa pode colaborar com o pouquinho que lhe cabe e toda a contribuição é importante, válida e ajuda o projeto a crescer!

## Legal! E, agora, como contribuir com o Pituguês?

### Primeiramente, precisamos entender onde está o Pituguês!&#x20;

O Pituguês é uma das linguagens de programação que existem dentro de um projeto maior que é a linguagem Delégua. Como o código-fonte de Pituguês está em Delégua, podemos dizer que ele é, até o momento, mais um "dialeto de programação", pois ele acaba por ser uma variante de uma linguagem maior.&#x20;

Dialetos nada mais são do que variações de uma mesma língua e na programação essa lógica também é válida. Pense em um país continental como o Brasil, dividido em 5 regiões e com 27 estados (incluindo o Distrito Federal). Embora existam discussões no campo da linguística se as variações do português no Brasil sejam dialetos ou apenas sotaques, é inegável que podemos notar diferenças bem acentuadas de expressões e entonações entre regiões diferentes ao ponto de um nortista ter dificuldade de entender a fala de um sulista e vice-versa.

E, talvez você não saiba, mas  já tenha tido contato com dialetos de outras linguagens de programação! Podemos dizer que TypeScript seria um dialeto de JavaScript, por exemplo, uma vez que usa toda a sintaxe do JavaScript e adiciona a tipagem à linguagem.&#x20;

Outro exemplo interessante são os dialetos PL/SQL e PL/pgSQL que unem a lógica de programação procedural com linguagem de consulta.  Se você for comparar, eles são bastante similares, mas ainda possuem diferenças em sua sintaxe que são bem características.

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
var idade = 15

se idade < 18:
    imprima('Menoridade')
senao:
    imprima('Maioridade')
```

Você pode notar que, sim, são linguagens bastante semelhantes, mas a principal diferença é que Pituguês vai utilizar a indentação para determinar os blocos de código, enquanto Delégua usa as chaves. Nesse exemplo, também podemos notar que a palavra-reservada para retornar um valor textual é diferente. Pituguês vai usar "imprima" e Delégua, "escreva".

### Onde fica o código-fonte de Pituguês e como ele é organizado?

Comentamos repetidas vezes que ele é um dialeto de Delégua e que vive dentro do código-fonte de Delégua. Você pode encontrar o [repositório do núcleo de Delégua disponível no Github](https://github.com/DesignLiquido/delegua) e, além disso, também está disponibilizado este [tutorial de como contribuir com o Pituguês](https://github.com/DesignLiquido/pitugues-docs/blob/principal/CONTRIBUTING.md).
