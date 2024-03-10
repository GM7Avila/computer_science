# Domain System Name

- https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/

- Nomes de domínio (site.com)
- O [[Browser]] intera por meio de endereços IP [endereços IP](The%20Internet%20Explaneid.md).
- O DNS foi criado com a finalidade de criar um sistema de nomes de forma hierárquica, baseado em *domínios*.

- Os principais serviços providos pelo DNS são:
	- Mapeamento de nomes de hospedeiros em endereço IP
	- Apelidos para hospedeiros
	- Identificação de servidores de correios eletrônicos
	- Distribuição de carga
	- Descoberta de nomes de hospedeiros (maepament reverso)
- Atualmente o DNS é definido no RFCs 1034 e 1035

---
## Como funciona?

### Servidores DNS - Carregamento de uma página Web

- Recursor DNS: receber consultas de clientes e realizar consultas adicionais para satissfazer a consulta. Pode ser pensado como o bibliotecário.
- Servidor Raiz: primeira etapa na **resolução** de nomes de host legíveis por humanos em IP - raiz. Pode ser pensado como indice de uma biblioteca que aponta para diferentes estantes de livros.
- Servidor de Nomes TLD: É a próxima etapa na busca por um endereço IP específico e hospeda a última parte de um nome de host (em example.com, o servidor TLD é "com"). Pode ser pensado como uma estante específica de livros em um setor.
- Servidor de Nomes Autoritativo: É a última parada na consulta do ervidor de nomes. Se o servidor de nomes autoritativo tiver acesso ao registro solicitado, ele retorna o endereço IP solicitado ao Recuror DNS. Pode er pensando como um livro na estante.

---

Um endereço IP é fornecido a cada dispositivo na Internet, quando um usuário deseja carregar uma página web, deve ocorrer uma tradução entre o que o usuário digita em seu browser e o endereço (site.com) copatível com a máquina necessário para localizar a página desejada. Para que isso aconteça, é necessário recursos de hardware que abastecem o DNS:

- Key Temrs
	- DNS Zone (database): netflix.com e todos seus registros
	- ZoneFiles: o arquivo que guarda a zone no disco.
	- Name Server (NS): Servidores DNS que hospedam uma ou mais zonas (armazenando um ou mais ZoneFiles).
- Name Server

---
## Espaço de Nomes

- O espaço de nomes do DNS é dividido em domínios, que são estruturas em níveis.
- O primeiro nível é estruturado em:
	- **Domínios genéricos**: informam o tipo de organização ao qual o domínio está vinculado.
	- **Domínio de países**: possuem uma entrada para cada país.
- A atribuição de nomes leva em consideração as fronteiras organizacionais, e não as redes físicas (a rede física não interessa para o DNS, domínios podem estar em diferente pontos geográficos).

- DNS Namespaces: Genéricos (empresas) ou Countries.
<img src="https://s.profissionaisti.com.br/wp-content/uploads/2018/04/mapa-2.png">

Ex.: domínio br (organizacional)
```
		br
		/ \
	gov    com
	/ \
 sus www
  |
 www
```

- Cada domínio tem seu nome definido pelo caminho entre ele e a raiz (www.sus.gov.br)
	- Domínio br é a raiz

- Cada domínio controla como são criados seus subdomínios
	- Não há restrições na quantidade de subdomínios que podem ser criados dentro de um domínio.

- O nome de domínio não faz distinção entre maiúsculas e minúsculas.

- O DNS é implementado sobre o protocolo [[12. Sockets|UDP]]
	- Cabe ao software DNS garantir a comunicação confiável (já que a comunicação via UDP não é confiável)
	- O servidor DNS aguarda por requisições na [[06. Endereço de Porta|porta]] 53.
		- A porta só fica "ocupada" no momento do [[12. Sockets|bind]] do socket.
		- O que existe na verdade é uma fila de entrada de solicitações.
		- O Socket é como um arquivo nesse sentido, em que as solicitações ficam armazenadas em fila.

---
## Resolução de nomes

- O espaço de nomes DNS é dividido em Zonas
	- As zonas são independentes e possuem um servidor de nomes principal **e pelo menos um servidor de nomes secundário** (em caso de queda ou balanceamento);
- Cada cliente executa um software conhecido como **solucionador** (resolver) - que faz parte do sistema operacional. 
	- Sempre que uma aplicação necessita resolver um nome, solicita que o solucionador local encontre o endereço associado ao nome.
	- O solucionador repassará a solicitação ao se servidor DNS.
	- Ele pode armazenar em cache nomes já visitados/solicitados.

- Os três principais componentes do DNS são:
	- Registro de recursos armazenados em um banco de dados distribuído.
	- Servidores de nomes DNS responsáveis pela manutenção de zonas específicas.
	- Solucionadores DNS em execução nos clientes.

### Como funciona?

- Quando o solucionador solicita a resolução de um nome ao servidor DNS pode acontecer:
	1. O servidor DNS é o responsável pela zona
		- O servidor resolve o nome solicitado e o devolve ao solucionador
	2. O servidor DNS não é o responsável pela zona, mas possui a resolução em cache
		- O servidor envia a resolução ao solucionador
	3. O servidor DNS não é o responsável pela zona e não possui a resolução em cache
		- O servidor necessitará realizar uma *busca para resolver o nome*

- Nenhum servidor DNS isolado tem todos os mapeamentos para todos os hospedeiros da Internet
- Os mapeamentos são distribuídos pelos servidores DNS

```                         
				    Servidor DNS raiz
					/       |         \
                 /        |          \
                /         |           \
    servidor DNS      serv. DNS       servidores DNS  
        com          organização           edu
   /         \            |              /     \
serv. DNS   serv. DNS     |      servid. DNS   servid. DNS
yahoo.com    amazon.com   |       poly.edu      umass.edu
                          |  
                          |
                    servidores DNS
	                    pbs.org  
```

---
### Consulta recursiva e consulta iterativa

- O DNS provê 2 tipos de consutlas:
	1. **Consulta recursiva** - quando a consulta retorna a resolução do nome de forma completa, sem retornar respostas parciais.
	2. **Consulta iterativa** - quando para obter uma resposta completa é necessário realizar uma série de iterações com servidores obtendo, várias respostas parciais.

![[Pasted image 20240310143951.png]]

1. Solucionador solicita a solução do domínio e o servidor resolve por completo
2. Solucionador solicita a solução do domínio e o servidor resolve em uma série de iterações até resolver.
	- Normalmente servidores DNS só permitem consultas recursivas á maquinas dentro de sua própria organização.
	- Se for uma máquina de fora da organização, a consulta será iterativa com o intuito de diminuir a carga do mesmo.

---
