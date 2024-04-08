# Pilhas 📚
A [[Pilha]] é uma estrutura de dados que serve como uma coleção de elementos, permitindo o acesso somente a partir do último dado inserido.

## Aplicações
- Avaliação de Expressões e [[Parsing Sintático]];
- Algoritmos de [[Backtracking]];
- Gerenciamento de Memória em Tempo de Compilação;
- Implementação de diversos algoritmos como Graham scan;
- Operações de *undo* e *redo* em aplicações;
- Controle de navegação em browser;
- Endereçamento de instruções em microprocessadores;
- Análise de expressões aritméticas.

## Operações
- **Push**: colocar um novo elemento sob a pilha.
- **Pop**: remover o elemento do topo da pilha.
- O único elemento acessível na pilha é o elemento do topo (top).

![[Pasted image 20230503153928.png]]

Além dessas operações, existem algumas outras operações comuns implementadas em pilhas, como **peek** (leitura do valor armazenado no top, sem no entanto remove-lo - às vezes chamado de top), **isFull**, **isEmpity**.

## Listas, Pilhas e Filas 

### Listas
- Os dados são armazenados de forma sequencial, em estruturas chamadas de nós;
- O principal tipo de lista, é a **lista encadeada** (linked list);
- Os dados são armazenados em nós, estruturas individuais compostas por um campo de informações e um campo de endereço (ponteiro de ligação - link), que conecta ao próximo nó na lista.
- A lista pode ser simples ou dupla. Uma lista duplamente encadeada contém também ponteiros para elementos anteriores.
- Uma lista encadeada também pode ser do tipo Circular.
- Amplamente empregadas na implementação de pilhas e filas.

- linked list:
![[Pasted image 20230504004651.png]]

> https://youtu.be/OwiHoj-mAi8?list=PLucm8g_ezqNpHdoSlPrLMB1Ga8dBrNRsz&t=274

#### operações em lista
- get()
- insert()
- remove()
- removeAt()
- replace()
- size()
- isEmpty()
- isFull()