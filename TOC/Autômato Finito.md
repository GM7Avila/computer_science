# Autômato Finito

(#def.1) São autômatos que possuem um número finito de estados e trabalham com uma **entrada finita**. Eles podem ser divididos em duas categorias: autômatos finitos determinísticos (Deterministic Finite Automata - DFA) e autômatos finitos não determinísticos (Nondeterministic Finite Automata - NFA).

(#def.2) Um sistema de estados finitos é um **modelo matemático** que descreve um sistema (de estados finitos) com entrada e saída.

A saída é quem determina se a palavra pertence ou não pertence à [Linguagem](Linguagem.md). 

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
- [[Deterministic finite automaton]]
- [[Nondeterministic Finite Automaton]]

Tanto os autômatos finitos determinísticos (AFDs) quanto os autômatos finitos não determinísticos (NFAs) têm diversas aplicações práticas na área de Teoria da Computação e em outras áreas da ciência da computação, como:

- *Processamento de linguagens naturais*: AFDs e NFAs são usados em algoritmos de análise sintática para identificar se uma sentença ou expressão gramatical é válida em uma determinada linguagem. Por exemplo, um AFD pode ser usado para validar endereços de e-mail ou para reconhecer números de telefone em um texto.
 
- *Compiladores:* AFDs e NFAs são usados para reconhecer tokens em uma linguagem de programação e gerar a tabela de símbolos necessária para a compilação. Por exemplo, um NFA pode ser usado para reconhecer identificadores em uma linguagem de programação.
   
- *Verificação de modelos*: AFDs e NFAs são usados na verificação formal de sistemas para verificar se um modelo de sistema atende a uma determinada propriedade de segurança. Por exemplo, um NFA pode ser usado para modelar o comportamento de um sistema e verificar se ele viola uma propriedade de segurança, como a ausência de deadlocks.

- *Reconhecimento de padrões:* AFDs e NFAs são usados em algoritmos de reconhecimento de padrões para identificar sequências de caracteres em um texto ou imagem. Por exemplo, um NFA pode ser usado para reconhecer um padrão em uma imagem ou para identificar palavras em uma transcrição de áudio.

- *Análise léxica*: AFDs e NFAs são usados em algoritmos de análise léxica para identificar as unidades lexicais de uma linguagem. Por exemplo, um AFD pode ser usado para reconhecer palavras-chave em uma linguagem de programação.