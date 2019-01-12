---
title: Como adicionar blobs a objetos duplos Digital do Azure | Documentos da Microsoft
description: Saiba como adicionar blobs a objetos duplos Digital do Azure.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: 8b17d1ce4ae0b9c37f6ce8d64ecebd25c5c70db3
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231194"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Adicionar blobs a objetos duplos Digital do Azure

Os BLOBs são representações de dados não estruturadas de tipos de ficheiros comuns, como imagens e registos. BLOBs de controlam o tipo de dados que eles representam, utilizando um tipo de MIME (por exemplo: "imagem/jpeg") e metadados (nome, descrição, tipo e assim por diante).

Duplos Digital do Azure suporta anexar blobs para dispositivos, espaços e os utilizadores. BLOBs podem representar uma foto de perfil para um utilizador, uma fotografia de dispositivo, um vídeo, um mapa, um zip de firmware, dados JSON, um registo, etc.

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-an-overview"></a>Carregamento de blobs: uma descrição geral

Pode usar várias partes pedidos para carregar blobs para os pontos finais e suas respectivas funcionalidades.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>Metadados do blob

Para além **Content-Type** e **Content-Disposition**, pedidos de várias partes de blob de duplos Digital do Azure tem de especificar o corpo JSON correto. O corpo JSON para submeter depende do tipo de operação de pedido HTTP que está a ser efetuada.

Os esquemas JSON quatro principais são:

![Esquemas JSON][1]

A documentação de Swagger descreve esses esquemas de modelo detalhes.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Saiba como utilizar a documentação de referência, lendo [como utilizar o Swagger](./how-to-use-swagger.md).

### <a name="examples"></a>Exemplos

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Carregar um ficheiro de texto como um blob e associá-lo com um espaço, fazer um pedido de HTTP POST autenticado para:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs
```

Com o corpo do seguinte:

```plaintext
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

| Valor | Substituir |
| --- | --- |
| USER_DEFINED_BOUNDARY | Um nome de limites com várias partes de conteúdo |

O código a seguir é uma implementação de .NET do carregamento de BLOBs mesmo, usando a classe [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metadata in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

Nos dois exemplos:

1. Certifique-se de que os cabeçalhos incluem: `Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
1. Certifique-se de que o corpo é com várias partes:

   - A primeira parte contém os metadados de blob necessários.
   - A segunda parte contém o arquivo de texto.

1. Certifique-se de que o arquivo de texto é fornecido como `Content-Type: text/plain`.

## <a name="api-endpoints"></a>Pontos finais da API

As secções seguintes descrevem os principais relacionados com BLOBs pontos finais da API e suas funcionalidades.

### <a name="devices"></a>Dispositivos

Pode anexar blobs a dispositivos. A imagem seguinte mostra a documentação de referência do Swagger para as APIs de gestão. Especifica relacionados com o dispositivo pontos finais da API para consumo de blob e quaisquer parâmetros de caminho necessário passar neles.

![Blobs de dispositivo][2]

Por exemplo, para atualizar ou criar um blob e anexar o blob para um dispositivo, fazer um pedido de HTTP PATCH autenticado para:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parâmetro | Substituir |
| --- | --- |
| *YOUR_BLOB_ID* | O ID de blob pretendido |

Devolvem de pedidos com êxito uma **DeviceBlob** objeto JSON na resposta. **DeviceBlob** objetos estão em conformidade com o esquema JSON do seguinte:

| Atributo | Tipo | Descrição | Exemplos |
| --- | --- | --- | --- |
| **DeviceBlobType** | Cadeia | Uma categoria de BLOBs que pode ser anexada a um dispositivo | `Model` e `Specification` |
| **DeviceBlobSubtype** | Cadeia | Uma subcategoria de blob que seja mais específica que **DeviceBlobType** | `PhysicalModel`, `LogicalModel`, `KitSpecification`, e `FunctionalSpecification` |

> [!TIP]
> Utilize a tabela anterior para lidar com dados do pedido devolvido com êxito.

### <a name="spaces"></a>Espaços

Também pode anexar blobs aos espaços. A imagem seguinte apresenta uma lista de todos os espaço pontos finais da API responsáveis pelo tratamento de blobs. Ele também lista quaisquer parâmetros de caminho para passar para esses pontos de extremidade.

![Blobs de espaço][3]

Por exemplo, para devolver um blob anexado a um espaço, fazer um pedido HTTP GET autenticado para:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parâmetro | Substituir |
| --- | --- |
| *YOUR_BLOB_ID* | O ID de blob pretendido |

Um pedido de PATCH para o mesmo ponto final descrições de metadados de atualizações e cria novas versões do blob. O pedido HTTP é feito através do método PATCH, juntamente com todos os metadados necessários e dados de formulário com várias partes.

Operações bem-sucedidas retornam uma **SpaceBlob** objeto que está em conformidade com o esquema abaixo. Pode usá-lo para consumir dados retornados.

| Atributo | Tipo | Descrição | Exemplos |
| --- | --- | --- | --- |
| **SpaceBlobType** | Cadeia | Uma categoria de BLOBs que pode ser anexada a um espaço | `Map` e `Image` |
| **SpaceBlobSubtype** | Cadeia | Uma subcategoria de blob que seja mais específica que **SpaceBlobType** | `GenericMap`, `ElectricalMap`, `SatelliteMap`, e `WayfindingMap` |

### <a name="users"></a>Utilizadores

Pode anexar blobs a modelos de utilizador (por exemplo, para associar uma foto de perfil). A imagem seguinte mostra os pontos finais de API de utilizadores relevantes e quaisquer parâmetros de caminho necessário, como `id`:

![Blobs de utilizador][4]

Por exemplo, para obter um blob anexado a um utilizador, efetuar um pedido HTTP GET autenticado com quaisquer dados de formulário necessária para:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parâmetro | Substituir |
| --- | --- |
| *YOUR_BLOB_ID* | O ID de blob pretendido |

O JSON devolvido (**UserBlob** objetos) está em conformidade com os seguintes modelos JSON:

| Atributo | Tipo | Descrição | Exemplos |
| --- | --- | --- | --- |
| **UserBlobType** | Cadeia | Uma categoria de BLOBs que pode ser anexada a um utilizador | `Image` e `Video` |
| **UserBlobSubtype** |  Cadeia | Uma subcategoria de blob que seja mais específica que **UserBlobType** | `ProfessionalImage`, `VacationImage`, e `CommercialVideo` |

## <a name="common-errors"></a>Erros comuns

Um erro comum é para não incluir as informações de cabeçalho correto:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre a documentação de referência do Swagger duplos Digital do Azure, leia [Use Azure Digital duplos Swagger](how-to-use-swagger.md).

- Para carregar blobs através do Postman, leia [como configurar o Postman](./how-to-configure-postman.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
