# Árvore Binária
- Quando ordenadas elas condduzem a pesquisas, inserções e exclusões rápidas.
- Cada item em uma árvore consiste em informação juntamente com um elo ao membro esquerdo e um elo ao membro direito.

## Elementos

- Struct Nó de uma Árvore
```C
typedef struct treeNode{
	int valor;
	struct treeNode *left;
	struct treeNode *right;
	
} TreeNode;
```

- Struct ArvoreBinaria = raiz (NULL);
```C
typedef struct binaryTree{
	TreeNode *root;
}
```