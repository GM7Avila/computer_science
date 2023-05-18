# Operador Ponto e Operador Seta

## Operador Ponto
- Atribuir o campo de structs (registro) através de ponto. 
- Identificação de campo de uma variável.
```C
typedef struct {
	char nome[50];
	char apelido[30];
} rgFilho;

int main() {
	rgFilho filho;
	
	strcpy(filho.nome, "Guilherme Medeiros Avila");
	strcpy(filho.apelido, "Avila");
}
```

## Operador seta 
```C
int main(){
	rgFilho filho;
	
	rgFilho *p; // ponteiro do tipo rgFilho
	p = &filho; // ponteiro p recebe o endereço de filho (aponta para filho)
	
	printf("Nome: %s\n", p -> nome);
	printf("Apelido: %s\n\n", p -> apelido);
}
```

```C
int main(){
	rgFilho filho;
	
	rgFilho *p; // ponteiro do tipo rgFilho
	p = &filho; // ponteiro p recebe o endereço de filho (aponta para filho)
	
	printf("Nome: %s\n", (*p).nome);
	printf("Apelido: %s\n\n", (*p).apelido);
}
```

- Note: ``*(p)`` é o conteúdo do ponteiro, então  ``*(&rgFilho)``, logo ``rgFilho``. 
