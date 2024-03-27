# Camada de Rede na Internet
#ip #ipv4 
## Protocolo IPv4
- Na Internet a camada de rede trata cada pacote de forma **independente**.
	- O pacote atual não tem qualquer relação com pacotes anteriores ou posteriores.
	- Pacotes com mesma origem e destino podem passar por diferente rotas.

- **Datagrama**: unida básica de informação que passa pela internet.
- **Fragmentos**: partes de um datagrama - quebrado -que era muito grande (exemplo: fragmentos na ethernet podem ser no máximo de 1500 bytes)
	- Dados precisam caber dentro da porção de dados da rede física.
	- Pode ser que seja necessário fragmentar os dados para que eles possam ser transportados pela rede.
	- O tamanho do fragmento é determinado pelo **MTU (Maximum Transmission Unit)** - de acordo com a interface de hardware da rede.
	- O **IPv4** especifica que a **fragmentação ocorre em cada roteador** baseado na MTU da interface pela qual o datagrama IP deve passar.
		- Ex.: se o MTU de saída for menor do que o MTU de entrada de um roteador, o datagrama deve ser quebrado em fragmentos menores - isso vale para cada roteador no caminho.
	- *Quando um datagrama é quebrado em fragmentos, **cada um desses fragmentos passsam a ser um novo datagrama***

### Datagrama IPv4
<img src="https://i.ytimg.com/vi/UY_SMiKYbzg/maxresdefault.jpg">
*Datagrama*: Cabeçalho (20 bytes) + Dados

- **Versão**: 4
- **Comprimento de cabeçaho (IHL)**: tamanho do cabeçalho em palavras de 32 bits
- **Tipos de serviço**: permite que o hospedeiro informe à sub-rede o tipo de rede que deseja - são possíveis várias combinações de confiabilidade e velocidade.
- **Comprimento do datagrama**: comprimento total - tamanho máximo 65535 bytes
- **Identificação (16 bits)**: permite que o hospedeiro destino determine a qual datagrama pertence um fragmento recém chegado.

```
|-----------------------|      |----------------|  |----------------|
|    Datagrama (3)      | ===> | Frag.   (3)    |  | Frag.   (3)    |
|-----------------------|      |----------------|  |----------------|
```

- **Flags**
	- **DF** - *Don't Fragment* - o datagrama proíbe a fragmentação (caso for necessário, o roteador devolverá uma mensagem informando que não foi permitido fragmentar o datagrama pois o DF=1)
	- **MF** - *More Fragments* - é o número que indica se há mais fragmentos. Exemplo: Fragmento contendo MF = 1, significa que há mais fragmentos depois dele; Fragmento contendo MF = 0, significa que não há mais fragmentos depois dele.

```
|-----------------------|      |----------------|  |----------------|
|    Datagrama          | ===> | Frag.  MF = 1  |  | Frag.  MF = 0  |
|-----------------------|      |----------------|  |----------------|
```

- **Deslocamento de Fragmentação (13 bits)** - posição dos fragmentos - ordem correta dos fragmentos (múltiplos de 8: $16 bits - 2 (flags)$ = $13 bits$ ; $2^3 = 8$)

```
|-----------------------|      |----------------|  |----------------|
|    Datagrama          | ===> | Frag. Index 0  |  | Frag. Index 39 |
|-----------------------|      |----------------|  |----------------|
```

**❖ Ex.:** Datagrama se dividiu em 3 Fragmentos (1,2 e 3):

| Fragmento | Bytes               | ID  | Deslocamento                                                                        |
| --------- | ------------------- | --- | ----------------------------------------------------------------------------------- |
| 1         | 1480 bytes de dados | 777 | 0 (o que significa que os dados devem ser inseridos a partir do byte 0)             |
| 2         | 1480 bytes de dados | 777 | 185 (dados devem ser inseridos a partir do byte 1480 - **note que 185 x 8 = 1480**) |
| 3         | 1020 bytes de dados | 777 | 370 (dados devem ser inseridos a partir do byte 2960 - **note que 370 x 8 = 2960**) |

