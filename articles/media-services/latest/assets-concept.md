---
title: Ativos nos serviços de multimédia do Azure | Microsoft Docs
description: Este artigo fornece uma explicação de quais são os recursos e como são utilizados pelos serviços de suporte de dados do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 791871fc3da98b380da9dbe32333a55f670c22e8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638284"
---
# <a name="assets"></a>Elementos

Um **Asset** contém ficheiros digitais (incluindo as vídeo, áudio, imagens, coleções de miniaturas, controla de texto e legendas ficheiros) e os metadados sobre estes ficheiros. Depois dos ficheiros digitais são carregados para um elemento, pode ser utilizados nos serviços de suporte de dados de codificação e fluxos de trabalho de transmissão em fluxo.

Um recurso está mapeado para um contentor do blob no [conta de armazenamento do Azure](storage-account-concept.md) e os ficheiros no elemento são armazenados como blobs de blocos num contentor. Pode interagir com os ficheiros de recurso dos contentores utilizando os clientes do SDK de armazenamento.

Media Services do Azure suporta camadas de Blob quando a conta utiliza para fins gerais v2 (GPv2) de armazenamento. Com GPv2, pode mover o armazenamento de frio ou ficheiros para esporádico. Armazenamento de frio é adequado para arquivar os ficheiros de origem quando já não é necessário (por exemplo, depois de se tem sido codificados.)

Nos Media Services v3, a entrada da tarefa pode ser criada de recursos ou a partir do URL de HTTP (s). Para criar um recurso que pode ser utilizado como entrada para a sua tarefa, consulte [criar uma entrada de tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md).

Além disso, leia sobre [contas de armazenamento nos serviços de suporte de dados](storage-account-concept.md) e [transformações e tarefas](transform-concept.md).

## <a name="asset-definition"></a>Definição de recurso

A tabela seguinte apresenta as propriedades do elemento e fornece as respetivas definições.

|Nome|Tipo|Descrição|
|---|---|---|
|Id|cadeia|ID de recurso completamente qualificado para o recurso.|
|name|cadeia|O nome do recurso.|
|properties.alternateId |cadeia|O ID alternativo do elemento.|
|properties.assetId |cadeia|O ID de recurso.|
|Properties.Container |cadeia|O nome do contentor do blob de recurso.|
|Properties.created |cadeia|A data de criação do elemento.|
|Properties.Description |cadeia|A descrição do recurso.|
|properties.lastModified |cadeia|A data última modificação do elemento.|
|properties.storageAccountName |cadeia|O nome da conta de armazenamento.|
|properties.storageEncryptionFormat |AssetStorageEncryptionFormat |O formato de encriptação do recurso. Um dos None ou MediaStorageEncryption.|
|tipo|cadeia|O tipo de recurso.|

Para a definição completa, consulte [ativos](https://docs.microsoft.com/rest/api/media/assets).

## <a name="filtering-ordering-paging"></a>Paginação de filtragem, ordenação,

Os Media Services suportam as seguintes opções de consulta de OData para ativos: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

### <a name="filteringordering"></a>Ordenação/filtragem

A tabela seguinte mostra como estas opções podem ser aplicadas para as propriedades de recurso: 

|Nome|Filtro|Ordem|
|---|---|---|
|Id|Suporta:<br/>Igual a<br/>Mais do que<br/>Inferior a|Suporta:<br/>Ascendente<br/>Descendente|
|name|||
|properties.alternateId |Suporta:<br/>Igual a||
|properties.assetId |Suporta:<br/>Igual a||
|Properties.Container |||
|Properties.created|Suporta:<br/>Igual a<br/>Mais do que<br/>Inferior a|Suporta:<br/>Ascendente<br/>Descendente|
|Properties.Description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|tipo|||

Os seguintes c# exemplo filtros na data de criação:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>Paginação

É suportada paginação para cada uma das ordens de ordenação ativado quatro. 

Se uma resposta de consulta contém muitas (atualmente através de 1000) itens, o serviço devolve um "@odata.nextLink" propriedade para obter a seguinte página de resultados. Isto pode ser utilizado para a página através do conjunto de resultados de todo. O tamanho de página não é configurável pelo utilizador. 

Se os recursos são criados ou eliminados durante a paginação através da coleção, as alterações serão refletidas nos resultados devolvidos (se essas alterações na parte da coleção que não tenha sido transferida.) 

O exemplo do c# seguinte mostra como enumerar através de todos os recursos da conta.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

Para obter exemplos REST, consulte [ativos - lista](https://docs.microsoft.com/rest/api/media/assets/list)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)
