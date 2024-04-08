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
- **ENDEREÇO IP ORIGEM E DESTINO** - [[04. Endereço IPv4|endereço IP]] de quem envia e de quem recebe.
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
- Rotulação de fluxo e prioridade
	- Permite rotular pacotes que pertecem a fluxos particulares para os quais o remetente requisita tratamento especial - ex.: serviço de tempo real.
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

---

- Alguns campos que aparecem no datagrama IPv4 não estão presentes no datagrama IPv6:
	- **Fragmentação/Remontagem**
		- O IPv6 **não permite** fragmentação e remontagem em *roteadores intermediários*.
		- Essas operações podem ser realizadas ==apenas pela origeme pelo destino.==
		- Se um datagrama IPv6 recebido por um roteador for muito grande, o roteador **descartará** o datagrama e devolverá ao remetente uma mensagem de erro ICMP "Pacote muito grande".
		- Os pacotes devem sair da origem com o tamanho ideal para trafegar por todos os enlaces - isso diminui bastante o processamento nos roteadores.
	- **Chekcsum do cabeçalho**
		- Essta tarefa hoje em dia já é realizada por protocolos de transporte e de enlace de dados.
		- Os projetistas estavam mais preocupados com o processamento rápido de pacotes.
			- Antes, qualquer alteração do campo TTL (incrementado a cada roteador) provocava uma necessidade de uma nova soma de verificação - isso tornava o trafego lento.

---

