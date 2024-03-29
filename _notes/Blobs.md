# Binary Large Objects

## Definição
- Banco dedados, armazenamento de dados, [[Git Object Model|Git]], etc.
- É uma ==coleção de dados binários== armazenados como uma única unidade:
	- imagens
	- arquivos de áudio
	- arquivo de vídeo
	- documentos 
	- qualquer outra forma de dados binários grandes.

- É um arquivo bruto - **não estruturado** - que pode ter uma unidade de armazenamento digital com vários gigabytes de tamanho - ele é compactado em um único arquivo que é armazenado em um banco de dados.

## Gerenciado Blobs
- Um banco de dados não tem capacidade de gerenciar blobs - devido a sua natureza e por serem pesados. 
- Uma opção na escolha de uma solução de **armazenamento para BLOB** é o [[Digital Asset Management (DAM)]].
	- Assim a responsabilidade de gerenciar os blobs cabe ao DAM, e o servidor pode cumprir seu papel com eficiência.