- **TTL (Time to Live)** - Contador usado para limitar a vida útil do pacote, decrementado a cada roteador que passa.
- **Protocolo da camada superior** - Para qual ==protocolo de transporte== o datagrama deverá ser entregue; 
	- Exemplo: 6 para TCP e 17 para UDP.
- **Cheksum** - confere o cabeçalho para determinar se ocorreram erros durante sua transmissão - limitado apenas ao cabeçalho (não verifica os dados).
- **ENDEREÇO IP ORIGEM E DESTINO** - [[04. Endereço IP|endereço IP]] de quem envia e de quem recebe.
- **Dados** - carrega os dados da [[a3.2 - Camada de Transporte|camada de transporte]]
### Fragmentação e Reconstrução do Datagrama IPv4
- Nem todos os protocolos de camada de enlace podem transportar pacotes do mesmo tamanho.
	- MTU do IEEE 802.3 (Ethernet): 1500 bytes
	- MTU do IEEE 802.11 (Wi-fi): 7981 bytes
	- MTU PPPoE: 1492 bytes
	- MTU HDLC: 1564 bytes
- O roteador precisa interligar enlaces que podem possuir diferentes MTU, então pode ser necessário **fragmentar** o datagrama.
- O datagrama precisa ser reconstruído antes de ser entregue ao [[12. Sockets|protocolo de transporte]] no destino. 

> OBS: Os roteadores não são capaz de remontar os fragmentos em datagramas, eless apenas realizam a fragmentação.

### Endereçamento IPv4
Um **hospedeiro** (qualquer entidade na rede que tenha endereço IP e possa trocar informação na rede) normalmente possui apenas uma interface com a rede de computadores.

- Quando o protocolo IP de um hospedeiro quer enviar um datagrama, ele o faz por meio dessa interface.
- A **interface** de rede é a **fronteira entre o hospedeiro e a rede física**.
- O **endereço IP** está associado a cada interface de rede (cada interface tem seu IP) - e não com o hospedeiro que contém aquela interface.

- Cada endereço IP tem comprimento de 32 bits (4 bytes).
	- `00000000 00000000 00000000 00000000`
	- `11111111 11111111 11111111 11111111`
	- Há um total de 2^32 endereços possíveis (aprox. 4 bilhões de endereços)
- Cada **interface** em cada **hospedeiro** da Internet tem que ter um ==endereço IP globalmente exclusivo==.

```
   Sub-rede 1                 Sub-rede 2

[A]   [B]   [C]             [D]   [E]   [F]
 |     |     |    Roteador   |     |     |
 |-----|-----|------(X)------|-----|-----|
.                  /   \
.        192.168.0.1   192.168.1.1

IP A - 192.168.0.2
IP B - 192.168.0.3
IP C - 192.168.0.4

IP D - 192.168.1.2
IP E - 192.168.1.3
IP F - 192.168.1.4
```

- Usamos `/24` (conhecida como **máscara de sub-rede**) ao final do IP para definir que os 24 bits mais à esquerda definem o endereço da sub-rede.
	- **192.168.0**.0==/24== `<-- uma subrede`
	- **192.168.1**.0==/24== `<-- outra subrede`
	- 192.168.1.**0**/24 `<-- 0 se refere a um host dentro dessa subrede`
#### Roteamento Interdomínio sem Classes (CIDR)
- O Classless Interdomain Routing é a estratégia de endereços da internet.
- O CIDR generaliza a noção de endereçamento de sub-rede.

O endereço IP de 32 bits é divido então em 2 partes: **A.B.C.D/X**
- `Endereço / Quantidade de Bits`
- `A.B.C.D / X`
- **Prefixo** (`X`) - determina qual parte do endereço IP identifica a rede.

