# 🔍 Algoritmos de Busca

## Busca Linear em Listas
*Algoritmo para responder o índice no qual se encontra um determinado valor:*
	- todos os elementos são percorridos
```Python 
""" Retorna o indicie do elemento se ele estiver na lista ou None, caso contrário """

def busca(lista, elm):
	'for i (variavel de iteração) - in rage(tamanho da lista - 1)'
	for i in range(len(lista)):
		if lista[i] == elem:
			return i
	return None 

lista = [9, "5", 32, 0, "python", 11]
elemento = 32
indice = busca(lista, elemento)

if indice is not None :
	print ("O indice do elemento {} é {}".format(elemento, indice))
else : 
	print ("O elemento {} não se encontra na lista". format(elemento))
```

- Calculando a complexidade do pior caso (limite de recursos que o algoritmo irá consumir) : nesse caso, a pior hipótese é a busca de um elemento que não se encontra na lista → *2 * n + 1 → 2 * n → O(n) - função linear*

## Busca binária em Listas
- Para o caso de listas ordenadas;
- Busca pelo centro e elimina metade da lista;

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