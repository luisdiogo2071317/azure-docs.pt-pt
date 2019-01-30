---
title: Manter o fornecedor de recursos do MySQL no Azure Stack | Documentos da Microsoft
description: Saiba como pode manter o serviço de fornecedor de recursos do MySQL no Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: fdc75f169ebd4b85e5e413277c265922fb27dfdb
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239728"
---
# <a name="mysql-resource-provider-maintenance-operations"></a>Operações de manutenção do fornecedor de recursos do MySQL

O fornecedor de recursos do MySQL é executado numa máquina virtual bloqueada. Para ativar as operações de manutenção, terá de atualizar a segurança da máquina virtual. Para tal, utilize o princípio de privilégio mínimo, pode utilizar DBAdapterMaintenance do ponto de extremidade do PowerShell Just Enough Administration (JEA). O pacote de instalação do fornecedor de recursos inclui um script para esta operação.

## <a name="update-the-virtual-machine-operating-system"></a>Atualize o sistema de operativo da máquina virtual

Como o fornecedor de recursos é executado num *utilizador* máquina virtual, tem de aplicar as atualizações e patches necessários quando terem sido lançadas. Pode utilizar os pacotes de atualização do Windows que são fornecidos como parte do ciclo do patch de atualização e para aplicar as atualizações para a VM.

Atualize a máquina de virtual de fornecedor utilizando um dos seguintes métodos:

- Instale o pacote mais recente do fornecedor de recursos com uma imagem do Windows Server 2016 Core atualmente corrigida.
- Instalar um pacote de atualização do Windows durante a instalação do ou Atualize para o fornecedor de recursos.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Atualizar as definições do Windows Defender de máquina virtual

Para atualizar as definições do Defender, siga estes passos:

1. As definições do Windows Defender update a partir de transferência [definição do Windows Defender](https://www.microsoft.com/en-us/wdsi/definitions).

    Na página de definições, desloque-se para baixo até "Manualmente, transfira e instale as definições". Transfira o ficheiro de 64 bits "Antivírus do Windows Defender para Windows 10 e Windows 8.1".

    Em alternativa, utilize [esse link direto](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) para download/execução o ficheiro de fpam fe.exe.

2. Abra uma sessão do PowerShell para o ponto final de manutenção do MySQL recursos fornecedor adaptador da máquina virtual.

3. Copie o ficheiro de atualização de definições para o adaptador do fornecedor de recursos VM com a sessão de ponto final de manutenção.

4. Na sessão do PowerShell de manutenção, execute o _DBAdapterWindowsDefenderDefinitions atualização_ comando.

5. Depois de instalar as definições, recomendamos que elimine o ficheiro de atualização de definições utilizando a _Remove-ItemOnUserDrive)_ comando.

**Exemplo de script do PowerShell para atualizar as definições.**

Pode editar e execute o seguinte script para atualizar as definições do Defender. Substitua os valores no script com valores do seu ambiente.

```powershell
# Set credentials for the local admin on the resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Provide the public IP address for the adapter VM.
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.  
Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' `
    -Outfile $localPathToDefenderUpdate  

# Create a session to the maintenance endpoint.
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $vmLocalAdminCreds -ConfigurationName DBAdapterMaintenance

# Copy the defender update file to the adapter virtual machine.
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\"

# Install the update definitions.
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}

# Cleanup the definitions package file and session.
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession

```

## <a name="secrets-rotation"></a>Rotação de segredos

*Estas instruções só se aplicam a sistemas integrados do Azure Stack.*

Quando utilizar os fornecedores de recursos do SQL e MySQL com o Azure Stack, sistemas integrados, o operador do Azure Stack é responsável por girando os segredos de infraestrutura de fornecedor de recursos seguintes para se certificar de que não expirar:

- Certificado de SSL externo [fornecido durante a implementação](azure-stack-pki-certs.md).
- A recurso fornecedor VM conta senha de administrador local fornecida durante a implementação.
- Senha de usuário de diagnóstico (dbadapterdiag) de fornecedor de recursos.

### <a name="powershell-examples-for-rotating-secrets"></a>Exemplos do PowerShell para efetuar a rotação de segredos

**Altere todos os segredos ao mesmo tempo.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd `  
    -VMLocalCredential $localCreds

