# Algoritmos de Ordenação

> https://www.toptal.com/developers/sorting-algorithms

## 1. Selection Sort 🧮
Suponha a seguinte lista : [7,5,1,8,3]
1. Identifica que *1* é o menor elemento da lista
2. Troca o *1* com o elemento da primeira posição (*7*)
3. Nova lista: [1,5,7,8,3]

1. Identifica que *3* é o menor elemento da lista (**excluindo** o 1)
2. Troca o *3* com o elemento da segunda posição *5*
3. Nova lista: [1,3,7,8,5]

Repete o processo até todos os elementos ficarem em ordem...
1. Encontrando um valor mínimo...
```python
lista = [7,5,1,8,3]
minimo = lista[0]

for i in range(len(lista)):
	if lista[i] < minimo 
		minimo = lista[i]
print(minimo)		
```

2. definindo selection_sort
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

![[Pasted image 20230503101408.png]]

## 2. Bubble Sort 🫧
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

![[Pasted image 20230503101542.png]]

## Insertion Sort 🃏
- Ordenação de cartas.
- Eficiente para order um ==número pequeno de elementos==.
- Divide o array em dois - parte ordenada e parte não ordenada.
- A ideia básica é percorrer o vetor de dados várias vezes, e em cada passagem, comparar um item com os itens que o precedem na lista. Se o item for menor do que o item anterior, troca-os de posição. O processo é repetido até que todo o vetor esteja ordenado.
- A implementação do Insertion Sort começa com o primeiro item da lista considerado ordenado. O próximo item é então tomado e comparado com os itens que o precedem na lista. Se o item é maior do que o item anterior, ele é considerado em sua posição correta e o próximo item é tomado. Se o item é menor do que o item anterior, ele é movido para a posição correta na lista ordenada. Esse processo é repetido até que todos os itens tenham sido classificados.
- Embora o Insertion Sort não seja tão eficiente quanto alguns outros algoritmos de ordenação, ele tem a vantagem de ser fácil de entender e implementar, e funciona bem em pequenas listas ou quando a lista já está quase ordenada.

```cpp
#include <iostream>
using namespace std;

void insertionSort(int arr[], int n) {
	int i, key, j;
	for (i = 1; i < n; i++) {
		key = arr[i];
		j = i - 1;
			   
		/* Move os elementos do array que são maiores que a chave,
		para uma posição à frente de sua posição atual */
		       
		while (j >= 0 && arr[j] > key) {
			arr[j + 1] = arr[j];
			j = j - 1;
		}
	    arr[j + 1] = key;
	}
}
```

![[Pasted image 20230503103021.png]]