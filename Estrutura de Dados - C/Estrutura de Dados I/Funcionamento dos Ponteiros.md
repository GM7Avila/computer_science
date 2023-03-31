# Ponteiros
São variáveis que armazenam o [[Variáveis e Memória RAM|endereço de memória]] de outra variável. Ou seja, um ponteiro aponta para uma outra variável, permitindo que seu conteúdo possa ser acessado e modificado indiretamente.

## Declarando ponteiros
Um ponteiro é declarado usando um asterisco (`*`) antes do nome da variável. Por exemplo:
```C
int *ptr
```
Nessa declaração ``int`` é o tipo da variável o ponteiro ``ptr`` aponta. O asterisco indica que ``ptr`` é um ponteiro - int pointer.

Para inicializar um ponteiro, é necessário atribuir a ele o endereço de memória de uma outra variável usando o [[Operador de endereço|operador de endereço]] (`&`).
```C
int x = 10;
int *ptr = &x;
```
Nesse exemplo, o ponteiro `ptr` agora aponta para a variável `x`. É possível acessar o valor da variável `x` indiretamente usando o ponteiro `ptr` e o [[Operador de desreferência|operador de desreferência]] (`*`). 

Por exemplo, para obter o valor de `x` usando o ponteiro `ptr`, você pode fazer o seguinte:
```JAVA
int valor = *ptr;
```
Nesse exemplo, o operador `*` é usado para acessar o valor da variável para a qual o ponteiro `ptr` aponta, que é a variável `x`.

## Múltiplos ponteiros
```C
int a = 10;
int *p1 = NULL;
int *p2;

p1 = &a;
p2 = p1;
*p2 = 4;
```
- `a` é salvo no seu endereço (ex.: **5000**) com o seu valor inteiro (`10`).
- `p1` é um ponteiro que guarda variáveis do tipo inteiro, ela ficará salvo no próximo endereço disponível (como a foi salvo em **5000** e ocupa **4 bytes**, fica diosponível **5004**)
-   os ponteiros ocupam **8 bytes** de memória, então o intervalo de memória que p1 ocupa é 5004-5012;
- como `*p1` recebe NULL ele não está apontando para lugar nenhum (não há lixo de memória - terra)
- `&a` = 5000, `&p1` = 5004, `&p2` = 5012
- `p2` não está atribuido a `NULL`, então ele está salvando **“lixo de memória (memory garbage)”**.

-   valor das variáveis:    
    -   `a` ⇒ `10` (inteiro)
    -   `p1` ⇒ `NULL`
    -   `p2` ⇒ `####`

-   em seguida `p1` recebe o **endereço de memória** da variável `a` (`&a`) ⇒ `p1 = &a`
-   o valor em `p1`, que antes era `NULL` passa a ser o endereço de `a`, ou seja, **5000.**
-   `p2` guarda o mesmo endereço de memória que `p1` está guardando, `p2` → **5000**;
-   conteúdo de p2 (endereço de um inteiro) = 5000;
    -   **_p2 = _(5000)__ **= a = 10** //ir até aonde p2 está apontando e pegar o conteúdo
    -   ***p2 = 4;** //substitui o valor de 10 por 4
        -   `a` ⇒ `4`
        -   `p1` ⇒ `4`
        -   `p2` ⇒ `4`

> 🚨**ATENÇÃO!**
>  `*p1 = &a` atribui o endereço de `a` ao ponteiro `p1`, enquanto `*p1 = a` atribui o valor de `a` à posição de memória apontada por `p1`.

## Ponteiro de ponteiros
### exemplo 1
- Ao analisar o tipo de um ponteiro, devemos analisar o conteúdo sempre da direita para a esquerda. Nesse sentido, `int *` é um ponteiro de inteiro.
- Vejamos um exemplo onde se aplica ponteiro de ponteiros:
```C
#include <stdio.h>
#include <stdlib.h>

int main (){
    
    int a = 10;
    int *p1 = &a;
    int **p2 = &p1;
    
    printf("&a = %p, a = %d\n", &a, a);
    printf("&p1 = %p, p1 = %p, *p1 = %d\n", &p1, p1, *p1);
    printf("&p2 = %p, p2 = %p, *p2 = %p, **p2 = %d", &p2, p2, *p2, **p2);
	
    printf ("\n\n**p2 = 99\n\n");
    **p2 = 99;
	
    printf("&a = %p, a = %d\n", &a, a);
    printf("&p1 = %p, p1 = %p, *p1 = %d\n", &p1, p1, *p1);
    printf("&p2 = %p, p2 = %p, *p2 = %p, **p2 = %d", &p2, p2, *p2, **p2);
	
    return 0;
}
```
- No exemplo, ``p2`` é um ponteiro de um ponteiro inteiro, ou seja, *ele é um ponteiro que guarda o endereço de um outro ponteiro, que por sua vez gaurda o endereço de um inteiro*)
- O resultado no console será o seguinte:
```
&a = 0061FF1C, a = 10 
&p1 = 0061FF18, p1 = 0061FF1C, *p1 = 10 
&p2 = 0061FF14, p2 = 0061FF18, *p2 = 0061FF1C, **p2 = 10 

**p2 = 99 

&a = 0061FF1C, a = 99 
&p1 = 0061FF18, p1 = 0061FF1C, *p1 = 99 
&p2 = 0061FF14, p2 = 0061FF18, *p2 = 0061FF1C, **p2 = 99
```

### exemplo 2
```C
#include <stdio.h>
#include <stdlib.h>

int main(){
    float z = 2.5;
    float *fp;

    fp = &z; //fp = 5000 (endereço ficticio)

    printf("*&z = %f\n", *&z);
    printf("*fp = %f\n", *fp); // *(&z)
    printf("**&fp = %f\n", **&fp);
}
```
- explicação:
``*&z`` = ``*(5000)`` = 2.5
fp = &z
``*fp`` = ``*(5000)`` = 2.5
``**&fp`` = ``**(&fp)`` = ``*(*(5004))`` = ``*(5000)`` = 2.5

---