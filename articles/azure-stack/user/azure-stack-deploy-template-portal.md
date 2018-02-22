---
title: Implementar modelos com o portal na pilha do Azure | Microsoft Docs
description: Saiba como utilizar o portal de pilha do Azure para implementar modelos.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 30c077747685ad7617148e9130926120420b098b
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Implementar modelos utilizando o portal de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Utilize o portal para implementar modelos Azure Resource Manager para o kit de desenvolvimento de pilha do Azure.

Modelos do Resource Manager implementar e aprovisionar todos os recursos para a sua aplicação numa operação única e coordenada.

1. Inicie sessão no portal, clique em **novo**, clique em **personalizada**e, em seguida, clique em **implementação do modelo**.
2. Clique em **Editar modelo**, em seguida, cole o código de modelo JSON para o painel e, em seguida, clique em **guardar**.
3. Clique em **Editar parâmetros**, escreva os valores para os parâmetros listados e, em seguida, clique em **OK**.
4. Clique em **subscrição**, selecione a subscrição que pretende utilizar e, em seguida, clique em **OK**.
5. Clique em **grupo de recursos**, escolha um grupo de recursos existente ou crie um novo e, em seguida, clique em **OK**.
6. Clique em **Criar**. Um novo mosaico no dashboard controla o progresso da implementação do modelo.

## <a name="next-steps"></a>Passos Seguintes
[Implementar modelos com o PowerShell](azure-stack-deploy-template-powershell.md)

