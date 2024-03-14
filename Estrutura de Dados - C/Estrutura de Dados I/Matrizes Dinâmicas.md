# Matrizes Dinâmicas

- Alocação de matriz na memória [[Memória Heap VS Memória Stack|Heap]];
- Vetor de vetor (array de array);
- Alocação para cada linha;

> [!NOTE] BOA PRÁTICA
> - É uma boa prática de programção iniciar os ponteiros para NULL (valor 0 para ponteiros) - não apontar para lixo de memória;

- matriz bidimensional dinâmica:
```C
int **m = NULL; //nrwos = 2; ncols = 3;

m = (int**) calloc(nrows, sizeof(int*));
for (int i = 0; i<nrows; i++){
	m[i] = (int*) calloc(ncols, sizeof(int));
}
```


