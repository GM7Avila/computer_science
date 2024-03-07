# Switch

> <span style="font-style: italic; font-size:20px; font-family: Georgia, serif; color: #7baf56">"O Switch é um comutador de dados que trabalha com endereço MAC para redes internas.</span>
## Definição

- Concetar dispostivos em uma rede entre si, permitindo que eles se comuniquem por meio 

<img src="https://www.pcweenie.com/images/hni/s03p014_connectRouterDiagram.png">

- Portas: Ethernet
- Plug and play - não precisa de configuração para funcionar
- ==Comutação de dados== (endereça o destino do dado - diferente do hub)
- **Camada 2 - enlace de dados (data-link no modelo OSI)** - ele determina para onde enviar cada quadro de mensagem recebido observando o [[Endereço de Acesso de Controle (MAC)|endereço de controle de acesso à mídia (MAC)]].
- Permite a comunicação dos dados entre os dispositivos conectados através da **TABELA DE ENDEREÇOS MAC**

<img src="https://www.computernetworkingnotes.org/images/cisco/ccna-study-guide/csg36-04-switch-learning-process.png">

### source MAC address X destination MAC address
Quando um dispositivo envia dados, o switch verifica a tabela MAC para determinar a porta pela qual os dados devem ser enviados. O endereço MAC de origem é usado para atualizar a tabela MAC, enquanto o endereço MAC de destino é usado para encaminhar os dados para a porta correta. Assim, a tabela MAC ajuda a garantir que os dados sejam enviados para o dispositivo correto na rede.

## Etapas do mapeamento da Tabela MAC

> Switchs fazem parte da camada 2 do moledo OSI - (Data Link Layer); Por isso ele usa endereços MAC, ao em vez do próprio indereço IP, pois o indereço IP faz parte da camada 3 (Network Layer) - ou seja, ele não tem acesso direto ao IP dos aparelhos por si só.

- A tabela começa vazia, com nenhum valor para MAC Address nem suas respectivas portas.

| MAC ADDRESS | PORT |
| ----------- | ---- |
|             |      |
|             |      |

- Quando um dispositivo tenta se comunicar com outro (na mesma rede) ele envia um pacote ao Switch, com seu endereço MAC e o endereço MAC para qual dispositivo ele deseja enviar o pacote.

| MAC ADDRESS | PORT |
| ---- | ---- |
| 1111.1111.1111 | F0 |
| 2222.2222.2222 | **???** |

- O roteador recebe a informação, porém ele não tem informação salva na tabela sobre a porta que o receptor se encontra (para onde ele deve enviar o acote). Então ele envia cópias do pacote para todos as portas / devices. 
- Ao receber, apenas o dispositivo receptor com o endereço MAC correto usa o frame, e o restante descarta.

- Quando o dispositivo 2222.2222.2222 enviar uma mensagem pelo switch, o switch registrará na tabela suas informações.

| MAC ADDRESS | PORT |
| ---- | ---- |
| 1111.1111.1111 | F0 |
| 2222.2222.2222 | F1 |

- Nas próximas conexões ele precisará apenas consultar a CAM Table para enviar os pacotes, já que ela já foi populada.

## Topologia para Switchs

- **Topologia estrela** - ponto central (switch)

![[Pasted image 20221215151956.png]]

- Conectar vários computadores em uma rede (concentrador de conexão);
- *Comutador de dados* utilizando como concetrador de conexão de redes; 
- Diferente do hub -> além de ser concetrador, ele comuta os dados, *permitindo que eles passem de um computador a outro diretamente*.
	- **Comutação de dados:** encaminhar os dados somente para a porta de destino;
- Recebe o sinal e envia para um computador destinatário da rede (o hub recebe e envia para todos os computadores da rede);

