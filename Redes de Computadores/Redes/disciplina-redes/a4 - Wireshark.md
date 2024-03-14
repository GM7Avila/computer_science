---
banner: "https://www.networkcomputing.com/sites/default/files/styles/flexslider_full/public/3-Wireshark_0.jpg?itok=9Hx_lktt"
banner_y: 0.34002
banner_icon:
---
# Laboratório de Wireshark 


<img src="https://www.wireshark.org/assets/img/wireshark-logo-big.png" width="100%">

## Packet Sniffer

- Ferramenta de software/dispositivo de hardware capaz de interceptar e registrar o tráfego da rede que passa por um determinado nó.
	- Diagnístico de rede
	- Monitoramento de tráfego
	- Segurança de rede
	- Maliciosa: invasores -> intercepção de informações confidenciais

- Configuração de Sniffer: Modo promíscuo
	- Recebe e processa todos os pacotes que passam pela rede, o que é essencial para que os sniffers funcionem efetivamente.

- Sniffer atua na camada de enlace
	- Captura os pacotes que passam pela interface de rede do computador;
	- Um sniffer atua passivamente (não se apresenta na rede), apenas capturando os pacotes.
		- Os pacotes recebidos nunca são explicitamente endereçados ao sniffer.
		- O sniffer recebe uma cópia dos pacotes que passam pela interface de rede do hospedeiro.

> [!NOTE] Exemplo: FTP vs SFTP para o Sniffer
> Enquanto o FTP transmite dados em texto simples, tornando-os vunleráveis a serem capturados por um sniffer, o SFTP (baseado no SSH) utiliza criptografia simétrica para garantir a segurança dos dados durante a transferência, tornando-os essencialmente imunes à interceptação de sniffer.

- O Sniffer é uma adição ao software do hospedeiro e **consiste em duas partes:**

![[Sniffer|100%]]

- **Biblioteca de captura de Pacotes**
	- Recebe uma *cópia* de cada quadro da ==camada de enlace== que é enviado ou recebido pelo hospedeiro.
	- O normal é que quando a informação passa por um nó na rede, ela não ultrapasse da camada de enlace (e sim utilize-a apenas como consulta para saber se é o destino correto, e caso não for, continuar a rota). Entretanto o Sniffer utiliza de mecanismos internos para receber cópias da informação, mesmo não sendo o destinatário.
	
- **Analisador de Pacotes**
	- Exibe os encapsulamentos e o conteúdo de todos os campos dos protocolos presentes.
	- Deve conhecer a estrutura de todos os protocolos.
	- Wireshark (aplicação)
## Busca
- A busca é realizada com os caracteres em minúsculo
- usar `.`, buscará as propriedades internas do elemento; é possivel fazer variados tipos de buscas, exemplos:
	- `udp.port==53` - busca pela porta 53 que usa UDP
	- `ip.src==193.168.1.1` - filtro mostra apenas os pacotes que têm o endereço IP de origem especificado; útil para exanminar o tráfego originado de um determiando host.
	- `tcp.flags.syn==1 and tcp.flags.ack==0` - pacotes TCP que tem o flag SYN definidos como 1 (indicando uma solicitação de conexão) e o flag ACK definido como zero (indicando que não é uma confirmação de conexão); isso pode ser útil para encontrar tentativas de conexão TCP.

## Captura de Pacotes

> [!NOTE] Protocolo DHCP
> O protocolo DHCP é usado para configuração automática de endereço IP. Quando a máquina incializa, ela envia um pacote (**pacote de descoberta** - enviado por toda a rede de difusão) solicitando informações de configurações de rede. O servidor DCHP fornece essas informações à maquinas - configuração automática.
> 
> **Leasetime** - tempo de aluguel: por quanto tempo uma máquina pode usar um endereço IP em uma determianda rede.

### Lista - Lab Wireshark UDP
1. Source Port, Destination Port, Length, Checksum
2. Todos tem 1 byte ($4*2=8$ bytes) 
3. É o comprimento do segmento (UDP): é a soma do length da data + length UDP ($1227-1219 = 8$ bytes)
4. 8 Bytes
5. numero de 2 bytes = 1111.1111 = 2^16 = 65535 (total); porém a carga útil é a capacidade de dados que o UDP pode levar para a camada superior: 65535 bytes - 8 bytes (cabeçalho) = 65527 (**teórico**); Porpem na prática ainda temos que considerar o IP (se considerarmos o ipv4 seria 65527 - 20 bytes = 65507)
6. Protocol: UDP (17)