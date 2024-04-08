0:01 /# Algoritmo de Dijkstra

## Objetivo e Elementos

> Algoritmo de caminho mais curto - grafos ponderados

> Análise por adjacência

- Descobrir a distância da **origem (s)** até o **destino**;
	- **d[v]** é o comprimento do caminho mais curto do vértice $v$ até o vértice $s$ - vértice inicial;
		- Caminhos desconhecidos começam com o valor de infinito (∞);
		- **Inicialmente** $d[v]$ é **∞** (positivo) - exceto para o nó inicial ($d[s] = 0)$;
			- Isso porquê qualquer novo valor será menor que infinito, então qualquer novo valor será uma solução válida de caminho mais curto.
			- Infinito sempre será mensurado como maior valor possível na tomada de decisão dos caminhos;
	- **δ(s, v)** é o comprimento mais curto do nó $s$ ao nó $v$;

- **Π[v]** é o ==predecessor== de v no caminho mais curto de s para v.
	- **Inicialmente** todos os predecessores dos vértices são NULO;
	- Ao longo das iterações nós armazenamos o predecessor - isso será últil para realizamos o [[Backtracking|backtracking]] ao final do processo de busca.

```mathematica
   A ------> B
   |        / |
   |       /  |
   |      /   |
   |     /    |
   v    v     v
   C <-- D <-- E
```

- $Π[B] = A$ (significa que o vértice A é *predecessor* de B);
- $Π[C] = A$ (o vértice A é *predecessor* de C);
- $Π[D] = B$ (o vértice B é *predecessor* de D);
- $Π[E] = D$ (o vértice D é *predecessor* de E);

---
### Observações
> O Algoritmo de Dijkstra consegue atuar em grafos com ciclos e que possua arestas direcionadas ou não direcionadas. 

> [!NOTE] Depth-first Search para grafos sem ciclo
> Se o grafo não possui ciclo, compensa mais utilizar o algoritmo de [[01. Search|Depth-first search]] - por questão de menor complexidade - isso por que a busca em profundidade para o caso de busca de menor caminho em grafos com ciclos ocorrerá o descarte de arestas. Veja o exemplo abaixo:

```mathematica
Aresta (a,d) desconsiderada

0 A---1---B 1
  |       |
  4       2
  |       |
6 D---3---C 3
```

| Problema / Circunstância                                                       | Algoritmo(s) Recomendado(s)                          |
| ------------------------------------------------------------------------------ | ---------------------------------------------------- |
| Encontrar o caminho mais curto em um grafo não ponderado                       | BFS (Busca em Largura)                               |
| Encontrar o caminho mais curto em um grafo ponderado com todos os pesos iguais | BFS (Busca em Largura)                               |
| Encontrar o caminho mais curto em um grafo ponderado com pesos não negativos   | Dijkstra                                             |
| Encontrar o caminho mais curto em um grafo com arestas de peso negativo        | Bellman-Ford                                         |
| Encontrar todos os pares de caminhos mais curtos em um grafo ponderado         | Floyd-Warshall                                       |
| Verificar se há ciclos negativos em um grafo ponderado                         | Bellman-Ford                                         |
| Explorar todos os vértices de um grafo                                         | DFS (Busca em Profundidade)                          |
| Verificar se há um caminho entre dois vértices em um grafo                     | DFS ou BFS dependendo da situação e das preferências |
| Determinar a árvore de busca em profundidade de um grafo                       | DFS (Busca em Profundidade)                          |
| Determinar a árvore de busca em largura de um grafo                            | BFS (Busca em Largura)                               |

| Problema / Circunstância                                                  | Algoritmo(s) Não Recomendado(s)                                          |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Grafo com ciclos e necessidade de encontrar caminho mais curto            | DFS (Busca em Profundidade)                                              |
| Grafo com pesos negativos e necessidade de encontrar o caminho mais curto | Dijkstra (não funciona corretamente com pesos negativos)                 |
| Grafo com pesos negativos e necessidade de encontrar ciclos negativos     | Dijkstra (não pode detectar ciclos negativos)                            |
| Grafo com muitos ciclos e necessidade de encontrar o caminho mais curto   | BFS (pode ser menos eficiente em grafos densamente conectados)           |
| Grafo muito grande com muitas arestas                                     | Bellman-Ford (tem uma complexidade de tempo maior que outros algoritmos) |

