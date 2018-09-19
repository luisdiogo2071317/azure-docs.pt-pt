---
title: Referência V3.0 de API de texto do tradutor
titlesuffix: Azure Cognitive Services
description: Documentação de referência para o V3.0 de API de texto do Translator.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 9282d8af30cbfb3346394bcd71510faf8d8c8a21
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129391"
---
# <a name="translator-text-api-v30"></a>V3.0 de API de texto do tradutor

## <a name="whats-new"></a>Novidades

A versão 3 da API de texto do Translator fornece uma API da Web baseados em JSON modernos. Melhora a capacidade de utilização e desempenho, a consolidação de recursos existentes em menos operações e fornece novos recursos.

 * Transliteração converter texto num idioma de um script para outro script.
 * Tradução para vários idiomas numa solicitação.
 * Deteção de idioma, tradução e Transliteração numa solicitação.
 * Dicionário para traduções alternativas de pesquisa de um período, para encontrar back traduções e exemplos que mostram os termos utilizados no contexto.
 * Resultados de deteção de idioma mais informativos.

## <a name="base-urls"></a>Base URLs

Texto API v3.0 está disponível na nuvem seguinte:

| Descrição | Região | URL Base                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Global | API.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Autenticação

Subscrever a API de texto do Translator nos serviços cognitivos da Microsoft e utilizar a sua chave de subscrição (disponível no portal do Azure) para autenticar. 

A forma mais simples é passar a chave secreta do Azure para o serviço de Microsoft Translator usando o cabeçalho do pedido `Ocp-Apim-Subscription-Key`.

Uma alternativa é usar a chave secreta para obter um token de autorização do serviço de tokens de. Em seguida, passa o token de autorização para o serviço Microsoft Translator, com o `Authorization` cabeçalho do pedido. Para obter um token de autorização, fazer um `POST` pedido para o URL seguinte:

| Ambiente     | URL do serviço de autenticação                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Seguem-se os pedidos de exemplo para obter um token dado uma chave secreta:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Um pedido com êxito devolve o token de acesso codificada como texto simples no corpo da resposta. O token válido é passado para o serviço Microsoft Translator, como um token de portador a autorização.

```
Authorization: Bearer <Base64-access_token>
```

Um token de autenticação é válido durante 10 minutos. O token deve ser reutilizado ao fazer várias chamadas para as APIs de Microsoft Translator. No entanto, se o seu programa faz solicitações para a API do Translator ao longo de um longo período de tempo, em seguida, seu programa tem de pedir um novo token de acesso em intervalos regulares (por exemplo, a cada 8 minutos).

Para resumir, um pedido de cliente para a API do Translator irá incluir um cabeçalho de autorização obtido da tabela a seguir:

<table width="100%">
  <th width="30%">Cabeçalhos</th>
  <th>Descrição</th>
  <tr>
    <td>OCP-Apim-Subscription-Key</td>
    <td>*Utilizar com a subscrição de serviços cognitivos, se estiver passando a chave secreta*.<br/>O valor é a chave secreta do Azure para a sua subscrição para a API de texto do Translator.</td>
  </tr>
  <tr>
    <td>Autorização</td>
    <td>*Utilizar com subscrição dos serviços cognitivos, se estiver passando um token de autenticação.*<br/>O valor é o token de portador: "portador <token>'.</td>
  </tr>
</table> 

## <a name="errors"></a>Erros

Uma resposta de erro padrão é um objeto JSON com o par nome/valor com o nome `error`. O valor também é um objeto JSON com propriedades:

  * `code`: Um código de erro definidas pelo servidor.

  * `message`: Uma cadeia de caracteres fornecendo uma representação legível do erro.

Por exemplo, um cliente com uma subscrição de avaliação gratuita iria receber o erro seguinte assim que a quota gratuita está esgotada:

```
{
  "error": {
    "code":403000,
    "message":"The subscription has exceeded its free quota."
    }
}
```
