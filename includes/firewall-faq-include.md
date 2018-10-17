---
title: incluir ficheiro
description: incluir ficheiro
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 10/15/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 1d52c9b3b9b91873bd6f34443f162c74d724a65c
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49367097"
---
### <a name="what-is-azure-firewall"></a>O que é o Azure Firewall?

O Azure Firewall é um serviço de segurança de rede gerido e com base na cloud que protege os recursos da Rede Virtual do Azure. É um totalmente com monitoração de estado firewall-como-serviço com elevada disponibilidade incorporada e a escalabilidade da cloud sem restrições. Pode criar, impor e registar centralmente políticas de conectividade de rede e aplicações entre subscrições e redes virtuais.

### <a name="what-capabilities-are-supported-in-azure-firewall"></a>Que capacidades são suportadas no Firewall do Azure?

* Firewall com monitorização de estado como serviço
* Elevada disponibilidade incorporada com escalabilidade da cloud sem restrições
* Filtragem FQDN
* Etiquetas FQDN
* Regras de filtragem de tráfego de rede
* Suporte SNAT de saída
* Suporte DNAT de entrada
* Criar centralmente, impor e políticas de conectividade de rede e aplicativos de registo em VNETs e subscrições do Azure
* Integração total com o Azure Monitor para fins de registo e análise

### <a name="what-is-the-pricing-for-azure-firewall"></a>Quais são os preços para o Firewall do Azure?

Firewall do Azure tem um custo fixo + custos variáveis:

* Taxa fixa: $1.25/firewall/hour
* Taxa variável: $0.03/ GB processado pelo firewall (entrada ou saída)

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>O que é o modelo de implementação típica para o Firewall do Azure?

Pode implantar o Firewall do Azure em qualquer rede virtual, mas os clientes normalmente implementação-la numa rede virtual central e configurar o peering entre outras redes virtuais ao mesmo num modelo de hub-and-spoke. Em seguida, pode definir a rota padrão das redes virtuais em modo de peering para apontar para esta rede virtual de central firewall.

### <a name="how-can-i-install-the-azure-firewall"></a>Como posso instalar o Firewall do Azure?

Pode configurar Firewall do Azure com o portal do Azure, PowerShell, REST API, ou com os modelos. Ver [Tutorial: implementar e configurar a Firewall do Azure no portal do Azure](../articles/firewall/tutorial-firewall-deploy-portal.md) para obter instruções passo a passo.

### <a name="what-are-some-azure-firewall-concepts"></a>Quais são alguns conceitos de Firewall do Azure?

Firewall do Azure oferece suporte a regras e coleções de regras. Uma coleção de regras é um conjunto de regras que partilham a mesma ordem e a prioridade. Coleções de regras são executadas na ordem de sua prioridade. Coleções de regras de rede são de prioridade superior à coleções de regras de aplicação e todas as regras são de terminação.

Existem dois tipos de coleções de regras:

* *Regras de aplicações*: permitem-lhe configurar nomes de domínio completamente qualificado (FQDN) que podem ser acedidos a partir de uma sub-rede.
* *Regras de rede*: permitem-lhe configurar as regras que contêm os endereços de origem, protocolos, portas de destino e endereços de destino.

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>O Firewall do Azure suporta a filtragem de tráfego de entrada?

Firewall do Azure suporta a filtragem de entrada e saída. Proteção de entrada é para os protocolos não-HTTP/S. Para protocolos FTP, RDP e SSH de exemplo.

### <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Os serviços de registo e análise são suportados pela Firewall do Azure?

