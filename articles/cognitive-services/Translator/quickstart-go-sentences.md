---
title: 'Início Rápido: obter o comprimento de frases, Go – API de Texto do Microsoft Translator'
titleSuffix: Azure Cognitive Services
description: Neste guia de início rápido, encontrará o comprimento de frases no texto através da API de Texto do Microsoft Translator com o Go.
services: cognitive-services
author: noellelacharite
manager: erhopf
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: erhopf
ms.openlocfilehash: 92ea2291760edf16863dc58a00bac4389f818e72
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49644933"
---
# <a name="quickstart-get-sentence-lengths-with-the-translator-text-rest-api-go"></a>Início Rápido: obter os comprimentos de frases com a API de Texto do Microsoft Translator (Go)

Neste guia de introdução, encontrará o comprimento de frases no texto através da API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Terá de instalar a [distribuição do Go](https://golang.org/doc/install) para executar este código. O código de exemplo utiliza apenas bibliotecas **principais**, pelo que não há dependências externas.

Para utilizar a API de Texto do Microsoft Translator também precisa de uma chave de subscrição; veja [How to sign up for the Translator Text API (Como inscrever-se na API de Texto do Microsoft Translator)](translator-text-how-to-signup.md).

## <a name="breaksentence-request"></a>Pedido de BreakSentence

O seguinte código separa o texto de origem em frases através do método [BreakSentence](./reference/v3-0-break-sentence.md) (Separar Frase).

1. Crie um novo projeto de Go no seu editor de código preferido.
2. Adicione o código indicado abaixo.
3. Substitua o valor `subscriptionKey` por uma chave de acesso válida para a sua subscrição.
4. Guarde o ficheiro com uma extensão ".go".
5. Abra uma linha de comandos num computador com o Go instalado.
6. Compile o ficheiro, por exemplo, com o comando "go build quickstart-sentences.go".
7. Execute o ficheiro, por exemplo, com o comando "quickstart-sentences".

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
    const uriPath = "/breaksentence?api-version=3.0"

    const uri = uriBase + uriPath

    const text = "How are you? I am fine. What did you do today?"

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

## <a name="breaksentence-response"></a>Resposta do BreakSentence

É devolvida uma resposta com êxito em JSON, tal como apresentado no exemplo seguinte:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Passos seguintes

Explore os pacotes Go para APIs Serviços Cognitivos a partir do [SDK do Azure para Go](https://github.com/Azure/azure-sdk-for-go) no GitHub.

> [!div class="nextstepaction"]
> [Explorar os pacotes Go no GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
