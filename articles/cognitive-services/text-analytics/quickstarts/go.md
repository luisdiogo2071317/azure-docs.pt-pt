---
title: Guia de introdução do Go para os Serviços Cognitivos do Azure, API de Análise de Texto | Microsoft Docs
description: Obtenha informações e exemplos de códigos para o ajudar a começar a utilizar rapidamente a API de Análise de Texto nos Serviços Cognitivos da Microsoft no Azure.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: nolachar
ms.openlocfilehash: 0969700434e3f848aebfa833f0816c6f9f019560
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "43771435"
---
# <a name="quickstart-for-text-analytics-api-with-go"></a>Guia de introdução da API de Análise de Texto com o Go 
<a name="HOLTop"></a>

Este artigo explica como [detetar idiomas](#Detect), [analisar sentimentos](#SentimentAnalysis), [extrair expressões-chave](#KeyPhraseExtraction) e [identificar entidades ligadas](#Entities) através das [APIs de Análise de Texto](//go.microsoft.com/fwlink/?LinkID=759711) com o Go.

Veja as [definições de API](//go.microsoft.com/fwlink/?LinkID=759346) para ter acesso à documentação técnica sobre APIs.

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter uma [conta de API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com a **API de Análise de Texto**. Pode utilizar o **escalão gratuito para 5000 transações/mês** para concluir este guia de introdução.

Também tem de ter a [chave de ponto final e acesso](../How-tos/text-analytics-how-to-access-key.md) que foi gerada automaticamente durante a sua inscrição.

<a name="Detect"></a>

## <a name="detect-language-request"></a>Pedido de deteção de idioma

A API Deteção de Idioma deteta o idioma de um documento de texto através do [método Detetar Idioma](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

1. Crie um novo projeto de Go no seu editor de código preferido.
1. Adicione o código indicado abaixo.
1. Substitua o valor `subscriptionKey` por uma chave de acesso válida para a sua subscrição.
1. Substitua a localização em `uriBase` (atualmente `westcentralus`) pela região em que se inscreveu.
1. Guarde o ficheiro com uma extensão ".go".
1. Abra uma linha de comandos num computador com o Go instalado.
1. Compile o ficheiro, por exemplo, com o comando "go build quickstart.go".
1. Execute o ficheiro, por exemplo, com o comando "quickstart".

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    /*
    Replace or verify the region.

    You must use the same region in your REST API call as you used to obtain your access keys.
    For example, if you obtained your access keys from the westus region, replace 
    "westcentralus" in the URI below with "westus".

    NOTE: Free trial access keys are generated in the westcentralus region, so if you are using
    a free trial access key, you should not need to change this region.
    */
    const uriBase =    "https://westcentralus.api.cognitive.microsoft.com"
    const uriPath = "/text/analytics/v2.0/languages"

    const uri = uriBase + uriPath

    data := []map[string]string{
        {"id": "1", "text": "This is a document written in English."},
        {"id": "2", "text": "Este es un document escrito en Español."},
        {"id": "3", "text": "这是一个用中文写的文件"},
    }

    documents, err := json.Marshal(&data)
    if err != nil {
        fmt.Printf("Error marshaling data: %v\n", err)
        return
    }

    r := strings.NewReader("{\"documents\": " + string(documents) + "}")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
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

## <a name="detect-language-response"></a>Resposta da deteção de idioma

É devolvida uma resposta com êxito em JSON, tal como é apresentado no exemplo seguinte:

```json

{
   "documents": [
      {
         "id": "1",
         "detectedLanguages": [
            {
               "name": "English",
               "iso6391Name": "en",
               "score": 1.0
            }
         ]
      },
      {
         "id": "2",
         "detectedLanguages": [
            {
               "name": "Spanish",
               "iso6391Name": "es",
               "score": 1.0
            }
         ]
      },
      {
         "id": "3",
         "detectedLanguages": [
            {
               "name": "Chinese_Simplified",
               "iso6391Name": "zh_chs",
               "score": 1.0
            }
         ]
      }
   ],
   "errors": [

   ]
}
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment-request"></a>Pedido de análise de sentimentos

A API da Análise de Sentimentos deteta o sentimento de um conjunto de registos de texto através do [método Sentimento](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9). O seguinte exemplo classifica dois documentos, um em inglês e outro em espanhol.

1. Crie um novo projeto de Go no seu editor de código preferido.
1. Adicione o código indicado abaixo.
1. Substitua o valor `subscriptionKey` por uma chave de acesso válida para a sua subscrição.
1. Substitua a localização em `uriBase` (atualmente `westcentralus`) pela região em que se inscreveu.
1. Guarde o ficheiro com uma extensão ".go".
1. Abra uma linha de comandos num computador com o Go instalado.
1. Compile o ficheiro, por exemplo, com o comando "go build quickstart.go".
1. Execute o ficheiro, por exemplo, com o comando "quickstart".

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    /*
    Replace or verify the region.

    You must use the same region in your REST API call as you used to obtain your access keys.
    For example, if you obtained your access keys from the westus region, replace 
    "westcentralus" in the URI below with "westus".

    NOTE: Free trial access keys are generated in the westcentralus region, so if you are using
    a free trial access key, you should not need to change this region.
    */
    const uriBase =    "https://westcentralus.api.cognitive.microsoft.com"
    const uriPath = "/text/analytics/v2.0/sentiment"

    const uri = uriBase + uriPath

    data := []map[string]string{
        {"id": "1", "language": "en", "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."},
        {"id": "2", "language": "es", "text": "Este ha sido un dia terrible, llegué tarde al trabajo debido a un accidente automobilistico."},
    }

    documents, err := json.Marshal(&data)
    if err != nil {
        fmt.Printf("Error marshaling data: %v\n", err)
        return
    }

    r := strings.NewReader("{\"documents\": " + string(documents) + "}")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
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

## <a name="analyze-sentiment-response"></a>Analisar a resposta do sentimento

É devolvida uma resposta com êxito em JSON, tal como é apresentado no exemplo seguinte: 

```json
{
   "documents": [
      {
         "score": 0.99984133243560791,
         "id": "1"
      },
      {
         "score": 0.024017512798309326,
         "id": "2"
      },
   ],
   "errors": [   ]
}
```

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases-request"></a>Pedido de extração de expressões-chave

A API de Extração de Expressões-Chave extrai expressões-chave de um documento de texto através do [método Expressões-Chave](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6). O seguinte exemplo extrai expressões-chave para o documento em inglês e o documento em espanhol.

1. Crie um novo projeto de Go no seu editor de código preferido.
1. Adicione o código indicado abaixo.
1. Substitua o valor `subscriptionKey` por uma chave de acesso válida para a sua subscrição.
1. Substitua a localização em `uriBase` (atualmente `westcentralus`) pela região em que se inscreveu.
1. Guarde o ficheiro com uma extensão ".go".
1. Abra uma linha de comandos num computador com o Go instalado.
1. Compile o ficheiro, por exemplo, com o comando "go build quickstart.go".
1. Execute o ficheiro, por exemplo, com o comando "quickstart".

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    /*
    Replace or verify the region.

    You must use the same region in your REST API call as you used to obtain your access keys.
    For example, if you obtained your access keys from the westus region, replace 
    "westcentralus" in the URI below with "westus".

    NOTE: Free trial access keys are generated in the westcentralus region, so if you are using
    a free trial access key, you should not need to change this region.
    */
    const uriBase =    "https://westcentralus.api.cognitive.microsoft.com"
    const uriPath = "/text/analytics/v2.0/keyPhrases"

    const uri = uriBase + uriPath

    data := []map[string]string{
        {"id": "1", "language": "en", "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."},
        {"id": "2", "language": "es", "text": "Si usted quiere comunicarse con Carlos, usted debe de llamarlo a su telefono movil. Carlos es muy responsable, pero necesita recibir una notificacion si hay algun problema."},
        {"id": "3", "language": "en", "text": "The Grand Hotel is a new hotel in the center of Seattle. It earned 5 stars in my review, and has the classiest decor I've ever seen."},
    }

    documents, err := json.Marshal(&data)
    if err != nil {
        fmt.Printf("Error marshaling data: %v\n", err)
        return
    }

    r := strings.NewReader("{\"documents\": " + string(documents) + "}")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
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

## <a name="extract-key-phrases-response"></a>Resposta da extração de expressões-chave

É devolvida uma resposta com êxito em JSON, tal como é apresentado no exemplo seguinte: 

```json
{
   "documents": [
      {
         "keyPhrases": [
            "HDR resolution",
            "new XBox",
            "clean look"
         ],
         "id": "1"
      },
      {
         "keyPhrases": [
            "Carlos",
            "notificacion",
            "algun problema",
            "telefono movil"
         ],
         "id": "2"
      },
      {
         "keyPhrases": [
            "new hotel",
            "Grand Hotel",
            "review",
            "center of Seattle",
            "classiest decor",
            "stars"
         ],
         "id": "3"
      }
   ],
   "errors": [  ]
}
```

<a name="Entities"></a>

## <a name="identify-linked-entities-request"></a>Pedido de identificação de entidades associadas

A API da Associação de Entidades identifica entidades conhecidas num documento de texto através do [método Associação de Entidades](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634). O seguinte exemplo identifica as entidades dos documentos em inglês.

1. Crie um novo projeto de Go no seu editor de código preferido.
1. Adicione o código indicado abaixo.
1. Substitua o valor `subscriptionKey` por uma chave de acesso válida para a sua subscrição.
1. Substitua a localização em `uriBase` (atualmente `westcentralus`) pela região em que se inscreveu.
1. Guarde o ficheiro com uma extensão ".go".
1. Abra uma linha de comandos num computador com o Go instalado.
1. Compile o ficheiro, por exemplo, com o comando "go build quickstart.go".
1. Execute o ficheiro, por exemplo, com o comando "quickstart".

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    /*
    Replace or verify the region.

    You must use the same region in your REST API call as you used to obtain your access keys.
    For example, if you obtained your access keys from the westus region, replace 
    "westcentralus" in the URI below with "westus".

    NOTE: Free trial access keys are generated in the westcentralus region, so if you are using
    a free trial access key, you should not need to change this region.
    */
    const uriBase =    "https://westcentralus.api.cognitive.microsoft.com"
    const uriPath = "/text/analytics/v2.0/entities"

    const uri = uriBase + uriPath

    data := []map[string]string{
        {"id": "1", "language": "en", "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."},
        {"id": "2", "language": "en", "text": "The Seattle Seahawks won the Super Bowl in 2014."},
    }

    documents, err := json.Marshal(&data)
    if err != nil {
        fmt.Printf("Error marshaling data: %v\n", err)
        return
    }

    r := strings.NewReader("{\"documents\": " + string(documents) + "}")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
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

## <a name="identify-linked-entities-response"></a>Resposta da identificação de entidades associadas

É devolvida uma resposta com êxito em JSON, tal como é apresentado no exemplo seguinte:

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "name": "Xbox One",
                    "matches": [
                        {
                            "text": "XBox One",
                            "offset": 23,
                            "length": 8
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Xbox One",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                    "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                },
                {
                    "name": "4K resolution",
                    "matches": [
                        {
                            "text": "4K",
                            "offset": 63,
                            "length": 2
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "4K resolution",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/4K_resolution",
                    "bingId": "1d4d689e-9cbf-b9eb-6ecf-f3296aaa96d8"
                }
            ]
        },
        {
            "id": "2",
            "entities": [
                {
                    "name": "Seattle Seahawks",
                    "matches": [
                        {
                            "text": "Seattle Seahawks",
                            "offset": 4,
                            "length": 16
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Seattle Seahawks",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Seattle_Seahawks",
                    "bingId": "1bf844db-8225-f90c-a78e-1787fb8af0ce"
                },
                {
                    "name": "Super Bowl",
                    "matches": [
                        {
                            "text": "Super Bowl",
                            "offset": 29,
                            "length": 10
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Super Bowl",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Super_Bowl",
                    "bingId": "ce1fece8-34c4-6249-a1aa-2e779294760e"
                }
            ]
        }
    ],
    "errors": []
}
```

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Análise de Texto com o Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Consulte também

 [Descrição Geral da Análise de Texto](../overview.md)  
 [Perguntas Mais Frequentes (FAQ)](../text-analytics-resource-faq.md)
