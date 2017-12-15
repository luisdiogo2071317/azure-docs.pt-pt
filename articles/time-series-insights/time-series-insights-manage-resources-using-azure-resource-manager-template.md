---
title: "Como gerir o seu ambiente de informações de séries de tempo do Azure utilizando modelos Azure Resource Manager | Microsoft Docs"
description: "Este artigo descreve como gerir o ambiente de informações de séries de tempo do Azure através de programação com o Azure Resource Manager."
services: time-series-insights
ms.service: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 12/08/2017
ms.openlocfilehash: b09d4a1aea56a4e306f80a1b43d519d313fd73ab
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="create-time-series-insights-resources-using-azure-resource-manager-templates"></a>Criar recursos de informações de séries de tempo utilizando os modelos Azure Resource Manager

Este artigo descreve como criar e implementar os recursos de informações de séries de tempo utilizando os modelos Azure Resource Manager, PowerShell e o fornecedor de recursos de informações de séries de tempo.

Informações de séries de tempo suporta os seguintes recursos:
   | Recurso | Descrição |
   | --- | --- |
   | Ambiente | Num ambiente de informações de séries de tempo é um agrupamento lógico de eventos que são ler a partir de mediadores de evento, armazenados e disponibilizados para consulta. Para obter mais informações consulte [planear o ambiente de informações de séries de tempo do Azure](time-series-insights-environment-planning.md) |
   | Origem de evento | Uma origem de evento é uma ligação a um mediador de eventos a partir da qual Insights de séries de tempo lê e ingere eventos para o ambiente. Origens de eventos atualmente suportados são IoT Hub e o Hub de eventos. |
   | Conjunto de dados de referência | Conjuntos de dados de referência fornecem metadados sobre os eventos no ambiente. Metadados de conjuntos de dados de referência serão associado de eventos de entrada. Conjuntos de dados de referência são definidos como recursos pelas respetivas propriedades chave do evento. Os metadados real que constitui o conjunto de dados de referência é carregado ou modificado através de dados plane APIs. |
   | Política de acesso | Políticas de acesso de conceder permissões para emitir consultas de dados, manipular dados de referência no ambiente e partilhar consultas guardadas e perspetivas associadas ao ambiente. Para obter mais informações consulte [conceder acesso a dados para um ambiente de informações de séries de tempo utilizando o portal do Azure](time-series-insights-data-access.md) |

Um modelo de Gestor de recursos é um ficheiro JSON que define a infraestrutura e a configuração de recursos num grupo de recursos. Para obter mais informações, consulte os seguintes documentos:

- [Gestor de recursos descrição geral do Azure - implementação do modelo](../azure-resource-manager/resource-group-overview.md#template-deployment)
- [Implementar recursos com modelos do Resource Manager e o Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)

O [201-timeseriesinsights-ambiente-com-eventhub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-timeseriesinsights-environment-with-eventhub) modelo de início rápido é publicado no GitHub. Este modelo cria um ambiente de informações de séries de tempo, uma origem de evento subordinada configurado para consumir eventos a partir de um Hub de eventos e políticas que concedam acesso a dados do ambiente de acesso. Se um Hub de eventos existentes não está especificado, será criada com a implementação.

## <a name="deploy-the-quickstart-template-locally-using-powershell"></a>Implementar o modelo de início rápido localmente utilizando o PowerShell

O procedimento seguinte descreve como utilizar o PowerShell para implementar um modelo Azure Resource Manager que cria um ambiente de informações de séries de tempo, uma origem de evento subordinada configurado para consumir eventos a partir de um Hub de eventos e políticas que concedam acesso a de acesso a dados do ambiente. Se um Hub de eventos existentes não está especificado, será criada com a implementação.

O fluxo de trabalho aproximado é o seguinte:

1. Instale o PowerShell.
1. Crie o modelo e um ficheiro de parâmetros.
1. No PowerShell, inicie sessão sua conta do Azure.
1. Se não existir, crie um novo grupo de recursos.
1. Teste a implementação.
1. Implemente o modelo.

### <a name="install-powershell"></a>Instalar o PowerShell

Instalar o Azure PowerShell, seguindo as instruções em [introdução ao Azure PowerShell](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Criar um modelo

Clone ou copiar o [201-timeseriesinsights-ambiente-com-eventhub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.json) modelo a partir do GitHub.

### <a name="create-a-parameters-file"></a>Criar um ficheiro de parâmetros

Para criar um ficheiro de parâmetros, copie o [201-timeseriesinsights-ambiente-com-eventhub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.parameters.json) ficheiro.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "eventHubNamespaceName": {
          "value": "GEN-UNIQUE"
      },
      "eventHubName": {
          "value": "GEN-UNIQUE"
      },
      "consumerGroupName": {
          "value": "GEN-UNIQUE"
      },
      "environmentName": {
        "value": "GEN-UNIQUE"
      },
      "eventSourceName": {
        "value": "GEN-UNIQUE"
      }
  }
}
```

#### <a name="required-parameters"></a>Parâmetros necessários

   | Parâmetro | Descrição |
   | --- | --- |
   | eventHubNamespaceName | O espaço de nomes do hub de eventos de origem. |
   | eventHubName | O nome do hub de eventos de origem. |
   | consumerGroupName | O nome do grupo de consumidores que irá utilizar o serviço de informações de série de tempo para ler os dados do hub de eventos. **Nota:** para evitar a contenção de recursos, este grupo de consumidores tem de ser dedicado para o serviço de informações de série de tempo e não partilhado com outros leitores. |
   | EnvironmentName | O nome do ambiente. Não é possível incluir o nome: ' <', ' >', '%', '&', ': ','\\','?', '/' e qualquer controlar carateres. Todos os outros carateres são permitidos.|
   | eventSourceName | O nome do recurso de subordinados de origem de evento. Não é possível incluir o nome: ' <', ' >', '%', '&', ': ','\\','?', '/' e qualquer controlar carateres. Todos os outros carateres são permitidos. |

#### <a name="optional-parameters"></a>Parâmetros opcionais

   | Parâmetro | Descrição |
   | --- | --- |
   | existingEventHubResourceId | Um ID de recurso opcional de um Hub de eventos existentes que será ligado para o ambiente de informações de séries de tempo através de origem do evento. **Nota:** o utilizador a implementar o modelo tem de ter privilégios para efetuar a operação de listkeys no Hub de eventos. Não se for passado nenhum valor, será criado um novo hub de eventos pelo modelo. |
   | environmentDisplayName | Um nome amigável opcional para mostrar em interfaces de utilizador ou ferramentas em vez do nome do ambiente. |
   | environmentSkuName | O nome do sku. Para obter mais informações, consulte o [página de preços de informações de séries de tempo](https://azure.microsoft.com/pricing/details/time-series-insights/).  |
   | environmentSkuCapacity | A capacidade de unidade do Sku. Para obter mais informações, consulte o [página de preços de informações de séries de tempo](https://azure.microsoft.com/pricing/details/time-series-insights/).|
   | environmentDataRetentionTime | O período de tempo mínimo eventos o ambiente estarão disponíveis para consulta. O valor tem de ser especificado no formato ISO 8601, por exemplo "P30D" para uma política de retenção de 30 dias. |
   | eventSourceDisplayName | Um nome amigável opcional para mostrar em interfaces de utilizador ou ferramentas em vez do nome de origem do evento. |
   | eventSourceTimestampPropertyName | A propriedade de evento que será utilizada como timestamp a origem de evento. Se não está especificado um valor para timestampPropertyName, ou se for especificada um valor nulo ou uma cadeia vazia, será utilizada a hora de criação de eventos. |
   | eventSourceKeyName | O nome da chave de acesso partilhado que o serviço de informações de série de tempo irá utilizar para ligar para o hub de eventos. |
   | accessPolicyReaderObjectIds | Uma lista de IDs dos utilizadores ou aplicações no Azure AD que devem ter acesso de leitor para o ambiente do objeto. O objectId de principal de serviço pode ser obtido ao chamar o **Get-AzureRMADUser** ou **Get-AzureRMADServicePrincipal** cmdlets. Ainda, a criar uma política de acesso para grupos do Azure AD não é suportada. |
   | accessPolicyContributorObjectIds | Uma lista de IDs dos utilizadores ou aplicações no Azure AD que devem ter acesso de contribuinte ao ambiente do objeto. O objectId de principal de serviço pode ser obtido ao chamar o **Get-AzureRMADUser** ou **Get-AzureRMADServicePrincipal** cmdlets. Ainda, a criar uma política de acesso para grupos do Azure AD não é suportada. |

Por exemplo, o seguinte ficheiro de parâmetros seria utilizado para criar um ambiente e uma origem de evento que lê eventos a partir de um hub de eventos existentes. Também cria duas políticas de acesso que conceda acesso de contribuinte para o ambiente.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "eventHubNamespaceName": {
            "value": "tsiTemplateTestNamespace"
        },
        "eventHubName": {
            "value": "tsiTemplateTestEventHub"
        },
        "consumerGroupName": {
            "value": "tsiTemplateTestConsumerGroup"
        },
        "environmentName": {
          "value": "tsiTemplateTestEnvironment"
        },
        "eventSourceName": {
          "value": "tsiTemplateTestEventSource"
        },
        "existingEventHubResourceId": {
          "value": "/subscriptions/{yourSubscription}/resourceGroups/MyDemoRG/providers/Microsoft.EventHub/namespaces/tsiTemplateTestNamespace/eventhubs/tsiTemplateTestEventHub"
        },
        "accessPolicyContributorObjectIds": {
            "value": [
                "AGUID001-0000-0000-0000-000000000000",
                "AGUID002-0000-0000-0000-000000000000"
            ]
        }
    }
  }
```