> Os endereços IP de hospedeiros dentro de uma organização compartilham o prefixo comum. Isso reduz consideravelmente o tamanho da tabela de repasse nos roteadores.
> 
> Ex.: roteamento entre países - basta levar em conta o prefixo do IP para chegar até o país desejado - menos trabalho para os roteadores.

- Dentro da ==sub-rede== os **últimos (32-X) bits** serão todos diferentes (como no esquema anterior); 
- Ex.: IP da Rede de uma Organização A.B.C.D/24
	- Os primeiros 24 bits (marcados por 1 no exemplo abaixo) do endereço especificam o *prefixo* da rede da organização e são comuns aos endereços IP de todos os hospedeiros da organização.
		-  `11111111 11111111 11111111 00000000`
	- A estrutura interna poderia ser tal que esses 8 bits mais à direita, seriam usados para criar uma **sub-rede** dentro da organização.
		- `00000000 00000000 00000000 11111111`

- As sub-redes não estão restritas a segmentos ethernet queconectam múltiplos hospedeiros a uma interface de roteador. Os pontos que ligam os roteadores também são considerado subredes (*qualquer ligação na internet é uma subrede*).

<img src="https://d2vlcm61l7u1fs.cloudfront.net/media%2F834%2F834dbbf4-7d51-4ec2-9927-ea312b21536f%2Fphp2n1C6V.png" width="45%">

- Sub-redes IP que estão presentes na imagem:
	- **Subnet A**: ==233.1.1==.0==/24== 
	- **Subnet B**: ==233.1.2==.0==/24==
	- **Subnet C**: ==233.1.3==.0==/24==
	- **Subnet D**: ==233.1.9==.0==/24==
	- **Subnet E**: ==233.1.8==.0==/24==
	- **Subnet F:** ==233.1.7==.0==/24==

> Antigamente com as redes classe A, B e C se desperdiçava muitos endereços, pois se utilizavam bytes inteiros para definir subrede.

**Algumas observações...**
- Dois endereços especiais de uma sub-rede não podem ser utilizados
	- O primeiro e o último endereço da faixa de endereços da organização
	- O primeiro é reservado para o **endereço de rede** 
		- Identifica a rede como um todo
		- Todos os bits que não fazem parte do prefixo de rede (ou seja, campos de host) recebem o valor 0
	- O último endereço da faixa é utilizado como **endereço de difusão (broadcast)**;
		- Quando colocado como destino de uma mensagem, esta deve ser entregue a todos os hospedeiros da sub-rede.
		- Roteadores não repassam mensagens de difusão.
			- A difusão fica limitada ao segmento de rede limitado pelo roteador.
		- Todos os bits que não fazem parte do prefixo de rede recebem o valor 1.
- O endereço `255.255.255.255` é um **endereço especial de difusão**
	- A mensagem é entregue a todos os demais hospedeiros que estão na mesma sub-rede do hospedeiro que enviou a mensagem.
- A distribuição de endereços IP na internet é controlada pela **ICANN** (Internet Corporation Assigned Names and Numbers)
### Sub-redes
- O mundo exterior (internet) enxerga a rede de uma organização como uma rede única - não importa informações sobre sua estrutura interna - um único ponto de entrada.
- O mecanismo que permite tal divisão é conhecido como **máscara de rede** (ou máscara de sub-rede)
- Uma máscara de rede possui 32 bits, dividido em 4 campos com 8 bits cada, seguindo o padrão: 
	- bits de rede - bit 1
	- bits de sub-rede - bit 1
	- bits do hospedeiro - bit 0

No geral existem categorias de máscara (padrão), dividias em classes:

| Classe | Decimal       | Notação CIDR | Hexadecimal |
| ------ | ------------- | ------------ | ----------- |
| A      | 255.0.0.0     | /8           | FF:00:00:00 |
| B      | 255.255.0.0   | /16          | FF:FF:00:00 |
| C      | 255.255.255.0 | /24          | FF:FF:FF:00 |
- **Classe A**: 8 bits representam a rede e os 24 restantes os hospedeiros.
- **Classe B**: 16 bits representam a rede e 16 os hospedeiros.
- **Classe C**: 24 bits representam a rede e 8 bits os hospedeiros.
---
**❖ EXEMPLO** 
- O endereço IP 168.23.0.0 com a máscara de rede padrão: 255.255.0.0

