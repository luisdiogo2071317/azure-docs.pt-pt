---
title: Implementar modelos com o Visual Studio na pilha do Azure | Microsoft Docs
description: Saiba como implementar modelos com o Visual Studio na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 6cd722fedc0483e37ce6ee491d74a7c985111353
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605128"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Implementar modelos na pilha do Azure com o Visual Studio

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode utilizar o Visual Studio para implementar modelos Azure Resource Manager para a pilha do Azure.

## <a name="to-deploy-a-template"></a>Para implementar um modelo

1. [Instalar e ligar](azure-stack-install-visual-studio.md) à pilha do Azure com o Visual Studio.
2. Abra o Visual Studio.
3. Selecione **ficheiro**e, em seguida, selecione **novo**. No **novo projeto**, selecione **grupo de recursos do Azure**.
4. Introduza um **nome** para o novo projeto e, em seguida, selecione **OK**.
5. No **selecionar modelo do Azure**, escolha **início rápido do Azure pilha** na lista pendente.
6. Selecione **101-criar--conta de armazenamento**e, em seguida, selecione **OK**.
7. No seu novo projeto, expanda o **modelos** no nó **Explorador de soluções** para ver os modelos disponíveis.
8. No **Explorador de soluções**, escolha o nome do seu projeto e, em seguida, selecione **implementar**. Selecione **nova implementação**.
9. No **implementar no grupo de recursos**, utilize o **subscrição** na lista pendente para selecionar a sua subscrição do Microsoft Azure pilha.
10. Do **grupo de recursos** lista, escolha um grupo de recursos existente ou crie um novo.
11. Do **localização do grupo de recursos** lista, escolha uma localização e, em seguida, selecione **implementar**.
12. No **Editar parâmetros**, forneça valores para os parâmetros (que variam consoante o modelo) e, em seguida, selecione **guardar**.

## <a name="next-steps"></a>Passos Seguintes

* [Implementar modelos com a linha de comandos](azure-stack-deploy-template-command-line.md)
* [Desenvolver modelos para a pilha do Azure](azure-stack-develop-templates.md)
