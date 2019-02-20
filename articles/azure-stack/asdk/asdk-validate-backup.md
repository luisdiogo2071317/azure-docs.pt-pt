---
title: Validar uma cópia de segurança do Azure Stack com o ASDK | Documentos da Microsoft
description: Como validar uma cópia de segurança de sistemas integrados do Azure Stack com o ASDK.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 02/15/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 02/15/2019
ms.openlocfilehash: 31c5d068c8fcd0b6edea7cff63098131d848a14e
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416383"
---
# <a name="use-the-asdk-to-validate-an-azure-stack-backup"></a>Utilize o ASDK para validar uma cópia de segurança do Azure Stack
Depois de implementar o Azure Stack e o aprovisionamento de recursos de utilizador, tais como ofertas, planos, quotas e subscrições, deve [ativar cópia de segurança do Azure Stack infraestrutura](../azure-stack-backup-enable-backup-console.md). Agendamento e execução de cópias de segurança da infraestrutura regular irão garantir que os dados de gestão de infraestrutura não são perdidos se ocorrer uma falha de serviço ou de hardware de catastrófico.

> [!TIP]
> Recomendamos que [executar cópias de segurança a pedido](../azure-stack-backup-back-up-azure-stack.md) antes de iniciar este procedimento para garantir que tenha uma cópia dos dados mais recentes de infraestrutura disponíveis. Lembre-se de que capture o ID da cópia de segurança depois de concluir com êxito a cópia de segurança. Este ID será necessário durante a recuperação de nuvem. 

Cópias de segurança de infraestrutura do Azure Stack contenham dados importantes sobre a nuvem que pode ser restaurada durante a nova implementação do Azure Stack. Pode usar o ASDK para validar estas cópias de segurança sem afetar a sua nuvem de produção. 

A validar as cópias de segurança ASDK é suportada para os seguintes cenários:

|Cenário|Objetivo|
|-----|-----|
|Valide as cópias de segurança de infraestrutura de uma solução integrada.|Validação da vida útil curta que os dados na cópia de segurança são válidos.|
|Saiba o fluxo de trabalho ponto-a-ponto de recuperação.|Utilize ASDK para validar a cópia de segurança completa e restaurar a experiência.|
|     |     |

O seguinte cenário **není** suportada ao validar as cópias de segurança a ASDK:

|Cenário|Objetivo|
|-----|-----|
|Compilação ASDK para criar a cópia de segurança e restaurar.|Restaure dados de cópia de segurança de uma versão anterior do ASDK para uma versão mais recente.|
|     |     |


## <a name="cloud-recovery-deployment"></a>Implementação de recuperação na cloud
Cópias de segurança de infra-estrutura da sua implementação de sistemas integrados podem ser validadas pela execução de uma implantação de recuperação na cloud do ASDK. Este tipo de implementação, dados de serviço específico são restaurados a partir de cópia de segurança depois do ASDK está instalado no computador anfitrião.

### <a name="prereqs"></a>Pré-requisitos de recuperação de nuvem
Antes de iniciar uma implementação de recuperação na cloud do ASDK, certifique-se de que tem as seguintes informações:

**Requisitos de instalador de interface do Usuário**

*Instalador de interface do Usuário atual só suporta a chave de encriptação*

|Pré-requisito|Descrição|
|-----|-----|
|Caminho da partilha de cópia de segurança|O UNC partilha caminho da cópia de segurança mais recente do Azure Stack que será utilizada para recuperar informações de infraestrutura do Azure Stack. Esta partilha local será criada durante o processo de implementação de recuperação na cloud.|
|ID da cópia de segurança para restaurar|O ID de cópia de segurança, sob a forma de alfanumérico de "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", que identifica a cópia de segurança para serem restaurados durante a recuperação de nuvem.|
|IP do servidor de tempo|Um IP de servidor de hora válido, como 132.163.97.2, é necessário para implementação do Azure Stack.|
|Palavra-passe do certificado externo|A palavra-passe para o certificado externo utilizado pelo Azure Stack. A cópia de segurança de AC contém certificados externos que precisam ser restaurados com esta palavra-passe.|
|Chave de encriptação de cópia de segurança|Necessário se tiver atualizado para a versão do Azure Stack 1901 ou definições de cópia de segurança e posteriores ainda está configurado com uma chave de encriptação. Chave de encriptação foi preterida a partir de 1901. O instalador irá suportar a chave de encriptação com versões anteriores modo de compatibilidade para, pelo menos, 3 versões. Depois de atualizar as definições de cópia de segurança para utilizar um certificado, consulte a tabela seguinte para as informações necessárias.|

|     |     | 

**Requisitos de instalador do PowerShell**

*Instalador de PowerShell atual oferece suporte a certificado de encriptação de chave ou desencriptação*

