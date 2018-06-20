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
ms.openlocfilehash: 541a8e83029fe1dc0ba386d1906b366e63041882
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268242"
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
|nome|||
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

Se uma resposta de consulta contém muitas (atualmente através de 1000) itens, o serviço devolve um "\@odata.nextLink" propriedade para obter a seguinte página de resultados. Isto pode ser utilizado para a página através do conjunto de resultados de todo. O tamanho de página não é configurável pelo utilizador. 

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


### <a name="storage-side-encryption"></a>Encriptação do lado do armazenamento

Para proteger os seus recursos inativos, os ativos devem ser encriptados pela encriptação do lado do armazenamento. A tabela seguinte mostra como a encriptação do lado do armazenamento funciona nos Media Services:

|opção de encriptação|Descrição|Serviços de Multimédia v2|Serviços de Multimédia v3|
|---|---|---|---|
|Encriptação de armazenamento de serviços de multimédia|AES 256 encriptação, chave gerida pelos Media Services|Suportado<sup>(1)</sup>|Não suportado<sup>(2)</sup>|
|[Encriptação do serviço de armazenamento para dados Inativos](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Chave de encriptação do lado do servidor oferecidas pelo armazenamento do Azure, gerida pelo Azure ou pelo cliente|Suportadas|Suportadas|
|[Encriptação do lado do cliente do Storage](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Oferecidas pelo armazenamento do Azure, chave gerido pelo cliente no Cofre de chaves de encriptação do lado do cliente|Não suportado|Não suportado|

<sup>1</sup> enquanto os Media Services suporta o processamento do conteúdo na limpar/sem qualquer forma de encriptação, fazê-lo, por isso, não é recomendado.

<sup>2</sup> v3 de Media Services, a encriptação de armazenamento (encriptação AES 256) só é suportada para em efeitos de compatibilidade quando os elementos criados com os Media Services v2. Significado v3 funciona com o armazenamento existente encriptados ativos, mas não permitirá a criação de novos.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)
