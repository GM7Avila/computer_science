# Autômatos, Computabilidade e Complexidade
## Teoria da Computação

A **Teoria da Computação** é composta de três partes centrais:
-   Linguagens Formais e Autômatos
-   Computabilidade
-   Complexidade

Essas áreas são **interligadas** pela questão:

-   Quais são as capacidades e as limitações fundamentais e as fundamentais dos computadores?

### Teoria das Linguagens Formais e Autômatos
- Conceito de [Linguagem](Linguagem.md)
-   Trata das definições e propriedades de modelos matemático de computação:
    -   Um modelo chamado de **Autômato** ([Autômato Finito](Autômato%20Finito.md) - conceito de, que é usado em processamento de textos, projeto de hardware, entre outros…)
    -   Outro modelo, denominado **Gramática** ( Exemplo: Gramática Livre-de-Contexto, que é utilizado em compiladores, processamento de linguagem natural, entre outros…).

### Teoria da Computabilidade
- Apresenta a Máquina de Turing (MT)
	- Apresenta a Máquina de Turing (MT)
	- Estabelece a conexão entre a noção informal de algoritmo (solúvel efetivamente) e a definição precisa por uma MT.
	- **Máquina de Turing** (MT), estabelece a conexão entre noção informal de algoritmo (solúvel efetivamente) e a definição precisa por uma MT.
	    -   ***Se um problema não pode ser resolvido por uma MT, então não existe nenhuma soluçã computável para ele.***
		    - Um exemplo é o problmea de verificar se um enunciado matemático é verdadeiro ou falso.
			    - Parece uma questão natural para ser resolvida por um computador.
			    - Mas nenhum algoritmo de computador (ou MT) pode realizar essa tarefa.

### Teoria da Complexidade
- Trata da dificuldade de resolver um problema.
	- Classificações de problema de acordo com a **dificuldade computacional**.
	- Nem todos os **problemas computáveis**, podem ser resolvidos na prática: os recursos computacionais requeridos (tempo ou espaço) podem ser proibitivos.
	- Um ***exemplo*** é o **problema de fatoração de números grandes**
		- ***Nem mesmo um supercomputador pode encontrar o todos os fatores de um número grande (> 500 dígitos) antes do sol se apagar*** 
		- Por isso é importante saber se um problema é complexo (assim sabemos as técnicas que serão necessárias, eurísticas e aproximações para resolver o problema; assim como existem áreas da computação em que é "bom" um problema ser complexo, como Criptgorafia.
		
