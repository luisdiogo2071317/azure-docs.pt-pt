---
title: Eliminar soft para blobs de armazenamento do Azure (pré-visualização) | Microsoft Docs
description: Storage do Azure oferece agora a eliminação de forma recuperável (pré-visualização) para objetos de blob, para que mais facilmente pode recuperar dados quando está indica modificada ou eliminada por uma aplicação ou outro utilizador de conta de armazenamento.
services: storage
author: MichaelHauss
manager: vamshik
ms.service: storage
ms.topic: article
ms.date: 03/21/2018
ms.author: mihauss
ms.openlocfilehash: 0e728f9f9754d76d893b12309bb52201d772efbf
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
ms.locfileid: "34057864"
---
# <a name="soft-delete-for-azure-storage-blobs-preview"></a>Eliminação de forma recuperável para blobs de armazenamento do Azure (pré-visualização)

## <a name="overview"></a>Descrição geral

Storage do Azure oferece agora a eliminação de forma recuperável (pré-visualização) para objetos de blob, para que mais facilmente pode recuperar dados quando está indica modificada ou eliminada por uma aplicação ou outro utilizador de conta de armazenamento.

## <a name="how-does-it-work"></a>Como funciona?

Quando ativada, eliminação de forma recuperável permite-lhe guardar e recuperar os seus dados de blobs ou instantâneos do blob são eliminados. Esta proteção expande-se aos dados de BLOBs é apagar como resultado de uma substituição.

Quando os dados são eliminados, passa para um Estado eliminado de forma recuperável em vez de ser apagadas permanentemente. Quando a eliminação de forma recuperável está ativada e substituir os dados, é gerado um instantâneo eliminado de forma recuperável para guardar o estado dos dados substituídos. Objetos eliminados de forma recuperável estão invisíveis, a menos que explicitamente listados.
Pode configurar a quantidade de tempo, são possível recuperar dados eliminados de forma recuperável antes de expirar permanentemente.

