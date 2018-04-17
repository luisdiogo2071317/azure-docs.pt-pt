---
title: Considerações sobre a integração para sistemas de pilha do Azure integrado de rede | Microsoft Docs
description: Saiba o que pode fazer para planear a integração de rede do Centro de dados com vários nós do Azure pilha.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: e6438c353d84510ee918df120e6d54df0607c89d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="network-connectivity"></a>Conectividade de rede
Este artigo fornece informações de infraestrutura de rede de pilha do Azure para o ajudar a decidir como pretende integrar melhor pilha do Azure no seu ambiente de rede existente. 

> [!NOTE]
> Para resolver nomes DNS externos da pilha do Azure (por exemplo, www.bing.com), tem de fornecer os servidores DNS para reencaminhar pedidos DNS. Para obter mais informações sobre os requisitos de DNS de pilha do Azure, consulte [integração de centro de dados de pilha do Azure - DNS](azure-stack-integrate-dns.md).

## <a name="physical-network-design"></a>Design de rede física
A solução de pilha do Azure requer uma infraestrutura física resiliente e de elevada para suportar a operação e serviços. O diagrama seguinte representa o nosso estrutura recomendada:

![Estrutura recomendada de rede de pilha do Azure](media/azure-stack-network/recommended-design.png)


## <a name="logical-networks"></a>Redes lógicas
Redes lógicas representam uma abstração das infraestruturas de rede física subjacente. São utilizados para organizar e simplificar as atribuições de rede para anfitriões, máquinas virtuais e serviços. Como parte da criação de rede lógica, os sites de rede são criados para definir as redes locais virtuais (VLANs), sub-redes IP e os pares sub-rede/VLAN do IP estão associados com a rede lógica em cada localização física.

A tabela seguinte mostra as redes lógicas e os intervalos de sub-rede IPv4 associados que terá de planear:

| Rede lógica | Descrição | Tamanho | 
| -------- | ------------- | ------------ | 
| VIP público | Pilha do Azure utiliza um total de 32 endereços desta rede. Oito endereços IP públicos são utilizados para um pequeno conjunto de serviços de pilha do Azure e os restantes são utilizados por máquinas virtuais inquilinas. Se planeia utilizar o serviço de aplicações e os fornecedores de recursos do SQL Server, 7 mais endereços são utilizados. | / 26 (62 anfitriões) - /22 (1022 anfitriões)<br><br>Recomendado = /24 (254 anfitriões) | 
| Infraestrutura de comutador | Endereços IP-to-Point para efeitos de encaminhamento, dedicados mudar interfaces de gestão e os endereços de loopback atribuídos ao comutador. | /26 | 
| Infraestrutura | Utilizado para componentes internos de pilha do Azure para comunicar. | /24 |
| Privado | Utilizado para a rede de armazenamento e VIPs privadas. | /24 | 
| BMC | Utilizado para comunicar com os BMCs nos anfitriões físicos. | /27 | 
| | | |

## <a name="network-infrastructure"></a>Infraestrutura de rede
A infraestrutura de rede para a pilha do Azure é composta por várias redes lógicas que estão configuradas nos comutadores. O diagrama seguinte mostra estas redes lógicas e a forma integrar com o top-of-rack (TOR), controlador de gestão de placa base (BMC) e limite comutadores (rede de cliente).

![Ligações de diagrama e o comutador de rede lógica](media/azure-stack-network/NetworkDiagram.png)

### <a name="bmc-network"></a>Rede BMC
Esta rede se encontra dedicada para ligar todos os os controladores bmc (também conhecido como serviço processadores, por exemplo, iDRAC Integrated Lights-out, iBMC, etc.) para a rede de gestão. Se estiver presente, o anfitrião de ciclo de vida de Hardware (HLH) encontra-se nesta rede e pode fornecer software específico do OEM para manutenção de hardware ou a monitorização. 

