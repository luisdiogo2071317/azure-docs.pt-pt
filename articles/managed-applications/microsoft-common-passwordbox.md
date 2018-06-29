---
title: Elemento de IU PasswordBox do Azure | Microsoft Docs
description: Descreve o elemento de IU Microsoft.Common.PasswordBox para o portal do Azure.
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
ms.openlocfilehash: adaece4a450f758c22ffecb356826962806d9d9b
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060108"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Elemento de IU Microsoft.Common.PasswordBox
Um controlo que pode ser utilizado para fornecer e confirmar uma palavra-passe.

## <a name="ui-sample"></a>Exemplo de IU
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "",
    "validationMessage": ""
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Observações
- Este elemento não suporta o `defaultValue` propriedade.
- Para detalhes de implementação de `constraints`, consulte [Microsoft.Common.TextBox](microsoft-common-textbox.md).
- Se `options.hideConfirmation` está definido como **verdadeiro**, a segunda caixa de texto para confirmar a palavra-passe do utilizador está oculto. O valor predefinido é **falso**.

## <a name="sample-output"></a>Resultado da amostra
```json
"p4ssw0rd"
```

## <a name="next-steps"></a>Passos Seguintes
* Para uma introdução ao criar as definições de IU, consulte [introdução CreateUiDefinition](create-uidefinition-overview.md).
* Para obter uma descrição de propriedades comuns de elementos de IU, consulte [CreateUiDefinition elementos](create-uidefinition-elements.md).
