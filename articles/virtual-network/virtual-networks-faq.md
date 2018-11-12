---
title: FAQ da rede Virtual do Azure | Documentos da Microsoft
description: Respostas às perguntas mais frequentes sobre as redes virtuais do Microsoft Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 54bee086-a8a5-4312-9866-19a1fba913d0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: jdial
ms.openlocfilehash: 6c429931a7a17ab62892ecc774a5cca15a532f72
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51237639"
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Rede Virtual do Azure perguntas mais frequentes (FAQ)

## <a name="virtual-network-basics"></a>Noções básicas de rede virtual

### <a name="what-is-an-azure-virtual-network-vnet"></a>O que é uma rede Virtual do Azure (VNet)?
Uma rede Virtual do Azure (VNet) é uma representação da sua própria rede na cloud. É um isolamento lógico da nuvem do Azure dedicado à sua subscrição. Pode utilizar VNets para aprovisionar e gerir redes privadas virtuais (VPNs) no Azure e, opcionalmente, ligar as VNets outras Vnet no Azure ou com o ambiente infraestrutura de TI para criar soluções híbridas ou em vários locais. Cada VNet que criou tem seu próprio bloco CIDR e pode ser associado a outras redes de VNets e no local, desde que os blocos CIDR não se sobrepõem. Também tem controlo das definições de servidor DNS para VNets e a segmentação da VNet em sub-redes.

Utilize VNets para:

* Crie um dedicado privada apenas na cloud VNet, às vezes, não exigem uma configuração em vários locais para a sua solução. Quando cria uma VNet, seus serviços e VMs dentro da VNet podem comunicar diretamente e segura entre si na cloud. Ainda é possível configurar ligações de ponto final para as VMs e serviços que requerem comunicação da Internet, como parte da sua solução.
* Expanda seu centro de dados com VNets de forma segura, é possível criar o site a site (S2S) VPNs tradicionais para dimensionar a capacidade do seu datacenter com segurança. VPNs de S2S utilize IPSEC para fornecer uma ligação segura entre o gateway de VPN empresarial e o Azure.
* Cenários de nuvem híbrida ativar VNets dão-lhe a flexibilidade para suportar uma variedade de cenários de nuvem híbrida. Pode ligar com segurança os aplicativos baseados na nuvem para qualquer tipo de sistema no local, como mainframes e sistemas Unix.

