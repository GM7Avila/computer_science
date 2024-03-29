# Commit Object

- Um commit é geralmente criado por `git commit`, que cria um commit cujo o parent normalmente é o **HEAD** atual, e cuja [[Object - Tree|árvore]] é obtida do conteúdo atualmente armazenado no índice do commit.

> HEAD é uma referência para a branch atual
> 
> $ cat .git/HEAD
> ref: refs/heads/master

> git log verifica o log de todos os commits e a HEAD e branch atual

- Vincula o estado de uma árvore com uma descrição do commit.
- O objeto commit é definido pelos seguintes elementos:
	- **tree** - o nome SHA1 da tree, representando o conteudo do diretório no determinado instante.
	- **parent(s)** - o nome SHA1 dos commits anteriores na história do projeto (pais). Pode haver um ou mais - no caso da imagem há apenas um commit anterior (apenas um pai). Um commit sem pai é chamado commit *root*. Um projeto também pode ter *múltiplas raízes*, embora isso não seja comum.
	- **author** - quem fez a alteração junto da data e hora
	- **commiter** - quem realizou o commit
	- **comment** - descrição do commit feito pelo commiter.

<img src="http://shafiul.github.io/gitbook/assets/images/figure/object-commit.png">

O commit em si não conhém nenhuma informação sobre o que realmente mudou; todas as alterações são calculadas comparando o conteúdo da tree referenciada nesse commit com as árvores associas aos seus commits pais. 

> Em particular, o git não tenta registrar renomeações de arquivos explicitamente, embora possa identificar casos em que a existência do mesmos dados de arquivo em caminhos de mudança sugere uma renomeação.