---
title: Gerir o ciclo de vida de armazenamento do Azure
description: Saiba como criar regras de política de ciclo de vida aos dados de classificação por vencimento de transição de acesso frequente para esporádico e de arquivo.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: article
ms.date: 11/01/2018
ms.author: yzheng
ms.component: common
ms.openlocfilehash: c6647ff97b078ca5afa5c66833a1617f6b3ec0f1
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978813"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Gerir o ciclo de vida de armazenamento de Blobs do Azure (pré-visualização)

Os conjuntos de dados têm ciclos de vida exclusivos. Alguns dados são acedidos com frequência no início do ciclo de vida, mas a necessidade de acesso cai drasticamente como a sua idade aumenta. Alguns dados permanecem Inativos na cloud e são raramente acedidos armazenados uma vez. Alguns dados expirarem dias ou meses após a criação, enquanto outros conjuntos de dados são lidos e modificados durante seus tempos de vida ativamente. Gestão de ciclo de vida de armazenamento de Blobs do Azure (pré-visualização) oferece uma política de avançado e baseado em regras que pode ser usado em contas de armazenamento de BLOBs e GPv2 para fazer a transição de seus dados para os escalões de acesso apropriado ou expirar no final do seu ciclo de vida.

Política de gestão do ciclo de vida ajuda-o:

- Faça a transição de blobs para uma camada de armazenamento mais esporádica (frequente para acesso esporádico, de acesso frequente para arquivo, ou acesso esporádico para arquivo) para otimizar o desempenho e custo
- Eliminar blobs no final dos respetivos ciclos de vida
- Definir regras para ser executado uma vez por dia ao nível da conta de armazenamento
- Aplicar regras a contentores ou um subconjunto de blobs (utilizando o prefixos de que filtros)

Considere um conjunto de dados que são acedidos com frequência durante o estágio inicial do ciclo de vida, é necessário apenas ocasionalmente após duas semanas e são raramente acedidos após um mês e muito mais. Neste cenário, armazenamento de acesso frequente é melhor nos estágios iniciais, armazenamento de acesso esporádico é mais adequado para acesso ocasional e armazenamento de arquivo é a melhor opção de escalão após a sua idade aumenta mais de um mês. Ao ajustar camadas de armazenamento sentido para a idade dos dados, é possível projetar as opções de armazenamento menos dispendiosas para as suas necessidades. Para obter esta transição, regras de política de gestão do ciclo de vida estão disponíveis para mover dados de classificação por vencimento para escalões torna mais esporádica.

## <a name="storage-account-support"></a>Suporte de conta de armazenamento

Política de gestão do ciclo de vida está disponível com ambos os fins gerais v2 (GPv2) conta e a conta de armazenamento de Blobs. Pode atualizar uma conta de fins gerais (GPv1) existente para uma conta GPv2 através de um processo de um clique simples no portal do Azure, sem períodos de indisponibilidade. Para obter mais informações sobre contas de armazenamento, consulte [descrição geral da conta de armazenamento do Azure](../common/storage-account-overview.md) para saber mais.  

## <a name="pricing"></a>Preços 

