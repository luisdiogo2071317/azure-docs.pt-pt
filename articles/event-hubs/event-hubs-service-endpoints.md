---
title: Virtual rede pontos finais de serviço - Event Hubs do Azure | Documentos da Microsoft
description: Este artigo fornece informações sobre como ponto final de serviço de eventhub adda a uma rede virtual.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 18c63b9c16ed9e82972a933d2aee5990d2fa84ac
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53081579"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Utilizar pontos finais de serviço de rede Virtual com o Event Hubs do Azure

A integração do Hubs de eventos com [pontos finais de serviço de rede Virtual (VNet)] [ vnet-sep] permite o acesso seguro às capacidades de mensagens de cargas de trabalho como as máquinas virtuais que estão vinculadas a virtual redes, com o caminho do tráfego de rede que estão protegidas em ambas as extremidades. 

> [!IMPORTANT]
> Redes virtuais são suportadas no **padrão** e **dedicado** escalões de Hubs de eventos. Não é suportada no escalão básico. 

Depois de configurado para ser associada a pelo menos um ponto final do serviço de sub-rede virtual de rede, o respetivo dos Hubs de eventos espaço de nomes já não aceita tráfego de qualquer lugar, mas autorizado sub-redes nas redes virtuais. Da perspectiva de rede virtual, ligando um espaço de nomes de Hubs de eventos a um ponto de extremidade de serviço configura um túnel de rede isolado da sub-rede da rede virtual para o serviço de mensagens.

O resultado é uma relação isolada e privada entre as cargas de trabalho ligada à sub-rede e o namespace respectivo do Hubs de eventos, apesar do endereço de rede observable do sistema de mensagens serviço ponto final que está a ser num intervalo IP público.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Cenários de segurança avançada ativados pela integração de VNet 

Soluções que requerem uma segurança forte e compartimentalizada e, em que sub-redes da rede virtual fornecem a segmentação de fornecida entre os serviços compartmentalized, geralmente ainda precisam de caminhos de comunicação entre serviços que residem em compartimentos desses.

Qualquer rotas IP imediata entre compartimentos, incluindo aquelas com HTTPS por TCP/IP, carrega o risco de exploração de vulnerabilidades da camada de rede em segurança. Serviços de mensagens fornecem caminhos de comunicação completamente isolado, onde as mensagens até mesmo são escritas no disco à medida que eles fazem a transição entre partes. Cargas de trabalho em duas redes virtuais diferentes que estão ambos vinculadas para a mesma instância de Hubs de eventos podem comunicar com eficiência e fiável através de mensagens, enquanto a integridade de limite de isolamento de rede correspondentes é preservada.
 
Isso significa que a segurança confidencial soluções na cloud não ganhará acesso apenas às capacidades do Azure líder da indústria fiáveis e escaláveis assíncronas mensagens, mas agora podem utilizar mensagens para criar caminhos de comunicação entre a solução segura compartments que são inerentemente mais seguro do que o que é conseguido com qualquer modo de comunicação de ponto-a-ponto, incluindo o HTTPS e outros protocolos de socket protegida por TLS.

## <a name="bind-event-hubs-to-virtual-networks"></a>Ligar Hubs de eventos para redes virtuais

*Regras de rede virtual* são o recurso de segurança de firewall que controla se o seu espaço de nomes de Hubs de eventos do Azure aceita ligações a partir de uma sub-rede de rede virtual específico.

Um espaço de nomes de Hubs de eventos de enlace a uma rede virtual é um processo de dois passos. Tem primeiro de criar uma **ponto final de serviço de rede Virtual** numa sub-rede de rede Virtual e enable-lo "Eventhub", como explicado no [descrição geral do ponto final de serviço] [ vnet-sep]. Depois de adicionar o ponto final de serviço, vincular o espaço de nomes de Hubs de eventos ao mesmo com um *regra de rede virtual*.

A regra de rede virtual é uma associação com o nome do espaço de nomes dos Hubs de eventos com uma sub-rede de rede virtual. Embora exista a regra, todas as cargas de trabalho ligadas à sub-rede são concedidas acesso ao espaço de nomes dos Hubs de eventos. Os Hubs de eventos em si nunca estabelece as ligações de saída, não precisa de obter acesso e é, portanto, nunca concedida acesso à sua sub-rede, permitindo que esta regra.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Criar uma regra de rede virtual com modelos Azure Resource Manager

O modelo do Resource Manager seguinte permite adicionar uma regra de rede virtual para um espaço de nomes de Hubs de eventos existente.

Parâmetros do modelo:

* **namespaceName**: espaço de nomes de Hubs de eventos.
* **vnetRuleName**: nome para a regra de rede Virtual a ser criada.
* **virtualNetworkingSubnetId**: caminho totalmente qualificado do Resource Manager para a sub-rede de rede virtual; por exemplo, `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` para a sub-rede de predefinição de uma rede virtual.

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
          "vnetRuleName":{  
             "type":"string",
             "metadata":{  
                "description":"Name of the Authorization rule"
             }
          },
          "virtualNetworkSubnetId":{  
             "type":"string",
             "metadata":{  
                "description":"subnet Azure Resource Manager ID"
             }
          }
      },
    "resources": [
        {
            "apiVersion": "2018-01-01-preview",
            "name": "[concat(parameters('namespaceName'), '/', parameters('vnetRuleName'))]",
            "type":"Microsoft.EventHub/namespaces/VirtualNetworkRules",         
            "properties": {             
                "virtualNetworkSubnetId": "[parameters('virtualNetworkSubnetId')]"  
            }
        } 
    ]
}
```

Para implementar o modelo, siga as instruções para [do Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as redes virtuais, consulte as seguintes ligações:

- [Pontos finais de serviço de rede virtual do Azure][vnet-sep]
- [Filtragem de IPs de Hubs de eventos do Azure][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md