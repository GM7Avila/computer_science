# Struct
- Pacote de variáveis que podem ter tipos diferentes;
- Visa representar grupos de dados que resultem em algo mais concreto;
- Cada variável é um **campo** do registro;
- Em C, registros são conhecidos como `structs`;

- Acesso dos elementos utilizando o ponto: ``structX.name``.

## Padrões de definição
```C
struct [nome]{
	tipo campo1;
	tipo campo2;
	tipo campoN;
};

struct [nome] [nomeDaVariavel];
```

```C
struct [nome]{
	...
} [nomeDaVariavel], [nomeDaVariavel];
```

```C 
typedef struct [Nome]{
	...
}[TipoDeDado]; // apelido
```

## Alocação Estática de Structs
![[Pasted image 20230609002141.png]]

## Alocação Dinâmica de Structs

- Podemos alocar ==instâncias== de structs **dinâmicamente**.
```C
Aluno *ted = (Aluno*) calloc(1, sizeof(Aluno));
```

- Para acessar os **campos** de uma struct partindo de um **ponteiro** usamos o operador [[Operador Ponto e Operador Seta|operador seta]] :
```C
strcpy(ted->nome, "Ted");
ted->idade = 10;
```

![[Pasted image 20230609002548.png]]

## Exemplo CRUD de Structs
- *Create Read Update Delete*
 - **EX.:** Considere que o Aluno possui um livro favorito, que por simplificação possui título, páginas e preço.
	 - Codifique a struct de Livro e adapte a struct de Aluno;
 - Crie funções de Criação(C), Delete(D) e de impressão para as Structs.
 - Na função Delete, garanta que o ponteiro é atribuído como `NULL` após a desalocação. 

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

typedef struct _livro {
    char titulo[100];
    unsigned int num_paginas;
    float preco;
} Livro;

typedef struct _aluno {
    char nome[100];
    int idade;
    Livro *livro_fav;
} Aluno;

// "construtor" para livros
Livro *create_livro(const char *titulo, unsigned int num_paginas, float preco) {
    Livro *livro = (Livro *) calloc(1, sizeof(Livro));
    strcpy(livro->titulo, titulo);
    livro->num_paginas = num_paginas;
    livro->preco = preco;
    return livro;
}

void destroy_livro(Livro **livro_ref) {
    Livro *livro = *livro_ref;
    free(livro);
    *livro_ref = NULL;
}

Livro *copy_livro(const Livro *livro) {
    return create_livro(livro->titulo, livro->num_paginas, livro->preco);
}

void print_livro(const Livro *livro) {
    printf("Titulo: %s\n", livro->titulo);
    printf("Num Paginas: %d\n", livro->num_paginas);
    printf("Preco: R$ %.2f\n\n", livro->preco);
}

Aluno *create_aluno(const char *nome, int idade, const Livro *livro_fav) {
    Aluno *aluno = (Aluno *) calloc(1, sizeof(Aluno));
    strcpy(aluno->nome, nome);
    aluno->idade = idade;
    aluno->livro_fav = copy_livro(livro_fav);
    return aluno;
} 

void destroy_aluno(Aluno **aluno_ref) {
    Aluno *aluno = *aluno_ref;
    destroy_livro(&aluno->livro_fav);
    free(aluno);
    *aluno_ref = NULL;
}

void print_aluno(const Aluno *aluno) {
    printf("Nome: %s\n", aluno->nome);
    printf("Idade: %d\n", aluno->idade);
    puts("Livro Favorito");
    puts("-----");
    print_livro(aluno->livro_fav);
}
  
bool livros_sao_iguais(const Livro *livro_1, const Livro *livro_2) {
    if (strcmp(livro_1->titulo, livro_2->titulo) == 0) {
        return true;
    }
    else {
        return false;
    }
}
  
