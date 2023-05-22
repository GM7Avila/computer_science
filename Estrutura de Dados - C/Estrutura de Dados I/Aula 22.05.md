# Recursividade
- Função fatorial, árvore (filho a esquerda e a direita, ambos são árvores);
- Deve ter condição de parada - **caso base** - evitar loop infinito;
- A cada chamada da função é criada na memória uma instância na pilha.
- A função é executada até que todas as instâncias sejam resolvidas (liberadas da pilha);

```C
int fatorial(int n){
	if(n <= 1){
		return 1
	}
	else { 
		return n * fatorial(n-1)
	}
}
```

## Vantagens
- Código limpo e legível

## Desvantagens
- Ficar atento a loops muito grandes.
- Desempenho

# Pilha
- Inserção e remoção de itens;
- Quantidade de elementos dado pelo topo;
- Objeto dinâmico e mutável;
- Lista LIFO (last-in first-out) : ultimo a entrar é o primeiro a sair
- push, pop
- underflow (tentativa inválida de desempilhar ou acessar um item de uma pilha vazia)
- overflow

```C
struct pilha{
	int topo;
	int item[TAMANHO_PILHA];
}
```

---

- [ ] Operador Seta e prioridade
- [ ] Recursividade de Fibonacci
- [ ] Por que Fibonacci cresce exponencialmente usando recursividade - desempenho em recursividade
- [ ] Estrutura Pilha
- [ ] Profundidade de aninhamento - contagem de parênteses
- [ ] exercício 8 - Pilha
- [ ] exercício 9 - prática de pilha

