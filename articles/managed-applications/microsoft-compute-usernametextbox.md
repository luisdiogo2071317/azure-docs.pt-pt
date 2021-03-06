---
title: Elemento de IU UserNameTextBox do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Compute.UserNameTextBox para o portal do Azure.
services: managed-applications
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 9f07c5bf9ba1f1880fa142beb52455522425e68d
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063306"
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Elemento de IU Microsoft.Compute.UserNameTextBox
Um controlo de caixa de texto com validação incorporadas para nomes de utilizador do Windows e Linux.

## <a name="ui-sample"></a>Exemplo de IU
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a>Observações
- Se `constraints.required` está definido como **verdadeiro**, em seguida, a caixa de texto tem de ter um valor para validar com êxito. O valor predefinido é **verdadeiro**.
- `osPlatform` tem de ser especificado e pode ser **Windows** ou **Linux**.
- `constraints.regex` é um padrão de expressão regular de JavaScript. Se for especificado, em seguida, valor da caixa de texto tem de corresponder ao padrão para validar com êxito. O valor predefinido é **nulo**.
- `constraints.validationMessage` é uma cadeia a apresentar quando o valor da caixa de texto falha de validação especificada pelo `constraints.regex`. Se não for especificado, em seguida, são utilizadas as mensagens de validação incorporadas a caixa de texto. O valor predefinido é **nulo**.
- Este elemento tem de validação incorporada com base no valor especificado para `osPlatform`. A validação incorporada pode ser utilizada juntamente com uma expressão regular personalizada. Se um valor para `constraints.regex` for especificado, em seguida, são acionadas validações incorporadas e personalizadas.

## <a name="sample-output"></a>Resultado da amostra
```json
"Example name"
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
