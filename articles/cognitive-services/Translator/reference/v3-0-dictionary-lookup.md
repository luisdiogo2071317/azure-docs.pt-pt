---
title: Método de pesquisa do Microsoft tradutor texto dicionário de API | Microsoft Docs
description: Utilize o método de pesquisa de dicionário de API do Microsoft tradutor texto.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 31435fcfca61517bfc72d534e911a1dcadbee52b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354974"
---
# <a name="text-api-30-dictionary-lookup"></a>Texto API 3.0: Pesquisa de dicionário

Fornece traduções alternativas para um word e um pequeno número de expressões idiomatic. Cada tradução tem uma parte de voz e uma lista de traduções anterior. Os back-traduções ativar um utilizador compreender a tradução de contexto. O [dicionário exemplo](.\v3-0-dictionary-examples.md) mais Desagregue para Consulte exemplo utiliza de cada par de tradução permite a operação.

## <a name="request-url"></a>URL do pedido

Enviar um `POST` pedido para:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0
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

O corpo do pedido é uma matriz JSON. Cada elemento de matriz é um objeto JSON com uma propriedade de cadeia denominado `Text`, que representa o termo de pesquisa.

```json
[
    {"Text":"fly"}
]
```

As seguintes limitações aplicam-se:

* A matriz pode ter no máximo 10 elementos.
* O valor de texto de um elemento de matriz não pode exceder os 100 carateres, incluindo espaços.

## <a name="response-body"></a>Corpo da resposta

Uma resposta com êxito é uma matriz JSON com um resultado para cada cadeia na matriz de entrada. Um objeto de resultado inclui as seguintes propriedades:

  * `normalizedSource`: Uma cadeia dá ao formulário normalizado do período da origem. Por exemplo, se o pedido é "JOÃO", o formulário normalizado será "João". O conteúdo deste campo torna-se a entrada para [exemplos de pesquisa](.\v3-0-dictionary-examples.md).
    
  * `displaySource`: Uma cadeia de fornecer o termo de origem num formulário melhor adequados para apresentação de utilizador final. Por exemplo, se a entrada é "JOÃO", o formato de apresentação refletirá a ortografia do nome habitual: "João". 

  * `translations`: Uma lista de traduções o termo de origem. Cada elemento da lista é um objeto com as seguintes propriedades:

    * `normalizedTarget`: Uma cadeia que lhe confere a forma normalizada deste prazo na linguagem de destino. Este valor deve ser utilizado como entrada para [exemplos de pesquisa](.\v3-0-dictionary-examples.md).

    * `displayTarget`: Uma cadeia de fornecer o termo na linguagem de destino e num formulário melhor adequados para apresentação de utilizador final. Geralmente, esta só será diferem do `normalizedTarget` em termos de maiúsculas/minúsculas. Por exemplo, um nome adequado, como "Juan" terão `normalizedTarget = "juan"` e `displayTarget = "Juan"`.

    * `posTag`: Uma cadeia de associar esta termo com uma etiqueta de parte de voz.

        | Nome da etiqueta | Descrição  |
        |----------|--------------|
        | ADJ      | Adjectives   |
        | ADV      | Adverbs      |
        | CONJ     | Conjunctions |
        | DET      | Determiners  |
        | MODAL    | Verbos        |
        | SUBSTANTIVO     | Substantivos        |
        | PREPARAÇÃO     | Prepositions |
        | PRON     | Pronouns     |
        | VERBO     | Verbos        |
        | OUTROS    | Outros        |

        Como uma nota de implementação, estas etiquetas foram identificadas por parte dos-voz etiquetagem lado do inglês e, em seguida, colocar mais frequentes na etiqueta para cada par de origem/destino. Por isso, se pessoas traduzir frequentemente uma palavra espanhol para uma etiqueta de parte de voz diferentes em inglês, etiquetas poderão acabar por ser incorreto (relativamente à palavra espanhol).

    * `confidence`: Um valor entre 0,0 e 1,0 que representa a "confiança" (ou talvez mais com precisão, "probabilidade dos dados de formação") de que par de conversão. A soma de pontuações de confiança para uma palavra de origem pode ou não pode soma para 1.0. 

    * `prefixWord`: Dar uma cadeia da palavra a apresentar como um prefixo da tradução. Atualmente, este é o determiner gendered dos substantivos, idiomas que tiver gendered determiners. Por exemplo, o prefixo da word espanhol "mosca" é "la", uma vez que "mosca" é um nome feminine no espanhol. Isto apenas é dependente a tradução e não a origem. Se houver um prefixo, estará a cadeia vazia.
    
    * `backTranslations`: Uma lista de "back traduções" do destino. Por exemplo, origem palavras que o destino pode traduzir-se. A lista é garantida que contém a palavra de origem que foi pedida (por exemplo, se a origem word que está a ser procurado é "viaje até", em seguida, é garantido que "viaje até" vai estar no `backTranslations` lista). No entanto, não é garantido de ser a primeira posição e, muitas vezes, não será possível. Cada elemento do `backTranslations` lista é um objeto descrito pelas seguintes propriedades:

        * `normalizedText`: Uma cadeia de dar a forma normalizada do termo de origem que é um back-tradução de destino. Este valor deve ser utilizado como entrada para [exemplos de pesquisa](.\v3-0-dictionary-examples.md).        

        * `displayText`: Uma cadeia de fornecer o termo de origem que é um back-tradução de destino num formulário melhor adequados para apresentação de utilizador final.

        * `numExamples`: Um número inteiro que representa o número de exemplos que estão disponíveis para este par de conversão. Exemplos reais tem de ser obtidos com uma chamada separada para [exemplos de pesquisa](.\v3-0-dictionary-examples.md). O número destina-se principalmente para facilitar a apresentar num UX. Por exemplo, uma interface de utilizador pode adicionar uma hiperligação para a tradução back se o número de exemplos é maior que zero e mostrar a tradução de back como texto não encriptado, se existirem não exemplos. Tenha em atenção que o número real de exemplos devolvidos por uma chamada para [exemplos de pesquisa](.\v3-0-dictionary-examples.md) pode ser inferior a `numExamples`porque filtragem adicionais pode ser aplicada no momento para remover "incorretos" exemplos.
        
        * `frequencyCount`: Um número inteiro que representa a frequência deste par de tradução de dados. O objetivo principal deste campo é proporcionam uma interface de utilizador com um meio para ordenar traduções back para que os termos mais frequentes primeiro.

    > [!NOTE]
    > Se o termo que está a ser comparados de segurança não existe no dicionário, a resposta é 200 (OK), mas a `translations` lista é uma lista vazia.

