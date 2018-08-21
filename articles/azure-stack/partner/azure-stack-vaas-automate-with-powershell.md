---
title: Automatizar a validação do Azure Stack com o PowerShell | Documentos da Microsoft
description: Pode automatizar a validação do Azure Stack com o PowerShell.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 1cb4b1a7cfd72ea302676244a53af58e77215aa9
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235455"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Automatizar a validação do Azure Stack com o PowerShell 

Validação como um serviço (VaaS) fornece a capacidade de automatizar a inicialização do testes usando o **LaunchVaaSTests.ps1** script.

Pode utilizar o PowerShell para o fluxo de trabalho seguinte:

- Fluxo de trabalho de aprovação de teste

Este script abrange os quatro elementos de um fluxo de trabalho:

- Instala os pré-requisitos.
- Instala e iniciar o agente local.
- Inicia uma categoria de testes, como integração, funcional, confiabilidade.
- Geração de ficheiros de relatórios de cada teste passar o resultado para a monitorização e relatórios.

## <a name="launch-the-test-pass-workflow"></a>Iniciar o fluxo de trabalho de aprovação de teste

1. Abra uma linha de comandos elevada do PowerShell.

2. Execute o seguinte script para transferir o script de automação:

    ````PowerShell  
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://vaastestpacksprodeastus.blob.core.windows.net/packages/Microsoft.VaaS.Scripts.3.0.0.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ````

3. Execute o seguinte script com os seus valores:

    ````PowerShell  
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>"  -AsPlainText -Force)
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<ServiceAdminUser>", (ConvertTo-SecureString "<ServiceAdminPassword>" -AsPlainText -Force)
    $TenantAdminCreds = New-Object System.Management.Automation.PSCredential "<TenantAdminUser>", (ConvertTo-SecureString "<TenantAdminPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
        -VaaSAccountTenantId <VaaSAccountTenantId> `
        -VaaSSolutionName <VaaSSolutionName> `
        -VaaSTestPassName <VaaSTestPassName> `
        -VaaSTestCategories Integration,Functional `
        -MaxScriptWaitTimeInHours 12 `
        -ServiceAdminCreds $ServiceAdminCreds `
    ````

    **Parâmetros**

    | Parâmetro | Descrição |
    | --- | --- |
    | VaaSUserld | O ID de utilizador VaaS. | 
    | VaaSUserPassword | A palavra-passe VaaS. |
    | ServiceAdminUser | Sua conta de administrador de serviço do Azure Stack.  |
    | ServiceAdminPassword | A palavra-passe de serviço de Azure Stack.  |
    | TenantAdminUser | O administrador para o inquilino principal.  |
    | TenantAdminPassword | A palavra-passe para o inquilino principal.  |
    | FunctionalCategory| Integração, funcional, ou. Fiabilidade. Se utilizar vários valores, separe cada valor por uma vírgula.  |

4. Reveja os resultados de seu teste. Para obter informações sobre como ler os resultados do teste, consulte [Monitorizar testes](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).
 - Para saber mais sobre o PowerShell no Azure Stack, veja a [módulo do Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) referência.