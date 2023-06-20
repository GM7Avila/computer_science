# Tabela Hash

![[Pasted image 20230619200419.png]]

## Motivação
- Sabemos que:
	- Busca sequencial executa em tempo O(n)
	- Busca binária executa em tepo O(log(n))
	- Busca binária exige vetor ordenado
- Seria possível efetuar uma **busca** em tempo melhor do que O(log(n))?
- Quais restrições devem existir sobre os dados?

- Tabelas de Hash (ou Tabelas Hash) permitem buscas em tempo constante, satisfeitas algumas restrições.
- Essa estrutura pode ter vários nomes como: dicionário (python), mapas, arrays associativos, e assim por diante.
- A princípio a chave de busca pode ser de qualquer tipo.

![[Pasted image 20230619200445.png]]

## Glossário

1. ``Chave``: É o valor usado para indexar e buscar os dados na tabela hash. Cada elemento armazenado na tabela deve ter uma chave única.
    
2. ``Valor`` (ou dado): É a informação associada à chave na tabela hash. Pode ser qualquer tipo de dado, como um número, uma string, uma estrutura, etc.
    
3. ``Função de Dispersão (Hash Function)``: É uma função que recebe a chave como entrada e calcula um índice (ou endereço) na tabela hash onde o valor correspondente será armazenado. A função de dispersão é responsável por distribuir uniformemente as chaves pela tabela, minimizando colisões.
    
4. ``Slot``: É uma posição (ou localização) na tabela hash onde os elementos são armazenados. Cada slot pode conter um elemento (chave-valor) ou pode estar vazio.
    
5. ``Array`` (ou vetor): É a estrutura de dados subjacente utilizada para armazenar os slots da tabela hash. Geralmente, é um array de tamanho fixo, onde cada elemento representa um slot.
    
6. ``Colisões``: Ocorrem quando duas ou mais chaves são mapeadas para o mesmo slot na tabela hash. Existem diferentes técnicas para lidar com colisões, como encadeamento (chaining), endereçamento aberto (open addressing) e rehashing.

## Tipo Abstrato de Dados
- `retrieveItem(k)`: retorna uma entrada com chave igual a `k`, se ela existir. Caso contrário retorna `NULL`;
- `insertItem(k,v)`: insere uma entrada `v` na chave `k` se a chave não existir. Caso contrário, atualiza o valor associado a `k`.
- `deleteItem(k)`: remove a chave `k` e o valor associado a ela.
- Outros métodos: 
	- `size()` - retorna o número de entradas; 
	- `keySet()` - retorna uma lista encadeada de todas as chaves armazenadas na tabela;
	- `values()` - retorna uma coleção contendo todos os valores associados com as chaves armazenadas na tabela;
	- `entrySet()` - retorna uma coleção contendo todas as entradsa (chave-valor) da tabela.
![[Pasted image 20230619163101.png]]

## Implementação
- A prórpia chave deve ser usada para organizar os dados em memória - a chave deve indicar aonde está um determinado registro;
- Cada entrada da estrutura é composta por um par "*chave-valor*" (k, v). A associação entre k e v define o **mapeamento**;
- A chave é um identificador único, e deve ser vista como um "endereço" para seu valor;

- A tabela pode ser organizada em memória como um vetor, dado que esse permite acesso em tempo constante.
- As chaves podem ser de qualquer tipo de dado, **mas** para efetuarmos a busca no vetor, precisaremos de uma função que mapeie em **números inteiros**; 
- Strings, Objetos, Inteiros, Floats, etc → **Função de Mapeamento** → Inteiro

- Seja `h` a função que faz o mapeamento (também chamada de função de espalhamento) e `k` a chave, o endereço de memória será dado por `h(k)`.
- Se os valores retornados por `h(k)` forem bem destribuídos em um intervalo entre 0 e N-1, então precisaremos de um vetor de capacidade N;
- Assumindo ausência de ==colisões==, essa estrutura básica seria suficiente.

### função de Hash ou de espalhamento
- A função de hash `h` mapeia cada chave em um intervalo de 0 a N-1, onde N é a capacidade do arranjo.
- É possível tratar colisões, mas a melhor estratégia por enquanto é tentar evitá-las.
- Uma função de hash é boa se minimizada a ocorrência de colisões.

- A primeira tarefa da função será transformar chaves de tipos arbitrários em interios.
- Exemplo: vamos assumir que queremos armazenar informações de funcionários de uma empresa e indexar essas informações pelo `login` único da pessoa.
- O login pode ser o primeiro nome da pessoa, mas se este já foi escolhido por alguém, então outro deve ser selecionado pelo funcionário.

![[Pasted image 20230619164440.png]]
- Buscar o nome ulisses, seria buscar no vetor a posição 776;
- Entretando essa estratégia gera colisões: 
	- Orlando: 751
	- Odnalro: 751
	- Adriana: 720
	- Ariadna: 720

- Uma melhor implementação para a função de espalhamento (hash), levaria em conta a posição dos caracteres `c`, na cadeia C = (c0, c1, c2, ..., ck-1);
- Podemos então calcular a diferença das palavras, considerando que cada posição das strings, tenha um peso diferente elevado ao seu valor:

