# Operador de Desreferência

Em C, o operador de desreferência é um operador unário representado pelo caractere asterisco (`*`) que é usado em conjunto com um [[Funcionamento dos Ponteiros|ponteiro]] para acessar o valor da variável armazenado no endereço de memória apontado por esse ponteiro.

A **sintaxe** para usar o operador de desreferência em C é a seguinte:

```C
tipo_de_dado *ponteiro;
tipo_de_dado variavel;

ponteiro = &variavel; // atribui o endereço da variável ao ponteiro 
*ponteiro = valor; // atribui o valor ao endereço de memória apontado pelo ponteiro
```


Nesse exemplo, `ponteiro` é um ponteiro do tipo `tipo_de_dado`, que aponta para a variável `variavel`. O operador de endereço `&` é usado para obter o endereço de memória da variável `variavel`, que é atribuído ao ponteiro `ponteiro`.

O operador de desreferência `*` é usado para acessar o valor armazenado no endereço de memória apontado pelo ponteiro `ponteiro`. No exemplo acima, o valor `valor` é atribuído ao endereço de memória apontado pelo ponteiro `ponteiro` usando o operador de desreferência `*`.

Os operadores de desreferência são comuns em linguagens como C e C++, onde ponteiros são usados para manipular diretamente a memória. Eles permitem que os programadores acessem valores armazenados em diferentes endereços de memória e operem diretamente na memória de um programa. No entanto, o uso indevido de ponteiros e operadores de desreferência pode levar a erros graves, como violações de acesso de memória e vazamentos de memória, e requer cuidado e atenção especial.

---

Quando usamos o operador `*` em C, estamos realizando uma operação de desreferência (desfaz a referência armazenada em um ponteiro e obtém o valor apontado por ele), ou seja, estamos ==acessando o valor armazenado na posição de memória apontada por um ponteiro==.

Internamente, quando o operador `*` é aplicado a um ponteiro, o compilador sabe que precisa acessar o valor na posição de memória apontada pelo ponteiro. Ele utiliza a informação do tipo de dado que o ponteiro aponta para determinar o tamanho correto de memória a ser acessado.

O processo de desreferência envolve a seguinte sequência de ações:

1.  O compilador determina o **tamanho do tipo de dado** apontado pelo ponteiro.
2.  O compilador calcula o **endereço** de memória referenciado pelo ponteiro.
3.  O compilador **acessa o valor** armazenado na posição de memória calculada.

Em termos práticos, quando usamos o operador `*` para desreferenciar um ponteiro em uma expressão como `*p`, onde `p` é um ponteiro, o compilador acessa o valor armazenado na posição de memória apontada por `p` e o utiliza na expressão.

Por exemplo, considerando o código a seguir:

```C
int x = 10;
int *p = &x;
int y = *p;
```

O ponteiro `p` aponta para a variável `x`. Ao usar o operador `*p` na terceira linha, o compilador acessa o valor armazenado na posição de memória apontada por `p`, ou seja, o valor de `x`, e o atribui à variável `y`. Assim, `y` recebe o valor 10.
