# Invariante de Loop

## Overview

> Provar que um algoritmo está correto para qualquer condição de um problema;

- Escolher uma **propriedade** do algoritmo que deseja-se provar a ==corritude==.
- Se a propriedade se manter antes da execução, durante e depois do loop envolvido, o algoritmo está ==correto==.
- A propriedade deve ser **==invariante**== ao loop.

---

*Exemplo* - provar que o algoritmo abaixo está correto por invariante de loop:

```c
int n = 10;
int soma = 10;

for(int i = 1;i <= n; i++){
	soma += i;
}
```

1. Escolher uma propriedade invariante ao loop: `soma`
	- Propriedade: a variável `soma` sempre resultará em um número inteiro positivo.
	- Se essa propriedade for verdadeira, antes, durante e depois do loop o algoritmo está correto.

2. Analisando:
	- Inicialização: soma é um inteiro positivo (antes);
	- Execução: soma continua sendo um inteiro positivo (durante a execução do loop);
	- Término do loop: soma continua sendo um inteiro positivo (ao final da execução do loop);
	- Então, está provado a corritude do algoritmo.