![[Pasted image 20230619170804.png]]
- Agora Orlando e Odnalro levam valores diferentes de identificação.   
![[Pasted image 20230619172129.png]]
- Entretanto, um valor alto de `a` pode levar a um overflow do intervalo dos inteiros.
- A compressão dos valores pode fazer parte da função nesse caso. O resto da divisão por N estabiliza os valores em um intervalo 0 à N-1;
	- ``i mod N`` 
	- Lembre-se: módulo é o resto da divisão, expresso em `%` em C++.
- O tamanho do array N pode aumentar ou diminuir o número de colisões. 
	- Se usarmos N = 1000, teremos muito menos colisões do que com N =100.

- ==Para ajudar o espalhamento das chaves, é interessante utilizar um **número primo para N** (para quebrar alguns padrões que possam se repetir)== - isso pode diminuir a chance de ocorrer padrões na distribuição de dados.
	- Por exemplo, se temos as chaves {200, 205, 210, 215, 220, ..., 600}..
		- Com N = 100, cada chave irá colidir com várias outras chaves.
		- Com N = 101, **não teremos colisões**

**mais sobre utilização dos números primos na tabela hash...**
1. **Redução de colisões:** Ao utilizar um número primo como tamanho da tabela hash, é possível reduzir a ocorrência de colisões. Isso ocorre porque números primos têm menos divisores, o que resulta em uma distribuição mais uniforme dos elementos ao longo da tabela. Dessa forma, é menos provável que vários elementos sejam mapeados para a mesma posição da tabela, o que reduz a quantidade de colisões.
2. **Evitar padrões de repetição:** Se um tamanho de tabela não for um número primo, pode ocorrer um padrão repetitivo na distribuição dos dados. Por exemplo, se o tamanho da tabela for uma potência de 2, a função de hash pode se concentrar em certos bits dos valores de entrada, resultando em agrupamentos indesejáveis de elementos na tabela. Ao utilizar um número primo como tamanho da tabela, os padrões repetitivos são quebrados, resultando em uma distribuição mais aleatória dos elementos.

Portanto, ao escolher um tamanho para uma tabela hash, utilizar um número primo pode ajudar a melhorar a eficiência e a distribuição dos dados, minimizando colisões e padrões repetitivos.

- Uma função Hash boa: produz um número baixo de colisões; é facilmente computável; é uniforme.

#### Fator de Carga
O fator de carga de uma tabela hash é uma métrica que ==indica a ocupação da tabela em relação à sua capacidade total==. É calculado dividindo o número de elementos armazenados na tabela pelo número total de slots disponíveis.

>Fc= Nº de Elementos (inseridos) / Capacidade da Tabela (tamanho do vetor)

- O resultado do cálculo é um valor entre 0 e 1. Quanto mais próximo de 1, maior é a ocupação da tabela. 
- Um fator de carga alto significa que a tabela está mais cheia, o que pode levar a um aumento no número de colisões (quando dois ou mais elementos são mapeados para o mesmo slot da tabela) e, consequentemente, a uma diminuição no desempenho da tabela hash.
- Idealmente, ==busca-se manter um fator de carga não tão baixo== para evitar colisões excessivas e garantir um desempenho eficiente da tabela (muito perto do zero, memória alocada sem uso; muito alta, poucos espaços vazios, mais chances de colisão).
- Dependendo da implementação da tabela hash, pode ser necessário redimensionar a tabela (aumentar sua capacidade) quando o fator de carga atinge um limite pré-definido. Isso é feito para evitar uma degradação significativa no desempenho e manter uma distribuição equilibrada dos elementos na tabela hash.

![[Pasted image 20230619200645.png]]

- Exemplo: vetor de tamanho 6;
	- Fator de Carga para 4 elementos: Fc = 4/6 - 0,67; 
	- Função de Espalhamento para **inserir** o elemento inteiro 41: 41 % 6 = 5, ou seja a chave de 41 é alocada na posiçaõ 5 do vetor.
	- Função de Espalhamento para **buscar** o elemento 13: 13 % 6 = 1; acessao a posição 1 do vetor e retorna se o elemento está na tabela ou não.

## Tratamento de Colisões
- Encadeamento Exterior ou Separado
	- Listas Encadeadas: ![[Pasted image 20230619204201.png]]
- Encadeamento Interior ou Aberto
	- **Heterogêneo**: Colisões ficam separadas no vetor dos elementos que já foram inseridos. Há uma região do vetor (a partir de certa posição) que é destinada para colisões. 
	  
	  Problema: *a partir do momento que a região das colisões começa a aumentar, teremos problemas de performance na busca pela chave. Pois será necessário percorrer toda a região para buscar o elemento (utilizando algoritmos de busca).*
	  
	  Remoção: *É necessário realocar um elemento para o topo da região de colisões após a remoção do próprio topo, para que o algoritmo não encontre o primeiro elemento `NULL` e entenda que não houve outras colisões - caso elas existem.*
		  Vazio ≠ Disponível -- *tratar a diferença no código*!!
	  
	- **Homogêneo (Teste Linear)** - os elementos de colisão são inseridos na mesma região que os elementos já inseridos (que provocaram a colisão). 

## Referência
> https://www.youtube.com/watch?v=jQ0r7P8rC1M
