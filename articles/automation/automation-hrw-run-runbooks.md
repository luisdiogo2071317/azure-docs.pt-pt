---
title: Executar runbooks num trabalho de Runbook híbrida de automatização do Azure
description: Este artigo fornece informações sobre a execução de runbooks em computadores no seu local datacenter ou o fornecedor de nuvem com a função Runbook Worker híbrido.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/05/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 6fc89469e1a9b2d7142e38f7aea5596fc9adb4e4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Runbooks em execução num Runbook Worker híbrido

Não há qualquer diferença na estrutura de runbooks que são executados na automatização do Azure e os que executam o um Runbook Worker híbrido. Os Runbooks que utiliza com cada provavelmente diferem significativamente entanto, uma vez que os runbooks direcionada para um Runbook Worker híbrido normalmente gerir recursos no próprio computador local ou relativamente aos recursos no ambiente local onde está implementada, enquanto os runbooks do A automatização do Azure, normalmente, gerir os recursos na nuvem do Azure.

Quando criar runbooks para executar um Runbook Worker híbrido, deve editar e testar runbooks num computador que aloja o worker híbrido. A máquina do anfitrião tem todos os módulos do PowerShell e acesso de rede que tem de gerir e aceder aos recursos locais. Depois de um runbook foi editado e testado no computador de trabalho híbrida, pode carregá-lo para o ambiente de automatização do Azure onde está disponível para ser executada no worker híbrido. É importante saber que as tarefas executadas sob a conta de sistema Local que pode introduzir ligeiras diferenças, quando criar runbooks para um Runbook Worker híbrido Isto deve ser levado em consideração.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>A iniciar um runbook no Runbook Worker híbrido

[Iniciar um Runbook na automatização do Azure](automation-starting-a-runbook.md) descreve diferentes métodos para iniciar um runbook. Runbook Worker híbrido adiciona um **RunOn** opção onde pode especificar o nome de um grupo de trabalho de Runbook híbrida. Se não for especificado um grupo, em seguida, o runbook está obtido e execute por uma dos trabalhadores nesse grupo. Se esta opção não for especificada, em seguida, é executado na automatização do Azure como habitualmente.

Quando inicia um runbook no portal do Azure, é-lhe apresentado um **executar em** opção onde pode selecionar **Azure** ou **Worker híbrido**. Se selecionar **Worker híbrido**, em seguida, pode selecionar o grupo de uma lista pendente.

Utilize o **RunOn** parâmetro. Pode utilizar o seguinte comando para iniciar um runbook denominado Test-Runbook num grupo de trabalho de Runbook híbrida denominado MyHybridGroup através do Windows PowerShell.

```powershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> O **RunOn** parâmetro foi adicionado para o **AzureAutomationRunbook início** cmdlet na versão 0.9.1 do Microsoft Azure PowerShell. Deve [transferir a versão mais recente](https://azure.microsoft.com/downloads/) se tiver um anteriormente instalado. Só tem de instalar esta versão numa estação de trabalho em que estiver a iniciar o runbook a partir do Windows PowerShell. Não é necessário instalá-lo no computador de trabalho, a menos que pretende iniciar runbooks a partir desse computador. Atualmente não é possível iniciar um runbook num Runbook Worker híbrido partir de outro runbook, uma vez que isto iria requer a versão mais recente do Azure Powershell para ser instalada na sua conta de automatização. A versão mais recente é atualizada automaticamente na automatização do Azure e automaticamente enviadas por push para baixo para os trabalhadores em breve.

## <a name="runbook-permissions"></a>Permissões de Runbook

Os Runbooks em execução num Runbook Worker híbrido não é possível utilizar o mesmo método que é normalmente utilizado para autenticação de runbooks nos recursos do Azure, uma vez que estão a aceder a recursos fora do Azure. O runbook ou pode fornecer a sua própria autenticação aos recursos locais, ou pode especificar uma conta RunAs para fornecer um contexto de utilizador para todos os runbooks.

### <a name="runbook-authentication"></a>Autenticação de Runbook

Por predefinição, os runbooks executados no contexto da conta do sistema local no computador local, pelo que têm de fornecer as suas próprias autenticação a recursos que acedem a.

Pode utilizar [credencial](automation-credentials.md) e [certificado](automation-certificates.md) ativos no runbook com os cmdlets que permitem-lhe especificar as credenciais para que pode autenticar em recursos diferentes. O exemplo seguinte mostra uma parte de um runbook que reinicia um computador. Este obtém as credenciais de um recurso de credenciais e o nome do computador de um recurso de variável e, em seguida, utiliza estes valores com o cmdlet de reiniciar o computador.

```powershell-interactive
$Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
$Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

