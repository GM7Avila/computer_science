# Complexidade de Algoritmos
- Analisar a quantidade de iterações que o código leva do início ao fim analisando seu pior caso possível.
![[Pasted image 20230502185851.png]]

## Noção Intuitiva
- Calcular quantidade de recursos demandada pelo algoritmo (complexidade);
- Complexidade - tempo + espaço
- Variação da complexidade conforme a entrada do algoritmo

Exemplo 1 
```Python
def inverter_lista(lista):
	tamanho = len(lista)
	limite = tamanho//2
	
	for i in range(limite):
		aux = lista[i]
		lista[i] = lista[n-i]
		lista[tamanho-i] = aux

"""
# Calculo da complexidade de espaço (memória) - desconsiderando tipo das variáveis - aproximado:
	associado à variáveis no algoritmo : 5 variáveis 
	lista é uma variavel que tem tamanho variado [N]
	
	>> ou seja: 4 + N memória -> N
	
# Calculo da complexidade de tempo (processamento) através das operações elementares - aproximado: 
	
	>> 2 + 3 *(N/2) -> 3N/2
	
"""
```

> [[Operações Elementares]]

*Note*: no exemplo à cima,  a complexidade varia conforme a entrada de elementos na lista (N). Sendo assim,  levando em consideração que a complexidade de tempo é dada por f(x) = 2 + 3 * (N/2), podemos traçar um gráfico linear (função do primeiro grau), da seguinte forma: 

![[Pasted image 20230328120537.png]]

Exemplo 2
```Python
def inverter_lista2(lista):
	nova_lista = []
	tamanho = len(lista)
	for i in range(tamanho):
		nova_lista.append(lista[tamanho-i])
	return nova_lista	

"""
# Complexidade de Espaço
	3 + 2*N -> 2N
# Complexidade de Tempo
	2 + N -> N

"""
```

Note que para ambos os casos, as constantes 3 e 2 pouco importarão em grandes quantidades de N, pois eles se tornam irrelevantes. Exemplo: 3 + 2 * 100000 (podemos desconsiderar o 3).

## Big O - Conteúdo 1

> https://www.youtube.com/watch?v=UQzCFkRbIrE
> Recomendação de Livro > *Introduction to Algorithms - Thomas H Cormen*

### O que é o Big O Notation?
A notação "big O" é uma forma de descrever o *comportamento assintótico de um algoritmo*, ou seja, como ele se comporta quando o tamanho do problema aumenta infinitamente (limite ao infinito). Por exemplo, suponha que você tenha um algoritmo que classifica uma lista de números em ordem crescente. Se a lista tiver n elementos, o tempo de execução do algoritmo pode depender de n de diferentes maneiras, dependendo do algoritmo utilizado.

#### Ideia de Limite
Assim como em cálculo, em que estudamos o comportamento de uma função quando a variável se aproxima de um determinado valor, na análise de algoritmos com notação big O, estudamos o comportamento do tempo de execução do algoritmo à medida que o tamanho do problema se aproxima de um valor infinitamente grande.

Nesse sentido, a notação big O é uma maneira de resumir o comportamento assintótico de um algoritmo em uma expressão matemática, assim como em cálculo, onde os limites são usados para resumir o comportamento de uma função em um valor ou ponto específico.

Ao estudar o comportamento de um algoritmo com a notação big O, estamos essencialmente tentando descobrir o limite de uma função que representa o tempo de execução do algoritmo à medida que o tamanho do problema aumenta.

Portanto, podemos dizer que a notação big O é uma ferramenta útil para analisar a complexidade de algoritmos e, assim como no cálculo, pode ser usada para entender o comportamento de uma função em um limite ou ponto específico.

### Aplicação
Usando a notação big O, podemos dizer que o tempo de execução do algoritmo é O(n), o que significa que o tempo de execução aumenta linearmente com o tamanho da lista. Se o algoritmo levasse O(n^2) tempo para classificar a lista, isso significaria que o tempo de execução aumenta quadraticamente com o tamanho da lista.

Então, para calcular a complexidade de um algoritmo usando a notação big O, você precisa identificar o fator que mais contribui para o tempo de execução do algoritmo à medida que o tamanho do problema aumenta. Em seguida, você expressa essa contribuição em termos de uma função matemática que representa a ordem de magnitude do tempo de execução à medida que o tamanho do problema aumenta.

Em resumo, a notação big O é uma forma de descrever a complexidade de um algoritmo em termos de como ele se comporta em relação ao tamanho do problema. A complexidade pode ser expressa como uma função matemática que representa o tempo de execução do algoritmo em relação ao tamanho do problema.

### Exemplo 1
Vamos utilizar como exemplo o seguinte algoritmo de [[Algoritmos de Busca|busca simples]] em [[backend/JAVA/README]], que recebe como entrada um array de inteiros e um número inteiro a ser buscado, e retorna a posição em que o número foi encontrado, ou -1 caso contrário:

```JAVA
public static int buscaSimples(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == x) {
            return i;
        }
    }
    return -1;
}
```

Para calcular a complexidade desse algoritmo, *primeiro precisamos identificar o fator que mais contribui para o tempo de execução à medida que o tamanho do problema aumenta*. Nesse caso, o fator é o *tamanho do array*, já que o algoritmo precisa percorrer cada elemento do array para buscar o número desejado.

Em seguida, podemos expressar a complexidade desse algoritmo em notação big O. Como o algoritmo percorre cada elemento do array, a complexidade é linear em relação ao tamanho do array, e podemos escrever isso como O(n), onde n é o tamanho do array.

