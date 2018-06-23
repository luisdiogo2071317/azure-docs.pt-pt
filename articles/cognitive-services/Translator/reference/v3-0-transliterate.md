---
title: Microsoft tradutor texto API Transliterate método | Microsoft Docs
description: Utilize o método Transliterate de API do Microsoft tradutor texto.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: fdd6fa9236f0c02685198b6de3228c444993dad6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354968"
---
# <a name="text-api-30-transliterate"></a>Texto API 3.0: Transliterate

Converte texto num idioma a partir de um script para outro script.

## <a name="request-url"></a>URL do pedido

Enviar um `POST` pedido para:

```HTTP
https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0
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
  <tr>
    <td>Idioma</td>
    <td>*Necessário parâmetro*.<br/>Especifica o idioma do texto para converter a partir de um script. Idiomas possíveis estão listados no `transliteration` âmbito obtido ao consultar o serviço para o respetivo [idiomas suportados](.\v3-0-languages.md).</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Necessário parâmetro*.<br/>Especifica o script utilizado pelo texto de entrada. Pesquisa [idiomas suportados](.\v3-0-languages.md) utilizando o `transliteration` âmbito, para localizar a entrada scripts disponíveis para o idioma seleccionado.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Necessário parâmetro*.<br/>Especifica o script de saída. Pesquisa [idiomas suportados](.\v3-0-languages.md) utilizando o `transliteration` âmbito, para localizar os scripts de saída disponível para a combinação de idioma selecionada e o script de entrada.</td>
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

O corpo do pedido é uma matriz JSON. Cada elemento de matriz é um objeto JSON com uma propriedade de cadeia denominado `Text`, que representa a cadeia a converter.

```json
[
    {"Text":"こんにちは"},
    {"Text":"さようなら"}
]
```

As seguintes limitações aplicam-se:

* A matriz pode ter no máximo 10 elementos.
* O valor de texto de um elemento de matriz não pode exceder 1000 caracteres, incluindo espaços.
* O texto completo incluído no pedido não pode exceder 5000 carateres, incluindo espaços.

## <a name="response-body"></a>Corpo da resposta

Uma resposta com êxito é uma matriz JSON com um resultado de cada elemento da matriz de entrada. Um objeto de resultado inclui as seguintes propriedades:

  * `text`: Uma cadeia que é o resultado da conversão de cadeia de entrada para o script de saída.
  
  * `script`: Uma cadeia que especifica o script utilizado na saída.

Um resposta JSON de exemplo é:

```json
[
    {"text":"konnnichiha","script":"Latn"},
    {"text":"sayounara","script":"Latn"}
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

O exemplo seguinte mostra como converter duas cadeias japonês Romanized japonês.

# <a name="curltabcurl"></a>[curl](#tab/curl)

O payload JSON para o pedido neste exemplo:

```
[{"text":"こんにちは","script":"jpan"},{"text":"さようなら","script":"jpan"}]
```

Se estiver a utilizar cUrl numa janela da linha de comandos que não suporta carateres Unicode, tome o seguinte payload JSON e guarde-o num ficheiro denominado `request.txt`. Certifique-se guardar o ficheiro com `UTF-8` codificação.

```
curl -X POST "https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0&language=ja&fromScript=Jpan&toScript=Latn" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d @request.txt
```

---
