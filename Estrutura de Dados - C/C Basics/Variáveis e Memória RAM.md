# Variáveis e Memória RAM

> Todo programa executado no computador é executado a partir da memória RAM

Toda variável declarada em C, sempre será associada á:
- Um nome
- Um tipo
- Um valor
- Um *endereço de memória*

```C
//exemplo 1 
int a = 10; 
int b, c;

b = 20; 
c = a + b;
```

- O computador sempre busca o primeiro endereço de memória RAM disponível para guardar o valor das variáveis.
- Toda variável tem um tipo, e cada tipo de dados possui um tamanho necessário para ser acessado na memória. Por exemplo: inteiros necessitam de 4 bytes na memória RAM, ou então 32 bits - todo tipo de dado tem um tamanho.
- O próximo endereço disponível no exemplo, para a alocação da variável ``inteiro`` (precisando de 4 bytes na memória) b, será 3020 + 4 bytes = byte 3024;
- Note, o endereço 3020 é composto por 4 bytes de memória: 3020, 3021, 3022, 3023. Ou seja, o valor 10 em binário é armazenado ocupando todo esse espaço na memória: 0000000…1010
- Como não atribuimos nenhum valor inicialmente para b e c, a memória é ocupada pelo que é chamado de lixo de memória (representado no exemplo por ####);
- Usando & no nome da variável em C, conseguimos pesquisar o endereço de memória da variável.
- Quando os valores são atribuidos as variáveis, eles são então alocados na memória.
- A variável inteira fica salva no byte que representa o seu respectivo valor em binário. É aonde a variável reside.
- Exemplo variável b → ocupa 4 bytes a partir de 3024: 3024, 3025, 3026, 3027 (cada um desses endereços de memória correspondendo a um byte); 20 em binário em 4 bytes: 000000…10100 → 32 bits.

![[Pasted image 20230216195307.png]]
![[Pasted image 20230216195315.png]]
![[Pasted image 20230216195323.png]]
![[Pasted image 20230216195332.png]]
![[Pasted image 20230216195339.png]]

![[Pasted image 20230216195352.png]]

## Armazenamento de variáveis inteiras
Em uma alocação de memória do tipo int, o número "10" é salvo em forma de bits na memória do computador. Um inteiro em C geralmente ocupa 4 bytes (32 bits) de memória.

O número 10 em binário é representado como ``1010``, portanto, em uma alocação de memória do tipo int, o número 10 seria armazenado como:

00000000 00000000 00000000 0000**1010**

Note que essa representação pode variar dependendo do sistema em que o código C está sendo executado, como a ordem dos bytes (little-endian ou big-endian), mas a essência da representação permanece a mesma.

## Armazenamento de Strings
Em C, não existe um tipo de dados nativo para representar uma string, como existe em outras linguagens de programação. Em vez disso, uma string em C é representada como um array de caracteres, que é um conjunto de elementos do tipo char, onde cada caractere representa um símbolo na string.

A string em C é armazenada como uma sequência de caracteres terminada com o caractere nulo (`'\0'`), que indica o final da string. Portanto, para armazenar uma string de N caracteres em C, é necessário alocar um array de N+1 elementos do tipo char, onde o último elemento é o caractere nulo que indica o final da string.

Por exemplo, a string "hello" em C seria armazenada em uma alocação de memória como o seguinte array de caracteres:

``'h' 'e' 'l' 'l' 'o' '\0' ``
0x68 0x65 0x6C 0x6C 0x6F 0x00

Onde os valores hexadecimais correspondem ao código ASCII dos caracteres. Note que o caractere nulo ``('\0')`` é adicionado automaticamente pelo compilador no final da string, indicando o seu fim.
```C
char str[6] = {'h', 'e', 'l', 'l', 'o', '\0'};
```

Em alternativa, também é possível declarar uma string em C usando uma notação de string literal, que é um texto delimitado por aspas duplas. Por exemplo:
```C
`char str[] = "hello";`
```
Neste caso, o compilador irá automaticamente alocar um array de tamanho suficiente para armazenar a string, incluindo o caractere nulo (`'\0'`). Observe que o tamanho do array é definido como 6, o que é o número de caracteres na palavra "hello" mais o caractere nulo.

## Armazenamento de Char
Em C, um único caractere (tipo `char`) é alocado em 1 byte (8 bits) de memória. O valor do caractere é representado em forma de bits na memória.

O valor de um caractere em C pode ser especificado usando seu valor numérico na tabela ASCII ou usando seu caractere de escape. Por exemplo, o caractere 'A' tem um valor numérico de 65 em ASCII. Portanto, para armazenar o caractere 'A' em uma alocação de memória do tipo `char` em C, o valor numérico 65 seria representado em forma de bits na memória. Em binário, 65 é representado como 01000001.

Assim, a representação do caractere 'A' em uma alocação de memória do tipo `char` em C seria a seguinte:

01000001

Cada bit representa um valor binário, com o valor mais significativo à esquerda. Observe que esse é apenas um exemplo para o caractere 'A', e a representação binária pode ser diferente para outros caracteres, dependendo de seu valor numérico na tabela ASCII.

