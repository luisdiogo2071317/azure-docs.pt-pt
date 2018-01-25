---
title: Iniciar e parar a pilha do Azure | Microsoft Docs
description: Saiba como iniciar e encerrar a pilha do Azure.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: mabrigg
ms.openlocfilehash: 98bf75f5883b734c785ed1a3ed924afca1737c56
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="start-and-stop-azure-stack"></a>Iniciar e parar a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas (versão 1712 e posterior)*

## <a name="stop-azure-stack"></a>Parar a pilha do Azure 

Encerre a pilha do Azure com os seguintes passos:

1. Abra uma sessão de ponto final com privilégios (PEP) a partir de uma máquina com acesso à rede para as VMs de ERCS de pilha do Azure. Para obter instruções, consulte [utilizando o ponto final com privilégios na pilha de Azure](azure-stack-privileged-endpoint.md).

2. A partir de PEP, execute:

    ```powershell
      Stop-AzureStack
    ```

3. Aguarde que todos os nós físicos de pilha do Azure a potência desativado.

> [!Note]  
> Pode verificar o estado de energia de um nó físico ao seguir as instruções do fabricante de equipamento Original (OEM) que forneceu o hardware de pilha do Azure. 

## <a name="start-azure-stack"></a>Iniciar a pilha do Azure 

Começar a pilha do Azure com os seguintes passos. Siga estes passos, independentemente de como parou a pilha do Azure.

1. Ligar a todos os nós físicos no seu ambiente de pilha do Azure. Certifique-se as instruções para os nós físicos de ativação ao seguir as instruções do fabricante de equipamento Original (OEM) que é fornecido o hardware para a pilha do Azure.

2. Aguarde até que os serviços de infraestrutura de pilha do Azure é iniciado. Serviços de infraestrutura de pilha do Azure podem exigir a duas horas para concluir o processo de início. Pode verificar o estado de início da pilha do Azure com o [ **Get-ActionStatus** cmdlet](#get-the-startup-status-for-azure-stack).


## <a name="get-the-startup-status-for-azure-stack"></a>Obter o estado de arranque para a pilha do Azure

Obter o arranque para a rotina de arranque de pilha do Azure com os seguintes passos:

1. Abra uma sessão de ponto final com privilégios de uma máquina com acesso à rede para as VMs de ERCS de pilha do Azure.

2. A partir de PEP, execute:

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Resolver problemas de arranque e encerramento da pilha do Azure

Se os serviços de infraestrutura e de inquilino não iniciar duas horas após a energia no seu ambiente de pilha do Azure, execute os seguintes passos. 

1. Abra uma sessão de ponto final com privilégios de uma máquina com acesso à rede para as VMs de ERCS de pilha do Azure.

2. Execute: 

    ```powershell
      Test-AzureStack
      ```

3. Reveja o resultado e resolva quaisquer erros de estado de funcionamento. Para obter mais informações, consulte [execute um teste de validação da pilha de Azure](azure-stack-diagnostic-test.md).

4. Execute:

    ```powershell
      Start-AzureStack
    ```

5. Se executar **início AzureStack** resultados de uma falha, contacte o suporte ao cliente de serviços da Microsoft. 

## <a name="next-steps"></a>Passos Seguintes 

Saiba mais sobre a ferramenta de diagnóstico de pilha do Azure e emitir o registo, consulte [ferramentas de diagnóstico de pilha do Azure. Ferramentas de diagnóstico de pilha do Azure. Ferramentas de diagnóstico de pilha do Azure. Ferramentas de diagnóstico de pilha do Azure.
