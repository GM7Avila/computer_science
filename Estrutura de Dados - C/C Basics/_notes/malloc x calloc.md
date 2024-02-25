# Alocação Dinâmica

## malloc
```C
// não inicializa os valores (lixo de memória)
int *p = (int *)malloc(10 * sizeof(int));
free(p);
```
## calloc
```C
// inicializa os valores como zero
int *p = (int *)calloc(10, sizeof(int));
free(p);
```

## boas práticas - alocação dinâmica
1. **Verifique erros de alocação**:
    
```c
if (ptr == NULL) {     
	printf("Erro na alocação de memória.\n");     
	exit(EXIT_FAILURE); 
}
```
	
2. **Libere memória alocada**:
    
```C
free(ptr);
```
    
3. **Inicialize ponteiros**:
    
```C
int *ptr = NULL;
```
    
4. **Evite vazamentos de memória**:
	
```C
if (ptr != NULL) {
	free(ptr);     
    ptr = NULL; 
}
```
	
5. **Calcule o tamanho necessário corretamente**:
    
    ```C
    int n = 10; 
    int *ptr = (int *)malloc(n * sizeof(int));
    ```
    
6. **Evite acesso a memória inválida**:
    
    ```C
    if (ptr != NULL) {     
	    *ptr = 10; // Acesso seguro, ptr foi alocado corretamente 
	}
	```
    
7. **Evite vazamentos de ponteiro**:
    
```C
free(ptr); 
ptr = NULL;
```
