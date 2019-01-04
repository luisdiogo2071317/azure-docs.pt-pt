---
title: Ativos nos serviços de multimédia - Azure | Documentos da Microsoft
description: Este artigo fornece uma explicação sobre o que estão ativos, e como elas são usadas pelos serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/01/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 8507d51f0d4d49d89fc24b38ed73df7488261daa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969580"
---
# <a name="assets"></a>Elementos

Nos serviços de multimédia do Azure, um [Asset](https://docs.microsoft.com/rest/api/media/assets) contém ficheiros digitais (incluindo o vídeo, áudio, imagens, coleções de miniaturas, pistas de texto e ficheiros de legendas) e os metadados relativos a esses ficheiros. Depois dos ficheiros digitais são carregados para um elemento, eles podem ser usados nos serviços de multimédia de codificação, transmissão em fluxo, analisar fluxos de trabalho de conteúdo. Para obter mais informações, consulte a [ficheiros digitais são carregados nos ativos](#upload-digital-files-into-assets) secção abaixo.

Um elemento é mapeado para um contentor de BLOBs no [conta de armazenamento do Azure](storage-account-concept.md) e os ficheiros no elemento são armazenados como blobs de blocos nesse contentor. Serviços de multimédia suportam as camadas de Blob, quando a conta utiliza para fins gerais v2 (GPv2) de armazenamento. Com a GPv2, pode mover arquivos para [acesso esporádico ou arquivo armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). **Arquivo** armazenamento é adequado para arquivamento de arquivos de origem quando já não for necessário (por exemplo, depois de ter sido codificados).

O **arquivo** camada de armazenamento só é recomendada para ficheiros de origem muito grandes que já tenham sido codificados e o resultado da tarefa de codificação foi colocado num contêiner de blob de saída. Os blobs no contentor de saída que deseja associar um recurso e a utilização para transmitir em fluxo ou analisar os seus conteúdos, tem de existir uma **frequente** ou **esporádico** camada de armazenamento.

## <a name="asset-definition"></a>Definição do recurso

A tabela seguinte mostra as propriedades do recurso e fornece as respetivas definições.

|Nome|Descrição|
|---|---|
|ID|ID de recurso completamente qualificado para o recurso.|
|nome|O nome do recurso.|
|properties.alternateId |O ID alternativo do ativo.|
|properties.assetId |O ID de recurso.|
|Properties.Container |O nome do contentor de BLOBs de recurso.|
|Properties.created |A data de criação do recurso.<br/> DateTime é sempre em formato UTC.|
|Properties.Description|A descrição do recurso.|
|properties.lastModified |A data última modificação do recurso. <br/> DateTime é sempre em formato UTC.|
|properties.storageAccountName |O nome da conta de armazenamento.|
|properties.storageEncryptionFormat |O formato de encriptação ativo. Um dos None ou MediaStorageEncryption.|
|tipo|O tipo de recurso.|

Para obter uma definição completa, consulte [ativos](https://docs.microsoft.com/rest/api/media/assets).

## <a name="upload-digital-files-into-assets"></a>Ficheiros digitais são carregados nos ativos

Um dos fluxos de trabalho comuns dos serviços de multimédia é carregar, codificar e transmitir um ficheiro. Esta seção descreve os passos gerais.

1. Utilize a API dos serviços de multimédia v3 para criar um novo elemento de "entrado". Esta operação cria um contentor na conta de armazenamento associada à conta de Media Services. A API devolve o nome do contentor (por exemplo, `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Se já tiver um contentor de BLOBs que pretende associar um recurso, pode especificar o nome do contentor ao criar o recurso. Serviços de multimédia atualmente suporta apenas blobs na raiz do contentor e não com caminhos no nome do ficheiro. Portanto, um contentor com o nome de ficheiro "input.mp4" irá funcionar. No entanto, um contentor com o nome de ficheiro "videos/inputs/input.mp4", não funcionará.

    Pode utilizar a CLI do Azure para carregar diretamente para qualquer conta de armazenamento e o contentor que tem direitos para na sua subscrição. <br/>O nome do contentor tem de ser exclusivo e siga as diretrizes de nomenclatura de armazenamento. O nome não tem de seguir o nome do contentor de elemento de serviços de multimédia (GUID de recurso) de formatação. 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Obter um URL de SAS com permissões de leitura / escrita que serão utilizadas para ficheiros digitais são carregados para o contentor de elemento. Pode utilizar a API de serviços de multimédia para [lista os URLs do contentor de elemento](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Utilize as APIs de armazenamento do Azure ou SDKs (por exemplo, o [API do REST de armazenamento](../../storage/common/storage-rest-api-auth.md), [SDK de JAVA](../../storage/blobs/storage-quickstart-blobs-java-v10.md), ou [SDK de .NET](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) para carregar ficheiros para o contentor de elemento. 
4. Utilize serviços de multimédia v3 APIs para criar uma transformação e uma tarefa para processar o seu elemento "entrado". Para obter mais informações, consulte [transforma e tarefas](transform-concept.md).
5. Stream o conteúdo do elemento de "saída".

> [!TIP]
> Para obter um exemplo de .NET completo que mostra como: criar o elemento, obter um URL de SAS gravável para contentor do recurso no armazenamento, carregue o ficheiro para o contentor no armazenamento com o URL de SAS, consulte [criar uma entrada da tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Criar um novo elemento

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

Para obter um exemplo do REST, veja a [criar um elemento com REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) exemplo.

O exemplo mostra como criar os **corpo do pedido** onde pode especificar informações úteis, como a descrição, nome do contentor, conta de armazenamento e outras informações.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Para obter um exemplo completo, veja [criar uma entrada da tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md). Em serviços de multimédia v3, entrada de um trabalho também pode ser criada a partir de URLs de HTTPS (veja [criar uma entrada da tarefa a partir de um URL HTTPS](job-input-from-http-how-to.md)).

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
|nome|Suporta: Eq, Gt, Lt|Suporta: Ascendente e descendente|
|properties.alternateId |Suporta: EQ||
|properties.assetId |Suporta: EQ||
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

Se os recursos são criados ou eliminados enquanto a paginação por meio da coleção, as alterações são refletidas em resultados devolvidos (se essas alterações na parte da coleção que não foi transferida). 

#### <a name="c-example"></a>Exemplo do c#

O exemplo do c# seguinte mostra como a enumeração por meio de todos os recursos na conta.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

#### <a name="rest-example"></a>Exemplo do REST

Considere o seguinte exemplo de onde $skiptoken é utilizado. Certifique-se de que substitua *amstestaccount* com o nome da conta e o conjunto a *api-version* valor para a versão mais recente.

Se solicitar uma lista de recursos, como este:

```
GET  https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01 HTTP/1.1
x-ms-client-request-id: dd57fe5d-f3be-4724-8553-4ceb1dbe5aab
Content-Type: application/json; charset=utf-8
```

Obteria volta uma resposta semelhante ao seguinte:

```
HTTP/1.1 200 OK
 
{
"value":[
{
"name":"Asset 0","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 0","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-5a4f-470a-9d81-6037d7c23eff","created":"2018-12-11T22:12:44.98Z","lastModified":"2018-12-11T22:15:48.003Z","container":"asset-98d07299-5a4f-470a-9d81-6037d7c23eff","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
},
// lots more assets
{
"name":"Asset 517","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 517","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-912e-447b-a1ed-0f723913b20d","created":"2018-12-11T22:14:08.473Z","lastModified":"2018-12-11T22:19:29.657Z","container":"asset-fd05a503-912e-447b-a1ed-0f723913b20d","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
}
],"@odata.nextLink":"https:// management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517"
}
```

Em seguida, poderia solicitar a página seguinte, enviando uma solicitação get:

```
https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517
```

Para obter mais exemplos REST, consulte [ativos - lista](https://docs.microsoft.com/rest/api/media/assets/list)

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

[Diferenças entre o suporte de dados de serviços v2 e v3](migrate-from-v2-to-v3.md)
