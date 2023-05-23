# Matrizes

## Array Bidimensional

![[Pasted image 20230522182927.png]]

```C
#include <stdio.h>
#include <stdlib.h>

int main(void) {
	
	int **matriz; 
	int i, j, linhas, colunas;
	
	printf("Quantas linhas tem a matriz? ");
	scanf("%d", &linhas);
	printf("Quantas colunas tem a matriz? ");
	scanf("%d", &colunas);
	
	matriz = (int **) malloc(linhas*sizeof(int));
	
	for (i=0; i<linhas; i++){
		matriz[i] = (int *) malloc(colunas*sizeof(int));
	}
	
	for (i=0; i<linhas; i++){
		for (j=0; j<colunas; j++) {
		  printf("Valor de mat[%d][%d]: ", i, j);
		  scanf("%d", &matriz[i][j]);
		}
	}
	
	for (i=0; i<linhas; i++){
		free(matriz[i]);
	}
	
	free(matriz);
	return 0;
}
```

## Array Tridimensional

### por alocação estática
```C
int main(){
	// 2 fatias, 2 linhas, 3 colunas
	int m[2][2][3] = {
		// fatia [0]
		{
			{0, 1, 2}, // linha [0]
			{3, 4, 5} // linha[1] 
		},
		
		// fatia [1]		
		{
			{6, 7, 8}, //linha[0]
			{9, 10, 11} //linha[1]
		}
	};
}
```
 
### por alocação dinâmica

```C
int main() {

    int n_slices = 2;
    int n_rows = 2;
    int n_cols = 3;

    /*********** ALOCAÇÃO DINÂMICA DA MATRIZ ***********/

    int ***m = (int***) calloc(n_slices, sizeof(int**));

    // para cada fatia
    for (int k=0; k < n_slices; k++) {
        // aloca em um index de fatia os index de linhas
        m[k] = (int**) calloc(n_rows, sizeof(int*));
        
        // para cada linha
        for (int i=0; i < n_rows; i++) {
            // aloca em um index de linha os index de colunas
            m[k][i] = (int*) calloc(n_cols, sizeof(int));
        }
    }

    /*************************************************/

    int count = 0;
    printf("&m = %p, m = %p\n\n", &m, m);

    // para cada fatia
    for (int k = 0; k < n_slices; k++) {
        printf("&m[%d] = %p, m[%d] = %p\n", k, &m[k], k, m[k]);
        // para cada linha
        for (int i = 0; i < n_rows; i++) {
        printf("&m[%d][%d] = %p, m[%d][%d] = %p\n",
                k, i, &m[k][i], k, i, m[k][i]);
            
            // para cada coluna
            for (int j = 0; j < n_cols; j++) {
                m[k][i][j] = count++;
                printf("&m[%d][%d][%d] = %p, m[%d][%d][%d] = %d\n",
                       k, i, j, &m[k][i][j],
                       k, i, j, m[k][i][j]);
            } puts("");
        } puts("");
    }

    /********* DESALOCAÇÃO *********/

    // para cada fatia
    for (int k = 0; k < n_slices; k++) {
        // para cada linha
        for (int i = 0; i < n_rows; i++) {
            free(m[k][i]);
        }
        free(m[k]);
    } 
    
    free(m);
    m = NULL;

    /*************************************************/

    return 0;

}
```

## Desalocando uma matriz dinâmica
- Devemos desalocar toda as regiões que foram alocadas, seguindo a ordem dos últimos alocados para os primeiros alocados.
- Acessar cada elemento e apaga-los em ordem.

```C
// desalocando as linhas
for(int i=0; i<nrows; i++){
	free(m[i]); // i = 0 (free()) -> i = 1 (free())
}

// desalocando m
free(m); 
m = NULL;
```

- Note que o ``free()`` foi chamado 3 vezes, a mesma quantidade de vezes que chamamos ``calloc``, ou seja, devemos liberar todos os espaços alocados;
- Como boa prática de programação ``m = NULL``;

## Layout de Dados: Row and Column Major Order
- Estratégias para armazenar **arrays multidimensionais** de forma linear na Memória RAM - layout de dados.

- É importante saber o layout de dados usado para passar corretamente arrays entre programas escritos em diferentes linguagens de programação.

- Importância por questões de desempenho / performance.
	- CPU's processam **dados sequenciais** de forma **mais eficiente** do que dados não sequenciais → CPU Caching;
	- Fazer o acesso dos elemetos de forma não sequencial, implica em um programa mais lento - menor performance.
