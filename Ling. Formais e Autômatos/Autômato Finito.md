# Autômato Finito
Um sistema de estados finitos é um **modelo matemático** que descreve um sistema (de estados finitos) com entrada e saída.

A saída é quem determina se a palavra pertence ou não pertence à [Linguagem](./Linguagem). 

- **Estado**: Mostra a situação atual e passada do sistema.
- **Ação**: determina a transição de um estado para outro.

Um sistema de estados finitos possui uma quantidade de memória limitada.

- Não existe memória de estados anteriores
- ***Podemos dizer que é um computador que tem apenas 1 bit de memória (0 / 1)***

- Processador das [[Linguagens Regulares]] 

---

## Componentes
- q0 é definido como estado inicial
- para cada símbolo, definimos uma **função de trainsição - δ** de saída para outros estados.
- Estados de **aceitação** ou **final**.

## AF como Processadores de cadeia
- a saída será **aceita** ou **rejeitada** a cada w. 
- começa no ``q0`` e ``w = w1 w2 ... wn`` é lido da esquerda para a direita.
- após ler cada símbolo, move-se de um estado para outro conforme a função de transição.
- uma **cadeia w é aceita** se ao processamors ``w1 w2 ... wn`` o AF chega em um estado de aceitação.

## Classficação
- [Autômato Finito Determinístico](./AFD)