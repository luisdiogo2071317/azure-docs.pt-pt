---
title: Como adicionar blobs a objetos duplos Digital do Azure | Documentos da Microsoft
description: Noções básicas sobre como adicionar blobs a objetos duplos Digital do Azure
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adgera
ms.openlocfilehash: 0929a4a63eee35d21723c980887d6b4217898682
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688164"
---
# <a name="how-to-add-blobs-to-objects-in-azure-digital-twins"></a>Como adicionar blobs a objetos duplos Digital do Azure

Os BLOBs são representações de dados não estruturadas de tipos de ficheiros comuns (como imagens e registos). BLOBs de manter o controle de que tipo de dados que eles representam através de um tipo MIME (por exemplo: "imagem/jpeg") e metadados (nome, descrição, tipo, etc.).

Duplos Digital do Azure suporta anexar Blobs para dispositivos, espaços e os utilizadores. BLOBs podem representar uma foto de perfil para um utilizador, uma fotografia de dispositivo, um vídeo, um mapa ou um registo.

> [!NOTE]
> Este artigo pressupõe que:
> * Que sua instância está corretamente configurada para receber pedidos de API de gestão.
> * Que corretamente autenticado utilizando um cliente REST da sua preferência.

## <a name="uploading-blobs-an-overview"></a>Carregamento de blobs: uma descrição geral

Pedidos com várias partes são utilizados para carregar Blobs para os pontos finais e suas respectivas funcionalidades.

> [!IMPORTANT]
> Pedidos de várias partes necessitam de três partes essenciais de informações. Duplos Digital do Azure:
> * R **Content-Type** cabeçalho:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
> * R **Content-Disposition**: `form-data; name="metadata"`.
> * O conteúdo do ficheiro para carregar.
>
> Exatamente **Content-Type** e **Content-Disposition** podem variar dependendo do cenário de utilização.

Cada solicitação com várias partes é dividida em vários segmentos. Com várias partes pedidos feitos para as APIs de gestão de duplos Digital do Azure são divididos em **dois** (**2**) partes:

1. A primeira parte é necessária e contém metadados de BLOBs como um tipo de MIME associado pela **Content-Type** e **Content-Disposition** acima.

1. A segunda parte contém os conteúdos do Blob reais (o conteúdo do arquivo de dados não estruturado).  

Nenhuma das duas partes são necessárias para **aplicar o PATCH** pedidos. Ambas são necessárias para **POST** ou operações de criação.

### <a name="blob-metadata"></a>Metadados do blob

Para além **Content-Type** e **Content-Disposition**, pedidos de várias partes também tem de especificar o corpo JSON correto. O corpo JSON para submeter depende do tipo de operação de pedido HTTP a ser efetuada.

Os quatro principais esquemas JSON utilizados são:

![Blobs de espaço][1]

Esses esquemas de modelo são descritas detalhadamente completa na documentação do Swagger fornecida.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Saiba como utilizar a documentação de referência fornecida lendo [como utilizar o Swagger](./how-to-use-swagger.md).

### <a name="examples"></a>Exemplos

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Para fazer um pedido POST que carrega um ficheiro de texto como um Blob e associa-a com um espaço:

```plaintext
POST YOUR_MANAGEMENT_API_URL/spaces/blobs HTTP/1.1
Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"

--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "ParentId": "54213cf5-285f-e611-80c3-000d3a320e1e",
  "Name": "My First Blob",
  "Type": "Map",
  "SubType": "GenericMap",
  "Description": "A well chosen description",
  "Sharing": "None"
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="myblob.txt"
Content-Type: text/plain

This is my blob content. In this case, some text, but I could also be uploading a picture, an JSON file, a firmware zip, etc.

--USER_DEFINED_BOUNDARY--
```

| Valor do parâmetro | Substituir |
| --- | --- |
| *USER_DEFINED_BOUNDARY* | Um nome de limites com várias partes de conteúdo |

