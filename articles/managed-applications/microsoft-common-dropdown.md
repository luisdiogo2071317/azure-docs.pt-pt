---
title: Elemento de IU de lista pendente do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Common.DropDown para o portal do Azure.
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
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: f953e1dc15e12c37c30a86ebd7536b1126bf18f7
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062005"
---
# <a name="microsoftcommondropdown-ui-element"></a>Elemento de IU Microsoft.Common.DropDown
Um controlo de seleção com uma lista pendente.

## <a name="ui-sample"></a>Exemplo de IU
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Example drop down",
  "defaultValue": "Value two",
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
    ],
    "required": true
  },
  "visible": true
}
```

## <a name="remarks"></a>Observações

- A etiqueta de `constraints.allowedValues` é o texto apresentado para um item e respetivo valor é o valor de saída do elemento quando selecionado.
- Se for especificado, o valor predefinido tem de ser uma etiqueta presente no `constraints.allowedValues`. Se não for especificado, o primeiro item `constraints.allowedValues` está selecionada. O valor predefinido é **nulo**.
- `constraints.allowedValues` tem de ter pelo menos um item.
- Para emular um valor não necessário, adicione um item com uma etiqueta e o valor de `""` (uma cadeia vazia) para `constraints.allowedValues`.

## <a name="sample-output"></a>Resultado da amostra
```json
"two"
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
