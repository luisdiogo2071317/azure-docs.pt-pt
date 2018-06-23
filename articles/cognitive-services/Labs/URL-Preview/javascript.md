---
title: Guia de introdução do JavaScript para o URL do projeto de pré-visualização - serviços cognitivos Microsoft | Microsoft Docs
description: Script de exemplo para rapidamente começar a utilizar a API do Bing URL Preview no Microsoft serviços cognitivos no Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 9041a88a292fb2dabead69195ebc3074e2ecf486
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354025"
---
# <a name="url-preview-in-javascript"></a>Pré-visualização de URL em JavaScript 

A aplicação de página única seguinte utiliza JavaScript para criar uma versão de pré-visualização do URL para o site SwiftKey: https://swiftkey.com/en. 

## <a name="prerequisites"></a>Pré-requisitos

Obter uma chave de acesso para a versão de avaliação gratuita [laboratórios de serviços cognitivos](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Cenário de código
O seguinte exemplo de javascript inclui um objeto de entrada da caixa de texto em que o utilizador introduz o URL para pré-visualização.  Quando o utilizador clica o **pré-visualização** botão, as rotas de método onclick para `getPreview` onde o código gera um pedido Web para o **UrlPreview** ponto final.

O código cria um *XMLHttpRequest*, adiciona o *Ocp-Apim-Subscription-Key* cabeçalho e a chave e envia o pedido.  Adiciona um processador de eventos assíncrona para processar a resposta.

Se a resposta devolve com êxito, o processador atribui o texto JSON da resposta para o `demo` parágrafo na página. Outros elementos de resposta são definidos para os parágrafos seguintes para apresentação.

**Resposta JSON não processada**

````
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile typing",
  "url": "https://swiftkey.com/en",
  "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

````

**Demonstração de execução**

![Exemplo de pré-visualização do Url de JavaScript](./media/java-script-demo.png)

## <a name="running-the-application"></a>Executar a aplicação

Para executar a aplicação:

1. Substitua o `YOUR-SUBSCRIPTION-KEY` valor com uma chave de acesso válido para a sua subscrição.
2. Guarde o HTML e o script para um ficheiro com extensão. HTML.
3. Execute a página Web num browser.
4. Utilizar o URL existente, ou introduza outro na caixa de texto.
5. Clique em de **pré-visualização** botão.

**Código de origem:**

```
<!DOCTYPE html>

<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="utf-8" />
    <title>urlPreview Demo</title>
</head>
<body>
    <h3>URL Preview Demo</h3>

    Page to preview: <input type="url" id="myURL" value="https://swiftkey.com/en">
    <button onclick="getPreview()">Preview</button>

    <p id="demo"></p>
    <br />
    <p id="jsonDesc"></p>
    <p><a id="familyFriendly"></a></p>
    <a id="contentUrl"></a>
    <p />
    <img id="jsonImage" />

    <script>

        var BING_ENDPOINT = "https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search"; 
        var key = "YOUR-SUBSCRIPTION-KEY";
        var xhr;

        function getPreview() {
            xhr = new XMLHttpRequest();

            var queryUrl = BING_ENDPOINT + "?q=" +
                encodeURIComponent(document.getElementById("myURL").value);
            xhr.open('GET', queryUrl, true);
            xhr.setRequestHeader("Ocp-Apim-Subscription-Key", key);

            xhr.send();

            xhr.addEventListener("readystatechange", processRequest, false);
        }

        function processRequest(e) {

            if (xhr.readyState == 4 && xhr.status == 200) {
                document.getElementById("demo").innerHTML = xhr.responseText;
                var obj = JSON.parse(xhr.responseText);
                document.getElementById("jsonDesc").innerHTML = obj.description;
                document.getElementById("familyFriendly").innerHTML = "Family Friendly: " + obj.isFamilyFriendly;
                document.getElementById("contentUrl").innerHTML = obj.url;
                document.getElementById("contentUrl").href = obj.url;
                document.getElementById("jsonImage").width = 350;
                document.getElementById("jsonImage").src = obj.primaryImageOfPage.contentUrl;

            }

        }

    </script>

</body>
</html>

```

## <a name="next-steps"></a>Passos Seguintes
- [Início rápido c#](csharp.md)
- [Guia de introdução do Java](java-quickstart.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)