```
|  168  |  23  |  0  |  0  |
|----Rede------|---Host----|

| 255  |  255  |  0  |  0  |
|----Rede------|---Host----|
```

- A organização pode querer trabalhar com várias sub-redes internas, para isso ela cria uma máscara de sub-rede com 8 bits:
```
| 255  |  255  |  0  |  0  | <--Mundo externo
|----Rede------|---Host----|


| 255 | 255 | 255 |  0  |
|----Rede---|  |     |
.              |
Sub-rede_______|     | 
Hospedeiro___________|  
```
---
- Ao dividir uma rede em sub-redes, deve-se alocar os bits para a sub-rede a partir dos bits de mais alta ordem (bits mais à esquerda) do campo do hospedeiro.
- Quanto mais bits à esquerda, maior a capacidade de sub-redes, e menor a capacidade de hospedeiros nessa sub-rede.
- Quanto menos bits à direita, menor a capacidade de sub-redes, e maior capacidade de hospedeiros nessa sub-rede.

- A tabela mostra os valores usados no campo do hospedeiro quando se divide uma tabela em sub-redes.

| **128** | **64** | **32** | **16** | **8** | **4** | **2** | **1** | -----   |
| ------- | ------ | ------ | ------ | ----- | ----- | ----- | ----- | ------- |
| 0       | 0      | 0      | 0      | 0     | 0     | 0     | 0     | **0**   |
| 1       | 0      | 0      | 0      | 0     | 0     | 0     | 0     | **128** |
| 1       | 1      | 0      | 0      | 0     | 0     | 0     | 0     | **192** |
| 1       | 1      | 1      | 0      | 0     | 0     | 0     | 0     | **224** |
| 1       | 1      | 1      | 1      | 0     | 0     | 0     | 0     | **240** |
| 1       | 1      | 1      | 1      | 1     | 0     | 0     | 0     | **248** |
| 1       | 1      | 1      | 1      | 1     | 1     | 0     | 0     | **252** |
| 1       | 1      | 1      | 1      | 1     | 1     | 1     | 0     | **254** |
| 1       | 1      | 1      | 1      | 1     | 1     | 1     | 1     | **255** |
- De uma forma mais geral, essas são todas as máscaras que podem existir:
	- Não existe máscara menor que /8 (e na prática não existem máscaras para redes maiores que /30: 31 e 32 são usadas para mecanismos de seguraça para identificar um host individual).
	
![[Pasted image 20240319193442.png]]

- Existem várias **razões** para se **dividir uma rede em sub-redes**:
	- Isolar o tráfego de uma sub-rede, reduzindo assim o tráfego total da rede.
	- Proteger / limitar o acesso a uma sub-rede.
	- Associação de uma sub-rede com um departamento ou espaço geográfico específico.

---
**❖ EXEMPLO - PROBLEMA**
- Um adm recebeu para uso a rede 172.16.22.0/24, e que precisa utilizar internamente 4 sub-redes de igual tamanho.

1. Qual a máscara de sub-rede ve ser utilizada?
   - /24 = **255.255.255**.==0== = **11111111.11111111.11111111**.==00000000==
	   - Neste caso, ==0== é o único campo onde podemos manipular para a criação de sub-redes. Todo o restante é (255.255.255) é informação já usada pelo endereço IP (estrutura fora da organização).
	   - Para isso vamos "roubar" alguns bits do campo do host, e leva-los ao campo de rede (==assim aumentamos a capacidade de sub-redes porém diminuímos a capacidade de hospedeiros para cada uma das redes criadas==)
	   - Então, se é desejado a criação de 4 subredes, precisamos de 2 bits, assim 2^2 = 4;
	   - Máscara de sub-rede: **11111111.11111111.11111111**.**11**==000000==
		   - Em notação decimal com pontos: 255.255.255.192 ou seja, **máscara /26**
	