Firewall do Azure está integrado com o Azure Monitor para visualizar e analisar os registos de firewall. Os registos podem ser enviados para o Log Analytics, o armazenamento do Azure ou os Hubs de eventos. Eles podem ser analisados no Log Analytics ou por diferentes ferramentas como o Excel e Power BI. Para obter mais informações, consulte [Tutorial: registos de Firewall de Azure Monitor](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Como Firewall do Azure funciona forma diferente de serviços existentes como NVAs no marketplace?

Firewall do Azure é um serviço de firewall básica que pode atender a determinados cenários de cliente. Espera-se que terá uma combinação de NVAs de terceiros e a Firewall do Azure. Funcionar melhor em conjunto é uma prioridade de núcleo.

### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>O que é a diferença entre o WAF do Gateway de aplicação e o Firewall do Azure?

A Firewall de aplicações Web (WAF) é um recurso do Gateway de aplicação que fornece proteção centralizada de entrada de seus aplicativos web de exploits e vulnerabilidades comuns. Firewall do Azure fornece proteção de entrada para os protocolos não-HTTP/S (por exemplo, RDP, SSH, FTP), a proteção de nível de rede de saída para todas as portas e protocolos e a proteção de ao nível da aplicação de saída HTTP/S.

### <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>O que é a diferença entre grupos de segurança de rede (NSGs) e o Firewall do Azure?

O serviço de Firewall do Azure complementa a funcionalidade de grupo de segurança de rede. Juntos, eles fornecem uma maior segurança de rede de "defesa em profundidade". Grupos de segurança de rede fornecem o tráfego de camada de rede distribuída filtragem para limitar o tráfego para recursos dentro de redes virtuais em cada subscrição. Firewall do Azure é uma rede totalmente com monitoração de estado e centralizada firewall como-serviço, que fornece proteção de nível de rede e de aplicativo em diferentes subscrições e redes virtuais.

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Como posso configurar o Firewall do Azure com meus pontos finais de serviço?

Para obter acesso seguro aos serviços de PaaS, recomendamos que pontos finais de serviço. Pode optar por ativar pontos finais de serviço na sub-rede de Firewall do Azure e desativá-las nas redes virtuais spoke ligada. Dessa forma se beneficia da segurança de ponto final funcionalidades - service e de registo central para todo o tráfego.

### <a name="how-can-i-stop-and-start-azure-firewall"></a>Como parar e iniciar a Firewall do Azure?

Pode utilizar o Azure PowerShell *desalocar* e *alocar* métodos.

Por exemplo:

```azurepowershell
# Stop an exisitng firewall

$azfw = Get-AzureRmFirewall -Name "FW Name” -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzureRmFirewall -AzureFirewall $azfw
```

```azurepowershell
#Start a firewall

$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzureRmPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzureRmFirewall -AzureFirewall $azfw
```

> [!NOTE]
> Deverá realocar um firewall e o IP público para o grupo de recursos originais e a subscrição.

### <a name="what-are-the-known-service-limits"></a>Quais são os limites de serviço conhecidas?

* Firewall do Azure tem um limite não restritivo de 1000 TB por firewall por mês.
* Uma instância do Firewall do Azure que está a executar numa rede virtual central tem limitações, com um máximo de 50 redes virtuais do spoke de peering de rede virtual.  
* Firewall do Azure não funciona com o global peering, pelo que deve ter, pelo menos, uma implementação de firewall por região.
* Firewall do Azure suporta regras de 10 mil de aplicações e as regras de rede de 10 mil.

### <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>Pode a Firewall do Azure numa rede virtual de hub para a frente e filtrar o tráfego de rede entre duas redes virtuais do spoke?

Sim, pode utilizar a Firewall do Azure numa rede virtual para o tráfego e filtro de rota entre a rede virtual do spoke dois hub. As sub-redes em cada uma das redes virtuais "spoke" tem de ter UDR apontando para o Firewall do Azure como um gateway predefinido para este cenário funcione corretamente.

### <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network"></a>Pode reencaminhar o Firewall do Azure e filtrar o tráfego de rede entre sub-redes na mesma rede virtual?

O tráfego entre sub-redes na mesma rede virtual ou numa rede virtual diretamente em modo de peering é encaminhado diretamente, mesmo que o UDR aponta para o Firewall do Azure como o gateway predefinido. O método recomendado para segmentação de rede interna é usar os grupos de segurança de rede. Para enviar tráfego de sub-rede para sub-rede para o firewall neste cenário, o UDR tem de conter o prefixo de rede de sub-rede de destino explicitamente em ambas as sub-redes.

### <a name="are-there-any-firewall-resource-group-restrictions"></a>Existem qualquer firewall restrições do grupo de recursos?

Sim. O firewall, sub-rede, VNet e o endereço IP público, que todos têm de estar no mesmo grupo de recursos.
