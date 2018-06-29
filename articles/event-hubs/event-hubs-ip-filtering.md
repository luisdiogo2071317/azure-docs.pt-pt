---
title: Filtros de ligação do IP de Hubs de eventos do Azure | Microsoft Docs
description: Utilize IP filtragem às ligações do bloco de endereços IP específicos para os Event Hubs do Azure.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: 425a5b641fbfd2e52e1294c6317b51ff2a584aa3
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036394"
---
# <a name="use-ip-filters"></a>Utilizar filtros de IP

Para cenários em que o Event Hubs do Azure só é acessível a partir do determinados sites bem conhecidos, o *filtro IP* funcionalidade permite-lhe configurar regras para rejeitar ou aceitar o tráfego proveniente de endereços IPv4 específicos. Por exemplo, estes endereços podem ser de um gateway empresarial do NAT.

## <a name="when-to-use"></a>Quando utilizar

Utilizam dois importantes casos em que é útil bloquear pontos finais de Event Hubs para determinados endereços IP são os seguintes:

- Os hubs de eventos devem receber o tráfego apenas a partir de um especificado intervalo de endereços IP e rejeitar tudo o resto. Por exemplo, estiver a utilizar os Hubs de eventos com [Azure Express Route] [ express-route] para criar ligações privadas à sua infraestrutura no local. 
- Terá de rejeitar o tráfego de endereços IP que tenham sido identificados como suspeita pelo administrador do Event Hubs.

## <a name="how-filter-rules-are-applied"></a>Como são aplicadas as regras de filtro

São aplicadas as regras de filtro IP ao nível do espaço de nomes de Event Hubs. Por conseguinte, as regras são aplicadas a todas as ligações de clientes que utilizam os protocolos suportados.

Qualquer tentativa de ligação de um endereço IP que corresponde que uma regra IP rejecting no espaço de nomes de Event Hubs é rejeitada como não autorizado. A resposta não menciona a regra IP.

## <a name="default-setting"></a>Definição predefinida

Por predefinição, o **filtro IP** grelha no portal para os Event Hubs está vazia. Esta predefinição significa que o seu hub de eventos aceita ligações a partir de qualquer endereço IP. Esta predefinição é equivalente a uma regra que aceita o intervalo de endereços IP 0.0.0.0/0.

## <a name="ip-filter-rule-evaluation"></a>Avaliação da regra de filtro IP

Regras do filtro de IP serão aplicadas por ordem e a primeira regra que corresponde ao endereço IP determina a ação de aceitar ou rejeitar.

Por exemplo, se pretender aceitar endereços 70.37.104.0/24 o intervalo e rejeitar tudo o resto, a primeira regra na grelha deve aceitar o endereço intervalo 70.37.104.0/24. A seguinte regra deve rejeitar todos os endereços, utilizando o intervalo 0.0.0.0/0.

> [!NOTE]
> Rejeitar endereços IP pode impedir que outros serviços do Azure (por exemplo, o Azure Stream Analytics, Virtual Machines do Azure ou o Explorador do dispositivo no portal) interagir com os Event Hubs.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Criar uma regra de rede virtual com modelos Azure Resource Manager

O modelo do Resource Manager seguinte permite adicionar uma regra de rede virtual para um espaço de nomes de Event Hubs existente.

Parâmetros do modelo:

- **ipFilterRuleName** tem de ser uma cadeia exclusiva, sensível, alfanumérica, até 128 carateres de comprimento.
- **ipFilterAction** está **rejeitar** ou **aceitar** como a ação a aplicar a regra de filtro IP.
- **ipMask** é um único endereço IPv4 ou um bloco de endereços IP na notação CIDR. Por exemplo, no CIDR notação 70.37.104.0/24 representa os endereços IPv4 256 70.37.104.0 para 70.37.104.255, com que indica o número de bits de prefixo significativo para o intervalo de 24.

```json
{  
   "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{     
          "namespaceName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the namespace"
             }
          },
          "ipFilterRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "ipFilterAction":{  
             "type":"string",
             "allowedValues": ["Reject", "Accept"],
             "metadata":{  
                "description":"IP Filter Action"
             }
          },
          "IpMask":{  
             "type":"string",
             "metadata":{  
                "description":"IP Mask"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('ipFilterRuleName'))]",
            "type": "Microsoft.EventHub/Namespaces/IPFilterRules",
            "properties": {
                "FilterName":"[parameters('ipFilterRuleName')]",
                "Action":"[parameters('ipFilterAction')]",              
                "IpMask": "[parameters('IpMask')]"
            }
        } 
    ]
}
```

Para implementar o modelo, siga as instruções para [do Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Passos Seguintes

Para obter acesso restrições para os Event Hubs para redes virtuais do Azure, consulte a seguinte hiperligação:

- [Pontos finais do serviço de rede virtual para os Event Hubs][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: event-hubs-service-endpoints.md