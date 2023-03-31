# Ordenação

## Bubble Sort
O Bubble Sort é um algoritmo de ordenação simples que percorre repetidamente a lista a ser ordenada, comparando cada par de elementos adjacentes e os trocando de posição se estiverem na ordem errada. Esse processo é repetido até que a lista esteja completamente ordenada.

```cpp
int main(){
	const int n = 5;
	int vect[n] = {5,3,1,7,2};
	
	for(int i=0;i<n;i++){
		    
		for(int j=0;j<n-1;j++){
			if(vect[j] > vect[j+1]){
				int aux = vect[j];
				vect[j] = vect[j+1];
				vect[j+1] = aux;
			}
		}
		  
	}
	
	return 0;
}
```

Entretanto não é muito eficiente em termos de tempo de execução para listas maiores ou complexas, já que sua complexidade é O(n^2), onde "n" é o número de elementos a serem ordenados.

## Selection Sort
- Suponha a seguinte lista : [7,5,1,8,3]
1. Identifica que *1* é o menor elemento da lista
2. Troca o *1* com o elemento da primeira posição (*7*)
3. Nova lista: [1,5,7,8,3]

1. Identifica que *3* é o menor elemento da lista (excluindo o 1)
2. Troca o *3* com o elemento da segunda posição *5*
3. Nova lista: [1,3,7,8,5]

Repete o processo até todos os elementos ficarem em ordem...

- Encontrando um valor mínimo...
```python
lista = [7,5,1,8,3]
minimo = lista[0]

for i in range(len(lista)):
	if lista[i] < minimo 
		minimo = lista[i]
print(minimo)		
```

- definindo selection_sort
```python
lista = [7,5,1,8,3]

def selection_sort(lista):
	min_index  = j
	for j in range(len(lista)-1):
		for i in range(j, len(lista)):
			if lista[i] < lista[min_index]
			min_index = i
		if lista[j] > lista[min_index]:
			aux = lista[j]
			lista[j] = lista[min_index]
			lista[min_index] = aux

```

