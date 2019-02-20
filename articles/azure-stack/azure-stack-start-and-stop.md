---
title: Iniciar e parar o Azure Stack | Documentos da Microsoft
description: Saiba como iniciar e desligar o Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/19/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 6b14f5e8967567030ce854d05c53a8d26027e9a6
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428844"
---
# <a name="start-and-stop-azure-stack"></a>Iniciar e parar o Azure Stack
Deve seguir os procedimentos neste artigo corretamente encerrar e reiniciar serviços do Azure Stack. Encerramento fisicamente irá desligar todo o ambiente do Azure Stack. Iniciar ativa a todas as funções de infraestrutura e devolve recursos para o estado de energia que eles estavam antes do Desligamento de inquilino.

## <a name="stop-azure-stack"></a>Parar o Azure Stack 

Encerre o Azure Stack com os seguintes passos:

1. Prepare-se todas as cargas de trabalho em execução nos recursos de inquilino do seu ambiente do Azure Stack para o encerramento futuro. 

2. Abra uma sessão de ponto final com privilégios (PEP) a partir de uma máquina com acesso à rede para as VMs do Azure Stack ERCS. Para obter instruções, consulte [utilizando o ponto final com privilégios no Azure Stack](azure-stack-privileged-endpoint.md).

3. A partir de PEP, execute:

    ```powershell
      Stop-AzureStack
    ```

4. Aguarde que todos os nós físicos de Azure Stack para poder desativar.

> [!Note]  
> Pode verificar o estado de energia de um nó físico ao seguir as instruções do fabricante de equipamento Original (OEM) que forneceu o hardware do Azure Stack. 

## <a name="start-azure-stack"></a>Iniciar o Azure Stack 

Inicie o Azure Stack com os seguintes passos. Siga estes passos, independentemente de como o Azure Stack parado.

1. Energia em cada um de nós físicos em seu ambiente do Azure Stack. Verifique se as instruções para os nós físicos de ativação ao seguir as instruções do fabricante de equipamento Original (OEM) que é fornecido o hardware para o Azure Stack.

2. Aguarde até que os serviços de infraestrutura do Azure Stack é iniciado. Serviços de infraestrutura do Azure Stack podem exigir duas horas a concluir o processo de início. Pode verificar o estado de início do Azure Stack com o [ **Get-ActionStatus** cmdlet](#get-the-startup-status-for-azure-stack).

3. Certifique-se de que todos os seus recursos de inquilino tem devolvido para o estado que eles estavam antes do desligamento. Cargas de trabalho em execução nos recursos de inquilino poderão ter de ser reconfigurado após a inicialização pelo Gestor de carga de trabalho.

## <a name="get-the-startup-status-for-azure-stack"></a>Obter o estado de inicialização para o Azure Stack

Obter o arranque para a rotina de inicialização do Azure Stack com os seguintes passos:

1. Abra uma sessão de ponto final com privilégios de uma máquina com acesso à rede para as VMs do Azure Stack ERCS.

2. A partir de PEP, execute:

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Resolver problemas de inicialização e desligamento do Azure Stack

Execute os seguintes passos, se os serviços de infraestrutura e de inquilino não for iniciado com êxito 2 horas depois de poder no seu ambiente do Azure Stack. 

1. Abra uma sessão de ponto final com privilégios de uma máquina com acesso à rede para as VMs do Azure Stack ERCS.

2. Execute: 

    ```powershell
      Test-AzureStack
      ```

3. Reveja o resultado e resolva quaisquer erros de estado de funcionamento. Para obter mais informações, consulte [executar um teste de validação do Azure Stack](azure-stack-diagnostic-test.md).

4. Execute:

    ```powershell
      Start-AzureStack
    ```

5. Se executar **AzureStack início** resultados numa falha, contacte o suporte ao cliente de serviços da Microsoft. 

## <a name="next-steps"></a>Passos Seguintes 

Saiba mais sobre [ferramentas de diagnóstico do Azure Stack](azure-stack-diagnostics.md)
