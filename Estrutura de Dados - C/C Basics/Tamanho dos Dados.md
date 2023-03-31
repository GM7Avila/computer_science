# sizeof
Em C, você pode usar o operador `sizeof` para determinar o tamanho de um determinado tipo de dado em bytes. O operador `sizeof` é uma palavra-chave que retorna o tamanho em bytes do operando que é passado a ela.

Por exemplo, para descobrir o tamanho de um `int`, você pode escrever o seguinte código:

```C
#include <stdio.h>  
int main() {     
	printf("O tamanho de um inteiro é: %lu bytes\n", sizeof(int));     
	return 0; 
}`
```
O operador `sizeof` retorna um valor do tipo `size_t`, que é um tipo de dado inteiro sem sinal que pode armazenar o tamanho de qualquer objeto em bytes. O especificador de formato `%lu` é usado para imprimir um valor do tipo `size_t`.

```C
#include <stdio.h>
int main() { 
	int a; 
	printf("sizeof(a) = %ld bytes\n", sizeof(a)); 
	printf("sizeof(int) = %ld bytes\n", sizeof(int)); 
	printf("sizeof(short) = %ld bytes\n", sizeof(short)); 
	printf("sizeof(long) = %ld bytes\n", sizeof(long)); 
	printf("sizeof(unsigned long) = %ld bytes\n", sizeof(unsigned long)); 
	printf("sizeof(float) = %ld bytes\n", sizeof(float)); 
	printf("sizeof(double) = %ld bytes\n\n", sizeof(double)); 
	printf("sizeof(void *) = %ld bytes\n", sizeof(void *)); 
	printf("sizeof(int *) = %ld bytes\n", sizeof(int *)); 
	printf("sizeof(int **) = %ld bytes\n", sizeof(int **)); 
	printf("sizeof(int ***) = %ld bytes\n", sizeof(int ***)); 
	printf("sizeof(float *) = %ld bytes\n", sizeof(float *)); return 0; 
}
```



