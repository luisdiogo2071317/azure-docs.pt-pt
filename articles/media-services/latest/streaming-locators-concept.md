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
ms.date: 12/20/2018
ms.author: juliako
ms.openlocfilehash: 658843fd5acbe0d4e29947e99c00edf4909fe9f4
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742750"
---
# <a name="streaming-locators"></a>Localizadores de Transmissão em Fluxo

Tem de fornecer a seus clientes com um URL que eles podem usar para reproduzir codificado em ficheiros de vídeos ou áudio, terá de criar uma [localizador de transmissão em fluxo](https://docs.microsoft.com/rest/api/media/streaminglocators) e criar os URLs de transmissão em fluxo. Para obter mais informações, consulte [Stream um ficheiro](stream-files-dotnet-quickstart.md).

## <a name="streaminglocator-definition"></a>Definição de StreamingLocator

A tabela seguinte mostra as propriedades do StreamingLocator e fornece as respetivas definições.

|Nome|Descrição|
|---|---|
|ID |ID de recurso completamente qualificado para o recurso.|
|nome|O nome do recurso.|
|properties.alternativeMediaId|ID de suporte de dados alternativa deste localizador de transmissão em fluxo.|
|properties.assetName|Nome do elemento|
|properties.contentKeys|O ContentKeys utilizado por este localizador de transmissão em fluxo.|
|Properties.created|Hora de criação do localizador de transmissão em fluxo.|
|properties.defaultContentKeyPolicyName|Nome da predefinida ContentKeyPolicy utilizado por este localizador de transmissão em fluxo.|
|properties.endTime|A hora de fim do localizador de transmissão em fluxo.|
|properties.startTime|A hora de início do localizador de transmissão em fluxo.|
|properties.streamingLocatorId|O StreamingLocatorId do localizador de transmissão em fluxo.|
|properties.streamingPolicyName |Nome da política de transmissão em fluxo utilizada por este localizador de transmissão em fluxo. Especifique o nome da política de transmissão em fluxo que criou ou utilizar uma das políticas predefinidas de transmissão em fluxo. As políticas de transmissão em fluxo predefinido disponíveis são: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' e 'Predefined_MultiDrmStreaming'|
|tipo|O tipo de recurso.|

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

Para obter exemplos REST, consulte [localizadores de transmissão em fluxo - lista](https://docs.microsoft.com/rest/api/media/streaminglocators/list)

## <a name="next-steps"></a>Passos Seguintes

[Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)
