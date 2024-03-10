# Estrutura Fila
- Dados são inseridos sempre no final da fila;
- Quando um atendimento é realizado, o primeiro elemento é retirado da fila;
- Tipo FiFo: *First-In, First-Out*

## Conjunto de operações
- Inserir na fila: ocorre sempre no final da fila (fila sem prioridade)
- Remover da fila: ocorre sempre no início da fila.

## Nós
```C
typedef struct node{
	int content;
	struct node *next;
} Node;
```

## Inserindo Elementos
```C
void inserir(Node **f, int num){

    // aux -> ponteiro auxiliar para o inicio da fila

    Node *aux, *new = malloc(sizeof(Node));

    if( new != NULL ){
        new->value = num;
        new->next = NULL;

        if(*f == NULL){
            *f = new;
        } else {
            aux = *f;

            // Percorre a fila até o final
            while(aux->next)
                aux = aux->next;
            aux->next = new;

        }
    } else 
        printf("Erro de alocação");
}
```
