# Camada de Enlace de Dados

## Roteamento
- O **algortimo de roteamento** é a parte do software da camada de rede responsável pela decisão sobre qual **linha de saída** a ser usada na transmissão do pacote que entra. 
	- Em uma rede de datagramas essa decisão deverá ser tomada para todos os pacotes de dados recebidos.

### Princípio da Otimização

```
      A                     B                                      C
=====(X)========(X)========(X)=========(X)=========(X)============(X)=====
```

- O princípio da otimizaçã de Bellman estabelece que ***"se o roteador B estiver no caminho ótimo entre o roteador A e o roteador C, o caminho ótimo de B a C também estará na mesma rota"***.

>  BELLMAN, R.E. Dynamic Programming, Princeton, NJ> Princeton University Press, 1957.

- Ou seja, nesse caso **o melhor caminho (ótimo) depende do destino**.

- O princípio da otimização diz que o **conjunto de rotas ótimas** de todas as origens para um determinado destino é uma árvore conhecida como **árvore de escoamento**, cuja raiz é o hospedeiro destino.

- Os algoritmos de roteamento precisam descobrir a árvore de escoamento a partir da topologia da rede.

<img src="https://slideplayer.com.br/slide/3142144/11/images/21/%C3%81rvore+de+escoamento+%282%29.jpg" width="100%">

### Roteamento pelo caminho com menor custo
- Peso - distância.

- A ideia do roteamento pelo caminho mais curto é criar um grafo da sub-rede, com cada nó representando um roteador e cada arco indicando um enlace.
- Para escolher uma rota entre um determinado par de roteadores o algortimo deve encontrar o caminho mais curto entre eles no [[2. Grafos em C|grafo]].

- Uma forma de medir o comprimento do caminho é em **quantidade de saltos** - ou seja, quantidade de **enlaces** que devem ser utilizados.
	- Também devemos analisar o *trafégo nos enlaces*.
	- Uma outra métrica que devemos levar em consideração é a própria *distância geográfica*.

- No geral, os valores dos arcos podem ser obtidos como uma função (os critérios abaixo podem ser combinados ou não):
	- da distância
	- da largura de banda
	- do tráfego médio
	- do custo de comunicação
	- do comprmento médio de fila
	- do retardo detectado
	- outros fatores

- **ALGORITMO DE DIJKSTRA (1959)**
	- Cada roteador armazena sua menor distância até a origem e o caminho a ser seguido.
	- Na inicialização do algoritmo não existe caminho conhecido
		- A distância é marcada como *inifinito*
	- Conforme o algoritmo progride, os caminhos e seus custos são encontrados.
	- Ao final para obter a árvore de escoamento basta seguir o caminho por cada roteador até seu vizinho de menor custo.

![[3. Dijkstra|100%]]