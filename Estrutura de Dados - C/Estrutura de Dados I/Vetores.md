# Vetores Estáticos
- Cada elemento do vetor tem um endereço de memória.
- O endereço do vetor em si, é o endereço do primeiro elemento do vetor `v[0]`.
- Os index do vetor podem ser acessados com aritmética de endereços: 
	- ``(v+1) =  &v[1]`` 
	- ``*(v+1) = *(0x12983FF)``
- +1 significa uma quantia de memória igual ao tipo do array (+1 int= 1 x 4 bytes);  
- a variável ``v`` compartilha o endereço de memória da primeira posição ``v[0]``
