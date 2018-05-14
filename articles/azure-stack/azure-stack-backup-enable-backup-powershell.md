---
title: Ativar a cópia de segurança para a pilha do Azure com o PowerShell | Microsoft Docs
description: Ative o serviço de cópia de segurança de infraestrutura com o Windows PowerShell para que a pilha do Azure pode ser restaurada se ocorrer uma falha.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/10/2018
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 4faa6930c37f9d491a3efa4b34519dbb13761a9d
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Ativar a cópia de segurança para a pilha do Azure com o PowerShell

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Ativar o serviço de cópia de segurança de infraestrutura com o Windows PowerShell para efetuar cópias de segurança periódicas de:
 - Certificado de raiz e do serviço de identidade interna
 - Planos de utilizador, ofertas, subscrições
 - Segredos do Keyvault
 - Funções de RBAC de utilizador e políticas

Pode aceder os cmdlets do PowerShell para ativar a cópia de segurança, iniciar a cópia de segurança e obter informações de cópia de segurança através do ponto final de gestão de operador.

## <a name="prepare-powershell-environment"></a>Preparar o ambiente de PowerShell

Para obter instruções sobre como configurar o ambiente de PowerShell, consulte [instale o PowerShell para Azure pilha ](azure-stack-powershell-install.md).

## <a name="generate-a-new-encryption-key"></a>Gerar uma nova chave de encriptação

Instalar e PowerShell configurado para a pilha do Azure e as ferramentas de pilha do Azure.
 - Consulte [começar a trabalhar com o PowerShell na pilha de Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).
 - Consulte [ferramentas Transferir pilha do Azure a partir do GitHub](azure-stack-powershell-download.md)

Abra o Windows PowerShell com uma linha de comandos elevada e execute os seguintes comandos:
   
   ```powershell
    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
   ```
   
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
   Get-AzsBackupLocation | Select-Object -ExpandProperty externalStoreDefault | Select-Object -Property Path, UserName, Password | ConvertTo-Json
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