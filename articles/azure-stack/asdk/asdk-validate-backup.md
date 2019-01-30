---
title: Validar uma cópia de segurança do Azure Stack com o ASDK | Documentos da Microsoft
description: Como validar uma cópia de segurança de sistemas integrados do Azure Stack com o ASDK.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 09/05/2018
ms.openlocfilehash: 027d4a9f93032bfdd0f4cda96df74c92b5679540
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251576"
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



### <a name="cloud-recovery-prerequisites"></a>Pré-requisitos de recuperação de nuvem
Antes de iniciar uma implementação de recuperação na cloud do ASDK, certifique-se de que tem as seguintes informações:

|Pré-requisito|Descrição|
|-----|-----|
|Caminho da partilha de cópia de segurança.|O UNC partilha caminho da cópia de segurança mais recente do Azure Stack que será utilizada para recuperar informações de infraestrutura do Azure Stack. Esta partilha local será criada durante o processo de implementação de recuperação na cloud.|
|Chave de encriptação de cópia de segurança.|A chave de encriptação que foi utilizada para agendar cópia de segurança de infraestrutura para executar com o portal de administração do Azure Stack.|
|ID da cópia de segurança para restaurar.|O ID de cópia de segurança, sob a forma de alfanumérico de "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", que identifica a cópia de segurança para serem restaurados durante a recuperação de nuvem.|
|IP do servidor de tempo.|Um IP de servidor de hora válido, como 132.163.97.2, é necessário para implementação do Azure Stack.|
|Palavra-passe do certificado externo.|A palavra-passe para o certificado externo utilizado pelo Azure Stack. A cópia de segurança de AC contém certificados externos que precisam ser restaurados com esta palavra-passe.|
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

## <a name="deploy-the-asdk-in-cloud-recovery-mode"></a>Implementar o ASDK no modo de recuperação na cloud
O **InstallAzureStackPOC.ps1** script é utilizado para iniciar a recuperação de nuvem. 

> [!IMPORTANT]
> Instalação de ASDK suporta exatamente uma placa de interface de rede (NIC) para funcionamento em rede. Se tiver várias NICs, certifique-se de que apenas uma está ativada (e todas as outras desativadas) antes de executar o script de implementação.

Modificar os seguintes comandos do PowerShell para o seu ambiente e execute-os para implementar o ASDK no modo de recuperação de nuvem:

```powershell
cd C:\CloudDeployment\Setup     
$adminPass = Get-Credential Administrator
$key = ConvertTo-SecureString "<Your backup encryption key>" -AsPlainText -Force ` 
$certPass = Read-Host -AsSecureString  

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
-BackupEncryptionKeyBase64 $key -BackupStoreCredential $adminPass -BackupId "<Backup ID to restore>" `
-TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

## <a name="restore-infrastructure-data-from-backup"></a>Restaurar dados de infraestrutura de cópia de segurança
Após uma implementação de recuperação da cloud com êxito, tem de concluir o restauro utilizando o **AzureStack restauro** cmdlet. 

Depois de iniciar sessão como o operador de Azure Stack [instalar o Azure Stack do PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) e, em seguida, substituindo o seu ID de cópia de segurança para o `Name` parâmetro, execute o seguinte comando:

```powershell
Restore-AzsBackup -Name "<BackupID>"
```
Aguarde 60 minutos após chamar este cmdlet para iniciar a verificação de dados de cópia de segurança na cloud recuperado ASDK.

## <a name="next-steps"></a>Passos Seguintes
[Registar o Azure Stack](asdk-register.md)

