# Operadores de Endereço
Em programação, os operadores de endereço são usados para obter o endereço de memória de uma variável ou de um objeto em tempo de execução. Esses operadores são usados ​​para criar ponteiros, que são variáveis ​​que armazenam o endereço de memória de outra variável.

Existem dois tipos principais de operadores de endereço em C e C++:

1.  O operador de endereço unário `&`: esse operador é usado para obter o endereço de memória de uma variável. Por exemplo, se `x` é uma variável do tipo `int`, então `&x` retorna o endereço de memória onde `x` está armazenado. A sintaxe para usar o operador de endereço é a seguinte:
    
    ```C
    int x = 10; 
    int *ptr = &x;
	```
    
    Nesse exemplo, `&x` retorna o endereço de memória da variável `x`, que é atribuído ao ponteiro `ptr`.
    
2. Operador de desreferência unário `*`: esse operador é usado para ==acessar o valor armazenado no endereço de memória apontado por um ponteiro==. Por exemplo, se `ptr` é um ponteiro `int` que aponta para a variável `x`, então `*ptr` retorna o valor armazenado na variável `x`. A sintaxe para usar o operador de desreferência é a seguinte:

```C
int x = 10; 
int *ptr = &x; 
int valor = *ptr;`
```
   
Nesse exemplo, `*ptr` retorna o valor armazenado na variável `x`, que é atribuído à variável `valor`.

---

## Operador de Endereço em C

Em C, o operador `&` é chamado de operador de endereço e é usado para obter o endereço de uma variável na memória. Quando usamos o operador `&` em uma variável, ele retorna o endereço de memória da variável. Por exemplo, se tivermos a seguinte declaração de variável:

``` C
int a = 10;
```

Podemos usar o operador `&` para obter o endereço de `a` na memória:

```C
printf("%p", &a);
```

O código acima imprimirá o endereço de `a` na memória em hexadecimal. O operador `&` é frequentemente usado em conjunto com ponteiros em C, para passar endereços de memória como parâmetros para funções, ou para armazenar endereços de memória em ponteiros.