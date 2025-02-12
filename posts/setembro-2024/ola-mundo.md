# "Olá, Mundo!"

Eu sempre fui alguém que gosta do novo: seja aprender um novo esporte, experimentar uma nova receita ou até descobrir algo completamente desconhecido. Me recordo de quando criança, o primeiro dia de aula me trazia uma mistura de alegria e ansiedade, aquele friozinho na barriga por estar começando algo novo. Quando nos tornamos adultos, as primeiras vezes parecem menos frequentes e, talvez, menos emocionantes... até eu encontrar a programação.

Minha "primeira vez" com programação aconteceu quando recebi um inesperado 'sim' para participar de um curso de TI. Foi em um ambiente completamente diferente do que eu estava acostumada: um processo seletivo com testes de aptidão, texto sobre mim e análise de fit cultural. Quando finalmente cheguei ao primeiro encontro online, meu coração estava disparado, assim como nos primeiros dias de aula da infância. E que surpresa maravilhosa foi ver que o grupo era formado por mulheres de diferentes idades e histórias, cada uma com uma trajetória única, mas com a mesma vontade de aprender sobre aquele novo "assunto".

Mal sabia eu que, a partir daquele dia — em que fiz meu primeiro "Olá, Mundo!" —, meu destino se transformaria de forma tão maravilhosa.

Popular entre pessoas iniciantes e avançadas, a expressão "Olá, Mundo!" (em inglês: Hello, World!) - e algumas variações - ficou conhecida justamente por ser o primeiro contato das pessoas desenvolvedoras com uma linguagem de programação.&#x20;

Utilizada pela primeira vez pelo cientista da computação canadense Brian Kernighan, em 1973, na pesquisa “A Tutorial Introduction to the Programming Language B” (publicada antes do livro “C Programming Language” que é um dos mais conhecidos na área). A "expressão pegou" de uma forma tão grande, que até mesmo em outras linguagens virou como um "ritual de iniciação", e até hoje, mais de 50 anos depois ainda é comumente utilizado em cursos e tutoriais.&#x20;

Há também uma lenda pairando pelos terminais no modo dark (rs') que conta: uma vez, um programador iniciou seus estudos sem dar as devidas 'Boas-vindas' e, até hoje, é assombrado todos os dias por um bug diferente! :ghost: (Melhor não testar, né?!)

Em algumas linguagens de alto nível, como o nosso querido [**Python**](https://pt.wikipedia.org/wiki/Python) - feito para ser compreensível por todas as pessoas - a primeira vez pode ser mais fácil e simples, assim como aprender a fazer gelo!

```python
// # Hello world in Python 3

print("Hello World")
```

Já em outras linguagens - como o [**Java**](https://pt.wikipedia.org/wiki/Java_\(linguagem_de_programa%C3%A7%C3%A3o\)), a primeira vez pode ser um pouquinho mais complicada, mas com um pouco de teoria e prática você consegue compreender e replicar. É mais ou menos como andar de bicicleta...

```java
//// Hello World in Java

class HelloWorld {
  static public void main( String args[] ) {
    System.out.println( "Hello World!" );
  }
}
```

Agora, dar os primeiros passos em uma linguagem de baixo nível (significa que está mais próxima da linguagem em nível de máquina e possui menor abstração), até mesmo um simples "Hello, World!" pode se tornar algo bem complexo...&#x20;

```wasm
; Hello World for Intel Assembler (MSDOS)

mov ax,cs
mov ds,ax
mov ah,9
mov dx, offset Hello
int 21h
xor ax,ax
int 21h

Hello:
  db "Hello World!",13,10,"$"// Some code
```

Começar em uma área nova sem experiência alguma pode ser assustador (assim como esse Hello World em [**Assembly**](https://pt.wikipedia.org/wiki/Linguagem_assembly)), mas ter um apoio no foco e direcionamento, praticar em projetos reais e compartilhar as dores e conquistas do dia a dia pode ser transformador na sua carreira. E é por isso que estamos aqui.

No início temos medo de errar, e é normal. Mas, se eu pudesse te dar uma dica (para programação e para a vida), ela seria: "vai com medo mesmo!". Fato é que ninguém nasce sabendo, mesmo que alguém seja expert, já passou pela etapa de ser iniciante em algo, e realmente tá tudo bem errar - faz parte do processo de aprender.  E é por isso que aqui na Cumbuca Dev prezamos pelo aprendizado prático, com ambientes seguros e inclusivos para todas as pessoas, assim é possível aprender fazendo - errando, corrigindo e entendendo cada passo dentro do **seu** processo de aprendizado.

Tanto quando falamos de primeiras oportunidades — em que se trabalha fora deste mercado específico de TI — quanto para quem já está empregado na área, mas anseia ascender ou transicionar de carreira. Acreditamos que nossa metodologia pode te ajudar nesse início, seja ele qual for, e a partir dela melhorar seu panorama: a experiência de verdade em projetos utilizados por outras pessoas quebra o ciclo da não experiência e te aproxima a oportunidades; por consequência você faz uma escolha mais consciente e certeira (a vida real é bem diferente do que o vendedor de curso de "Do 0 à R$100.000,00 em 6 meses" diz) ; além de que sentir seu valor por meio das contribuições à comunidade deixam esta "Primeira Vez" muito mais recompensadora.&#x20;

Este também é oficialmente o "Olá, Mundo!" da Cumbuca Dev, e estar à frente desse projeto com a Camila tem feito todos os meus dias serem emocionantes como o primeiro dia de aula! Diariamente aprendo algo novo, conheço alguém da comunidade (aka pessoa maravilhosa), recebo uma mensagem inspiradora, penso em novas ideias para poder contribuir no aprendizado de outras pessoas e tantas outras novas atividades ligadas a empreender...&#x20;

A Cumbuca além de me ensinar todos os dias, deu próposito para minha vida e hoje poder ser parte do início de um novo desafio e (quem sabe!) uma grande transformação na vida de outras pessoas torna essa jornada ainda mais especial.

Se você chegou até aqui, desejo que tenha muitas "primeiras vezes" especiais, daquelas que fazem o coração bater mais forte. Que venham os novos começos, e que cada um deles seja o início de algo transformador em sua vida!

**Agora é sua vez!** Conte para a gente: como foi a seu primeiro "Olá, Mundo!" na programação? Compartilhe suas histórias e se junte à nossa comunidade para aprender, crescer e transformar o futuro!



Escrevendo pela primeira vez em um blog de tecnologia,

Maria Antônia Maia.&#x20;



