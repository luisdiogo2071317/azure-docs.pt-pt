---
title: Pontos finais de serviço de rede virtual e regras para o Azure Service Bus | Documentos da Microsoft
description: Adicione um ponto de extremidade do serviço de servicebus a uma rede virtual.
services: event-hubs
documentationcenter: ''
author: clemensv
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: clemensv
ms.openlocfilehash: f8874a1d9db754485e9624596465560981bd6425
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858050"
---
# <a name="use-virtual-network-service-endpoints-with-azure-service-bus"></a>Utilizar pontos finais de serviço de rede Virtual com o Azure Service Bus

A integração do Service Bus com [pontos finais de serviço de rede Virtual (VNet)] [ vnet-sep] permite o acesso seguro às capacidades de mensagens de cargas de trabalho como as máquinas virtuais que estão vinculadas às redes virtuais , com o caminho do tráfego de rede que estão protegido em ambas as extremidades. 

Depois de configurado para ser associada a pelo menos um ponto final do serviço de sub-rede virtual de rede, o espaço de nomes do Service Bus respectivo já não irá aceitar o tráfego de qualquer lugar, mas autorizado da rede ou redes virtuais. Da perspectiva de rede virtual, um espaço de nomes do Service Bus de ligação para um ponto de extremidade de serviço configura um túnel de rede isolado da sub-rede da rede virtual para o serviço de mensagens.

O resultado é uma relação isolada e privada entre as cargas de trabalho ligada à sub-rede e o namespace respectivo do Service Bus, apesar do endereço de rede observable do sistema de mensagens serviço ponto final que está a ser num intervalo IP público.

## <a name="enable-service-endpoints-with-service-bus"></a>Ativar pontos finais de serviço com o Service Bus

Redes virtuais são suportadas apenas num [escalão Premium](service-bus-premium-messaging.md) espaços de nomes do Service Bus. 

Uma consideração importante ao utilizar pontos finais de serviço de VNet com o Service Bus é que não devem ser habilitadas estes pontos finais em aplicações que misturam namespaces de barramento de serviço de escalão Standard e Premium. Uma vez que o escalão Standard não suporta a VNets, o ponto final está limitado a apenas o namespaces de escalão Premium. A VNet irá bloquear o tráfego para o espaço de nomes Standard. 

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Cenários de segurança avançada ativados pela integração de VNet 

Soluções que requerem uma segurança forte e compartimentalizada e, em que sub-redes da rede virtual fornecem a segmentação de fornecida entre os serviços compartmentalized, geralmente ainda precisam de caminhos de comunicação entre serviços que residem em compartimentos desses.

Qualquer rotas IP imediata entre compartimentos, incluindo aquelas com HTTPS por TCP/IP, carrega o risco de exploração de vulnerabilidades da camada de rede em segurança. Serviços de mensagens fornecem caminhos de comunicação completamente isolado, onde as mensagens até mesmo são escritas no disco à medida que eles fazem a transição entre partes. Cargas de trabalho em duas redes virtuais diferentes que estão ambos vinculadas à mesma instância do Service Bus podem comunicar com eficiência e fiável através de mensagens, enquanto a integridade de limite de isolamento de rede correspondentes é preservada.
 
Isso significa que a segurança confidencial soluções na cloud não ganhará acesso apenas às capacidades do Azure líder da indústria fiáveis e escaláveis assíncronas mensagens, mas agora podem utilizar mensagens para criar caminhos de comunicação entre a solução segura compartments que são inerentemente mais seguro do que o que é conseguido com qualquer modo de comunicação de ponto-a-ponto, incluindo o HTTPS e outros protocolos de socket protegida por TLS.

## <a name="binding-service-bus-to-virtual-networks"></a>Ligação de Service Bus para redes virtuais

*Regras de rede virtual* são o recurso de segurança de firewall que controla se o seu servidor de Azure Service Bus aceita ligações a partir de uma sub-rede de rede virtual específico.

Um espaço de nomes do Service Bus de enlace a uma rede virtual é um processo de dois passos. Tem primeiro de criar uma **ponto final de serviço de rede Virtual** numa sub-rede de rede Virtual e ative-o para "Servicebus", como explicado na [descrição geral do ponto final de serviço] [ vnet-sep]. Depois de adicionar o ponto final de serviço, vincular o espaço de nomes do Service Bus ao mesmo com um *regra de rede virtual*.

A regra de rede virtual é uma associação com o nome do espaço de nomes do Service Bus com uma sub-rede de rede virtual. Embora exista a regra, todas as cargas de trabalho ligadas à sub-rede são concedidas acesso ao espaço de nomes do Service Bus. Service Bus propriamente dito nunca estabelece as ligações de saída, não precisa de obter acesso e é, portanto, nunca concedido acesso à sua sub-rede, permitindo que esta regra.

### <a name="creating-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Criar uma regra de rede virtual com modelos Azure Resource Manager

O modelo do Resource Manager seguinte permite adicionar uma regra de rede virtual para um espaço de nomes do barramento de serviço existente.

Parâmetros do modelo:

* **namespaceName**: espaço de nomes do Service Bus.
* **vnetRuleName**: nome para a regra de rede Virtual a ser criada.
* **virtualNetworkingSubnetId**: caminho totalmente qualificado do Resource Manager para a sub-rede de rede virtual; por exemplo, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` para a sub-rede de predefinição de uma rede virtual.

Modelo:

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
            "type":"Microsoft.ServiceBus/namespaces/VirtualNetworkRules",           
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
- [Filtragem de IP do Service Bus do Azure][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: service-bus-ip-filtering.md