# CISC vs RISC | Architecture

## #CISC - Complex Instruction Set Computer

- Há diferentes níveis de complexidades de instruções.
- **Instrução complexa**: execução de várias operações em uma única instrução.
- Sendo assim, a arquitetura CISC permite a execução de operações compelxas em um único ciclo de clcok.

> A "filosofia" dos processadores CISC é abstrair a complexidade das operações para o hardware, em vez do software.

---
**❖ EXEMPLO** 
- MULT 1:2 , 2:3 (em arquiteturas CISC)
	- O processador busca os valores 2 e 3 na memória.
	- Os valores são carregados implicitamente em dois registradores separados.
	- A MULTIPLICAÇÃO é executada.
	- E finalmente o resultado é armazenado na memória.
<p style="color: gray; margin-top: 20px; margin-bottom: 10px; margin-left:0px"> ---> São instruções compactas que permitem executar multi operações (ou seja, operações complexas) em uma única instrução</p>
---
### issues 
-  **maior utilização de silício** - à medida que se criam novas instruções, o design dos processadores se tornaram cada vez mais complexos.

## #RISC - Reduced Instruction Set Computer

### A origem dos processadores RISC
No início dos anos 80, foi feito uma pesquisa que levantou os seguintes pontos até aquele instante:

- Dentre as instruções de um processador CISC, 20% eram instruções simples, e 80% eram instruções complexas;
- Porém, ao analisar o uso nos programas, a maior parte do tempo (quase 80%), era usado instruções simples, e apenas uma pequena parcela realmente usava instruções complexas.

Essa constatação levou a repensar o design do processador, e com isso começa a se desenvolver os processadores RISC - que ao contrário do CISC, dão preferência para o uso de instruções simples ao em vez de complexas, e abstraem a complexidade a nível de software.