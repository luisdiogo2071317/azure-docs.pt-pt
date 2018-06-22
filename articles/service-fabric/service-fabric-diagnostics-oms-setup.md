---
title: Azure Service Fabric - configurar a monitorização com o Log Analytics | Microsoft Docs
description: Saiba como configurar a análise de registos para visualizar e analisar eventos para monitorizar os clusters de Service Fabric do Azure.
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
ms.date: 4/03/2018
ms.author: srrengar
ms.openlocfilehash: 90a28162fb1f455c154ad4d2da7beac6bc785bc7
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301041"
---
# <a name="set-up-log-analytics-for-a-cluster"></a>Configurar a análise de registos para um cluster

Análise de registos é a nossa recomendação para monitorizar os eventos ao nível do cluster. Pode configurar a área de trabalho de análise de registos através do Azure Resource Manager, o PowerShell ou o Azure Marketplace. Se a manter um modelo do Resource Manager atualizada da sua implementação para utilização futura, utilize o mesmo modelo para configurar o ambiente de análise de registos. Implementação através do Marketplace é mais fácil se já tiver um cluster implementado com diagnóstico ativado. Se não tiver acesso de nível de subscrição da conta para o qual estiver a implementar, implemente utilizando o PowerShell ou o modelo do Resource Manager.

> [!NOTE]
> Para configurar o registo de análise para monitorizar o seu cluster, tem de ter ativado para ver eventos de plataforma ao nível do cluster ou de diagnóstico. Consulte [como configurar o diagnóstico na clusters do Windows](service-fabric-diagnostics-event-aggregation-wad.md) e [como configurar o diagnóstico na clusters do Linux](service-fabric-diagnostics-event-aggregation-lad.md) para obter mais informações

## <a name="deploy-a-log-analytics-workspace-by-using-azure-marketplace"></a>Implementar uma área de trabalho de análise de registos ao utilizar o Azure Marketplace

Se pretender adicionar uma área de trabalho de análise de registos, depois de implementar um cluster, visite no portal do Azure Marketplace e procure **análise de recursos de infraestrutura de serviço**. Esta é uma solução personalizada para implementações de Service Fabric que tenha dados específicos de Service Fabric. Este processo irá criar a solução (o dashboard para ver informações) e a área de trabalho (a agregação de dados de cluster subjacente).

1. Selecione **novo** no menu de navegação esquerdo. 

2. Procurar **serviço análise de recursos de infraestrutura**. Selecione o recurso que aparece.

