# Aula 3 - Camada de Aplicação e Serviços
## Camada de Aplicação
- Aplicações clássica de textos (1970 e 1980)
	- Correio eletrônico (SMTP, POP3 e IMAP)
	- Acesso remoto a computadores (TELNET)
	- Transferência de Arquivo (FTP)
	- Grupos de discussão (NNTP)
- A World Wide Web (HTTP) alcançou sucecsso estrondoso em meados da década de 90
	- Sistemas de busca (Altavista, Google, Bing, Yahoo, etc.)

### Princípios da aplicação de rede
- Desenvolver aplicações de rede é escrever programas que:
	- Executem em sistemas finais diferentes e se comuniquem entre si
- Programas podem ser completamente diferente mas se comunicam pelo protocolo HTTP (arquitetura cliente servidor);
	- Também pode ser idênticos (ccomo algumas aplicações P2P - ex.: bitTorrent)
- Não é necessário programar nada nos nós intermediários, apenas nos dispositivo que se comunicam (finais).
- Camada de aplicação (fim a fim): uma aplicação fala direto com outra aplicação.

### Arquitetura de aplicação
- Projetada pelo programador e determina como a aplicação é organizada nos sitemas finais.
- As arquiteturas mais utilizadas são:
	- Cliente-servidor
	- P2P - ponta a ponta entre os proprios dispositivos

#### Cliente-servidor
- Há um hospedeiro - servidor - sempre em funcionamento, que atende requisições de outro hospedeiro - cliente.
	- os clientes não se comunicam diretamente uns com outros
	- servidor possui endereço fixo bem conhecido(normalmente)
- Muitas vezes, um único servidor não é capaz de atender todas as requisições
	- como solução, com frequencia um **datacenter** acomodando uma grande quantidade de hospedeiros é usado para criar um servidor virtual poderos.
- Um **datacenter** pode ter centanass de milhares de servidores
	- Precisam ser alimentados e precisam de interconexão (redundante e com alta largura de banda)

## Serviços de Transporte providos pela internet
#### UDP vs TCP

| Aplicação                    | Protocolo de Camada de Aplicação              | Protocolo de Transporte subjacente |
| ---------------------------- | --------------------------------------------- | ---------------------------------- |
| Correio eletrônico           | SMTP                                          | TCP                                |
| Acesso a terminal remoto     | Telnet                                        | TCP                                |
| Web                          | HTTP                                          | TCP                                |
| Transferência de Arquivos    | FTP                                           | TCP                                |
| Multimídia em fluxo contínuo | HTTP (por exemplo, YouTube)                   | TCP                                |
| Telefonia por internet       | SIP, RTP, ou proprietaria (por exemplo Skype) | UDP ou TCP                         |
- Vídeos pré armazenados: TCP
- Vídeo em tempo real: UDP

> RFC INDEX
> - https://www.rfc-editor.org/rfc-index.html

## Serviço DNS (Domain Name System)
- Domain Name System: [[DNS]]
	- Nome -> IP 
	- IP -> nome (reverso)
	- Implementa [[12. Sockets|UDP]]

## Serviço WEB
- Permite acesso a documentos vinculados e espalhados por milhares de hospedeiros na internet.
### Breve história
- Nasceu na CERN, da necessidade de compartilhar relatórios cinetíficos, plantas, desenhosm fotos e outros documentos.
- 1991: modo texto apresentado na Hypertext'91
- Primeira interface grafic em 1993: Mosaic

### Página WEB
- Documento - contruído por objetos:
	- Um objeto pode ser:
		- Arquivo HTML
		- Imagem
		- applet Java
		- Vídeo
		- etc.
- A maioria das página web são cosntruídas em um arquivo base HTML e diversos objetos referenciados.
#### URL: Uniform Resource Locator
- O arquivo-base HTML referencia outros objetos na página com os **URLs** (*Uniform Resource Locator* - Localizador Uniforme de Recursos) dos objetos

- Cada URL tem dois componentes:
	- *nome de hospedeiro* que abriga o objeto
	- *nome do caminho* do objeto
