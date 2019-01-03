---
title: Transmissão em fluxo políticas nos serviços de multimédia do Azure | Documentos da Microsoft
description: Este artigo fornece uma explicação sobre o que são políticas de transmissão em fluxo e como elas são usadas pelos serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/22/2018
ms.author: juliako
ms.openlocfilehash: d74ce913a2189dd1062b30f9def919cbbabe7b64
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742529"
---
# <a name="streaming-policies"></a>Políticas de Transmissão em Fluxo

Serviços de multimédia do Azure v3, políticas de transmissão em fluxo para permitir-definir protocolos de transmissão em fluxo e opções de encriptação para sua StreamingLocators. Pode especificar o nome da política de transmissão em fluxo que criou ou utilizar uma das políticas predefinidas de transmissão em fluxo. As políticas de transmissão em fluxo predefinido atualmente disponíveis são: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' e 'Predefined_MultiDrmStreaming'.

> [!IMPORTANT]
> Quando utilizar um personalizado [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies), deve criar um conjunto limitado dessas políticas para a sua conta de serviço de suporte de dados e utilizá-los novamente para os localizadores de transmissão em fluxo sempre que as mesmas opções de encriptação e os protocolos são necessários. A sua conta de serviço de suporte de dados tem uma quota para o número de entradas de política de transmissão em fluxo. Devem não ser a criar uma nova política de transmissão em fluxo para cada localizador de transmissão em fluxo.

## <a name="streamingpolicy-definition"></a>Definição de StreamingPolicy

A tabela seguinte mostra as propriedades do StreamingPolicy e fornece as respetivas definições.

|Nome|Descrição|
|---|---|
|ID|ID de recurso completamente qualificado para o recurso.|
|nome|O nome do recurso.|
|properties.commonEncryptionCbcs|Configuração de CommonEncryptionCbcs|
|properties.commonEncryptionCenc|Configuração de CommonEncryptionCenc|
|Properties.created |Hora de criação da política de transmissão em fluxo|
|properties.defaultContentKeyPolicyName |ContentKey predefinido utilizado pela política de transmissão em fluxo atual|
|properties.envelopeEncryption  |Configuração de EnvelopeEncryption|
|properties.noEncryption|Configurações de NoEncryption|
|tipo|O tipo de recurso.|

A definição completa, consulte [políticas de transmissão em fluxo](https://docs.microsoft.com/rest/api/media/streamingpolicies).

## <a name="filtering-ordering-paging"></a>Paginação de filtragem, ordenação,

Serviços de multimédia suportam as seguintes opções de consulta de OData para políticas de transmissão em fluxo: 

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

A tabela seguinte mostra como essas opções podem ser aplicadas às propriedades StreamingPolicy: 

|Nome|Filtro|Encomenda|
|---|---|---|
|ID|||
|nome|Eq, ne, ge, le, gt, lt|Ascendente e descendente|
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|Properties.created |Eq, ne, ge, le, gt, lt|Ascendente e descendente|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|tipo|||

### <a name="pagination"></a>Paginação

Paginação é suportada para cada uma das quatro ordens de classificação ativado. Atualmente, o tamanho da página é 10.

> [!TIP]
> Deve sempre utilizar a ligação seguinte enumere a coleção e não dependem de um tamanho de página em particular.

Se uma resposta de consulta contém o número de itens, o serviço retornará um "\@OData. NextLink" propriedade para obter a próxima página de resultados. Isto pode ser utilizado para a página usando o conjunto de resultados inteiro. Não é possível configurar o tamanho da página. 

Se estiver a StreamingPolicy criada ou eliminada enquanto a paginação por meio da coleção, as alterações são refletidas no resultados retornados (se essas alterações na parte da coleção que não foi transferida.) 

O exemplo do c# seguinte mostra como a enumeração por meio de todos os StreamingPolicies na conta.

```csharp
var firstPage = await MediaServicesArmClient.StreamingPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Para obter exemplos REST, consulte [políticas de transmissão em fluxo - lista](https://docs.microsoft.com/rest/api/media/streamingpolicies/list)

## <a name="next-steps"></a>Passos Seguintes

[Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)
