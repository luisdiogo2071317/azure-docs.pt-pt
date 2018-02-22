---
title: Implementar modelos com o Visual Studio na pilha do Azure | Microsoft Docs
description: Saiba como implementar modelos com o Visual Studio na pilha do Azure.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: ecefa8c3695aad0593516cb9ad8812216e163192
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Implementar modelos na pilha do Azure com o Visual Studio

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Utilize o Visual Studio para implementar modelos Azure Resource Manager para o kit de desenvolvimento de pilha do Azure.

1. [Instalar e ligar](azure-stack-install-visual-studio.md) à pilha do Azure com o Visual Studio.
2. Abra o Visual Studio.
3. Clique em **ficheiro**, clique em **novo**e, no **novo projeto** clique da caixa de diálogo **grupo de recursos do Azure**.
4. Introduza um **nome** para o novo projeto e, em seguida, clique em **OK**.
5. No **selecionar modelo do Azure** caixa de diálogo, altere o *Mostrar modelos a partir desta localização* pendente para **início rápido de pilha do Azure**
6. Clique em **101-criar--conta de armazenamento**e, em seguida, clique em **OK**.  
7. No seu novo projeto, pode ver uma lista dos modelos disponíveis, expandindo o **modelos** no nó de **Explorador de soluções** painel.
8. No **Explorador de soluções** painel, faça duplo clique no nome do seu projeto, clique em **implementar**, em seguida, clique em **nova implementação**.
9. No **implementar no grupo de recursos** caixa de diálogo a **subscrição** pendente, selecione a sua subscrição do Microsoft Azure pilha.
10. No **grupo de recursos** lista, escolha um grupo de recursos existente ou crie um novo.
11. No **localização do grupo de recursos** lista, escolha uma localização e, em seguida, clique em **implementar**.
12. No **Editar parâmetros** caixa de diálogo, introduza valores para os parâmetros (que variam consoante o modelo) e, em seguida, clique em **guardar**.

## <a name="next-steps"></a>Passos Seguintes
[Implementar modelos com a linha de comandos](azure-stack-deploy-template-command-line.md)

[Desenvolver modelos para a pilha do Azure](azure-stack-develop-templates.md)

