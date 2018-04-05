---
title: Elemento de IU de caixa de texto do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Common.TextBox para o portal do Azure.
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
ms.openlocfilehash: daee95c177ecddfea1cf7d6d162906fcc86938b4
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
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
  "label": "Some text box",
  "defaultValue": "my value",
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
- Se `constraints.required` está definido como **verdadeiro**, em seguida, a caixa de texto tem de conter um valor para validar com êxito. O valor predefinido é **falso**.
- `constraints.regex` é um padrão de expressão regular de JavaScript. Se for especificado, em seguida, valor da caixa de texto tem de corresponder ao padrão para validar com êxito. O valor predefinido é **nulo**.
- `constraints.validationMessage` é uma cadeia a apresentar quando o valor da caixa de texto falhar na validação. Se não for especificado, em seguida, são utilizadas as mensagens de validação incorporadas a caixa de texto. O valor predefinido é **nulo**.
- É possível especificar um valor para `constraints.regex` quando `constraints.required` está definido como **falso**. Neste cenário, um valor não é necessário para a caixa de texto validar com êxito. Se for especificado, tem de corresponder o padrão de expressão regular.

## <a name="sample-output"></a>Resultado da amostra

```json
"foobar"
```

## <a name="next-steps"></a>Passos seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
