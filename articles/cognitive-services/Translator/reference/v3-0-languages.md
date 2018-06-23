---
title: Método de idiomas de texto API do Microsoft tradutor | Microsoft Docs
description: Utilize o método de idiomas de API do Microsoft tradutor texto.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 93c06218a560faf439f05903438d021b372ce257
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354157"
---
# <a name="text-api-30-languages"></a>Texto API 3.0: idiomas

Obtém o conjunto de idiomas atualmente suportados por outras operações da API de texto. 

## <a name="request-url"></a>URL do pedido

Enviar um `GET` pedido para:
```HTTP
https://api.cognitive.microsofttranslator.com/languages?api-version=3.0
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
    <td>scope</td>
    <td>*O parâmetro opcional*.<br/>Uma lista separada por vírgulas de nomes de definir o grupo de idiomas para devolver. Permitidos são os nomes de grupo: `translation`, `transliteration` e `dictionary`. Se nenhum âmbito for especificado, em seguida, são devolvidos todos os grupos, que é equivalente a transmissão `scope=translation,transliteration,dictionary`. Para decidir qual o conjunto de idiomas suportados é adequado para o seu cenário, consulte a descrição do [objeto resposta](#response-body).</td>
  </tr>
</table> 

Cabeçalhos de pedido são:

<table width="100%">
  <th width="20%">Cabeçalhos</th>
  <th>Descrição</th>
  <tr>
    <td>Idioma aceitar</td>
    <td>*Cabeçalho de pedido opcional*.<br/>O idioma a utilizar para cadeias de interface de utilizador. Alguns dos campos na resposta são os nomes de idiomas ou os nomes das regiões. Utilize este parâmetro para definir o idioma em que estes nomes são devolvidos. O idioma for especificado, fornecendo uma tag de idioma do BCP 47 corretamente formada. Por exemplo, utilize o valor `fr` para utilizar o valor ou um pedido de nomes em francês `zh-Hant` para nomes de pedido em chinês tradicional.<br/>Nomes são fornecidos no idioma do inglês, se não for especificada uma linguagem de destino ou quando a localização não está disponível.
    </td>
  </tr>
  <tr>
    <td>X ClientTraceId</td>
    <td>*Cabeçalho de pedido opcional*.<br/>Um GUID gerados pelo cliente para identificar exclusivamente o pedido.</td>
  </tr>
</table> 

Não é necessária a autenticação ao obter os recursos do idioma.

## <a name="response-body"></a>Corpo da resposta

Um cliente utiliza o `scope` parâmetro definir quais os grupos de idiomas é interessado de consulta.

* `scope=translation` Fornece os idiomas suportados para traduzir texto a partir de um idioma para outro idioma;

* `scope=transliteration` Fornece capacidades para converter o texto um idioma de um script para outro script;

* `scope=dictionary` Fornece pares de idioma para o qual `Dictionary` operações devolvem dados.

Um cliente pode obter vários grupos em simultâneo ao especificar uma lista separada por vírgulas de nomes. Por exemplo, `scope=translation,transliteration,dictionary` devolvam os idiomas suportados para todos os grupos.

Uma resposta com êxito é um objeto JSON com uma propriedade para cada grupo de pedido:

```json
{
    "translation": {
        //... set of languages supported to translate text (scope=translation)
    },
    "transliteration": {
        //... set of languages supported to convert between scripts (scope=transliteration)
    },
    "dictionary": {
        //... set of languages supported for alternative translations and examples (scope=dictionary)
    }
}
```

Segue-se o valor para cada propriedade.

* `translation` Propriedade

  O valor da `translation` propriedade é um dicionário de (chave, valor) pares. Cada chave é uma tag de idioma BCP 47. Uma chave identifica um idioma para o qual texto pode ser convertido em ou convertido do. O valor associado à chave é um objeto JSON com propriedades que descrevem o idioma:

  * `name`: Nome a apresentar o idioma no idioma pedido através de `Accept-Language` cabeçalho.

  * `nativeName`: Nome do idioma de apresentação no idioma nativo neste idioma.

  * `dir`: Direcionalidade, que é `rtl` para idiomas de direita para a esquerda ou `ltr` para idiomas da esquerda para a direita.

  Um exemplo é:
          
  ```json
  {
    "translation": {
      ...
      "fr": {
        "name": "French",
        "nativeName": "Français",
        "dir": "ltr"
      },
      ...
    }
  }
  ```

* `transliteration` Propriedade

  O valor da `transliteration` propriedade é um dicionário de (chave, valor) pares. Cada chave é uma tag de idioma BCP 47. Uma chave identifica um idioma para o qual texto pode ser convertido a partir de um script para outro script. O valor associado à chave é um objeto JSON com propriedades que descrevem o idioma e os respetivos scripts suportados:

  * `name`: Nome a apresentar o idioma no idioma pedido através de `Accept-Language` cabeçalho.

  * `nativeName`: Nome do idioma de apresentação no idioma nativo neste idioma.

  * `scripts`: Lista de scripts para converter do. Cada elemento do `scripts` lista tem propriedades:

    * `code`: O código que identifica o script.

    * `name`: Nome a apresentar o script no idioma pedido através de `Accept-Language` cabeçalho.

    * `nativeName`: Nome do idioma de apresentação no idioma nativo para o idioma.

    * `dir`: Direcionalidade, que é `rtl` para idiomas de direita para a esquerda ou `ltr` para idiomas da esquerda para a direita.

    * `toScripts`: Lista de scripts disponíveis para converter o texto a. Cada elemento do `toScripts` lista tem propriedades `code`, `name`, `nativeName`, e `dir` conforme descrito anteriormente.

  Um exemplo é:

  ```json
  {
    "transliteration": {
      ...
      "ja": {
        "name": "Japanese",
        "nativeName": "日本語",
        "scripts": [
          {
            "code": "Jpan",
            "name": "Japanese",
            "nativeName": "日本語",
            "dir": "ltr",
            "toScripts": [
              {
                "code": "Latn",
                "name": "Latin",
                "nativeName": "ラテン語",
                "dir": "ltr"
              }
            ]
          },
          {
            "code": "Latn",
            "name": "Latin",
            "nativeName": "ラテン語",
            "dir": "ltr",
            "toScripts": [
              {
                "code": "Jpan",
                "name": "Japanese",
                "nativeName": "日本語",
                "dir": "ltr"
              }
            ]
          }
        ]
      },
      ...
    }
  }
  ```

* `dictionary` Propriedade

  O valor da `dictionary` propriedade é um dicionário de (chave, valor) pares. Cada chave é uma tag de idioma BCP 47. A chave identifica um idioma para o qual traduções alternativas e back-traduções estão disponíveis. O valor é um objeto JSON que descreve o idioma de origem e os idiomas de destino com traduções disponíveis:

  * `name`: Nome a apresentar o idioma de origem no idioma pedido através de `Accept-Language` cabeçalho.

  * `nativeName`: Nome do idioma de apresentação no idioma nativo neste idioma.

  * `dir`: Direcionalidade, que é `rtl` para idiomas de direita para a esquerda ou `ltr` para idiomas da esquerda para a direita.

  * `translations`: Lista de idiomas com traduções alterative e exemplos para a consulta expressado no idioma de origem. Cada elemento do `translations` lista tem propriedades:

    * `name`: Nome a apresentar o idioma de destino no idioma pedido através de `Accept-Language` cabeçalho.

    * `nativeName`: O nome de apresentação do idioma de destino no idioma nativo para o idioma de destino.

    * `dir`: Direcionalidade, que é `rtl` para idiomas de direita para a esquerda ou `ltr` para idiomas da esquerda para a direita.
    
    * `code`: O código de idioma identificar o idioma de destino.

  Um exemplo é:

  ```json
  "es": {
    "name": "Spanish",
    "nativeName": "Español",
    "dir": "ltr",
    "translations": [
      {
        "name": "English",
        "nativeName": "English",
        "dir": "ltr",
        "code": "en"
      }
    ]
  },
  ```

A estrutura do objeto de resposta não será alterado sem uma alteração na versão da API. Para a mesma versão da API, a lista de idiomas disponíveis pode alterar ao longo do tempo, porque o Microsoft Translator continuamente expande a lista de idiomas suportados pelo que os respetivos serviços.

A lista de idiomas suportados não será alterado frequentemente. Para poupar largura de banda de rede e melhorar a capacidade de resposta, uma aplicação cliente deve considerar a colocação em cache de recursos do idioma e a tag de entidade correspondente (`ETag`). Em seguida, a aplicação cliente pode periodicamente (por exemplo, uma vez a cada 24 horas) consultar o serviço para obter o conjunto de idiomas suportados mais recente. Transmitir atual `ETag` valor um `If-None-Match` campo do cabeçalho irá permitir que o serviço otimizar a resposta. Se o recurso não foi modificado, o serviço será devolveu o código de estado 304 e um corpo de resposta vazia.

## <a name="response-headers"></a>Cabeçalhos de resposta

<table width="100%">
  <th width="20%">Cabeçalhos</th>
  <th>Descrição</th>
  <tr>
    <td>ETag</td>
    <td>Valor atual da tag de entidade para os grupos de pedido de idiomas suportados. Para fazer pedidos subsequentes mais eficiente, o cliente pode enviar o `ETag` valor um `If-None-Match` campo do cabeçalho.
    </td>
  </tr>
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
    <td>304</td>
    <td>O recurso não foi modificado desde a versão especificada por cabeçalhos de pedido `If-None-Match`.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Um dos parâmetros de consulta está em falta ou não é válido. Corrija os parâmetros do pedido.</td>
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
curl "https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation"
```

---
