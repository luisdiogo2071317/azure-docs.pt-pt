---
title: Implementar modelos com o Visual Studio no Azure Stack | Documentos da Microsoft
description: Saiba como implementar modelos com o Visual Studio no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 9cbe39c018d5ba91599d8ec429430fceaaa567c5
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064632"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Implementar modelos no Azure Stack com o Visual Studio

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode utilizar o Visual Studio para implementar os modelos Azure Resource Manager para o Azure Stack.

## <a name="to-deploy-a-template"></a>Para implementar um modelo

1. [Instalar e ligar](azure-stack-install-visual-studio.md) ao Azure Stack com o Visual Studio.
2. Abra o Visual Studio.
3. Selecione **arquivo**e, em seguida, selecione **New**. Na **novo projeto**, selecione **grupo de recursos do Azure**.
4. Introduza um **Name** para o novo projeto e, em seguida, selecione **OK**.
5. Na **selecionar modelo do Azure**, escolha **guia de introdução do Azure Stack** na lista pendente.
6. Selecione **101-criar-conta de armazenamento**e, em seguida, selecione **OK**.
7. No seu novo projeto, expanda o **modelos** nó **Explorador de soluções** para ver os modelos disponíveis.
8. Na **Explorador de soluções**, escolha o nome do seu projeto e, em seguida, selecione **implementar**. Selecione **nova implementação**.
9. Na **implementar no grupo de recursos**, utilize o **subscrição** na lista pendente para selecionar a sua subscrição do Microsoft Azure Stack.
10. Partir do **grupo de recursos** lista, selecione um grupo de recursos existente ou crie um novo.
11. Partir do **localização do grupo de recursos** lista, escolha uma localização e, em seguida, selecione **implementar**.
12. Na **Editar parâmetros**, forneça valores para os parâmetros (que variam consoante o modelo) e, em seguida, selecione **guardar**.

## <a name="next-steps"></a>Passos Seguintes

* [Implementar modelos com a linha de comandos](azure-stack-deploy-template-command-line.md)
* [Desenvolver modelos para o Azure Stack](azure-stack-develop-templates.md)
