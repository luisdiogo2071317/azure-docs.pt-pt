### <a name="supportedclientos"></a>Que sistemas operativos cliente posso utilizar com a ligação Ponto a Site?

São suportados os seguintes sistemas operativos cliente:

* Windows 7 (32 e 64 bits)
* Windows Server 2008 R2 (apenas 64 bits)
* Windows 8 (32 e 64 bits)
* Windows 8.1 (32 e 64 bits)
* Windows Server 2012 (apenas 64 bits)
* Windows Server 2012 R2 (apenas 64 bits)
* Windows Server 2016 (apenas 64 bits)
* Windows 10
* Versão OSX 10.11 para Mac (El Capitan)
* Versão macOS 10.12 para Mac (Sierra)

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Quantos pontos finais de cliente VPN posso ter na minha configuração Ponto a Site?

Suportamos até 128 clientes VPN para conseguir ligar a uma rede virtual ao mesmo tempo.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Posso atravessar proxies e firewalls com a capacidade Ponto a Site?

O Azure suporta dois tipos de opções de VPN Ponto a site:

* Secure Socket Tunneling Protocol (SSTP). SSTP é uma solução baseada em SSL proprietária da Microsoft que consegue penetrar firewalls, uma vez que a maioria das firewalls abre a porta TCP que o SSL 443 utiliza.

* VPN IKEv2. VPN IKEv2 é uma solução VPN IPsec baseada em normas que utiliza a porta UDP 500 e 4500 e o protocolo IP número 50. As firewalls nem sempre abrem estas portas, pelo que existe a possibilidade de a VPN IKEv2 não conseguir atravessar proxies e firewalls.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Se reiniciar o computador cliente configurado para Ponto a Site, a VPN restabelece ligação automaticamente?

Por predefinição, o computador cliente não restabelece a ligação VPN automaticamente.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>A ligação Ponto a Site suporta o restabelecimento de ligação automático e DDNS nos clientes VPN?

Atualmente, o restabelecimento de ligação automático e DDNS não são suportados nas VPNs Ponto a Site.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Posso ter configurações Site a Site e Ponto a Site coexistentes na mesma rede virtual?

Sim. Para o modelo de implementação do Resource Manager, tem de ter um tipo de VPN RouteBased para o seu gateway. No modelo de implementação clássica, precisa de um gateway dinâmico. Não suportamos a ligação Ponto a Site para gateways de VPN de encaminhamento estático ou gateways de VPN PolicyBased.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Posso configurar um cliente Ponto a Site para ligar a várias redes virtuais ao mesmo tempo?

Não. Um cliente Ponto a Site só pode ligar a recursos na VNet em que resida o gateway de rede virtual.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Que débito posso esperar através das ligações Site a Site ou Ponto a Site?

É difícil manter o débito exato dos túneis VPN. O IPsec e SSTP são protocolos VPN extremamente encriptados. O débito também é limitado pela latência e pela largura de banda entre o local e a Internet. Para um Gateway de VPN com apenas ligações VPN Ponto a Site IKEv2, o débito total que pode esperar depende do SKU de Gateway. Para obter mais informações sobre o débito, veja [SKUs de Gateway](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp-andor-ikev2"></a>Posso utilizar qualquer cliente VPN de software para a ligação Ponto a Site que suporte SSTP e/ou IKEv2?

Não. Só pode utilizar o cliente VPN nativo no Windows para SSTP e o cliente VPN nativo no Mac para IKEv2. Veja a lista de sistemas operativos cliente suportados.

### <a name="does-azure-support-ikev2-vpn-with-windows"></a>O Azure suporta a VPN IKEv2 no Windows?

IKEv2 é suportado no Windows 10 e o servidor de 2016. No entanto, para poder utilizar IKEv2, tem de instalar as atualizações e definir um valor de chave de registo localmente. SO versões anteriores ao Windows 10 não são suportadas e só podem utilizar o SSTP.

Para preparar o Windows 10 ou Server 2016 para IKEv2:

1. Instale a atualização.

  | Versão do SO | Data | Número/de ligação |
  |---|---|---|---|
  | Windows Server 2016<br>Windows 10 versão 1607 | 17 de Janeiro de 2018 | [KB4057142](https://support.microsoft.com/help/4057142/windows-10-update-kb4057142) |
  | Versão do Windows 10 1703 | 17 de Janeiro de 2018 | [KB4057144](https://support.microsoft.com/help/4057144/windows-10-update-kb4057144) |
  |  |  |  |  |

2. Defina o valor de chave de registo. Criar ou definir chave REG_DWORD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\ IKEv2\DisableCertReqPayload" no registo para 1.

### <a name="what-happens-when-i-configure-both-sstp-and-ikev2-for-p2s-vpn-connections"></a>O que acontece quando configurar SSTP e IKEv2 para ligações P2S VPN?

Quando configura o SSTP e IKEv2 num ambiente misto (composto por dispositivos Windows e Mac), o cliente de VPN do Windows tentará sempre túnel IKEv2 pela primeira vez, mas irá reverter para SSTP se a ligação de IKEv2 não for bem sucedida. MacOSX só irá ligar através de IKEv2.

### <a name="other-than-windows-and-mac-which-other-platforms-does-azure-support-for-p2s-vpn"></a>Que outras plataformas não Windows e Mac suporta o Azure na VPN P2S?

O Azure suporta apenas Windows e Mac na VPN P2S.

### <a name="i-already-have-an-azure-vpn-gateway-deployed-can-i-enable-radius-andor-ikev2-vpn-on-it"></a>Já tenho um Gateway de VPN do Azure implementado. Posso ativar RADIUS e/ou IKEv2 VPN no mesmo?

Sim, pode ativar estas novas funcionalidades nos gateways já implementadas utilizando o Powershell ou o portal do Azure, desde que o SKU de gateway que estiver a utilizar suporta RADIUS e/ou IKEv2. Por exemplo, o SKU básico de gateway VPN não suporta RADIUS ou IKEv2.
