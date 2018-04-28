---
title: Compilar configurações de DSC de automatização do Azure
description: Este artigo descreve como compilar as configurações de configuração de estado pretendido (DSC) para a automatização do Azure.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: b085a75acc4d7744587f0c93482435b5b8c28fc2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="compiling-configurations-in-azure-automation-dsc"></a>Compilar configurações de DSC de automatização do Azure

Pode compilar configurações de configuração de estado pretendido (DSC) de duas formas com a automatização do Azure: no portal do Azure e com o Windows PowerShell. A tabela seguinte ajuda-o a determinar quando deve utilizar o método com base em caraterísticas de cada:

### <a name="azure-portal"></a>Portal do Azure

* Método mais simples com a interface de utilizador interativa
* Formulário para fornecer valores de parâmetros simples
* Controlar facilmente o estado da tarefa
* Acesso autenticado com início de sessão do Azure

### <a name="windows-powershell"></a>Windows PowerShell

* Chamada a partir da linha de comandos com cmdlets do Windows PowerShell
* Pode ser incluído numa solução automatizada com vários passos
* Fornecer valores de parâmetros simples e complexos
* Controlar o estado da tarefa
* Cliente necessário para suportar os cmdlets do PowerShell
* Passar ConfigurationData
* As configurações que utilizarem credenciais de compilação

Depois de decidir um método de compilação, utilize os procedimentos seguintes para iniciar a compilação.

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a>Compilar uma configuração de DSC com o portal do Azure

1. Da sua conta de automatização, clique em **configurações de DSC**.
2. Clique uma configuração para abrir o painel.
3. Clique em **compilar**.
4. Se a configuração não tem parâmetros, lhe for pedido para confirmar que pretende compilá-la. Se a configuração tiver parâmetros, o **compilar a configuração** é aberto o painel para que possam fornecer valores de parâmetros. Consulte o seguinte [ **parâmetros básicos** ](#basic-parameters) secção para obter mais detalhes sobre os parâmetros.
5. O **tarefa de compilação** painel está aberto num modo a que pode controlar o estado da tarefa de compilação e as configurações de nó (documentos de configuração do MOF)-causado que seja colocada no servidor de solicitação do DSC do Azure da automatização.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>Compilar uma configuração de DSC com o Windows PowerShell

Pode utilizar [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) para iniciar a compilar com o Windows PowerShell. O código de exemplo seguinte inicia compilação de uma configuração de DSC chamada **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

`Start-AzureRmAutomationDscCompilationJob` Devolve um objeto de tarefa de compilação que pode utilizar para monitorizar o estado. Em seguida, pode utilizar este objeto de tarefa de compilação com [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) para determinar o estado da tarefa de compilação e [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) para ver o respetivos fluxos (saída). O código de exemplo seguinte inicia compilação do **SampleConfig** configuração, tem de aguardar até ser concluída e, em seguida, apresenta os fluxos.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a>Parâmetros básicos
Declaração de parâmetro nas configurações de DSC, incluindo os tipos de parâmetro e propriedades, funciona os mesmos que os runbooks de automatização do Azure. Consulte [iniciar um runbook na automatização do Azure](automation-starting-a-runbook.md) para saber mais sobre os parâmetros do runbook.

O exemplo seguinte utiliza dois parâmetros denominados **FeatureName** e **IsPresent**, para determinar os valores de propriedades no **ParametersExample.sample** configuração de nó, gerada durante a compilação.

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]

        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = "Present"
    if($IsPresent -eq $false)
    {
        $EnsureString = "Absent"
    }

    Node "sample"
    {
        WindowsFeature ($FeatureName + "Feature")
        {
            Ensure = $EnsureString
            Name = $FeatureName
        }
    }
}
```

Pode compilar configurações de DSC utilizar parâmetros básicos no portal do Automation DSC do Azure, ou com o Azure PowerShell:

### <a name="portal"></a>Portal

No portal, pode introduzir os valores de parâmetros depois de clicar em **compilar**.

![texto alternativo](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell

PowerShell requer parâmetros um [tabela hash](http://technet.microsoft.com/library/hh847780.aspx) em que a chave corresponde ao nome de parâmetro e o valor igual ao valor de parâmetro.

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

Para obter informações sobre a transmitir PSCredentials como parâmetros, consulte <a href="#credential-assets"> **ativos de credenciais** </a> abaixo.

## <a name="composite-resources"></a>Recursos compostos

**Recursos compostos** permitem-lhe utilizar configurações de DSC como recursos aninhados dentro de uma configuração. Isto permite-lhe aplicar configurações com várias para um único recurso. Consulte [recursos compostos: utilizar uma configuração de DSC como um recurso](https://docs.microsoft.com/powershell/dsc/authoringresourcecomposite) para saber mais sobre **composto de recursos**

> [!NOTE]
> Para que **recursos composto** para compilar corretamente, deve primeiro garantir que quaisquer recursos de DSC que depende o compostos primeiro estão instalados no repositório de módulos de conta de automatização do Azure ou se não importar corretamente.

Para adicionar um DSC **recursos composto**, tem de adicionar o módulo de recurso a um arquivo (*. zip). Vá para o repositório de módulos na sua conta de automatização do Azure. Em seguida, clique no botão 'Adicionar um módulo'.

![Adicionar módulo](./media/automation-dsc-compile/add_module.png)

Navegue para o diretório onde está localizado o arquivo. Selecione o ficheiro de arquivo e clique em OK.

![Selecione o módulo](./media/automation-dsc-compile/select_dscresource.png)

É direcionado novamente para o diretório de módulos, onde pode monitorizar o estado da sua **recursos composto** enquanto descompacta e regista com automatização do Azure.

![Importar recurso composto](./media/automation-dsc-compile/register_composite_resource.png)

Depois do módulo está registado, em seguida, pode clicar na mesma para validar que o **recursos composto** estão agora disponíveis para serem utilizados numa configuração.

![Validar composto recursos](./media/automation-dsc-compile/validate_composite_resource.png)

Em seguida, pode chamar o **recursos composto** para a configuração desta forma:

```powershell

    Node ($AllNodes.Where{$_.Role -eq "WebServer"}).NodeName
    {
            
            JoinDomain DomainJoin
            {
                DomainName = $DomainName
                Admincreds = $Admincreds
            }

            PSWAWebServer InstallPSWAWebServer
            {
                DependsOn = "[JoinDomain]DomainJoin"
            }        
    }