Eliminação de forma recuperável é retrocompatível; não tem de efetuar quaisquer alterações às suas aplicações para tirar partido das proteções que affords esta funcionalidade. No entanto, [recuperação de dados](#recovery) introduz um novo **anular eliminação Blob** API.

> [!NOTE]
> Durante a pré-visualização pública, a chamada de camada de Blob definido num blob com instantâneos não é permitida.
Eliminação de forma recuperável gera instantâneos para proteger os seus dados quando são substituída. Estamos ativamente uma solução para permitir a criação de camadas de blobs com instantâneos.

### <a name="configuration-settings"></a>Definições de configuração

Quando cria uma nova conta, eliminação de forma recuperável está desativada por predefinição. Eliminação de forma recuperável também está desativada por predefinição para contas do storage existentes. Pode ativar/desativar a funcionalidade e desativar em qualquer altura durante a vigência de uma conta de armazenamento.

Ainda poderá aceder e recuperar dados eliminados de forma recuperável quando a funcionalidade está desativada, partindo do princípio que os dados eliminados de forma recuperável foi guardados quando a funcionalidade foi anteriormente ligada. Quando ativar a eliminação de forma recuperável, terá também de configurar o período de retenção.

O período de retenção indica a quantidade de tempo que os dados eliminados de forma recuperável são armazenada e disponíveis para recuperação. Para os blobs e instantâneos do blob que são eliminados de forma explícita, o relógio do período de retenção é iniciado quando os dados são eliminados. Recuperável instantâneos eliminado gerados pela funcionalidade de eliminação de forma recuperável quando dados são substituídos, o relógio inicia quando o instantâneo é gerado. Atualmente, pode manter os dados eliminados recuperável, para entre 1 e 365 dias.

Pode alterar o período de retenção de eliminação de forma recuperável em qualquer altura. Um período de retenção atualizado apenas se aplica aos dados eliminados recentemente. Anteriormente dados eliminados irão expirar com base no período de retenção que foi configurado quando esses dados foi eliminados.

### <a name="saving-deleted-data"></a>A guardar dados eliminadas

Eliminação de forma recuperável preserva os dados em muitos casos onde os blobs instantâneos do blob são eliminados ou substituídos.

Quando um blob é substituído utilizando **colocar Blob**, **colocar bloco**, **colocar a lista de bloqueios** ou **BLOBs de cópia** um instantâneo do Estado do blob antes do Escreva a operação é gerada automaticamente. Este instantâneo é um instantâneo eliminado de forma recuperável; é invisível, a menos que os objetos eliminados de forma recuperável explicitamente estão listados. Consulte o [recuperação](#recovery) secção para saber como lista de objetos eliminados de forma recuperável.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-overwrite.png)

*Dados eliminados de forma recuperável estão cinzento, Active Directory dados enquanto estes se azul. Mais recentemente escritos dados aparece por baixo de dados mais antigos. Quando B0 seja substituído pelos B1, é gerado um instantâneo eliminado recuperável B0. Quando B1 seja substituído pelos B2, é gerado um instantâneo eliminado recuperável B1.*

> [!NOTE]
> Eliminação de forma recuperável apenas affords substituir a proteção para operações de cópia quando está ligado conta do blob de destino.

> [!NOTE]
> Eliminação de forma recuperável não suportar substituir a proteção para os blobs na camada de arquivo. Se um blob no arquivo é substituído com um novo blob em qualquer camada, o blob substituído permanentemente expirou.

Quando **eliminar BLOBs** denomina-se num instantâneo, esse instantâneo está marcado como recuperável eliminado. Não é gerado um novo instantâneo.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-delete-snapshot.png)

*Dados eliminados de forma recuperável estão cinzento, Active Directory dados enquanto estes se azul. Mais recentemente escritos dados aparece por baixo de dados mais antigos. Quando **instantâneo Blob** é chamado, B0 se tornar um instantâneo e B1 é o estado ativo do blob. Quando o instantâneo B0 é eliminado, está marcado como recuperável eliminado.\*

Quando **eliminar BLOBs** é chamado um blob base (qualquer blob que é não-se de um instantâneo), esse blob está marcado como recuperável eliminado. Consistente com o comportamento anterior, chamar **eliminar BLOBs** num blob que tenha instantâneos de Active Directory devolve um erro. Chamar **eliminar BLOBs** num blob com instantâneos eliminados de forma recuperável não devolveu um erro. Ainda pode eliminar um blob e todos os respetivos instantâneos numa única operação quando a eliminação de forma recuperável está ativada. Se o fizer, marca o blob base e instantâneos de forma recuperável como eliminado.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-include.png)

*Dados eliminados de forma recuperável estão cinzento, Active Directory dados enquanto estes se azul. Mais recentemente escritos dados aparece por baixo de dados mais antigos. Aqui, um **eliminar BLOBs** é efetuada uma chamada para eliminar B2 e instantâneos todos os associados. O blob Active Directory, B2 e associados todos os instantâneos são marcados como recuperável eliminado.\*

> [!NOTE]
> Quando um blob de eliminados de forma recuperável é substituído, é automaticamente gerado um instantâneo de eliminados recuperável do Estado do blob antes da operação de escrita. O novo blob herda a camada do blob substituída.

Eliminação de forma recuperável não guarde os seus dados em casos de contentor ou conta eliminações, nem quando os metadados de BLOBs e BLOBs propriedades são substituídas. Para proteger uma conta de armazenamento de eliminação errada, pode configurar um bloqueio com o Azure Resource Manager. Consulte o artigo do Azure Resource Manager [recursos de bloqueio para impedir alterações inesperado](../../azure-resource-manager/resource-group-lock-resources.md) para obter mais informações.

A tabela seguinte detalha comportamento esperado quando a eliminação de forma recuperável está ativada:

| Operação de REST API                                                                                                | Tipo de recurso                 | Descrição                                                                                                 | Alteração do comportamento                                                                                                                                                                                                                                                                                                                                                   |
|-------------------------------------------------------------------------------------------------------------------|-------------------------------|-------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Eliminar](/rest/api/storagerp/StorageAccounts/Delete)              | Conta                       | Elimina a conta de armazenamento, incluindo todos os contentores e blobs nele contidos.                           | Nenhuma alteração. Não são recuperáveis contentores e blobs na conta eliminado.                                                                                                                                                                                                                                                                                          |
| [Eliminar o contentor](/rest/api/storageservices/fileservices/delete-container)       | Contentor                     | Elimina o contentor, incluindo todos os blobs nele contidos.                                                | Nenhuma alteração. Os BLOBs no contentor eliminado não são recuperáveis.                                                                                                                                                                                                                                                                                                       |
| [Colocar Blob](/rest/api/storageservices/fileservices/put-blob)                       | Bloco, de acréscimo e Blobs de páginas | Cria um novo blob ou substitui um blob existente num contentor                                          | Se utilizado para substituir um blob existente, um instantâneo do Estado do blob antes da chamada é gerado automaticamente. Isto também se aplica a um blob eliminado anteriormente recuperável se e apenas se está a ser substituído por um blob do mesmo tipo (blocos, acrescentar ou página). Se está substituído por um blob de um tipo diferente, todos os dados eliminados recuperável existentes irão expirar permanentemente. |
| [Eliminar o Blob](/rest/api/storageservices/fileservices/delete-blob)                 | Bloco, de acréscimo e Blobs de páginas | Marcas de um blob ou um instantâneo de BLOBs para eliminação. O blob ou um instantâneo mais tarde é eliminado durante a recolha de lixo | Se utilizada para eliminar que um instantâneo de blob, esse instantâneo está marcado como recuperável eliminado. Se utilizada para eliminar um blob, esse blob está marcado como recuperável eliminado.                                                                                                                                                                                                                           |
| [Copiar Blob](/rest/api/storageservices/fileservices/copy-blob)                     | Bloco, de acréscimo e Blobs de páginas | Copia um blob de origem para um blob de destino na mesma conta de armazenamento ou outra conta de armazenamento.       | Se utilizado para substituir um blob existente, um instantâneo do Estado do blob antes da chamada é gerado automaticamente. Isto também se aplica a um blob eliminado anteriormente recuperável se e apenas se está a ser substituído por um blob do mesmo tipo (blocos, acrescentar ou página). Se está substituído por um blob de um tipo diferente, todos os dados eliminados recuperável existentes irão expirar permanentemente. |
| [Colocar bloco](/rest/api/storageservices/put-block)                                  | Blobs de bloco                   | Cria um novo bloco de ser consolidada como parte de um blob de bloco.                                                | Se utilizado para consolidar um bloco de para um blob ativo não há nenhuma alteração. Se utilizados para consolidar um bloco de para um blob de forma recuperável eliminados, é criado um novo blob e é automaticamente gerado um instantâneo para capturar o estado do blob eliminado recuperável.                                                                                                                      |
| [Colocar a lista de bloqueios](/rest/api/storageservices/fileservices/put-block-list)           | Blobs de bloco                   | Consolida um blob, especificando o conjunto de blocos IDs que compõem o blob de bloco.                             | Se utilizado para substituir um blob existente, um instantâneo do Estado do blob antes da chamada é gerado automaticamente. Isto também se aplica a um blob eliminado anteriormente recuperável se e apenas se se tratar de um Blob de bloco. Se está substituído por um blob de um tipo diferente, todos os dados eliminados recuperável existentes irão expirar permanentemente.                                                |
| [Colocar a página](/rest/api/storageservices/fileservices/put-page)                       | Blobs de Página                    | Escreve um intervalo de páginas para um Blob de página.                                                                     | Nenhuma alteração. Dados de Blob de página que são substituídos ou limpo utilizando esta operação não são guardados e não são recuperáveis.                                                                                                                                                                                                                                                   |
| [Acrescentar bloco](/rest/api/storageservices/fileservices/append-block)               | Blobs de acréscimo                  | Escreve um bloco de dados ao fim de um Blob de acréscimo                                                         | Nenhuma alteração.                                                                                                                                                                                                                                                                                                                                                           |
| [Defina as propriedades de Blob](/rest/api/storageservices/fileservices/set-blob-properties) | Bloco, de acréscimo e Blobs de páginas | Define os valores das propriedades do sistema definidas para um blob.                                                       | Nenhuma alteração. Propriedades de blob substituída não são recuperáveis.                                                                                                                                                                                                                                                                                                          |
| [Definir os metadados do Blob](/rest/api/storageservices/fileservices/set-blob-metadata)     | Bloco, de acréscimo e Blobs de páginas | Define metadados definidos pelo utilizador para o blob especificado como um ou mais pares nome-valor.                          | Nenhuma alteração. Não são possível recuperar os metadados de blob substituída.                                                                                                                                                                                                                                                                                                             |

