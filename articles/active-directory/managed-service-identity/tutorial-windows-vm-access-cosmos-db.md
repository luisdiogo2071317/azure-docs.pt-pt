---
title: Utilizar uma MSI de VM do Windows para aceder ao Azure Cosmos DB
description: Um tutorial que explica o processo de utilização de uma Identidade de Serviço Gerida (MSI) Atribuída ao Sistema numa VM do Windows, para aceder ao Azure Cosmos DB.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: daveba
ms.openlocfilehash: ed1aff411ae5446688fe717ddc4570ea756c4c1e
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37904276"
---
# <a name="tutorial-use-a-windows-vm-msi-to-access-azure-cosmos-db"></a>Tutorial: utilizar uma MSI de VM do Windows para aceder ao Azure Cosmos DB

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra-lhe como criar e utilizar uma MSI de VM do Windows para aceder ao Cosmos DB. Saiba como:

> [!div class="checklist"]
> * Criar uma VM do Windows com a MSI ativada 
> * Criar uma conta do Cosmos DB
> * Conceder acesso à MSI da VM do Windows às chaves de acesso da conta do Cosmos DB
> * Obter um token de acesso com a MSI da VM do Windows para chamar o Azure Resource Manager
> * Obter chaves de acesso do Azure Resource Manager para fazer chamadas do Cosmos DB

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]


## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows num novo grupo de recursos

Neste tutorial, vamos criar uma nova VM do Windows.  Também pode ativar o MSI numa VM existente.

