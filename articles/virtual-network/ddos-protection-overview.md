---
title: Descrição geral de padrão de proteção do Azure DDoS | Microsoft Docs
description: Saiba mais sobre o serviço de proteção do Azure DDoS.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2018
ms.author: jdial
ms.openlocfilehash: 705f69f9143e3d2b27a3099f340218aaa12931f8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
ms.locfileid: "31601904"
---
# <a name="azure-ddos-protection-standard-overview"></a>Descrição geral de DDoS proteção padrão do Azure

Distribuída ataques denial of service (DDoS distribuídos) são algumas das preocupações de disponibilidade e segurança maiores enfrentam os clientes que estão a mover as aplicações para a nuvem. Um ataque DDoS tenta a esgotar os recursos de uma aplicação, tornando a aplicação disponível para utilizadores legítimos. Ataques de DDoS podem ser segmentados em qualquer ponto final que está publicamente acessível através da internet.

Proteção DDoS do Azure, combinada com aplicação design as melhores práticas, fornecer defesa contra ataques de DDoS. Proteção DDos do Azure fornece os seguintes escalões de serviço:

- **Básico**: ativada automaticamente como parte da plataforma do Azure, sem encargos adicionais. A monitorização de tráfego sempre ativado e em tempo real mitigação de ataques de nível de rede comuns, fornecem as mesmas defesas utilizadas pelos serviços online da Microsoft. A escala completa de rede global do Azure pode ser utilizada para distribuir e mitigar o tráfego de ataque em regiões. Proteção é fornecida para IPv4 e IPv6 Azure [endereços IP públicos](virtual-network-public-ip-address.md).
- **Standard**: fornece capacidades de mitigação adicionais relativamente a camada de serviço básico que estão otimizados especificamente para recursos de rede Virtual do Azure. Padrão de proteção DDoS é simple ativar e não necessita de aplicação alterações. Políticas de proteção são otimizadas através de monitorização do tráfego dedicado e algoritmos do machine learning. As políticas são aplicadas para endereços IP públicos associados a recursos implementados em redes virtuais, tais como as instâncias de Balanceador de carga do Azure, o Gateway de aplicação do Azure e o Azure Service Fabric. Telemetria em tempo real está disponível nas vistas de monitorização do Azure durante um ataque e para o histórico. Proteção de camada de aplicação pode ser adicionada através do [Firewall de aplicações Web do Azure Application Gateway](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Proteção é fornecida para o IPv4 Azure [endereços IP públicos](virtual-network-public-ip-address.md).

![Padrão de proteção DDoS do Azure](./media/ddos-protection-overview/ddospic.png)

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>Tipos de ataques de DDoS atenua DDoS proteção padrão

Padrão de proteção DDoS pode mitigar os seguintes tipos de ataques:

- **Ataques volumetric**: objetivo do ataque é inundar a camada de rede com uma quantidade substancial de tráfego seemingly legítima. Inclui inundações UDP, inundações amplificação e outras inundações pacote falsificado. Padrão de proteção DDoS atenua estes potenciais ataques de gigabytes multi absorbing e limpeza, com uma escala de rede global do Azure, automaticamente.
- **Ataques de protocolo**: estes ataques compor um destino inacessível, por explorá uma vulnerabilidade na camada 3 e pilha de protocolo de camada 4. Inclui, ataques de uma inundação SIN, ataques de reflexão e outros ataques de protocolo. Padrão de proteção DDoS mitiga ataques, differentiating entre tráfego malicioso e legítimo, interagir com o cliente e a bloquear tráfego malicioso. 
- **Ataques de camada de recursos (aplicação)**: estes ataques de pacotes de aplicação web, para interromper a transmissão de dados entre anfitriões de destino. Os ataques incluem HTTP violações de protocolo, SQL Server injeção, processamento de scripts entre sites e outros ataques de camada 7. Utilizar o Azure [firewall de aplicações do Gateway de aplicação web](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), com DDoS proteção padrão, para fornecer defesa contra estes ataques. Também existem ofertas de firewall de aplicação web de terceiros disponíveis a [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall).

Padrão de proteção DDoS protege os recursos numa rede virtual, incluindo endereços IP públicos associados a máquinas virtuais, balanceadores de carga e gateways de aplicação. Quando conjugada com a firewall de aplicações do Gateway de aplicação web, DDoS proteção Standard pode fornecer completa camada 3 para a camada 7 capacidade de mitigação.

## <a name="ddos-protection-standard-features"></a>Funcionalidades DDoS proteção padrão

![Funcionalidade de DDoS](./media/ddos-protection-overview/ddosfeatures.png)

Padrão de proteção DDoS funcionalidades incluem:

- **Integração da plataforma nativa:** nativamente integrada no Azure. Inclui a configuração através do portal do Azure. Padrão de proteção DDoS compreende os recursos e a configuração do recurso.
- **Proteção de chave de ative:** simplificado configuração protege imediatamente todos os recursos numa rede virtual, assim que DDoS proteção padrão está ativada. Não é necessária nenhuma definição de utilizador ou intervenção. Padrão de proteção DDoS instantaneamente e automaticamente atenua o ataque, uma vez que é detetado.
- **A monitorização de tráfego sempre ativado:** seus padrões de tráfego de aplicação são monitorizados 24 horas por dia, 7 dias por semana, à procura de indicadores de ataques de DDoS. Mitigação é executada quando as políticas de proteção são excedidas.
- **Otimização adaptável:** a criação de perfis de tráfego inteligente aprende o tráfego da sua aplicação ao longo do tempo, seleciona e atualiza o perfil que é mais adequado para o seu serviço. O perfil ajusta como tráfego alterações ao longo do tempo.
- **A camada 3 para proteção de camada 7:** fornece proteção DDoS de pilha completa, quando utilizado com uma firewall de aplicação web.
- **Escala de mitigação extensas:** 60 através de tipos de ataque diferentes podem ser mitigados com capacidade global, para proteger contra os ataques de DDoS conhecidos maiores.
- **Ataques métricas:** Summarized métricas de cada ataque estão acessíveis por meio do Monitor do Azure.
- **Alerta de ataque:** alertas podem ser configurados no início e paragem de um ataque e ao longo da duração do ataque, através de métricas de ataque incorporada. Alertas de integram o seu software operacional, como a análise de registos do Microsoft Azure, o Splunk, o Storage do Azure, o E-Mail e o portal do Azure.
- **Custo garantia:** transferência de dados e os créditos de serviço de escalamento horizontal de aplicação para ataques de DDoS documentados.

## <a name="ddos-protection-standard-mitigation"></a>Mitigação DDoS proteção padrão

Padrão de proteção DDoS monitoriza a utilização de tráfego real e compara-constantemente relativamente a limiares definidos na política DDoS. Quando for excedido o limiar de tráfego, DDoS mitigação é iniciada automaticamente. Quando o tráfego devolve abaixo do limiar, é removida a mitigação.

![Mitigação](./media/ddos-protection-overview/mitigation.png)

Durante a mitigação, é redirecionado o tráfego enviado para o recurso protegido pelo serviço de proteção DDoS e são executadas verificações do várias, tais como as seguintes verificações:

- Certifique-se de pacotes está em conformidade com as especificações de internet e não são um formato incorreto.
- Interagir com o cliente para determinar se o tráfego é, potencialmente, um pacote a denuncia (por exemplo: SIN Auth ou o Cookie de SIN ou remover um pacote para a origem de retransmiti-lo).
- Pacotes de limite de taxa, se nenhum outro método de imposição podem ser executadas.

Proteção DDoS bloqueia o tráfego de ataque e reencaminha o tráfego restante para o destino pretendido. Dentro de alguns minutos de deteção de ataques, será notificado através de métricas de Monitor do Azure. Ao configurar o registo de telemetria DDoS proteção padrão, pode escrever os registos para as opções disponíveis para análise futura. Os dados métricos no Monitor do Azure para DDoS proteção padrão são mantidos durante 30 dias.

Microsoft tem parcerias com [BreakingPoint nuvem](https://www.ixiacom.com/products/breakingpoint-cloud) para criar uma interface onde pode gerar tráfego contra ativada para proteção DDoS os endereços IP públicos para simulações. A simulação de ponto de interrupção nuvem permite-lhe:

- Valide o Microsoft Azure DDoS proteção padrão protege os recursos do Azure contra ataques de DDoS
- Otimizar o seu processo de resposta a incidentes enquanto sob ataque DDoS
- Compatibilidade de DDoS do documento
- Preparar as equipas de segurança de rede

## <a name="next-steps"></a>Passos Seguintes

- [Configurar o padrão de proteção DDoS](manage-ddos-protection.md)