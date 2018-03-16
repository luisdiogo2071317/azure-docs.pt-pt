---
title: "Azure Service Fabric - configurar a monitorização com o OMS Log Analytics | Microsoft Docs"
description: Saiba como configurar o Operations Management Suite para visualizar e analisar eventos para monitorizar os clusters de Service Fabric do Azure.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2017
ms.author: dekapur
ms.openlocfilehash: 98ac32b011744ce388762322edd538b467f93494
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="set-up-operations-management-suite-log-analytics-for-a-cluster"></a>Configurar a análise de registos do Operations Management Suite para um cluster

Pode configurar uma área de trabalho do Operations Management Suite (OMS) através do Azure Resource Manager, o PowerShell ou o Azure Marketplace. Se a manter um modelo do Resource Manager atualizada da sua implementação para utilização futura, utilize o mesmo modelo para configurar o ambiente do OMS. Implementação através do Marketplace é mais fácil se já tiver um cluster implementado com diagnóstico ativado. Se não tiver acesso de nível de subscrição na conta que pretende implementar OMS, implemente utilizando o PowerShell ou o modelo do Resource Manager.

> [!NOTE]
> Para configurar o OMS para monitorizar o seu cluster, tem de ter ativado para ver eventos de plataforma ao nível do cluster ou de diagnóstico.

## <a name="deploy-oms-by-using-azure-marketplace"></a>Implementar o OMS, utilizando o Azure Marketplace

Se pretender adicionar uma área de trabalho do OMS depois de implementar um cluster, visite no portal do Azure Marketplace e procure **análise de recursos de infraestrutura de serviço**:

1. Selecione **novo** no menu de navegação esquerdo. 

2. Procurar **serviço análise de recursos de infraestrutura**. Selecione o recurso que aparece.

