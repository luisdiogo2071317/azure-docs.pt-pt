---
title: Saiba como gerir contas de base de dados no Azure Cosmos DB
description: Saiba como gerir contas de base de dados no Azure Cosmos DB
services: cosmos-db
author: christopheranderson
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/17/2018
ms.author: chrande
ms.openlocfilehash: 67cd78d4900b8ce53cf0c50116c02a9c1b967687
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958768"
---
# <a name="manage-database-accounts-in-azure-cosmos-db"></a>Gerir contas de base de dados no Azure Cosmos DB

Este artigo descreve como gerir a sua conta do Cosmos DB para configurar o multi-homing, adicionar/remover uma região, configurar várias regiões de escrita e configurar prioridades de ativação pós-falha. 

## <a name="create-a-database-account"></a>Criar uma conta de base de dados

### <a id="create-database-account-via-portal"></a>Portal do Azure

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a id="create-database-account-via-cli"></a>CLI do Azure

```bash
# Create an account
az cosmosdb create --name <Cosmos DB Account name> --resource-group <Resource Group Name>
```

## <a name="configure-clients-for-multi-homing"></a>Configurar clientes para multi-homing

### <a id="configure-clients-multi-homing-dotnet"></a>SDK do .NET

```csharp
// Create a new Connection Policy
ConnectionPolicy policy = new ConnectionPolicy
    {
        // Note: These aren't required settings for multi-homing,
        // just suggested defaults
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true,
    };
// Add regions to Preferred locations
// The name of the location will match what you see in the portal/etc.
policy.PreferredLocations.Add("East US");
policy.PreferredLocations.Add("North Europe");

// Pass the Connection policy with the preferred locations on it to the client.
DocumentClient client = new DocumentClient(new Uri(this.accountEndpoint), this.accountKey, policy);
```

### <a id="configure-clients-multi-homing-java-async"></a>SDK do Java Async

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setPreferredLocations(Collections.singleton("West US"));
AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConnectionPolicy(policy).build();
```

### <a id="configure-clients-multi-homing-java-sync"></a>SDK do Java Sync

```java
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
Collection<String> preferredLocations = new ArrayList<String>();
preferredLocations.add("Australia East");
connectionPolicy.setPreferredLocations(preferredLocations);
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy);
```

### <a id="configure-clients-multi-homing-javascript"></a>SDK do Node.js/JavaScript/TypeScript

```javascript
// Set up the connection policy with your preferred regions
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations = ["West US", "Australia East"];

// Pass that connection policy to the client
const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy
});
```

### <a id="configure-clients-multi-homing-python"></a>SDK do Python

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.PreferredLocations = ['West US', 'Japan West']
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy)

```

## <a name="addremove-regions-from-your-database-account"></a>Adicionar/remover regiões da conta de base de dados

### <a id="add-remove-regions-via-portal"></a>Portal do Azure

1. Navegue para a sua conta do Azure Cosmos DB e abra o menu **Replicar dados globalmente**.

2. Para adicionar regiões, selecione uma ou mais regiões no mapa ao clicar nos hexágonos vazios com a etiqueta **"+"** correspondente à região pretendida. Também pode adicionar uma região ao selecionar a opção **+ Adicionar região** e uma região no menu pendente.

3. Para remover regiões, desmarque uma ou mais regiões no mapa ao clicar nos hexágonos azuis com uma marca de verificação ou selecione o ícone de "caixote do lixo" (🗑) junto à região no lado direito.

4. Clique em Guardar para guardar as alterações.

   ![Menu Adicionar/remover regiões](./media/how-to-manage-database-account/add-region.png)

No modo de escrita de região única, não é possível remover a região de escrita. Tem de efetuar a ativação pós-falha para uma região diferente antes de eliminar essa região de escrita atual.

No modo de escrita de várias regiões, pode adicionar/remover qualquer região desde que tenha, pelo menos, uma região.

### <a id="add-remove-regions-via-cli"></a>CLI do Azure

```bash
# Given an account created with 1 region like so
az cosmosdb create --name <Cosmos DB Account name> --resource-group <Resource Group name> --locations 'eastus=0'

# Add a new region by adding another region to the list
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --locations 'eastus=0 westus=1'

# Remove a region by removing a region from the list
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --locations 'westus=0'
```

## <a name="configure-multiple-write-regions"></a>Configurar várias regiões de escrita

### <a id="configure-multiple-write-regions-portal"></a>Portal do Azure

Quando criar uma conta de base de dados, certifique-se de que a definição **Escritas de Várias Regiões** está ativada.

![Captura de ecrã da criação da Conta do Cosmos DB](./media/how-to-manage-database-account/account-create.png)

### <a id="configure-multiple-write-regions-cli"></a>CLI do Azure

