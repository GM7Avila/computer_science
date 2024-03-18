# Transporte Orientado à Conexão

#BitAlternating #GoBackN #TCP

## Transferência Confiável de Dados

- **Camada de Transporte** utiliza serviços da camada de ==rede==

- Em particular o ==protocolo IP== pode:
	- Entregar pacotes duplicados
	- Entregar pacotes com erros
	- Entregar pacotes fora de ordem
	- Perder pacotes
- Cabe ao serviço de transporte **corrigir** todos esses problemas e entregar ao destino sem errors e na ordem correta.
	- A forma mais comum, é dar ao **transmissor** informações sobre a *recepção dos dados* 

- Normalmente o receptor retorna informações de controle contendo:
	- **confirmações positivas**: avisar que os dados foram recebidos com sucesso
	- **confirmações negativas:** avisar que houve erro na recepção
	
	- essas confirmações podem ser transportadas em um campode controle no cabeçalho
		- técnica conhecida como **piggybacking**
		- ao ser informado que houve erro o transmissor pode ==retransmitir o **segmento**== 

### Verificação de Erro

1. **VERIFICAR ERRO**
- O transmissor pode realizar um **==Checksum==** sobre os dados e enviar o resultado em um campo de cabeçalho.
- O receptor realiza o mesmo processo nos dados recebidos e compara ao resultado recebido.

2. **AVISAR SOBRE O ERRO**
- ==Caso==: **dados se perdem completamente**
	- Se o segmento não chegar ao receptor ele não consegue continuar o processo, e isso pode significar que os dados foram perdidos.
	- Para isso, o transmissor utiliza de **temporizadores** para avisar ao protocolo quando já passou muito tempo da transmissão de um segmento e ele não foi confirmado.
		- Nesse caso o transmissor **retransmite** o segmento.

- ==Caso==: o segmento chegar corretamente, porém ao enviar a confirmação ela for perdida, o segmento será retransmetido **indevidamente** - isso geraria uma duplicação de segmentos.

```
T           R
|---------->| 🆗
|           |
|   ❌<-----| ⚠️ [ERRO ao enviar segmento de confirmação]
|           |
|---------->| 🆗 [temproizador do transmissor excedeu o tempo]
|           |      ➥ ⚠️ Segmentos duplicados   

```

- Para corrigir atribuimos também **números de sequência** aos segmentos enviados, para o receptor distinguir as retrnasmissões originais.
```
T           R
|---------->| 🆗 [Segmento N recebido]
|           |
|   ❌<-----| ⚠️ [ERRO ao enviar segmento de confirmação]
|           |
|---------->| ⚠️ [ERRO: Segmento N recebido, se esperava N+1]
|           |      ➥ Este segmento já foi recebido => descartar
|           |      ➥ ✅ Sem Segmentos duplicados
|<----------|      ➥ [ENVIAR confirmação novamente]

```

- ==Caso==: segmentos recebidos fora de ordem
	- Os números de sequência também servirão para corrigir a ordem dos segmentos.
	- Imagine que o segmento 3 foi enviado primeiro do que o segmento 2.

```
T           R
|---------->|    [Segmento 1 recebido]--------|
|           |                                 |
|<----------|   [ENVIAR confirmação de 1]     |
|           |                                 |     Reordenando...
|---------->|    [Segmento 3 recebido]--------|---->[1,3,2]-->[1,2,3]
|           |                                 |
|<----------|   [ENVIAR confirmação de 1]     |
|           |                                 |
|---------->|    [Segmento 2 recebido]--------|        
```

### Bit Alternative
- Protocolo simples
- Capaz de realizar transferência confiável entre dois hospedeiros
- Tranmissão do tipo **Stop and Wait** 
- **Transmissor**:
	- Envia -> espera pelo temporizador ou mensagem de erro -> retransmite
	- Só passa para o próximo segmento quando garantir por confirmação positiva.
