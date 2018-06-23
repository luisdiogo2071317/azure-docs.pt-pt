---
title: Método de traduzir texto Microsoft tradutor API | Microsoft Docs
titleSuffix: Cognitive Services
description: Utilize o método traduzir de API do Microsoft tradutor texto.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: d8d5e1e2fac747fa733f1d92c08008b7eac2a1bc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355628"
---
# <a name="text-api-30-translate"></a>Texto API 3.0: traduzir

Converte texto.

## <a name="request-url"></a>URL do pedido

Enviar um `POST` pedido para:

```HTTP
https://api.cognitive.microsofttranslator.com/translate?api-version=3.0
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
    <td>*O parâmetro opcional*.<br/>Especifica a linguagem de texto de entrada. Localizar os idiomas estão disponíveis para traduzir de procurando [idiomas suportados](.\v3-0-languages.md) utilizando o `translation` âmbito. Se o `from` parâmetro não for especificado, a deteção automática de idioma é aplicada ao determinar o idioma de origem.</td>
  </tr>
  <tr>
    <td>para</td>
    <td>*Necessário parâmetro*.<br/>Especifica o idioma do texto de saída. O idioma de destino tem de ser o [idiomas suportados](.\v3-0-languages.md) incluído no `translation` âmbito. Por exemplo, utilizar `to=de` traduzir para alemão.<br/>É possível converter para vários idiomas em simultâneo, repetindo o parâmetro na cadeia de consulta. Por exemplo, utilizar `to=de&to=it` traduzir alemão e italiano.</td>
  </tr>
  <tr>
    <td>textType</td>
    <td>*O parâmetro opcional*.<br/>Define se o texto a ser traduzido é texto simples ou texto HTML. Qualquer HTML tem de ser um elemento bem formado e completo. Os valores possíveis são: `plain` (predefinição) ou `html`.</td>
  </tr>
  <tr>
    <td>categoria</td>
    <td>*O parâmetro opcional*.<br/>Uma cadeia que especifica a categoria (domínio) da tradução. Este parâmetro é utilizado para obter traduções de um sistema personalizado criado com [personalizada tradutor](../customization.md). Valor predefinido é: `general`.</td>
  </tr>
  <tr>
    <td>ProfanityAction</td>
    <td>*O parâmetro opcional*.<br/>Especifica a forma como devem ser tratados de profanities no traduções. Os valores possíveis são: `NoAction` (predefinição), `Marked` ou `Deleted`. Para compreender as formas para tratar profanity, consulte [processamento Profanity](#handle-profanity).</td>
  </tr>
  <tr>
    <td>ProfanityMarker</td>
    <td>*O parâmetro opcional*.<br/>Especifica a forma como profanities deve ser marcado como no traduções. Os valores possíveis são: `Asterisk` (predefinição) ou `Tag`. Para compreender as formas para tratar profanity, consulte [processamento Profanity](#handle-profanity).</td>
  </tr>
  <tr>
    <td>includeAlignment</td>
    <td>*O parâmetro opcional*.<br/>Especifica se pretende incluir projecção de alinhamento do texto de origem para texto traduzida. Os valores possíveis são: `true` ou `false` (predefinição). </td>
  </tr>
  <tr>
    <td>includeSentenceLength</td>
    <td>*O parâmetro opcional*.<br/>Especifica se pretende incluir limites de frases para o texto de entrada e o texto traduzido. Os valores possíveis são: `true` ou `false` (predefinição).</td>
  </tr>
  <tr>
    <td>suggestedFrom</td>
    <td>*O parâmetro opcional*.<br/>Especifica uma linguagem de contingência se o idioma de texto de entrada não pode ser identificado. A deteção automática de idioma é aplicada quando o `from` parâmetro for omitido. Se falhar a deteção, o `suggestedFrom` idioma será assumido.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*O parâmetro opcional*.<br/>Especifica o script de texto de entrada.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*O parâmetro opcional*.<br/>Especifica o script do texto traduzido.</td>
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

O corpo do pedido é uma matriz JSON. Cada elemento de matriz é um objeto JSON com uma propriedade de cadeia denominado `Text`, que representa a cadeia a converter.

```json
[
    {"Text":"I would really like to drive your car around the block a few times."}
]
```

As seguintes limitações aplicam-se:

* A matriz pode ter no máximo de 25 elementos.
* O texto completo incluído no pedido não pode exceder 5000 carateres, incluindo espaços.

## <a name="response-body"></a>Corpo da resposta

Uma resposta com êxito é uma matriz JSON com um resultado para cada cadeia na matriz de entrada. Um objeto de resultado inclui as seguintes propriedades:

  * `detectedLanguage`: Um objeto que descrevem o idioma detetado através das seguintes propriedades:

      * `language`: Uma cadeia representando o código de idioma detetado.

      * `score`: Um valor de vírgula flutuante que indica a confiança no resultado. A classificação é entre zero e um e uma pontuação baixa indica uma confiança baixa.

    O `detectedLanguage` propriedade só está presente no objeto de resultado quando a deteção automática de idioma é solicitada.

  * `translations`: Uma matriz de resultados de conversão. O tamanho da matriz corresponde ao número de idiomas de destino especificado através de `to` parâmetro de consulta. Cada elemento na matriz inclui:

    * `to`: Uma cadeia representando o código de idioma do idioma de destino.

    * `text`: Uma cadeia de texto traduzido a dar.

    * `transliteration`: Um objeto dar traduzido texto do script especificado pelo `toScript` parâmetro.

      * `script`: Uma cadeia que especifica o script de destino.   

      * `text`: Uma cadeia de texto traduzido a dar no script de destino.

    O `transliteration` objeto não está incluído se transliteration não ocorrer.

    * `alignment`: Um objeto com uma propriedade de cadeia única chamada `proj`, que é mapeado texto traduzidas em texto de entrada. As informações de alinhamento só são fornecidas quando o parâmetro de pedido `includeAlignment` é `true`. Alinhamento é devolvido como um valor de cadeia do seguinte formato: `[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]]`.  Os dois pontos separa o início e índice de fim, o travessão separa os idiomas e espaço separa as palavras. Uma palavra poderão ficar alinhadas com zero, um ou vários palavras no outro idioma e as palavras aligned poderão estar não contíguos. Quando não existem informações de alinhamento estiver disponíveis, o elemento de alinhamento estará vazio. Consulte [obter informações de alinhamento](#obtain-alignment-information) para um exemplo e restrições.

    * `sentLen`: Um objeto devolver limites de frases nos textos de entrada e de saída.

      * `srcSentLen`: Uma matriz de número inteiro que representa os comprimentos das frases no texto de entrada. O comprimento da matriz é o número de frases e os valores são o comprimento de cada frases.

      * `transSentLen`: Uma matriz de número inteiro que representa os comprimentos das frases no texto traduzido. O comprimento da matriz é o número de frases e os valores são o comprimento de cada frases.

    Limites de frases só são incluídos quando o parâmetro de pedido `includeSentenceLength` é `true`.

  * `sourceText`: Um objeto com uma propriedade de cadeia única chamada `text`, que fornece o texto de entrada no script de idioma de origem predefinido. `sourceText` a propriedade está presente apenas quando a entrada é expresso num script que não é o habitual script para o idioma. Por exemplo, se a entrada foram Árabe escrito no script latinos, em seguida, `sourceText.text` seria o mesmo texto Arabic convertido em Arab script.

Exemplo de respostas JSON são fornecidos no [exemplos](#examples) secção.

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

### <a name="translate-a-single-input"></a>Converte um valor único

Este exemplo mostra como converter uma frase único do inglês, chinês simplificado para.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

O corpo da resposta é:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    }
]
```

