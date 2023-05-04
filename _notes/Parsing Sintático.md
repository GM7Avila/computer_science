# Parsing Sintático
É uma etapa fundamental do processo de compilação, na qual o analisador sintático (parser) verifica se a **sequência de tokens** (símbolos léxicos) gerada pelo **analisador léxico** (lexer) está de acordo com a [[Linguagem|gramática]] da linguagem. Essa verificação envolve a construção de uma **árvore sintática** que presesenta a estrutura gramatical do código fonte.

Durante a análise sintática, o parser utiliza uma ou mais [[Pilha (Stack)|pilhas]] para armazenar informações sobre a estrutura sintática do código fonte. A pilha é responsável por realizar a análise sintática por meio do parser, utilizando a técnica chamada de *parsing preditivo recursivo descendente*.

![[Pasted image 20230503152845.png]]

Nessa técnica o analisador sintático utiliza a pilha para armazenar uma sequência de símbolos da gramática que espera encontrar no código fonte. À medida que o analisador léxico fornece os tokens, o parser verifica se o próximo símbolo esperado está no topo da pilha. Se estiver, o parser remove o símbolo da pilha, e consome o token do analisador léxico. Se não estiver, o passe gera um **erro de sintaxe**. 
