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
 
```
00000000.00000000.00000000.00011111
   000  .   000  .   000  .  000/32
```

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
- Classe A: 8 bits representam a rede e os 24 restantes os hospedeiros.
- Classe B: 16 bits representam a rede e 16 os hospedeiros.
- Classe C: 24 bits representam a rede e 8 bits os hospedeiros.