# Aut Finito Não Determinístico (AFND ou NFA)

Ao contrário de um AFD, um NFA pode ter várias transições possíveis para um mesmo estado a partir de um mesmo símbolo de entrada, ou pode ter transições que não consomem nenhum símbolo de entrada. Isso significa que, em um NFA, para um dado estado e símbolo de entrada, há uma escolha não determinística de qual transição seguir para chegar ao próximo estado.

Para lidar com essa não determinação, um NFA pode ter um ou mais estados iniciais e finais, e a aceitação de uma entrada depende do caminho seguido pelo autômato a partir do estado inicial. Se houver algum caminho que permita ao autômato alcançar um estado final, a entrada é aceita.

Assim como o AFD, o NFA também pode ser implementado em uma tabela de transição, mas com transições vazias (ε-transições) para lidar com as múltiplas escolhas de transição. O uso de ε-transições também permite que o NFA reconheça algumas linguagens regulares que não podem ser reconhecidas por um AFD.

No entanto, a não determinação do NFA pode aumentar a complexidade do processamento em algumas situações, pois pode ser necessário explorar todas as possíveis escolhas de transição para determinar se uma entrada é aceita ou não. Por isso, é comum converter um NFA em um AFD equivalente para uma implementação mais eficiente.