---
## Processo de relaxamento

- Relaxamento é o processo pelo qual tentamos atualizar o valor de $d[v]$ para um valor *mais curto*, se encontrarmos um caminho mais curto para alcançar v. 
- A medida que o algoritmo progride, ele atualiza os valores de $d[v]$ para refletir os caminhos mais curtos encontrados.

```mathematica
// w - peso | u - origem | v - destino 

2            9
u ----5----> v
      |
      | Relaxamento
      |      
      |      7 
u ----5----> v      
```

- Pseudocódigo (relaxamento):
```c
// w(u,v) -> peso da aresta (u até v)
Relaxamento(u,v,w)
	if d[v] > d[u] + w(u,v)
		then d[v] = d[u] + w(u,v)
		Π[v] = u // predecessor de v recebe u = chegou em v a partir de u
	
```

![[Relaxamento|100%]]

- Através do processo de relaxamento, $d[v]$ deve eventualmente se tornar $δ(s, v)$, que é o comprimento do caminho mais curto de s para v - pois ao final, o que estamos buscando é a distância mais curta entre o nó inicial e o nó v que será o nó final - em algum momento (o nó $v$ atualiza a cada iteração no grafo);

## Funcionamento
Para cada aresta (u,v) em E, assuma que w(u,v) ≥ 0, mantenha um conjunto S de vértices cujos pesos finais de caminho mais curto foram determinados. Selecione repetidamente u em V-S com a estimativa de caminho mais curto mínima, adicione u a S, relaxe todas as arestas que saem de u (adjacentes a u);

- Pseudocódigo
```c
Dijkstra(G,w,s)
	IniciaOrigemUnica(G,s)
	A = null // conjunto solução
	Q = V[G] // fila de prioridades
	
	while(Q != 0)
		u = ExtrairMinimo(Q) // uso do heap de fibonacci (recomendado)
		A = AU{u} // adiciona o vértice ao conjunto solução
		
		for v ∈ Ajd[u]: // para todos vertices que são adjacentes de u
			relaxamento(u,v,w)  
		
	retorne A	
```

- `relaxamento(u,v,w)` é uma operação implícita de *DECREASE-KEY*;
	- significa reduzir a estimativa de custo(ou prioridade) associada a um vértice na fila de prioridade.
### Complexidade do Algoritmo
O desempenho do algoritmo de Dijkstra depende do número de vértices `|V|` e arestas `|E|` do grafo. 

```C
Dijkstra(G,w,s) 
	IniciaOrigemUnica(G,s) // O(n)
	A = null // O(1)
	Q = V[G] // O(1)
	
	while(Q != 0) // log(n)
		u = ExtrairMinimo(Q)        
			A = AU{u}               
			for v ∈ Ajd[u]:            
				relaxamento(u,v,w)  // O(n)
		
	retorne A	
```
- A [[Complexidade de Algoritmos|complexidade]] em `IniciaOrigemUnica(G,s)` é *O(V)*, onde V é o número de **vértices**, pois precisamos fazer a analização para todos os vértices.
- A complexidade em `while(Q!=0)` com a utilização do [[Heap de Fibonacci]] é *log(V)* - onde V é a quantidade de vértices (a cada iteração, analisamos os vértices adjacentes - ==a cada iteração estamos diminuindo a quantidade de vértices adjacentes que devemos analisar==);
- Ao analisar as arestas em `for v ∈ Ajd[u]` teremos uma complixdade *O(E)*, onde E é o número de **arestas**.

- Com isso, a complexidade final do algoritmo será: *O(V logV + E)*;

![[3. Dijkstra|100%]]

## Aplicações

<img src="https://gaia.cs.umass.edu/kurose_ross/LMS/images/Chapter_5_KC/Dijsktra_kc_network.png">

