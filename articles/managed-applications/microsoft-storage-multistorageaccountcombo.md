---
title: Elemento de IU MultiStorageAccountCombo do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Storage.MultiStorageAccountCombo para o portal do Azure.
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
ms.openlocfilehash: f5fa81d53e1728e8f566a2a39aed8311828b20c7
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37108710"
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Elemento de IU Microsoft.Storage.MultiStorageAccountCombo
Um grupo de controlos para criar várias contas de armazenamento com nomes que começam com um prefixo comuns.

## <a name="ui-sample"></a>Exemplo de IU
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Observações
- O valor para `defaultValue.prefix` é concatenado com números inteiros de um ou mais para gerar a sequência de nomes de conta de armazenamento. Por exemplo, se `defaultValue.prefix` é **sa** e `count` é **2**, em seguida, os nomes da conta de armazenamento **sa1** e **sa2** são gerados. Os nomes das contas de armazenamento gerado são validadas exclusividade automaticamente.
- Os nomes de conta de armazenamento são gerados com base lexicographically `count`. Por exemplo, se `count` é 10, em seguida, os nomes de conta de armazenamento de terminar com dois dígitos números inteiros (01, 02, 03).
- O valor predefinido para `defaultValue.prefix` é **nulo**e para `defaultValue.type` é **Premium_LRS**.
- Qualquer tipo que não foram especificado no `constraints.allowedTypes` está oculto e qualquer tipo que não foram especificadas no `constraints.excludedTypes` é apresentado. `constraints.allowedTypes` e `constraints.excludedTypes` são opcionais, mas não podem ser utilizados em simultâneo.
- Além de gerar nomes de conta de armazenamento, `count` é utilizado para definir o multiplicador adequado para o elemento. Suporta um valor estático, tal como **2**, ou como um valor dinâmico de outro elemento, `[steps('step1').storageAccountCount]`. O valor predefinido é **1**.

## <a name="sample-output"></a>Resultado da amostra

```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