### Endereço IPv6
- Posicionado **diretamente na internet**
- Endereços de 128 bits (16 bytes
- Representado por 8 blocos de 16 bits cada (representados em hexadecimal), separados por caractere dois pontos.
	- Ex.: `2022:0000:0000:0000:0003:4567:00FA:B100`

- São possíveis duas otimizações:
	1. Os zeros à esquerda dentro de um grupo podem ser omitidos
		- 0123 pode ser escrito como 123.
	2. Um ou mais grupos consecutivos de 16 bits zero podem ser substituídos por um par de sinais de dois-pontos (uma única vez);
		- O endereço anterior pode ser escrito como:
		- 2022==::3==:4567:FA:B100

- Endereços IPv4 podem ser escritos com um par de sinais de dois pontos em um número decimal tradicional:
	- `::192.31.20.46`

#### Categorias de Endereço
1. **Unicast**
	- Define um único hospedeiro.
	- O datagrama enviado a um endereço unicast deve ser encaminhado para esse hospedeiro específico.
2. **Anycast**
	- Define um grupo de hospedeiros com endereços que possuem o mesmo prefixo.
	- Um datagrama enviado a um endereço anycast deve ser **encaminhado para exatamente um dos membros do grupo**
3. **Multicast**
	- Define um grupo de computadores.
	- Um datagrama enviado deve ser **encaminhado para CADA membro do grupo**.

#### Espaços de endereço
- O espaço de endereços tem propósitos diferentes.
- A primeira parte do é chamada de **==prefixo de tipo==**
	- Prefixo de comprimento variável que define o **propósito** do endereço.

```
|<-------------------128 bits--------------------->|
|<--Variável->|<-------------Variável------------->|

+-------------+------------------------------------+
| PREFIXO DE  |          RESTANTE DO               |
|    TIPO     |            ENDEREÇO                |
+-------------+------------------------------------+
```

#### Endereços Unicast baseados no provedor
- É utilizado por um hospedeiro normal como endereço unicast.

```
|<-------------------128 bits----------------------------------------->|
|<--8bits-->|
|--3--|--5--|
.           .
+-----+-----+----------+-----------+-----------+---------------------+
|     |     | id de    | id de     | id de     |  identificador de   |
|  *  |  *  | provedor | assinante | sub-rede  |        node         |
+--|--+--|--+----------+-----------+-----------+---------------------+
   |     |
  010    |
(tipo)   |
         |
     INTERNIC 11000
     RIPNIC   01000
     APNIC    10100   
    (id de registro) 
```

- Identificador de tipo
	- Campo de 3 bits que define o endereço como sendo baseado no provedor.
- Identificador de Registro
	- Campo de 5 bits que indica o órgão que registrou o endereço
	- Atualmente não está mais em uso e seu conteúdo é preenchido com zeros.
- Identificador de provedor
	- Campo de comprimento variável que identifica o provedor de acesso à internet.
- Identificador de assinante
	- Identifica a orgnaização que se inscreve por meio do provedor.
- Identificador de sub-rede
	- Define uma rede específica sob o domínio do assinante.
- Identificador de node
	- Define a identidade do nó conectado a uma sub-rede
	- Um comprimento de 48 bits é recomendado para esse campo para torná-lo compatível com o endereço físico de 48 bis usado pelo [[IEEE 802]].

#### Endereços Reservados
- **==PREFIXO RESERVADO==** `00000000`
	- **Endereço não especificado**
		- O endereço inteiro consiste em zeros.
		- Este endereço é usado quando um host não sabe seu prórpio endereço e envia uma consulta para descobri-lo.
		- Esse endereço (não especificado) não pode ser usado como endereço de destino.
	- **Endereço de retorno** (local)
		- Endereço usado por um host para testar ele mesmo, sem entrar na rede.
		- Consiste no prfeixo `00000000`, seguido de 119 bits zero e um bit 1.
	- **Endereços IPv4** 
		- Dois formados foram definidos para incorporar endereços IPv4 em endereços IPv6:
		- **Endereço compatível**
			- Endereço de 96 bits zero seguidos de 32 bits de endereço IPv4.
			- Utilizado quando um hospedeiro usando IPv6 quer enviar uma mensagem a outro hospedeiro usando IPv6 mas o datagrama passa por uma região usando IPv4.
		- **Endereço mapeado**
			- Compreende 80 bits zero, seguidos de 16 bits um, seguidos do endereço IPv4 de 32 bits.
			- Usado quando um hospedeiro que migrou para o IPv6 quer enviar um pacote para um hospedeiro que ainda está usando IPv4 (o roteador faz compatibilização).

```
ENDEREÇO COMPATÍVEL

+-----------+---------------------------------------+----------------+
|  8 bits   |              88 bits                  |   32 bits      |
| 00000000  |         Todos os valores 0            |   End. IPv4    |
+-----------+---------------------------------------+----------------+


Exemplo de Transformação de Endereço

+---------------+                                       +---------------+
|     IPv6      |                                       |     IPv4      |
| 0::020D:110E  |<------------------------------------->|   2.13.17.14  |
+---------------+                                       +---------------+
```

```
ENDEREÇO MAPEADO

+-----------+--------------------+---------------------+-----------------+
|  8 bits   |     72 bits        |  16 bits            |    32 bits      |
| 00000000  | Todos os valores 0 |  Todos os valores 1 |    End. IPv4    |
+-----------+--------------------+---------------------+-----------------+
```

#### Endereços Locais
- ==**PREFIXO RESERVADO**== `1111 1110`
	- **Endereço local de link** - endereço de autoconfiguração
		- Usados se uma rede local utilizar os protocolos Internet, mas não estiver conectado à internet.
		- Usa o prefixo `1111 1110 10`
		- O endereço local de link é usado em uma ==rede isolada== e não tem efeito global.
		- Ninguém fora de uma rede isolada pode enviar uma mensagem aos hospedeiros ligados a uma rede que usam esses endereços.
	- **Endereço local de site** - 
		- Usados se um site com várias redes que usam os protocolos da internet, mas não está conectado à internet.
		- Usa o prfeixo `1111 1110 11`
		- Ninguém fora de uma rede isolada pode enviar mensagens aos hospedeiros ligados a uma rede que usam esses endereços.

```
ENDEREÇO LOCAL DE LINK

+------------+---------------------------------------+----------------+
|  8 bits    |              70 bits                  |   48 bits      |
| 1111111010 |         Todos os valores 0            |   End. do Nó   |
+------------+---------------------------------------+----------------+
```

```
ENDEREÇO LOCAL DE SITE

+------------+--------------------+---------------+-------------------+
|  8 bits    |      38 bits       |   32 bits     |      48 bits      |
| 1111111011 | Todos os valores 0 | End. sub-rede |    End. do Nó     |
+------------+--------------------+---------------+-------------------+
```

#### Endereços Multicast
- Usados para definir um grupo de hospedeiros, em vez de apenas um.
- Todos usam o prefixo `11111111` no primeiro campo.
- O segundo campo é um flag que define o endereço de grupo como:
	- **Permanente** - Definido pelas autoridades da Internet e pode ser acessado o tempo todo.
	- **Transitório** - Usado apenas temporariamente - 
		- ex.: uma radio que transmitirá para a internet usa flag para endereço temporario de multicast - quem desejar, se associa a esse grupo multicast e recebe uma cópia da programação da rádio.
- O terceiro campo define o escopo do endereço de grupo.

---
### Protocolo de Controle da Internet
- O IP precisa de protocolos de controle para operar na Internet.
	- Ele é responsável por manter unida toda a internet, porém ele não consegue realizar sozinho todo o trabalho necessário.
	- Para auxiá-lo foram desenvolvidos protocolos auxiliares na camada de rede, são os **protocolos de controle**.

> *Relembrando*... A camada de Rede não envia informação diretamente pela rede física. A rede física não conhece a camada de rede - nada da camada de rede tem significado nenhum para a camada de enlace e transmissão. Porém o protocolo IP é fundamental para a transmissão de pacotes na internet, então precisamos ter um mapeamento do endereço da camada de rede com o endereço da camada de enlace. 
> 
> - Endereço da camada de rede: utilizado pela internet
> - Endereço da camada de enlace: utilizado pela placa de rede

#### ARP (Addres Resolution Protocol)

> O Protocolo mapeia IP (rede) e MAC (enlace).

- Na internet cada hospedeiro possui um ou mais endereços IP, porém eles *não podem ser usados diretamente* para o envio de informações pela rede física.
  
- O *adaptador de rede* trabalha com **endereços físicos** da camada de *enlace*.
	- ==A camada de Enlace **não reconhece** endereços IP.==
	- O ARP providencia a entrega **local**.

- É necessário encontrar uma forma de *mapear* os endereços de rede nos endereços de enlace.

- Para descobrir o endereço de enlace do destinatário é enviado um pacote por difusão perguntando: "**A quem pertence o endereço IP A.B.C.D?"
- O hospedeiro destinatário responderá informando seu *endereço de enlace*.
- O protocolo responsável por essa comunicação é o ARP (Address Resolution Protocol - RFC 826).

##### EXEMPLO 1 - MESMA SUB-REDE
<img src="https://www.researchgate.net/publication/321766678/figure/fig1/AS:571073150320640@1513165857362/Address-Resolution-Protocol-ARP-attack.png">

- A máquina envia um ARP request informando seu endereço IP e endereço MAC origem, assim como o endereço IP com quem ela deseja se comunicar - por broadcast.
- Os dispositivos que recebe e não possui o endereço IP solicitado pelo request apenas descarta a informação.
- O dispositivo que recebe a informação e possui o endereço IP e MAC do emissor, envia uma mensagem de reply, informando seu endereço MAC.
- O emissor então recebe a resposta, e usa as informações para meapar endereço MAC e IP do dispositivo que recebeu a solicitação.

- **Otimizações**:
	- Depois que uma máquina executa o ARP, ela armazena o resultado em **cache**.
		- Evita a necessidade de novos mapeamentos se houverem vários pacotes a serem trocados.
	- Em muitos casos, o destinatário precisará enviar uma resposta.
		- Um novo mapeamento pode ser evitado fazendo com o que o destinatário inclua em sua *tabela* o mapeamento entre os endereços de rede e de enlace do transmissor.

- **Observação**
	- Se um hospedeiro precisar enviar dados para outro hospedeiro que se encontra em uma **sub-rede diferente**, este sistema de entrega direta não funcionará.
	- Nesse caso, o hospedeiro transmissor precisará enviar seus dados para um [[Roteadores|roteador]], que deverá providenciar o encaminhamento do datagrama para a sub-rede do destinatário - com isso pode ser necessário que o protocolo ARP seja utilizado mais de uma vez ao longo do caminho.

##### EXEMPLO 2 - DIFERENTES SUB-REDES
- Através da **máscara de sub-rede** o protocolo consegue verificar se dois hospedeiros estão na mesma sub-rede - se for o caso, o envio pode acontecer diretamente, como no último exemplo.
- Entretanto, no caso de *sub-redes diferentes* um ou mais roteadores estarão no meio do caminho - intermediador.

- Supondo os seguintes dispositivos e seus endereços IP e MAC:
```
192.168.0.1 - 00:fa:b1:cc:23:41      10.0.0.1 - 00:fa:b1:cc:23:42
.                       \             /
.                        \           /
[PC-1]                    \         /                             [PC-2]
  |                         Roteador                                 |
  +---------------------------(X)------------------------------------+

192.168.0.12                                                    10.0.0.51
00:13:45:98:da:a2                                       00:12:54:fd:ab:45
```

- Se o PC-1 deseja se comunicar com o PC-2, que está em outra sub-rede ele precisará enviar a informação pelo Roteador.
- A camada de enlace deve se preocupar sempre com o próximo hospedeiro na rede - o roteador. Então os dados de enlace de destino são dados do próprio roteador.
- Já a camda de rede diz quem é o próprio destino final em toda a rede (no caso o PC-2) - cada camada trabalha no seu nível.
- Isso é encapsulado: `{ Enlace [ Rede ( Dados IP ) ] }
  
```
Endereço IP:  192.168.0.12             PC-1           (IP)
Máscara:      255.255.255.0            Sub-rede
Roteador:     192.168.0.1              Roteador       (IP)
Servidor DNS: 8.8.8.8                  DNS

+------------------------------[ENLACE]----------------------------------+
|                                                                        |
|  From: 00:13:45:98:da:a2                PC-1           (MAC)           |
|  To: 00:fa:b1:cc:23:41                  Roteador       (MAC)           |
|                                                                        |
+-------------------------------[REDE]-----------------------------------+
|                                                                        |
|  From: 192.168.0.13                     PC-1           (IP)            |
|  To: 10.0.0.51                          PC-2           (IP)            |
|                                                                        |
+------------------------------------------------------------------------+
```

- O roteador consultará quem possui o endereço IP de destino (segundo a informação recebida pelo PC-1): "Quem possui o endereço IP 10.0.0.51?"
- O PC-2 responde, enviando seu endereço MAC.

```
+------------------------------[ENLACE]----------------------------------+
|                                                                        |
|  From: 00:fa:b1:cc:23:42                Roteador       (MAC)           |
|  To: 00:12:54:fd:ab:45                  PC-2           (MAC)           |
|                                                                        |
+-------------------------------[REDE]-----------------------------------+
|                                                                        |
|  From: 192.168.0.13                     PC-1           (IP)            |
|  To: 10.0.0.51                          PC-2           (IP)            |
|                                                                        |
+------------------------------------------------------------------------+
```

#### DHCP (Dynamic Host Configuration Protocol)
- Protocolo **orientado a estado**.

> Servidores DHCP fornecem endereços IP aos hospedeiros da rede.

- Para ter acesso à Internet um hospedeiro precisa ser configurado com alguns parâmetros mínimos:
	- Endereço IP
	- Máscara de sub-rede
	- Endereço do gateway (roteador)
	- Endereço do servidor DNS

- Essas informações podem ser configuradas manualmente pelos administradores da rede, ou podem ser **obtidas automaticamente** - responsabilidade do DHCP (RFC 2131).

- O administrador do sistema configura um servidor fornecendo um grupo de endereços IP.
	- Sempre que um novo computador se conceta à rede, entra em contato com o servidor e solicita um endereço.
	- O servidor opta por um dos endereços especificados pelo administrador e aloca tal endereço para o computador.

- De modo geral o DHCP permite três tipos de atribuição de endereços:
	- **Configuração manual** 
		- O administrador especifica o endereço que cada hospedeiro receberá quando se concetar à rede.
	- **Configuração automática**
		- O administrador permite que um ==Servidor DHCP== atribua um endereço quando um hospedeiro se conectar pela primeira vez à rede.
		- A partir desse momento este endereço estará reservado para este hospedeiro para sempre.
	- **Configuração dinâmica** 
		- O servidor =="empresta"== um endereço a um hospedeiro, por *tempo limitado* - determinado pelo administrador da rede.
		- Quando o hospedeiro não estiver mais utilizando esse endereço, ele poderá ser alocado a outro hospedeiro.

---
##### ESTADOS DHCP
- Um cliente começa no estado **==INIT==**;
- O cliente entra em contato com todos os servidores DHCP difundindo uma mensagem DHCP **DISCOVER** - 255.255.255.255 (endereço de difusão local).
	- Utiliza a porta 67 do protocolo UDP
	- Após, ele passa para o estado **==SELECTING==**.

- Os servidores recebem a mensagem e enviam uma mensagem DHCP **==OFFER==**
	- As mensagens DHCP OFFER contém informações de configuração e o endereço IP oferecido pelo servidor - o cliente escolhe um entre os oferecidos no caso de mais de uma opção.

- O cliente seleciona um dos servidores e envia uma mensagem DHCP **REQUEST** solicitando uma alocação.
	- O cliente vai para o estado **==REQUESTING==**;

- O servidor responde com a mensagem DHCP **ACK** - confirmação de uso.
	- Da início à **alocação**
	- O cliente segue para o estado **==BOUND==**, que é o estado normal de operação.
		- Enquanto estiver no estado limite, ele poderá usar o endereço IP.

- O cliente configura três **temporizadores** que controlam: **renovação**, **revinculação** e o **fim da alocação**.
	- O valor padrão para o primeiro temporizador é a metade do valor do tempo total da alocação.
	- Quando o temporizador ultrapassar o tempo determinado pela primeira vez, o cliente deverá tentar *renovar* a alocação.
	- Para *solicitar uma renovação* o cliente transmite uma mensagem DHCP **REQUEST** ao servidor que lhe fez a alocação.
	- O cliente muda para o estado **==RENEWING==** - renovação.
		- Caso aprove a renovação, o servidor envia um DHCP **ACK**.
			- O cliente retorna ao estado **==BOUND**==
		- Se não aprovar a renovação, o servidor envia um DHCP **NACK**
			- O cliente **interompe** imediatamente o uso do endereço.
		- Retorna o endereço **INIT**.

- Caso o cliente esteja no estado ==**RENEWING**== e não chegue nenhuma resposta, provavelmente o servidor que concedeu a alocação estará fora de alcance.
  
- Para contornar essa situação O DHCP conta com um **segundo temporizador**, que ==expira após 87,5% do período da alocação==.
	- Faz com que o cliente muda para o estado **==REBINDING==** (VINCULA NOVAMENTE).
	  
- O cliente passa a difundir a mensagem DHCP **REQUEST** para qualquer servidor DHCP.
- Caso receba uma *resposta positiva*, o cliente muda para o estado **==BOUND==** e reconfigura os temporizadores.
- Se a *resposta for negativa*, o cliente muda para o estado **==INIT==** e interrompe imediatamente o uso do endereço IP.

<img src="https://www.researchgate.net/publication/2786978/figure/fig4/AS:669496704503831@1536631861541/DHCP-client-state-diagram-for-DHCP-protocoll20.png" width="80%">

---
#### ICMP (Internet Control Message Protocol)
- RFC 792

- Durante o tráfego de informações pela rede **podem ocorrer erros ou situações anormais** que precisam ser do conhecimento dos hospedeiros.
- Com o **ICMP**, hospedeiros conseguem enviar ==**mensagens de controle e erros**== para outros hospedeiros.
- O erro sempre é reportado ao sistema que gerou o datagrama - baseado no campo IP origem do datagrama.

- O ICMP também inclui um mecanismo de envio e resposta (**echo**) para testar quando um destino é alcançável ou não.
	- O [[Basic Networking Commands|comando]] `ping` utiliza esse protocolo.

- Uma mensagem ICMP é carregada dentro de um datagrama IP.

- As mensagens ICMP mais importantes são:
	- **Destination unreachable** - destino inalcançável
		- O datagrama não pode ser entregue.
		- A sub-rede ou roteador não pode localizar o destino, ou um datagrama com o bit DF (não fragmentado).
	- **Time exceeded** - tempo excedido
		- Campo "tempo de vida (TTL)" do IP chegou a zero.
		- Houve um descarte do pacote devido ao contador ter sido zerado.
	- **Parameter problem** - problema de parâmetro
		- Campo de cabeçalho inválido.
		- Indica a existência de um problema no software IP do *hospedeiro transmissor* ou no software de um *roteador* pelo qual o pacote transitou.
	- **Source quench** - redução de origem
		- Conhecida como pacote regulador.
		- Usada para ajustar os hospedeiros que estejam enviando pacotes demais a fim de controlar o congestionamento na rede - controla a velocidade enviado uma solicitação ao hospedeiro.
	- **Redirect** - Redirecionar
		- Quando um roteador percebe que o pacote pode ter sido incorretamente roteado.
		- É usado pelo roteador para informar ao hospedeiro transmissor a respeito do provável erro.
		- É um caso mais raro de acontecer.
	- **Echo request** - Solicitação de eco
		- Perguntar a um hospedeiro se ele está ativo.
		- Ao receber a mensagem "echo request", o destino deve enviar de volta uma mensagem "echo reply".
		- `ping`
	- **Echo reply** - Resposta de eco
		- Confirmação de que o hospedeiro está ativo.
	- **Timestamp request** - solicitação de carimbo de data e hora
		- Mesmo que o "echo request", mas solicita que a hora da partida e chegada da mensagem sejam registrados na mensagem de resposta.
	- **Timestamp reply** - resposta de carimbo de data e hora
		- Resposta à mensagem "timestamp request"

- Hoje em dia há protocolos mais atualizados - como o IMTP - que fazem melhor esta função.

