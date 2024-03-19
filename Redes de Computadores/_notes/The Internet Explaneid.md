# The internet explained
Um resumo pessoal do conteúdo dos seguintes artigos/sites:
- https://www.vox.com/2014/6/16/18076282/the-internet
- http://web.stanford.edu/class/msande91si/www-spr04/readings/week1/InternetWhitepaper.htm

## O que é a internet?
- Rede descentralizada de redes de computadores mundial;

## Onde está a internet?
- **Datacenters** - salas de servidores que arazenam os dados do usuário e hospedam aplicativos online. 
- **Backbone** - redes de longa distância - principalmente em cabos de fibra óptica, que transportam dados entre datacenters e consumidores. Os provedores de backbone frequentemente conectam suas redes em *pontos de troca de internet*, geralmente localizados em grandes cidades.
- **Last mile** - é a parte da internet que conecta residências e pequenas empresas à internet. Também inclui torres que de sinal de internet.

## Internet sem fio
- Redes Wi-Fi - espectro não licenciado: frequências eletromagnéticas que estão disponíveis para qualquer pessoa gratuitamente, com limites estritos de potência para evitar conflitos com redes vizinhas.
- Redes celulares - dividias em células com alcances determiandos geografiacmente. Cada célula possui uma torre em seu centro que fornece serviços aos dispositivos. A rede é transferida autoamticamente ao trocar de célula. Utilizam espectro licenciado de alta potência.

## Núvem
- Utilização da internet para acessar o arquivo na nuvem;
- Servidores de Data Center -> Google Drive, One Drive, Hotmail, etc.

## IPv4 e IPv6
- O IPv4, padrão atual da internet, permite cerca de 4 bilhões de endereços IP (hoje em dia esta quantidade de IP é considerada pequena e está quase esgotada);
- Assim os engenhieros da internet desenvolveram um novo padrão chamado IPv6, permitindo um número de 39 dígitos para IPs.

## Endereço IP
- Endereço de Protoclo da Internet (Internet Protocol) são os endereços dos computadores na rede.
- O departamento da ICANN conhecido como Internet Assigned Numbers Authority é responsável pela distribuição de endereçõs IP.

- Cada copmputador/aparelho conectado à internet dev possuis eu endereço exclusivo no formato nnn.nnn.nnn.nnn (ipv4), onde nnn deve ser um número de 0 a 255 (como explicado em [[04. Endereço IP]])
![[ruswp_diag1.gif]]
- Quando conectamos na internet por meio de um provedor de serviços de internet (ISP), geralmente é recebido um endereço de IP temporário durante a sessão de discagem.
- Caso nos conectamos a partir de uma LAN, o coputador pode ter um enedereço IP permanente ou pode obeter um endereço temporário de um servidor DHCP (Dynamic Host Configuration Protocol).

> **Check It Out - O Programa Ping**
> Se você estiver usando o Microsoft Windows ou um tipo de Unix e tiver uma conexão com a Internet, existe um programa útil para verificar se um computador na Internet está ativo. Chama-se **ping** , provavelmente devido ao som produzido por sistemas de sonar submarinos mais antigos. 1 Se estiver usando o Windows, inicie uma janela de prompt de comando. Se você estiver usando um tipo de Unix, acesse um prompt de comando. Digite ping www.yahoo.com. O programa ping enviará um 'ping' (na verdade, uma mensagem de solicitação de eco ICMP (Internet Control Message Protocol)) para o computador nomeado. O computador pingado responderá com uma resposta. O programa ping contará o tempo expirado até que a resposta volte (se ela voltar). Além disso, se você inserir um nome de domínio (ou seja, www.yahoo.com) em vez de um endereço IP, o ping resolverá o nome do domínio e exibirá o endereço IP do computador. Mais informações sobre nomes de domínio e resolução de endereço posteriormente.

## World Wide Web
- Publicações na internet - plataforma aberta;
- Suporta hiperlinks (navegação em documentos);
- Aplicativos baseados em Web (desenvolvimento web);
- *World Wide Web Consortium (W3C)* - organização oficial de padrões web;
- Broswer web (navegadores) - renderizadores de páginas web.
- Web != internet; Web é um serviço/aplicação da internet.