É importante notar que chamar a "Colocar página" para substituir ou limpar intervalos de um Blob de página irá não gerar automaticamente instantâneos. Discos da máquina virtual são apoiados por Blobs de páginas e utilizar **colocar página** para escrever dados.

### <a name="recovery"></a>Recuperação

Para tornar mais fácil recuperar dados eliminados, podemos ter introduziu uma nova API de "Anular eliminação Blob". Chamar a API anular eliminação no blob base eliminado recuperável restaura- e todas as associadas instantâneos eliminados recuperável, como o Active Directory. Ao chamar a API anular eliminação num blob base Active Directory restauros todos os associados instantâneos eliminados recuperável, como o Active Directory. Quando os instantâneos forem restaurados como o Active Directory, aspeto instantâneos gerados pelo utilizador; Não substituir o blob base.

Para restaurar um blob para um instantâneo eliminado recuperável específico pode chamar **anular eliminação Blob** no base blob. Em seguida, pode copiar o instantâneo ao longo do blob agora-ativo. Também pode copiar o instantâneo para um blob de novo.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-recover.png)

*Dados eliminados de forma recuperável estão cinzento, Active Directory dados enquanto estes se azul. Mais recentemente escritos dados aparece por baixo de dados mais antigos. Aqui, **anular eliminação Blob** denomina-se no blob B, deste modo, restaurar o base blob, B1 e instantâneos todos os associados, aqui apenas B0, como o Active Directory. No segundo passo, B0 é copiada através do blob base. Esta operação de cópia gera um instantâneo eliminado recuperável B1.\*

Para ver os blobs eliminados de forma recuperável e instantâneos do blob, pode optar por incluir dados eliminados no **lista Blobs**. Pode escolher para visualizar apenas recuperável eliminados base blobs, ou para incluir também instantâneos do blob eliminado de forma recuperável. Para todos os dados eliminados recuperável, pode ver a hora quando os dados foram eliminados, bem como o número de dias antes dos dados serão permanentemente expirou.

### <a name="example"></a>Exemplo

Segue-se o resultado da consola de um script de .NET que carrega, substitui a instantâneos, eliminações e restauros eliminar um blob com o nome "Olámundo" quando está ativado:

```bash
Upload:
- HelloWorld (is soft deleted: False, is snapshot: False)

Overwrite:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Snapshot:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Delete (including snapshots):
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: False)

Undelete:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Copy a snapshot over the base blob:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)
```

Consulte o [passos](#Next steps) na secção um ponteiro para a aplicação que provocou este resultado.

## <a name="pricing-and-billing"></a>Preços e faturação

Todos os dados eliminados de forma recuperável é faturada a taxa mesmo como dados do Active Directory. Não será cobrada para dados que estão a ser eliminados definitivamente após o período de retenção configurado. Para obter uma descrição mais aprofundada para instantâneos e a forma como estes acumular encargos, consulte [compreender como instantâneos acumular encargos](storage-blob-snapshots.md).

Não será faturado para transações relacionadas com a geração automática de instantâneos. Será faturado por **anular eliminação Blob** transações a taxa de "Operações de escrita".

Para obter mais detalhes sobre os preços para o armazenamento de Blobs do Azure no geral, veja o [página de preços do Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

Quando ativar inicialmente eliminação de forma recuperável, recomendamos que utilize um período de retenção pequeno para compreender melhor a forma como a funcionalidade irão afetar a fatura.

## <a name="quickstart"></a>Início Rápido

### <a name="azure-portal"></a>Portal do Azure
Para ativar a eliminação de forma recuperável, navegue para o **eliminação de forma recuperável** opção em **serviço Blob**. Em seguida, clique em **ativado** e introduza o número de dias que pretenda manter os dados eliminados de forma recuperável.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-configuration.png)

Para ver os blobs eliminados de forma recuperável, selecione o **Mostrar eliminar blobs** caixa de verificação.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted.png)

Para ver instantâneos eliminados flexível para um blob especificado, selecione o blob, em seguida, clique em **ver instantâneos**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