- **[[a7 - Camada de Enlace de Dados|Rede de Computadores - Roteamento]]** - O algoritmo de Dijkstra é amplamente utilizado em [[Roteadores|roteadores]] e [[Switches|switches]] para encontrar os *caminhos mais curtos* em redes de computadores, incluindo a internet. Ele é essencial para o roteamento de ==pacotes de dados e para determinar as rotas mais eficientes== para a transmissão de dados.
  
- **Sistemas de Navegação e GPS**: Aplicações de navegação, como sistemas GPS e aplicativos de mapeamento, utilizam o algoritmo de Dijkstra para calcular as ==rotas mais curtas entre pontos de origem e destino==. Isso é fundamental para orientar os motoristas, pedestres e usuários de transporte público em suas viagens. ==Logística de entrega e mercadoria==;
  
- **Redes de Distribuição de Energia**: Na engenharia elétrica, o algoritmo de Dijkstra é aplicado em ==redes de distribuição de energia== para determinar as rotas mais eficientes para a transmissão de energia elétrica. Isso é importante para garantir uma distribuição eficiente e confiável de eletricidade.
  
- **Modelagem de Redes e Sistemas Complexos**: Em pesquisa e modelagem de sistemas complexos, o algoritmo de Dijkstra é utilizado para ==analisar redes sociais, redes de transporte, redes de comunicação, redes de colaboração==, entre outros. Ele ajuda a entender a estrutura e dinâmica desses sistemas.
  
- **Redes Sociais e Plataformas de Comunicação**: Em redes sociais e plataformas de comunicação, o algoritmo de Dijkstra pode ser empregado para recomendar ==conexões entre usuários==, calcular distâncias entre perfis, ou até mesmo para determinar o caminho mais curto entre você e um amigo em um jogo online.

- **Jogos e Simulações**: Em jogos de computador e simulações, o algoritmo de Dijkstra pode ser utilizado para ==determinar os movimentos mais eficientes== de personagens ou unidades, encontrar o caminho mais curto entre locais no jogo, ou calcular trajetórias para navegação de veículos em ambientes virtuais.

- **Sistema de Recomendação de Conteúdo**: Em plataformas web que oferecem ==recomendações de conteúdo==, como vídeos, músicas, ou artigos, o algoritmo de Dijkstra pode ser aplicado para calcular ==caminhos mais curtos entre os interesses dos usuários e os itens recomendados==, melhorando a personalização e relevância das recomendações.

- **Otimização de Roteamento de Requisições**: Em servidores web, o algoritmo de Dijkstra pode ser utilizado para otimizar o ==roteamento de requisições entre servidores e microsserviços em uma arquitetura distribuída==. Isso ajuda a minimizar o tempo de resposta e maximizar a disponibilidade dos serviços.

## Código em C
>Fonte: http://desenvolvendosoftware.com.br/algoritmos/grafos/algoritmo-de-dijkstra.html

```C
// Performs a Dijkstra's Algorithm in a graph.
static size_t dijkstra(const struct graph *g, size_t src, size_t dest, size_t *path)
{
    struct heap *h = NULL;
    size_t distances[g->nnodes];
 
    assert((h = heap_create(g->nnodes)) != NULL);
 
    // Push initial distances.
    distances[src] = 0;
    for (size_t i = 0; i < g->nnodes; i++) {
        if (i != src) {
            path[i] = g->nnodes;
            distances[i] = (size_t)(-1);
        }
        heap_push(h, i, distances[i]);
    }
 
    // Process all nodes that in the graph.
    while (heap_length(h) != 0) {
        size_t i = heap_pop(h);
 
        // Shortest path found.
        if (i == dest) {
            break;
        }
 
        // Process all neighbor nodes.
        for (size_t j = 0; j < g->nnodes; j++) {
            struct link *l = graph_link(g, i, j);
 
            // This is a neighbor.
            if (l->weight > 0) {
                size_t new_distance = distances[i] + l->weight;
 
                // Update distances.
                if (new_distance < distances[j]) {
                    path[j] = i;
                    distances[j] = new_distance;
                    heap_increase_priority(h, j, new_distance);
                }
            }
        }
    }
 
    // Release resources.
    heap_destroy(h);
 
    return (distances[dest]);
}
```