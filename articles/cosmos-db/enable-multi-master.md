---
title: Ativar com vários mestres para contas do Azure Cosmos DB
description: Este artigo descreve como ativar o suporte de vários mestre ao criar uma conta do Azure Cosmos DB com o portal do Azure, PowerShell, CLI ou um modelo Azure Resource Manager.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 71579e28f389d91498ef5f37c5d43dc9d5140234
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963224"
---
# <a name="enable-multi-master-for-azure-cosmos-db-accounts"></a>Ativar com vários mestres para contas do Azure Cosmos DB

Suporte de vários mestre é ativado ao criar uma conta do Azure Cosmos DB. Uma conta do Azure Cosmos DB pode ser criada com o portal do Azure, PowerShell, CLI ou um modelo Azure Resource Manager.

> [!IMPORTANT]
> Atualmente, o suporte de vários mestre pode ser ativado para apenas novas contas de Azure Cosmos DB. Contas existentes do Azure Cosmos DB não é possível utilizar a funcionalidade. Estamos a trabalhar para fornecer suporte para as contas existentes e irá anunciar esse suporte quando estiver disponível.

Depois de criar uma conta do Azure Cosmos DB com o suporte de vários mestre, pode criar bases de dados, contentores, carregar documentos e atribuir políticas de resolução de conflito. Para resolução de conflitos em exemplos de código com vários mestres, consulte [exemplos de código de vários mestres](multi-master-conflict-resolution.md#code-samples) artigo.

## <a name="enable-multi-master-using-azure-portal"></a>Ativar com vários mestres, através do portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com/)

2. Clique em selecionar **criar um recurso > bases de dados > do Azure Cosmos DB**.

3. Na **nova conta** painel, introduza as seguintes definições para uma nova conta do Azure Cosmos DB:

   |**Definição**  |**Valor sugerido** |**Descrição**|
   |---------|---------|---------|
   |Subscrição   | {A sua subscrição}  |Selecione a subscrição do Azure a utilizar para esta conta do Azure Cosmos DB.  |
   |Grupo de Recursos  |   {O nome do grupo de recursos}    |  Selecione um grupo de recursos existente ou selecione **criar novo**, em seguida, introduza um novo nome de grupo de recursos para a sua conta. |
   |Nome da Conta | {nome da sua conta}   |  Introduza um nome exclusivo para identificar a sua conta do Azure Cosmos DB.        |
   |API  |   Qualquer   |  Selecione um tipo de API.   |
   |Localização  | Selecionar qualquer região   | Selecione a localização geográfica na qual vai alojar a sua conta do Azure Cosmos DB. Pode escolher qualquer região, uma vez que esta conta será em várias regiões.  |
   |Ativar redundância geográfica   |  Ativar  |  Selecione para ativar principal múltiplo seja selecionado abaixo.   |
   |Ativar Principal Múltiplo | Ativar  | Selecione para ativar principal múltiplo para esta conta. |


## <a name="using-multi-master-in-sdks"></a>Com múltiplos principais no SDKs

Com uma conta com vários mestres ativada, dentro dos seus aplicativos, pode aproveitar mastering multi ao tirar partido do ConnectionPolicy, conforme mostrado abaixo.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
{
   ConnectionMode = ConnectionMode.Direct,
   ConnectionProtocol = Protocol.Tcp,
   UseMultipleWriteLocations = true,
};
policy.PreferredLocations.Add("West US");
policy.PreferredLocations.Add("North Europe");
policy.PreferredLocations.Add("Southeast Asia");
```

## <a name="enable-multi-master-using-powershell"></a>Ativar vários mestres com o PowerShell

Também pode criar uma conta do Cosmos DB ativada com múltiplos principal ao definir o `enableMultipleWriteLocations` parâmetro como "true". Para criar uma conta do Azure Cosmos DB com vários mestres ativado, abra uma janela do PowerShell e execute o seguinte script:

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = "<ip-range-filter>"

$consistencyPolicy = @{"defaultConsistencyLevel"="Session";
                       "maxIntervalInSeconds"= "10";
                       "maxStalenessPrefix"="200"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

## <a name="enable-multi-master-using-cli"></a>Ativar vários mestres com a CLI

Pode ativar vários mestres definindo o parâmetro de enable-múltiplo-escrita-localizações como "true". Para criar uma conta do Azure Cosmos DB com vários mestres ativado, abra o CLI do Azure ou na cloud shell e execute o seguinte comando:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --default-consistency-level "Session" \
   --enable-automatic-failover "true" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="enable-multi-master-using-resource-manager-template"></a>Ativar a utilização de vários mestres modelo do Resource Manager

O seguinte código JSON é um modelo do Resource Manager de exemplo que pode utilizar para implementar uma conta do Azure Cosmos DB. Para saber mais sobre o formato de modelo do Resource Manager e a sintaxe, consulte [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) documentação. O parâmetro principal a ser observada neste modelo é "enableMultipleWriteLocations": true.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String"
        },
        "locationName": {
            "type": "String"
        },
        "defaultExperience": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2015-04-08",
            "location": "[parameters('location')]",
            "tags": {
                "defaultExperience": "[parameters('defaultExperience')]"
            },
            "properties": {
                "databaseAccountOfferType": "Standard",
                "locations": [
                    {
                        "id": "[concat(parameters('name'), '-', parameters('location'))]",
                        "failoverPriority": 0,
                        "locationName": "[parameters('locationName')]"
                    }
                ],
                "isVirtualNetworkFilterEnabled": false,
                "enableMultipleWriteLocations": true,
                "virtualNetworkRules": [],
                "dependsOn": []
            }
        }
    ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a ativar o suporte de vários mestre para contas do Azure Cosmos DB. Em seguida, observe os seguintes recursos:

* [através de vários mestres com bases de dados do código-fonte aberto NoSQL](multi-master-oss-nosql.md)

* [Noções básicas sobre a resolução de conflitos no Azure Cosmos DB](multi-master-conflict-resolution.md)