## <a name="examples"></a>Exemplos

Este exemplo mostra como procurar traduções alternativas no espanhol do período da inglês `fly` .

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly'}]"
```

---

O corpo da resposta (abreviado para efeitos de clareza) é:

```
[
    {
        "normalizedSource":"fly",
        "displaySource":"fly",
        "translations":[
            {
                "normalizedTarget":"volar",
                "displayTarget":"volar",
                "posTag":"VERB",
                "confidence":0.4081,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":4637},
                    {"normalizedText":"flying","displayText":"flying","numExamples":15,"frequencyCount":1365},
                    {"normalizedText":"blow","displayText":"blow","numExamples":15,"frequencyCount":503},
                    {"normalizedText":"flight","displayText":"flight","numExamples":15,"frequencyCount":135}
                ]
            },
            {
                "normalizedTarget":"mosca",
                "displayTarget":"mosca",
                "posTag":"NOUN",
                "confidence":0.2668,
                "prefixWord":"",
                "backTranslations":[
                    {"normalizedText":"fly","displayText":"fly","numExamples":15,"frequencyCount":1697},
                    {"normalizedText":"flyweight","displayText":"flyweight","numExamples":0,"frequencyCount":48},
                    {"normalizedText":"flies","displayText":"flies","numExamples":9,"frequencyCount":34}
                ]
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```

Este exemplo mostra o que acontece quando o termo que está a ser procurado não existe para o par de dicionário válido.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/lookup?api-version=3.0&from=en&to=es" -H "X-ClientTraceId: 875030C7-5380-40B8-8A03-63DACCF69C11" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly123456'}]"
```

---

Uma vez que o termo não foi encontrado no dicionário, o corpo da resposta inclui vazio `translations` lista.

```
[
    {
        "normalizedSource":"fly123456",
        "displaySource":"fly123456",
        "translations":[]
    }
]
```