Uma implementação de .NET de carregamento do mesmo Blob é apresentada abaixo usando a classe [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metaData in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

## <a name="api-endpoints"></a>Pontos finais da API

Abaixo, são fornecidas instruções de pontos finais de núcleos e suas funcionalidades específicas.

### <a name="devices"></a>Dispositivos

BLOBs podem ser anexados a dispositivos. A imagem abaixo (que mostram a documentação de referência do Swagger para as APIs de gestão) Especifica pontos finais da API relacionados com o dispositivo para consumo de Blob e quaisquer parâmetros de caminho necessário passar neles:

![Blobs de dispositivo][2]

Por exemplo, para atualizar ou criar um Blob e anexar o Blob para um dispositivo, é efetuado um pedido de PATCH para:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parâmetro | Substituir |
| --- | --- |
| *YOUR_BLOB_ID* | O ID de Blob pretendido |

Pedidos com êxito irão devolver um objeto JSON de DeviceBlob na resposta. DeviceBlobs está em conformidade com o esquema JSON do seguinte:

| Atributo | Tipo | Descrição | Exemplos |
| --- | --- | --- | --- |
| **DeviceBlobType** | Cadeia | Uma categoria de BLOBs que pode ser anexada a um dispositivo | `Model` e `Specification` |
| **DeviceBlobSubtype*** | Cadeia | Uma subcategoria de Blob que seja mais granular do que DeviceBlobType | `PhysicalModel`, `LogicalModel`, `KitSpecification`, e `FunctionalSpecification` |

> [!TIP]
> Utilize a tabela acima para lidar com dados do pedido devolvido com êxito.

### <a name="spaces"></a>Espaços

BLOBs também podem ser anexados a espaços. A imagem abaixo apresenta uma lista de todos os espaço pontos finais da API responsáveis por processar Blobs, juntamente com quaisquer parâmetros de caminho para passar neles:

![Blobs de espaço][3]

Por exemplo, para devolver um Blob anexado a um espaço, fazer um pedido GET para:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parâmetro | Substituir |
| --- | --- |
| *YOUR_BLOB_ID* | O ID de Blob pretendido |

Fazer o pedido de PATCH para o mesmo ponto final, poderá atualizar uma descrição de metadados e criar uma nova versão do Blob. O pedido HTTP é feito usando o método PATCH, juntamente com todos os metadados necessários e com várias partes de dados do formulário.

Operações com êxito irão devolver um SpaceBlob que está em conformidade com o esquema abaixo e pode ser usado para consumir dados devolvidos:

| Atributo | Tipo | Descrição | Exemplos |
| --- | --- | --- | --- |
| **SpaceBlobType** | Cadeia | Uma categoria de BLOBs que pode ser anexada a um espaço | `Map` e `Image` |
| **SpaceBlobSubtype** | Cadeia | Uma subcategoria de Blob que seja mais granular do que SpaceBlobType | `GenericMap`, `ElectricalMap`, `SatelliteMap`, e `WayfindingMap` |

### <a name="users"></a>Utilizadores

BLOBs podem ser anexados a modelos de utilizador (dizer associar uma foto de perfil). A imagem abaixo ilustra os pontos finais da API de utilizadores relevantes e quaisquer parâmetros de caminho necessário como uma `id`:

![Blobs de utilizador][4]

Por exemplo, para obter um Blob anexado a um utilizador, fazer uma solicitação GET com quaisquer dados de formulário necessárias para:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parâmetro | Substituir |
| --- | --- |
| *YOUR_BLOB_ID* | O ID de Blob pretendido |

JSON devolvido (UserBlobs) será está em conformidade com os seguintes modelos JSON:

| Atributo | Tipo | Descrição | Exemplos |
| --- | --- | --- | --- |
| **UserBlobType** | Cadeia | Uma categoria de BLOBs que pode ser anexada a um utilizador | `Image` e `Video` |
| **UserBlobSubtype** |  Cadeia | Uma subcategoria de Blob que seja mais granular do que UserBlobType | `ProfessionalImage`, `VacationImage`, e `CommercialVideo` |

## <a name="common-errors"></a>Erros comuns

Não, incluindo as informações de cabeçalho correto:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a documentação de referência do Azure Digital duplos Swagger fornecida, leia [como utilizar o Swagger de duplos Digital](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