Portanto, podemos dizer que a complexidade desse algoritmo de busca simples é O(n), o que significa que o tempo de execução aumenta linearmente com o tamanho do array.

### Exemplo 2

## Big O - Conteúdo 2

> https://www.youtube.com/watch?v=zXBaLEGv0iM

Segue abaixo uma tabela com a complexidade dos algoritmos mais comuns, baseado na notação Big O:

![[Pasted image 20230302175319.png]]

- Indica o comportamento do código / algorítmo em termo de crescimento do tempo de execução, baseado nos valores de entrada.
- Complexidade dada por O(N);
- Exemplos (do melhor ao pior caso): $O(1)$ (constante), $O(log N)$, $O(N)$ (linear), $O(N^2)$, $O(2^N)$, $O(N!)$ ...

#### Exemplo
- Em C++, por exemplo, em 1 segundo, conseguimos executar cerca de $10^7$ passos / iterações.
- Imagine que em um problema, temos que a entrada de um vetor é $10^6$, considerando que a complexidade do algoritmo é $O(N^2)$ e o algoritmo precisa ser executado em 1 segundo. Isso seria possível?
- Considerando a entrada N de $10^6$, a complexidade desse problema será de $O((10^6)^2$), ou seja, supera a complexidade de 1 segundo, onde é possível executar cerca de $10^7$ operações.

### Passos para calcular a complexidade

1 - Levar em consideração apenas as *repetições do código*
2 - Verificar a complexidade das *funções/métodos próprios da linguagem* (se utilizado)
3 - Ignorar as constantes e utilizar o termo de maior grau

- Determinando a complexidade dos seguintes códigos:

#### Exemplo 1

```CPP
bool exemplo1(vector<int> v, int X){
	int tamanho = v.size(); // O(1)
	for(int i=0; i < tamanho; i++){ // O(N)
		if(v[i] == X) return true; // O(1)
	}
	return false; // O(1)
}
```

- 1º passo - o laço for nesse caso repete `tamanho` vezes.
- 2º passo - método ``size()`` é própria de C++ (vector), e sua complixidade segundo a referência de C++ online (https://cplusplus.com/reference/vector/vector/size/) sua complexidade é **constante**.
- 3º passo - ignorar as constantes e utilizar o termo de maior grau: *O(N)*, que é a complexidade do nosso código. 

#### Exemplo 2
```CPP
bool exemplo2(vector<int> v){
	int tamanho = v.size(); // O(1)
	for(int j=0; j<tamanho; j++){ // O(N)
		for(int i=0; i<tamanho; i++){ // O(N)
			if(j!=i && v[j]==v[i]) // O(1)
				return true
		}
	}
	return false;
}
```
- $O(N) * O(N)$ = $O(N)^2$ - complexidade quadrática

#### Exemplo 3
```cpp
int exemplo3(vector<int> v){
	int tamanho = v.size();
	int x = 0;
	
	for(int i=0; i<tamanho; i++){ // O(N)
		for(int j=0; j<tamanho; j++){ // O(N) 
			if(v[i] == v[j]) x++;
		}
	}
	
	int y = 0;
	for(int k=0; j<tamanho; j++){  // O(N)
		if(v[k]==10){
				y = 2*y;
		} 
	}
	
	int z = 0;
	for (int l; l<tamanho; l++){ // O(N)
		if(v[l] == 10) {
			Z+=5;
		}
	}
	return x+y+z;
}

// O(N)*O(N) + O(N) + O(N)
// O(N)*O(N) + 2*O(N)
// O(N^2) + O(N) <-ignorou a constante 2
// O(N^2) <-considerou o termo de maior grau
```

#### Exemplo 4
```CPP
bool exemplo4 (vector<int> v, vector<int> w) { 
	int tamanho = v.size();
	int tamanho2 = w.size();
	
	for(int i=0; i<tamanho; i++){ // O(N)
		for(int j=0; j<tamanho2; j++){ // O(M)
			if(v[i] == v[j])
				return true;
		}
	}
	return false;
}

// O(N) * O(M)
```

#### Exemplo 5
- Note: quantidade de Linhas não necessariamente está ligada com complexidade e eficiência do código;
- Neste exemplo, mesmo contendo mais linha, e fazendo a mesma função, veremos que o algoritmo exemplo5_A é menos complexo que o algoritmo exemplo5_B;
```CPP
bool exemplo5_A(vector<int> idade){
	int tamanho = idades.size(); //size -> cte 
	int menor_idade = 200;
	
	for(int i=0; i<tamanho; i++){ // O(N)
		if(idade[i] < menor_idade){
			menor_idade = idades[i];
		}
	}
	int cont = 0;
	for(int in=0; i<tamanho; i++){ // O(N)
		if(v[i] == menor_idade){
			const++;
		}
	}
	return cont > 1;
}

// SOMA DAS COMPLEXIDADES
// O(N) + O(N)
// 2 O(N)
// O(N)
```

```cpp
bool exemplo5_B(vector<int> idades){
	sort(idades.begin(), idades.end());
	return idades[0] == idades[1];
}

// Nenhuma repetição
// Complexidade do sort em C++ -> O(N*log2(N))
// Complexidade final O(NlogN)
```

#### Exemplo 6
```CPP
bool exemplo6(set<int> s, vector<int> v){
	int tamanho = v.size();
	for(int i=0; i<tamanho; i++) { //O(N)
		if(s.count(v[i])) return true; // O(LogN)
	}
	return false;
}

// O(N) * O(logN)
```