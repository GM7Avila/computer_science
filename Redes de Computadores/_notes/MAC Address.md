# Endereço MAC

## Media Access Control (MAC)

- Também conhecindo como endereço físico, é um atributo alfanumérico exclusivo de 12 caracteres usados para identificar dispositivos eletrônicos **individuais** na rede.

- O número MAC é uma sequência de 12 dígitos hexadecimais, representado por *seis pares de números hexadecimais* separados por dois pontos, exemplo: `01:23:45:67:89:ab`
	- Os 6 primeiros dígitos são o *código OUI* (Identificador Exclusivo Organizacional) ou *company_id* - que identifica o fornecedor e/ou o frabricando desse produto.
	- --->  [[https://www.wireshark.org/tools/oui-lookup.html|Wireshark - OUI Lookup Tool]]
	- A segunda metade é o NIC (Network Interface Controller) específico, atribuído pelo fornecedor a cada dispositivo.



> **NOTA** - Se um dispositivo está conectado usando uma porta ethernet e uma porta wriless, então ele terá pelo menos dois endereços macs.
> 
> Ex.:
> --> Wriless Adapter: `ee:ee:ee:ee:ee:e1`
> --> Ethernet Adapter: `ee:ee:ee:ee:ee:e1`


