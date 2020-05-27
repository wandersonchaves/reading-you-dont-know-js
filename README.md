# You Don't Know JS: Escopos & Clausuras

## Cápitulo 1: O que é Escopo?

A inclusão de variáveis em nossos programas gera perguntas mais interessantes como: onde essas variáveis vivem? Em outras palavras, onde elas são armazenadas? E, mais importante, como nossos programas as encontram quando eles precisam delas?

Estas perguntas mostram a necessidade de um conjunto bem definido de regras para armazenar variáveis em algum lugar, e para encontrar essas variáveis posteriormente. Nós iremos chamar esse conjunto de regras: Escopo.

### Teoria de Compiladores

Em um processo tradicional de uma linguagem compilada, um pedaço de código fonte, seu programa, vai tipicamente passar por três passos antes de ser executado, grosseiramente chamado "compilação":

1. **Tokenização/Análise Léxica:** quebrar uma string de caracteres em pedaços com algum significado (para a linguagem), chamados tokens. Por exemplo, considere o programa: `var a = 2;`. Esse programa provavelmente seria quebrado nos seguintes tokens: `var`, `a`, `=`, `2`, e `;`. Espaço em branco pode ou não ser mantido como um token, dependendo se tem ou não algum significado.

**Nota:** A diferença entre Tokenização e Análise Léxica é sutil e teórica, mas centraliza-se no fato desses tokens serem ou não identificados de uma maneira *stateless* ou *stateful*. Colocando de maneira simples, se o Tokenizador fosse invocar regras de análise stateful para saber se `a` deve ser considerado um token distinto ou apenas parte de outro token, isso seria Análise Léxica.

2. **Análise:** pegar um conjunto (array) de tokens e transformar isso numa árvore de elementos aninhados, que juntos representam a estrutura gramática do programa. Essa árvore é conhecida como "AST" (**A**bstract **S**yntax **T**ree, que, em tradução livre, significa: Árvore Sintática Abstrata).

A árvore para `var a = 2;` pode começar com um nó de nível superior chamado `VariableDeclaration`, que tem um nó filho chamado `Identifier` (cujo valor é `a`), e outro nó filho chamado `AssignmentExpression` que por sua vez tem um filho chamado `NumericLiteral` (cujo valor é `2`).

3. **Geração de código:** o processo de obter uma AST e transformar isso em código executável. Esta parte varia muito dependendo da linguagem, da plataforma-alvo, etc.

Então, em vez de focar em detalhes, nós vamos apenas olhar superficialmente e dizer que existe uma forma de obter nossa AST descrita acima para `var a = 2;` e transformá-la em instruções de máquina para de fato criar uma variável chamada `a` (incluindo a reserva de memória, etc), e então armazenar um valor em `a`.

**Nota:** Os detalhes de como o motor administra recursos do sistema estão além do que iremos cobrir, então nós vamos apenas considerar que esse motor é capaz de criar e armazenar variáveis conforme necessário.
