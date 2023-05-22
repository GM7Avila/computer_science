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
