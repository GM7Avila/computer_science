# Tipos Abstratos de Dados
- Visa **desvincular** o tipo de dado(*estrutura de dados* e *operações* que manipulam) de sua **implementação**;
- Quando definimos um tipo abstrato de dados (TAD), estamos preocupados com *o que ele faz* (especificação) e não *como ele faz* (implementação);
- Ideia parecida com [[03.1. Encapsulamento|Encapsulamento]] em Orientação a Objetos:
	- Encapsulamos os dados e detalhes do usuário, fornecendo apenas uma [[03.1.2 Interfaces|interface pública]] (métodos/operações) para manipulá-los.

![[Pasted image 20230609233916.png]]

- Especificações: ex.: assinaturas das funções (sem implementações)
- Semântica: documentação, ex.: em comentários
- Implementação: implementação das funções;

## TAD em Linguagem C
- Separamos a **especificação** do tipo em um ==arquivo de cabeçalho (.h)== e sua **implementação** em um ==arquivo fonte (.c)== 
	- `seu_tad.h` especificação da TAD
	- `seu_tad.c` implementação da TAD

- Os programas ou outras TAD's que utilizam seu_tad devem incluir suas especificações: 
	- `#include "seu_tad.h"` - arquivo de cabeçalho (especificações, assinaturas + semântica)

## Vantagens de TAD
- Reutilização:
	- abstração de detalhes da implementação;
- Facilidade de manutenção:
	- mudanças de implementação do TAD não afetam o código fonte dos programas que o utilizam (ocultamento da informação);
- Corretude:
	- Códigos testados em diferentes contextos;

