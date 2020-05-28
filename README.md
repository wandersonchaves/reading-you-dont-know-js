# You Don't Know JS: Escopos & Clausuras

## Cápitulo 1: O que é Escopo?

Escopo é o conjunto de regras que determina onde e como uma variável (identificador) pode ser buscada. Esta busca pode ter como objetivo a atribuição de um valor para a variável, que chamamos de referência LHS (left-hand-side, ou, em tradução livre, lado esquerdo), ou então pode ter como objetivo a leitura do seu valor, que chamamos de referência RHS (right-hand-side, ou, em tradução livre, lado direito).

Referências LHS resultam das operações de atribuição. Atribuições relacionadas a *Escopo* podem ocorrer tanto com o uso do operador `=` quanto com a passagem de argumentos para uma função, quando os valores são atribuídos aos parâmetros definidos na sua declaração.

O *Motor* JavaScript primeiro compila o código para depois executá-lo, e, para isso, divide instruções como `var a = 2;` em dois passos distintos:

1. Primeiro, `var a` para declaração no *Escopo* atual. Isto é realizado bem no início, antes da execução do código.

2. Em seguida, `a = 2` para buscar pela variável (referência LHS) para então atribuir-lhe um valor, se encontrada.

Ambas buscas por referências LHS e RHS iniciam no *Escopo* atual de execução, e em caso de necessidade (ou seja, na ocasião de não serem encontradas por lá), seguem em frente pelos *Escopos* aninhados, um escopo (andar) de cada vez, buscando pelo identificador, até que atinjam o escopo global (último andar) e encerrem a busca, encontrando ou não a variável.

Referências RHS não satisfeitas resultam no lançamento de um `ReferenceError`. Referências LHS não satisfeitas resultam na criação automática e implícita de uma variável global de mesmo nome (caso não esteja em "Modo estrito" (strict mode) [^note-strictmode]) ou em um `ReferenceError` (caso esteja em "Modo estrito" (strict mode) [^note-strictmode]).

## Capítulo 2: Escopo Léxico

Escopo léxico significa que o escopo é definido pelo local onde funções são declaradas, através de decisões que são tomadas no momento da escrita do código. A fase de Análise Léxica da compilação é capaz de saber onde e como todos os identificadores são declarados, e portanto prever como serão buscados durante a execução.

Dois mecanismos do JavaScript podem "trapacear" o escopo léxico: `eval(..)` e `with`. O primeiro pode modificar o escopo existente (em tempo de execução) ao interpretar uma string de "código" que contenha uma ou mais declarações. O segundo cria um escopo léxico totalmente novo (também em tempo de execução) ao tratar a referência para um objeto como um "escopo" e as propriedades deste objeto como identificadores deste escopo.

O problema destes mecanismos é que ambos acabam com a capacidade do *Motor* de executar, em tempo de compilação, otimizações na consulta de escopo, porque o *Motor* precisa supor de modo pessimista que todas as otimizações serão inválidas. O código irá ser executado mais lentamente como resultado da utilização de qualquer uma destas funcionalidades. **Não utilize-as.**

## Capítulo 3: Escopo de função vs. Bloco de escopo

As funções são as unidades mais comuns de escopo em JavaScript. Variáveis e funções que são declaradas dentro de outra função são essencialmente "escondidas" de qualquer um dos "escopos" envolvidos, que é um princípio de design intencional de um bom software.

Mas as funções são de nenhuma maneira a única unidade do escopo. Escopo do bloco refere-se à ideia de que as variáveis e funções podem pertencer a um bloco arbitrário (geralmente, qualquer par de `{ .. }`) de código, em vez de apenas para a função envolvida.

Começando com ES3, a estrutura `try/catch` tem escopo do bloco na cláusula `catch`.

Em ES6, é introduzida a palavra-chave `let` (um primo para a palavra-chave `var`) para permitir que as declarações de variáveis em qualquer bloco arbitrário de código. `if (..) {let a = 2; }` Irá declarar uma variável `a` que, essencialmente, sequestrará o escopo do bloco `if`'s `{ .. }` e auto atribuíra-se lá.

Embora alguns parecem acreditar, o escopo do bloco não deveria ser tomado como uma substituição pura e simples de escopo da função `var`. Ambas as funcionalidades co-existem, e os desenvolvedores podem e deveriam usar ambas as técnicas de escopo da função e escopo do bloco, onde respectivamente apropriados para produzir melhor, mais código legível/sustentável.

## Capítulo 4: Hoisting

Podemos ser tentados a olhar para `var a = 2;` como sendo uma instrução, mas o *Motor* do JavaScript não vê dessa maneira. Ele vê `var a` e `a = 2` como duas instruções separadas, a primeira como uma tarefa da fase de compilação e a segunda como tarefa da fase de execução.

Isso nos leva à concluir que todas as declarações em um escopo, independente de onde elas aparecerem, são processadas *primeiro* antes do próprio código ser executado. Você pode entender esse processo como declarações sendo "movidas" para o topo de seus respectivos escopos, o qual nós chamamos de hoisting.

As próprias declarações são "elevadas", mas atribuições, mesmo atribuições de expressões de função, *não* são "elevadas".

Cuidado com declarações duplicadas, especialmente misturadas entre declarações de variável normal e de função -- há um certo perigo, caso isso aconteça!

## Capítulo 5: Clausuras de Escopo

Closure parece para os não-iluminados como um mundo místico à parte dentro do Javascript que apenas as poucas almas corajosas podem alcançar. Mas é na verdade apenas um padrão e quase um fato óbvio de como nós escrevemos código em um ambiente de escopo léxico, onde funções são valores e podem ser passados adiante à vontade.

**Closure é quando uma função consegue lembrar e acessar seu escopo léxico mesmo quando invocada fora do seu escopo léxico.**

Closures podem nos enganar, por exemplo, com loops, se não tivermos cuidado para vê-los e saber como eles funcionam. Mas eles também são uma ferramenta imensamente poderosa, permitindo padrões como os módulos em suas variadas formas.

Módulos requerem duas características chave: 1) uma função de encapsulamento externa ser invocada, para criar o escopo fechado 2) o valor de retorno da função de encapsulamento deve incluir referência para ao menos uma função interna que então tenha closure sobre o escopo privado interno do encapsulamento.

Agora nós podemos ver closures em todo nosso código existente, e temos a habilidade para reconhece-los e aproveitá-los para nosso próprio benefício!