---
title: Método de idiomas de reconhecimento de voz API Microsoft tradutor | Microsoft Docs
titleSuffix: Cognitive Services
description: Utilize o método de idiomas de API de reconhecimento de voz de conversor de Microsoft.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 05/18/18
ms.author: v-jansko
ms.openlocfilehash: 5396e3be17345c3c36197a9b6cbace86e1f574c1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355700"
---
# <a name="speech-api-languages"></a>API de reconhecimento de voz: idiomas

Microsoft Translator continuamente expande a lista de idiomas suportados pelo que os respetivos serviços. Utilize esta API para detetar o conjunto de idiomas atualmente disponíveis para utilização com o serviço de tradução de reconhecimento de voz.

Exemplos de código que demonstra a utilização da API para obter os idiomas disponíveis estão disponíveis a partir de [site Microsoft tradutor Github](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Notas de implementação

OBTER /languages 

Um vasto conjunto de idiomas está disponível para transcribe reconhecimento de voz para traduzir texto transcribed e para produzir o reconhecimento de voz sintetizado da tradução.

Um cliente utiliza o `scope` parâmetro para definir os conjuntos de idiomas é interessado de consulta.

* **Reconhecimento de voz para texto:** utilizar o parâmetro de consulta `scope=speech` para obter o conjunto de idiomas disponíveis transcribe reconhecimento de voz no texto.
* **Conversão de texto:** utilizar o parâmetro de consulta `scope=text` para obter o conjunto de idiomas disponíveis para traduzir texto transcribed.
* **Text-to-Speech:** utilizar o parâmetro de consulta `scope=tts` para obter o conjunto de idiomas e voices disponíveis para sintetizar texto traduzido no reconhecimento de voz.

Um cliente pode obter vários conjuntos em simultâneo ao especificar uma lista separada por vírgulas de opções. Por exemplo, `scope=speech,text,tts`.

Uma resposta com êxito é um objeto JSON com uma propriedade para o conjunto de cada pedido.

```
{
    "speech": {
        //... Set of languages supported for speech-to-text
    },
    "text": {
        //... Set of languages supported for text translation
    },
    "tts": {
        //... Set of languages supported for text-to-speech
    }
}
```

Uma vez que um cliente pode utilizar o `scope` deve ser devolvido o parâmetro de consulta para selecionar os conjuntos de idiomas suportados, uma resposta real só pode incluir um subconjunto de todas as propriedades mostradas acima.

Segue-se o valor fornecido com cada propriedade.

### <a name="speech-to-text-speech"></a>Voz texto (reconhecimento de voz)

O valor associado a propriedade de reconhecimento de voz para texto, `speech`, é um dicionário de (chave, valor) pares. Cada chave identifica um idioma suportado para texto de reconhecimento de voz. A chave é o identificador de que o cliente passa para a API. O valor associado à chave é um objeto com as seguintes propriedades:

* `name`: O nome do idioma de apresentação.
* `language`: Tag de idioma de escritas idioma associada. Consulte "Texto transation" abaixo.
Um exemplo é:

```
{
    "speech": {
        "en-US": { name: "English", language: "en" }
    }
}
```

### <a name="text-translation-text"></a>Conversão de texto (texto)

O valor associado a `text` propriedade também é um dicionário em que cada chave identifica um idioma suportado para a conversão de texto. O valor associado à chave descreve o idioma:

* `name`: O nome do idioma de apresentação.
* `dir`: Direcionalidade que é `rtl` para idiomas de direita para a esquerda ou `ltr` para idiomas da esquerda para a direita.

Um exemplo é:

```
{
    "text": {
        "en": { name: "English", dir: "ltr" }
    }
}
```

### <a name="text-to-speech-tts"></a>Text-to-Speech (tts)

O valor associado a propriedade text-to-speech, tts, também é um dicionário em que cada chave identifica uma voz suportada. Atributos de um objeto de voz são:

* `displayName`: Nome a apresentar a voz.
* `gender`: Género de voz (male ou female).
* `locale`: Tag de idioma de voz com subtag idioma principal e subtag de região.
* `language`: Tag de idioma de escritas idioma associada.
* `languageName`: O nome do idioma de apresentação.
* `regionName`: Nome de apresentação da região de neste idioma.

Um exemplo é:

```
{
    "tts": {
        "en-US-Zira": {
            "displayName": "Zira",
            "gender": "female",
            "locale": "en-US",
            "languageName": "English",
            "regionName": "United States",
            "language": "en"
        }
}
```

### <a name="localization"></a>Localização
O serviço devolve todos os nomes no idioma do cabeçalho 'Language aceitar', para todos os idiomas suportados na conversão de texto.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)
Objeto que descreve o conjunto de idiomas suportados.

Valor de ModelExample: 

Langagues {voz (objeto, opcional), o texto (objeto, opcional), tts (objeto, opcional)}

### <a name="headers"></a>Cabeçalhos

|Cabeçalho|Descrição|Tipo|
:--|:--|:--|
X-RequestId|O valor gerado pelo servidor para identificar o pedido e utilizado para fins de resolução de problemas.|cadeia|

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|
|versão de API    |Versão da API solicitada pelo cliente. Valores permitidos são: `1.0`.|consulta|cadeia|
|scope  |Conjuntos de idiomas suportados ou voices para devolver ao cliente. Este parâmetro for especificado como uma lista separada por vírgulas de palavras-chave. As palavras-chave seguintes estão disponíveis:<ul><li>`speech`: Fornece o conjunto de idiomas suportados para transcribe reconhecimento de voz.</li><li>`tts`: Fornece o conjunto de voices suportado para a conversão de conversão de texto em voz.</li><li>`text`: Fornece o conjunto de idiomas suportados para traduzir texto.</li></ul>Se não for especificado um valor, o valor de `scope` assume a predefinição `text`.|consulta|cadeia|
|X ClientTraceId    |Um GUID gerados pelo cliente utilizado para rastrear um pedido. Para facilitar a resolução de problemas, os clientes devem fornecer um novo valor de cada pedido e inicie-lo.|cabeçalho|cadeia|
|Idioma aceitar    |Alguns dos campos na resposta são nomes de idiomas ou regiões. Utilize este parâmetro para definir o idioma em que os nomes são devolvidos. O idioma for especificado, fornecendo uma tag de idioma do BCP 47 corretamente formada. Selecione uma etiqueta da lista de identificadores de idioma devolvido com o `text` âmbito. Para idiomas não suportados, os nomes são fornecidos no idioma inglês.<br/>Por exemplo, utilize o valor `fr` para utilizar o valor ou um pedido de nomes em francês `zh-Hant` para nomes de pedido em chinês tradicional.|cabeçalho|cadeia|
    
### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400|Pedido incorreto. Verifique os parâmetros de entrada para garantir que são válidos. O objeto de resposta inclui uma descrição mais detalhada do erro.|
|429|Demasiados pedidos.|
|500|Ocorreu um erro. Se o erro persistir, comunique-lo com o identificador de rastreio de cliente (X-ClientTraceId) ou pedir identificador (X-RequestId).|
|503|Servidor temporariamente indisponível. Repita o pedido. Se o erro persistir, comunique-lo com o identificador de rastreio de cliente (X-ClientTraceId) ou pedir identificador (X-RequestId).|
