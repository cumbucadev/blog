# Lexador

No nosso post anterior sobre como programar o Pituguês, aprendemos que ela é uma linguagem interpretada e que sua arquitetura de projeto é composta por três camadas: o Lexador, o Avaliador Sintático e o Interpretador. Cada uma delas possui sua responsabilidade para que possamos desenvolver uma linguagem de programação e executá-la e nós vamos começar a desbravar um pouquinho de cada uma delas, começando por este artigo em que falaremos do Lexador mostrar um pouquinho como podemos contribuir com o seu código.

Mas, primeiramente...

## O que é um Lexador?

Nada mais é do que um programa que irá percorrer e escanear, da esquerda para a direita, os caracteres da nossa linguagem. Nesta etapa, serão identificados os tokens e seu devido papel naquela instrução. Dentro do código-fonte de Pituguês, eles serão chamados, respectivamente, de  símbolos e tipos de símbolos.

Nós teremos diferentes categorias de símbolos (tokens):

### Palavras-reservadas

Também chamadas de _keywords_, são palavras que estão presentes na linguagem de programação e são utilizadas para se escrever instruções do que queremos que nosso programa faça. Por exemplo, palavras como "while", "for", "const", "else" e entre outras que já fazem parte da linguagem e não podem ser usada para nomear variáveis ou desenvolvimento de funções.

Aliás, as funções nativas de uma linguagem também são categorizadas como palavras-reservadas e não podem ser usadas para criar alguma outra instrução. Mas, não se preocupe, caso você não saiba que o nome que você deu a uma variável ou função não já exista na linguagem, ela mesma irá te avisar!

### Identificadores

### Constantes

### Operadores

### Lexemas

