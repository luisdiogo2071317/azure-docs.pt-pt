---
title: O Azure Service Fabric - configurar a monitorização com o Log Analytics | Documentos da Microsoft
description: Saiba como configurar o Log Analytics para visualizar e analisar eventos para monitorizar os seus clusters do Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/11/2018
ms.author: srrengar
ms.openlocfilehash: a73a288852eea713623b65324853761e10fad282
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220471"
---
# <a name="set-up-log-analytics-for-a-cluster"></a>Configurar o Log Analytics para um cluster

O log Analytics é a nossa recomendação para monitorizar os eventos de nível de cluster. Pode configurar a área de trabalho do Log Analytics através do Azure Resource Manager, o PowerShell ou o Azure Marketplace. Se mantiver um modelo do Resource Manager atualizada da sua implementação para uso futuro, utilize o mesmo modelo para configurar o ambiente do Log Analytics. Implantação por meio de mercado é mais fácil se já tiver um cluster implementado com o diagnóstico ativado. Se não tiver acesso ao nível da subscrição na conta para a qual estiver a implementar, implemente com o PowerShell ou o modelo do Resource Manager.

> [!NOTE]
> Para configurar o Log Analytics para monitorizar o seu cluster, terá de ter o diagnóstico ativado para ver eventos de nível de cluster ou ao nível da plataforma. Consulte a [como configurar diagnósticos em clusters do Windows](service-fabric-diagnostics-event-aggregation-wad.md) e [como configurar diagnósticos em clusters do Linux](service-fabric-diagnostics-event-aggregation-lad.md) para obter mais informações

## <a name="deploy-a-log-analytics-workspace-by-using-azure-marketplace"></a>Implementar uma área de trabalho do Log Analytics com o Azure Marketplace

Se pretender adicionar uma área de trabalho do Log Analytics depois de implementar um cluster, vá para o Azure Marketplace no portal e procure **análise do Service Fabric**. Esta é uma solução personalizada para implementações do Service Fabric, que tem dados específicos para o Service Fabric. Este processo irá criar a solução (o dashboard para ver informações) e a área de trabalho (a agregação dos dados de cluster subjacente).

1. Selecione **New** no menu de navegação esquerdo. 

2. Procure **análise de recursos de infraestrutura do serviço**. Selecione o recurso que aparece.