- **Receptor**
	- Aguarda segmento
	- Verifica se o segmento está correto (número de sequência)
		- Mesmo que nesse caso, só envia um segmento de cada vez e não sai de ordem, ainda assim precisamos do número de sequência. Pode acontecer do receptor *fazer a confirmação, e a confirmação se perder* ao enviar para o transmissor.

![[bit alternante|100%]]

- Um único bit é suficiente para saber se a informação chegou ou não.
- Um exemplo de protocolo que utiliza essa técnica para garantir a entrega dos dados transferios é o [[_TCP-IP Protocol Suite|TFTP (Trivial File Transfer Protocol)]]

> [!NOTE] TFTP vs FTP
> O [[_Sockets FTP]] utiliza o protocolo TCP para a comunicação, ou seja, é orientado a conexão, enquanto o TFTP funciona sobre o [[12. Sockets|UDP]] - não confiável.

- No geral, é um protocolo **simples** mas de **baixo desempenho**.
	- O transmissor fica aguardando uma resposta.

### Transferência de Dados com Paralelismo

- É necessário aumentar a faixa de valores que compõem os números de sequência (enquanto no bit alternative só precisávamos de um único bit: 0 ou 1)
- O transmissor deve possuir **buffers** suficientes para armazenar todas as mensagens que foram transmitidas mas ainda *não confirmadas* - para eventuais retransmissões.

- A retransmissão da mensagem pode ser tratada por duas técnicas:
	- **Go-Back-N**: Todos os segmentos a partir do que não foi confirmado são retransmitidos (==implementado sobre o [[_Sockets TCP|TCP]]==)
	- **Repetição Seletiva**: São retransmitidos apenas os segmentos não confirmados (==mais sofisticado - controle maior==).
- Ambas permitem a transmissão de múltiplos segmentos *sem que seja necessário esperar a confirmação de cada um*. Ambas tem desempenho muito parecido.


- Mas existe uma quantidade limite de segmentos que podem ser enviados (sem confirmação) - **tamanho da janela de transmissão**.
	- Faixa de números de sequência dos segmentos que podem ser transmitidos após o último segmento confirmado.

![[_janela-de-transmissão|100%]]

- **Janela deslizante** - Conforme as confirmações positivas, os segmentos são considerados confirmados e a janela se "desloca para a direita" (assim são conhecidos os protocolos que usam dessa característica).

#### GO-BACK-N
- É um protocolo de janela deslizando de tamanho N - é possível enviar até N segmentos sem a necessidade de confirmação.
	- O **transmissor** deve ter ==bufferes suficientes para armazenar esses N segmentos==.

- Quando é solicitado uma transmissão o transmissor:
	- ==Verifica== se há espaço na janela de transmissão.
	- ==Caso haja espaço==, o o segmento é transmitido e *uma cópia é armazenada no buffer* de transmissão.
	- Caso a ==janela esteja cheia==, o processoq ue solicitou o envio do segmento deve ser bloqueado, até que a transmissão seja liberada.

- Trabalha com ==**confirmações positivas do tipo cumulativas**==
	- Na confirmação comulativa, quando um segmento de números N é confirmado significa que todos os outros segmentos anteriores a N também forem recebidos com sucesso. 
	- Se recebeu 5, então: 5, 4, 3, 2 e 1 foram recebidos corretamente.
- Não trabalha com confirmações negativas
	- A única forma do transmissor saber que é necessário retransmitir é pelo ==temporizador== - quando houver time out, todos os segmentos que estão dentro da *janela de transmissão* serão retransmitidos.
- Sempre que chega uma confirmação positiva e ainda existem segmentos não confirmados na janela, **o temporizador é reiniciado**.