1. Clique no botão **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3. Introduza as informações da máquina virtual. O **Nome de Utilizador** e a **Palavra-passe** aqui criados são as credenciais a utilizar para iniciar sessão na máquina virtual.
4. Escolha uma **Subscrição** para a máquina virtual na lista pendente.
5. Para selecionar um novo **Grupo de Recursos** no qual irá criar a sua máquina virtual, selecione **Criar Novo**. Quando terminar, clique em **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. Na página Definições, mantenha as predefinições e clique em **OK**.

   ![Texto alternativo da imagem](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Ativar a MSI na sua VM 

Uma MSI de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD, sem ter de colocar as credenciais no código. Nos bastidores, ativar a MSI numa Máquina Virtual através do portal do Azure faz duas coisas: regista a sua VM no Azure AD para criar uma identidade gerida e configura a identidade na VM.

1. Selecione a **Máquina Virtual** na qual pretende ativar a MSI.  
2. Na barra de navegação esquerda, clique em **Configuração**. 
3. Vai ver a **Identidade de Serviço Gerida**. Para registar e ativar a MSI, selecione **Sim**; se desejar desativá-la, selecione Não. 
4. Certifique-se de que clica em **Guardar** para guardar a configuração.  
   ![Texto alternativo da imagem](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="create-a-cosmos-db-account"></a>Criar uma conta do Cosmos DB 

Se ainda não tiver uma, crie uma conta do Cosmos DB. Pode ignorar este passo e utilizar uma conta do Cosmos DB existente. 

1. Clique no botão **+/Criar novo serviço**, no canto superior esquerdo do portal do Azure.
2. Clique em **Bases de Dados** e, em seguida, **Azure Cosmos DB**, e um painel “Nova conta” é apresentado.
3. Introduza um **ID** para a conta do Cosmos DB, que irá utilizar mais tarde.  
4. A **API** deve ser definida como "SQL". A abordagem descrita neste tutorial pode ser utilizada com os outros tipos de API disponíveis, mas os passos neste tutorial são para a API de SQL.
5. Certifique-se de que a **Subscrição** e o **Grupo de Recursos** correspondem aos perfis que especificou quando criou a VM no passo anterior.  Selecione uma **Localização** em que o Cosmos DB esteja disponível.
6. Clique em **Criar**.

## <a name="create-a-collection-in-the-cosmos-db-account"></a>Criar uma coleção na conta do Cosmos DB

Em seguida, adicione uma coleção de dados à conta do Cosmos DB, que possa consultar em passos posteriores.

1. Navegue até à sua conta do Cosmos DB recentemente criada.
2. No separador **Descrição Geral**, clique no botão **+/Adicionar Coleção**, e um painel “Adicionar Coleção” surge.
3. Dê à a coleção um ID de base de dados, um ID de coleção, selecione uma capacidade de armazenamento, introduza uma chave de partição, introduza um valor de débito e clique em **OK**.  Neste tutorial, é suficiente utilizar a opção "Teste" como o ID de base de dados e o ID de coleção, selecionar uma capacidade de armazenamento fixa e o débito mais baixo (400 RU/s).  

## <a name="grant-windows-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Conceder acesso à MSI da VM do Windows às chaves de acesso da conta do Cosmos DB

O Cosmos DB não suporta nativamente a autenticação do Azure AD. No entanto, pode utilizar uma MSI para obter uma chave de acesso do Cosmos DB do Resource Manager e utilizar a chave para aceder ao Cosmos DB. Neste passo, pode conceder acesso MSI às chaves da conta do Cosmos DB.

Para conceder o acesso de identidade da MSI à conta do Cosmos DB no Azure Resource Manager com o PowerShell, atualize os valores de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` e `<COSMOS DB ACCOUNT NAME>` para o seu ambiente. Substitua `<MSI PRINCIPALID>` pela propriedade `principalId` devolvida pelo comando `az resource show` em [Obter o principalID da MSI da VM do Linux](#retrieve-the-principalID-of-the-linux-VM's-MSI).  O Cosmos DB suporta dois níveis de granularidade ao utilizar chaves de acesso: acesso de leitura/escrita à conta e acesso só de leitura à conta.  Atribua a função `DocumentDB Account Contributor` se pretender obter as chaves de leitura/escrita para a conta, ou atribua a função `Cosmos DB Account Reader Role` se quiser obter só as chaves de leitura para a conta:

```azurepowershell
$spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
```

## <a name="get-an-access-token-using-the-windows-vms-msi-to-call-azure-resource-manager"></a>Obter um token de acesso com a MSI da VM do Windows para chamar o Azure Resource Manager

No resto do tutorial, iremos trabalhar a partir da VM que criámos anteriormente. 

Terá de utilizar os cmdlets PowerShell do Azure Resource Manager nesta parte.  Se não o tiver instalado, [transfira a versão mais recente](https://docs.microsoft.com/powershell/azure/overview) antes de continuar.

Também terá de instalar a versão mais recente da [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) na sua VM do Windows.

1. No portal do Azure, navegue para **Máquinas Virtuais**, aceda à sua máquina virtual do Windows e, em seguida, na página **Descrição Geral**, clique em **Ligar** na parte superior. 
2. Introduza o seu **Nome de Utilizador** e a **Palavra-passe** que adicionou quando criou a VM do Windows. 
3. Agora que já criou uma **Ligação ao Ambiente de Trabalho Remoto** com a máquina virtual, abra o PowerShell na sessão remota.
4. Através de Invoke-WebRequest do PowerShell, faça um pedido ao ponto final da MSI local para obter um token de acesso para o Azure Resource Manager.

    ```powershell
        $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```

    > [!NOTE]
    > No pedido anterior, o valor do parâmetro "recurso" tem de ser uma correspondência exata para o que é esperado pelo Azure AD. Ao utilizar o ID de recurso do Azure Resource Manager, tem de incluir a barra à direita no URI.
    
    Em seguida, extraia o elemento "Content", que é armazenado como uma cadeia formatada do JavaScript Object Notation (JSON) no objeto $response. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Em seguida, extraia o token de acesso da resposta.
    
    ```powershell
    $ArmToken = $content.access_token
    ```

## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Obter chaves de acesso do Azure Resource Manager para fazer chamadas do Cosmos DB

Agora, utilize o PowerShell para chamar o Resource Manager com o token de acesso que obteve na secção anterior para obter a chave de acesso à conta do Cosmos DB. Assim que tivermos a chave de acesso, podemos fazer consultas no Cosmos DB. Certifique-se de que substitui os valores de parâmetros `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` e `<COSMOS DB ACCOUNT NAME>` pelos seus próprios valores. Substitua o valor `<ACCESS TOKEN>` pelo o token de acesso que obteve anteriormente.  Se pretender obter chaves de leitura/escrita, utilize o tipo de operação de chave `listKeys`.  Se pretender obter chaves só de leitura, utilize o tipo de operação de chave `readonlykeys`:

```powershell
Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.DocumentDb/databaseAccounts/<COSMOS DB ACCOUNT NAME>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
A resposta dá-lhe a lista de Chaves.  Por exemplo, se obtiver chaves só de leitura:

```powershell
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```
Agora que tem a chave de acesso para a conta do Cosmos DB, pode passá-la a um SDK do Cosmos DB e fazer chamadas para aceder à conta.  Para obter um rápido exemplo, pode passar a chave de acesso à CLI do Azure.  Pode obter o <COSMOS DB CONNECTION URL> no separador **Descrição Geral** no painel da conta do Cosmos DB no portal do Azure.  Substitua o <ACCESS KEY> pelo valor que obteve acima:

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

Este comando da CLI devolve os detalhes da coleção:

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

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar uma Identidade de Serviço Gerida do Windows para aceder ao Cosmos DB.  Para saber mais sobre o Cosmos DB, veja:

> [!div class="nextstepaction"]
>[Descrição geral do Azure Cosmos DB](/azure/cosmos-db/introduction)


