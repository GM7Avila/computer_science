# Transporte Orientado à Conexão

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

- ==Caso==: o segmento chegar corretamente, porém ao enviar a confirmação ela for perdida, o segmento será retransmetido **indevidamente** - isso geraria uma duplicação de pacotes.

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
|           |      ➥ Este pacote já foi recebido => descartar
|           |      ➥ ✅ Sem Segmentos duplicados
|<----------|      ➥ [ENVIAR confirmação novamente]

```

- ==Caso==: segmentos recebidos fora de ordem
	- Os números de sequência também servirão para corrigir a ordem dos pacotes.
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