```bash
az cosmosdb create --name <Cosmos DB Account name> --resource-group <Resource Group name> --enable-multiple-write-locations true
```

### <a id="configure-multiple-write-regions-arm"></a>Modelo do Resource Manager

O código JSON seguinte é um modelo de exemplo do Resource Manager. Pode utilizá-lo para implementar uma conta do Azure Cosmos DB com uma política de consistência como Estagnação Limitada, um intervalo de estagnação máximo de 5 segundos e o número máximo de pedidos obsoletos tolerado em 100. Para saber mais sobre o formato de modelo do Resource Manager e a sintaxe, veja a documentação do [Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "BoundedStaleness",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
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


## <a name="enable-manual-failover-for-your-cosmos-account"></a>Ativar a ativação pós-falha manual para a conta do Cosmos

### <a id="enable-manual-failover-via-portal"></a>Portal do Azure

1. Navegue para a sua conta do Azure Cosmos DB e abra o menu **"Replicar dados globalmente"**.

2. Clique no botão **"Ativação Pós-falha Manual"** na parte superior do menu.

   ![Menu Replicar dados globalmente](./media/how-to-manage-database-account/replicate-data-globally.png)

3. No menu **"Ativação Pós-falha Manual"**, selecione a nova região de escrita e a caixa para marcar que compreende que esta opção vai alterar a sua região de escrita.

4. Clique em "OK" para acionar a ativação pós-falha.

   ![Menu do portal de ativação pós-falha manual](./media/how-to-manage-database-account/manual-failover.png)

### <a id="enable-manual-failover-via-cli"></a>CLI do Azure

```bash
# Given your account currently has regions with priority like so: 'eastus=0 westus=1'
# Change the priority order to trigger a failover of the write region
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --locations 'eastus=1 westus=0'
```

## <a name="enable-automatic-failover-for-your-cosmos-account"></a>Ativar a ativação pós-falha automática para a conta do Cosmos

### <a id="enable-automatic-failover-via-portal"></a>Portal do Azure

1. Na sua conta do Azure Cosmos DB, abra o painel **"Replicar dados globalmente"**. 

2. Clique no botão **"Ativação Pós-falha Automática"** na parte superior do painel.

   ![Menu Replicar dados globalmente](./media/how-to-manage-database-account/replicate-data-globally.png)

3. No painel **"Ativação Pós-falha Automática"**, certifique-se de que a opção **Ativar Ativação Pós-falha Automática** está definida como **ATIVADO**. 

4. Clique em Guardar na parte inferior do menu.

   ![Menu do portal de ativação pós-falha automática](./media/how-to-manage-database-account/automatic-failover.png)

Também pode definir as prioridades de ativação pós-falha neste menu.

### <a id="enable-automatic-failover-via-cli"></a>CLI do Azure

```bash
# Enable automatic failover on account creation
az cosmosdb create --name <Cosmos DB Account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Enable automatic failover on an existing account
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --enable-automatic-failover true

# Disable automatic failover on an existing account
az cosmosdb update --name <Cosmos DB Account name> --resource-group <Resource Group name> --enable-automatic-failover false
```

## <a name="set-failover-priorities-for-your-cosmos-account"></a>Definir prioridades de ativação pós-falha para a conta do Cosmos

### <a id="set-failover-priorities-via-portal"></a>Portal do Azure

1. Na sua conta do Azure Cosmos DB, abra o painel **"Replicar dados globalmente"**. 

2. Clique no botão **"Ativação Pós-falha Automática"** na parte superior do painel.

   ![Menu Replicar dados globalmente](./media/how-to-manage-database-account/replicate-data-globally.png)

3. No painel **"Ativação Pós-falha Automática"**, certifique-se de que a opção **Ativar Ativação Pós-falha Automática** está definida como **ATIVADO**. 

4. É possível modificar a prioridade de ativação pós-falha ao clicar e arrastar as regiões de leitura através dos três pontos no lado esquerdo da linha apresentada quando paira o cursor sobre os mesmos. 

5. Clique em Guardar na parte inferior do menu.

   ![Menu do portal de ativação pós-falha automática](./media/how-to-manage-database-account/automatic-failover.png)

Não é possível modificar a região de escrita neste menu. Para alterar a região de escrita manualmente, tem de fazer uma ativação pós-falha manual.

### <a id="set-failover-priorities-via-cli"></a>CLI do Azure

```bash
az cosmosdb failover-priority-change --name <Cosmos DB Account name> --resource-group <Resource Group name> --failover-policies 'eastus=0 westus=2 southcentralus=1'
```

## <a name="next-steps"></a>Passos seguintes

Pode saber mais sobre a gestão de conflitos de dados e níveis de consistência no Cosmos DB com os documentos seguintes:

* [Como gerir a consistência](how-to-manage-consistency.md)
* [Como gerir conflitos entre regiões](how-to-manage-conflicts.md)

