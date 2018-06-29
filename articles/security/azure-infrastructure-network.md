---
title: Arquitetura de rede do Azure
description: Este artigo fornece uma descrição geral da rede de infraestrutura do Microsoft Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 67781f196b445c9330e0dcb1fc7d8b0a1a53cbc0
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102370"
---
# <a name="azure-network-architecture"></a>Arquitetura de rede do Azure
A arquitetura de rede do Azure segue uma versão modificada do setor padrão distribuição/Core/acesso modelo, com camadas de hardware distintos. As camadas incluem:

- Core (Routers do Centro de dados)
- Distribuição (Routers de acesso e a agregação de secretária L2) - a camada de distribuição separa L3 encaminhamento da mudança L2
- Acesso (comutadores de anfitrião de secretária L2)

A arquitetura de rede tem dois níveis de camada 2 comutadores, uma camada agregar o tráfego a partir do outro e a camada 2 ciclos para incorporar redundância. Isto proporciona vantagens, como um mais flexível VLAN os requisitos de espaço e melhora a porta de dimensionamento. A arquitetura mantém L2 e L3 distintos, que permite a utilização de hardware em cada uma das camadas de distinct na rede e minimiza falhas na camada de um de afetarem a outros camada (s). Permite a utilização de ramais da partilha, tais como a conectividade com a infraestrutura de L3 de recursos.

## <a name="network-configuration"></a>Configuração da rede
A arquitetura de rede de um cluster do Microsoft Azure num centro de dados consiste nos seguintes dispositivos:

- Routers (Centro de dados, o Router de acesso e Routers de folha de limite)
- Comutadores (agregação e a parte superior do comutadores Rack)
- Digi CMs
- PDUs ou Nucleons

A figura abaixo fornece uma descrição geral de alto nível da arquitetura de rede do Azure dentro de um cluster. O Azure tem duas arquiteturas separadas. Alguns clientes do Azure existentes e serviços partilhados de residir na arquitetura do LAN predefinido (DLA) enquanto regiões novo e os clientes virtuais serão acedidos através de arquitetura de Quantum 10 (Q10). A arquitetura DLA é uma estrutura de árvore tradicional com routers do Active Directory acesso passivo e segurança ACLs aplicadas para os routers de acesso. A arquitetura de 10 Quantum é uma estrutura de clos/mesh de routers onde ACLs não são aplicadas nos routers mas abaixo através do Software carga balanceamento (SLB) de encaminhamento ou VLANs definidas por software.

![Rede do Azure][1]

### <a name="quantum-10-devices"></a>Dispositivos de quantum 10
A estrutura de Quantum 10 efetua a propagação de mudança de camada 3 através de vários dispositivos de uma estrutura CLOS/mesh. As vantagens do Q10 design incluem maior capacidade e maior capacidade de dimensionar a infraestrutura de rede existente. A estrutura emprega routers de folha de limite, spine comutadores e superior de bastidor Routers para passar o tráfego para clusters em várias rotas permite tolerância a falhas. Serviços de segurança, tais como a tradução de endereços de rede são processadas através do balanceamento de carga de Software em vez de em dispositivos de hardware.

### <a name="access-routers"></a>Routers de acesso
Os routers de distribuição/acesso L3 (ARs) execute a funcionalidade principal de encaminhamento para as camadas de distribuição e acesso. Estes dispositivos são implementados como um par e são o gateway predefinido para sub-redes. Cada par de AR pode suportar vários pares de comutador de agregação L2, dependendo da capacidade. O número máximo depende a capacidade de dispositivo, bem como os domínios de falha. Uma típica capacidade esperada com base no número seria três pares de comutador de agregação L2 por par de AR.

### <a name="l2-aggregation-switches"></a>L2 Comutadores de agregação  
Estes dispositivos servem como um ponto de agregação para o tráfego de L2. Estão a camada de distribuição para os recursos de infraestrutura L2 e pode processar grandes quantidades de tráfego. Porque estes dispositivos agregar o tráfego, 802.1 q é necessária a funcionalidade e as tecnologias de largura de banda alta, tais como a agregação de porta e 10GE são utilizadas.

### <a name="l2-host-switches"></a>L2 Comutadores de anfitrião
Ligam anfitriões diretamente a estes comutadores. Podem ser comutadores eletrodomésticos montados ou implementações de chassis. O 802.1 q padrão permite a designação de uma VLAN como uma VLAN nativa, encará-los que VLAN como normal pacotes Ethernet (requeira). Em circunstâncias normais, frames Jumbo no VLAN nativo são transmitidas e recebidos requeira num 802.1 q a porta de ramal. Esta funcionalidade foi concebida para migração para compatibilidade com não e 802.1 q-802.1 q dispositivos compatíveis com. Nesta arquitetura, a infraestrutura de rede utiliza a VLAN nativa.

Esta arquitetura Especifica um padrão para a seleção de VLAN nativo que assegura, sempre que possível, que os dispositivos de AR têm uma VLAN nativa exclusiva para cada ramal e L2Aggregation para ramais de L2Aggregation. L2Aggregation para ramais de comutador de L2Host tem uma VLAN nativa não predefinidas.

### <a name="link-aggregation-8023ad"></a>Agregação de ligação (802.3 AD)
Agregação de ligação (desfasamento) permite várias ligações individuais para ser agrupadas e tratada como uma única ligação lógica. O número utilizado para designar interfaces de canal de porta tem de ser padronizado para tornar mais fácil a depuração operacional, o resto da rede irá utilizar o mesmo número em ambas as extremidades de um canal de porta. Isto requer um padrão para determinar o fim do canal de porta a utilizar para definir o número seguinte disponível.

