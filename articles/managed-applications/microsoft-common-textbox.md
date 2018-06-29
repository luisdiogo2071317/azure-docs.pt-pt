---
title: Elemento de IU de caixa de texto do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Common.TextBox para o portal do Azure.
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
ms.openlocfilehash: fa3e5fff8080acb9e824ffe27f6c149054804830
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063651"
---
# <a name="microsoftcommontextbox-ui-element"></a>Elemento de IU Microsoft.Common.TextBox
Um controlo que pode ser utilizado para editar o texto não formatado.

## <a name="ui-sample"></a>Exemplo de IU
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Example text box 1",
  "defaultValue": "my text value",
  "toolTip": "Use only allowed characters",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a>Observações
- Se `constraints.required` está definido como **verdadeiro**, em seguida, a caixa de texto tem de ter um valor para validar com êxito. O valor predefinido é **falso**.
- `constraints.regex` é um padrão de expressão regular de JavaScript. Se for especificado, em seguida, valor da caixa de texto tem de corresponder ao padrão para validar com êxito. O valor predefinido é **nulo**.
- `constraints.validationMessage` é uma cadeia a apresentar quando o valor da caixa de texto falhar na validação. Se não for especificado, em seguida, são utilizadas as mensagens de validação incorporadas a caixa de texto. O valor predefinido é **nulo**.
- É possível especificar um valor para `constraints.regex` quando `constraints.required` está definido como **falso**. Neste cenário, um valor não a é necessário para a caixa de texto validar com êxito. Se for especificado, tem de corresponder o padrão de expressão regular.

## <a name="sample-output"></a>Resultado da amostra

```json
"my text value"
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
