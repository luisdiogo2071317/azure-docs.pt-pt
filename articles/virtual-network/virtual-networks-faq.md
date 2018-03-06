---
title: FAQ da rede Virtual do Azure | Microsoft Docs
description: "Respostas às perguntas mais frequentes sobre as redes virtuais do Microsoft Azure."
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
ms.date: 03/01/2018
ms.author: jdial
ms.openlocfilehash: 8800dc59306c349daba8f4d9703e0c713eed06ec
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Rede Virtual do Azure perguntas mais frequentes (FAQ)

## <a name="virtual-network-basics"></a>Noções básicas de rede virtuais

### <a name="what-is-an-azure-virtual-network-vnet"></a>O que é uma rede Virtual do Azure (VNet)?
Uma rede Virtual do Azure (VNet) é uma representação da sua própria rede na nuvem. É um isolamento lógico da nuvem do Azure dedicado à sua subscrição. Pode utilizar as VNets para aprovisionar e gerir redes privadas virtuais (VPNs) no Azure e, opcionalmente, ligar VNets outras VNets no Azure ou com no local a infraestrutura de TI para criar soluções híbridas ou em vários locais. Cada VNet que criou tem bloco CIDR e pode ser associado a outras redes locais e as VNets, desde que os blocos CIDR não se podem sobrepor. Também tem controlo das definições do servidor DNS para VNets e a segmentação da VNet em sub-redes.

Utilize as VNets para:

* Crie um dedicado privada apenas na nuvem VNet, por vezes, não precisa de uma configuração de vários locais para a sua solução. Quando cria uma VNet, os seus serviços e VMs dentro da VNet podem comunicar diretamente e segura entre si na nuvem. Pode ainda configurar ligações de ponto final para as VMs e serviços que necessitem de comunicação da Internet, como parte da sua solução.
* Expandir o seu centro de dados com as VNets em segurança, pode criar VPNs de (S2S) do site para site tradicionais de forma segura dimensionar a capacidade de centro de dados. S2S VPNs utilize IPSEC para fornecer uma ligação segura entre o gateway de VPN empresarial e o Azure.
* Cenários de nuvem híbrida ativar VNets dão-lhe a flexibilidade para suportar uma gama de cenários de nuvem híbrida. Pode ligar de forma segura aplicações baseadas na nuvem a qualquer tipo de sistema no local como mainframes e sistemas Unix.

