# Tipos de Alocação de Memória

- [[Alocação Estática e Dinâmica]]

![[Pasted image 20230517172754.png]]

## Alocação Estática 
- O espaço para as variáveis é reservado no **início da execução**;
- Cada variável tem seu **endereço** fixado e a área de memória ocupada por ela se **mantém constante** durante **toda a execução**;
- São ==alocadas na Stack da Memória RAM== (tamanho disponível na stack é menor que a da heap); 
- Variáveis locais são liberadas da stack ao fim da execução da função, e globais, ao fim da execução do programa (sistema operacional desaloca automaticamente).
- **Toda variável declarada é alocada na memória stack** (- Samuca, Xavecoding).

## Alocação Dinâmica
- O espaço para as variáveis é alocado dinâmicamente durante a execução do programa.
- Pode ser criada ou eliminada em qualquer momento, ocupa o espaço na memória só quando está sendo utilizada.
- ==São alocadas na Heap (free Store) da memória RAM.==
- Liberação de memória feita **manualmente** pelo programador (PERIGO!)

```C
int *a = (int*)malloc(10 * sizeof(int));
float *b = (float*)calloc(5, sizeof(float));
free(a);
free(b);
```

![[Pasted image 20230517180230.png]]

### Por que usar memória Dinâmica?
- A alocação dinâmica é o professo que aloca memória em tempo de execução.
- Ela é utilizada quando não se sabe ao certo quanto de memória será necessário para o armazenamento dos elementos.
- Tamanho de memóra alocado conforme necessidade.
- Evita o desperdício de memória.

### Malloc 
- Memory ALLOCation
- Aloca um bloco de bytes **consecutivos** na memória heap e ==retorna o endereço desse bloco==.
- Inicializa todos os elementos com lixo de memória.
- ``tipo *v = (tipo*)malloc(n*sizeof(tipo));`` ou ``tipo *v = malloc(n*sizeof(tipo));`` 
- ``int *v = (int*)malloc(5*sizoef(int));`` aloca 5 espaços na memória heap consecutivos, sendo cada espaço possuindo 4 bytes - ou seja, 20 bytes consecutivos na memória. 
- é retornado o ==endereço base== da região alocada = endereço do primeiro elemento da sequência.
- ***como a função malloc retorna um endereço, devemos atribui-la à um ponteiro daquele tipo.***

### Calloc
- Aloca um bloco de bytes consecutivos, porém inicializa todos os valores com 0 (null para ponteiros).
- `tipo *v = (tipo*) calloc(n, sizeof(tipo));` ou `tipo *v = calloc(n,sizeof(tipo))`
- é retornado o endereço base da região alocada = endereço do primeiro elemento da sequência.