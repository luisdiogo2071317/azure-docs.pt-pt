---
title: Elemento de IU OptionsGroup do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Common.OptionsGroup para o portal do Azure.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: 4b2ce987e311a12e3833fcc1dbae2b48b40135ff
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Elemento de IU Microsoft.Common.OptionsGroup
Um controlo de seleção com uma linha de opções disponíveis.

## <a name="ui-sample"></a>Exemplo de IU
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
  "defaultValue": "my value",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Observações
- A etiqueta de `constraints.allowedValues` é o texto apresentado para um item e respetivo valor é o valor de saída do elemento quando selecionado.
- Se for especificado, o valor predefinido tem de ser uma etiqueta presente no `constraints.allowedValues`. Se não for especificado, o primeiro item `constraints.allowedValues` está selecionada por predefinição. O valor predefinido é **nulo**.
- `constraints.allowedValues` tem de conter pelo menos um item.
- Este elemento não suporta o `constraints.required` propriedade; tem de selecionar um item para validar com êxito.

## <a name="sample-output"></a>Resultado da amostra
```json
"Bar"
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
