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

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fdbcfb23-61a5-44a3-9a30-df9887d4c2b3/Untitled.png)

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