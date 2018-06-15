---
title: Elemento de IU StorageAccountSelector do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Storage.StorageAccountSelector para o portal do Azure.
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
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: 4a18d629b959e4f1abf4ec2df28a31180efec89f
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34261042"
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Elemento de IU Microsoft.Storage.StorageAccountSelector
Um controlo para selecionar uma conta de armazenamento novo ou existente.

## <a name="ui-sample"></a>Exemplo de IU
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Observações
- Se for especificado, `defaultValue.name` está a ser validado automaticamente exclusividade. Se o nome da conta de armazenamento não for exclusivo, o utilizador tem de especificar um nome diferente ou escolher uma conta de armazenamento existente.
- O valor predefinido para `defaultValue.type` é **Premium_LRS**.
- Qualquer tipo que não foram especificado no `constraints.allowedTypes` está oculto e qualquer tipo que não foram especificadas no `constraints.excludedTypes` é apresentado.
`constraints.allowedTypes` e `constraints.excludedTypes` são opcionais, mas não podem ser utilizados em simultâneo.
- Se `options.hideExisting` é **verdadeiro**, o utilizador não é possível escolher uma conta de armazenamento existente. O valor predefinido é **falso**.


## <a name="sample-output"></a>Resultado da amostra
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
