---
title: Iniciar e parar o Kit de desenvolvimento de pilha do Azure (ASDK) | Microsoft Docs
description: Saiba como iniciar e encerrar baixo Azure pilha Development Kit (ASDK).
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
ms.date: 04/11/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: dfb565803746ecdda9b36a4e12a3c3f2b4d9e0d0
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31427029"
---
# <a name="start-and-stop-the-azure-stack-development-kit-asdk"></a>Iniciar e parar o Kit de desenvolvimento de pilha do Azure (ASDK)
Não é recomendado reiniciar o computador de anfitrião ASDK simplesmente. Em vez disso, deve seguir os procedimentos neste artigo corretamente encerrar e reiniciar os serviços ASDK. 

## <a name="stop-azure-stack"></a>Parar a pilha do Azure 
Para corretamente encerrar os serviços de pilha do Azure e o computador de anfitrião ASDK, utilize os seguintes comandos do PowerShell:

1. Inicie sessão como AzureStack\CloudAdmin no computador anfitrião ASDK.
2. Abra o PowerShell como administrador (não ISE do PowerShell).
3. Execute os seguintes comandos para estabelecer uma sessão de ponto final com privilégios (PEP): 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Em seguida, na sessão PEP, utilize o **Stop-AzureStack** cmdlet para parar serviços do Azure pilha e encerrar o computador de anfitrião ASDK:

   ```powershell
   Stop-AzureStack
   ```
5. Reveja a saída do PowerShell para garantir que todos os serviços de pilha do Azure são encerrados com êxito antes do computador de anfitrião ASDK será encerrado. O processo de encerramento demora vários minutos.

## <a name="start-azure-stack"></a>Iniciar a pilha do Azure 
Serviços ASDK devem iniciar automaticamente quando o computador anfitrião é iniciado. No entanto, o tempo de arranque de serviços de infraestrutura ASDK varia consoante o desempenho da configuração de hardware de computador do anfitrião ASDK. Pode demorar várias horas para todos os serviços para reiniciado com êxito em alguns casos.

Independentemente da forma como o ASDK foi encerrado, deve utilizar os seguintes passos para verificar se todos os serviços de pilha do Azure são iniciadas e completamente operacional depois do computador anfitrião tem a alimentação ligado: 

1. Ligar o computador de anfitrião ASDK. 
2. Inicie sessão como AzureStack\CloudAdmin no computador anfitrião ASDK.
3. Abra o PowerShell como administrador (não ISE do PowerShell).
4. Execute os seguintes comandos para estabelecer uma sessão de ponto final com privilégios (PEP):

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. Em seguida, na sessão PEP, execute os seguintes comandos para verificar o estado de arranque dos serviços de pilha do Azure:

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. Reveja a saída para se certificar de que os serviços do Azure pilha tem reiniciado com êxito.

Para saber mais sobre os procedimentos recomendados para corretamente encerrar e reiniciar os serviços de pilha do Azure, consulte o artigo [início e paragem do Azure pilha](.\.\azure-stack-start-and-stop.md). 

## <a name="troubleshoot-startup-and-shutdown"></a>Resolver problemas de arranque e encerramento 
Se os serviços de pilha do Azure não iniciados com êxito dentro de duas horas depois de energia no computador anfitrião ASDK, execute estes passos:

1. Inicie sessão como AzureStack\CloudAdmin no computador anfitrião ASDK.
2. Abra o PowerShell como administrador (não ISE do PowerShell).
3. Execute os seguintes comandos para estabelecer uma sessão de ponto final com privilégios (PEP):

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Em seguida, na sessão PEP, execute os seguintes comandos para verificar o estado de arranque dos serviços de pilha do Azure:

   ```powershell
   Test-AzureStack
   ```
5. Reveja o resultado e resolva quaisquer erros. Para obter mais informações, consulte [execute um teste de validação da pilha de Azure](.\.\azure-stack-diagnostic-test.md).
6. Reinicie os serviços de pilha do Azure de dentro da sessão PEP executando o **início AzureStack** cmdlet:

   ```powershell
   Start-AzureStack
   ```

Se executar **início AzureStack** resultados de uma falha, visite o [fórum de suporte do Azure pilha](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack) obter ASDK suporte de resolução de problemas. 

## <a name="next-steps"></a>Passos Seguintes 
Saiba mais sobre a ferramenta de diagnóstico de pilha do Azure e emitir o registo, consulte [ferramentas de diagnóstico do Azure pilha](.\.\azure-stack-diagnostics.md).