O HLH também aloja a VM de implementação (DVM). O DVM é utilizado durante a implementação de pilha do Azure e é removido quando concluir a implementação. O DVM requer acesso à internet em cenários de implementação ligado para testar, validar e aceder a vários componentes. Estes componentes podem ser dentro e fora da sua rede empresarial; Por exemplo NTP, DNS e do Azure. Para obter mais informações sobre os requisitos de conectividade, consulte o [secção NAT integração da firewall do Azure pilha](azure-stack-firewall.md#network-address-translation). 

### <a name="private-network"></a>Rede privada
Este /24 254 anfitrião do rede (IP) é privado para a região de pilha do Azure (expande para além dos dispositivos de comutador do limite da região de pilha do Azure) e está dividido em duas sub-redes:

- **Rede de armazenamento**. Migração em direto uma /25 126 anfitrião do rede (IP) utilizado para suportar a utilização de tráfego de armazenamento direto de espaços e o bloco de mensagem de servidor (SMB) e a máquina virtual. 
- **Rede IP virtual interna**. A/25 VIPs dedicados para meramente internos para o Balanceador de carga do software de rede.

### <a name="azure-stack-infrastructure-network"></a>Rede de infraestrutura de pilha do Azure
Isto/24 rede se encontra dedicado para componentes internos de pilha do Azure para que estes possam comunicar e trocar dados entre si próprios. Esta sub-rede necessita de endereços IP encaminháveis, mas é mantida privada para a solução através de listas de controlo de acesso (ACL). Não é esperado para ser encaminhados para além dos comutadores de limite, exceto para um intervalo de pequeno equivalente num tamanho de ao/27 rede utilizada pelas alguns destes serviços quando necessitem de acesso a recursos externos e/ou da internet. 

### <a name="public-infrastructure-network"></a>Rede de infraestrutura públicas
Isto/27 rede é o intervalo de pequeno da sub-rede de infraestrutura do Azure pilha mencionado anteriormente, não necessita de endereços IP públicos, mas requerem acesso à internet através de um NAT ou um Proxy transparente. Esta rede será atribuída para o sistema de consola de recuperação de emergência (ERCS), a VM ERCS requer acesso à internet durante o registo para o Azure e cópias de segurança da infraestrutura. A VM ERCS devem ser encaminhável para a sua rede de gestão para fins de resolução de problemas.

### <a name="public-vip-network"></a>Rede de VIP pública
A rede de VIP público é atribuída ao controlador de rede na pilha do Azure. Não é uma rede lógica no comutador. O SLB utiliza o conjunto de endereços e atribui/32 redes para cargas de trabalho inquilinas. Na tabela de encaminhamento de comutador, estes IPs 32 estão anunciados como uma rota disponível através do BGP. Esta rede contém os endereços IP externo acessível ou públicos. A infraestrutura de pilha do Azure utiliza 8 endereços desta rede VIP público enquanto o resto é utilizado pelas VMs inquilino. O tamanho da rede nesta sub-rede variam entre um mínimo de /26 (64 anfitriões) para um máximo de /22 (1022 anfitriões), recomendamos que planeie um /24 rede.

### <a name="switch-infrastructure-network"></a>Rede de infraestrutura do comutador
Isto/26 rede é a sub-rede que contenha sub-redes/30 IP encaminhável-to-Point (anfitrião 2 IPs) e loopbacks que estão dedicados/32 sub-redes para a banda no comutador gestão e o ID de router BGP. Este intervalo de endereços IP tem de ser encaminhável externamente da solução pilha do Azure para o seu centro de dados, podem ser IPs privado ou público.

### <a name="switch-management-network"></a>Rede de gestão do comutador
Este /29 (anfitrião de 6 IPs) rede se encontra dedicada para ligar as portas de gestão dos parâmetros. Permitir o acesso fora de banda para a implementação, gestão e resolução de problemas. Esta é calculada a partir da rede de infraestrutura comutador mencionada acima.

## <a name="publish-azure-stack-services"></a>Publicar os serviços de pilha do Azure
Terá de disponibilizar os serviços do Azure pilha aos utilizadores da pilha fora do Azure. Pilha do Azure configura vários pontos finais para as respetivas funções de infraestrutura. Estes pontos finais são atribuídos VIPs do conjunto de endereço IP público. Uma entrada DNS é criada para cada ponto final na zona DNS externa, que foi especificada no momento da implementação. Por exemplo, o portal de utilizador está atribuído a entrada de anfitrião DNS do portal.  *&lt;região >.&lt; FQDN >*.

### <a name="ports-and-urls"></a>URLs e portas
Para tornar os serviços de pilha do Azure (por exemplo, portais do Azure Resource Manager, DNS, etc.) disponível para redes externas, tem de permitir tráfego de entrada para estes pontos finais para URLs, portas e protocolos específicos.
 
Numa implementação em que um uplinks de proxy transparentes para um servidor proxy tradicionais, tem de permitir URLs e portas específicas para ambos [entrada](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound) e [saída](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound) comunicação. Estes incluem portas e URLs para identidade, sindicação do marketplace, patch e atualização, registo e dados de utilização.

## <a name="next-steps"></a>Passos Seguintes
[Conectividade de limite](azure-stack-border-connectivity.md)