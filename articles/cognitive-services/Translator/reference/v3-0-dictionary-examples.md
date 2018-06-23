---
title: Método de exemplos de dicionário de API do Microsoft tradutor texto | Microsoft Docs
description: Utilize o método de exemplos de dicionário de API do Microsoft tradutor texto.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 9960f3be42090edaec1df935d70e4c1a0d25b691
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354979"
---
# <a name="text-api-30-dictionary-examples"></a>Texto API 3.0: Exemplos de dicionário

Fornece exemplos mostram como os termos no dicionário são utilizados no contexto. Esta operação é utilizada em conjunto com [pesquisa de dicionário](.\v3-0-dictionary-lookup.md).

## <a name="request-url"></a>URL do pedido

Enviar um `POST` pedido para:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
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
    <td>de</td>
    <td>*Necessário parâmetro*.<br/>Especifica a linguagem de texto de entrada. O idioma de origem tem de ser o [idiomas suportados](.\v3-0-languages.md) incluído no `dictionary` âmbito.</td>
  </tr>
  <tr>
    <td>para</td>
    <td>*Necessário parâmetro*.<br/>Especifica o idioma do texto de saída. O idioma de destino tem de ser o [idiomas suportados](.\v3-0-languages.md) incluído no `dictionary` âmbito.</td>
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
    <td>*Opcional*.<br/>Um GUID gerados pelo cliente para identificar exclusivamente o pedido. Pode omitir este cabeçalho se incluir o ID de rastreio na cadeia de consulta utilizando um parâmetro de consulta com o nome `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Corpo do pedido

O corpo do pedido é uma matriz JSON. Cada elemento de matriz é um objeto JSON com as seguintes propriedades:

  * `Text`: Uma cadeia que especifica o termo de pesquisa. Isto deve ter o valor de um `normalizedText` campo de back-traduções de anterior [pesquisa de dicionário](.\v3-0-dictionary-lookup.md) pedido. Também pode ser o valor da `normalizedSource` campo.

  * `Translation`: Uma cadeia que especifica o texto traduzido devolvido anteriormente pelo [pesquisa de dicionário](.\v3-0-dictionary-lookup.md) operação. Isto deve ter o valor da `normalizedTarget` campo no `translations` lista do [pesquisa de dicionário](.\v3-0-dictionary-lookup.md) resposta. O serviço irá devolver exemplos para o par de palavras de destino de origem específicos.

Um exemplo é:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

As seguintes limitações aplicam-se:

* A matriz pode ter no máximo 10 elementos.
* O valor de texto de um elemento de matriz não pode exceder os 100 carateres, incluindo espaços.

## <a name="response-body"></a>Corpo da resposta

Uma resposta com êxito é uma matriz JSON com um resultado para cada cadeia na matriz de entrada. Um objeto de resultado inclui as seguintes propriedades:

  * `normalizedSource`: Uma cadeia dá ao formulário normalizado do período da origem. Geralmente, este deve ser idêntico ao valor da `Text` campo no índice de lista correspondente no corpo do pedido.
    
  * `normalizedTarget`: Uma cadeia que lhe confere a forma normalizada do termo de destino. Geralmente, este deve ser idêntico ao valor da `Translation` campo no índice de lista correspondente no corpo do pedido.
  
  * `examples`: Uma lista de exemplos para a (prazo de origem, o termo de destino) par. Cada elemento da lista é um objeto com as seguintes propriedades:

    * `sourcePrefix`: A cadeia a concatenar _antes_ o valor de `sourceTerm` para formar um exemplo completo. Não adicione um caráter de espaço, uma vez que é já existir quando deve ser. Este valor pode ser uma cadeia vazia.

    * `sourceTerm`: Um igual de cadeia para o termo real é pesquisado. A cadeia é adicionada com `sourcePrefix` e `sourceSuffix` para formar exemplo completo. O valor é separado, para poder ser marcado numa interface de utilizador, por exemplo, por bolding-lo.

    * `sourceSuffix`: A cadeia a concatenar _depois_ o valor de `sourceTerm` para formar um exemplo completo. Não adicione um caráter de espaço, uma vez que é já existir quando deve ser. Este valor pode ser uma cadeia vazia.

    * `targetPrefix`: Uma cadeia semelhante `sourcePrefix` mas para o destino.

    * `targetTerm`: Uma cadeia semelhante `sourceTerm` mas para o destino.

    * `targetSuffix`: Uma cadeia semelhante `sourceSuffix` mas para o destino.

    > [!NOTE]
    > Se não houver nenhuma exemplos no dicionário, a resposta é 200 (OK), mas a `examples` lista é uma lista vazia.

## <a name="examples"></a>Exemplos

Este exemplo mostra como procurar exemplos do par constituídos pelo termo inglês `fly` e a tradução Espanhol `volar`.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

---

O corpo da resposta (abreviado para efeitos de clareza) é:

```
[
    {
        "normalizedSource":"fly",
        "normalizedTarget":"volar",
        "examples":[
            {
                "sourcePrefix":"They need machines to ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Necesitan máquinas para ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },      
            {
                "sourcePrefix":"That should really ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Eso realmente debe ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```
