---
title: Iniciar e parar o Azure Stack Development Kit (ASDK) | Documentos da Microsoft
description: Saiba como iniciar e encerrar o para baixo do Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0b1affb708a8d481a1378fa691b6547865b0b214
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52963594"
---
# <a name="start-and-stop-the-azure-stack-development-kit-asdk"></a>Iniciar e parar o Azure Stack Development Kit (ASDK)
Não é recomendado para simplesmente reiniciar o computador de anfitrião ASDK. Em vez disso, deve seguir os procedimentos neste artigo para encerrar e reiniciar serviços ASDK adequadamente. 

## <a name="stop-azure-stack"></a>Parar o Azure Stack 
Corretamente encerrar os serviços do Azure Stack e o computador do anfitrião ASDK, utilize os seguintes comandos do PowerShell:

1. Inicie sessão como AzureStack\CloudAdmin, no computador anfitrião ASDK.
2. Abra o PowerShell como administrador (não ISE do PowerShell).
3. Execute os seguintes comandos para estabelecer uma sessão de ponto final com privilégios (PEP): 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Em seguida, na sessão PEP, utilize o **Stop AzureStack** cmdlet para parar serviços do Azure Stack e encerrar o computador do anfitrião ASDK:

   ```powershell
   Stop-AzureStack
   ```
5. Reveja o resultado do PowerShell para garantir que todos os serviços do Azure Stack são encerrados com êxito antes do computador do anfitrião ASDK encerra o tempo limite. O processo de encerramento demora vários minutos.

## <a name="start-azure-stack"></a>Iniciar o Azure Stack 
Serviços ASDK devem iniciar automaticamente quando o computador anfitrião é iniciado. No entanto, o tempo de inicialização de serviços de infraestrutura ASDK varia consoante o desempenho da configuração de hardware de computador do anfitrião ASDK. Pode demorar várias horas para todos os serviços reiniciar com êxito em alguns casos.

Independentemente de como o ASDK foi encerrada, deve usar os seguintes passos para verificar se todos os serviços do Azure Stack são iniciadas e totalmente operacional após o computador anfitrião está ligado: 

1. Power no computador anfitrião ASDK. 
2. Inicie sessão como AzureStack\CloudAdmin, no computador anfitrião ASDK.
3. Abra o PowerShell como administrador (não ISE do PowerShell).
4. Execute os seguintes comandos para estabelecer uma sessão de ponto final com privilégios (PEP):

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. Em seguida, na sessão PEP, execute os seguintes comandos para verificar o estado de inicialização dos serviços do Azure Stack:

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. Reveja o resultado para garantir que os serviços do Azure Stack tem reiniciado com êxito.

Para saber mais sobre os procedimentos recomendados para corretamente encerrar e reiniciar serviços do Azure Stack, veja [início e fim do Azure Stack](../azure-stack-start-and-stop.md). 

## <a name="troubleshoot-startup-and-shutdown"></a>Resolver problemas de inicialização e desligamento 
Se a serviços do Azure Stack não iniciar com êxito nas duas horas depois de energia no seu computador de anfitrião ASDK, execute estas etapas:

1. Inicie sessão como AzureStack\CloudAdmin, no computador anfitrião ASDK.
2. Abra o PowerShell como administrador (não ISE do PowerShell).
3. Execute os seguintes comandos para estabelecer uma sessão de ponto final com privilégios (PEP):

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Em seguida, na sessão PEP, execute os seguintes comandos para verificar o estado de inicialização dos serviços do Azure Stack:

   ```powershell
   Test-AzureStack
   ```
5. Reveja o resultado e resolva quaisquer erros. Para obter mais informações, consulte [executar um teste de validação do Azure Stack](../azure-stack-diagnostic-test.md).
6. Reinicie os serviços do Azure Stack de dentro da sessão PEP ao executar o **AzureStack início** cmdlet:

   ```powershell
   Start-AzureStack
   ```

Se executar **Start-AzureStack** resultados numa falha, visite o [fórum de suporte do Azure Stack](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack) obter ASDK resolução de problemas de suporte. 

## <a name="next-steps"></a>Passos Seguintes 
Saiba mais sobre a ferramenta de diagnóstico do Azure Stack e emitir o Registro em log, consulte [ferramentas de diagnóstico do Azure Stack](../azure-stack-diagnostics.md).
