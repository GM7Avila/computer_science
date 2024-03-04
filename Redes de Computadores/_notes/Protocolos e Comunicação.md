# Protocolos e Comunicação

## Network Communication
1. A mensagem deve ser codificada
2. A mensagem deve ser formatada e encapsulada
3. Tempo sincronizado
4. Vazão de dados no canal 
5. Opções de envio (message delivery)

## Elements of a Protocol
1. Mensagem Codificada
2. Mensagem Encapsulada e Formatada
3. Tempo de Mensagem
4. Tamanho da Mensagem
5. Message Delivery Options

### Codificação de Mensagem
Message Source **->** Enconder (Signal) **->** Transmission **->** Decodificação **->** Message

### Formatação de Mensagem e Encapsulamento
- Informações do destinatário são adicionadas aos dados pelo remetente - qual fluxo seguir?
- Os dados são encapsulados junto dessa informação.

### Tempo de Mensagem
- Dispositivos não operam na mesma velocidade.
- Controle de Fluxo
- Tempo de Resposta

>  Um dispositivo pode atropelar o outro de informação, enquanto o outro ainda está tentando processar o que está acontecendo. Pra isso devemos sincronizar o tempo de envio das mensagens.

- O receptor e o emissor devem chegar a um acordo de time share

> Eles chegam a um acordo de qual será a velocidade de pacotes baseado na capacidade de processamento de cada um - o dispositivo mais lento que determinará a velocidade de comunicação.

> **Uma das responsabilidades da [[04. Modelo OSI|Camada de Enlace]] é definir o tempo de mensagem na comunicação.**

### Dimensionamento da Mensagem
- Se a mensagem for muito grande, ela é divida em pacotes menores.
- Os pacotes menores devem ser numerados para ter um controle caso eles se percam pela rede e para reordena-los na sequência correta.

### Message Delivery Protocol

<img src="https://www.freetimelearning.com/images/interview_questions/CCNA-Cast.png">

#### Unicast
- O frame é enviado de um dispositivo a outro (1 para 1).
#### Multicast
- O frame é enviado para um ou mais dispositivos selecionados.
#### Broadcast
- O frame é enviado para todos os dispositivos da rede (sem excessão).