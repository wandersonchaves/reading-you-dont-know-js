# Capítulo 1: Assincronia: Agora & Depois

Um programa JavaScript é (praticamente) sempre quebrado em dois ou mais pedaços, onde o primeiro pedaço roda agora e o próximo roda depois em resposta a um evento. Apesar do programa ser executado pedaço a pedaço, todos eles compartilham o mesmo acesso ao estado e escopo do programa, então cada modificação ao estado é feito em cima do estado anterior.

Sempre que existem eventos a serem executados, o loop de eventos roda até a fila estar vazia. Cada iteração do loop de eventos é um "tick". Interação do usuário, IO, e temporizadores enfileiram eventos na fila de eventos.

Em qualquer momento, apenas um evento pode ser processado da lista por vez. Enquanto um evento executa, o processamento pode, direta ou indiretamente, causar um ou mais eventos subsequentes.

Concorrência é quando duas ou mais cadeias de eventos se intercalam ao longo do tempo, de maneira que de uma perspectiva ampla, elas aparentem estar rodando simultaneamente (apesar de que em qualquer momento apenas um evento é processado).

Frequentemente é necessário fazer algum tipo de coordenação de interação entre "processos" concorrentes (diferentes de processos de sistema operacional), por exemplo, para garantir ordenamento ou prevenir "condição de corrida". Tais "processos" também podem cooperar ao se separar em pedaços menores e permitir que outros "processos" intercalem.

# Capítulo 2: Callbacks

Os retornos de chamada são a unidade fundamental de assincronia no JS. Mas eles não são suficientes para o cenário em evolução da programação assíncrona à medida que o JS amadurece.

Primeiro, nossos cérebros planejam as coisas de maneira semântica sequencial, bloqueadora e de encadeamento único, mas os callbacks de chamada expressam o fluxo assíncrono de uma maneira não linear e não sequencial, o que dificulta muito o raciocínio sobre esse código. Ruim raciocinar sobre código é um código ruim que leva a erros ruins.

Precisamos de uma maneira de expressar assincronia de maneira mais síncrona, seqüencial e bloqueadora, assim como nossos cérebros.

Segundo, e mais importante, os callbacks de chamada sofrem *inversão de controle*, pois implicitamente cedem controle a outra parte (geralmente um utilitário de terceiros que não está sob seu controle!) Para invocar a *continuação* do seu programa. Essa transferência de controle nos leva a uma lista preocupante de problemas de confiança, como se o callback de chamada é chamado mais vezes do que o esperado.

É possível inventar uma lógica ad hoc para resolver esses problemas de confiança, mas é mais difícil do que deveria e produz código mais complicado e mais difícil de manter, além de código que provavelmente não está suficientemente protegido desses perigos até que você seja visivelmente mordido pelo código. insetos.

Precisamos de uma solução generalizada para **todos os problemas de confiança**, que possam ser reutilizados para o número de callbacks de chamada que criarmos, sem toda a sobrecarga extra.

Precisamos de algo melhor do que callbacks de chamada. Eles nos serviram bem até esse ponto, mas o *futuro* do JavaScript exige padrões assíncronos mais sofisticados e capazes. Os capítulos seguintes deste livro mergulharão nessas evoluções emergentes.

# Capítulo 3: Promises

Promessas são incríveis. Usa-os. Eles resolvem a *inversão de problemas de controle* que nos atormentam com código apenas de retornos de chamada.

Eles não se livram dos retornos de chamada, apenas redirecionam a orquestração desses retornos para um mecanismo intermediário confiável que fica entre nós e outro utilitário.

As cadeias de promessas também começam a abordar (embora certamente não perfeitamente) uma maneira melhor de expressar o fluxo assíncrono de maneira sequencial, o que ajuda nossos cérebros a planejar e manter melhor o código JS assíncrono. Veremos uma solução ainda melhor para esse problema no próximo capítulo!