Também pode tirar partido [InlineScript](automation-powershell-workflow.md#inlinescript), que lhe permite executar blocos de código noutro computador com as credenciais especificadas pelo [parâmetro comum de PSCredential](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="runas-account"></a>Conta RunAs

Em vez de ter runbooks fornecer as seus próprios autenticação aos recursos locais, pode especificar um **RunAs** conta para um grupo de trabalho híbrida. Especificar um [recurso de credencial](automation-credentials.md) que tem acesso aos recursos locais e todos os runbooks executados estas credenciais quando em execução num Runbook Worker híbrido no grupo.

O nome de utilizador para a credencial tem de ser um dos seguintes formatos:

* domínio ome de utilizador
* username@domain
* nome de utilizador (para contas locais no computador local)

Utilize o procedimento seguinte para especificar uma conta RunAs para um grupo de trabalho híbridas:

1. Criar um [recurso de credencial](automation-credentials.md) com acesso aos recursos locais.
2. Abra a conta de automatização no portal do Azure.
3. Selecione o **grupos de trabalho híbrida** mosaico e, em seguida, selecione o grupo.
4. Selecione **todas as definições** e, em seguida, **definições do grupo de trabalho híbrida**.
5. Alteração **Run** de **predefinido** para **personalizada**.
6. Selecione a credencial e clique em **guardar**.

### <a name="automation-run-as-account"></a>Conta Run As de automatização

Como parte do processo de compilação automatizada para a implementação de recursos no Azure, pode necessitar de acesso a sistemas para suportar uma tarefa ou o conjunto de passos numa sequência de implementação no local. Para suportar a autenticação no Azure utilizando a conta Run As, tem de instalar o certificado da conta Run As.

O runbook do PowerShell seguinte, *exportação RunAsCertificateToHybridWorker*, exporta o certificado Run As da conta de automatização do Azure e transfere e importa-os para o arquivo de certificados do computador local numa versão híbrida trabalho ligado para a mesma conta. Depois de concluído este passo, verifica que o worker pode autenticar com êxito para o Azure utilizando a conta Run As.

```powershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
Run this runbook in the hybrid worker where you want the certificate installed.
This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
AUTHOR: Azure Automation Team
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test that authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm Azure Resource Manager calls are working
Get-AzureRmAutomationAccount | Select-Object AutomationAccountName
```

Guardar o *exportação RunAsCertificateToHybridWorker* runbook para o seu computador com um `.ps1` extensão. Importe-o para a sua conta de automatização e editar o runbook, alterar o valor da variável `$Password` com a sua própria palavra-passe. Publicar e, em seguida, executar o runbook direcionada para o grupo de trabalho híbrida que são executados e autenticar runbooks com a conta Run As. O fluxo de trabalho relatórios ao tentar importar o certificado para o arquivo do computador local e está de acordo com várias linhas, dependendo de quantos contas de automatização são definidas na sua subscrição e se a autenticação é efetuada com êxito.

## <a name="job-behavior"></a>Comportamento de tarefa

As tarefas são processadas ligeiramente diferente no Runbook Workers híbridos de são quando são executadas em sandboxes do Azure. Um principal diferença é que não há nenhum limite na duração de tarefa num Runbook Workers híbridos. Se tiver um runbook de longa execução que pretende garantir que é resiliente para reiniciar possíveis, por exemplo se reinicia o computador que aloja o worker híbrido. Se a máquina de anfitrião de worker híbrido é reiniciado, em seguida, todas as tarefas de runbook em execução for reiniciado desde o início ou a partir do último ponto de verificação para runbooks do fluxo de trabalho do PowerShell. Se uma tarefa de runbook for reiniciada mais de 3 horas, em seguida, estiver suspenso.

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a>Resolução de problemas de runbooks num Runbook Worker híbrido

Os registos são armazenados localmente em cada função de trabalho híbrida no C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. Híbridos também registam erros e eventos no registo de eventos do Windows no **aplicações e serviços Logs\Microsoft-SMA\Operational**. Os eventos relacionados com os runbooks executados na função de trabalho são escritos **aplicações e serviços Logs\Microsoft-Automation\Operational**. O **Microsoft SMA** registo inclui muitas mais eventos relacionados com a tarefa de runbook enviada para o trabalho e o processamento do runbook. Enquanto o **automatização do Microsoft** registo de eventos tem muitos eventos com detalhes a prestar assistência com a resolução de problemas de execução do runbook, contém o resultado da tarefa de runbook.

[Runbook de saída e mensagens](automation-runbook-output-and-messages.md) são enviadas para a automatização do Azure do híbrida workers, tal como tarefas de runbook é executado na nuvem. Também pode ativar os fluxos de verboso e progresso da mesma forma que faria para outros runbooks.

Se os runbooks não são concluir com êxito e a tarefa de resumo mostra um Estado de **suspenso**, consulte o artigo de resolução de problemas [Runbook Worker híbrido: uma tarefa de runbook termina com o estado suspenso](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre os diferentes métodos que podem ser utilizados para iniciar um runbook, consulte o artigo [iniciar um Runbook na automatização do Azure](automation-starting-a-runbook.md).
* Para compreender os diferentes procedimentos para trabalhar com runbooks do PowerShell e o fluxo de trabalho do PowerShell na automatização do Azure utilizando o editor de texto, consulte [editar um Runbook na automatização do Azure](automation-edit-textual-runbook.md)
