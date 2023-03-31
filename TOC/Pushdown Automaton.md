# Autômato de Pilha

Um Autômato de Pilha (PDA, do inglês *Pushdown Automaton*) é uma extensão do *autômato finito* que utiliza uma **pilha como memória auxiliar**. A pilha pode ser usada para armazenar informações durante a leitura de uma entrada e assim ==permitir que o autômato reconheça linguagens que não são regulares==, como por exemplo as linguagens do tipo {a^n b^n c^n}.

O PDA é composto por um conjunto de estados, um alfabeto de entrada, um alfabeto de pilha e uma função de transição que determina como o autômato se comporta diante de uma entrada. A função de transição leva em consideração o símbolo lido da entrada, o símbolo do topo da pilha e o próximo estado que o autômato irá assumir.

O PDA é utilizado em diversas áreas da computação, como por exemplo na análise sintática de linguagens formais, na compilação de linguagens de programação, na modelagem de sistemas de memória compartilhada, entre outras. Em geral, o PDA é aplicado em problemas que requerem alguma forma de armazenamento temporário para informações relevantes, como é o caso das pilhas.