3. Selecione **Criar**.

    ![Análise de SF OMS no Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Na janela de criação de análise de recursos de infraestrutura de serviço, selecione **Selecione uma área de trabalho** para o **área de trabalho OMS** campo e, em seguida, **criar uma nova área de trabalho**. Preencha as entradas necessárias. O único requisito aqui é que a subscrição para o cluster do Service Fabric e a área de trabalho do OMS é o mesmo. Quando as suas entradas tem sido validadas, a área de trabalho do OMS começa a implementar. A implementação demora apenas alguns minutos.

5. Quando terminar, selecione **criar** novamente na parte inferior da janela de criação de análise de recursos de infraestrutura de serviço. Certifique-se de que a nova área de trabalho aparece em **área de trabalho OMS**. Esta ação adiciona a solução para a área de trabalho que criou.

Se estiver a utilizar o Windows, continue com os seguintes passos para ligar OMS à conta do storage onde os eventos de cluster são armazenados. 

>[!NOTE]
>Ativar esta experiência para os clusters do Linux ainda não está disponível. 

### <a name="connect-the-oms-workspace-to-your-cluster"></a>Ligar a área de trabalho do OMS ao cluster 

1. A área de trabalho tem de estar ligados dos dados de diagnóstico provenientes do seu cluster. Vá para o grupo de recursos no qual criou a solução de análise de recursos de infraestrutura de serviço. Selecione **ServiceFabric\<nameOfOMSWorkspace\>**  e aceda à página de descrição geral. A partir daí, pode alterar as definições de solução, as definições de área de trabalho e aceder ao portal do OMS.

2. No menu de navegação esquerdo, em **origens de dados da área de trabalho**, selecione **registos de contas de armazenamento**.

3. No **os registos da conta de armazenamento** página, selecione **adicionar** na parte superior para adicionar registos do cluster para a área de trabalho.

4. Selecione **conta de armazenamento** para adicionar a conta apropriada criada no seu cluster. Se utilizou o nome predefinido, a conta de armazenamento é **sfdg\<resourceGroupName\>**. Também pode confirmar isto com o modelo Azure Resource Manager utilizado para implementar o cluster, ao verificar o valor utilizado para **applicationDiagnosticsStorageAccountName**. Se o nome não for apresentada, desloque para baixo e selecione **carregar mais**. Selecione o nome de conta de armazenamento.

5. Especifique o tipo de dados. Defina-o como **eventos de recursos de infraestrutura de serviço**.

6. Certifique-se de que a origem é automaticamente definida para **WADServiceFabric\*EventTable**.

7. Selecione **OK** para ligar a sua área de trabalho para registos do seu cluster.

    ![Adicionar os registos da conta de armazenamento ao OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

A conta agora apresentado como parte da sua conta do storage registos em origens de dados da sua área de trabalho.

Adicionou a solução de análise de recursos de infraestrutura de serviço numa área de trabalho de análise de registos do OMS que agora está corretamente ligada à plataforma do cluster e a tabela de registo de aplicação. Pode adicionar origens adicionais para a área de trabalho da mesma forma.


## <a name="deploy-oms-by-using-a-resource-manager-template"></a>Implementar o OMS, utilizando um modelo do Resource Manager

Quando implementar um cluster utilizando um modelo do Resource Manager, o modelo cria uma nova área de trabalho do OMS, adiciona a solução de Service Fabric para a área de trabalho e configura-o para ler os dados das tabelas de armazenamento adequado.

Pode utilizar e modificar [este modelo de exemplo](https://azure.microsoft.com/resources/templates/service-fabric-oms/) para satisfazer os seus requisitos. Os modelos que dão-lhe diferentes opções para configurar uma área de trabalho do OMS pode ser encontrado em [modelos de Service Fabric e OMS](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

Efetue as seguintes alterações:
1. Adicionar `omsWorkspaceName` e `omsRegion` para os parâmetros adicionando o seguinte fragmento a parâmetros definidos na sua *Template* ficheiro. Pode modificar os valores predefinidos, como julgar. Além disso, adicione os dois parâmetros novos na sua *Parameters. JSON* ficheiro para definir os respetivos valores para a implementação de recursos:
    
    ```json
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "sfomsworkspace",
        "metadata": {
            "description": "Name of your OMS Log Analytics Workspace"
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
            "description": "Specify the Azure Region for your OMS workspace"
        }
    }
    ```

    O `omsRegion` valores têm de estar em conformidade com um conjunto específico de valores. Escolha a que esteja mais próximo para a implementação do cluster.

2. Se enviar os registos da aplicação à OMS, confirme primeiro que o `applicationDiagnosticsStorageAccountType` e `applicationDiagnosticsStorageAccountName` são incluídos como parâmetros no modelo. Se não estiver incluídas, adicioná-los para a secção de variáveis e editar os respetivos valores conforme necessário. Também pode inclui-los como parâmetros ao seguir o formato anterior.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Adicione a solução de Service Fabric OMS variáveis do modelo:

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

## <a name="deploy-oms-by-using-azure-powershell"></a>Implementar o OMS, utilizando o Azure PowerShell

Também pode implementar o recurso de análise de registos do OMS através do PowerShell, utilizando o `New-AzureRmOperationalInsightsWorkspace` comando. Para utilizar este método, certifique-se de que instalou [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1). Utilize este script para criar uma área de trabalho de análise de registos do OMS novo e adicione-lhe a solução de Service Fabric: 

```PowerShell

$SubscriptionName = "<Name of your subscription>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<OMS Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Login-AzureRmAccount
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

Quando tiver terminado, siga os passos na secção anterior para ligar a análise de registos do OMS à conta de armazenamento adequado.

Também pode adicionar outras soluções ou efetue modificações adicionais à sua área de trabalho do OMS através do PowerShell. Para obter mais informações, consulte [Gerir análise de registos com o PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md).

## <a name="next-steps"></a>Passos Seguintes
* [Implementar o agente do OMS](service-fabric-diagnostics-oms-agent.md) para os nós para recolher os contadores de desempenho e recolher estatísticas de docker registos e para os contentores
* Obter familiarized com o [de registo de pesquisa e consultar](../log-analytics/log-analytics-log-searches.md) funcionalidades disponibilizadas como parte da análise de registos
* [Utilize o estruturador de vistas para criar vistas personalizadas no Log Analytics](../log-analytics/log-analytics-view-designer.md)
