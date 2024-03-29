# Binary Large Object

> Os Blobs são fundamentais para o funcionamento interno do Git, pois permitem que ele armazene e rastrie as versões de cada arquivo em um projeto.
> 
> Quando revisamos o **histórico de commit**, estamos visualizando essencialmente uma **série de blobs** que representam o estado dos arquivos em cada *ponto de tempo*.

<img src="http://shafiul.github.io/gitbook/assets/images/figure/object-blob.png">

- É um tipo de [[Git Object Model|objeto]] que representa o conteúdo real de um arquivo.
	- Bloco de dados binários que armazena o conteúdo de um arquivo específico em um determinado ponto do tempo.

- Quando damos `add` a um arquivo no repositório Git, é criado um **blob** para representar o *conteúdo* deste arquivo.
	- armazenado em `.git/objects`, e identificados por um hash [[Hash SHA-1|SHA-1]] - calculado com base no conteúdo do arquivo.

- Cada vez que fazemos um commit que *modifica um arquivo*, o Git cria um novo **blob** para representar o conteúdo modificado desse arquivo.

- O blob é inteiramente definido pelos seus **dados**, se dois arquivos em um diretório tiverem o mesm conteúdo, eles *compartilharão o mesmo objeto blob*. O objeto é totalmente *independente* de sua localização na árvore de diretórios e renomear um arquivo não altera o bojeto ao qual o arquivo está associado.
