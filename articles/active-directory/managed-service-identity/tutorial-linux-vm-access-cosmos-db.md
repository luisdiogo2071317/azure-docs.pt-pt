---
title: Utilize um MSI de VM Linux para aceder à base de dados do Azure Cosmos
description: Um tutorial que explica o processo de utilizar um System-Assigned geridos serviço de identidade (MSI) numa VM com Linux, para aceder à base de dados do Azure Cosmos.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: skwan
ms.openlocfilehash: 507986e4fa83e1821b1d7a1938b356feee81e9d2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="use-a-linux-vm-msi-to-access-azure-cosmos-db"></a>Utilize um MSI de VM Linux para aceder à base de dados do Azure Cosmos 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


Este tutorial mostra como criar e utilizar um MSI de VM do Linux. Saiba como:

> [!div class="checklist"]
> * Criar uma VM com Linux com MSI ativada
> * Criar uma conta do Cosmos DB
> * Criar uma coleção na conta de base de dados do Cosmos
> * Conceder o acesso do MSI para uma instância de base de dados do Azure Cosmos
> * Obter o `principalID` do do MSI da VM com Linux
> * Obter um token de acesso e utilizá-la para chamar o Azure Resource Manager
> * Obter chaves de acesso do Azure Resource Manager para efetuar chamadas de base de dados do Cosmos

## <a name="prerequisites"></a>Pré-requisitos

Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com) antes de continuar.

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Para executar os exemplos de script CLI neste tutorial, tem duas opções:

- Utilize [Shell de nuvem do Azure](~/articles/cloud-shell/overview.md) do portal do Azure ou através de **tente-** botão, localizado no canto superior direito de cada bloco de código.
- [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 ou posterior) se preferir utilizar uma consola local do CLI.

## <a name="sign-in-to-azure"></a>Inicie sessão no  Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual Linux num novo grupo de recursos

Para este tutorial, crie um novo MSI ativada VM com Linux.

