---
title: "Ativar a cópia de segurança para a pilha do Azure com o PowerShell | Microsoft Docs"
description: "Ative o serviço de cópia de segurança de infraestrutura com o Windows PowerShell para que a pilha do Azure pode ser restaurada se ocorrer uma falha."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 7DFEFEBE-D6B7-4BE0-ADC1-1C01FB7E81A6
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: cbec6242fb4e185c9801a93fc2c4b35721269c2f
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Ativar a cópia de segurança para a pilha do Azure com o PowerShell

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Ative o serviço de cópia de segurança de infraestrutura com o Windows PowerShell para que a pilha do Azure pode ser restaurada se ocorrer uma falha. Pode aceder os cmdlets do PowerShell para ativar a cópia de segurança, iniciar a cópia de segurança e obter informações de cópia de segurança através do ponto final de gestão de operador.

## <a name="download-azure-stack-tools"></a>Descarregar as ferramentas de pilha do Azure

Instalar e PowerShell configurado para a pilha do Azure e as ferramentas de pilha do Azure. Consulte [começar a trabalhar com o PowerShell na pilha de Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).

##  <a name="load-the-connect-and-infrastructure-modules"></a>Carregar os módulos de ligar e a infraestrutura

Abra o Windows PowerShell com uma linha de comandos elevada e execute os seguintes comandos:

   ```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1
    
    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
   ```

##  <a name="setup-rm-environment-and-log-into-the-operator-management-endpoint"></a>Configurar o ambiente Rm e de registo para o ponto final de gestão de operador

Na mesma sessão do PowerShell, edite o seguinte script do PowerShell, adicionando as variáveis para o seu ambiente. Execute o script atualizado para configurar o ambiente de RM e inicie sessão no ponto final de gestão de operador.

| Variável    | Descrição |
|---          |---          |
| $TenantName | Nome de inquilino do Azure Active Directory. |
| Nome da conta do operador        | O nome de conta de operador de pilha do Azure. |
| Ponto final do Gestor de recursos do Azure | URL para o Gestor de recursos do Azure. |

   ```powershell
   # Specify Azure Active Directory tenant name
    $TenantName = "contoso.onmicrosoft.com"
    
    # Set the module repository and the execution policy
    Set-PSRepository `
      -Name "PSGallery" `
      -InstallationPolicy Trusted
    
    Set-ExecutionPolicy RemoteSigned `
      -force
    
    # Configure the Azure Stack operator’s PowerShell environment.
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint "https://adminmanagement.seattle.contoso.com"
    
    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience "https://graph.windows.net/"
    
    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName $TenantName `
      -EnvironmentName AzureStackAdmin
    
    # Sign-in to the operator's console.
    Login-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID 
    
   ```
## <a name="generate-a-new-encryption-key"></a>Gerar uma nova chave de encriptação

Na mesma sessão do PowerShell, execute os seguintes comandos:

   ```powershell
   $encryptionkey = New-EncryptionKeyBase64
   ```

> [!Warning]  
> Tem de utilizar as ferramentas de AzureStack para gerar a chave.

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Forneça a chave de partilha, credenciais e encriptação de cópias de segurança para ativar a cópia de segurança

Na mesma sessão do PowerShell, edite o seguinte script do PowerShell, adicionando as variáveis para o seu ambiente. Execute o script atualizado para fornecer a chave de partilha, credenciais e encriptação de cópias de segurança para o serviço de cópia de segurança da infraestrutura.

| Variável        | Descrição   |
|---              |---                                        |
| $username       | Tipo de **Username** com o domínio e o nome de utilizador para a localização do disco partilhado. Por exemplo, `Contoso\administrator`. |
| $password       | Tipo de **palavra-passe** para o utilizador. |
| $sharepath      | Escreva o caminho para o **localização de armazenamento de cópia de segurança**. Tem de utilizar uma cadeia de convenção de Nomenclatura Universal (UNC) para o caminho para uma partilha de ficheiros alojado num dispositivo separado. Uma cadeia em UNC Especifica a localização dos recursos, tais como ficheiros partilhados ou dispositivos. Para garantir a disponibilidade dos dados de cópia de segurança, o dispositivo deve estar num local separado. |

   ```powershell
    $username = "domain\backupoadmin"
    $password = "password"
    $credential = New-Object System.Management.Automation.PSCredential($username, ($password| ConvertTo-SecureString -asPlainText -Force))  
    $location = Get-AzsLocation
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"
    
    Set-AzSBackupShare -Location $location.Name -Path $sharepath -UserName $credential.UserName -Password $credential.GetNetworkCredential().password -EncryptionKey $encryptionkey
   ```
   
##  <a name="confirm-backup-settings"></a>Confirme as definições de cópia de segurança

Na mesma sessão do PowerShell, execute os seguintes comandos:

   ```powershell
   Get-AzsBackupLocation | Select-Object -Property Path, UserName, Password | ConvertTo-Json 
   ```

O resultado deverá ser semelhante a saída JSON seguinte:

   ```json
      {
    "ExternalStoreDefault":  {
        "Path":  "\\\\serverIP\\AzSBackupStore\\contoso.com\\seattle",
        "UserName":  "domain\backupoadmin",
        "Password":  null
       }
   } 
   ```

## <a name="next-steps"></a>Passos Seguintes

 - Saiba como executar uma cópia de segurança, consulte [cópia de segurança do Azure pilha](azure-stack-backup-back-up-azure-stack.md ).  
- Saiba como verificar que executou a cópia de segurança, consulte [Confirmar cópia de segurança foi concluída no portal de administração](azure-stack-backup-back-up-azure-stack.md ).
