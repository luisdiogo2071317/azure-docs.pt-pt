---
title: Notas de versão do Azure de pilha de validação, como um serviço | Documentos da Microsoft
description: Notas de versão do Azure de pilha de validação, como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: a6245472f0fc3f65dcbeebdf251387efed5f02b8
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333029"
---
# <a name="release-notes-for-validation-as-a-service"></a>Notas de versão para a validação como um serviço

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Este artigo tem as notas de versão para a validação de pilha do Azure como um serviço.

## <a name="version-401"></a>Versão 4.0.1

8 de Outubro de 2018

- Pré-requisitos de VaaS

    `Install-VaaSPrerequisites` já não necessita de credenciais de administrador da cloud. Se estiver a executar a versão mais recente deste cmdlet, consulte [transfira e instale o agente](azure-stack-vaas-local-agent.md#download-and-install-the-agent) para os comandos revisados para a instalação de pré-requisitos. Seguem-se os comandos:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

## <a name="version-400"></a>Versão 4.0.0

29 de Agosto de 2018

- Pré-requisitos de VaaS e VHD de atualizações

    `Install-VaaSPrerequisites` agora requer credenciais de administrador da nuvem para resolver um problema durante a validação do pacote. A documentação em [transfira e instale o agente](azure-stack-vaas-local-agent.md#download-and-install-the-agent) foi atualizado com o seguinte:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    $CloudAdminCreds = New-Object System.Management.Automation.PSCredential "<cloudAdminDomain\username>", (ConvertTo-SecureString "<cloudAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region `
                              -CloudAdminCredentials $CloudAdminCreds
    ```
    > [!NOTE]
    > O `$CloudAdminCreds` necessários pelo script é para o Azure Stack instância a ser validada. Não são as credenciais do Azure Active Directory utilizadas pelo inquilino VaaS.

- Atualização do agente local

    A versão anterior do agente local não é compatível com o 4.0.0 atual versão do serviço. Todos os utilizadores têm de atualizar os agentes locais. Ver [transfira e instale o agente](azure-stack-vaas-local-agent.md#download-and-install-the-agent) para obter instruções sobre como instalar o agente mais recente.

- Atualização de automatização de PowerShell

    Foram efetuadas alterações a `LaunchVaaSTests` scripts do PowerShell que exigem a versão mais recente dos pacotes de criação de scripts. Ver [iniciar o fluxo de trabalho de aprovação de teste](azure-stack-vaas-automate-with-powershell.md#launch-the-test-pass-workflow) para obter instruções sobre como instalar a versão mais recente do pacote de criação de scripts.

- Validação como um Portal de serviço

  - Notificações de assinatura do pacote

    Quando um pacote de personalização OEM é enviado como parte do fluxo de trabalho de validação do pacote, o formato de pacote será validado para se certificar de que ela segue à especificação publicada. Se o pacote não estiver em conformidade, a execução falhará. Notificações por correio eletrónico serão enviadas para o endereço de e-mail do contacto do Azure Active Directory registado para o inquilino.

  - Categoria de teste interativa

    O **Interactive** categoria de teste foi adicionada. Esses testes permitem que os parceiros para exercer interativos e não automatizada cenários do Azure Stack.

  - Verificação de recurso interativo

    A capacidade de fornecer comentários focado para determinadas funcionalidades está agora disponível no fluxo de trabalho de aprovação de teste. O `OEM Update on Azure Stack 1806 RC Validation 5.1.4.0` teste verifica para ver se as atualizações específicas foram aplicadas corretamente e, em seguida, recolhe comentários.

## <a name="next-steps"></a>Passos Seguintes

- [Resolver problemas de validação como um serviço](azure-stack-vaas-troubleshoot.md)