A funcionalidade de gestão do ciclo de vida de é gratuita em pré-visualização. Os clientes são cobrados os custos de operação normais para o [listar os Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) e [Set Blob Tier](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) chamadas de API. Ver [preços do Blob de blocos](https://azure.microsoft.com/pricing/details/storage/blobs/) para saber mais sobre os preços.

## <a name="register-for-preview"></a>Registar-se na pré-visualização 
Para se inscrever na pré-visualização pública, terá de submeter um pedido para registar esta funcionalidade para a sua subscrição. Depois do seu pedido é aprovado (dentro de alguns dias), qualquer conta GPv2 ou de armazenamento de BLOBs nova e existente no E.U.A. oeste 2, e.u.a. centro-oeste, E.U.A. Leste 2 e Europa Ocidental terão a funcionalidade ativada. Durante a pré-visualização, é suportada apenas BLOBs de blocos. Tal como acontece com a maior parte das visualizações, esta funcionalidade não deve ser utilizada para cargas de trabalho de produção até atingir GA.

Para submeter um pedido, execute os seguintes comandos do PowerShell ou a CLI.

### <a name="powershell"></a>PowerShell

Para submeter um pedido:

```powershell
Register-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage 
```
Pode verificar o estado de aprovação de registo com o seguinte comando:
```powershell
Get-AzureRmProviderFeature -FeatureName DLM -ProviderNamespace Microsoft.Storage
```
Se a funcionalidade de aprovação e registada corretamente, deverá receber o estado "Registado".

### <a name="azure-cli"></a>CLI do Azure

Para submeter um pedido: 
```cli
az feature register --namespace Microsoft.Storage --name DLM
```
Pode verificar o estado de aprovação de registo com o seguinte comando:
```cli
az feature show --namespace Microsoft.Storage --name DLM
```
Se a funcionalidade de aprovação e registada corretamente, deverá receber o estado "Registado". 


## <a name="add-or-remove-a-policy"></a>Adicionar ou remover uma política 

Pode adicionar, editar ou remover uma política através do portal do Azure, [PowerShell](https://www.powershellgallery.com/packages/AzureRM.Storage/5.0.3-preview), [REST APIs](https://docs.microsoft.com/rest/api/storagerp/managementpolicies/createorupdate), ou ferramentas de cliente nos seguintes idiomas: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node. js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby](  https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Para navegar para a sua conta de armazenamento, selecione Todos os Recursos, em seguida, selecione a sua conta de armazenamento.

3. No painel Definições, clique em **gestão de ciclo de vida** agrupados sob o serviço de BLOBs para ver e/ou alterar a sua política.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

> [!NOTE]
Se ativar as regras de firewall para a sua conta de armazenamento, pedidos de gestão do ciclo de vida podem ser bloqueados. Pode desbloqueá-lo, fornecendo exceções. Para obter mais informações, consulte a seção de exceções no [configurar firewalls e redes virtuais](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policy"></a>Política

Uma política de gestão do ciclo de vida é uma coleção de regras num documento JSON:

```json
{
  "version": "0.5",
  "rules": [
    {
      "name": "rule1",
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```


Dentro de uma política, os dois parâmetros são necessários:

| Nome do parâmetro | Tipo de parâmetro | Notas |
|----------------|----------------|-------|
| versão        | Expressado como uma cadeia de caracteres `x.x` | O número de versão de pré-visualização é 0,5 |
| regras          | Uma matriz de objetos de regra | Pelo menos uma regra é necessária em cada política. Durante a pré-visualização, pode especificar até 4 regras de acordo com a política. |

Dentro de cada regra, três parâmetros são necessários:

| Nome do parâmetro | Tipo de parâmetro | Notas |
|----------------|----------------|-------|
| Nome           | Cadeia | Um nome de regra pode conter qualquer combinação de carateres de alfanuméricos. Nome da regra diferencia maiúsculas de minúsculas. Tem de ser exclusivo dentro de uma política. |
| tipo           | Um valor de enumeração | É o valor válido para a pré-visualização `Lifecycle` |
| definição     | Um objeto que define a regra de ciclo de vida | Cada definição é formada por com um conjunto de filtros e um conjunto de ações. |

## <a name="rules"></a>Regras

Cada definição de regra inclui um conjunto de filtros e um conjunto de ações. O [filtrar o conjunto](#rule-filters) é utilizado para limitar as ações de regras para um determinado conjunto de objetos dentro de um contêiner ou nomes de objetos. O [conjunto de ação](#rule-actions) aplica-se a camada ou eliminar ações para o conjunto filtrado de objetos.

### <a name="sample-rule"></a>Exemplo de regra
A seguinte regra de exemplo filtra a conta para executar as ações apenas no `container1/foo`. Para todos os objetos que estão dentro `container1` **AND** começa com `foo`, estas ações seguintes são executadas: 

- Camada de blob para o escalão acesso esporádico 30 dias após a última modificação
- Blob de camada para camada de arquivo 90 dias após a última modificação
- Eliminar blob 2,555 dias (7 anos) após a última modificação
- Eliminar instantâneos de blob de 90 dias após a criação do instantâneo

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "ruleFoo", 
      "type": "Lifecycle", 
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}

```

### <a name="rule-filters"></a>Filtros de regra

Filtros de limitam as ações de regras a um subconjunto de blobs na conta de armazenamento. Se vários filtros definidos, uma lógica `AND` é executada em todos os filtros.

Durante a pré-visualização, os filtros válidos incluem:

| Nome do filtro | Tipo de filtro | Notas | É necessário |
|-------------|-------------|-------|-------------|
| blobTypes   | Uma matriz de valores de enum predefinidos. | Para a versão de pré-visualização, só `blockBlob` é suportada. | Sim |
| prefixMatch | Uma matriz de cadeias de caracteres para prefixos ser corresponder. Uma cadeia de caracteres de prefixo tem de começar com um nome de contentor. Por exemplo, se todos os blobs em "https://myaccount.blob.core.windows.net/container1/foo/..." deve ser correspondido para uma regra, o prefixMatch seria "container1/foo". | Se prefixMatch não está definido, as regras são aplicadas a todos os blobs na conta. | Não |

### <a name="rule-actions"></a>Ações de regras

Ações são aplicadas para os blobs filtrados quando for cumprida a condição de execução.

Em pré-visualização, o gerenciamento de ciclo de vida suporta a disposição em camadas e eliminação de BLOBs e a eliminação de instantâneos de blob. Cada regra tem de ter pelo menos uma ação definida em blobs ou instantâneos de blob.

| Ação        | Base de Blob                                   | Instantâneo      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Suporta os blobs atualmente na camada de acesso frequente         | Não suportado |
| tierToArchive | Suporta os blobs atualmente no escalão acesso esporádico ou de acesso frequente | Não suportado |
| delete        | Suportadas                                   | Suportadas     |

>[!NOTE] 
Se mais de uma ação é definida no mesmo blob, gerenciamento de ciclo de vida aplica-se a ação menos dispendiosa para o blob. (por exemplo, a ação `delete` é mais barato do que a ação `tierToArchive`. Ação `tierToArchive` é mais barato do que a ação `tierToCool`.)

Em pré-visualização, as condições de execução da ação baseiam-se na idade. Base blob utiliza hora da última modificação para controlar a idade e a hora da criação de instantâneo de usos de instantâneos para controlar a idade de Blobs.

| Condição de ação de execução | Valor de condição | Descrição |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Valor de número inteiro que indica a idade em dias | Condição válida para ações de blob de base |
| daysAfterCreationGreaterThan     | Valor de número inteiro que indica a idade em dias | Condição válida para ações de instantâneo de blob | 

## <a name="examples"></a>Exemplos
Os exemplos seguintes demonstram como abordar cenários comuns com regras de política de ciclo de vida.

### <a name="move-aging-data-to-a-cooler-tier"></a>Mover dados de classificação por vencimento para uma camada mais esporádica

O exemplo seguinte demonstra como fazer a transição de blobs de blocos, o prefixo `container1/foo` ou `container2/bar`. A política de transições de blobs que ainda não foram modificadas em mais de 30 dias para armazenamento de acesso esporádico e não modificados em 90 dias para a camada de armazenamento de blobs:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "agingRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "container1/foo", "container2/bar" ]
          },
          "actions": {
            "baseBlob": {
              "tierToCool": { "daysAfterModificationGreaterThan": 30 },
              "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```

### <a name="archive-data-at-ingest"></a>Dados de arquivo na ingestão 

Alguns dados permanecem inativos na nuvem e são raramente acedidos após serem armazenados ou não são acedidos de todo. Estes dados são melhores ser arquivados imediatamente depois do que é ingerido. A política de ciclo de vida seguinte está configurada para arquivar dados na ingestão. Neste exemplo blobs na conta de armazenamento dentro do contentor de blocos de transições `archivecontainer` imediatamente numa camada de arquivo. A transição imediata é conseguida ao agir em blobs de 0 dias após a hora da última modificação:

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "archiveRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "archivecontainer" ]
          },
          "actions": {
            "baseBlob": { 
                "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
            }
          }
        }      
    }
  ]
}