|Pré-requisito|Descrição|
|-----|-----|
|Caminho da partilha de cópia de segurança|O UNC partilha caminho da cópia de segurança mais recente do Azure Stack que será utilizada para recuperar informações de infraestrutura do Azure Stack. Esta partilha local será criada durante o processo de implementação de recuperação na cloud.|
|ID da cópia de segurança para restaurar|O ID de cópia de segurança, sob a forma de alfanumérico de "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", que identifica a cópia de segurança para serem restaurados durante a recuperação de nuvem.|
|IP do servidor de tempo|Um IP de servidor de hora válido, como 132.163.97.2, é necessário para implementação do Azure Stack.|
|Palavra-passe do certificado externo|A palavra-passe para o certificado externo utilizado pelo Azure Stack. A cópia de segurança de AC contém certificados externos que precisam ser restaurados com esta palavra-passe.|
|Palavra-passe de certificação de desencriptação|Opcional. Obrigatório apenas se a cópia de segurança é encriptada através de um certificado. A palavra-passe destina-se com assinatura automática do certificado (. pfx) que contém a chave privada necessária para desencriptar os dados de cópia de segurança.|
|Chave de encriptação de cópia de segurança|Opcional. Necessário se tiver atualizado para a versão do Azure Stack 1901 ou definições de cópia de segurança e posteriores ainda está configurado com uma chave de encriptação. O instalador irá suportar a chave de encriptação com versões anteriores modo de compatibilidade para, pelo menos, 3 versões. Depois de atualizar as definições de cópia de segurança para utilizar um certificado, tem de fornecer a palavra-passe para o certificado de desencriptação.|
|     |     | 

## <a name="prepare-the-host-computer"></a>Preparar o computador anfitrião 
Como numa implementação de ASDK normal, o ambiente do sistema anfitrião ASDK deve estar preparado para a instalação. Quando o computador de anfitrião do kit de desenvolvimento foi preparado, ele será inicializado do disco de rígido da máquina virtual CloudBuilder.vhdx para iniciar a implantação de ASDK.

No computador anfitrião ASDK, transfira um novo cloudbuilder.vhdx correspondente para a mesma versão do Azure Stack que foram copiadas e siga as instruções para [preparar o computador do anfitrião ASDK](asdk-prepare-host.md).

Depois de reiniciado o servidor de anfitrião do cloudbuilder.vhdx, tem de criar uma partilha de ficheiros e copiar os dados de cópia de segurança para. A partilha de ficheiros deve ser acessível para a conta que executa a configuração; Administrador nestes comandos do PowerShell de exemplo: 

```powershell
$shares = New-Item -Path "c:\" -Name "Shares" -ItemType "directory"
$azsbackupshare = New-Item -Path $shares.FullName -Name "AzSBackups" -ItemType "directory"
New-SmbShare -Path $azsbackupshare.FullName -FullAccess ($env:computername + "\Administrator")  -Name "AzSBackups"
```