- No ==**receptor**==, quando chega um segmento o **número de sequência** e a **checksum** são verificados.
	- ==Se ambos estivere corretos==: encaminhados ao processo receptor e confirmação positiva é enviada ao transmissor.
	- ==Se não==: o protocolo descarta o segmento e envia ao transmissor uma *confirmação positiva* relativa ao **último segmento que recebeu corretamente**.
	- Quando o temporizador retransmite todos a partir desse ponto.

> [Go-Back Protocol Simulation - (pearsoncmg.com)](https://media.pearsoncmg.com/aw/ecs_kurose_compnetwork_7/cw/content/interactiveanimations/go-back-n-protocol/index.html)
> 
> [Go-Back-N protocol | packet lost | sliding window protocol | transport layer (youtube.com)](https://www.youtube.com/watch?v=Vnye8YJSnDU)
> [Go-Back-N protocol | acknowledgment lost | Sliding Window Protocol | Transport layer (youtube.com)](https://www.youtube.com/watch?v=9JJWFbqAjpY)


![[Pasted image 20240317203521.png]]

![[Pasted image 20240317204226.png]]

- Ao perder o Pkt3, o Receiver retornará para todos os outros segmentos o valor do último segmento antes do 3, que no caso foi o segmento 2 - isso quer dizer que ele solicitará uma retransmissão de todos os segmentos depois do 2 - **teste isso no simulador**.

---
**❖ EXEMPLO** 
- Erro no envio do Pkt 5 (legenda abaixo)
	- Azul escuro: transmitido e confirmado
	- Azul claro: transmitido e não confirmado
	- Verde: erro
	- Amarelo: Ack
![[Pasted image 20240317205714.png]]
![[Pasted image 20240317205731.png]]
- O protocolo descarta o segmento e envia ao transmissor uma *confirmação positiva* relativa ao **último segmento que recebeu corretamente**.
- O time out vai estourar e ==enviar novamente toda a janela **a partir do último segmento verificado==. 
- Ou seja, se um falha: retransmite tudo a partir do ponto que falhou.
![[Pasted image 20240317205859.png]]


![[Pasted image 20240317204559.png]]

#### REPETIÇÃO SELETIVA

> [Selective Repeat Protocol Simulation (unicam.it)](https://computerscience.unicam.it/marcantoni/reti/applet/SelectiveRepeatProtocol/selRepProt.html)

- Não faz retransmissão de todos os segmentos no erro - *apenas os perdidos*.
- Cada segmento deve ser confirmado individualmente.
- Como as confirmações são individuais, o protocolo ==não faz uso de confirmação cumulativa==

- No ==**receptor**== os segmentos recebidos corretamente são todos confirmados, independente de estarem ou não na ordem correta.
	- Deve ter buffer para armazenar todos ele - só vai retransmitir o que se perdeu.
	- Caso ocorra de chegar um segmento **já confirmado**, o receptor **descarta** e envia ao transmissor uma *confirmação positiva**.

---
## Protocolo TCP

- O [[12. Sockets|TCP]] (Transmission Conrtol Protocol) é um protocolo orientado à conexão e indicado para aplicações que *necessitam trocar grande quantidade de dados através de uma rede com múltiplos roteadores*. Algumas características do TCP:
	- orientado a conexão
	- confiável: dados entre na ordem correta e sem erros
	- ponto a ponto: conexão entre dois processos
	- full duplex: simultâneamente em qualquer direção
	- fluxo de bytes: não garente que os dados sejam entregues no destino na mesma porção em que foram entregues na origem - ou seja, ele ==não preserva as fronteiras entre as mensagens==.

- Ofere um **fluxo de bytes** confiável.
	- O TCP não enxerca como mensagens individuais a se transmitir e sim, a nível de bytes a serem transmitidos.
		- Pode juntar diferentes mensagens em um único segmento.
		- Pode quebrar a mensagem em vários segmentos no momento do envio.
		- Se a aplicação que está utiliazndo o TCP necessitar que as fronteiras entre as mensagens sejam preservadas, *caberá ao programador da aplicação* implementar essa lógica.
- Na transmissão
	- Aceita fluxos de dados da aplicação
	- divide-os em partes de no máximo 64 Kbytes
	- Envia cada parte em um datagrama IP distinto.
- Na recepção
	- Restaura o fluxo de dados original


- O **Protocolo IP** não oferece nenhuma garantia de entrega dos segmentos no hospedeiro destino. *Cabe ao TCP*:
	- Administrar temporizadores.
	- Retransmitir os dados sempre que necessário.

- **Mecanismos** do TCP:
	- *Checksum*
	- *Temporizador*
	- *Número de sequência*
	- *Reconhecimento positivo*: usado pelo destinatário para avisar ao remetente que um segmento ou conjunto de segmentos foi recebido corretamente.
	- *Janela/paralelismo*: O remetente pode ficar restrito a enviar somente segmentos com números de sequência que caiam dentro de uma determinada faixa.

### Formato do Segmento
- Cada segmento TCP começa com um cabeçalho fixo de **20 bytes**
	- Cada linha do cabeçalho tem 32 bits (4 bytes)
- Depois dessas opções pode haver *65515 bytes de dados* - provenientes da camada superior - na prática ainda temos que subtrair 20 bytes do IP.
- **Segmentos sem dados** -> confirmação e mensagem de controle.

<img src="https://www.bosontreinamentos.com.br/wp-content/uploads/2015/10/cabe%C3%A7alho-tcp-campos-.jpg"width="100%">

- **Porte de Origem e Destino**: identificam os [[06. Endereço de Porta|as portas onde os processos estão sendo executados]].
	- Permitem que aplicações compartilhem o uso do TCP: *multiplexação e demultiplexação*.
- **Número de sequência** - a nível de byte - indica o próximo byte que espera ser recebido pelo destinatário.
- **Número de reconhecimento** - indica o próximo byte esperado pelo destinatário
	- Numero de sequência: A manda pra B dados de 1 a 6
	- Numero de reconhecimento: B manda para A um segmento com reconhecimento com o valor 7 - o próximo byte que ele está aguardando.
- **Comprimento do cabeçalho** (HLEN) - informa o tamanho do cabeçalho em palavras de 32 bits - pode variar em função das opções.
- **Campo de Flags** (1 bit cada)
	- **URG** - Ponteiro urgente - indica que o segmento carrega dados urgentes;
	- **ACK** - Acknowledgement - indica que o campo "número de confirmação" contém uma confirmação;
	- **PSH** - Push - Solicita ao receptor para entregar os dados à aplicação assim que chegar, em vez de armazená-los em um buffer;
	- **RST** - Usado para rejeitar um segmento inválido ou recusar uma tentativa de conexão;
	- **SYN** - Usado para estabelecer conexões;
	- **FIN** - Usado para encerrar uma conexão;

### Conexão TCP
- Estabelecimento da conexão: troca de segmentos iniciais 
- Protocolo ponto a ponto full duplex
- Cliente Servidor
	- Cliente inicia a conexão
	- Servidor recebe a solicitação de conexão

#### Estabelecimento da conexão
- São trocados 3 segmentos para o estabelecimeto da conexão
	- **Apresentação de três vias** (**three way handshake**)

<img src="https://i.stack.imgur.com/y17TW.png">

1. Cliente envia um segmento TCP 
	- Sem dados
	- `SYN` = 1
	- Envia um número de segmento *aleatório* no campo **número de sequência** (`client_isn` = 8000)
2. O **datagrama IP** contendo o segmento TCP SYN chega ao servidor
	- O servidor aloca buffers e variáveis TCP à conexão.
	- O servidor envia um segmento de **aceitação de conexão** ao TCP cliente.
		- Sem dados
		- Possui o bit `SYN` = 1
		- O campo **número de reconhecimento** possui o valor `client_isn + 1` = 8001
		- Envia um número de segmento aleatório no campo **número de sequência** (`server_isn`)
3. O cliente recebe o segmento `SYNACK` enviado pelo servidor
	- Aloca buffers e variáveis TCP á conexão. 
	- O cliente envia ao servidor mais um segmento.
		- *Pode conter dados*
		- Possui o `SYN` = 0
		- O campo de **número de reconhecimento** possui o valor `server_isn + 1`

#### Recusando uma Conexão
- Caso haja a tentativa de conexão com um hospedeiro que não possui um processo aguardando por conexão ([[12. Sockets|listen]]) na porta destino, a emtodade TCP no hospedeiro destino recusará a conexão. Exemplo:
	- Supondo que listen tenha reservado espaço para uma determinada quantidade de conexões;
	- O cliente faz solicitações até que estoura a capacidade de conexões.
	- Um novo cliente tenta solicitar a conexão - o servidor não tem mais espaço reservado em buffer para conexão. 
	- O TCP precisa recusar essa conexão:
		- O cliente manda um segmento com bit `SYN` para o servidor.
		- O servidor responde um segmento com o bit `RST` ativado.
		- Agora o cliente sabe que a conexão não foi aceita.

#### Encerramento da Conexão
- Qualquer um dos lados pode solicitar o encerramento da conexão - `FIN` ativado;
- As conexões são **encerradas de forma independente em cada sentido**.
	- Se existem dois hospedeiros A e B, e o ==hospedeiro A encerra a conexão==, a conexão será encerrada apenas no sentido de A para B.
	- B poderá continuar enviando dados para A.
	- ==A continuará confirmando todos os dados recebidos de B.==

### Política de Transmissão TCP
O TCP deve controlar as informações transmitidas e realizar retransmissões sempre que necessário.
- Precisa saber quais dados chegaram com sucesso
- Número de sequência e de reconhecimento são fundamentais

- **Número de sequência:** relativo à ==posição ocupada pelo byte no fluxo de dados da conexão==
	- O TCP não preserva o limite entre as mensagens
	- **Posição do dado dentro da mensagem.**
	- Se um segmento TCP possui número de sequência N, o primeiro byte de dados carregado pelo segmento deve entrar na posição N do fluxo de dados da conexão.
- **Número de reconhecimento:** indica ao transmissor qual é a ==posição do **próximo byte** que o receptor espera receebr no fluxo de dados==

___
**❖ EXEMPLO** 
- Um cliente enviará ao servidor um arquivo que possui exatamente 22231 bytes, e que fará o envio utilizando um software que utiliza o protocolo TCP para transporte. Suponha que:
	- O número de sequência do primeiro byte no fluxo de dados da transmissão é 1.
	- Por questão de tecnologia da rede o TCP necessite fazer a transferência de dados em segmentos que carreguem exatamente 10000 bytes de dados da aplicação.

```
C                                    S
|   SEQ=1 , DADOS (1 a 10000)        |
|----------------------------------->|
|                                    |
|          ACK = 10001               |
|<-----------------------------------|
|                                    |
|  SEQ=10001 , DADOS (10001 a 20000) |
|----------------------------------->|
|                                    |
|          ACK = 20001               |
|<-----------------------------------|
|                                    |
|  SEQ=20001 , DADOS (20001 a 22231) |
|----------------------------------->|
|                                    |
|          ACK = 22232               |
|<-----------------------------------|
```

**Envio Fora de Ordem e Time out** 

![[_TCP Segmentos fora de Ordem]]

---
### Controle de Fluxo
> 6.5.8 Política de Transmissão do TCP - Tanenbaum

![[_TCP Controle de Fluxo|100%]]

> Repare que as mensagens não tem fronteiras estabelecidas, são tratadas apenas a nível de bytes.

- Quando chegam dados, a entidade TCP os recebe e os coloca em um **buffer**, e avisa ao protocolo da aplicação receptora que dados estão disponíveis para serem retirados do buffer.
- O TCP provê um **serviço de controle de fluxo** para suas aplicações (no caso do UDP a aplicação deve implementar).

- Se houver perda durante a confirmação, o TCP enviará novamente a mensagem, porém o receptor verificará que o **número de sequência** enviado já foi alocado no buffer, então ele apenas descartará a informação e enviará uma nova mensagem de confirmação, assim como a que havia perdido.
### Congestionamento
- Em uma rede de computadores pode ocorrer de vários hospedeiros realizarem transferência de dados tentando utilizar a vazão máxima que seus enlaces permitem.
- O tráfego no núcleo da rede pode se elevar a níveis que comprometem a capciade de transferência, podendo causar longas filas no roteadores e consequentemente perda de muitos segmentos - *congestionamento*.

> [!NOTE] Controle de congestionammento e Controle de fluxo
> Ambas as ferramentas de controle podem ser parecidas, mas são tarefas completamentes distintas. O **controle de fluxo** cuida da transmissão entre dois processos, a fim de evitar o estouro de buffer. Já o **controle de congestionamemnto** é uma **questão global que diz respeito a toda a rede**, e que afeta todas as conexões que passam pela parte da rede que está congestionada.

- **Suponha o seguinte esquema**:
```
Switch1 ----------- Roteador ----------- Switch2
/     \                                  /     \
A     B                                 C       D
```
 
- Um roteador que consegue processar dados do switch1 para o switch2 a uma taxa de 100Mbps, e que todos os enlaces têm uma capacidade de transmitir a uma taxa de 1000Mbps cada.

- Suponha que o hospedeiro A esteja enviado dados para o hospedeiro D com uma taxa Ta e que o hospedeiro B esteja enviando para o C com uma taxa Tb.
	- Enquanto ==Ta + Tb < taxa de transmissão do roteador== (100Mbps), será possível realizar a entrega dos segmentos normalmente pelo roteador.
	- Porém, se Ta + Tb começar a se aproximar de 100Mbps ou for superior a taxa (por exemplo, se os quatro começam a enviar em uma taxa cujo somatório seja maior que o suportado pelo roteador):
		- A meória do roteador encherá e perderá segmentos.
		- Os buffers de saída do roteador começarão a encher e os segmentos passarão a permanecer cada vez mais tempo dentro dos buffers.
		- Haverá um aumento significativo no atraso de transmissão.
		- Chegará um momento emq ue não será mais possível enfileirar os segmentos, sendo assim, o roteador os descarta.

![[_TCP Início do Congestionamento]]

- Antes mesmo antes de atingir os 100Mbps os buffers de saída já começam a encher provocando atraso nas entregas dos segmentos.
- ==Em uma rede congestionada, a taxa de transferência de uma conexão diminui a cada roteador pelo qual os dados passam== - quanto mais roteadores os segmentos precisarem trafegar, maior será a perda de dados.
- Isso acarreta na ==queda da taxa de dados entregues corretamente== (chega ao extremo fazendo com que a taxa de transferência chegue a zero);

![[_Taxa de transmissão x integridade dos dados]]

- Enquanto está dentro do limite dos roteadores a taxa de transmissão e a taxa de integridade da entrega dos dados é praticamente diretamente proporcional. 
- Quando se atinge o limite da taxa de transferência dos roteadores, o desempenho na entrega corretamente dos segmentos cai drasticamente - pois além das perdas de segmentos, o protocolo TCP começa a realizar uma série de retransmissões, o que acaba piorando ainda mais a situação.

### Controle de Congestionamento no TCP
- O TCP opera utilizando serviços oferecidos pelo protocolo IP, porém *o IP não fornece explicitamente informações relativas ao congestionamento* da rede;
- O TCP faz uso de um ==controle de congestionamento **fim a fim**== (origem-destino);
	- No transmissor, é limitado a taxa de transmissão de acordo com o congestionamento percebido na rede.
	- Se a recepção for de que não há congestionamento, pode-se aumentar a taxa de transmissão.
	- Se a recepção for de que está havendo congestionamento, a entidade TCP deverá diminuir sua taxa de transmissão.
- O controle de congestionamento é realizado criando uma ==**janela de congestionamento**==
	- Seu tamanho representa a *quantidade de bytes que o transmissor pode ter na rede a qualquer momento* - limita a transmissão.
- Quando for realizar uma transmissão, a entidade TCP deve verificar tanto o tamanho da *janela de controle de fluxo* quanto o tamanho da *janela de congestionamento*.
	- Os dados serão retransmitidos sempre dentro da menor dentre essas janelas.

- Considerando:
	- **rwnd** - tamanho da janela de controle de fluxo
	- **cwnd** - tamanho da janela de controle de congestionamento
	- o transmissor utiliza sempre o menor para transmitir.
- o transmisso deverá respeitar a equação:
	- Tamanho da Janela de transmissão $= UltimoByteEnviado - UltimoByteConfirmado$
	- $UltimoByteEnviado - UltimoByteConfirmado <= mínimo(cwnd, rwnd)$
	- logo:  $JanelaTransm.size <= mínimo(cwnd, rwnd)$
- ou seja, o tamanho da janela de transmissão vai ser o menor tamanho entre cwnd e rwnd

- Quando o temporizador de um transmissor se esgota (**evento de perda**): considera-se que houve perda de segmentos e que o TCP deverá retransmitir.
- Outro exemplo de **evento de perda** é quando chegam sucessivas **confirmações com o mesmo número de confirmação**.
	- *Eficiência*: é comum que depois da terceira confirmação com o mesmo número o TCP adiante as retransmissões.


- ==O TCP considera que a perda de segmentos se dá apenas por **problemas de congestionamento**==:
1.  **Se não está ocorrendo ==evento de perdas==**
	- Ele assume que não há **congestionamento**
	- Pode *aumentar o tramanho da janela de congestionamento, ==aumentando sua taxa de transmissão==.
2. **Se a rede estiver passando por ==dificuldade na entrega==
	- Os segmentos de dados demorarão para chegar ao destino
		- As confirmações também demorarão para chegar no transmissor.
	- Faz com que a *taxa de aumento da janela de congestionamento seja baixa.
3. **Se a rede estiver com ==sobra de banda== - os segmentos chegando com sucesso
	- Os segmentos de dados chegarão rapidamente ao destino
		- As confirmações também chegarão rapidamente ao transmissor.
	- Faz com que a *taxa de aumento da janela de congestionamento seja alta.
- Por isso dizemos que o TCP é um ==**PROTOCOLO ALTO REGULADO**==
- Constante busca pela melhor taxa de transmissão.

> O tempo de estouro do temporizador é definido de acordo com a situação da rede - regulado pela conexão.

### Partida Lenta
- No início da conexão o TCP inicia uma partida lenta:
	- No início da conexão o tamanho da janela de controle de congestionamento é estipulado em 1MSS - só pode enviar um segmento inicialmente (*não começa com paralelismo e sim como bit alternado*)

- Variáveis
	- **cwnd** - tamanho da janela de controle de congestionamento;
	- **MSS** - Maximum Segment Size 
		- Representa a maior quantidade de dados que pode ser enviado em um segmetno TCP.
		- Valor negociado no momento do estabelecimento da conexão.
	- **RTT** - Round Trip Time (tempo de viagem de ida e volta).
		- Tempo que um pequeno segmento leva para sair do transmissor, chegar no receptor e voltar como confirmação para o transmissor.
		- 1 RTT é o tempo de ida e volta (confirmação)

A taxa de transmissão T pode ser calculada: $T=MSS/RTT$
- Quanto foi transmitido em um determinado tempo = taxa de transmissão (kbps)
- Uma rede que possua um MSS=1500 bytes e RTT 20ms
	- 1500bits/20ms = 12000bits/0,020s = 600kbps

Abaixo temos a média do RTT ao se comunicar com google.com.br: 4 MS
```
> ping www.google.com.br

Disparando www.google.com.br [1800:f30:4204:834::2001] com 32 bytes de dados:
Resposta de 1800:f30:4204:834::2001: tempo=4ms
Resposta de 1800:f30:4204:834::2001: tempo=8ms
Resposta de 1800:f30:4204:834::2001: tempo=4ms
Resposta de 1800:f30:4204:834::2001: tempo=6ms

Estatísticas do Ping para 1800:f30:4204:834::2001:
    Pacotes: Enviados = 4, Recebidos = 4, Perdidos = 0 (0% de perda),
    
Aproximar um número redondo de vezes em milissegundos:
    Mínimo = 4ms, Máximo = 8ms, Média = 5ms
```

Na prática...
- A cada confirmação recebida, a janela de transmissão *aumenta em 1MSS* (crescendo exponencialmente).
- A **janela de controle de congestionamento** dobra a cada tempo de ida e volta.
- Para o exemplo anterior, onde a taxa incial é 600kbbps, a taxa de transmissão pode ultrapassar 300Mpbs em apenas 200 ms.
![[partidalenta.png]]

- Caso aconteça evento de perdas (temporizador estourou sem recebimento) algumas ações são tomadas.
	- Uma variável conhecida como **ssthresh** (slow start threshold - limiar de partida lenta) recebe a mentade do valor atual do **cwnd**.
	- **cwnd** recebe o valor 1
	- O processo de partida lenta é reiniciado.

<img src="https://www.researchgate.net/publication/256871809/figure/fig6/AS:668503946297344@1536395169060/TCP-Slow-Start-and-Congestion-Avoidance.ppm">

- A partir da segunda rodada o valor de cwd continua aumentando em 1 MSS a cada confirmação recebida, até o se iguale ao valor de ssthresh
	- A partir desse momento o TCP muda para o modo **prevenção de congestionamento** (congestion avoidance).

- No caso de recebimento de 3 confirmações com o mesmo valor o TCP entra no modo de **recuperação rápida**.

> O algoritmo de controle de congestionamento TCP é padronizado pelo RFC 5681

### Modo de Prevenção de Congestionamento
- Quando entra no modo prevenção de congestionamento, o valor de `cwnd` é metade do valor que tinha quando o congestionamento ocorreu pela última vez.
- O TCP passa a aumentar o tamanho de `cwnd` mais lentamente - em vez de aumentar o valor de cwnd em 1MMS a cada confirmação que chega, seu valor é aumentado em 1MMS a cada RTT (*tempo de ida e volta*).

- A cada esgotamento do temporizador do transmissor, o algoritmo calcula um novo valor **ssthresh** (novamente metade do valor atual de cwnd) e reinicia o processo de partida lenta.
- Esse algoritmo faz com que o transmissor fique sempre **próximo da taxa limite** em que pode transmitir, *obtendo um bom desempenho para toda a rede*.

- A **recuperação rápida** se inicia quando são recebidas 3 confirmações com mesmo número de sequência.
- O TCP assume que pode ter havido a perda do segmento e providencia sua retransmissão.
- Após a retransmissão do segmento:
	- A variável ssthresh recebe o valor correspondente a metade do valor contindo na variável cwnd.
	- A variável cwnd no entendo é atualizada com valor sshthresh+3 - ou seja, *não precisa ir à zero*.
	- A Imagem abaixo mostra esse comportamento (linha vermelha).

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20220202202717/tahoerenocongestionwindowcompare.png">

- O comportamento ao longo do tempo (com congestionamento) é o formato de dente de serra;

---

> Continuação: camada de rede aula 6