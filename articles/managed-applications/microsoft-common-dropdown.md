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
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: c8966c70fba1cbb17d377223ed5348075ebb7adc
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34266902"
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
  "label": "Some drop down",
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
- Se for especificado, o valor predefinido tem de ser uma etiqueta presente no `constraints.allowedValues`. Se não for especificado, o primeiro item `constraints.allowedValues` está selecionada. O valor predefinido é **nulo**.
- `constraints.allowedValues` tem de conter pelo menos um item.
- Este elemento não suporta o `constraints.required` propriedade. Para emular este comportamento, adicionar um item com uma etiqueta e o valor de `""` (uma cadeia vazia) para `constraints.allowedValues`.

## <a name="sample-output"></a>Resultado da amostra
```json
"Bar"
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