### <a name="how-do-i-get-started"></a>Como posso começar?
Visite o [documentação da rede Virtual](https://docs.microsoft.com/azure/virtual-network/) para começar a utilizar. Este conteúdo fornece informações de descrição geral e implementação para todos os recursos da VNet.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Pode utilizar VNets sem conectividade em vários locais?
Sim. Pode utilizar uma VNet sem o ligar a local. Por exemplo, pode executar controladores de domínio do Active Directory do Microsoft Windows Server e farms do SharePoint unicamente numa VNet do Azure.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Posso executar a otimização de WAN entre uma VNet ou VNets e o meu Datacenter no local?
Sim. Pode implementar um [aplicação virtual de rede de otimização de WAN](https://azure.microsoft.com/marketplace/?term=wan+optimization) de vários fornecedores através do Azure Marketplace.

## <a name="configuration"></a>Configuração

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Que ferramentas posso utilizar para criar uma VNet?
Pode utilizar as seguintes ferramentas para criar ou configurar uma VNet:

* Portal do Azure
* PowerShell
* CLI do Azure
* Um ficheiro de configuração de rede (netcfg - para VNets clássicas apenas). Consulte a [configurar uma VNet com um ficheiro de configuração de rede](virtual-networks-using-network-configuration-file.md) artigo.

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Os intervalos de endereços pode usar nas minhas VNets?
Qualquer intervalo de endereços IP definidos na [RFC 1918](http://tools.ietf.org/html/rfc1918). Por exemplo, 10.0.0.0/16.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Pode ter endereços IP públicos nas minhas VNets?
Sim. Para obter mais informações sobre intervalos de endereços IP públicos, consulte [criar uma rede virtual](manage-virtual-network.md#create-a-virtual-network). Endereços IP públicos não são diretamente acessíveis a partir da internet.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>Existe um limite para o número de sub-redes na minha VNet?
Sim. Ver [limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) para obter detalhes. Espaços de endereços de sub-rede não podem sobrepor um do outro.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Existem restrições sobre como utilizar endereços IP dentro destas sub-redes?
Sim. O Azure reserva alguns endereços IP em cada sub-rede. Os endereços IP do próprio e apelidos de cada sub-rede estão reservados para conformidade com o protocolo, juntamente com os endereços de x.x.x.1 x.x.x.3 de cada sub-rede, que são utilizadas para serviços do Azure.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Como pequenos e grandes como podem ser VNets e sub-redes?
A sub-rede menor suportada é /29, sendo o maior /8 (através de definições de sub-rede CIDR).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Posso usar meu VLANs para o Azure com VNets?
Não. As VNets estiverem sobreposições de camada 3. Azure não suporta qualquer semântica de camada 2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Pode especificar políticas de encaminhamento personalizadas no meu VNets e sub-redes?
Sim. Pode criar uma tabela de rotas e associe-o a uma sub-rede. Para obter mais informações sobre o encaminhamento do Azure, consulte [descrição geral do encaminhamento](virtual-networks-udr-overview.md#custom-routes).

### <a name="do-vnets-support-multicast-or-broadcast"></a>VNets suportarem multicast ou difusão?
Não. Multicast e de difusão não são suportadas.

### <a name="what-protocols-can-i-use-within-vnets"></a>Quais protocolos pode usar nas VNets?
Pode utilizar protocolos TCP, UDP e TCP/IP de ICMP nas VNets. Unicast é suportado nas VNets, com exceção de configuração protocolo DHCP (Dynamic Host) via Unicast (porta UDP/68 de origem / destino da porta UDP/67). Multicast, difusão, pacotes encapsulado IP no IP e pacotes de Generic Routing Encapsulation (GRE) são bloqueadas nas VNets. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Eu ping meu roteadores padrão dentro de uma VNet?
Não.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Pode utilizar tracert para diagnosticar a conectividade?
Não.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Posso adicionar sub-redes depois de criar a VNet?
Sim. Sub-redes podem ser adicionados às VNets em qualquer altura, desde que o intervalo de endereços de sub-rede não faz parte de outra sub-rede e há espaço disponível no intervalo de endereços da rede virtual.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Posso modificar o tamanho da minha sub-rede depois de criá-la?
Sim. Pode adicionar, remover, expandir ou reduzir uma sub-rede, se existirem não existem VMs ou serviços implementados no mesmo.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Posso modificar sub-redes depois que criei-los?
Sim. Pode adicionar, remover e modificar os blocos CIDR utilizados por uma VNet.

### <a name="if-i-am-running-my-services-in-a-vnet-can-i-connect-to-the-internet"></a>Se estiver a executar meus serviços numa VNet, pode ligar à internet?
Sim. Todos os serviços implementados numa VNet podem ligar a saída à internet. Para saber mais sobre ligações de internet de saída no Azure, veja [ligações de saída](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Se desejar ligar entrada a um recurso implementado através do Resource Manager, o recurso tem de ter um endereço IP público atribuído ao mesmo. Para saber mais sobre endereços IP públicos, veja [endereços IP públicos](virtual-network-public-ip-address.md). Todos os serviços de Cloud do Azure implementadas no Azure tem um VIP publicamente endereçável atribuído ao mesmo. Defina pontos finais de entrada para as funções de PaaS e os pontos finais para máquinas virtuais para ativar estes serviços aceitar ligações a partir da internet.

### <a name="do-vnets-support-ipv6"></a>VNets suportarem o IPv6?
Não. Não é possível utilizar IPv6 com VNets neste momento. Pode no entanto, as máquinas virtuais de balanceamento de endereços IPv6 de atribuir a balanceadores de carga do Azure para carregar. Para obter detalhes, consulte [descrição geral de IPv6 para o Azure Load Balancer](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="can-a-vnet-span-regions"></a>Uma VNet pode abranger a regiões?
Não. Uma VNet está limitada a uma única região. Uma rede virtual, no entanto, abrangem as zonas de disponibilidade. Para saber mais sobre as zonas de disponibilidade, veja [Descrição geral das zonas de disponibilidade](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Pode ligar redes virtuais em diferentes regiões com o peering de rede virtual. Para obter detalhes, consulte [descrição geral o peering de rede Virtual](virtual-network-peering-overview.md)

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Pode ligar uma rede virtual a outra VNet no Azure?
Sim. Pode ligar uma VNet para outra VNet através de um:
- **Peering de rede virtual**: para obter detalhes, consulte [descrição geral do VNet peering](virtual-network-peering-overview.md)
- **Um Gateway de VPN do Azure**: para obter detalhes, consulte [configurar uma ligação VNet a VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

## <a name="name-resolution-dns"></a>Resolução de nomes (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Quais são minhas opções de DNS para VNets?
Utilize a tabela de decisão sobre o [resolução de nomes para VMs e instâncias de função](virtual-networks-name-resolution-for-vms-and-role-instances.md) página que os orientam durante todo o DNS opções disponíveis.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Pode especificar servidores DNS para uma VNet?
Sim. Pode especificar endereços IP do servidor DNS nas definições da VNet. A configuração é aplicada como servidores DNS predefinido para todas as VMs na VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>O número de servidores DNS posso especificar?
Referência [limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits).

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Posso modificar meu servidores DNS após ter criado a rede?
Sim. Pode alterar a lista de servidores DNS para a sua VNet em qualquer altura. Se alterar a sua lista de servidores DNS, terá de reiniciar cada uma das VMs na sua VNet para que eles para pegar o novo servidor DNS.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>O que é fornecida pelo Azure DNS e funciona com VNets?
O DNS fornecida pelo Azure é um serviço DNS de multi-inquilino oferecido pela Microsoft. Azure regista todas as suas VMs e instâncias de função do serviço de cloud neste serviço. Este serviço fornece resolução de nomes pelo nome de anfitrião para VMs e instâncias de função contidas no mesmo serviço cloud e através do FQDN para VMs e instâncias de função na mesma VNet. Para saber mais sobre o DNS, consulte [resolução de nomes para VMs e serviços Cloud instâncias de função](virtual-networks-name-resolution-for-vms-and-role-instances.md).

Existe uma limitação para os primeiros 100 serviços em nuvem uma VNet para a resolução de nome entre inquilinos através de DNS fornecida pelo Azure. Se estiver a utilizar o seu próprio servidor DNS, esta limitação não se aplica.

### <a name="can-i-override-my-dns-settings-on-a-per-vm-or-cloud-service-basis"></a>Pode substituir as minhas definições DNS numa base de serviços por VM ou na cloud?
Sim. Pode definir os servidores DNS por VM ou serviço cloud para substituir as predefinições de rede. No entanto, é recomendado que utilize toda a rede DNS tanto quanto possível.

### <a name="can-i-bring-my-own-dns-suffix"></a>Posso usar o meu próprio sufixo DNS?
Não. Não é possível especificar um sufixo DNS personalizado para as suas VNets.

## <a name="connecting-virtual-machines"></a>Ligação de máquinas virtuais

### <a name="can-i-deploy-vms-to-a-vnet"></a>Pode implementar VMs a uma VNet?
Sim. Todas as interfaces de rede (NIC) ligadas a uma VM implementada através do modelo de implementação do Resource Manager tem de estar ligadas a uma VNet. As VMs implementadas por meio do modelo de implementação clássica, podem ser ligadas opcionalmente a uma VNet.

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>Quais são os diferentes tipos de endereços IP que posso atribuir às VMs?
* **Privada:** atribuídos a cada NIC dentro de cada VM. O endereço é atribuído através do método estático ou dinâmico. Endereços IP privados são atribuídos a partir do intervalo que especificou nas definições de sub-rede da VNet. Recursos implementados através do modelo de implementação clássica são atribuídos endereços IP privados, mesmo que eles não estiverem conectados a uma VNet. O comportamento do método de alocação é diferente dependendo de um recurso foi implementado com o Resource Manager ou o modelo de implementação clássica: 

  - **Gestor de recursos**: endereços IP privados atribuídos com o método dinâmico ou estático permanecerá atribuído a uma máquina virtual (Resource Manager) até que o recurso seja eliminado. A diferença é que selecionar o endereço a atribuir ao utilizar estático e escolhe o Azure ao utilizar dinâmico. 
  - **Clássico**: endereços IP privados atribuídos com o método dinâmico podem ser alterados quando uma máquina virtual (clássico) VM é reiniciada após ter sido no estado parado (desalocada). Se precisa garantir que o endereço IP privado de um recurso implementado através do modelo de implementação clássica nunca mudará, atribua um endereço IP privado com o método estático.

* **Público:** opcionalmente atribuído a NICs ligadas a VMs implementadas através do modelo de implementação Azure Resource Manager. O endereço pode ser atribuído com o método de alocação estático ou dinâmico. Instâncias de função de todas as VMs e serviços Cloud implementadas através do modelo de implementação clássica existem no serviço cloud, que é atribuído um *dinâmica*, pública endereço IP virtual (VIP). Uma pública *estático* endereço IP, chamado um [endereço IP reservado](virtual-networks-reserved-public-ip.md), opcionalmente, pode ser atribuído como um VIP. Pode atribuir endereços IP públicos individuais instâncias de função VMs ou serviços Cloud implementados através do modelo de implementação clássica. Estes endereços são denominados [IP público de nível de instância (ILPIP](virtual-networks-instance-level-public-ip.md) endereços e pode ser atribuído dinamicamente.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Pode reservar um endereço IP privado para uma VM que vou criar mais tarde?
Não. Não é possível reservar um endereço IP privado. Se um endereço IP privado estiver disponível, é atribuído a uma instância de função de VM ou pelo servidor DHCP. A VM pode ou não ser aquele que pretende que o endereço IP privado atribuído a. No entanto, pode, alterar o endereço IP privado de uma VM já criado, para qualquer endereço IP privado disponível.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Fazer a alteração de endereços IP privada para as VMs numa VNet?
Depende. Se a VM foi implementada com o Resource Manager, não, independentemente do endereço IP foi atribuído com o método de alocação estático ou dinâmico. Se a VM foi implementada por meio do modelo de implementação clássica, endereços IP dinâmicos podem alterar quando uma VM é iniciada depois de ter sido no estado parado (desalocada). O endereço é libertado a partir de uma VM implementada através de qualquer modelo de implementação quando a VM é eliminada.

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>Posso manualmente atribuir endereços IP a NICs dentro do sistema operacional VM?
Sim, mas não é recomendada, a menos que necessário, por exemplo, quando a atribuição de IP de vários endereços para uma máquina virtual. Para obter detalhes, consulte [a adicionar IP de vários endereços para uma máquina virtual](virtual-network-multiple-ip-addresses-portal.md#os-config). Se o endereço IP atribuído a uma NIC do Azure anexados a uma VM alterações e o endereço IP dentro do sistema de operativo da VM é diferente, perder a conectividade à VM.

### <a name="if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system-what-happens-to-my-ip-addresses"></a>Se eu parar um bloco de implementação do serviço em nuvem ou encerrar uma VM a partir do sistema operativo, o que acontece aos meus endereços IP?
Nada. Os endereços IP (VIP público, público e privado) permanecem atribuídos ao bloco de implementação do serviço cloud ou VM.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-redeploying"></a>Posso mover VMs de uma sub-rede para outra sub-rede numa VNet sem implementar novamente?
Sim. Pode encontrar mais informações na [como mover uma VM ou instância de função a uma sub-rede diferente](virtual-networks-move-vm-role-to-subnet.md) artigo.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Pode configurar um endereço MAC estático para a minha VM?
Não. Um endereço MAC não pode ser configurado de forma estática.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-its-created"></a>O endereço MAC permanecem os mesmos para a minha VM depois de criado?
Sim, o endereço MAC permanece o mesmo para uma VM implementada através do Resource Manager e os modelos de implementação clássica, até serem eliminada. Anteriormente, o endereço de MAC foi lançado se a VM foi parada (desalocada), mas agora o endereço MAC é mantido, mesmo quando a VM está no Estado desalocada.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Ligar à internet a partir de uma VM numa VNet?
Sim. Instâncias de função todas as VMs e serviços Cloud, implementadas numa VNet podem ligar à Internet.

## <a name="azure-services-that-connect-to-vnets"></a>Serviços do Azure que ligar VNets

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Pode utilizar aplicações de Web do serviço de aplicações do Azure com uma VNet?
Sim. Pode implementar aplicações Web dentro de uma VNet com um ASE (ambiente de serviço de aplicações). Se tiver uma ligação de ponto a site configurada para a sua VNet, todas as aplicações Web pode ligar em segurança e aceder aos recursos na VNet. Para obter mais informações, veja os artigos seguintes:

* [Criar aplicações Web num ambiente do serviço de aplicações](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Integrar a sua aplicação com uma rede Virtual do Azure](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Utilizar ligações híbridas e de integração de VNet com as aplicações Web](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Pode implementar serviços em nuvem com funções web e de trabalho (PaaS) numa VNet?
Sim. (Opcionalmente), pode implementar instâncias de função dos serviços Cloud nas VNets. Para tal, especifique o nome da VNet e os mapeamentos de função/sub-rede na seção de configuração de rede da sua configuração de serviço. Não é necessário atualizar qualquer um dos seus binários.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-to-a-vnet"></a>Pode ligar uma Máquina Virtual de dimensionamento definido (VMSS) a uma VNet?
Sim. Tem de ligar um VMSS a uma VNet.

### <a name="is-there-a-complete-list-of-azure-services-that-can-i-deploy-resources-from-into-a-vnet"></a>Existe que uma lista completa do Azure de serviços que posso implementar os recursos a partir de numa VNet?
Sim, para obter detalhes, consulte [integração da rede Virtual para serviços do Azure](virtual-network-for-azure-services.md).

### <a name="which-azure-paas-resources-can-i-restrict-access-to-from-a-vnet"></a>Que recursos de PaaS do Azure posso limitar o acesso a partir de uma VNet?

Recursos implementados por meio de alguns serviços PaaS do Azure (por exemplo, o armazenamento do Azure e base de dados do Azure SQL), pode restringir o acesso de rede apenas para os recursos numa VNet através da utilização de pontos finais de serviço de rede virtual. Para obter detalhes, consulte [descrição geral de pontos finais de serviço de rede Virtual](virtual-network-service-endpoints-overview.md).

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Pode mover meus serviços dentro e fora de VNets?
Não. Não é possível mover serviços dentro e fora de VNets. Para mover um recurso para outra VNet, terá de eliminar e voltar a implementar o recurso.

## <a name="security"></a>Segurança

### <a name="what-is-the-security-model-for-vnets"></a>O que é o modelo de segurança para VNets?
VNets estão isoladas um do outro e outros serviços alojados na infraestrutura do Azure. Uma VNet é um limite de fidedignidade.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>Posso restringir o fluxo de tráfego de entrada ou saída para recursos ligados à VNet?
Sim. Pode aplicar [grupos de segurança de rede](security-overview.md) para sub-redes individuais dentro de uma VNet, NICs ligadas a uma VNet, ou ambos.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>Pode implementar uma firewall entre os recursos ligados à VNet?
Sim. Pode implementar um [aplicação virtual de rede de firewall](https://azure.microsoft.com/marketplace/?term=firewall) de vários fornecedores através do Azure Marketplace.

### <a name="is-there-information-available-about-securing-vnets"></a>Há informações disponíveis sobre como proteger a VNets?
Sim. Para obter detalhes, consulte [descrição geral da segurança de rede de Azure](../security/security-network-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="apis-schemas-and-tools"></a>APIs, os esquemas e ferramentas

### <a name="can-i-manage-vnets-from-code"></a>Pode gerir VNets a partir do código?
Sim. Pode utilizar as APIs REST para VNets na [do Azure Resource Manager](/rest/api/virtual-network) e [clássicas (gestão de serviço)](https://go.microsoft.com/fwlink/?LinkId=296833) modelos de implementação.

### <a name="is-there-tooling-support-for-vnets"></a>Existe suporte de ferramentas para VNets?
Sim. Saiba mais sobre como utilizar:
- Portal do Azure para implementar VNets através da [do Azure Resource Manager](manage-virtual-network.md#create-a-virtual-network) e [clássico](virtual-networks-create-vnet-classic-pportal.md) modelos de implementação.
- PowerShell para gerenciar VNets implementadas através da [Resource Manager](/powershell/module/azurerm.network) e [clássico](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0) modelos de implementação.
- A interface de linha de comandos do Azure (CLI) para implementar e gerir VNets implementadas através da [Resource Manager](/cli/azure/network/vnet) e [clássico](../virtual-machines/azure-cli-arm-commands.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-commands-to-manage-network-resources) modelos de implementação.  

## <a name="vnet-peering"></a>VNet peering

### <a name="what-is-vnet-peering"></a>O que é o VNet peering?
VNet peering (ou peering da rede virtual) permite-lhe ligar redes virtuais. Uma ligação VNet peering entre redes virtuais permite-lhe encaminhar tráfego entre as mesmas em privado por meio de endereços IPv4. Máquinas virtuais nas VNets em modo de peering podem comunicar entre si, como se fossem dentro da mesma rede. Nestas redes virtuais podem estar na mesma região ou em diferentes regiões (também conhecido como Global VNet Peering). Também é possível criar ligações de VNet peering entre subscrições do Azure.

### <a name="can-i-create-a-peering-connection-to-a-vnet-in-a-different-region"></a>Pode criar uma ligação de peering para uma VNet numa região diferente?
Sim. Global VNet peering permite-lhe configurar o peering entre VNets em diferentes regiões. Global VNet peering está disponível em todas as regiões públicas do Azure. Não é possível globalmente emparelhamento de regiões públicas do Azure para nuvens nacionais. Global peering não está atualmente disponível em clouds nacionais.

### <a name="can-i-enable-vnet-peering-if-my-virtual-networks-belong-to-subscriptions-within-different-azure-active-directory-tenants"></a>Posso habilitar o VNet Peering se meu redes virtuais pertencem a subscrições dentro de diferentes inquilinos do Azure Active Directory?
Sim. É possível estabelecer o VNet Peering (sejam locais ou globais) se as suas subscrições pertencem a diferentes inquilinos do Azure Active Directory. Pode fazê-lo através do PowerShell ou CLI. Portal ainda não é suportado.

### <a name="my-vnet-peering-connection-is-in-initiated-state-why-cant-i-connect"></a>Minha pertença a ligação de peering de VNet *iniciado* Estado, por que não posso ligar?
Se a sua ligação de peering está num estado iniciado, isso significa que criou apenas uma ligação. Tem de ser criada uma ligação bidirecional para estabelecer uma ligação com êxito. Por exemplo, para se ligar a VNet A VNet b, tem de ser criada uma ligação de também a Vneta e de Vneta para também. Criação de ambas as ligações alterará o estado para *ligado.*

### <a name="my-vnet-peering-connection-is-in-disconnected-state-why-cant-i-create-a-peering-connection"></a>Minha pertença a ligação de peering de VNet *desligado* Estado, por que não é possível criar uma ligação de peering?
Se a sua ligação de peering de VNet está num estado desligado, significa um dos links criados foi eliminado. Para voltar a estabelecer uma ligação de peering, terá de eliminar a ligação e recriar.

### <a name="can-i-peer-my-vnet-with-a-vnet-in-a-different-subscription"></a>Posso ligar a minha VNet com uma VNet numa subscrição diferente?
Sim. Pode configurar o peering entre VNets entre subscrições e entre regiões.

### <a name="can-i-peer-two-vnets-with-matching-or-overlapping-address-ranges"></a>Pode posso configurar o peering entre duas VNets com intervalos de endereços correspondente ou sobrepostos?
Não. Espaços de endereços tem de ser não overalap ative o Peering de VNet.

### <a name="how-much-do-vnet-peering-links-cost"></a>Fazer quanto de custos de ligações de peering de VNet?
Não existe nenhum custo associado para a criação de uma ligação de peering de VNet. Transferência de dados em ligações de peering é cobrada. [Veja aqui](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="is-vnet-peering-traffic-encrypted"></a>Tráfego de VNet peering é encriptado?
Não. O tráfego entre recursos em VNets em modo de peering é privado e isolada. Continua a ser completamente no Backbone do Microsoft.

### <a name="why-is-my-peering-connection-in-a-disconnected-state"></a>Por que é a minha ligação de peering num Estado desconectado?
Aprofundar a ligações de peering de VNet *desligado* estado quando uma ligação VNet peering é eliminada. Tem de eliminar ambas as ligações para restabelecer uma ligação de peering com êxito.

### <a name="if-i-peer-vneta-to-vnetb-and-i-peer-vnetb-to-vnetc-does-that-mean-vneta-and-vnetc-are-peered"></a>Se eu configurar o peering também a Vneta e posso criar o peering Vneta no mesmo peer, que significa também e estão no mesmo peer em modo de peering?
Não. Peering transitivo não é suportada. Deve configurar o peering também e estão no mesmo peer para que isso ocorra.

### <a name="are-there-any-bandwidth-limitations-for-peering-connections"></a>Existem limitações de largura de banda para ligações de peering?
Não. VNet peering, sejam locais ou globais, não impõe quaisquer restrições de largura de banda. Largura de banda é apenas limites pelo recurso VM ou de computação.

## <a name="virtual-network-tap"></a>TAP de rede virtual

### <a name="which-azure-regions-are-available-for-virtual-network-tap"></a>Que regiões do Azure estão disponíveis para teste de rede virtual?
Durante a pré-visualização de programador, a capacidade está disponível na região e.u.a. Centro-Oeste. As interfaces de rede monitorizado, o recurso TAP da rede virtual e a solução de recoletor ou análise tem de ser implementados na mesma região.

### <a name="does-virtual-network-tap-support-any-filtering-capabilities-on-the-mirrored-packets"></a>O TAP da rede Virtual suporta qualquer capacidades de filtragem nos pacotes espelhados?
Recursos de filtragem não são suportados com a pré-visualização TAP da rede virtual. Quando uma configuração de TOQUE é adicionada a uma interface de rede uma cópia em profundidade de todos os a entrada e o tráfego de saída na interface de rede é transmitido para o destino de TOQUE.

### <a name="can-multiple-tap-configurations-be-added-to-a-monitored-network-interface"></a>Várias configurações de TOQUE podem ser adicionadas a uma interface de rede monitorizado?
Uma interface de rede monitorizada pode ter apenas uma configuração de TOQUE. Verifique com o indivíduo [soluções de parceiros](virtual-network-tap-overview.md#virtual-network-tap-partner-solutions) para a capacidade de transmitir várias cópias do tráfego de TOQUE para as ferramentas de análise da sua preferência.

### <a name="can-the-same-virtual-network-tap-resource-aggregate-traffic-from-monitored-network-interfaces-in-more-than-one-virtual-network"></a>O mesmo recurso TAP da rede virtual pode agregar o tráfego de interfaces de rede monitorizados em mais de uma rede virtual?
Sim. Da mesma rede virtual recursos de TOQUE pode ser utilizada para agregar tráfego de espelho de interfaces de rede monitorizados em redes virtuais em modo de peering na mesma subscrição ou numa subscrição diferente. O recurso de TOQUE de rede virtual e o destino de Balanceador de carga ou interface de rede de destino tem de estar na mesma subscrição. Todas as subscrições têm de estar no mesmo inquilino do Azure Active Directory.

### <a name="are-there-any-performance-considerations-on-production-traffic-if-i-enable-a-virtual-network-tap-configuration-on-a-network-interface"></a>Existem quaisquer considerações de desempenho no tráfego de produção se ativar a uma configuração de TAP da rede virtual numa interface de rede?

Rede virtual TOQUE está em developer preview. Durante a pré-visualização, não existe nenhum contrato de nível de serviço. O recurso não deve ser utilizado para cargas de trabalho de produção. Quando uma interface de rede de máquina virtual é ativada com uma configuração de TOQUE, os mesmos recursos no anfitrião do azure atribuída à máquina virtual para enviar o tráfego de produção é utilizado para executar a função de espelhamento e enviar os pacotes espelhados. Selecione o correto [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Windows](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tamanho da máquina virtual para se certificar de que recursos suficientes disponíveis para a máquina virtual enviar o tráfego de produção e o tráfego de espelho.

### <a name="is-accelerated-networking-for-linuxcreate-vm-accelerated-networking-climd-or-windowscreate-vm-accelerated-networking-powershellmd-supported-with-virtual-network-tap"></a>É accelerated networking para [Linux](create-vm-accelerated-networking-cli.md) ou [Windows](create-vm-accelerated-networking-powershell.md) suportado com o teste de rede virtual?

Poderá adicionar uma configuração de TOQUE numa interface de rede ligada a uma máquina virtual que está ativada com redes aceleradas. Mas o desempenho e a latência na máquina virtual serão afetados pela adição de configuração de TOQUE, uma vez que a descarga de tráfego de espelhamento de não é atualmente suportada pelo Azure accelerated networking.