```

### <a name="expire-data-based-on-age"></a>Expirar dados com base na idade

Espera-se que alguns dados expirar dias ou meses após a criação para reduzir os custos ou estar em conformidade com as normas governamentais. Uma política de gestão do ciclo de vida pode ser configurada para expirar dados pela eliminação com base na idade de dados. O exemplo seguinte mostra uma política que elimina todos os blobs de blocos (com nenhuma prefixo especificado) anteriores a 365 dias.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "expirationRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ]
          },
          "actions": {
            "baseBlob": {
              "delete": { "daysAfterModificationGreaterThan": 365 }
            }
          }
        }      
    }
  ]
}
```

### <a name="delete-old-snapshots"></a>Eliminar instantâneos antigos

Para dados que são modificados e acedidos regularmente ao longo de seu ciclo de vida, os instantâneos são frequentemente utilizados para controlar as versões mais antigas dos dados. Pode criar uma política que elimina os instantâneos antigos com base na idade de instantâneo. A idade do instantâneo é determinada por avaliar o tempo de criação do instantâneo. Esta regra de política eliminações bloquear instantâneos de BLOBs no contentor `activedata` que são de 90 dias ou mais antigos após a criação do instantâneo.

```json
{
  "version": "0.5",
  "rules": [ 
    {
      "name": "snapshotRule", 
      "type": "Lifecycle", 
      "definition": 
        {
          "filters": {
            "blobTypes": [ "blockBlob" ],
            "prefixMatch": [ "activedata" ]
          },
          "actions": {            
            "snapshot": {
              "delete": { "daysAfterCreationGreaterThan": 90 }
            }
          }
        }      
    }
  ]
}
```
## <a name="faq"></a>FAQ
### <a name="i-created-a-new-policy-why-are-the-actions-specified-not-executed-immediately"></a>Criei uma nova política, por que as ações especificadas não foi executada imediatamente? 

Política de ciclo de vida é executada uma vez por dia pela plataforma. Depois de uma nova política estiver definida, pode demorar até 24 horas para ações como disposição em camadas ou eliminação para ser iniciado e executado.  

## <a name="next-steps"></a>Passos Seguintes

Saiba como recuperar dados após a eliminação acidental:

- [Eliminação de forma recuperável para blobs de armazenamento do Azure ](../blobs/storage-blob-soft-delete.md)
