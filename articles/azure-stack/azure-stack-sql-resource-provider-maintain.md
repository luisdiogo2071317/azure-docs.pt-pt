---
title: Manter o fornecedor de recursos do SQL Server na pilha do Azure | Microsoft Docs
description: Saiba como pode manter o serviço de fornecedor de recursos do SQL Server na pilha do Azure.
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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: ad899739dab1dc51d64368d2136ab87f73f6f3a0
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300915"
---
# <a name="sql-resource-provider-maintenance-operations"></a>Operações de manutenção de fornecedor de recursos SQL

O fornecedor de recursos do SQL Server é executado numa máquina virtual bloqueada para baixo. Para ativar operações de manutenção, terá de atualizar a segurança da máquina virtual. Para fazê-lo utilizando o princípio do menor privilégio, pode utilizar [PowerShell apenas suficiente administração (JEA)](https://docs.microsoft.com/en-us/powershell/jea/overview) endpoint *DBAdapterMaintenance*. O pacote de instalação do fornecedor de recursos inclui um script para esta operação.

## <a name="patching-and-updating"></a>Aplicação de patches e atualizar

O fornecedor de recursos do SQL Server não está manutenção como parte da pilha do Azure, porque é um componente de suplemento. A Microsoft fornece atualizações para o fornecedor de recursos do SQL Server, conforme necessário. Quando for lançada uma placa atualizada do SQL Server, é fornecido um script para aplicar a actualização. Este script cria um novo fornecedor de recursos VM, migrar o estado do fornecedor antigo VM para a nova VM. Para obter mais informações, consulte [atualizar o fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-update.md).

### <a name="provider-virtual-machine"></a>Máquina virtual do fornecedor

Porque o fornecedor de recursos é executado num *utilizador* máquina virtual, terá de aplicar os patches necessários e as atualizações quando se está a ser lançados. Pode utilizar os pacotes de atualização do Windows que são fornecidos como parte do ciclo de patch e atualização para aplicar as atualizações para a VM.

## <a name="backuprestoredisaster-recovery"></a>Recuperação de cópia de segurança/restauro/após desastre

 Porque é um componente de suplemento, o fornecedor de recursos do SQL Server não é uma cópia de segurança como parte de um processo de Azure pilha negócio continuidade desastre recuperação (BCDR). Scripts serão fornecidos para as seguintes operações:

- Cópia de segurança de informações de estado (armazenadas numa conta de armazenamento de pilha do Azure).
- Se uma recuperação de pilha completa é necessária a restaurar o fornecedor de recursos.

>[!NOTE]
>Se tiver de efetuar uma recuperação, os servidores de base de dados devem ser recuperadas antes do fornecedor de recursos é restaurado.

## <a name="updating-sql-credentials"></a>Atualizar as credenciais do SQL

Está responsável pela criação e manutenção de contas de administrador do sistema em servidores SQL. O fornecedor de recursos tem uma conta com estas privilégios para gerir bases de dados para os utilizadores, mas não tem acesso a dados dos utilizadores. Se precisar de atualizar as palavras-passe de administrador do sistema em servidores SQL, pode utilizar a interface de administrador do fornecedor de recursos para alterar uma palavra-passe armazenadas. Estas palavras-passe é armazenadas no Cofre de chaves na sua instância de pilha do Azure.

Para modificar as definições, selecione **procurar** &gt; **recursos administrativo** &gt; **SQL que aloja servidores** &gt; **Inícios de sessão do SQL Server** e selecione um nome de utilizador. A alteração têm de ser efetuada na instância do SQL Server primeiro (e todas as réplicas, se necessário.) Em **definições**, selecione **palavra-passe**.

![Atualizar a palavra-passe de administrador](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="secrets-rotation"></a>Rotação de segredos

*Estas instruções aplicam-se apenas para Azure pilha integrada sistemas versão 1804 e mais tarde. Não tente rodar os segredos em versões de pilha do pre-1804 do Azure.*

Quando utilizar os fornecedores de recursos do SQL Server e o MySQL com pilha do Azure integrado sistemas, pode rodar os seguintes segredos de infraestrutura (implementação):

- Certificado SSL externo [fornecido durante a implementação](azure-stack-pki-certs.md).
- O recurso fornecedor VM administrador local conta palavra-passe fornecida durante a implementação.
- Palavra-passe da utilizador diagnóstico (dbadapterdiag) do fornecedor recursos.

### <a name="powershell-examples-for-rotating-secrets"></a>Exemplos do PowerShell para rodar segredos

**Altere todos os segredos ao mesmo tempo.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd  `
    -VMLocalCredential $localCreds
```

**Altere a palavra-passe de utilizador de diagnóstico.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd
```

**Altere a palavra-passe da conta de administrador local de VM.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds
```

**Altere a palavra-passe de certificado SSL.**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd
```

### <a name="secretrotationsqlproviderps1-parameters"></a>Parâmetros de SecretRotationSQLProvider.ps1

|Parâmetro|Descrição|
|-----|-----|
|AzCredential|Credencial de conta de administrador de serviços de pilha do Azure.|
|CloudAdminCredential|Azure pilha nuvem domínio conta credencial de administrador.|
|PrivilegedEndpoint|Ponto final com privilégios para aceder à Get AzureStackStampInformation.|
|DiagnosticsUserPassword|Palavra-passe da conta do utilizador diagnóstico.|
|VMLocalCredential|Conta de administrador local no MySQLAdapter VM.|
|DefaultSSLCertificatePassword|Predefinição certificado SSL (* pfx) palavra-passe.|
|DependencyFilesLocalPath|Caminho local do ficheiros de dependência.|
|     |     |

### <a name="known-issues"></a>Problemas conhecidos

**Problema**: registos de rotação de segredos.<br>
Os registos para rotação de segredos não são recolhidos automaticamente se o script personalizado rotação secreta falhar quando for executada.

**Solução**:<br>
Utilize o cmdlet Get-AzsDBAdapterLogs para recolher todos os registos do fornecedor de recursos, incluindo AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log, guardados no C:\Logs.

## <a name="update-the-virtual-machine-operating-system"></a>Atualize o sistema operativo da máquina virtual

Utilize um dos seguintes métodos para atualizar o sistema operativo da máquina virtual.

- Instale o pacote mais recente do fornecedor de recursos utilizando uma imagem do Windows Server 2016 Core patched atualmente.
- Instalar um pacote do Windows Update durante a instalação do ou Atualize para o fornecedor de recursos.

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Atualizar as definições do Windows Defender de máquina virtual

Para atualizar as definições do Windows Defender:

1. As definições do Windows Defender update a partir de transferência [Windows Defender definição](https://www.microsoft.com/en-us/wdsi/definitions).

   As definições de atualização de página, desloque para baixo para "Manualmente, transfira e instale as definições". Transfira o ficheiro de 64 bits "Windows Defender antivírus para o Windows 10 e Windows 8.1".

   Em alternativa, utilize [esta ligação direta](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64) para transferência/executar ficheiro fpam fe.exe.

2. Crie uma sessão do PowerShell para o ponto final de manutenção do SQL Server recursos fornecedor adaptador da máquina virtual.

3. Copie o ficheiro de atualização de definições para a máquina virtual utilizando a sessão de ponto final de manutenção.

4. Na sessão do PowerShell de manutenção, execute o *atualização DBAdapterWindowsDefenderDefinitions* comando.

5. Depois de instalar as definições, recomendamos que elimine o ficheiro de atualização de definições utilizando o *remover ItemOnUserDrive* comando.

**Exemplo de script do PowerShell para atualizar as definições.**

Pode editar e execute o seguinte script para atualizar as definições de Defender. Substitua os valores no script de valores do seu ambiente.

```powershell
# Set credentials for local admin on the resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Provide the public IP address for the adapter VM.
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download the Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.
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

## <a name="collect-diagnostic-logs"></a>Recolher registos de diagnóstico

Para recolher registos de máquina virtual bloqueada para baixo, pode utilizar o ponto final do PowerShell apenas suficiente administração (JEA) *DBAdapterDiagnostics*. Este ponto final fornece os seguintes comandos:

- **Get-AzsDBAdapterLog**. Este comando cria um pacote zip dos registos de diagnóstico do fornecedor de recursos e guarda o ficheiro na unidade de utilizador da sessão. Pode executar este comando sem quaisquer parâmetros e as últimas quatro horas de registos são recolhidas.
- **Remover AzsDBAdapterLog**. Este comando remove os pacotes de registo existentes no fornecedor de recursos VM.

### <a name="endpoint-requirements-and-process"></a>Requisitos de ponto final e processos

Quando um fornecedor de recursos está instalado ou atualizado, o **dbadapterdiag** é criada a conta de utilizador. Irá utilizar esta conta para recolher registos de diagnóstico.

>[!NOTE]
>A palavra-passe da conta de dbadapterdiag é o mesmo que a palavra-passe utilizada para o administrador local na máquina virtual que é criado durante uma implementação do fornecedor ou a atualização.

Para utilizar o *DBAdapterDiagnostics* comandos, criar uma sessão remota do PowerShell para a máquina virtual do fornecedor de recursos e executar o **Get-AzsDBAdapterLog** comando.

Defina o intervalo de tempo para a coleção de registo utilizando o **FromDate** e **ToDate** parâmetros. Se não especificar um ou ambos estes parâmetros, são utilizadas as seguintes predefinições:

- FromDate corresponde a quatro horas antes da hora atual.
- ToDate é o tempo atual.

**Exemplo de script do PowerShell para recolher registos.**

O script seguinte mostra como recolher registos de diagnóstico do fornecedor de recursos VM.

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
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
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session.
$session | Remove-PSSession
```

## <a name="next-steps"></a>Passos Seguintes

[Adicionar SQL Server que aloja servidores](azure-stack-sql-resource-provider-hosting-servers.md)
