---
title: Implementar modelos através do portal na pilha do Azure | Microsoft Docs
description: Saiba como utilizar o portal de pilha do Azure para implementar modelos.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: eea4f568f21693764222c8fdbe3316bf6008cc05
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604238"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Implementar modelos utilizando o portal de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode utilizar o portal para implementar modelos Azure Resource Manager para a pilha do Azure.

## <a name="to-deploy-a-template"></a>Para implementar um modelo

1. Inicie sessão no portal, selecione **novo**e, em seguida, selecione **personalizada**.
2. Selecione **implementação do modelo**.
3. Selecione **Editar modelo**e, em seguida, cole o código de modelo JSON para a janela de código. Selecione **Guardar**.
4. Selecione **Editar parâmetros**, forneça valores para os parâmetros que são apresentados e, em seguida, selecione **OK**.
5. Selecione **subscrição**. Selecione a subscrição que pretende utilizar e, em seguida, selecione **OK**.
6. Selecione **grupo de recursos**. Escolha um grupo de recursos existente ou crie um novo e, em seguida, selecione **OK**.
7. Selecione **Criar**. Um novo mosaico no dashboard controla o progresso da implementação do modelo.

## <a name="next-steps"></a>Passos Seguintes

* [Implementar modelos com o PowerShell](azure-stack-deploy-template-powershell.md)
