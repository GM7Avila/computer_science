# Linguagens Regulares
Em Teoria da Computação, uma linguagem regular é uma linguagem que pode ser reconhecida por um autômato finito determinístico (DFA) ou não determinístico (NFA), ou pode ser descrita por uma expressão regular. Uma linguagem regular é uma classe importante de linguagens formais que são usadas em muitas aplicações, como reconhecimento de padrões, análise léxica de linguagens de programação e processamento de textos.

As linguagens regulares são fechadas sob operações regulares, como união, interseção e complemento. Elas também podem ser usadas para definir gramáticas regulares, que são gramáticas formais que geram apenas linguagens regulares.

A Teoria da Computação também estuda propriedades das linguagens regulares, como a bomba de estado, que é uma propriedade que garante que todas as linguagens regulares são finitas ou infinitas e periódicas.

Em resumo, as linguagens regulares são uma classe importante de linguagens formais que podem ser reconhecidas por autômatos finitos ou expressões regulares, e são usadas em várias aplicações em Ciência da Computação.

## Exemplo
Um exemplo simples de uma linguagem regular é a linguagem formada por todas as sequências de zero ou mais caracteres 'a' seguidos por um único caractere 'b'. Essa linguagem pode ser descrita por uma expressão regular simples, como a seguinte:

```
(a* b)
```

Isso significa que a linguagem é formada por zero ou mais caracteres 'a', seguidos por um único caractere 'b'. Alguns exemplos de sequências que pertencem a essa linguagem são:

```
b
ab
aab
aaab
```

Podemos construir um autômato finito determinístico (DFA) ou não determinístico (NFA) para reconhecer essa linguagem. O DFA correspondente teria um estado inicial que se move para um estado final quando lê um caractere 'b', e um estado que se mantém lendo zero ou mais caracteres 'a' até que encontre um 'b'. A partir do estado final, o DFA iria para um estado de rejeição se lê qualquer caractere 'a' ou 'b'.