2. Quantas interfaces podem ser configuradas em cada sub-rede?
	- Nova máscara: **11111111.11111111.11111111**.**11**==000000==
	- Temos 6 bits (0 à direita) para identificar os hosts, logo 2^6 = 64 endereços
	- ==O primeiro e último endereços **são reservados para endereço de rede e difusão**==, então sobram **62 interfaces em cada sub-rede** (incluindo a interface do roteador)
	  
3. Qual a faixa de endereços de cada sub-rede?
	- 172.16.22.0 = 10101100.00010000.00010110.**00**000000
	- Os primeiros 16 bits (==10101100.00010000.00010110==.**00**000000) representam a subrede original.
	- O que foi retirado do campo de host foram os dois bits (**00**) do último byte, usados para representar as sub-redes, ==isso significa que eles vão de 00 até 01 (00,01,10,11 - 4 subredes)==
	
	- Então a **primeira** sub-rede (**00**) será:
	- 10101100.00010000.00010110.**00**==000000== a 10101100.00010000.00010110.**00**==111111==
	- 172.16.22.0 a 172.16.22.63
	- 0 ao 63 (64 endereços)
	  
	- A **segunda** sub-rede (**01**) será:
	- 10101100.00010000.00010110.**01**==000000== a 10101100.00010000.00010110.**01**==111111==
	- 172.16.22.64 a 172.16.22.12
	  
	 - A **terceira** sub-rede (**10**) será:
	- 10101100.00010000.00010110.**10**==000000== a 10101100.00010000.00010110.**10**==111111==
	- 172.18.22.128 a 172.16.22.191
	
	- A **quarta** sub-rede (**11**) será
	- 10101100.00010000.00010110.**11**==000000== a 10101100.00010000.00010110.**11**==111111==
	- 172.16.22.192 a 172.16.22.255
	

> [!NOTE] Um método mais fácil
O endereço IP é 172.16.22.0, isso significa que os endereços vão de 172.16.22.0 até 172.16.22.255 = 256 valores de intervalo. Se queremos saber as 4 sub-redes, tendo em vista que dividimos por 4, então os intervalos para cada sub-rede será: 256 intervalos / 4 sub-redes = cada sub-rede terá 64 endereços (0~63; 64~127;128~191;192~255);

4. Quais destes endereços são utilizáveis?
	- Endereço da rede é sempre o primeiro endereço
	- O endereço de difusão é sempre o último endereço
	- Então é só desconsirá-los para cada intervalo, exemplo:
		- 172.16.22.0 ~ 172.16.22.63: 172.16.22.1 a 172.17.22.62

---
### Network Address Translation (NAT)
- Com o NAT uma organização pode **utilizar internamente uma faixa de endereços que não é válida na internet** (solucionar o problema dos ipv4 escassos).
	- Quando for necessário acessar a internet, o NAT traduz o endereço interno (não válido) para um endereço válido.

- **Ideia básica**:
	- Atribuir a cada organização uma pequena quantidade de endereços IP para tráfego na Internet.
	- Dentro da organização todo computador obtém um endereço IP exclusivo (também conhecido como **IP privado**).
```        
 <--válido-- Roteador <-invalido-    [A] [B] [C]
(INTERNET)-----(X)--------------------|---|---|
.               | \                   \   |   /
mapeamento<-----|  NAT                IP privado
```

- Três *intervalos* de endereços IP foram declarados como **endereços privativos** (reservados).
	- As organizações podem utilizá-los internamente da maneira que quiserem.
	- Nenhum pacote contendo esses endereços pode aparecer na própria Internet.
	- Os três intervalos reservados são dados na tabela (endereços usados internamente)

