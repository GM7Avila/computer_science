# 🔍 Algoritmos de Busca

## 1. Busca Linear

```JAVA
static boolean linear(vetor, search){
	
	for(int i=0; i < vetor.length; i++){
		if(vetor[i] == search){
			return true;
		}
	}
	return false;
}
```

- Todos os elementos são percorridos um a um (em ordem) - linear.
- Custo computacional: linear; 
- Quanto maior o número de elementos, mais demorado.

- Calculando a [[ComputingTheory/Algoritmos/Complexidade de Algoritmos|complexidade]] do pior caso (limite de recursos que o algoritmo irá consumir) : 
	Nesse caso, a pior hipótese é a busca de um elemento que não se encontra na lista:
	*2 * n + 1 → 2 * n → O(n) - função linear*

---

## 2. Busca Binária

- Vetor ordenado.
- Busca em dicionário / livro.
- Marcamos o início e o fim do vetor;
- Dividimos o vetor em duas partes a partir do meio.
- Compara o meio com o valor desejado, é maior ou menor?
- Descarta a parte onde o valor não se encontra e repete o processo.

```java
int inicio = 0;
int fim = vetor.length - 1;
int meio;

while(inicio <= fim) {
	meio = (inicio + fim)/2;
	
	if(array[meio] == elemento){
		return meio; // resolveu
	} else if (array[meio] < elemento){
		inicio = meio + 1; // da um novo valor ao inicio
	} else {
		fim = meio - 1; // da um novo valor ao fim
	}
}
```

- Enquanto a ordem de início e fim estiverem corretas, realiza.
- Meio = (valor do início + valor do fim) dividido por 2.
- Quando o valor não é encontrado no vetor, o início passa a ser maior que o fim, e assim a condição `while(inicio<=fim) = false` - ou seja, sai do while.

### Exemplo de execução
- **Exemplo**: lista: [3,9,17,21,28,50,66,84,87,92]
	Buscando por *66*
	meio da lista: [3,9,17,21,   28   ,50,66,84,87,92]
	Pergunta: *66* é maior que o meio da lista *28* ?
		*verdade* : ele está a direita, descarte todos os elementos à esquerda do meio
		*falso* : ele está a esquerda, descarte todos os elementos à direita do meio
		> no caso, 66 > 28, então é *verdade*
	Nova lista [-------,50,66,84,87,92]
	
	Repete o algoritmo apenas com o lado direito
	Novo valor do centro: *84*
	meio da lista: [50,66,   84   ,87,92]
	Pergunta: *66* é maior que o meio da lista *84* ?
		*verdade* : ele está a direita, descarte todos os elementos à esquerda do meio
		*falso* : ele está a esquerda, descarte todos os elementos à direita do meio
		> no caso, 66 < 84, então é *falso*
	Nova lista [50,66] - agora sobrou apenas duas opções!



