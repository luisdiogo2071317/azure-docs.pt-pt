---
title: Gerir o ciclo de vida do armazenamento do Azure
description: Saiba como criar regras de política de ciclo de vida aos dados de againg de transição de frequente para as camadas de acesso esporádico e de arquivo.
services: storage
author: yzheng-msft
manager: jwillis
ms.service: storage
ms.workload: storage
ms.topic: article
ms.date: 04/30/2018
ms.author: yzheng
ms.openlocfilehash: b141adc9025f2f40acdfbd1f2d7f378173463956
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34805161"
---
# <a name="managing-the-azure-blob-storage-lifecycle-preview"></a>Gerir o ciclo de vida de armazenamento de Blobs do Azure (pré-visualização)

Conjuntos de dados tem ciclos de vida exclusivos. Alguns dados são acedidos frequentemente numa fase inicial no ciclo de vida, mas a necessidade de acesso significativamente ignora como a sua idade aumenta. Alguns dados permanecem Inativos na nuvem e são raramente acedidos armazenados uma vez. Alguns dados expirarem dias ou os meses após a criação, enquanto outros conjuntos de dados são lidos e modificados durante o respetivas durações ativamente. Gestão de ciclo de vida do Blob Storage do Azure (pré-visualização) oferece uma política de avançado, baseadas em regras que pode utilizar a transição os dados para a camada de acesso melhores e expirar os dados no fim do respetivo ciclo de vida.

Política de gestão do ciclo de vida ajuda-o:

- Transição de blobs para uma camada de armazenamento cooler (frequente para esporádico, frequente para arquivo, ou esporádico para arquivar) para otimizar o desempenho e custo
- Eliminar blobs no fim do respetivos ciclos de vida
- Definir regras para ser executada uma vez por dia ao nível da conta de armazenamento (suporta contas do storage GPv2 e BLOBs)
- Aplicar regras a contentores ou um subconjunto de blobs (utilizando os prefixos como filtros)

Considere um conjunto de dados que são acedidos com frequência durante a fase inicial do ciclo de vida, é necessário apenas ocasionalmente após duas semanas e são raramente acedidos após um mês e acima. Neste cenário, armazenamento frequente é melhor durante as fases antecipadas, armazenamento esporádico é mais adequado para acesso ocasional e armazenamento de arquivo é a melhor opção de camadas após a sua idade aumenta ao longo de um mês. Ao ajustar as camadas de armazenamento no que respeita à idade dos dados, pode estruturar as opções de armazenamento menos dispendiosas para as suas necessidades. Para alcançar esta transição, políticas de gestão do ciclo de vida estão disponíveis para mover dados de envelhecimento para cooler camadas.

## <a name="storage-account-support"></a>Suporte de conta de armazenamento

Política de gestão do ciclo de vida está disponível com os dois fins gerais v2 (GPv2) conta e a conta de armazenamento de Blobs. Pode converter uma conta de objetivo geral (GPv1) existente para uma conta de GPv2 através de um processo de um clique simples no portal do Azure. Para saber mais, veja [Azure storage account options](../common/storage-account-options.md) (Opções de contas de armazenamento do Azure).  

## <a name="pricing"></a>Preços 