Para obter mais informações, consulte o [parâmetros](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) artigo.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Inicie sessão no Azure e definir a subscrição do Azure

A partir da linha de comandos do PowerShell, execute o seguinte comando:

```powershell
Login-AzureRmAccount
```

Lhe for pedido que inicie sessão na sua conta do Azure. Depois de iniciar sessão, execute o seguinte comando para ver as subscrições disponíveis:

```powershell
Get-AzureRMSubscription
```

Este comando devolve uma lista de subscrições do Azure disponíveis. Escolha uma subscrição para a sessão atual, executando o seguinte comando. Substitua `<YourSubscriptionId>` com o GUID da subscrição do Azure que pretende utilizar:

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Definir o grupo de recursos

Se não tiver um recurso existente de grupo, crie um novo grupo de recursos com o **New-AzureRmResourceGroup** comando. Forneça o nome do grupo de recursos e localização que pretende utilizar. Por exemplo:

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Se tiver êxito, é apresentado um resumo do novo grupo de recursos.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Testar a implementação

Validar a sua implementação, executando o `Test-AzureRmResourceGroupDeployment` cmdlet. Quando a implementação de teste, fornece os parâmetros exatamente tal como faria ao executar a implementação.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

### <a name="create-the-deployment"></a>Criar a implementação

Para criar a nova implementação, execute o `New-AzureRmResourceGroupDeployment` cmdlet e forneça os parâmetros necessários quando lhe for pedido. Os parâmetros incluem um nome para a sua implementação, o nome do seu grupo de recursos e o caminho ou URL para o ficheiro de modelo. Se o **modo** parâmetro não for especificado, o valor predefinido de **Incremental** é utilizado. Para obter mais informações, consulte [Incremental e completas implementações](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).

O seguinte comando pede-lhe para os parâmetros obrigatórios cinco na janela do PowerShell:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

Para especificar um ficheiro de parâmetros em vez disso, utilize o seguinte comando:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Também pode utilizar parâmetros inline ao executar o cmdlet de implementação. O comando é o seguinte:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Para executar um [concluída](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) implementação, defina o **modo** parâmetro para **concluída**:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

## <a name="verify-the-deployment"></a>Verificar a implementação

Se os recursos são implementados com êxito, é apresentado um resumo da implementação na janela do PowerShell:

```powershell
DeploymentName          : azuredeploy
ResourceGroupName       : MyDemoRG
ProvisioningState       : Succeeded
Timestamp               : 12/9/2017 01:06:54
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          eventHubNamespaceName  String               <eventHubNamespaceName>
                          eventHubName     String                     <eventHubName>
                          consumerGroupName  String                   <consumerGroupName>
                          existingEventHubResourceId  String
                          environmentName  String                     <environmentName>
                          environmentDisplayName  String
                          environmentSkuName  String                  S1
                          environmentSkuCapacity  Int                 1
                          environmentDataRetentionTime  String        P30D
                          eventSourceName  String                     <eventSourceName>
                          eventSourceDisplayName  String
                          eventSourceTimestampPropertyName  String
                          eventSourceKeyName  String                  manage
                          accessPolicyReaderObjectIds  Array          []
                          accessPolicyContributorObjectIds  Array     [
                            "AGUID001-0000-0000-0000-000000000000",
                            "AGUID002-0000-0000-0000-000000000000"
                          ]

Outputs                 :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          dataAccessFQDN   String
                          <guid>.env.timeseries.azure.com
```

## <a name="deploy-the-quickstart-template-through-the-azure-portal"></a>Implementar o modelo de início rápido através do portal do Azure

Home page do modelo de início rápido no GitHub também inclui um **implementar no Azure** botão. Clicar nela abre uma página de implementação personalizada no portal do Azure. Nesta página, pode introduzir ou selecionar valores para cada um dos parâmetros do [necessários parâmetros](time-series-insights-manage-resources-using-azure-resource-manager-template.md#required-parameters) ou [parâmetros opcionais](time-series-insights-manage-resources-using-azure-resource-manager-template.md#optional-parameters) tabelas. Depois de preencher as definições, clicar no **Compra** botão vai iniciar a implementação do modelo.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-timeseriesinsights-environment-with-eventhub%2Fazuredeploy.json" target="_blank"> <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Passos seguintes

- Para obter informações sobre programaticamente gerir recursos de informações de séries de tempo utilizando REST APIs, consulte [gestão de informações de séries de tempo](https://docs.microsoft.com/rest/api/time-series-insights-management/).
