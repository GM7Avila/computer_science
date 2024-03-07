# Basic Network Commands
## Windows
- ipconfig (informação da camada 3 - Network Layer)
	- IPv6 Address
	- Temporary IPv6 Address
	- Link-local IPv6 Address
	- IPv4 Address
	- Subnet Mask
	- Default Gateway

- ipconfig/all (informações da camada 3 + camada 2)
	- lista também informações de endereço MAC

- nslookup: solicitar informações ao DNS
```
[avila@mint~]$ nslookup
> www.google.com.br
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	www.google.com.br
Address: 142.251.134.99
Name:	www.google.com.br
Address: 2800:3f0:4004:800::2003
```

- ping: verificar acesso entre duas máquinas
	- `ping 142.251.134.99` - faz um ping para o google
	- envia pacotes e recebe de volta;
	- indica possível perda de pacote;

- tracert (Trace Route): informa o IP e retorna a rota que um pacote percorre até o endereço.
  
## Linux
### 1. ifconfig
- Exibe informações sobre as interfaces de rede, incluindo endereços IP, máscaras de sub-rede e endereços MAC. 
	- `$ ifconfig`

- Para versões recentes, o comando `ifconfig` foi substituído por `ip` 
	- `$ ip address show`
### 2. ip
- Oferece uma gama de funcionalidades para configuração e exibição de informações de rede.
	- `$ ip addr show`

- Para visualizar informações mais detalhadas sobre a configuração de todas as interfaces de rede.
	- `$ ip -s link show`
### 3. nslookup
- Interroga servidores de nomes DNS para obter informações sobre domínios.
	- `$ nslookup example.com`
### 4. ping

- Testa a conectividade com um host específico, enviando pacotes ICMP Echo Request e aguardando respostas.
	- `$ ping example.com`
### 5. traceroute

- Mostra a rota que os pacotes IP levam para alcançar um host, listando os saltos intermediários.
	- `$ traceroute example.com`
### 6. route

- Exibe e manipula a tabela de roteamento do kernel.
	- `$ route -n`

