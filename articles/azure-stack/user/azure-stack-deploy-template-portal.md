---
title: Implementar modelos com o portal no Azure Stack | Documentos da Microsoft
description: Saiba como utilizar o portal do Azure Stack para implementar modelos.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 4013b7136fd365b1d37348cfc1f0b46cce787ce2
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363128"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Implementar modelos com o portal do Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode utilizar o portal para implementar os modelos Azure Resource Manager para o Azure Stack.

## <a name="to-deploy-a-template"></a>Para implementar um modelo

1. Inicie sessão no portal, selecione **+ criar um recurso**e, em seguida, selecione **personalizado**.
2. Selecione **implementação do modelo**.
3. Selecione **Editar modelo**e, em seguida, cole o código de modelo JSON para a janela de código. Selecione **Guardar**.
4. Selecione **Editar parâmetros**, forneça valores para os parâmetros que são apresentados e, em seguida, selecionam **OK**.
5. Selecione **subscrição**. Escolha a subscrição que pretende utilizar e, em seguida, selecione **OK**.
6. Selecione **grupo de recursos**. Escolha um grupo de recursos existente ou crie um novo e, em seguida, selecione **OK**.
7. Selecione **Criar**. Um novo mosaico no dashboard controla o progresso da implementação do modelo.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a implementação de modelos, veja:

[Implementar modelos com o PowerShell](azure-stack-deploy-template-powershell.md)
