# Operadores de Endereço
Em programação, os operadores de endereço são usados para obter o endereço de memória de uma variável ou de um objeto em tempo de execução. Esses operadores são usados ​​para criar ponteiros, que são variáveis ​​que armazenam o endereço de memória de outra variável.

Existem dois tipos principais de operadores de endereço em C e C++:

1.  O operador de endereço unário `&`: esse operador é usado para obter o endereço de memória de uma variável. Por exemplo, se `x` é uma variável do tipo `int`, então `&x` retorna o endereço de memória onde `x` está armazenado. A sintaxe para usar o operador de endereço é a seguinte:
    
    ```C
    int x = 10; 
    int *ptr = &x;
	```
    
    Nesse exemplo, `&x` retorna o endereço de memória da variável `x`, que é atribuído ao ponteiro `ptr`.
    
2.  O operador de desreferência unário `*`: esse operador é usado para acessar o valor armazenado no endereço de memória apontado por um ponteiro. Por exemplo, se `ptr` é um ponteiro do tipo `int` que aponta para a variável `x`, então `*ptr` retorna o valor armazenado na variável `x`. A sintaxe para usar o operador de desreferência é a seguinte:
    
    ```C
	int x = 10; 
	int *ptr = &x; 
	int valor = *ptr;`
	```
    
    Nesse exemplo, `*ptr` retorna o valor armazenado na variável `x`, que é atribuído à variável `valor`.
    
Os operadores de endereço são muito úteis em programação, especialmente quando trabalhamos com estruturas de dados complexas, como matrizes, listas encadeadas e árvores. Eles permitem que criemos ponteiros para variáveis e objetos e acessemos seus valores e estruturas de forma eficiente e flexível.