O `translations` matriz inclui um elemento, que fornece a conversão da única informação de texto na entrada.

### <a name="translate-a-single-input-with-language-auto-detection"></a>Converte uma única entrada com a deteção automática de idioma

Este exemplo mostra como converter uma frase único do inglês, chinês simplificado para. O pedido não especifica a linguagem de entrada. A deteção automática do idioma de origem é utilizada em vez disso.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

O corpo da resposta é:

```
[
    {
        "detectedLanguage": {"language": "en", "score": 1.0},
        "translations":[
            {"text": "你好, 你叫什么名字？", "to": "zh-Hans"}
        ]
    }
]
```
A resposta é semelhante à resposta do exemplo anterior. Uma vez que a deteção automática de idioma foi solicitada, a resposta também inclui informações sobre o idioma detetada para o texto de entrada. 

### <a name="translate-with-transliteration"></a>Traduzir com transliteration

Vamos expandir o anterior exemplo adicionando transliteration. O pedido seguinte pede-lhe uma conversão chinês escrita no script em latim.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Latn" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

O corpo da resposta é:

```
[
    {
        "detectedLanguage":{"language":"en","score":1.0},
        "translations":[
            {
                "text":"你好, 你叫什么名字？",
                "transliteration":{"text":"nǐ hǎo , nǐ jiào shén me míng zì ？","script":"Latn"},
                "to":"zh-Hans"
            }
        ]
    }
]
```

