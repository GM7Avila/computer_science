# Roteadores

> <span style="font-style: italic; font-size:20px; font-family: Georgia, serif; color: #7baf56">"O roteador recebe o ponto de rede do provedor e distribui o acesso à internet para os dispositivos da rede. Ele faz parte da camada 3 do modelo OSI.</span> 

## Definição
- Dispositivo que distribui o ponto de rede - **Camada 3 : Rede**.
- O roteador é o segundo dispositivo no fluxo da internet, vindo logo após o processo de modularização feito pelo [[Modem|modem]]. 
- Podemos usa-lo para conectar uma ou mais redes. 
	- Assim, dispositivos de redes diferentes conseguem se comunicar entre si através de um roteador.
	- Exemplo: um roteador conectando três redes internas em uma empresa:

<div class="container" style="display: flex; justify-content: center; align-items: center;"> <span style="display: flex; justify-content: center; align-items: center; max-width: 100%; height: auto;"> <img src="https://www.networkacademy.io/sites/default/files/inline-images/Enterprise%20LAN_1.png" alt="Modulação" style="max-width: 70%; height: auto;" /> </span> </div>

- Se um dispositivo da rede Office 1 tentar enviar uma mensagem para um dispositivo da rede Office 2 ele consegue através do roteador.
- O roteador consulta uma tabela interna das redes conectadas à sua interface - **tabela de roteamento**.

## Tabela de Roteamento

<div class="container" style="display: flex; justify-content: center; align-items: center;"> <span style="display: flex; justify-content: center; align-items: center; max-width: 100%; height: auto;"> <img src="https://ars.els-cdn.com/content/image/3-s2.0-B9781437778076100117-f11-06-9781437778076.jpg" alt="Modulação" style="max-width: 100%; height: auto;" /> </span> </div>

- Ao receber um pacote de uma das redes conectadas a ele, o roteador verifica o endereço IP do destinatário e consulta para ver se ele está na tabela, assim como verifica em qual porta ele está conectado (cabo) para poder encaminha-lo.

- Porém, ao receber um pacote com o destino para algum website por exemplo, ele usará a rota padrão, consultando o DNS pelo provedor. A rota padrão dos roteadores também constam nas tabelas.

## Roteadores em Redes Domésticas
- Em uma rede doméstica simples, geralmente temos apenas uma sub-rede, e o trabalho do roteador é apenas fornecer conectivdade  entre a rede local (LAN) doméstica a rede remota (WAN) - *internet gateway*.

## Outras funções do Roteador
- O roteador também executa outras funções, como:
	- Firewall 
	- NAT
	- DHCP