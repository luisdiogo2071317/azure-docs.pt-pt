---
title: Identidade de serviço do Azure Data Factory | Documentos da Microsoft
description: Saiba mais sobre a identidade de serviço de fábrica de dados no Azure Data Factory.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: jingwang
ms.openlocfilehash: 7937836daad5ad299f3e5b7b6b7994ae40a833fd
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56446891"
---
# <a name="azure-data-factory-service-identity"></a>Identidade de serviço do Azure Data Factory

Este artigo ajuda-o a compreender o que é a identidade de serviço do data factory e como ela funciona.

## <a name="overview"></a>Descrição geral

Ao criar uma fábrica de dados, uma identidade de serviço pode ser criada juntamente com a criação de fábrica. A identidade de serviço é um aplicativo gerenciado registrado para o diretório de atividade do Azure e representa nesta fábrica de dados específico.

Identidade do serviço de fábrica de dados beneficia as seguintes funcionalidades:

- [Store credencial no Azure Key Vault](store-credentials-in-key-vault.md), caso em que a identidade do serviço de fábrica de dados é utilizada para autenticação do Azure Key Vault.
- Conectores, incluindo [armazenamento de Blobs do Azure](connector-azure-blob-storage.md), [Gen1 de armazenamento do Azure Data Lake](connector-azure-data-lake-store.md), [Gen2 de armazenamento do Azure Data Lake](connector-azure-data-lake-storage.md), [Azure SQL Database](connector-azure-sql-database.md), e [armazém de dados SQL do Azure](connector-azure-sql-data-warehouse.md).
- [Atividade da Web](control-flow-web-activity.md).

## <a name="generate-service-identity"></a>Gerar uma identidade de serviço

Identidade de serviço do Data factory é gerada da seguinte forma:

- Ao criar a fábrica de dados por meio **portal do Azure ou o PowerShell**, identidade do serviço será sempre criada automaticamente.
- Ao criar a fábrica de dados por meio **SDK**, identidade de serviço será criada apenas se especificar "identidade = FactoryIdentity() nova" no objeto de fábrica para a criação. Veja o exemplo na [guia de introdução do .NET - criar fábrica de dados](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Ao criar a fábrica de dados por meio **REST API**, identidade de serviço será criada apenas se especificou a secção "identity" no corpo do pedido. Veja o exemplo na [REST de início rápido - criar a fábrica de dados](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Se achar que a fábrica de dados não tem uma identidade de serviço associada seguindo [obter a identidade do serviço](#retrieve-service-identity) instrução, pode explicitamente gerar uma através da atualização da fábrica de dados com o iniciador de identidade por meio de programação:

- [Gerar uma identidade de serviço com o PowerShell](#generate-service-identity-using-powershell)
- [Gerar uma identidade de serviço com a REST API](#generate-service-identity-using-rest-api)
- Gerar uma identidade de serviço com um modelo Azure Resource Manager
- [Gerar uma identidade de serviço com o SDK](#generate-service-identity-using-sdk)

>[!NOTE]
>- Não é possível alterar a identidade do serviço. A atualizar uma fábrica de dados que já têm uma identidade de serviço não terá qualquer impacto, a identidade de serviço é mantida inalterada.
>- Se atualizar uma fábrica de dados que já tem uma identidade de serviço sem especificar o parâmetro de "identity" no objeto de fábrica ou sem especificar a secção "identity" no corpo do pedido REST, obterá um erro.
>- Quando elimina uma fábrica de dados, será eliminada a identidade de serviço associado ao longo.

### <a name="generate-service-identity-using-powershell"></a>Gerar uma identidade de serviço com o PowerShell

Chamar **Set-AzureRmDataFactoryV2** novamente, o comando, em seguida, verá "Identity" campos gerados recentemente:

```powershell
PS C:\WINDOWS\system32> Set-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="generate-service-identity-using-rest-api"></a>Gerar uma identidade de serviço com a REST API

Chamar abaixo API com a secção "identity" no corpo do pedido:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2018-06-01
```

**Corpo do pedido**: adicionar "identity": {"type": "SystemAssigned"}.

```json
{
    "name": "<dataFactoryName>",
    "location": "<region>",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
```

**Resposta**: identidade de serviço é criada automaticamente e a secção "identity" é preenchida em conformidade.

```json
{
    "name": "<dataFactoryName>",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2018-06-01"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "765ad4ab-XXXX-XXXX-XXXX-51ed985819dc",
        "tenantId": "72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory",
    "type": "Microsoft.DataFactory/factories",
    "location": "<region>"
}
```

### <a name="generate-service-identity-using-an-azure-resource-manager-template"></a>Gerar uma identidade de serviço com um modelo Azure Resource Manager

**Modelo**: adicionar "identity": {"type": "SystemAssigned"}.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "resources": [{
        "name": "<dataFactoryName>",
        "apiVersion": "2018-06-01",
        "type": "Microsoft.DataFactory/factories",
        "location": "<region>",
        "identity": {
            "type": "SystemAssigned"
        }
    }]
}
```

### <a name="generate-service-identity-using-sdk"></a>Gerar uma identidade de serviço com o SDK

Chamar a função de create_or_update de fábrica de dados com a identidade = FactoryIdentity() novo. Código de exemplo através do .NET:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-service-identity"></a>Obter a identidade de serviço

Pode obter a identidade de serviço a partir do portal do Azure ou através de programação. As secções seguintes mostram alguns exemplos.

>[!TIP]
> Se não vir a identidade de serviço, [gerar uma identidade de serviço](#generate-service-identity) através da atualização da sua fábrica.

### <a name="retrieve-service-identity-using-azure-portal"></a>Obter a identidade do serviço através do portal do Azure

Pode encontrar as informações de identidade de serviço do portal do Azure -> sua fábrica de dados -> Definições -> propriedades:

- ID DE IDENTIDADE DE SERVIÇO
- INQUILINO DE IDENTIDADE DE SERVIÇO
- **ID de aplicação do serviço de identidade** > Copie este valor

![Obter a identidade de serviço](media/data-factory-service-identity/retrieve-service-identity-portal.png)

### <a name="retrieve-service-identity-using-powershell"></a>Obter a identidade de serviço com o PowerShell

A identidade de serviço, ID de principal e o ID de inquilino será retornada quando obtém uma fábrica de dados específicos da seguinte forma:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Copie o ID de principal, em seguida, executar abaixo comandos do Azure Active Directory com o principal ID como parâmetro para obter o **ApplicationId**, que utiliza para conceder acesso:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

## <a name="next-steps"></a>Passos Seguintes
Consulte os tópicos seguintes que introduzir quando e como utilizar a identidade de serviço de fábrica de dados:

- [Credencial de Store no Azure Key Vault](store-credentials-in-key-vault.md)
- [Copiar dados de/para o Azure Data Lake Store com identidades geridas de autenticação de recursos do Azure](connector-azure-data-lake-store.md)

Ver [identidades geridas por para descrição geral de recursos do Azure](/azure/active-directory/managed-identities-azure-resources/overview) para obter mais informações sobre identidades geridas para recursos do Azure, que identidade de serviço de fábrica de dados se baseia. 