O resultado da conversão inclui agora um `transliteration` propriedade, o que lhe dá o texto traduzido utilizando carateres em latim.

### <a name="translate-multiple-pieces-of-text"></a>Traduzir várias peças de texto

Traduzir os vários cadeias em simultâneo é simplesmente um fim de especificar uma matriz de cadeias no corpo do pedido.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}, {'Text':'I am fine, thank you.'}]"
```

---

O corpo da resposta é:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    },            
    {
        "translations":[
            {"text":"我很好，谢谢你。","to":"zh-Hans"}
        ]
    }
]
```

### <a name="translate-to-multiple-languages"></a>Converter para vários idiomas

Este exemplo mostra como converter a entrada mesma para vários idiomas num pedido.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

O corpo da resposta é:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"},
            {"text":"Hallo, was ist dein Name?","to":"de"}
        ]
    }
]
```

### <a name="handle-profanity"></a>Processar profanity

Normalmente, o serviço de tradutor irão manter profanity que está presente na origem na tradução. O grau de profanity e o contexto que torna palavras profanas diferem entre as culturas e como resultado o grau de profanity na linguagem de destino pode ser amplified ou reduzido.

Se quiser evitar obter profanity na tradução, independentemente da presença de profanity no texto de origem, pode utilizar o profanity filtragem opção. A opção permite-lhe escolher se pretende ver profanity eliminado, se pretende marcar profanities com etiquetas adequadas (dando-lhe a opção para adicionar o seus próprios pós-processamento) ou não se quiser nenhuma ação executada. Os valores aceites de `ProfanityAction` são `Deleted`, `Marked` e `NoAction` (predefinição).

<table width="100%">
  <th width="20%">ProfanityAction</th>
  <th>Ação</th>
  <tr>
    <td>`NoAction`</td>
    <td>Este é o comportamento predefinido. Profanity passarão a partir da origem de destino.<br/><br/>
    **Origem de exemplo (japonês)**: 彼はジャッカスです。<br/>
    **Tradução de exemplo (inglês)**: ele é um jackass.
    </td>
  </tr>
  <tr>
    <td>`Deleted`</td>
    <td>Palavras profanas serão removidas da saída sem substituição.<br/><br/>
    **Origem de exemplo (japonês)**: 彼はジャッカスです。<br/>
    **Tradução de exemplo (inglês)**: ele é um.
    </td>
  </tr>
  <tr>
    <td>`Marked`</td>
    <td>Palavras profanas são substituídas por um marcador no resultado. O marcador depende o `ProfanityMarker` parâmetro.<br/><br/>
Para `ProfanityMarker=Asterisk`, profanas palavras são substituídas por `***`:<br/>
    **Origem de exemplo (japonês)**: 彼はジャッカスです。<br/>
    **Tradução de exemplo (inglês)**: ele é um \* \* \*.<br/><br/>
Para `ProfanityMarker=Tag`, profanas palavras são rodeadas por etiquetas XML &lt;profanity&gt; e &lt;/profanity&gt;:<br/>
    **Origem de exemplo (japonês)**: 彼はジャッカスです。<br/>
    **Tradução de exemplo (inglês)**: ele é um &lt;profanity&gt;jackass&lt;/profanity&gt;.
  </tr>
</table> 

Por exemplo:

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Esta ação devolve:

```
[
    {
        "translations":[
            {"text":"Das ist eine *** gute Idee.","to":"de"}
        ]
    }
]
```

Compare com:

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked&profanityMarker=Tag" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Devolve o último esse pedido:

```
[
    {
        "translations":[
            {"text":"Das ist eine <profanity>verdammt</profanity> gute Idee.","to":"de"}
        ]
    }
]
```

### <a name="translate-content-with-markup-and-decide-whats-translated"></a>Converter o conteúdo com o markup e decidir o que está a converter

É comum para converter o conteúdo que inclui o markup, tais como o conteúdo a partir de uma página HTML ou conteúdo a partir de um documento XML. Incluir o parâmetro de consulta `textType=html` ao traduzir o conteúdo com etiquetas. Além disso, por vezes, é útil excluir o conteúdo específico da tradução. Pode utilizar o atributo `class=notranslate` para especificar o conteúdo que deve permanecer no respetivo idioma original. No exemplo seguinte, o conteúdo dentro da primeira `div` não será possível converter elemento enquanto o conteúdo no segundo `div` será possível converter o elemento.

```
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

