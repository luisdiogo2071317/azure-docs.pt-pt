---
title: "Considerações sobre planeamento para a pilha do Azure integrado sistemas | Microsoft Docs"
description: "Saiba o que pode fazer agora a planear e preparar para a pilha do Azure de vários nós."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: cd14f0e5259e5c0b6cbf11790bbdf08164267ffa
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Considerações sobre planeamento para a pilha do Azure integrado sistemas
Se estiver interessado num sistema pilha do Azure integrado, deve compreender algumas das principais considerações sobre planeamento em torno da implementação e como o sistema se enquadra no seu centro de dados. Este artigo fornece uma descrição geral de alto nível destas considerações para o ajudar a tomar decisões importantes infraestrutura para o seu sistema de vários nós de pilha do Azure. Uma compreensão destas considerações ajuda-o ao trabalhar com o fornecedor do hardware OEM à medida que implementam a pilha do Azure para o seu centro de dados.  

> [!NOTE]
> Pilha do Azure só podem ser adquiridos com vários nós sistemas de fornecedores de hardware autorizado. 

Para implementar o não existe pilha do Azure são um conjunto das decisões que necessita para se integrar corretamente pilha do Azure com o seu ambiente. Terá de fornecer estas informações para o fornecedor de solução durante o processo de planeamento e estar pronto para o fornecedor de hardware antes de começa a implementação ajudar o processo de aceder de forma rápida e facilmente.

As informações necessárias intervalos entre redes, segurança e informações de identidade com muitas decisões importantes que poderá exigir o conhecimento de várias áreas diferentes e os decisores. Por conseguinte, poderá ter de obter pessoas de várias equipas na sua organização para se certificar de que tem informações todos requeridas prontas antes do início da implementação. Pode ajudar a comunicar com o fornecedor de hardware ao recolher estas informações, como podem ter conselhos útil para tomar as decisões.

Ao pesquisar e recolher as informações necessárias, poderá ter de efetuar algumas alterações de configuração de pré-implementação ao seu ambiente de rede. Isto pode incluir reservar espaços de endereços IP para a solução de pilha do Azure e configurar os routers, comutadores e as firewalls para se preparar para a conectividade com as nova solução comutadores da pilha do Azure. Certifique-se ter especialista de área do requerente lined cópias de segurança e envolvidos em todo o projeto de implementação para o ajudar a planear.

## <a name="management-considerations"></a>Considerações sobre gestão
Pilha do Azure é um sistema selado, em que a infraestrutura é bloqueada de um permissões e a perspetiva de rede. Listas de controlo de acesso (ACL) de rede são aplicadas para bloquear todo o tráfego de entrada não autorizado e todas as comunicações desnecessárias entre os componentes de infraestrutura. Isto torna difícil para os utilizadores não autorizados a aceder ao sistema.

Para operações e gestão diária, há sem acesso de administrador sem restrições para a infraestrutura. Os operadores do Azure da pilha tem de gerir o sistema através do portal de administrador ou através do Azure Resource Manager (através da CLI do Azure, PowerShell ou a API REST). Não há nenhum acesso ao sistema por outras ferramentas de gestão, tais como o Gestor de Hyper-V ou o Gestor de clusters de ativação pós-falha. Para ajudar a proteger o sistema, o software de terceiros (por exemplo, agentes) não pode ser instalado no interior de componentes da infraestrutura de pilha do Azure. Interoperabilidade com o software de segurança e de gestão externo ocorre através da CLI do Azure, PowerShell ou a API REST.

Quando um nível mais elevado de acesso necessária para a resolução de problemas que não estão resolvidos passos mediation alerta, tem de trabalhar com o suporte. Através do suporte, há um método para fornecer acesso de administrador total temporário para o sistema para executar operações mais avançadas. 

## <a name="identity-considerations"></a>Considerações de identidade

### <a name="choose-identity-provider"></a>Escolha o fornecedor de identidade
Terá de considerar o fornecedor de identidade que pretende utilizar para a implementação de pilha do Azure, Azure AD ou AD FS. Não é possível comutar a fornecedores de identidade após a implementação sem reimplementação completa do sistema.

