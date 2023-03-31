# Autômato Finito Determinístico

A principal característica que diferencia um AFD de um autômato finito não determinístico (NFA) é que para cada estado e símbolo de entrada, o AFD possui **apenas uma transição possível** para um próximo estado. Isso significa que um AFD é determinístico, ou seja, dado um estado e um símbolo de entrada, sempre haverá uma única transição possível para um próximo estado.

Um AFD é uma forma eficiente de reconhecer uma [[Linguagens Regulares|linguagem regular]], pois pode ser implementado em uma tabela de transição, onde as transições entre os estados são pré-calculadas e armazenadas. Quando uma entrada é fornecida, o autômato começa no estado inicial e usa a tabela de transição para avançar para o próximo estado até que todo o input seja consumido. Se o estado final for alcançado, a entrada é aceita, caso contrário, ela é rejeitada.

## Definição matemática

Um AFD é uma 5-upla **(Q, Σ, δ, q0, F)**
- Q é um conjunto finito de estados;
- Σ é o alfabeto de cadeia de entrada;
- δ : Q x Σ → *Q (domínio imagem) | ao processar (p,a) → q (par p,a onde p pertence ao conjunto Q e a ao conjunto Σ) temos como resposta um valor q ao conjunto Q*
- q0 ∈ Q
- F ⊆ Q 

Se A é o conjunto de todas as palavras (cadeias) aceitas por M, dizemos que A é uma linguagem aceita por M, e escrevemos:
- L(M) = A
- Logo dizemos que M **reconhece** ou **aceita** uma linguagem A

![[Pasted image 20220930232233.png]]

![[Pasted image 20220930232249.png]]