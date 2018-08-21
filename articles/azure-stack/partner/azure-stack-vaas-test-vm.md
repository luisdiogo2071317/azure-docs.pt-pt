---
title: Implementar o agente local e testar máquinas de virtuais de imagem na validação do Azure Stack como serviço | Documentos da Microsoft
description: Implementar o agente local e máquinas de virtuais de imagem para a validação do Azure Stack como um serviço de teste.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 9d32c23f66563988d023df3bf6a33efa30237e57
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235495"
---
# <a name="deploy-the-local-agent-and-test-virtual-machines"></a>Implementar as máquinas de virtuais locais do agente e teste

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Saiba como utilizar a validação como um agente local do serviço (VaaS) para verificar se o seu hardware. O agente local tem de ser implementado na solução do Azure Stack que está a ser validada antes de executar testes de validação.

> [!Note]  
> Deve certificar-se de que a máquina, onde o agente local está em execução, não perca o acesso à Internet. A máquina só deve ser acessível aos utilizadores que estão autorizados a utilizar o Azure Stack VaaS.

Para testar as suas máquinas virtuais:

1. Instalar o agente local
2. Injetar falhas no seu sistema
3. Execute o agente local

## <a name="download-and-start-the-local-agent"></a>Transferir e iniciar o agente local

Transferi o agente para um computador que cumpra os pré-requisitos no Centro de dados que não faz parte do sistema do Azure Stack, mas que tem acesso a todos os pontos finais do Azure Stack.

### <a name="machine-prerequisites"></a>Pré-requisitos de máquina

Verifique se o seu computador cumpre os seguintes critérios:

- Acesso a todos os pontos de extremidade do Azure Stack
- .NET 4.6 e o PowerShell 5.0 instalado
- Pelo menos 8 GB de RAM
- Processadores de núcleo do mínimo de 8
- Espaço em disco mínimo de 200 GB
- Conectividade de rede estável à internet

O Azure Stack é o sistema em teste. A máquina não deve fazer parte do Azure Stack ou alojado na cloud do Azure Stack.

### <a name="download-and-install-the-agent"></a>Transfira e instale o agente

1. Abra o Windows PowerShell numa linha de elevada no computador que irá utilizar para executar os testes.
2. Execute o seguinte comando para transferir o agente local:

    ```PowerShell  
        Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.3.2.0.nupkg" -outfile "OnPremAgent.zip"
        Expand-Archive -Path ".\OnPremAgent.zip" -DestinationPath VaaSOnPremAgent.3.2.0 -Force
        Set-Location VaaSOnPremAgent.3.2.0\lib\net46
    ````

3. Execute o seguinte comando para instalar as dependências do agente local:

    ```PowerShell  
        $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-VaaSPrerequisites -AadTenantId <AadTenantId> `
        -ServiceAdminCreds <ServiceAdminCreds> `
        -ArmEndpoint https://adminmanagement.<ExternalFqdn> `
        -Region <Region>
    ````

    **Parâmetros**

    | Parâmetro | Descrição |
    | --- | --- |
    | aadServiceAdminUser | O utilizador de administrador global do seu inquilino do Azure AD. Por exemplo pode ser vaasadmin@contoso.onmicrosoft.com. |
    | aadServiceAdminPassword | A palavra-passe para o utilizador de administrador global. |
    | AadTenantId | ID de inquilino do Azure AD para a conta do Azure registado com a validação como um serviço. |
    | ExternalFqdn | Pode obter o nome de domínio completamente qualificado do arquivo de configuração. Para obter instruções, consulte [testar parâmetros para a validação como um serviço do Azure Stack](azure-stack-vaas-parameters-test.md). |
    | Região | A região do seu inquilino do Azure AD. |

O comando transfere uma imagem de repositório (PIR) de imagem pública (VHD do SO) e a cópia de um armazenamento de Blobs do Azure para o armazenamento do Azure Stack. 

![Transferir a pré-requisitos](media/installingprereqs.png)

> [!Note]  
> Se estiver a ter baixa velocidade de rede ao transferir estas imagens, transferi-los separadamente para uma partilha local e especificar o parâmetro **- LocalPackagePath** *FileShareOrLocalPath*. Pode encontrar mais orientações sobre seu download do PIR na secção [identificador de conectividade de rede lenta](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity) dos [resolver problemas de validação como um serviço](azure-stack-vaas-troubleshoot.md).

## <a name="fault-injection"></a>Injeção de falha

A Microsoft criou o Azure Stack para resiliência e a tolerar vários tipos de falhas de software e hardware. Injeção de falha aumenta a taxa de falhas no sistema. Este aumento ajuda-o a descobrir problemas anteriormente, para que pode reduzir o número de incidentes que inativar o sistema.

Execute os seguintes comandos para injetar falhas no seu sistema.

1. Abra o Windows PowerShell numa linha de comandos elevada.

2. Execute o seguinte comando:

    ```PowerShell  
        Import-Module .\VaaSPreReqs.psm1 -Force
        Install-ServiceFabricSDK Install-ServiceFabricSDK
    ```

## <a name="run-the-agent"></a>Execute o agente

1. Abra o Windows PowerShell numa linha de comandos elevada.

2. Execute o seguinte comando:

    ````PowerShell  
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u <VaaSUserId> -t <VaaSTenantId>
    ````

      **Parâmetros**  
    
    | Parâmetro | Descrição |
    | --- | --- |
    | VaaSUserId | ID de utilizador utilizada para iniciar sessão Portal do VaaS (por exemplo, UserName@Contoso.com) |
    | VaaSTenantId | ID de inquilino do Azure AD para a conta do Azure registado com a validação como um serviço. |

    > [!Note]  
    > Quando executa o agente, o atual diretório de trabalho tem de ser a localização do anfitrião de motor de tarefa executável, **Microsoft.VaaSOnPrem.TaskEngineHost.exe.**

Se não vir os erros relatados, em seguida, o agente local foi concluída com êxito. O texto de exemplo seguinte é apresentada na janela da consola.

`Heartbeat Callback at 11/8/2016 4:45:38 PM`

![Introdução ao agente](media/startedagent.png)

Um agente é identificado exclusivamente pelo respetivo nome. Por predefinição, utiliza o nome de nome (FQDN) de domínio completamente qualificado da máquina do onde foi iniciado. Deve minimizar a janela para evitar qualquer cliques acidentais na janela de como alterar o foco interrompe todas as outras ações.

## <a name="next-steps"></a>Passos Seguintes

- [Validar uma nova solução do Azure Stack](azure-stack-vaas-validate-solution-new.md)  
- Se tiver conectividade de Internet lenta ou intermitente, pode baixar a imagem do PIR. Para obter mais informações, consulte [identificador de conectividade de rede lenta](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity).
- Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).