Os números especificados para o L2Agg ao comutador L2Host são os números de porta de canal utilizados no lado L2Agg. Porque o intervalo de números é mais limitado no lado do L2Host, o padrão é utilizar números 1 e 2 no lado do L2Host referir-se para o canal de porta Ir para o lado "a" e "b", respetivamente.

### <a name="vlans"></a>VLANs
A arquitetura de rede utiliza VLANs para servidores de grupo em conjunto para um único domínio de difusão. Números VLAN 802.1 q conformidade com as normas que suporta as VLANs numeradas 1 – 4094.

### <a name="customer-vlans"></a>Cliente VLANS
Os clientes têm várias opções de implementação de VLAN podem implementar através do Portal do Azure para satisfazer as necessidades de separação e arquitetura da sua solução. Estas soluções são implementadas através de máquinas virtuais. Para referência cliente exemplos de arquitetura visitam [arquiteturas de referência de Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/).

### <a name="edge-architecture"></a>Arquitetura de limite
Os datacenters do Azure são criados com base no infraestruturas de rede altamente redundantes e bem aprovisionada. Redes dentro dos centros de dados do Azure são implementadas com "necessário e um" (N + 1) arquiteturas de redundância ou superior. Funcionalidades de ativação pós-falha completa dentro e entre centros de dados ajudam a garantir a disponibilidade de rede e o serviço. Externamente, centros de dados são servidos pelo circuitos de rede dedicado, largura de banda alta redundantly ligar propriedades com mais 1200 fornecedores de serviços Internet global em vários pontos de peering, fornecendo que excedam 2.000 Gbps da capacidade de limite potenciais através da rede.

Filtragem routers na camada de limite e acesso de rede do Microsoft Azure fornecem segurança bem estabelecida ao nível do pacote para impedir que as tentativas não autorizadas a ligar ao Azure. Ajudam a garantir que o conteúdo real dos pacotes de conter dados no formato esperado e está em conformidade com o esquema de comunicação cliente/servidor esperado. Azure implementa uma arquitetura em camadas, constituída pelo seguinte segregação de rede e os componentes de controlo de acesso:

- Routers de contorno - segregar o ambiente da aplicação da Internet. Routers de limite foram concebidos para fornecer proteção de software spoof e limitar o acesso através de listas de controlo de acesso (ACL).
- Routers de distribuição (acesso) - routers de acesso são concebidos para permitir a que Microsoft apenas aprovados endereços IP, fornecer ligações de spoofing de antimalware e estabelecidas utilizando ACLs.

### <a name="a10-ddos-mitigation-architecture"></a>A10 Arquitetura de mitigação de DDOS
Ataques denial of service continuar a apresentar uma ameaça real para a fiabilidade dos serviços online da Microsoft. Como ataques de se tornar mais direcionados, mais sofisticadas e como serviços da Microsoft em mais diferentes geografias, tornar-se a capacidade para fornecer mecanismos eficazes para identificar e minimizar o impacto de ataques é uma prioridade alta.

Os detalhes seguintes explicam como o sistema de mitigação A10 DDOS é implementado numa perspetiva de arquitetura de rede.  
Microsoft Azure utiliza A10 dispositivos de rede ao DCR (Datacenter Router) que fornece deteção automática e mitigação. A solução de A10 utiliza a monitorização de rede do Azure para pacotes de rede de fluxo de exemplo e determinar se existe um ataque. Depois de detetada ataque, A10 dispositivos são utilizados como scrubbers para mitigar ataques. Depois de mitigação, mais limpo tráfego é permitido no Centro de dados do Azure diretamente a partir de DCR. A solução de A10 é utilizada para proteger a infraestrutura de rede do Azure.

Proteção DDoS na solução A10 incluem:

- Proteção de inundar UDP IPv4 e IPv6
- ICMP IPv4 e IPv6 inundar proteção
- TCP IPv4 e IPv6 inundar proteção
- Proteção de ataque de TCP SIN para IPv4 e IPv6
- Ataque de fragmentação

> [!NOTE]
> Proteção DDoS é fornecida por predefinição para todos os clientes do Azure.
>
>

## <a name="network-connection-rules"></a>Regras de ligação de rede
Azure implementa routers de limite na sua rede, que fornecem segurança ao nível do pacote para impedir que as tentativas não autorizadas a ligar ao Microsoft Azure. Estes Certifique-se de que o conteúdo real dos pacotes de conter dados no formato esperado e estar em conformidade com o esquema de comunicação cliente/servidor esperado.

Routers de limite segregar o ambiente da aplicação da Internet. Routers de limite foram concebidos para fornecer proteção de software spoof e limitar o acesso através de listas de controlo de acesso (ACL). Estes routers de limite são configurados utilizando uma abordagem em camadas de ACL para protocolos de rede de limite que têm permissão para os routers de limite de trânsito e acesso a routers.

Dispositivos de rede são posicionados nas localizações de acesso e o limite e atuam como pontos de limites, onde são aplicados filtros de entrada e/ou de saída.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Funcionalidades de segurança da base de dados de SQL do Microsoft Azure](azure-infrastructure-sql.md)
- [Gestão e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização da infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Integridade da infraestrutura do Azure](azure-infrastructure-integrity.md)
- [Proteção de dados de cliente no Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-network/network-arch.png