| Faixa                                 | Máscara       | Classe | Faixa reservada      |
| ------------------------------------- | ------------- | ------ | -------------------- |
| **10**.0.0.0 a **10**.255.255.255     | 255.0.0.0     | A      | -                    |
| **172.16**.0.0 a **172.31**.255.255   | 255.255.0.0   | B      | 32 redes reservadas  |
| **192.168**.0.0 a **192.168**.255.255 | 255.255.255.0 | C      | 256 redes reservadas |
- Dentro da organização toda máquina tem um endereço exclusivo que não é válido pela Internet.
- Quando um pacote deixa a organização, ele passa por uma caixa NAT normalmente um roteador:
	- Converte o endereço de origem no endereço IP válido da organização.
	- O pacote com o novo endereço poderá transitar sem problemas na Internet.
- Quando a resposta do pacote voltar
	- Voltá para o endereço IP válido da organização
	- A caixa NAT **mantém uma tabela** na qual poderá ==mapear que máquina enviou qual requisição== para a internet (e seus respectivos endereços).

```
.                                    [A] [B] [C]
(INTERNET)-----(X)--------------------|---|---|
.              / \                    \   |   /
   200.20.30.40   192.168.0.1         IP privado  
```

| IP-interno  | Porta-interna | IP-map       | Porta-map | IP-ext        | Porta-ext |
| ----------- | ------------- | ------------ | --------- | ------------- | --------- |
| 192.168.0.1 | 5798          | 200.20.30.40 | 3297      | 210.150.45.78 | 80        |
 
- Interação do cliente `192.168.0.4` com servidor web `210.150.45.78` (na internet)
	- Pacote de Envio
		- IP Origem: `192.168.0.1`
		- Porta Origem: `5787`
		- IP Destino: `210.150.45.78`
		- Porta Destino: `80`
	
	- Quando passar pelo Roteador, o pacote acima não poderá entrar na internet com o endereço de origem (tanto o IP quanto a porta).
	- Então o NAT mapeia um endereço apropriado para o pacote de acordo com os requisitos da internet - **registra na tabela.**
	- Pacote de saída pelo NAT
		- IP Origem: `200.20.30.40`   **<<------**
		- Porta: `3297`                        **<<------**
		- IP Destino: `210.150.45.78`
		- Porta Destino: `80`
		
	- A resposta chega normalmente ao NAT que fará o mapeamento para o endereço interno.

### Rede de Datagramas
- Em uma rede de datagramas, toda vez que um sistema final quer enviar um pacote le coloca o endereço destino no pacote e o envia pela rede.
- Ao ser transmitido o pacote passa por uma série de roteadores - cada um usa o endereço destino para repassar o datagrama.
	- O roteador transmite o pacote para aquela interface de enlace de saída.

- Se todos os endereços (datagrama IP) possuem 32 bits
	- Uma execução de força bruta da tabela de repasse teria um registro para cada endereço de destino possível
	- Há *mais de quatro bilhões de endereços possíveis* - **isso é fora de questão!**

- Agora suponha que o roteador tenha 4 enalces numerados de 0 a 3, e que os pacotes devem ser repassados para as interfaces de enlace como mostrado a seguir:

![[Pasted image 20240320114936.png]]

- Resumindo a tabela (considerando apenas o início - pois todo o restante dos bits mudam)

| Faixa de endereços de destino | Interface de enlace |
| ----------------------------- | ------------------- |
| 11001000 00010111 00010       | 0                   |
| 11001000 00010111 00011000    | 1                   |
| 11001000 00010111 00011       | 2                   |
| Rota padrão                   | 3                   |
 
- Sendo assim a tabela poderia ter apenas 4 registros.
- Quando há várias concordâncias de prefixos, o roteador usa a **regra da concordância do prefixo mais longo** (interface de enlace 2 e 3 são iguais até certo ponto, então vale o prefixo mais longo).

