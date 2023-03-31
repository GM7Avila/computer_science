# Função
Estruturas que agrupam um bloco de comandos, que retornam um único valor ao final da execução (ou então void):

```C
tipo funcao (tipo param1, tipo param2, ... tipo paramN){ 
	comandos... 
	return valor_de_retorno; 
}
```
- Toda a função deve possuir um tipo, que determina o tipo do valor de retorno.
- Uma função pode não ter parâmetros, basta não informa-los.
- Funções não podem *ser declaradas* dentro de outras funções, ou seja, C não suporta [[Funções aninhadas|funções aninhadas]] (como Python e JavaScript).
