# Autômato Finito Determinístico
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