3. Selecione **Criar**.

    ![Análise de recursos de infraestrutura de serviço no Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Na janela de criação de análise de recursos de infraestrutura de serviço, selecione **Selecione uma área de trabalho** para o **área de trabalho OMS** campo e, em seguida, **criar uma nova área de trabalho**. Preencha as entradas necessárias. O único requisito aqui é que a subscrição para o cluster do Service Fabric e a área de trabalho é o mesmo. Quando as suas entradas tem sido validadas, inicia a sua área de trabalho implementar. A implementação demora apenas alguns minutos.

5. Quando terminar, selecione **criar** novamente na parte inferior da janela de criação de análise de recursos de infraestrutura de serviço. Certifique-se de que a nova área de trabalho aparece em **área de trabalho OMS**. Esta ação adiciona a solução para a área de trabalho que criou.

Se estiver a utilizar o Windows, continue com os seguintes passos para estabelecer ligação a análise de registos para a conta do storage onde os eventos de cluster são armazenados. 

>[!NOTE]
>Ativar esta experiência para os clusters do Linux ainda não está disponível. 

### <a name="connect-the-log-analytics-workspace-to-your-cluster"></a>Ligar a área de trabalho de análise do registo ao cluster 

1. A área de trabalho tem de estar ligados dos dados de diagnóstico provenientes do seu cluster. Vá para o grupo de recursos no qual criou a solução de análise de recursos de infraestrutura de serviço. Selecione **ServiceFabric\<nameOfWorkspace\>**  e aceda à página de descrição geral. A partir daí, pode alterar as definições da solução, as definições da área de trabalho e a área de trabalho de análise do registo de acesso.

2. No menu de navegação esquerdo, em **origens de dados da área de trabalho**, selecione **registos de contas de armazenamento**.

3. No **os registos da conta de armazenamento** página, selecione **adicionar** na parte superior para adicionar registos do cluster para a área de trabalho.

4. Selecione **conta de armazenamento** para adicionar a conta apropriada criada no seu cluster. Se utilizou o nome predefinido, a conta de armazenamento é **sfdg\<resourceGroupName\>**. Também pode confirmar isto com o modelo Azure Resource Manager utilizado para implementar o cluster, ao verificar o valor utilizado para **applicationDiagnosticsStorageAccountName**. Se o nome não for apresentada, desloque para baixo e selecione **carregar mais**. Selecione o nome de conta de armazenamento.

5. Especifique o tipo de dados. Defina-o como **eventos de recursos de infraestrutura de serviço**.

6. Certifique-se de que a origem é automaticamente definida para **WADServiceFabric\*EventTable**.

7. Selecione **OK** para ligar a sua área de trabalho para registos do seu cluster.

    ![Adicionar os registos da conta de armazenamento para análise de registos](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

A conta agora apresentado como parte da sua conta do storage registos em origens de dados da sua área de trabalho.

Adicionou a solução de análise de recursos de infraestrutura de serviço numa área de trabalho de análise de registos que agora está corretamente ligada à plataforma do cluster e a tabela de registo de aplicação. Pode adicionar origens adicionais para a área de trabalho da mesma forma.


## <a name="deploy-log-analytics-by-using-a-resource-manager-template"></a>Implementar a análise de registos, utilizando um modelo do Resource Manager

Quando implementar um cluster utilizando um modelo do Resource Manager, o modelo cria uma nova área de trabalho de análise de registos, adiciona a solução de Service Fabric para a área de trabalho e configura-o para ler os dados das tabelas de armazenamento adequado.

Pode utilizar e modificar [este modelo de exemplo](https://github.com/krnese/azure-quickstart-templates/tree/master/service-fabric-oms) para satisfazer os seus requisitos.

Efetue as seguintes alterações:
1. Adicionar `omsWorkspaceName` e `omsRegion` para os parâmetros adicionando o seguinte fragmento a parâmetros definidos na sua *Template* ficheiro. Pode modificar os valores predefinidos, como julgar. Além disso, adicione os dois parâmetros novos na sua *Parameters. JSON* ficheiro para definir os respetivos valores para a implementação de recursos:
    
    ```json
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "sfomsworkspace",
        "metadata": {
            "description": "Name of your Log Analytics Workspace"
        }
    },
    "omsRegion": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "West Europe",
            "East US",
            "Southeast Asia"
        ],
        "metadata": {
            "description": "Specify the Azure Region for your Log Analytics workspace"
        }
    }
    ```

    O `omsRegion` valores têm de estar em conformidade com um conjunto específico de valores. Escolha a que esteja mais próximo para a implementação do cluster.

2. Se enviar os registos da aplicação para análise de registos, confirme primeiro que o `applicationDiagnosticsStorageAccountType` e `applicationDiagnosticsStorageAccountName` são incluídos como parâmetros no modelo. Se não estiver incluídas, adicioná-los para a secção de variáveis e editar os respetivos valores conforme necessário. Também pode inclui-los como parâmetros ao seguir o formato anterior.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Adicione a solução de Service Fabric para variáveis do modelo:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. Adicione o seguinte ao final da secção de recursos, após o qual o recurso de cluster do Service Fabric está declarado:

    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', parameters('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "id": "[resourceId('Microsoft.OperationsManagement/solutions/', variables('solution'))]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
    ```
    
    > [!NOTE]
    > Se tiver adicionado o `applicationDiagnosticsStorageAccountName` como uma variável, certifique-se de modificar cada referência ao mesmo para `variables('applicationDiagnosticsStorageAccountName')` em vez de `parameters('applicationDiagnosticsStorageAccountName')`.

5. Implementar o modelo como uma atualização do Gestor de recursos para o cluster utilizando o `New-AzureRmResourceGroupDeployment` API no módulo AzureRM PowerShell. Um comando de exemplo seria:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    O Azure Resource Manager detetar que este comando é uma atualização para um recurso existente. Processa apenas as alterações entre o modelo de implementação existente a ocasionar e o novo modelo fornecido.

## <a name="deploy-log-analytics-by-using-azure-powershell"></a>Implementar a análise de registos com o Azure PowerShell

Também pode implementar o recurso de análise de registos através do PowerShell, utilizando o `New-AzureRmOperationalInsightsWorkspace` comando. Para utilizar este método, certifique-se de que instalou [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1). Utilize este script para criar uma nova área de trabalho de análise de registos e adicionar-lhe a solução de Service Fabric: 

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

Quando tiver terminado, siga os passos na secção anterior para ligar a análise de registos para a conta de armazenamento adequado.

Também pode adicionar outras soluções ou efetue modificações adicionais à sua área de trabalho de análise de registos com o PowerShell. Para obter mais informações, consulte [Gerir análise de registos com o PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md).

## <a name="next-steps"></a>Passos Seguintes
* [Implementar o agente de análise do registo](service-fabric-diagnostics-oms-agent.md) para os nós para recolher os contadores de desempenho e recolher estatísticas de docker registos e para os contentores
* Obter familiarized com o [de registo de pesquisa e consultar](../log-analytics/log-analytics-log-searches.md) funcionalidades disponibilizadas como parte da análise de registos
* [Utilize o estruturador de vistas para criar vistas personalizadas no Log Analytics](../log-analytics/log-analytics-view-designer.md)
