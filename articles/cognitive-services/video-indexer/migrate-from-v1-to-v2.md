---
title: Migrar do indexador de vídeo do Azure API v1 para v2 | Microsoft Docs
description: Este tópico explica como migrar a API do Azure vídeo indexador v1 para v2.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 05/13/2018
ms.author: juliako
ms.openlocfilehash: 00f5a0d7c9a37c24866cd5e1259800d5e431ccc3
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356472"
---
# <a name="migrate-from-the-video-indexer-api-v1-to-v2"></a>Migrar as vídeo indexador API v1 para v2

> [!Note]
> As APIs do vídeo indexador V1 estão agora preteridas e serão removidas no 1ª de Agosto de 2018. Deve começar a utilizar as APIs do indexador de vídeo v2 para evitar interrupções.
>
> Para desenvolver com APIs de v2 do indexador de vídeo, consulte as instruções que se encontram [aqui](https://api-portal.videoindexer.ai/). 

Este artigo descreve as alterações que foram introduzidas no v2.  

## <a name="api-changes"></a>Alterações de API

### <a name="authorization-and-operations"></a>Operações e autorização

Na versão v2, o indexador de vídeo alterar o modelo de autenticação e autorização da API. Existem dois conjuntos de APIs: 

* Autorização 
* Operações

O **autorização** API é utilizada para obter os tokens de acesso para chamar o **operações** API. O **operações** API contém todas as vídeo indexador APIs, tais como as vídeo de carregamento, Get insights e outras operações.

Uma vez que [subscrever](video-indexer-get-started.md) para o **autorização** API, será capaz de obter os tokens de acesso ao transferir a chave de subscrição (tal como fez no v1.)

## <a name="getting-and-using-the-access-token-for-operations-apis"></a>Obter e utilizar o token de acesso para APIs de operações

Quando chamar o **operações** APIs, a chave de subscrição já não ser utilizadas. Em vez disso, irá passar os tokens de acesso obtidos através do **autorização** API. 

Cada pedido deve ter um token válido, o nível de acesso da API são chamar de correspondência. Por exemplo, as operações do utilizador, como obter as suas contas, exigem um token de acesso de utilizador. Operações na conta de nível, tais como a lista de todos os vídeos, necessitam de um token de acesso da conta. Operações de vídeos, tais como as vídeo reindex, necessitam de um token de acesso de conta ou um token de acesso de vídeo.

Para facilitar as coisas, pode utilizar **autorização** API > **GetAccounts** para obter as contas sem obtenção de um utilizador token primeiro. Pode também colocar obter as contas com tokens válidos, permitindo-lhe ignorar uma chamada adicional para obter um token de conta.

Para obter mais informações sobre os tokens de acesso diferentes, consulte [utilização do Azure vídeo indexador API](video-indexer-use-apis.md).

### <a name="locations"></a>Localizações

Cada chamada à API deve incluir a localização da sua conta do indexador de vídeo. Chamadas de API sem a localização ou com uma localização incorreta irão falhar.

Os valores descritos na seguinte tabela aplicam-se. O **Param valor** é o valor quando passa através da API.

|**Nome**|**Valor de param**|**Descrição**|
|---|---|---|
|Avaliação|Versão de avaliação|Utilizado para contas de avaliação. Por exemplo: https://api.videoindexer.ai/trial/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|EUA Oeste|westus2|Utilizado para a região do Azure EUA oeste 2.  Por exemplo: https://api.videoindexer.ai/westus2/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Europa do Norte |europanorte|Utilizado para a região Europa do Norte de Azure. Por exemplo: https://api.videoindexer.ai/northeurope/Accounts/{accountId}/Videos/{videoId}/Index?language=English. |
|Ásia Oriental|ásiaoriental|Utilizado para a região do Azure Ásia Oriental. Por exemplo: https://api.videoindexer.ai/eastasia/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|

### <a name="data-model"></a>Modelo de dados

Indexador de vídeo tem agora um modelo de dados simplificada para disponibilizar muito insights mais clara. Para obter mais informações sobre a saída produzida pela v2 API, consulte [Examine a saída de vídeo indexador produzida pela v2 API](video-indexer-output-json-v2.md).

### <a name="swagger"></a>Swagger

As definições de vídeo indexador API foram atualizadas em conformidade e estão disponíveis para transferir através da [API portal](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token).


### <a name="v1-vs-v2-examples"></a>Vs V1 exemplos V2

As novas APIs do V2 envolvem 3 parâmetros principais:

1. [Localização] - conforme descrito acima. A versão de avaliação, westus2, northeurope ou eastasia.
2. [YOUR_ACCOUNT_ID] - Guid de uma id da sua conta. Obter obter todas as contas (descritas abaixo).
3. [YOUR_VIDEO_ID] - o id do vídeo (por exemplo, "d4fa369abc"). Devolvido quando carregar um vídeo ou quando procurar vídeos.

#### <a name="uploading-a-video-in-v1"></a>Carregar um vídeo no V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=some_name&description=some_description&privacy=private&videoUrl=http://URL_TO_YOUR_VIDEO
```

#### <a name="uploading-a-video-in-v2"></a>Carregar um vídeo na V2:

1. Obter um acesso token para o pedido de carregamento:

  Pode optar por obter todas as contas e os tokens de acesso:

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

#### <a name="getting-insights-in-v1"></a>Obter o insights na V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]
```
  
#### <a name="getting-insights-in-v2"></a>Obter o insights na V2:

1. Utilize o token de acesso de conta ou obter um token de acesso de nível de vídeo:

  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/AccessToken
  ```
  
2. Obter conhecimentos aprofundados:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

#### <a name="getting-video-processing-state-in-v1"></a>Ao obter o estado de processamento de vídeo no V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]/State
```
  
#### <a name="getting-video-processing-state-in-v2"></a>Ao obter o estado de processamento de vídeo no V2:

Na API v2, o estado de processamento é devolvido como parte da API para obter o índice de vídeo.

1. Utilize o token de acesso de conta ou obter um token de acesso de nível de vídeo:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```
  
2. Obter conhecimentos aprofundados:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

## <a name="next-steps"></a>Passos Seguintes

[Utilize o indexador de vídeo do Azure API](video-indexer-use-apis.md)

