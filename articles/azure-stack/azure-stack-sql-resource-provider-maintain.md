---
title: Bases de dados SQL a utilizar na pilha do Azure | Microsoft Docs
description: Saiba como pode implementar bases de dados do SQL Server como um serviço na pilha do Azure e os passos rápidos para implementar o adaptador de fornecedor de recursos do SQL Server.
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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 53436d131672622ae1a72a1bb84d5aa83fdbdc0c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="maintenance-operations"></a>Operações de manutenção 
O fornecedor de recursos do SQL Server é um bloqueado para baixo de máquina virtual. A atualização de segurança de recursos fornecedor da máquina virtual pode ser feita o ponto final do PowerShell apenas suficiente administração (JEA) _DBAdapterMaintenance_. Um script é fornecido com o pacote de instalação no RP para facilitar a estas operações.

## <a name="patching-and-updating"></a>Aplicação de patches e atualizar
O fornecedor de recursos do SQL Server não está manutenção como parte da pilha de Azure conforme é um componente de suplemento. Microsoft irá fornecer atualizações para o fornecedor de recursos do SQL Server, conforme necessário. O fornecedor de recursos do SQL Server é instanciado num _utilizador_ máquina virtual sob a subscrição do fornecedor predefinido. Por conseguinte, é necessário fornecer patches do Windows, assinaturas do antivírus, etc. Os Windows update a pacotes que são fornecidos como parte do ciclo de patch e atualização pode ser utilizado para aplicar as atualizações para a VM do Windows. Quando for lançada uma placa de atualizados, é fornecido um script para aplicar a actualização. Este script cria uma nova VM RP e migra qualquer Estado que já tenha.

 ## <a name="backuprestoredisaster-recovery"></a>Recuperação de cópia de segurança/restauro/após desastre
 O fornecedor de recursos do SQL Server não é uma cópia de segurança como parte do processo de pilha de Azure BC-DR, dado que é um componente de suplemento. Scripts serão fornecidos para facilitar a:
- Cópia de segurança de informações de estado necessários (armazenadas numa conta de armazenamento de pilha do Azure)
- Restaurar o RP em caso de uma recuperação de pilha completa torna-se necessário.
Servidores de base de dados devem ser recuperadas primeiro (se necessário), antes do recurso de fornecedor é restaurado.

## <a name="updating-sql-credentials"></a>Atualizar as credenciais do SQL
É responsável pela criação e manutenção de contas de administrador de sistema nos seus servidores SQL. O fornecedor de recursos tem uma conta com estas privilégios para gerir bases de dados em nome dos utilizadores - não é necessário acesso aos dados dessas bases de dados. Se precisar de atualizar as palavras-passe de sa nos seus servidores SQL, pode utilizar a capacidade de atualização da interface de administrador do fornecedor de recursos para alterar a palavra-passe armazenadas utilizado pelo fornecedor de recursos. Estas palavras-passe é armazenadas no Cofre de chaves na sua instância de pilha do Azure.

Para modificar as definições, clique em **procurar** &gt; **recursos administrativo** &gt; **SQL que aloja servidores** &gt; **Inícios de sessão do SQL Server** e selecione um nome de início de sessão. A alteração têm de ser efetuada na instância do SQL Server primeiro (e todas as réplicas, se necessário). No **definições** painel, clique em **palavra-passe**.

![Atualizar a palavra-passe de administrador](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="update-the-virtual-machine-operating-system"></a>Atualize o sistema operativo da máquina virtual
Existem várias formas de atualizar a VM do Windows Server:
* Instalar o pacote mais recente do fornecedor de recursos utilizando uma imagem do Windows Server 2016 Core atualmente patched
* Instalar um pacote do Windows Update durante a instalação ou atualização no RP

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>Atualizar as definições do Windows Defender de máquina virtual
Siga estes passos para atualizar as definições de Defender:

1. As definições do Windows Defender update a partir de transferência [definição do Windows Defender](https://www.microsoft.com/en-us/wdsi/definitions)

    Nessa página, em "Manualmente, transfira e instale as definições" transferir "Windows Defender antivírus do Windows 10 e Windows 8.1" ficheiro de 64 bits. 
    
    Ligação direta: https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64

2. Criar uma sessão do PowerShell para o ponto final de manutenção do SQL Server RP adaptador da máquina virtual
3. Copie o ficheiro de atualização de definições para a máquina de placa de base de dados utilizando a sessão de ponto final de manutenção
4. Sobre a manutenção do PowerShell sessão invocar o _atualização DBAdapterWindowsDefenderDefinitions_ comando
5. Após a instalação, é recomendado para remover o ficheiro de atualização de definições utilizadas. Podem ser removido a sessão de manutenção utilizando o _remover ItemOnUserDrive)_ comando.


Eis um exemplo de script para atualizar as definições de Defender (substituir o endereço ou o nome da máquina virtual com o valor real):

```powershell
# Set credentials for the diagnostic user
$diagPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$diagCreds = New-Object System.Management.Automation.PSCredential `
    ("dbadapterdiag", $vmLocalAdminPass)$diagCreds = Get-Credential

# Public IP Address of the DB adapter machine
$databaseRPMachine  = "XX.XX.XX.XX"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"
 
# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions. 
Invoke-WebRequest -Uri https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64 `
    -Outfile $localPathToDefenderUpdate 

# Create session to the maintenance endpoint
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $diagCreds -ConfigurationName DBAdapterMaintenance
# Copy defender update file to the db adapter machine
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\mpam-fe.exe"
# Install the update file
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinitions -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession
```


## <a name="collect-diagnostic-logs"></a>Recolher registos de diagnóstico
O fornecedor de recursos do SQL Server é um bloqueado para baixo de máquina virtual. Se for necessário recolher registos de máquina virtual, um ponto final de PowerShell apenas suficiente administração (JEA) _DBAdapterDiagnostics_ é fornecido para o efeito. Existem dois comandos estão disponíveis através deste ponto final:

* Get-AzsDBAdapterLog - prepara um pacote zip que contém os registos de diagnóstico RP e coloca-o na unidade de utilizador de sessão. O comando pode ser chamado sem parâmetros e irá recolher as últimos quatro horas de registos.
* Remove-AzsDBAdapterLog - limpa os pacotes de registo existentes no fornecedor de recursos VM

Uma conta de utilizador denominada _dbadapterdiag_ é criada durante a implementação de RP ou atualização para ligar ao ponto final de diagnóstico para extrair os registos RP. A palavra-passe desta conta é o mesmo que a palavra-passe fornecida para a conta de administrador local durante a implementação/atualização.

Para utilizar estes comandos, terá de criar uma sessão remota do PowerShell para a máquina virtual do fornecedor de recursos e invocar o comando. Opcionalmente, pode fornecer parâmetros FromDate e ToDate. Se não especificar um ou ambos, a FromDate quatro horas antes da hora atual, e o ToDate será a hora atual.

Este script de exemplo demonstra a utilização destes comandos:

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<see above>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, $diagnosticsUserPassword)
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
        -ConfigurationName DBAdapterDiagnostics

# Sample captures logs from the previous one hour
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup logs
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session
$session | Remove-PSSession
```

## <a name="next-steps"></a>Passos Seguintes
[Adicionar SQL Server que aloja servidores](azure-stack-sql-resource-provider-hosting-servers.md)