- ex.: http://www.gnu.org/graphics/heckert_gnu.transp.small.png
	- nome do hospedeiro: `www.gnu.org`
		- não faz diferença entre maiusculo e minusculo
	- nome do caminho `/graphics/heckert_gnu.transp.small.png`
		- depende da maquina (windows ou unix)
			- windows: faz diferença ser maiuscula/minuscula
			- unix: não faz diferença ser maiuscula/minuscula

### HTTP - HyperText Transfer Protocol
- Protocolo de Transferência de Hipertexto
	- É definido no RFC 1945 e no RF 2616

- É executado tanto no cliente quanto no servidor
- *Define a estrutura das mensagens trocadas e o modo como o cliente e o servidor trocam as mensagens*.

- Quando um usuário requisita uma pagina web, o *navegador* envia ao servidor **mensagens de requisição HTTP** para os objetos da página.
- O servidor recebe as requisições e responde com **mensagens de resposta HTTp** que contém os objetos.

- O HTTP **usa o TCP** como seu protocolo de transporte subjacente
- O Cliente HTTP inicia uma conexão TCP com o servidor
	- Os processos do navegador e do servidor acessam o TCP por meio de suas interfaces de [[12. Sockets|Sockets]].

> Como usa o TCP, o HTTP não precisa se preocupar com dados perdidos ou detalhes de como se recuperar da perda de dados ou os reordenar.

<img src="https://miro.medium.com/v2/resize:fit:697/1*fPLxbzS5zdJlKrzxf4VVXQ.png">

- O servidor envia ao cliente os arquivos solicitados sem armazenar qualquer informação de estado sobre o cliente
	- O HTTP é denominado um protocolo **sem estado** 
	- Simplifica o projeto;
	- Permite manipular milhares de conexões simultâneamente;
	
> [!NOTE] HTTP: protocolo sem Estado
> Não guarda um registro das ações do cliente que trará condição de resposta - ele não armazena estado e não se importa com estados do cliente; Cada pergunta é individual - não interessa se é a primeira pergunta, etc. O servidor web vai interagir como se fosse sempre a primeira pergunta

- A web usa **arquitetura de aplicação Cliente-Servidor**
	- Um servidor Web estará sempre funcionando, tem um endereço IP fixo, trabalha sempre na mesma porte (porta 80), e pode atender requisições de milhões de navegadores diferentes.

- O programador da aplicação precisa tomar uma importante decisão:
	- *Cada par de requisição/resposta deve ser enviado por uma conexão TCP distinta?*
		- ==Conexões não persistentes==
	- *Ou todas as requisições e suas respostas devem ser enviadas por uma mesma conexão TCP?*
		- ==Conexão persistente== (menos carga no servidor) - modo padrão

#### Formato da Mensagem HTTP
- Exemplo de Mensagem de Requisição HTTP:

- A primeira linha é a linha de requisição
	- Possui 3 campos: 
		- Método
		- URL
		- HTTP
- As linhas subsequente são de cabeçalho


```HTTP
GET /software/software.html HTTP/1.1 
Host: www.gnu.org
Connection: close
User-agent: Mozilla/5.0 
Accept-language: pt-BR

```

- A mensagem de requisição é escrita em texto ASCII comum.
- No excemplo é constituída de cinco linhas, cada qual seguida de um *carriage return e *line feed*.
- A última linha é seguida de um comando adicional de *carriage return* e *line feed*

#### Métodos HTTP
| Método | Descrição                                                       |
| ------ | --------------------------------------------------------------- |
| GET    | Solicita a leitura de uma página/objeto Web                     |
| HEAD   | Solicita a leitura de um cabeçalho de página/objeto Web         |
| PUT    | Solicita o armazenamento de uma página/objeto da Web            |
| POST   | Acrescenta a um recurso (por exemplo, uma página Web ou objeto) |
| DELETE | Remove a página/objeto da Web                                   |
| LINK   | Conecta dois recursos existentes                                |
| UNLINK | Desfaz uma conexão entre dois recursos                          |
