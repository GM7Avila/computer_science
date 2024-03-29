# Como o git armazena os arquivos?
## Git Object Model

> [!NOTE] O que são objetos no Git?
> São unidades básicas de armazenamento de dados. Eles representam diferentes aspectos do conteúdo e do histórico de um repositório Git. 
> 
> Cada ojbto é armazenado em um arquivo no diretório `.git/objects` do repositório Git. Os objetos são identificados por seus hashes SHA1, calculados com base no conteúdo do objeto, e usados para recuperar e referenciar objetos em operações Git.
### Git add
1. Inicialmente faremos o `git add` de um repositório recém criado, com apenas um arquivo local `a.c` (untracked).
```bash
git add .
```

```bash
[avila@mint~/Desktop/teste]$ tree .git

.git
├── branches
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── objects
│   ├── a5
│   │   └── 517c7e85ee3f523636abbd3eb89157e34fa7ef
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
```

- Podemos notar que em objects foi criado uma pasta `6f` com o arquivo `16acbbc80d444145a09d897a93591cd806d3f8` - que é um **[[Hash SHA-1|hash SHA-1]]** que representa um ==**objeto**== dentro do diretório. 

- O hash é feito de acordo com o conteúdo do objeto, assim temos algumas vantagens em relação a isso:
	- o Git pode facilmente determinar se dois objetos são identicos ou não apenas comparando-os.
	- como o nome dos objetos são calculados da mesma maneira em todos os respositórios, o mesmo conteúdo, armazenado em dois repositórios diferentes, será sempre armazenado com o *mesmo nome*.
	- o Git pode detectar **mudanças/erros** ao ler um objeto, *verificando se o nome* do objeto ainda é o hash SHA-1 do seu conteúdo original. 
### Objects

> [!NOTE] Tipos de objetos no Git
> 1. **Blob** (conteúdo de arquivo) - Contém o conteúdo real de um arquivo. É armazenado como arquivo binário, sem nenhuma estrutura adicional.
> 2. **Tree** (árvore de diretórios) - Um objeto tree contém informações sobre a estrutura de diretórios em um determinado ponto no tempo. Ele armazena pares de *modo/permissão* e nome de arquivo, juntamente com os hashes SHA-1 dos blobs (ou sub-árvores, se houver) correspondentes a esses arquivos/diretórios.
> 3. **Commit** - Representa um ponto na história do repositório Git. Ele contém metadados sobre o commit, como autor, mensagem, data e hora, além de um hash SHA-1 que *aponta para a tree* que representa o estado do repositório no momento do commit.
> 4. **Tag** - USado para marcar um commit específicio com um nome significativo. Contém informações sobre o nome da tag, a mensagem associada e o hash SHA-1 do commit que está sendo marcado

- Especificamente, este hash identifica um objeto dentro do diretório de objetos do git, que é aonde o git armazena os ==**blobs**== (que são os conteúdos de arquivos), **==árvores==**, **==commits==** e ==**tags**==.
  
- Cada objeto possui três campos:
	- type - tipos do objeto
		- [[Object - Blob|Blob]] 
		- [[Object - Tree]] 
		- Commit 
		- Tag
	- size - size of the contents
	- content

---
- Techo do código fonte do Git de um `object` (escrito em C): um array representando os possíveis tipos de objeto:
	- link: https://github.com/git/git/blob/master/object.c
```C
static const char *object_type_strings[] = {
	NULL,		/* OBJ_NONE = 0 */
	"commit",	/* OBJ_COMMIT = 1 */
	"tree",	/* OBJ_TREE = 2 */
	"blob",	/* OBJ_BLOB = 3 */
	"tag",		/* OBJ_TAG = 4 */
};
```
---
### Fazendo o primeiro commit
- Voltando ao exemplo inicial, agora fazendo um commit:
```bash
$ git commit -m "first commit"

[master (root-commit) ffc3f70] First commit
 1 file changed, 3 insertions(+)
 create mode 100644 a.c
```

- O commit também gera um objeto com hash: `ffc37f70`
	- Consultando no git log:
```bash
$ git log

commit ffc3f70ba3d750485607d1c621c094d76b20c21d (HEAD -> master)
Author: GM7Avila <GM7Avila@email.com>
Date:   Thu Mar 28 12:47:32 2024 -0300

    First commit
(END)
```

- E então a estrutura dos objects em `.git/objects` atualizado fica assim:
	- `a5517` : (Object) blob
	- `f9bbb` : (Object) tree
	- `ffc3f` : (Object) commit
	  
```
[...]
  |
  ├── objects
  │   ├── a5
  │   │   └── 517c7e85ee3f523636abbd3eb89157e34fa7ef
  │   ├── f9
  │   │   └── bbb55b6a5b01fc7f500557c407bc6c22d44ac0
  │   ├── ff
  │   │   └── c3f70ba3d750485607d1c621c094d76b20c21d
  │   ├── info
  │   └── pack
  |
[...]
```

- Internamente, o commit está ligado aos blobs através dos objetos tree:
```
COMMIT                 TREE                    BLOB
+--------------+       +----------------+      +------------+
| commit ffc3f |       | tree f9bbb55   |      | blob a5517 |
| First Commit |------>| blob a5517 a.c |----->| a.c        |
+--------------+       +----------------+      +------------+
```

---
- **NOTE** - Se fosse o caso de ter feito o commit de mais de um arquivo, a tree em questão apontaria para dois blob diferentes:  
```
COMMIT                 TREE                    BLOB
+--------------+       +----------------+      +------------+
| commit ffc3f |       | tree f9bbb55   |      | blob a5517 |
| First Commit |------>| blob a5517 a.c |----->| a.c        |
|              |       | blob 3c211 b.c |      +------------+  
+--------------+       +--------+-------+      
.                               |              BLOB
.                               |              +------------+
.                               +------------->| blob 3c211 |
.                                              | b.c        |
.                                              +------------+
```
---
### Snapshots
- Assim ao visitar o commit ele apontará para as informações referente aquel instante. Essa associação que a **tree** faz, é o que conhecemos como [[snapshot]] (foto do estado atual do projeto; ==a tree é o componente que representa o Snapshot de um projeto em determinado momento==).
	- Cada entrada na árvore aponta para um blob correspondente ao conteúdo do arquivo naquele momento.
	- Dessa forma, o snapshot em um commit captura não apenas o estado atual dos arquivos, mas também sua estrutura de diretórios e a relação entre eles.
	- Assim, o snapshot de um commit **captura** não apenas o **==estado atual==** dos arquivos, mas também sua **==estrutura de diretórios e a relação entre eles==**.

> Essa abordagem de **snapshot** é fundamental para o funcionamento do Git, pois permite que cada commit represente um estado *completo e independente* do projeto em um determinado momento, facilitando a recuperação de versões anteriores do projeto e o acompanhamento das mudanças ao longo do tempo.

---
### Fazendo um novo commit