```

**Altere a palavra-passe de utilizador de diagnóstico.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd

```

**Altere a palavra-passe da conta do administrador local de VM.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds

```

**Altere a palavra-passe de certificado SSL.**

```powershell
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd

```

### <a name="secretrotationmysqlproviderps1-parameters"></a>Parâmetros de SecretRotationMySQLProvider.ps1

|Parâmetro|Descrição|
|-----|-----|
|AzCredential|Credencial da conta de administrador de serviço de pilha do Azure.|
|CloudAdminCredential|O Azure Stack na cloud domínio conta credencial de administrador.|
|PrivilegedEndpoint|Ponto final com privilégios para aceder a Get-AzureStackStampInformation.|
|DiagnosticsUserPassword|Palavra-passe da conta do utilizador diagnóstico.|
|VMLocalCredential|A conta de administrador local na MySQLAdapter VM.|
|DefaultSSLCertificatePassword|Padrão de certificado SSL (* pfx) palavra-passe.|
|DependencyFilesLocalPath|Caminho local do ficheiros de dependência.|
|     |     |

### <a name="known-issues"></a>Problemas conhecidos

**Problema:**<br>
Os registos para a rotação de segredos não são recolhidos automaticamente se o script de rotação secreta falhar quando é executada.

**Solução:**<br>
Utilize o cmdlet Get-AzsDBAdapterLogs para recolher todos os logs de recursos fornecedor, incluindo AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log, salvo na C:\Logs.

## <a name="collect-diagnostic-logs"></a>Recolher registos de diagnóstico

Para recolher registos de máquina virtual bloqueada, pode utilizar o ponto de extremidade do PowerShell Just Enough Administration (JEA) DBAdapterDiagnostics. Este ponto final fornece os seguintes comandos:

- **Get-AzsDBAdapterLog**. Este comando cria um pacote zip dos registos de diagnóstico do fornecedor de recursos e guarda o ficheiro na unidade de utilizador da sessão. Pode executar este comando sem quaisquer parâmetros e as últimas quatro horas de registos são recolhidas.

- **Remove-AzsDBAdapterLog**. Este comando remove pacotes de registo existentes no fornecedor de recursos VM.

### <a name="endpoint-requirements-and-process"></a>Requisitos do ponto final e o processo

Quando um fornecedor de recursos é instalado ou atualizado, é criada a conta de utilizador dbadapterdiag. Irá utilizar esta conta para recolher registos de diagnóstico.

>[!NOTE]
>A palavra-passe da conta de dbadapterdiag é o mesmo que a palavra-passe utilizada para o administrador local na máquina virtual que é criado durante uma implementação de fornecedor ou a atualização.

Para utilizar o _DBAdapterDiagnostics_ comandos, criar uma sessão remota do PowerShell para a máquina de virtual de fornecedor de recursos e executar o **Get-AzsDBAdapterLog** comando.

Definir o intervalo de tempo de recolha de registos com o **FromDate** e **ToDate** parâmetros. Se não especificar um ou ambos os parâmetros, são usadas as seguintes predefinições:

* FromDate corresponde a quatro horas antes da hora atual.
* ToDate é a hora atual.

**Exemplo de script do PowerShell para recolher registos.**

O script seguinte mostra como recolher registos de diagnóstico a partir do fornecedor de recursos VM.

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'
$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds
        -ConfigurationName DBAdapterDiagnostics

# Sample that captures logs from the previous hour.
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs to the user drive.
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup the logs.
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove-AzsDBAdapterLog}
# Close the session.
$session | Remove-PSSession

```

## <a name="next-steps"></a>Passos Seguintes

[Remover o fornecedor de recursos do MySQL](azure-stack-mysql-resource-provider-remove.md)