```

## <a name="configurationdata"></a>ConfigurationData
**ConfigurationData** permite separar estrutural configuração a partir de qualquer configuração específico do ambiente ao utilizar o PowerShell DSC. Consulte [separando "Que" a partir de "Onde" no DSC do PowerShell](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) para saber mais sobre **ConfigurationData**.

> [!NOTE]
> Pode utilizar **ConfigurationData** durante a compilação no DSC da automatização do Azure com o Azure PowerShell, mas não no portal do Azure.

A configuração de DSC de exemplo seguinte utiliza **ConfigurationData** através de **$ConfigurationData** e **$AllNodes** palavras-chave. Terá também de [ **xWebAdministration** módulo](https://www.powershellgallery.com/packages/xWebAdministration/) para este exemplo:

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure   = "Present"
        }
    }
}
```

Pode compilar a configuração de DSC anterior com o PowerShell. O PowerShell seguinte adiciona duas configurações de nó para o servidor de solicitação do DSC do Azure Automation: **ConfigurationDataSample.MyVM1** e **ConfigurationDataSample.MyVM3**:

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "MyVM1"
            Role = "WebServer"
        },
        @{
            NodeName = "MyVM2"
            Role = "SQLServer"
        },
        @{
            NodeName = "MyVM3"
            Role = "WebServer"
        }
    )

    NonNodeData = @{
        SomeMessage = "I love Azure Automation DSC!"
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ConfigurationDataSample" -ConfigurationData $ConfigData
```

## <a name="assets"></a>Elementos

Referências de recurso são os mesmos nas configurações de DSC de automatização do Azure e runbooks. O seguinte para obter mais informações, consulte:

* [Certificados](automation-certificates.md)
* [Ligações](automation-connections.md)
* [Credenciais](automation-credentials.md)
* [Variáveis](automation-variables.md)

### <a name="credential-assets"></a>Ativos de credenciais

Enquanto as configurações de DSC na automatização do Azure podem referenciar ativos de credenciais utilizando **Get-AutomationPSCredential**, ativos de credenciais podem também ser transmitidos através dos parâmetros, se assim o desejar. Se uma configuração assume um parâmetro de **PSCredential** escreva, em seguida, tem de transmitir o nome de cadeia de um recurso de credencial de automatização do Azure como valor do parâmetro, que, em vez de um objeto PSCredential. Nos bastidores, o recurso de credencial de automatização do Azure com esse nome é obtido e transmitido para a configuração.

Manter as credenciais seguras em configurações de nó (documentos de configuração do MOF) requer encriptar as credenciais no ficheiro MOF de configuração de nó. A automatização do Azure assume este passo adicional e encripta todo o ficheiro MOF. No entanto, atualmente tem indicar DSC do PowerShell é pode as credenciais ser debitado em texto simples durante a geração de MOF de configuração de nó, porque o PowerShell DSC não sabe que da automatização do Azure irá encriptar todo o ficheiro MOF após a respetiva geração através de uma tarefa de compilação.

Pode dizer DSC do PowerShell que pode as credenciais ser debitado em texto simples na configuração de nó geradas MOFs utilizando [ **ConfigurationData**](#configurationdata). Deverá passar `PSDscAllowPlainTextPassword = $true` através de **ConfigurationData** para o nome de cada bloco de nó que aparece na configuração de DSC e utiliza as credenciais.

O exemplo seguinte mostra uma configuração de DSC que utiliza um recurso de credencial de automatização.

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential "SomeCredentialAsset"

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath = "\\Server\share\path\file.ext"
            DestinationPath = "C:\destinationPath"
            Credential = $Cred
        }
    }
}
```

Pode compilar a configuração de DSC anterior com o PowerShell. O PowerShell seguinte adiciona duas configurações de nó para o servidor de solicitação do DSC do Azure Automation: **CredentialSample.MyVM1** e **CredentialSample.MyVM2**.

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "*"
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = "MyVM1"
        },
        @{
            NodeName = "MyVM2"
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "CredentialSample" -ConfigurationData $ConfigData
```

> [!NOTE]
> Depois de concluída a compilação poderá receber um erro a indicar: **o módulo de 'Management' não foi importado porque o snap-in 'Management' já foi importado.** Este aviso pode ser ignorado com segurança.

## <a name="importing-node-configurations"></a>Importar configurações de nó

Também pode importar configurações de nó (MOFs) que têm de ser compilado fora do Azure. Uma vantagem desta situação é que as configurações de nó podem ser assinadas.
Uma configuração de nó assinada é verificada localmente num nó gerido pelo agente de DSC, garantindo que a configuração a ser aplicada ao nó provenientes de uma origem autorizada.

> [!NOTE]
> Pode utilizar Importar assinado configurações na sua conta de automatização do Azure, mas a automatização do Azure não suporta atualmente compilar configurações assinadas.

> [!NOTE]
> Um ficheiro de configuração do nó tem de ser não superior a 1 MB para permitir que seja possível importar para a automatização do Azure.

Pode saber como assinar configurações de nó em https://msdn.microsoft.com/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.

### <a name="importing-a-node-configuration-in-the-azure-portal"></a>Importar uma configuração de nó no portal do Azure

1. Da sua conta de automatização, clique em **configurações de nó DSC** em **gestão de configuração**.

    ![Configurações de nó DSC](./media/automation-dsc-compile/node-config.png)
2. No **configurações de nó DSC** painel, clique em **adicionar um NodeConfiguration**.
3. No **importação** painel, clique no ícone de pasta junto a **ficheiro de configuração de nó** caixa de texto para procurar um ficheiro de configuração do nó (MOF) no seu computador local.

    ![Procurar ficheiro local](./media/automation-dsc-compile/import-browse.png)
4. Introduza um nome no **nome da configuração** caixa de texto. Este nome tem de corresponder ao nome da configuração a partir da qual a configuração do nó foi compilada.
5. Clique em **OK**.

### <a name="importing-a-node-configuration-with-powershell"></a>Importar uma configuração de nó com o PowerShell

Pode utilizar o [importação AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet para importar uma configuração de nó para a sua conta de automatização.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



