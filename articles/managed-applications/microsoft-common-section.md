---
title: Elemento de IU secção do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Common.Section para o portal do Azure.
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
ms.openlocfilehash: 46ea2e3d404ac3ec9b7f909257451991dbb55f53
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="microsoftcommonsection-ui-element"></a>Elemento de IU Microsoft.Common.Section
Um controlo que agrupa um ou mais elementos sob um cabeçalho.

## <a name="ui-sample"></a>Exemplo de IU
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Observações
- `elements` tem de conter pelo menos um elemento e pode conter todos os tipos de elemento, exceto `Microsoft.Common.Section`.
- Este elemento não suporta o `toolTip` propriedade.

## <a name="sample-output"></a>Resultado da amostra
Para aceder os valores de saída de elementos na `elements`, utilize o [basics()](create-uidefinition-functions.md#basics) ou [steps()](create-uidefinition-functions.md#steps) funções e a notação de pontos:

```json
basics('section1').element1
```

Elementos do tipo `Microsoft.Common.Section` ter não existem valores de saída próprios.

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
