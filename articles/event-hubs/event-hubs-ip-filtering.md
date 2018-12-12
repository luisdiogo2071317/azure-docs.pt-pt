---
title: Restringir o acesso com filtros IP - Event Hubs do Azure | Documentos da Microsoft
description: Utilize o IP de filtragem para bloquear as conexões de endereços IP específicos para os Hubs de eventos do Azure.
services: event-hubs
documentationcenter: ''
author: spelluru
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.custom: seodec18
ms.topic: article
ms.date: 12/06/2018
ms.author: spelluru
ms.openlocfilehash: d21bf32ce804d2c8f6177eb7f789474bc41c9733
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53087512"
---
# <a name="restrict-access-to-azure-event-hubs-using-ip-filters"></a>Restringir o acesso ao utilizar os filtros IP de Event Hubs do Azure
Para cenários em que os Hubs de eventos do Azure deve ser apenas acessíveis a partir de certos sites conhecidos, o *filtro IP* funcionalidade permite-lhe configurar regras para rejeitar ou aceitar o tráfego proveniente de endereços de IPv4 específicos. Por exemplo, estes endereços podem ser de um gateway empresarial do NAT.

## <a name="when-to-use"></a>Quando utilizar

Utilizam dois importantes casos em que é útil bloquear os Hubs de eventos para determinados endereços IP são os seguintes:

- Os hubs de eventos devem receber o tráfego apenas a partir de um especificado intervalo de endereços IP e rejeitar tudo o resto. Por exemplo, estiver a utilizar os Hubs de eventos com [Azure Express Route] [ express-route] criar ligações privadas para a sua infraestrutura no local. 
- Precisa recusar tráfego a partir de endereços IP que tenham sido identificados como sendo suspeitas pelo administrador do Hubs de eventos.

## <a name="how-filter-rules-are-applied"></a>Como são aplicadas as regras de filtro

As regras de filtro IP são aplicadas ao nível do espaço de nomes de Hubs de eventos. Por conseguinte, as regras são aplicadas a todas as ligações de clientes usando qualquer protocolo suportado.

Qualquer tentativa de ligação de um endereço IP que corresponde que uma regra IP rejeitando no espaço de nomes de Hubs de eventos é rejeitada como não autorizado. A resposta não menciona a regra IP.

## <a name="default-setting"></a>Definição predefinida

Por predefinição, o **filtro IP** grelha no portal dos Hubs de eventos está vazia. Essa configuração padrão significa que o seu hub de eventos aceita ligações a partir de qualquer endereço IP. Essa configuração padrão é equivalente a uma regra que aceita o intervalo de endereços IP 0.0.0.0/0.

## <a name="ip-filter-rule-evaluation"></a>Avaliação da regra de filtro IP

Regras do filtro de IP serão aplicadas por ordem e a primeira regra que corresponde ao endereço IP determina a ação de aceitar ou rejeitar.

Por exemplo, se pretender aceitar endereços no 70.37.104.0/24 intervalo e rejeitar tudo o resto, a primeira regra na grade deve aceitar o endereço intervalo 70.37.104.0/24. A seguinte regra deve rejeitar a todos os endereços mediante a utilização do intervalo 0.0.0.0/0.

> [!NOTE]
> Rejeitar endereços IP pode impedir que outros serviços do Azure (por exemplo, o Azure Stream Analytics, máquinas virtuais do Azure ou o Device Explorer no portal) interagir com os Hubs de eventos.

### <a name="creating-an-ip-filter-rule-with-azure-resource-manager-templates"></a>Criar uma regra de filtro IP com modelos Azure Resource Manager

> [!IMPORTANT]
> Redes virtuais são suportadas no **padrão** e **dedicado** escalões de Hubs de eventos. Não é suportada no escalão básico. 

O modelo do Resource Manager seguinte permite adicionar uma regra de filtro IP para um espaço de nomes de Hubs de eventos existente.

Parâmetros do modelo:

- **ipFilterRuleName** tem de ser uma cadeia exclusiva, maiúsculas de minúsculas, de alfanumérica, até 128 carateres de comprimento.
- **ipFilterAction** está **rejeitar** ou **Accept** como a ação a aplicar a regra de filtro IP.
- **ipMask** é um único endereço IPv4 ou um bloco de endereços IP na notação CIDR. Por exemplo, no CIDR notação 70.37.104.0/24 representa os 256 endereços IPv4 de 70.37.104.0 para 70.37.104.255, com o que indica o número de bits de prefixo significativo para o intervalo de 24.

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

Para obter acesso omezující aos Hubs de eventos para redes virtuais do Azure, consulte a seguinte hiperligação:

- [Pontos finais de serviço de rede virtual para os Hubs de eventos][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: event-hubs-service-endpoints.md