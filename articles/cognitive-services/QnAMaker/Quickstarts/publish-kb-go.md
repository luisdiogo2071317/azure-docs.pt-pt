---
title: 'Guia de Início Rápido: Publicar a Base de Dados de Conhecimento no Go – Criador de FAQ'
titleSuffix: Azure Cognitive Services
description: Como publicar uma base de dados de conhecimento no Go para o Criador de FAQ.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: qna-maker
ms.topic: quickstart
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: 5f146bf89ba2f41f392f7f713fe2cec86a7ef11c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033736"
---
# <a name="publish-a-knowledge-base-in-go"></a>Publicar uma base de dados de conhecimento no Go

O seguinte código publica uma base de dados de conhecimento existente através do método [Publicar](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe).

1. Crie um novo projeto do Go no seu IDE favorito.
2. Adicione o código indicado abaixo.
3. Substitua o valor `key` por uma chave de acesso válida para a sua subscrição.
4. Execute o programa.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

// NOTE: Replace this with a valid knowledge base ID.
var kb string = "ENTER ID HERE";

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/knowledgebases/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func post(uri string, content string) string {
    req, _ := http.NewRequest("POST", uri, bytes.NewBuffer([]byte(content)))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.Itoa(len(content)))
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    if(response.StatusCode == 204) {
        return "{'result' : 'Success.'}"
    } else {
        return string(body)
    }
}

func publish(uri string, req string) string {
    fmt.Println("Calling " + uri + ".")
    return post(uri, req)
}

func main() {
    var uri = host + service + method + kb
    body := publish(uri, "")
    fmt.Printf(body + "\n")

}
```

## <a name="the-publish-a-knowledge-base-response"></a>Resposta da publicação de uma base de dados de conhecimento

É devolvida uma resposta com êxito em JSON, tal como é apresentado no exemplo seguinte: 

```json
{
  "result": "Success."
}
```

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Referência à API REST do Criador de FAQ](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)