Certifique-se de que o **Mostrar eliminar instantâneos** caixa de verificação está selecionada.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Ao clicar num blob eliminado recuperável ou instantâneo, tenha em atenção as novas propriedades de blob. Indicam quando o objeto foi eliminado e quantos dias restantes até que o blob ou um instantâneo de blob permanentemente expirou. Se o objeto eliminado recuperável não é um instantâneo, também terá a opção para anular eliminação-lo.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-properties.png)

Lembre-se de que undeleting um blob também irá anular eliminação todos os instantâneos associados. Para anular a eliminação de forma recuperável instantâneos eliminados para um blob de Active Directory, clique em de blob e selecione **anular eliminação todos os instantâneos**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Depois de anular eliminação instantâneos de um blob, pode clicar em **promover** copiar um instantâneo através de blob de raiz, deste modo, restaurar o blob para o instantâneo.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-promote-snapshot.png)

### <a name="powershell"></a>PowerShell
Para ativar a eliminação de forma recuperável, Atualize as propriedades do serviço do cliente do blob. O exemplo seguinte ativa a eliminação de forma recuperável para um subconjunto de contas numa subscrição:

```powershell
Set-AzureRmContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzureRMStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzureStorageDeleteRetentionPolicy -RetentionDays 7
```

Para recuperar blobs que foram eliminados acidentalmente, chame Undelete os blobs. Lembre-se de que a chamar **anular eliminação Blob**, ambos no Active Directory e recuperável blobs eliminados, irá restaurar instantâneos eliminados de forma recuperável todos os associados como o Active Directory. O exemplo seguinte chama Undelete em todos os recuperável eliminados e Active Directory blobs num contentor:
```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzureStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
### <a name="python-client-library"></a>Biblioteca de clientes do Python

Para ativar a eliminação de forma recuperável, Atualize as propriedades do serviço do cliente do blob:

```python
# Make the requisite imports
from azure.storage.blob import BlockBlobService
from azure.storage.common.models import DeleteRetentionPolicy

# Initialize a block blob service
block_blob_service = BlockBlobService(account_name='<enter your storage account name>', account_key='<enter your storage account key>')

# Set the blob client's service property settings to enable soft delete
block_blob_service.set_blob_service_properties(delete_retention_policy = DeleteRetentionPolicy(enabled = True, days = 7))
```

### <a name="net-client-library"></a>Biblioteca de cliente .net

Para ativar a eliminação de forma recuperável, Atualize as propriedades do serviço do cliente do blob:

```csharp
// Get the blob client’s service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client’s service property settings
blobClient.SetServiceProperties(serviceProperties);
```

Para recuperar blobs que foram eliminados acidentalmente, chame Undelete os blobs.
Lembre-se de que a chamar **anular eliminação Blob**, ambos no Active Directory e recuperável blobs eliminados, irá restaurar instantâneos eliminados de forma recuperável todos os associados como o Active Directory. O exemplo seguinte chama Undelete em todos os recuperável eliminados e Active Directory blobs num contentor:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Recuperar para uma versão de blob específico, primeira chamada Undelete para um blob, em seguida, copie o instantâneo pretendido através de blob. O exemplo seguinte recupera um blob de bloco para o instantâneo gerado mais recentemente:

```csharp
// Undelete
await blockBlob.UndeleteAsync();

// List all blobs and snapshots in the container prefixed by the blob name
IEnumerable<IListBlobItem> allBlobVersions = container.ListBlobs(
    prefix: blockBlob.Name, useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Snapshots);

// Restore the most recently generated snapshot to the active blob    
CloudBlockBlob copySource = allBlobVersions.First(version => ((CloudBlockBlob)version).IsSnapshot &&
    ((CloudBlockBlob)version).Name == blockBlob.Name) as CloudBlockBlob;
