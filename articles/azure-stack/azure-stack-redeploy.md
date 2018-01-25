---
title: Reimplementar a pilha do Azure | Microsoft Docs
description: Reimplemente a pilha do Azure.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 795af5ea-892d-40b1-a080-42e4472e4bba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeffgilb
ms.openlocfilehash: 0dec5ea70376ff1c8cf488689f1a66190256f6ff
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="redeploy-azure-stack"></a>Reimplementar a pilha do Azure
Se receber um erro durante a implementação de pilha do Azure, pode executar novamente a configuração utilizando o seguinte comando do PowerShell: `.\InstallAzureStackpoc.ps1 -rerun`. Este comando irá reinicie a configuração de pilha do Azure no ponto de falhado anteriormente sem ser necessário recomeçar do início. Se receber o mesmo erro de configuração novamente, poderá ser necessário efetuar uma reimplementação completa para o problema de endereços. 

Para voltar a pilha do Azure, tem de iniciar através do zero conforme descrito abaixo.

## <a name="steps-to-redeploy-azure-stack"></a>Passos para voltar a pilha do Azure
1. No anfitrião do kit de desenvolvimento, abra uma consola elevada do PowerShell > navegue para o script de asdk installer.ps1 > executá-la > clique **reiniciar**.
2. Selecione o sistema operativo base (não **Azure pilha**) e clique em **seguinte**.
3. Depois de reiniciado o anfitrião do kit de desenvolvimento, elimine o ficheiro de CloudBuilder.vhdx que foi utilizado como parte da implementação anterior.
4. [Implementar o kit de desenvolvimento](azure-stack-run-powershell-script.md).

## <a name="next-steps"></a>Passos Seguintes
[Ligar ao Azure Stack](azure-stack-connect-azure-stack.md)

