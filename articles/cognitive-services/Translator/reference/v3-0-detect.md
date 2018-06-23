---
title: Microsoft tradutor texto API detetar método | Microsoft Docs
description: Utilize o método Microsoft tradutor texto API detetar.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 7e81e91230e1ada4423d77d22134b1b64df65d9d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354973"
---
# <a name="text-api-30-detect"></a>Texto API 3.0: detetar

Identifica o idioma de um fragmento de texto.

## <a name="request-url"></a>URL do pedido

Enviar um `POST` pedido para:

```HTTP
https://api.cognitive.microsofttranslator.com/detect?api-version=3.0
```

## <a name="request-parameters"></a>Parâmetros do pedido

Os parâmetros requeridos transmitidos a cadeia de consulta são:

<table width="100%">
  <th width="20%">Parâmetro de consulta</th>
  <th>Descrição</th>
  <tr>
    <td>versão de API</td>
    <td>*Necessário parâmetro*.<br/>Versão da API solicitada pelo cliente. Valor tem de ser `3.0`.</td>
  </tr>
</table> 

Cabeçalhos de pedido incluem:

<table width="100%">
  <th width="20%">Cabeçalhos</th>
  <th>Descrição</th>
  <tr>
    <td>_Uma autorização_<br/>_Cabeçalho_</td>
    <td>*Cabeçalho de pedido necessários*.<br/>Consulte [as opções disponíveis para autenticação](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*Cabeçalho de pedido necessários*.<br/>Especifica o tipo de conteúdo do payload. Os valores possíveis são: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Cabeçalho de pedido necessários*.<br/>O comprimento do corpo do pedido.</td>
  </tr>
  <tr>
    <td>X ClientTraceId</td>
    <td>*Opcional*.<br/>Um GUID gerados pelo cliente para identificar exclusivamente o pedido. Tenha em atenção que pode omitir este cabeçalho se incluir o ID de rastreio na cadeia de consulta utilizando um parâmetro de consulta com o nome `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Corpo do pedido

O corpo do pedido é uma matriz JSON. Cada elemento de matriz é um objeto JSON com uma propriedade de cadeia denominado `Text`. Deteção de idioma é aplicada ao valor da `Text` propriedade. Um corpo do pedido de exemplo procura assim o desejar que:

```json
[
    { "Text": "Ich würde wirklich gern Ihr Auto um den Block fahren ein paar Mal." }
]
```

As seguintes limitações aplicam-se:

* A matriz pode ter no máximo de 100 elementos.
* O valor de texto de um elemento de matriz não pode exceder a 10.000 carateres, incluindo espaços.
* O texto completo incluído no pedido de não pode ter mais de 50.000 carateres, incluindo espaços.

## <a name="response-body"></a>Corpo da resposta

Uma resposta com êxito é uma matriz JSON com um resultado para cada cadeia na matriz de entrada. Um objeto de resultado inclui as seguintes propriedades:

  * `language`: Código de idioma detetado.

  * `score`: Um valor de vírgula flutuante que indica a confiança no resultado. A classificação é entre zero e um e uma pontuação baixa indica uma confiança baixa.

  * `isTranslationSupported`: Um valor booleano que é verdadeiro se o idioma detetado é um dos idiomas suportados para a conversão de texto.

  * `isTransliterationSupported`: Um valor booleano que é verdadeiro se o idioma detetado é um dos idiomas suportados para transliteration.
  
  * `alternatives`: Uma matriz de outros idiomas possíveis. Cada elemento da matriz é um outro objeto com as mesmas propriedades listado acima: `language`, `score`, `isTranslationSupported` e `isTransliterationSupported`.

Um resposta JSON de exemplo é:

```json
[
  {
    "language": "de",
    "score": 0.92,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 0.23,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "sk",
        "score": 0.23,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="response-headers"></a>Cabeçalhos de resposta

<table width="100%">
  <th width="20%">Cabeçalhos</th>
  <th>Descrição</th>
  <tr>
    <td>X-RequestId</td>
    <td>Valor gerado pelo serviço para identificar o pedido. É utilizado para fins de resolução de problemas.</td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Códigos de estado de resposta

Seguem-se os códigos de estado HTTP possíveis que devolve um pedido. 

<table width="100%">
  <th width="20%">Código de Estado</th>
  <th>Descrição</th>
  <tr>
    <td>200</td>
    <td>Êxito.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Um dos parâmetros de consulta está em falta ou não é válido. Corrija os parâmetros do pedido.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>Não foi possível autenticar o pedido. Verifique se as credenciais estão especificado e válido.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>O pedido não está autorizado. Verifique os detalhes da mensagem de erro. Isto indica frequentemente que foram utilizadas todas as traduções livres fornecidas com uma subscrição de avaliação cópias de segurança.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>O chamador está a enviar demasiados pedidos.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Ocorreu um erro inesperado. Se o erro persistir, comunique com: data e hora da falha, identificador de pedido de cabeçalho de resposta `X-RequestId`e o identificador de cliente de cabeçalho de pedido `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Servidor temporariamente indisponível. Repita o pedido. Se o erro persistir, comunique com: data e hora da falha, identificador de pedido de cabeçalho de resposta `X-RequestId`e o identificador de cliente de cabeçalho de pedido `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Exemplos

O exemplo seguinte mostra como obter os idiomas suportados para a conversão de texto.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/detect?api-version=3.0" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'What language is this text written in?'}]"
```

---
