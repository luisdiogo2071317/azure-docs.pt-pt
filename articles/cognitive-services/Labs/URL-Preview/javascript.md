---
title: Início rápido de JavaScript para o URL do projeto de pré-visualização - serviços cognitivos da Microsoft | Documentos da Microsoft
description: Exemplo de script para começar rapidamente com a API de pré-visualização do URL do Bing nos serviços cognitivos da Microsoft no Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: dda6f7c105dfbadc3c22f0c008aa8759fe12fa03
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301357"
---
# <a name="url-preview-in-javascript"></a>Pré-visualização de URL em JavaScript 

A seguinte aplicação de página única utiliza JavaScript para criar uma visualização de URL para o site de SwiftKey: https://swiftkey.com/en. 

## <a name="prerequisites"></a>Pré-requisitos

Obter uma chave de acesso para a avaliação gratuita [laboratórios dos serviços cognitivos](https://labs.cognitive.microsoft.com/en-us/project-url-preview)

## <a name="code-scenario"></a>Cenário de código
O exemplo javascript a seguir inclui um objeto de entrada de caixa de texto em que o utilizador introduz o URL para a pré-visualização.  Quando o usuário clica o **pré-visualização** botão, as rotas de método onclick ao `getPreview` em que o código gera uma solicitação da Web para o **UrlPreview** ponto final.

O código cria um *XMLHttpRequest*, adiciona o *Ocp-Apim-Subscription-Key* cabeçalho e a chave e envia o pedido.  Ele adiciona um manipulador de eventos assíncronos para processar a resposta.

Se a resposta retorna com êxito, o manipulador atribui o texto JSON da resposta para o `demo` parágrafo na página. Outros elementos de resposta são definidos para os parágrafos seguintes para exibição.

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

**A demonstração em execução**

![Exemplo de pré-visualização de Url de JavaScript](./media/java-script-demo.png)

## <a name="running-the-application"></a>Executar a aplicação

Para executar a aplicação:

1. Substitua o `YOUR-SUBSCRIPTION-KEY` valor com uma chave de acesso válido para a sua subscrição.
2. Guarde o HTML e script num ficheiro com extensão. HTML.
3. Execute a página da Web num browser.
4. Utilize o URL existente ou introduza outro na caixa de texto.
5. Clique nas **pré-visualização** botão.

**Código-fonte:**

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
- [Início rápido em C#](csharp.md)
- [Início rápido de Java](java-quickstart.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)
