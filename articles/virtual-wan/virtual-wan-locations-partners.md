---
title: WAN Virtual do Azure para parceiros locais | Documentos da Microsoft
description: Este artigo contém uma lista de parceiros de WAN Virtual do Azure e locais de hub
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: 76542392522ff14642ed7ccc6de8ed543d66513f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984998"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Parceiros WAN virtual e locais de virtual hub

Este artigo fornece informações sobre o Virtual WAN suportada regiões e parceiros preferenciais para conectividade para o Virtual Hub.

A WAN Virtual do Azure é um serviço de rede que fornece conectividade ramo a ramo otimizada e automatizada através do Azure. A WAN Virtual permite-lhe ligar e configurar dispositivos de ramo para comunicar com o Azure. Isso pode ser feito manualmente ou através de dispositivos do fornecedor preferencial através de um parceiro preferido Virtual WAN. Utilizar dispositivos de parceiro preferido, permite que a facilidade de uso, a simplificação da conectividade e gestão de configuração. De maneira automatizada para o Hub Virtual, é estabelecer a conectividade do dispositivo no local. Um hub virtual é uma rede virtual gerida pela Microsoft. O hub contém vários pontos finais de serviço para ativar a conectividade da rede no local (vpnsite). Só pode ter um hub por região.

## <a name="regions"></a>Regiões

A lista de regiões suportadas e disponíveis são os seguintes:

|Região geopolítica | Regiões do Azure|
|---|---|
|América do Norte | E.U.A. Leste, E.U.A. Oeste, E.U.A. Leste 2, E.U.A. Oeste 2, E.U.A. Central, E.U.A. Centro-Sul, E.U.A. Centro-Norte, E.U.A. Centro-Oeste, Canadá Central, Canadá Leste |
|América do Sul |Sul do Brasil |
| Europa | França Central, Sul de França, Europa do Norte, Europa Ocidental, Oeste do Reino Unido, Sul do Reino Unido |
| Ásia | Ásia Oriental, Sudeste Asiático |
| Japão  | Oeste do Japão, Leste do Japão |
| Austrália | Sudeste da Austrália, Leste da Austrália | 
| Governo da Austrália | Austrália Central, Austrália Central 2 |
| Índia | Oeste da Índia, Índia Central, Sul da Índia |
| Coreia do Sul | Centro da Coreia, Sul da Coreia do Sul | 

## <a name="automation-from-connectivity-partners"></a>Automatização de parceiros de conectividade

Esta secção descreve os detalhes de alto nível de automatização a partir de fornecedores de conectividade.

Os dispositivos que se ligam a WAN Virtual do Azure têm automatização incorporada para se ligar. Isso normalmente é definido se na gestão de dispositivos da interface do Usuário (ou equivalente), que configura o gerenciamento de configuração e de conectividade entre o dispositivo de ramo VPN para um ponto de extremidade de VPN de Hub Virtual do Azure (gateway de VPN).

A automação de alto nível seguinte é configurada no Centro de consola/Gestão de dispositivos:

* Permissões adequadas para o dispositivo aceder ao grupo de recursos de WAN Virtual de Azure
* Carregando-a de dispositivo do ramo no WAN Virtual do Azure
* Transferência automática de informações de conectividade do Azure 
* Configuração do dispositivo de ramo no local 

Alguns parceiros de conectividade podem expandir a automatização para incluir a criação da VNet do Hub Virtual do Azure e o Gateway de VPN. Se quiser saber mais sobre a automatização, veja [configurar a automação – parceiros WAN](virtual-wan-configure-automation-providers.md).

## <a name="connectivity-through-preferred-partners"></a>Conectividade através de parceiros preferenciais

Se o seu parceiro de dispositivo de ramo não estiver listado na secção abaixo, contacte o seu fornecedor de dispositivo do ramo e tê-los, contacte-ao enviar um e-mail para azurevirtualwan@microsoft.com.

Pode verificar as ligações seguintes para obter mais informações sobre os serviços oferecidos pelos parceiros preferenciais. 

|Parceiros preferenciais|
|---|
|[Barracuda Networks](https://www.barracuda.com/AzurevWAN)|
| [Ponto de verificação](https://www.checkpoint.com/solutions/microsoft-azure-virtual-wan/) |
| [Citrix](https://www.citrix.com/global-partners/microsoft/sd-wan-for-azure-virtual-wan.html)|
|[Netfoundry](https://netfoundry.io/solutions/netfoundry-for-microsoft-azure-virtual-wan/)|
|[Da Palo Alto Networks](https://researchcenter.paloaltonetworks.com/2018/09/azure-vwan-integration/) |
|[Tecnologia de riverbed](https://www.riverbed.com/go/steelconnect-azurewan.html)|
|[Tecnologia de 128](https://www.128technology.com/partners/azure) |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Virtual WAN, consulte a [FAQ de WAN Virtual](virtual-wan-faq.md).

Para obter mais informações sobre como automatizar a conectividade WAN Virtual do Azure, consulte [parceiros WAN Virtual - como automatizar](virtual-wan-configure-automation-providers.md).
