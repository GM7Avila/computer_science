# Análise Assintótica

- Mostra como o tempo de execução (não é o tempo do relógio) cresce a medida que aumentamos a entada;
  
- Analise em curva do gráfico - **taxa de crescimento**.

- Constante não é relevante! A diferença vai ser a "altura no gráfico", porém a inclinação (taxa de crescimento) é a mesma;

- Entradas muito grandes n² teve maior força, por isso, TERMOS DE MENOR GRAU não importam na analise. Por isso devemos levar em consideração apenas o termo de maior grau, ou seja, a notação será apenas Θ(n^2) - termo dominante (maior grau);  
---

- Análise linear Θ(n)
```mathematica
T(n) - tempo de exec.

suponha
n=100
T(100) = 7 * 100 = 700

se dobrarmos a entrada
T(200) = 7 * 200 = 2 * 700

Concluindo: dobramos a entrada e consequentemente dobramos o tempo de execução (na mesma proporção) - então T(n) = 7 * n é o mesmo que Θ(n)

> note: a constante não é relevante! a diferença vai ser a altura no gráfico, porém a inclinação (taxa de crescimento) é a mesma
```

- Análise quadrática: Θ(n²)
```mathematica
T(n) - tempo de exec.

suponha
t(n) = 5n^2
t(2) = 5 * 2^2 = 5 * 4 = 20

se dobrarmos a entrada
t(4) = 5 * 4^2 = 5 * 16 = 80 = 4 * 20

Concluindo: se dobrarmos a entrada quadriplicamos o tempo de execução - então T(n) = 5n^2 = Θ(n^2)

> note: a constante não é relevante! a diferença vai ser a altura no gráfico, porém a inclinação (taxa de crescimento) é a mesma
```

- Termos de menor grau não importam!
```mathematica
T(n) = n^2 + 8n + 80 = Θ(n^2)

suponha
t(10) = 260

se dobrarmos a entrada
t(20) = 640

se n for mil
t(1000) = 1008080

se dobrarmos isso
t(2000) = 4016080

> note: entradas muito grandes n² teve maior força, por isso, TERMOS DE MENOR GRAU não importam na analise. Por isso devemos levar em consideração apenas o termo de maior grau, ou seja, a notação será apenas Θ(n^2) - termo dominante (maior grau);  
```

<img src="https://www.usna.edu/Users/cs/crabbe/SD311/2024/lectures/bigO/constant.gif">

- Mesmo que o n² comece melhor que n, isto vale apenas para termos de menor grau, após o ponto de interseção entre as duas retas esse cenário muda.

---