> Embora o NAT funcione, ele tem um problema: ele viola os conceitos de protocolos de rede quando usa o número de porta - uma informação da camada de transporte. Por isso desenvolveu-se o IPv6.
> 
> Um protocolo não deve necessitar usar informações de outra camada para funcionar/trabalhar (no caso o NAT tem essa dependência - ao usar endereços de porta).

---
## Protocolo IPv6
- Número de computadores aumentou - espaço de 32 bits escaço
	- *Em fev. de 2011 o IANA alocou o último bloco de endereços IPv4*
- NAT viola o princípio de camadas
- Avanço das tecnologias e recursos de redes (como exemplo o surgimento da transmissão de áudio e vídeo em tempo real)

> A adoção do IPV6 tem tido alguns atrasos pelos seguintes motivos:
> 
> O IPv4 adotou algumas medidas como: endereçamento sem classe, uso de DHCP para alocação dinâmica de endereços e o NAT; Essas medidas tem sustentado o uso um pouco mais prolongado do IPv4.
> 
> Por outro lado, a rápida expansão do uso da Internet e os novos serviço, como o IP móvel e telefonia móvel compatível com IP, podem exigir a rápida substituição do IPv4 pelo IPv6.

### Vantagen em relação ao IPv4
- Espaço de endereço maior (128 bits de comprimento contra 32 bits do IPv4)
- Introdução de um novo tipo de endereço para qualquer membro do grupo (**anycast**)
	- Permite que um datagrama seja entregue a qualquer hospedeiro do grupo.
		- Basta entregar para um hospedeiro de um grupo.
	- **IPv4**: unicast, multicast.
	- **IPv6**: unicast, multicast, anycast.
- Formato de cabeçalho melhor
	- Opções são separadas do cabeçalho de base e *inseridass somente quando necessário*
- Novas opções
	- Oferece funcionalidades adicionais
- Margem para amplicação
	- É projetado para permitir a apliação do protocolo, caso seja exigido por *novas tecnologias ou aplicações*
- Suporte para alocação de recursos
- Suporte para mais segurança
	- Operação de criptografia e autenticação proporcionam a confidencialidade e integridade ao pacote.
### Datagrama 
- IPv6 - datagrama minimalista/simplificado (+velocidade e modular)
<img src="https://electronicspost.com/wp-content/uploads/2016/05/4.24.png">

- **Versão**: Versão do protocolo: 6
- **Classe de Tráfego**: Também recebe o nome de ==serviços diferenciados==; Usado para distinguir a clase de serviço para pacotes com diferentes requiito de entrega em tempo real - exemplo: informar que é um pacote de vídeo (para um tratamento melhor).
- **Rótulo de Fluxo**: Permitem que origem de destino marquem grupo de pacotes que têm os mesmos requisitos que devem ser tratados da mesma maneira pela rede.
	- O fluxo pode ser configurado com antecedência e ter um identificador atribuído a ele.
		- Quando possui valor diferente de zero os roteadores podem verificar que tipo de tratamento especial ele exige.
	- É uma tentativa de ter a flexibilidade de uma rede de datagramas com as garantias de uma rede de ==circuitos virtuais==.
- **Comprimento da carga útil**: Quantidade de bytes que seguem o cabeçalho (apenas o comprimento do ==Dados==). Em relação ao IPv4 os bytes do cabeçalho deixaram de ser contados como parte do tamanho.
- **Próximo cabeçalho**: Torna o IPv6 mais minimalista, com a possiblidade de haver outros cabeçalho de extensão se for preciso (opcional). 
	- Informa quais dos cabeçalhos de extensão seguem esse cabeçalho (se houver algum)
	- Se esse cabeçalho for o último, o campo revelará para qual tratador de protocolo de transporte (TCP, UDP, ...) o pacote deverá ser enviado.
- **Limite de saltos**: TTL da IPv4 - é decrementado a cada salto. Quando o contador de saltos do pacote chegar a zero, ele é ==descartado==.
- **Endereço de origem e destino**: 128 bit cada (16 bytes cada).

