---
title: Ativos de certificado na automatização do Azure
description: Certificados podem ser armazenados em segurança na automatização do Azure para que possam ser acedidos por runbooks ou configurações de DSC para se autenticarem do Azure e os recursos de terceiros.  Este artigo explica os detalhes de certificados e como trabalhar com os mesmos no texto e gráficos de criação.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 70d5c777f98a1e09125c7ef42ba597009bde084b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34194342"
---
# <a name="certificate-assets-in-azure-automation"></a>Ativos de certificado na automatização do Azure

Certificados podem ser armazenados em segurança na automatização do Azure para que possam ser acedidos por runbooks ou configurações de DSC utilizando o **Get-AzureRmAutomationCertificate** atividade para recursos do Azure Resource Manager. Esta capacidade permite-lhe criar runbooks e configurações de DSC que utilizam certificados para autenticação ou adiciona-os para o Azure ou recursos de terceiros.

>[!NOTE]
>Proteger recursos na automatização do Azure incluem as credenciais, certificados, ligações e as variáveis encriptadas. Estes elementos são encriptados e armazenados na automatização do Azure com uma chave exclusiva que é gerada para cada conta de automatização. Esta chave é armazenada no Cofre de chaves. Antes de o armazenamento de um recurso seguro, a chave é carregada a partir do Cofre de chaves e, em seguida, utilizada para encriptar o elemento.

## <a name="azurerm-powershell-cmdlets"></a>Cmdlets do AzureRM PowerShell
Para AzureRM, os cmdlets na tabela seguinte são utilizados para criar e gerir recursos de credencial de automatização com o Windows PowerShell. Estes são enviados como parte do [AzureRM.Automation módulo](/powershell/azure/overview) que está disponível para utilização nos runbooks de automatização e configurações de DSC.

|Cmdlets|Descrição|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate)|Obtém informações sobre um certificado a utilizar num runbook ou configuração de DSC. Apenas pode obter o certificado propriamente dito da atividade de Get-AutomationCertificate.|
|[New-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationcertificate)|Cria um novo certificado para a automatização do Azure.|
[Remove-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationcertificate)|Remove um certificado da automatização do Azure.|Cria um novo certificado para a automatização do Azure.
|[Set-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationcertificate)|Define as propriedades de um certificado existente, incluindo o carregamento do ficheiro de certificado e definir a palavra-passe para um ficheiro. pfx.|
|[Add-AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Carrega um certificado de serviço para o serviço de nuvem especificada.|

## <a name="activities"></a>Atividades
As atividades na tabela seguinte são utilizadas para aceder aos certificados de um runbook e configurações de DSC.

| Atividades | Descrição |
|:---|:---|
|Get-AutomationCertificate|Obtém um certificado a utilizar num runbook ou configuração de DSC. Devolve um [System.Security.Cryptography.X509Certificates.X509Certificate2](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate2.aspx) objeto.|

> [!NOTE] 
> Deve evitar utilizar variáveis no parâmetro – Name de **Get-AutomationCertificate** num runbook ou configuração de DSC conforme complicates deteção de dependências entre runbooks ou configuração de DSC e as variáveis da automatização no momento da concepção.

## <a name="python2-functions"></a>Funções de Python2

A função na tabela seguinte é utilizada para aceder aos certificados num Python2 runbook.

| Função | Descrição |
|:---|:---|
| automationassets.get_automation_certificate | Obtém informações sobre um recurso de certificado. |

> [!NOTE]
> Tem de importar o **automationassets** módulo no início do runbook Python para poder aceder as funções de recurso.

## <a name="creating-a-new-certificate"></a>Criar um novo certificado

Quando cria um novo certificado, carregar um ficheiro. cer ou. pfx para automatização do Azure. Se marcar o certificado como exportável, pode transferi-la fora do arquivo de certificados de automatização do Azure. Se não é exportável, em seguida, esta só pode ser utilizada para assinatura dentro do runbook ou configuração de DSC. A automatização do Azure necessita do certificado com o fornecedor: **Microsoft avançada RSA e fornecedor de criptografia AES**.

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Para criar um novo certificado com o portal do Azure

1. Da sua conta de automatização, clique em de **ativos** mosaico para abrir o **ativos** painel.
1. Clique em de **certificados** mosaico para abrir o **certificados** painel.
1. Clique em **adicionar um certificado** na parte superior do painel.
1. Escreva um nome para o certificado no **nome** caixa.
1. Para procurar um ficheiro. cer ou. pfx, clique em **selecione um ficheiro** em **carregar um ficheiro de certificado**. Se selecionar um ficheiro. pfx, especifique uma palavra-passe e se é permitido para ser exportada.
1. Clique em **criar** para guardar o novo elemento de certificado.

### <a name="to-create-a-new-certificate-with-windows-powershell"></a>Para criar um novo certificado com o Windows PowerShell

O exemplo seguinte demonstra como criar um novo certificado de automatização e marcá-la exportável. Esta ação importa um ficheiro. pfx existente.

```powershell-interactive
$certName = 'MyCertificate'
$certPath = '.\MyCert.pfx'
$certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup
```

## <a name="using-a-certificate"></a>Utilizar um certificado

Para utilizar um certificado, utilize o **Get-AutomationCertificate** atividade. Não é possível utilizar o [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) cmdlet, uma vez que devolve informações sobre o recurso de certificado, mas não o certificado propriamente dito.

### <a name="textual-runbook-sample"></a>Exemplo de textual runbook

O código de exemplo seguinte mostra como adicionar um certificado para um serviço em nuvem num runbook. Neste exemplo, a palavra-passe é obtida a partir de uma variável de automatização encriptados.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-sample"></a>Exemplo de um runbook gráfico

Adicionar um **Get-AutomationCertificate** para um runbook gráfico ao clicar no certificado no painel de biblioteca do editor gráfico e seleção de **adicionar à tela**.

![Adicionar certificado à tela](media/automation-certificates/automation-certificate-add-to-canvas.png)

A imagem seguinte mostra um exemplo de utilização de um certificado de um runbook gráfico. Este é o mesmo do exemplo anterior para adicionar um certificado para um serviço em nuvem a partir de um runbook textual.

![Criação de gráficos de exemplo ](media/automation-certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Exemplo de Python2
O exemplo seguinte mostra como aceder a certificados em Python2 runbooks.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert 
```

## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações sobre como trabalhar com ligações para controlar o fluxo lógico de atividades de runbook foi concebido para efetuar, consulte [nas hiperligações na criação de gráficos](automation-graphical-authoring-intro.md#links-and-workflow). 
