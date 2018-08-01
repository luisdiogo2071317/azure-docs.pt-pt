---
title: incluir ficheiro
description: incluir ficheiro
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 7/30/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: e23579479c61810d651bebae7b486b53aaaf0d42
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39361370"
---
### <a name="what-is-azure-firewall"></a>O que é o Azure Firewall?

O Azure Firewall é um serviço de segurança de rede gerido e com base na cloud que protege os recursos da Rede Virtual do Azure. É um totalmente com monitoração de estado firewall-como-serviço com elevada disponibilidade incorporada e a escalabilidade da cloud sem restrições. Pode criar, impor e registar centralmente políticas de conectividade de rede e aplicações entre subscrições e redes virtuais. Firewall do Azure está atualmente em pré-visualização pública.

### <a name="what-capabilities-are-supported-in-the-azure-firewall-public-preview-release"></a>Que capacidades são suportadas na versão de pré-visualização pública do Firewall do Azure?  

* Firewall com monitoração de estado como um serviço
* Elevada disponibilidade incorporada com escalabilidade da cloud sem restrições
* Filtragem FQDN 
* Regras de filtragem de tráfego de rede
* Suporte SNAT de saída
* Criar centralmente, impor e políticas de conectividade de rede e aplicativos de registo em VNETs e subscrições do Azure
* Integração total com o Azure Monitor para fins de registo e análise 

### <a name="how-can-i-join-the-azure-firewall-public-preview"></a>Como é possível associar a pré-visualização pública de Firewall do Azure?

Firewall do Azure está atualmente uma pré-visualização pública gerida que pode participar em utilizando o comando do PowerShell Register-AzureRmProviderFeature conforme explicado na documentação da pré-visualização pública do Azure Firewall.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Quais são os preços para o Firewall do Azure?

Firewall do Azure tem um custo fixo + custos de variável. Os preços são abaixo, e eles são ainda mais um desconto de 50% durante a pré-visualização pública.

* Taxa fixa: $1.25/firewall/hour
* Taxa variável: $0.03/ GB processado pelo firewall (entrada ou saída).

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>O que é o modelo de implementação típica para o Firewall do Azure?

Embora seja possível implantar o Firewall do Azure em qualquer VNET, os clientes normalmente seriam implantar o Firewall do Azure numa central VNET e configurar o peering entre outras VNETs a ele num modelo de Hub e Spoke. Em seguida, pode definir a rota predefinida das VNETs em modo de peering para apontar para esta VNET central do Firewall.

### <a name="how-can-i-install-the-azure-firewall"></a>Como posso instalar o Firewall do Azure?

Pode configurar a Firewall do Azure através do portal do Azure, PowerShell, REST API ou modelos. Ver [Tutorial: implementar e configurar a Firewall do Azure no portal do Azure](../articles/firewall/tutorial-firewall-deploy-portal.md) para obter instruções passo a passo.

### <a name="what-are-some-azure-firewall-concepts"></a>Quais são alguns conceitos de Firewall do Azure?

Firewall do Azure oferece suporte a regras e coleções de regras. Uma coleção de regras é um conjunto de regras que partilham a mesma ordem e a prioridade. Coleções de regras são executadas na ordem de sua prioridade, coleções de regras de rede são de prioridade superior à coleções de regras de aplicação, todas as regras são de terminação.
Existem dois tipos de coleções de regras:

* Regras de aplicações: permite-lhe configurar nomes de domínio completamente qualificado (FQDN) que podem ser acedidos a partir de uma sub-rede. 
* Regras de rede: permite-lhe configurar regras que contém o endereço de origem, protocolo, porta de destino e endereço de destino. 

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>O Firewall do Azure suporta a filtragem de tráfego de entrada?

Oferece suporte a pré-visualização pública do Azure do Firewall, filtragem de saída apenas. Proteção para os protocolos não-HTTP/S de entrada (ex: RDP, SSH, FTP) provisoriamente o envio está planejado para o Azure de Firewall de GA.  
 
### <a name="what-logginganalytics-is-supported-by-the-azure-firewall"></a>O registo/analytics é suportado pela Firewall do Azure?

Firewall do Azure está integrado com o Azure Monitor para visualizar e analisar os registos de Firewall. Os registos podem ser enviados para o Log Analytics, o armazenamento do Azure ou o Hub de eventos. Eles podem ser analisados no Log Analytics ou por diferentes ferramentas como o Excel e Power BI. Ver [Tutorial: registos de Firewall de Azure Monitor](../articles/firewall/tutorial-diagnostics.md) para obter mais detalhes.

### <a name="how-does-azure-firewall-work-relative-to-existing-like-nvas-in-the-marketplace"></a>Como funciona o Firewall do Azure em relação ao existente, como as NVAs no marketplace?

Firewall do Azure é um serviço de firewall básica que pode atender a determinados cenários de cliente. Espera-se que terá uma combinação de NVAs de terceiros e a Firewall do Azure. Funcionar melhor em conjunto é uma prioridade de núcleo.
 
### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>O que é a diferença entre o WAF do Gateway de aplicação e o Firewall do Azure?

A Firewall de aplicações Web (WAF) é um recurso do Gateway de aplicação que fornece proteção centralizada de entrada de seus aplicativos web de exploits e vulnerabilidades comuns. Firewall do Azure fornece proteção ao nível da rede de saída para todas as portas e protocolos e a proteção ao nível da aplicação de saída HTTP/S. Proteção de entrada para os protocolos de não-HTTP/S (por exemplo, o protocolo RDP, SSH, FTP) provisoriamente o envio está planejada para GA do Firewall do Azure.

### <a name="what-is-the-difference-between-network-security-groups-nsg-and-azure-firewall"></a>O que é a diferença entre grupos de segurança de rede (NSG) e o Firewall do Azure?

O serviço de Firewall do Azure complementa a funcionalidade de grupo de segurança de rede e em conjunto fornece melhor segurança de rede de defesa em profundidade. Os NSGs fornecem tráfego de camada de rede distribuída filtragem para limitar o tráfego para recursos dentro de redes virtuais em cada subscrição.  Firewall do Azure é uma rede totalmente com monitoração de estado e centralizada firewall como-serviço, oferecendo proteção ao nível da rede e a aplicação em diferentes subscrições e redes virtuais (VNets). 

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Como posso configurar o Firewall do Azure com meus pontos finais de serviço?

Para obter acesso seguro aos serviços de PaaS, recomendamos que pontos finais de serviço. Os clientes de Firewall do Azure podem optar por ativar pontos finais de serviço na sub-rede de Firewall do Azure e desativá-la em VNETs spoke ligada para se beneficiar dos dois recursos – segurança de ponto final de serviço e o registo central para todo o tráfego.

### <a name="what-are-the-known-service-limits"></a>Quais são os limites de serviço conhecidas?

* Firewall do Azure tem um limite não restritivo de 1000 TB/firewall/mês. 
* Firewall do Azure que está em execução numa central VNET é sujeitos a limitações de peering de VNET: máximo de 50 spoke VNETs.  
* Firewall do Azure não funciona com o global peering, pelo que deve ter, pelo menos, uma implementação de firewall por região.
* Regras do firewall do Azure suporta 10 mil aplicações e as regras de rede de 10 mil.