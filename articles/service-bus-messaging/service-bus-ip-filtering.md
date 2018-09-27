---
title: Filtros de ligação de IP do Service Bus do Azure | Documentos da Microsoft
description: Como utilizar o IP de filtragem para bloquear as conexões de endereços IP específicos para o Azure Service Bus.
services: service-bus
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: clemensv
ms.openlocfilehash: ccb759a9151d734aa99a6f6b9c68e6072874dd84
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394833"
---
# <a name="use-ip-filters"></a>Utilizar filtros de IP

Para cenários nos quais o Azure Service Bus só é acessível a partir de certos sites conhecidos, o *filtro IP* funcionalidade permite-lhe configurar regras para rejeitar ou aceitar o tráfego proveniente de endereços de IPv4 específicos. Por exemplo, estes endereços podem ser de um gateway empresarial do NAT.

## <a name="when-to-use"></a>Quando utilizar

Existem dois casos de uso específicos em que é útil bloquear os pontos finais do Service Bus para determinados endereços IP:

- Service Bus deve receber o tráfego apenas a partir de um especificado intervalo de endereços IP e rejeitar tudo o resto. Por exemplo, estiver a utilizar o Service Bus com [Azure Express Route] [ express-route] criar ligações privadas para a sua infraestrutura no local.
- Precisa recusar tráfego a partir de endereços IP que tenham sido identificados como sendo suspeitas pelo administrador do Service Bus.

## <a name="how-filter-rules-are-applied"></a>Como são aplicadas as regras de filtro

As regras de filtro IP são aplicadas ao nível do espaço de nomes do Service Bus. Por conseguinte, as regras são aplicadas a todas as ligações de clientes usando qualquer protocolo suportado.

Qualquer tentativa de ligação de um endereço IP que corresponde que uma regra IP rejeitando no espaço de nomes do Service Bus é rejeitada como não autorizado. A resposta não menciona a regra IP.

## <a name="default-setting"></a>Definição predefinida

Por predefinição, o **filtro IP** grelha no portal do Service Bus está vazia. Essa configuração padrão significa que o seu espaço de nomes aceita ligações de qualquer endereço IP. Essa configuração padrão é equivalente a uma regra que aceita o intervalo de endereços IP 0.0.0.0/0.

## <a name="ip-filter-rule-evaluation"></a>Avaliação da regra de filtro IP

Regras do filtro de IP serão aplicadas por ordem e a primeira regra que corresponde ao endereço IP determina a ação de aceitar ou rejeitar.

Por exemplo, se pretender aceitar endereços no 70.37.104.0/24 intervalo e rejeitar tudo o resto, a primeira regra na grade deve aceitar o endereço intervalo 70.37.104.0/24. A seguinte regra deve rejeitar a todos os endereços mediante a utilização do intervalo 0.0.0.0/0.

> [!NOTE]
> Rejeitar endereços IP pode impedir que outros serviços do Azure (por exemplo, o Azure Stream Analytics, máquinas virtuais do Azure ou o Device Explorer no portal) interagir com o Service Bus.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Criar uma regra de rede virtual com modelos Azure Resource Manager

O modelo do Resource Manager seguinte permite adicionar uma regra de rede virtual para um espaço de nomes do barramento de serviço existente.

Parâmetros do modelo:

- **ipFilterRuleName** tem de ser uma cadeia de caracteres exclusiva, maiúsculas de minúsculas, de alfanumérica até 128 carateres de comprimento.
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
            "type": "Microsoft.ServiceBus/Namespaces/IPFilterRules",
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

Para obter acesso omezující ao Service Bus para redes virtuais do Azure, consulte a seguinte hiperligação:

- [Pontos finais de serviço de rede virtual para o Service Bus][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services