A funcionalidade de gestão do ciclo de vida é gratuita em pré-visualização. Os clientes são-lhe cobrados o custo das operações regulares para o [lista Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) e [definir a camada de Blob](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) chamadas da API. Consulte [preços do Blob de bloco](https://azure.microsoft.com/pricing/details/storage/blobs/) para obter mais informações sobre preços.

## <a name="register-for-preview"></a>Registar-se na pré-visualização 
Para se inscrever na pré-visualização pública, terá de submeter um pedido para registar esta funcionalidade para a sua subscrição. Depois do pedido é aprovado (dentro de alguns dias), qualquer conta GPv2 ou armazenamento de BLOBs existente e nova nos EUA oeste, 2 e Central EUA oeste terão a funcionalidade ativada. Durante a pré-visualização, é suportada apenas BLOBs de blocos. Tal como acontece com a maioria das pré-visualizações, esta funcionalidade não deve ser utilizada para cargas de trabalho de produção até atingir depois da disponibilidade geral

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
Se a funcionalidade de aprovação e registada corretamente, deverá receber o estado de "Registado".

### <a name="cli-20"></a>CLI 2.0

Para submeter um pedido: 
```cli
az feature register –-namespace Microsoft.Storage –-name DLM
```
Pode verificar o estado de aprovação de registo com o seguinte comando:
```cli
-az feature show –-namespace Microsoft.Storage –-name DLM
```
Se a funcionalidade de aprovação e registada corretamente, deverá receber o estado de "Registado". 


## <a name="add-or-remove-policies"></a>Adicionar ou remover políticas 

Pode adicionar, editar ou remover uma política com as ferramentas de cliente, REST APIs, PowerShell ou portal do Azure nos seguintes idiomas: [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/8.0.0-preview), [Python](https://pypi.org/project/azure-mgmt-storage/2.0.0rc3/), [Node.js]( https://www.npmjs.com/package/azure-arm-storage/v/5.0.0), [Ruby]( https://rubygems.org/gems/azure_mgmt_storage/versions/0.16.2). 

### <a name="azure-portal"></a>Portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Para navegar para a sua conta de armazenamento, selecione Todos os Recursos, em seguida, selecione a sua conta de armazenamento.

3. No painel de definições, clique em **ciclo de vida de gestão** agrupados ao abrigo do serviço Blob para ver e/ou alterar as políticas.

### <a name="powershell"></a>PowerShell

```powershell
$rules = '{ ... }' 

Set-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName] -Policy $rules 

Get-AzureRmStorageAccountManagementPolicy -ResourceGroupName [resourceGroupName] -StorageAccountName [storageAccountName]
```

> [!NOTE]
Se ativar as regras de firewall para a sua conta de armazenamento, podem ser bloqueados pedidos de gestão do ciclo de vida. Pode desbloqueá-lo, fornecendo as exceções. Para obter mais informações, consulte a secção de exceções [configurar as firewalls e redes virtuais](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

## <a name="policies"></a>Políticas

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


Dentro de uma política, são necessários dois parâmetros:

| Nome do parâmetro | Tipo de parâmetro | Notas |
|----------------|----------------|-------|
| versão        | Uma cadeia expressada como `x.x` | O número de versão de pré-visualização é 0,5 |
| regras          | Uma matriz de objetos de regra | É necessário pelo menos uma regra em cada política. Durante a pré-visualização, pode especificar até 10 regras de acordo com a política. |

Parâmetros necessários dentro de uma regra são:

| Nome do parâmetro | Tipo de parâmetro | Notas |
|----------------|----------------|-------|
| Nome           | Cadeia | Um nome de regra pode conter qualquer combinação de carateres alfa numérico. Nome da regra está maiúsculas e minúsculas. Tem de ser exclusivo dentro de uma política. |
| tipo           | Um valor de enumeração | É o valor válido para a pré-visualização `Lifecycle` |
| definição     | Um objeto que define a regra de ciclo de vida | Cada definição é efetuada cópia de segurança com um conjunto de filtro e um conjunto de ação. |

## <a name="rules"></a>Regras

Cada definição da regra inclui um conjunto de filtro e um conjunto de ação. A regra de exemplo seguinte modifica a camada de base para blobs de blocos com prefixo `foo`. Na política, estas regras são definidas como:

- Blob de camada para armazenamento de 30 dias após a última modificação
- Blob de armazenamento de arquivos de camada de 90 dias após a última modificação
- Eliminar o blob 2,555 dias (7 anos) após a última modificação
- Elimine os instantâneos do blob 90 dias após a criação de instantâneos

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
          "prefixMatch": [ "foo" ]
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

## <a name="rule-filters"></a>Filtros de regra

Filtros de limitam as ações de regra a um subconjunto de blobs numa conta de armazenamento. Se for definidos um vários filtros, uma lógica `AND` é efetuada em todos os filtros.

Durante a pré-visualização, filtros válidos incluem:

| Nome do filtro | Tipo de filtro | Notas | É necessário |
|-------------|-------------|-------|-------------|
| blobTypes   | Uma matriz de valores de enumeração predefinidas. | Para a versão de pré-visualização, apenas `blockBlob` é suportada. | Sim |
| prefixMatch | Uma matriz de cadeias de prefixos ser corresponder. | Se não estiver definida, esta regra aplica-se a todos os blobs na conta. | Não |

### <a name="rule-actions"></a>Ações de regra

As ações são aplicadas para os blobs filtrados quando for cumprida a condição de execução.

Pré-visualização, gestão de ciclo de vida suporta a criação de camadas e eliminação de BLOBs e eliminação de instantâneos do blob. Cada regra tem de ter pelo menos uma ação definida em blobs ou instantâneos do blob.

| Ação        | Base Blob                                   | Instantâneo      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Suporta blobs atualmente na camada de acesso frequente         | Não suportado |
| tierToArchive | Suporta blobs atualmente em frequente ou esporádica camada | Não suportado |
| eliminar        | Suportadas                                   | Suportadas     |

Pré-visualização, as condições de execução da ação baseiam-se na idade. Base blob utiliza hora da última modificação para controlar a idade e tempo de criação de instantâneos utiliza instantâneo para controlar a idade do blob.

| Condição de execução da ação | Valor da condição | Descrição |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Valor de número inteiro que indica a idade em dias | Condição válida para ações de base blob |
| daysAfterCreationGreaterThan     | Valor de número inteiro que indica a idade em dias | Condição válida para ações de instantâneos do blob | 

## <a name="examples"></a>Exemplos
Os exemplos seguintes demonstram como abordar cenários comuns com as regras de política do ciclo de vida.

### <a name="move-aging-data-to-a-cooler-tier"></a>Mover dados de envelhecimento para uma camada cooler

O exemplo seguinte demonstra como efetuar a transição o prefixo de blobs de blocos `foo` ou `bar`. A política passa não modificados nos 90 dias para a camada de arquivo de blobs e blobs que ainda não foi modificados mais de 30 dias para armazenamento esporádico:

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
            "prefixMatch": [ "foo", "bar" ]
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

### <a name="archive-data-at-ingest"></a>Dados de arquivo em inserção 

Alguns dados permanecem inativos na nuvem e são raramente acedidos após serem armazenados ou não são acedidos de todo. Estes dados são melhores sejam arquivadas, imediatamente depois é ingerido. A política de ciclo de vida seguinte está configurada para arquivar dados na inserção. Este exemplo faz a transição de blobs de blocos numa conta de armazenamento com o prefixo de `archive` imediatamente para uma camada de arquivo. A transição imediata é conseguida ao atuar em blobs 0 dias após a hora da última modificação:

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
            "prefixMatch": [ "archive" ]
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

### <a name="expire-data-based-on-age"></a>Expirar os dados com base na duração

Alguns dados deverá expirar dias ou os meses após a criação para reduzir os custos ou estar em conformidade com regulamentos government. Uma política de gestão do ciclo de vida pode ser configurada para expirar data pela eliminação com base na duração de dados. O exemplo seguinte mostra uma política que elimina todos os blobs de blocos (com nenhuma prefixo especificado) anterior a 365 dias.

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

### <a name="delete-old-snapshots"></a>Elimine os instantâneos antigos

Para dados que são modificados e acedidos regularmente ao longo da respetiva duração, os instantâneos são frequentemente utilizados para controlar as versões anteriores dos dados. Pode criar uma política que elimina antigos instantâneos com base na duração de instantâneo. A duração de instantâneo é determinada através da avaliação do tempo de criação de instantâneo. Esta regra de política eliminações bloquear instantâneos do blob com o prefixo `activeData` que são 90 dias ou mais antigos após a criação do instantâneo.

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
            "prefixMatch": [ "activeData" ]
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

## <a name="next-steps"></a>Passos Seguintes

Saiba como recuperar dados após a eliminação acidental:

- [Eliminar soft para blobs de armazenamento do Azure ](../blobs/storage-blob-soft-delete.md)
