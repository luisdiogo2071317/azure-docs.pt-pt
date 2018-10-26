---
title: Conteúdo de diretivas de chave nos serviços de multimédia do Azure | Documentos da Microsoft
description: Este artigo fornece uma explicação das quais são as diretivas de chave de conteúdo e como elas são usadas pelos serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: 24597c36ab6569abb53c7be331631556e2abfe66
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085962"
---
# <a name="content-key-policies"></a>Políticas de Chave de Conteúdo

Pode utilizar os serviços de multimédia do Azure para proteger os seus suportes de dados a partir do momento em que deixa seu computador por meio de armazenamento, processamento e entrega. Com os serviços de multimédia, pode fornecer seu conteúdo ao vivo e sob demanda dinamicamente encriptado com o Advanced Encryption Standard (AES-128) ou qualquer um dos sistemas de gestão (DRM) três direitos digitais principais: Microsoft PlayReady, Widevine da Google e Apple FairPlay. Serviços de multimédia também fornecem um serviço para entrega de chaves AES e o DRM (PlayReady, Widevine e FairPlay) licenças para os clientes autorizados.

Em serviços de multimédia do Azure v3, políticas de chaves de conteúdo permitem que especifique a forma como a chave de conteúdo é entregue ao fim de clientes via o componente de entrega de chave de serviços de multimédia. Para obter mais informações, consulte [descrição geral da proteção de conteúdo](content-protection-overview.md).

## <a name="contentkeypolicies-definition"></a>Definição de ContentKeyPolicies

A tabela seguinte mostra as propriedades do ContentKeyPolicy e fornece as respetivas definições.

|Nome|Descrição|
|---|---|
|ID|ID de recurso completamente qualificado para o recurso.|
|nome|O nome do recurso.|
|Properties.created |A data de criação da política|
|Properties.Description |Uma descrição para a política.|
|properties.lastModified|Data da última modificação da política|
|Properties.Options |As opções de política de chave.|
|properties.policyId    |O ID de política de legado.|
|tipo   |O tipo de recurso.|

A definição completa, consulte [diretivas de chave de conteúdo](https://docs.microsoft.com/rest/api/media/contentkeypolicies).

## <a name="filtering-ordering-paging"></a>Paginação de filtragem, ordenação,

Serviços de multimédia suportam as seguintes opções de consulta de OData para ContentKeyPolicies: 

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
|Properties.created |Eq, ne, ge, le, gt, lt|Ascendente e descendente|
|Properties.Description |Eq, ne, ge, le, gt, lt||
|properties.lastModified    |Eq, ne, ge, le, gt, lt|Ascendente e descendente|
|Properties.Options |||
|properties.policyId    |Eq, ne||
|tipo   |||

### <a name="pagination"></a>Paginação

Paginação é suportada para cada uma das quatro ordens de classificação ativado. Atualmente, o tamanho da página é 10.

> [!TIP]
> Deve sempre utilizar a ligação seguinte enumere a coleção e não dependem de um tamanho de página em particular.

Se uma resposta de consulta contém o número de itens, o serviço retornará um "\@OData. NextLink" propriedade para obter a próxima página de resultados. Isto pode ser utilizado para a página usando o conjunto de resultados inteiro. Não é possível configurar o tamanho da página. 

Se estiver a StreamingPolicy criada ou eliminada enquanto a paginação por meio da coleção, as alterações são refletidas no resultados retornados (se essas alterações na parte da coleção que não foi transferida.) 

O exemplo do c# seguinte mostra como a enumeração por meio de todos os ContentKeyPolicies na conta.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Para obter exemplos REST, consulte [diretivas de chave de conteúdo - lista](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)

## <a name="next-steps"></a>Passos Seguintes

[Utilizar a encriptação dinâmica de AES-128 e o serviço de entrega de chave](protect-with-aes128.md)

[Utilizar DRM dinâmico licença e de encriptação de serviço de entrega](protect-with-drm.md)