Para criar uma VM ativada de MSI:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [início de sessão az](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure na qual gostaria de implementar a VM:

   ```azurecli-interactive
   az login
   ```

2. Criar um [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para a implementação da VM e os respetivos recursos relacionados, utilizar e contenção [criar grupo az](/cli/azure/group/#az_group_create). Pode ignorar este passo se já tiver o grupo de recursos que pretende utilizar em vez disso:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Criar uma VM utilizando [az vm criar](/cli/azure/vm/#az_vm_create). O exemplo seguinte cria uma VM chamada *myVM* com um MSI, conforme solicitado pelo `--assign-identity` parâmetro. O `--admin-username` e `--admin-password` parâmetros especificam a conta de nome e palavra-passe de utilizador administrativo para início de sessão na máquina virtual. Atualize estes valores conforme adequado para o seu ambiente: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12

## Create a Cosmos DB account 

If you don't already have one, create a Cosmos DB account. You can skip this step and use an existing Cosmos DB account. 

1. Click the **+/Create new service** button found on the upper left-hand corner of the Azure portal.
2. Click **Databases**, then **Azure Cosmos DB**, and a new "New account" panel  displays.
3. Enter an **ID** for the Cosmos DB account, which you use later.  
4. **API** should be set to "SQL." The approach described in this tutorial can be used with the other available API types, but the steps in this tutorial are for the SQL API.
5. Ensure the **Subscription** and **Resource Group** match the ones you specified when you created your VM in the previous step.  Select a **Location** where Cosmos DB is available.
6. Click **Create**.

## Create a collection in the Cosmos DB account

Next, add a data collection in the Cosmos DB account that you can query in later steps.

1. Navigate to your newly created Cosmos DB account.
2. On the **Overview** tab click the **+/Add Collection** button, and an "Add Collection" panel slides out.
3. Give the collection a database ID, collection ID, select a storage capacity, enter a partition key, enter a throughput value, then click **OK**.  For this tutorial, it is sufficient to use "Test" as the database ID and collection ID, select a fixed storage capacity and lowest throughput (400 RU/s).  

## Retrieve the `principalID` of the Linux VM's MSI

To gain access to the Cosmos DB account access keys from the Resource Manager in the following section, you need to retrieve the `principalID` of the Linux VM's MSI.  Be sure to replace the `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` (resource group in which you VM resides), and `<VM NAME>` parameter values with your own values.

```azurecli-interactive
az resource show --id /subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe> --api-version 2017-12-01
```
A resposta inclui os detalhes do sistema atribuídos do MSI (nota principalID dado que é utilizada na secção seguinte):

```bash  
{
    "id": "/subscriptions/<SUBSCRIPTION ID>/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe>",
  "identity": {
    "principalId": "6891c322-314a-4e85-b129-52cf2daf47bd",
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533f8",
    "type": "SystemAssigned"
 }

```
## <a name="grant-your-linux-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Conceder o acesso do MSI da VM de Linux para as chaves de acesso da conta de base de dados do Cosmos

Cosmos DB não suportam de forma nativa a autenticação do Azure AD.  No entanto, pode utilizar um MSI para obter uma chave de acesso do Cosmos BD do Gestor de recursos, em seguida, utilizar a chave para aceder à base de dados do Cosmos.  Neste passo, pode conceder que o sistema atribuído acesso do MSI para as chaves para a conta de base de dados do Cosmos.

Para conceder o acesso de identidade do MSI para a conta de base de dados do Cosmos no Azure Resource Manager utilizando a CLI do Azure, atualize os valores para `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, e `<COSMOS DB ACCOUNT NAME>` para o seu ambiente. Substitua `<MSI PRINCIPALID>` com o `principalId` propriedade devolvida pelo `az resource show` no [obter principalID do MSI da VM com Linux](#retrieve-the-principalID-of-the-linux-VM's-MSI).  BD do cosmos suporta dois níveis de granularidade quando utilizar chaves de acesso: leitura/escrita acesso à conta e o acesso só de leitura à conta.  Atribuir o `DocumentDB Account Contributor` função se pretender obter chaves de leitura/escrita para a conta ou atribuir o `Cosmos DB Account Reader Role` função se pretender obter chaves de só de leitura para a conta:

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role '<ROLE NAME>' --scope "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMODS DB ACCOUNT NAME>"
```

A resposta inclui os detalhes para a atribuição de função criada:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>/providers/Microsoft.Authorization/roleAssignments/5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "name": "5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "properties": {
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-linux-vms-msi-and-use-it-to-call-azure-resource-manager"></a>Obter um token de acesso utilizando o MSI da VM com Linux e utilizá-la para chamar o Azure Resource Manager

Para o resto do tutorial, trabalhar a partir da VM que criou anteriormente.

Para concluir estes passos, precisa de um cliente SSH. Se estiver a utilizar o Windows, pode utilizar o cliente SSH o [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/install_guide). Se precisar de assistência para configurar as chaves do seu cliente SSH, consulte [como chaves de utilizar o SSH com o Windows no Azure](../../virtual-machines/linux/ssh-from-windows.md), ou [como criar e utilizar um par de chaves público e privado SSH para VMs com Linux no Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. No portal do Azure, navegue para **máquinas virtuais**, vá para a máquina virtual Linux, em seguida, a partir de **descrição geral** página clique **Connect** na parte superior. Copie a cadeia para ligar à VM. 
2. Ligar à VM utilizando o cliente SSH.  
3. Em seguida, é-lhe pedido que introduza no seu **palavra-passe** adicionado ao criar o **VM com Linux**. Deve, em seguida, ser sessão com êxito.  
4. Utilize o CURL para obter acesso token para o Azure Resource Manager: 
     
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true   
    ```
 
    > [!NOTE]
    > O pedido anterior, o valor do parâmetro "recursos" tem de ser uma correspondência exata para que é esperado pelo Azure AD. Ao utilizar o ID de recurso do Azure Resource Manager, tem de incluir a barra no final no URI.
    > Na resposta seguinte, o elemento de access_token conforme foi shortened de uma forma abreviada.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```
    
## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Obter chaves de acesso do Azure Resource Manager para efetuar chamadas de base de dados do Cosmos  

Agora utilize CURL para chamar o Resource Manager utilizando o token de acesso que obteve na secção anterior para obter a chave de acesso da conta de base de dados do Cosmos. Assim que tivermos a chave de acesso, iremos pode consultar a base de dados do Cosmos. Não se esqueça de substituir o `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, e `<COSMOS DB ACCOUNT NAME>` valores de parâmetros com os seus próprios valores. Substitua o `<ACCESS TOKEN>` valor com o token de acesso que obteve anteriormente.  Se pretender obter chaves de leitura/escrita, utilize o tipo de chave operação `listKeys`.  Se pretender obter chaves de só de leitura, utilize o tipo de operação de chave `readonlykeys`:

```bash 
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT NAME>/<KEY OPERATION TYPE>?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> O texto no URL anterior é maiúsculas e minúsculas, por isso, certifique-se se estiver a utilizar superior-minúsculas para os grupos de recursos para refleti-lo em conformidade. Além disso, é importante saber o que se trata de um pedido POST não um pedido GET e certifique-se de que transmita um valor para capturar um limite de comprimento com -d que pode ser nulo.  

A resposta CURL dá-lhe a lista de chaves.  Por exemplo, se depara-se as chaves só de leitura:  

```bash 
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```

Agora que tem a chave de acesso para a conta de base de dados do Cosmos pode transmiti-lo para um SDK de BD do Cosmos e efetuar chamadas para a conta de acesso.  Para obter um exemplo rápido, pode passar a chave de acesso para a CLI do Azure.  Pode obter o <COSMOS DB CONNECTION URL> do **descrição geral** separador no painel de conta de base de dados do Cosmos no portal do Azure.  Substitua o <ACCESS KEY> com o valor obtido acima:

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

Este comando da CLI devolve detalhes sobre a coleção:

```bash
{
  "collection": {
    "_conflicts": "conflicts/",
    "_docs": "docs/",
    "_etag": "\"00006700-0000-0000-0000-5a8271e90000\"",
    "_rid": "Es5SAM2FDwA=",
    "_self": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/",
    "_sprocs": "sprocs/",
    "_triggers": "triggers/",
    "_ts": 1518498281,
    "_udfs": "udfs/",
    "id": "Test",
    "indexingPolicy": {
      "automatic": true,
      "excludedPaths": [],
      "includedPaths": [
        {
          "indexes": [
            {
              "dataType": "Number",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "String",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "Point",
              "kind": "Spatial"
            }
          ],
          "path": "/*"
        }
      ],
      "indexingMode": "consistent"
    }
  },
  "offer": {
    "_etag": "\"00006800-0000-0000-0000-5a8271ea0000\"",
    "_rid": "f4V+",
    "_self": "offers/f4V+/",
    "_ts": 1518498282,
    "content": {
      "offerIsRUPerMinuteThroughputEnabled": false,
      "offerThroughput": 400
    },
    "id": "f4V+",
    "offerResourceId": "Es5SAM2FDwA=",
    "offerType": "Invalid",
    "offerVersion": "V2",
    "resource": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/"
  }
}
```

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral do MSI, consulte [geridos serviço de identidade (MSI) para recursos do Azure](overview.md).