### <a name="how-do-i-get-started"></a>Como posso começar?
Visite o [documentação das redes virtuais](https://docs.microsoft.com/azure/virtual-network/) para começar a utilizar. Este conteúdo fornece informações de descrição geral e implementação para todas as funcionalidades de VNet.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Pode utilizar as VNets sem conectividade em vários locais?
Sim. Pode utilizar uma VNet sem o ligar a local. Por exemplo, é possível executar controladores de domínio do Active Directory do Microsoft Windows Server e farms do SharePoint apenas numa VNet do Azure.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Pode efetuar otimização de WAN entre VNets ou uma VNet e os meus Centro de dados no local?

Sim. Pode implementar um [aplicação virtual de rede de otimização de WAN](https://azure.microsoft.com/marketplace/?term=wan+optimization) de vários fornecedores através do Azure Marketplace.

## <a name="configuration"></a>Configuração

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>As ferramentas de utilizar para criar uma VNet?
Pode utilizar as ferramentas seguintes para criar ou configurar uma VNet:

* Portal do Azure
* PowerShell
* CLI do Azure
* Um ficheiro de configuração de rede (netcfg - para apenas as VNets clássicas). Consulte o [configurar uma VNet com um ficheiro de configuração de rede](virtual-networks-using-network-configuration-file.md) artigo.

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Os intervalos de endereços podem utilizar a minha vnets?
Qualquer intervalo de endereços IP definidos no [RFC 1918](http://tools.ietf.org/html/rfc1918). Por exemplo, 10.0.0.0/16.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Pode ter endereços IP públicos no meu VNets?
Sim. Para mais informações sobre os intervalos de endereços IP públicos, consulte [criar uma rede virtual](virtual-network-manage-network.md#create-a-virtual-network). Endereços IP públicos não são diretamente acessíveis a partir da internet.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>Existe um limite ao número de sub-redes na minha VNet?
Sim. Consulte [Azure limita](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) para obter mais detalhes. Espaços de endereços da sub-rede não se podem sobrepor um do outro.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Existem restrições sobre como utilizar estas sub-redes de endereços IP?
Sim. Azure reserva-se alguns endereços IP dentro de cada sub-rede. Os endereços IP primeiro e últimos de cada sub-rede estão reservados para compatibilidade com o protocolo, juntamente com os endereços de x.x.x.1 x.x.x.3 de cada sub-rede, que são utilizadas para serviços do Azure.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Como pequenos e grandes como podem ser VNets e sub-redes?
A sub-rede mais pequeno suportada é /29, não sendo o maior /8 (utilizando as definições de sub-rede CIDR).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Posso usar o meu VLANs para o Azure utilizando as VNets?
Não. As VNets estão sobrepõe de camada 3. Azure não suporta qualquer semântica de camada 2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Pode especificar políticas de encaminhamento personalizadas no meu VNets e sub-redes?
Sim. Pode criar uma tabela de rota e associá-la a uma sub-rede. Para obter mais informações sobre o encaminhamento no Azure, consulte [descrição geral de encaminhamento](virtual-networks-udr-overview.md#custom-routes).

### <a name="do-vnets-support-multicast-or-broadcast"></a>As VNets suportam multicast ou difusão?
Não. Multicast e difusão não são suportadas.

### <a name="what-protocols-can-i-use-within-vnets"></a>Protocolos de que pode utilizar nas VNets?
Pode utilizar protocolos de TCP, UDP e TCP/IP de ICMP nas VNets. Unicast é suportado nas VNets, à exceção da configuração do protocolo DHCP (Dynamic Host) através de Unicast (porta UDP/68 de origem / destino porta UDP/67). Multicast, difusão, pacotes de IP-em-IP encapsulado e pacotes de Generic Routing Encapsulation (GRE) estão bloqueadas nas VNets. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Posso ping meu routers predefinido numa VNet?
Não.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Pode utilizar tracert para diagnosticar conectividade?
Não.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Posso adicionar sub-redes depois de é criada a VNet?
Sim. Sub-redes podem ser adicionadas ao VNets em qualquer altura, desde que o intervalo de endereços da sub-rede não faz parte de outra sub-rede e existe espaço disponível no intervalo de endereços da rede virtual.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Posso modificar o tamanho da minha sub-rede depois de criar?
Sim. Pode adicionar, remover, expandir ou reduzir uma sub-rede, se existirem sem VMs ou serviços implementados no mesmo.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Posso modificar sub-redes depois de criá-las?
Sim. Pode adicionar, remover e modificar os blocos CIDR utilizados por uma VNet.

### <a name="if-i-am-running-my-services-in-a-vnet-can-i-connect-to-the-internet"></a>Se estiver a executar os meus serviços numa VNet, ligar à internet?
Sim. Todos os serviços implementados numa VNet podem ligar a saída à internet. Para saber mais sobre ligações de internet de saída no Azure, consulte o artigo [ligações de saída](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Se pretender ligar entrada para um recurso implementado através do Resource Manager, o recurso tem de ter um endereço IP público atribuído. Para saber mais sobre os endereços IP públicos, consulte o artigo [endereços IP públicos](virtual-network-public-ip-address.md). Todos os serviços de nuvem do Azure implementado no Azure tem um VIP publicamente endereçável atribuído. Defina pontos finais de entrada para funções de PaaS e os pontos finais para máquinas virtuais para ativar estes serviços aceitar ligações a partir da internet.

### <a name="do-vnets-support-ipv6"></a>As VNets suportam o IPv6?
Não. Não é possível utilizar o IPv6 com as VNets neste momento. Pode, contudo, endereços de atribuir IPv6 para balanceadores de carga do Azure para carregar equilibrar as máquinas virtuais. Para obter mais informações, consulte [descrição geral de IPv6 para o Balanceador de carga do Azure](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="can-a-vnet-span-regions"></a>Uma VNet pode abranger a regiões?
Não. Uma VNet está limitada a uma única região. Uma rede virtual, no entanto, span zonas de disponibilidade. Para saber mais sobre as zonas de disponibilidade, veja [Descrição geral das zonas de disponibilidade](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Pode ligar redes virtuais em diferentes regiões com o peering de rede virtual. Para obter mais informações, consulte [descrição geral de peering da rede Virtual](virtual-network-peering-overview.md)

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Pode ligar uma VNet para outra VNet no Azure?
Sim. Pode ligar uma VNet para outra VNet através de:
- **Peering de rede virtual**: para obter mais informações, consulte [descrição geral do VNet peering](virtual-network-peering-overview.md)
- **Um Gateway de VPN do Azure**: para obter mais informações, consulte [configurar uma ligação VNet a VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

## <a name="name-resolution-dns"></a>Resolução de nomes (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Quais são as minhas opções de DNS para VNets?
Utilize a tabela de decisão no [resolução de nomes para VMs e instâncias de função](virtual-networks-name-resolution-for-vms-and-role-instances.md) página para ajudá-lo através de todos os DNS opções disponíveis.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Pode especificar servidores DNS para uma VNet?
Sim. Pode especificar endereços IP do servidor DNS nas definições da VNet. A definição será aplicada como servidores DNS predefinido para todas as VMs na VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Quantos servidores DNS que pode a especificar
Referência [Azure limita](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits).

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Posso modificar os meus servidores DNS depois criei rede?
Sim. Pode alterar a lista de servidores DNS para a sua VNet em qualquer altura. Se alterar a lista de servidores DNS, terá de reiniciar cada uma das VMs na sua VNet serem para recolher o novo servidor DNS.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>O que é DNS fornecidos pelo Azure e funciona com as VNets?
DNS fornecidos pelo Azure é um serviço DNS de multi-inquilino disponibilizado pela Microsoft. Azure regista todas as VMs e instâncias de função Serviço de nuvem neste serviço. Este serviço fornece resolução de nomes pelo nome do anfitrião para as VMs e instâncias de função contidas no mesmo serviço de nuvem e através do FQDN para VMs e instâncias de função na mesma VNet. Para saber mais sobre o DNS, consulte o artigo [resolução de nomes para VMs e serviços em nuvem instâncias de função](virtual-networks-name-resolution-for-vms-and-role-instances.md).

Há uma limitação para os serviços de 100 nuvem primeiro numa VNet para resolução de nomes de inquilino em vários locais através do DNS fornecidos pelo Azure. Se estiver a utilizar o seu próprio servidor DNS, esta limitação não se aplica.

### <a name="can-i-override-my-dns-settings-on-a-per-vm-or-cloud-service-basis"></a>Pode substituir as minhas definições DNS numa base de serviço por VM ou na nuvem?
Sim. Pode definir os servidores DNS por serviço VM ou na nuvem para substituir as definições de rede predefinido. No entanto, é recomendado que utilize o DNS de toda a rede quanto possível.

### <a name="can-i-bring-my-own-dns-suffix"></a>Pode colocar meu próprio sufixo DNS?
Não. Não é possível especificar um sufixo DNS personalizado para as suas VNets.

## <a name="connecting-virtual-machines"></a>Ligar máquinas virtuais

### <a name="can-i-deploy-vms-to-a-vnet"></a>Pode implementar VMs para uma VNet?
Sim. Todas as interfaces de rede (NIC) ligadas a uma VM implementada através do modelo de implementação Resource Manager devem estar ligadas a uma VNet. VMs implementadas através do modelo de implementação clássica, opcionalmente, podem ser ligadas a uma VNet.

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>Quais são os diferentes tipos de endereços IP que pode atribuição ao VMs?
* **Privados:** atribuídos a cada NIC dentro de cada VM. O endereço é atribuído a utilizar é o método estático ou dinâmico. Atribuição de endereços IP privados do intervalo que especificou nas definições de sub-rede da VNet. Recursos implementados através do modelo de implementação clássica são atribuídos a endereços IP privados, mesmo que não estão ligados a uma VNet. O comportamento do método de alocação é diferente consoante se um recurso foi implementado com o Gestor de recursos ou o modelo de implementação clássica: 

  - **Gestor de recursos**: um endereço IP privado atribuído com o método dinâmico ou estático permanecerá atribuído a uma máquina virtual (Resource Manager) até que o recurso é eliminado. A diferença é que selecionou o endereço para atribuir ao utilizar estática e Azure escolhe quando utilizar dinâmico. 
  - **Clássico**: um endereço IP privado atribuído com o método dinâmico podem ser alterados quando uma máquina virtual (clássica) VM é reiniciada após ter sido no estado parado (desalocado). Se precisar de Certifique-se de que o endereço IP privado para um recurso implementado através do modelo de implementação clássica nunca é alterado, atribua um endereço IP privado com o método estático.

* **Público:** opcionalmente atribuído a NICs anexados a VMs implementadas através do modelo de implementação Azure Resource Manager. O endereço pode ser atribuído com o método de alocação estático ou dinâmico. Instâncias de função de todas as VMs e serviços em nuvem implementadas através do modelo de implementação clássica existem dentro de um serviço em nuvem, que é atribuído um *dinâmica*, pública endereço IP virtual (VIP). Um público *estático* endereço IP, chamado um [endereço IP reservado](virtual-networks-reserved-public-ip.md), opcionalmente, pode ser atribuída como um VIP. Pode atribuir endereços IP públicos para VMs ou serviços em nuvem função instâncias individuais implementadas através do modelo de implementação clássica. Estes endereços são denominados [IP público de nível de instância (ILPIP](virtual-networks-instance-level-public-ip.md) endereços e podem ser atribuídos dinamicamente.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Pode reservar um endereço IP privado para uma VM que irá criar numa altura posterior?
Não. Não é possível reservar um endereço IP privado. Se estiver disponível um endereço IP privado, é atribuído a uma instância VM ou função pelo servidor DHCP. A VM pode ou não ser que pretende que o endereço IP privado atribuído a. No entanto, pode alterar o endereço IP privado de uma VM já criado, para qualquer endereço IP privado disponível.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Efetue a alteração de endereços IP privada para as VMs numa VNet?
Depende. Se a VM foi implementada através do Gestor de recursos, não, independentemente se o endereço IP foi atribuído com o método de alocação estático ou dinâmico. Se a VM foi implementada através do modelo de implementação clássica, endereços IP dinâmicos podem alterar quando uma VM é iniciada depois de ter sido no estado parado (desalocado). O endereço é libertado a partir de uma VM implementada através de um modelo de implementação quando a VM é eliminada.

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>Pode manualmente atribuição de endereços IP ao NICs no sistema de operativo VM?
Sim, mas não é recomendado, exceto se necessário, por exemplo, quando a atribuição de IP de vários endereços a uma máquina virtual. Para obter mais informações, consulte [Adicionar IP múltiplos endereços para uma máquina virtual](virtual-network-multiple-ip-addresses-portal.md#os-config). Se o endereço IP atribuído a um NIC de Azure ligado a uma VM alterações e o endereço IP no sistema de operativo VM diferente, perder a conectividade à VM.

### <a name="if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system-what-happens-to-my-ip-addresses"></a>Se posso parar um bloco de implementação do serviço em nuvem ou encerramento de uma VM a partir do sistema operativo, o que acontece a minha endereços IP?
Nada. Os endereços IP (VIP público, público e privado) permanecem atribuídos para a ranhura de implementação do serviço de nuvem ou VM.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-redeploying"></a>Pode posso mover VMs de uma sub-rede para outra sub-rede numa VNet sem Reimplementar?
Sim. Pode encontrar mais informações no [como mover uma VM ou instância de função para outra sub-rede](virtual-networks-move-vm-role-to-subnet.md) artigo.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Pode configurar um endereço MAC estático para a minha VM?
Não. Um endereço MAC não pode ser configurado estaticamente.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-its-created"></a>O endereço MAC permanecer o mesmo para a minha VM quando for criado?
Sim, o endereço de MAC foi alterada para uma VM implementada através do Gestor de recursos e os modelos de implementação clássica até serem eliminada. Anteriormente, o endereço de MAC foi libertado se a VM foi parada (desalocada), mas agora o endereço MAC é mantido, mesmo quando a VM se encontra no Estado desalocado.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Posso ligar à internet de uma VM numa VNet?
Sim. Todas as VMs e serviços em nuvem instâncias de função implementadas numa VNet podem ligar à Internet.

## <a name="azure-services-that-connect-to-vnets"></a>Serviços do Azure que se ligam a VNets

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Pode utilizar aplicações de Web do serviço de aplicações do Azure com uma VNet?
Sim. Pode implementar aplicações Web dentro de uma VNet com ASE (ambiente de serviço de aplicações). Se tiver uma ligação ponto a site configurada para a sua VNet, todas as aplicações Web podem ligar e aceder a recursos na VNet em segurança. Para obter mais informações, veja os artigos seguintes:

* [Criar Web Apps num ambiente de serviço de aplicações](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Integrar a sua aplicação com uma Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Utilizar a integração de VNet e as ligações híbridas com as Web Apps](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Pode implementar serviços em nuvem com funções web e de trabalho (PaaS) numa VNet?
Sim. (Opcionalmente), pode implementar instâncias de função de serviços em nuvem nas VNets. Para tal, especifique o nome da VNet e os mapeamentos de função/sub-rede na secção de configuração de rede da sua configuração de serviço. Não é necessário atualizar qualquer um dos seus binários.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-to-a-vnet"></a>Pode ligar uma Máquina Virtual escala definido (VMSS) para uma VNet?
Sim. Tem de ligar um VMSS para uma VNet.

### <a name="is-there-a-complete-list-of-azure-services-that-can-i-deploy-resources-from-into-a-vnet"></a>Existe que uma lista completa do Azure para serviços que posso implementar recursos para uma VNet?

Sim, para obter mais informações, consulte o artigo [integração de rede Virtual para os serviços do Azure](virtual-network-for-azure-services.md).

### <a name="which-azure-paas-resources-can-i-restrict-access-to-from-a-vnet"></a>Quais os recursos do Azure PaaS posso restringir acesso a partir de uma VNet?

Recursos implementados através de alguns serviços do Azure PaaS (por exemplo, o Storage do Azure e SQL Database do Azure), pode restringir o acesso de rede para apenas os recursos numa VNet através da utilização de pontos finais do serviço de rede virtual. Para obter mais informações, consulte [descrição geral de pontos finais de serviço de rede Virtual](virtual-network-service-endpoints-overview.md).

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Pode mover os meus serviços dentro ou fora do VNets?
Não. Não é possível mover serviços dentro ou fora do VNets. Para mover um recurso para outra VNet, tem de eliminar e voltar a implementar o recurso.

## <a name="security"></a>Segurança

### <a name="what-is-the-security-model-for-vnets"></a>O que é o modelo de segurança para VNets?
As VNets estão isoladas entre si e outros serviços alojados na infraestrutura do Azure. Uma VNet é um limite de fidedignidade.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>Pode restringir o fluxo de tráfego de entrada ou saída aos recursos ligados à VNet?
Sim. Pode aplicar [grupos de segurança de rede](security-overview.md) a sub-redes individuais numa VNet, NICs ligado a uma VNet, ou ambos.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>Pode implementar uma firewall entre os recursos ligados à VNet?
Sim. Pode implementar um [aplicação virtual de rede de firewall](https://azure.microsoft.com/marketplace/?term=firewall) de vários fornecedores através do Azure Marketplace.

### <a name="is-there-information-available-about-securing-vnets"></a>Existe informações disponíveis sobre como proteger VNets?
Sim. Para obter mais informações, consulte [descrição geral de segurança de rede de Azure](../security/security-network-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="apis-schemas-and-tools"></a>APIs, esquemas e ferramentas

### <a name="can-i-manage-vnets-from-code"></a>Pode gerir as VNets a partir do código?
Sim. Pode utilizar as APIs REST para VNets no [do Azure Resource Manager](/rest/api/virtual-network) e [clássico (gestão de serviço)](http://go.microsoft.com/fwlink/?LinkId=296833) modelos de implementação.

### <a name="is-there-tooling-support-for-vnets"></a>Existe suporte de ferramentas para VNets?
Sim. Saiba mais sobre como utilizar:
- Portal do Azure para implementar as VNets através de [do Azure Resource Manager](virtual-networks-create-vnet-arm-pportal.md) e [clássico](virtual-networks-create-vnet-classic-pportal.md) modelos de implementação.
- PowerShell para gerir as VNets implementadas através de [Resource Manager](/powershell/module/azurerm.network) e [clássico](/powershell/module/azure/?view=azuresmps-3.7.0) modelos de implementação.
- A interface de linha de comandos do Azure (CLI) para implementar e gerir as VNets implementadas através de [Resource Manager](/cli/azure/network/vnet) e [clássico](../virtual-machines/azure-cli-arm-commands.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-commands-to-manage-network-resources) modelos de implementação.  
