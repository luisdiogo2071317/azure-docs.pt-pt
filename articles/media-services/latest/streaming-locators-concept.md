---
title: Transmissão em fluxo localizadores nos serviços de multimédia do Azure | Documentos da Microsoft
description: Este artigo fornece uma explicação sobre o que são os localizadores de transmissão em fluxo e como elas são usadas pelos serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 1757ca84e7390f1ecd2d6d1e90a085372d3e4c57
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381186"
---
# <a name="streaming-locators"></a>Localizadores de Transmissão em Fluxo

Para fornecer a seus clientes com um URL que eles podem usar para reproduzir com codificação de vídeo ou arquivos de áudio, tem de criar uma [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) e criar os URLs de transmissão em fluxo. Para obter mais informações, consulte [Stream um ficheiro](stream-files-dotnet-quickstart.md).

## <a name="streaminglocator-definition"></a>Definição de StreamingLocator

A tabela seguinte mostra as propriedades do StreamingLocator e fornece as respetivas definições.

|Nome|Tipo|Descrição|
|---|---|---|
|ID |cadeia|ID de recurso completamente qualificado para o recurso.|
|nome   |cadeia|O nome do recurso.|
|properties.alternativeMediaId  |cadeia|ID de suporte de dados alternativa deste localizador de transmissão em fluxo.|
|properties.assetName   |cadeia|Nome do elemento|
|properties.contentKeys |StreamingLocatorContentKey []|O ContentKeys utilizado por este localizador de transmissão em fluxo.|
|Properties.created |cadeia|Hora de criação do localizador de transmissão em fluxo.|
|properties.defaultContentKeyPolicyName |cadeia|Nome da predefinida ContentKeyPolicy utilizado por este localizador de transmissão em fluxo.|
|properties.endTime |cadeia|A hora de fim do localizador de transmissão em fluxo.|
|properties.startTime   |cadeia|A hora de início do localizador de transmissão em fluxo.|
|properties.streamingLocatorId  |cadeia|O StreamingLocatorId do localizador de transmissão em fluxo.|
|properties.streamingPolicyName |cadeia|Nome da política de transmissão em fluxo utilizada por este localizador de transmissão em fluxo. Especifique o nome da política de transmissão em fluxo que criou ou utilizar uma das políticas predefinidas de transmissão em fluxo. As políticas de transmissão em fluxo predefinido disponíveis são: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' e ' Predefined_ MultiDrmStreaming'|
|tipo   |cadeia|O tipo de recurso.|

A definição completa, consulte [localizadores de transmissão em fluxo](https://docs.microsoft.com/rest/api/media/streaminglocators).

## <a name="filtering-ordering-paging"></a>Paginação de filtragem, ordenação,

Serviços de multimédia suportam as seguintes opções de consulta de OData para os localizadores de transmissão em fluxo: 

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

A tabela seguinte mostra como essas opções podem ser aplicadas às propriedades StreamingLocator: 

|Nome|Filtro|Encomenda|
|---|---|---|
|ID |||
|nome|Eq, ne, ge, le, gt, lt|Ascendente e descendente|
|properties.alternativeMediaId  |||
|properties.assetName   |||
|properties.contentKeys |||
|Properties.created |Eq, ne, ge, le, gt, lt|Ascendente e descendente|
|properties.defaultContentKeyPolicyName |||
|properties.endTime |Eq, ne, ge, le, gt, lt|Ascendente e descendente|
|properties.startTime   |||
|properties.streamingLocatorId  |||
|properties.streamingPolicyName |||
|tipo   |||

### <a name="pagination"></a>Paginação

Paginação é suportada para cada uma das quatro ordens de classificação ativado. Atualmente, o tamanho da página é 10.

> [!TIP]
> Deve sempre utilizar a ligação seguinte enumere a coleção e não dependem de um tamanho de página em particular.

Se uma resposta de consulta contém o número de itens, o serviço retornará um "\@OData. NextLink" propriedade para obter a próxima página de resultados. Isto pode ser utilizado para a página usando o conjunto de resultados inteiro. Não é possível configurar o tamanho da página. 

Se estiver a StreamingLocators criadas ou eliminadas enquanto a paginação por meio da coleção, as alterações são refletidas no resultados retornados (se essas alterações na parte da coleção que não foi transferida.) 

O exemplo do c# seguinte mostra como a enumeração por meio de todos os StreamingLocators na conta.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

Para obter exemplos REST, consulte [localizadores de transmissão em fluxo - lista](https://docs.microsoft.com/rest/api/media/streaminglocators/streaminglocators_list)

## <a name="next-steps"></a>Passos Seguintes

[Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)