A opção do fornecedor de identidade não tem efeito em máquinas virtuais de inquilino, o sistema de identidade e contas utilizarem, se possam juntar-se um domínio do Active Directory, etc. Este é separada.

Pode saber mais sobre como escolher um fornecedor de identidade no [decisões de implementação para o artigo de sistemas integrada de pilha do Azure](.\azure-stack-deployment-decisions.md).

### <a name="ad-fs-and-graph-integration"></a>Integração do AD FS e o gráfico
Se optar por implementar pilha do Azure através do AD FS como o fornecedor de identidade, tem de integrar a instância do AD FS na pilha do Azure com uma instância existente do AD FS através de uma fidedignidade de Federação. Isto permite que as identidades na floresta do Active Directory existente para autenticar com recursos na pilha do Azure.

Também pode integrar o serviço do gráfico na pilha do Azure com o Active Directory existente. Isto permite-lhe gerir baseada em funções controlo de acesso (RBAC) na pilha do Azure. Quando o acesso a um recurso é delegado, a conta de utilizador na floresta do Active Directory existente, utilizando o protocolo LDAP procura o componente de gráfico.

O diagrama seguinte mostra o fluxo de tráfego do AD FS e do Graph integrado.
![Diagrama que mostra o fluxo de tráfego do AD FS e o gráfico](media/azure-stack-deployment-planning/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Modelo de licenciamento

Tem de decidir o modelo de licenciamento que pretende utilizar. Para uma implementação ligada, pode escolher pay-como-utiliza ou com base na capacidade de licenciamento. Pay-como-utiliza requer uma ligação para o Azure para reportar a utilização, que, em seguida, é faturada através de comércio do Azure. Apenas capacidade com base no licenciamento é suportada se implementar desligado da internet. Para obter mais informações sobre os modelos de licenciamento, consulte [Microsoft Azure pilha empacotamento e preços](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Decisões de nomenclatura

Tem de pensar sobre a forma como pretende planear o seu espaço de nomes pilha do Azure, especialmente o nome de região e nome de domínio externo. O nome de domínio completamente qualificado (FQDN) da sua implementação de pilha do Azure para pontos finais de destinado ao público é a combinação destes dois nomes, &lt; *região*&gt;&lt;*external_FQDN*  &gt;, por exemplo, *east.cloud.fabrikam.com*. Neste exemplo, os portais de pilha do Azure seria disponíveis nos seguintes URLs:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

A tabela seguinte resume estas decisões de nomenclatura de domínio.

| Nome | Descrição | 
| -------- | ------------- | 
|Nome da região | O nome da sua região do Azure pilha primeiro. Este nome é utilizado como parte do FQDN para os públicos endereços IP virtuais (VIPs) que gere a pilha do Azure. Normalmente, o nome da região é um identificador de localização física, tais como uma localização de centro de dados. | 
| Nome de domínio externo | O nome da zona de sistema de nomes de domínio (DNS) para pontos finais com acesso externo VIPs. Utilizado no FQDN para estes VIPs públicos. | 
| Nome de domínio (interno) privada | O nome do domínio (e zona DNS interna) criado na pilha do Azure para a gestão de infraestrutura. 
| | |

## <a name="certificate-requirements"></a>Requisitos de certificado

Para a implementação, terá de fornecer certificados de Secure Sockets Layer (SSL) para pontos finais de destinado ao público. Um nível elevado, certificados têm os seguintes requisitos:

- Pode utilizar um certificado de caráter universal único ou pode utilizar um conjunto de certificados dedicados e utilizar carateres universais apenas para pontos finais, como o armazenamento e o Cofre de chaves.
- Os certificados podem ser emitidos por uma autoridade de certificação fidedigna pública (AC) ou uma AC gerida pelo cliente.

Para obter mais informações sobre o PKI os certificados são necessários para implementar a pilha do Azure e como obtê-las, consulte, [os requisitos de certificados de infraestrutura de chave pública do Azure pilha](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> As informações do certificado PKI fornecidas devem ser utilizadas como orientação geral. Antes de adquirir todos os certificados PKI para a pilha do Azure, trabalhar com o seu parceiro de hardware do OEM. Os utilizadores deverão fornecer orientações de certificado e os requisitos mais detalhadas.



## <a name="time-synchronization"></a>Sincronização de hora
Tem de escolher uma hora específica servidor com é utilizado para sincronizar a pilha do Azure.  Sincronização de hora é fundamental para a pilha do Azure e as respetivas funções de infraestrutura, como é utilizado para gerar pedidos de Kerberos que são utilizados para autenticar serviços internos entre si.

Tem de especificar um IP para o servidor de sincronização de hora, como embora a maioria dos componentes na infraestrutura pode resolver um URL, algumas podem apenas suportar a endereços IP. Se estiver estão a utilizar a opção de implementação de desligado, tem de especificar um servidor de tempo na sua rede empresarial que se pode ser contactado a partir da rede de infraestrutura na pilha do Azure. Esta operação pode requerer consideração adicional durante o planeamento da parte da integração de rede do projeto de implementação.


## <a name="network-connectivity"></a>Conectividade de rede
Esta secção fornece informações de infraestrutura de rede de pilha do Azure que irão ajudar a tomar decisões sobre como melhor integram pilha do Azure no seu ambiente de rede existente importantes. 

> [!NOTE]
> Para resolver nomes DNS externos da pilha do Azure (por exemplo, www.bing.com), terá de fornecer os servidores DNS que pilha do Azure pode utilizar para reencaminhar pedidos DNS para o qual não é autoritativa pilha do Azure. Para obter mais informações sobre o DNS de pilha do Azure requisitos, consulte [integração de centro de dados de pilha do Azure - DNS](azure-stack-integrate-dns.md).

### <a name="physical-network-design"></a>Design de rede física
A solução de pilha do Azure requer uma infraestrutura física resiliente e de elevada para suportar a operação e serviços. Segue-se um diagrama do nosso estrutura recomendada:

![Estrutura recomendada de rede de pilha do Azure](media/azure-stack-deployment-planning/recommended-design.png)


### <a name="logical-networks"></a>Redes lógicas
Redes lógicas representam uma abstração das infraestruturas de rede física subjacente. São utilizados para organizar e simplificar as atribuições de rede para anfitriões, máquinas virtuais e serviços. Como parte da criação de rede lógica, os sites de rede são criados para definir as redes locais virtuais (VLANs), sub-redes IP e os pares sub-rede/VLAN do IP estão associados com a rede lógica em cada localização física.
A infraestrutura de rede para a pilha do Azure utiliza as seguintes redes lógicas que estão configuradas nos comutadores:

### <a name="network-infrastructure"></a>Infraestrutura de rede
A infraestrutura de rede para a pilha do Azure é composta por várias redes lógicas que estão configuradas nos comutadores. O diagrama seguinte mostra estas redes lógicas e a forma integrar com o top-of-rack (TOR), controlador de gestão de placa base (BMC) e limite comutadores (rede de cliente).

![Ligações de diagrama e o comutador de rede lógica](media/azure-stack-deployment-planning/NetworkDiagram.png)

#### <a name="bmc-network"></a>Rede BMC
Esta rede se encontra dedicada para ligar todos os os controladores bmc (também conhecido como serviço processadores, por exemplo, iDRAC Integrated Lights-out, iBMC, etc.) para a rede de gestão. Se estiver presente, o anfitrião de ciclo de vida de hardware (HLH) estarão localizado nesta rede e pode fornecer software específico do OEM para manutenção de hardware e/ou monitorizar. 

#### <a name="private-network"></a>Rede privada
Este /24 (anfitrião 254 IPs) rede é privado para a região de pilha do Azure (expande para além dos dispositivos de comutador do limite da região de pilha do Azure) e está dividido em duas sub-redes:

- **Rede de armazenamento**. Um /25 (anfitrião 126 IPs) utilizado para suportar a utilização de espaços de armazenamento direto (S2D) e tráfego de armazenamento de bloco de mensagem de servidor (SMB) e migração em direto da máquina virtual de rede. 
- **Rede IP virtual interna**. A/25 VIPs dedicados para meramente internos para o Balanceador de carga de software (SLB) de rede.

#### <a name="azure-stack-infrastructure-network"></a>Rede de infraestrutura de pilha do Azure
Isto/24 rede se encontra dedicado para componentes internos de pilha do Azure para que estes possam comunicar e trocar dados entre si próprios. Esta sub-rede necessita de endereços IP encaminháveis, mas é mantida privada para a solução através de listas de controlo de acesso (ACL).  Não é esperado para ser encaminhados para além dos comutadores de limite, exceto para um intervalo muito pequeno equivalente num tamanho de ao/27 rede, o que é utilizado por alguns destes serviços quando necessitem de acesso a recursos externos e/ou da internet. 

#### <a name="public-infrastructure-network"></a>Rede de infraestrutura públicas
Isto/27 rede é o intervalo muito pequeno da sub-rede de infraestrutura do Azure pilha mencionado anteriormente, não necessita de endereços IP públicos, mas requerem acesso à internet através de um NAT ou um Proxy transparente. Esta rede será atribuída para o sistema de consola de recuperação de emergência (ERCS). A VM ERCS requer acesso à internet durante o registo para o Azure e devem ser encaminhável para a sua rede de gestão para fins de resolução de problemas.

#### <a name="public-vip-network"></a>Rede de VIP pública
A rede de VIP público é atribuída ao controlador de rede na pilha do Azure. Não é uma rede lógica no comutador. O SLB utiliza o conjunto de endereços e atribui/32 redes para cargas de trabalho inquilinas. Na tabela de encaminhamento de comutador, estes IPs 32 estão anunciados como uma rota disponível através do BGP. Esta rede contém os endereços IP externo acessível ou públicos. A infraestrutura de pilha do Azure utiliza, pelo menos, 8 endereços desta rede VIP público enquanto o resto é utilizado pelas VMs inquilino. O tamanho da rede nesta sub-rede variam entre um mínimo de /26 (64 anfitriões) para um máximo de /22 (1022 anfitriões), recomendamos que planeie um /24 rede.

#### <a name="switch-infrastructure-network"></a>Rede de infraestrutura do comutador
Isto/26 rede é a sub-rede que contenha sub-redes/30 IP encaminhável-to-Point (anfitrião 2 IPs) e loopbacks que estão dedicados/32 sub-redes para a banda no comutador gestão e o ID de router BGP. Este intervalo de endereços IP tem de ser encaminhável externamente da solução pilha do Azure para o seu centro de dados e podem ser IPs privado ou público. Por exemplo, dentro de um cenário de fornecedor de serviços tenanted de múltiplos que IPs públicos poderão ser necessários, enquanto dentro de uma implementação empresarial controlada de forma Apertada, IPs privados podem ser o preferencial.

#### <a name="switch-management-network"></a>Rede de gestão do comutador
Este /29 (anfitrião de 6 IPs) rede se encontra dedicada para ligar as portas de gestão dos parâmetros. Permitir o acesso fora de banda para a implementação, gestão e resolução de problemas. Esta é calculada a partir da rede de infraestrutura comutador mencionada acima.

### <a name="transparent-proxy"></a>PROXY TRANSPARENTE
A solução de pilha do Azure não suporta proxies normal web. Se o Centro de dados requer que todo o tráfego para utilizar um proxy, tem de configurar um proxy transparente ao processar todo o tráfego de bastidor para processá-lo, de acordo com a política, separar o tráfego entre as zonas na sua rede. Um proxy transparente (também conhecido como um intercetar, inline ou proxy forçada) interceta a comunicação normal na camada de rede sem necessidade de qualquer configuração de cliente especiais. Os clientes não tem de ser em consideração a existência de proxy.

### <a name="publish-azure-stack-services"></a>Publicar os serviços de pilha do Azure

Terá de disponibilizar os serviços do Azure pilha aos utilizadores da pilha fora do Azure. Pilha do Azure configura vários pontos finais para as respetivas funções de infraestrutura. Estes pontos finais são atribuídos VIPs do conjunto de endereço IP público. Uma entrada DNS é criada para cada ponto final na zona DNS externa, que foi especificada no momento da implementação. Por exemplo, o portal de utilizador está atribuído a entrada de anfitrião DNS do portal.  *&lt;região >.&lt; FQDN >*.

#### <a name="ports-and-urls"></a>URLs e portas

Para tornar os serviços de pilha do Azure (por exemplo, portais do Azure Resource Manager, DNS, etc.) disponível para redes externas, tem de permitir tráfego de entrada para estes pontos finais para URLs, portas e protocolos específicos.
 
Numa implementação em que um uplink de proxy transparentes para um servidor proxy tradicionais, tem de permitir URLs e portas específicas para comunicação de saída. Estes incluem portas e URLs para identidade, sindicação do marketplace, patch e atualização, registo e dados de utilização.

Para obter mais informações, consulte:
- [Protocolos e portas de entrada](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound)
- [URLs e portas de saída](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound)

### <a name="connect-azure-stack-to-azure"></a>Ligar a pilha do Azure para o Azure

Para cenários de nuvem híbrida, terá de planear a forma como pretende ligar pilha do Azure para o Azure. Existem dois métodos suportados para ligar redes virtuais na pilha do Azure para redes virtuais no Azure: 
- **Site-site**. Uma ligação de rede privada virtual (VPN) através de IPsec (IKE v1 e IKE v2). Este tipo de ligação requer um dispositivo VPN ou de encaminhamento e serviço de acesso remoto (RRAS). Para obter mais informações sobre gateways de VPN no Azure, consulte [sobre o Gateway de VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). A comunicação por este túnel é encriptada e é segura. No entanto, a largura de banda é limitada pelo débito máximo do túnel (100-200 Mbps).
- **NAT de saída**. Por predefinição, todas as máquinas virtuais na pilha do Azure irá ter conetividade para redes externas através de NAT de saída. Cada rede virtual que é criado na pilha do Azure obtém um endereço IP público atribuído. Se a máquina virtual diretamente é atribuída um endereço IP público, ou se encontrar atrás de um balanceador de carga com um endereço IP público, terá acesso de saída através de NAT de saída com o VIP da rede virtual. Isto funciona apenas para comunicação que é iniciada pela máquina virtual e destinada a redes externas (internet ou da intranet). Não pode ser utilizado para comunicar com a máquina virtual a partir de fora.

#### <a name="hybrid-connectivity-options"></a>Opções de conectividade híbrida

Para conectividade híbrida, é importante considerar o tipo de implementação que pretende oferecer e onde irá ser implementada. Terá de considerar a necessidade isolar o tráfego de rede por inquilino e se tem uma implementação de intranet ou à internet.

- **Pilha do Azure de inquilino único**. Uma implementação de pilha do Azure que procura, pelo menos de uma perspetiva de rede, como se fosse um inquilino. Podem existir que várias subscrições de inquilino, mas como qualquer serviço de intranet, todo o tráfego percorre as mesmas redes. Tráfego de rede a partir de uma subscrição percorre a mesma ligação de rede como outra subscrição e não tem de ser isolada através de um túnel encriptado.

- **Pilha do Azure multi-inquilino**. Uma implementação de pilha do Azure onde o tráfego de cada subscrição de inquilino que está vinculado a redes externas ao Azure pilha deve ser isolado do tráfego de rede dos outros inquilinos.
 
- **Implementação de intranet**. Uma implementação de pilha do Azure que se encontre numa intranet empresarial, normalmente, num espaço de endereços IP privados e atrás de firewalls de um ou mais. Os endereços IP públicos não são verdadeiramente públicos, como não pode ser encaminhados diretamente através da internet pública.

- **Implementação de Internet**. Uma implementação de pilha do Azure que está ligada ao público os endereços IP público encaminháveis para a internet internet e utiliza para o intervalo de VIP público. A implementação ainda pode manter-se protegido por uma firewall, mas o intervalo de VIP público é diretamente acessível a partir da internet e o Azure público.
 
A tabela seguinte resume os cenários de conectividade híbrida, com o profissionais de TI, contras e casos de utilização.

| Cenário | Método de conectividade | Profissionais de TI | Contras | Boa para |
| -- | -- | --| -- | --|
| Único inquilino do Azure pilha, implementação de intranet | NAT de saída | Uma melhor largura de banda para transferências mais rápidas. Simples de implementar; Não existem gateways necessários. | Tráfego não encriptado; sem isolamento ou a encriptação para além de TOR. | Implementações em empresas onde todos os inquilinos são igualmente fidedignos.<br><br>Empresas que têm um circuito ExpressRoute do Azure para o Azure. |
| Pilha de Azure multi-inquilino, implementação de intranet | VPN de site a site | Tráfego de inquilino VNet para o destino é seguro. | Largura de banda é limitada pelo túnel VPN de site para site.<br><br>Necessita de um gateway de rede virtual e um dispositivo VPN na rede de destino. | Implementações em empresas onde algumas tráfego de inquilino tem de estar protegidas de outros inquilinos. |
| Único inquilino do Azure pilha, implementação de internet | NAT de saída | Uma melhor largura de banda para transferências mais rápidas. | Tráfego não encriptado; sem isolamento ou a encriptação para além de TOR. | Alojamento de cenários em que o inquilino obtém as seus próprios implementação da pilha do Azure e um circuito dedicado para o ambiente de pilha do Azure. Por exemplo, ExpressRoute e de mudança de etiqueta Multiprotocol (MPLS).
| Pilha de Azure multi-inquilino, implementação de internet | VPN de site a site | Tráfego de inquilino VNet para o destino é seguro. | Largura de banda é limitada pelo túnel VPN de site para site.<br><br>Necessita de um gateway de rede virtual e um dispositivo VPN na rede de destino. | Cenários em que pretende oferecem uma nuvem de multi-inquilino o fornecedor de alojamento, em que os inquilinos não confiam entre si e o tráfego tem de estar encriptado.
|  |  |  |  |  |

#### <a name="using-expressroute"></a>Com o ExpressRoute

Pode ligar a pilha do Azure ao Azure através de [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para intranet de inquilino único e cenários de multi-inquilinos. Irá precisar de um circuito ExpressRoute aprovisionado através de [um fornecedor de conectividade](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

O diagrama seguinte mostra o ExpressRoute para um cenário de inquilino único (em que "Ligação de cliente" é o circuito de ExpressRoute).

![Cenário de ExpressRoute único inquilino do diagrama que mostra](media/azure-stack-deployment-planning/ExpressRouteSingleTenant.PNG)

O diagrama seguinte mostra o ExpressRoute para um cenário de multi-inquilino.

![Cenário de ExpressRoute do diagrama que mostra multi-inquilino](media/azure-stack-deployment-planning/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Monitorização externo
Para obter uma vista única de todos os alertas de implementação da pilha do Azure e dispositivos e integrar alertas IT service gestão fluxos de trabalho existentes para emissão de permissões, pode integrar pilha do Azure com o Centro de dados externo soluções de monitorização.

Incluído com a solução de pilha do Azure, o anfitrião de ciclo de vida de hardware (HLH) é um computador fora da pilha do Azure que executa as ferramentas de gestão do fornecedor fornecidos pelo OEM para hardware. Pode utilizar estas ferramentas ou outras soluções que diretamente integram com soluções de monitorização existentes no seu centro de dados.

A tabela seguinte resume a lista de opções disponíveis atualmente.

| Área | Solução de monitorização externa |
| -- | -- |
| Software de pilha do Azure | - [Pacote de gestão de pilha do Azure para o Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>- [Plug-in da Nagios](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>-Baseado em REST chamadas de API | 
| Servidores físicos (BMCs através de IPMI) | -Pacote de gestão do fornecedor do operations Manager<br>-Solução de fornecido de fornecedor de hardware do OEM<br>-Fornecedor de hardware Nagios plug-ins | OEM suportado parceiro (incluída) de solução de monitorização | 
| Dispositivos de rede (SNMP) | -Deteção de dispositivos de rede do operations Manager<br>-Solução de fornecido de fornecedor de hardware do OEM<br>-Comutador da Nagios Plug-in |
| Monitorização de estado de funcionamento de subscrição de inquilino | - [Pacote de gestão do System Center para o Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Tenha em atenção os seguintes requisitos:
- A solução que utiliza tem de ser sem agente. Não é possível instalar agentes de terceiros dentro de componentes de pilha do Azure. 
- Se pretender utilizar o System Center Operations Manager, necessita do Operations Manager 2012 R2 ou o Operations Manager 2016.

## <a name="backup-and-disaster-recovery"></a>Cópia de segurança e recuperação após desastre

Planear a cópia de segurança e recuperação após desastre envolve a planear para ambos os Azure pilha infraestrutura subjacente que aloja máquinas virtuais de IaaS e PaaS serviços e para aplicações de inquilino e os dados. Terá de planear estas separadamente.

### <a name="protect-infrastructure-components"></a>Proteger os componentes de infraestrutura

Pilha do Azure efetua cópias de segurança de componentes de infraestrutura a uma partilha que especificar.

- Irá precisar de uma partilha de ficheiros SMB externa no servidor de ficheiros baseado no Windows existente ou um dispositivo de terceiros.
- Deve utilizar esta mesma partilha para a cópia de segurança de comutadores de rede e o anfitrião de ciclo de vida de hardware. O fornecedor do hardware OEM irão ajudá-lo a fornecer orientações para cópia de segurança e restauro destes componentes, como estes são externos à pilha do Azure. Está responsável pela execução de fluxos de trabalho de cópia de segurança com base na recomendação do fornecedor OEM.

Se ocorrer a perda catastrófica de dados, pode utilizar a cópia de segurança da infraestrutura para dados de implementação de reseed como recursos federados (em implementações desligados), implementação entradas e identificadores, contas de serviço, certificado de raiz AC, ofertas, e planos as subscrições, quotas, atribuições de política e a função RBAC e segredos do Cofre de chaves.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Proteger aplicações de inquilino em máquinas de virtuais do IaaS

Pilha do Azure não fazer uma cópia de segurança inquilino aplicações e dados. Tem de planear para proteção de recuperação após desastre e de cópia de segurança para um destino externo à pilha do Azure. Proteção de inquilino é uma atividade condicionada por inquilino. Para máquinas virtuais de IaaS, os inquilinos podem utilizar tecnologias no convidado para proteger ficheiros, dados de aplicação e pastas do Estado do sistema. No entanto, como um fornecedor de serviço ou enterprise, pode pretender oferecer uma solução de cópia de segurança e recuperação no mesmo datacenter ou externamente numa nuvem.

Para fazer uma cópia de segurança das máquinas virtuais de Linux ou IaaS do Windows, tem de utilizar produtos de cópia de segurança com acesso ao sistema operativo convidado para proteger o ficheiro, pasta, estado do sistema operativo e dados da aplicação. Pode utilizar a cópia de segurança do Azure, System Center Data Center Protection Manager, ou suportada produtos de terceiros.

Para replicar dados para uma localização secundária e orquestrar ativação pós-falha de aplicação, se ocorrer um desastre, pode utilizar o Azure Site Recovery ou suportados produtos de terceiros. (No lançamento inicial dos sistemas integrados, do Azure Site Recovery não suporta a reativação pós-falha. No entanto, pode conseguir reativação pós-falha através de um processo manual.) Além disso, as aplicações que suportem replicação nativa (como o Microsoft SQL Server) podem replicar dados para outra localização onde a aplicação está em execução.

> [!IMPORTANT]
> Na versão inicial dos sistemas integrados, estamos vai suportar tecnologias de proteção que funcionam ao nível do convidado da máquina virtual IaaS. Não é possível instalar os agentes em servidores de infraestrutura subjacente.

## <a name="next-steps"></a>Passos seguintes

- Para obter informações sobre casos de utilização, aquisição, parceiros e fornecedores de hardware do OEM, consulte o [Azure pilha](https://azure.microsoft.com/overview/azure-stack/) página de produto.
- Para obter informações sobre o plano e a georreplicação disponibilidade pilha do Azure integrados sistemas, consulte o documento técnico: [pilha do Azure: uma extensão do Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
