---
title: Ativar cópia de segurança para o Azure Stack com o PowerShell | Documentos da Microsoft
description: Ative o serviço de cópia de segurança de infraestrutura com o Windows PowerShell para que o Azure Stack pode ser restaurado se ocorrer uma falha.
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
ms.openlocfilehash: 4fb40904e59e78e416d4598472a6adeb498e49f4
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968889"
---
# <a name="enable-backup-for-azure-stack-with-powershell"></a>Ativar cópia de segurança para o Azure Stack com o PowerShell

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Ativar o serviço de cópia de segurança da infraestrutura com o Windows PowerShell reserve cópias de segurança periódicas de:
 - Certificado de serviço e a raiz de identidades interno
 - Planos de utilizador, ofertas, as subscrições
 - Segredos do Cofre de chaves
 - Políticas e funções de RBAC do utilizador

Pode acessar os cmdlets do PowerShell para ativar cópia de segurança, inicie a cópia de segurança e obter informações de cópia de segurança através de ponto final de gestão de operador.

## <a name="prepare-powershell-environment"></a>Preparar o ambiente do PowerShell

Para obter instruções sobre como configurar o ambiente do PowerShell, consulte [instalar o PowerShell para o Azure Stack ](azure-stack-powershell-install.md). Para iniciar sessão no Azure Stack, veja [configurar o ambiente de operador e inicie sessão no Azure Stack](azure-stack-powershell-configure-admin.md).

## <a name="provide-the-backup-share-credentials-and-encryption-key-to-enable-backup"></a>Forneça a chave de encriptação, credenciais e partilha de cópia de segurança para ativar a cópia de segurança

Na mesma sessão do PowerShell, edite o seguinte script do PowerShell ao adicionar as variáveis para o seu ambiente. Execute o script atualizado para fornecer a chave de encriptação, credenciais e partilha de cópia de segurança para o serviço de cópia de segurança da infraestrutura.

| Variável        | Descrição   |
|---              |---                                        |
| $username       | Tipo de **nome de utilizador** com o domínio e o nome de utilizador para o local de unidade compartilhada com acesso suficiente para ler e escrever ficheiros. Por exemplo, `Contoso\backupshareuser`. |
| $key            | Tipo de **chave de encriptação** utilizado para encriptar cada cópia de segurança. |
| $password       | Tipo de **palavra-passe** para o utilizador. |
| $sharepath      | Escreva o caminho para o **localização de armazenamento de cópia de segurança**. Tem de utilizar uma cadeia de caracteres de convenção de Nomenclatura Universal (UNC) para o caminho para uma partilha de ficheiros hospedado num dispositivo separado. Uma cadeia de caracteres UNC Especifica a localização de recursos, tais como ficheiros partilhados ou dispositivos. Para garantir a disponibilidade dos dados de cópia de segurança, o dispositivo deve estar num local separado. |

   ```powershell
    $username = "domain\backupadmin"
   
    $Secure = Read-Host -Prompt ("Password for: " + $username) -AsSecureString
    $Encrypted = ConvertFrom-SecureString -SecureString $Secure
    $password = ConvertTo-SecureString -String $Encrypted
    
    $BackupEncryptionKeyBase64 = ""
    $tempEncryptionKeyString = ""
    foreach($i in 1..64) { $tempEncryptionKeyString += -join ((65..90) + (97..122) | Get-Random | % {[char]$_}) }
    $tempEncryptionKeyBytes = [System.Text.Encoding]::UTF8.GetBytes($tempEncryptionKeyString)
    $BackupEncryptionKeyBase64 = [System.Convert]::ToBase64String($tempEncryptionKeyBytes)
    $BackupEncryptionKeyBase64
    
    $Securekey = ConvertTo-SecureString -String $BackupEncryptionKeyBase64 -AsPlainText -Force
    $Encryptedkey = ConvertFrom-SecureString -SecureString $Securekey
    $key = ConvertTo-SecureString -String $Encryptedkey
    
    $sharepath = "\\serverIP\AzSBackupStore\contoso.com\seattle"

    Set-AzSBackupShare -BackupShare $sharepath -Username $username -Password $password -EncryptionKey $key
   ```
   
##  <a name="confirm-backup-settings"></a>Confirme as definições de cópia de segurança

Na mesma sessão do PowerShell, execute os seguintes comandos:

   ```powershell
    Get-AzsBackupLocation | Select-Object -Property Path, UserName, AvailableCapacity
   ```

O resultado deverá ser semelhante a seguinte saída:

   ```powershell
    Path                        : \\serverIP\AzSBackupStore\contoso.com\seattle
    UserName                    : domain\backupadmin
    AvailableCapacity           : 60 GB
   ```

## <a name="next-steps"></a>Passos Seguintes

 - Aprenda a executar uma cópia de segurança, consulte [cópia de segurança do Azure Stack](azure-stack-backup-back-up-azure-stack.md ).  
 - Saiba como verificar se executou a cópia de segurança, consulte [Confirmar cópia de segurança foi concluída no portal de administração](azure-stack-backup-back-up-azure-stack.md ).
