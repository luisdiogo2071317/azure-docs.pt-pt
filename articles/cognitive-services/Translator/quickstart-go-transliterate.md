---
title: A Tradução de Texto converte o script de texto com Go | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste guia de introdução, pode converter texto de um idioma de um script para outro através da API de Texto do Microsoft Translator com o Go nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: nolachar
ms.openlocfilehash: f01fe38ef0605ad399edc03b52dc5b8ecc75275b
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "43771300"
---
# <a name="quickstart-transliterate-text-with-go"></a>Guia de introdução: transliterar texto com o Go

Neste guia de introdução, irá converter texto de um idioma de um script para outro através da API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Terá de instalar a [distribuição do Go](https://golang.org/doc/install) para executar este código. O código de exemplo utiliza apenas bibliotecas **principais**, pelo que não há dependências externas.

Para utilizar a API de Texto do Microsoft Translator também precisa de uma chave de subscrição; veja [How to sign up for the Translator Text API (Como inscrever-se na API de Texto do Microsoft Translator)](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Pedido de transliteração

Os passos seguintes convertem texto num idioma de um script para outro script com o método [Transliteração](./reference/v3-0-transliterate.md).

1. Crie um novo projeto de Go no seu editor de código preferido.
2. Adicione o código indicado abaixo.
3. Substitua o valor `subscriptionKey` por uma chave de acesso válida para a sua subscrição.
4. Guarde o ficheiro com uma extensão ".go".
5. Abra uma linha de comandos num computador com o Go instalado.
6. Compile o ficheiro, por exemplo, com o comando "go build quickstart-transliterate.go".
7. Compile o ficheiro, por exemplo, com o comando "quickstart-transliterate".

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    const uriBase = "https://api.cognitive.microsofttranslator.com"
    const uriPath = "/transliterate?api-version=3.0"

    // Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script
    const params = "&language=ja&fromScript=jpan&toScript=latn"

    const uri = uriBase + uriPath + params

    // Transliterate "good afternoon".
    const text = "こんにちは"

    r := strings.NewReader("[{\"Text\" : \"" + text + "\"}]")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.FormatInt(req.ContentLength, 10))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    resp, err := client.Do(req)
    if err != nil {
        fmt.Printf("Error on request: %v\n", err)
        return
    }
    defer resp.Body.Close()

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Printf("Error reading response body: %v\n", err)
        return
    }

    var f interface{}
    json.Unmarshal(body, &f)

    jsonFormatted, err := json.MarshalIndent(f, "", "  ")
    if err != nil {
        fmt.Printf("Error producing JSON: %v\n", err)
        return
    }
    fmt.Println(string(jsonFormatted))
}
```

## <a name="transliterate-response"></a>Resposta de transliteração

É devolvida uma resposta com êxito em JSON, tal como apresentado no exemplo seguinte:

```json
[
  {
    "script": "latn",
    "text": "konnnichiha"
  }
]
```

## <a name="next-steps"></a>Passos seguintes

Explore os pacotes Go para APIs Serviços Cognitivos a partir do [SDK do Azure para Go](https://github.com/Azure/azure-sdk-for-go) no GitHub.

> [!div class="nextstepaction"]
> [Explorar os pacotes Go no GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
