---
title: incluir ficheiro
description: incluir ficheiro
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 8/13/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: a63a12658bd0a4b4d018d51824af9814691a3cbf
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "40182147"
---
### <a name="what-is-azure-firewall"></a>O que é o Azure Firewall?

O Azure Firewall é um serviço de segurança de rede gerido e com base na cloud que protege os recursos da Rede Virtual do Azure. É um totalmente com monitoração de estado firewall-como-serviço com elevada disponibilidade incorporada e a escalabilidade da cloud sem restrições. Pode criar, impor e registar centralmente políticas de conectividade de rede e aplicações entre subscrições e redes virtuais. Firewall do Azure está atualmente em pré-visualização pública.

### <a name="which-capabilities-are-supported-in-the-azure-firewall-public-preview-release"></a>Que funcionalidades são suportadas na versão de pré-visualização pública do Firewall do Azure?  

* Firewall com monitorização de estado como serviço
* Elevada disponibilidade incorporada com escalabilidade da cloud sem restrições
* Filtragem FQDN 
* Regras de filtragem de tráfego de rede
* Suporte SNAT de saída
* A capacidade de criar centralmente, impor e iniciar as políticas de rede e aplicativos da conectividade entre subscrições do Azure e redes virtuais
* Integração total com o Azure Monitor para o registo e análise 

### <a name="how-can-i-join-the-azure-firewall-public-preview"></a>Como é possível associar a pré-visualização pública de Firewall do Azure?

Firewall do Azure está atualmente uma pré-visualização pública gerida que pode participar em através do comando Register-AzureRmProviderFeature PowerShell. Este comando é explicado na documentação da pré-visualização pública do Firewall do Azure.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Quais são os preços para o Firewall do Azure?

Firewall do Azure tem um custo fixo e variável. Os preços são os seguintes e ainda mais um desconto de 50% durante a pré-visualização pública.

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

Oferece suporte a pré-visualização pública do Azure do Firewall, filtragem de saída apenas. Proteção de entrada para os protocolos não-HTTP/S (por exemplo, RDP, SSH ou FTP) provisoriamente o envio está planejada para GA do Firewall do Azure.  
 
### <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Os serviços de registo e análise são suportados pela Firewall do Azure?

Firewall do Azure está integrado com o Azure Monitor para visualizar e analisar os registos de firewall. Os registos podem ser enviados para o Log Analytics, o armazenamento do Azure ou os Hubs de eventos. Eles podem ser analisados no Log Analytics ou por diferentes ferramentas como o Excel e Power BI. Para obter mais informações, consulte [Tutorial: registos de Firewall de Azure Monitor](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Como Firewall do Azure funciona forma diferente de serviços existentes como NVAs no marketplace?

Firewall do Azure é um serviço de firewall básica que pode atender a determinados cenários de cliente. Espera-se que terá uma combinação de NVAs de terceiros e a Firewall do Azure. Funcionar melhor em conjunto é uma prioridade de núcleo.
 
### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>O que é a diferença entre o WAF do Gateway de aplicação e o Firewall do Azure?

A Firewall de aplicações Web (WAF) é um recurso do Gateway de aplicação que fornece proteção centralizada de entrada de seus aplicativos web de exploits e vulnerabilidades comuns. Firewall do Azure fornece proteção de nível de rede de saída para todas as portas e protocolos e a proteção de nível de aplicativo para saída HTTP/S. Proteção de entrada para os protocolos de não-HTTP/S (por exemplo, o protocolo RDP, SSH, FTP) provisoriamente o envio está planejada para GA do Firewall do Azure.

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

### <a name="what-are-the-known-service-limits"></a>Quais são os limites de serviço conhecidas?

* Firewall do Azure tem um limite não restritivo de 1000 TB por firewall por mês. 
* Uma instância do Firewall do Azure que está a executar numa rede virtual central tem limitações, com um máximo de 50 redes virtuais do spoke de peering de rede virtual.  
* Firewall do Azure não funciona com o global peering, pelo que deve ter, pelo menos, uma implementação de firewall por região.
* Firewall do Azure suporta regras de 10 mil de aplicações e as regras de rede de 10 mil.