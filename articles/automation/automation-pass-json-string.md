---
title: Passar um objeto JSON para um runbook da Automatização do Azure
description: Como passar parâmetros para um runbook como um objeto JSON
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
keywords: json do runbook do PowerShell, a automatização do azure
ms.openlocfilehash: 1bdeef02621924bbb7af1e676d2b275229761081
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "42061481"
---
# <a name="pass-a-json-object-to-an-azure-automation-runbook"></a>Passar um objeto JSON para um runbook da Automatização do Azure

Pode ser útil armazenar dados que pretende passar para um runbook num ficheiro JSON.
Por exemplo, poderá criar um ficheiro JSON que contém todos os parâmetros que pretende passar para um runbook.
Para tal, terá de converter o JSON para uma cadeia de caracteres e, em seguida, converter a cadeia de caracteres para um objeto do PowerShell antes de transmitir o seu conteúdo para o runbook.

Neste exemplo, vamos criar um script do PowerShell que chama [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook) para iniciar um runbook do PowerShell, passando o conteúdo do JSON para o runbook.
O runbook do PowerShell inicia uma VM do Azure, introdução os parâmetros para a VM a partir do JSON que foi passado.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, precisa do seguinte:

* Subscrição do Azure. Se ainda não tiver uma, pode [ativar as vantagens de subscritor do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou <a href="/pricing/free-account/" target="_blank">[inscrever-se numa conta gratuita](https://azure.microsoft.com/free/).
* [Conta de automatização](automation-sec-configure-azure-runas-account.md) para manter o runbook e autenticar-se nos recursos do Azure.  Esta conta tem de ter permissão para iniciar e parar a máquina virtual.
* Uma máquina virtual do Azure. Paramos e iniciamos esta máquina, pelo que não deve ser uma VM de produção.
* O Azure Powershell instalado numa máquina local. Ver [instalar e configurar o Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) para obter informações sobre como obter o Azure PowerShell.

## <a name="create-the-json-file"></a>Criar o ficheiro JSON

Escreva o seguinte teste de um ficheiro de texto e guarde-o como `test.json` em algum lugar no seu computador local.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-the-runbook"></a>Criar o runbook

Crie um novo runbook do PowerShell com o nome "Test-Json" na automatização do Azure.
Para saber como criar um novo runbook do PowerShell, veja [meu primeiro runbook do PowerShell](automation-first-runbook-textual-powershell.md).

Para aceitar os dados JSON, o runbook tem de executar um objeto como um parâmetro de entrada.

O runbook, em seguida, pode utilizar as propriedades definidas no JSON.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 Guardar e publicar este runbook na conta de automatização.

## <a name="call-the-runbook-from-powershell"></a>Chamar o runbook do PowerShell

Agora pode chamar o runbook do seu computador local com o Azure PowerShell.
Execute os seguintes comandos do PowerShell:

1. Inicie sessão no Azure:
   ```powershell
   Connect-AzureRmAccount
   ```
    São-lhe pedido que introduza as credenciais do Azure.

   > [!IMPORTANT]
   > **Add-AzureRmAccount** agora é um alias para **Connect-AzureRMAccount**. Quando pesquisar a sua biblioteca de itens, se não vir **Connect-AzureRMAccount**, pode utilizar **Add-AzureRmAccount**, ou pode atualizar os módulos na conta de automatização.

1. Obter o conteúdo do ficheiro JSON e convertê-la numa cadeia de caracteres:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath` é o caminho onde guardou o ficheiro JSON.
1. Converter o conteúdo de cadeia de caracteres de `$json` um objeto do PowerShell:
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. Criar uma tabela de hash para os parâmetros para `Start-AzureRmAutomationRunbook`:
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   Tenha em atenção que estiver a definir o valor de `Parameters` o objeto do PowerShell que contém os valores do ficheiro JSON. 
1. Iniciar o runbook
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

O runbook utiliza os valores do ficheiro JSON para iniciar uma VM.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre a edição de runbooks do PowerShell e o fluxo de trabalho do PowerShell com um editor de texto, consulte [editar runbooks textuais na automatização do Azure](automation-edit-textual-runbook.md) 
* Para saber mais sobre criar e importar runbooks, consulte [criar ou importar um runbook na automatização do Azure](automation-creating-importing-runbook.md)