Em seguida, copie os ficheiros de cópia de segurança do Azure Stack mais recente para a partilha criada recentemente. A estrutura de pastas dentro da partilha deve ser: `\\<ComputerName>\AzSBackups\MASBackup\<BackupID>\`.

Por fim, copie o certificado de desencriptação (. pfx) para o diretório de certificado: `C:\CloudDeployment\Setup\Certificates\` e mude o nome de ficheiro a `BackupDecryptionCert.pfx`.

## <a name="deploy-the-asdk-in-cloud-recovery-mode"></a>Implementar o ASDK no modo de recuperação na cloud

> [!IMPORTANT]
> 1. O instalador da interface do Usuário atual só suporta a chave de encriptação. Só é possível validar as cópias de segurança de sistemas que continuam a utilizar a chave de encriptação. Se a cópia de segurança foi encriptada num sistema integrado ou ASDK usando o certificado, tem de utilizar o instalador do PowerShell (**InstallAzureStackPOC.ps1**). 
> 2. O instalador do PowerShell (**InstallAzureStackPOC.ps1**) oferece suporte a chave de encriptação ou certificados.
> 3. Instalação de ASDK suporta exatamente uma placa de interface de rede (NIC) para funcionamento em rede. Se tiver várias NICs, certifique-se de que apenas uma está ativada (e todas as outras desativadas) antes de executar o script de implementação.

### <a name="use-the-installer-ui-to-deploy-the-asdk-in-recovery-mode"></a>Utilizar o instalador da interface do Usuário para implementar o ASDK no modo de recuperação
Os passos nesta secção mostram como implementar o ASDK através de uma interface gráfica do usuário (GUI) fornecida ao transferir e executar o **asdk installer.ps1** script do PowerShell.

> [!NOTE]
> A interface de utilizador do instalador para o Development Kit do Azure Stack é um script aberto, com base no WCF e o PowerShell.

> [!IMPORTANT]
> O instalador da interface do Usuário atual só suporta a chave de encriptação.

1. Depois do computador anfitrião é inicializado com êxito na imagem CloudBuilder.vhdx, iniciam sessão utilizando as credenciais de administrador especificada quando [preparado o computador de anfitrião do kit de desenvolvimento](asdk-prepare-host.md) para a instalação de ASDK. Isso deve ser o mesmo que as credenciais de administrador local de anfitrião de kit de desenvolvimento.
2. Abra uma consola elevada do PowerShell e execute o  **&lt;letra de unidade > \AzureStack_Installer\asdk-installer.ps1** script do PowerShell. O script pode agora ser numa unidade diferente que C:\ na imagem CloudBuilder.vhdx. Clique em **recuperar**.

    ![Script do instalador ASDK](media/asdk-validate-backup/1.PNG) 

3. Introduza informações de diretório do Azure AD (opcionais) e a palavra-passe de administrador local para o computador do anfitrião ASDK na página de fornecedor e as credenciais de identidade. Clique em **Seguinte**.

    ![Página de identidades e credenciais](media/asdk-validate-backup/2.PNG) 

4. Selecione a placa de rede a ser utilizado pelo computador do anfitrião ASDK e clique em **seguinte**. Todas as outras interfaces de rede serão desativadas durante a instalação de ASDK. 

    ![Interface do adaptador de rede](media/asdk-validate-backup/3.PNG) 

5. Na página de configuração de rede, forneça o servidor de tempo válido e endereços IP do reencaminhador DNS. Clique em **Seguinte**.

    ![Página de configuração de rede](media/asdk-validate-backup/4.PNG) 

6. Quando as propriedades de cartão de interface de rede foram verificadas, clique em **seguinte**. 

    ![Verificação de definições de placa de rede](media/asdk-validate-backup/5.PNG) 

7. Indique as informações necessárias descritas anteriormente em [secção pré-requisitos](#prereqs) na página de definições de cópia de segurança e o nome de utilizador e palavra-passe a ser utilizado para aceder à partilha. Clique em **seguinte**: 

   ![Página de definições de cópia de segurança](media/asdk-validate-backup/6.PNG) 

8. Reveja o script de implementação a ser utilizado para implementar o ASDK na página de resumo. Clique em **Deploy** para iniciar a implantação. 

    ![Página de resumo](media/asdk-validate-backup/7.PNG) 


### <a name="use-powershell-to-deploy-the-asdk-in-recovery-mode"></a>Utilize o PowerShell para implementar o ASDK no modo de recuperação

Modificar os seguintes comandos do PowerShell para o seu ambiente e execute-os para implementar o ASDK no modo de recuperação de nuvem:

**Utilize o script de InstallAzureStackPOC.ps1 para iniciar a recuperação de cloud com a chave de encriptação.**

```powershell
cd C:\CloudDeployment\Setup     
$adminpass = Read-Host -AsSecureString -Prompt "Local Administrator password"
$certPass = Read-Host -AsSecureString -Prompt "Password for the external certificate"
$backupstorecredential = Read-Host -AsSecureString -Prompt "Credential for backup share"
$key = Read-Host -AsSecureString -Prompt "Your backup encryption key"

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass `
 -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
 -BackupEncryptionKeyBase64 $key `
 -BackupStoreCredential $backupstorecredential `
 -BackupId "<Backup ID to restore>" `
 -TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

**Utilize o script de InstallAzureStackPOC.ps1 para iniciar a recuperação de nuvem com o certificado de desencriptação.**

```powershell
cd C:\CloudDeployment\Setup     
$adminpass = Read-Host -AsSecureString -Prompt "Local Administrator password"
$certPass = Read-Host -AsSecureString -Prompt "Password for the external certificate"
$backupstorecredential = Read-Host -AsSecureString -Prompt "Credential for backup share"
$decryptioncertpassword  = Read-Host -AsSecureString -Prompt "Password for the decryption certificate"

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass `
 -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
 -BackupDecryptionCertPassword $decryptioncertpassword `
 -BackupStoreCredential $backupstorecredential `
 -BackupId "<Backup ID to restore>" `
 -TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

## <a name="complete-cloud-recovery"></a>Recuperação na cloud completa 
Após uma implementação de recuperação da cloud com êxito, tem de concluir o restauro utilizando o **AzureStack restauro** cmdlet. 

Depois de iniciar sessão como o operador de Azure Stack [instalar o Azure Stack do PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) e execute os seguintes comandos para especificar o certificado e a palavra-passe a utilizar ao restaurar a partir da cópia de segurança:

**Modo de recuperação com o ficheiro de certificado**

> [!NOTE] 
> A implementação do Azure Stack não persiste o certificado de desencriptação por motivos de segurança. Terá de fornecer novamente o certificado de desencriptação e a palavra-passe associada.

```powershell
$decryptioncertpassword = Read-Host -AsSecureString -Prompt "Password for the decryption certificate"
Restore-AzsBackup -ResourceId "<BackupID>" `
 -DecryptionCertPath "<path to decryption certificate with file name (.pfx)>" `
 -DecryptionCertPassword $decryptioncertpassword
```

**Modo de recuperação com a chave de encriptação**
```powershell
$decryptioncertpassword = Read-Host -AsSecureString -Prompt "Password for the decryption certificate"
Restore-AzsBackup -ResourceId "<BackupID>"
```

Aguarde 60 minutos após chamar este cmdlet para iniciar a verificação de dados de cópia de segurança na cloud recuperado ASDK.

## <a name="next-steps"></a>Passos Seguintes
[Registar o Azure Stack](asdk-register.md)

