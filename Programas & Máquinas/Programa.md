# Programas

**Programa** → Conjunto de operações e testes compostos de acordo com uma estrutura de controle, que define o que deve ser executado. O tipo de estrutura de controle determina a classificação dos programas.

**Máquina** → Interpreta os programas de acordo com os dados fornecidos, desde que possua uma interpretação para cada operação ou teste que constitui o programa.

O Objetivo de uma máquina é surprir todas as informações necessárias para que a computação de um programa possa ser descrita.

Cabe a máquina suprir o significado aos identificadores das operações e testes.

-   **Maquinas equivalentes:** máquinas capazem de simular uma às outras.

**Programa para uma máquina:** P é um programa para uma máquina M se todos os identificadores de operação e testes usados em P estiverem definidos em M através das correspondentes funções.

---

**Estrutura de Controle** → Instruções que definem a ordem em que um programa deve ser executado:

-   **recursivo**: baseado em sub-rotinas recursivas.
-   **monolítico**: baseada em desvios condicionais e incondicionais;
-   **iterativo**: estruturas de iteração de trechos de programas;

A classe dos programas iterativos está contida na classe dos programas monolíticos, que por sua vez está contida na classe dos programas recursivos:

-   _iterativos_ **⊂** _monolíticos_ **⊂** _recursivos_

**Computação** → Sequência de estados assumidos por uma máquina durante a execução do programa, considerando um valor inicial. É o histórico do funcionamento da máquina para o programa, considerado um valor inicial.

**Função computada** → Função que associa valores de entrada com valores de saída produzidos pela execução de um programa em uma máquina.

-   Conceitos de Função computada
    -   **programas fortemente equivalentes:** funções computadas que coincidem para qualquer máquina.
    -   **programas equivalentes:** funções computadas que concidem para uma só máquina.
    -   **máquinas equivalentes:** se as máquinas podem simular umas às outras.

---

## Instruções Rotuladas

-   Operação: a ser executada;
-   Teste: Determina um desvio **condicional;**
-   Parada: Desvio incondicional.

## 2.1.1 Monolítico

```xml
01.  faça F vá_para 2
02.  se T1 então vá_para 1 se_não vá_para 3
03.  faça G vá_para 4
04.  se T2 então retornar se_não vá_para 1
```

-   rótulos: `01, 02, 03, 04`
    
-   instrução rotulada: `01. faça F vá_para 2`
    
-   Um programa monolíticio P é um par ordenado $P = (I,r)$
    
    -   I é um conjunto finito de instruções rotuladas, **sendo que não existem duas instruções diferentes com mesmo rótulo.**
    -   r é um rótulo distinguido denominado **rótulo inicial** que identifica a instrução rotulada em I
    -   um rótulo, é dito rótulo final se é referenciado por pelo menos uma instrução de I, mas não rotula qualquer instrução de i.

## 2.1.2 Iterativo
Como e por que surgiu?

-   **origem**: solucionar problemas decorrente da manutenção dos prgoramas monolíticos (no qual existe grande liberdade de desvios incondicionais o que pode ocasionar **“quebras de lógica”** com frequência.
-   substituir desvios incondicionais por estruturas de controle de ciclos/repetições → melhor estrutura de desvio.
-   essas noções deram origem o que hoje é chamado de **Programação Estruturada.**

São baseados em três mecanismos de composição(sequencias) de programas:

-   **sequencial**: composição de dois programas, resultando em um terceiro, cujo efeito é execução do primeiro e depois a execução do segundo
-   **condicional**: “ “ “ “ cujo efeito é a execução de somente um dos dois programas componentes dependendo do **resultado de um teste.**
-   **enquanto**: “ “ “ “ composição de um programa, resultando em um segundo, cujo efeito é a execução repetidamente do programa componente
-   até: enquanto mas o resultado do teste é falso

## 2.1.3 Recurssivo
Programas recursivos são aqueles que utilizam a técnica de [[Iteratividade x Recursividade|recursão]], ou seja, a definição de uma função que chama a si mesma dentro de sua própria definição. A recursão é uma técnica poderosa para resolver problemas que podem ser expressos de maneira recursiva, ou seja, em termos de subproblemas menores do mesmo tipo.

Na prática, a recursão é usada em muitos algoritmos e programas de computador, como em algoritmos de busca em profundidade em grafos, cálculo de fatorial, ordenação de árvores, entre outros. Além disso, muitas linguagens de programação suportam a recursão e a incentivam como uma técnica poderosa de resolução de problemas.

Porém, é importante lembrar que a recursão pode levar a problemas de desempenho e uso excessivo de memória, especialmente se não for implementada de maneira eficiente. Além disso, é necessário garantir que a recursão tenha um ponto de parada definido para evitar loops infinitos.

### 2.1.3.1 Fatorial
Um exemplo clássico de função recursiva é a função fatorial, que pode ser definida de forma recursiva da seguinte maneira:

```JavaScript
function fatorial(n) {
  if (n <= 1) {
    return 1;
  } else {
    return n * fatorial(n - 1);
  }
}

// Exemplo de uso
console.log(fatorial(5)); // resultado: 120
```

Nesse exemplo, a função `fatorial` é definida de forma recursiva, recebendo como argumento um número inteiro `n`. 

Se `n` for menor ou igual a 1, a função retorna 1 (caso base da recursão). Caso contrário, a função retorna o valor de `n` multiplicado pelo resultado da chamada recursiva de `fatorial(n - 1)`, ou seja, o fatorial de `n - 1`. 

O processo recursivo continua até chegar no caso base, quando a função começa a retornar os resultados e desempilhar as chamadas da pilha de execução.

#### pilha
No caso de funções recursivas, a [[Pilha|pilha]] de execução é usada para manter o controle do fluxo de execução da função. Quando uma função é chamada, uma nova entrada é adicionada à pilha de execução, que contém informações como o endereço de retorno e os valores dos parâmetros da função.

No caso da função fatorial, cada chamada recursiva adiciona uma nova entrada à pilha de execução. Quando a função atinge o caso base (n <= 1), ela começa a retornar os resultados e desempilhar as chamadas da pilha de execução.

Por exemplo, se chamarmos a função fatorial(5), a pilha de execução seria preenchida da seguinte forma:

scssCopy code

`1. fatorial(5) 2. fatorial(4) 3. fatorial(3) 4. fatorial(2) 5. fatorial(1)`

A primeira entrada adicionada à pilha de execução é a chamada de fatorial(5). Em seguida, a chamada de fatorial(4) é adicionada à pilha, e assim por diante até que seja atingido o caso base (n <= 1).

A partir desse ponto, a função começa a retornar os resultados, e cada chamada desempilha a próxima chamada da pilha de execução. O resultado de cada chamada recursiva é então usado para calcular o resultado da chamada anterior, até que o resultado final seja calculado e retornado.

Assim, a pilha de execução é uma ferramenta essencial para o controle de fluxo em funções recursivas, permitindo que o resultado final seja calculado de forma eficiente a partir de múltiplas chamadas recursivas.