3. Selecione **Criar**.

    ![Análise do Service Fabric no Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Na janela de criação de análise do Service Fabric, selecione **Selecione uma área de trabalho** para o **área de trabalho OMS** campo e, em seguida **criar uma nova área de trabalho**. Preencha as entradas necessárias. O único requisito aqui é que a subscrição para o cluster do Service Fabric e a área de trabalho é o mesmo. Quando as entradas foram validadas, sua área de trabalho inicie a implementação. A implementação demora apenas alguns minutos.

5. Quando terminar, selecione **criar** novamente na parte inferior da janela de criação de análise do Service Fabric. Certifique-se de que a área de trabalho nova é apresentada debaixo **área de trabalho OMS**. Esta ação adiciona a solução para a área de trabalho que criou.

Se estiver a utilizar o Windows, continue com os seguintes passos para ligar do Log Analytics para a conta de armazenamento onde os eventos de cluster são armazenados. 

>[!NOTE]
>Ativar esta experiência para os clusters do Linux ainda não está disponível. 

### <a name="connect-the-log-analytics-workspace-to-your-cluster"></a>Ligar a área de trabalho do Log Analytics ao seu cluster 

1. A área de trabalho precisa de estar ligado para os dados de diagnóstico originários do seu cluster. Vá para o grupo de recursos em que criou a solução de análise do Service Fabric. Selecione **ServiceFabric\<nameOfWorkspace\>**  e vá para a página de descrição geral. A partir daí, pode alterar as definições de solução, as definições de área de trabalho e acesso a área de trabalho do Log Analytics.

2. No menu de navegação esquerdo, sob **origens de dados de área de trabalho**, selecione **registos de contas de armazenamento**.

3. Sobre o **registos de conta de armazenamento** página, selecione **Add** na parte superior para adicionar registos do seu cluster para a área de trabalho.

4. Selecione **conta de armazenamento** para adicionar a conta apropriada criada no seu cluster. Se utilizou o nome predefinido, a conta de armazenamento for **sfdg\<resourceGroupName\>**. Também pode confirmar isto com o modelo Azure Resource Manager utilizado para implementar o seu cluster, ao verificar o valor utilizado para **applicationDiagnosticsStorageAccountName**. Se o nome não aparecer, desloque para baixo e selecione **carregar mais**. Selecione o nome de conta de armazenamento.

5. Especifique o tipo de dados. Defina-o como **eventos de Service Fabric**.

6. Certifique-se de que a origem é automaticamente definida como **WADServiceFabric\*EventTable**.

7. Selecione **OK** para ligar a sua área de trabalho para registos do seu cluster.

    ![Adicionar registos de conta de armazenamento para o Log Analytics](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

A conta são agora aparece como parte da sua conta de armazenamento registos nas origens de dados da sua área de trabalho.

Adicionou a solução de análise do Service Fabric numa área de trabalho do Log Analytics que agora está corretamente ligada a plataforma do seu cluster e a tabela de logs do aplicativo. Pode adicionar outras fontes para a área de trabalho da mesma forma.


## <a name="deploy-log-analytics-by-using-a-resource-manager-template"></a>Implementar o Log Analytics, utilizando um modelo do Resource Manager

Quando implementar um cluster utilizando um modelo do Resource Manager, o modelo cria uma nova área de trabalho do Log Analytics, adiciona a solução de Service Fabric para a área de trabalho e configura-o para ler dados a partir das tabelas de armazenamento adequado.

Pode utilizar e modificar [este modelo de exemplo](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) para satisfazer os seus requisitos. Este modelo faz o seguinte

* Cria um cluster do Service Fabric de 5 nós
* Cria uma área de trabalho do Log Analytics e a solução de Service Fabric
* Configura o agente do OMS para recolher e enviar 2 contadores de desempenho de exemplo para a área de trabalho
* Configura WAD para recolher o Service Fabric e envia-os para as tabelas de armazenamento do Azure (WADServiceFabric * EventTable)
* Configura a área de trabalho do Log Analytics para ler os eventos a partir dessas tabelas


Pode implementar o modelo como uma atualização do Gestor de recursos ao seu cluster, utilizando o `New-AzureRmResourceGroupDeployment` API no módulo AzureRM PowerShell. Um comando de exemplo seria:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "<resourceGroupName>" -TemplateFile "<templatefile>.json" 
``` 

O Azure Resource Manager Deteta que este comando é uma atualização de um recurso existente. Só processa as alterações entre o modelo de dirigir a implementação existente e o novo modelo fornecido.

## <a name="deploy-log-analytics-by-using-azure-powershell"></a>Implementar o Log Analytics com o Azure PowerShell

Também pode implementar o seu recurso do Log Analytics através do PowerShell ao utilizar o `New-AzureRmOperationalInsightsWorkspace` comando. Para utilizar este método, certifique-se de que instalou [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1). Utilize este script para criar uma nova área de trabalho do Log Analytics e adicionar a solução de Service Fabric para o mesmo: 

```PowerShell

$SubscriptionName = "<Name of your subscription>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

Quando tiver terminado, siga os passos na secção anterior para ligar o Log Analytics para a conta de armazenamento adequado.

Também pode adicionar outras soluções ou fazer outras modificações a sua área de trabalho do Log Analytics com o PowerShell. Para obter mais informações, consulte [gerir o Log Analytics com o PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md).

## <a name="next-steps"></a>Passos Seguintes
* [Implementar o agente do Log Analytics](service-fabric-diagnostics-oms-agent.md) para os nós para recolher contadores de desempenho e recolher estatísticas de docker e registos para os seus contentores
* Familiarizar-se com o [pesquisas e consultas de registo](../log-analytics/log-analytics-log-searches.md) funcionalidades oferecidos como parte do Log Analytics
* [Utilizar o estruturador de vistas para criar vistas personalizadas no Log Analytics](../log-analytics/log-analytics-view-designer.md)
