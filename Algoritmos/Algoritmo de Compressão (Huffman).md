# Algoritmo de Compressão de Dados (Huffman Code)

## 1. Conceito
- Processo de compactação de dados **sem perda** de informação;
- Aplicação principal: texto;
- Proposto por *David A. Huffman* (1952) - MIT;
- Faz uso de diversas [[obsidian-github/computer_science/Estrutura de Dados - C/Estrutura de Dados I/README|estrutura de dados]] como: tabelas, listas encadeadas / fila e árvore binária.
---
## 2. Etapas do Algoritmo
- 2.1. Determinar a frequência de cada "elemento" e ordenar os elementos;
- 2.2. Montar uma [[Árvore Binária]] agrupando os símbolos de acordo com a frequência;
- 2.3. Percorrer a árvore para codificar ou decodificar os dados.

==**Ex.:  Comprimir a frase "Vamos aprender a programar"**==; 
- Ocorrência dos caracteres: 25 x **8 bits** = **200 bits**;

**VALOR NA TABELA ASCII** 
V = 0101 0110
a = 0110 0001
m = 0110 1101
o = 0110 1111
s =  0111 0011
' ' =  0010 0000
[...]
### 2.1.1. Obtendo a Frequência de cada elemento da frase**
- V = 1
- a = 5
- m = 2
- o = 2
- s = 1
- ' ' = 3
- p = 2
- r = 4
- e = 2
- n =1
- d = 1
- g = 1
### 2.1.2. Ordenando os elemento pela frequência** 
- Ordenação (crescente ou decrescente) em uma [[Fila|fila]], onde cada elemento da fila serão nós que vão compor a **Árvore de Huffman**;
- Os nós contém o caractere e a frequência que eles aparecem (ex.: `[V=1]`);
- **OBS:** Devemos remover os elementos sempre pelos elementos de menor frequência
- Fila Ordenada: `[[V=1], [s=1], [n=1],...,[e=2],[''=3],[r=4],[a=5]]`;

### 2.2. Criando a Árvore de Huffman
- Em seguida iremos remover os elementos de dois em dois. Esses elementos serão nós folha, e filhos de um **nó pai neutro - intermediário** (representado na imagem por +).
- O nó intermediário possui **frequência** igual a soma da frequência de seus nós filhos - exemplo da imagem: `[V=1]` + `[s=1]` = `[+=2]`

![[Pasted image 20231010123147.png]]

- O próximo passo, é inserir de forma ordenada o nó pai na lista encadeada, e repetir o processo até que a lista tenha apenas um elemento (**raiz**).

![[Pasted image 20231010123818.png]]

### 2.3.1. Percorrer a árvore para codificação/decodificação

#### codificando
- Com a árvore montada, podemos montar o ==dicionário== percorrendo a árvore para cada elemento.
- Para isso devemos percorrer a árvore até os nós folhas (caracteres) e salvar em uma tabela/dicionário o novo código (substituindo o ASCII).
- A codificação se dá pela atribuição de zeros e um ao percorrer a árvore da seguinte maneira: salvamos zero para cada nó a esquerda que percorremos e um para cada nó a direita. 
- Assim, para chegar ao elemento folha `[a=5]` percorremos dois nós a esquerda: 00 (2 bits); este será o novo valor para aquele caractere, substituindo o antigo valor `0110 0001`da tabela ASCII, que ocupava 1 byte (compressão de dados).

![[Pasted image 20231010124048.png]]
#### decodificando
A frase em questão codificada é representada por: 
- `111100010011100111110110011011011110010001011110101`

Para decodificar devemos percorrer a árvore seguindo os comandos partindo da raiz. Para os valores 1 percorremos à direita, e para os valores 0 percorremos à esquerda, até que se encontre um nó folha. Ao encontrar o nó folha (caractere) retornamos o caractere em questão e voltamos para a raiz continuando o processo.

> Vídeo aula: https://www.youtube.com/watch?v=o8UPZ_KDWdU

---
## 3. Conclusão
- Antes da compressão: 25 * 8 bits = **200 bits**;
- Depois da compressão: somatório dos bits = **85 bits**;
- Compressão de **mais de 50%**.

![[Pasted image 20231010124854.png]]

### Resumo do passo a passo
1. Ler o texto e gerar a tabela de frequência
2. Gerar a lista/fila com Nós de árvore
3. Gerar a árvore de Huffman
4. Gerar a tabela de códigos (dicionário)
5. Codificar o texto
6. Decodificar o texto

---

## 4. Implementação
### Tabela
- A tabela pode ser um vetor de inteiros de tamanho 256, com índices no intervalo entre 0 e 255 abrangendo todos os índices da tabela ASCII. `#define TAM 256`

