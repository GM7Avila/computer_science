# Tree Object

- Uma árvore é um [[Git Object Model|objeto]] simples que possui vários [[Funcionamento dos Ponteiros|ponteiros]] para [[Object - Blob|blobs]] e outras árvores - geralmente representa o conteúdo de um diretório ou subdiretórios.

<img src="http://shafiul.github.io/gitbook/assets/images/figure/object-tree.png">

- A árvore contém uma lista de entradas, cada um com um modo, tipo, valor SHA1 e nome. 
- Podemos usar o comando `git ls-tree` para listar as informações de um objeto tree:

> Note - na presença de **[[Submodules]]**, as árvores também podem conter commits como entrada.

```
$ git ls-tree fb3a8bdd0ce
100644 blob 63c918c667fa005ff12ad89437f2fdc80926e21c    .gitignore
100644 blob 5529b198e8d14decbe4ad99db3f7fb632de0439d    .mailmap
100644 blob 6ff87c4664981e4397625791c8ea3bbb5f2279a3    COPYING
040000 tree 2fb783e477100ce076f6bf57e4a6f026013dc745    Documentation
100755 blob 3c0032cec592a765692234f1cba47dfdcc3a9200    GIT-VERSION-GEN
100644 blob 289b046a443c0647624607d471289b2c7dcd470b    INSTALL
100644 blob 4eb463797adc693dc168b926b6932ff53f17d0b1    Makefile
100644 blob 548142c327a6790ff8821d67c2ee1eff7a656b52    README
...
```

> Um objeto referenciado por uma árvore pode ser um blob, representando o conteúdo de um arquivo, ou outra árvore, representando o conteúdo de um subdiretório. Como as árvores e os blobs, como todos os outros objetos, são nomeados pelo hash SHA1 de seu conteúdo, duas árvores têm o mesmo nome SHA1 se e somente se seu conteúdo (incluindo, recursivamente, o conteúdo de todos os subdiretórios) for idêntico. Isso permite que o git determine rapidamente as diferenças entre dois objetos de árvore relacionados, pois pode ignorar quaisquer entradas com nomes de objetos idênticos.