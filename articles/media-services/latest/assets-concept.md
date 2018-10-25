---
title: Ativos nos serviços de multimédia do Azure | Documentos da Microsoft
description: Este artigo fornece uma explicação sobre o que estão ativos, e como elas são usadas pelos serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 62cc4634a0f76b0562d5b3c1355a7442fc5cf989
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985238"
---
# <a name="assets"></a>Elementos

Uma **Asset** contém ficheiros digitais (incluindo o vídeo, áudio, imagens, coleções de miniaturas, pistas de texto e ficheiros de legendas) e os metadados relativos a esses ficheiros. Depois dos ficheiros digitais são carregados para um elemento, poderia ser usados nos serviços de multimédia, codificar e transmitir fluxos de trabalho.

Um elemento é mapeado para um contentor de BLOBs no [conta de armazenamento do Azure](storage-account-concept.md) e os ficheiros no elemento são armazenados como blobs de blocos nesse contentor. Pode interagir com os ficheiros de recursos dos contentores com os clientes do SDK de armazenamento.

Serviços de multimédia do Azure suporta escalões de Blob, quando a conta utiliza para fins gerais v2 (GPv2) de armazenamento. Com a GPv2, pode mover arquivos para esporádica ou de armazenamento amovível. Armazenamento esporádico é adequado para arquivamento de arquivos de origem quando já não for necessário (por exemplo, depois que eles tenham sido codificados.)

Em serviços de multimédia v3, na entrada da tarefa pode ser criada dos recursos ou a partir de URLs de HTTP (s). Para criar um elemento que pode ser utilizado como entrada para a sua tarefa, consulte [criar uma entrada da tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md).

Além disso, leia sobre [contas de armazenamento nos serviços de multimédia](storage-account-concept.md) e [transformações e tarefas](transform-concept.md).

## <a name="asset-definition"></a>Definição do recurso

A tabela seguinte mostra as propriedades do recurso e fornece as respetivas definições.

|Nome|Tipo|Descrição|
|---|---|---|
|ID|string|ID de recurso completamente qualificado para o recurso.|
|nome|string|O nome do recurso.|
|properties.alternateId |string|O ID alternativo do ativo.|
|properties.assetId |string|O ID de recurso.|
|Properties.Container |string|O nome do contentor de BLOBs de recurso.|
|Properties.created |string|A data de criação do recurso.|
|Properties.Description |string|A descrição do recurso.|
|properties.lastModified |string|A data última modificação do recurso.|
|properties.storageAccountName |string|O nome da conta de armazenamento.|
|properties.storageEncryptionFormat |AssetStorageEncryptionFormat |O formato de encriptação ativo. Um dos None ou MediaStorageEncryption.|
|tipo|string|O tipo de recurso.|

A definição completa, consulte [ativos](https://docs.microsoft.com/rest/api/media/assets).

## <a name="filtering-ordering-paging"></a>Paginação de filtragem, ordenação,

Serviços de multimédia suportam as seguintes opções de consulta de OData para ativos: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

Descrição do operador:

* EQ = igual a
* Ne = não igual a
* A ge = maior que ou igual a
* Le = menor ou igual a
* Gt = maior que
* Lt = menor que

### <a name="filteringordering"></a>Filtragem/ordenação

A tabela seguinte mostra como essas opções podem ser aplicadas às propriedades de recurso: 

|Nome|Filtro|Encomenda|
|---|---|---|
|ID|||
|nome|Suporta: Eq, Gt, Lt|Suporta: ascendente e descendente|
|properties.alternateId |Suporta: Eq||
|properties.assetId |Suporta: Eq||
|Properties.Container |||
|Properties.created|Suporta: Eq, Gt, Lt| Suporta: Ascendente e descendente|
|Properties.Description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|tipo|||

O exemplo seguinte c# filtra na data de criação:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>Paginação

Paginação é suportada para cada uma das quatro ordens de classificação ativado. Atualmente, o tamanho da página é 1000.

> [!TIP]
> Deve sempre utilizar a ligação seguinte enumere a coleção e não dependem de um tamanho de página em particular.

Se uma resposta de consulta contém o número de itens, o serviço retornará um "\@OData. NextLink" propriedade para obter a próxima página de resultados. Isto pode ser utilizado para a página usando o conjunto de resultados inteiro. Não é possível configurar o tamanho da página. 

Se os recursos são criados ou eliminados enquanto a paginação por meio da coleção, as alterações são refletidas no resultados retornados (se essas alterações na parte da coleção que não foi transferida.) 

O exemplo do c# seguinte mostra como a enumeração por meio de todos os recursos na conta.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

Para obter exemplos REST, consulte [ativos - lista](https://docs.microsoft.com/rest/api/media/assets/list)

## <a name="storage-side-encryption"></a>Encriptação do lado do armazenamento

Para proteger os seus ativos inativos, os recursos devem ser encriptados pela encriptação do lado do armazenamento. A tabela seguinte mostra como a encriptação do lado do armazenamento funciona nos serviços de multimédia:

|Opção de encriptação|Descrição|Serviços de Multimédia v2|Serviços de Multimédia v3|
|---|---|---|---|
|Encriptação de armazenamento dos serviços de multimédia|Encriptação AES-256, chave gerida pelos serviços de multimédia|Suportado<sup>(1)</sup>|Não suportado<sup>(2)</sup>|
|[Encriptação do serviço de armazenamento para dados Inativos](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Chave de encriptação do lado do servidor oferecidas pelo armazenamento do Azure, gerida pelo Azure ou pelo cliente|Suportadas|Suportadas|
|[Encriptação do lado do cliente de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Oferecidas pelo armazenamento do Azure, chave gerida pelo cliente no Cofre de chaves de encriptação do lado do cliente|Não suportado|Não suportado|

<sup>1</sup> enquanto os serviços de multimédia oferece suporte à manipulação de conteúdo de forma/sem qualquer outra forma de encriptação, ao fazê-lo por isso, não é recomendado.

<sup>2</sup> em serviços de multimédia v3, a encriptação de armazenamento (encriptação AES-256) só é suportada para em versões anteriores compatibilidade quando os recursos foram criados com os serviços de multimédia v2. O que significa v3 funciona com o armazenamento existente encriptado ativos, mas não permitirá que a criação de novos itens.

## <a name="next-steps"></a>Passos Seguintes

[Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)
