---
title: Pontos finais de serviço de rede virtuais e as regras para Event Hubs do Azure | Microsoft Docs
description: Adicione um ponto final de serviço de Microsoft.ServiceBus a uma rede virtual.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: clemensv
ms.openlocfilehash: a23e5414cd3c60192badfee65b14c49cd5e96f4e
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036370"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Utilizar pontos finais do serviço de rede Virtual com o Event Hubs do Azure

A integração dos Event Hubs com [pontos finais do serviço de rede Virtual (VNet)] [ vnet-sep] permite um acesso seguro às capacidades de mensagens de cargas de trabalho como máquinas virtuais que estão vinculadas a virtual redes, com o caminho de tráfego de rede que está a ser protegidos em ambas as extremidades. 

Depois de ter configurado para ser vinculado a pelo menos um ponto final do serviço de sub-rede virtual de rede, o respetivo Event Hubs espaço de nomes já não aceita tráfego a partir de qualquer local, mas autorizado a rede virtual. Da perspetiva de rede virtual, um espaço de nomes de Event Hubs do enlace para um ponto final de serviço configura um túnel de rede isolado de sub-rede da rede virtual para o serviço de mensagens.

O resultado é uma relação isolada e privada entre as cargas de trabalho vinculado à sub-rede e respetivo Event Hubs espaço de nomes, mantenha apesar do endereço de rede observable do serviço de mensagens serviço ponto final que está a ser um intervalo de IP público.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Cenários de segurança avançada ativados através da integração de VNet 

Soluções que requerem segurança sólida e compartmentalized e onde sub-redes da rede virtual fornecem segmentação entre os serviços compartmentalized, geralmente ainda precisam de caminhos de comunicação entre os serviços que residem nesses compartimentos.

Nenhuma rota IP imediata entre os compartimentos, incluindo as devidas HTTPS através de TCP/IP, acarreta o risco de exploração de vulnerabilidades de camada de rede em segurança. Serviços de mensagens fornecem caminhos de comunicação completamente insulated, onde as mensagens mesmo são escritas no disco como estes transitar entre as partes. Cargas de trabalho em duas redes virtuais distintos ambos vinculadas a mesma instância de Event Hubs possam comunicar de forma eficiente e fiável através de mensagens, enquanto a integridade de limites de isolamento de rede correspondentes é preservada.
 
Isto significa que a segurança confidencial soluções de nuvem não só obterem acesso ao Azure líder da indústria fiáveis e escaláveis assíncronas capacidades de mensagens, mas agora podem utilizar mensagens para criar caminhos de comunicação entre a solução segura compartments que são inerentemente mais segura do que o que é alcançável com qualquer modo de comunicação de ponto-a-ponto, incluindo HTTPS e outros protocolos de socket protegida por TLS.

## <a name="bind-event-hubs-to-virtual-networks"></a>Vincular os Event Hubs para redes virtuais

*Regras de rede virtual* são a funcionalidade de segurança de firewall que controla se o servidor de Event Hubs do Azure aceita ligações a partir de uma sub-rede de rede virtual específico.

Um espaço de nomes de Event Hubs do enlace a uma rede virtual é um processo de dois passos. Terá primeiro de criar um **ponto final do serviço de rede Virtual** numa sub-rede da rede Virtual e ative-o para "Microsoft.ServiceBus" como explicado no [descrição geral do ponto final de serviço] [ vnet-sep]. Depois de adicionar o ponto final de serviço, vincular o espaço de nomes de Event Hubs para a mesma com uma *regra de rede virtual*.

A regra de rede virtual é uma associação com o nome do espaço de nomes de Event Hubs com uma sub-rede de rede virtual. Enquanto a regra existe, todas as cargas de trabalho vinculadas à sub-rede concedidas acesso ao espaço de nomes de Event Hubs. Os Event Hubs próprio nunca estabelecer ligações de saída, não precisa de obter acesso e, por conseguinte, nunca terá acesso à sua sub-rede Ativando a esta regra.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Criar uma regra de rede virtual com modelos Azure Resource Manager

O modelo do Resource Manager seguinte permite adicionar uma regra de rede virtual para um espaço de nomes de Event Hubs existente.

Parâmetros do modelo:

* **namespaceName**: espaço de nomes de Event Hubs.
* **vnetRuleName**: nome para a regra de rede Virtual a ser criado.
* **virtualNetworkingSubnetId**: caminho completamente qualificado do Gestor de recursos para a sub-rede de rede virtual; por exemplo, `subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` para a sub-rede predefinida de uma rede virtual.

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

- [Pontos finais do serviço de rede virtual do Azure][vnet-sep]
- [Filtragem de IP do Event Hubs do Azure][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md