Eis um exemplo de pedido para ilustrar.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&textType=html" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'<div class=\"notranslate\">This will not be translated.</div><div>This will be translated.</div>'}]"
```

---

A resposta é:

```
[
    {
        "translations":[
            {"text":"<div class=\"notranslate\">This will not be translated.</div><div>这将被翻译。</div>","to":"zh-Hans"}
        ]
    }
]
```

### <a name="obtain-alignment-information"></a>Obter informações de alinhamento

Para receber as informações de alinhamento, especifique `includeAlignment=true` na cadeia de consulta.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeAlignment=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation.'}]"
```

---

A resposta é:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique.",
                "to":"fr",
                "alignment":{"proj":"0:2-0:1 4:9-3:9 11:14-11:19 16:17-21:24 19:25-40:50 27:37-29:38 38:38-51:51"}
            }
        ]
    }
]
```

As informações de alinhamento começa com `0:2-0:1`, o que significa que os primeiros três carateres no texto de origem (`The`) mapa para os primeiros dois carateres no texto traduzido (`La`).

Tenha em atenção as seguintes restrições:

* Alinhamento só é devolvido para um subconjunto dos pares de idioma:
  - do inglês para qualquer idioma;
  - a partir de qualquer outro idioma inglês, exceto para chinês simplificado, chinês tradicional e Letão inglês;
  - de japonês, coreano ou de coreano como japonês.
* Não irá receber alinhamento se o frase for uma tradução encontra. Exemplo de uma tradução encontra é "Este é um teste", "Posso adoram," e outros frases de alta frequência.

### <a name="obtain-sentence-boundaries"></a>Obter os limites de frases

Para receber informações sobre o comprimento de frases no texto de origem e traduzido texto, especifique `includeSentenceLength=true` na cadeia de consulta.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeSentenceLength=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation. The best machine translation technology cannot always provide translations tailored to a site or users like a human. Simply copy and paste a code snippet anywhere.'}]"
```

---

A resposta é:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique. La meilleure technologie de traduction automatique ne peut pas toujours fournir des traductions adaptées à un site ou des utilisateurs comme un être humain. Il suffit de copier et coller un extrait de code n’importe où.",
                "to":"fr",
                "sentLen":{"srcSentLen":[40,117,46],"transSentLen":[53,157,62]}
            }
        ]
    }
]
```

### <a name="translate-with-dynamic-dictionary"></a>Traduzir com dicionário dinâmico

Se já conhece a tradução de que pretende aplicar a uma palavra ou uma frase de acesso, pode fornecê-lo como markup dentro do pedido. O dicionário dinâmico apenas é seguro para substantivos compostos como nomes de adequado e produto.

A marcação para fornecer utiliza a seguinte sintaxe.

``` 
<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>
```

Por exemplo, considere o frase English "é uma entrada de dicionário do wordomatic palavra." Para preservar o word _wordomatic_ a tradução, enviar o pedido:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.'}]"
```

O resultado é:

```
[
    {
        "translations":[
            {"text":"Das Wort "wordomatic" ist ein Wörterbucheintrag.","to":"de"}
        ]
    }
]
```

Esta funcionalidade funciona da mesma forma com `textType=text` ou com `textType=html`. A funcionalidade deve ser utilizada com moderação. A forma adequada e até que ponto melhor da personalização tradução é conversor de personalizada a utilizar. Tradutor personalizado faz com que a utilização total de contexto e análises probabilidades. Se tiver ou pode suportar os dados de formação que mostra o seu trabalho ou frase no contexto de criar, obter muito melhores resultados. [Saiba mais sobre personalizada tradutor](../customization.md).
 





