---
title: Guia de introdução do JavaScript para a pesquisa de Visual Bing API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra como carregar uma imagem para a API de pesquisa do Bing Visual e obter informações sobre a imagem a novamente.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: dd28c829d8d24980a746244dc6aca880d2d69224
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355093"
---
# <a name="your-first-bing-visual-search-query-in-javascript"></a>A primeira consulta de pesquisa de Visual Bing em JavaScript

API de pesquisa do Bing Visual devolve informações sobre uma imagem que fornecer. Pode fornecer a imagem utilizando o URL da imagem, uma insights token, ou através do carregamento de uma imagem. Para obter informações sobre estas opções, consulte [o que é a API de pesquisa do Bing Visual?](../overview.md) Este artigo demonstra o carregamento de uma imagem. Carregamento de uma imagem pode ser útil em cenários móveis onde tirar uma fotografia de um landmark bem conhecido e voltar a informação sobre a mesma. Por exemplo, as informações podem incluir trivia sobre o landmark. 

Se carregar uma imagem local, o seguinte mostra os dados do formulário que tem de incluir no corpo do POST. Os dados do formulário têm de incluir o cabeçalho de disposição de conteúdo. O `name` parâmetro tem de ser definido como "de imagem" e o `filename` parâmetro pode ser definido como qualquer cadeia. O conteúdo do formulário é o binário da imagem. O tamanho da imagem máximo, pode carregar é de 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Este artigo inclui uma aplicação de consola simples que envia um pedido de API de pesquisa do Bing Visual e apresenta os resultados de pesquisa JSON. Enquanto esta aplicação é escrita em JavaScript, a API é um serviço RESTful Web compatível com qualquer linguagem de programação que pode efetuar pedidos HTTP e analisar JSON. 

## <a name="prerequisites"></a>Pré-requisitos

Terá de [Node.js 6](https://nodejs.org/en/download/) para executar este código.

Para este início rápido, pode utilizar um [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) chave de subscrição ou uma chave de subscrição paga.

## <a name="running-the-application"></a>Executar a aplicação

O seguinte mostra como enviar a mensagem utilizando FormData no Node.js.

Para executar esta aplicação, siga estes passos:

1. Crie uma pasta para o seu projeto (ou utilize o seu IDE ou editor favorito).
2. Uma linha de comandos ou terminal, navegue para a pasta que acabou de criar.
3. Instale os módulos do pedido:  
  ```  
  npm install request  
  ```  
3. Instale os módulos de dados do formulário:  
  ```  
  npm install form-data  
  ```  
4. Crie um ficheiro denominado GetVisualInsights.js e adicione o seguinte código ao mesmo.
5. Substitua o `subscriptionKey` valor com a sua chave de subscrição.
6. Substitua o `imagePath` valor com o caminho da imagem para carregar.
7. Execute o programa.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');
var fs = require('fs');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';
var imagePath = "<pathtoyourimagegoeshere>";

var form = new FormData();
form.append("image", fs.createReadStream(imagePath));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


## <a name="next-steps"></a>Passos Seguintes

[Obter conhecimentos aprofundados sobre uma imagem utilizando um token de insights](../use-insights-token.md)  
[Tutorial de aplicação de página única pesquisa Visual do Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral de pesquisa de Visual do Bing](../overview.md)  
[Experimente](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referência da API de pesquisa do Bing Visual](https://aka.ms/bingvisualsearchreferencedoc)