int main() {

    Livro *livro_harry = create_livro("Harry Potter 1", 200, 25);
  
    print_livro(livro_harry);
    livro_harry->preco = 10;
    print_livro(livro_harry);
  
    Aluno *ted = create_aluno("Ted", 20, livro_harry);
    print_aluno(ted);
  
    // os exemplares sao iguais?
    puts("\nted->livro_fav == livro_harry");
    puts("os exemplares sao iguais?");

    if (ted->livro_fav == livro_harry) {
        puts("TRUE");
    }
    else {
        puts("FALSE");
    }
    puts("\n");
    
    // a obra é a mesma?
    puts("\nlivros_sao_iguais");
    puts("a obra é a mesma ?");

    if (livros_sao_iguais(ted->livro_fav, livro_harry)) {
        puts("TRUE");
    }
    else {
        puts("FALSE");
    }
    
    puts("\n");
    destroy_livro(&livro_harry);
    print_aluno(ted);
    destroy_aluno(&ted);
    return 0;
}
```

## Vetores de Structs
- Proposta de implementação: criar um ponteiro que aponte para um array de ponteiros para alunos, ao em vez de um ponteiro que aponte para um array de alunos em si (sendo assim será o uso de um ponteiro de ponteiro de alunos). 
- Assim podemos diferenciar quando um ponteiro para aluno é um vetor ou um elemento só. Essa é uma ==boa prática==.

```C
int main(){
	
	Livro **vet = (Livro **) calloc(3, sizeof(Livro *));
	
	vet[0] = create_livro("Entendendo algoritmos", 261, 75);
	vet[1] = create_livro("Eloquent JavaScript", 300, 120);
	vet[2] = create_livro("Engenharia de Software", 1055, 50);
	
	for(int i=0; i<3; i++){
		print_livro(vet[i]);
	}
	
	for(int i=0; i<3; i++){
		destroy_livro(&vet[i]);
	}
	
	free(vet);
	vet = NULL; 
}
```

- Podemos também criar uma estrutura `vetor_livros` para que o código fique mais limpo. Dessa forma as informações do ponteiro duplo e o número de elementos do vetor fica sempre agrupado.

## Exercícios

1. Qual a saída do programa baixo que imprime o tamanho das variáveis? Explique sua resposta (considere o padrão do comportamento da memória RAM estudado).
```C
Livro livro1;
Livro *livro2 = (Livro*) calloc(1, sizeof(Livro));
printf("Tamanho do livro: %d bytes\n", sizeof(livro1));
printf("Tamanho do livro: %d bytes\n", sizeof(livro2));
return 0;
```

- Resposta:
> No primeiro print será impresso 100 x `1 bytes` = `100 bytes` + 1 x `4 bytes` = `4 bytes` + 1 x `4 bytes` = `4 bytes`; Total: `108 bytes` - alocação estática.
> No segundo, temos um ponteiro para Livro, ou seja, apenas `8 bytes`. 
> Já o tamanho do conteúdo do livro 2 (`sizeof(*livro2)`) = `108 bytes`

2. Analise o trecho de código abaixo e explique se os itens propostos funcionam ou não. Utilize a representação de memória se necessário. 

```C
Livro livro1 = {.titulo = "Harry Potter 1", 30.00, 250};
Livro livro2 = CriaLivro("O Senhor dos Anéis", 40.00, 343);
```

a.  `printf( "titulo1 = %s\n", livro1.titulo );`
b.  `printf( "titulo1 = %s\n", livro1->titulo );`
c.  `printf( "titulo1 = %s\n", &livro1->titulo );`
d.  `printf( "titulo1 = %s\n", (&livro1)->titulo );`
e.  `printf( "titulo2 = %s\n", livro2.titulo );`
f.   `printf( "titulo2 = %s\n", livro2->titulo );`
g.  `printf( "titulo2 = %s\n", *livro2.titulo );`
h.  `printf( "titulo2 = %s\n", (*livro2).titulo );`
i.   `printf( "titulo2 = %s\n", livro2[0].titulo );`

> a.Funciona normalmente, livro1 é uma declaração estática, sem a utilização de ponteiros, portanto podemos usar normalmente o operador ponto.
> b. Não , espera-se o uso de operador seta quando utilizamos ponteiros.
> c. Não, precisa do parênteses.
> d. Sim, `&livro1` se comporta como um ponteiro de Livro.
> e. Não, espera-se o uso de operador ponto quando utilizamos o próprio tipo da estrutura.
> f. Sim
> g. Não
> h. Sim, pois seu conteúdo é o prórpio tipo, podendo usar o operador ponto.
> i. Sim, pois a primeira posição é o mesmo endereço da estrutura - aritmética de ponteiros.