blockBlob.StartCopy(copySource);
```

## <a name="should-i-use-soft-delete"></a>Deve utilizar eliminação de forma recuperável?

Se existir a possibilidade de que os seus dados acidentalmente são alterados ou eliminados por uma aplicação ou outro utilizador de conta de armazenamento, recomendamos a ativação da eliminação de forma recuperável.
Eliminação de forma recuperável é uma parte de uma estratégia de proteção de dados e pode ajudar a evitar perda de dados inadvertidas.

## <a name="faq"></a>FAQ

**Para que tipo de armazenamento posso utilizar a eliminação de forma recuperável?**

Atualmente, a eliminação de forma recuperável só está disponível para armazenamento de BLOBs (objetos).

**Eliminação de forma recuperável está disponível para todos os tipos de conta de armazenamento?**

Sim, a eliminação de forma recuperável está disponível para contas do blob storage, bem como para os blobs em contas do storage para fins gerais. Isto aplica-se às contas standard e premium. Eliminação de forma recuperável não está disponível para discos geridos.

**Eliminação de forma recuperável está disponível para todas as camadas de armazenamento?**

Sim, a eliminação de forma recuperável está disponível para todas as camadas de armazenamento, incluindo frequente, útil e de arquivo. No entanto, a eliminação de forma recuperável não suportar substituir a proteção para os blobs na camada de arquivo.

**As contas de armazenamento Premium têm um por limite de instantâneos do blob de 100. Instantâneos eliminados de forma recuperável contam para este limite?**

Não, soft eliminada instantâneos não é considerado este limite.

**Pode ativar na eliminação de forma recuperável para contas do storage existentes?**

Sim, a eliminação de forma recuperável é configurável existentes e novas contas do storage.

**Se eliminar uma conta de toda ou contentor com eliminação de forma recuperável ativada, todos os blobs associados guardar?**

Não, se eliminar uma conta de toda ou contentor, todos os blobs associados serão eliminados permanentemente. Para saber como proteger uma conta de armazenamento de eliminações acidentais, consulte o artigo do Azure Resource Manager [recursos de bloqueio para impedir alterações inesperado](../../azure-resource-manager/resource-group-lock-resources.md).

**Pode ver as métricas de capacidade para os dados eliminados?**

Dados eliminados de forma recuperável são incluídos como parte da sua capacidade de conta de armazenamento total. Para obter mais informações sobre o controlo e a capacidade de armazenamento de monitorização, consulte o [análise de armazenamento](../common/storage-analytics.md) artigo.

**Se posso desativar a eliminação de forma recuperável, posso continuarão a poder aceder a dados eliminados de forma recuperável?**

Sim, ainda será capaz de aceder e recuperar dados eliminados de forma recuperável não expirados quando a eliminação de forma recuperável está desativada.

**Pode ler e copiar recuperável instantâneos eliminados do meu blob?**

Sim, mas tem de chamar Undelete no blob primeiro.

**Eliminação de forma recuperável está disponível para todos os tipos de BLOBs?**

Sim, a eliminação de forma recuperável está disponível para Blobs de blocos, os Blobs de acréscimo e Blobs de páginas.

**Eliminação de forma recuperável está disponível para discos da máquina virtual?**

Eliminação de forma recuperável está disponível para os discos premium e padrão de não geridos. Eliminação de forma recuperável só irá ajudar a recuperar dados eliminados por **eliminar BLOBs**, **colocar Blob**, **colocar a lista de bloqueios**, **colocar bloco** e **Copiar Blob**. Dados substituídos por uma chamada para **colocar página** não é recuperável.

**É necessário alterar as minhas aplicações existentes para utilizar a eliminação de forma recuperável?**

É possível tirar partido da eliminação de forma recuperável independentemente da versão de API que está a utilizar. No entanto, para listar e recuperar blobs eliminados de forma recuperável e instantâneos do blob, terá de utilizar a versão de 2017-07-29 do [API de REST dos serviços de armazenamento](https://docs.microsoft.com/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) ou superior. Em geral, recomendamos sempre a utilizar a versão mais recente, independentemente se estão a utilizar esta funcionalidade.

## <a name="next-steps"></a>Passos seguintes

* [Código de exemplo do .NET](https://github.com/Azure-Samples/storage-dotnet-blob-soft-delete)

* [API de REST do serviço blob](/rest/api/storageservices/fileservices/blob-service-rest-api)

* [Replicação de armazenamento do Azure](../common/storage-redundancy.md)

* [Conceber aplicações altamente disponíveis utilizando RA-GRS](../common/storage-designing-ha-apps-with-ragrs.md)

* [O que fazer se ocorrer uma falha de armazenamento do Azure](../common/storage-disaster-recovery-guidance.md)
