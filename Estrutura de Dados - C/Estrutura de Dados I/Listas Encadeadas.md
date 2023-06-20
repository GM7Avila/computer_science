# Lista Encadeada 

## Motivação - Um problema em arrays
- Suponha que queremos armazenar uma lista de alunos em um vetor/array;
- Sabemos inicialmente, que a quantidade de alunos é 5;
- Então alocamos um vetor de 5 posições e inserimos os alunos:

```C
Aluno **alunos = (Aluno**)calloc(5, sizeof(Aluno*));

for(int i=0; i<5; i++){
	alunos[i] = create_aluno();
}
```

- E se quiséssemos inserir um novo aluno?
- O vetor atual não tem espaço suficiente, então devemos implementar uma solução:
	- Opção 1: Utilizando `realloc` (comando perigoso);
	- Opção 2: Criar um novo vetor com 6 posições e copiar os dados (a cada novo aluno, precisamos alocar um novo vetor, copiar os dados de um vetor a outro é algo ineficiente);
	- Opção 3: Criar um vetor muito grande, com muitas posições, de modo que "sempre" teremos espaço para inserir um novo aluno (problema: deslocamento dos elementos);

- A melhor implementação para isso, é utilizar uma **lista encadeada**:

## Lista Encadeada
- Uma lista encadeada é uma representação de uma **sequência de elementos/objetos** na memória do computador;
- Os elementos ==nós da lista==, são armazenados em **posições quaisquer da memória** e são ligados por **ponteiros**;
- Logo, elementos consecutivos da lista **não** ficam necessariamente em **posições consecutivas** da memória;

![[Pasted image 20230610001857.png]]

- Próximo do último elemento é ``NULL`` - aterramento;
- Aritmética de ponteiros não funciona aqui! (Não são endereços consecutivos - blocos não contidos na memória)

### Outras classificações
- Lista circulares: Último elemento aponta para o primeiro elemento da lista (lista circular); 

![[Pasted image 20230610002422.png]]

- Lista duplamente encadeada: Cada nó possui um ponteiro para o próximo nó e para o nó anterior.

![[Pasted image 20230610002450.png]]

- Lista duplamente encadeada circular: 

![[Pasted image 20230610002521.png]]

## Lista Encadeada Simples
- Próximo elemento após o último elemento da lista é `NULL`;
- Se o ponteiro L aponta para o **primeiro nó da Lista**, podemos dizer que **L é uma Lista**.
```C
typedef struct node {
	int value;
	struct node *next;
} Node;
```

**Outra implementação**
- Temos um **Tipo próprio** para a Lista, que gardará um ponteiro para seu início;
- Este novo tipo nos dá liberdade para **guardarmos outras informações**, como número de nós da Lista, outros ponteiros, etc;
- Com isso, podemos **otimizar** algumas operações comuns de Listas encadeadas;
- Esta solução é parecida com a Lista Encadeada **com Cabeça** porém ==mais robusta==.

![[Pasted image 20230610113727.png]]

- Estrutura de Lista Encadeada:
```C
typedef struct _linked_list {
	Node *begin;
	// pode implementar mais campos para futuras implementações
} LinkedList;
```

- Criando as funções de operação de uma lista encadeada:
	- `create_linkedList()` cria uma lista encadeada, retornando um ponteiro de lista `L`;
	- ``create_node(int val)`` cria um nó de uma lista encadeada, passando um valor inteiro `val` para o nó, e em seguida retornando o ponteiro de nó `N`.

```C
LinkedList *create_linkedList(){
    LinkedList *L = (LinkedList *) calloc(1, sizeof(LinkedList));
    L->begin = NULL;
    
    return L;
}
```

```C
Node *create_node(int val){
    Node *N = (Node *) calloc(1, sizeof(Node));
    N->value = val;
    N->next = NULL;

    return N;
}
```

