---
title: Migrar do indexador de vídeos do Azure API v1 para v2 | Documentos da Microsoft
description: Este tópico explica como migrar da API de indexador de vídeo do Azure do v1 para v2.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: b1737960a4142f5c0d949ce8c2524c34fe9cd79e
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41987565"
---
# <a name="migrate-from-the-video-indexer-api-v1-to-v2"></a>Migrar a partir do indexador de vídeos API v1 para v2

> [!Note]
> A API do Video Indexer V1 foi preterido no dia 1 de Agosto de 2018. Agora, deve utilizar a API do Video Indexer v2. <br/>Para programar com APIs do Video Indexer v2, veja as instruções presentes [aqui](https://api-portal.videoindexer.ai/). 

Este artigo descreve as alterações que foram introduzidas no v2.  

## <a name="api-changes"></a>Alterações à API

### <a name="authorization-and-operations"></a>Operações e de autorização

Na versão v2, o indexador de vídeo alterado o modelo de autenticação e autorização da API. Existem dois conjuntos de APIs: 

* Autorização 
* Operações

O **autorização** API é utilizada para obter os tokens de acesso para chamar os **operações** API. O **operações** API contém todos os vídeos indexador APIs, como vídeo de carregamento, recolha informações e outras operações.

Uma vez que [subscrever](video-indexer-get-started.md) para o **autorização** API, será capaz de obter os tokens de acesso ao transmitir a chave de subscrição (tal como fez no v1.)

## <a name="getting-and-using-the-access-token-for-operations-apis"></a>Obtenção e uso o token de acesso para operações de APIs

Ao chamar o **operações** APIs, a chave de subscrição já não serão usadas. Em vez disso, passará os tokens de acesso obtidos através da **autorização** API. 

Cada pedido deve ter um token válido, o nível de acesso da API está a chamar de correspondência. Por exemplo, operações em seu usuário, como obter as suas contas, exigem um token de acesso do utilizador. Operações na conta de nível, tais como a lista de todos os vídeos, exigem um token de acesso da conta. Operações nos vídeos, como vídeo reindex, exigem um token de acesso de conta ou um token de acesso de vídeo.

Para facilitar as coisas, pode usar **autorização** API > **GetAccounts** para obter as suas contas sem obtenção de um utilizador token pela primeira vez. Também pode fazer obter as contas com tokens válidos, permitindo-lhe ignorar uma chamada adicional para obter um token de conta.

Para obter mais informações sobre os tokens de acesso diferentes, consulte [utilizar a API de indexador de vídeo do Azure](video-indexer-use-apis.md).

### <a name="locations"></a>Localizações

Cada chamada à API deve incluir a localização da sua conta do Video Indexer. Chamadas de API, sem a localização ou com uma localização incorreta irão falhar.

Os valores descritos na tabela a seguir se aplicam. O **valor de parâmetro** é o valor que quando passa através da API.

|**Nome**|**Valor de parâmetro**|**Descrição**|
|---|---|---|
|Avaliação|Versão de avaliação|Utilizado para contas de avaliação. Por exemplo: https://api.videoindexer.ai/trial/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|EUA Oeste|westus2|Utilizado para a região do Azure E.u.a. oeste 2.  Por exemplo: https://api.videoindexer.ai/westus2/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Europa do Norte |europanorte|Utilizado para a região da Europa do Norte de Azure. Por exemplo: https://api.videoindexer.ai/northeurope/Accounts/{accountId}/Videos/{videoId}/Index?language=English. |
|Ásia Oriental|ásiaoriental|Utilizado para a região do Azure Ásia Oriental. Por exemplo: https://api.videoindexer.ai/eastasia/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|

### <a name="data-model"></a>Modelo de dados

Indexador de vídeos agora tem um modelo de dados simplificada para fornecer muito mais claras insights. Para obter mais informações sobre o resultado produzido pela v2 API, consulte [examinar a saída do indexador de vídeo produzida pela v2 API](video-indexer-output-json-v2.md).

### <a name="swagger"></a>Swagger

As definições de API do Video Indexer foram atualizadas de acordo e está disponíveis para transferência através da [portal de API](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token).


### <a name="v1-vs-v2-examples"></a>V1 vs exemplos V2

As novas APIs de V2 envolvem 3 parâmetros de principais:

1. [Local] - conforme descrito acima. A versão de avaliação, westus2, northeurope ou Sudeste asiático.
2. [YOUR_ACCOUNT_ID] - Guid de um id da sua conta. Obter ao obter todas as contas (descritas abaixo).
3. [YOUR_VIDEO_ID] - o id do seu vídeo (por exemplo, "d4fa369abc"). Devolvido ao carregar um vídeo ou ao procurar vídeos.

#### <a name="uploading-a-video-in-v1"></a>Carregar um vídeo no V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=some_name&description=some_description&privacy=private&videoUrl=http://URL_TO_YOUR_VIDEO
```

#### <a name="uploading-a-video-in-v2"></a>Carregar um vídeo no V2:

1. Obter um acesso de token para o pedido de carregamento:

  Pode obter todas as contas e os tokens de acesso:

    ```
    https://api.videoindexer.ai/auth/[LOCATION]/Accounts?generateAccessTokens=true&allowEdit=true
    ```

  Ou obtenha o token de acesso da conta específica:
  
  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/AccessToken?allowEdit=true
  ```
2. Carregar um vídeo:

  ```
  POST https://api.videoindexer.ai/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos?name=MySample&description=MySampleDescription&videoUrl=[URL_ENCODED_VIDEO_URL]&accessToken=eyJ0eXAiOiJ...
  ```

#### <a name="getting-insights-in-v1"></a>A obter informações no V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]
```
  
#### <a name="getting-insights-in-v2"></a>A obter informações no V2:

1. Utilize o token de acesso de conta ou obter um token de acesso de nível de vídeo:

  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/AccessToken
  ```
  
2. Obtenha informações:

  ```
  https://api.videoindexer.ai/[LOCATION]/Accounts[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

#### <a name="getting-video-processing-state-in-v1"></a>Ao obter o estado de processamento de vídeo no V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]/State
```
  
#### <a name="getting-video-processing-state-in-v2"></a>Ao obter o estado de processamento de vídeo no V2:

No v2 de API, o estado de processamento é devolvido como parte da API de índice do vídeo de introdução.

1. Utilize o token de acesso de conta ou obter um token de acesso de nível de vídeo:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```
  
2. Obtenha informações:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

## <a name="next-steps"></a>Passos Seguintes

[Utilizar a API do indexador de vídeo do Azure](video-indexer-use-apis.md)