## SSL
- *Secure Sockets Layer*
- Família de tecnologias de criptografia responsável pela privacidade dos usuários da web.
- Indicado pelo cadeado na URL.
![[Pasted image 20221216174819.png]]

## DNS (Domain Name System)
- O DNS é o sistema de domínio dos websites (endereços referentes ao IP do site).
- Sistema distribuído e hierárquico (subdomínios, rotas);

### 1. Servidores raiz.
### 2. Servidores Autoritativos.
#### Domínios de primeiro Nível
- ccTLDs (.pt, .br) - *países*
- gTLDs (.com, .org, .info) - *genéricos*

Os *computadores dos usuários* utilizam um componente chamado *resolver*, que consultam o servidor denominado *recursivo*, geralmente disponibilizado pelo *provedor da internet*. 

Os servidores recursivos, consultam toda a árvore de *servidores autoritativos*, caso não haja no histórico do provedor,  ele busca os servidores raízes. Inicialmente os autoritativos ccTLDs. Caso não haja, le continua descendo as camadas até achar o endereço correto de ip e então salva no histórico para futuras buscas de domínio. 

## Pacotes
- Package é a unidade básica de informação;
- A informação é dividida em pacotes dirigíveis;
- Um pacote tem duas partes: 
	- Cabeçalho - contendo informações que ajudam o pacote chegar no destino, incluindo o comprimento do pacote, origem e destino.
	- Dados reais - um pacote pode conter até 64 kilobytes de dados (aproximadamente 20 páginas de texto simples).
- Caso um roteador de internet esteja congestionado, ele descarta os pacotes para melhorar seu desempenho.
- É responsabilidade do computador remetente detectar que um pacote não chegou ao seu destino e solicitar outra cópia.

## Pilhas e Pacotes de protocolo
- Protocolos geralmente são integrados ao *Sistema Operacional do computador* e são utilizados na comunicação de computadores na rede.
- A pilha de protocolos utilizada na rede é chamada de **TCP/IP** (devido aos dois principais protocolos de comunicação usados - Transmission Control Protocol/Internet Protocol)
![[Pasted image 20221216201313.png]]

- Exemplo: O computador de IP 1.2.3.4 deseja enviar um "Hello computer!" para o computador de IP 5.6.7.8?

![[ruswp_diag2.gif]]

1. Se a mensagem a ser enviada for longa, cada camada de pilha pela qual a mensagem passa pode dividi-la em pedaços menores de dados. Isso ocorre porque os dados enviados pela Internet (e pela maioria das redes de computadores) são enviados em *blocos gerenciáveis*. Na Internet, esses blocos de dados são conhecidos como **pacotes** .
2. Os pacotes passariam pela Camada de Aplicação e continuariam na camada TCP. A cada pacote é atribuído um **número de porta** . As portas serão explicadas posteriormente, mas basta dizer que muitos programas podem estar usando a pilha TCP/IP e enviando mensagens. Precisamos saber qual programa no computador de destino precisa receber a mensagem porque ele estará escutando em uma porta específica.
3. Depois de passar pela camada TCP, os pacotes seguem para a camada IP. É aqui que cada pacote recebe seu endereço de destino, 5.6.7.8.
4. Agora que nossos pacotes de mensagens têm um número de porta e um endereço IP, eles estão prontos para serem enviados pela Internet. A camada de hardware se encarrega de transformar nossos pacotes contendo o texto alfabético de nossa mensagem em sinais eletrônicos e transmiti-los pela linha telefônica.
5. Na outra ponta da linha telefônica, seu ISP tem uma conexão direta com a Internet. **O roteador** do ISP examina o endereço de destino em cada pacote e determina para onde enviá-lo. Freqüentemente, a próxima parada do pacote é outro roteador. Mais informações sobre roteadores e infraestrutura de Internet posteriormente.
6. Eventualmente, os pacotes chegam ao computador 5.6.7.8. Aqui, os pacotes começam na parte inferior da pilha TCP/IP do computador de destino e seguem para cima.
7. À medida que os pacotes sobem na pilha, todos os dados de roteamento adicionados pela pilha do computador remetente (como endereço IP e número da porta) são retirados dos pacotes.
8. Quando os dados atingem o topo da pilha, os pacotes foram reagrupados em sua forma original, "Hello computer 5.6.7.8!"





