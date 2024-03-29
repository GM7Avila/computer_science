# Backtracking

> Quando um beco sem saída é alcançado, o algoritmo volta ao ponto de decisão anterior e explora um caminho diferente até que uma solução seja encontrada ou todas as possibilidades tenham sido esgotadas.

Backtracking é uma técnica de programação usada para encontrar todas as soluções possíveis para um problema. Ela consiste em uma busca exaustiva por todas as combinações possíveis de soluções e é frequentemente usada em problemas de otimização, jogos, algoritmos de busca e outros.

A técnica de backtracking usa uma estrutura de dados em pilha para armazenar informações sobre as soluções parciais já exploradas e, assim, permitir a retroceder (backtrack) e explorar outras possibilidades. A pilha é usada para manter um registro dos estados anteriores e, assim, permitir que o algoritmo retorne a um estado anterior e tente uma nova solução.

A pilha é usada para armazenar informações sobre as soluções parciais que já foram exploradas. Quando uma solução parcial é encontrada, ela é armazenada na pilha e o algoritmo continua a explorar outras possibilidades. Se, em algum momento, uma solução parcial não puder ser estendida para uma solução completa, o algoritmo retrocede para o estado anterior (desempilhando a pilha) e tenta outra possibilidade.

Assim, a técnica de backtracking usa uma pilha para manter um registro dos estados anteriores e permitir que o algoritmo retroceda e explore outras possibilidades, até encontrar todas as soluções possíveis.