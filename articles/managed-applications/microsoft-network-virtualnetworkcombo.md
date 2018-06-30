---
title: Elemento de IU VirtualNetworkCombo do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Network.VirtualNetworkCombo para o portal do Azure.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: 2c2553d9ffb1dfbe032385fb77e234a8b96cb239
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110070"
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Elemento de IU Microsoft.Network.VirtualNetworkCombo
Um grupo de controlos para selecionar uma rede virtual nova ou existente.

## <a name="ui-sample"></a>Exemplo de IU
Quando o utilizador escolhe uma nova rede virtual, o utilizador pode personalizar o nome de cada sub-rede e o prefixo de endereço. A configuração de sub-redes é opcional.

![Microsoft.Network.VirtualNetworkCombo novo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-new.png)

Quando o utilizador seleciona uma rede virtual existente, o utilizador tem de mapear cada sub-rede que requer que o modelo de implementação para uma sub-rede existente. Sub-redes neste caso, é necessário configurar.

![Microsoft.Network.VirtualNetworkCombo existente](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-existing.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a>Observações
- Se for especificado, o primeiro sem sobreposição de endereços prefixo do tamanho `defaultValue.addressPrefixSize` é determinado automaticamente com base nas redes virtuais existentes na subscrição do utilizador.
- O valor predefinido para `defaultValue.name` e `defaultValue.addressPrefixSize` é **nulo**.
- `constraints.minAddressPrefixSize` tem de ser especificado. Redes virtuais existentes com um espaço de endereços inferior ao valor especificado não se encontram disponíveis para seleção.
- `subnets` tem de ser especificado, e `constraints.minAddressPrefixSize` tem de ser especificado para cada sub-rede.
- Ao criar uma nova rede virtual, o prefixo de endereço de cada sub-rede é calculado automaticamente com base no prefixo do endereço de rede virtual e o respetivo `addressPrefixSize`.
- Quando utilizar um existente virtual de rede, quaisquer sub-redes menores do que o respetivo `constraints.minAddressPrefixSize` não estão disponíveis para seleção. Além disso, se for especificado, as sub-redes que não tenham, pelo menos, `minAddressCount` endereços disponíveis estão indisponíveis para seleção. O valor predefinido é **0**. Para se certificar de que os endereços disponíveis contíguos, especifique **verdadeiro** para `requireContiguousAddresses`. O valor predefinido é **verdadeiro**.
- Criar sub-redes na rede virtual existente não é suportada.
- Se `options.hideExisting` é **verdadeiro**, o utilizador não é possível escolher uma rede virtual existente. O valor predefinido é **falso**.

## <a name="sample-output"></a>Resultado da amostra

```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
